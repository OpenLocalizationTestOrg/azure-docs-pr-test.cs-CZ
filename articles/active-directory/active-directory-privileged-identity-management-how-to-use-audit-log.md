---
title: "protokol auditování hello aaaHow toouse v Azure AD Privileged Identity managementu | Microsoft Docs"
description: "Zjistěte, jak protokolu auditů hello toouse v rozšíření Azure Privileged Identity Management hello."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 5d13a6dd-1fcb-4e76-82fb-cb2f4f0e4357
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 36987eaab9fe02c5dd7b4f4705e487299430745d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-audit-log-in-pim"></a>Použití protokolu auditu hello v PIM
Hello Privileged Identity Management (PIM) auditu protokolu toosee můžete použít všechny přiřazení uživatelských hello a aktivace v daném časovém období. Pokud chcete, aby historie úplné auditu hello toosee aktivity ve vašem klientovi, včetně správce, koncového uživatele a aktivity synchronizace, můžete použít hello [přístupu a použití sestav Azure Active Directory.](active-directory-view-access-usage-reports.md)

## <a name="navigate-toohello-audit-log"></a>Přejděte toohello protokolu auditu
Z hello [portál Azure](https://portal.azure.com) řídicí panel, vyberte hello **Azure AD Privileged Identity Management** aplikace. Odtud, přístup k protokolu auditu hello kliknutím **spravovat privilegované role** > **historie auditu** hello PIM řídicím panelu.

## <a name="hello-audit-log-graph"></a>Graf protokolu auditu Hello
Ve spojnicovém grafu můžete hello auditu protokolu tooview hello celkový počet aktivací, maximální počet aktivací za den a průměrný počet aktivací za den.  Můžete také filtrovat hello dat podle role, pokud je víc než jedné role v historie auditu hello.

Použití hello **čas**, **akce**, a **role** tlačítka toosort hello protokolu.

## <a name="hello-audit-log-list"></a>Seznam protokolu auditu Hello
Hello sloupců v seznamu protokolů auditu hello jsou:

* **Žadatel** -hello uživatele, který požadovaný aktivaci role hello nebo změnit.  Pokud je hodnota hello "Azure systém", zkontrolujte protokol auditu Azure hello Další informace.
* **Uživatel** -hello uživatele, který je aktivace nebo přiřazenou roli tooa.
* **Role** -hello role přiřazení nebo aktivaci uživatelem hello.
* **Akce** – hello akcí hello žadatele. To může zahrnovat přiřazení, zrušení, aktivace nebo deaktivace.
* **Čas** – Pokud hello akce došlo k chybě.
* **Důvody** – Pokud jakýkoli text byl zadali do pole Důvod hello během aktivace, se zobrazí tady.
* **Vypršení platnosti** – pouze relevantní pro aktivaci role.

## <a name="filter-hello-audit-log"></a>Protokol auditování hello filtru
Můžete filtrovat hello informace, které se zobrazí v protokolu auditování hello kliknutím hello **filtru** tlačítko.  Hello **aktualizace grafu parametry okno** se zobrazí.

Po nastavení hello filtry, klikněte na tlačítko **aktualizace** toofilter hello data v protokolu hello.  Pokud hello data nezobrazí okamžitě, aktualizujte stránku hello.

### <a name="change-hello-date-range"></a>Změnit rozsah hello
Použití hello **Dnes**, **minulého týdne**, **poslední měsíc**, nebo **vlastní** tlačítka toochange hello časové rozmezí hello protokolu auditu.

Pokud vyberete hello **vlastní** tlačítko, budete mít **z** pole pro datum a **k** datum pole toospecify rozsah dat pro protokol hello.  Můžete buď zadat hello data ve formátu MM/DD/RRRR nebo klikněte na hello **kalendáře** ikonu a vyberte hello datum z kalendáře.

### <a name="change-hello-roles-included-in-hello-log"></a>Změna role hello součástí hello protokolu
Zaškrtněte nebo zrušte zaškrtnutí políčka hello **Role** políčko další tooeach role tooinclude nebo vyloučit z hello protokolu.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

