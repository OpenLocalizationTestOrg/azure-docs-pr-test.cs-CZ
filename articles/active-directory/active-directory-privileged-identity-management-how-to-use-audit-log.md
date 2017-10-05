---
title: "Jak používat protokol auditu v Azure AD Privileged Identity managementu | Microsoft Docs"
description: "Naučte se používat protokol auditu v rozšíření Azure Privileged Identity Management."
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
ms.openlocfilehash: 7d9a5255a64d46c1388d328a606b3f297d61262b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-audit-log-in-pim"></a><span data-ttu-id="dd794-103">Použití protokolu auditování v PIM</span><span class="sxs-lookup"><span data-stu-id="dd794-103">Using the audit log in PIM</span></span>
<span data-ttu-id="dd794-104">Protokol auditu Privileged Identity Management (PIM) můžete zobrazit všechny uživatele přiřazení a aktivace v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="dd794-104">You can use the Privileged Identity Management (PIM) audit log to see all the user assignments and activations within a given time period.</span></span> <span data-ttu-id="dd794-105">Pokud chcete zobrazit historii úplné auditu aktivity ve vašem klientovi, včetně správce, koncového uživatele a aktivity synchronizace můžete použít [přístupu a použití sestav Azure Active Directory.](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="dd794-105">If you want to see the full audit history of activity in your tenant, including administrator, end user, and synchronization activity, you can use the [Azure Active Directory access and usage reports.](active-directory-view-access-usage-reports.md)</span></span>

## <a name="navigate-to-the-audit-log"></a><span data-ttu-id="dd794-106">Přejděte na protokol auditu</span><span class="sxs-lookup"><span data-stu-id="dd794-106">Navigate to the audit log</span></span>
<span data-ttu-id="dd794-107">Z [portál Azure](https://portal.azure.com) řídicí panel, vyberte **Azure AD Privileged Identity Management** aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd794-107">From the [Azure portal](https://portal.azure.com) dashboard, select the **Azure AD Privileged Identity Management** app.</span></span> <span data-ttu-id="dd794-108">Odtud, přístup k protokolu auditu kliknutím **spravovat privilegované role** > **historie auditu** v řídicím panelu PIM.</span><span class="sxs-lookup"><span data-stu-id="dd794-108">From there, access the audit log by clicking **Manage privileged roles** > **Audit history** in the PIM dashboard.</span></span>

## <a name="the-audit-log-graph"></a><span data-ttu-id="dd794-109">Graf protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="dd794-109">The audit log graph</span></span>
<span data-ttu-id="dd794-110">Chcete-li zobrazit celkový počet aktivací, maximální počet aktivací za den a průměrný počet aktivací za den ve spojnicovém grafu, můžete použít protokol auditu.</span><span class="sxs-lookup"><span data-stu-id="dd794-110">You can use the audit log to view the total activations, max activations per day, and average activations per day in a line graph.</span></span>  <span data-ttu-id="dd794-111">Data lze filtrovat také role, pokud je víc než jedné role v historie auditu.</span><span class="sxs-lookup"><span data-stu-id="dd794-111">You can also filter the data by role if there is more than one role in the audit history.</span></span>

<span data-ttu-id="dd794-112">Použití **čas**, **akce**, a **role** tlačítka seřadíte v protokolu.</span><span class="sxs-lookup"><span data-stu-id="dd794-112">Use the **time**, **action**, and **role** buttons to sort the log.</span></span>

## <a name="the-audit-log-list"></a><span data-ttu-id="dd794-113">Seznam protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="dd794-113">The audit log list</span></span>
<span data-ttu-id="dd794-114">Sloupce v seznamu protokolů auditu jsou:</span><span class="sxs-lookup"><span data-stu-id="dd794-114">The columns in the audit log list are:</span></span>

* <span data-ttu-id="dd794-115">**Žadatel** -uživatel, který požadovaný aktivaci role nebo změnit.</span><span class="sxs-lookup"><span data-stu-id="dd794-115">**Requestor** - the user who requested the role activation or change.</span></span>  <span data-ttu-id="dd794-116">Pokud je hodnota "Azure systém", zkontrolujte protokol auditu Azure pro další informace.</span><span class="sxs-lookup"><span data-stu-id="dd794-116">If the value is "Azure System", check the Azure audit log for more information.</span></span>
* <span data-ttu-id="dd794-117">**Uživatel** -uživatel, který je přiřazený k roli nebo aktivace.</span><span class="sxs-lookup"><span data-stu-id="dd794-117">**User** - the user who is activating or assigned to a role.</span></span>
* <span data-ttu-id="dd794-118">**Role** – přiřazené roli, nebo uživatel aktivoval.</span><span class="sxs-lookup"><span data-stu-id="dd794-118">**Role** - the role assigned or activated by the user.</span></span>
* <span data-ttu-id="dd794-119">**Akce** – akce prováděné žadatel.</span><span class="sxs-lookup"><span data-stu-id="dd794-119">**Action** - the actions taken by the requestor.</span></span> <span data-ttu-id="dd794-120">To může zahrnovat přiřazení, zrušení, aktivace nebo deaktivace.</span><span class="sxs-lookup"><span data-stu-id="dd794-120">This can include assignment, unassignment, activation, or deactivation.</span></span>
* <span data-ttu-id="dd794-121">**Čas** – Pokud akce došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="dd794-121">**Time** - when the action occurred.</span></span>
* <span data-ttu-id="dd794-122">**Důvody** – Pokud jakýkoli text byl zadali do pole Důvod během aktivace, se zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="dd794-122">**Reasoning** - if any text was entered into the reason field during activation, it will show up here.</span></span>
* <span data-ttu-id="dd794-123">**Vypršení platnosti** – pouze relevantní pro aktivaci role.</span><span class="sxs-lookup"><span data-stu-id="dd794-123">**Expiration** - only relevant for activation of roles.</span></span>

## <a name="filter-the-audit-log"></a><span data-ttu-id="dd794-124">Filtrovat protokol auditu</span><span class="sxs-lookup"><span data-stu-id="dd794-124">Filter the audit log</span></span>
<span data-ttu-id="dd794-125">Můžete filtrovat informace, které se zobrazí v protokolu auditování kliknutím **filtru** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd794-125">You can filter the information that shows up in the audit log by clicking the **Filter** button.</span></span>  <span data-ttu-id="dd794-126">**Aktualizace grafu parametry okno** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="dd794-126">The **Update chart parameters blade** will appear.</span></span>

<span data-ttu-id="dd794-127">Jakmile jste nastavili filtry, klikněte na tlačítko **aktualizace** k filtrování dat v protokolu.</span><span class="sxs-lookup"><span data-stu-id="dd794-127">After you set the filters, click **Update** to filter the data in the log.</span></span>  <span data-ttu-id="dd794-128">Pokud data nezobrazí okamžitě, aktualizujte stránku.</span><span class="sxs-lookup"><span data-stu-id="dd794-128">If the data doesn't appear right away, refresh the page.</span></span>

### <a name="change-the-date-range"></a><span data-ttu-id="dd794-129">Změnit rozsah dat</span><span class="sxs-lookup"><span data-stu-id="dd794-129">Change the date range</span></span>
<span data-ttu-id="dd794-130">Použití **Dnes**, **minulého týdne**, **poslední měsíc**, nebo **vlastní** Chcete-li změnit časové rozmezí protokol auditu.</span><span class="sxs-lookup"><span data-stu-id="dd794-130">Use the **Today**, **Past Week**, **Past Month**, or **Custom** buttons to change the time range of the audit log.</span></span>

<span data-ttu-id="dd794-131">Pokud vyberete **vlastní** tlačítko, budete mít **z** pole pro datum a **k** pole pro datum k určení rozsahu dat pro protokol.</span><span class="sxs-lookup"><span data-stu-id="dd794-131">When you choose the **Custom** button, you will be given a **From** date field and a **To** date field to specify a range of dates for the log.</span></span>  <span data-ttu-id="dd794-132">Můžete zadat data ve formátu MM/DD/RRRR nebo klikněte na **kalendáře** ikonu a vyberte datum z kalendáře.</span><span class="sxs-lookup"><span data-stu-id="dd794-132">You can either enter the dates in MM/DD/YYYY format or click on the **calendar** icon and choose the date from a calendar.</span></span>

### <a name="change-the-roles-included-in-the-log"></a><span data-ttu-id="dd794-133">Změna role zahrnuté v protokolu</span><span class="sxs-lookup"><span data-stu-id="dd794-133">Change the roles included in the log</span></span>
<span data-ttu-id="dd794-134">Zaškrtněte nebo zrušte zaškrtnutí políčka **Role** zaškrtávací políčko vedle každé role zahrnout nebo vyloučit z protokolu.</span><span class="sxs-lookup"><span data-stu-id="dd794-134">Check or uncheck the **Role** checkbox next to each role to include or exclude it from the log.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="dd794-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd794-135">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

