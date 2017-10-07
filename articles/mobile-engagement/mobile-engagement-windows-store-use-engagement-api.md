---
title: "aaaHow tooUse hello Engagement rozhraní API na univerzální pro Windows"
description: "Jak tooUse hello Engagement rozhraní API na univerzální pro Windows"
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
ms.openlocfilehash: 0256b839c28e4ef6c530106408d744038fa711ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-universal"></a><span data-ttu-id="7007b-103">Jak tooUse hello Engagement rozhraní API na univerzální pro Windows</span><span class="sxs-lookup"><span data-stu-id="7007b-103">How tooUse hello Engagement API on Windows Universal</span></span>
<span data-ttu-id="7007b-104">Tento dokument je dokument toohello rozšíření [jak tooIntegrate Engagement na univerzální pro Windows](mobile-engagement-windows-store-integrate-engagement.md): poskytuje v hloubka podrobnosti o tom, jak toouse hello Engagement API tooreport statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7007b-104">This document is an add-on toohello document [How tooIntegrate Engagement on Windows Universal](mobile-engagement-windows-store-integrate-engagement.md): it provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="7007b-105">Mějte na paměti, že pokud chcete pouze tooreport zapojení vaší aplikace relací, aktivit, dojde k chybě a technické informace, pak hello nejjednodušší způsob, jak se toomake všechny vaše `Page` dílčí třídy dědí hello `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="7007b-105">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `Page` sub-classes inherit from hello `EngagementPage` class.</span></span>

<span data-ttu-id="7007b-106">Pokud chcete, aby toodo další, např. Pokud potřebujete tooreport aplikace konkrétní události, chyb a úlohy, nebo pokud máte tooreport aktivitami aplikace jiným způsobem než jednu implementaci hello hello `EngagementPage` třídy, pak je nutné toouse hello Zapojení rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7007b-106">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementPage` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="7007b-107">Hello rozhraní API Engagement poskytuje hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="7007b-107">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="7007b-108">Dostanete toothose metody prostřednictvím `EngagementAgent.Instance`.</span><span class="sxs-lookup"><span data-stu-id="7007b-108">You can access toothose methods through `EngagementAgent.Instance`.</span></span>

<span data-ttu-id="7007b-109">I v případě, že modul agentem hello nebyla inicializována, každé rozhraní API toohello volání je odložení a budou spuštěny znovu, pokud je k dispozici hello agent.</span><span class="sxs-lookup"><span data-stu-id="7007b-109">Even if hello agent module has not been initialized, each call toohello API is deferred, and will be executed again when hello agent is available.</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="7007b-110">Koncepty engagementu</span><span class="sxs-lookup"><span data-stu-id="7007b-110">Engagement concepts</span></span>
<span data-ttu-id="7007b-111">Hello následujících částí Upřesnit hello běžné [koncepty Mobile Engagementu](mobile-engagement-concepts.md) pro platformu Windows Universal hello.</span><span class="sxs-lookup"><span data-stu-id="7007b-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for hello Windows Universal platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="7007b-112">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="7007b-112">`Session` and `Activity`</span></span>
<span data-ttu-id="7007b-113">*Aktivity* obvykle souvisí s jednu stránku hello aplikace, která je toosay hello *aktivity* spustí, když se zobrazí stránku hello a zastaví, když je uzavřený stránku hello: jedná o případ hello při hello Je engagement SDK integrovaná pomocí hello `EngagementPage` třídy.</span><span class="sxs-lookup"><span data-stu-id="7007b-113">An *activity* is usually associated with one page of hello application, that is toosay hello *activity* starts when hello page is displayed and stops when hello page is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementPage` class.</span></span>

<span data-ttu-id="7007b-114">Ale *aktivity* můžete ručně kontrolovat také pomocí hello Engagement rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="7007b-114">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="7007b-115">To vám umožní toosplit danou stránku v několika sub částí tooget další podrobnosti o využití hello části této stránky (například jak často tooknow a jak dlouho se používají dialogová okna v rámci této stránce).</span><span class="sxs-lookup"><span data-stu-id="7007b-115">This allows you toosplit a given page in several sub parts tooget more details about hello usage of this page (for example tooknow how often and how long dialogs are used inside this page).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="7007b-116">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="7007b-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="7007b-117">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="7007b-117">User starts a new Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-118">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-118">Reference</span></span>
            void StartActivity(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7007b-119">Je třeba toocall `StartActivity()` Každá aktivita uživatele hello čas změny.</span><span class="sxs-lookup"><span data-stu-id="7007b-119">You need toocall `StartActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="7007b-120">Hello první volání funkce toothis spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="7007b-120">hello first call toothis function starts a new user session.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7007b-121">Hello SDK automaticky volá metodu EndActivity hello, při zavření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="7007b-121">hello SDK automatically calls hello EndActivity method when hello application is closed.</span></span> <span data-ttu-id="7007b-122">Proto DŮRAZNĚ doporučujeme toocall hello StartActivity metoda vždy, když změny aktivity hello hello uživatele a volání tooNEVER, že hello EndActivity metoda od voláním této metody vynutí toobe hello aktuální relace skončila.</span><span class="sxs-lookup"><span data-stu-id="7007b-122">Thus, it is HIGHLY recommended toocall hello StartActivity method whenever hello activity of hello user changes, and tooNEVER call hello EndActivity method, since calling this method forces hello current session toobe ended.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="7007b-123">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-123">Example</span></span>
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="7007b-124">Uživatel končí jeho aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="7007b-124">User ends his current Activity</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-125">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-125">Reference</span></span>
            void EndActivity()

<span data-ttu-id="7007b-126">Tím končí hello aktivity a hello relace.</span><span class="sxs-lookup"><span data-stu-id="7007b-126">This ends hello activity and hello session.</span></span> <span data-ttu-id="7007b-127">Pokud skutečně víte, jaké úlohy, by neměla volat tuto metodu.</span><span class="sxs-lookup"><span data-stu-id="7007b-127">You should not call this method unless you really know what you're doing.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-128">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-128">Example</span></span>
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a><span data-ttu-id="7007b-129">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="7007b-129">Reporting Jobs</span></span>
### <a name="start-a-job"></a><span data-ttu-id="7007b-130">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="7007b-130">Start a job</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-131">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-131">Reference</span></span>
            void StartJob(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7007b-132">Můžete vytvořit hello úlohy tootrack certains přes v časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="7007b-132">You can use hello job tootrack certains tasks over a period of time.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-133">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-133">Example</span></span>
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a><span data-ttu-id="7007b-134">Ukončit úlohu</span><span class="sxs-lookup"><span data-stu-id="7007b-134">End a job</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-135">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-135">Reference</span></span>
            void EndJob(string name)

<span data-ttu-id="7007b-136">Jakmile úloha sledovány v rámci úlohy byla ukončena, by měly volat metoda EndJob hello pro tuto úlohu zadáním hello název úlohy.</span><span class="sxs-lookup"><span data-stu-id="7007b-136">As soon as a task tracked by a job has been terminated, you should call hello EndJob method for this job, by supplying hello job name.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-137">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-137">Example</span></span>
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a><span data-ttu-id="7007b-138">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="7007b-138">Reporting Events</span></span>
<span data-ttu-id="7007b-139">Je k dispozici tři typy událostí:</span><span class="sxs-lookup"><span data-stu-id="7007b-139">There is three types of events :</span></span>

* <span data-ttu-id="7007b-140">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="7007b-140">Standalone events</span></span>
* <span data-ttu-id="7007b-141">Události relací</span><span class="sxs-lookup"><span data-stu-id="7007b-141">Session events</span></span>
* <span data-ttu-id="7007b-142">Události úlohy</span><span class="sxs-lookup"><span data-stu-id="7007b-142">Job events</span></span>

### <a name="standalone-events"></a><span data-ttu-id="7007b-143">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="7007b-143">Standalone Events</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-144">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-144">Reference</span></span>
            void SendEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7007b-145">Samostatné události může dojít mimo hello kontext relace.</span><span class="sxs-lookup"><span data-stu-id="7007b-145">Standalone events can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-146">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-146">Example</span></span>
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a><span data-ttu-id="7007b-147">Události relací</span><span class="sxs-lookup"><span data-stu-id="7007b-147">Session events</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-148">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-148">Reference</span></span>
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7007b-149">Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během jeho relace.</span><span class="sxs-lookup"><span data-stu-id="7007b-149">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-150">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-150">Example</span></span>
<span data-ttu-id="7007b-151">**Bez dat:**</span><span class="sxs-lookup"><span data-stu-id="7007b-151">**Without data :**</span></span>

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

<span data-ttu-id="7007b-152">**S daty:**</span><span class="sxs-lookup"><span data-stu-id="7007b-152">**With data :**</span></span>

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a><span data-ttu-id="7007b-153">Události úlohy</span><span class="sxs-lookup"><span data-stu-id="7007b-153">Job Events</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-154">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-154">Reference</span></span>
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="7007b-155">Události úlohy jsou obvykle použité tooreport hello akce prováděné uživatelem během úlohy.</span><span class="sxs-lookup"><span data-stu-id="7007b-155">Job events are usually used tooreport hello actions performed by a user during a Job.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-156">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-156">Example</span></span>
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a><span data-ttu-id="7007b-157">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="7007b-157">Reporting Errors</span></span>
<span data-ttu-id="7007b-158">Existují tři typy chyb:</span><span class="sxs-lookup"><span data-stu-id="7007b-158">There are three types of errors :</span></span>

* <span data-ttu-id="7007b-159">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="7007b-159">Standalone errors</span></span>
* <span data-ttu-id="7007b-160">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="7007b-160">Session errors</span></span>
* <span data-ttu-id="7007b-161">Chyby úloh</span><span class="sxs-lookup"><span data-stu-id="7007b-161">Job errors</span></span>

### <a name="standalone-errors"></a><span data-ttu-id="7007b-162">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="7007b-162">Standalone errors</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-163">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-163">Reference</span></span>
            void SendError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7007b-164">Jinak zvláštní toosession chyby samostatné může dojít k chybám mimo hello kontext relace.</span><span class="sxs-lookup"><span data-stu-id="7007b-164">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-165">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-165">Example</span></span>
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a><span data-ttu-id="7007b-166">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="7007b-166">Session errors</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-167">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-167">Reference</span></span>
            void SendSessionError(string name, Dictionary<object, object> extras = null)

<span data-ttu-id="7007b-168">Relace chyby jsou obvykle použité tooreport hello chyby během jeho relace, které mají vliv hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="7007b-168">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-169">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-169">Example</span></span>
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a><span data-ttu-id="7007b-170">Chyby úloh</span><span class="sxs-lookup"><span data-stu-id="7007b-170">Job Errors</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-171">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-171">Reference</span></span>
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

<span data-ttu-id="7007b-172">Chyby může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.</span><span class="sxs-lookup"><span data-stu-id="7007b-172">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-173">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-173">Example</span></span>
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a><span data-ttu-id="7007b-174">Generování sestav havárií</span><span class="sxs-lookup"><span data-stu-id="7007b-174">Reporting Crashes</span></span>
<span data-ttu-id="7007b-175">Hello agent poskytuje dvě metody toodeal dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="7007b-175">hello agent provides two methods toodeal with crashes.</span></span>

### <a name="send-an-exception"></a><span data-ttu-id="7007b-176">Odeslat výjimku</span><span class="sxs-lookup"><span data-stu-id="7007b-176">Send an exception</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-177">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-177">Reference</span></span>
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a><span data-ttu-id="7007b-178">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-178">Example</span></span>
<span data-ttu-id="7007b-179">Výjimku kdykoli můžete odeslat voláním:</span><span class="sxs-lookup"><span data-stu-id="7007b-179">You can send an exception at any time by calling :</span></span>

            EngagementAgent.Instance.SendCrash(aCatchedException);

<span data-ttu-id="7007b-180">Můžete použít také volitelný parametr tooterminate hello engagement relace v hello stejnou dobu, než odesílání hello havárií.</span><span class="sxs-lookup"><span data-stu-id="7007b-180">You can also use an optional parameter tooterminate hello engagement session at hello same time than sending hello crash.</span></span> <span data-ttu-id="7007b-181">toodo tedy volání:</span><span class="sxs-lookup"><span data-stu-id="7007b-181">toodo so, call :</span></span>

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

<span data-ttu-id="7007b-182">Pokud tak učiníte, úlohy a hello relace budou uzavřeny právě po odeslání hello havárií.</span><span class="sxs-lookup"><span data-stu-id="7007b-182">If you do that, hello session and jobs will be closed just after sending hello crash.</span></span>

### <a name="send-an-unhandled-exception"></a><span data-ttu-id="7007b-183">Odeslat k neošetřené výjimce</span><span class="sxs-lookup"><span data-stu-id="7007b-183">Send an unhandled exception</span></span>
#### <a name="reference"></a><span data-ttu-id="7007b-184">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-184">Reference</span></span>
            void SendCrash(Exception e)

<span data-ttu-id="7007b-185">Zapojení také poskytuje metoda toosend neošetřených výjimek, pokud máte **zakázané** Engagement automatické **havárií** vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="7007b-185">Engagement also provides a method toosend unhandled exceptions if you have **DISABLED** Engagement automatic **crash** reporting.</span></span> <span data-ttu-id="7007b-186">To je obzvláště užitečná při použití uvnitř hello aplikace UnhandledException obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="7007b-186">This is especially useful when used inside hello application UnhandledException event handler.</span></span>

<span data-ttu-id="7007b-187">Tato metoda bude **vždy** ukončit relaci engagement hello a úlohy po volání.</span><span class="sxs-lookup"><span data-stu-id="7007b-187">This method will **ALWAYS** terminate hello engagement session and jobs after being called.</span></span>

#### <a name="example"></a><span data-ttu-id="7007b-188">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-188">Example</span></span>
<span data-ttu-id="7007b-189">Můžete ji tooimplement použít vlastní UnhandledExceptionEventArgs obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="7007b-189">You can use it tooimplement your own UnhandledExceptionEventArgs handler.</span></span> <span data-ttu-id="7007b-190">Například přidejte hello `Current_UnhandledException` metoda hello `App.xaml.cs` souboru:</span><span class="sxs-lookup"><span data-stu-id="7007b-190">For example, add hello `Current_UnhandledException` method of hello `App.xaml.cs` file :</span></span>

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

<span data-ttu-id="7007b-191">V souboru App.xaml.cs v "Veřejné App() {}" přidáte:</span><span class="sxs-lookup"><span data-stu-id="7007b-191">In App.xaml.cs in "Public App(){}" add:</span></span>

            Application.Current.UnhandledException += Current_UnhandledException;

## <a name="device-id"></a><span data-ttu-id="7007b-192">Id zařízení</span><span class="sxs-lookup"><span data-stu-id="7007b-192">Device Id</span></span>
            String EngagementAgent.Instance.GetDeviceId()

<span data-ttu-id="7007b-193">Voláním této metody můžete získat id zařízení engagement hello.</span><span class="sxs-lookup"><span data-stu-id="7007b-193">You can get hello engagement device id by calling this method.</span></span>

## <a name="extras-parameters"></a><span data-ttu-id="7007b-194">Parametry funkce</span><span class="sxs-lookup"><span data-stu-id="7007b-194">Extras parameters</span></span>
<span data-ttu-id="7007b-195">Libovolná data může být připojené tooan události, k chybě, aktivity nebo úlohy.</span><span class="sxs-lookup"><span data-stu-id="7007b-195">Arbitrary data can be attached tooan event, an error, an activity or a job.</span></span> <span data-ttu-id="7007b-196">Tato data můžete strukturu pomocí slovníku.</span><span class="sxs-lookup"><span data-stu-id="7007b-196">These data can be structured using a dictionary.</span></span> <span data-ttu-id="7007b-197">Klíče a hodnoty můžou být jakéhokoli typu.</span><span class="sxs-lookup"><span data-stu-id="7007b-197">Keys and values can be of any type.</span></span>

<span data-ttu-id="7007b-198">Data funkce se serializují, takže pokud chcete tooinsert vlastní typ v funkce budete mít tooadd kontraktu dat pro tento typ.</span><span class="sxs-lookup"><span data-stu-id="7007b-198">Extras data are serialized so if you want tooinsert your own type in extras you have tooadd a data contract for this type.</span></span>

### <a name="example"></a><span data-ttu-id="7007b-199">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-199">Example</span></span>
<span data-ttu-id="7007b-200">Vytvoříme novou třídu "Osoba".</span><span class="sxs-lookup"><span data-stu-id="7007b-200">We create a new class "Person".</span></span>

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

<span data-ttu-id="7007b-201">Potom přidáme `Person` instance tooan navíc.</span><span class="sxs-lookup"><span data-stu-id="7007b-201">Then, we will add a `Person` instance tooan extra.</span></span>

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> <span data-ttu-id="7007b-202">Když vložíte jiné typy objektů, ujistěte se, že jejich metodu ToString() implementovaná tooreturn lidské čitelných řetězců.</span><span class="sxs-lookup"><span data-stu-id="7007b-202">If you put other types of objects, make sure their ToString() method is implemented tooreturn a human readable string.</span></span>
> 
> 

### <a name="limits"></a><span data-ttu-id="7007b-203">Omezení</span><span class="sxs-lookup"><span data-stu-id="7007b-203">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="7007b-204">Klíče</span><span class="sxs-lookup"><span data-stu-id="7007b-204">Keys</span></span>
<span data-ttu-id="7007b-205">Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="7007b-205">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="7007b-206">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="7007b-206">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="7007b-207">Velikost</span><span class="sxs-lookup"><span data-stu-id="7007b-207">Size</span></span>
<span data-ttu-id="7007b-208">Funkce omezeny příliš**1024** znaků na jednu volání.</span><span class="sxs-lookup"><span data-stu-id="7007b-208">Extras are limited too**1024** characters per call.</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="7007b-209">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="7007b-209">Reporting Application Information</span></span>
### <a name="reference"></a><span data-ttu-id="7007b-210">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="7007b-210">Reference</span></span>
            void SendAppInfo(Dictionary<object, object> appInfos)

<span data-ttu-id="7007b-211">Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí funkce SendAppInfo() hello.</span><span class="sxs-lookup"><span data-stu-id="7007b-211">You can manually report tracking information (or any other application specific information) using hello SendAppInfo() function.</span></span>

<span data-ttu-id="7007b-212">Všimněte si, že tato data lze odeslat přírůstkově: pouze hello nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.</span><span class="sxs-lookup"><span data-stu-id="7007b-212">Note that this data can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span> <span data-ttu-id="7007b-213">Jako funkce událostí použití slovníku\<objektu, objekt\> tooattach data.</span><span class="sxs-lookup"><span data-stu-id="7007b-213">Like event extras, use a Dictionary\<object, object\> tooattach data.</span></span>

### <a name="example"></a><span data-ttu-id="7007b-214">Příklad</span><span class="sxs-lookup"><span data-stu-id="7007b-214">Example</span></span>
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="7007b-215">Omezení</span><span class="sxs-lookup"><span data-stu-id="7007b-215">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="7007b-216">Klíče</span><span class="sxs-lookup"><span data-stu-id="7007b-216">Keys</span></span>
<span data-ttu-id="7007b-217">Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="7007b-217">Each key in hello object must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*$`

<span data-ttu-id="7007b-218">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="7007b-218">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="7007b-219">Velikost</span><span class="sxs-lookup"><span data-stu-id="7007b-219">Size</span></span>
<span data-ttu-id="7007b-220">Informace o aplikaci je omezená příliš**1024** znaků na jednu volání.</span><span class="sxs-lookup"><span data-stu-id="7007b-220">Application information is limited too**1024** characters per call.</span></span>

<span data-ttu-id="7007b-221">Předchozí příklad, hello JSON odeslán toohello serveru v hello je 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="7007b-221">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"birthdate":"1983-12-07","gender":"female"}

## <a name="logging"></a><span data-ttu-id="7007b-222">Protokolování</span><span class="sxs-lookup"><span data-stu-id="7007b-222">Logging</span></span>
### <a name="enable-logging"></a><span data-ttu-id="7007b-223">Povolení protokolování</span><span class="sxs-lookup"><span data-stu-id="7007b-223">Enable Logging</span></span>
<span data-ttu-id="7007b-224">Hello SDK může být nakonfigurované tooproduce protokolů testování v konzole IDE hello.</span><span class="sxs-lookup"><span data-stu-id="7007b-224">hello SDK can be configured tooproduce test logs in hello IDE console.</span></span>
<span data-ttu-id="7007b-225">Tyto protokoly nejsou ve výchozím nastavení aktivované.</span><span class="sxs-lookup"><span data-stu-id="7007b-225">These logs are not activated by default.</span></span> <span data-ttu-id="7007b-226">toocustomize se aktualizovat hello vlastnost `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnota dostupná z hello `EngagementTestLogLevel` výčtu, například:</span><span class="sxs-lookup"><span data-stu-id="7007b-226">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

