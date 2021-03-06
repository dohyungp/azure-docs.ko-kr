---
title: Azure Application Insights 개요 대시보드 | Microsoft Docs
description: Azure Application Insights 및 개요 대시보드 기능을 사용하여 애플리케이션을 모니터링합니다.
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 714488d88b269f252dfd6677f847db315969a270
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90970617"
---
# <a name="application-insights-overview-dashboard"></a>Application Insights 개요 대시보드

Application Insights에는 애플리케이션의 상태 및 성능을 한 눈에 빠르게 파악할 수 있도록 요약 개요 창이 항상 제공됩니다. 새 개요 대시보드에는 더 빠르고 유연한 환경이 제공됩니다.

## <a name="how-do-i-test-out-the-new-experience"></a>새 환경을 테스트하려면 어떻게 할까요?

이제 새 개요 대시보드가 기본적으로 시작합니다.

![개요 미리 보기 창](./media/overview-dashboard/overview.png)

## <a name="better-performance"></a>성능 향상

시간 범위 선택이 간단한 한 번의 클릭 인터페이스로 간소화되었습니다.

![시간 범위](./media/overview-dashboard/app-insights-overview-dashboard-03.png)

성능이 전반적으로 크게 향상되었습니다. **검색** 및 **분석** 같은 인기 있는 기능에 한 번의 클릭으로 액세스합니다. 동적으로 업데이트되는 KPI 타일이 각각 해당 Application Insights 기능에 인사이트를 제공합니다. 실패한 요청에 대해 자세히 알려면 **조사** 헤더 아래에서 **오류**를 선택합니다.

![오류](./media/overview-dashboard/app-insights-overview-dashboard-04.png)

## <a name="application-dashboard"></a>애플리케이션 대시보드

애플리케이션 대시보드는 Azure의 기존 대시보드 기술을 활용하여 애플리케이션의 상태와 성능을 완벽하게 사용자 지정할 수 있는 단일 창 보기를 제공합니다.

기본 대시보드에 액세스하려면 왼쪽 위 모서리에 있는 _애플리케이션 대시보드_를 선택합니다.

![스크린샷 강조 표시 된 응용 프로그램 대시보드 단추를 표시 합니다.](./media/overview-dashboard/app-insights-overview-dashboard-05.png)

대시보드에 처음 액세스하는 경우 기본 보기가 시작됩니다.

![대시보드 보기](./media/overview-dashboard/0001-dashboard.png)

원하는 경우 기본 보기를 유지할 수 있습니다. 또는 팀의 요구에 가장 적합하도록 대시보드에서 추가하고 삭제할 수도 있습니다.

> [!NOTE]
> Application Insights 리소스에 액세스할 수 있는 모든 사용자는 동일한 애플리케이션 대시보드 환경을 공유합니다. 한 명의 사용자가 대시보드를 변경하면 모든 사용자의 보기가 수정됩니다.

개요 환경으로 돌아가려면 다음을 선택합니다.

![개요 단추](./media/overview-dashboard/app-insights-overview-dashboard-07.png)

## <a name="troubleshooting"></a>문제 해결

**타일 설정 구성** 을 선택 하 고 31 일을 초과 하 여 사용자 지정 시간 범위를 설정 하는 경우 기본 데이터 보존 90 일에도 불구 하 고, 대시보드는 31 일 이상 데이터를 표시 하지 않습니다. 현재이 동작에 대 한 해결 방법이 없습니다.

## <a name="next-steps"></a>다음 단계

- [깔때기](./usage-funnels.md)
- [보존](./usage-retention.md)
- [사용자 흐름](./usage-flows.md)

