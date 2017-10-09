---
title: "aaaUnexpected aplikace v seznamu aplikací | Microsoft Docs"
description: "Jak toosee všechny aplikace ve vašem klientovi a pochopit, jak aplikace se objeví v seznamu všechny aplikace v rámci podnikové aplikace"
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
ms.openlocfilehash: 2f974046b655bc36a05f002c56511a8a988cd01c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-application-in-my-applications-list"></a>Neočekávané aplikace v seznamu Moje aplikace

Tento článek vám pomůže toounderstand jak aplikace se objeví v vaše **všechny aplikace** v rámci **podnikové aplikace, které**. 

## <a name="how-toosee-all-applications-in-your-tenant"></a>Jak toosee všechny aplikace ve vašem klientovi

toosee všechny hello aplikací ve vašem klientovi, musíte toouse hello **filtru** řízení tooshow **všechny aplikace** pod hello **všechny aplikace** seznamu. toodo tento, postupujte podle kroků hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

6.  Klikněte na tlačítko hello použití hello **filtru** řízení hello horní části hello **seznam všech aplikací**.

7.  Na hello **filtru** okno, sada hello **zobrazit** možnost příliš**všechny aplikace.**

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Proč konkrétní aplikace se zobrazuje v seznamu všechny aplikace?

Při filtrovat příliš**všechny aplikace**, hello **všechny aplikace** **seznamu** ukazuje každý objekt zabezpečení služby ve vašem klientovi. Hlavní objekty služeb můžete zobrazit v tomto seznamu různými způsoby:

1.  Když přidáte z Galerie aplikace hello jakékoli aplikace, včetně:

   1. **Galerii aplikací Azure AD** – aplikaci, která jsou předem integrované pro jednotné přihlašování s Azure AD.

   2. **Aplikace Proxy aplikace** – aplikace běžící v místním prostředí, které mají tooprovide zabezpečené jednotného přihlašování tooexternally.

   3. **Aplikace vyvinuté vlastní** – aplikace, které vaše organizace si přeje toodevelop na hello platformy vývoj aplikací Azure AD, ale který ještě neexistuje.

   4. **Aplikace bez Galerie** – přineste si vlastní aplikace! Všechny webový odkaz, který má být nebo všechny aplikace, která vykreslí pole uživatelské jméno a heslo, podporuje protokoly SAML nebo OpenID Connect, nebo podporuje SCIM, který chcete toointegrate pro jednotné přihlašování s Azure AD.

2.  Když registrujete nebo přihlášení k a 3<sup>VP</sup> aplikaci strany integrované s Azure Active Directory. Příkladem toho je [Smartsheet](https://app.smartsheet.com/b/home) nebo [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).

3.  Pokud registrujete nebo přidávání licencí tooa uživatele nebo skupiny tooa první strany aplikaci, jako například [Microsoft Office 365](http://products.office.com/).

4.  Když přidáte novou registraci aplikace tak, že vytvoříte vlastní vyvinuté aplikace, pomocí hello [aplikace registru](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).

5.  Když přidáte novou registraci aplikace tak, že vytvoříte vlastní vyvinuté aplikace, pomocí hello [portálu pro registraci aplikace V2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).

6.  Když přidáváte aplikaci provádíte vývoj pomocí sady Visual Studio [metody ověřování ASP.net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) nebo [připojené služby](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).

7.  Při vytváření objektu zabezpečení služby pomocí hello [modulu Azure AD PowerShell](/powershell/azure/install-adv2?view=azureadps-2.0).

8.  Pokud jste [souhlas tooan aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) za správce toouse data ve vašem klientovi.

9.  Když [uživatel souhlasí tooan aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toouse data ve vašem klientovi.

10. Když povolíte určité služby, které ukládají data ve vašem klientovi. Příkladem toho je resetování hesla, která je modelovaná jako hlavní toostore služby heslo resetovat zásady bezpečně.

Přečtěte si další informace o aplikací, jak jsou přidat tooyour directory tooget [jak a proč se aplikace přidávají tooAzure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a>Chci tooremove aplikace tooan přiřazení konkrétního uživatele nebo skupiny

tooremove uživatele nebo skupinu přiřazení tooan aplikace, postupujte podle kroků hello uvedené v hello [odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) článku.

## <a name="i-want-toodisable-all-access-tooan-application-for-every-user"></a>Chci toodisable všechny aplikace tooan přístupu pro každého uživatele

toodisable všechny tooan přihlášení uživatele-aplikací, postupujte podle kroků hello uvedených v hello [zakázat uživatelská přihlášení pro podnikové aplikace v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) článku.

## <a name="i-want-toodelete-an-application-entirely"></a>Chci, aby toodelete aplikaci zcela

příliš**odstranit aplikaci**, postupujte podle pokynů hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello chcete toodelete.

7.  Po načtení hello aplikace, klikněte na **odstranit** ikonu z hello hlavní aplikace **přehled** okno.

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a>Chci toodisable všechny budoucí uživatelská souhlasu operations tooany aplikace

Zakázání souhlas uživatele pro celý adresář zabránit koncovým uživatelům z souhlas tooany aplikace. Správci mohou stále souhlas na behalves uživatele. souhlas toolearn Další informace o aplikaci a proč může nebo nemusí nepřejete toodo, články [Principy uživatelů a správce souhlasu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

příliš**zakažte všechny operace souhlasu budoucí uživatele v adresáři celý**, postupujte podle pokynů hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **uživatelská nastavení**.

6.  Zakažte všechny operace souhlasu budoucí uživatele tak, že nastavení hello **uživatelé můžou aplikace tooaccess svá data** přepnutí příliš**ne** a klikněte na tlačítko hello **Uložit** tlačítko.

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
