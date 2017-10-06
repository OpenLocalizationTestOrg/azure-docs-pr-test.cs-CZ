---
title: "aaaHow tooUse hello Engagement rozhraní API v systému Android"
description: "Nejnovější Android SDK – jak tooUse hello Engagement rozhraní API v systému Android"
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
ms.openlocfilehash: e0b2d484616c0c7874e77c5283d94c3063949ed2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-android"></a><span data-ttu-id="b1b2b-103">Jak tooUse hello Engagement rozhraní API v systému Android</span><span class="sxs-lookup"><span data-stu-id="b1b2b-103">How tooUse hello Engagement API on Android</span></span>
<span data-ttu-id="b1b2b-104">Tento dokument je dokument toohello rozšíření [Reporting rozšířené možnosti pro Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-104">This document is an add-on toohello document [Advanced Reporting options for Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md).</span></span> <span data-ttu-id="b1b2b-105">Poskytuje v hloubka podrobnosti o tom, jak toouse hello Engagement API tooreport statistik vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-105">It provides in depth details about how toouse hello Engagement API tooreport your application statistics.</span></span>

<span data-ttu-id="b1b2b-106">Mějte na paměti, že pokud chcete pouze tooreport zapojení vaší aplikace relací, aktivit, dojde k chybě a technické informace, pak hello nejjednodušší způsob, jak se toomake všechny vaše `Activity` dílčí třídy dědí hello odpovídající `EngagementActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-106">Keep in mind that if you only want Engagement tooreport your application's sessions, activities, crashes and technical information, then hello simplest way is toomake all your `Activity` sub-classes inherit from hello corresponding `EngagementActivity` class.</span></span>

<span data-ttu-id="b1b2b-107">Pokud chcete, aby toodo další, např. Pokud potřebujete tooreport aplikace konkrétní události, chyb a úlohy, nebo pokud máte tooreport aktivitami aplikace jiným způsobem než jednu implementaci hello hello `EngagementActivity` třídy, pak je nutné toouse hello Zapojení rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-107">If you want toodo more, for example if you need tooreport application specific events, errors and jobs, or if you have tooreport your application's activities in a different way than hello one implemented in hello `EngagementActivity` classes, then you need toouse hello Engagement API.</span></span>

<span data-ttu-id="b1b2b-108">Hello rozhraní API Engagement poskytuje hello `EngagementAgent` třídy.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-108">hello Engagement API is provided by hello `EngagementAgent` class.</span></span> <span data-ttu-id="b1b2b-109">Instance této třídy může načíst volání hello `EngagementAgent.getInstance(Context)` statickou metodu (Všimněte si, že hello `EngagementAgent` objekt vrácený je typu singleton).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-109">An instance of this class can be retrieved by calling hello `EngagementAgent.getInstance(Context)` static method (note that hello `EngagementAgent` object returned is a singleton).</span></span>

## <a name="engagement-concepts"></a><span data-ttu-id="b1b2b-110">Koncepty engagementu</span><span class="sxs-lookup"><span data-stu-id="b1b2b-110">Engagement concepts</span></span>
<span data-ttu-id="b1b2b-111">Hello následujících částí Upřesnit hello běžné [koncepty Mobile Engagementu](mobile-engagement-concepts.md), pro platformu Android hello.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-111">hello following parts refine hello common [Mobile Engagement Concepts](mobile-engagement-concepts.md), for hello Android platform.</span></span>

### <a name="session-and-activity"></a><span data-ttu-id="b1b2b-112">`Session` a `Activity`</span><span class="sxs-lookup"><span data-stu-id="b1b2b-112">`Session` and `Activity`</span></span>
<span data-ttu-id="b1b2b-113">Pokud uživatel hello zůstane více než pár sekundách nečinnosti mezi dvěma *aktivity*, potom jeho posloupnost *aktivity* je rozdělit na dvě odlišné *relací*.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-113">If hello user stays more than a few seconds idle between two *activities*, then his sequence of *activities* is split in two distinct *sessions*.</span></span> <span data-ttu-id="b1b2b-114">Tyto několik sekund, se nazývají hello "časový limit relace".</span><span class="sxs-lookup"><span data-stu-id="b1b2b-114">These few seconds are called hello "session timeout".</span></span>

<span data-ttu-id="b1b2b-115">*Aktivity* obvykle souvisí s jeden obrazovky aplikace hello, který je toosay hello *aktivity* spustí, když se zobrazí úvodní obrazovka a zastaví, když je uzavřený úvodní obrazovka: Toto je hello případ, kdy Hello Engagement SDK je integrovaná s použitím hello `EngagementActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-115">An *activity* is usually associated with one screen of hello application, that is toosay hello *activity* starts when hello screen is displayed and stops when hello screen is closed: this is hello case when hello Engagement SDK is integrated by using hello `EngagementActivity` classes.</span></span>

<span data-ttu-id="b1b2b-116">Ale *aktivity* můžete ručně kontrolovat také pomocí hello Engagement rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-116">But *activities* can also be controlled manually by using hello Engagement API.</span></span> <span data-ttu-id="b1b2b-117">To umožňuje toosplit dané obrazovky v několika dílčí části tooget hello další podrobnosti o použití této obrazovce (například jak často tooknown a jak dlouho se používají dialogová okna v rámci této obrazovce).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-117">This allows toosplit a given screen in several sub parts tooget more details about hello usage of this screen (for example tooknown how often and how long dialogs are used inside this screen).</span></span>

## <a name="reporting-activities"></a><span data-ttu-id="b1b2b-118">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="b1b2b-118">Reporting Activities</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b1b2b-119">Nepotřebujete tooreport aktivity, jako jsou popsané v této části, pokud používáte hello `EngagementActivity` třídy a jeho variant, jak je popsáno v hello jak tooIntegrate Engagement Android dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-119">You don't need tooreport activities like described in this section if you are using hello `EngagementActivity` class and its variants as explained in hello How tooIntegrate Engagement on Android document.</span></span>
> 
> 

### <a name="user-starts-a-new-activity"></a><span data-ttu-id="b1b2b-120">Uživatel spustí novou aktivitu</span><span class="sxs-lookup"><span data-stu-id="b1b2b-120">User starts a new Activity</span></span>
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing hello current activity is required for Reach toodisplay in-app notifications, passing null will postpone such announcements and polls.

<span data-ttu-id="b1b2b-121">Je třeba toocall `startActivity()` Každá aktivita uživatele hello čas změny.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-121">You need toocall `startActivity()` each time hello user activity changes.</span></span> <span data-ttu-id="b1b2b-122">Hello první volání funkce toothis spustí novou relaci uživatele.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-122">hello first call toothis function starts a new user session.</span></span>

<span data-ttu-id="b1b2b-123">Tato funkce je v každé aktivity nejlepší místo toocall Hello `onResume` zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-123">hello best place toocall this function is on each activity `onResume` callback.</span></span>

### <a name="user-ends-his-current-activity"></a><span data-ttu-id="b1b2b-124">Uživatel končí jeho aktuální aktivita</span><span class="sxs-lookup"><span data-stu-id="b1b2b-124">User ends his current Activity</span></span>
            EngagementAgent.getInstance(this).endActivity();

<span data-ttu-id="b1b2b-125">Je třeba toocall `endActivity()` alespoň jednou po dokončení hello uživatele poslední aktivita.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-125">You need toocall `endActivity()` at least once when hello user finishes his last activity.</span></span> <span data-ttu-id="b1b2b-126">Tento příkaz informuje hello vyprší Engagement SDK hello uživatel aktuálně nečinný, a že hello uživatelské relace nutné toobe uzavřít jednou hello časový limit relace (při volání `startActivity()` vyprší časový limit relace hello hello relaci je jednoduše obnovit).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-126">This informs hello Engagement SDK that hello user is currently idle, and that hello user session need toobe closed once hello session timeout will expire (if you call `startActivity()` before hello session timeout expires, hello session is simply resumed).</span></span>

<span data-ttu-id="b1b2b-127">Tato funkce je v každé aktivity nejlepší místo toocall Hello `onPause` zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-127">hello best place toocall this function is on each activity `onPause` callback.</span></span>

## <a name="reporting-events"></a><span data-ttu-id="b1b2b-128">Události vytváření sestav</span><span class="sxs-lookup"><span data-stu-id="b1b2b-128">Reporting Events</span></span>
### <a name="session-events"></a><span data-ttu-id="b1b2b-129">Události relací</span><span class="sxs-lookup"><span data-stu-id="b1b2b-129">Session events</span></span>
<span data-ttu-id="b1b2b-130">Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během jeho relace.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-130">Session events are usually used tooreport hello actions performed by a user during his session.</span></span>

<span data-ttu-id="b1b2b-131">**Příklad bez doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="b1b2b-131">**Example without extra data:**</span></span>

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

<span data-ttu-id="b1b2b-132">**Příklad s doplňující data:**</span><span class="sxs-lookup"><span data-stu-id="b1b2b-132">**Example with extra data:**</span></span>

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

### <a name="standalone-events"></a><span data-ttu-id="b1b2b-133">Samostatné události</span><span class="sxs-lookup"><span data-stu-id="b1b2b-133">Standalone Events</span></span>
<span data-ttu-id="b1b2b-134">Jinak zvláštní toosession, samostatné události může dojít k událostem mimo hello kontext relace.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-134">Contrary toosession events, standalone events can occur outside of hello context of a session.</span></span>

<span data-ttu-id="b1b2b-135">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="b1b2b-135">**Example:**</span></span>

<span data-ttu-id="b1b2b-136">Předpokládejme, že chcete tooreport události, ke kterým dochází při aktivaci všesměrového vysílání příjemce:</span><span class="sxs-lookup"><span data-stu-id="b1b2b-136">Suppose you want tooreport events occurring when a broadcast receiver is triggered:</span></span>

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a><span data-ttu-id="b1b2b-137">Zasílání zpráv o chybách</span><span class="sxs-lookup"><span data-stu-id="b1b2b-137">Reporting Errors</span></span>
### <a name="session-errors"></a><span data-ttu-id="b1b2b-138">Chyby relace</span><span class="sxs-lookup"><span data-stu-id="b1b2b-138">Session errors</span></span>
<span data-ttu-id="b1b2b-139">Relace chyby jsou obvykle použité tooreport hello chyby během jeho relace, které mají vliv hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-139">Session errors are usually used tooreport hello errors impacting hello user during his session.</span></span>

<span data-ttu-id="b1b2b-140">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="b1b2b-140">**Example:**</span></span>

            /** hello user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* hello user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a><span data-ttu-id="b1b2b-141">Samostatné chyby</span><span class="sxs-lookup"><span data-stu-id="b1b2b-141">Standalone errors</span></span>
<span data-ttu-id="b1b2b-142">Jinak zvláštní toosession chyby samostatné může dojít k chybám mimo hello kontext relace.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-142">Contrary toosession errors, standalone errors can occur outside of hello context of a session.</span></span>

<span data-ttu-id="b1b2b-143">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="b1b2b-143">**Example:**</span></span>

<span data-ttu-id="b1b2b-144">Hello následující příklad ukazuje, jak tooreport chybu vždy, když hello paměti je nedostatek na telefonu hello při procesu vaše aplikace běží.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-144">hello following example shows how tooreport an error whenever hello memory becomes low on hello phone while your application process is running.</span></span>

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a><span data-ttu-id="b1b2b-145">Úlohy sestav</span><span class="sxs-lookup"><span data-stu-id="b1b2b-145">Reporting Jobs</span></span>
### <a name="example"></a><span data-ttu-id="b1b2b-146">Příklad</span><span class="sxs-lookup"><span data-stu-id="b1b2b-146">Example</span></span>
<span data-ttu-id="b1b2b-147">Předpokládejme, že chcete tooreport hello trvání přihlašovací proces:</span><span class="sxs-lookup"><span data-stu-id="b1b2b-147">Suppose you want tooreport hello duration of your login process:</span></span>

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context toocall hello Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a><span data-ttu-id="b1b2b-148">Sestava chyb během úlohy</span><span class="sxs-lookup"><span data-stu-id="b1b2b-148">Report Errors during a Job</span></span>
<span data-ttu-id="b1b2b-149">Chyby může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-149">Errors can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="b1b2b-150">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="b1b2b-150">**Example:**</span></span>

<span data-ttu-id="b1b2b-151">Předpokládejme, že chcete tooreport chybu během jste přihlášení proces:</span><span class="sxs-lookup"><span data-stu-id="b1b2b-151">Suppose you want tooreport an error during you login process:</span></span>

<span data-ttu-id="b1b2b-152">[...] veřejné přihlášení void (kontextu kontextu,...) {</span><span class="sxs-lookup"><span data-stu-id="b1b2b-152">[...] public void signIn(Context context, ...) {</span></span>

              /* We need an Android context toocall hello Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try toosign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report hello error tooEngagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a><span data-ttu-id="b1b2b-153">Události vytváření sestav během úlohy</span><span class="sxs-lookup"><span data-stu-id="b1b2b-153">Reporting Events during a job</span></span>
<span data-ttu-id="b1b2b-154">Události může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-154">Events can be related tooa running job instead of being related toohello current user session.</span></span>

<span data-ttu-id="b1b2b-155">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="b1b2b-155">**Example:**</span></span>

<span data-ttu-id="b1b2b-156">Předpokládejme, že máme sociálních sítí a používáme úlohy tooreport hello celkový čas během které hello je uživatel připojený toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-156">Suppose we have a social network, and we use a job tooreport hello total time during which hello user is connected toohello server.</span></span> <span data-ttu-id="b1b2b-157">Hello uživatele můžete zůstat připojeni pozadí i v případě, že mu používá jiná aplikace nebo když je v režimu spánku hello phone, takže není žádná relace.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-157">hello user can stay connected in background even when he's using another application or when hello phone is sleeping, so there is no session.</span></span>

<span data-ttu-id="b1b2b-158">Hello uživatel může přijímat zprávy z jeho přátel, jedná se o událost úlohy.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-158">hello user can receive messages from his friends, this is a job event.</span></span>

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

## <a name="extra-parameters"></a><span data-ttu-id="b1b2b-159">Další parametry</span><span class="sxs-lookup"><span data-stu-id="b1b2b-159">Extra parameters</span></span>
<span data-ttu-id="b1b2b-160">Libovolná data může být připojené tooevents, chyb, aktivity a úlohy.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-160">Arbitrary data can be attached tooevents, errors, activities and jobs.</span></span>

<span data-ttu-id="b1b2b-161">Tato data mohou být strukturovaná, používá třída sady pro Android (ve skutečnosti, funguje jako další parametry v Android záměry).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-161">This data can be structured, it uses Android's Bundle class (actually, it works like extra parameters in Android Intents).</span></span> <span data-ttu-id="b1b2b-162">Všimněte si, že sady může obsahovat pole nebo jiné sady instancí.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-162">Note that a Bundle can contain arrays or another Bundle instances.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b1b2b-163">Ujistěte se, když vložíte v parametrech parcelable nebo serializable, jejich `toString()` metoda je implementovaná tooreturn řetězec čitelná pro člověka.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-163">If you put in parcelable or serializable parameters, make sure their `toString()` method is implemented tooreturn a human-readable string.</span></span> <span data-ttu-id="b1b2b-164">Serializovatelné třídy, které obsahovat jiné přechodné pole, které nejsou serializovatelný provede Android havárií, když budete volat`bundle.putSerializable("key",value);`</span><span class="sxs-lookup"><span data-stu-id="b1b2b-164">Serializable classes that contain non transient fields that are not serializable will make Android crash when you will call `bundle.putSerializable("key",value);`</span></span>
> 
> [!WARNING]
> <span data-ttu-id="b1b2b-165">Zhuštěný pole v další parametry nejsou podporovány, to znamená, nebude možné serializovat jako pole.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-165">Sparse arrays in extra parameters are not supported, that is, it won't be serialized as an array.</span></span> <span data-ttu-id="b1b2b-166">Je třeba je převést na standardní pole před použitím v další parametry.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-166">You should convert them into standard arrays before using it in extra parameters.</span></span>
> 
> 

### <a name="example"></a><span data-ttu-id="b1b2b-167">Příklad</span><span class="sxs-lookup"><span data-stu-id="b1b2b-167">Example</span></span>
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a><span data-ttu-id="b1b2b-168">Omezení</span><span class="sxs-lookup"><span data-stu-id="b1b2b-168">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="b1b2b-169">Klíče</span><span class="sxs-lookup"><span data-stu-id="b1b2b-169">Keys</span></span>
<span data-ttu-id="b1b2b-170">Každý klíč v hello `Bundle` musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="b1b2b-170">Each key in hello `Bundle` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="b1b2b-171">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-171">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="b1b2b-172">Velikost</span><span class="sxs-lookup"><span data-stu-id="b1b2b-172">Size</span></span>
<span data-ttu-id="b1b2b-173">Funkce omezeny příliš**1024** znaků na jednu volání (po zakódování ve formátu JSON hello zapojení služby).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-173">Extras are limited too**1024** characters per call (once encoded in JSON by hello Engagement service).</span></span>

<span data-ttu-id="b1b2b-174">Předchozí příklad, hello JSON odeslán toohello serveru v hello je 58 znaků:</span><span class="sxs-lookup"><span data-stu-id="b1b2b-174">In hello previous example, hello JSON sent toohello server is 58 characters long:</span></span>

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a><span data-ttu-id="b1b2b-175">Informace o vytváření sestav aplikace</span><span class="sxs-lookup"><span data-stu-id="b1b2b-175">Reporting Application Information</span></span>
<span data-ttu-id="b1b2b-176">Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí hello `sendAppInfo()` funkce.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-176">You can manually report tracking information (or any other application specific information) using hello `sendAppInfo()` function.</span></span>

<span data-ttu-id="b1b2b-177">Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze hello nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.</span><span class="sxs-lookup"><span data-stu-id="b1b2b-177">Note that these information can be sent incrementally: only hello latest value for a given key will be kept for a given device.</span></span>

<span data-ttu-id="b1b2b-178">Jako funkce událostí hello třída sady použité tooabstract informace o aplikaci, Všimněte si, že maticových nebo dílčí obsahuje ureitou bude považována za plochý řetězce (pomocí serializace JSON).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-178">Like event extras, hello Bundle class is used tooabstract application information, note that arrays or sub-bundles will be treated as flat strings (using JSON serialization).</span></span>

### <a name="example"></a><span data-ttu-id="b1b2b-179">Příklad</span><span class="sxs-lookup"><span data-stu-id="b1b2b-179">Example</span></span>
<span data-ttu-id="b1b2b-180">Tady je datum narození a pohlaví uživatele toosend ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="b1b2b-180">Here is a code sample toosend user gender and birthdate:</span></span>

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a><span data-ttu-id="b1b2b-181">Omezení</span><span class="sxs-lookup"><span data-stu-id="b1b2b-181">Limits</span></span>
#### <a name="keys"></a><span data-ttu-id="b1b2b-182">Klíče</span><span class="sxs-lookup"><span data-stu-id="b1b2b-182">Keys</span></span>
<span data-ttu-id="b1b2b-183">Každý klíč v hello `Bundle` musí odpovídat hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="b1b2b-183">Each key in hello `Bundle` must match hello following regular expression:</span></span>

`^[a-zA-Z][a-zA-Z_0-9]*`

<span data-ttu-id="b1b2b-184">Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-184">It means that keys must start with at least one letter, followed by letters, digits or underscores (\_).</span></span>

#### <a name="size"></a><span data-ttu-id="b1b2b-185">Velikost</span><span class="sxs-lookup"><span data-stu-id="b1b2b-185">Size</span></span>
<span data-ttu-id="b1b2b-186">Informace o aplikaci jsou omezené příliš**1024** znaků na jednu volání (po zakódování ve formátu JSON hello zapojení služby).</span><span class="sxs-lookup"><span data-stu-id="b1b2b-186">Application information are limited too**1024** characters per call (once encoded in JSON by hello Engagement service).</span></span>

<span data-ttu-id="b1b2b-187">Předchozí příklad, hello JSON odeslán toohello serveru v hello je 44 znaků:</span><span class="sxs-lookup"><span data-stu-id="b1b2b-187">In hello previous example, hello JSON sent toohello server is 44 characters long:</span></span>

            {"expiration":"2016-12-07","status":"premium"}
