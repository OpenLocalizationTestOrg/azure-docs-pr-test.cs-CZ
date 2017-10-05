---
title: "Azure Mobile Engagement webové rozhraní API sady SDK | Microsoft Docs"
description: "Nejnovější aktualizace a postupy pro Web SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8a87d5ac-d8b7-4a0d-bdee-414dbcc561b2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: 54c22ce6a03e382b1bbde102bccc97deec249b30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a><span data-ttu-id="562f7-103">Ve webové aplikaci pomocí rozhraní API služby Azure Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="562f7-103">Use the Azure Mobile Engagement API in a web application</span></span>
<span data-ttu-id="562f7-104">Tento dokument je doplňkem k dokumentu, který se dozvíte, jak k [integrovat Mobile Engagement ve webové aplikaci](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="562f7-104">This document is an addition to the document that tells you how to [integrate Mobile Engagement in a web application](mobile-engagement-web-integrate-engagement.md).</span></span> <span data-ttu-id="562f7-105">Poskytuje podrobné informace týkající se sestav vaší statistik aplikace pomocí rozhraní API služby Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="562f7-105">It provides in-depth details about how to use the Azure Mobile Engagement API to report your application statistics.</span></span>

<span data-ttu-id="562f7-106">Rozhraní API služby Mobile Engagement poskytuje `engagement.agent` objektu.</span><span class="sxs-lookup"><span data-stu-id="562f7-106">The Mobile Engagement API is provided by the `engagement.agent` object.</span></span> <span data-ttu-id="562f7-107">Výchozí sada SDK webové služby Azure Mobile Engagement alias je `engagement`.</span><span class="sxs-lookup"><span data-stu-id="562f7-107">The default Azure Mobile Engagement Web SDK alias is `engagement`.</span></span> <span data-ttu-id="562f7-108">Můžete změnit definici tento alias z konfigurace sady SDK.</span><span class="sxs-lookup"><span data-stu-id="562f7-108">You can redefine this alias from the SDK configuration.</span></span>

## <a name="mobile-engagement-concepts"></a><span data-ttu-id="562f7-109">Koncepty Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="562f7-109">Mobile Engagement concepts</span></span>
<span data-ttu-id="562f7-110">Následující části Upřesnit běžné [koncepty Mobile Engagementu](mobile-engagement-concepts.md) pro webovou platformu.</span><span class="sxs-lookup"><span data-stu-id="562f7-110">The following parts refine common [Mobile Engagement concepts](mobile-engagement-concepts.md) for the web platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="562f7-111">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="562f7-111">`Session` and `Activity`</span></span>
<span data-ttu-id="562f7-112">Pokud uživatel zůstane nečinné více než několik sekund mezi dvěma aktivitami, uživatele posloupnost aktivit je rozdělit na dvě odlišné relace.</span><span class="sxs-lookup"><span data-stu-id="562f7-112">If the user stays idle for more than a few seconds between two activities, the user's sequence of activities is split into two distinct sessions.</span></span> <span data-ttu-id="562f7-113">Tyto několik sekund, se nazývají časový limit relace.</span><span class="sxs-lookup"><span data-stu-id="562f7-113">These few seconds are called the session timeout.</span></span>

<span data-ttu-id="562f7-114">Pokud webová aplikace není deklarovat konec aktivity uživatele samostatně (voláním `engagement.agent.endActivity` funkce), server Mobile Engagement automaticky vyprší platnost relace uživatele do tří minut po zavření stránky aplikace.</span><span class="sxs-lookup"><span data-stu-id="562f7-114">If your web application doesn't declare the end of user activities by itself (by calling the `engagement.agent.endActivity` function), the Mobile Engagement server automatically expires the user session within three minutes after the application page is closed.</span></span> <span data-ttu-id="562f7-115">Tomu se říká časový limit relace serveru.</span><span class="sxs-lookup"><span data-stu-id="562f7-115">This is called the server session timeout.</span></span>

### `Crash`
<span data-ttu-id="562f7-116">Ve výchozím nastavení nejsou vytvořeny automatizované sestavy nezachycená výjimek jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="562f7-116">Automated reports of uncaught JavaScript exceptions are not created by default.</span></span> <span data-ttu-id="562f7-117">Však může hlásit havárií ručně pomocí `sendCrash` funkce (viz část o nahlašování dojde k chybě).</span><span class="sxs-lookup"><span data-stu-id="562f7-117">However, you can report crashes manually by using the `sendCrash` function (see the section on reporting crashes).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="562f7-118">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="562f7-118">Reporting activities</span></span>
<span data-ttu-id="562f7-119">Zprávy o aktivity uživatelů zahrnuje, když uživatel spustí novou aktivitu, a když uživatel končí aktuální aktivita.</span><span class="sxs-lookup"><span data-stu-id="562f7-119">Reporting on user activity includes when a user starts a new activity, and when the user ends the current activity.</span></span>

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="562f7-120">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="562f7-120">User starts a new activity</span></span>
    engagement.agent.startActivity("MyUserActivity");

<span data-ttu-id="562f7-121">Je třeba volat `startActivity()` Každá aktivita uživatele čas změny.</span><span class="sxs-lookup"><span data-stu-id="562f7-121">You need to call `startActivity()` each time user activity changes.</span></span> <span data-ttu-id="562f7-122">První volání této funkce spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="562f7-122">The first call to this function starts a new user session.</span></span>

### <a name="user-ends-the-current-activity"></a><span data-ttu-id="562f7-123">Uživatel končí aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="562f7-123">User ends the current activity</span></span>
    engagement.agent.endActivity();

<span data-ttu-id="562f7-124">Je třeba volat `endActivity()` alespoň jednou po dokončení uživatele jejich poslední aktivita.</span><span class="sxs-lookup"><span data-stu-id="562f7-124">You need to call `endActivity()` at least once when the user finishes their last activity.</span></span> <span data-ttu-id="562f7-125">Tento příkaz informuje Mobile Engagement Web SDK, že uživatel je v současné době nečinnosti a že se musí po vypršení časového limitu relace je třeba ukončit relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="562f7-125">This informs the Mobile Engagement Web SDK that the user is currently idle, and that the user session needs to be closed after the session timeout expires.</span></span> <span data-ttu-id="562f7-126">Když zavoláte `startActivity()` předtím, než vyprší časový limit relace, relaci jednoduše obnovit.</span><span class="sxs-lookup"><span data-stu-id="562f7-126">If you call `startActivity()` before the session timeout expires, the session is simply resumed.</span></span>

<span data-ttu-id="562f7-127">Protože neexistuje žádný spolehlivý volání pro při zavření okna Navigátor, je to často složité nebo dokonce znemožňují catch konec aktivity uživatele v prostředí webové.</span><span class="sxs-lookup"><span data-stu-id="562f7-127">Because there's no reliable call for when the navigator window is closed, it's often difficult or impossible to catch the end of user activities inside a web environment.</span></span> <span data-ttu-id="562f7-128">To je důvod, proč server Mobile Engagement automaticky vyprší platnost relace uživatele do tří minut po zavření stránky aplikace.</span><span class="sxs-lookup"><span data-stu-id="562f7-128">That's why the Mobile Engagement server automatically expires the user session within three minutes after the application page is closed.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="562f7-129">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="562f7-129">Reporting events</span></span>
<span data-ttu-id="562f7-130">Zprávy o události popisuje relace události a události samostatné.</span><span class="sxs-lookup"><span data-stu-id="562f7-130">Reporting on events covers session events and standalone events.</span></span>

### <a name="session-events"></a><span data-ttu-id="562f7-131">Události relací</span><span class="sxs-lookup"><span data-stu-id="562f7-131">Session events</span></span>
<span data-ttu-id="562f7-132">Relace události se obvykle používají k hlášení akcí prováděná uživatelem během relace uživatele.</span><span class="sxs-lookup"><span data-stu-id="562f7-132">Session events usually are used to report the actions performed by a user during the user's session.</span></span>

<span data-ttu-id="562f7-133">**Příklad bez doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="562f7-133">**Example without extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

<span data-ttu-id="562f7-134">**Příklad s doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="562f7-134">**Example with extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a><span data-ttu-id="562f7-135">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="562f7-135">Standalone events</span></span>
<span data-ttu-id="562f7-136">Na rozdíl od relace události může dojít k událostem samostatné mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="562f7-136">Unlike session events, standalone events can occur outside the context of a session.</span></span>

<span data-ttu-id="562f7-137">K tomu použít ``engagement.agent.sendEvent`` místo ``engagement.agent.sendSessionEvent``.</span><span class="sxs-lookup"><span data-stu-id="562f7-137">For that, use ``engagement.agent.sendEvent`` instead of ``engagement.agent.sendSessionEvent``.</span></span>

## <a name="reporting-errors"></a><span data-ttu-id="562f7-138">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="562f7-138">Reporting errors</span></span>
<span data-ttu-id="562f7-139">Zprávy o chybách obsahuje chyby relace a samostatné chyby.</span><span class="sxs-lookup"><span data-stu-id="562f7-139">Reporting on errors covers session errors and standalone errors.</span></span>

### <a name="session-errors"></a><span data-ttu-id="562f7-140">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="562f7-140">Session errors</span></span>
<span data-ttu-id="562f7-141">Relace chyby jsou obvykle používají k hlášení chyb, které mají vliv na uživatele během relace uživatele.</span><span class="sxs-lookup"><span data-stu-id="562f7-141">Session errors usually are used to report the errors that have an impact on the user during the user's session.</span></span>

<span data-ttu-id="562f7-142">**Příklad bez doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="562f7-142">**Example without extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

<span data-ttu-id="562f7-143">**Příklad s doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="562f7-143">**Example with extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="562f7-144">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="562f7-144">Standalone errors</span></span>
<span data-ttu-id="562f7-145">Na rozdíl od relace chyb může dojít k chybám samostatné mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="562f7-145">Unlike session errors, standalone errors can occur outside the context of a session.</span></span>

<span data-ttu-id="562f7-146">K tomu použít `engagement.agent.sendError` místo `engagement.agent.sendSessionError`.</span><span class="sxs-lookup"><span data-stu-id="562f7-146">For that, use `engagement.agent.sendError` instead of `engagement.agent.sendSessionError`.</span></span>

## <a name="reporting-jobs"></a><span data-ttu-id="562f7-147">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="562f7-147">Reporting jobs</span></span>
<span data-ttu-id="562f7-148">Zprávy o zahrnuje úlohy vytváření sestav chyb a událostí, ke kterým došlo během úlohy a vytváření sestav dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="562f7-148">Reporting on jobs covers reporting errors and events that occur during a job, and reporting crashes.</span></span>

<span data-ttu-id="562f7-149">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="562f7-149">**Example:**</span></span>

<span data-ttu-id="562f7-150">Pokud chcete monitorovat požadavek AJAX, použijete následující:</span><span class="sxs-lookup"><span data-stu-id="562f7-150">If you want to monitor an AJAX request, you'd use the following:</span></span>

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a><span data-ttu-id="562f7-151">Generování sestav chyb během úlohy</span><span class="sxs-lookup"><span data-stu-id="562f7-151">Reporting errors during a job</span></span>
<span data-ttu-id="562f7-152">Chyby může souviset s probíhající úlohou místo pro aktuální relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="562f7-152">Errors can be related to a running job instead of to the current user session.</span></span>

<span data-ttu-id="562f7-153">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="562f7-153">**Example:**</span></span>

<span data-ttu-id="562f7-154">Pokud chcete zobrazovat chyby, pokud se nezdaří požadavek AJAX:</span><span class="sxs-lookup"><span data-stu-id="562f7-154">If you want to report an error if an AJAX request fails:</span></span>

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="562f7-155">Události vytváření sestav během úlohy</span><span class="sxs-lookup"><span data-stu-id="562f7-155">Reporting events during a job</span></span>
<span data-ttu-id="562f7-156">Události může souviset s probíhající úlohou místo pro aktuální uživatelskou relaci, Poděkování `engagement.agent.sendJobEvent` funkce.</span><span class="sxs-lookup"><span data-stu-id="562f7-156">Events can be related to a running job instead of to the current user session, thanks to the `engagement.agent.sendJobEvent` function.</span></span>

<span data-ttu-id="562f7-157">Tato funkce funguje stejně jako `engagement.agent.sendJobError`.</span><span class="sxs-lookup"><span data-stu-id="562f7-157">This function works exactly like `engagement.agent.sendJobError`.</span></span>

### <a name="reporting-crashes"></a><span data-ttu-id="562f7-158">Vytváření sestav dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="562f7-158">Reporting crashes</span></span>
<span data-ttu-id="562f7-159">Použití `sendCrash` funkce do sestavy dojde k chybě ručně.</span><span class="sxs-lookup"><span data-stu-id="562f7-159">Use the `sendCrash` function to report crashes manually.</span></span>

<span data-ttu-id="562f7-160">`crashid` Argument je řetězec, který určuje typ havárie.</span><span class="sxs-lookup"><span data-stu-id="562f7-160">The `crashid` argument is a string that identifies the type of crash.</span></span>
<span data-ttu-id="562f7-161">`crash` Argument je obvykle trasování zásobníku havárie jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="562f7-161">The `crash` argument usually is the stack trace of the crash as a string.</span></span>

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a><span data-ttu-id="562f7-162">Další parametry</span><span class="sxs-lookup"><span data-stu-id="562f7-162">Extra parameters</span></span>
<span data-ttu-id="562f7-163">Libovolná data můžete připojit k události, chyba, aktivity nebo úlohy.</span><span class="sxs-lookup"><span data-stu-id="562f7-163">You can attach arbitrary data to an event, error, activity, or job.</span></span>

<span data-ttu-id="562f7-164">Data může být jakýkoli objekt JSON (ale ne pole nebo primitivní typ).</span><span class="sxs-lookup"><span data-stu-id="562f7-164">The data can be any JSON object (but not an array or primitive type).</span></span>

<span data-ttu-id="562f7-165">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="562f7-165">**Example:**</span></span>

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="562f7-166">Omezení</span><span class="sxs-lookup"><span data-stu-id="562f7-166">Limits</span></span>
<span data-ttu-id="562f7-167">Omezení, které platí pro další parametry jsou v oblastech regulárních výrazů pro klíče, hodnotové typy a velikosti.</span><span class="sxs-lookup"><span data-stu-id="562f7-167">Limits that apply to extra parameters are in the areas of regular expressions for keys, value types, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="562f7-168">Klíče</span><span class="sxs-lookup"><span data-stu-id="562f7-168">Keys</span></span>
<span data-ttu-id="562f7-169">Každý klíč v objektu musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="562f7-169">Each key in the object must match the following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="562f7-170">To znamená, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="562f7-170">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="values"></a><span data-ttu-id="562f7-171">Hodnoty</span><span class="sxs-lookup"><span data-stu-id="562f7-171">Values</span></span>
<span data-ttu-id="562f7-172">Hodnoty jsou omezeny na řetězec, počtu a typů logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="562f7-172">Values are limited to string, number, and Boolean types.</span></span>

#### <a name="size"></a><span data-ttu-id="562f7-173">Velikost</span><span class="sxs-lookup"><span data-stu-id="562f7-173">Size</span></span>
<span data-ttu-id="562f7-174">Funkce jsou omezeny na 1024 znaků na jednu volání (po Mobile Engagement SDK webové kóduje ho ve formátu JSON).</span><span class="sxs-lookup"><span data-stu-id="562f7-174">Extras are limited to 1,024 characters per call (after the Mobile Engagement Web SDK encodes it in JSON).</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="562f7-175">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="562f7-175">Reporting application information</span></span>
<span data-ttu-id="562f7-176">Můžete ručně odesílat zprávy o sledování informace (nebo všechny ostatní informace specifické pro aplikace) pomocí `sendAppInfo()` funkce.</span><span class="sxs-lookup"><span data-stu-id="562f7-176">You can manually report tracking information (or any other application-specific information) by using the `sendAppInfo()` function.</span></span>

<span data-ttu-id="562f7-177">Všimněte si, že tyto údaje lze odeslat postupně.</span><span class="sxs-lookup"><span data-stu-id="562f7-177">Note that this information can be sent incrementally.</span></span> <span data-ttu-id="562f7-178">Pro určité zařízení se zachová pouze nejnovější hodnotu pro konkrétní klíč.</span><span class="sxs-lookup"><span data-stu-id="562f7-178">Only the latest value for a specific key will be kept for a specific device.</span></span>

<span data-ttu-id="562f7-179">Jako funkce událostí můžete použít libovolný objekt JSON abstrahovat informace o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="562f7-179">Like event extras, you can use any JSON object to abstract application information.</span></span> <span data-ttu-id="562f7-180">Všimněte si, že pole nebo dílčí objekty jsou považovány za plochý řetězce (pomocí serializace JSON).</span><span class="sxs-lookup"><span data-stu-id="562f7-180">Note that arrays or sub-objects are treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="562f7-181">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="562f7-181">**Example:**</span></span>

<span data-ttu-id="562f7-182">Zde je ukázka kódu pro odesílání pohlaví uživatele a datum narození:</span><span class="sxs-lookup"><span data-stu-id="562f7-182">Here is a code sample for sending the user's gender and birth date:</span></span>

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a><span data-ttu-id="562f7-183">Omezení</span><span class="sxs-lookup"><span data-stu-id="562f7-183">Limits</span></span>
<span data-ttu-id="562f7-184">Omezení, která se týkají informací o aplikaci nejsou v oblastech regulárních výrazů pro klíče a velikost.</span><span class="sxs-lookup"><span data-stu-id="562f7-184">Limits that apply to application information are in the areas of regular expressions for keys, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="562f7-185">Klíče</span><span class="sxs-lookup"><span data-stu-id="562f7-185">Keys</span></span>
<span data-ttu-id="562f7-186">Každý klíč v objektu musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="562f7-186">Each key in the object must match the following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="562f7-187">To znamená, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="562f7-187">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="562f7-188">Velikost</span><span class="sxs-lookup"><span data-stu-id="562f7-188">Size</span></span>
<span data-ttu-id="562f7-189">Informace o aplikaci je omezená na 1024 znaků na jednu volání (po Mobile Engagement SDK webové kóduje ho ve formátu JSON).</span><span class="sxs-lookup"><span data-stu-id="562f7-189">Application information is limited to 1,024 characters per call (after the Mobile Engagement Web SDK encodes it in JSON).</span></span>

<span data-ttu-id="562f7-190">V předchozím příkladu je JSON odeslat na server 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="562f7-190">In the preceding example, the JSON sent to the server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
