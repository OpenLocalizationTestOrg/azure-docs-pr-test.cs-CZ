---
title: "Postup konfigurace samoobslužné služby aplikace přiřazení | Microsoft Docs"
description: "Povolit přístup k aplikaci Samoobslužné služby umožnit uživatelům najít vlastní aplikace"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cfa3d9803cd66308f21241bd61816654fd92eb2b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-configure-self-service-application-assignment"></a>Postup konfigurace samoobslužné služby aplikace přiřazení

Než uživatelům můžete zjistit samoobslužné aplikací z jejich přístupového panelu, musíte povolit **přístup k aplikaci Samoobslužné služby** pro všechny aplikace, které chcete povolit uživatelům samoobslužné zjišťování a požádat o přístup k.

Tato funkce je skvělý způsob, jak ušetřit čas i finanční prostředky jako skupinu IT a důrazně doporučujeme jako součást nasazení moderních aplikací s Azure Active Directory.

Pomocí této funkce můžete:

-   Umožní uživatelům samoobslužné zjišťování aplikací z [Panel přístupu aplikace](https://myapps.microsoft.com/) bez bothering IT oddělení.

-   Přidání těchto uživatelů do skupinu předem nakonfigurovaná tak, aby v tématu, který žádá o přístup, odebrat přístup a spravovat role přiřazené k nim.

-   Volitelně můžete povolit obchodní schvalovatel ke schválení žádosti o přístup aplikace, není třeba IT oddělení.

-   Volitelně můžete nakonfigurujte až 10 jednotlivce, kteří mohou schválit přístup k této aplikaci.

-   Volitelně můžete povolit obchodní schvalovatel k nastavení hesla uživatele, můžete použít k přihlášení do aplikace, vpravo od schvalovatele obchodní [Panel přístupu aplikace](https://myapps.microsoft.com/).

-   Volitelně můžete automaticky přiřadíte přímo přiřazené aplikační role uživatele samoobslužné služby.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Povolit přístup k aplikaci Samoobslužné služby umožnit uživatelům najít vlastní aplikace

Přístup k aplikaci Samoobslužné služby je skvělým způsobem, jak povolit uživatelům samoobslužné zjišťování aplikací, můžete povolit obchodní skupiny můžete schválit přístup pro tyto aplikace. Můžete povolit obchodní skupině pro správu přiřazené pro tyto uživatele pro heslo jednotné přihlašování v aplikacích vpravo z jejich přístup panelů přihlašovací údaje.

Pokud chcete povolit samoobslužné služby aplikaci přístup k aplikaci, postupujte podle následujících kroků:

1.  Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.

3.  Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.

  * Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**

6.  Vyberte aplikaci, které chcete povolit samoobslužné přístup ze seznamu.

7.  Po načtení aplikace, klikněte na **samoobslužné služby** navigační nabídce vlevo aplikace.

8.  Pokud chcete povolit přístup k aplikaci Samoobslužné služby pro tuto aplikaci, zapněte **povolit uživatelům žádat o přístup k této aplikaci?** přepnutím **Ano.**

9.  V dalším kroku vyberte skupiny, které uživatelům, kteří požadují by se měl přístup k této aplikaci přidat, klikněte na tlačítko modulu pro výběr vedle popisek **skupinu, pro kterou má přiřazené byli přidáni uživatelé?** a vyberte skupinu.

10. **Volitelné:** nastaví, pokud chcete vyžadovat schválení obchodní před uživatelé mají povolen přístup **vyžadovat schválení před udělením přístupu k této aplikaci?** přepnutím **Ano**.

11. **Volitelné: pro aplikace pomocí hesla jednotné přihlašování na pouze** Pokud chcete povolit tyto firmy schvalovatelů k zadání hesla, které se odesílají na tuto žádost o schválení uživatelé, nastavte **povolit schvalovatelů k nastavení hesla uživatele pro tuto aplikaci?** přepnutím **Ano**.

12. **Volitelné:** pro zadání schvalovatelů firmy, kteří mají povoleno schválit přístup k této aplikaci, klikněte na výběr vedle popisek **kdo může schválit přístup k této aplikaci?** můžete vybrat až 10 jednotlivé obchodní schvalovatele.

   >[!NOTE]
   >Skupiny nejsou podporovány.
   >
   >

13. **Volitelné:** **pro aplikace, které zveřejňují role**, pokud chcete přiřadit roli schválené uživatelé samoobslužné služby, klikněte na modulu pro výběr vedle **do role, které by měl být přiřazena uživatelům v této aplikaci?** vyberte roli, ke kterému by se měla přiřadit těmto uživatelům.

14. Klikněte **Uložit** tlačítka v horní části okna dokončit.

Po dokončení konfigurace samoobslužné služby aplikace, uživatelé mohou přejít na jejich [Panel přístupu aplikace](https://myapps.microsoft.com/) a klikněte na tlačítko **+ přidat** tlačítko k vyhledání aplikace, na které jste povolili samoobslužné služby přístup. Obchodní schvalovatelů také zobrazit oznámení v jejich [Panel přístupu aplikace](https://myapps.microsoft.com/). Můžete povolit e-mail s upozorněním, když uživatel požaduje přístup k aplikaci, která vyžaduje schválení. 

Tato schválení podporovat pouze jeden schválení pracovních, což znamená, že pokud zadáte několik schvalovatelů, může jeden schvalovatel schvalovatel přístup k aplikaci.

## <a name="next-steps"></a>Další kroky
[Nastavení služby Azure Active Directory pro samoobslužnou správu skupin](active-directory-accessmanagement-self-service-group-management.md)
