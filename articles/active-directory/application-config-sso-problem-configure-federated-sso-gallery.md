---
title: "aaaProblem konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD | Microsoft Docs"
description: "Některé běžné problémy hello se můžete setkat při konfiguraci federovaný jeden adres přihlášení pomocí SAML pro aplikace, které jsou uvedeny v hello Azure AD Application Gallery"
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
ms.openlocfilehash: 2ae1e511bd49b19159e1ab83cf79a7db5403b9a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Problém konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD

Pokud dojde k potížím při konfiguraci aplikace. Ověřte, že jste provedli všechny kroky hello v kurzu hello aplikace hello. V konfiguraci aplikace hello máte vložené dokumentaci na tom, jak tooconfigure hello aplikace. Navíc můžete přístup hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.

## <a name="cant-add-another-instance-of-hello-application"></a>Nelze přidat jiná instance aplikace hello

tooadd druhou instanci aplikace, je třeba toobe moci:

-   Jedinečný identifikátor pro druhou instanci hello nakonfigurujte. Nebudete moct tooconfigure hello, používá stejný identifikátor pro první instanci hello.

-   Nakonfigurujte jiný certifikát než hello, jedna pro hello první instance.

Pokud hello aplikace nepodporuje žádné z výše uvedených hello. Potom nebudete moct tooconfigure druhou instanci.

## <a name="cant-add-hello-identifier-or-hello-reply-url"></a>Nelze přidat hello identifikátor nebo hello adresa URL odpovědi

Pokud nejste možné tooconfigure hello identifikátor nebo hello adresa URL odpovědi, potvrďte hello identifikátor a adresa URL odpovědi hodnoty odpovídat hello vzory předem nakonfigurované pro aplikaci hello.

vzory hello tooknow předem nakonfigurované pro aplikaci hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.** Přejděte toostep 7. Pokud jste už v okně Konfigurace aplikace hello na Azure AD.

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte **na základě SAML přihlašování** z hello **režimu** rozevíracího seznamu.

9.  Přejděte toohello **identifikátor** nebo **adresa URL odpovědi** textovému poli, v části hello **oddílu domény a adresy URL.**

10. Existují tři způsoby tooknow hello podporované vzory pro aplikace hello:

   * V textovém poli hello, uvidíte nalezeny hello podporovány jako zástupný znak *příklad:* <https://contoso.com>.

   * Pokud vzor hello není podporován, zobrazí se při pokusu o tooenter hello hodnotu v textovém poli hello červený vykřičník. Umístěte ukazatel myši nad hello červený vykřičník, uvidíte vzory hello podporována.

   * V kurzu hello hello aplikace můžete také načíst informace o vzory hello podporována. V části hello **konfigurovat Azure AD jednotné přihlašování** části. Přejděte toohello krok pro hodnoty nakonfigurované hello pod hello **domény a adresy URL** části.

Pokud hello se hodnoty neshodují s hello vzory předem nakonfigurované na Azure AD. Můžete:

-   Práce s hello aplikace dodavatele tooget hodnot, které odpovídají hello vzor předem nakonfigurované na Azure AD

-   Nebo se obrátit na tým Azure AD v < aadapprequest@microsoft.com > nebo komentář v hello kurz toorequest hello aktualizace schémat hello podporována pro aplikaci hello

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a>Kde lze nastavit formát hello EntityID (identifikátor uživatele)

Nebudete moct tooselect hello EntityID (identifikátor uživatele) formát, Azure AD odešle toohello aplikace v odpovědi hello po ověření uživatele.

Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest. Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části hello NameIDPolicy,

## <a name="cant-find-hello-azure-ad-metadata-toocomplete-hello-configuration-with-hello-application"></a>Nelze najít hello hello konfigurace toocomplete metadata Azure AD s aplikací hello

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

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a>Nevíte, jak deklarace SAML toocustomize odeslané tooan aplikace

toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
