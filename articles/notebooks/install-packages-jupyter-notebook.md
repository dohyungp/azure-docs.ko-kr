---
title: Jupyter 노트북에서 패키지 설치-Azure Notebooks 미리 보기
description: 'Azure에서 실행 되는 Jupyter 노트북 내에서 Python, R 및 F # 패키지를 설치 하는 방법에 대해 알아봅니다.'
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: 43d38c9f3a4b0095967ab3e103ea729ec86ea2bd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85831219"
---
# <a name="install-packages-from-within-azure-notebooks-preview"></a>Azure Notebooks 미리 보기 내에서 패키지 설치

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

[프로젝트 수준에서 노트 환경](configure-manage-azure-notebooks-projects.md#configure-the-project-environment)을 구성할 수 있지만 개별 노트 내에서 직접 패키지를 설치할 수 있습니다.

노트에서 설치한 패키지는 현재 서버 세션에만 적용됩니다. 서버가 종료되면 패키지 설치가 유지되지 않습니다.

## <a name="python"></a>Python

Python의 패키지는 코드 셀 내에서 pip 또는 conda 명령을 사용하여 설치할 수 있습니다.

```bash
!pip install <package_name>

!conda install <package_name> -y
```

명령 출력에 요구 사항이 이미 충족된 것으로 표시될 경우 Azure Notebooks에 기본적으로 패키지가 포함될 수 있습니다. 패키지를 [프로젝트 환경 설치 단계](configure-manage-azure-notebooks-projects.md#configure-the-project-environment)를 통해 설치할 수도 있습니다.

## <a name="r"></a>R

R의 패키지는 코드 셀에서 `install.packages` 함수를 사용하여 CRAN 또는 GitHub에서 설치할 수 있습니다.

```r
install.packages("package_name")
```

또한 devtools 라이브러리를 사용하여 GitHub에서 시험판 및 기타 개발 패키지를 설치할 수도 있습니다.

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

F#의 패키지는 코드 셀 내에서 Paket 종속성 관리자를 호출하여 [nuget.org](https://www.nuget.org)에서 설치할 수 있습니다. 먼저, 다음과 같이 Paket 관리자를 로드합니다.

```fsharp
#load "Paket.fsx"
```

그런 후 패키지를 설치합니다.

```fsharp
Paket.Package
  [ "MathNet.Numerics"
    "MathNet.Numerics.FSharp"
  ]
```

그런 다음 Paket 생성기를 로드 합니다.
```fsharp
#load "Paket.Generated.Refs.fsx"
```

라이브러리를 엽니다.
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>다음 단계

- [방법: 프로젝트 구성 및 관리](configure-manage-azure-notebooks-projects.md)
- [방법: 슬라이드 쇼 표시](present-jupyter-notebooks-slideshow.md)
