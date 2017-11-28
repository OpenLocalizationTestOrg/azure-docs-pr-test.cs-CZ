---
title: "aaaGet spuštění se Schedulerem na portálu Azure | Microsoft Docs"
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
ms.openlocfilehash: 58255c0ad19da65932f8b1d36cb8fef1ff6e651b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a><span data-ttu-id="f2442-103">Začínáme se Schedulerem na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f2442-103">Get started with Azure Scheduler in Azure portal</span></span>
<span data-ttu-id="f2442-104">Je snadno toocreate naplánované úlohy ve službě Azure Scheduler.</span><span class="sxs-lookup"><span data-stu-id="f2442-104">It's easy toocreate scheduled jobs in Azure Scheduler.</span></span> <span data-ttu-id="f2442-105">V tomto kurzu se dozvíte jak toocreate úlohu.</span><span class="sxs-lookup"><span data-stu-id="f2442-105">In this tutorial, you'll learn how toocreate a job.</span></span> <span data-ttu-id="f2442-106">Taky poznáte možnosti Scheduleru pro sledování a správu.</span><span class="sxs-lookup"><span data-stu-id="f2442-106">You'll also learn Scheduler's monitoring and management capabilities.</span></span>

## <a name="create-a-job"></a><span data-ttu-id="f2442-107">Vytvoření úlohy</span><span class="sxs-lookup"><span data-stu-id="f2442-107">Create a job</span></span>
1. <span data-ttu-id="f2442-108">Přihlaste se příliš[portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f2442-108">Sign in too[Azure portal](https://portal.azure.com/).</span></span>  
2. <span data-ttu-id="f2442-109">Klikněte na tlačítko **+ nový** > typ *Plánovač* hello vyhledávacího pole > vyberte **Plánovač** ve výsledcích > klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f2442-109">Click **+New** > type *Scheduler* in hello search box >  select **Scheduler** in results > click **Create**.</span></span>
   
    ![][marketplace-create]
3. <span data-ttu-id="f2442-110">Teď vytvoříme úlohu, která jednoduše vyšle požadavek GET na http://www.microsoft.com/.</span><span class="sxs-lookup"><span data-stu-id="f2442-110">Let’s create a job that simply hits http://www.microsoft.com/ with a GET request.</span></span> <span data-ttu-id="f2442-111">V hello **Plánovač úloh** obrazovky, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="f2442-111">In hello **Scheduler Job** screen, enter hello following information:</span></span>
   
   1. <span data-ttu-id="f2442-112">**Název:**`getmicrosoft`</span><span class="sxs-lookup"><span data-stu-id="f2442-112">**Name:** `getmicrosoft`</span></span>  
   2. <span data-ttu-id="f2442-113">**Předplatné:** Vaše předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="f2442-113">**Subscription:** Your Azure subscription</span></span>   
   3. <span data-ttu-id="f2442-114">**Kolekce úloh:** Vyberte existující kolekci úloh nebo klikněte na **Vytvořit novou** &gt; zadejte název.</span><span class="sxs-lookup"><span data-stu-id="f2442-114">**Job Collection:** Select an existing job collection, or click **Create New** > enter a name.</span></span>
4. <span data-ttu-id="f2442-115">Vedle **nastavení akce**, definovat hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f2442-115">Next, in **Action Settings**, define hello following values:</span></span>
   
   1. <span data-ttu-id="f2442-116">**Typ akce:**` HTTP`</span><span class="sxs-lookup"><span data-stu-id="f2442-116">**Action Type:** ` HTTP`</span></span>  
   2. <span data-ttu-id="f2442-117">**Metoda:**`GET`</span><span class="sxs-lookup"><span data-stu-id="f2442-117">**Method:** `GET`</span></span>  
   3. <span data-ttu-id="f2442-118">**Adresa URL:**` http://www.microsoft.com`</span><span class="sxs-lookup"><span data-stu-id="f2442-118">**URL:** ` http://www.microsoft.com`</span></span>  
      
      ![][action-settings]
5. <span data-ttu-id="f2442-119">Nakonec nastavíme plán.</span><span class="sxs-lookup"><span data-stu-id="f2442-119">Finally, let's define a schedule.</span></span> <span data-ttu-id="f2442-120">Hello úlohy by se mohl definovat jako jednorázová, ale My vytvoříme plán opakování:</span><span class="sxs-lookup"><span data-stu-id="f2442-120">hello job could be defined as a one-time job, but let’s pick a recurrence schedule:</span></span>
   
   1. <span data-ttu-id="f2442-121">**Opakování**: `Recurring`</span><span class="sxs-lookup"><span data-stu-id="f2442-121">**Recurrence**: `Recurring`</span></span>
   2. <span data-ttu-id="f2442-122">**Začátek**: Dnešní datum</span><span class="sxs-lookup"><span data-stu-id="f2442-122">**Start**: Today's date</span></span>
   3. <span data-ttu-id="f2442-123">**Opakovat každý**: `12 Hours`</span><span class="sxs-lookup"><span data-stu-id="f2442-123">**Recur every**: `12 Hours`</span></span>
   4. <span data-ttu-id="f2442-124">**Konec**: Dva dny po dnešním datu</span><span class="sxs-lookup"><span data-stu-id="f2442-124">**End by**: Two days from today's date</span></span>  
      
      ![][recurrence-schedule]
6. <span data-ttu-id="f2442-125">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="f2442-125">Click **Create**</span></span>

## <a name="manage-and-monitor-jobs"></a><span data-ttu-id="f2442-126">Správa a sledování úloh</span><span class="sxs-lookup"><span data-stu-id="f2442-126">Manage and monitor jobs</span></span>
<span data-ttu-id="f2442-127">Po vytvoření úlohy se zobrazí v hello hlavním řídicím panelu Azure.</span><span class="sxs-lookup"><span data-stu-id="f2442-127">Once a job is created, it appears in hello main Azure dashboard.</span></span> <span data-ttu-id="f2442-128">Klikněte na úlohu hello a nový otevře se okno s hello následující karty:</span><span class="sxs-lookup"><span data-stu-id="f2442-128">Click hello job and a new window opens with hello following tabs:</span></span>

1. <span data-ttu-id="f2442-129">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f2442-129">Properties</span></span>  
2. <span data-ttu-id="f2442-130">Nastavení akce</span><span class="sxs-lookup"><span data-stu-id="f2442-130">Action Settings</span></span>  
3. <span data-ttu-id="f2442-131">Plán</span><span class="sxs-lookup"><span data-stu-id="f2442-131">Schedule</span></span>  
4. <span data-ttu-id="f2442-132">Historie</span><span class="sxs-lookup"><span data-stu-id="f2442-132">History</span></span>
5. <span data-ttu-id="f2442-133">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="f2442-133">Users</span></span>
   
   ![][job-overview]

### <a name="properties"></a><span data-ttu-id="f2442-134">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f2442-134">Properties</span></span>
<span data-ttu-id="f2442-135">Tyto vlastnosti jen pro čtení popisují metadata správy hello pro úlohu Scheduleru hello.</span><span class="sxs-lookup"><span data-stu-id="f2442-135">These read-only properties describe hello management metadata for hello Scheduler job.</span></span>

   ![][job-properties]

### <a name="action-settings"></a><span data-ttu-id="f2442-136">Nastavení akce</span><span class="sxs-lookup"><span data-stu-id="f2442-136">Action settings</span></span>
<span data-ttu-id="f2442-137">Kliknutím na úlohu v hello **úlohy** obrazovce můžete tooconfigure, které úlohy.</span><span class="sxs-lookup"><span data-stu-id="f2442-137">Clicking on a job in hello **Jobs** screen allows you tooconfigure that job.</span></span> <span data-ttu-id="f2442-138">Díky tomu můžete nakonfigurovat upřesňující nastavení, pokud jste nenakonfigurovali v hello Průvodci rychlým vytvořením.</span><span class="sxs-lookup"><span data-stu-id="f2442-138">This lets you configure advanced settings, if you didn't configure them in hello quick-create wizard.</span></span>

<span data-ttu-id="f2442-139">Pro všechny typy akcí můžete změnit zásady opakovaných pokusů hello a hello akci chyby.</span><span class="sxs-lookup"><span data-stu-id="f2442-139">For all action types, you may change hello retry policy and hello error action.</span></span>

<span data-ttu-id="f2442-140">Pro akce úlohy typy HTTP a HTTPS můžete změnit tooany hello metoda akce HTTP povolena.</span><span class="sxs-lookup"><span data-stu-id="f2442-140">For HTTP and HTTPS job action types, you may change hello method tooany allowed HTTP verb.</span></span> <span data-ttu-id="f2442-141">Můžete také přidat, odstranit nebo změnit hello hlavičky a základní ověřovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f2442-141">You may also add, delete, or change hello headers and basic authentication information.</span></span>

<span data-ttu-id="f2442-142">Pro akce fronty úložiště můžete změnit účet úložiště hello, název fronty, SAS token a text.</span><span class="sxs-lookup"><span data-stu-id="f2442-142">For storage queue action types, you may change hello storage account, queue name, SAS token, and body.</span></span>

<span data-ttu-id="f2442-143">Pro akce sběrnice můžete změnit hello obor názvů, cestu k tématu/frontě, nastavení ověřování, typ přenosu, vlastnosti zprávy a text zprávy.</span><span class="sxs-lookup"><span data-stu-id="f2442-143">For service bus action types, you may change hello namespace, topic/queue path, authentication settings, transport type, message properties, and message body.</span></span>

   ![][job-action-settings]

### <a name="schedule"></a><span data-ttu-id="f2442-144">Plán</span><span class="sxs-lookup"><span data-stu-id="f2442-144">Schedule</span></span>
<span data-ttu-id="f2442-145">Díky tomu můžete znovu nakonfigurovat plán hello, pokud chcete toochange hello plán, který jste vytvořili v hello Průvodci rychlým vytvořením.</span><span class="sxs-lookup"><span data-stu-id="f2442-145">This lets you reconfigure hello schedule, if you'd like toochange hello schedule you created in hello quick-create wizard.</span></span>

<span data-ttu-id="f2442-146">Jedná se možnost toobuild [komplexní plány a pokročilé opakování ve vaší úloze](scheduler-advanced-complexity.md)</span><span class="sxs-lookup"><span data-stu-id="f2442-146">This is an opportunity toobuild [complex schedules and advanced recurrence in your job](scheduler-advanced-complexity.md)</span></span>

<span data-ttu-id="f2442-147">Můžete změnit hello počáteční datum a čas, plán opakování a hello koncové datum a čas (Pokud hello úloha opakuje).</span><span class="sxs-lookup"><span data-stu-id="f2442-147">You may change hello start date and time, recurrence schedule, and hello end date and time (if hello job is recurring.)</span></span>

   ![][job-schedule]

### <a name="history"></a><span data-ttu-id="f2442-148">Historie</span><span class="sxs-lookup"><span data-stu-id="f2442-148">History</span></span>
<span data-ttu-id="f2442-149">Hello **historie** karta zobrazené vybrané metriky pro každé provedení úlohy v systému hello hello vybrané úlohy.</span><span class="sxs-lookup"><span data-stu-id="f2442-149">hello **History** tab displays selected metrics for every job execution in hello system for hello selected job.</span></span> <span data-ttu-id="f2442-150">Tyto metriky poskytují v reálném čase hodnoty ohledně kondice vašeho Scheduleru hello:</span><span class="sxs-lookup"><span data-stu-id="f2442-150">These metrics provide real-time values regarding hello health of your Scheduler:</span></span>

1. <span data-ttu-id="f2442-151">Status</span><span class="sxs-lookup"><span data-stu-id="f2442-151">Status</span></span>  
2. <span data-ttu-id="f2442-152">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="f2442-152">Details</span></span>  
3. <span data-ttu-id="f2442-153">Opakované pokusy</span><span class="sxs-lookup"><span data-stu-id="f2442-153">Retry attempts</span></span>
4. <span data-ttu-id="f2442-154">Výskyt: 1., 2., 3. atd.</span><span class="sxs-lookup"><span data-stu-id="f2442-154">Occurrence: 1st, 2nd, 3rd, etc.</span></span>
5. <span data-ttu-id="f2442-155">Čas zahájení provedení</span><span class="sxs-lookup"><span data-stu-id="f2442-155">Start time of execution</span></span>  
6. <span data-ttu-id="f2442-156">Čas ukončení provedení</span><span class="sxs-lookup"><span data-stu-id="f2442-156">End time of execution</span></span>
   
   ![][job-history]

<span data-ttu-id="f2442-157">Kliknutím na spustit tooview jeho **podrobnosti historie**, včetně kompletní odpovědi hello na každé provedení.</span><span class="sxs-lookup"><span data-stu-id="f2442-157">You can click on a run tooview its **History Details**, including hello whole response for every execution.</span></span> <span data-ttu-id="f2442-158">Toto dialogové okno vám umožní toocopy hello odpovědi toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="f2442-158">This dialog box also allows you toocopy hello response toohello clipboard.</span></span>

   ![][job-history-details]

### <a name="users"></a><span data-ttu-id="f2442-159">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="f2442-159">Users</span></span>
<span data-ttu-id="f2442-160">Řízení přístupu na základě role ve službě Azure Scheduler umožňuje přesnou správu přístupu.</span><span class="sxs-lookup"><span data-stu-id="f2442-160">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure Scheduler.</span></span> <span data-ttu-id="f2442-161">jak toouse hello kartě Uživatelé odkazovat příliš toolearn[řízení přístupu](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="f2442-161">toolearn how toouse hello Users tab, refer too[Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md)</span></span>

## <a name="see-also"></a><span data-ttu-id="f2442-162">Viz také</span><span class="sxs-lookup"><span data-stu-id="f2442-162">See also</span></span>
 [<span data-ttu-id="f2442-163">Co je Scheduler?</span><span class="sxs-lookup"><span data-stu-id="f2442-163">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="f2442-164">Koncepty, terminologie a hierarchie entit Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f2442-164">Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="f2442-165">Plány a fakturace v Azure Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f2442-165">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="f2442-166">Jak toobuild komplexní plány a pokročilé opakování ve službě Azure Scheduler</span><span class="sxs-lookup"><span data-stu-id="f2442-166">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="f2442-167">Referenční materiály k rozhraní REST API Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f2442-167">Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="f2442-168">Rutiny PowerShellu pro Scheduler – referenční informace</span><span class="sxs-lookup"><span data-stu-id="f2442-168">Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="f2442-169">Vysoká dostupnost a spolehlivost Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f2442-169">Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="f2442-170">Omezení, výchozí hodnoty a kódy chyb Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f2442-170">Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="f2442-171">Odchozí ověření Scheduleru</span><span class="sxs-lookup"><span data-stu-id="f2442-171">Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

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
