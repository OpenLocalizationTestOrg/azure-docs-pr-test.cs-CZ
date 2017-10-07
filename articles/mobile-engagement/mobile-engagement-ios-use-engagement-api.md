---
title: "aaaHow tooUse hello Engagement rozhraní API v systému iOS"
description: "Nejnovější iOS SDK – jak tooUse hello Engagement rozhraní API v systému iOS"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1fb4509e-3804-46c1-949f-1cf727f91f9f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7fb9b95ad319cf3b1e2de81b5d6aee5b30266069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-ios"></a><span data-ttu-id="818ad-103">Jak tooUse hello Engagement rozhraní API v systému iOS</span><span class="sxs-lookup"><span data-stu-id="818ad-103">How tooUse hello Engagement API on iOS</span></span>
<span data-ttu-id="818ad-104">Tento dokument je dokument toohello rozšíření jak tooIntegrate Engagement v systému iOS: poskytuje v hloubka podrobnosti o tom, jak toouse hello Engagement API tooreport statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="818ad-104">This document is an add-on toohello document How tooIntegrate Engagement on iOS: it provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="818ad-105">Mějte na paměti, pouze pokud chcete tooreport zapojení vaší aplikace relací, aktivit, dojde k chybě a technické informace, pak hello nejjednodušší způsob je toomake všechny vaše vlastní `UIViewController` objekty dědí hello odpovídající `EngagementViewController` – třída .</span><span class="sxs-lookup"><span data-stu-id="818ad-105">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your custom `UIViewController` objects inherit from hello corresponding `EngagementViewController` class.</span></span>

<span data-ttu-id="818ad-106">Pokud chcete, aby toodo další, např. Pokud potřebujete tooreport aplikace konkrétní události, chyb a úlohy, nebo pokud máte tooreport aktivitami aplikace jiným způsobem než jednu implementaci hello hello `EngagementViewController` třídy, pak je nutné toouse hello Zapojení rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="818ad-106">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementViewController` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="818ad-107">Hello rozhraní API Engagement poskytuje hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="818ad-107">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="818ad-108">Instance této třídy může načíst volání hello `[EngagementAgent shared]` statickou metodu (Všimněte si, že hello `EngagementAgent` objekt vrácený je typu singleton).</span><span class="sxs-lookup"><span data-stu-id="818ad-108">An instance of this class can be retrieved by calling hello `[EngagementAgent shared]` static method (note that hello `EngagementAgent` object returned is a singleton).</span></span>

<span data-ttu-id="818ad-109">Předtím, než všechny volání rozhraní API, hello `EngagementAgent` objekt je nutné inicializovat voláním metody hello`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span><span class="sxs-lookup"><span data-stu-id="818ad-109">Before any API calls, hello `EngagementAgent` object must be initialized by calling hello method `[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="818ad-110">Koncepty engagementu</span><span class="sxs-lookup"><span data-stu-id="818ad-110">Engagement concepts</span></span>
<span data-ttu-id="818ad-111">Hello následujících částí Upřesnit hello běžné [koncepty Mobile Engagementu](mobile-engagement-concepts.md) pro platformu iOS hello.</span><span class="sxs-lookup"><span data-stu-id="818ad-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for hello iOS platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="818ad-112">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="818ad-112">`Session` and `Activity`</span></span>
<span data-ttu-id="818ad-113">*Aktivity* obvykle souvisí s jeden obrazovky aplikace hello, který je toosay hello *aktivity* spustí, když se zobrazí úvodní obrazovka a zastaví, když je uzavřený úvodní obrazovka: Toto je hello případ, kdy Hello Engagement SDK je integrovaná s použitím hello `EngagementViewController` třídy.</span><span class="sxs-lookup"><span data-stu-id="818ad-113">An *activity* is usually associated with one screen of hello application, that is toosay hello *activity* starts when hello screen is displayed and stops when hello screen is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementViewController` classes.</span></span>

<span data-ttu-id="818ad-114">Ale *aktivity* můžete ručně kontrolovat také pomocí hello Engagement rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="818ad-114">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="818ad-115">To umožňuje toosplit dané obrazovky v několika dílčí části tooget hello další podrobnosti o použití této obrazovce (například jak často tooknown a jak dlouho se používají dialogová okna v rámci této obrazovce).</span><span class="sxs-lookup"><span data-stu-id="818ad-115">This allows toosplit a given screen in several sub parts tooget more details about hello usage of this screen (for example tooknown how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="818ad-116">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="818ad-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="818ad-117">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="818ad-117">User starts a new Activity</span></span>
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

<span data-ttu-id="818ad-118">Je třeba toocall `startActivity()` Každá aktivita uživatele hello čas změny.</span><span class="sxs-lookup"><span data-stu-id="818ad-118">You need toocall `startActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="818ad-119">Hello první volání funkce toothis spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="818ad-119">hello first call toothis function starts a new user session.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="818ad-120">Uživatel končí jeho aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="818ad-120">User ends his current Activity</span></span>
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> <span data-ttu-id="818ad-121">Měli byste **nikdy** volání této funkce samotnými, s výjimkou, pokud chcete, aby toosplit jedno použití aplikace do několik relací: volání funkce toothis by ale ukončilo hello okamžitě, aktuální relace tak následných volání příliš`startActivity()`by zahájit novou relaci.</span><span class="sxs-lookup"><span data-stu-id="818ad-121">You should **NEVER** call this function by yourself, except if you want toosplit one use of your application into several sessions: a call toothis function would end hello current session immediately, so, a subsequent call too`startActivity()` would start a new session.</span></span> <span data-ttu-id="818ad-122">Tato funkce je automaticky volána hello SDK při zavření aplikace.</span><span class="sxs-lookup"><span data-stu-id="818ad-122">This function is automatically called by hello SDK when your application is closed.</span></span>
> 
> 

## <a name="reporting-events"></a><span data-ttu-id="818ad-123">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="818ad-123">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="818ad-124">Události relací</span><span class="sxs-lookup"><span data-stu-id="818ad-124">Session events</span></span>
<span data-ttu-id="818ad-125">Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během jeho relace.</span><span class="sxs-lookup"><span data-stu-id="818ad-125">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

<span data-ttu-id="818ad-126">**Příklad bez doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="818ad-126">**Example without extra data:**</span></span>

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

<span data-ttu-id="818ad-127">**Příklad s doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="818ad-127">**Example with extra data:**</span></span>

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a><span data-ttu-id="818ad-128">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="818ad-128">Standalone events</span></span>
<span data-ttu-id="818ad-129">Jinak zvláštní toosession události, samostatné události lze použít mimo kontext hello relace.</span><span class="sxs-lookup"><span data-stu-id="818ad-129">Contrary toosession events, standalone events can be used outside of hello context of a session.</span></span>

<span data-ttu-id="818ad-130">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="818ad-130">**Example:**</span></span>

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a><span data-ttu-id="818ad-131">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="818ad-131">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="818ad-132">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="818ad-132">Session errors</span></span>
<span data-ttu-id="818ad-133">Relace chyby jsou obvykle použité tooreport hello chyby během jeho relace, které mají vliv hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="818ad-133">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

<span data-ttu-id="818ad-134">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="818ad-134">**Example:**</span></span>

    /** hello user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* hello user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="818ad-135">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="818ad-135">Standalone errors</span></span>
<span data-ttu-id="818ad-136">Jinak zvláštní toosession chyb, chyb samostatné lze použít mimo kontext hello relace.</span><span class="sxs-lookup"><span data-stu-id="818ad-136">Contrary toosession errors, standalone errors can be used outside of hello context of a session.</span></span>

<span data-ttu-id="818ad-137">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="818ad-137">**Example:**</span></span>

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a><span data-ttu-id="818ad-138">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="818ad-138">Reporting Jobs</span></span>
<span data-ttu-id="818ad-139">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="818ad-139">**Example:**</span></span>

<span data-ttu-id="818ad-140">Předpokládejme, že chcete tooreport hello trvání přihlašovací proces:</span><span class="sxs-lookup"><span data-stu-id="818ad-140">Suppose you want tooreport hello duration of your login process:</span></span>

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a><span data-ttu-id="818ad-141">Sestava chyb během úlohy</span><span class="sxs-lookup"><span data-stu-id="818ad-141">Report Errors during a Job</span></span>
<span data-ttu-id="818ad-142">Chyby může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.</span><span class="sxs-lookup"><span data-stu-id="818ad-142">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="818ad-143">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="818ad-143">**Example:**</span></span>

<span data-ttu-id="818ad-144">Předpokládejme, že chcete tooreport chybu během procesu přihlášení:</span><span class="sxs-lookup"><span data-stu-id="818ad-144">Suppose you want tooreport an error during your login process:</span></span>

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try toosign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a><span data-ttu-id="818ad-145">Události během úlohy</span><span class="sxs-lookup"><span data-stu-id="818ad-145">Events during a job</span></span>
<span data-ttu-id="818ad-146">Události může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.</span><span class="sxs-lookup"><span data-stu-id="818ad-146">Events can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="818ad-147">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="818ad-147">**Example:**</span></span>

<span data-ttu-id="818ad-148">Předpokládejme, že máme sociálních sítí a používáme úlohy tooreport hello celkový čas během které hello je uživatel připojený toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="818ad-148">Suppose we have a social network, and we use a job tooreport hello total time during which hello user is connected toohello server.</span></span> <span data-ttu-id="818ad-149">Hello uživatel může přijímat zprávy z jeho přátel, jedná se o událost úlohy.</span><span class="sxs-lookup"><span data-stu-id="818ad-149">hello user can receive messages from his friends, this is a job event.</span></span>

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

## <a name="extra-parameters"></a><span data-ttu-id="818ad-150">Další parametry</span><span class="sxs-lookup"><span data-stu-id="818ad-150">Extra parameters</span></span>
<span data-ttu-id="818ad-151">Libovolná data může být připojené tooevents, chyb, aktivity a úlohy.</span><span class="sxs-lookup"><span data-stu-id="818ad-151">Arbitrary data can be attached tooevents, errors, activities and jobs.</span></span>

<span data-ttu-id="818ad-152">Tato data mohou být strukturovaná, používá třída NSDictionary pro iOS.</span><span class="sxs-lookup"><span data-stu-id="818ad-152">This data can be structured, it uses iOS's NSDictionary class.</span></span>

<span data-ttu-id="818ad-153">Všimněte si, že funkce může obsahovat `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` či jiné `NSDictionary` instance.</span><span class="sxs-lookup"><span data-stu-id="818ad-153">Note that extras can contain `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` or other `NSDictionary` instances.</span></span>

> [!NOTE]
> <span data-ttu-id="818ad-154">Hello speciálním parametrem je serializováno ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="818ad-154">hello extra parameter is serialized in JSON.</span></span> <span data-ttu-id="818ad-155">Pokud chcete různé objekty toopass než hello ty, které jsou popsané výše, je nutné implementovat následující metodu v třídě hello:</span><span class="sxs-lookup"><span data-stu-id="818ad-155">If you want toopass different objects than hello ones described above, you must implement hello following method in your class:</span></span>
> 
> <span data-ttu-id="818ad-156">-(NSString*) JSONRepresentation;</span><span class="sxs-lookup"><span data-stu-id="818ad-156">-(NSString*)JSONRepresentation;</span></span>
> 
> <span data-ttu-id="818ad-157">Hello metoda by měla vrátit reprezentaci JSON objektu.</span><span class="sxs-lookup"><span data-stu-id="818ad-157">hello method should return a JSON representation of your object.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="818ad-158">Příklad</span><span class="sxs-lookup"><span data-stu-id="818ad-158">Example</span></span>
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a><span data-ttu-id="818ad-159">Omezení</span><span class="sxs-lookup"><span data-stu-id="818ad-159">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="818ad-160">Klíče</span><span class="sxs-lookup"><span data-stu-id="818ad-160">Keys</span></span>
<span data-ttu-id="818ad-161">Každý klíč v hello `NSDictionary` musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="818ad-161">Each key in hello `NSDictionary` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="818ad-162">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="818ad-162">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="818ad-163">Velikost</span><span class="sxs-lookup"><span data-stu-id="818ad-163">Size</span></span>
<span data-ttu-id="818ad-164">Funkce omezeny příliš**1024** znaků na jednu volání (po zakódování ve formátu JSON hello Engagement agenta).</span><span class="sxs-lookup"><span data-stu-id="818ad-164">Extras are limited too**1024** characters per call (once encoded in JSON by hello Engagement agent).</span></span>

<span data-ttu-id="818ad-165">Předchozí příklad, hello JSON odeslán toohello serveru v hello je 58 znaků:</span><span class="sxs-lookup"><span data-stu-id="818ad-165">In hello previous example, hello JSON sent toohello server is 58 characters long:</span></span>

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="818ad-166">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="818ad-166">Reporting Application Information</span></span>
<span data-ttu-id="818ad-167">Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí hello `sendAppInfo:` funkce.</span><span class="sxs-lookup"><span data-stu-id="818ad-167">You can manually report tracking information (or any other application specific information) using hello `sendAppInfo:` function.</span></span>

<span data-ttu-id="818ad-168">Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze hello nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.</span><span class="sxs-lookup"><span data-stu-id="818ad-168">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="818ad-169">Jako funkce událostí hello `NSDictionary` třída je použité tooabstract informace o aplikaci, Všimněte si, že maticových nebo dílčí slovníky, budou považovány za plochý řetězce (pomocí serializace JSON).</span><span class="sxs-lookup"><span data-stu-id="818ad-169">Like event extras, hello `NSDictionary` class is used tooabstract application information, note that arrays or sub-dictionaries will be treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="818ad-170">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="818ad-170">**Example:**</span></span>

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a><span data-ttu-id="818ad-171">Omezení</span><span class="sxs-lookup"><span data-stu-id="818ad-171">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="818ad-172">Klíče</span><span class="sxs-lookup"><span data-stu-id="818ad-172">Keys</span></span>
<span data-ttu-id="818ad-173">Každý klíč v hello `NSDictionary` musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="818ad-173">Each key in hello `NSDictionary` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="818ad-174">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="818ad-174">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="818ad-175">Velikost</span><span class="sxs-lookup"><span data-stu-id="818ad-175">Size</span></span>
<span data-ttu-id="818ad-176">Informace o aplikaci jsou omezené příliš**1024** znaků na jednu volání (po zakódování ve formátu JSON hello Engagement agenta).</span><span class="sxs-lookup"><span data-stu-id="818ad-176">Application information are limited too**1024** characters per call (once encoded in JSON by hello Engagement agent).</span></span>

<span data-ttu-id="818ad-177">Předchozí příklad, hello JSON odeslán toohello serveru v hello je 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="818ad-177">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
