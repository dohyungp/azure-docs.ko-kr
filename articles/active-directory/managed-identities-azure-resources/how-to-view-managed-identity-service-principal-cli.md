---
title: 관리 id의 서비스 사용자 보기-Azure CLI-Azure AD
description: Azure CLI를 사용하여 관리 ID의 서비스 주체를 보기 위한 단계별 지침입니다.
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
ms.date: 11/29/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurecli
ms.openlocfilehash: b8eec72666eadf90a401dc8f0adb77df77dbf782
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90969299"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-azure-cli"></a>Azure CLI를 사용하여 관리 ID의 서비스 주체 보기

Azure 리소스에 대한 관리 ID는 Azure Active Directory에서 자동으로 관리 ID를 Azure 서비스에 제공합니다. 이 ID를 사용하면 Azure AD 인증을 지원하는 모든 서비스에 인증할 수 있으므로 코드에 자격 증명을 포함할 필요가 없습니다. 

이 문서에서는 Azure CLI를 사용하여 관리 ID의 서비스 주체를 보는 방법을 알아봅니다.

## <a name="prerequisites"></a>필수 구성 요소

- Azure 리소스에 대한 관리 ID를 잘 모르는 경우 [개요 섹션](overview.md)을 확인하세요.
- 아직 Azure 계정이 없는 경우 [체험 계정에 가입](https://azure.microsoft.com/free/)합니다.
- [가상 머신](./qs-configure-portal-windows-vm.md#system-assigned-managed-identity) 또는 [애플리케이션에서 시스템 할당 ID](../../app-service/overview-managed-identity.md#add-a-system-assigned-identity)를 사용하도록 설정합니다.
- 예제 스크립트를 실행 하기 위해 두 가지 옵션이 있습니다.
    - 코드 블록의 오른쪽 위 모퉁이에 있는 **사용해 보기** 단추를 사용 하 여 열 수 있는 [Azure Cloud Shell](../../cloud-shell/overview.md)을 사용 합니다.
    - 최신 버전의 [Azure CLI](/cli/azure/install-azure-cli)를 설치한 다음 [az login](/cli/azure/reference-index#az-login)을 사용 하 여 Azure에 로그인 하 여 스크립트를 로컬로 실행 합니다. 리소스를 만들려는 Azure 구독과 연결 된 계정을 사용 합니다.   

## <a name="view-the-service-principal"></a>서비스 주체 보기

다음 명령은 관리 ID가 사용하도록 설정된 VM 또는 애플리케이션의 서비스 주체를 보는 방법을 보여 줍니다. `<VM or application name>`을 고유한 값으로 바꿉니다. 

```azurecli-interactive
az ad sp list --display-name <VM or application name>
```

## <a name="next-steps"></a>다음 단계

Azure CLI를 사용하여 Azure AD 서비스 주체를 관리하는 방법에 대한 자세한 내용은 [az ad sp](/cli/azure/ad/sp)를 참조하세요.
