---
title: Azure CLI를 사용 하 여 리소스에 관리 id 액세스 할당-Azure AD
description: Azure CLI를 사용 하 여 한 리소스에 관리 되는 id를 할당 하 고 다른 리소스에 액세스 하는 방법에 대 한 단계별 지침입니다.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2017
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurecli
ms.openlocfilehash: 13c69dda1e300bcff95b6a017fdeb308a6bbf3a4
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90969260"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-azure-cli"></a>Azure CLI를 사용하여 리소스에 관리 ID 액세스 권한 할당

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

관리 ID를 사용하여 Azure 리소스를 구성하면 모든 보안 주체와 마찬가지로 다른 리소스에 관리 ID 액세스 권한을 제공할 수 있습니다. 이 예제에서는 Azure CLI를 사용하여 Azure 가상 머신 또는 가상 머신 확장 집합의 관리 ID 액세스 권한을 Azure 스토리지 계정에 부여하는 방법을 보여 줍니다.

## <a name="prerequisites"></a>필수 구성 요소

- Azure 리소스에 대한 관리 ID를 잘 모르는 경우 [개요 섹션](overview.md)을 확인하세요. **[시스템 할당 ID와 사용자 할당 관리 ID의 차이점](overview.md#managed-identity-types)을 반드시 검토하세요**.
- 아직 Azure 계정이 없으면 계속하기 전에 [평가판 계정](https://azure.microsoft.com/free/)에 등록해야 합니다.
- 예제 스크립트를 실행 하기 위해 두 가지 옵션이 있습니다.
    - 코드 블록의 오른쪽 위 모퉁이에 있는 **사용해 보기** 단추를 사용 하 여 열 수 있는 [Azure Cloud Shell](../../cloud-shell/overview.md)을 사용 합니다.
    - 최신 버전의 [Azure CLI](/cli/azure/install-azure-cli)를 설치한 다음 [az login](/cli/azure/reference-index#az-login)을 사용 하 여 Azure에 로그인 하 여 스크립트를 로컬로 실행 합니다. 리소스를 만들려는 Azure 구독과 연결 된 계정을 사용 합니다.

## <a name="use-azure-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Azure RBAC를 사용 하 여 다른 리소스에 관리 되는 id 액세스 권한 부여

[Azure 가상 머신](qs-configure-cli-windows-vm.md) 또는 [Azure 가상 머신 확장 집합](qs-configure-cli-windows-vmss.md)과 같은 Azure 리소스에서 관리 ID를 사용하도록 설정하면 다음을 수행합니다. 

1. 다음 예제에서는 스토리지 계정 액세스 권한을 Azure 가상 머신에 부여합니다. 먼저 [az resource list](/cli/azure/resource/#az-resource-list)를 사용하여 myVM이라는 가상 머신에 대한 서비스 주체를 가져옵니다.

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Azure 가상 머신 확장 집합의 경우 "DevTestVMSS"라는 가상 머신 확장 집합에 대한 서비스 사용자를 가져오는 것을 제외하고는 명령이 동일합니다.
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

1. 서비스 사용자 ID를 가져오면 [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create)를 사용하여 가상 머신 또는 가상 머신 확장 집합 "읽기 권한자" 액세스 권한을 "myStorageAcct"라는 스토리지 계정에 부여합니다.

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="next-steps"></a>다음 단계

- [Azure 리소스에 대한 관리 ID 개요](overview.md)
- Azure 가상 머신에서 관리 ID를 사용하려면 [Azure CLI를 사용하여 Azure VM에서 Azure 리소스에 대한 관리 ID 구성](qs-configure-cli-windows-vm.md)을 참조하세요.
- Azure 가상 머신에서 관리 ID를 사용하려면 [Azure CLI를 사용하여 가상 머신 확장 집합에서 Azure 리소스에 대한 관리 ID 구성](qs-configure-cli-windows-vmss.md)을 참조하세요.
