---
title: "na stránce aplikace po přihlášení aaaError | Microsoft Docs"
description: "Jak tooresolve problémy s Azure AD přihlášení během vlastní aplikace hello vysílá chybu"
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
ms.openlocfilehash: 317b6f8e6417520ead80ae4e26c591ba6b134683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Chyby na stránce aplikace po přihlášení

V tomto scénáři podepsané hello uživatele v Azure AD, ale aplikace hello zobrazuje chybu neumožňuje hello uživatele toosuccessfully dokončit hello přihlašovací v toku. V tomto scénáři aplikace hello nepřijímá hello odpovědi problém službou Azure AD.

Existují některé možné důvody, proč nebylo aplikace hello přijímat hello odpověď z Azure AD. Pokud hello Chyba v aplikaci hello není dostatečně zrušte tooknow toho, co chybí v hello odpověď, a pak:

-   Pokud aplikace hello Galerie hello Azure AD, ověřte všechny kroky hello v článku hello [jak toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler) toocapture SAML požadavků, odpověď SAML a tokenu SAML.

-   Sdílet odpověď SAML hello s tooknow dodavatele aplikace hello toho, co chybí.

## <a name="missing-attributes-in-hello-saml-response"></a>Chybějící atributů v hello odpověď SAML

tooadd atribut v toobe konfigurace Azure AD hello odeslaný v odpovědi hello Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko **prohlížení a úpravy atributy všechny ostatní uživatele v části** hello **uživatelské atributy** části tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.

   tooadd atribut:

   * Klikněte na tlačítko **přidat atribut**. Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.

   * Klikněte na tlačítko **uložit.** Zobrazí hello nový atribut v tabulce hello.

9.  Uložte konfiguraci hello.

Při příštím přihlášení uživatele hello toohello aplikace Azure AD odeslat hello nový atribut v hello odpověď SAML.

## <a name="hello-application-expects-a-different-user-identifier-value-or-format"></a>aplikace Hello očekává formát nebo jinou hodnotu identifikátor uživatele

Hello přihlášení toohello aplikace selhává protože hello odpověď SAML chybí atributy, jako je například role nebo aplikace hello je očekáván jiný formát hello EntityID atribut.

## <a name="add-an-attribute-in-hello-azure-ad-application-configuration"></a>Přidáte atribut v konfiguraci aplikace hello Azure AD:

toochange hello hodnota identifikátoru uživatele, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  V části hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.

## <a name="change-entityid-user-identifier-format"></a>Změna formátu EntityID (identifikátor uživatele)

Pokud aplikace hello očekává jiného formátu pro atribut EntityID hello. Potom nebudete moct tooselect hello EntityID (identifikátor uživatele) formát, Azure AD odešle toohello aplikace v odpovědi hello po ověření uživatele.

Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest. Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.

## <a name="hello-application-expects-a-different-signature-method-for-hello-saml-response"></a>aplikace Hello očekává jiný podpis metody pro hello odpověď SAML

toochange, které části tokenu SAML hello jsou digitálně podepsané službou Azure Active Directory. Postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko **zobrazit upřesňující nastavení podpisový certifikát** pod hello **SAML podpisový certifikát** části.

9.  Vyberte odpovídající hello **podepisování možnost** očekávanou hello aplikace:

  * Přihlašování SAML odpovědi

  * Přihlašování SAML odpovědi a kontrolní výraz

  * Přihlašovací kontrolního výrazu SAML

Při příštím přihlášení uživatele hello toohello aplikace Azure AD přihlášení hello součástí vybrané odpověď SAML hello.

## <a name="hello-application-expects-hello-signing-algorithm-toobe-sha-1"></a>aplikace Hello očekává hello podepisování toobe algoritmus SHA-1

Ve výchozím nastavení Azure AD podepisuje tokeny SAML hello pomocí hello většina algoritmus zabezpečení. Změna hello přihlašovací algoritmus tooSHA-1 se nedoporučuje, pokud se vyžaduje aplikace hello.

toochange hello podpisový algoritmus, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Klikněte na tlačítko **zobrazit upřesňující nastavení podpisový certifikát** pod hello **SAML podpisový certifikát** části.

9.  Vyberte algoritmus SHA-1, v hello **podpisový algoritmus**.

Uživatel se přihlásí v aplikaci toohello hello příště, Azure AD přihlášení hello tokenu SAML pomocí algoritmu SHA-1.

## <a name="next-steps"></a>Další kroky
[Jak toodebug na základě SAML jeden přihlašování tooapplications v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
