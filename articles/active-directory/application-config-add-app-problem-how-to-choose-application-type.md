---
title: "Po přidání aplikace zadejte aplikaci, pro kterou toouse toochoose aaaHow | Microsoft Docs"
description: "Pochopení hello podporované typy aplikací můžete integrovat se službou Azure AD a jejich související možnosti konfigurace"
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
ms.openlocfilehash: 46e3672e7f5048b0fa54171f0fc169362c9d5ac6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-which-application-type-toouse-when-adding-an-application"></a>Jak toochoose, která aplikace zadejte toouse při přidání aplikace

Tento článek vám pomůže toounderstand hello čtyři hlavní typy aplikací, které lze integrovat s Azure AD:

* Co je podporováno každou z nich
* Proč můžete vybrat aplikaci, pro kterou
* Jak tooconfigure vlastnosti základní tyto aplikace, jako například jak budou uživatelé **zřízený**, nebo co **jednotného přihlašování** toouse technologie.

## <a name="supported-application-types-in-azure-ad"></a>Typy podporovaných aplikací ve službě Azure AD

Azure AD podporuje čtyři hlavní typy aplikací, které můžete přidat pomocí hello **přidat** funkce najít v části **podnikové aplikace, které**. Mezi ně patří:

-   **Galerii aplikací Azure AD** – aplikaci, která jsou předem integrované pro jednotné přihlašování s Azure AD.

-   **Aplikace Proxy aplikace** – aplikace běžící v místním prostředí, které mají tooprovide zabezpečené jednotného přihlašování tooexternally.

-   **Aplikace vyvinuté vlastní** – aplikace, které vaše organizace si přeje toodevelop na hello platformy vývoj aplikací Azure AD, ale který ještě neexistuje.

-   **Aplikace bez Galerie** – přineste si vlastní aplikace! Všechny webový odkaz, který má být nebo všechny aplikace, která vykreslí pole uživatelské jméno a heslo, podporuje protokoly SAML nebo OpenID Connect, nebo podporuje SCIM, který chcete toointegrate pro jednotné přihlašování s Azure AD.

## <a name="features-and-capabilities-supported-by-all-hello-above-application-types"></a>Funkce a možnosti nepodporuje všechny hello výše typy aplikací

Hello následující funkce jsou podporovány všechny hello výše 4 typy aplikací ve službě Azure AD:

-   **Rychlý start** – zprovoznění aplikace rychle podle [kroky jednoduché nasazení](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

-   **Obecné vlastnosti správy** – získání [přímé odkazu deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan aplikace [přizpůsobit hello branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) aplikace, nebo [vypnout aplikaci hello](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) pro všechny uživatele.

-   **Správa uživatelů a skupin** – [přiřadit](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) nebo [odebrat](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) aplikace tooan uživatelů a skupin a volitelně přiřadit hello konkrétní aplikaci rolí tyto uživatele a skupiny mají přístup k

-   **Přístup k aplikaci Samoobslužné služby** – povolit vaši uživatelé toorequest [přístup k aplikaci Samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan aplikaci z jejich přístup k aplikaci panely buď přidáním aplikace přímo nebo [ připojení ke skupině povoleno samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), volitelně vyžadující schválení obchodní podél hello způsob

-   **Protokoly přihlášení** – najdete v části [všechny hello přihlášení tooan aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), nebo všechny aplikace

-   **Protokoly auditu** – viz [podrobné protokoly auditu o aplikaci tooan úpravy](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), nebo tooall aplikace

-   **Podmíněné a na základě riziko přístup** – nastavte výkonné [pravidel přístupu na základě podmínky](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) , se vynutí při pokusí toosign v konkrétní aplikaci tooa

-   **Zobrazení oprávnění** – zobrazit všechny hello [OAuth2 oprávnění](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) má aplikace přístup tooin adresáře z jednoho místa

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Jednotné přihlašování a zřizování režimy podporované typy určitou aplikací

Následující tabulka Hello popisuje hello různých jednotné přihlašování a zřizování režimy podporované každou hello výše typy aplikací. Můžete použít tuto tabulku toohelp toounderstand aplikaci, pro kterou je třeba tooadd toosupport dosažení určitého cíle.

  ![Tabulka typů aplikací](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a>Jak toochoose jediný režim přihlášení

Hello podporované **jednotného přihlašování** režimy pro aplikace, Azure AD jsou uvedeny níže.

-   **Azure AD jednotné přihlašování zakázáno** – zvolte Azure AD jednotné přihlašování zakázáno **režimu přihlašování** Pokud ještě není připraven toointegrate tuto aplikaci pomocí jednotného přihlašování s Azure AD, nebo ho jednoduše testování

-   **Propojené přihlášení** – zvolte hello [propojené přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud máte aplikaci, která je již připojen k existujícího jeden přihlašování řešení, nebo pokud chcete toopublish jednoduchý odkaz pro uživatele v jejich [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) nebo [Spouštěč aplikace Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Založené na heslech přihlašování** – zvolte hello [založené na heslech přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud vaše aplikace vykreslí pole HTML uživatelské jméno a heslo a chcete toostore, uživatelské jméno a heslo bezpečně toobe přehrány toohello aplikaci později.

-   **Na základě SAML přihlašování** – zvolte hello [na základě SAML přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) na základě rolí toospecific aplikace jednotného přihlašování režim, pokud vaše aplikace podporuje protokoly hello SAML nebo OpenID Connect, nebo chcete, aby uživatelé moct toomap toobe v pravidlech definujete v vaší SAML deklarací *

   >[!NOTE]
   >Tato možnost není k dispozici, pokud proxy aplikace hello je nakonfigurovaný pro aplikaci.
   >
   >

-   **Na základě záhlaví přihlašování** – tuto možnost vyberte, [na základě záhlaví přihlašování](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) jediný přihlašování režim Pokud máte aplikace pomocí PingAccess, který podporuje hlavičky protokolu HTTP na základě ověřování, které chcete tooperform jednotné přihlašování na příliš

   >[!NOTE]
   >Tato možnost je dostupná, pouze pokud proxy aplikace hello a PingAccess nakonfigurovaný pro aplikaci.
   >
   >

-   **Integrované ověřování systému Windows** – zvolte hello [integrované ověřování systému Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednotné přihlašování v režimu při vystavení místní WIA aplikaci, která chcete tooperform jednotné přihlašování na příliš

   >[!NOTE]
   >Tato možnost je dostupná, pouze pokud proxy aplikace hello je nakonfigurovaný pro aplikaci.
   >
   >

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

6.  Vyberte hello aplikaci, pro které chcete tooconfigure jednotné přihlašování.

7.  Po načtení hello aplikace, klikněte na **jednotného přihlašování** z aplikace hello levém navigační nabídky.

## <a name="how-toochoose-a-provisioning-mode"></a>Jak toochoose režim zřizování

-   **Ruční zřizování** – zvolte hello [ruční](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) režimu zřizování, pokud máte existující účty, nebo chcete toomanage účty pro tuto aplikaci mimo Azure AD.

-   **Automatické zřizování** – zvolte hello [automatické](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **režim zřizování** Pokud chcete zřizování tooenable automatické založené na rozhraní API nebo zrušte zřizování uživatelských účtů toothis aplikace 

   >[!NOTE]
   >Tato možnost je dostupná pouze pro aplikace v rámci hello **vybrané** kategorie hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).
   >
   >

-   **Na základě SCIM automatické zřizování** – použít [na základě SCIM automatické zřizování](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) Pokud vaše aplikace podporuje protokol hello SCIM pro zjišťování toousers změny a skupin, které jsou automaticky vygenerované změny aplikace tooany integrované s Azure AD 

   >[!NOTE]
   >Tato možnost není uvedena jako konkrétní režim zřizování, ale je povolená ve výchozím nastavení pro všechny aplikace, které jsou integrované s Azure AD.
   >
   >

## <a name="how-tooset-an-applications-provisioning-mode"></a>Jak je tooset aplikace režimu zřizování

tooset aplikace na **zřizování** režimu hello postupujte podle pokynů níže:

tooset aplikace na **jednotného přihlašování** režimu hello postupujte podle pokynů níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci, pro které chcete tooconfigure zřizování.

7.  Po načtení hello aplikace, klikněte na **zřizování** z aplikace hello levém navigační nabídky.

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
