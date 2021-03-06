---
title: Azure Analysis Services에서 장기 실행 작업에 대 한 모범 사례 | Microsoft Docs
description: 이 문서에서는 장기 실행 작업에 대 한 모범 사례를 설명 합니다.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: owend
ms.openlocfilehash: 5195d63180d45ad408256264e3cf10174e55cd96
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87551951"
---
# <a name="best-practices-for-long-running-operations"></a>장기 실행 작업에 대한 모범 사례

Azure Analysis Services에서 *노드* 는 서버 리소스가 실행 되는 호스트 가상 컴퓨터를 나타냅니다. 서버 리소스가 다른 노드로 이동 하는 경우 장기 실행 쿼리, 새로 고침 작업 및 쿼리 확장 동기화와 같은 일부 작업은 실패할 수 있습니다. 이 시나리오의 일반적인 오류 메시지는 다음과 같습니다.

- "장기 실행 XMLA 요청을 찾으려고 시도 하는 동안 오류가 발생 했습니다. 서비스를 업그레이드 하거나 서버를 다시 시작 하 여 요청이 중단 되었을 수 있습니다. "
- ' <guid> ' 모델에 대 한 ID가 ' ' 인 작업 <database> 은 업데이트 없이 중지 되었으므로 새로 고침 요청을 취소 하는 중 서비스 오류 (비활성)로 인해 취소 되었습니다. 이것은 내부 서비스 문제입니다. 이 문제가 반복적으로 발생 하는 경우 도움을 받으려면 작업을 다시 제출 하거나 티켓을 파일에 제출 하세요. "

장기 실행 작업이 중단 될 수 있는 많은 이유가 있습니다. 예를 들어 Azure의 업데이트는 다음과 같습니다. 
- 운영 체제 패치 
- 보안 업데이트
- Azure Analysis Services 서비스 업데이트
- 업데이트를 Service Fabric 합니다. Service Fabric은 Azure Analysis Services를 포함 하 여 다양 한 Microsoft 클라우드 서비스에서 사용 하는 플랫폼 구성 요소입니다.

서비스에서 발생 하는 업데이트 외에도 부하 분산으로 인해 노드 간 서비스가 자연스럽 게 이동 합니다. 노드 이동은 클라우드 서비스의 예상 부분입니다. Azure Analysis Services는 노드 이동의 영향을 최소화 하려고 하지만 완전히 제거할 수는 없습니다. 

노드 이동 외에도 다른 오류가 발생할 수 있습니다. 예를 들어 데이터 원본 데이터베이스 시스템이 오프 라인 상태 이거나 네트워크 연결이 끊어질 수 있습니다. 새로 고치는 동안 파티션에 1000만 개의 행이 있고 1 1/1000000 행에 오류가 발생 하는 경우 오류 발생 시 새로 고침을 다시 시작할 방법이 없습니다. 서비스를 처음부터 다시 시작 해야 합니다. 

## <a name="refresh-rest-api"></a>새로 고침 REST API

데이터 새로 고침 같은 장기 실행 작업의 경우 서비스 중단이 어려울 수 있습니다. Azure Analysis Services에는 서비스 중단의 부정적 영향을 완화 하는 데 도움이 되는 REST API 포함 되어 있습니다. 자세한 내용은 [REST API를 사용 하 여 비동기 새로 고침](analysis-services-async-refresh.md)을 참조 하세요.
 
REST API 외에도 장기 실행 새로 고침 작업 중에 잠재적인 문제를 최소화 하는 데 사용할 수 있는 다른 방법이 있습니다. 주요 목표는 새로 고침 작업을 처음부터 다시 시작 하는 것을 방지 하는 것이 아니라 단계에서 커밋할 수 있는 작은 일괄 처리에서 새로 고침을 수행 하는 것입니다. 
 
REST API는 이러한 다시 시작을 허용 하지만 파티션 생성 및 삭제의 완전 한 유연성을 허용 하지 않습니다. 시나리오에 복잡 한 데이터 관리 작업이 필요한 경우 솔루션에는 논리적 일괄 처리 형태를 포함 해야 합니다. 예를 들어 트랜잭션을 사용 하 여 여러 개의 데이터를 처리 하는 경우 별도의 일괄 처리를 사용 하면 오류를 처음부터 다시 시작 하지 않아도 되지만 중간 검사점에서 다시 시작 하지 않아도 됩니다. 
 
## <a name="scale-out-query-replicas"></a>쿼리 복제본 확장

REST 또는 사용자 지정 논리를 사용 하는 경우에도 일괄 처리를 처리 하는 동안 클라이언트 응용 프로그램 쿼리에서 일관 되지 않은 결과 나 중간 결과를 반환할 수 있습니다 처리가 진행 되는 동안 클라이언트 응용 프로그램 쿼리에서 반환한 일관 된 데이터가 필요 하 고 모델 데이터가 중간 상태 이면 읽기 전용 쿼리 복제본으로 [확장](analysis-services-scale-out.md) 을 사용할 수 있습니다.

읽기 전용 쿼리 복제본을 사용 하면 새로 고침이 일괄 처리로 수행 되는 동안 클라이언트 응용 프로그램 사용자가 읽기 전용 복제본에 대 한 이전 데이터 스냅숏을 계속 쿼리할 수 있습니다. 새로 고침이 완료 되 면 읽기 전용 복제본을 최신 상태로 만들기 위해 동기화 작업을 수행할 수 있습니다.


## <a name="next-steps"></a>다음 단계

[REST API를 사용한 비동기 새로 고침](analysis-services-async-refresh.md)  
[Azure Analysis Services 규모 확장](analysis-services-scale-out.md)  
[Analysis Services 고가용성](analysis-services-bcdr.md)  
[Azure 서비스에 대 한 다시 시도 지침](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific)   

