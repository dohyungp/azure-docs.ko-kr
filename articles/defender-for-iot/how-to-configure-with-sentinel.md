---
title: IoT 용 Azure 센티널 구성 (미리 보기)
description: IoT 용 Defender 솔루션에서 데이터를 수신 하도록 Azure 센티널를 구성 하는 방법을 설명 합니다.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: 71147352c5b75195ed0dff2b05acc5315f3183cc
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90940060"
---
# <a name="connect-your-data-from-defender-for-iot-to-azure-sentinel-preview"></a>IoT 용 Defender의 데이터를 Azure 센티널에 연결 (미리 보기)

Azure 센티널의 IoT 데이터 커넥터용 Azure Security Center은 현재 공개 미리 보기로 제공 됩니다. 이 기능은 서비스 수준 계약 없이 제공 되며 프로덕션 워크 로드에는 권장 되지 않습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/)을 참조하세요.

이 가이드에서는 IoT 용 Defender 데이터를 Azure 센티널에 연결 하는 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 사전 요구 사항
> * 연결 설정
> * Log Analytics 경고 보기

IoT 용 Defender에서 경고를 연결 하 고 Azure 센티널로 직접 스트리밍합니다.

## <a name="prerequisites"></a>사전 요구 사항

- 작업 영역 **읽기** 및 **쓰기** 권한이 있어야 합니다.
- **IoT 용 Defender는** 관련 IoT Hub에서 **사용 하도록 설정** 해야 합니다.
- 연결 하려는 **Azure IoT Hub** 에 대 한 **읽기** 및 **쓰기** 권한이 모두 있어야 합니다.
- 또한 **Azure IoT Hub 리소스 그룹**에 대 한 **읽기** 및 **쓰기** 권한이 있어야 합니다.

> [!NOTE]
> 일반 Azure 리소스 경고를 보내려면 구독에서 실행 되는 Azure Security Center 표준 계층 라이선스가 있어야 합니다. IoT 용 Defender에 필요한 무료 계층 라이선스를 사용 하는 경우 IoT 관련 경고만 Azure 센티널로 전달 됩니다.

## <a name="connect-to-defender-for-iot"></a>IoT 용 Defender에 연결

1. Azure 센티널에서 **데이터 커넥터** 를 선택한 다음 **IoT 용 Defender** 타일을 클릭 합니다.
1. 오른쪽 창의 맨 아래에서 **커넥터 페이지 열기**를 클릭 합니다.
1. 연결을 클릭 하 고, 경고 및 장치 경고가 Azure 센티널로 스트리밍할 각 IoT Hub 구독 옆에 있는 **연결**을 클릭 합니다.
    - IoT 용 Defender가 해당 허브에서 사용 하도록 설정 되지 않은 경우 활성화 경고 메시지가 표시 됩니다. **사용** 링크를 클릭 하 여 서비스를 시작 하 고 사용 하도록 설정 합니다.
1. IoT에 대해 Defender에서 경고를 통해 Azure 센티널에서 인시던트를 자동으로 생성할지 여부를 결정할 수 있습니다. **인시던트 만들기**에서 **사용** 을 선택 하 여 생성 된 경고에서 인시던트를 자동으로 생성 하는 규칙을 사용 하도록 설정 합니다.  이 규칙은 **분석**  >  **활성** 규칙에서 변경 하거나 편집할 수 있습니다.

> [!NOTE]
>연결 변경 후 허브 목록을 새로 고치는 데 10 초 이상 걸릴 수 있습니다.

## <a name="log-analytics-alert-display"></a>Log Analytics 경고 표시

Log Analytics에서 관련 스키마를 사용 하 여 IoT 경고에 대 한 Defender를 표시 하려면 다음을 수행 합니다.

1. **Logs**  >  **securityinsights**  >  **securityinsights**를 열거나 **securityinsights**를 검색 합니다.
1. 다음 kql 필터를 사용 하 여 IoT 생성 된 경고에 대 한 Defender만 표시 하도록 필터링 합니다.

```kusto
SecurityAlert | where ProductName == "Defender for IoT"
```

### <a name="service-notes"></a>서비스 정보

IoT Hub 연결한 후에는 약 15 분 후에 허브 데이터를 Azure 센티널에서 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 IoT 용 Defender를 Azure 센티널에 연결 하는 방법을 알아보았습니다. 위협 검색 및 보안 데이터 액세스에 대해 자세히 알아보려면 다음 문서를 참조 하세요.

- Azure 센티널을 사용 하 여 데이터에 대 [한 가시성과 잠재적 위협을](https://docs.microsoft.com/azure/sentinel/quickstart-get-visibility)파악 하는 방법을 알아봅니다.

- [IoT 보안 데이터에 액세스](how-to-security-data-access.md) 하는 방법 알아보기
