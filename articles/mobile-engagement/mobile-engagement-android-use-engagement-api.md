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
# <a name="how-toouse-hello-engagement-api-on-android"></a>Jak tooUse hello Engagement rozhraní API v systému Android
Tento dokument je dokument toohello rozšíření [Reporting rozšířené možnosti pro Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md). Poskytuje v hloubka podrobnosti o tom, jak toouse hello Engagement API tooreport statistik vaší aplikace.

Mějte na paměti, že pokud chcete pouze tooreport zapojení vaší aplikace relací, aktivit, dojde k chybě a technické informace, pak hello nejjednodušší způsob, jak se toomake všechny vaše `Activity` dílčí třídy dědí hello odpovídající `EngagementActivity` třídy.

Pokud chcete, aby toodo další, např. Pokud potřebujete tooreport aplikace konkrétní události, chyb a úlohy, nebo pokud máte tooreport aktivitami aplikace jiným způsobem než jednu implementaci hello hello `EngagementActivity` třídy, pak je nutné toouse hello Zapojení rozhraní API.

Hello rozhraní API Engagement poskytuje hello `EngagementAgent` třídy. Instance této třídy může načíst volání hello `EngagementAgent.getInstance(Context)` statickou metodu (Všimněte si, že hello `EngagementAgent` objekt vrácený je typu singleton).

## <a name="engagement-concepts"></a>Koncepty engagementu
Hello následujících částí Upřesnit hello běžné [koncepty Mobile Engagementu](mobile-engagement-concepts.md), pro platformu Android hello.

### <a name="session-and-activity"></a>`Session` a `Activity`
Pokud uživatel hello zůstane více než pár sekundách nečinnosti mezi dvěma *aktivity*, potom jeho posloupnost *aktivity* je rozdělit na dvě odlišné *relací*. Tyto několik sekund, se nazývají hello "časový limit relace".

*Aktivity* obvykle souvisí s jeden obrazovky aplikace hello, který je toosay hello *aktivity* spustí, když se zobrazí úvodní obrazovka a zastaví, když je uzavřený úvodní obrazovka: Toto je hello případ, kdy Hello Engagement SDK je integrovaná s použitím hello `EngagementActivity` třídy.

Ale *aktivity* můžete ručně kontrolovat také pomocí hello Engagement rozhraní API. To umožňuje toosplit dané obrazovky v několika dílčí části tooget hello další podrobnosti o použití této obrazovce (například jak často tooknown a jak dlouho se používají dialogová okna v rámci této obrazovce).

## <a name="reporting-activities"></a>Sestavy aktivit
> [!IMPORTANT]
> Nepotřebujete tooreport aktivity, jako jsou popsané v této části, pokud používáte hello `EngagementActivity` třídy a jeho variant, jak je popsáno v hello jak tooIntegrate Engagement Android dokumentu.
> 
> 

### <a name="user-starts-a-new-activity"></a>Uživatel spustí novou aktivitu
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing hello current activity is required for Reach toodisplay in-app notifications, passing null will postpone such announcements and polls.

Je třeba toocall `startActivity()` Každá aktivita uživatele hello čas změny. Hello první volání funkce toothis spustí novou relaci uživatele.

Tato funkce je v každé aktivity nejlepší místo toocall Hello `onResume` zpětného volání.

### <a name="user-ends-his-current-activity"></a>Uživatel končí jeho aktuální aktivita
            EngagementAgent.getInstance(this).endActivity();

Je třeba toocall `endActivity()` alespoň jednou po dokončení hello uživatele poslední aktivita. Tento příkaz informuje hello vyprší Engagement SDK hello uživatel aktuálně nečinný, a že hello uživatelské relace nutné toobe uzavřít jednou hello časový limit relace (při volání `startActivity()` vyprší časový limit relace hello hello relaci je jednoduše obnovit).

Tato funkce je v každé aktivity nejlepší místo toocall Hello `onPause` zpětného volání.

## <a name="reporting-events"></a>Události vytváření sestav
### <a name="session-events"></a>Události relací
Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během jeho relace.

**Příklad bez doplňující data:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Příklad s doplňující data:**

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

### <a name="standalone-events"></a>Samostatné události
Jinak zvláštní toosession, samostatné události může dojít k událostem mimo hello kontext relace.

**Příklad:**

Předpokládejme, že chcete tooreport události, ke kterým dochází při aktivaci všesměrového vysílání příjemce:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a>Zasílání zpráv o chybách
### <a name="session-errors"></a>Chyby relace
Relace chyby jsou obvykle použité tooreport hello chyby během jeho relace, které mají vliv hello uživatele.

**Příklad:**

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

### <a name="standalone-errors"></a>Samostatné chyby
Jinak zvláštní toosession chyby samostatné může dojít k chybám mimo hello kontext relace.

**Příklad:**

Hello následující příklad ukazuje, jak tooreport chybu vždy, když hello paměti je nedostatek na telefonu hello při procesu vaše aplikace běží.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a>Úlohy sestav
### <a name="example"></a>Příklad
Předpokládejme, že chcete tooreport hello trvání přihlašovací proces:

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

### <a name="report-errors-during-a-job"></a>Sestava chyb během úlohy
Chyby může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.

**Příklad:**

Předpokládejme, že chcete tooreport chybu během jste přihlášení proces:

[...] veřejné přihlášení void (kontextu kontextu,...) {

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

### <a name="reporting-events-during-a-job"></a>Události vytváření sestav během úlohy
Události může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.

**Příklad:**

Předpokládejme, že máme sociálních sítí a používáme úlohy tooreport hello celkový čas během které hello je uživatel připojený toohello serveru. Hello uživatele můžete zůstat připojeni pozadí i v případě, že mu používá jiná aplikace nebo když je v režimu spánku hello phone, takže není žádná relace.

Hello uživatel může přijímat zprávy z jeho přátel, jedná se o událost úlohy.

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

## <a name="extra-parameters"></a>Další parametry
Libovolná data může být připojené tooevents, chyb, aktivity a úlohy.

Tato data mohou být strukturovaná, používá třída sady pro Android (ve skutečnosti, funguje jako další parametry v Android záměry). Všimněte si, že sady může obsahovat pole nebo jiné sady instancí.

> [!IMPORTANT]
> Ujistěte se, když vložíte v parametrech parcelable nebo serializable, jejich `toString()` metoda je implementovaná tooreturn řetězec čitelná pro člověka. Serializovatelné třídy, které obsahovat jiné přechodné pole, které nejsou serializovatelný provede Android havárií, když budete volat`bundle.putSerializable("key",value);`
> 
> [!WARNING]
> Zhuštěný pole v další parametry nejsou podporovány, to znamená, nebude možné serializovat jako pole. Je třeba je převést na standardní pole před použitím v další parametry.
> 
> 

### <a name="example"></a>Příklad
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Omezení
#### <a name="keys"></a>Klíče
Každý klíč v hello `Bundle` musí odpovídat hello následující regulární výraz:

`^[a-zA-Z][a-zA-Z_0-9]*`

Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).

#### <a name="size"></a>Velikost
Funkce omezeny příliš**1024** znaků na jednu volání (po zakódování ve formátu JSON hello zapojení služby).

Předchozí příklad, hello JSON odeslán toohello serveru v hello je 58 znaků:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Informace o vytváření sestav aplikace
Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí hello `sendAppInfo()` funkce.

Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze hello nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.

Jako funkce událostí hello třída sady použité tooabstract informace o aplikaci, Všimněte si, že maticových nebo dílčí obsahuje ureitou bude považována za plochý řetězce (pomocí serializace JSON).

### <a name="example"></a>Příklad
Tady je datum narození a pohlaví uživatele toosend ukázka kódu:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Omezení
#### <a name="keys"></a>Klíče
Každý klíč v hello `Bundle` musí odpovídat hello následující regulární výraz:

`^[a-zA-Z][a-zA-Z_0-9]*`

Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).

#### <a name="size"></a>Velikost
Informace o aplikaci jsou omezené příliš**1024** znaků na jednu volání (po zakódování ve formátu JSON hello zapojení služby).

Předchozí příklad, hello JSON odeslán toohello serveru v hello je 44 znaků:

            {"expiration":"2016-12-07","status":"premium"}
