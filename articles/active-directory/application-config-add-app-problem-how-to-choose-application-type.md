---
title: "Jak vybrat typů aplikací, které se mají použít při přidávání aplikace | Microsoft Docs"
description: "Podporované typy aplikací můžete integrovat se službou Azure AD pochopit a jejich související možnosti konfigurace"
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
ms.openlocfilehash: e0d41d1933531c2c633613bcbc1bbcbf075d6a69
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-choose-which-application-type-to-use-when-adding-an-application"></a>Jak vybrat typů aplikací, které se mají použít při přidávání aplikace

Tento článek vám pomůže lépe porozumět čtyři hlavní typy aplikací, které lze integrovat s Azure AD:

* Co je podporováno každou z nich
* Proč můžete vybrat aplikaci, pro kterou
* Postup konfigurace vlastností základní těchto aplikací, jako je jak budou uživatelé **zřízený**, nebo co **jednotného přihlašování** technologie používat.

## <a name="supported-application-types-in-azure-ad"></a>Typy podporovaných aplikací ve službě Azure AD

Azure AD podporuje čtyři typy hlavní aplikace, které můžete přidat pomocí **přidat** funkce najít v části **podnikové aplikace, které**. Mezi ně patří:

-   **Galerii aplikací Azure AD** – aplikaci, která jsou předem integrované pro jednotné přihlašování s Azure AD.

-   **Aplikace Proxy aplikace** – aplikace běžící v místním prostředí, které byste chtěli poskytnout zabezpečený jednotné přihlašování k externě.

-   **Aplikace vyvinuté vlastní** – aplikaci, kterou vaše organizace chce vývoji na platformu vývoj aplikací Azure AD, ale který ještě neexistuje.

-   **Aplikace bez Galerie** – přineste si vlastní aplikace! Všechny webový odkaz, který má být nebo všechny aplikace, která vykreslí pole uživatelské jméno a heslo, podporuje protokoly SAML nebo OpenID Connect, nebo podporuje SCIM, který chcete integrovat pro jednotné přihlašování s Azure AD.

## <a name="features-and-capabilities-supported-by-all-the-above-application-types"></a>Funkce a možnosti nepodporuje všechny výše uvedené typy aplikací

Žádné z výše uvedených typů 4 aplikací ve službě Azure AD jsou podporovány následující funkce:

-   **Rychlý start** – zprovoznění aplikace rychle podle [kroky jednoduché nasazení](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

-   **Obecné vlastnosti správy** – získání [přímé odkazu deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) aplikace, [přizpůsobit branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) aplikace, nebo [zakázání aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) pro všechny uživatele.

-   **Správa uživatelů a skupin** – [přiřadit](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) nebo [odebrat](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) uživatelů a skupin k aplikaci a volitelně přiřadit role konkrétní aplikaci tito uživatelé a skupiny mají přístup k

-   **Přístup k aplikaci Samoobslužné služby** – mohou uživatelé požádat o [přístup k aplikaci Samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) z jejich panelů přístupu aplikace buď pomocí přidání aplikace přímo k aplikaci nebo [připojení ke skupině povoleno samoobslužné služby](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), volitelně vyžadující schválení obchodní na cestě

-   **Protokoly přihlášení** – najdete v části [všechny přihlášení k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), nebo všechny aplikace

-   **Protokoly auditu** – viz [podrobné protokoly auditu o změny aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), nebo pro všechny aplikace

-   **Podmíněné a na základě riziko přístup** – nastavte výkonné [pravidel přístupu na základě podmínky](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) , se vynucují, když se uživatelé pokusí přihlásit na konkrétní aplikaci

-   **Zobrazení oprávnění** – zobrazit žádné z [OAuth2 oprávnění](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) má aplikace přístup k ve vašem adresáři z jedné umístit

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Jednotné přihlašování a zřizování režimy podporované typy určitou aplikací

Následující tabulka popisuje různé jednotné přihlašování a zřizování režimy podporované každou z výše uvedených typů aplikací. Tato tabulka vám pomůže vám pomůže lépe porozumět aplikaci, pro kterou je nutné přidat na podporu dosažení určitého cíle.

  ![Tabulka typů aplikací](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Jak vybrat jediný režim přihlášení

U podporovaných **jednotného přihlašování** režimy pro aplikace, Azure AD jsou uvedeny níže.

-   **Azure AD jednotné přihlašování zakázáno** – zvolte Azure AD jednotné přihlašování zakázáno **režimu přihlašování** Pokud ještě nejsou připraveny k integraci tuto aplikaci pomocí jednotného přihlašování s Azure AD, nebo ho jednoduše testování

-   **Propojené přihlášení** – zvolte [propojené přihlášení](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud máte aplikaci, která je již připojen k existujícího jeden přihlašování řešení, nebo pokud chcete publikovat jednoduchý odkaz pro uživatele v jejich [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) nebo [Spouštěč aplikace Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Založené na heslech přihlašování** – zvolte [založené na heslech přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **režimu přihlašování** Pokud vaše aplikace vykreslí pole HTML uživatelské jméno a heslo a chcete uložit tento uživatelské jméno a heslo do přehrány do aplikace později

-   **Na základě SAML přihlašování** – zvolte [na základě SAML přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) jednotné přihlašování v režimu, pokud vaše aplikace podporuje protokoly SAML nebo OpenID Connect, nebo chcete být schopni mapování uživatelů na konkrétní aplikaci role na základě pravidel, můžete definovat vaše SAML deklaracemi identity *

   >[!NOTE]
   >Tato možnost není k dispozici, když je proxy aplikací nakonfigurovaný pro aplikaci.
   >
   >

-   **Na základě záhlaví přihlašování** – tuto možnost vyberte, [na základě záhlaví přihlašování](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) jediný přihlašování režim Pokud máte aplikace pomocí PingAccess, který podporuje hlavičky protokolu HTTP na základě ověřování, které chcete provést jednotné přihlašování k 

   >[!NOTE]
   >Tato možnost je dostupná, pouze pokud proxy aplikace a PingAccess nakonfigurovaný pro aplikaci.
   >
   >

-   **Integrované ověřování systému Windows** – zvolte [integrované ověřování systému Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) jednotné přihlašování v režimu při vystavení, kterou chcete provést jednotné přihlašování k aplikaci WIA místní 

   >[!NOTE]
   >Tato možnost je dostupná, pouze pokud je nakonfigurován proxy aplikace pro aplikaci.
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Režimy jeden přihlašování pro aplikace vyvinuté vlastní

Aplikací mít vlastní vyvinuté pomocí [zákaznických aplikace](#_Custom-Developed_Applications) prostředí podporovat i další jeden přihlašování režimy nejsou uvedené výše. Mezi ně patří:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) na základě přihlášení

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) na základě přihlášení

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) na základě přihlášení

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) na základě přihlášení

Pro čtení [Příručka pro vývojáře Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) Další informace o tom, jak vytvořit vyvinuté vlastní aplikaci, která podporuje tyto režimy jednom přihlášení.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Postup nastavení aplikace jediný přihlašování režim

Nastavení aplikace **jednotného přihlašování** režimu, postupujte podle pokynů níže:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

  * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, pro který chcete nakonfigurovat jednotné přihlašování.

7.  Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.

## <a name="how-to-choose-a-provisioning-mode"></a>Jak vybrat režim zřizování

-   **Ruční zřizování** – zvolte [ruční](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) režimu zřizování, pokud máte existující účty, nebo chcete spravovat účty pro tuto aplikaci mimo Azure AD.

-   **Automatické zřizování** – zvolte [automatické](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **režim zřizování** Pokud chcete povolit automatické zřizování na základě rozhraní API a jeho rušení uživatelských účtů do této aplikace 

   >[!NOTE]
   >Tato možnost je dostupná pouze pro aplikace v rámci **vybrané** kategorii [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).
   >
   >

-   **Na základě SCIM automatické zřizování** – použít [na základě SCIM automatické zřizování](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) Pokud vaše aplikace podporuje protokol SCIM pro zjišťování změny uživatelů a skupin, které jsou automaticky vygenerované změny všech aplikací integrované s Azure AD 

   >[!NOTE]
   >Tato možnost není uvedena jako konkrétní režim zřizování, ale je povolená ve výchozím nastavení pro všechny aplikace, které jsou integrované s Azure AD.
   >
   >

## <a name="how-to-set-an-applications-provisioning-mode"></a>Postup nastavení aplikace je režimu zřizování

Chcete-li nastavit aplikace **zřizování** režimu, postupujte podle pokynů níže:

Nastavení aplikace **jednotného přihlašování** režimu, postupujte podle pokynů níže:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

  * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, pro kterou chcete provést konfiguraci zřizování.

7.  Po načtení aplikace, klikněte na **zřizování** navigační nabídce vlevo aplikace.

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
