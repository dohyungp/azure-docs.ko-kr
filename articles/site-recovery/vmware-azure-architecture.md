---
title: Azure Site Recovery의 VMware VM 재해 복구 아키텍처
description: 이 문서에서는 Azure Site Recovery를 사용하여 온-프레미스 VMware VM과 Azure 간 재해 복구를 설정할 때 사용되는 구성 요소 및 아키텍처를 간략하게 설명합니다.
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: raynew
ms.openlocfilehash: 4b1b8a0cfa98d48d7cb92474c1572f17c79ffd0d
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498955"
---
# <a name="vmware-to-azure-disaster-recovery-architecture"></a>VMware와 Azure 간 재해 복구 아키텍처

이 문서에서는 [Azure Site Recovery](site-recovery-overview.md) 서비스를 사용하여 온-프레미스 VMware 사이트와 Azure 간에 VMware VM(가상 머신)의 재해 복구 복제, 장애 조치(failover) 및 복구를 배포할 때 사용되는 아키텍처와 프로세스에 대해 설명합니다.


## <a name="architectural-components"></a>아키텍처 구성 요소

다음 표와 그림은 VMware와 Azure 간 재해 복구에 사용되는 구성 요소를 개략적으로 보여 줍니다.

**구성 요소** | **요구 사항** | **세부 정보**
--- | --- | ---
**Azure** | Azure 구독, 캐시, 관리 디스크 및 Azure 네트워크에 대 한 Azure Storage 계정 | 온-프레미스 Vm에서 복제 된 데이터는 Azure storage에 저장 됩니다. 장애 조치를 온-프레미스에서 Azure로 실행할 때 복제된 데이터를 사용하여 Azure VM을 만듭니다. Azure VM을 만들 때 Azure 가상 네트워크에 연결합니다.
**구성 서버 컴퓨터** | 단일 온-프레미스 컴퓨터입니다. 다운로드한 OVF 템플릿에서 배포할 수 있는 VMware VM으로 실행하는 것이 좋습니다.<br/><br/> 이 컴퓨터는 구성 서버, 프로세스 서버 및 마스터 대상 서버를 포함하는 모든 온-프레미스 Site Recovery 구성 요소를 실행합니다. | **구성 서버**: 온-프레미스와 Azure 간의 통신을 조정하여 데이터 복제를 관리합니다.<br/><br/> **프로세스 서버**: 기본적으로 구성 서버에 설치됩니다. 복제 데이터를 수신하고, 캐싱, 압축 및 암호화를 사용하여 최적화하며, Azure Storage로 보냅니다. 또한 프로세스 서버는 복제하려는 VM에 Azure Site Recovery 모바일 서비스를 설치하고, 온-프레미스 컴퓨터의 자동 검색을 수행합니다. 배포가 늘어나면 프로세스 서버로 실행하는 별도의 프로세스 서버를 추가하여 더 큰 복제 트래픽을 처리할 수 있습니다.<br/><br/> **마스터 대상 서버**: 기본적으로 구성 서버에 설치됩니다. Azure에서 장애 복구(Failback) 중에 복제 데이터를 처리합니다. 대규모 배포의 경우 장애 복구를 위해 추가적인 별도의 마스터 대상 서버를 추가할 수 있습니다.
**VMware 서버** | VMware VM은 온-프레미스 vSphere ESXi 서버에서 호스트됩니다. 호스트를 관리하려면 vCenter 서버를 사용하는 것이 좋습니다. | Site Recovery 배포 중에 VMware 서버를e Recovery Services 자격 증명 모음에 추가합니다.
**복제된 컴퓨터** | 복제한 각 VMware VM에 모바일 서비스가 설치됩니다. | 프로세스 서버에서 자동 설치를 수행할 수 있도록 하는 것이 좋습니다. 또는 서비스를 수동으로 설치 하거나 자동화 된 배포 방법 (예: Configuration Manager)을 사용할 수 있습니다.

![VMware에서 Azure로 복제 아키텍처 관계를 보여 주는 다이어그램](./media/vmware-azure-architecture/arch-enhanced.png)

## <a name="set-up-outbound-network-connectivity"></a>아웃 바운드 네트워크 연결 설정

Site Recovery가 예상 대로 작동 하려면 아웃 바운드 네트워크 연결을 수정 하 여 환경을 복제할 수 있도록 해야 합니다.

> [!NOTE]
> Site Recovery는 인증 프록시를 사용하여 네트워크 연결을 제어하도록 지원하지 않습니다.

### <a name="outbound-connectivity-for-urls"></a>URL에 대한 아웃바운드 연결

URL 기반 방화벽 프록시를 사용하여 아웃바운드 연결을 제어하는 경우 다음 URL에 대한 액세스를 허용합니다.

| **이름**                  | **상용**                               | **정부**                                 | **설명** |
| ------------------------- | -------------------------------------------- | ---------------------------------------------- | ----------- |
| 스토리지                   | `*.blob.core.windows.net`                  | `*.blob.core.usgovcloudapi.net`              | VM에서 원본 지역의 캐시 스토리지 계정에 데이터를 쓸 수 있도록 합니다. |
| Azure Active Directory    | `login.microsoftonline.com`                | `login.microsoftonline.us`                   | Site Recovery 서비스 URL에 대한 권한 부여 및 인증을 제공합니다. |
| 복제               | `*.hypervrecoverymanager.windowsazure.com` | `*.hypervrecoverymanager.windowsazure.com`   | VM이 Site Recovery 서비스와 통신할 수 있도록 합니다. |
| Service Bus               | `*.servicebus.windows.net`                 | `*.servicebus.usgovcloudapi.net`             | VM이 Site Recovery 모니터링 및 진단 데이터를 쓸 수 있도록 합니다. |

## <a name="replication-process"></a>복제 프로세스

1. VM에 대해 복제를 사용하도록 설정하면 지정된 복제 정책을 사용하여 Azure Storage에 초기 복제가 시작됩니다. 다음 사항에 유의하세요.
    - VMware VM의 경우 복제는 VM에서 실행 중인 모바일 서비스 에이전트를 사용하여 블록 수준에서 거의 지속적으로 이루어집니다.
    - 복제 정책 설정이 적용됩니다.
        - **RPO 임계값**입니다. 이 설정은 복제에 영향을 주지 않습니다. 모니터링에 도움이 됩니다. 현재 RPO가 지정 임계값 한도를 초과하는 경우 이벤트가 발생하고 선택적으로 메일이 전송됩니다.
        - **복구 지점 보존**. 이 설정은 중단이 발생하는 경우 얼마나 오래전으로 되돌아갈지 지정합니다. Premium Storage의 최대 보존 기간은 24시간입니다. 표준 스토리지의 경우는 72시간입니다. 
        - **앱 일치 스냅샷**. 앱 요구 사항에 따라 1 ~ 12 시간 마다 앱 일치 스냅숏을 만들 수 있습니다. 스냅샷은 표준 Azure Blob 스냅샷입니다. VM에서 실행되는 모바일 에이전트는 이 설정에 따라 VSS 스냅샷을 요청하며 이 특정 시점을 복제 스트림의 애플리케이션 일치 지점으로 북마크합니다.

2. 트래픽은 인터넷을 통해 Azure Storage 공용 엔드포인트에 복제됩니다. 또는 [Microsoft 피어 링](../expressroute/expressroute-circuit-peerings.md#microsoftpeering)과 함께 Azure express 경로를 사용할 수 있습니다. 온-프레미스 사이트에서 Azure로의 사이트 간 VPN(가상 사설망)을 통한 트래픽 복제는 지원되지 않습니다.
3. 초기 복제 작업을 수행 하면 복제 사용 시 컴퓨터의 전체 데이터가 Azure로 전송 됩니다. 초기 복제가 완료되면 Azure에 대한 델타 변경 내용의 복제가 시작됩니다. 머신의 추적된 변경 내용이 프로세스 서버로 전송됩니다.
4. 다음과 같이 통신이 발생합니다.

    - VM은 복제 관리를 위해 HTTPS 443 인바운드 포트에 대한 온-프레미스 구성 서버와 통신합니다.
    - 구성 서버는 HTTPS 443 아웃바운드 포트를 통해 Azure를 사용하는 복제를 오케스트레이션합니다.
    - VM은 HTTPS 9443 인바운드 포트의 프로세스 서버(구성 서버 컴퓨터에서 실행)로 복제 데이터를 전송합니다. 이 포트는 수정할 수 있습니다.
    - 프로세스 서버는 복제 데이터를 수신 하 고, 최적화 하 고, 암호화 하 고 포트 443 아웃 바운드를 통해 Azure storage로 보냅니다.
5. 복제 데이터 로그는 먼저 Azure의 캐시 저장소 계정에 저장 됩니다. 이러한 로그는 처리 되 고 데이터는 Azure 관리 디스크 (asr seed disk)에 저장 됩니다. 이 디스크에는 복구 지점이 생성 됩니다.

![VMware에서 Azure로 복제 프로세스를 표시 하는 다이어그램입니다.](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="resynchronization-process"></a>다시 동기화 프로세스

1. 경우에 따라서 초기 복제 또는 델타 변경 내용을 전송 하는 동안 원본 컴퓨터와 프로세스 서버 간 또는 프로세스 서버와 Azure 간의 네트워크 연결에 문제가 있을 수 있습니다. 이러한 방법 중 하나를 통해 Azure로 데이터를 전송 하는 동안 오류가 발생할 수 있습니다.
2. 데이터 무결성 문제를 방지 하 고 데이터 전송 비용을 최소화 하기 위해 컴퓨터를 다시 동기화 하도록 컴퓨터를 표시 Site Recovery.
3. 또한 원본 컴퓨터와 Azure에 저장 된 데이터 간의 일관성을 유지 하기 위해 다음과 같은 상황에서 컴퓨터를 다시 동기화 하도록 표시할 수 있습니다.
    - 컴퓨터가 강제 종료 되는 경우
    - 컴퓨터에서 디스크 크기 조정과 같이 구성 변경을 수행 하는 경우 (디스크 크기를 2tb에서 4 TB로 수정)
4. 다시 동기화는 델타 데이터만 Azure에 보냅니다. 원본 컴퓨터와 Azure에 저장 된 데이터 간의 데이터 체크섬을 계산 하 여 온-프레미스와 Azure 간의 데이터 전송을 최소화 합니다.
5. 기본적으로 다시 동기화는 업무 시간 이외에 실행되도록 예약됩니다. 일정대로 기본 다시 동기화 시간까지 기다릴 수 없으면 수동으로 VM을 다시 동기화할 수 있습니다. 이렇게 하려면 Azure Portal로 이동 하 고 VM > 다시 **동기화**를 선택 합니다.
6. 업무 시간 외에 기본 다시 동기화가 실패 하 고 수동 개입이 필요한 경우 Azure Portal에서 특정 컴퓨터에 오류가 생성 됩니다. 오류를 해결 하 고 다시 동기화를 수동으로 트리거할 수 있습니다.
7. 다시 동기화가 완료 되 면 델타 변경 내용의 복제가 다시 시작 됩니다.

## <a name="failover-and-failback-process"></a>장애 조치 및 장애 복구 프로세스

복제가 설정되고 재해 복구 훈련(테스트 장애 조치)을 실행하여 모든 것이 예상대로 작동하는지 확인한 후 필요에 따라 장애 조치와 장애 복구를 실행할 수 있습니다.

1. 단일 컴퓨터에서 장애 조치(Failover)를 수행하거나 여러 VM을 동시에 장애 조치(Failover)할 복구 계획을 만들 수 있습니다. 단일 컴퓨터 장애 조치(Failover) 이외의 복구 계획을 사용할 때의 이점은 다음과 같습니다.
    - 단일 복구 계획에 앱의 모든 VM을 포함하여 앱 종속성을 모델링할 수 있습니다.
    - 스크립트, Azure Runbook 및 수동 작업에 대한 일시 중지를 추가할 수 있습니다.
2. 초기 장애 조치를 트리거한 후 이를 커밋하여 Azure VM에서 워크로드 액세스를 시작합니다.
3. 기본 온-프레미스 사이트를 다시 사용할 수 있는 경우 장애 복구(Failback)를 준비할 수 있습니다. 장애 복구(Failback)를 위해서는 다음을 포함한 장애 복구 인프라를 설정해야 합니다.

    * **Azure의 임시 프로세스 서버**: Azure에서 장애 복구하려면 Azure에서 복제를 처리하는 프로세스 서버로 작동하도록 Azure VM을 설정합니다. 장애 복구를 완료한 후 이 VM을 삭제할 수 있습니다.
    * **VPN 연결**: 장애 복구하려면 Azure 네트워크에서 온-프레미스 사이트로의 VPN 연결(또는 ExpressRoute)이 필요합니다.
    * **별도의 마스터 대상 서버**: 기본적으로 구성 서버와 함께 온-프레미스 VMware VM에 설치된 마스터 대상 서버에서 장애 복구를 처리합니다. 대량 트래픽을 장애 복구해야 하는 경우 별도의 온-프레미스 마스터 대상 서버를 이 용도로 설정합니다.
    * **장애 복구 정책**: 온-프레미스 사이트에 다시 복제하려면 장애 복구 정책이 필요합니다. 이 정책은 온-프레미스에서 Azure로의 복제 정책을 만들 때 자동으로 만들어졌습니다.
4. 구성 요소를 배치한 후 다음 3가지 동작에 따라 장애 복구(Failback)가 발생합니다.

    - 1단계: Azure VM을 다시 보호하여 Azure에서 다시 온-프레미스 VMware VM으로의 복제하도록 합니다.
    -  2단계: 온-프레미스 사이트에서 장애 조치를 실행합니다.
    - 3단계: 워크로드가 장애 복구되면 온-프레미스 VM에 대한 복제를 다시 사용하도록 설정합니다.
    
 

![Azure에서 VMware 장애 복구를 보여 주는 다이어그램](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>다음 단계

[이 자습서](vmware-azure-tutorial.md)를 따라 VMware와 Azure 간의 복제를 활성화합니다.
