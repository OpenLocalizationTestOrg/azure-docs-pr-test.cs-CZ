---
title: "aaaHow tooassign uživatelů a skupin tooan aplikace | Microsoft Docs"
description: "Přiřazení uživatelů toohello aplikace toogrant přístup"
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
ms.openlocfilehash: e039a26e4b8f88ad747354859f1071b8f74b6789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-and-groups-tooan-application"></a>Jak aplikace tooan tooassign uživatelů a skupin

Než mohou uživatelé provádět žádné hello pod pro konkrétní aplikaci, musíte toofirst **přiřaďte jim toohello aplikace** toogrant je přístup:

-   Přístup k aplikaci pomocí **přejdete přímo adresu URL aplikace toohello** (také označované jako spouštěná SP přihlášení).

-   Přístup k aplikaci pomocí hello **adresu URL pro přístup uživatelů** na aplikace **vlastnosti** stránky (také označované jako spouštěná IDP přihlašování).

-   V tématu aplikace se zobrazují v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/) nebo mobilních aplikací.

-   V tématu aplikace se zobrazují v jejich [Spouštěč aplikace Office 365](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).

## <a name="methods-tooassign-applications-with-azure-active-directory"></a>Metody tooassign aplikací s Azure Active Directory 

Aplikace s Azure Active Directory můžete přiřadit 3 způsoby:

-   [Přiřazení uživatele přímo tooan aplikace jako správce](#assign-a-user-directly-as-an-administrator)

-   [Přiřadit ke skupině přímo tooan aplikace jako správce](#assign-a-group-directly-to-an-application-as-an-administrator)

-   [Povolit aplikaci Samoobslužné služby přístup tooallow uživatelé toofind svých vlastních aplikacích](#enable-self-service-application-access-to-allow-users-to-find-their-own-applications)

## <a name="assign-a-user-directly-as-an-administrator"></a>Přiřazení uživatele přímo jako správce

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

## <a name="assign-a-group-directly-tooan-application-as-an-administrator"></a>Přiřadit ke skupině přímo tooan aplikace jako správce

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

Po krátké době být hello uživatelů v rámci hello skupin, které jste vybrali možnost toolaunch hello tyto aplikace pomocí metody popsané v části popis řešení hello. Pokud jsou dynamické skupiny, může být zpoždění některé další zpracování v těchto přiřazení zobrazování pro uživatele v rámci těchto přiřazeny skupiny.

## <a name="enable-self-service-application-access-tooallow-users-toofind-their-own-applications"></a>Povolit aplikaci Samoobslužné služby přístup tooallow uživatelé toofind svých vlastních aplikacích

Přístup k aplikaci Samoobslužné služby je skvělý způsob tooallow uživatelé tooself-zjišťovat aplikace, můžete také povolit hello firmy skupiny tooapprove přístup toothose aplikace. Můžete povolit, že pověření hello hello firmy skupiny toomanage přiřadili toothose uživatelů pro heslo jednotné přihlašování v aplikacích přímo z jejich panelů přístup.

tooenable aplikace Samoobslužné služby přístup tooan aplikace, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte aplikaci hello chcete tooenable samoobslužné služby přístup toofrom hello seznamu.

7.  Po načtení hello aplikace, klikněte na **samoobslužné služby** z aplikace hello levém navigační nabídky.

8.  tooenable přístup k aplikaci Samoobslužné služby pro tuto aplikaci, aktivujte hello **povolit uživatelům toorequest přístup toothis aplikace?** přepnutí příliš**Ano.**

9.  V dalším kroku tooselect hello skupiny toowhich uživatelů, kteří požadují přístup toothis aplikace by měla být přidány, klikněte na tlačítko hello selektor další toohello popisek **toowhich skupina by měla přiřazená byli přidáni uživatelé?** a vyberte skupinu.

10. **Volitelné:** nechcete-li toorequire obchodní schválení předtím, než mohou uživatelé přístup, nastavte hello **vyžadovat schválení před udělením přístupu toothis aplikace?** přepnutí příliš**Ano**.

11. **Volitelné: pro aplikace pomocí hesla jednotné přihlašování na pouze** Pokud chcete tyto firmy schvalovatelů toospecify hello hesel, která se posílají toothis žádost o schválení uživatelé tooallow, nastavte hello **tooset schvalovatelů povolit hesla uživatele pro tuto aplikaci?**  přepnutí příliš**Ano**.

12. **Volitelné:** toospecify hello firmy schvalovatelů, kteří mají povoleno tooapprove přístup toothis aplikace, klikněte na tlačítko hello selektor další toohello popisek **kdo je povolená tooapprove přístup toothis aplikace?** tooselect nahoru too10 jednotlivé obchodní schvalovatelů.

  >[!NOTE]
  >Skupiny nejsou podporovány.
  >
  >

13. **Volitelné:** **pro aplikace, které zveřejňují role**, pokud chcete, aby role tooa tooassign schválené uživatelé samoobslužné služby, klikněte na tlačítko Další toohello hello selektor **toowhich role lze přiřadit uživatelům v této aplikace?**  tooselect hello role toowhich by měla být přiřazená tyto uživatele.

14. Klikněte na tlačítko hello **Uložit** tlačítko hello horní části okna toofinish hello.

Po dokončení konfigurace samoobslužné služby aplikace, uživatelé mohou přejít tootheir [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko hello **+ přidat** tlačítko toofind hello aplikace toowhich jste povolili Samoobslužné služby přístup. Obchodní schvalovatelů také zobrazit oznámení v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/). Můžete povolit e-mail s upozorněním, když uživatel požaduje přístup tooan aplikace, která vyžaduje schválení. 

Tato schválení podporovat pouze jeden schválení pracovních, tj. Pokud zadáte několik schvalovatelů, jeden schvalovatel může schvalovatel přístup toohello aplikace.

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
