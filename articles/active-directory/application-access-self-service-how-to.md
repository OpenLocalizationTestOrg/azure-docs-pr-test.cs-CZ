---
title: "přiřazení samoobslužné služby aplikace tooconfigure aaaHow | Microsoft Docs"
description: "Povolit aplikaci Samoobslužné služby přístup tooallow uživatelé toofind svých vlastních aplikacích"
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
ms.openlocfilehash: d25a0146c4c8cebf9c2ae8c516f094a8eccb4570
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-self-service-application-assignment"></a>Jak přiřazení tooconfigure samoobslužné služby aplikace

Než uživatelům můžete zjistit samoobslužné aplikací z jejich přístupového panelu, musíte tooenable **přístup k aplikaci Samoobslužné služby** tooany aplikace, které chcete tooallow uživatelé tooself-zjišťovat a požádat o přístup k.

Tato funkce je skvělým způsobem, abyste toosave čas a peníze jako IT oddělení a důrazně doporučujeme jako součást nasazení moderních aplikací s Azure Active Directory.

Pomocí této funkce můžete:

-   Umožní uživatelům samoobslužné zjišťování aplikací z hello [Panel přístupu aplikace](https://myapps.microsoft.com/) bez bothering hello IT skupiny.

-   Přidejte tyto skupiny předem nakonfigurovaná tooa uživatelů tak, aby v tématu, který žádá o přístup, odebrání přístupu a spravovat role hello přiřazené toothem.

-   Volitelně povolení schvalovatel obchodní tooapprove aplikace požadavků na přístup, není třeba hello IT oddělení.

-   Volitelně můžete nakonfigurujte až too10 jednotlivce, kteří mohou schválit přístup toothis aplikace.

-   Volitelně můžete povolit schvalovatel obchodní tooset hello hesla uživatele, můžete použít toosign v aplikaci toohello vpravo od schvalovatele hello firmy [Panel přístupu aplikace](https://myapps.microsoft.com/).

-   Volitelně můžete automaticky přiřadíte přímo přiřazené uživatelům samoobslužné služby tooan aplikační role.

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
[Nastavení služby Azure Active Directory pro samoobslužnou správu skupin](active-directory-accessmanagement-self-service-group-management.md)
