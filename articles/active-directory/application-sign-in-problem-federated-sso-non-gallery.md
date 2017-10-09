---
title: "aaaProblems přihlášení aplikace bez Galerie tooa konfigurovaná pro federované jednotné přihlašování | Microsoft Docs"
description: "Pokyny pro hello konkrétních problémů, kterým může být vystaven při přihlášení tooan aplikace konfigurovaná pro na základě SAML federované jednotné přihlašování s Azure AD"
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
ms.openlocfilehash: 1243456695c097f404a66fc89893efa2afdaaf22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooa-non-gallery-application-configured-for-federated-single-sign-on"></a>Potíže s přihlášením aplikace bez Galerie tooa konfigurovaná pro federované jednotné přihlašování

tootroubleshoot problém, je třeba konfigurace aplikace hello tooverify ve službě Azure AD jako postupujte podle kroků:

-   Jste provedli všechny kroky konfigurace hello pro hello galerii aplikací Azure AD.

-   Hello identifikátor a adresa URL odpovědi, které jsou nakonfigurované v AAD shodovat se očekávaných hodnot v aplikaci hello

-   Přiřadili jste uživatelé toohello aplikace

## <a name="application-not-found-in-directory"></a>Aplikace nebyla nalezena v adresáři

*Chyba AADSTS70001: Aplikace s identifikátorem 'https://contoso.com' nebyl nalezen v adresáři hello*.

**Možná příčina**

atribut vystavitele Hello odešle z tooAzure hello aplikací že AD v hello SAML požadavku se neshoduje se hello identifikátor hodnotu nakonfigurovanou v hello aplikace Azure AD.

**Řešení**

Ujistěte se, tento atribut hello vystavitele v hello SAML požadavku, že je jeho odpovídající hello identifikátor hodnotu nakonfigurovanou v Azure AD:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  <span id="_Hlk477190042" class="anchor"></span>Přejděte příliš**domény a adresy URL** části. Ověřte, zda text hello hodnota v hello identifikátor textové pole je odpovídající hodnota hello hodnota identifikátoru hello zobrazí chyba hello.

Poté, co jste aktualizovali hodnota identifikátoru hello ve službě Azure AD a jeho odpovídající hello hodnotu odešle hello aplikací v žádosti SAML hello, musí být schopný toosign v toohello aplikaci.

## <a name="hello-reply-address-does-not-match-hello-reply-addresses-configured-for-hello-application"></a>Adresa pro odpověď Hello neodpovídá hello odpovědi adresy nakonfigurované pro aplikace hello. 

*Chyba AADSTS50011: adresa odpovědi hello 'https://contoso.com' neodpovídá hello odpovědi adresy nakonfigurované pro aplikace hello* 

**Možná příčina** 

Hello žádost SAML hello hodnotu AssertionConsumerServiceURL neodpovídá hello adresa URL odpovědi hodnotou nebo vzorem, které jsou nakonfigurované ve službě Azure AD. Hello AssertionConsumerServiceURL hodnota v žádosti SAML hello je hello URL se zobrazí chyba hello. 

**Řešení** 

Zkontrolujte hodnotu AssertionConsumerServiceURL hello do žádost SAML hello jeho odpovídající hello adresa URL odpovědi hodnota nakonfigurovat ve službě Azure AD. 
 
1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.** 

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello. 

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky. 

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky. 

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací. 

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**
  
6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Přejděte příliš**domény a adresy URL** části. Ověřte nebo aktualizujte hodnotu hello v hello adresa URL odpovědi textbox toomatch hello žádost SAML hello AssertionConsumerServiceURL hodnotu.

  * Pokud nevidíte textbox hello adresa URL odpovědi, vyberte hello **zobrazit upřesňující nastavení adresy URL** zaškrtávací políčko. 

Poté, co byly aktualizovány hello adresa URL odpovědi hodnotu ve službě Azure AD a má odpovídající hodnotu hello odešle hello aplikací v hello žádost SAML, byste měli mít toosign v toohello aplikaci.

## <a name="user-not-assigned-a-role"></a>Není přiřazen k roli uživatele

*Chyba AADSTS50105: hello přihlášení uživatele sebrian@contoso.com' není přiřazena role tooa aplikace hello*

**Možná příčina**

Hello uživateli nebyl udělen přístup toohello aplikace ve službě Azure AD.

**Řešení**

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

## <a name="not-a-valid-saml-request"></a>Není platný SAML požadavku na

*Chyba AADSTS75005: hello požadavku není platný zprávu protokolu typu Saml2.*

**Možná příčina**

Azure AD nepodporuje hello SAML požadavku odeslaná hello aplikací pro jednotné přihlašování. Jsou některé běžné problémy:

-   Chybí povinná pole v požadavku SAML hello

-   Metoda požadavku kódovaný SAML

**Řešení**

1.  Zaznamenejte žádost SAML. postupujte podle kurzu hello na [jak toodebug na základě SAML jeden přihlašování tooapplications ve službě Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn, jak požádat o toocapture hello SAML.

2.  Obraťte se na dodavatele aplikace hello a sdílené složky:

    -   Žádost SAML

    -   [Protokol požadavky pro Azure AD jedním přihlašování SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Se musí ověřit podporují implementace hello Azure AD SAML pro jednotné přihlašování.

## <a name="no-resource-in-requiredresourceaccess-list"></a>Žádný prostředek v seznamu requiredResourceAccess

*Chyba AADSTS65005: hello klientská aplikace vyžaduje přístup tooresource ' 00000002-0000-0000-c000-000000000000'. Tento požadavek se nezdařil, protože hello klienta nebyl zadán tento prostředek ve svém seznamu requiredResourceAccess*.

**Možná příčina**

objekt aplikace Hello je poškozený.

**Řešení**

toosolve hello problém, aplikace hello odebrat z adresáře hello. Pak přidejte a překonfigurujte hello aplikace, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Klikněte na tlačítko **odstranit** v levém horním hello aplikace hello **přehled** okno.

8.  Aktualizujte Azure AD a přidání aplikace hello z Galerie Azure AD hello. Potom nakonfigurujte hello aplikaci znovu.

Po překonfigurování hello aplikace, musí být schopný toosign v toohello aplikaci.

## <a name="certificate-or-key-not-configured"></a>Certifikát nebo klíče není nakonfigurováno

Chyba AADSTS50003: Žádné podpisový klíč nakonfigurován.

**Možná příčina**

objekt aplikace Hello je poškozený a Azure AD nerozpoznal hello certifikát nakonfigurovaný pro aplikaci hello.

**Řešení**

toodelete a vytvořte nový certifikát, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko **vytvořit nový certifikát** pod hello **SAML podpisový certifikát** části.

9.  Vyberte datum vypršení platnosti. Potom klikněte na **uložit.**

10. Zkontrolujte **aktivujte nový certifikát** toooverride hello active certifikátu. Potom klikněte na **Uložit** hello horní části okna hello a přijmout certifikát výměny tooactivate hello.

11. V části hello **SAML podpisový certifikát** klikněte na tlačítko **odebrat** tooremove hello **nepoužitý** certifikátu.

## <a name="problem-when-customizing-hello-saml-claims-sent-tooan-application"></a>Problém při přizpůsobení deklarace SAML hello odeslané tooan aplikace

toolearn způsobu deklarací identity atributu toocustomize hello SAML odeslané tooyour aplikace, najdete v [deklarací mapování v Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) Další informace.

## <a name="next-steps"></a>Další kroky
[Protokol požadavky pro Azure AD jedním přihlašování SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)
