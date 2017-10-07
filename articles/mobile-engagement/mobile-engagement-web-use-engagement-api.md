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
# <a name="use-hello-azure-mobile-engagement-api-in-a-web-application"></a>Použití hello rozhraní API služby Azure Mobile Engagement ve webové aplikaci
Tento dokument je dokument toohello přidání informující o tom, jak příliš[integrovat Mobile Engagement ve webové aplikaci](mobile-engagement-web-integrate-engagement.md). Poskytuje podrobné informace o tom, jak toouse hello tooreport rozhraní API služby Azure Mobile Engagement statistik vaší aplikace.

Hello Mobile Engagement API poskytuje hello `engagement.agent` objektu. výchozí sada SDK webové služby Azure Mobile Engagement alias je Hello `engagement`. Můžete změnit definici tento alias z konfigurace sady SDK hello.

## <a name="mobile-engagement-concepts"></a>Koncepty Mobile Engagementu
Hello následujících částí Upřesnit běžné [koncepty Mobile Engagementu](mobile-engagement-concepts.md) pro hello webové platformy.

### <a name="session-and-activity"></a>`Session` a `Activity`
Pokud uživatel hello zůstane nečinné více než několik sekund mezi dvěma aktivitami, hello uživatele posloupnost aktivit je rozdělit na dvě odlišné relace. Tyto několik sekund, se nazývají hello časový limit relace.

Pokud webová aplikace není deklarovat hello konec aktivity uživatele samostatně (pomocí volání hello `engagement.agent.endActivity` funkce), server hello Mobile Engagement automaticky vyprší hello relace uživatele do tří minut po zavření stránky aplikace hello. Tomu se říká časový limit relace server hello.

### `Crash`
Ve výchozím nastavení nejsou vytvořeny automatizované sestavy nezachycená výjimek jazyka JavaScript. Však může hlásit havárií ručně pomocí hello `sendCrash` funkce (viz část hello na reporting havárií).

## <a name="reporting-activities"></a>Sestavy aktivit
Zprávy o aktivity uživatelů zahrnuje, když uživatel spustí novou aktivitu, a při ukončení hello aktuální aktivita uživatele hello.

### <a name="user-starts-a-new-activity"></a>Uživatel spustí novou aktivitu
    engagement.agent.startActivity("MyUserActivity");

Je třeba toocall `startActivity()` Každá aktivita uživatele čas změny. Hello první volání funkce toothis spustí novou relaci uživatele.

### <a name="user-ends-hello-current-activity"></a>Uživatel končí hello aktuální aktivita
    engagement.agent.endActivity();

Je třeba toocall `endActivity()` alespoň jednou po dokončení hello uživatele jejich poslední aktivita. Tento příkaz informuje hello Mobile Engagement Web SDK je aktuálně nečinnosti hello uživatele, a že hello uživatelské relace musí toobe po vyprší časový limit relace hello ukončeno. Když zavoláte `startActivity()` vyprší časový limit relace hello hello relaci je jednoduše obnovit.

Protože neexistuje žádný spolehlivý volání pro při zavření okna Navigátor hello, je obtížné nebo dokonce znemožňují toocatch hello konec aktivity uživatele v prostředí webové. Proč je hello Mobile Engagement serveru automaticky vyprší hello uživatelské relace do tří minut po zavření stránky aplikace hello.

## <a name="reporting-events"></a>Události vytváření sestav
Zprávy o události popisuje relace události a události samostatné.

### <a name="session-events"></a>Události relací
Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během relace hello uživatele.

**Příklad bez doplňující data:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Příklad s doplňující data:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Samostatné události
Na rozdíl od relace události může dojít k událostem samostatné mimo hello kontext relace.

K tomu použít ``engagement.agent.sendEvent`` místo ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Zasílání zpráv o chybách
Zprávy o chybách obsahuje chyby relace a samostatné chyby.

### <a name="session-errors"></a>Chyby relace
Relace chyby jsou obvykle použité tooreport hello chyby, které mají vliv na uživatele hello během relace hello uživatele.

**Příklad bez doplňující data:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Příklad s doplňující data:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Samostatné chyby
Na rozdíl od relace chyby může dojít k chybám samostatné mimo hello kontext relace.

K tomu použít `engagement.agent.sendError` místo `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Úlohy sestav
Zprávy o zahrnuje úlohy vytváření sestav chyb a událostí, ke kterým došlo během úlohy a vytváření sestav dojde k chybě.

**Příklad:**

Pokud chcete toomonitor požadavek AJAX, použijte následující hello:

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

### <a name="reporting-errors-during-a-job"></a>Generování sestav chyb během úlohy
Chyby může být související tooa spuštění úlohy místo toohello se aktuální uživatelská relace.

**Příklad:**

Pokud chcete, aby tooreport chybu, pokud požadavek AJAX selže:

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

### <a name="reporting-events-during-a-job"></a>Události vytváření sestav během úlohy
Události může být související tooa spuštění úlohy místo toohello se aktuální uživatelská relace, thanks toohello `engagement.agent.sendJobEvent` funkce.

Tato funkce funguje stejně jako `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Vytváření sestav dojde k chybě
Použití hello `sendCrash` dojde k chybě funkce tooreport ručně.

Hello `crashid` argument je řetězec, který identifikuje hello typ havárie.
Hello `crash` argument je obvykle trasování zásobníku hello hello havárie jako řetězec.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Další parametry
Můžete připojit libovolná data tooan události, chyba, aktivity nebo úlohy.

Hello dat může být jakýkoli objekt JSON (ale ne pole nebo primitivní typ).

**Příklad:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Omezení
Omezení, které se vztahují tooextra parametry jsou v oblastech hello regulárních výrazů pro klíče, hodnotové typy a velikosti.

#### <a name="keys"></a>Klíče
Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:

    ^[a-zA-Z][a-zA-Z_0-9]*

To znamená, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).

#### <a name="values"></a>Hodnoty
Hodnoty jsou omezené toostring, počtu a typů logická hodnota.

#### <a name="size"></a>Velikost
Funkce jsou omezené too1 024 znaků na jednu volání (po hello Mobile Engagement Web SDK kóduje ho ve formátu JSON).

## <a name="reporting-application-information"></a>Informace o vytváření sestav aplikace
Můžete ručně odesílat zprávy o sledování informace (nebo všechny ostatní informace specifické pro aplikace) pomocí hello `sendAppInfo()` funkce.

Všimněte si, že tyto údaje lze odeslat postupně. Pro určité zařízení se zachová hello nejnovější hodnotu pouze pro konkrétního klíče.

Jako funkce událostí můžete použít informace o všech JSON objektu tooabstract aplikace. Všimněte si, že pole nebo dílčí objekty jsou považovány za plochý řetězce (pomocí serializace JSON).

**Příklad:**

Zde je ukázka kódu pro odesílání hello uživatele pohlaví a datum narození:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Omezení
Omezení, které se vztahují tooapplication informace jsou v oblastech hello regulárních výrazů pro klíče a velikost.

#### <a name="keys"></a>Klíče
Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:

    ^[a-zA-Z][a-zA-Z_0-9]*

To znamená, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).

#### <a name="size"></a>Velikost
Informace o aplikaci je omezená too1 024 znaků na jednu volání (po hello Mobile Engagement Web SDK kóduje ho ve formátu JSON).

V předchozím příkladu hello hello JSON odeslané toohello serveru je 44 znaků:

    {"birthdate":"1983-12-07","gender":"female"}
