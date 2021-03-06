---
title: Azure Synapse Link에서 주문형 SQL을 사용 하 여 Azure Cosmos DB 데이터 쿼리 (미리 보기)
description: 이 문서에서는 Azure Synapse Link (미리 보기)에서 SQL 주문형을 사용 하 여 Azure Cosmos DB를 쿼리 하는 방법에 대해 알아봅니다.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 09/15/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: c64a42c66a3b1c1810c17347e18979d599b36b6f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90938463"
---
# <a name="query-azure-cosmos-db-data-using-sql-on-demand-in-azure-synapse-link-preview"></a>Azure Synapse Link에서 주문형 SQL을 사용 하 여 Azure Cosmos DB 데이터 쿼리 (미리 보기)

SQL server 서버를 사용 하지 않는 경우 (이전에는 SQL 주문형) 트랜잭션 워크 로드의 성능에 영향을 주지 않고 [Azure Synapse Link](../../cosmos-db/synapse-link.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 로 설정 된 Azure Cosmos DB 컨테이너의 데이터를 거의 실시간으로 분석할 수 있습니다. [분석 저장소](../../cosmos-db/analytical-store-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 에서 데이터를 쿼리 하는 친숙 한 T-sql 구문과 t-sql 인터페이스를 통한 광범위 한 BI 및 임시 쿼리 도구에 대 한 통합 연결을 제공 합니다.

> [!NOTE]
> SQL 주문형 Azure Cosmos DB 분석 저장소 쿼리에 대 한 지원은 현재 제어 된 미리 보기로 제공 됩니다. 

Azure Cosmos DB를 쿼리 하는 경우 대부분의 [SQL 함수 및 연산자](overview-features.md)를 포함 하 여 전체 [선택](/sql/t-sql/queries/select-transact-sql.md?view=sql-server-ver15&preserve-view=true) 노출 영역이 [OPENROWSET](develop-openrowset.md) 함수를 통해 지원 됩니다. Azure Blob Storage Azure Data Lake Storage 또는 [create external table as select](develop-tables-cetas.md#cetas-in-sql-on-demand)를 사용 하 여 데이터와 함께 Azure Cosmos DB에서 데이터를 읽는 쿼리 결과를 저장할 수도 있습니다. 현재 [CETAS](develop-tables-cetas.md#cetas-in-sql-on-demand)를 사용 하 여 AZURE COSMOS DB에 SQL 요청 시 쿼리 결과를 저장할 수 없습니다.

이 문서에서는 Synapse 링크를 사용 하는 Azure Cosmos DB 컨테이너에서 데이터를 쿼리 하는 SQL 주문형을 사용 하 여 쿼리를 작성 하는 방법을 알아봅니다. 그런 다음 Azure Cosmos DB 컨테이너를 통해 SQL 주문형 뷰를 빌드하고 [이](./tutorial-data-analyst.md) 자습서의 Power BI 모델에 연결 하는 방법에 대해 자세히 알아볼 수 있습니다. 

## <a name="overview"></a>개요

Azure Cosmos DB 분석 저장소에서 데이터 쿼리 및 분석을 지원 하기 위해 SQL 주문형은 다음 구문을 사용 합니다 `OPENROWSET` .

```sql
OPENROWSET( 
       'CosmosDB',
       '<Azure Cosmos DB connection string>',
       <Container name>
    )  [ < with clause > ]
```

Azure Cosmos DB 연결 문자열은 Azure Cosmos DB 계정 이름, 데이터베이스 이름, 데이터베이스 계정 마스터 키 및 작동 하는 선택적 영역 이름을 지정 합니다 `OPENROWSET` . 연결 문자열의 형식은 다음과 같습니다.
```sql
'account=<database account name>;database=<database name>;region=<region name>;key=<database account master key>'
```

구문에서 따옴표 없이 Azure Cosmos DB 컨테이너 이름이 지정 됩니다 `OPENROWSET` . 컨테이너 이름에 특수 문자 (예: 대시 '-')가 있는 경우이 이름은 `[]` 구문에서 (대괄호) 안에 래핑됩니다 `OPENROWSET` .

> [!NOTE]
> 주문형 SQL은 트랜잭션 저장소 Azure Cosmos DB 쿼리를 지원 하지 않습니다.

## <a name="sample-data-set"></a>샘플 데이터 세트

이 문서의 예는에 대 한 유럽 센터의 데이터를 기반으로 합니다 [(예: 질병 방지 및 제어 (ECDC) COVID-19 사례](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) 및 [Covid-19 Open Research 데이터 집합 (코드-19), doi: 10.5281/zenodo. 3715505)](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/). 

이러한 페이지에서 데이터의 라이선스와 구조를 확인 하 고 [Ecdc](https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json) 및 [Cord19](https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json) 데이터 집합에 대 한 샘플 데이터를 다운로드할 수 있습니다.

이 문서와 함께 보여주는 SQL을 사용 하 여 Cosmos DB 데이터를 쿼리 하는 방법에 대 한 자세한 내용을 확인 하려면 다음 리소스를 만들어야 합니다.
* [Synapse Link가 설정](../../cosmos-db/configure-synapse-link.md) 된 Azure Cosmos DB 데이터베이스 계정
* 이라는 Azure Cosmos DB 데이터베이스 `covid`
* 라는 두 개의 Azure Cosmos DB 컨테이너 `EcdcCases` 와 `Cord19` 위의 예제 데이터 집합을 로드 했습니다.

## <a name="explore-azure-cosmos-db-data-with-automatic-schema-inference"></a>자동 스키마 유추를 사용 하 여 Azure Cosmos DB 데이터 탐색

Azure Cosmos DB에서 데이터를 탐색 하는 가장 쉬운 방법은 자동 스키마 유추 기능을 활용 하는 것입니다. 문에서 절을 생략 하면 `WITH` `OPENROWSET` SQL 요청 시 Azure Cosmos DB 컨테이너의 분석 저장소 스키마를 자동으로 검색 (유추) 하도록 지시할 수 있습니다.

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases) as documents
```
위의 예제에서는 `covid` `MyCosmosDbAccount` Azure Cosmos DB 키 (위 예제의 더미)를 사용 하 여 인증 된 Azure Cosmos DB 계정에서 SQL 요청 시 데이터베이스에 연결 하도록 지시 합니다. 그런 다음 해당 지역의 컨테이너 분석 저장소에 액세스 합니다 `EcdcCases` `West US 2` . 특정 속성의 프로젝션이 없으므로 `OPENROWSET` 함수는 Azure Cosmos DB 항목의 모든 속성을 반환 합니다.

동일한 Azure Cosmos DB 데이터베이스의 다른 컨테이너에서 데이터를 탐색 해야 하는 경우 동일한 연결 문자열을 사용 하 고 세 번째 매개 변수로 필요한 컨테이너를 참조할 수 있습니다.

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19) as cord19
```

## <a name="explicitly-specify-schema"></a>명시적으로 스키마 지정

의 자동 스키마 유추 기능은 `OPENROWSET` 간단 하 고 사용 하기 쉬운 querience 제공 하지만, 비즈니스 시나리오에서는 Azure Cosmos DB 데이터의 관련 속성만 읽도록 스키마를 명시적으로 지정 해야 할 수 있습니다.

`OPENROWSET` 컨테이너의 데이터에서 읽을 속성을 명시적으로 지정 하 고 해당 데이터 형식을 지정할 수 있습니다. 다음 구조를 포함 하는 [Ecdc COVID 데이터 집합](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) 의 일부 데이터를 Azure Cosmos DB으로 가져왔습니다.

```json
{"date_rep":"2020-08-13","cases":254,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-12","cases":235,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-11","cases":163,"countries_and_territories":"Serbia","geo_id":"RS"}
```

Azure Cosmos DB의 이러한 플랫 JSON 문서를 Synapse SQL의 행 및 열 집합으로 표시할 수 있습니다. `OPENROWSET` 함수를 사용 하면 절에서 읽을 속성의 하위 집합 및 정확한 열 형식을 지정할 수 있습니다 `WITH` .

```sql
SELECT TOP 10 *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       EcdcCases
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```

이 쿼리의 결과는 다음과 같을 수 있습니다.

| date_rep | cases | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

Azure Cosmos DB 값에 사용 해야 하는 SQL 형식에 대 한 자세한 내용은이 문서의 끝에 있는 [sql 형식 매핑 규칙](#azure-cosmos-db-to-sql-type-mappings) 을 검토 하세요.

## <a name="querying-nested-objects-and-arrays"></a>중첩 된 개체 및 배열 쿼리

Azure Cosmos DB를 사용 하면 중첩 된 개체 또는 배열로 작성 하 여 더 복잡 한 데이터 모델을 나타낼 수 있습니다. Azure Cosmos DB에 대 한 Synapse Link의 자동 동기화 기능은 분석 저장소에서 스키마 표현을 관리 하는 데 사용할 수 있습니다 .이는 중첩 된 데이터 형식 처리를 포함 하 여 SQL 요청 시 다양 한 쿼리를 허용 합니다.

예를 들어 [코드-19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) 데이터 집합에는 다음 구조를 따라 JSON 문서가 있습니다.

```json
{
    "paper_id": <str>,                   # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": <array of objects>    # list of author dicts, in order
        ...
     }
     ...
}
```

Azure Cosmos DB의 중첩 된 개체와 배열은 함수가 읽을 때 쿼리 결과에 JSON 문자열로 표시 됩니다 `OPENROWSET` . Sql 열에서 sql 열로 이러한 복합 형식의 값을 읽는 한 가지 옵션은 SQL JSON 함수를 사용 하는 것입니다.

```sql
SELECT
    title = JSON_VALUE(metadata, '$.title'),
    authors = JSON_QUERY(metadata, '$.authors'),
    first_author_name = JSON_VALUE(metadata, '$.authors[0].first')
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( metadata varchar(MAX) ) AS docs;
```

이 쿼리의 결과는 다음과 같을 수 있습니다.

| title | authors | first_autor_name |
| --- | --- | --- |
| Epidemi에 대 한 보충 정보 ... |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien |  

다른 옵션으로 절을 사용 하는 경우 개체에서 중첩 된 값의 경로를 지정할 수도 있습니다 `WITH` .

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    WITH ( title varchar(1000) '$.metadata.title',
           authors varchar(max) '$.metadata.authors'
    ) AS docs;
```

[Synapse 링크의 복합 데이터 유형](../how-to-analyze-complex-schema.md) 분석 및 [주문형 SQL의 중첩 된 구조](query-parquet-nested-types.md)에 대해 자세히 알아보세요.

> [!IMPORTANT]
> 대신 텍스트에서 예기치 않은 문자가 표시 `MÃƒÂ©lade` `Mélade` 되는 경우 데이터베이스 데이터 정렬이 [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) 데이터 정렬로 설정 되지 않습니다. 
> 과 같은 일부 SQL 문을 사용 하 여 [데이터베이스의 데이터 정렬을](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) 일부 UTF8 데이터 정렬로 변경 `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` 합니다.


## <a name="flattening-nested-arrays"></a>중첩 된 배열 평면화

Azure Cosmos DB 데이터에는 [Cord19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) 데이터 집합에서 authors 배열과 같은 중첩 된 하위 배열이 있을 수 있습니다.

```json
{
    "paper_id": <str>,                      # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": [                        # list of author dicts, in order
            {
                "first": <str>,
                "middle": <list of str>,
                "last": <str>,
                "suffix": <str>,
                "affiliation": <dict>,
                "email": <str>
            },
            ...
        ],
        ...
}
```

경우에 따라 최상위 항목 (메타 데이터)의 속성을 배열 (작성자)의 모든 요소와 "조인" 해야 할 수도 있습니다. SQL 주문형 요청을 통해 `OPENJSON` 중첩 된 배열에 함수를 적용 하 여 중첩 된 구조를 평면화 할 수 있습니다.

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Cord19
    ) WITH ( title varchar(1000) '$.metadata.title',
             authors varchar(max) '$.metadata.authors' ) AS docs
      CROSS APPLY OPENJSON ( authors )
                  WITH (
                       first varchar(50),
                       last varchar(50),
                       affiliation nvarchar(max) as json
                  ) AS a
```

이 쿼리의 결과는 다음과 같을 수 있습니다.

| title | authors | first | last | 정보 |
| --- | --- | --- | --- | --- |
| Epidemi에 대 한 보충 정보 ... |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien | Mélade | `   {"laboratory":"Centre de Recher…` |
Epidemi에 대 한 보충 정보 ... | `[{"first":"Nicolas","last":"4#","suffix":"","affiliation":{"laboratory":"","institution":"U…` | Nicolas | 3-4 # |`{"laboratory":"","institution":"U…` | 
| Epidemi에 대 한 보충 정보 ... |   `[{"first":"Beza","last":"Ramazindrazana","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Beza | Ramazindrazana | `{"laboratory":"Centre de Recher…` |
| Epidemi에 대 한 보충 정보 ... |   `[{"first":"Olivier","last":"Flores","suffix":"","affiliation":{"laboratory":"UMR C53 CIRAD, …` | Olivier | Flores |`{"laboratory":"UMR C53 CIRAD, …` |     

> [!IMPORTANT]
> 대신 텍스트에서 예기치 않은 문자가 표시 `MÃƒÂ©lade` `Mélade` 되는 경우 데이터베이스 데이터 정렬이 [UTF8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) 데이터 정렬로 설정 되지 않습니다. 
> 과 같은 일부 SQL 문을 사용 하 여 [데이터베이스의 데이터 정렬을](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) 일부 UTF8 데이터 정렬로 변경 `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` 합니다.

## <a name="azure-cosmos-db-to-sql-type-mappings"></a>SQL 형식 매핑 Azure Cosmos DB

Azure Cosmos DB 트랜잭션 저장소는 스키마에 관계 없이 분석 저장소는 분석 쿼리 성능을 최적화 하기 위해 스키마 화 된 됩니다. Synapse Link의 자동 동기화 기능을 사용 하 여 Azure Cosmos DB는 분석 저장소에서 중첩 된 데이터 형식 처리를 포함 하는 스키마 표현을 관리 합니다. SQL 주문형 요청은 분석 저장소를 쿼리 하므로 Azure Cosmos DB 입력 데이터 형식을 SQL 데이터 형식에 매핑하는 방법을 이해 하는 것이 중요 합니다.

SQL (Core) API의 Azure Cosmos DB 계정은 숫자, 문자열, 부울, null, 중첩 된 개체 또는 배열의 JSON 속성 유형을 지원 합니다. 에서 절을 사용 하는 경우 이러한 JSON 형식과 일치 하는 SQL 형식을 선택 해야 `WITH` `OPENROWSET` 합니다. Azure Cosmos DB에서 다른 속성 유형에 사용 해야 하는 SQL 열 유형 아래를 참조 하십시오.

| Azure Cosmos DB 속성 유형 | SQL 열 형식 |
| --- | --- |
| 부울 | bit |
| 정수 | bigint |
| Decimal | float |
| 문자열 | varchar (UTF8 데이터베이스 데이터 정렬) |
| 날짜/시간 (ISO 형식 문자열) | varchar (30) |
| 날짜 시간 (unix 타임 스탬프) | bigint |
| Null | `any SQL type` 
| 중첩 된 개체 또는 배열 | varchar (max) (UTF8 데이터베이스 데이터 정렬), JSON 텍스트로 직렬화 됨 |

Mongo DB API 종류의 Azure Cosmos DB 계정을 쿼리하려면 [여기](../../cosmos-db/analytical-store-introduction.md#analytical-schema)에서 사용할 확장 속성 이름 및 분석 저장소에서 전체 충실도 스키마 표현에 대해 자세히 알아볼 수 있습니다.

## <a name="next-steps"></a>다음 단계

자세한 내용은 다음 문서를 참조하세요.

- [주문형 SQL에서 뷰를 만들고 사용 하는 방법](create-use-views.md) 
- [Azure Cosmos DB에 대 한 SQL 주문형 뷰를 빌드하고 DirectQuery를 통해 Power BI 모델에 연결 하는 방법에 대 한 자습서](./tutorial-data-analyst.md)
