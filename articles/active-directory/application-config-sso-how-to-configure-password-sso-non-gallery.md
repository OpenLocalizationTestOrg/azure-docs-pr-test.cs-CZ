---
title: "aaaHow tooconfigure heslo jednotné přihlašování pro jiný Galerie applicationn | Microsoft Docs"
description: "Jak tooconfigure vlastní aplikace bez Galerie pro zabezpečení založené na heslech jednotné přihlašování při není uveden ve hello Azure AD Application Gallery"
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
ms.openlocfilehash: e3d0e658f792a0a63110a198811edc66acd6ccf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie

Kromě volby toohello najít v galerii aplikací hello Azure AD, máte také možnost tooadd hello **aplikace bez Galerie** když chcete, aby aplikace hello není v seznamu uvedeno. Díky této funkci můžete přidat libovolné aplikace, která již existuje ve vaší organizaci, nebo jakékoli aplikaci jiného výrobce, který můžete použít od dodavatele, který již není součástí hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

Po přidání aplikace bez Galerie je pak můžete nakonfigurovat hello jeden metoda přihlašování tato aplikace používá výběrem hello **jednotné přihlašování** navigační položka na podniková aplikace v hello [portálu Azure ](https://portal.azure.com/).

Jeden z hello jednotné přihlašování metody dostupné tooyou je hello [založené na heslech jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) možnost. S hello **přidání aplikace bez Galerie** prostředí, můžete integrovat jakékoli aplikace, která vykreslí uživatelské jméno ve formátu HTML a heslo vstupní pole, i když není v našem sadu předem integrovaných aplikací.

Hello způsobem, jak to funguje je oškrabávání technologie, která je součástí hello stránku přístupového panelu rozšíření, které umožňuje tooauto-rozpoznat uživatelské jméno a heslo vstupní pole, bezpečně uložit instance konkrétní aplikaci. Bezpečně přehráním uživatelských jmen a hesel toothose pole, když uživatel přejde toothat aplikace na panel přístupu aplikace hello.

Toto je skvělým způsobem tooget spuštění rychle integraci jakékoliv aplikace do Azure AD a umožňuje:

-   Integrovat **všechny aplikace v hello, world** s klientovi Azure AD, takže pokud vykreslí uživatelské jméno a heslo vstupní pole HTML

-   Povolit **jednotné přihlašování pro vaše uživatele** bezpečně ukládání a přehrání uživatelských jmen a hesel pro aplikaci hello jste integrované s Azure AD

-   **Automaticky rozpoznat vstup** polí pro každou aplikaci, a umožňují toomanually zjištění těchto polí pomocí hello rozšíření prohlížeče panely přístup, v případě, že je nenajde automatického zjištění

-   **Podpora aplikací, které vyžadují více polí přihlášení** pro aplikace, které vyžadují více než jen vydávaných toosign v pole uživatelské jméno a heslo

-   **Přizpůsobení popisků hello** hello uživatelské jméno a heslo vstupních polí se uživatelům zobrazí na hello [Panel přístupu aplikace](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) při jejich zadat své přihlašovací údaje

-   Povolit vaše **uživatelé** tooprovide své vlastní uživatelská jména a hesla pro všechny existující účty aplikace zadávání v ručně na hello [aplikace přístupového panelu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Povolit **člen skupiny hello firmy** toospecify hello uživatelských jmen a hesel přiřazený uživatel tooa pomocí hello [samoobslužného přístup k aplikaci](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funkce

-   Povolit **správce** toospecify hello uživatelských jmen a hesel přiřazený uživatel tooa pomocí přihlašovacích údajů aktualizace hello funkci, kdy [přiřazení uživatelská tooan aplikace](#_How_to_configure_1)

-   Povolit **správce** toospecify hello sdílené uživatelské jméno nebo heslo použité pro skupinu uživatelů pomocí přihlašovacích údajů aktualizace hello funkci, kdy [přiřazení skupinu tooan aplikaci](#assign-an-application-to-a-group-directly)

Níže popisuje, jak můžete povolit [založené na heslech jednotné přihlašování](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) tooany aplikace přidat pomocí hello **přidání aplikace bez Galerie** prostředí.

## <a name="overview-of-steps-required"></a>Přehled kroků potřebných

tooconfigure aplikaci z Galerie Azure AD hello budete muset:

-   [Přidání aplikace bez Galerie](#add-a-non-gallery-application)

-   [Konfigurace aplikace hello pro heslo jednotné přihlašování](#configure-the-application-for-password-single-sign-on)

-   [Přiřadit hello aplikace tooa uživatel nebo skupina](#assign-the-application-to-a-user-or-a-group)

    -   [Přiřadit přímo k aplikaci tooan uživatele](#assign-a-user-to-an-application-directly)

    -   [Přiřazení skupiny tooa aplikace přímo](#assign-an-application-to-a-group-directly)

## <a name="add-a-non-gallery-application"></a>Přidání aplikace bez Galerie

tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:

1.  Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno

6.  Klikněte na tlačítko **Galerie jiná aplikace.**

7.  Zadejte název vaší aplikace hello v hello **název** textové pole. Vyberte **přidat.**

Po krátké době být okno Konfigurace aplikací mít toosee hello.

## <a name="configure-hello-application-for-password-single-sign-on"></a>Konfigurace aplikace hello pro heslo jednotné přihlašování

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

## <a name="assign-a-user-tooan-application-directly"></a>Přiřadit přímo k aplikaci tooan uživatele

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

## <a name="assign-an-application-tooa-group-directly"></a>Přiřazení skupiny tooa aplikace přímo

tooassign jeden nebo více skupin tooan aplikace přímo, postupujte podle následujících kroků hello:

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

10. Typ v hello **název úplné skupiny** hello skupiny, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Pozastavte ukazatel myši nad hello **skupiny** v seznamu tooreveal hello **políčko**. Klikněte na tlačítko hello políčko další toohello skupiny profilu fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli příliš**přidat více než jednu skupinu**, typ v jiném **název úplné skupiny** do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole a klikněte na zaškrtávací políčko tooadd hello této skupiny toohello **vybrané** seznamu.

13. Po dokončení výběru skupiny klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.

14. **Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect toohello tooassign role skupinách vybrali.

15. Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybrané skupiny.

Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
