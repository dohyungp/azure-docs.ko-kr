---
title: Azure Policy 준수 감사 및 관리
titleSuffix: Azure Machine Learning
description: Azure Policy를 사용 하 여 Azure Machine Learning에 대 한 기본 제공 정책을 사용 하는 방법을 알아봅니다.
author: jhirono
ms.author: jhirono
ms.date: 09/15/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.openlocfilehash: e7eebb22efe32b290e078348337049c6c3e762db
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90997204"
---
# <a name="audit-and-manage-azure-machine-learning-using-azure-policy"></a>Azure Policy를 사용 하 여 Azure Machine Learning 감사 및 관리

[Azure Policy](/azure/governance/policy) 은 Azure 리소스가 정책을 준수 하는지 확인할 수 있도록 하는 거 버 넌 스 도구입니다. Azure Machine Learning를 사용 하 여 다음 정책을 할당할 수 있습니다.

* **고객 관리 키**: 작업 영역에서 고객이 관리 하는 키를 사용 해야 하는지 여부를 감사 하거나 적용 합니다.
* **개인 링크**: 작업 영역에서 개인 끝점을 사용 하 여 가상 네트워크와 통신 하는지 여부를 감사 합니다.

구독 또는 리소스 그룹 수준 등의 여러 범위에서 정책을 설정할 수 있습니다. 자세한 내용은 [Azure Policy 설명서](/azure/governance/policy/overview)를 참조 하세요.

## <a name="built-in-policies"></a>기본 제공 정책

Azure Machine Learning는 Azure Machine Learning와 관련 된 일반적인 시나리오에 사용할 수 있는 정책 집합을 제공 합니다. 이러한 정책 정의를 기존 구독에 할당 하거나 고유한 사용자 지정 정의를 만드는 기준으로 사용할 수 있습니다. Azure Machine Learning에 대 한 기본 제공 정책의 전체 목록은 [Azure Machine Learning에 대 한 기본 제공 정책](/azure/governance/policy/samples/built-in-policies#machine-learning)을 참조 하세요.

Azure Machine Learning와 관련 된 기본 제공 정책 정의를 보려면 다음 단계를 사용 합니다.

1. [Azure Portal](https://portal.azure.com) __Azure Policy__ 으로 이동 합니다.
1. __정의__를 선택 합니다.
1. __유형__에서 _기본 제공_을 선택 하 고 __범주__에 대해 __Machine Learning__를 선택 합니다.

여기에서 정책 정의를 선택 하 여 볼 수 있습니다. 정의를 보는 동안 __할당__ 링크를 사용 하 여 특정 범위에 정책을 할당 하 고 정책에 대 한 매개 변수를 구성할 수 있습니다. 자세한 내용은 [정책 할당-포털](/azure/governance/policy/assign-policy-portal)을 참조 하세요.

[Azure PowerShell](/azure/governance/policy/assign-policy-powershell), [Azure CLI](https://docs.microsoft.com/azure/governance/policy/assign-policy-azurecli)및 [템플릿을](/azure/governance/policy/assign-policy-template)사용 하 여 정책을 할당할 수도 있습니다.

## <a name="workspaces-encryption-with-customer-managed-key"></a>고객 관리 키를 사용 하 여 작업 영역 암호화

작업 영역을 CMK (고객이 관리 하는 키)로 암호화할지 또는 Microsoft 관리 키를 사용 하 여 메트릭과 메타 데이터를 암호화 해야 하는지 여부를 제어 합니다. CMK를 사용 하는 방법에 대 한 자세한 내용은 엔터프라이즈 보안 문서의 [Azure Cosmos DB](concept-enterprise-security.md#azure-cosmos-db) 섹션을 참조 하세요.

이 정책을 구성 하려면 effect 매개 변수를 __audit__ 또는 __deny__로 설정 합니다. __Audit__로 설정 된 경우 cmk 없이 작업 영역을 만들 수 있으며 활동 로그에 경고 이벤트가 생성 됩니다.

정책이 __거부__로 설정 된 경우 cmk를 지정 하지 않으면 작업 영역을 만들 수 없습니다. CMK를 사용 하지 않고 작업 영역을 만들려고 하면와 유사한 오류가 발생 하 `Resource 'clustername' was disallowed by policy` 고 활동 로그에 오류가 생성 됩니다. 이 오류의 일부로 정책 식별자도 반환 됩니다.

## <a name="workspaces-should-use-private-link"></a>작업 영역에서 개인 링크를 사용 해야 함

작업 영역에서 azure 개인 링크를 사용 하 여 Azure Virtual Network와 통신 해야 하는지 여부를 제어 합니다. 개인 링크를 사용 하는 방법에 대 한 자세한 내용은 [작업 영역에 대 한 개인 링크 구성](how-to-configure-private-link.md)을 참조 하세요.

이 정책을 구성 하려면 effect 매개 변수를 __audit__로 설정 합니다. 개인 링크를 사용 하지 않고 작업 영역을 만드는 경우 활동 로그에 경고 이벤트가 생성 됩니다.

## <a name="next-steps"></a>다음 단계

* [Azure Policy 설명서](/azure/governance/policy/overview)
* [Azure Machine Learning에 대 한 기본 제공 정책](policy-reference.md)
* [Azure Security Center를 사용 하 여 보안 정책 작업](/azure/security-center/tutorial-security-policy)