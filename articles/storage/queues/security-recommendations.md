---
title: Queue storage에 대 한 보안 권장 사항
titleSuffix: Azure Storage
description: Queue storage에 대 한 보안 권장 사항에 대해 알아봅니다. 이 지침을 구현 하면 공유 책임 모델에 설명 된 대로 보안 의무를 달성 하는 데 도움이 됩니다.
services: storage
author: tamram
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: tamram
ms.custom: security-recommendations
ms.openlocfilehash: 70bb96da858c94fbd2c75d56cda4e705f2ffa3ba
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90986630"
---
# <a name="security-recommendations-for-queue-storage"></a>Queue storage에 대 한 보안 권장 사항

이 문서에는 Queue storage에 대 한 보안 권장 사항이 포함 되어 있습니다. 이러한 권장 사항을 구현하면 공유 책임 모델에 설명된 대로 보안 의무를 충족하는 데 도움이 됩니다. Microsoft에서 서비스 공급자 책임을 충족하기 위해 수행하는 작업에 대한 자세한 내용은 [클라우드 컴퓨팅에 대한 공유 책임](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/225366/1/Shared%20Responsibility%20for%20Cloud%20Computing-2019-10-25.pdf)을 참조하세요.

이 문서에 포함된 일부 권장 사항은 Azure Security Center에서 자동으로 모니터링할 수 있습니다. Azure Security Center는 Azure에서 리소스를 보호하는 첫 번째 방어선입니다. Azure Security Center에 대 한 자세한 내용은 [Azure Security Center 무엇입니까?](../../security-center/security-center-intro.md)를 참조 하세요.

Azure Security Center는 Azure 리소스의 보안 상태를 주기적으로 분석 하 여 잠재적인 보안 취약성을 식별 합니다. 그런 다음 이를 해결하는 방법에 대한 권장 사항을 제공합니다. Azure Security Center 권장 사항에 대한 자세한 내용은 [Azure Security Center의 보안 권장 사항](../../security-center/security-center-recommendations.md)을 참조하세요.

## <a name="data-protection"></a>데이터 보호

| 권장 | 주석 | Security Center |
|-|----|--|
| Azure Resource Manager 배포 모델 사용 | Azure Resource Manager 배포 모델을 사용 하 여 새로운 저장소 계정을 만듭니다 .이를 통해 RBAC (고급 액세스 제어) 및 감사, 리소스 관리자 기반 배포 및 거 버 넌 스, 관리 되는 id에 대 한 액세스, 암호에 대 한 Azure Key Vault 액세스, Azure AD 기반 인증 및 Azure Storage 데이터 및 리소스에 대 한 액세스 권한 부여를 비롯 한 중요 한 보안 향상을 위해 가능 하면 클래식 배포 모델을 사용 하는 기존 저장소 계정을 Azure Resource Manager를 사용 하 여 마이그레이션합니다. Azure Resource Manager에 대 한 자세한 내용은 [Azure Resource Manager 개요](/azure/azure-resource-manager/resource-group-overview)를 참조 하세요. | - |
| 모든 저장소 계정에 대해 advanced threat protection 사용 | Azure Storage에 대 한 Advanced threat protection은 저장소 계정에 액세스 하거나 악용 하려는 비정상적이 고 잠재적으로 유해한 시도를 감지 하는 추가 보안 인텔리전스 계층을 제공 합니다. 보안 경고는 활동의 비정상 상황에서 발생 하며, 의심 스러운 활동의 세부 정보와 위협 조사 및 해결 방법에 대 한 권장 사항을 포함 하 여 전자 메일을 통해 구독 관리자에 게 전송 되는 경우에 Azure Security Center 트리거됩니다. 자세한 내용은 [Azure Storage에 대 한 Advanced threat protection](../common/azure-defender-storage-configure.md)을 참조 하세요. | [예](../../security-center/security-center-sql-service-recommendations.md) |
| SAS (공유 액세스 서명) 토큰을 HTTPS 연결로만 제한 | 클라이언트에서 SAS 토큰을 사용 하 여 큐 데이터에 액세스 하는 경우 도청의 위험을 최소화 하는 데 도움이 되는 HTTPS 필요 자세한 내용은 [SAS (공유 액세스 서명)를 사용 하 여 Azure Storage 리소스에 대 한 제한 된 액세스 권한 부여](../common/storage-sas-overview.md)를 참조 하세요. | - |

## <a name="identity-and-access-management"></a>ID 및 액세스 관리

| 권장 | 주석 | Security Center |
|-|----|--|
| Azure AD (Azure Active Directory)를 사용 하 여 큐 데이터에 대 한 액세스 권한 부여 | Azure AD는 큐 저장소에 대 한 요청에 권한을 부여 하기 위해 공유 키를 통해 뛰어난 보안과 사용 편의성을 제공 합니다. 자세한 내용은 [Azure Active Directory를 사용 하 여 Azure blob 및 큐에 대 한 액세스 권한 부여](../common/storage-auth-aad.md)를 참조 하세요. | - |
| RBAC를 통해 Azure AD 보안 주체에 사용 권한을 할당 하는 경우 최소 권한의 원칙을 염두에 두어야 합니다. | 사용자, 그룹 또는 응용 프로그램에 역할을 할당 하는 경우 해당 보안 주체에 게 해당 작업을 수행 하는 데 필요한 권한만 부여 합니다. 리소스에 대 한 액세스를 제한 하면 의도 하지 않은 데이터의 악의적인 오용을 방지할 수 있습니다. | - |
| Azure Key Vault를 사용 하 여 계정 액세스 키 보호 | Microsoft는 Azure AD를 사용 하 여 Azure Storage 요청에 권한을 부여 하는 것을 권장 합니다. 그러나 공유 키 인증을 사용 해야 하는 경우에는 Azure Key Vault를 사용 하 여 계정 키를 보호 해야 합니다. 응용 프로그램을 사용 하 여 키를 저장 하는 대신 런타임에 키 자격 증명 모음에서 키를 검색할 수 있습니다. | - |
| 정기적으로 계정 키 다시 생성 | 정기적으로 계정 키를 회전 하면 악의적인 행위자에 게 데이터를 노출 하는 위험을 줄일 수 있습니다. | - |
| SAS에 사용 권한을 할당 하는 경우 최소 권한의 원칙을 염두에 두어야 합니다. | SAS를 만들 때 클라이언트에서 해당 기능을 수행 하는 데 필요한 사용 권한만 지정 합니다. 리소스에 대 한 액세스를 제한 하면 의도 하지 않은 데이터의 악의적인 오용을 방지할 수 있습니다. | - |
| 클라이언트에 발급 하는 SAS에 대해 해지 계획을 준비 해야 합니다. | SAS가 손상 되 면 가능한 한 빨리 해당 SAS를 해지 하는 것이 좋습니다. 사용자 위임 SAS를 해지 하려면 사용자 위임 키를 취소 하 여 해당 키와 연결 된 모든 서명을 빠르게 무효화 합니다. 저장 된 액세스 정책과 연결 된 서비스 SAS를 해지 하려면 저장 된 액세스 정책을 삭제 하거나, 정책의 이름을 바꾸거나, 만료 시간을 과거 시간으로 변경할 수 있습니다. 자세한 내용은 [SAS (공유 액세스 서명)를 사용 하 여 Azure Storage 리소스에 대 한 제한 된 액세스 권한 부여](../common/storage-sas-overview.md)를 참조 하세요.  | - |
| 서비스 SAS가 저장 된 액세스 정책과 연결 되지 않은 경우 만료 시간을 1 시간 이하로 설정 합니다. | 저장 된 액세스 정책과 연결 되지 않은 서비스 SAS는 취소할 수 없습니다. 이러한 이유로, SAS가 1 시간 이내에 유효 하도록 만료 시간을 제한 하는 것이 좋습니다. | - |

## <a name="networking"></a>네트워킹

| 권장 | 주석 | Security Center |
|-|----|--|
| 저장소 계정에 대 한 TLS (Transport Layer Security)의 최소 필수 버전을 구성 합니다.  | 해당 계정에 대 한 최소 버전의 TLS를 구성 하 여 Azure Storage 계정에 대 한 요청을 수행 하려면 클라이언트가 보다 안전한 버전의 TLS를 사용 해야 합니다. 자세한 내용은 [저장소 계정에 대 한 TLS (Transport Layer Security)의 최소 필수 버전 구성](../common/transport-layer-security-configure-minimum-version.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) 을 참조 하세요.| - |
| 모든 저장소 계정에 대해 **보안 전송 필요** 옵션을 사용 하도록 설정 합니다. | **보안 전송 필요** 옵션을 사용 하도록 설정 하는 경우 저장소 계정에 대해 수행 된 모든 요청은 보안 연결을 통해 수행 되어야 합니다. HTTP를 통해 수행 된 모든 요청은 실패 합니다. 자세한 내용은 [Azure Storage에서 보안 전송 필요](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)를 참조 하세요. | [예](../../security-center/security-center-sql-service-recommendations.md) |
| 방화벽 규칙 사용 | 저장소 계정에 대 한 액세스를 지정 된 IP 주소 또는 범위에서 시작 되는 요청 또는 Azure Virtual Network (VNet)의 서브넷 목록에서 제한 하도록 방화벽 규칙을 구성 합니다. 방화벽 규칙을 구성 하는 방법에 대 한 자세한 내용은 [Azure Storage 방화벽 및 가상 네트워크 구성](../common/storage-network-security.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)을 참조 하세요. | - |
| 신뢰할 수 있는 Microsoft 서비스에서 저장소 계정에 액세스 하도록 허용 | 스토리지 계정에 대해 방화벽 규칙을 켜면 기본적으로 Azure VNet(Virtual Network) 내에서 작동하는 서비스 또는 허용되는 퍼블릭 IP 주소에서 요청하지 않는 한, 들어오는 데이터 요청이 차단됩니다. 차단되는 요청에는 다른 Azure 서비스, Azure Portal, 로깅 및 메트릭 서비스 등이 포함됩니다. 신뢰할 수 있는 Microsoft 서비스에서 저장소 계정에 액세스할 수 있도록 허용 하는 예외를 추가 하 여 다른 Azure 서비스의 요청을 허용할 수 있습니다. 신뢰할 수 있는 Microsoft 서비스에 대 한 예외를 추가 하는 방법에 대 한 자세한 내용은 [Azure Storage 방화벽 및 가상 네트워크 구성](../common/storage-network-security.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)을 참조 하세요.| - |
| 프라이빗 엔드포인트 사용 | 개인 끝점은 Azure Virtual Network (VNet)에서 저장소 계정으로 개인 IP 주소를 할당 합니다. 개인 링크를 통해 VNet과 저장소 계정 간의 모든 트래픽을 보호 합니다. 개인 끝점에 대 한 자세한 내용은 [Azure 개인 끝점을 사용 하 여 전용으로 저장소 계정에 연결](../../private-link/create-private-endpoint-storage-portal.md)을 참조 하세요. | - |
| VNet 서비스 태그 사용 | 서비스 태그는 지정된 Azure 서비스의 IP 주소 접두사 그룹을 나타냅니다. Microsoft는 서비스 태그에 포함되는 주소 접두사를 관리하고 주소가 변경되면 서비스 태그를 자동으로 업데이트합니다. Azure Storage에서 지 원하는 서비스 태그에 대 한 자세한 내용은 [Azure 서비스 태그 개요](../../virtual-network/service-tags-overview.md)를 참조 하세요. 서비스 태그를 사용 하 여 아웃 바운드 네트워크 규칙을 만드는 방법을 보여 주는 자습서는 [PaaS 리소스에 대 한 액세스 제한](../../virtual-network/tutorial-restrict-network-access-to-resources.md)을 참조 하세요. | - |
| 특정 네트워크에 대 한 네트워크 액세스 제한 | 액세스를 요구 하는 클라이언트를 호스팅하는 네트워크에 대 한 네트워크 액세스를 제한 하면 네트워크 공격에 대 한 리소스 노출을 줄일 수 있습니다. | [예](../../security-center/security-center-sql-service-recommendations.md) |

## <a name="loggingmonitoring"></a>로깅/모니터링

| 권장 | 주석 | Security Center |
|-|----|--|
| 요청에 대 한 권한을 부여 하는 방법 추적 | Azure Storage 로깅을 사용 하 여 Azure Storage에 대해 만들어진 각 요청에 대 한 권한이 부여 되었는지 추적 합니다. 로그는 OAuth 2.0 토큰을 사용 하거나 공유 키를 사용 하거나 공유 액세스 서명 (SAS)을 사용 하 여 요청이 익명으로 수행 되었는지 여부를 나타냅니다. 자세한 내용은 [Azure Storage analytics 로깅](../common/storage-analytics-logging.md)을 참조 하세요. | - |

## <a name="next-steps"></a>다음 단계

- [Azure 보안 설명서](https://docs.microsoft.com//azure/security/)
- [보안 개발 설명서](https://docs.microsoft.com/azure/security/develop/)입니다.
