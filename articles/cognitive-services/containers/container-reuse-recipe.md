---
title: Docker 컨테이너에 대 한 조리법
titleSuffix: Azure Cognitive Services
description: 배포 및 다시 사용을 위해 구성 설정의 일부 또는 전부를 사용 하 여 컨테이너를 빌드, 테스트 및 저장 하는 방법을 알아봅니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 7380ff58d033a68565de7e419ff318f7bdec121d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80875081"
---
# <a name="create-containers-for-reuse"></a>재사용할 컨테이너 만들기

이러한 컨테이너 레 서를 사용 하 여 다시 사용할 수 있는 Cognitive Services 컨테이너를 만들 수 있습니다. 컨테이너를 시작할 때 필요 _하지_ 않도록 일부 또는 모든 구성 설정을 사용 하 여 컨테이너를 만들 수 있습니다.

이 새 컨테이너 계층 (설정 포함)이 있고 로컬에서 테스트 한 경우 컨테이너 레지스트리에 컨테이너를 저장할 수 있습니다. 컨테이너가 시작 되 면 현재 컨테이너에 저장 되어 있지 않은 설정만 필요 합니다. 개인 레지스트리 컨테이너는 이러한 설정을 전달할 수 있는 구성 공간을 제공 합니다.

## <a name="docker-run-syntax"></a>Docker 실행 구문

`docker run`이 문서의 모든 예제에서는 Windows 콘솔에 줄 연속 문자가 있는 것으로 가정 `^` 합니다. 사용자의 용도에 따라 다음 사항을 고려 합니다.

* Docker 컨테이너에 대해 잘 알고 있지 않은 경우 인수 순서를 변경하지 마세요.
* Windows 이외의 운영 체제 또는 Windows 콘솔 이외의 다른 운영 체제를 사용 하는 경우 콘솔 및 시스템에 올바른 콘솔/터미널, 탑재를 위한 폴더 구문 및 줄 연속 문자를 사용 합니다.  Cognitive Services 컨테이너는 Linux 운영 체제 이므로 대상 탑재는 Linux 스타일 폴더 구문을 사용 합니다.
* `docker run`예를 들어 드라이브에서 디렉터리를 사용 `c:` 하 여 Windows에서 사용 권한 충돌을 방지 합니다. 입력 디렉터리로 특정 디렉터리를 사용해야 할 경우 Docker 서비스 권한을 받아야 할 수도 있습니다.

## <a name="store-no-configuration-settings-in-image"></a>이미지에 구성 설정 저장 안 함

`docker run`각 서비스에 대 한 예제 명령은 컨테이너에 구성 설정을 저장 하지 않습니다. 콘솔 또는 레지스트리 서비스에서 컨테이너를 시작 하는 경우 이러한 구성 설정이 전달 되어야 합니다. 개인 레지스트리 컨테이너는 이러한 설정을 전달할 수 있는 구성 공간을 제공 합니다.

## <a name="reuse-recipe-store-all-configuration-settings-with-container"></a>조리법 재사용: 모든 구성 설정을 컨테이너에 저장 합니다.

모든 구성 설정을 저장 하려면 `Dockerfile` 해당 설정으로을 만듭니다.

이 방법의 문제:

* 새 컨테이너에는 원래 컨테이너와 별도의 이름과 태그가 있습니다.
* 이러한 설정을 변경 하려면 Dockerfile의 값을 변경 하 고, 이미지를 다시 빌드하고, 레지스트리에 다시 게시 해야 합니다.
* 컨테이너 레지스트리 또는 로컬 호스트에 대 한 액세스 권한이 있는 사용자는 컨테이너를 실행 하 고 Cognitive Services 끝점을 사용할 수 있습니다.
* 인식 서비스에 입력 탑재를 요구 하지 않는 경우 `COPY` Dockerfile에 줄을 추가 하지 마세요.

Dockerfile을 만들고 사용 하려는 기존 Cognitive Services 컨테이너에서 가져온 다음 Dockerfile의 docker 명령을 사용 하 여 컨테이너에 필요한 정보를 설정 하거나 가져옵니다.

이 예제는 다음과 같이 작성되었습니다.

* 을 `{BILLING_ENDPOINT}` 사용 하 여 호스트의 환경 키에서 청구 끝점을 설정 `ENV` 합니다.
* `{ENDPOINT_KEY}`' ENV '를 사용 하 여 호스트의 환경 키에서 청구 API 키를 설정 합니다.

### <a name="reuse-recipe-store-billing-settings-with-container"></a>조리법 재사용: 컨테이너를 사용 하 여 청구 설정 저장

이 예제에서는 Dockerfile에서 Text Analytics ' 감정 컨테이너를 빌드하는 방법을 보여 줍니다.

```Dockerfile
FROM mcr.microsoft.com/azure-cognitive-services/sentiment:latest
ENV billing={BILLING_ENDPOINT}
ENV apikey={ENDPOINT_KEY}
ENV EULA=accept
```

컨테이너를 [로컬로](#how-to-use-container-on-your-local-host) 빌드 및 실행 하 고 필요에 따라 [개인 레지스트리 컨테이너](#how-to-add-container-to-private-registry) 에서 실행 합니다.

### <a name="reuse-recipe-store-billing-and-mount-settings-with-container"></a>조리법 재사용: 컨테이너를 사용 하 여 청구 및 탑재 설정 저장

이 예제에서는 Dockerfile에서 청구 및 모델을 저장 하는 Language Understanding를 사용 하는 방법을 보여 줍니다.

* 를 사용 하 여 호스트의 파일 시스템에서 Language Understanding (LUIS) 모델 파일을 복사 `COPY` 합니다.
* LUIS 컨테이너는 둘 이상의 모델을 지원 합니다. 모든 모델이 동일한 폴더에 저장 된 경우 하나의 `COPY` 문만 필요 합니다.
* 모델 입력 디렉터리의 상대 부모에서 docker 파일을 실행 합니다. 다음 예에서는 `docker build` `docker run` 의 상대 부모에서 및 명령을 실행 합니다 `/input` . 명령의 첫 번째는 `/input` `COPY` 호스트 컴퓨터의 디렉터리입니다. 두 번째는 `/input` 컨테이너의 디렉터리입니다.

```Dockerfile
FROM <container-registry>/<cognitive-service-container-name>:<tag>
ENV billing={BILLING_ENDPOINT}
ENV apikey={ENDPOINT_KEY}
ENV EULA=accept
COPY /input /input
```

컨테이너를 [로컬로](#how-to-use-container-on-your-local-host) 빌드 및 실행 하 고 필요에 따라 [개인 레지스트리 컨테이너](#how-to-add-container-to-private-registry) 에서 실행 합니다.

## <a name="how-to-use-container-on-your-local-host"></a>로컬 호스트에서 컨테이너를 사용 하는 방법

Docker 파일을 빌드하려면를 `<your-image-name>` 이미지의 새 이름으로 바꾸고 다음을 사용 합니다.

```console
docker build -t <your-image-name> .
```

이미지를 실행 하 고 컨테이너가 중지 될 때 제거 합니다 ( `--rm` ).

```console
docker run --rm <your-image-name>
```

## <a name="how-to-add-container-to-private-registry"></a>개인 레지스트리에 컨테이너를 추가 하는 방법

Dockerfile을 사용 하 여 개인 컨테이너 레지스트리에 새 이미지를 추가 하려면 다음 단계를 수행 합니다.  

1. `Dockerfile`조리법 다시 사용의 텍스트를 사용 하 여를 만듭니다. 에는 `Dockerfile` 확장명이 없습니다.

1. 꺾쇠 괄호의 모든 값을 고유한 값으로 바꿉니다.

1. 다음 명령을 사용 하 여 명령줄 또는 터미널에서 이미지에 파일을 빌드합니다. 꺾쇠 괄호의 값을 `<>` 사용자 고유의 컨테이너 이름 및 태그로 바꿉니다.  

    태그 옵션인는 `-t` 컨테이너에 대해 변경한 내용에 대 한 정보를 추가 하는 방법입니다. 예를 들어의 컨테이너 이름은 `modified-LUIS` 원래 컨테이너가 계층화 되었음을 나타냅니다. 태그 이름은 `with-billing-and-model` Language Understanding (LUIS) 컨테이너가 수정 된 방법을 나타냅니다.

    ```Bash
    docker build -t <your-new-container-name>:<your-new-tag-name> .
    ```

1. 콘솔에서 Azure CLI에 로그인 합니다. 이 명령은 브라우저를 열고 인증을 요구 합니다. 인증 된 후에는 브라우저를 닫고 콘솔에서 작업을 계속할 수 있습니다.

    ```azurecli
    az login
    ```

1. 콘솔에서 Azure CLI를 사용 하 여 개인 레지스트리에 로그인 합니다.

    꺾쇠 괄호의 값을 `<my-registry>` 고유한 레지스트리 이름으로 바꿉니다.  

    ```azurecli
    az acr login --name <my-registry>
    ```

    서비스 사용자가 할당 된 경우 docker 로그인을 사용 하 여 로그인 할 수도 있습니다.

    ```Bash
    docker login <my-registry>.azurecr.io
    ```

1. 개인 레지스트리 위치를 사용 하 여 컨테이너에 태그를 태그 합니다. 꺾쇠 괄호의 값을 `<my-registry>` 고유한 레지스트리 이름으로 바꿉니다. 

    ```Bash
    docker tag <your-new-container-name>:<your-new-tag-name> <my-registry>.azurecr.io/<your-new-container-name-in-registry>:<your-new-tag-name>
    ```

    태그 이름을 사용 하지 않는 경우 `latest` 가 암시 됩니다.

1. 새 이미지를 개인 컨테이너 레지스트리에 푸시합니다. 개인 컨테이너 레지스트리를 볼 때 다음 CLI 명령에 사용 되는 컨테이너 이름이 리포지토리의 이름이 됩니다.

    ```Bash
    docker push <my-registry>.azurecr.io/<your-new-container-name-in-registry>:<your-new-tag-name>
    ```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Container Instance 만들기 및 사용](azure-container-instance-recipe.md)

<!--
## Store input and output configuration settings

Bake in input params only

FROM containerpreview.azurecr.io/microsoft/cognitive-services-luis:<tag>
COPY luisModel1 /input/
COPY luisModel2 /input/

## Store all configuration settings

If you are a single manager of the container, you may want to store all settings in the container. The new, resulting container will not need any variables passed in to run. 

Issues with this approach:

* In order to change these settings, you will have to change the values of the Dockerfile and rebuild the file. 
* If someone gets access to your container registry or your local host, they can run the container and use the Cognitive Services endpoints. 

The following _partial_ Dockerfile shows how to statically set the values for billing and model. This example uses the 

```Dockerfile
FROM <container-registry>/<cognitive-service-container-name>:<tag>
ENV billing=<billing value>
ENV apikey=<apikey value>
COPY luisModel1 /input/
COPY luisModel2 /input/
```

->