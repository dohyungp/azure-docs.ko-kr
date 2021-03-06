---
title: 모든 Azure Key Vault에 대해 사용하도록 설정되는 일시 삭제 | Microsoft Docs
description: 이 문서를 사용하여 일시 삭제를 모든 키 자격 증명 모음에 적용합니다.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 07/27/2020
ms.author: sudbalas
ms.openlocfilehash: c5509d6a284ab7afe827f67b79b7be027e76f66c
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90068849"
---
# <a name="soft-delete-will-be-enabled-on-all-key-vaults"></a>모든 키 자격 증명 모음에 대해 사용하도록 설정되는 일시 삭제

> [!WARNING]
> **호환성이 손상되는 변경**: 일시 삭제를 옵트아웃하는 기능은 연말에 더 이상 사용되지 않으며, 모든 키 자격 증명 모음에 대해 일시 삭제 보호가 자동으로 설정됩니다.  Azure Key Vault 사용자와 관리자는 즉시 키 자격 증명 모음에 대해 일시 삭제를 사용하도록 설정해야 합니다.

키 자격 증명 모음에 대해 일시 삭제 보호를 사용하지 않고 비밀을 삭제하면 해당 비밀이 영구적으로 삭제됩니다. 사용자는 현재 키 자격 증명 모음을 만드는 동안 일시 삭제를 옵트아웃할 수 있지만, 사용자의 실수로 또는 악의적으로 삭제하지 못하도록 비밀을 보호하기 위해 Microsoft는 곧 **모든** 키 자격 증명 모음에 대해 일시 삭제 보호를 사용하도록 설정하며, 사용자는 더 이상 일시 삭제를 옵트아웃하거나 해제할 수 있는 옵션을 사용할 수 없습니다.

:::image type="content" source="../media/softdeletediagram.png" alt-text="<대체 텍스트>":::

일시 삭제 기능에 대한 자세한 내용은 [Azure Key Vault 일시 삭제 개요](soft-delete-overview.md)를 참조하세요.

## <a name="how-do-i-respond-to-breaking-changes"></a>호환성이 손상되는 변경에 대응하는 방법

키 자격 증명 모음 개체는 일시 삭제된 상태의 키 자격 증명 모음 개체와 동일한 이름으로 만들 수 없습니다.  예를 들어 키 자격 증명 모음 A에서 `test key`라는 키를 삭제하는 경우 일시 삭제된 `test key` 개체가 제거될 때까지 키 자격 증명 모음 A에서 `test key`라는 새 키를 만들 수 없습니다.

### <a name="application-changes"></a>애플리케이션 변경

애플리케이션에서 일시 삭제를 사용하도록 설정하지 않는다고 가정하고 삭제된 비밀 또는 키 자격 증명 모음 이름을 즉시 다시 사용할 수 있다고 예상하는 경우 이 변경 내용을 적용하려면 애플리케이션 논리를 다음과 같이 변경해야 합니다.

1. 원래 키 자격 증명 모음 또는 비밀을 삭제합니다.
2. 일시 삭제된 상태의 키 자격 증명 모음 또는 비밀을 제거합니다.
3. 대기 – 즉시 다시 만들면 충돌이 발생할 수 있습니다.
4. 키 자격 증명 모음을 동일한 이름으로 다시 만듭니다.
5. 다시 시도를 구현합니다. 만들기 작업에서 이름 충돌 오류가 계속 발생하는 경우 최악의 시나리오에서 DNS 레코드를 업데이트하는 데 최대 10분이 걸릴 수 있습니다.

### <a name="administration-changes"></a>관리 변경

비밀을 영구적으로 삭제할 수 있는 액세스 권한이 필요한 보안 주체는 이러한 비밀과 키 자격 증명 모음을 제거할 수 있는 추가 액세스 정책 권한을 부여받아야 합니다.

일시 삭제를 해제하도록 위임하는 Azure Policy가 키 자격 증명 모음에 있는 경우 이 정책을 사용하지 않도록 설정해야 합니다.  환경에 적용된 Azure Policy를 제어하는 관리자에게 이 문제를 에스컬레이션해야 할 수 있습니다. 이 정책을 사용하지 않도록 설정하면 적용된 정책의 범위에서 새 키 자격 증명 모음을 만들 수 없게 됩니다.

조직에서 법적 규정 준수 요구 사항을 따르고 있고 삭제된 키 자격 증명 모음과 비밀을 연장 기간 동안 복구 가능한 상태로 유지하도록 허용할 수 없는 경우 일시 삭제 보존 기간을 조직의 표준에 맞게 조정해야 합니다(7-90일 사이에서 구성 가능).

## <a name="procedures"></a>프로시저

### <a name="audit-your-key-vaults-to-check-if-soft-delete-is-enabled"></a>키 자격 증명 모음을 감사하여 일시 삭제를 사용하도록 설정되었는지 확인

1. Azure 포털에 로그인합니다.
2. "Azure Policy"를 검색합니다.
3. "정의"를 선택합니다.
4. [범주] 아래의 필터에서 "Key Vault"를 선택합니다.
5. "Key Vault 개체를 복구할 수 있어야 함" 정책을 선택합니다.
6. "할당"을 클릭합니다.
7. 범위를 구독으로 설정합니다.
8. "검토 + 만들기"를 선택합니다.
9. 전체 환경 검색을 완료하는 데 최대 24시간이 걸릴 수 있습니다.
10. Azure Policy 블레이드에서 "규정 준수"를 클릭합니다.
11. 적용한 정책을 선택합니다.

이제 일시 삭제를 사용하도록 설정된 키 자격 증명 모음(준수 리소스) 및 일시 삭제를 사용하도록 설정하지 않은 키 자격 증명 모음(비준수 리소스)을 필터링하여 확인할 수 있습니다.

### <a name="turn-on-soft-delete-for-an-existing-key-vault"></a>기존 키 자격 증명 모음에 대해 일시 삭제 설정

1. Azure 포털에 로그인합니다.
2. Key Vault를 검색합니다.
3. 설정 아래에서 "속성"을 선택합니다.
4. [일시 삭제] 아래에서 "이 자격 증명 모음 및 해당 개체 복구 사용"에 해당하는 라디오 단추를 선택합니다.
5. 일시 삭제 보존 기간을 설정합니다.
6. “저장”을 선택합니다.

### <a name="grant-purge-access-policy-permissions-to-a-security-principal"></a>보안 주체에 제거 액세스 정책 권한 부여

1. Azure 포털에 로그인합니다.
2. Key Vault를 검색합니다.
3. 설정 아래에서 "액세스 정책"을 선택합니다.
4. 액세스 권한을 부여하려는 서비스 주체를 선택합니다.
5. 키, 비밀 및 인증서 권한 아래의 각 드롭다운에 대해 "권한 있는 작업"까지 아래로 스크롤하여 "제거" 권한을 선택합니다.

## <a name="frequently-asked-questions"></a>질문과 대답

### <a name="does-this-change-affect-me"></a>이 변경이 내게 영향을 주나요?

일시 삭제를 이미 설정했거나 키 자격 증명 모음 개체를 삭제하고 동일한 이름으로 다시 만들지 않으면 키 자격 증명 모음의 동작이 변경되지 않을 수 있습니다.

동일한 명명 규칙을 사용하여 키 자격 증명 모음 개체를 자주 삭제하고 다시 만드는 애플리케이션이 있는 경우 필요한 동작을 유지하려면 애플리케이션 논리를 변경해야 합니다. 위의 "호환성이 손상되는 변경에 대응하는 방법" 섹션을 참조하세요.

### <a name="how-do-i-benefit-from-this-change"></a>이 변경으로 인해 얻을 수 있는 이점은 무엇인가요?

일시 삭제 보호는 실수로 또는 악의적으로 삭제되지 않도록 방지하는 추가적인 보호 계층을 조직에 제공합니다. 키 자격 증명 모음 관리자는 복구 권한 및 제거 권한 모두에 대한 액세스를 제한할 수 있습니다.

사용자가 실수로 키 자격 증명 모음 또는 비밀을 삭제하는 경우 비밀 또는 키 자격 증명 모음을 영구적으로 삭제하는 위험 없이 비밀 자체를 복구할 수 있는 액세스 권한을 부여할 수 있습니다. 이 셀프 서비스 프로세스는 환경의 가동 중지 시간을 최소화하고 비밀의 가용성을 보장합니다.

### <a name="how-do-i-find-out-if-i-need-to-take-action"></a>작업을 수행해야 하는지 여부를 확인하려면 어떻게 해야 하나요?

"키 자격 증명 모음을 감사하여 일시 삭제가 설정되었는지 확인하는 절차" 섹션에 나와 있는 위의 단계를 따르세요. 일시 삭제가 설정되지 않은 키 자격 증명 모음은 이 변경의 영향을 받습니다. 감사하는 데 도움이 되는 추가 도구는 곧 제공될 예정입니다. 그러면 이 문서가 업데이트됩니다.

### <a name="what-action-do-i-need-to-take"></a>수행해야 하는 작업은 무엇인가요?

애플리케이션 논리를 변경할 필요가 없는지 확인합니다. 확인되었으면 모든 키 자격 증명 모음에 대해 일시 삭제를 설정합니다. 이렇게 하면 연말에 모든 키 자격 증명 모음에 대해 일시 삭제를 설정할 때 호환성이 손상되는 변경의 영향을 받지 않습니다.

### <a name="by-when-do-i-need-to-take-action"></a>언제까지 이 작업을 수행해야 하나요?

연말까지 모든 키 자격 증명 모음에 대해 일시 삭제가 설정됩니다. 애플리케이션에서 영향을 받지 않도록 하려면 가능한 한 빨리 키 자격 증명 모음에 대해 일시 삭제를 설정합니다.

## <a name="what-will-happen-if-i-dont-take-any-action"></a>이 작업을 수행하지 않으면 어떻게 되나요?

이 작업을 수행하지 않으면 연말에 모든 키 자격 증명 모음에 대해 일시 삭제가 자동으로 설정됩니다. 그러면 먼저 일시 삭제된 상태에서 제거하지 않고 키 자격 증명 모음 개체를 삭제하고 동일한 이름으로 다시 만들려고 시도하는 경우 충돌 오류가 발생할 수 있습니다. 이로 인해 애플리케이션 또는 자동화가 실패할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- 이 변경과 관련된 질문이 있는 경우 [akvsoftdelete@microsoft.com](mailto:akvsoftdelete@microsoft.com)에서 문의합니다.
- [일시 삭제 개요](soft-delete-overview.md)를 참조합니다.
