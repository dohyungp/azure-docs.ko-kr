---
title: 확장 리소스 종류
description: 다른 리소스 유형의 기능을 확장 하는 데 사용 되는 Azure 리소스 유형을 나열 합니다.
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 8b80c63d361f3ad8199fd669178f7bf88dabe02e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90969753"
---
# <a name="resource-types-that-extend-capabilities-of-other-resources"></a>다른 리소스의 기능을 확장 하는 리소스 종류

확장 리소스는 다른 리소스의 기능에 추가 되는 리소스입니다. 예를 들어 리소스 잠금은 확장 리소스입니다. 리소스 잠금을 삭제 하거나 수정 하지 못하도록 다른 리소스에 적용 합니다. 리소스 잠금을 직접 만드는 것은 적절 하지 않습니다. 확장 리소스는 항상 다른 리소스에 적용 됩니다.

## <a name="extension-resource-types"></a>확장 리소스 종류

- Microsoft Advisor/구성
- Microsoft Advisor/권장 사항
- Microsoft Advisor/비 표시 오류
- AlertsManagement/경고
- AlertsManagement/alertsSummary
- Microsoft. Authorization/accessReviewScheduleDefinitions
- Microsoft. Authorization/accessReviewScheduleSettings
- Microsoft. Authorization/checkAccess
- Microsoft. Authorization/denyAssignments
- Microsoft. Authorization/findOrphanRoleAssignments
- Microsoft.Authorization/locks
- Microsoft. 권한 부여/사용 권한
- Microsoft.Authorization/policyAssignments
- Microsoft. Authorization/policyDefinitions
- Microsoft. Authorization/policyExemptions
- Microsoft. Authorization/policySetDefinitions
- Microsoft. Authorization/privateLinkAssociations
- Microsoft.Authorization/roleAssignments
- Microsoft. Authorization/roleAssignmentsUsageMetrics
- Microsoft. Authorization/roleDefinitions
- Microsoft. automanage/configurationprofil
- Microsoft. 청구/billingPeriods
- Microsoft. 청구/billingPermissions
- Microsoft. 청구/billingRoleAssignments
- Microsoft. 청구/billingRoleDefinitions
- Microsoft. 청구/createBillingRoleAssignment
- Microsoft. 청사진/blueprintAssignments
- Microsoft. 청사진/청사진
- Microsoft. ChangeAnalysis/resourceChanges
- Microsoft 소비량/AggregatedCost
- Microsoft 소비량/잔액
- Microsoft 소비량/예산
- Microsoft 소비량/요금
- Microsoft 소비량/CostTags
- Microsoft 소비량/크레딧
- Microsoft 사용량과 이벤트
- Microsoft의 소비/예측
- Microsoft 사용량과 많은
- Microsoft 소비량/마켓플레이스
- Microsoft의 소비율/OperationResults
- Microsoft 사용/OperationStatus
- Microsoft 소비량/Pricesheets
- Microsoft. 소비/제품
- Microsoft 소비량/ReservationDetails
- Microsoft 소비량/ReservationRecommendationDetails
- Microsoft 소비량/ReservationRecommendations
- Microsoft 소비량/ReservationSummaries
- Microsoft 소비량/ReservationTransactions
- Microsoft. 소비/태그
- Microsoft 소비/테 넌 트
- Microsoft 사용량과 사용 약관
- Microsoft 소비량/사용량 세부 정보
- ContainerInstance/serviceAssociationLinks
- CostManagement/경고
- CostManagement/예산
- CostManagement/costAllocationRules
- CostManagement/차원
- CostManagement/내보내기
- CostManagement/ExternalSubscriptions
- CostManagement/예측
- CostManagement/정보
- CostManagement/쿼리
- CostManagement/Reportconfigs
- CostManagement/보고서
- CostManagement/showbackRules
- CostManagement/뷰
- Microsoft.CustomProviders/associations
- Microsoft.EventGrid/eventSubscriptions
- Microsoft.EventGrid/extensionTopics
- GuestConfiguration/configurationprofil
- GuestConfiguration/guestConfigurationAssignments
- GuestConfiguration/소프트웨어
- GuestConfiguration/Microsoft 프로필
- GuestConfiguration/\\D 업데이트
- microsoft. a s e/기준
- microsoft insights/calculatebaseline
- microsoft insights/dataCollectionRuleAssociations
- microsoft insights/diagnosticSettings
- microsoft insights/diagnosticSettingsCategories
- microsoft insights/eventtypes
- microsoft insights/extendedDiagnosticSettings
- microsoft insights/generateLiveToken
- microsoft insights/찾은 guestdiagnosticsettingsassociation
- microsoft insights/logDefinitions
- microsoft 인 사이트/로그
- microsoft insights/metricbaselines
- microsoft insights/metricDefinitions
- microsoft insights/metricNamespaces
- microsoft. 통찰력/메트릭
- microsoft insights/myWorkbooks 문서
- microsoft. a s e/토폴로지
- microsoft. a s e/트랜잭션
- microsoft insights/vmInsightsOnboardingStatuses
- KubernetesConfiguration/extensions
- KubernetesConfiguration/sourceControlConfigurations
- Microsoft. Maintenance/applyUpdates
- Microsoft. 유지 관리/configurationAssignments
- Microsoft. 유지 관리/업데이트
- Microsoft.managedidentity/Id
- Microsoft ManagedServices/registrationAssignments
- Microsoft ManagedServices/registrationDefinitions
- OperationalInsights/storageInsightConfigs
- Microsoft.operationsmanagement/managementassociations
- Microsoft PolicyInsights/증명
- Microsoft PolicyInsights/Policyinsights
- Microsoft PolicyInsights/Policyinsights
- Microsoft PolicyInsights/policyTrackedResources
- Microsoft.PolicyInsights/remediations
- Microsoft RecoveryServices/backupProtectedItems
- Microsoft RecoveryServices/replicationEligibilityResults
- Microsoft ResourceHealth/availabilityStatuses
- Microsoft ResourceHealth/childAvailabilityStatuses
- Microsoft ResourceHealth/childResources
- Microsoft ResourceHealth/이벤트
- Microsoft ResourceHealth/impactedResources
- Microsoft ResourceHealth/알림
- Microsoft .Resources/링크
- Microsoft .Resources/태그
- Microsoft. Security/adaptiveNetworkHardenings
- Microsoft. Security/advancedThreatProtectionSettings
- Microsoft. Security/assessmentMetadata
- Microsoft. 보안/평가
- Microsoft. Security/complianceResults
- Microsoft. Security/규격
- Microsoft. Security/dataCollectionAgents
- Microsoft. Security/deviceSecurityGroups
- Microsoft. Security/InformationProtectionPolicies
- Microsoft. Security/I이상 센서
- Microsoft. Security/jitPolicies
- Microsoft. Security/serverVulnerabilityAssessments
- Microsoft. Security/sqlVulnerabilityAssessments
- Microsoft SecurityInsights/집계
- Microsoft SecurityInsights/alertRules
- Microsoft SecurityInsights/alertRuleTemplates
- Microsoft SecurityInsights/automationRules
- Microsoft SecurityInsights/책갈피
- Microsoft SecurityInsights/사례
- Microsoft SecurityInsights/dataConnectors
- Microsoft SecurityInsights/dataConnectorsCheckRequirements
- Microsoft SecurityInsights/엔터티
- Microsoft SecurityInsights/entityQueries
- Microsoft SecurityInsights/인시던트
- Microsoft SecurityInsights/officeConsents
- Microsoft SecurityInsights/설정
- Microsoft SecurityInsights/threatIntelligence
- Microsoft SecurityInsights/watchlists
- SoftwarePlan/hybridUseBenefits
- Microsoft. Subscription/CreateSubscription
- microsoft. 지원/지원 티켓
- WorkloadMonitor/구성 요소
- WorkloadMonitor/monitorInstances
- WorkloadMonitor/모니터
- WorkloadMonitor/notificationSettings

## <a name="next-steps"></a>다음 단계

- Azure Resource Manager 템플릿에서 확장 리소스의 리소스 ID를 가져오려면 [Extensionresourceid](../templates/template-functions-resource.md#extensionresourceid)를 사용 합니다.
- 템플릿에 확장 리소스를 만드는 방법에 대 한 예는 [Event Grid 이벤트 구독](/azure/templates/microsoft.eventgrid/2019-06-01/eventsubscriptions)을 참조 하십시오.
