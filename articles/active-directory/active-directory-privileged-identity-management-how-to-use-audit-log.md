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
# <a name="using-hello-audit-log-in-pim"></a><span data-ttu-id="daf7f-103">Použití protokolu auditu hello v PIM</span><span class="sxs-lookup"><span data-stu-id="daf7f-103">Using hello audit log in PIM</span></span>
<span data-ttu-id="daf7f-104">Hello Privileged Identity Management (PIM) auditu protokolu toosee můžete použít všechny přiřazení uživatelských hello a aktivace v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="daf7f-104">You can use hello Privileged Identity Management (PIM) audit log toosee all hello user assignments and activations within a given time period.</span></span> <span data-ttu-id="daf7f-105">Pokud chcete, aby historie úplné auditu hello toosee aktivity ve vašem klientovi, včetně správce, koncového uživatele a aktivity synchronizace, můžete použít hello [přístupu a použití sestav Azure Active Directory.](active-directory-view-access-usage-reports.md)</span><span class="sxs-lookup"><span data-stu-id="daf7f-105">If you want toosee hello full audit history of activity in your tenant, including administrator, end user, and synchronization activity, you can use hello [Azure Active Directory access and usage reports.](active-directory-view-access-usage-reports.md)</span></span>

## <a name="navigate-toohello-audit-log"></a><span data-ttu-id="daf7f-106">Přejděte toohello protokolu auditu</span><span class="sxs-lookup"><span data-stu-id="daf7f-106">Navigate toohello audit log</span></span>
<span data-ttu-id="daf7f-107">Z hello [portál Azure](https://portal.azure.com) řídicí panel, vyberte hello **Azure AD Privileged Identity Management** aplikace.</span><span class="sxs-lookup"><span data-stu-id="daf7f-107">From hello [Azure portal](https://portal.azure.com) dashboard, select hello **Azure AD Privileged Identity Management** app.</span></span> <span data-ttu-id="daf7f-108">Odtud, přístup k protokolu auditu hello kliknutím **spravovat privilegované role** > **historie auditu** hello PIM řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="daf7f-108">From there, access hello audit log by clicking **Manage privileged roles** > **Audit history** in hello PIM dashboard.</span></span>

## <a name="hello-audit-log-graph"></a><span data-ttu-id="daf7f-109">Graf protokolu auditu Hello</span><span class="sxs-lookup"><span data-stu-id="daf7f-109">hello audit log graph</span></span>
<span data-ttu-id="daf7f-110">Ve spojnicovém grafu můžete hello auditu protokolu tooview hello celkový počet aktivací, maximální počet aktivací za den a průměrný počet aktivací za den.</span><span class="sxs-lookup"><span data-stu-id="daf7f-110">You can use hello audit log tooview hello total activations, max activations per day, and average activations per day in a line graph.</span></span>  <span data-ttu-id="daf7f-111">Můžete také filtrovat hello dat podle role, pokud je víc než jedné role v historie auditu hello.</span><span class="sxs-lookup"><span data-stu-id="daf7f-111">You can also filter hello data by role if there is more than one role in hello audit history.</span></span>

<span data-ttu-id="daf7f-112">Použití hello **čas**, **akce**, a **role** tlačítka toosort hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="daf7f-112">Use hello **time**, **action**, and **role** buttons toosort hello log.</span></span>

## <a name="hello-audit-log-list"></a><span data-ttu-id="daf7f-113">Seznam protokolu auditu Hello</span><span class="sxs-lookup"><span data-stu-id="daf7f-113">hello audit log list</span></span>
<span data-ttu-id="daf7f-114">Hello sloupců v seznamu protokolů auditu hello jsou:</span><span class="sxs-lookup"><span data-stu-id="daf7f-114">hello columns in hello audit log list are:</span></span>

* <span data-ttu-id="daf7f-115">**Žadatel** -hello uživatele, který požadovaný aktivaci role hello nebo změnit.</span><span class="sxs-lookup"><span data-stu-id="daf7f-115">**Requestor** - hello user who requested hello role activation or change.</span></span>  <span data-ttu-id="daf7f-116">Pokud je hodnota hello "Azure systém", zkontrolujte protokol auditu Azure hello Další informace.</span><span class="sxs-lookup"><span data-stu-id="daf7f-116">If hello value is "Azure System", check hello Azure audit log for more information.</span></span>
* <span data-ttu-id="daf7f-117">**Uživatel** -hello uživatele, který je aktivace nebo přiřazenou roli tooa.</span><span class="sxs-lookup"><span data-stu-id="daf7f-117">**User** - hello user who is activating or assigned tooa role.</span></span>
* <span data-ttu-id="daf7f-118">**Role** -hello role přiřazení nebo aktivaci uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="daf7f-118">**Role** - hello role assigned or activated by hello user.</span></span>
* <span data-ttu-id="daf7f-119">**Akce** – hello akcí hello žadatele.</span><span class="sxs-lookup"><span data-stu-id="daf7f-119">**Action** - hello actions taken by hello requestor.</span></span> <span data-ttu-id="daf7f-120">To může zahrnovat přiřazení, zrušení, aktivace nebo deaktivace.</span><span class="sxs-lookup"><span data-stu-id="daf7f-120">This can include assignment, unassignment, activation, or deactivation.</span></span>
* <span data-ttu-id="daf7f-121">**Čas** – Pokud hello akce došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="daf7f-121">**Time** - when hello action occurred.</span></span>
* <span data-ttu-id="daf7f-122">**Důvody** – Pokud jakýkoli text byl zadali do pole Důvod hello během aktivace, se zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="daf7f-122">**Reasoning** - if any text was entered into hello reason field during activation, it will show up here.</span></span>
* <span data-ttu-id="daf7f-123">**Vypršení platnosti** – pouze relevantní pro aktivaci role.</span><span class="sxs-lookup"><span data-stu-id="daf7f-123">**Expiration** - only relevant for activation of roles.</span></span>

## <a name="filter-hello-audit-log"></a><span data-ttu-id="daf7f-124">Protokol auditování hello filtru</span><span class="sxs-lookup"><span data-stu-id="daf7f-124">Filter hello audit log</span></span>
<span data-ttu-id="daf7f-125">Můžete filtrovat hello informace, které se zobrazí v protokolu auditování hello kliknutím hello **filtru** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="daf7f-125">You can filter hello information that shows up in hello audit log by clicking hello **Filter** button.</span></span>  <span data-ttu-id="daf7f-126">Hello **aktualizace grafu parametry okno** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="daf7f-126">hello **Update chart parameters blade** will appear.</span></span>

<span data-ttu-id="daf7f-127">Po nastavení hello filtry, klikněte na tlačítko **aktualizace** toofilter hello data v protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="daf7f-127">After you set hello filters, click **Update** toofilter hello data in hello log.</span></span>  <span data-ttu-id="daf7f-128">Pokud hello data nezobrazí okamžitě, aktualizujte stránku hello.</span><span class="sxs-lookup"><span data-stu-id="daf7f-128">If hello data doesn't appear right away, refresh hello page.</span></span>

### <a name="change-hello-date-range"></a><span data-ttu-id="daf7f-129">Změnit rozsah hello</span><span class="sxs-lookup"><span data-stu-id="daf7f-129">Change hello date range</span></span>
<span data-ttu-id="daf7f-130">Použití hello **Dnes**, **minulého týdne**, **poslední měsíc**, nebo **vlastní** tlačítka toochange hello časové rozmezí hello protokolu auditu.</span><span class="sxs-lookup"><span data-stu-id="daf7f-130">Use hello **Today**, **Past Week**, **Past Month**, or **Custom** buttons toochange hello time range of hello audit log.</span></span>

<span data-ttu-id="daf7f-131">Pokud vyberete hello **vlastní** tlačítko, budete mít **z** pole pro datum a **k** datum pole toospecify rozsah dat pro protokol hello.</span><span class="sxs-lookup"><span data-stu-id="daf7f-131">When you choose hello **Custom** button, you will be given a **From** date field and a **To** date field toospecify a range of dates for hello log.</span></span>  <span data-ttu-id="daf7f-132">Můžete buď zadat hello data ve formátu MM/DD/RRRR nebo klikněte na hello **kalendáře** ikonu a vyberte hello datum z kalendáře.</span><span class="sxs-lookup"><span data-stu-id="daf7f-132">You can either enter hello dates in MM/DD/YYYY format or click on hello **calendar** icon and choose hello date from a calendar.</span></span>

### <a name="change-hello-roles-included-in-hello-log"></a><span data-ttu-id="daf7f-133">Změna role hello součástí hello protokolu</span><span class="sxs-lookup"><span data-stu-id="daf7f-133">Change hello roles included in hello log</span></span>
<span data-ttu-id="daf7f-134">Zaškrtněte nebo zrušte zaškrtnutí políčka hello **Role** políčko další tooeach role tooinclude nebo vyloučit z hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="daf7f-134">Check or uncheck hello **Role** checkbox next tooeach role tooinclude or exclude it from hello log.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="daf7f-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="daf7f-135">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

