---
title: "Začínáme se Schedulerem na webu Azure Portal | Dokumentace Microsoftu"
description: "Začínáme se Schedulerem na portálu Azure"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: e69542ec-d10f-4f17-9b7a-2ee441ee7d68
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: deli
ms.openlocfilehash: 3861ee121ed1c4d086ea81640e84d924d7d17ea1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a><span data-ttu-id="f3851-103">Začínáme se Schedulerem na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f3851-103">Get started with Azure Scheduler in Azure portal</span></span>
<span data-ttu-id="f3851-104">Ve službě Azure Scheduler je vytváření plánovaných úloh snadné.</span><span class="sxs-lookup"><span data-stu-id="f3851-104">It's easy to create scheduled jobs in Azure Scheduler.</span></span> <span data-ttu-id="f3851-105">V tomto kurzu se naučíte, jak vytvořit úlohu.</span><span class="sxs-lookup"><span data-stu-id="f3851-105">In this tutorial, you'll learn how to create a job.</span></span> <span data-ttu-id="f3851-106">Taky poznáte možnosti Scheduleru pro sledování a správu.</span><span class="sxs-lookup"><span data-stu-id="f3851-106">You'll also learn Scheduler's monitoring and management capabilities.</span></span>

## <a name="create-a-job"></a><span data-ttu-id="f3851-107">Vytvoření úlohy</span><span class="sxs-lookup"><span data-stu-id="f3851-107">Create a job</span></span>
1. <span data-ttu-id="f3851-108">Přihlaste se k [portálu Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f3851-108">Sign in to [Azure portal](https://portal.azure.com/).</span></span>  
2. <span data-ttu-id="f3851-109">Klikněte na **+Nový** > v poli pro vyhledávání zadejte *Scheduler* > ve výsledcích vyberte **Scheduler** > klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f3851-109">Click **+New** > type *Scheduler* in the search box >  select **Scheduler** in results > click **Create**.</span></span>
   
    ![][marketplace-create]
3. <span data-ttu-id="f3851-110">Teď vytvoříme úlohu, která jednoduše vyšle požadavek GET na http://www.microsoft.com/.</span><span class="sxs-lookup"><span data-stu-id="f3851-110">Let’s create a job that simply hits http://www.microsoft.com/ with a GET request.</span></span> <span data-ttu-id="f3851-111">Na obrazovce **Úloha Scheduleru** zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="f3851-111">In the **Scheduler Job** screen, enter the following information:</span></span>
   
   1. <span data-ttu-id="f3851-112">**Název:** `getmicrosoft`</span><span class="sxs-lookup"><span data-stu-id="f3851-112">**Name:** `getmicrosoft`</span></span>  
   2. <span data-ttu-id="f3851-113">**Předplatné:** Vaše předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="f3851-113">**Subscription:** Your Azure subscription</span></span>   
   3. <span data-ttu-id="f3851-114">**Kolekce úloh:** Vyberte existující kolekci úloh nebo klikněte na **Vytvořit novou** > zadejte název.</span><span class="sxs-lookup"><span data-stu-id="f3851-114">**Job Collection:** Select an existing job collection, or click **Create New** > enter a name.</span></span>
4. <span data-ttu-id="f3851-115">Dál klikněte na **Nastavení akce** a definujte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f3851-115">Next, in **Action Settings**, define the following values:</span></span>
   
   1. <span data-ttu-id="f3851-116">**Typ akce:** ` HTTP`</span><span class="sxs-lookup"><span data-stu-id="f3851-116">**Action Type:** ` HTTP`</span></span>  
   2. <span data-ttu-id="f3851-117">**Metoda:** `GET`</span><span class="sxs-lookup"><span data-stu-id="f3851-117">**Method:** `GET`</span></span>  
   3. <span data-ttu-id="f3851-118">**Adresa URL:** ` http://www.microsoft.com`</span><span class="sxs-lookup"><span data-stu-id="f3851-118">**URL:** ` http://www.microsoft.com`</span></span>  
      
      ![][action-settings]
5. <span data-ttu-id="f3851-119">Nakonec nastavíme plán.</span><span class="sxs-lookup"><span data-stu-id="f3851-119">Finally, let's define a schedule.</span></span> <span data-ttu-id="f3851-120">Úloha se může definovat jako jednorázová, ale my vytvoříme plán opakování:</span><span class="sxs-lookup"><span data-stu-id="f3851-120">The job could be defined as a one-time job, but let’s pick a recurrence schedule:</span></span>
   
   1. <span data-ttu-id="f3851-121">**Opakování**: `Recurring`</span><span class="sxs-lookup"><span data-stu-id="f3851-121">**Recurrence**: `Recurring`</span></span>
   2. <span data-ttu-id="f3851-122">**Začátek**: Dnešní datum</span><span class="sxs-lookup"><span data-stu-id="f3851-122">**Start**: Today's date</span></span>
   3. <span data-ttu-id="f3851-123">**Opakovat každý**: `12 Hours`</span><span class="sxs-lookup"><span data-stu-id="f3851-123">**Recur every**: `12 Hours`</span></span>
   4. <span data-ttu-id="f3851-124">**Konec**: Dva dny po dnešním datu</span><span class="sxs-lookup"><span data-stu-id="f3851-124">**End by**: Two days from today's date</span></span>  
      
      ![][recurrence-schedule]
6. <span data-ttu-id="f3851-125">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="f3851-125">Click **Create**</span></span>

## <a name="manage-and-monitor-jobs"></a><span data-ttu-id="f3851-126">Správa a sledování úloh</span><span class="sxs-lookup"><span data-stu-id="f3851-126">Manage and monitor jobs</span></span>
<span data-ttu-id="f3851-127">Úloha se po vytvoření objeví v hlavním řídicím panelu Azure.</span><span class="sxs-lookup"><span data-stu-id="f3851-127">Once a job is created, it appears in the main Azure dashboard.</span></span> <span data-ttu-id="f3851-128">Klikněte na úlohu a otevře se nové okno s těmito kartami:</span><span class="sxs-lookup"><span data-stu-id="f3851-128">Click the job and a new window opens with the following tabs:</span></span>

1. <span data-ttu-id="f3851-129">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f3851-129">Properties</span></span>  
2. <span data-ttu-id="f3851-130">Nastavení akce</span><span class="sxs-lookup"><span data-stu-id="f3851-130">Action Settings</span></span>  
3. <span data-ttu-id="f3851-131">Plán</span><span class="sxs-lookup"><span data-stu-id="f3851-131">Schedule</span></span>  
4. <span data-ttu-id="f3851-132">Historie</span><span class="sxs-lookup"><span data-stu-id="f3851-132">History</span></span>
5. <span data-ttu-id="f3851-133">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="f3851-133">Users</span></span>
   
   ![][job-overview]

### <a name="properties"></a><span data-ttu-id="f3851-134">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f3851-134">Properties</span></span>
<span data-ttu-id="f3851-135">Tyto vlastnosti jsou jen pro čtení a popisují metadata správy pro úlohu Scheduleru.</span><span class="sxs-lookup"><span data-stu-id="f3851-135">These read-only properties describe the management metadata for the Scheduler job.</span></span>

   ![][job-properties]

### <a name="action-settings"></a><span data-ttu-id="f3851-136">Nastavení akce</span><span class="sxs-lookup"><span data-stu-id="f3851-136">Action settings</span></span>
<span data-ttu-id="f3851-137">Pokud chcete nakonfigurovat úlohu, klikněte na ni na obrazovce **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="f3851-137">Clicking on a job in the **Jobs** screen allows you to configure that job.</span></span> <span data-ttu-id="f3851-138">Pokud jste pokročilá nastavení nenakonfigurovali v průvodci rychlým vytvořením podle svých představ, můžete je konfigurovat tady.</span><span class="sxs-lookup"><span data-stu-id="f3851-138">This lets you configure advanced settings, if you didn't configure them in the quick-create wizard.</span></span>

<span data-ttu-id="f3851-139">Pro všechny typy akcí můžete změnit zásady opakovaných pokusů a akci při chybě.</span><span class="sxs-lookup"><span data-stu-id="f3851-139">For all action types, you may change the retry policy and the error action.</span></span>

<span data-ttu-id="f3851-140">Pro akce úloh HTTP a HTTPS můžete změnit metodu na libovolnou povolenou operaci HTTP.</span><span class="sxs-lookup"><span data-stu-id="f3851-140">For HTTP and HTTPS job action types, you may change the method to any allowed HTTP verb.</span></span> <span data-ttu-id="f3851-141">Taky můžete přidat, odstranit nebo změnit hlavičky a základní ověřovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f3851-141">You may also add, delete, or change the headers and basic authentication information.</span></span>

<span data-ttu-id="f3851-142">Pro akce fronty úložiště můžete změnit účet úložiště, název fronty, token SAS a hlavní část.</span><span class="sxs-lookup"><span data-stu-id="f3851-142">For storage queue action types, you may change the storage account, queue name, SAS token, and body.</span></span>

<span data-ttu-id="f3851-143">Pro akce sběrnice můžete změnit obory názvů, cestu k tématu/frontě, nastavení ověření, typ přenosu, vlastnosti zprávy a hlavní část zprávy.</span><span class="sxs-lookup"><span data-stu-id="f3851-143">For service bus action types, you may change the namespace, topic/queue path, authentication settings, transport type, message properties, and message body.</span></span>

   ![][job-action-settings]

### <a name="schedule"></a><span data-ttu-id="f3851-144">Plán</span><span class="sxs-lookup"><span data-stu-id="f3851-144">Schedule</span></span>
<span data-ttu-id="f3851-145">Pokud vám plán vytvořený v průvodci rychlým vytvořením nevyhovuje, můžete ho tady změnit.</span><span class="sxs-lookup"><span data-stu-id="f3851-145">This lets you reconfigure the schedule, if you'd like to change the schedule you created in the quick-create wizard.</span></span>

<span data-ttu-id="f3851-146">To je příležitost k sestavení [komplexních plánů a pokročilého opakování ve vaší úloze](scheduler-advanced-complexity.md).</span><span class="sxs-lookup"><span data-stu-id="f3851-146">This is an opportunity to build [complex schedules and advanced recurrence in your job](scheduler-advanced-complexity.md)</span></span>

<span data-ttu-id="f3851-147">Můžete změnit datum a čas zahájení, plán opakování a datum a čas ukončení (pokud se úloha opakuje).</span><span class="sxs-lookup"><span data-stu-id="f3851-147">You may change the start date and time, recurrence schedule, and the end date and time (if the job is recurring.)</span></span>

   ![][job-schedule]

### <a name="history"></a><span data-ttu-id="f3851-148">Historie</span><span class="sxs-lookup"><span data-stu-id="f3851-148">History</span></span>
<span data-ttu-id="f3851-149">Na kartě **Historie** jsou zobrazené vybrané metriky pro každé provedení vybrané úlohy v systému.</span><span class="sxs-lookup"><span data-stu-id="f3851-149">The **History** tab displays selected metrics for every job execution in the system for the selected job.</span></span> <span data-ttu-id="f3851-150">Tyto metriky v reálném čase poskytují hodnoty ohledně kondice vašeho Scheduleru:</span><span class="sxs-lookup"><span data-stu-id="f3851-150">These metrics provide real-time values regarding the health of your Scheduler:</span></span>

1. <span data-ttu-id="f3851-151">Status</span><span class="sxs-lookup"><span data-stu-id="f3851-151">Status</span></span>  
2. <span data-ttu-id="f3851-152">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="f3851-152">Details</span></span>  
3. <span data-ttu-id="f3851-153">Opakované pokusy</span><span class="sxs-lookup"><span data-stu-id="f3851-153">Retry attempts</span></span>
4. <span data-ttu-id="f3851-154">Výskyt: 1., 2., 3. atd.</span><span class="sxs-lookup"><span data-stu-id="f3851-154">Occurrence: 1st, 2nd, 3rd, etc.</span></span>
5. <span data-ttu-id="f3851-155">Čas zahájení provedení</span><span class="sxs-lookup"><span data-stu-id="f3851-155">Start time of execution</span></span>  
6. <span data-ttu-id="f3851-156">Čas ukončení provedení</span><span class="sxs-lookup"><span data-stu-id="f3851-156">End time of execution</span></span>
   
   ![][job-history]

<span data-ttu-id="f3851-157">Můžete kliknout na běh a zobrazí se **Podrobnosti historie** včetně kompletní odpovědi na každé provedení.</span><span class="sxs-lookup"><span data-stu-id="f3851-157">You can click on a run to view its **History Details**, including the whole response for every execution.</span></span> <span data-ttu-id="f3851-158">Toto dialogové okno vám taky umožní zkopírovat odpověď do schránky.</span><span class="sxs-lookup"><span data-stu-id="f3851-158">This dialog box also allows you to copy the response to the clipboard.</span></span>

   ![][job-history-details]

### <a name="users"></a><span data-ttu-id="f3851-159">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="f3851-159">Users</span></span>
<span data-ttu-id="f3851-160">Řízení přístupu na základě role ve službě Azure Scheduler umožňuje přesnou správu přístupu.</span><span class="sxs-lookup"><span data-stu-id="f3851-160">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure Scheduler.</span></span> <span data-ttu-id="f3851-161">Pokud se chcete naučit používat kartu Uživatelé, přečtěte si témě [Řízení přístupu Azure na základě rolí](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="f3851-161">To learn how to use the Users tab, refer to [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)</span></span>

## <a name="see-also"></a><span data-ttu-id="f3851-162">Viz také</span><span class="sxs-lookup"><span data-stu-id="f3851-162">See also</span></span>
 [<span data-ttu-id="f3851-163">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="f3851-163">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="f3851-164">Koncepty, terminologie a hierarchie entit Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f3851-164">Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="f3851-165">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f3851-165">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="f3851-166">Sestavení komplexních plánů a pokročilé opakování v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f3851-166">How to build complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="f3851-167">Referenční materiály k rozhraní REST API Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f3851-167">Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="f3851-168">Rutiny PowerShellu pro Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="f3851-168">Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="f3851-169">Vysoká dostupnost a spolehlivost Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f3851-169">Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="f3851-170">Omezení, výchozí hodnoty a kódy chyb Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f3851-170">Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="f3851-171">Odchozí ověření Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f3851-171">Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
