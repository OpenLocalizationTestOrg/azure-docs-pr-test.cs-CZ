---
title: "aaaHow tooUse hello Engagement rozhraní API na Windows Phone Silverlight"
description: "Jak tooUse hello Engagement rozhraní API na Windows Phone Silverlight"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ae2ba2e8-f75b-4dee-a164-a7dd65d35a23
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1e84be95cc910be7f1227b4ae60eb483a1939284
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-phone-silverlight"></a>Jak tooUse hello Engagement rozhraní API na Windows Phone Silverlight
Tento dokument je dokument toohello rozšíření [jak toointegrate Mobile Engagement v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md). Poskytuje v hloubka podrobnosti o tom, jak toouse hello Engagement API tooreport statistik vaší aplikace.

Pokud chcete pouze Engagement tooreport, aplikace relací, aktivit, dojde k chybě a technické informace, pak hello nejjednodušší způsob je toomake všechny vaše `PhoneApplicationPage` dílčí třídy dědí hello `EngagementPage` třídy.

Pokud chcete, aby toodo další, např. Pokud potřebujete tooreport aplikace konkrétní události, chyb a úlohy, nebo pokud máte tooreport aktivitami aplikace jiným způsobem než jednu implementaci hello hello `EngagementPage` třídy, pak je nutné toouse hello Zapojení rozhraní API.

Hello rozhraní API Engagement poskytuje hello `EngagementAgent` třídy. Dostanete toothose metody prostřednictvím `EngagementAgent.Instance`.

I v případě, že modul agentem hello nebyla inicializována, každé rozhraní API toohello volání je odložení a budou spuštěny znovu, pokud je k dispozici hello agent.

## <a name="engagement-concepts"></a>Koncepty engagementu
Následující části Hello Upřesnit hello koncepty Mobile Engagementu pro platformu Windows Phone hello.

### <a name="session-and-activity"></a>`Session` a `Activity`
*Aktivity* obvykle souvisí s jednu stránku hello aplikace, která je toosay hello *aktivity* spustí, když se zobrazí stránku hello a zastaví, když je uzavřený stránku hello: jedná o případ hello při hello Je engagement SDK integrovaná pomocí hello `EngagementPage` třídy.

Ale *aktivity* můžete ručně kontrolovat také pomocí hello Engagement rozhraní API. To umožňuje toosplit danou stránku v několika dílčí části tooget hello další podrobnosti o použití této stránky (například jak často tooknown a jak dlouho se používají dialogová okna v rámci této stránce).

## <a name="reporting-activities"></a>Sestavy aktivit
### <a name="user-starts-a-new-activity"></a>Uživatel spustí novou aktivitu
#### <a name="reference"></a>Referenční informace
            void StartActivity(string name, Dictionary<object, object> extras = null)

Je třeba toocall `StartActivity()` Každá aktivita uživatele hello čas změny. Hello první volání funkce toothis spustí novou relaci uživatele.

> [!IMPORTANT]
> Hello SDK automaticky volání metoda EndActivity hello při zavření aplikace hello. Proto DŮRAZNĚ doporučujeme toocall hello StartActivity metoda vždy, když aktivita hello změnu hello uživatele a volání tooNEVER, že hello EndActivity metoda od voláním této metody vynutí toobe hello aktuální relace skončila.
> 
> 

#### <a name="example"></a>Příklad
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Uživatel končí jeho aktuální aktivita
#### <a name="reference"></a>Referenční informace
            void EndActivity()

Je třeba toocall `EndActivity()` alespoň jednou po dokončení hello uživatele poslední aktivita. Tento příkaz informuje hello vyprší Engagement SDK hello uživatel aktuálně nečinný, a že hello uživatelské relace nutné toobe uzavřít jednou hello časový limit relace (při volání `StartActivity()` vyprší časový limit relace hello hello relace je jednoduše dál).

#### <a name="example"></a>Příklad
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a>Úlohy sestav
### <a name="start-a-job"></a>Spustit úlohu
#### <a name="reference"></a>Referenční informace
            void StartJob(string name, Dictionary<object, object> extras = null)

Můžete vytvořit hello úlohy tootrack certains přes v časovém intervalu.

#### <a name="example"></a>Příklad
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Ukončit úlohu
#### <a name="reference"></a>Referenční informace
            void EndJob(string name)

Jakmile úloha sledovány v rámci úlohy byla ukončena, by měly volat metoda EndJob hello pro tuto úlohu zadáním hello název úlohy.

#### <a name="example"></a>Příklad
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a>Události vytváření sestav
Je k dispozici tři typy událostí:

* Samostatné události
* Události relací
* Události úlohy

### <a name="standalone-events"></a>Samostatné události
#### <a name="reference"></a>Referenční informace
            void SendEvent(string name, Dictionary<object, object> extras = null)

Samostatné události může dojít mimo hello kontext relace.

#### <a name="example"></a>Příklad
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Události relací
#### <a name="reference"></a>Referenční informace
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Relace události jsou obvykle použité tooreport hello akce prováděné uživatelem během jeho relace.

#### <a name="example"></a>Příklad
**Bez dat:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**S daty:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Události úlohy
#### <a name="reference"></a>Referenční informace
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Události úlohy jsou obvykle použité tooreport hello akce prováděné uživatelem během úlohy.

#### <a name="example"></a>Příklad
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a>Zasílání zpráv o chybách
Je k dispozici tři typy chyb:

* Samostatné chyby
* Chyby relace
* Chyby úloh

### <a name="standalone-errors"></a>Samostatné chyby
#### <a name="reference"></a>Referenční informace
            void SendError(string name, Dictionary<object, object> extras = null)

Jinak zvláštní toosession chyby samostatné může dojít k chybám mimo hello kontext relace.

#### <a name="example"></a>Příklad
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Chyby relace
#### <a name="reference"></a>Referenční informace
            void SendSessionError(string name, Dictionary<object, object> extras = null)

Relace chyby jsou obvykle použité tooreport hello chyby během jeho relace, které mají vliv hello uživatele.

#### <a name="example"></a>Příklad
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Chyby úloh
#### <a name="reference"></a>Referenční informace
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Chyby může být spuštěna úloha namísto související tooa související toohello se aktuální uživatelská relace.

#### <a name="example"></a>Příklad
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a>Generování sestav havárií
Hello agent poskytuje dvě metody toodeal dojde k chybě.

### <a name="send-an-exception"></a>Odeslat výjimku
#### <a name="reference"></a>Referenční informace
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Příklad
Výjimku kdykoli můžete odeslat voláním:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Můžete použít také volitelný parametr tooterminate hello engagement relace v hello stejnou dobu, než odesílání hello havárií. toodo tedy volání:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Pokud tak učiníte, úlohy a hello relace budou uzavřeny právě po odeslání hello havárií.

### <a name="send-an-unhandled-exception"></a>Odeslat k neošetřené výjimce
#### <a name="reference"></a>Referenční informace
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Engagement poskytuje také metoda toosend neošetřené výjimky. To je obzvláště užitečná při použití uvnitř hello silverlight UnhandledException obslužné rutiny.

Tato metoda bude **vždy** ukončit relaci engagement hello a úlohy po volání.

#### <a name="example"></a>Příklad
Můžete ji tooimplement použít vlastní obslužnou rutinu UnhandledException (obzvláště pokud je zakázána hello automatické hlášení reporting funkce engagementu). Například v hello `Application_UnhandledException` metoda hello `App.xaml.cs` souboru:

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a>OnActivated
### <a name="reference"></a>Referenční informace
            void OnActivated(ActivatedEventArgs e)

Když hello uživatel přejde dopředu, mimo aplikaci, po hello deaktivováno událost se vyvolá, hello operační systém se pokusí aplikace hello tooput do stavu neaktivní. Aplikace hello je označení. V tomto procesu je aplikace ukončena, ale některá data o stavu hello hello aplikace a hello jednotlivé stránky v rámci aplikace hello je zachovaná.

Máte tooinsert `EngagementAgent.Instance.OnActivated(e)` v hello `Application_Activated` metoda z hello App.xaml.cs souboru tooreset hello Engagement agenta, když aplikace hello bylo označeno jako neplatné.

### <a name="example"></a>Příklad
            // Inside your App.xaml.cs file

            // Code tooexecute when hello application is activated (brought tooforeground)
            // This code will not execute when hello application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a>Id zařízení
            String GetDeviceId()

Voláním této metody můžete získat id zařízení engagement hello.

## <a name="extras-parameters"></a>Parametry funkce
Libovolná data může být připojené tooan události, k chybě, aktivity nebo úlohy. Tato data můžete strukturu pomocí slovníku. Klíče a hodnoty můžou být jakéhokoli typu.

Data funkce se serializují, takže pokud chcete tooinsert vlastní typ v funkce budete mít tooadd kontraktu dat pro tento typ.

### <a name="example"></a>Příklad
Vytvoříme novou třídu "Osoba".

            using System.Runtime.Serialization;

            namespace Engagement.Agent
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

Potom přidáme `Person` instance tooan navíc.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> Když vložíte jiné typy objektů, ujistěte se, že jejich metodu ToString() implementovaná tooreturn lidské čitelných řetězců.
> 
> 

### <a name="limits"></a>Omezení
#### <a name="keys"></a>Klíče
Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).

#### <a name="size"></a>Velikost
Funkce omezeny příliš**1024** znaků na jednu volání.

## <a name="reporting-application-information"></a>Informace o vytváření sestav aplikace
### <a name="reference"></a>Referenční informace
            void SendAppInfo(Dictionary<object, object> appInfos)

Můžete ručně sestavy sledování informace (nebo všechny ostatní aplikace konkrétní informace) pomocí funkce SendAppInfo() hello.

Všimněte si, že tyto údaje lze odeslat přírůstkově: pouze hello nejnovější hodnotu pro daný klíč budou zachovány pro dané zařízení. Jako funkce událostí použití slovníku\<objektu, objekt\> – tooattach informace.

### <a name="example"></a>Příklad
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Omezení
#### <a name="keys"></a>Klíče
Každý klíč v objektu hello musí odpovídat hello následující regulární výraz:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Znamená to, že klíče musí začínat aspoň jedním písmenem, za nímž následuje písmena, číslice nebo podtržítka (\_).

#### <a name="size"></a>Velikost
Informace o aplikaci jsou omezené příliš**1024** znaků na jednu volání.

Předchozí příklad, hello JSON odeslán toohello serveru v hello je 44 znaků:

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a>Protokolování
### <a name="enable-logging"></a>Povolení protokolování
Hello SDK může být nakonfigurované tooproduce protokolů testování v konzole IDE hello.
Tyto protokoly nejsou ve výchozím nastavení aktivované. toocustomize se aktualizovat hello vlastnost `EngagementAgent.Instance.TestLogEnabled` tooone hello hodnota dostupná z hello `EngagementTestLogLevel` výčtu, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
