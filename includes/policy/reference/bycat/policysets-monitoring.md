---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/16/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: f039b88dd629b437c17a2f08fe04faff0a993074
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90984529"
---
|Name |Description |정책 |버전 |
|---|---|---|---|
|[Virtual Machine Scale Sets에 Azure Monitor 사용](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |지정된 범위(관리 그룹, 구독 또는 리소스 그룹)에서 Virtual Machine Scale Sets에 대해 Azure Monitor를 사용하도록 설정합니다. Log Analytics 작업 영역을 매개 변수로 사용합니다. 참고: 확장 집합 upgradePolicy를 수동으로 설정한 경우 이에 대한 업그레이드를 호출하여 집합의 모든 VM에 확장을 적용해야 합니다. CLI에서 이는 az vmss update-instances입니다. |6 |1.0.1 |
|[VM용 Azure Monitor 사용](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |지정된 범위(관리 그룹, 구독 또는 리소스 그룹)에서 VM(가상 머신)에 대해 Azure Monitor를 사용하도록 설정합니다. Log Analytics 작업 영역을 매개 변수로 사용합니다. |10 |2.0.0 |
