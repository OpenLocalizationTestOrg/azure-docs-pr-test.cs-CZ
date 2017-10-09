---
title: "aaaHow tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie | Microsoft Docs"
description: "Jak tooconfigure federovaného jednotného přihlašování pro vlastní aplikaci bez galerie, které chcete toointegrate s Azure AD"
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
ms.openlocfilehash: f4a37cb500c075d0ce917ad8f13411f2ce208c56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a>Jak tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie

tooconfigure bez Galerie aplikace, budete muset toohave Azure AD premium a hello aplikace podporuje SAML 2.0. Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="overview-of-steps-required"></a>Přehled kroků potřebných
Zde je podrobný přehled hello kroky požadované tooconfigure federovaného jednotného přihlašování pro jiný Galerie (například vlastní) aplikace.

-   [Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)](#_Configuring_single_sign-on)

-   [Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Načtení metadat Azure AD a certifikátů](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)](#_Configuring_single_sign-on)

-   [Přiřazení uživatelů toohello aplikace](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-toonon-gallery-applications"></a>Konfigurace jednoho přihlášení toonon Galerie aplikací

tooconfigure jednotné přihlašování pro aplikace, která není v galerii Azure AD hello, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.

6.  Klikněte na tlačítko **aplikace bez Galerie** v hello **přidat vlastní aplikaci** části

7.  Zadejte název hello aplikace hello v hello **název** textové pole.

8.  Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.

9.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

10. Vyberte **na základě SAML přihlašování** v hello **režimu** rozevíracího seznamu.

11. Zadejte hello požadované hodnoty v **domény a adresy URL.** Tyto hodnoty by měly získat od dodavatele aplikace hello.

   1. aplikace hello tooconfigure jako jednotné přihlašování iniciované deklarací identity, zadejte hello adresa URL odpovědi a hello identifikátor.

   2. **Volitelné:** tooconfigure hello aplikace jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.

12. V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.

13. **Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.

   tooadd atribut:

   1. Klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   2. Klikněte na tlačítko **uložit.** Zobrazí hello nový atribut v tabulce hello.

14. Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello. Navíc má Azure AD adresy URL a certifikát nutný pro aplikace hello.

15. [Přiřazení uživatelů toohello aplikace.](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace

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

 >[! Poznámka:} Azure AD, vyberte formát hello atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello hello aplikací v hello SAML AuthRequest požadovaný formát. Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.
 >
 >

9.  tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.

   tooadd atribut:

   1. Klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   2. Klikněte na tlačítko **uložit.** Zobrazí hello nový atribut v tabulce hello.

## <a name="download-hello-azure-ad-metadata-or-certificate"></a>Stažení metadat hello Azure AD nebo certifikát

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

## <a name="assign-users-toohello-application"></a>Přiřazení uživatelů toohello aplikace

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

Po krátké době být hello uživatelů, které jste vybrali možnost toolaunch hello tyto aplikace pomocí metody popsané v části popis řešení hello.

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a>Přizpůsobují se deklarace identity SAML hello odeslané tooan aplikace

toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
