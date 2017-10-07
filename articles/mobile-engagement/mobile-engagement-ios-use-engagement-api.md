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
# <a name="how-toouse-hello-engagement-api-on-ios"></a>Jak tooUse hello Engagement rozhraní API v systému iOS
Tento dokument je dokument toohello rozšíření jak tooIntegrate Engagement v systému iOS: poskytuje v hloubka podrobnosti o tom, jak toouse hello Engagement API tooreport statistik vaší aplikace.

Mějte na paměti, pouze pokud chcete tooreport zapojení vaší aplikace relací, aktivit, dojde k chybě a technické informace, pak hello nejjednodušší způsob je toomake všechny vaše vlastní `UIViewController` objekty dědí hello odpovídající `EngagementViewController` – třída .

Pokud chcete, aby toodo další, např. Pokud potřebujete tooreport aplikace konkrétní události, chyb a úlohy, nebo pokud máte tooreport aktivitami aplikace jiným způsobem než jednu implementaci hello hello `EngagementViewController` třídy, pak je nutné toouse hello Zapojení rozhraní API.

Hello rozhraní API Engagement poskytuje hello `EngagementAgent` třídy. Instance této třídy může načíst volání hello `[EngagementAgent shared]` statickou metodu (Všimněte si, že hello `EngagementAgent` objekt vrácený je typu singleton).

Předtím, než všechny volání rozhraní API, hello `EngagementAgent` objekt je nutné inicializovat voláním metody hello`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

## <a name="engagement-concepts"></a>Koncepty engagementu
Hello následujících částí Upřesnit hello běžné [koncepty Mobile Engagementu](mobile-engagement-concepts.md) pro platformu iOS hello.

### <a name="session-and-activity"></a>`Session` a `Activity`
*Aktivity* obvykle souvisí s jeden obrazovky aplikace hello, který je toosay hello *aktivity* spustí, když se zobrazí úvodní obrazovka a zastaví, když je uzavřený úvodní obrazovka: Toto je hello případ, kdy Hello Engagement SDK je integrovaná s použitím hello `EngagementViewController` třídy.

Ale *aktivity* můžete ručně kontrolovat také pomocí hello Engagement rozhraní API. To umožňuje toosplit dané obrazovky v několika dílčí části tooget hello další podrobnosti o použití této obrazovce (například jak často tooknown a jak dlouho se používají dialogová okna v rámci této obrazovce).

## <a name="reporting-activities"></a>Sestavy aktivit
### <a name="user-starts-a-new-activity"></a>Uživatel spustí novou aktivitu
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Je třeba toocall `startActivity()` Každá aktivita uživatele hello čas změny. Hello první volání funkce toothis spustí novou relaci uživatele.

### <a name="user-ends-his-current-activity"></a>Uživatel končí jeho aktuální aktivita
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> Měli byste **nikdy** volání této funkce samotnými, s výjimkou, pokud chcete, aby toosplit jedno použití aplikace do několik relací: volání funkce toothis by ale ukončilo hello okamžitě, aktuální relace tak následných volání příliš`startActivity()`by zahájit novou relaci. Tato funkce je automaticky volána hello SDK při zavření aplikace.
> 
> 

## <a name="reporting-events"></a>Události vytváření sestav
### <a name="session-events"></a>Události relací
Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během jeho relace.

**Příklad bez doplňující data:**

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

**Příklad s doplňující data:**

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

### <a name="standalone-events"></a>Samostatné události
Jinak zvláštní toosession události, samostatné události lze použít mimo kontext hello relace.

**Příklad:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a>Zasílání zpráv o chybách
### <a name="session-errors"></a>Chyby relace
Relace chyby jsou obvykle použité tooreport hello chyby během jeho relace, které mají vliv hello uživatele.

**Příklad:**

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

### <a name="standalone-errors"></a>Samostatné chyby
Jinak zvláštní toosession chyb, chyb samostatné lze použít mimo kontext hello relace.

**Příklad:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a>Úlohy sestav
**Příklad:**

Předpokládejme, že chcete tooreport hello trvání přihlašovací proces:

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

### <a name="report-errors-during-a-job"></a>Sestava chyb během úlohy
Chyby může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.

**Příklad:**

Předpokládejme, že chcete tooreport chybu během procesu přihlášení:

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

### <a name="events-during-a-job"></a>Události během úlohy
Události může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.

**Příklad:**

Předpokládejme, že máme sociálních sítí a používáme úlohy tooreport hello celkový čas během které hello je uživatel připojený toohello serveru. Hello uživatel může přijímat zprávy z jeho přátel, jedná se o událost úlohy.

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

## <a name="extra-parameters"></a>Další parametry
Libovolná data může být připojené tooevents, chyb, aktivity a úlohy.

Tato data mohou být strukturovaná, používá třída NSDictionary pro iOS.

Všimněte si, že funkce může obsahovat `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` či jiné `NSDictionary` instance.

> [!NOTE]
> Hello speciálním parametrem je serializováno ve formátu JSON. Pokud chcete různé objekty toopass než hello ty, které jsou popsané výše, je nutné implementovat následující metodu v třídě hello:
> 
> -(NSString*) JSONRepresentation;
> 
> Hello metoda by měla vrátit reprezentaci JSON objektu.
> 
> 

### <a name="example"></a>Příklad
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Omezení
#### <a name="keys"></a>Klíče
Každý klíč v hello `NSDictionary` musí odpovídat hello následující regulární výraz:

`^[a-zA-Z][a-zA-Z_0-9]*`

Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).

#### <a name="size"></a>Velikost
Funkce omezeny příliš**1024** znaků na jednu volání (po zakódování ve formátu JSON hello Engagement agenta).

Předchozí příklad, hello JSON odeslán toohello serveru v hello je 58 znaků:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Informace o vytváření sestav aplikace
Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí hello `sendAppInfo:` funkce.

Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze hello nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení.

Jako funkce událostí hello `NSDictionary` třída je použité tooabstract informace o aplikaci, Všimněte si, že maticových nebo dílčí slovníky, budou považovány za plochý řetězce (pomocí serializace JSON).

**Příklad:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Omezení
#### <a name="keys"></a>Klíče
Každý klíč v hello `NSDictionary` musí odpovídat hello následující regulární výraz:

`^[a-zA-Z][a-zA-Z_0-9]*`

Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).

#### <a name="size"></a>Velikost
Informace o aplikaci jsou omezené příliš**1024** znaků na jednu volání (po zakódování ve formátu JSON hello Engagement agenta).

Předchozí příklad, hello JSON odeslán toohello serveru v hello je 44 znaků:

    {"birthdate":"1983-12-07","gender":"female"}
