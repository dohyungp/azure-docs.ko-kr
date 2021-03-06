---
title: 빠른 시작 - Azure Spring Cloud 서비스 프로비저닝
description: 앱 배포를 위한 Azure Spring Cloud 서비스 인스턴스를 만드는 방법을 설명합니다.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/03/2020
ms.custom: devx-track-java
ms.openlocfilehash: c91237e3a14c60e477f58be0bf62f634b462960b
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88951675"
---
# <a name="quickstart-provision-azure-spring-cloud-service"></a>빠른 시작: Azure Spring Cloud 서비스 프로비저닝

Azure Portal 또는 Azure CLI를 사용하여 Azure Spring Cloud를 인스턴스화할 수 있습니다.  두 방법 모두 다음 절차에 설명되어 있습니다.
## <a name="prerequisites"></a>사전 요구 사항

* [JDK 8 설치](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
* [Azure 구독에 가입](https://azure.microsoft.com/free/)
* (선택 사항) [Azure CLI 버전 2.0.67 이상을 설치](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)하고 `az extension add --name spring-cloud` 명령을 사용하여 Azure Spring Cloud 확장을 설치합니다.
* (선택 사항) [Azure Toolkit for IntelliJ를 설치](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/)하고 [로그인](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in)합니다.

## <a name="provision-an-instance-of-azure-spring-cloud"></a>Azure Spring Cloud의 인스턴스 프로비저닝

#### <a name="portal"></a>[포털](#tab/Azure-portal)

다음 절차에서는 Azure Portal을 사용하여 Azure Spring Cloud의 인스턴스를 만듭니다.

1. 새 탭에서 [Azure Portal](https://ms.portal.azure.com/)을 엽니다. 

2. 상단 검색 상자에서 **Azure Spring Cloud**를 검색합니다.

3. 결과에서 **Azure Spring Cloud**를 선택합니다.

    ![ASC 아이콘 시작](media/spring-cloud-quickstart-launch-app-portal/find-spring-cloud-start.png)

4. Azure Spring Cloud 페이지에서 **+ 추가**를 클릭합니다.

    ![ASC 아이콘 추가](media/spring-cloud-quickstart-launch-app-portal/spring-cloud-add.png)

5. Azure Spring Cloud **만들기** 페이지에 있는 양식을 채웁니다.  다음 지침을 고려하세요.
    - **구독**: 이 리소스 대한 요금이 청구될 구독을 선택합니다.
    - **리소스 그룹**: 새 리소스에 대한 리소스 그룹을 새로 만드는 것이 가장 좋습니다. 이는 이후 단계에서 **\<resource group name\>** 로 사용됩니다.
    - **서비스 세부 정보/이름**: **\<service instance name\>** 를 지정합니다.  이름은 4-32자 사이여야 하며, 소문자, 숫자 및 하이픈(-) 문자만 포함할 수 있습니다.  서비스 이름의 첫 글자는 문자여야 하며 마지막 문자는 문자 또는 숫자여야 합니다.
    - **위치**: 서비스 인스턴스에 대한 위치를 선택합니다.

    ![ASC 포털 시작](media/spring-cloud-quickstart-launch-app-portal/portal-start.png)

6. **검토 + 만들기**를 클릭합니다.

> [!div class="nextstepaction"]
> [문제가 발생했습니다.](https://www.research.net/r/javae2e?tutorial=asc-cli-quickstart&step=public-endpoint)

#### <a name="cli"></a>[CLI](#tab/Azure-CLI)

다음 절차에서는 Azure CLI 확장을 사용하여 Azure Spring Cloud의 인스턴스를 프로비저닝합니다.

1. Azure CLI에 로그인하고 활성 구독을 선택합니다. Azure Spring Cloud의 허용 목록에 추가된 활성 구독을 선택해야 합니다.

    ```azurecli
    az login
    az account list -o table
    az account set --subscription <Name or ID of subscription, skip if you only have 1 subscription>
    ```

1. Azure Spring Cloud 서비스의 이름을 준비합니다.  이름은 4-32자 사이여야 하며, 소문자, 숫자 및 하이픈(-) 문자만 포함할 수 있습니다.  서비스 이름의 첫 글자는 문자여야 하며 마지막 문자는 문자 또는 숫자여야 합니다.

1. Azure Spring Cloud 서비스를 포함할 리소스 그룹을 만듭니다.

    ```azurecli
    az group create --location eastus --name <resource group name>
    ```

    [Azure 리소스 그룹](../azure-resource-manager/management/overview.md)에 대해 자세히 알아봅니다.

1. Azure CLI 창을 열고 다음 명령을 실행하여 Azure Spring Cloud의 인스턴스를 프로비저닝합니다.

    ```azurecli
    az spring-cloud create -n <service instance name> -g <resource group name>
    ```

    서비스 인스턴스를 배포하는 데 약 5분이 걸립니다.
---

## <a name="next-steps"></a>다음 단계
> [!div class="nextstepaction"]
> [구성 서버 설정](spring-cloud-quickstart-setup-config-server.md)


