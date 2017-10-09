---
title: "aaaHow tooconfigure jeden přihlašování tooan aplikaci Proxy aplikace | Microsoft Docs"
description: "Jak můžete nakonfigurovat aplikaci proxy aplikace tooyour přihlašování rychle"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: e1289203177c77b3a8bcc9058c5c0b8ae50f243e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-single-sign-on-tooan-application-proxy-application"></a>Jak tooconfigure jednotné přihlašování tooan aplikaci Proxy aplikace

Jednotné přihlašování (SSO) umožňuje tooaccess vaši uživatelé aplikaci bez ověřování vícekrát. Ji umožňuje ověření jednotného toooccur hello v hello cloudu s Azure Active Directory a umožňuje služba hello nebo konektor tooimpersonate hello uživatele toocomplete žádné další ověřovací výzvy z aplikace hello.

## <a name="how-tooconfigure-single-sign-on"></a>Jak tooconfigure jednotného přihlašování
tooconfigure jednotné přihlašování, nejdřív zkontrolujte, že je vaše aplikace nakonfigurovaná pro předběžné ověření pomocí služby Azure Active Directory. toodo toto, příliš**Azure Active Directory**  - &gt; **podnikové aplikace, které**  - &gt; **všechny aplikace**  - &gt; Aplikace  **- &gt; Proxy aplikace**. Na této stránce najdete hello "Předběžné ověřování" v poli a ujistěte se, který je nastaven příliš "Azure Active Directory. 

Další informace o metodách hello předběžné ověření, najdete v kroku 4 hello [publikování dokumentu aplikace](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

   ![Metoda předběžného ověřování na portálu Azure](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Konfigurace režimy přihlašování pro aplikace Proxy aplikace
Další nakonfigurujeme hello konkrétní typ jednotného přihlašování. Hello přihlašování metody jsou klasifikovány podle jaký typ ověřování hello back-end aplikace používá. Proxy aplikace aplikace podporuje tři typy přihlašování:

-   **Založené na heslech přihlašování**: založené na heslech přihlášení lze použít pro všechny aplikace, která používá pole uživatelské jméno a heslo toosign v. Kroky konfigurace naleznete v našem [heslo SSO konfigurace dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#bring-your-own-password-sso-applications).

-   **Integrované ověřování systému Windows**: pro aplikace pomocí integrovaného ověřování systému Windows (IWA), jednotné přihlašování je povolené prostřednictvím použitím protokolu Kerberos omezené delegování (KCD). To dává oprávnění konektory Proxy aplikace v tooimpersonate uživatelů služby Active Directory a toosend a přijímat tokeny jejich jménem. Podrobné informace o konfiguraci použitím KCD lze nalézt v hello [jednotné přihlašování s použitím KCD dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd).

-   **Na základě záhlaví přihlašování**: na základě záhlaví přihlašování je povolená Díky partnerství a vyžadovat další konfiguraci. Podrobnosti o hello partnerství a podrobné pokyny pro konfiguraci aplikace tooan přihlášení, která používá hlavičky pro ověřování najdete v tématu hello [PingAccess dokumentaci k Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).

Každá z těchto možností najdete tak, že přejdete tooyour aplikaci v "Podnikové aplikace" a otevírání hello **jednotné přihlašování** stránky v levé nabídce hello. Všimněte si, že pokud vaše aplikace byla vytvořena v hello starý portál, se nemusí zobrazit všechny tyto možnosti.

Na této stránce můžete také najdete v jednom další možnost přihlášení: propojené přihlášení. Tato možnost je také podporována službou Proxy aplikací. Všimněte si však, tato možnost nepřidává žádné další aplikace toohello přihlášení. Ale nutné dodat, že aplikace hello už můžete mít jednotné přihlašování implementovaná pomocí jiné služby, jako je Active Directory Federation Services. 

Tato možnost umožňuje Správce toocreate aplikace tooan odkaz, která uživatelé nejprve zobrazovat při přístupu k aplikaci hello. Například pokud je, že aplikace, která je nakonfigurovaná tooauthenticate uživatele služby Active Directory Federation Services 2.0, správce použít hello "propojené přihlášení" možnost toocreate tooit odkaz na panel přístupu hello.

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
