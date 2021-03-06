---
title: Azure IoT Hub 직접 메서드 이해 | Microsoft Docs
description: 개발자 가이드 - 직접 메서드를 사용하여 서비스 앱의 디바이스에서 코드 호출
author: nberdy
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: rezas
ms.custom:
- amqp
- mqtt
- 'Role: Cloud Development'
- 'Role: IoT Device'
ms.openlocfilehash: 516b3bac5da2e078217d5c12f1efdf527b7c83a1
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90029072"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>IoT Hub의 직접 메서드 호출 및 이해

IoT Hub를 사용하면 클라우드의 디바이스에서 직접 메서드를 호출할 수 있습니다. 직접 메서드는 사용자가 지정한 시간 제한을 초과하는 즉시 성공하거나 실패한다는 점에서 HTTP 호출과 비슷한 디바이스와의 요청-응답 상호 작용을 나타냅니다. 이 방법은 즉각적인 조치 과정이 디바이스의 응답 여부에 따라 달라지는 시나리오에서 유용합니다.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

각 디바이스 메서드는 단일 디바이스를 대상으로 합니다. [여러 디바이스에서 작업 예약](iot-hub-devguide-jobs.md)에서는 여러 디바이스에서 직접 메서드를 호출하고 연결되지 않은 디바이스에 대한 메서드 호출을 예약하는 방법을 보여 줍니다.

IoT Hub에 **서비스 연결** 권한만 있다면 누구든 디바이스에서 메서드를 호출할 수 있습니다.

직접 메서드는 요청-응답 패턴을 따르며, 결과를 즉각적으로 확인해야 하는 통신에 유의미합니다. 팬 켜기 등, 대화형 디바이스 제어를 예로 들 수 있습니다.

desired 속성, 직접 메서드 또는 클라우드-디바이스 메시지 사용에 대해 궁금한 점이 있으면 [클라우드-디바이스 통신 지침](iot-hub-devguide-c2d-guidance.md)을 참조하세요.

## <a name="method-lifecycle"></a>메서드 수명 주기

직접 메서드는 디바이스에서 구현되며, 제대로 인스턴스화하기 위해 메서드 페이로드에 0개 이상의 입력이 필요할 수 있습니다. 직접 메서드는 서비스 지향 URI를 통해 호출합니다(`{iot hub}/twins/{device id}/methods/`). 디바이스는 디바이스별 MQTT 항목(`$iothub/methods/POST/{method name}/`) 또는 AMQP 링크(`IoThub-methodname` 및 `IoThub-status` 애플리케이션 속성)를 통해 직접 메서드를 수신합니다.

> [!NOTE]
> 디바이스에서 직접 메서드를 호출할 때 속성 이름과 값은 US-ASCII로 출력 가능한 영숫자만 포함할 수 있으며 다음 집합은 제외됩니다. ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``
> 

직접 메서드는 동기적 이며 시간 제한 기간 (기본값: 30 초, 5 초에서 300 초 사이로 설정 가능) 후 성공 하거나 실패 합니다. 직접 메서드는 디바이스가 온라인 상태에서 명령을 수신하는 경우에만 작동하기를 바라는 대화형 시나리오에서 유용합니다. 예를 들어 휴대폰에서 불을 켭니다. 이러한 시나리오에서는 클라우드 서비스가 결과에 최대한 빨리 대응할 수 있도록 즉각적인 성공이나 실패를 보려고 합니다. 디바이스는 메서드의 결과로 메시지 본문을 반환할 수 있지만 메서드가 반드시 그렇게 해야 하는 것은 아닙니다. 메서드 호출의 순서 지정 또는 동시성 의미 체계에 대한 보장은 없습니다.

직접 메서드는 클라우드 쪽 및 MQTT, AMQP, Websocket을 통한 MQTT 또는 장치 쪽에서 Websocket을 통한 AMQP의 HTTPS 전용입니다.

메서드 요청 및 응답에 대한 페이로드는 최대 128KB의 JSON 문서입니다.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>백 엔드 앱에서 직접 메서드 호출

이제 백 엔드 앱에서 직접 메서드를 호출합니다.

### <a name="method-invocation"></a>메서드 호출

디바이스의 직접 메서드 호출은 다음 항목으로 구성된 HTTPS 호출입니다.

* 디바이스와 관련된 *요청 URI* 및 [API 버전](https://docs.aws.amazon.com/cli/latest/reference/iot1click-devices/invoke-device-method.html):

    ```http
    https://fully-qualified-iothubname.azure-devices.net/twins/{deviceId}/methods?api-version=2018-06-30
    ```

* POST *메서드*

* 권한 부여, 요청 ID, 콘텐츠 형식 및 콘텐츠 인코딩을 포함 하는 *헤더* 입니다.

* 다음과 같은 형식의 투명한 JSON *본문*:

    ```json
    {
        "methodName": "reboot",
        "responseTimeoutInSeconds": 200,
        "payload": {
            "input1": "someInput",
            "input2": "anotherInput"
        }
    }
    ```

요청에서로 제공 되는 값은 `responseTimeoutInSeconds` IoT Hub 서비스가 장치에서 직접 메서드 실행이 완료 될 때까지 기다려야 하는 시간입니다. 이 시간 제한을 장치에서 직접 메서드를 실행 하는 데 예상 되는 시간 이상으로 설정 합니다. Timeout을 지정 하지 않으면 기본값인 30 초가 사용 됩니다. 의 최소값 및 최대값은 `responseTimeoutInSeconds` 각각 5 및 300 초입니다.

요청에서로 제공 되는 값은 `connectTimeoutInSeconds` 연결이 끊어진 장치가 온라인 상태가 될 때까지 IoT Hub 서비스에서 대기 해야 하는 직접 메서드를 호출 하는 시간입니다. 기본값은 0입니다. 즉, 직접 메서드를 호출할 때 장치가 이미 온라인 상태 여야 합니다. 의 최대값은 `connectTimeoutInSeconds` 300 초입니다.

#### <a name="example"></a>예제

이 예제에서는 Azure IoT Hub에 등록 된 IoT 장치에서 직접 메서드를 호출 하는 요청을 안전 하 게 시작할 수 있습니다.

시작 하려면 [Azure CLI에 대 한 Microsoft Azure IoT 확장](https://github.com/Azure/azure-iot-cli-extension) 을 사용 하 여 SharedAccessSignature를 만듭니다.

```bash
az iot hub generate-sas-token -n <iothubName> -du <duration>
```

그런 다음 권한 부여 헤더를 새로 생성 된 sharedaccesssignature로 바꾸고 `iothubName` ,, `deviceId` `methodName` 및 `payload` 매개 변수를 아래 예제 명령의 구현과 일치 하도록 `curl` 수정 합니다.  

```bash
curl -X POST \
  https://<iothubName>.azure-devices.net/twins/<deviceId>/methods?api-version=2018-06-30 \
  -H 'Authorization: SharedAccessSignature sr=iothubname.azure-devices.net&sig=x&se=x&skn=iothubowner' \
  -H 'Content-Type: application/json' \
  -d '{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}'
```

수정 된 명령을 실행 하 여 지정 된 직접 메서드를 호출 합니다. 성공적인 요청은 HTTP 200 상태 코드를 반환 합니다.

> [!NOTE]
> 위의 예제에서는 장치에서 직접 메서드를 호출 하는 방법을 보여 줍니다.  IoT Edge 모듈에서 직접 메서드를 호출 하려는 경우 아래와 같이 url 요청을 수정 해야 합니다.

```bash
https://<iothubName>.azure-devices.net/twins/<deviceId>/modules/<moduleName>/methods?api-version=2018-06-30
```
### <a name="response"></a>응답

백 엔드 앱은 다음 항목으로 구성된 응답을 받습니다.

* *HTTP 상태 코드*:
  * 200는 직접 메서드를 성공적으로 실행 했음을 나타냅니다.
  * 404는 장치 ID가 올바르지 않거나, 직접 메서드를 호출할 때 장치가 온라인 상태가 아님을 나타내며 `connectTimeoutInSeconds` 그 이후에는 (수반 되는 오류 메시지를 사용 하 여 근본 원인을 파악 함)
  * 504는 장치가 내에서 직접 메서드 호출에 응답 하지 않아 발생 한 게이트웨이 시간 초과를 나타냅니다 `responseTimeoutInSeconds` .

* ETag, 요청 ID, 콘텐츠 형식 및 콘텐츠 인코딩을 포함 하는 *헤더* 입니다.

* 다음과 같은 형식의 JSON *본문*:

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    `status`와 `body`는 모두 디바이스에 의해 제공되며 디바이스 자체의 상태 코드 및/또는 설명으로 응답하는 데 사용됩니다.

### <a name="method-invocation-for-iot-edge-modules"></a>IoT Edge 모듈에 대한 메서드 호출

모듈 ID를 사용하여 직접 메서드를 호출하는 작업은 [IoT 서비스 클라이언트 C# SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices/)에서 지원됩니다

이 작업의 경우 `ServiceClient.InvokeDeviceMethodAsync()` 메서드를 사용하고 `deviceId` 및 `moduleId`에서 매개 변수로 전달합니다.

## <a name="handle-a-direct-method-on-a-device"></a>디바이스에서 직접 메서드 처리

IoT 디바이스에서 직접 메서드를 처리하는 방법을 살펴보겠습니다.

### <a name="mqtt"></a>MQTT

다음 섹션은 MQTT 프로토콜에 대해 설명합니다.

#### <a name="method-invocation"></a>메서드 호출

디바이스는 MQTT 토픽으로 직접 메서드 요청을 수신합니다. `$iothub/methods/POST/{method name}/?$rid={request id}` 디바이스당 구독 수가 5로 제한됩니다. 따라서 각 직접 메서드를 개별적으로 구독하지 않는 것이 좋습니다. 대신 `$iothub/methods/POST/#`을 구독한 다음, 원하는 메서드 이름을 기반으로 배달된 메시지를 필터링하는 것이 좋습니다.

디바이스가 수신하는 본문은 다음과 같은 형식입니다.

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

메서드 요청은 QoS 0입니다.

#### <a name="response"></a>응답

디바이스는 `$iothub/methods/res/{status}/?$rid={request id}`에 응답을 보내는데 여기서:

* `status` 속성은 디바이스가 제공하는 메서드 실행 상태입니다.

* `$rid` 속성은 IoT Hub로부터 수신한 메서드 호출의 요청 ID입니다.

본문은 디바이스에 의해 설정되며 모든 상태가 될 수 있습니다.

### <a name="amqp"></a>AMQP

다음 섹션은 AMQP 프로토콜에 대해 설명합니다.

#### <a name="method-invocation"></a>메서드 호출

디바이스는 `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound` 주소에서 수신 링크를 만들어 직접 메서드 요청을 수신합니다.

AMQP 메시지는 메서드 요청을 나타내는 수신 링크에 도착하며 여기에는 다음 단원이 포함되어 있습니다.

* 해당하는 메서드 응답과 함께 다시 전달해야 하는 요청 ID가 포함된 상관 관계 ID 속성

* 호출 중인 메서드의 이름이 포함된 애플리케이션 속성 `IoThub-methodname`

* 메서드 페이로드가 JSON으로 포함된 AMQP 메시지 본문

#### <a name="response"></a>응답

디바이스는 `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound` 주소에서 메서드 응답을 반환하기 위한 전송 링크를 만듭니다.

메서드의 응답은 보내는 링크에서 반환 되 고 다음과 같이 구성 됩니다.

* 메서드의 요청 메시지에 전달 된 요청 ID를 포함 하는 상관 관계 ID 속성입니다.

* 사용자가 제공한 메서드 상태가 포함된 애플리케이션 속성 `IoThub-status`

* 메서드 응답이 JSON으로 포함된 AMQP 메시지 본문

## <a name="additional-reference-material"></a>추가 참조 자료

이 IoT Hub 개발자 가이드의 다른 참조 자료:

* [IoT Hub 엔드포인트](iot-hub-devguide-endpoints.md)는 각 IoT Hub에서 런타임 및 관리 작업에 대해 공개하는 다양한 엔드포인트에 대해 설명합니다.

* [제한 및 할당량](iot-hub-devguide-quotas-throttling.md) - IoT Hub를 사용할 때 적용되는 할당량과 예상되는 제한 동작에 대해 설명합니다.

* [Azure IoT 디바이스 및 서비스 SDK](iot-hub-devguide-sdks.md)는 IoT Hub와 상호 작용하는 디바이스 및 서비스 앱 모두를 개발할 때 사용할 수 있는 다양한 언어 SDK를 나열합니다.

* [디바이스 쌍, 작업 및 메시지 라우팅을 위한 IoT Hub 쿼리 언어](iot-hub-devguide-query-language.md)에서는 IoT Hub에서 디바이스 쌍 및 작업에 대한 정보를 검색하는 데 사용할 수 있는 IoT Hub 쿼리 언어에 대해 설명합니다.

* [IoT Hub MQTT 지원](iot-hub-mqtt-support.md)은 MQTT 프로토콜에 대한 IoT Hub 지원에 대해 자세히 설명합니다.

## <a name="next-steps"></a>다음 단계

직접 메서드를 사용하는 방법에 대해 알아봤으니 다음 IoT Hub 개발자 가이드 문서를 살펴보세요.

* [여러 디바이스에서 작업 예약](iot-hub-devguide-jobs.md)

이 문서에서 설명한 일부 개념을 시도해 보려면 다음과 같은 IoT Hub 자습서를 살펴보세요.

* [직접 메서드 사용](quickstart-control-device-node.md)
* [VS Code용 Azure IoT Tools를 사용하여 디바이스 관리](iot-hub-device-management-iot-toolkit.md)
