---
title: "aaaHow tooUse hello Engagement rozhraní API na Windows Phone Silverlight"
description: "Jak tooUse hello Engagement rozhraní API na Windows Phone Silverlight"
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
ms.openlocfilehash: 1e84be95cc910be7f1227b4ae60eb483a1939284
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-phone-silverlight"></a><span data-ttu-id="7d98f-103">Jak tooUse hello Engagement rozhraní API na Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="7d98f-103">How tooUse hello Engagement API on Windows Phone Silverlight</span></span>
<span data-ttu-id="7d98f-104">Tento dokument je dokument toohello rozšíření [jak toointegrate Mobile Engagement v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="7d98f-104">This document is an add-on toohello document [How toointegrate Mobile Engagement in your Windows Phone Silverlight app](mobile-engagement-windows-phone-integrate-engagement.md).</span></span> <span data-ttu-id="7d98f-105">Poskytuje v hloubka podrobnosti o tom, jak toouse hello Engagement API tooreport statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d98f-105">It provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="7d98f-106">Pokud chcete pouze Engagement tooreport, aplikace relací, aktivit, dojde k chybě a technické informace, pak hello nejjednodušší způsob je toomake všechny vaše `PhoneApplicationPage` dílčí třídy dědí hello `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="7d98f-106">If you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `PhoneApplicationPage` sub-classes inherit from hello `EngagementPage` class.</span></span>

<span data-ttu-id="7d98f-107">Pokud chcete, aby toodo další, např. Pokud potřebujete tooreport aplikace konkrétní události, chyb a úlohy, nebo pokud máte tooreport aktivitami aplikace jiným způsobem než jednu implementaci hello hello `EngagementPage` třídy, pak je nutné toouse hello Zapojení rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7d98f-107">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementPage` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="7d98f-108">Hello rozhraní API Engagement poskytuje hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="7d98f-108">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="7d98f-109">Dostanete toothose metody prostřednictvím `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="7d98f-109">You can access toothose methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="7d98f-110">I v případě, že modul agentem hello nebyla inicializována, každé rozhraní API toohello volání je odložení a budou spuštěny znovu, pokud je k dispozici hello agent.</span><span class="sxs-lookup"><span data-stu-id="7d98f-110">Even if hello agent module has not been initialized, each call toohello API is deferred, and will be executed again when hello agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="7d98f-111">Koncepty engagementu</span><span class="sxs-lookup"><span data-stu-id="7d98f-111">Engagement concepts</span></span>
<span data-ttu-id="7d98f-112">Následující části Hello Upřesnit hello koncepty Mobile Engagementu pro platformu Windows Phone hello.</span><span class="sxs-lookup"><span data-stu-id="7d98f-112">hello following parts refine hello Mobile Engagement Concepts for hello Windows Phone platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="7d98f-113">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="7d98f-113">`Session` and `Activity`</span></span>
<span data-ttu-id="7d98f-114">*Aktivity* obvykle souvisí s jednu stránku hello aplikace, která je toosay hello *aktivity* spustí, když se zobrazí stránku hello a zastaví, když je uzavřený stránku hello: jedná o případ hello při hello Je engagement SDK integrovaná pomocí hello `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="7d98f-114">An *activity* is usually associated with one page of hello application, that is toosay hello *activity* starts when hello page is displayed and stops when hello page is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementPage` class.</span></span>

<span data-ttu-id="7d98f-115">Ale *aktivity* můžete ručně kontrolovat také pomocí hello Engagement rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7d98f-115">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="7d98f-116">To umožňuje toosplit danou stránku v několika dílčí části tooget hello další podrobnosti o použití této stránky (například jak často tooknown a jak dlouho se používají dialogová okna v rámci této stránce).</span><span class="sxs-lookup"><span data-stu-id="7d98f-116">This allows toosplit a given page in several sub parts tooget more details about hello usage of this page (for example tooknown how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="7d98f-117">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="7d98f-117">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="7d98f-118">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="7d98f-118">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-119">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-119">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7d98f-120">Je třeba toocall `StartActivity()` Každá aktivita uživatele hello čas změny.</span><span class="sxs-lookup"><span data-stu-id="7d98f-120">You need toocall `StartActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="7d98f-121">Hello první volání funkce toothis spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d98f-121">hello first call toothis function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7d98f-122">Hello SDK automaticky volání metoda EndActivity hello při zavření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="7d98f-122">hello SDK automatically call hello EndActivity method when hello application is closed.</span></span> <span data-ttu-id="7d98f-123">Proto DŮRAZNĚ doporučujeme toocall hello StartActivity metoda vždy, když aktivita hello změnu hello uživatele a volání tooNEVER, že hello EndActivity metoda od voláním této metody vynutí toobe hello aktuální relace skončila.</span><span class="sxs-lookup"><span data-stu-id="7d98f-123">Thus, it is HIGHLY recommended toocall hello StartActivity method whenever hello activity of hello user change, and tooNEVER call hello EndActivity method, since calling this method forces hello current session toobe ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="7d98f-124">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-124">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="7d98f-125">Uživatel končí jeho aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="7d98f-125">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-126">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-126">Reference</span></span>
            void EndActivity()

<span data-ttu-id="7d98f-127">Je třeba toocall `EndActivity()` alespoň jednou po dokončení hello uživatele poslední aktivita.</span><span class="sxs-lookup"><span data-stu-id="7d98f-127">You need toocall `EndActivity()` at least once when hello user finishes his last activity.</span></span> <span data-ttu-id="7d98f-128">Tento příkaz informuje hello vyprší Engagement SDK hello uživatel aktuálně nečinný, a že hello uživatelské relace nutné toobe uzavřít jednou hello časový limit relace (při volání `StartActivity()` vyprší časový limit relace hello hello relace je jednoduše dál).</span><span class="sxs-lookup"><span data-stu-id="7d98f-128">This informs hello Engagement SDK that hello user is currently idle, and that hello user session need toobe closed once hello session timeout will expire (if you call `StartActivity()` before hello session timeout expires, hello session is simply continued).</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-129">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-129">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="7d98f-130">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="7d98f-130">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="7d98f-131">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="7d98f-131">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-132">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-132">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7d98f-133">Můžete vytvořit hello úlohy tootrack certains přes v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="7d98f-133">You can use hello job tootrack certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-134">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-134">Example</span></span>
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="7d98f-135">Ukončit úlohu</span><span class="sxs-lookup"><span data-stu-id="7d98f-135">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-136">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-136">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="7d98f-137">Jakmile úloha sledovány v rámci úlohy byla ukončena, by měly volat metoda EndJob hello pro tuto úlohu zadáním hello název úlohy.</span><span class="sxs-lookup"><span data-stu-id="7d98f-137">As soon as a task tracked by a job has been terminated, you should call hello EndJob method for this job, by supplying hello job name.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-138">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-138">Example</span></span>
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="7d98f-139">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="7d98f-139">Reporting Events</span></span>
<span data-ttu-id="7d98f-140">Je k dispozici tři typy událostí:</span><span class="sxs-lookup"><span data-stu-id="7d98f-140">There is three types of events :</span></span>

* <span data-ttu-id="7d98f-141">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="7d98f-141">Standalone events</span></span>
* <span data-ttu-id="7d98f-142">Události relací</span><span class="sxs-lookup"><span data-stu-id="7d98f-142">Session events</span></span>
* <span data-ttu-id="7d98f-143">Události úlohy</span><span class="sxs-lookup"><span data-stu-id="7d98f-143">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="7d98f-144">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="7d98f-144">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-145">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-145">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7d98f-146">Samostatné události může dojít mimo hello kontext relace.</span><span class="sxs-lookup"><span data-stu-id="7d98f-146">Standalone events can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-147">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-147">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="7d98f-148">Události relací</span><span class="sxs-lookup"><span data-stu-id="7d98f-148">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-149">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-149">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7d98f-150">Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během jeho relace.</span><span class="sxs-lookup"><span data-stu-id="7d98f-150">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-151">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-151">Example</span></span>
<span data-ttu-id="7d98f-152">**Bez dat:**</span><span class="sxs-lookup"><span data-stu-id="7d98f-152">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="7d98f-153">**S daty:**</span><span class="sxs-lookup"><span data-stu-id="7d98f-153">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="7d98f-154">Události úlohy</span><span class="sxs-lookup"><span data-stu-id="7d98f-154">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-155">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-155">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="7d98f-156">Události úlohy jsou obvykle použité tooreport hello akce prováděné uživatelem během úlohy.</span><span class="sxs-lookup"><span data-stu-id="7d98f-156">Job events are usually used tooreport hello actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-157">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-157">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="7d98f-158">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="7d98f-158">Reporting Errors</span></span>
<span data-ttu-id="7d98f-159">Je k dispozici tři typy chyb:</span><span class="sxs-lookup"><span data-stu-id="7d98f-159">There is three types of errors :</span></span>

* <span data-ttu-id="7d98f-160">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="7d98f-160">Standalone errors</span></span>
* <span data-ttu-id="7d98f-161">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="7d98f-161">Session errors</span></span>
* <span data-ttu-id="7d98f-162">Chyby úloh</span><span class="sxs-lookup"><span data-stu-id="7d98f-162">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="7d98f-163">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="7d98f-163">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-164">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-164">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7d98f-165">Jinak zvláštní toosession chyby samostatné může dojít k chybám mimo hello kontext relace.</span><span class="sxs-lookup"><span data-stu-id="7d98f-165">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-166">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-166">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="7d98f-167">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="7d98f-167">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-168">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-168">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7d98f-169">Relace chyby jsou obvykle použité tooreport hello chyby během jeho relace, které mají vliv hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d98f-169">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-170">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-170">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="7d98f-171">Chyby úloh</span><span class="sxs-lookup"><span data-stu-id="7d98f-171">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-172">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-172">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="7d98f-173">Chyby může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.</span><span class="sxs-lookup"><span data-stu-id="7d98f-173">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-174">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-174">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="7d98f-175">Generování sestav havárií</span><span class="sxs-lookup"><span data-stu-id="7d98f-175">Reporting Crashes</span></span>
<span data-ttu-id="7d98f-176">Hello agent poskytuje dvě metody toodeal dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="7d98f-176">hello agent provides two methods toodeal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="7d98f-177">Odeslat výjimku</span><span class="sxs-lookup"><span data-stu-id="7d98f-177">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-178">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-178">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="7d98f-179">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-179">Example</span></span>
<span data-ttu-id="7d98f-180">Výjimku kdykoli můžete odeslat voláním:</span><span class="sxs-lookup"><span data-stu-id="7d98f-180">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="7d98f-181">Můžete použít také volitelný parametr tooterminate hello engagement relace v hello stejnou dobu, než odesílání hello havárií.</span><span class="sxs-lookup"><span data-stu-id="7d98f-181">You can also use an optional parameter tooterminate hello engagement session at hello same time than sending hello crash.</span></span> <span data-ttu-id="7d98f-182">toodo tedy volání:</span><span class="sxs-lookup"><span data-stu-id="7d98f-182">toodo so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="7d98f-183">Pokud tak učiníte, úlohy a hello relace budou uzavřeny právě po odeslání hello havárií.</span><span class="sxs-lookup"><span data-stu-id="7d98f-183">If you do that, hello session and jobs will be closed just after sending hello crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="7d98f-184">Odeslat k neošetřené výjimce</span><span class="sxs-lookup"><span data-stu-id="7d98f-184">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="7d98f-185">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-185">Reference</span></span>
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

<span data-ttu-id="7d98f-186">Engagement poskytuje také metoda toosend neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="7d98f-186">Engagement also provides a method toosend unhandled exceptions.</span></span> <span data-ttu-id="7d98f-187">To je obzvláště užitečná při použití uvnitř hello silverlight UnhandledException obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="7d98f-187">This is especially useful when used inside hello silverlight UnhandledException event handler.</span></span>

<span data-ttu-id="7d98f-188">Tato metoda bude **vždy** ukončit relaci engagement hello a úlohy po volání.</span><span class="sxs-lookup"><span data-stu-id="7d98f-188">This method will **ALWAYS** terminate hello engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="7d98f-189">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-189">Example</span></span>
<span data-ttu-id="7d98f-190">Můžete ji tooimplement použít vlastní obslužnou rutinu UnhandledException (obzvláště pokud je zakázána hello automatické hlášení reporting funkce engagementu).</span><span class="sxs-lookup"><span data-stu-id="7d98f-190">You can use it tooimplement your own UnhandledException handler (especially if you have disabled hello automatic crash reporting feature of Engagement).</span></span> <span data-ttu-id="7d98f-191">Například v hello `Application_UnhandledException` metoda hello `App.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="7d98f-191">For example, in hello `Application_UnhandledException` method of hello `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a><span data-ttu-id="7d98f-192">OnActivated</span><span class="sxs-lookup"><span data-stu-id="7d98f-192">OnActivated</span></span>
### <a name="reference"></a><span data-ttu-id="7d98f-193">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-193">Reference</span></span>
            void OnActivated(ActivatedEventArgs e)

<span data-ttu-id="7d98f-194">Když hello uživatel přejde dopředu, mimo aplikaci, po hello deaktivováno událost se vyvolá, hello operační systém se pokusí aplikace hello tooput do stavu neaktivní.</span><span class="sxs-lookup"><span data-stu-id="7d98f-194">When hello user navigates forward, away from an application, after hello Deactivated event is raised, hello operating system will attempt tooput hello application into a dormant state.</span></span> <span data-ttu-id="7d98f-195">Aplikace hello je označení.</span><span class="sxs-lookup"><span data-stu-id="7d98f-195">Then, hello application is Tombstoning.</span></span> <span data-ttu-id="7d98f-196">V tomto procesu je aplikace ukončena, ale některá data o stavu hello hello aplikace a hello jednotlivé stránky v rámci aplikace hello je zachovaná.</span><span class="sxs-lookup"><span data-stu-id="7d98f-196">In this process an application is terminated but some data about hello state of hello application and hello individual pages within hello application is preserved.</span></span>

<span data-ttu-id="7d98f-197">Máte tooinsert `EngagementAgent.Instance.OnActivated(e)` v hello `Application_Activated` metoda z hello App.xaml.cs souboru tooreset hello Engagement agenta, když aplikace hello bylo označeno jako neplatné.</span><span class="sxs-lookup"><span data-stu-id="7d98f-197">You have tooinsert `EngagementAgent.Instance.OnActivated(e)` in hello `Application_Activated` method from hello App.xaml.cs file tooreset hello Engagement Agent when hello application has been Tombstoned.</span></span>

### <a name="example"></a><span data-ttu-id="7d98f-198">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-198">Example</span></span>
            // Inside your App.xaml.cs file

            // Code tooexecute when hello application is activated (brought tooforeground)
            // This code will not execute when hello application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a><span data-ttu-id="7d98f-199">Id zařízení</span><span class="sxs-lookup"><span data-stu-id="7d98f-199">Device Id</span></span>
            String GetDeviceId()

<span data-ttu-id="7d98f-200">Voláním této metody můžete získat id zařízení engagement hello.</span><span class="sxs-lookup"><span data-stu-id="7d98f-200">You can get hello engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="7d98f-201">Parametry funkce</span><span class="sxs-lookup"><span data-stu-id="7d98f-201">Extras parameters</span></span>
<span data-ttu-id="7d98f-202">Libovolná data může být připojené tooan události, k chybě, aktivity nebo úlohy.</span><span class="sxs-lookup"><span data-stu-id="7d98f-202">Arbitrary data can be attached tooan event, an error, an activity or a job.</span></span> <span data-ttu-id="7d98f-203">Tato data můžete strukturu pomocí slovníku.</span><span class="sxs-lookup"><span data-stu-id="7d98f-203">These data can be structured using a dictionary.</span></span> <span data-ttu-id="7d98f-204">Klíče a hodnoty můžou být jakéhokoli typu.</span><span class="sxs-lookup"><span data-stu-id="7d98f-204">Keys and values can be of any type.</span></span>

<span data-ttu-id="7d98f-205">Data funkce se serializují, takže pokud chcete tooinsert vlastní typ v funkce budete mít tooadd kontraktu dat pro tento typ.</span><span class="sxs-lookup"><span data-stu-id="7d98f-205">Extras data are serialized so if you want tooinsert your own type in extras you have tooadd a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="7d98f-206">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-206">Example</span></span>
<span data-ttu-id="7d98f-207">Vytvoříme novou třídu "Osoba".</span><span class="sxs-lookup"><span data-stu-id="7d98f-207">We create a new class "Person".</span></span>

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

<span data-ttu-id="7d98f-208">Potom přidáme `Person` instance tooan navíc.</span><span class="sxs-lookup"><span data-stu-id="7d98f-208">Then, we will add a `Person` instance tooan extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="7d98f-209">Když vložíte jiné typy objektů, ujistěte se, že jejich metodu ToString() implementovaná tooreturn lidské čitelných řetězců.</span><span class="sxs-lookup"><span data-stu-id="7d98f-209">If you put other types of objects, make sure their ToString() method is implemented tooreturn a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="7d98f-210">Omezení</span><span class="sxs-lookup"><span data-stu-id="7d98f-210">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="7d98f-211">Klíče</span><span class="sxs-lookup"><span data-stu-id="7d98f-211">Keys</span></span>
<span data-ttu-id="7d98f-212">Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="7d98f-212">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="7d98f-213">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="7d98f-213">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="7d98f-214">Velikost</span><span class="sxs-lookup"><span data-stu-id="7d98f-214">Size</span></span>
<span data-ttu-id="7d98f-215">Funkce omezeny příliš**1024** znaků na jednu volání.</span><span class="sxs-lookup"><span data-stu-id="7d98f-215">Extras are limited too**1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="7d98f-216">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="7d98f-216">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="7d98f-217">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7d98f-217">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="7d98f-218">Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí funkce SendAppInfo() hello.</span><span class="sxs-lookup"><span data-stu-id="7d98f-218">You can manually report tracking information (or any other application specific information) using hello SendAppInfo() function.</span></span>

<span data-ttu-id="7d98f-219">Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze hello nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.</span><span class="sxs-lookup"><span data-stu-id="7d98f-219">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="7d98f-220">Jako funkce událostí použití slovníku\<objektu, objekt\> – tooattach informace.</span><span class="sxs-lookup"><span data-stu-id="7d98f-220">Like event extras, use a Dictionary\<object, object\> tooattach informations.</span></span>

### <a name="example"></a><span data-ttu-id="7d98f-221">Příklad</span><span class="sxs-lookup"><span data-stu-id="7d98f-221">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="7d98f-222">Omezení</span><span class="sxs-lookup"><span data-stu-id="7d98f-222">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="7d98f-223">Klíče</span><span class="sxs-lookup"><span data-stu-id="7d98f-223">Keys</span></span>
<span data-ttu-id="7d98f-224">Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="7d98f-224">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="7d98f-225">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="7d98f-225">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="7d98f-226">Velikost</span><span class="sxs-lookup"><span data-stu-id="7d98f-226">Size</span></span>
<span data-ttu-id="7d98f-227">Informace o aplikaci jsou omezené příliš**1024** znaků na jednu volání.</span><span class="sxs-lookup"><span data-stu-id="7d98f-227">Application information are limited too**1024** characters per call.</span></span>

<span data-ttu-id="7d98f-228">Předchozí příklad, hello JSON odeslán toohello serveru v hello je 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="7d98f-228">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a><span data-ttu-id="7d98f-229">Protokolování</span><span class="sxs-lookup"><span data-stu-id="7d98f-229">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="7d98f-230">Povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="7d98f-230">Enable Logging</span></span>
<span data-ttu-id="7d98f-231">Hello SDK může být nakonfigurované tooproduce protokolů testování v konzole IDE hello.</span><span class="sxs-lookup"><span data-stu-id="7d98f-231">hello SDK can be configured tooproduce test logs in hello IDE console.</span></span>
<span data-ttu-id="7d98f-232">Tyto protokoly nejsou ve výchozím nastavení aktivované.</span><span class="sxs-lookup"><span data-stu-id="7d98f-232">These logs are not activated by default.</span></span> <span data-ttu-id="7d98f-233">toocustomize se aktualizovat hello vlastnost `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnota dostupná z hello `EngagementTestLogLevel` výčtu, například:</span><span class="sxs-lookup"><span data-stu-id="7d98f-233">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
