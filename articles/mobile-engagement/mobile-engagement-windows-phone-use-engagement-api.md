---
title: "Jak používat Engagement rozhraní API na Windows Phone Silverlight"
description: "Jak používat Engagement rozhraní API na Windows Phone Silverlight"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ae2ba2e8-f75b-4dee-a164-a7dd65d35a23
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: ec8b6c13ea052c8063dfde4321cdd286ab6cb817
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a><span data-ttu-id="ccc15-103">Jak používat Engagement rozhraní API na Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="ccc15-103">How to Use the Engagement API on Windows Phone Silverlight</span></span>
<span data-ttu-id="ccc15-104">Tento dokument je doplněk k dokumentu [jak integrovat Mobile Engagement v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="ccc15-104">This document is an add-on to the document [How to integrate Mobile Engagement in your Windows Phone Silverlight app](mobile-engagement-windows-phone-integrate-engagement.md).</span></span> <span data-ttu-id="ccc15-105">Poskytuje hloubka podrobnosti o tom, jak použít rozhraní API Engagement sestavy statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ccc15-105">It provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="ccc15-106">Pokud chcete Engagement ohlásí aplikace relací, aktivity, dojde k chybě a technické informace, pak se nejjednodušší způsob, jak provést všechny vaše `PhoneApplicationPage` dílčí třídy dědí `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="ccc15-106">If you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your `PhoneApplicationPage` sub-classes inherit from the `EngagementPage` class.</span></span>

<span data-ttu-id="ccc15-107">Pokud chcete informace, například pokud je třeba ohlásit určité události aplikace, chyb a úlohy, nebo pokud máte k hlášení aktivitami aplikace jiným způsobem než ten, implementované v `EngagementPage` třídy, pak budete muset použít rozhraní API zapojení.</span><span class="sxs-lookup"><span data-stu-id="ccc15-107">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementPage` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="ccc15-108">Rozhraní API Engagement poskytuje `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="ccc15-108">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="ccc15-109">Můžete přístup k těmto metodám prostřednictvím `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="ccc15-109">You can access to those methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="ccc15-110">I v případě, že modul agenta nebyla inicializována, každé volání rozhraní API je odložení a budou spuštěny znovu, pokud je agent k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ccc15-110">Even if the agent module has not been initialized, each call to the API is deferred, and will be executed again when the agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="ccc15-111">Koncepty engagementu</span><span class="sxs-lookup"><span data-stu-id="ccc15-111">Engagement concepts</span></span>
<span data-ttu-id="ccc15-112">Následující části Upřesnit koncepty Mobile Engagementu pro platformu Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="ccc15-112">The following parts refine the Mobile Engagement Concepts for the Windows Phone platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="ccc15-113">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="ccc15-113">`Session` and `Activity`</span></span>
<span data-ttu-id="ccc15-114">*Aktivity* je obvykle spojovány s jednu stránku aplikace, to znamená *aktivity* spustí, když se zobrazí na stránce a zastaví, když je uzavřený stránky: je tomu tak při sady Engagement SDK je integrován s použitím `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="ccc15-114">An *activity* is usually associated with one page of the application, that is to say the *activity* starts when the page is displayed and stops when the page is closed: this is the case when the Engagement SDK is integrated by using the `EngagementPage` class.</span></span>

<span data-ttu-id="ccc15-115">Ale *aktivity* můžete ručně kontrolovat také pomocí rozhraní API zapojení.</span><span class="sxs-lookup"><span data-stu-id="ccc15-115">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="ccc15-116">To umožňuje rozdělit danou stránku v několik částí Sub – Chcete-li získat další podrobnosti o použití této stránce (například jak často známé a jak dlouho se používají dialogová okna v rámci této stránce).</span><span class="sxs-lookup"><span data-stu-id="ccc15-116">This allows to split a given page in several sub parts to get more details about the usage of this page (for example to known how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="ccc15-117">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="ccc15-117">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="ccc15-118">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="ccc15-118">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-119">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-119">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ccc15-120">Je třeba volat `StartActivity()` pokaždé, když změny aktivity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ccc15-120">You need to call `StartActivity()` each time the user activity changes.</span></span> <span data-ttu-id="ccc15-121">První volání této funkce spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="ccc15-121">The first call to this function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ccc15-122">Sada SDK automaticky volejte metodu EndActivity při ukončení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ccc15-122">The SDK automatically call the EndActivity method when the application is closed.</span></span> <span data-ttu-id="ccc15-123">Proto důrazně volat metodu StartActivity vždy, když aktivita uživatele změny a nikdy volat metodu EndActivity, protože voláním této metody vynutí ukončit aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="ccc15-123">Thus, it is HIGHLY recommended to call the StartActivity method whenever the activity of the user change, and to NEVER call the EndActivity method, since calling this method forces the current session to be ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="ccc15-124">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-124">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="ccc15-125">Uživatel končí jeho aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="ccc15-125">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-126">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-126">Reference</span></span>
            void EndActivity()

<span data-ttu-id="ccc15-127">Je třeba volat `EndActivity()` alespoň jednou po dokončení uživatele poslední aktivita.</span><span class="sxs-lookup"><span data-stu-id="ccc15-127">You need to call `EndActivity()` at least once when the user finishes his last activity.</span></span> <span data-ttu-id="ccc15-128">Tento příkaz informuje sady Engagement SDK, že je uživatel aktuálně nečinnosti, a že relaci uživatele, který je třeba ukončit jednou časový limit relace vyprší (při volání `StartActivity()` předtím, než vyprší časový limit relace, relace se jednoduše pokračuje).</span><span class="sxs-lookup"><span data-stu-id="ccc15-128">This informs the Engagement SDK that the user is currently idle, and that the user session need to be closed once the session timeout will expire (if you call `StartActivity()` before the session timeout expires, the session is simply continued).</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-129">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="ccc15-130">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="ccc15-130">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="ccc15-131">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="ccc15-131">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-132">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-132">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ccc15-133">Úlohy můžete sledovat certains úlohy v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="ccc15-133">You can use the job to track certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-134">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-134">Example</span></span>
            // An upload begins...

            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="ccc15-135">Ukončit úlohu</span><span class="sxs-lookup"><span data-stu-id="ccc15-135">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-136">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-136">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="ccc15-137">Jakmile úloha sledovány v rámci úlohy byla ukončena, by měly volat metodu EndJob pro tuto úlohu zadáním název úlohy.</span><span class="sxs-lookup"><span data-stu-id="ccc15-137">As soon as a task tracked by a job has been terminated, you should call the EndJob method for this job, by supplying the job name.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-138">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-138">Example</span></span>
            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="ccc15-139">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="ccc15-139">Reporting Events</span></span>
<span data-ttu-id="ccc15-140">Je k dispozici tři typy událostí:</span><span class="sxs-lookup"><span data-stu-id="ccc15-140">There is three types of events :</span></span>

* <span data-ttu-id="ccc15-141">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="ccc15-141">Standalone events</span></span>
* <span data-ttu-id="ccc15-142">Události relací</span><span class="sxs-lookup"><span data-stu-id="ccc15-142">Session events</span></span>
* <span data-ttu-id="ccc15-143">Události úlohy</span><span class="sxs-lookup"><span data-stu-id="ccc15-143">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="ccc15-144">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="ccc15-144">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-145">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-145">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ccc15-146">Samostatné události může dojít mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="ccc15-146">Standalone events can occur outside of the context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-147">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-147">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="ccc15-148">Události relací</span><span class="sxs-lookup"><span data-stu-id="ccc15-148">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-149">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-149">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ccc15-150">Relace události se obvykle používají k hlášení akcí prováděná uživatelem během jeho relace.</span><span class="sxs-lookup"><span data-stu-id="ccc15-150">Session events are usually used to report the actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-151">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-151">Example</span></span>
<span data-ttu-id="ccc15-152">**Bez dat:**</span><span class="sxs-lookup"><span data-stu-id="ccc15-152">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="ccc15-153">**S daty:**</span><span class="sxs-lookup"><span data-stu-id="ccc15-153">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="ccc15-154">Události úlohy</span><span class="sxs-lookup"><span data-stu-id="ccc15-154">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-155">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-155">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="ccc15-156">Události úlohy jsou obvykle používají k hlášení akcí prováděná uživatelem během úlohy.</span><span class="sxs-lookup"><span data-stu-id="ccc15-156">Job events are usually used to report the actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-157">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-157">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="ccc15-158">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="ccc15-158">Reporting Errors</span></span>
<span data-ttu-id="ccc15-159">Je k dispozici tři typy chyb:</span><span class="sxs-lookup"><span data-stu-id="ccc15-159">There is three types of errors :</span></span>

* <span data-ttu-id="ccc15-160">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="ccc15-160">Standalone errors</span></span>
* <span data-ttu-id="ccc15-161">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="ccc15-161">Session errors</span></span>
* <span data-ttu-id="ccc15-162">Chyby úloh</span><span class="sxs-lookup"><span data-stu-id="ccc15-162">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="ccc15-163">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="ccc15-163">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-164">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-164">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ccc15-165">Rozporu s touto relací chyb může dojít k chybám samostatné mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="ccc15-165">Contrary to session errors, standalone errors can occur outside of the context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-166">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-166">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="ccc15-167">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="ccc15-167">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-168">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-168">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="ccc15-169">Relace chyby jsou obvykle používají k hlášení chyb během jeho relace, které mají vliv uživatele.</span><span class="sxs-lookup"><span data-stu-id="ccc15-169">Session errors are usually used to report the errors impacting the user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-170">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-170">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="ccc15-171">Chyby úloh</span><span class="sxs-lookup"><span data-stu-id="ccc15-171">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-172">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-172">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="ccc15-173">Chyby může souviset s probíhající úlohou místo má vztah k aktuální uživatelskou relaci.</span><span class="sxs-lookup"><span data-stu-id="ccc15-173">Errors can be related to a running job instead of being related to the current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-174">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="ccc15-175">Generování sestav havárií</span><span class="sxs-lookup"><span data-stu-id="ccc15-175">Reporting Crashes</span></span>
<span data-ttu-id="ccc15-176">Agent nabízí dvě metody jak nakládat s havárií.</span><span class="sxs-lookup"><span data-stu-id="ccc15-176">The agent provides two methods to deal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="ccc15-177">Odeslat výjimku</span><span class="sxs-lookup"><span data-stu-id="ccc15-177">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-178">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-178">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="ccc15-179">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-179">Example</span></span>
<span data-ttu-id="ccc15-180">Výjimku kdykoli můžete odeslat voláním:</span><span class="sxs-lookup"><span data-stu-id="ccc15-180">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="ccc15-181">Můžete také volitelný parametr ukončit relaci engagement ve stejnou dobu než odesílání havárii.</span><span class="sxs-lookup"><span data-stu-id="ccc15-181">You can also use an optional parameter to terminate the engagement session at the same time than sending the crash.</span></span> <span data-ttu-id="ccc15-182">Chcete-li tak učinit, zavolejte:</span><span class="sxs-lookup"><span data-stu-id="ccc15-182">To do so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="ccc15-183">Pokud tak učiníte, úlohy a relace budou uzavřeny právě po odeslání havárii.</span><span class="sxs-lookup"><span data-stu-id="ccc15-183">If you do that, the session and jobs will be closed just after sending the crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="ccc15-184">Odeslat k neošetřené výjimce</span><span class="sxs-lookup"><span data-stu-id="ccc15-184">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="ccc15-185">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-185">Reference</span></span>
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

<span data-ttu-id="ccc15-186">Engagement také poskytuje metodu pro odeslání neošetřených výjimek.</span><span class="sxs-lookup"><span data-stu-id="ccc15-186">Engagement also provides a method to send unhandled exceptions.</span></span> <span data-ttu-id="ccc15-187">To je obzvláště užitečná při použití uvnitř obslužné rutiny události UnhandledException silverlight.</span><span class="sxs-lookup"><span data-stu-id="ccc15-187">This is especially useful when used inside the silverlight UnhandledException event handler.</span></span>

<span data-ttu-id="ccc15-188">Tato metoda bude **vždy** ukončit relaci engagement a úlohy po volání.</span><span class="sxs-lookup"><span data-stu-id="ccc15-188">This method will **ALWAYS** terminate the engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="ccc15-189">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-189">Example</span></span>
<span data-ttu-id="ccc15-190">Můžete ho implementovat vlastní obslužnou rutinu UnhandledException (hlavně pokud mají zakázáno automatické hlášení funkci Engagement vytváření sestav).</span><span class="sxs-lookup"><span data-stu-id="ccc15-190">You can use it to implement your own UnhandledException handler (especially if you have disabled the automatic crash reporting feature of Engagement).</span></span> <span data-ttu-id="ccc15-191">Například v `Application_UnhandledException` metodu `App.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="ccc15-191">For example, in the `Application_UnhandledException` method of the `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a><span data-ttu-id="ccc15-192">OnActivated</span><span class="sxs-lookup"><span data-stu-id="ccc15-192">OnActivated</span></span>
### <a name="reference"></a><span data-ttu-id="ccc15-193">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-193">Reference</span></span>
            void OnActivated(ActivatedEventArgs e)

<span data-ttu-id="ccc15-194">Když uživatel přejde dopředu, mimo aplikaci, po deaktivováno událost se vyvolá, operační systém se pokusí aplikaci převést do stavu neaktivní.</span><span class="sxs-lookup"><span data-stu-id="ccc15-194">When the user navigates forward, away from an application, after the Deactivated event is raised, the operating system will attempt to put the application into a dormant state.</span></span> <span data-ttu-id="ccc15-195">Aplikace je označení.</span><span class="sxs-lookup"><span data-stu-id="ccc15-195">Then, the application is Tombstoning.</span></span> <span data-ttu-id="ccc15-196">V tomto procesu je aplikace ukončena, ale některá data o stavu aplikace a na jednotlivých stránkách v aplikaci je zachovaná.</span><span class="sxs-lookup"><span data-stu-id="ccc15-196">In this process an application is terminated but some data about the state of the application and the individual pages within the application is preserved.</span></span>

<span data-ttu-id="ccc15-197">Je nutné vložit `EngagementAgent.Instance.OnActivated(e)` v `Application_Activated` metoda ze souboru App.xaml.cs resetovat Engagement agenta, když aplikace bylo označeno jako neplatné.</span><span class="sxs-lookup"><span data-stu-id="ccc15-197">You have to insert `EngagementAgent.Instance.OnActivated(e)` in the `Application_Activated` method from the App.xaml.cs file to reset the Engagement Agent when the application has been Tombstoned.</span></span>

### <a name="example"></a><span data-ttu-id="ccc15-198">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-198">Example</span></span>
            // Inside your App.xaml.cs file

            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a><span data-ttu-id="ccc15-199">Id zařízení</span><span class="sxs-lookup"><span data-stu-id="ccc15-199">Device Id</span></span>
            String GetDeviceId()

<span data-ttu-id="ccc15-200">Voláním této metody můžete získat id zařízení v engagement.</span><span class="sxs-lookup"><span data-stu-id="ccc15-200">You can get the engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="ccc15-201">Parametry funkce</span><span class="sxs-lookup"><span data-stu-id="ccc15-201">Extras parameters</span></span>
<span data-ttu-id="ccc15-202">Libovolná data lze připojit k události, k chybě, aktivity nebo úlohy.</span><span class="sxs-lookup"><span data-stu-id="ccc15-202">Arbitrary data can be attached to an event, an error, an activity or a job.</span></span> <span data-ttu-id="ccc15-203">Tato data můžete strukturu pomocí slovníku.</span><span class="sxs-lookup"><span data-stu-id="ccc15-203">These data can be structured using a dictionary.</span></span> <span data-ttu-id="ccc15-204">Klíče a hodnoty můžou být jakéhokoli typu.</span><span class="sxs-lookup"><span data-stu-id="ccc15-204">Keys and values can be of any type.</span></span>

<span data-ttu-id="ccc15-205">Data funkce se serializují, takže pokud chcete, která se přidá vlastní typ funkce budete muset přidat kontraktu dat pro tento typ.</span><span class="sxs-lookup"><span data-stu-id="ccc15-205">Extras data are serialized so if you want to insert your own type in extras you have to add a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="ccc15-206">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-206">Example</span></span>
<span data-ttu-id="ccc15-207">Vytvoříme novou třídu "Osoba".</span><span class="sxs-lookup"><span data-stu-id="ccc15-207">We create a new class "Person".</span></span>

            using System.Runtime.Serialization;

            namespace Engagement.Agent
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

<span data-ttu-id="ccc15-208">Potom přidáme `Person` instance na další.</span><span class="sxs-lookup"><span data-stu-id="ccc15-208">Then, we will add a `Person` instance to an extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="ccc15-209">Když vložíte jiné typy objektů, ujistěte se, že jejich metodu ToString() je implementována vrátit lidského čitelných řetězců.</span><span class="sxs-lookup"><span data-stu-id="ccc15-209">If you put other types of objects, make sure their ToString() method is implemented to return a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="ccc15-210">Omezení</span><span class="sxs-lookup"><span data-stu-id="ccc15-210">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="ccc15-211">Klíče</span><span class="sxs-lookup"><span data-stu-id="ccc15-211">Keys</span></span>
<span data-ttu-id="ccc15-212">Každý klíč v objektu musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="ccc15-212">Each key in the object must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="ccc15-213">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="ccc15-213">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="ccc15-214">Velikost</span><span class="sxs-lookup"><span data-stu-id="ccc15-214">Size</span></span>
<span data-ttu-id="ccc15-215">Funkce jsou omezeny na **1024** znaků na jednu volání.</span><span class="sxs-lookup"><span data-stu-id="ccc15-215">Extras are limited to **1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="ccc15-216">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="ccc15-216">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="ccc15-217">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ccc15-217">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="ccc15-218">Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí SendAppInfo() funkce.</span><span class="sxs-lookup"><span data-stu-id="ccc15-218">You can manually report tracking information (or any other application specific information) using the SendAppInfo() function.</span></span>

<span data-ttu-id="ccc15-219">Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.</span><span class="sxs-lookup"><span data-stu-id="ccc15-219">Note that these information can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="ccc15-220">Jako funkce událostí použití slovníku\<objektu, objekt\> připojit – informace.</span><span class="sxs-lookup"><span data-stu-id="ccc15-220">Like event extras, use a Dictionary\<object, object\> to attach informations.</span></span>

### <a name="example"></a><span data-ttu-id="ccc15-221">Příklad</span><span class="sxs-lookup"><span data-stu-id="ccc15-221">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="ccc15-222">Omezení</span><span class="sxs-lookup"><span data-stu-id="ccc15-222">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="ccc15-223">Klíče</span><span class="sxs-lookup"><span data-stu-id="ccc15-223">Keys</span></span>
<span data-ttu-id="ccc15-224">Každý klíč v objektu musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="ccc15-224">Each key in the object must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="ccc15-225">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="ccc15-225">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="ccc15-226">Velikost</span><span class="sxs-lookup"><span data-stu-id="ccc15-226">Size</span></span>
<span data-ttu-id="ccc15-227">Informace o aplikaci jsou omezeny na **1024** znaků na jednu volání.</span><span class="sxs-lookup"><span data-stu-id="ccc15-227">Application information are limited to **1024** characters per call.</span></span>

<span data-ttu-id="ccc15-228">V předchozím příkladu je JSON odeslat na server 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="ccc15-228">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a><span data-ttu-id="ccc15-229">Protokolování</span><span class="sxs-lookup"><span data-stu-id="ccc15-229">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="ccc15-230">Povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="ccc15-230">Enable Logging</span></span>
<span data-ttu-id="ccc15-231">Sada SDK se dá nakonfigurovat k vytvoření protokolů testování v konzole IDE.</span><span class="sxs-lookup"><span data-stu-id="ccc15-231">The SDK can be configured to produce test logs in the IDE console.</span></span>
<span data-ttu-id="ccc15-232">Tyto protokoly nejsou ve výchozím nastavení aktivované.</span><span class="sxs-lookup"><span data-stu-id="ccc15-232">These logs are not activated by default.</span></span> <span data-ttu-id="ccc15-233">Chcete-li přizpůsobit tím, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu z hodnota dostupná z `EngagementTestLogLevel` výčtu pro instanci:</span><span class="sxs-lookup"><span data-stu-id="ccc15-233">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
