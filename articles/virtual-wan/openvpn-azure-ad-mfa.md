---
title: Azure AD 인증을 사용 하 여 VPN 사용자에 대해 MFA 사용
description: VPN 사용자에 대해 multi-factor authentication 사용
services: virtual-wan
author: kumudD
ms.service: virtual-wan
ms.topic: how-to
ms.date: 01/16/2020
ms.author: alzam
ms.openlocfilehash: 7adcdcf00f22020daf66192e777b1aca151349e0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082123"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users-by-using-azure-ad-authentication"></a>Azure AD 인증을 사용 하 여 VPN 사용자에 대해 MFA (Azure Multi-Factor Authentication) 사용

[!INCLUDE [overview](../../includes/vpn-gateway-vwan-openvpn-enable-mfa-overview.md)]

## <a name="enable-authentication"></a><a name="enableauth"></a>인증 사용

[!INCLUDE [enable authentication](../../includes/vpn-gateway-vwan-openvpn-enable-auth.md)]

## <a name="configure-sign-in-settings"></a><a name="enablesign"></a>로그인 설정 구성

[!INCLUDE [sign in](../../includes/vpn-gateway-vwan-openvpn-sign-in.md)]

## <a name="option-1---per-user-access"></a><a name="peruser"></a>옵션 1-사용자 당 액세스

[!INCLUDE [per user](../../includes/vpn-gateway-vwan-openvpn-per-user.md)]

## <a name="option-2---conditional-access"></a><a name="conditional"></a>옵션 2-조건부 액세스

[!INCLUDE [conditional access](../../includes/vpn-gateway-vwan-openvpn-conditional.md)]

## <a name="next-steps"></a>다음 단계

가상 네트워크에 연결 하려면 VPN 클라이언트 프로필을 만들고 구성 해야 합니다. [Azure에 대 한 지점 및 사이트 간 연결에 대 한 AZURE AD 인증 구성을](virtual-wan-point-to-site-azure-ad.md)참조 하세요.
