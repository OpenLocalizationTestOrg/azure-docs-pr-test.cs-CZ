---
title: "hello přístupového panelu se nezobrazují aaaAn přiřazené aplikace | Microsoft Docs"
description: "Poradce při potížích se aplikace nejsou zobrazeny v hello přístupového panelu"
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
ms.reviwer: japere
ms.openlocfilehash: 089883f406267df4552c7fc991883f335ad49fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-assigned-application-is-not-appearing-on-hello-access-panel"></a>Přiřazená aplikace se nezobrazují hello přístupového panelu

Hello přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spusťte cloudové aplikace této hello správce Azure AD udělil je přístup k. Tyto aplikace jsou konfigurovány jménem uživatele hello na portálu Azure AD hello. Hello aplikace musí být správně nakonfigurované a přiřazené toohello uživatele nebo skupiny hello uživatel je členem toosee hello aplikace hello přístupového panelu.

Typ Hello aplikací, které zobrazuje uživatel možná spadat hello následujících kategorií:

-   Aplikace Office 365

-   Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation

-   Aplikace založené na heslech jednotného přihlašování

-   Aplikace s stávajících řešení pro jednotné přihlašování

## <a name="general-issues-toocheck-first"></a>Obecné problémy toocheck nejprve

-   Pokud aplikaci jste právě přidali tooa uživatele, opakujte toosign a odhlašování do přístupového panelu hello uživatele po několika minutách toosee Pokud aplikace hello je přidána.

-   Pokud licence byla právě odebrána ze uživatele nebo skupiny hello uživatel je že členem skupiny to může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny pro toobe změny provedené. Povolit další dobu před přihlášením hello přístupového panelu.

## <a name="problems-related-tooapplication-configuration"></a>Problémy související tooapplication konfigurace

Aplikace nemusí zobrazovaných v Panel přístupu uživatele, protože není správně nakonfigurována. Tady jsou některé způsoby, jak můžete řešit problémy související tooapplication konfigurace:

-   [Jak tooconfigure federovaného jednotného přihlašování pro aplikaci Galerie Azure AD](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Jak tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Jak tooconfigure heslo jednotné přihlašování v aplikaci pro aplikaci Galerie Azure AD](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Jak tooconfigure heslo jednotné přihlašování aplikace pro aplikaci není Galerie](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Jak tooconfigure federovaného jednotného přihlašování pro aplikaci Galerie Azure AD

Všechny aplikace v galerii Azure AD hello povolit funkce Enterprise Single Sign-On obsahuje podrobný kurz k dispozici. Dostanete hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidat aplikaci z Galerie Azure AD hello](#add-an-application-from-the-azure-ad-gallery)

-   [Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Načtení metadat Azure AD a certifikátů](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Přidat aplikaci z Galerie Azure AD hello

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.

6.  V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.

7.  Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.

8.  Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.

9.  Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

Po krátké době být okno Konfigurace aplikací mít toosee hello.

#### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a>Konfigurovat jednotné přihlašování pro aplikace z Galerie Azure AD hello

tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

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

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace

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

   2. Klikněte na tlačítko **uložit.** Zobrazí se hello nový atribut v tabulce hello.

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Stažení metadat hello Azure AD nebo certifikát

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

### <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a>Jak tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie

tooconfigure bez Galerie aplikace, budete muset toohave Azure AD premium a hello aplikace podporuje SAML 2.0. Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)](#configuring-single-sign-on)

-   [Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Načtení metadat Azure AD a certifikátů](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)](#configuring-single-sign-on)

#### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)

tooconfigure jednotné přihlašování pro aplikace, která není v galerii Azure AD hello, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.

6.  Klikněte na tlačítko **aplikace bez Galerie** v hello **přidat vlastní aplikaci** části.

7.  Zadejte název hello aplikace hello v hello **název** textové pole.

8.  Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

9.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

10. Vyberte **na základě SAML přihlašování** v hello **režimu** rozevíracího seznamu.

11. Zadejte hello požadované hodnoty v **domény a adresy URL.** Tyto hodnoty by měly získat od dodavatele aplikace hello.

   1. aplikace hello tooconfigure jako jednotné přihlašování iniciované deklarací identity, zadejte hello adresa URL odpovědi a hello identifikátor.

   2.  **Volitelné:** tooconfigure hello aplikace jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.

12. V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.

13. **Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.

   tooadd atribut:

   1. Klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   2. Klikněte na tlačítko **uložit.** Zobrazí hello nový atribut v tabulce hello.

14. Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello. Navíc má Azure AD adresy URL a certifikát nutný pro aplikace hello.

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace

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

   1. Klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   2. Klikněte na tlačítko **uložit.** Zobrazí hello nový atribut v tabulce hello.

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Stažení metadat hello Azure AD nebo certifikát

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

### <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Jak tooconfigure heslo jednotného přihlašování pro aplikaci Galerie Azure AD

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidat aplikaci z Galerie Azure AD hello](#add-an-application-from-the-azure-ad-gallery)

-   [Konfigurace aplikace hello pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Přidat aplikaci z Galerie Azure AD hello

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.

6.  V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.

7.  Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.

8.  Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.

9.  Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

Po krátké době být okno Konfigurace aplikací mít toosee hello.

#### <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurace aplikace hello pro heslo jednotné přihlašování

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

9.  [Přiřazení uživatelů s aplikací toohello](#how-to-assign-a-user-to-an-application-directly).

10. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo. Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.

### <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidání aplikace bez Galerie](#add-a-non-gallery-application)

-   [Konfigurace aplikace hello pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a>Přidání aplikace bez Galerie

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.

6.  Klikněte na tlačítko **Galerie jiná aplikace.**

7.  Zadejte název vaší aplikace hello v hello **název** textové pole. Vyberte **přidat.**

Po krátké době být okno Konfigurace aplikací mít toosee hello.

#### <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurace aplikace hello pro heslo jednotné přihlašování

tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

    1.  Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte hello režimu **založené na heslech přihlášení.**

9.  Zadejte hello **přihlašovací adresa URL**. Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k. Zkontrolujte, zda jsou viditelné na adrese URL hello hello přihlášení pole.

10. [Přiřazení uživatelů s aplikací toohello](#how-to-assign-a-user-to-an-application-directly).

11. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo. Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.

## <a name="problems-related-tooassigning-applications-toousers"></a>Problémy související tooassigning aplikace toousers

Uživatel nemusí být zobrazuje aplikace na jejich přístupového panelu nejsou přiřazeny toohello aplikace. Tady jsou některé toocheck způsoby:

-   [Zkontrolujte, pokud uživatel je přiřazená toohello aplikace](#check-if-a-user-is-assigned-to-the-application)

-   [Jak tooassign tooan aplikace uživatele přímo](#how-to-assign-a-user-to-an-application-directly)

-   [Zkontrolujte, zda uživatel je přiřazena licence tooa související toohello aplikace](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Jak tooassign uživatel tooa licencí](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-toohello-application"></a>Zkontrolujte, pokud uživatel je přiřazená toohello aplikace

toocheck Pokud uživatel přiřazený toohello aplikace, postupujte podle kroků hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

6.  **Hledání** pro název hello aplikace hello nejistá.

7.  Klikněte na tlačítko **uživatelů a skupin**.

8.  Toosee zkontrolujte, zda váš uživatel toohello aplikace.

   * Pokud ne, postupujte podle kroků hello v "jak tooassign tooan aplikace uživatele přímo" toodo tak.

### <a name="how-tooassign-a-user-tooan-application-directly"></a>Jak tooassign tooan aplikace uživatele přímo

tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce**.

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

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a>Zkontrolujte, jestli je uživatel v rámci licence související toohello aplikace

toocheck uživatele je přiřazena licence, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.

  * Pokud uživatel hello přiřazené tooan Office licence tato povolí tooappear aplikace první strany Office na Panel přístupu hello uživatele.

### <a name="how-tooassign-a-user-a-license"></a>Jak tooassign uživatelské licence 

tooassign licence tooa uživatele, postupujte podle kroků hello níže:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, kteří uživatelé hello licence má přiřazeny.

8.  Klikněte na tlačítko hello **přiřadit** tlačítko.

9.  Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.

10. **Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty. Klikněte na tlačítko **Ok** po dokončení.

11. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign tyto licence toothis uživatele.

## <a name="problems-related-tooassigning-applications-toogroups"></a>Problémy související tooassigning aplikace toogroups

Uživatele může být zobrazuje aplikace na jejich přístupového panelu jsou součástí skupiny, která byla přiřazena aplikace hello. Tady jsou některé toocheck způsoby:

-   [Zkontrolujte členství uživatele ve skupinách](#check-a-users-group-memberships)

-   [Jak tooassign aplikaci tooa skupiny přímo](#how-to-assign-an-application-to-a-group-directly)

-   [Zkontrolujte, zda uživatel je součástí skupiny přiřazené licence tooa](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Jak tooassign tooa skupiny licencí](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a>Zkontrolujte členství uživatele ve skupinách

toocheck členství ve skupině, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **skupiny**.

8.  Zkontrolujte toosee, pokud uživatel je součástí aplikace toohello skupiny přiřazené.

  * Pokud chcete, aby tooremove hello uživatele ze skupiny hello **klikněte na řádek hello** hello skupiny a vyberte možnost odstranit.

### <a name="how-tooassign-an-application-tooa-group-directly"></a>Jak tooassign aplikaci tooa skupiny přímo

tooassign jeden nebo více skupin tooan aplikace přímo, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce**.

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.

7.  Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.

9.  Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.

10. Typ v hello **název úplné skupiny** hello skupiny, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Pozastavte ukazatel myši nad hello **skupiny** v seznamu tooreveal hello **políčko**. Klikněte na tlačítko hello políčko další toohello skupiny profilu fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli příliš**přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole a klikněte na zaškrtávací políčko tooadd hello této skupiny toohello **vybrané** seznamu.

13. Po dokončení výběru skupiny klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.

14. **Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect toohello tooassign role skupinách vybrali.

15. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybrané skupiny.

Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.

### <a name="check-if-a-user-is-part-of-group-assigned-tooa-license"></a>Zkontrolujte, zda uživatel je součástí skupiny přiřazené licence tooa

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všichni uživatelé**.

6.  **Hledání** pro uživatele hello vás zajímá a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **skupiny**.

8.  Klikněte na řádek hello určité skupiny.

9.  Klikněte na tlačítko **licence** toosee, které skupiny licencí hello přiřazenému tooit.

   * Pokud skupina hello přiřazené tooan Office licence to může povolit určité aplikace tooappear první strany Office na Panel přístupu hello uživatele.

### <a name="how-tooassign-a-license-tooa-group"></a>Jak tooassign tooa skupiny licencí

tooassign tooa skupiny licencí, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **uživatelů a skupin** v navigační nabídce hello.

5.  Klikněte na tlačítko **všechny skupiny**.

6.  **Hledání** hello skupiny, které vás zajímají a **klikněte na řádek hello** tooselect.

7.  Klikněte na tlačítko **licence** toosee, které skupiny licencí hello má přiřazeny.

8.  Klikněte na tlačítko hello **přiřadit** tlačítko.

9.  Vyberte **jeden nebo více produktů** z hello seznam dostupných produktů.

10. **Volitelné** klikněte na tlačítko hello **přiřazení možností** položky toogranularly přiřadit produkty. Klikněte na tlačítko **Ok** po dokončení.

11. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign těchto toothis skupin licencí. To může trvat dlouhou dobu, v závislosti na hello velikost a složitost hello skupiny.

>[!NOTE]
>toodo to rychlejší, vezměte v úvahu dočasně přiřazení licence toohello uživatele přímo. 
>
>

## <a name="next-steps"></a>Další kroky
[Přidat nové uživatele tooAzure služby Active Directory](active-directory-users-create-azure-portal.md)

