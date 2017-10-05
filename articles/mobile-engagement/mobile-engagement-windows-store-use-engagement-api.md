---
title: "Jak používat Engagement rozhraní API na univerzální pro Windows"
description: "Jak používat Engagement rozhraní API na univerzální pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: bb501fca-9cfe-4495-81df-b5efd6e0137b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 75fc134a5535e6113331470cf61df9c06eb8e2ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-windows-universal"></a><span data-ttu-id="42dc8-103">Jak používat Engagement rozhraní API na univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="42dc8-103">How to Use the Engagement API on Windows Universal</span></span>
<span data-ttu-id="42dc8-104">Tento dokument je doplněk k dokumentu [jak integrovat Engagement na univerzální pro Windows](mobile-engagement-windows-store-integrate-engagement.md): poskytuje hloubka podrobnosti o tom, jak použít rozhraní API Engagement sestavy statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="42dc8-104">This document is an add-on to the document [How to Integrate Engagement on Windows Universal](mobile-engagement-windows-store-integrate-engagement.md): it provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="42dc8-105">Mějte na paměti, že pokud chcete pouze Engagement ohlásí aplikace relací, aktivity, dojde k chybě a technické informace, pak nejjednodušší způsob, jak se zajistit všechny vaše `Page` dílčí třídy dědí `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="42dc8-105">Keep in mind that if you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your `Page` sub-classes inherit from the `EngagementPage` class.</span></span>

<span data-ttu-id="42dc8-106">Pokud chcete informace, například pokud je třeba ohlásit určité události aplikace, chyb a úlohy, nebo pokud máte k hlášení aktivitami aplikace jiným způsobem než ten, implementované v `EngagementPage` třídy, pak budete muset použít rozhraní API zapojení.</span><span class="sxs-lookup"><span data-stu-id="42dc8-106">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementPage` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="42dc8-107">Rozhraní API Engagement poskytuje `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="42dc8-107">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="42dc8-108">Můžete přístup k těmto metodám prostřednictvím `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="42dc8-108">You can access to those methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="42dc8-109">I v případě, že modul agenta nebyla inicializována, každé volání rozhraní API je odložení a budou spuštěny znovu, pokud je agent k dispozici.</span><span class="sxs-lookup"><span data-stu-id="42dc8-109">Even if the agent module has not been initialized, each call to the API is deferred, and will be executed again when the agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="42dc8-110">Koncepty engagementu</span><span class="sxs-lookup"><span data-stu-id="42dc8-110">Engagement concepts</span></span>
<span data-ttu-id="42dc8-111">Následující části Upřesnit nejběžnější [koncepty Mobile Engagementu](mobile-engagement-concepts.md) pro platformu Windows Universal.</span><span class="sxs-lookup"><span data-stu-id="42dc8-111">The following parts refine the common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for the Windows Universal platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="42dc8-112">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="42dc8-112">`Session` and `Activity`</span></span>
<span data-ttu-id="42dc8-113">*Aktivity* je obvykle spojovány s jednu stránku aplikace, to znamená *aktivity* spustí, když se zobrazí na stránce a zastaví, když je uzavřený stránky: je tomu tak při sady Engagement SDK je integrován s použitím `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="42dc8-113">An *activity* is usually associated with one page of the application, that is to say the *activity* starts when the page is displayed and stops when the page is closed: this is the case when the Engagement SDK is integrated by using the `EngagementPage` class.</span></span>

<span data-ttu-id="42dc8-114">Ale *aktivity* můžete ručně kontrolovat také pomocí rozhraní API zapojení.</span><span class="sxs-lookup"><span data-stu-id="42dc8-114">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="42dc8-115">To umožňuje rozdělit danou stránku v několik částí Sub – Chcete-li získat další podrobnosti o použití této stránce (například vědět, jak často a jak dlouho se používají dialogová okna v rámci této stránce).</span><span class="sxs-lookup"><span data-stu-id="42dc8-115">This allows you to split a given page in several sub parts to get more details about the usage of this page (for example to know how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="42dc8-116">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="42dc8-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="42dc8-117">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="42dc8-117">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-118">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-118">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="42dc8-119">Je třeba volat `StartActivity()` pokaždé, když změny aktivity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="42dc8-119">You need to call `StartActivity()` each time the user activity changes.</span></span> <span data-ttu-id="42dc8-120">První volání této funkce spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="42dc8-120">The first call to this function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42dc8-121">Sada SDK automaticky volá metodu EndActivity při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="42dc8-121">The SDK automatically calls the EndActivity method when the application is closed.</span></span> <span data-ttu-id="42dc8-122">Proto důrazně volat metodu StartActivity vždy, když aktivita změny uživatelů a nikdy volat metodu EndActivity, protože voláním této metody vynutí ukončit aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="42dc8-122">Thus, it is HIGHLY recommended to call the StartActivity method whenever the activity of the user changes, and to NEVER call the EndActivity method, since calling this method forces the current session to be ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="42dc8-123">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-123">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="42dc8-124">Uživatel končí jeho aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="42dc8-124">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-125">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-125">Reference</span></span>
            void EndActivity()

<span data-ttu-id="42dc8-126">Tím končí aktivity a relace.</span><span class="sxs-lookup"><span data-stu-id="42dc8-126">This ends the activity and the session.</span></span> <span data-ttu-id="42dc8-127">Pokud skutečně víte, jaké úlohy, by neměla volat tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="42dc8-127">You should not call this method unless you really know what you're doing.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-128">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-128">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="42dc8-129">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="42dc8-129">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="42dc8-130">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="42dc8-130">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-131">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-131">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="42dc8-132">Úlohy můžete sledovat certains úlohy v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="42dc8-132">You can use the job to track certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-133">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-133">Example</span></span>
            // An upload begins...

            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="42dc8-134">Ukončit úlohu</span><span class="sxs-lookup"><span data-stu-id="42dc8-134">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-135">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-135">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="42dc8-136">Jakmile úloha sledovány v rámci úlohy byla ukončena, by měly volat metodu EndJob pro tuto úlohu zadáním název úlohy.</span><span class="sxs-lookup"><span data-stu-id="42dc8-136">As soon as a task tracked by a job has been terminated, you should call the EndJob method for this job, by supplying the job name.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-137">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-137">Example</span></span>
            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="42dc8-138">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="42dc8-138">Reporting Events</span></span>
<span data-ttu-id="42dc8-139">Je k dispozici tři typy událostí:</span><span class="sxs-lookup"><span data-stu-id="42dc8-139">There is three types of events :</span></span>

* <span data-ttu-id="42dc8-140">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="42dc8-140">Standalone events</span></span>
* <span data-ttu-id="42dc8-141">Události relací</span><span class="sxs-lookup"><span data-stu-id="42dc8-141">Session events</span></span>
* <span data-ttu-id="42dc8-142">Události úlohy</span><span class="sxs-lookup"><span data-stu-id="42dc8-142">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="42dc8-143">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="42dc8-143">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-144">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-144">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="42dc8-145">Samostatné události může dojít mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="42dc8-145">Standalone events can occur outside of the context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-146">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-146">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="42dc8-147">Události relací</span><span class="sxs-lookup"><span data-stu-id="42dc8-147">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-148">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-148">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="42dc8-149">Relace události se obvykle používají k hlášení akcí prováděná uživatelem během jeho relace.</span><span class="sxs-lookup"><span data-stu-id="42dc8-149">Session events are usually used to report the actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-150">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-150">Example</span></span>
<span data-ttu-id="42dc8-151">**Bez dat:**</span><span class="sxs-lookup"><span data-stu-id="42dc8-151">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="42dc8-152">**S daty:**</span><span class="sxs-lookup"><span data-stu-id="42dc8-152">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="42dc8-153">Události úlohy</span><span class="sxs-lookup"><span data-stu-id="42dc8-153">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-154">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-154">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="42dc8-155">Události úlohy jsou obvykle používají k hlášení akcí prováděná uživatelem během úlohy.</span><span class="sxs-lookup"><span data-stu-id="42dc8-155">Job events are usually used to report the actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-156">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-156">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="42dc8-157">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="42dc8-157">Reporting Errors</span></span>
<span data-ttu-id="42dc8-158">Existují tři typy chyb:</span><span class="sxs-lookup"><span data-stu-id="42dc8-158">There are three types of errors :</span></span>

* <span data-ttu-id="42dc8-159">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="42dc8-159">Standalone errors</span></span>
* <span data-ttu-id="42dc8-160">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="42dc8-160">Session errors</span></span>
* <span data-ttu-id="42dc8-161">Chyby úloh</span><span class="sxs-lookup"><span data-stu-id="42dc8-161">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="42dc8-162">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="42dc8-162">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-163">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-163">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="42dc8-164">Rozporu s touto relací chyb může dojít k chybám samostatné mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="42dc8-164">Contrary to session errors, standalone errors can occur outside of the context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-165">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-165">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="42dc8-166">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="42dc8-166">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-167">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-167">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="42dc8-168">Relace chyby jsou obvykle používají k hlášení chyb během jeho relace, které mají vliv uživatele.</span><span class="sxs-lookup"><span data-stu-id="42dc8-168">Session errors are usually used to report the errors impacting the user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-169">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-169">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="42dc8-170">Chyby úloh</span><span class="sxs-lookup"><span data-stu-id="42dc8-170">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-171">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-171">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="42dc8-172">Chyby může souviset s probíhající úlohou místo má vztah k aktuální uživatelskou relaci.</span><span class="sxs-lookup"><span data-stu-id="42dc8-172">Errors can be related to a running job instead of being related to the current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-173">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-173">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="42dc8-174">Generování sestav havárií</span><span class="sxs-lookup"><span data-stu-id="42dc8-174">Reporting Crashes</span></span>
<span data-ttu-id="42dc8-175">Agent nabízí dvě metody jak nakládat s havárií.</span><span class="sxs-lookup"><span data-stu-id="42dc8-175">The agent provides two methods to deal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="42dc8-176">Odeslat výjimku</span><span class="sxs-lookup"><span data-stu-id="42dc8-176">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-177">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-177">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="42dc8-178">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-178">Example</span></span>
<span data-ttu-id="42dc8-179">Výjimku kdykoli můžete odeslat voláním:</span><span class="sxs-lookup"><span data-stu-id="42dc8-179">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="42dc8-180">Můžete také volitelný parametr ukončit relaci engagement ve stejnou dobu než odesílání havárii.</span><span class="sxs-lookup"><span data-stu-id="42dc8-180">You can also use an optional parameter to terminate the engagement session at the same time than sending the crash.</span></span> <span data-ttu-id="42dc8-181">Chcete-li tak učinit, zavolejte:</span><span class="sxs-lookup"><span data-stu-id="42dc8-181">To do so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="42dc8-182">Pokud tak učiníte, úlohy a relace budou uzavřeny právě po odeslání havárii.</span><span class="sxs-lookup"><span data-stu-id="42dc8-182">If you do that, the session and jobs will be closed just after sending the crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="42dc8-183">Odeslat k neošetřené výjimce</span><span class="sxs-lookup"><span data-stu-id="42dc8-183">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="42dc8-184">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-184">Reference</span></span>
            void SendCrash(Exception e)

<span data-ttu-id="42dc8-185">Zapojení také poskytuje metodu pro odeslání neošetřených výjimek, pokud máte **zakázané** Engagement automatické **havárií** vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="42dc8-185">Engagement also provides a method to send unhandled exceptions if you have **DISABLED** Engagement automatic **crash** reporting.</span></span> <span data-ttu-id="42dc8-186">To je obzvláště užitečná při použití uvnitř obslužné rutiny události UnhandledException aplikace.</span><span class="sxs-lookup"><span data-stu-id="42dc8-186">This is especially useful when used inside the application UnhandledException event handler.</span></span>

<span data-ttu-id="42dc8-187">Tato metoda bude **vždy** ukončit relaci engagement a úlohy po volání.</span><span class="sxs-lookup"><span data-stu-id="42dc8-187">This method will **ALWAYS** terminate the engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="42dc8-188">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-188">Example</span></span>
<span data-ttu-id="42dc8-189">Můžete ho implementovat vlastní UnhandledExceptionEventArgs obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="42dc8-189">You can use it to implement your own UnhandledExceptionEventArgs handler.</span></span> <span data-ttu-id="42dc8-190">Například přidat `Current_UnhandledException` metodu `App.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="42dc8-190">For example, add the `Current_UnhandledException` method of the `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

<span data-ttu-id="42dc8-191">V souboru App.xaml.cs v "Veřejné App() {}" přidáte:</span><span class="sxs-lookup"><span data-stu-id="42dc8-191">In App.xaml.cs in "Public App(){}" add:</span></span>

            Application.Current.UnhandledException += Current_UnhandledException;

## <a name="device-id"></a><span data-ttu-id="42dc8-192">Id zařízení</span><span class="sxs-lookup"><span data-stu-id="42dc8-192">Device Id</span></span>
            String EngagementAgent.Instance.GetDeviceId()

<span data-ttu-id="42dc8-193">Voláním této metody můžete získat id zařízení v engagement.</span><span class="sxs-lookup"><span data-stu-id="42dc8-193">You can get the engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="42dc8-194">Parametry funkce</span><span class="sxs-lookup"><span data-stu-id="42dc8-194">Extras parameters</span></span>
<span data-ttu-id="42dc8-195">Libovolná data lze připojit k události, k chybě, aktivity nebo úlohy.</span><span class="sxs-lookup"><span data-stu-id="42dc8-195">Arbitrary data can be attached to an event, an error, an activity or a job.</span></span> <span data-ttu-id="42dc8-196">Tato data můžete strukturu pomocí slovníku.</span><span class="sxs-lookup"><span data-stu-id="42dc8-196">These data can be structured using a dictionary.</span></span> <span data-ttu-id="42dc8-197">Klíče a hodnoty můžou být jakéhokoli typu.</span><span class="sxs-lookup"><span data-stu-id="42dc8-197">Keys and values can be of any type.</span></span>

<span data-ttu-id="42dc8-198">Data funkce se serializují, takže pokud chcete, která se přidá vlastní typ funkce budete muset přidat kontraktu dat pro tento typ.</span><span class="sxs-lookup"><span data-stu-id="42dc8-198">Extras data are serialized so if you want to insert your own type in extras you have to add a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="42dc8-199">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-199">Example</span></span>
<span data-ttu-id="42dc8-200">Vytvoříme novou třídu "Osoba".</span><span class="sxs-lookup"><span data-stu-id="42dc8-200">We create a new class "Person".</span></span>

            using System.Runtime.Serialization;

            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }

                // Properties

                [DataMember]
                public int Age
                {
                  get;
                  set;
                }

                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

<span data-ttu-id="42dc8-201">Potom přidáme `Person` instance na další.</span><span class="sxs-lookup"><span data-stu-id="42dc8-201">Then, we will add a `Person` instance to an extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="42dc8-202">Když vložíte jiné typy objektů, ujistěte se, že jejich metodu ToString() je implementována vrátit lidského čitelných řetězců.</span><span class="sxs-lookup"><span data-stu-id="42dc8-202">If you put other types of objects, make sure their ToString() method is implemented to return a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="42dc8-203">Omezení</span><span class="sxs-lookup"><span data-stu-id="42dc8-203">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="42dc8-204">Klíče</span><span class="sxs-lookup"><span data-stu-id="42dc8-204">Keys</span></span>
<span data-ttu-id="42dc8-205">Každý klíč v objektu musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="42dc8-205">Each key in the object must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="42dc8-206">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="42dc8-206">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="42dc8-207">Velikost</span><span class="sxs-lookup"><span data-stu-id="42dc8-207">Size</span></span>
<span data-ttu-id="42dc8-208">Funkce jsou omezeny na **1024** znaků na jednu volání.</span><span class="sxs-lookup"><span data-stu-id="42dc8-208">Extras are limited to **1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="42dc8-209">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="42dc8-209">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="42dc8-210">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="42dc8-210">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="42dc8-211">Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí SendAppInfo() funkce.</span><span class="sxs-lookup"><span data-stu-id="42dc8-211">You can manually report tracking information (or any other application specific information) using the SendAppInfo() function.</span></span>

<span data-ttu-id="42dc8-212">Všimněte si, že tato data lze odeslat přírůstkově: pouze nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.</span><span class="sxs-lookup"><span data-stu-id="42dc8-212">Note that this data can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="42dc8-213">Jako funkce událostí použití slovníku\<objektu, objekt\> připojit data.</span><span class="sxs-lookup"><span data-stu-id="42dc8-213">Like event extras, use a Dictionary\<object, object\> to attach data.</span></span>

### <a name="example"></a><span data-ttu-id="42dc8-214">Příklad</span><span class="sxs-lookup"><span data-stu-id="42dc8-214">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="42dc8-215">Omezení</span><span class="sxs-lookup"><span data-stu-id="42dc8-215">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="42dc8-216">Klíče</span><span class="sxs-lookup"><span data-stu-id="42dc8-216">Keys</span></span>
<span data-ttu-id="42dc8-217">Každý klíč v objektu musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="42dc8-217">Each key in the object must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="42dc8-218">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="42dc8-218">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="42dc8-219">Velikost</span><span class="sxs-lookup"><span data-stu-id="42dc8-219">Size</span></span>
<span data-ttu-id="42dc8-220">Informace o aplikaci je omezený na **1024** znaků na jednu volání.</span><span class="sxs-lookup"><span data-stu-id="42dc8-220">Application information is limited to **1024** characters per call.</span></span>

<span data-ttu-id="42dc8-221">V předchozím příkladu je JSON odeslat na server 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="42dc8-221">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

            {"birthdate":"1983-12-07","gender":"female"}

## <a name="logging"></a><span data-ttu-id="42dc8-222">Protokolování</span><span class="sxs-lookup"><span data-stu-id="42dc8-222">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="42dc8-223">Povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="42dc8-223">Enable Logging</span></span>
<span data-ttu-id="42dc8-224">Sada SDK se dá nakonfigurovat k vytvoření protokolů testování v konzole IDE.</span><span class="sxs-lookup"><span data-stu-id="42dc8-224">The SDK can be configured to produce test logs in the IDE console.</span></span>
<span data-ttu-id="42dc8-225">Tyto protokoly nejsou ve výchozím nastavení aktivované.</span><span class="sxs-lookup"><span data-stu-id="42dc8-225">These logs are not activated by default.</span></span> <span data-ttu-id="42dc8-226">Chcete-li přizpůsobit tím, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu z hodnota dostupná z `EngagementTestLogLevel` výčtu pro instanci:</span><span class="sxs-lookup"><span data-stu-id="42dc8-226">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

