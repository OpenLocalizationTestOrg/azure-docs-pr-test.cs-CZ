---
title: "aaaHow toodetermine jaké jednotného přihlašování metoda toouse | Microsoft Docs"
description: "Pochopení hello jeden přihlašování režimy podporované službou Azure AD a jak toopick které jeden toochoose pro hello aplikace vás zajímá."
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
ms.openlocfilehash: 64f0bc1dc8281d1ab8222fd50eaceaf710704886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetermine-what-single-sign-on-method-toouse"></a>Jak toodetermine jaké jednotného přihlašování toouse – metoda

Tento článek vám pomůže toounderstand hello jeden přihlašování režimy podporované službou Azure AD a jak toopick které jeden toochoose pro hello aplikace vás zajímá.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Jednotné přihlašování a zřizování režimy podporované typy určitou aplikací

Následující tabulka Hello popisuje hello různých jednotné přihlašování a zřizování režimy podporované každou hello výše typy aplikací. Můžete použít tuto tabulku toohelp toounderstand aplikaci, pro kterou je třeba tooadd toosupport dosažení určitého cíle.

  ![Tabulka typů Asie a Tichomoří](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a>Jak toochoose jediný režim přihlášení

Hello podporované **jednotného přihlašování** režimy pro aplikace, Azure AD jsou uvedeny níže.

-   **Azure AD jednotné přihlašování zakázáno** – zvolte Azure AD jednotné přihlašování zakázáno **režimu přihlašování** Pokud ještě není připraven toointegrate tuto aplikaci pomocí jednotného přihlašování s Azure AD, nebo ho jednoduše testování

-   **Propojené přihlášení** – zvolte hello [propojené přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud máte aplikaci, která je již připojen k existujícího jeden přihlašování řešení, nebo pokud chcete toopublish jednoduchý odkaz pro uživatele v jejich [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) nebo [Spouštěč aplikace Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Založené na heslech přihlašování** – zvolte hello [založené na heslech přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud vaše aplikace vykreslí pole HTML uživatelské jméno a heslo a chcete toostore, uživatelské jméno a heslo bezpečně toobe přehrány toohello aplikaci později.

-   **Na základě SAML přihlašování** – zvolte hello [na základě SAML přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) na základě rolí toospecific aplikace jednotného přihlašování režim, pokud vaše aplikace podporuje protokoly hello SAML nebo OpenID Connect, nebo chcete, aby uživatelé moct toomap toobe v pravidlech definujete v vaší SAML deklarací *(**Poznámka:** tato možnost není k dispozici, pokud proxy aplikace hello je nakonfigurovaný pro aplikaci) *

-   **Na základě záhlaví přihlašování** – tuto možnost vyberte, [na základě záhlaví přihlašování](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) jediný přihlašování režim Pokud máte aplikace pomocí PingAccess, který podporuje hlavičky protokolu HTTP na základě ověřování, které chcete tooperform jednotné přihlašování na příliš *(**Poznámka:** tato možnost je dostupná pouze v případě proxy aplikace hello a PingAccess konfigurován pro aplikaci) *

-   **Integrované ověřování systému Windows** – zvolte hello [integrované ověřování systému Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednotné přihlašování v režimu při vystavení místní WIA aplikaci, která chcete tooperform jednotné přihlašování na příliš*()*  *Poznámka:** tato možnost je dostupná, pouze pokud proxy aplikace hello je nakonfigurovaný pro aplikaci) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Režimy jeden přihlašování pro aplikace vyvinuté vlastní

Aplikací mít vlastní vyvinuté pomocí hello [zákaznických aplikace](#_Custom-Developed_Applications) prostředí podporovat i další jeden přihlašování režimy nejsou uvedené výše. Mezi ně patří:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) na základě přihlášení

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) na základě přihlášení

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) na základě přihlášení

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) na základě přihlášení

Čtení hello [Příručka pro vývojáře Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn informace o tom, jak toocreate vyvinuté vlastní aplikaci, která podporuje tyto jednotné přihlašování režimy.

## <a name="how-tooset-an-applications-single-sign-on-mode"></a>Jak je jeden tooset aplikace přihlašování v režimu

tooset aplikace na **jednotného přihlašování** režimu hello postupujte podle pokynů níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování

7.  Po načtení hello aplikace, klikněte na **jednotného přihlašování** z aplikace hello levém navigační nabídky.

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)

