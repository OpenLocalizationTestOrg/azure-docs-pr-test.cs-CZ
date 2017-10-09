---
title: "přihlášení aplikace tooan pomocí odkazu deeplink aaaProblems | Microsoft Docs"
description: "Jak problémy tootroubleshoot přístupu k aplikaci z adresy URL odkazu deeplink pomocí služby Azure AD"
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
ms.openlocfilehash: dc82410001ac05895cc0244c3a89ace71bcf01a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-using-a-deeplink"></a>Potíže s přihlášením tooan aplikace pomocí odkazu deeplink

Hello přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spusťte cloudové aplikace této hello správce Azure AD udělil je přístup k. 

Tyto aplikace jsou konfigurovány jménem uživatele hello na portálu Azure AD hello. Hello aplikace musí být správně nakonfigurované a přiřazené toohello uživatele nebo skupiny hello uživatel je členem toosee hello aplikace hello přístupového panelu.

Přístup uživatele nebo přímých odkazů URL jsou odkazy, které uživatelé mohou používat tooaccess jejich SSO heslo aplikace přímo z jejich adresy URL prohlížečů panely. Přechodem toothis odkaz uživatelé být automaticky přihlášeni hello aplikace bez nutnosti nejprve toogo toohello přístupového panelu. Toto je hello stejného propojení, která uživatelé používají tooaccess těchto aplikací od spuštění aplikace hello Office 365.

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

## <a name="checking-hello-deeplink"></a>Kontrola, zda text hello odkazu deeplink

toocheck, pokud máte správný odkazu deeplink hello, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

7.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

8.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

9.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

10. Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

11. Vyberte hello aplikaci, kterou chcete hello kontrola hello odkazu deeplink pro.

12. Najít hello popisek **adresu URL pro přístup uživatelů**. Tato adresa URL by měl odpovídat můžete odkazu deeplink.

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Jak tooinstall hello rozšíření přístup k panelu prohlížeče

tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:

1.  Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.

3.  V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.

4.  Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení. **Přidat** hello rozšíření tooyour prohlížeče.

5.  Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.

6.  Po instalaci **restartujte** relaci prohlížeče.

7.  Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO

Může také stáhnout hello rozšíření pro Chrome a Firefox z hello přímé odkazy níže:

-   [Rozšíření pro Chrome přístup panely](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření panelu Firefox přístup](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Jak tooconfigure heslo jednotného přihlašování pro aplikaci Galerie Azure AD

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidat aplikaci z Galerie Azure AD hello](#add-an-application-from-the-Azure-AD-gallery)

-   [Konfigurace aplikace hello pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Přidat aplikaci z Galerie Azure AD hello

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.

6.  V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.

7.  Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.

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

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte hello režimu **založené na heslech přihlášení.**

9.  [Přiřazení uživatelů s aplikací toohello](#how-to-assign-a-user-to-an-application-directly).

10. Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo. Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidání aplikace bez Galerie](#add-a-non-gallery-application)

-   [Konfigurace aplikace hello pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Přidání aplikace bez Galerie

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.

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

    1.  Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

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

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Pokud tento postup řešení není hello hello problém vyřešte. 

Otevřete lístek podpory se hello, pokud je k dispozici následující informace:

-   ID chyby korelace

-   Hlavní název uživatele (e-mailovou adresu uživatele)

-   TenantID

-   Typ prohlížeče

-   Dojde k časové pásmo a čas nebo období při chybě

-   Fiddler trasování

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
