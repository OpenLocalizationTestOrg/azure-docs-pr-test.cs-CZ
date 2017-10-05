---
title: "Jak používat Engagement rozhraní API v systému Android"
description: "Nejnovější Android SDK – jak používat Engagement rozhraní API v systému Android"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 09b62659-82ae-4a55-8784-fca0b6b22eaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: d353cd2fe47c54a0282cc5bb1b22b4a56e0cd82c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-android"></a><span data-ttu-id="be6dc-103">Jak používat Engagement rozhraní API v systému Android</span><span class="sxs-lookup"><span data-stu-id="be6dc-103">How to Use the Engagement API on Android</span></span>
<span data-ttu-id="be6dc-104">Tento dokument je doplněk k dokumentu [Reporting rozšířené možnosti pro Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="be6dc-104">This document is an add-on to the document [Advanced Reporting options for Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span></span> <span data-ttu-id="be6dc-105">Poskytuje hloubka podrobnosti o tom, jak použít rozhraní API Engagement sestavy statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="be6dc-105">It provides in depth details about how to use the Engagement API to report your application statistics.</span></span>

<span data-ttu-id="be6dc-106">Mějte na paměti, že pokud chcete pouze Engagement ohlásí aplikace relací, aktivity, dojde k chybě a technické informace, pak nejjednodušší způsob, jak se zajistit všechny vaše `Activity` dílčí třídy dědí odpovídající `EngagementActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="be6dc-106">Keep in mind that if you only want Engagement to report your application's sessions, activities, crashes and technical information, then the simplest way is to make all your `Activity` sub-classes inherit from the corresponding `EngagementActivity` class.</span></span>

<span data-ttu-id="be6dc-107">Pokud chcete informace, například pokud je třeba ohlásit určité události aplikace, chyb a úlohy, nebo pokud máte k hlášení aktivitami aplikace jiným způsobem než ten, implementované v `EngagementActivity` třídy, pak budete muset použít rozhraní API zapojení.</span><span class="sxs-lookup"><span data-stu-id="be6dc-107">If you want to do more, for example if you need to report application specific events, errors and jobs, or if you have to report your application's activities in a different way than the one implemented in the `EngagementActivity` classes, then you need to use the Engagement API.</span></span>

<span data-ttu-id="be6dc-108">Rozhraní API Engagement poskytuje `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="be6dc-108">The Engagement API is provided by the `EngagementAgent` class.</span></span> <span data-ttu-id="be6dc-109">Instance této třídy může načíst volání `EngagementAgent.getInstance(Context)` statickou metodu (Všimněte si, že `EngagementAgent` objekt vrácený je typu singleton).</span><span class="sxs-lookup"><span data-stu-id="be6dc-109">An instance of this class can be retrieved by calling the `EngagementAgent.getInstance(Context)` static method (note that the `EngagementAgent` object returned is a singleton).</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="be6dc-110">Koncepty engagementu</span><span class="sxs-lookup"><span data-stu-id="be6dc-110">Engagement concepts</span></span>
<span data-ttu-id="be6dc-111">Následující části Upřesnit nejběžnější [koncepty Mobile Engagementu](mobile-engagement-concepts.md), pro platformu Android.</span><span class="sxs-lookup"><span data-stu-id="be6dc-111">The following parts refine the common [Mobile Engagement Concepts](mobile-engagement-concepts.md), for the Android platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="be6dc-112">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="be6dc-112">`Session` and `Activity`</span></span>
<span data-ttu-id="be6dc-113">Je-li uživatel více než pár sekundách nečinnosti mezi dvěma *aktivity*, potom jeho posloupnost *aktivity* je rozdělit na dvě odlišné *relací*.</span><span class="sxs-lookup"><span data-stu-id="be6dc-113">If the user stays more than a few seconds idle between two *activities*, then his sequence of *activities* is split in two distinct *sessions*.</span></span> <span data-ttu-id="be6dc-114">Tyto několik sekund, se nazývají "časový limit relace".</span><span class="sxs-lookup"><span data-stu-id="be6dc-114">These few seconds are called the "session timeout".</span></span>

<span data-ttu-id="be6dc-115">*Aktivity* je obvykle spojovány s jednu obrazovku aplikace, to znamená *aktivity* spustí, když se zobrazí na obrazovce a zastaví, když je uzavřený obrazovky: to je případ, kdy je sady Engagement SDK integrovaná pomocí `EngagementActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="be6dc-115">An *activity* is usually associated with one screen of the application, that is to say the *activity* starts when the screen is displayed and stops when the screen is closed: this is the case when the Engagement SDK is integrated by using the `EngagementActivity` classes.</span></span>

<span data-ttu-id="be6dc-116">Ale *aktivity* můžete ručně kontrolovat také pomocí rozhraní API zapojení.</span><span class="sxs-lookup"><span data-stu-id="be6dc-116">But *activities* can also be controlled manually by using the Engagement API.</span></span> <span data-ttu-id="be6dc-117">To umožňuje rozdělit dané obrazovky v několik částí Sub – Chcete-li získat další podrobnosti o použití této obrazovky (například jak často známé a jak dlouho se používají dialogová okna v rámci této obrazovce).</span><span class="sxs-lookup"><span data-stu-id="be6dc-117">This allows to split a given screen in several sub parts to get more details about the usage of this screen (for example to known how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="be6dc-118">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="be6dc-118">Reporting Activities</span></span>
> [!IMPORTANT]
> <span data-ttu-id="be6dc-119">Nemusíte aktivity, například sestavy popsané v této části, pokud používáte `EngagementActivity` třídy a jeho variant, jak je popsáno v tom, jak integrovat Engagement Android dokumentu.</span><span class="sxs-lookup"><span data-stu-id="be6dc-119">You don't need to report activities like described in this section if you are using the `EngagementActivity` class and its variants as explained in the How to Integrate Engagement on Android document.</span></span>
> 
> 

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="be6dc-120">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="be6dc-120">User starts a new Activity</span></span>
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

<span data-ttu-id="be6dc-121">Je třeba volat `startActivity()` pokaždé, když změny aktivity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="be6dc-121">You need to call `startActivity()` each time the user activity changes.</span></span> <span data-ttu-id="be6dc-122">První volání této funkce spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="be6dc-122">The first call to this function starts a new user session.</span></span>

<span data-ttu-id="be6dc-123">Nejlepší místo k volání této funkce je v každé aktivity `onResume` zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="be6dc-123">The best place to call this function is on each activity `onResume` callback.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="be6dc-124">Uživatel končí jeho aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="be6dc-124">User ends his current Activity</span></span>
            EngagementAgent.getInstance(this).endActivity();

<span data-ttu-id="be6dc-125">Je třeba volat `endActivity()` alespoň jednou po dokončení uživatele poslední aktivita.</span><span class="sxs-lookup"><span data-stu-id="be6dc-125">You need to call `endActivity()` at least once when the user finishes his last activity.</span></span> <span data-ttu-id="be6dc-126">Tento příkaz informuje sady Engagement SDK, že je uživatel aktuálně nečinnosti, a že relaci uživatele, který je třeba ukončit jednou časový limit relace vyprší (při volání `startActivity()` předtím, než vyprší časový limit relace, relaci jednoduše obnovit).</span><span class="sxs-lookup"><span data-stu-id="be6dc-126">This informs the Engagement SDK that the user is currently idle, and that the user session need to be closed once the session timeout will expire (if you call `startActivity()` before the session timeout expires, the session is simply resumed).</span></span>

<span data-ttu-id="be6dc-127">Nejlepší místo k volání této funkce je v každé aktivity `onPause` zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="be6dc-127">The best place to call this function is on each activity `onPause` callback.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="be6dc-128">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="be6dc-128">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="be6dc-129">Události relací</span><span class="sxs-lookup"><span data-stu-id="be6dc-129">Session events</span></span>
<span data-ttu-id="be6dc-130">Relace události se obvykle používají k hlášení akcí prováděná uživatelem během jeho relace.</span><span class="sxs-lookup"><span data-stu-id="be6dc-130">Session events are usually used to report the actions performed by a user during his session.</span></span>

<span data-ttu-id="be6dc-131">**Příklad bez doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="be6dc-131">**Example without extra data:**</span></span>

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

<span data-ttu-id="be6dc-132">**Příklad s doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="be6dc-132">**Example with extra data:**</span></span>

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a><span data-ttu-id="be6dc-133">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="be6dc-133">Standalone Events</span></span>
<span data-ttu-id="be6dc-134">Rozporu s touto relace události může dojít k událostem samostatné mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="be6dc-134">Contrary to session events, standalone events can occur outside of the context of a session.</span></span>

<span data-ttu-id="be6dc-135">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="be6dc-135">**Example:**</span></span>

<span data-ttu-id="be6dc-136">Předpokládejme, že chcete do sestavy událostí, ke kterým dochází při aktivaci všesměrového vysílání příjemce:</span><span class="sxs-lookup"><span data-stu-id="be6dc-136">Suppose you want to report events occurring when a broadcast receiver is triggered:</span></span>

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a><span data-ttu-id="be6dc-137">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="be6dc-137">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="be6dc-138">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="be6dc-138">Session errors</span></span>
<span data-ttu-id="be6dc-139">Relace chyby jsou obvykle používají k hlášení chyb během jeho relace, které mají vliv uživatele.</span><span class="sxs-lookup"><span data-stu-id="be6dc-139">Session errors are usually used to report the errors impacting the user during his session.</span></span>

<span data-ttu-id="be6dc-140">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="be6dc-140">**Example:**</span></span>

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a><span data-ttu-id="be6dc-141">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="be6dc-141">Standalone errors</span></span>
<span data-ttu-id="be6dc-142">Rozporu s touto relací chyb může dojít k chybám samostatné mimo kontext relace.</span><span class="sxs-lookup"><span data-stu-id="be6dc-142">Contrary to session errors, standalone errors can occur outside of the context of a session.</span></span>

<span data-ttu-id="be6dc-143">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="be6dc-143">**Example:**</span></span>

<span data-ttu-id="be6dc-144">Následující příklad ukazuje, jak chcete sestavu chybu vždy, když velikost paměti je nedostatek v telefonu během procesu vaše aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="be6dc-144">The following example shows how to report an error whenever the memory becomes low on the phone while your application process is running.</span></span>

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a><span data-ttu-id="be6dc-145">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="be6dc-145">Reporting Jobs</span></span>
### <a name="example"></a><span data-ttu-id="be6dc-146">Příklad</span><span class="sxs-lookup"><span data-stu-id="be6dc-146">Example</span></span>
<span data-ttu-id="be6dc-147">Předpokládejme, že chcete nahlásit trvání přihlašovací proces:</span><span class="sxs-lookup"><span data-stu-id="be6dc-147">Suppose you want to report the duration of your login process:</span></span>

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a><span data-ttu-id="be6dc-148">Sestava chyb během úlohy</span><span class="sxs-lookup"><span data-stu-id="be6dc-148">Report Errors during a Job</span></span>
<span data-ttu-id="be6dc-149">Chyby může souviset s probíhající úlohou místo má vztah k aktuální uživatelskou relaci.</span><span class="sxs-lookup"><span data-stu-id="be6dc-149">Errors can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="be6dc-150">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="be6dc-150">**Example:**</span></span>

<span data-ttu-id="be6dc-151">Předpokládejme, že chcete proces přihlášení k chybě během můžete sestavy:</span><span class="sxs-lookup"><span data-stu-id="be6dc-151">Suppose you want to report an error during you login process:</span></span>

<span data-ttu-id="be6dc-152">[...] veřejné přihlášení void (kontextu kontextu,...) {</span><span class="sxs-lookup"><span data-stu-id="be6dc-152">[...] public void signIn(Context context, ...) {</span></span>

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="be6dc-153">Události vytváření sestav během úlohy</span><span class="sxs-lookup"><span data-stu-id="be6dc-153">Reporting Events during a job</span></span>
<span data-ttu-id="be6dc-154">Události může souviset s probíhající úlohou místo má vztah k aktuální uživatelskou relaci.</span><span class="sxs-lookup"><span data-stu-id="be6dc-154">Events can be related to a running job instead of being related to the current user session.</span></span>

<span data-ttu-id="be6dc-155">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="be6dc-155">**Example:**</span></span>

<span data-ttu-id="be6dc-156">Předpokládejme, že máme sociálních sítí, a úlohu do sestavy používáme celkovou dobu, během kterého uživatel se připojí k serveru.</span><span class="sxs-lookup"><span data-stu-id="be6dc-156">Suppose we have a social network, and we use a job to report the total time during which the user is connected to the server.</span></span> <span data-ttu-id="be6dc-157">Uživatele můžete zůstat připojeni pozadí i když mu používá jiná aplikace nebo když je v režimu spánku telefonu, takže není žádná relace.</span><span class="sxs-lookup"><span data-stu-id="be6dc-157">The user can stay connected in background even when he's using another application or when the phone is sleeping, so there is no session.</span></span>

<span data-ttu-id="be6dc-158">Uživatel může přijímat zprávy z jeho přátel, jedná se o událost úlohy.</span><span class="sxs-lookup"><span data-stu-id="be6dc-158">The user can receive messages from his friends, this is a job event.</span></span>

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

## <a name="extra-parameters"></a><span data-ttu-id="be6dc-159">Další parametry</span><span class="sxs-lookup"><span data-stu-id="be6dc-159">Extra parameters</span></span>
<span data-ttu-id="be6dc-160">Libovolná data lze připojit k události, chyb, aktivity a úlohy.</span><span class="sxs-lookup"><span data-stu-id="be6dc-160">Arbitrary data can be attached to events, errors, activities and jobs.</span></span>

<span data-ttu-id="be6dc-161">Tato data mohou být strukturovaná, používá třída sady pro Android (ve skutečnosti, funguje jako další parametry v Android záměry).</span><span class="sxs-lookup"><span data-stu-id="be6dc-161">This data can be structured, it uses Android's Bundle class (actually, it works like extra parameters in Android Intents).</span></span> <span data-ttu-id="be6dc-162">Všimněte si, že sady může obsahovat pole nebo jiné sady instancí.</span><span class="sxs-lookup"><span data-stu-id="be6dc-162">Note that a Bundle can contain arrays or another Bundle instances.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be6dc-163">Ujistěte se, když vložíte v parametrech parcelable nebo serializable, jejich `toString()` je implementována metoda vrátit čitelná pro člověka řetězec.</span><span class="sxs-lookup"><span data-stu-id="be6dc-163">If you put in parcelable or serializable parameters, make sure their `toString()` method is implemented to return a human-readable string.</span></span> <span data-ttu-id="be6dc-164">Serializovatelné třídy, které obsahovat jiné přechodné pole, které nejsou serializovatelný provede Android havárií, když budete volat`bundle.putSerializable("key",value);`</span><span class="sxs-lookup"><span data-stu-id="be6dc-164">Serializable classes that contain non transient fields that are not serializable will make Android crash when you will call `bundle.putSerializable("key",value);`</span></span>
> 
> [!WARNING]
> <span data-ttu-id="be6dc-165">Zhuštěný pole v další parametry nejsou podporovány, to znamená, nebude možné serializovat jako pole.</span><span class="sxs-lookup"><span data-stu-id="be6dc-165">Sparse arrays in extra parameters are not supported, that is, it won't be serialized as an array.</span></span> <span data-ttu-id="be6dc-166">Je třeba je převést na standardní pole před použitím v další parametry.</span><span class="sxs-lookup"><span data-stu-id="be6dc-166">You should convert them into standard arrays before using it in extra parameters.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="be6dc-167">Příklad</span><span class="sxs-lookup"><span data-stu-id="be6dc-167">Example</span></span>
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="be6dc-168">Omezení</span><span class="sxs-lookup"><span data-stu-id="be6dc-168">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="be6dc-169">Klíče</span><span class="sxs-lookup"><span data-stu-id="be6dc-169">Keys</span></span>
<span data-ttu-id="be6dc-170">Každý klíč v `Bundle` musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="be6dc-170">Each key in the `Bundle` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="be6dc-171">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="be6dc-171">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="be6dc-172">Velikost</span><span class="sxs-lookup"><span data-stu-id="be6dc-172">Size</span></span>
<span data-ttu-id="be6dc-173">Funkce jsou omezeny na **1024** znaků na jednu volání (po zakódování ve formátu JSON pomocí služby zapojení).</span><span class="sxs-lookup"><span data-stu-id="be6dc-173">Extras are limited to **1024** characters per call (once encoded in JSON by the Engagement service).</span></span>

<span data-ttu-id="be6dc-174">V předchozím příkladu je JSON odeslat na server 58 znaků:</span><span class="sxs-lookup"><span data-stu-id="be6dc-174">In the previous example, the JSON sent to the server is 58 characters long:</span></span>

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="be6dc-175">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="be6dc-175">Reporting Application Information</span></span>
<span data-ttu-id="be6dc-176">Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí `sendAppInfo()` funkce.</span><span class="sxs-lookup"><span data-stu-id="be6dc-176">You can manually report tracking information (or any other application specific information) using the `sendAppInfo()` function.</span></span>

<span data-ttu-id="be6dc-177">Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.</span><span class="sxs-lookup"><span data-stu-id="be6dc-177">Note that these information can be sent incrementally: only the latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="be6dc-178">Jako událostí funkce sady třída slouží k abstraktní informace o aplikaci, Všimněte si, že pole nebo dílčí sady bude považována za plochý řetězce (pomocí serializace JSON).</span><span class="sxs-lookup"><span data-stu-id="be6dc-178">Like event extras, the Bundle class is used to abstract application information, note that arrays or sub-bundles will be treated as flat strings (using JSON serialization).</span></span>

### <a name="example"></a><span data-ttu-id="be6dc-179">Příklad</span><span class="sxs-lookup"><span data-stu-id="be6dc-179">Example</span></span>
<span data-ttu-id="be6dc-180">Zde je ukázka kódu odesílání pohlaví uživatele a datum narození:</span><span class="sxs-lookup"><span data-stu-id="be6dc-180">Here is a code sample to send user gender and birthdate:</span></span>

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="be6dc-181">Omezení</span><span class="sxs-lookup"><span data-stu-id="be6dc-181">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="be6dc-182">Klíče</span><span class="sxs-lookup"><span data-stu-id="be6dc-182">Keys</span></span>
<span data-ttu-id="be6dc-183">Každý klíč v `Bundle` musí odpovídat následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="be6dc-183">Each key in the `Bundle` must match the following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="be6dc-184">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="be6dc-184">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="be6dc-185">Velikost</span><span class="sxs-lookup"><span data-stu-id="be6dc-185">Size</span></span>
<span data-ttu-id="be6dc-186">Informace o aplikaci jsou omezeny na **1024** znaků na jednu volání (po zakódování ve formátu JSON pomocí služby zapojení).</span><span class="sxs-lookup"><span data-stu-id="be6dc-186">Application information are limited to **1024** characters per call (once encoded in JSON by the Engagement service).</span></span>

<span data-ttu-id="be6dc-187">V předchozím příkladu je JSON odeslat na server 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="be6dc-187">In the previous example, the JSON sent to the server is 44 characters long:</span></span>

            {"expiration":"2016-12-07","status":"premium"}
