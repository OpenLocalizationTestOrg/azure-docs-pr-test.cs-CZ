---
title: "Jak používat rozhraní API Engagement v systému iOS"
description: "Nejnovější iOS SDK - použití rozhraní API Engagement v systému iOS"
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
ms.openlocfilehash: a31424da98205e97bdf57010cccfd044360f03dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-ios"></a><span data-ttu-id="95602-103">Jak používat rozhraní API Engagement v systému iOS</span><span class="sxs-lookup"><span data-stu-id="95602-103">How to Use the Engagement API on iOS</span></span>
<span data-ttu-id="95602-104">Tento dokument je doplněk k dokumentu jak integrovat Engagement v systému iOS: poskytuje hloubka podrobnosti o tom, jak použít rozhraní API Engagement sestavy statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="95602-104">This document is an add-on to the document How to Integrate Engagement on iOS: it provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="95602-105">Mějte na paměti, že pokud chcete pouze Engagement ohlásí aplikace relací, aktivity, dojde k chybě a technické informace, pak nejjednodušší způsob, jak je aby vaše vlastní `UIViewController` objekty dědí odpovídající `EngagementViewController` třídy.</span><span class="sxs-lookup"><span data-stu-id="95602-105">Keep in mind that if you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your custom `UIViewController` objects inherit from the corresponding `EngagementViewController` class.</span></span>

<span data-ttu-id="95602-106">Pokud chcete informace, například pokud je třeba ohlásit určité události aplikace, chyb a úlohy, nebo pokud máte k hlášení aktivitami aplikace jiným způsobem než ten, implementované v `EngagementViewController` třídy, pak budete muset použít rozhraní API zapojení.</span><span class="sxs-lookup"><span data-stu-id="95602-106">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementViewController` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="95602-107">Rozhraní API Engagement poskytuje `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="95602-107">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="95602-108">Instance této třídy může načíst volání `[EngagementAgent shared]` statickou metodu (Všimněte si, že `EngagementAgent` objekt vrácený je typu singleton).</span><span class="sxs-lookup"><span data-stu-id="95602-108">An instance of this class can be retrieved by calling the `[EngagementAgent shared]` static method (note that the `EngagementAgent` object returned is a singleton).</span></span>

<span data-ttu-id="95602-109">Před všechny volání rozhraní API `EngagementAgent` objekt je nutné inicializovat pomocí volání metody`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span><span class="sxs-lookup"><span data-stu-id="95602-109">Before any API calls, the `EngagementAgent` object must be initialized by calling the method `[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="95602-110">Koncepty engagementu</span><span class="sxs-lookup"><span data-stu-id="95602-110">Engagement concepts</span></span>
<span data-ttu-id="95602-111">Následující části Upřesnit nejběžnější [koncepty Mobile Engagementu](mobile-engagement-concepts.md) pro platformu iOS.</span><span class="sxs-lookup"><span data-stu-id="95602-111">The following parts refine the common [Mobile Engagement Concepts](mobile-engagement-concepts.md) for the iOS platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="95602-112">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="95602-112">`Session` and `Activity`</span></span>
<span data-ttu-id="95602-113">*Aktivity* je obvykle spojovány s jednu obrazovku aplikace, to znamená *aktivity* spustí, když se zobrazí na obrazovce a zastaví, když je uzavřený obrazovky: to je případ, kdy je sady Engagement SDK integrovaná pomocí `EngagementViewController` třídy.</span><span class="sxs-lookup"><span data-stu-id="95602-113">An *activity* is usually associated with one screen of the application, that is to say the *activity* starts when the screen is displayed and stops when the screen is closed: this is the case when the Engagement SDK is integrated by using the `EngagementViewController` classes.</span></span>

<span data-ttu-id="95602-114">Ale *aktivity* můžete ručně kontrolovat také pomocí rozhraní API zapojení.</span><span class="sxs-lookup"><span data-stu-id="95602-114">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="95602-115">To umožňuje rozdělit dané obrazovky v několik částí Sub – Chcete-li získat další podrobnosti o použití této obrazovky (například jak často známé a jak dlouho se používají dialogová okna v rámci této obrazovce).</span><span class="sxs-lookup"><span data-stu-id="95602-115">This allows to split a given screen in several sub parts to get more details about the usage of this screen (for example to known how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="95602-116">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="95602-116">Reporting Activities</span></span>
### <a name="user-starts-a-new-activity"></a><span data-ttu-id="95602-117">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="95602-117">User starts a new Activity</span></span>
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

<span data-ttu-id="95602-118">Je třeba volat `startActivity()` pokaždé, když změny aktivity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="95602-118">You need to call `startActivity()` each time the user activity changes.</span></span> <span data-ttu-id="95602-119">První volání této funkce spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="95602-119">The first call to this function starts a new user session.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="95602-120">Uživatel končí jeho aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="95602-120">User ends his current Activity</span></span>
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> <span data-ttu-id="95602-121">Měli byste **nikdy** volání této funkce samotnými, s výjimkou, pokud chcete rozdělit jedno použití aplikace do několik relací: volání této funkce by ukončení aktuální relace okamžitě, tedy následných volání `startActivity()` by zahájit novou relaci.</span><span class="sxs-lookup"><span data-stu-id="95602-121">You should **NEVER** call this function by yourself, except if you want to split one use of your application into several sessions: a call to this function would end the current session immediately, so, a subsequent call to `startActivity()` would start a new session.</span></span> <span data-ttu-id="95602-122">Tato funkce je automaticky volána sadou SDK, při zavření aplikace.</span><span class="sxs-lookup"><span data-stu-id="95602-122">This function is automatically called by the SDK when your application is closed.</span></span>
> 
> 

## <a name="reporting-events"></a><span data-ttu-id="95602-123">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="95602-123">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="95602-124">Události relací</span><span class="sxs-lookup"><span data-stu-id="95602-124">Session events</span></span>
<span data-ttu-id="95602-125">Relace události se obvykle používají k hlášení akcí prováděná uživatelem během jeho relace.</span><span class="sxs-lookup"><span data-stu-id="95602-125">Session events are usually used to report the actions performed by a user during his session.</span></span>

<span data-ttu-id="95602-126">**Příklad bez doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="95602-126">**Example without extra data:**</span></span>

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

<span data-ttu-id="95602-127">**Příklad s doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="95602-127">**Example with extra data:**</span></span>

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

### <a name="standalone-events"></a><span data-ttu-id="95602-128">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="95602-128">Standalone events</span></span>
<span data-ttu-id="95602-129">Rozporu s touto relace události lze použít samostatné události mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="95602-129">Contrary to session events, standalone events can be used outside of the context of a session.</span></span>

<span data-ttu-id="95602-130">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="95602-130">**Example:**</span></span>

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a><span data-ttu-id="95602-131">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="95602-131">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="95602-132">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="95602-132">Session errors</span></span>
<span data-ttu-id="95602-133">Relace chyby jsou obvykle používají k hlášení chyb během jeho relace, které mají vliv uživatele.</span><span class="sxs-lookup"><span data-stu-id="95602-133">Session errors are usually used to report the errors impacting the user during his session.</span></span>

<span data-ttu-id="95602-134">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="95602-134">**Example:**</span></span>

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a><span data-ttu-id="95602-135">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="95602-135">Standalone errors</span></span>
<span data-ttu-id="95602-136">Rozporu s touto relací chyby samostatné chyby lze mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="95602-136">Contrary to session errors, standalone errors can be used outside of the context of a session.</span></span>

<span data-ttu-id="95602-137">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="95602-137">**Example:**</span></span>

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a><span data-ttu-id="95602-138">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="95602-138">Reporting Jobs</span></span>
<span data-ttu-id="95602-139">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="95602-139">**Example:**</span></span>

<span data-ttu-id="95602-140">Předpokládejme, že chcete nahlásit trvání přihlašovací proces:</span><span class="sxs-lookup"><span data-stu-id="95602-140">Suppose you want to report the duration of your login process:</span></span>

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

### <a name="report-errors-during-a-job"></a><span data-ttu-id="95602-141">Sestava chyb během úlohy</span><span class="sxs-lookup"><span data-stu-id="95602-141">Report Errors during a Job</span></span>
<span data-ttu-id="95602-142">Chyby může souviset s probíhající úlohou místo má vztah k aktuální uživatelskou relaci.</span><span class="sxs-lookup"><span data-stu-id="95602-142">Errors can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="95602-143">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="95602-143">**Example:**</span></span>

<span data-ttu-id="95602-144">Předpokládejme, že chcete vykázat chybu během procesu přihlášení:</span><span class="sxs-lookup"><span data-stu-id="95602-144">Suppose you want to report an error during your login process:</span></span>

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
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

### <a name="events-during-a-job"></a><span data-ttu-id="95602-145">Události během úlohy</span><span class="sxs-lookup"><span data-stu-id="95602-145">Events during a job</span></span>
<span data-ttu-id="95602-146">Události může souviset s probíhající úlohou místo má vztah k aktuální uživatelskou relaci.</span><span class="sxs-lookup"><span data-stu-id="95602-146">Events can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="95602-147">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="95602-147">**Example:**</span></span>

<span data-ttu-id="95602-148">Předpokládejme, že máme sociálních sítí, a úlohu do sestavy používáme celkovou dobu, během kterého uživatel se připojí k serveru.</span><span class="sxs-lookup"><span data-stu-id="95602-148">Suppose we have a social network, and we use a job to report the total time during which the user is connected to the server.</span></span> <span data-ttu-id="95602-149">Uživatel může přijímat zprávy z jeho přátel, jedná se o událost úlohy.</span><span class="sxs-lookup"><span data-stu-id="95602-149">The user can receive messages from his friends, this is a job event.</span></span>

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

## <a name="extra-parameters"></a><span data-ttu-id="95602-150">Další parametry</span><span class="sxs-lookup"><span data-stu-id="95602-150">Extra parameters</span></span>
<span data-ttu-id="95602-151">Libovolná data lze připojit k události, chyb, aktivity a úlohy.</span><span class="sxs-lookup"><span data-stu-id="95602-151">Arbitrary data can be attached to events, errors, activities and jobs.</span></span>

<span data-ttu-id="95602-152">Tato data mohou být strukturovaná, používá třída NSDictionary pro iOS.</span><span class="sxs-lookup"><span data-stu-id="95602-152">This data can be structured, it uses iOS's NSDictionary class.</span></span>

<span data-ttu-id="95602-153">Všimněte si, že funkce může obsahovat `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` či jiné `NSDictionary` instance.</span><span class="sxs-lookup"><span data-stu-id="95602-153">Note that extras can contain `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` or other `NSDictionary` instances.</span></span>

> [!NOTE]
> <span data-ttu-id="95602-154">Speciálním parametrem je serializováno ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="95602-154">The extra parameter is serialized in JSON.</span></span> <span data-ttu-id="95602-155">Pokud chcete předat různé objekty než ty, které jsou popsané výše, je nutné implementovat metodu v třídě:</span><span class="sxs-lookup"><span data-stu-id="95602-155">If you want to pass different objects than the ones described above, you must implement the following method in your class:</span></span>
> 
> <span data-ttu-id="95602-156">-(NSString*) JSONRepresentation;</span><span class="sxs-lookup"><span data-stu-id="95602-156">-(NSString*)JSONRepresentation;</span></span>
> 
> <span data-ttu-id="95602-157">Metoda by měla vrátit reprezentaci JSON objektu.</span><span class="sxs-lookup"><span data-stu-id="95602-157">The method should return a JSON representation of your object.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="95602-158">Příklad</span><span class="sxs-lookup"><span data-stu-id="95602-158">Example</span></span>
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a><span data-ttu-id="95602-159">Omezení</span><span class="sxs-lookup"><span data-stu-id="95602-159">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="95602-160">Klíče</span><span class="sxs-lookup"><span data-stu-id="95602-160">Keys</span></span>
<span data-ttu-id="95602-161">Každý klíč v `NSDictionary` musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="95602-161">Each key in the `NSDictionary` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="95602-162">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="95602-162">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="95602-163">Velikost</span><span class="sxs-lookup"><span data-stu-id="95602-163">Size</span></span>
<span data-ttu-id="95602-164">Funkce jsou omezeny na **1024** znaků na jednu volání (po zakódování ve formátu JSON pomocí agenta Engagement).</span><span class="sxs-lookup"><span data-stu-id="95602-164">Extras are limited to **1024** characters per call (once encoded in JSON by the Engagement agent).</span></span>

<span data-ttu-id="95602-165">V předchozím příkladu je JSON odeslat na server 58 znaků:</span><span class="sxs-lookup"><span data-stu-id="95602-165">In the previous example, the JSON sent to the server is 58 characters long:</span></span>

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="95602-166">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="95602-166">Reporting Application Information</span></span>
<span data-ttu-id="95602-167">Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí `sendAppInfo:` funkce.</span><span class="sxs-lookup"><span data-stu-id="95602-167">You can manually report tracking information (or any other application specific information) using the `sendAppInfo:` function.</span></span>

<span data-ttu-id="95602-168">Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.</span><span class="sxs-lookup"><span data-stu-id="95602-168">Note that these information can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="95602-169">Události funkce, jako `NSDictionary` třída se používá k abstraktní informace o aplikaci, Všimněte si, že pole nebo dílčí slovník bude považována za plochý řetězce (pomocí serializace JSON).</span><span class="sxs-lookup"><span data-stu-id="95602-169">Like event extras, the `NSDictionary` class is used to abstract application information, note that arrays or sub-dictionaries will be treated as flat strings (using JSON serialization).</span></span>

<span data-ttu-id="95602-170">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="95602-170">**Example:**</span></span>

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a><span data-ttu-id="95602-171">Omezení</span><span class="sxs-lookup"><span data-stu-id="95602-171">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="95602-172">Klíče</span><span class="sxs-lookup"><span data-stu-id="95602-172">Keys</span></span>
<span data-ttu-id="95602-173">Každý klíč v `NSDictionary` musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="95602-173">Each key in the `NSDictionary` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="95602-174">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="95602-174">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="95602-175">Velikost</span><span class="sxs-lookup"><span data-stu-id="95602-175">Size</span></span>
<span data-ttu-id="95602-176">Informace o aplikaci jsou omezeny na **1024** znaků na jednu volání (po zakódování ve formátu JSON pomocí agenta Engagement).</span><span class="sxs-lookup"><span data-stu-id="95602-176">Application information are limited to **1024** characters per call (once encoded in JSON by the Engagement agent).</span></span>

<span data-ttu-id="95602-177">V předchozím příkladu je JSON odeslat na server 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="95602-177">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

    {"birthdate":"1983-12-07","gender":"female"}
