---
title: "přihlášení aplikace tooan z hello přístupový panel aaaProblems | Microsoft Docs"
description: "Jak problémy tootroubleshoot přístupu k aplikaci z hello Microsoft přístupový Panel Azure AD v myapps.microsoft.com"
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
ms.reviewer: japere
ms.openlocfilehash: 346c4da06416bb9b330bdd5b1201253af19ba58b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-from-hello-access-panel"></a>Potíže s přihlášením tooan aplikace hello přístupového panelu

Hello přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spusťte cloudové aplikace této hello správce Azure AD udělil je přístup k. 

Tyto aplikace jsou konfigurovány jménem uživatele hello na portálu Azure AD hello. Hello aplikace musí být správně nakonfigurované a přiřazené toohello uživatele nebo skupiny hello uživatel je členem toosee hello aplikace hello přístupového panelu.

Typ Hello aplikací, které zobrazuje uživatel možná spadat hello následujících kategorií:

-   Aplikace Office 365

-   Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation

-   Aplikace založené na heslech jednotného přihlašování

-   Aplikace s stávajících řešení pro jednotné přihlašování

## <a name="general-issues-toocheck-first"></a>Obecné problémy toocheck nejprve

-   Zajistěte, aby vaše použití **prohlížeče** , splňuje minimální požadavky hello hello přístupového panelu.

-   Zajistěte, aby prohlížeče hello uživatele přidala hello URL tooits aplikace hello **Důvěryhodné servery**.

-   Zkontrolujte, zda je aplikace hello toocheck **nakonfigurované** správně.

-   Zkontrolujte, zda text hello uživatelský účet je **povoleno** pro přihlášení.

-   Zkontrolujte, zda text hello uživatelský účet je **není uzamčen.**

-   Ujistěte se, zda text hello uživatele **není platnost nebo zapomenete heslo.**

-   Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.

-   Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.

-   Ujistěte se, že uživatele **ověřování kontaktní údaje** je toodate tooallow ověřování Multi-Factor Authentication nebo podmíněného přístupu zásady toobe vynucené.

-   Zkontrolujte že tooalso zkuste vymazání souborů cookie v prohlížeči a toosign v zkusit to znovu.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>Splňuje požadavky na prohlížeč pro hello přístupového panelu

Hello přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS. toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele. Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.

Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:

-   Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější

-   Hraniční Windows 10 Anniversary Edition nebo novější.

-   Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější

-   Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Jak tooinstall hello rozšíření přístup k panelu prohlížeče

tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:

1.  Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.

3.  V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.

4.  Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení. **Přidat** hello rozšíření tooyour prohlížeče.

5.  Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.

6.  Po instalaci **restartujte** relaci prohlížeče.

7.  Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO

Může také stáhnout hello rozšíření pro Chrome a hraniční z hello přímé odkazy níže:

-   [Rozšíření pro Chrome přístup panely](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření panelu přístup Edge](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Jak tooconfigure federovaného jednotného přihlašování pro aplikaci Galerie Azure AD

Všechny aplikace v galerii Azure AD hello povolit funkce Enterprise Single Sign-On má podrobný kurz k dispozici. Dostanete hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidat aplikaci z Galerie Azure AD hello](#add-an-application)

-   [Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Načtení metadat Azure AD a certifikátů](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Přiřazení uživatelů toohello aplikace](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Přidat aplikaci z Galerie Azure AD hello

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno

6.  V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.

7.  Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.

8.  Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.

9.  Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

Po krátké době být okno Konfigurace aplikací mít toosee hello.

### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a>Konfigurovat jednotné přihlašování pro aplikace z Galerie Azure AD hello

tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:

1.  <span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte **na základě SAML přihlašování** z hello **režimu** rozevíracího seznamu.

9.  Zadejte hello požadované hodnoty v **domény a adresy URL.** Tyto hodnoty by měly získat od dodavatele aplikace hello.

   1. aplikace hello tooconfigure jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota. Některé aplikace hello identifikátor je také požadovaná hodnota.

   2. aplikace hello tooconfigure jako jednotné přihlašování iniciované IdP, hello adresa URL odpovědi je požadovaná hodnota. Některé aplikace hello identifikátor je také požadovaná hodnota.

10. **Volitelné:** klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** Pokud chcete, aby toosee hello-požadované hodnoty.

11. V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.

12. **Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.

   tooadd atribut:

   1. Klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   2. Klikněte na tlačítko **uložit.** Zobrazí hello nový atribut v tabulce hello.

13. Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello. Navíc má adres URL metadat hello a certifikát vyžadovaný toosetup jednotné přihlašování s aplikací hello.

14. Klikněte na tlačítko **Uložit** toosave hello konfigurace.

15. Přiřazení uživatelů toohello aplikace.

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace

tooselect hello identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud aplikace hello chcete tooshow vytvořit tady nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  V části hello **uživatelské atributy** vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu. Hello vybranou možnost musí toomatch hello očekávaná hodnota v uživatel hello tooauthenticate aplikace hello.

    >[!NOTE]
    >Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest. Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.
    >
    >

9.  tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.

   tooadd atribut:

   1. Klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   2. Klikněte na tlačítko **uložit.** Zobrazí hello nový atribut v tabulce hello.

### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Stažení metadat hello Azure AD nebo certifikát

metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce. V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.

    Azure AD neposkytuje adresu URL tooget hello metadat. Hello metadata mohou být načteny pouze jako soubor XML.

## <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a>Jak tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie

tooconfigure bez Galerie aplikace, budete muset toohave Azure AD premium a hello aplikace podporuje SAML 2.0. Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)](#configuring-single-sign-on)

-   [Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Načtení metadat Azure AD a certifikátů](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)](#configuring-single-sign-on)

### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)

tooconfigure jednotné přihlašování pro aplikace, která není v galerii Azure AD hello, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno

6.  Klikněte na tlačítko **aplikace bez Galerie** v hello **přidat vlastní aplikaci** části

7.  Zadejte název hello aplikace hello v hello **název** textové pole.

8.  Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

9.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

10. Vyberte **na základě SAML přihlašování** v hello **režimu** rozevíracího seznamu

11. Zadejte hello požadované hodnoty v **domény a adresy URL.** Tyto hodnoty by měly získat od dodavatele aplikace hello.

  1. aplikace hello tooconfigure jako jednotné přihlašování iniciované deklarací identity, zadejte hello adresa URL odpovědi a hello identifikátor.

  2. **Volitelné:** tooconfigure hello aplikace jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.

12. V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.

13. **Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.

   tooadd atribut:

   1. Klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   2. Klikněte na tlačítko **uložit.** Zobrazí hello nový atribut v tabulce hello.

14. Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello. Navíc má Azure AD adresy URL a certifikát nutný pro aplikace hello.

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace

tooselect hello identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  V části hello **uživatelské atributy** vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu. Hello vybranou možnost musí toomatch hello očekávaná hodnota v uživatel hello tooauthenticate aplikace hello.

   >[!NOTE]
   >Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest. Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.
   >
   >

9.  tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.

   tooadd atribut:

   1. klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   Klikněte na 2 **uložit.** Zobrazí hello nový atribut v tabulce hello.

### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Stažení metadat hello Azure AD nebo certifikát

metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce. V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.

    Azure AD neposkytuje adresu URL tooget hello metadat. Hello metadata mohou být načteny pouze jako soubor XML.

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Jak tooconfigure heslo jednotného přihlašování pro aplikaci Galerie Azure AD

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidat aplikaci z Galerie Azure AD hello](#add-an-application)

-   [Konfigurace aplikace hello pro heslo jednotné přihlašování](#configure-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Přidat aplikaci z Galerie Azure AD hello

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno

6.  V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello

7.  Vyberte hello aplikaci tooconfigure pro jednotné přihlašování

8.  Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.

9.  Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

Po krátké době být okno Konfigurace aplikací mít toosee hello.

### <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurace aplikace hello pro heslo jednotné přihlašování

tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

 * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte hello režimu **založené na heslech přihlášení.**

9.  Přiřazení uživatelů toohello aplikace.

10. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo. Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidání aplikace bez Galerie](#add-a-non-gallery-application)

-   [Konfigurace aplikace hello pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Přidání aplikace bez Galerie

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno

6.  Klikněte na tlačítko **Galerie jiná aplikace.**

7.  Zadejte název vaší aplikace hello v hello **název** textové pole. Vyberte **přidat.**

Po krátké době být okno Konfigurace aplikací mít toosee hello.

### <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurace aplikace hello pro heslo jednotné přihlašování

tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

 * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte hello režimu **založené na heslech přihlášení.**

9.  Zadejte hello **přihlašovací adresa URL**. Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k. Zkontrolujte, zda jsou viditelné na adrese URL hello hello přihlášení pole.

10. Přiřazení uživatelů toohello aplikace.

11. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo. Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.

## <a name="how-tooassign-a-user-tooan-application-directly"></a>Jak tooassign tooan aplikace uživatele přímo

tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.

7.  Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.

9.  Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.

10. Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**. Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.

13. Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.

14. **Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.

15. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.

Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Pokud tento postup řešení není hello hello problém vyřešte

Otevřete lístek podpory se hello, pokud je k dispozici následující informace:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   TenantID

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)

