---
title: 제한 및 할당량 IoT 플러그 앤 플레이 미리 보기 | Microsoft Docs
description: IoT 플러그 앤 플레이 미리 보기를 사용 하는 경우 적용 되는 제한, 할당량 및 제한에 대해 알아봅니다.
author: prashmo
ms.author: prashmo
ms.date: 07/21/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 5c4377120f61792b580225a22b9f5ff51b5e1b64
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337401"
---
# <a name="iot-plug-and-play-preview-limits-quotas-and-throttles"></a>IoT 플러그 앤 플레이 미리 보기 제한, 할당량 및 제한

이 문서에서는 공개 미리 보기에 적용 되는 IoT 플러그 앤 플레이 별 제한, 할당량 및 제한에 대해 설명 합니다. 기존 [IoT Hub 할당량과 제한이](../iot-hub/iot-hub-devguide-quotas-throttling.md) 적용 됩니다.

## <a name="iot-hub"></a>IoT Hub

공개 미리 보기의 경우 다음과 같은 제한과 할당량이 IoT hub에 적용 됩니다.

| 제한, 제한 사항 및 제한 | 값 | 메모 |
|-----|-----|-----|
| 허브 당 등록할 수 있는 인터페이스 수 | 1500 ||
| 구성 요소 이름의 최대 크기 | 1-64 문자 | 허용 되는 문자: a-z, a-z, 0-9 (첫 번째 문자 아님) 및 밑줄 (첫 번째 또는 마지막 문자가 아님) |
| 속성 이름의 최대 크기 | 1-64 문자 | 허용 되는 문자: a-z, a-z, 0-9 (첫 번째 문자 아님) 및 밑줄 (첫 번째 또는 마지막 문자가 아님) |
| 속성 값의 최대 크기 | Digital Twins 정의 언어 [속성과](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#property) 동일 | 깊이 있는 5 개의 수준 이며 배열이 나 배열이 포함 된 복잡 한 스키마가 아닐 수도 있습니다. |
| 명령 이름의 최대 크기 | 1-64 문자 | 허용 되는 문자: a-z, a-z, 0-9 (첫 번째 문자 아님) 및 밑줄 (첫 번째 또는 마지막 문자가 아님)|
| 디바이스 쌍 크기 | [IoT Hub 제한과](../iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) 동일 ||

## <a name="parser-library"></a>파서 라이브러리

파서 라이브러리는 [디지털 Twins 정의 언어](https://github.com/Azure/opendigitaltwins-dtdl)에 적용 되는 제한을 따릅니다.

## <a name="next-steps"></a>다음 단계

제안 된 다음 단계는 [IoT 플러그 앤 플레이 아키텍처](concepts-architecture.md)를 검토 하는 것입니다.
