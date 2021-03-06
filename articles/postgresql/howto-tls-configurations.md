---
title: TLS 구성-Azure Portal Azure Database for PostgreSQL 단일 서버
description: Azure Database for PostgreSQL 단일 서버에 대 한 Azure Portal를 사용 하 여 TLS 구성을 설정 하는 방법을 알아봅니다.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: how-to
ms.date: 06/02/2020
ms.openlocfilehash: 26470709b37c2623c581499ec55572da402e96cb
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906465"
---
# <a name="configuring-tls-settings-in-azure-database-for-postgresql-single---server-using-azure-portal"></a>Azure Portal를 사용 하 여 Azure Database for PostgreSQL 단일 서버에서 TLS 설정 구성

이 문서에서는 연결에 대해 허용 되는 최소 TLS 버전을 적용 하 고 최소 tls 버전의 모든 연결을 거부 하 여 네트워크 보안을 강화 하도록 Azure Database for PostgreSQL를 구성 하는 방법을 설명 합니다.

Azure Database for PostgreSQL에 연결 하는 데 TLS 버전을 적용할 수 있습니다. 이제 고객은 데이터베이스 서버에 대 한 최소 TLS 버전을 설정 하도록 선택할 수 있습니다. 예를 들어 최소 TLS 설정 버전을 TLS 1.0로 설정 하면 서버에서 TLS 1.0, 1.1 및 1.2 +를 사용 하는 클라이언트의 연결을 허용 한다는 의미입니다. 대신, 최소 tls 버전을 1.2 이상으로 설정 하면 TLS 1.2을 사용 하는 클라이언트 로부터의 연결만 허용 하 고 TLS 1.0 및 TLS 1.1를 사용 하는 모든 연결이 거부 됩니다.

## <a name="prerequisites"></a>사전 요구 사항

이 방법 가이드를 완료하려면 다음이 필요합니다.

* [Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL-단일 서버에 대 한 TLS 구성 설정

PostgreSQL 최소 TLS 버전을 설정 하려면 다음 단계를 수행 합니다.

1. [Azure Portal](https://portal.azure.com/)에서 기존 Azure Database for PostgreSQL를 선택 합니다.

1.  Azure Database for PostgreSQL-단일 서버 페이지의 **설정**에서 **연결 보안** 을 클릭 하 여 연결 보안 구성 페이지를 엽니다.

1. **최소 tls 버전**에서 **1.2** 을 선택 하 여 PostgreSQL 단일 서버에 대해 TLS 1.2 보다 작은 tls 버전의 연결을 거부 합니다.

    :::image type="content" source="./media/howto-tls-configurations/setting-tls-value.png" alt-text="단일 서버 TLS 구성 Azure Database for PostgreSQL":::

1. **저장**을 클릭하여 변경 내용을 저장합니다.

1. 연결 보안 설정이 성공적으로 설정 되었는지 확인 하는 알림이 나타납니다.

    :::image type="content" source="./media/howto-tls-configurations/setting-tls-value-success.png" alt-text="Azure Database for PostgreSQL-단일 서버 TLS 구성 성공":::

## <a name="next-steps"></a>다음 단계

[메트릭에 대 한 경고를 만드는 방법](howto-alert-on-metric.md) 에 대해 알아봅니다.