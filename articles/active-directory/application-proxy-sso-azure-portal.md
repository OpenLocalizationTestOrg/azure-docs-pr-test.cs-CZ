---
title: "aaaSingle přihlašování tooapps s proxy aplikace služby Azure AD | Microsoft Docs"
description: "Zapněte jednotné přihlašování pro aplikace publikované místní s Azure AD Application Proxy v hello portálu Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5ff288d36163b74215677d9e34c93c985ac33d54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Heslo vaulting pro jednotné přihlašování pomocí Proxy aplikace

Azure Proxy aplikace Active Directory vám pomůže zlepšit produktivitu tím, že publikujete místní aplikace tak, aby vzdálení zaměstnanci mít bezpečný přístup, je příliš. V hello portálu Azure můžete také nastavit jeden přihlašování (SSO) toothese aplikace. Uživatelé potřebují pouze tooauthenticate s Azure AD a přístupem podniková aplikace bez nutnosti toosign akci.

Proxy aplikací podporuje několik [jednotné přihlašování režimy](application-proxy-sso-overview.md). Založené na heslech přihlášení je určený pro aplikace, které používají kombinace uživatelského jména a hesla pro ověřování. Když konfigurujete založené na heslech přihlašování pro aplikace, uživatelé, aby toosign v jednou toohello místní aplikace. Poté Azure Active Directory ukládá přihlašovací informace hello a automaticky poskytuje ji aplikace toohello Pokud vaši uživatelé k němu přístup vzdáleně. 

Má už máte publikována a testování vaší aplikace pomocí Proxy aplikací. Pokud ne, postupujte podle kroků hello v [publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md) pak se vraťte se sem. 

## <a name="set-up-password-vaulting-for-your-application"></a>Nastavit heslo vaulting pro vaši aplikaci

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.
2. Vyberte **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**.
3. Hello seznamu vyberte hello aplikace, které chcete tooset si pomocí jednotného přihlašování.  
4. Vyberte **jednotného přihlašování**.

   ![Vybrat jednotné přihlašování](./media/application-proxy-sso-azure-portal/select-sso.png)

5. Režim jednotné přihlašování hello zvolte **založené na heslech přihlašování**.
6. Hello přihlašování adresy URL zadejte adresu URL hello pro stránku hello, kde uživatelé zadat svoje uživatelské jméno a heslo toosign tooyour aplikace mimo podnikovou síť hello. To může být hello externí adresu URL, kterou jste vytvořili při publikování aplikace hello prostřednictvím Proxy aplikace. 

   ![Zvolte založené na heslech přihlašování a zadejte adresu URL](./media/application-proxy-sso-azure-portal/password-sso.png)

7. Vyberte **Uložit**.

<!-- Need toorepro?
7. hello page should tell you that a sign-in form was successfully detected at hello provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow hello instructions toopoint out where hello sign-in credentials go. 
-->

## <a name="test-your-app"></a>Testování aplikace

Přejděte tooexternal adresu URL, kterou jste nakonfigurovali pro aplikace tooyour vzdáleného přístupu. Přihlaste se pomocí přihlašovacích údajů pro tuto aplikaci (nebo hello pověření pro účet testů, které jste nastavili s přístupem). Po přihlášení úspěšně, musí být schopný tooleave hello aplikace a vraťte bez opětovného zadávání přihlašovacích údajů. 

## <a name="next-steps"></a>Další kroky

- Přečtěte si informace o dalších způsobů tooimplement [jednotné přihlašování pomocí Proxy aplikace](application-proxy-sso-overview.md)
- Další informace o [důležité informace o zabezpečení pro přístup k aplikacím vzdáleně pomocí proxy aplikace služby Azure AD](application-proxy-security-considerations.md)