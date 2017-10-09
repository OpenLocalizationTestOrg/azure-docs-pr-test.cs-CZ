---
title: "pracovní postupy Privileged Identity Management schválení aaaAzure | Microsoft Docs"
description: "Další informace o schválení pracovních postupů v Privileged Identity Management (PIM)"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 4afaf5c138798a803eb3d3b7905b9361d65792cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="approvals-preview"></a>Schválení (Preview)

## <a name="overview"></a>Přehled

Schválení Privileged Identity Management můžete nakonfigurovat role toorequire schválení pro aktivaci a vyberte jeden nebo více uživatelů nebo skupin jako delegovaný schvalovatelů. Zachovat čtení toolearn jak tooconfigure role a vyberte schvalovatelů.

>[!NOTE]
Mějte prosím na paměti, že tato funkce je stále ve vývoji a pravděpodobně narazíte na chyby. Hello funkce, včetně textu a konvence vytváření názvů se mohou změnit a by se neměla považovat za poslední.


## <a name="key-terminology"></a>Terminologie klíče

*Oprávněný uživatel Role* – oprávněné role uživatel je uživatelem v rámci vaší organizace, která byla přiřazena role tooan Azure AD jako způsobilých (role vyžaduje aktivaci).

*Delegovaná schvalovatel* – delegované schvalovatel je jeden nebo více jednotlivce nebo skupiny v rámci služby Azure AD, kteří jsou zodpovědní za schválení žádosti o aktivaci role.

## <a name="scenarios"></a>Scénáře

privátní Preview verzi Hello podporuje hello následující scénáře:

**Jako privilegované Role správce (PRA) můžete:**

-   [Povolit schválení pro konkrétní role](#enable-approval-for-specific-roles)

-   [Zadejte schvalovatel uživatelů nebo skupin tooapprove požadavků](#specify-approver-users-and/or-groups-to-approve-requests)

-   [Zobrazit historii požadavku a schválení všech privilegovaných rolí](#view-request-and-approval-history-for-all-privileged-roles)

**Jako určeného schvalovatele můžete:**

-   [Zobrazit čeká na schválení (počet požadavků)](#view-pending-approvals-requests)

-   [Schválit nebo odmítnout žádosti o zvýšení oprávnění role (jeden nebo hromadně)](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [uveďte její odůvodnění moje schválení nebo zamítnutí](#provide-justification-for-my-approval/rejection) 

**Jako oprávněný uživatel roli můžete:**

-   [žádost o aktivaci role, který vyžaduje schválení](#request-activation-of-a-role-that-requires-approval)

-   [Zobrazit stav hello tooactivate vaší žádosti](#view-the-status-of-your-request-to-activate)

-   [dokončení úkolu ve službě Azure AD, pokud byla schválena aktivace](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Navigace

Aktualizovali jsme hello navigační toosupport schválení

![](media/azure-ad-pim-approval-workflow/image001.png)

Hello výchozí cílová stránka poskytuje pohodlné přístup tooinformation o PIM a hello novou dokumentaci schválení.

![](media/azure-ad-pim-approval-workflow/image002.png)

Přidali jsme také novou část pro všechny uživatele PIM, historie Moje auditu. Zde můžete najít všechny hello informace relevantní tooyour identity. To zahrnuje všechny vaše požadavky na vyřízení a dokončený, všechny rozhodnutí, které jste udělali o hello požadavků, které vyřešíte a všechny vaše posledních aktivace role z jednoho místa.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>Povolit schválení pro konkrétní role

tooenable schválení pro určité role, nejprve vyberte adresář role z levého navigačního hello.

![](media/azure-ad-pim-approval-workflow/image004.png)

Najděte a vyberte nastavení v levém navigačním role Directory hello

![](media/azure-ad-pim-approval-workflow/image006.png)

Vyberte privilegované role:

![](media/azure-ad-pim-approval-workflow/image009.png)

Vyberte možnost "Povolit" v hello vyžadují části:

![](media/azure-ad-pim-approval-workflow/image011.png)

Po povolení bude rozbalte okno hello tooshow hello následující podrobnosti:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
Pokud je nesmí zadat všechny schvalovatelů, stanou se hello PRA(s) schvalujících výchozí hello. PRA(s) by požadované tooapprove aktivace všechny požadavky pro tuto roli.

### <a name="specify-approver-users-andor-groups-tooapprove-requests"></a>Zadejte schvalovatel uživatelů nebo skupin tooapprove požadavků

schválení toodelegate, klikněte na možnost hello příliš "Vyberte schvalovatelů":

![](media/azure-ad-pim-approval-workflow/image015.png)

Až okno Vyberte schvalovatelů hello načte, pravděpodobně vyhledávání pro konkrétního uživatele nebo skupiny pomocí hello panelu Hledat v horní části hello nebo výběrem ze seznamu předem vyplněná hello a pak klikněte na tlačítko "Vyberte" po dokončení:

![](media/azure-ad-pim-approval-workflow/image017.png)

Poznámka: Může vybrat více uživatelů nebo skupin současně.

Výběr se zobrazí v seznamu hello vybrané schvalovatelů, jak vidíte níže:

![](media/azure-ad-pim-approval-workflow/image019.png)

tooremove schvalovatele, jednoduše klikněte hello odebrat tlačítko Další tootheir název.

tooadd další schvalovatele, proces opakování hello.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>Zobrazit historii požadavku a schválení všech privilegovaných rolí

Historie tooview požadavku a schválení všech privilegovaných rolí, vyberte historie auditu z řídicího panelu hello:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Můžete řadit data hello akcí a vyhledejte "Aktivace schválené"

### <a name="view-pending-approvals-requests"></a>Zobrazit čeká na schválení (počet požadavků)

Jako delegovaný schvalovatel obdržíte e-mailová oznámení při žádost čeká na schválení. tooview tyto požadavky hello PIM portálu na kartě řídicí panel (v nové navigační hello) vyberte hello "čekající žádosti o schválení" hello levé navigační panel.

![](media/azure-ad-pim-approval-workflow/image023.png)

Z tohoto místa se zobrazí seznam žádosti čekající na schválení:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>Schválit nebo odmítnout žádosti o zvýšení oprávnění role (jeden nebo hromadně)

Vyberte hello požadavky chcete tooapprove nebo odepřít a klikněte na tlačítko hello na panelu akcí, která odpovídá vaše rozhodnutí:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>Uveďte její odůvodnění moje schválení nebo zamítnutí

To bude otevřete nové okno tooapprove nebo odepřít více požadavků najednou. Zadejte odůvodnění pro vaše rozhodnutí, a klikněte na tlačítko Schválit (nebo odepřít) v dolní hello nebo hello okno:

![](media/azure-ad-pim-approval-workflow/image029.png)

Po dokončení zpracování žádosti o hello symbolu stavu hello bude odrážet rozhodnutí, které jste nastavili (v tomto příkladu rozhodnutí hello je schválit):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>Žádost o aktivaci role, který vyžaduje schválení

Žádají o aktivaci role, která vyžaduje schválení lze zahájit z původní navigační PIM hello nebo nové navigační hello, jako hello proces aktivace zůstane role hello stejné. Jednoduše vyberte hello seznamu rolí k aktivaci role:

![](media/azure-ad-pim-approval-workflow/image033.png)

Pokud privilegovaných rolí vyžaduje Vícefaktorové ověřování, budete vyzváni k dokončení této úlohy nejdřív:

![](media/azure-ad-pim-approval-workflow/image035.png)

Po dokončení, kliknout na aktivovat a zadejte odůvodnění (v případě potřeby):

![](media/azure-ad-pim-approval-workflow/image037.png)

žadatel Hello uvidí, že je oznámení, že hello žádosti čekající na schválení:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-hello-status-of-your-request-tooactivate"></a>Zobrazení stavu hello tooactivate vaší žádosti

Zobrazení stavu hello tooactivate nevyřízenou žádost musí mít přístup nové navigace. V levém navigačním panelu hello vyberte kartu "Moje žádosti o" hello:

![](media/azure-ad-pim-approval-workflow/image041.png)

stav žádosti Hello výchozí příliš "Čeká na vyřízení", ale můžete přepínat toosee všechny nebo odepření požadavků.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>Dokončení úkolu ve službě Azure AD, pokud byla schválena aktivace

Po schválení žádosti hello hello role je aktivní a veškerou práci, kterou vyžaduje tato role může pokračovat.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Další kroky

Váš názor se hodí v situaci toous. Můžete volné tooshare komentáře nebo zpětnou vazbu s námi zde!
