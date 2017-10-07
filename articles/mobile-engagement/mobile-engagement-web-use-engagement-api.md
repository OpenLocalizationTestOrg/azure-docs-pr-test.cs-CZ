---
title: "aaaAzure API sady SDK webové Mobile Engagement | Microsoft Docs"
description: "Hello nejnovější aktualizace a postupy pro hello Web SDK pro Azure Mobile Engagement"
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
ms.openlocfilehash: ec1261d6ad573b8c3ad6d5f616ab7bbe560d6fe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-mobile-engagement-api-in-a-web-application"></a><span data-ttu-id="4325c-103">Použití hello rozhraní API služby Azure Mobile Engagement ve webové aplikaci</span><span class="sxs-lookup"><span data-stu-id="4325c-103">Use hello Azure Mobile Engagement API in a web application</span></span>
<span data-ttu-id="4325c-104">Tento dokument je dokument toohello přidání informující o tom, jak příliš[integrovat Mobile Engagement ve webové aplikaci](mobile-engagement-web-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="4325c-104">This document is an addition toohello document that tells you how too[integrate Mobile Engagement in a web application](mobile-engagement-web-integrate-engagement.md).</span></span> <span data-ttu-id="4325c-105">Poskytuje podrobné informace o tom, jak toouse hello tooreport rozhraní API služby Azure Mobile Engagement statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4325c-105">It provides in-depth details about how toouse hello Azure Mobile Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="4325c-106">Hello Mobile Engagement API poskytuje hello `engagement.agent` objektu.</span><span class="sxs-lookup"><span data-stu-id="4325c-106">hello Mobile Engagement API is provided by hello `engagement.agent` object.</span></span> <span data-ttu-id="4325c-107">výchozí sada SDK webové služby Azure Mobile Engagement alias je Hello `engagement`.</span><span class="sxs-lookup"><span data-stu-id="4325c-107">hello default Azure Mobile Engagement Web SDK alias is `engagement`.</span></span> <span data-ttu-id="4325c-108">Můžete změnit definici tento alias z konfigurace sady SDK hello.</span><span class="sxs-lookup"><span data-stu-id="4325c-108">You can redefine this alias from hello SDK configuration.</span></span>

## <a name="mobile-engagement-concepts"></a><span data-ttu-id="4325c-109">Koncepty Mobile Engagementu</span><span class="sxs-lookup"><span data-stu-id="4325c-109">Mobile Engagement concepts</span></span>
<span data-ttu-id="4325c-110">Hello následujících částí Upřesnit běžné [koncepty Mobile Engagementu](mobile-engagement-concepts.md) pro hello webové platformy.</span><span class="sxs-lookup"><span data-stu-id="4325c-110">hello following parts refine common [Mobile Engagement concepts](mobile-engagement-concepts.md) for hello web platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="4325c-111">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="4325c-111">`Session` and `Activity`</span></span>
<span data-ttu-id="4325c-112">Pokud uživatel hello zůstane nečinné více než několik sekund mezi dvěma aktivitami, hello uživatele posloupnost aktivit je rozdělit na dvě odlišné relace.</span><span class="sxs-lookup"><span data-stu-id="4325c-112">If hello user stays idle for more than a few seconds between two activities, hello user's sequence of activities is split into two distinct sessions.</span></span> <span data-ttu-id="4325c-113">Tyto několik sekund, se nazývají hello časový limit relace.</span><span class="sxs-lookup"><span data-stu-id="4325c-113">These few seconds are called hello session timeout.</span></span>

<span data-ttu-id="4325c-114">Pokud webová aplikace není deklarovat hello konec aktivity uživatele samostatně (pomocí volání hello `engagement.agent.endActivity` funkce), server hello Mobile Engagement automaticky vyprší hello relace uživatele do tří minut po zavření stránky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4325c-114">If your web application doesn't declare hello end of user activities by itself (by calling hello `engagement.agent.endActivity` function), hello Mobile Engagement server automatically expires hello user session within three minutes after hello application page is closed.</span></span> <span data-ttu-id="4325c-115">Tomu se říká časový limit relace server hello.</span><span class="sxs-lookup"><span data-stu-id="4325c-115">This is called hello server session timeout.</span></span>

### `Crash`
<span data-ttu-id="4325c-116">Ve výchozím nastavení nejsou vytvořeny automatizované sestavy nezachycená výjimek jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4325c-116">Automated reports of uncaught JavaScript exceptions are not created by default.</span></span> <span data-ttu-id="4325c-117">Však může hlásit havárií ručně pomocí hello `sendCrash` funkce (viz část hello na reporting havárií).</span><span class="sxs-lookup"><span data-stu-id="4325c-117">However, you can report crashes manually by using hello `sendCrash` function (see hello section on reporting crashes).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="4325c-118">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="4325c-118">Reporting activities</span></span>
<span data-ttu-id="4325c-119">Zprávy o aktivity uživatelů zahrnuje, když uživatel spustí novou aktivitu, a při ukončení hello aktuální aktivita uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="4325c-119">Reporting on user activity includes when a user starts a new activity, and when hello user ends hello current activity.</span></span>

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="4325c-120">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="4325c-120">User starts a new activity</span></span>
    engagement.agent.startActivity("MyUserActivity");

<span data-ttu-id="4325c-121">Je třeba toocall `startActivity()` Každá aktivita uživatele čas změny.</span><span class="sxs-lookup"><span data-stu-id="4325c-121">You need toocall `startActivity()` each time user activity changes.</span></span> <span data-ttu-id="4325c-122">Hello první volání funkce toothis spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="4325c-122">hello first call toothis function starts a new user session.</span></span>

### <a name="user-ends-hello-current-activity"></a><span data-ttu-id="4325c-123">Uživatel končí hello aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="4325c-123">User ends hello current activity</span></span>
    engagement.agent.endActivity();

<span data-ttu-id="4325c-124">Je třeba toocall `endActivity()` alespoň jednou po dokončení hello uživatele jejich poslední aktivita.</span><span class="sxs-lookup"><span data-stu-id="4325c-124">You need toocall `endActivity()` at least once when hello user finishes their last activity.</span></span> <span data-ttu-id="4325c-125">Tento příkaz informuje hello Mobile Engagement Web SDK je aktuálně nečinnosti hello uživatele, a že hello uživatelské relace musí toobe po vyprší časový limit relace hello ukončeno.</span><span class="sxs-lookup"><span data-stu-id="4325c-125">This informs hello Mobile Engagement Web SDK that hello user is currently idle, and that hello user session needs toobe closed after hello session timeout expires.</span></span> <span data-ttu-id="4325c-126">Když zavoláte `startActivity()` vyprší časový limit relace hello hello relaci je jednoduše obnovit.</span><span class="sxs-lookup"><span data-stu-id="4325c-126">If you call `startActivity()` before hello session timeout expires, hello session is simply resumed.</span></span>

<span data-ttu-id="4325c-127">Protože neexistuje žádný spolehlivý volání pro při zavření okna Navigátor hello, je obtížné nebo dokonce znemožňují toocatch hello konec aktivity uživatele v prostředí webové.</span><span class="sxs-lookup"><span data-stu-id="4325c-127">Because there's no reliable call for when hello navigator window is closed, it's often difficult or impossible toocatch hello end of user activities inside a web environment.</span></span> <span data-ttu-id="4325c-128">Proč je hello Mobile Engagement serveru automaticky vyprší hello uživatelské relace do tří minut po zavření stránky aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4325c-128">That's why hello Mobile Engagement server automatically expires hello user session within three minutes after hello application page is closed.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="4325c-129">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="4325c-129">Reporting events</span></span>
<span data-ttu-id="4325c-130">Zprávy o události popisuje relace události a události samostatné.</span><span class="sxs-lookup"><span data-stu-id="4325c-130">Reporting on events covers session events and standalone events.</span></span>

### <a name="session-events"></a><span data-ttu-id="4325c-131">Události relací</span><span class="sxs-lookup"><span data-stu-id="4325c-131">Session events</span></span>
<span data-ttu-id="4325c-132">Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během relace hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="4325c-132">Session events usually are used tooreport hello actions performed by a user during hello user's session.</span></span>

<span data-ttu-id="4325c-133">**Příklad bez doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="4325c-133">**Example without extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

<span data-ttu-id="4325c-134">**Příklad s doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="4325c-134">**Example with extra data:**</span></span>

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a><span data-ttu-id="4325c-135">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="4325c-135">Standalone events</span></span>
<span data-ttu-id="4325c-136">Na rozdíl od relace události může dojít k událostem samostatné mimo hello kontext relace.</span><span class="sxs-lookup"><span data-stu-id="4325c-136">Unlike session events, standalone events can occur outside hello context of a session.</span></span>

<span data-ttu-id="4325c-137">K tomu použít ``engagement.agent.sendEvent`` místo ``engagement.agent.sendSessionEvent``.</span><span class="sxs-lookup"><span data-stu-id="4325c-137">For that, use ``engagement.agent.sendEvent`` instead of ``engagement.agent.sendSessionEvent``.</span></span>

## <a name="reporting-errors"></a><span data-ttu-id="4325c-138">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="4325c-138">Reporting errors</span></span>
<span data-ttu-id="4325c-139">Zprávy o chybách obsahuje chyby relace a samostatné chyby.</span><span class="sxs-lookup"><span data-stu-id="4325c-139">Reporting on errors covers session errors and standalone errors.</span></span>

### <a name="session-errors"></a><span data-ttu-id="4325c-140">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="4325c-140">Session errors</span></span>
<span data-ttu-id="4325c-141">Relace chyby jsou obvykle použité tooreport hello chyby, které mají vliv na uživatele hello během relace hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="4325c-141">Session errors usually are used tooreport hello errors that have an impact on hello user during hello user's session.</span></span>

<span data-ttu-id="4325c-142">**Příklad bez doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="4325c-142">**Example without extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

<span data-ttu-id="4325c-143">**Příklad s doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="4325c-143">**Example with extra data:**</span></span>

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="4325c-144">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="4325c-144">Standalone errors</span></span>
<span data-ttu-id="4325c-145">Na rozdíl od relace chyby může dojít k chybám samostatné mimo hello kontext relace.</span><span class="sxs-lookup"><span data-stu-id="4325c-145">Unlike session errors, standalone errors can occur outside hello context of a session.</span></span>

<span data-ttu-id="4325c-146">K tomu použít `engagement.agent.sendError` místo `engagement.agent.sendSessionError`.</span><span class="sxs-lookup"><span data-stu-id="4325c-146">For that, use `engagement.agent.sendError` instead of `engagement.agent.sendSessionError`.</span></span>

## <a name="reporting-jobs"></a><span data-ttu-id="4325c-147">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="4325c-147">Reporting jobs</span></span>
<span data-ttu-id="4325c-148">Zprávy o zahrnuje úlohy vytváření sestav chyb a událostí, ke kterým došlo během úlohy a vytváření sestav dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="4325c-148">Reporting on jobs covers reporting errors and events that occur during a job, and reporting crashes.</span></span>

<span data-ttu-id="4325c-149">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="4325c-149">**Example:**</span></span>

<span data-ttu-id="4325c-150">Pokud chcete toomonitor požadavek AJAX, použijte následující hello:</span><span class="sxs-lookup"><span data-stu-id="4325c-150">If you want toomonitor an AJAX request, you'd use hello following:</span></span>

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

### <a name="reporting-errors-during-a-job"></a><span data-ttu-id="4325c-151">Generování sestav chyb během úlohy</span><span class="sxs-lookup"><span data-stu-id="4325c-151">Reporting errors during a job</span></span>
<span data-ttu-id="4325c-152">Chyby může být související tooa spuštění úlohy místo toohello se aktuální uživatelská relace.</span><span class="sxs-lookup"><span data-stu-id="4325c-152">Errors can be related tooa running job instead of toohello current user session.</span></span>

<span data-ttu-id="4325c-153">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="4325c-153">**Example:**</span></span>

<span data-ttu-id="4325c-154">Pokud chcete, aby tooreport chybu, pokud požadavek AJAX selže:</span><span class="sxs-lookup"><span data-stu-id="4325c-154">If you want tooreport an error if an AJAX request fails:</span></span>

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

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="4325c-155">Události vytváření sestav během úlohy</span><span class="sxs-lookup"><span data-stu-id="4325c-155">Reporting events during a job</span></span>
<span data-ttu-id="4325c-156">Události může být související tooa spuštění úlohy místo toohello se aktuální uživatelská relace, thanks toohello `engagement.agent.sendJobEvent` funkce.</span><span class="sxs-lookup"><span data-stu-id="4325c-156">Events can be related tooa running job instead of toohello current user session, thanks toohello `engagement.agent.sendJobEvent` function.</span></span>

<span data-ttu-id="4325c-157">Tato funkce funguje stejně jako `engagement.agent.sendJobError`.</span><span class="sxs-lookup"><span data-stu-id="4325c-157">This function works exactly like `engagement.agent.sendJobError`.</span></span>

### <a name="reporting-crashes"></a><span data-ttu-id="4325c-158">Vytváření sestav dojde k chybě</span><span class="sxs-lookup"><span data-stu-id="4325c-158">Reporting crashes</span></span>
<span data-ttu-id="4325c-159">Použití hello `sendCrash` dojde k chybě funkce tooreport ručně.</span><span class="sxs-lookup"><span data-stu-id="4325c-159">Use hello `sendCrash` function tooreport crashes manually.</span></span>

<span data-ttu-id="4325c-160">Hello `crashid` argument je řetězec, který identifikuje hello typ havárie.</span><span class="sxs-lookup"><span data-stu-id="4325c-160">hello `crashid` argument is a string that identifies hello type of crash.</span></span>
<span data-ttu-id="4325c-161">Hello `crash` argument je obvykle trasování zásobníku hello hello havárie jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="4325c-161">hello `crash` argument usually is hello stack trace of hello crash as a string.</span></span>

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a><span data-ttu-id="4325c-162">Další parametry</span><span class="sxs-lookup"><span data-stu-id="4325c-162">Extra parameters</span></span>
<span data-ttu-id="4325c-163">Můžete připojit libovolná data tooan události, chyba, aktivity nebo úlohy.</span><span class="sxs-lookup"><span data-stu-id="4325c-163">You can attach arbitrary data tooan event, error, activity, or job.</span></span>

<span data-ttu-id="4325c-164">Hello dat může být jakýkoli objekt JSON (ale ne pole nebo primitivní typ).</span><span class="sxs-lookup"><span data-stu-id="4325c-164">hello data can be any JSON object (but not an array or primitive type).</span></span>

<span data-ttu-id="4325c-165">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="4325c-165">**Example:**</span></span>

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="4325c-166">Omezení</span><span class="sxs-lookup"><span data-stu-id="4325c-166">Limits</span></span>
<span data-ttu-id="4325c-167">Omezení, které se vztahují tooextra parametry jsou v oblastech hello regulárních výrazů pro klíče, hodnotové typy a velikosti.</span><span class="sxs-lookup"><span data-stu-id="4325c-167">Limits that apply tooextra parameters are in hello areas of regular expressions for keys, value types, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="4325c-168">Klíče</span><span class="sxs-lookup"><span data-stu-id="4325c-168">Keys</span></span>
<span data-ttu-id="4325c-169">Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="4325c-169">Each key in hello object must match hello following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="4325c-170">To znamená, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="4325c-170">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="values"></a><span data-ttu-id="4325c-171">Hodnoty</span><span class="sxs-lookup"><span data-stu-id="4325c-171">Values</span></span>
<span data-ttu-id="4325c-172">Hodnoty jsou omezené toostring, počtu a typů logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="4325c-172">Values are limited toostring, number, and Boolean types.</span></span>

#### <a name="size"></a><span data-ttu-id="4325c-173">Velikost</span><span class="sxs-lookup"><span data-stu-id="4325c-173">Size</span></span>
<span data-ttu-id="4325c-174">Funkce jsou omezené too1 024 znaků na jednu volání (po hello Mobile Engagement Web SDK kóduje ho ve formátu JSON).</span><span class="sxs-lookup"><span data-stu-id="4325c-174">Extras are limited too1,024 characters per call (after hello Mobile Engagement Web SDK encodes it in JSON).</span></span>

## <a name="reporting-application-information"></a><span data-ttu-id="4325c-175">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="4325c-175">Reporting application information</span></span>
<span data-ttu-id="4325c-176">Můžete ručně odesílat zprávy o sledování informace (nebo všechny ostatní informace specifické pro aplikace) pomocí hello `sendAppInfo()` funkce.</span><span class="sxs-lookup"><span data-stu-id="4325c-176">You can manually report tracking information (or any other application-specific information) by using hello `sendAppInfo()` function.</span></span>

<span data-ttu-id="4325c-177">Všimněte si, že tyto údaje lze odeslat postupně.</span><span class="sxs-lookup"><span data-stu-id="4325c-177">Note that this information can be sent incrementally.</span></span> <span data-ttu-id="4325c-178">Pro určité zařízení se zachová hello nejnovější hodnotu pouze pro konkrétního klíče.</span><span class="sxs-lookup"><span data-stu-id="4325c-178">Only hello latest value for a specific key will be kept for a specific device.</span></span>

<span data-ttu-id="4325c-179">Jako funkce událostí můžete použít informace o všech JSON objektu tooabstract aplikace.</span><span class="sxs-lookup"><span data-stu-id="4325c-179">Like event extras, you can use any JSON object tooabstract application information.</span></span> <span data-ttu-id="4325c-180">Všimněte si, že pole nebo dílčí objekty jsou považovány za plochý řetězce (pomocí serializace JSON).</span><span class="sxs-lookup"><span data-stu-id="4325c-180">Note that arrays or sub-objects are treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="4325c-181">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="4325c-181">**Example:**</span></span>

<span data-ttu-id="4325c-182">Zde je ukázka kódu pro odesílání hello uživatele pohlaví a datum narození:</span><span class="sxs-lookup"><span data-stu-id="4325c-182">Here is a code sample for sending hello user's gender and birth date:</span></span>

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a><span data-ttu-id="4325c-183">Omezení</span><span class="sxs-lookup"><span data-stu-id="4325c-183">Limits</span></span>
<span data-ttu-id="4325c-184">Omezení, které se vztahují tooapplication informace jsou v oblastech hello regulárních výrazů pro klíče a velikost.</span><span class="sxs-lookup"><span data-stu-id="4325c-184">Limits that apply tooapplication information are in hello areas of regular expressions for keys, and size.</span></span>

#### <a name="keys"></a><span data-ttu-id="4325c-185">Klíče</span><span class="sxs-lookup"><span data-stu-id="4325c-185">Keys</span></span>
<span data-ttu-id="4325c-186">Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="4325c-186">Each key in hello object must match hello following regular expression:</span></span>

    ^[a-zA-Z][a-zA-Z_0-9]*

<span data-ttu-id="4325c-187">To znamená, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="4325c-187">This means that keys must start with at least one letter, followed by letters, digits, or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="4325c-188">Velikost</span><span class="sxs-lookup"><span data-stu-id="4325c-188">Size</span></span>
<span data-ttu-id="4325c-189">Informace o aplikaci je omezená too1 024 znaků na jednu volání (po hello Mobile Engagement Web SDK kóduje ho ve formátu JSON).</span><span class="sxs-lookup"><span data-stu-id="4325c-189">Application information is limited too1,024 characters per call (after hello Mobile Engagement Web SDK encodes it in JSON).</span></span>

<span data-ttu-id="4325c-190">V předchozím příkladu hello hello JSON odeslané toohello serveru je 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="4325c-190">In hello preceding example, hello JSON sent toohello server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
