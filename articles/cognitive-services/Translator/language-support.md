---
title: 언어 지원-번역기
titleSuffix: Azure Cognitive Services
description: Cognitive Services Translator는 NMT (신경망 변환)를 사용 하 여 텍스트를 텍스트로 변환 하기 위한 다음 언어를 지원 합니다.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 06/10/2020
ms.author: swmachan
ms.openlocfilehash: 9c745395026b8b7e8c58fcb4b7cc67971d971a7c
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89300224"
---
# <a name="language-and-region-support-for-text-and-speech-translation"></a>텍스트 및 음성 변환에 대 한 언어 및 지역 지원

Translator를 사용 하 여 70 개 이상의 텍스트 번역 언어로 번역 합니다. NMT (신경망 변환)는 고품질 AI 기반 컴퓨터 번역을 위한 새로운 표준 이며, 신경망을 사용할 수 있을 때 변환기의 V3을 사용 하 여 기본값으로 사용할 수 있습니다.

또한 사용자 지정 변환기와 함께 번역기를 사용 하 여 사용자의 비즈니스 및 업계에서 사용 되는 용어를 이해 하 고 Microsoft Speech Service를 사용 하 여 앱에 음성 번역을 추가 하는 신경망을 빌드할 수 있습니다.

[기계 번역 작동 방식에 대해 자세히 알아보기](https://www.microsoft.com/translator/mt.aspx)

## <a name="text-translation"></a>텍스트 번역
텍스트 번역은 변환기에서 사용 가능한 모든 언어로 변환 작업을 통해 사용할 수 있습니다. 또한 API는 검색 작업을 사용 하 여 언어 검색을 제공 하 고, 음 차 작업을 사용 하 여 음을, 사전 조회 및 사전 예제 작업을 사용 하 여 사전을 제공 합니다. 이러한 각 작업에 사용할 수 있는 언어는 다음과 같습니다. 

### <a name="translate"></a>번역

Translator는 텍스트-텍스트 번역을 위해 다음과 같은 언어를 지원 합니다. 

[변환 작업 참조 문서 보기](reference/v3-0-translate.md)

|언어|  언어 코드|
|:-----|:-----:|
|아프리칸스어| `af`|
|아랍어|    `ar`    |
|벵골어|    `bn`    |
|보스니아어(라틴 문자)|   `bs`    |
|불가리아어| `bg`    |
|광둥어(번체)|   `yue`|
|카탈로니아어|   `ca`    |
|중국어 간체|    `zh-Hans`|
|중국어 번체|   `zh-Hant`       |
|크로아티아어|  `hr`    |
|체코어| `cs`    |
|다리어|  `prs`   |
|덴마크어|    `da`        |
|네덜란드어| `nl`|
|영어|   `en`    |
|에스토니아어|  `et`    |
|피지어|    `fj`    |
|필리핀어|  `fil`   |
|핀란드어|   `fi`    |
|프랑스어|    `fr`    |
|독일어|    `de`    |
|그리스어| `el`    |
|구자라트어|  `gu`    |
|아이티 크리올|    `ht`        |
|히브리어 |`he`   |
|힌디어| `hi`    |
|몽 다오어| `mww`   |
|헝가리어| `hu`    |
|아이슬란드어| `is`    |
|인도네시아어|    `id`    |
|아일랜드어 | `ga`|
|이탈리아어|   `it`    |
|일본어|  `ja`    |
|칸나다어|`kn`|
|카자흐어|`kk`|
|스와힐리어| `sw`    |
|클링곤어|   `tlh-Latn`  |
|클링곤어(plqaD)|   `tlh-Piqd`  |
|한국어 |`ko`   |
|쿠르드어 (중부)  |`ku`   |
|쿠르드어 (북부) |`kmr`  |
|라트비아어|   `lv`    |
|리투아니아어|    `lt`    |
|마다가스카르어|  `mg`    |
|말레이어| `ms`        |
|말라얄람어| `ml` |
|몰타어|   `mt`    |
|마오리어| `mi`  |
|마라티어| `mr`  |
|노르웨이어| `nb`    |
|오리야어|  `or`    |
|파슈토어|    `ps`    |
|페르시아어|   `fa`    |
|폴란드어|    `pl`    |
|포르투갈어(브라질)|   `pt-br` |
|포르투갈어(포르투갈)| `pt-pt` |
|펀잡어|`pa`|
|케레타로 오토미어|   `otq`   |
|루마니아어|  `ro`    |
|러시아어|   `ru`    |
|사모아어|    `sm`    |
|세르비아어(키릴 자모)|    `sr-Cyrl`|
|세르비아어(라틴 문자)|   `sr-Latn`       |
|슬로바키아어|    `sk`    |
|슬로베니아어| `sl`    |
|스페인어|   `es`    |
|스웨덴어|   `sv`    |
|타히티어|  `ty`    |
|타밀어| `ta`    |
|텔루구어|    `te`    |
|태국어|  `th`    |
|통가어|    `to`    |
|터키어|   `tr`        |
|우크라이나어| `uk`    |
|우르두어|  `ur`    |
|베트남어|    `vi`    |
|웨일스어| `cy`    |
|유카텍 마야어|  `yua`   |

> [!NOTE]
> 언어 코드 `pt` 는 기본적으로 `pt-br` 포르투갈어 (브라질)로 바뀝니다.

### <a name="detect"></a>Detect

Translator는 변환 및 음에 대해 다음과 같은 언어를 검색 합니다.

[작업 참조 검색 설명서 보기](reference/v3-0-detect.md)

|언어|  언어 코드|
|:-----|:-----:|
|아프리칸스어| `af`|
|아랍어|    `ar`    |
|불가리아어| `bg`    |
|카탈로니아어|   `ca`    |
|중국어 간체|    `zh-Hans`|
|중국어 번체|   `zh-Hant`       |
|크로아티아어|  `hr`    |
|체코어| `cs`    |
|덴마크어|    `da`        |
|네덜란드어| `nl`|
|영어|   `en`    |
|에스토니아어|  `et`    |
|핀란드어|   `fi`    |
|프랑스어|    `fr`    |
|독일어|    `de`    |
|그리스어| `el`    |
|구자라트어|  `gu`    |
|아이티 크리올|    `ht`        |
|히브리어 |`he`   |
|힌디어| `hi`    |
|헝가리어| `hu`    |
|아이슬란드어| `is`    |
|인도네시아어|    `id`    |
|아일랜드어 | `ga`|
|이탈리아어|   `it`    |
|일본어|  `ja`    |
|스와힐리어| `sw`    |
|클링곤어|   `tlh-Latn`  |
|한국어 |`ko`   |
|쿠르드어 (중부)  |`ku-Arab`  |
|라트비아어|   `lv`    |
|리투아니아어|    `lt`    |
|말레이어| `ms`        |
|몰타어|   `mt`    |
|노르웨이어| `nb`    |
|파슈토어|    `ps`    |
|페르시아어|   `fa`    |
|폴란드어|    `pl`    |
|포르투갈어(브라질)|   `pt-br` |
|포르투갈어(포르투갈)| `pt-pt` |
|루마니아어|  `ro`    |
|러시아어|   `ru`    |
|세르비아어(키릴 자모)|    `sr-Cyrl`|
|세르비아어(라틴 문자)|   `sr-Latn`       |
|슬로바키아어|    `sk`    |
|슬로베니아어| `sl`    |
|스페인어|   `es`    |
|스웨덴어|   `sv`    |
|타히티어|  `ty`    |
|태국어|  `th`    |
|터키어|   `tr`        |
|우크라이나어| `uk`    |
|우르두어|  `ur`    |
|베트남어|    `vi`    |
|웨일스어| `cy`    |
|유카텍 마야어|  `yua`   |

### <a name="transliterate"></a>Transliterate

음역 방법은 다음과 같은 언어를 지원합니다. "대상/원본"에서 "<-->"은 언어가 나열된 스크립트에서/로 음역될 수 있음을 나타냅니다. "-->"은 언어가 한 스크립트에서 다른 스크립트로만 음역될 수 있음을 나타냅니다.

[음 차 작업 참조 설명서 보기](reference/v3-0-translate.md)


| 언어    | 언어 코드 | 스크립트 | 끝/시작 | 스크립트|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| 아랍어 | `ar` | 아랍어 `Arab` | <--> | 라틴어 `Latn` |
| 벵골어  | `bn` | 벵골어 `Beng` | <--> | 라틴어 `Latn` |
| 중국어(간체) | `zh-Hans` | 중국어 간체 `Hans`| <--> | 라틴어 `Latn` |
| 중국어(간체) | `zh-Hans` | 중국어 간체 `Hans`| <--> | 중국어 번체 `Hant`|
| 중국어(번체) | `zh-Hant` | 중국어 번체 `Hant`| <--> | 라틴어 `Latn` |
| 중국어(번체) | `zh-Hant` | 중국어 번체 `Hant`| <--> | 중국어 간체 `Hans` |
| 구자라트어 | `gu`  | 구자라트어 `Gujr` | <--> | 라틴어 `Latn` |
| 히브리어 | `he` | 히브리어 `Hebr` | <--> | 라틴어 `Latn` |
| 힌디어 | `hi` | 데바나가리어 `Deva` | <--> | 라틴어 `Latn` |
| 일본어 | `ja` | 일본어 `Jpan` | <--> | 라틴어 `Latn` |
| 칸나다어 | `kn` | 칸나다어 `Knda` | <--> | 라틴어 `Latn` |
| 말라얄람어 | `ml` | 말라얄람어 `Mlym` | <--> | 라틴어 `Latn` |
| 마라티어 | `mr` | 데바나가리어 `Deva` | <--> | 라틴어 `Latn` |
| 오리야어 | `or` | 오리야어 `Orya` | <--> | 라틴어 `Latn` |
| 펀잡어 | `pa` | 굴묵키어 `Guru`  | <--> | 라틴어 `Latn`  |
| 세르비아어(키릴 자모) | `sr-Cyrl` | 키릴 자모 `Cyrl`  | --> | 라틴어 `Latn` |
| 세르비아어(라틴 문자) | `sr-Latn` | 라틴어 `Latn` | --> | 키릴 자모 `Cyrl`|
| 타밀어 | `ta` | 타밀어 `Taml` | <--> | 라틴어 `Latn` |
| 텔루구어 | `te` | 텔루구어 `Telu` | <--> | 라틴어 `Latn` |
| 태국어 | `th` | 태국어 `Thai` | --> | 라틴어 `Latn` |

### <a name="dictionary"></a>Dictionary

사전은 조회 및 예제 방법을 사용하여 다음 언어를 영어로 또는 영어를 다음 언어로 나타내도록 지원합니다.

[사전 조회](reference/v3-0-dictionary-lookup.md) 및 [사전 예제](reference/v3-0-dictionary-examples.md) 작업에 대 한 참조 설명서를 봅니다.

| 언어    | 언어 코드 |
|:----------- |:-------------:|
| 아프리칸스어      | `af`          |
| 아랍어       | `ar`          |
| 벵골어      | `bn`          |
| 보스니아어(라틴 문자)      | `bs`          |
| 불가리아어      | `bg`          |
| 카탈로니아어      | `ca`          |
| 중국어 간체      | `zh-Hans`          |
| 크로아티아어      | `hr`          |
| 체코어      | `cs`          |
| 덴마크어      | `da`          |
| 네덜란드어      | `nl`          |
| 에스토니아어      | `et`          |
| 핀란드어      | `fi`          |
| 프랑스어      | `fr`          |
| 독일어      | `de`          |
| 그리스어      | `el`          |
| 아이티 크리올      | `ht`          |
| 히브리어      | `he`          |
| 힌디어      | `hi`          |
| 몽 다오어      | `mww`          |
| 헝가리어      | `hu`          |
| 아이슬란드어    | `is`  |
| 인도네시아어      | `id`          |
| 이탈리아어      | `it`          |
| 일본어      | `ja`          |
| 스와힐리어      | `sw`          |
| 클링곤어      | `tlh`          |
| 한국어      | `ko`          |
| 라트비아어      | `lv`          |
| 리투아니아어      | `lt`          |
| 말레이어      | `ms`          |
| 몰타어      | `mt`          |
| 노르웨이어      | `nb`          |
| 페르시아어      | `fa`          |
| 폴란드어      | `pl`          |
| 포르투갈어(브라질)     | `pt-br`          |
| 루마니아어      | `ro`          |
| 러시아어      | `ru`          |
| 세르비아어(라틴 문자)      | `sr-Latn`          |
| 슬로바키아어     | `sk`          |
| 슬로베니아어      | `sl`          |
| 스페인어      | `es`          |
| 스웨덴어      | `sv`          |
| 타밀어      | `ta`          |
| 태국어      | `th`          |
| 터키어      | `tr`          |
| 우크라이나어      | `uk`          |
| 우르두어      | `ur`          |
| 베트남어      | `vi`          |
| 웨일스어      | `cy`          |

### <a name="access-the-translator-language-list-programmatically"></a>프로그래밍 방식으로 번역기 언어 목록 액세스

언어 메서드를 사용 하 여 번역기에 대해 지원 되는 언어 목록을 검색할 수 있습니다. 영어 또는 지원되는 다른 언어의 언어 이름 뿐만 아니라 기능, 언어 코드별로 목록을 볼 수 있습니다. 이 목록은 새 언어를 사용할 수 있을 때 Microsoft Translator 서비스에서 자동으로 업데이트됩니다.

[언어 작업 참조 설명서 보기](reference/v3-0-languages.md)

## <a name="customization"></a>사용자 지정

[사용자 지정 변환기](https://aka.ms/CustomTranslator)를 사용 하 여 영어로 사용자 지정 하는 데 사용할 수 있는 언어는 다음과 같습니다.

| 언어    | 언어 코드 |
|:----------- |:-------------:|
|아프리칸스어| `af`|
| 아랍어       | `ar`          |
| 벵골어      | `bn`          |
| 보스니아어(라틴 문자)      | `bs`          |
| 불가리아어      | `bg`          |
|카탈로니아어|   `ca`    |
| 중국어 간체      | `zh-Hans`          |
|중국어 번체|   `zh-Hant`   |
| 크로아티아어      | `hr`          |
| 체코어      | `cs`          |
| 덴마크어      | `da`          |
| 네덜란드어      | `nl`          |
| 영어    | `en`     |
| 에스토니아어      | `et`          |
|피지어|    `fj`    |
|필리핀어|  `fil`   |
| 핀란드어      | `fi`          |
| 프랑스어      | `fr`          |
| 독일어      | `de`          |
| 그리스어      | `el`          |
| 구자라트어| `gu`    |
| 히브리어      | `he`          |
| 힌디어      | `hi`          |
| 헝가리어      | `hu`          |
| 아이슬란드어 | `is` |
| 인도네시아어|   `id`    |
| 아일랜드어 | `ga`  |
| 이탈리아어      | `it`          |
| 일본어      | `ja`          |
|칸나다어|`kn`|
| 스와힐리어|    `sw`    |
| 한국어      | `ko`          |
| 라트비아어      | `lv`          |
| 리투아니아어      | `lt`          |
| 마다가스카르어| `mg`    |
| 말레이어|    `ms`        |
|몰타어|   `mt`    |
| 마오리어| `mi`  |
| 마라티어| `mr`  |
| 노르웨이어      | `nb`          |
| 페르시아어      | `fa`          |
| 폴란드어      | `pl`          |
| 포르투갈어(브라질) | `pt-br` |
| 펀잡어|`pa`|
| 루마니아어      | `ro`          |
| 러시아어      | `ru`          |
| 사모아어|   `sm`    |
| 세르비아어(라틴 문자)      | `sr-Latn`          |
| 슬로바키아어     | `sk`          |
| 슬로베니아어      | `sl`          |
| 스페인어      | `es`          |
| 스웨덴어      | `sv`          |
|타히티어|  `ty`    |
| 태국어      | `th`          |
|통가어|    `to`    |
| 터키어      | `tr`          |
| 우크라이나어      | `uk`          |
| 우르두어| `ur`    |
| 베트남어      | `vi`          |
| 웨일스어 | `cy` |

## <a name="speech-translation"></a>Speech Translation
음성 변환은 Cognitive Services 음성 서비스와 함께 번역기를 사용 하 여 사용할 수 있습니다. 음성 번역을 사용 하는 방법에 대해 자세히 알아보고 [사용 가능한 모든 언어 옵션](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support)을 보려면 [음성 서비스 설명서](https://docs.microsoft.com/azure/cognitive-services/speech-service/) 를 참조 하세요.

### <a name="speech-to-text"></a>음성 텍스트 변환
선택한 텍스트 언어로 번역 하기 위해 음성을 텍스트로 변환 합니다. 음성 텍스트 변환은 음성 합성과 함께 사용 될 경우 음성 변환 또는 음성 변환에 사용 됩니다.

| 언어    |
|:----------- |
|아랍어|
|광둥어(번체)|
|카탈로니아어|
|중국어 간체|
|중국어 번체|
|덴마크어|
|네덜란드어|
|영어|
|핀란드어|
|프랑스어|
|독일어|
|구자라트어|
|힌디어|
|이탈리아어|
|일본어|
|한국어|
|마라티어|
|노르웨이어|
|폴란드어|
|포르투갈어(브라질)|
|포르투갈어(포르투갈)|
|러시아어|
|스페인어|
|스웨덴어|
|타밀어|
|텔루구어|
|태국어|
|터키어|

### <a name="text-to-speech"></a>텍스트 음성 변환
텍스트를 음성으로 변환 합니다. 텍스트 음성 변환은 번역 결과의 가청 출력을 추가 하거나 음성-텍스트와 함께 사용 하는 경우 음성-음성 변환에 사용 됩니다. 

| 언어    |
|:----------- |
|아랍어|
|불가리아어|
|광둥어(번체)|
|카탈로니아어|
|중국어 간체|
|중국어 번체|
|크로아티아어|
|체코어|
|덴마크어|
|네덜란드어|
|영어|
|핀란드어|
|프랑스어|
|독일어|
|그리스어|
|히브리어|
|힌디어|
|헝가리어|
|인도네시아어|
|이탈리아어|
|일본어|
|한국어|
|말레이어|
|노르웨이어|
|폴란드어|
|포르투갈어(브라질)|
|포르투갈어(포르투갈)|
|루마니아어|
|러시아어|
|슬로바키아어|
|슬로베니아어|
|스페인어|
|스웨덴어|
|타밀어|
|텔루구어|
|태국어|
|터키어|
|베트남어|

## <a name="view-the-language-list-on-the-microsoft-translator-website"></a>Microsoft Translator 웹 사이트에서 언어 목록 보기

언어에 대 한 간략 한 개요를 보려면 Microsoft Translator 웹 사이트에서 음성 번역을 위해 텍스트 번역 및 음성 서비스에 대해 번역기에서 지 원하는 모든 언어를 표시 합니다. 이 목록에는 언어 코드와 같은 개발자별 정보는 포함되지 않습니다.

[언어 목록을 참조하세요](https://www.microsoft.com/translator/languages.aspx).
