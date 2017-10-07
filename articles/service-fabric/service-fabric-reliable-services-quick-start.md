---
title: "aaaCreate vaší první aplikace Service Fabric v jazyce C# | Microsoft Docs"
description: "Úvod toocreating aplikace Microsoft Azure Service Fabric s bezzstavovými i stavovými službami."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: vturecek
ms.openlocfilehash: e95e67cc84be1b83c936b250cae9112ddc77b963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a>Začínáme s Reliable Services
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-services-quick-start.md)
> * [Java v Linuxu](service-fabric-reliable-services-quick-start-java.md)
> 
> 

Aplikace Azure Service Fabric obsahuje jednu nebo více služeb, které spustíte kód. Tento průvodce vám ukáže, jak toocreate bezzstavovými i stavovými aplikací Service Fabric pomocí [spolehlivé služby](service-fabric-reliable-services-introduction.md).  Toto video Microsoft Virtual Academy také ukazuje, jak toocreate bezstavové spolehlivé služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965">  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a>Základní koncepty
spuštění se službami Reliable Services, můžete pouze tooget potřebovat toounderstand pár základních konceptech:

* **Typ služby**: Toto je implementace služby. Je definována pomocí třídy hello napíšete, která rozšiřuje `StatelessService` a ostatní kódu nebo v něm, použít název a číslo verze závislosti.
* **S názvem instance služby**: toorun vaší služby, vytvoříte pojmenované instance typu služby mnohem jako vytvoření instance objektu typu třídy. Instance služby má název v podobě hello identifikátoru URI pomocí hello "fabric: /", jako například scheme "fabric: / MyApp/Moje_služba".
* **Hostitel služby**: hello s názvem instance služby vytvoříte toorun nutné v hostitelském procesu. Hostitel služby Hello je právě proces, kde můžete spustit instance služby.
* **Registrace služby**: registrace soustřeďuje všechny informace dohromady. Hello typ služby musí být zaregistrován s hello Service Fabric runtime ve službě hostitele tooallow Service Fabric toocreate instance ho toorun.  

## <a name="create-a-stateless-service"></a>Vytvoření bezstavové služby
Bezstavové služby je typ služby, který je aktuálně hello norm v cloudových aplikacích. Považuje bezstavové, protože samotnou službu hello neobsahuje data, která potřebují toobe uložené spolehlivě nebo vysoké dostupnosti. Pokud dojde instanci bezstavové služby, všechny jeho vnitřní stav bude ztracena. V tomto typu služby stavu musí být trvalý tooan externí úložiště, jako je Azure tabulky nebo databázi SQL pro něj toobe provedené vysoce dostupné a spolehlivé.

Spusťte Visual Studio 2015 nebo 2017 Visual Studio jako správce a vytvořit nový projekt aplikace Service Fabric s názvem *HelloWorld*:

![Použít hello nový projekt dialogové okno pole toocreate nové aplikace Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Pak vytvořte bezstavové služby projektu s názvem *HelloWorldStateless*:

![V hello druhé dialogové okno vytvořte projekt bezstavové služby](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

Řešení nyní obsahuje dva projekty:

* *Hello World*. Toto je hello *aplikace* projekt, který obsahuje vaše *služby*. Obsahuje taky hello manifest aplikace, která popisuje hello aplikace a také řadu skriptů prostředí PowerShell, které vám pomůžou toodeploy vaší aplikace.
* *HelloWorldStateless*. Toto je projekt služby hello. Obsahuje implementace hello bezstavové služby.

## <a name="implement-hello-service"></a>Implementace služby hello
Otevřete hello **HelloWorldStateless.cs** souboru v projektu služby hello. V Service Fabric můžete službu spustit veškeré obchodní logiky. rozhraní API služby Hello poskytuje dva vstupní body kódu:

* Metodu zprostředkovává vstupního bodu, názvem *RunAsync*, kde můžete začít provádění jakékoli úlohy, včetně dlouho běžící výpočetních úloh.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* Bod položka komunikace, kde můžete zařadit do zásobníku komunikace podle vlastní volby, jako je ASP.NET Core. Toto je, kde můžete spustit přijímá žádosti od uživatelů a dalších služeb.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

V tomto kurzu se zaměříme na hello `RunAsync()` metody vstupní bod. Toto je, kde můžete okamžitě začít kód spuštěný.
Šablona projektu Hello obsahuje ukázkové provádění `RunAsync()` který zvětší kumulativní počet.

> [!NOTE]
> Podrobnosti o způsobu toowork s komunikaci ve formě zásobníku najdete v tématu [Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md)
> 
> 

### <a name="runasync"></a>RunAsync
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

Platforma Hello volá tuto metodu, pokud instance služby je umístěný a připravené tooexecute. Bezstavové služby, která jednoduše znamená po otevření hello instance služby. Token zrušení získáte toocoordinate při instanci služby musí toobe uzavřen. V Service Fabric může tento cyklus otevření nebo uzavření instance služby k mnohokrát průběhu životnosti hello hello služby jako celek. Toto může nastat z různých důvodů, včetně:

* Hello systém přesune vaší instance služby pro vyrovnávání prostředků.
* K chybám dochází v kódu.
* Hello aplikace nebo systému byla upgradována.
* základní hardware Hello dojde k výpadku.

Tato orchestration spravuje hello systému tookeep služby vysoce dostupné a správně vyrovnáváním.

`RunAsync()`by neměly blokovat synchronně. Implementaci RunAsync by měla vrátit úlohu nebo await na všechny operace dlouho běžící nebo blokování tooallow hello runtime toocontinue. Poznámka: v hello `while(true)` smyčky v předchozí příklad hello úloh vrácení `await Task.Delay()` se používá. Pokud vaše úlohy musí blokovat synchronně, měli byste naplánovat novou úlohu s `Task.Run()` ve vaší `RunAsync` implementace.

Zrušení úlohy je spolupráci úsilí řízená hello zadaný token zrušení. Hello systém bude čekat vaše úloha tooend (podle úspěšné dokončení, zrušení nebo selhání) předtím, než ji přesune. Je důležité toohonor hello zrušení tokenu, dokončit všechny fungovat a ukončete `RunAsync()` provést co nejrychleji při hello systému požadavky zrušení.

V tomto příkladu bezstavové služby je počet hello uložené v místní proměnné. Ale protože je bezstavové služby, hello hodnotu, která je uložená existuje pouze pro hello aktuální životní cyklus jeho instanci služby. Když služba hello přesune nebo restartování, dojde ke ztrátě hello hodnoty.

## <a name="create-a-stateful-service"></a>Vytvoření stavové služby
Service Fabric zavádí nový typ služby, která je stavový. Stavové služby můžete udržovat stav spolehlivě v rámci služby hello, samostatně, umístěn společně s hello kód, který se používá. Stav je vysoké dostupnosti pomocí Service Fabric bez hello nutné toopersist stavu tooan externím obchodu.

tooconvert hodnota čítače z bezstavové toohighly dostupné a trvalé, i když služba hello přesune nebo restartuje, musíte stavové služby.

V hello stejné *HelloWorld* aplikace, můžete přidat novou službu kliknutím pravým tlačítkem na hello služby odkazů v projektu aplikace hello a výběr **Přidat -> Nový Service Fabric Service**.

![Přidání služby tooyour aplikace Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

Vyberte **stavové služby** a pojmenujte ji *HelloWorldStateful*. Klikněte na **OK**.

![Použít hello nový projekt dialogové okno pole toocreate nové stavové služby Service Fabric](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Aplikace musí mít teď dvě služby: hello bezstavové služby *HelloWorldStateless* a stavové služby hello *HelloWorldStateful*.

Stavová služba má hello stejné vstupní body jako bezstavové služby. Hlavní rozdíl Hello je hello dostupnost *zprostředkovatele stavu* který spolehlivě uložení stavu. Service Fabric se dodává s implementace zprostředkovatele stav názvem [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md), která vám umožňuje vytvořit struktury replikovaná data prostřednictvím hello spolehlivé správce stavu. Stavové spolehlivé služby pomocí zprostředkovatele stavu ve výchozím nastavení.

Otevřete **HelloWorldStateful.cs** v *HelloWorldStateful*, který obsahuje následující metodě RunAsync hello:

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, hello transaction aborts, all changes are
            // discarded, and nothing is saved toohello secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a>RunAsync
`RunAsync()`funguje podobně jako v stavová a Bezstavová služby. Však v stavové služby platformy hello provede další práci vaším jménem předtím, než se provede `RunAsync()`. Tento pracovní může obsahovat zajistit, že hello spolehlivé správce stavu a spolehlivé kolekce jsou připravené toouse.

### <a name="reliable-collections-and-hello-reliable-state-manager"></a>Spolehlivé kolekce a hello spolehlivé správce stavu
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) je slovník implementace, které můžete použít tooreliably ukládání stavu ve službě hello. Service Fabric a spolehlivé kolekcí mohou ukládat data přímo ve službě bez nutnosti hello externí trvalého úložiště. Spolehlivé kolekce dají vašim datům vysoce dostupný. Service Fabric dosahuje tak, že vytváření a správu více *repliky* služby za vás. Také poskytuje rozhraní API, které abstrahuje tokeny hello složitosti správy tyto repliky a jejich přechodů mezi stavy.

Spolehlivé kolekce můžete ukládat jakýkoli typ rozhraní .NET, včetně vlastních typů, pomocí několika upozornění:

* Service Fabric umožňuje vašemu stavu vysoce dostupné podle *replikace* úložiště stavu napříč uzly a spolehlivé kolekce datový disk toolocal na jednotlivé repliky. To znamená, že vše, co je uložen v spolehlivé kolekce musí být *serializovatelný*. Ve výchozím nastavení, použijte spolehlivé kolekce [kontraktu](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) pro serializaci, proto je důležité toomake se, že jsou vaše typy [nepodporuje hello serializátor kontraktu dat](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) při použití výchozí hello serializátor.
* Objekty jsou replikovány pro zajištění vysoké dostupnosti při potvrzení transakce na spolehlivé kolekce. Objekty uložené v kolekcích spolehlivé udržovaly v místní paměti ve službě. To znamená, že máte toohello místní referenční objekt.
  
   Je důležité, neprovádějte místní instancí těchto objektů bez provádění operace aktualizace na hello spolehlivé kolekce v transakci. To je proto nebude automaticky replikovat změny toolocal instance objektů. Musíte znovu vložit objekt hello zpět do slovníku hello nebo použijte jednu z hello *aktualizace* metody na hello slovníku.

Hello spolehlivé správce stavu spravuje spolehlivé kolekce za vás. Můžete jednoduše pokládat hello spolehlivé správce stavu pro kolekci spolehlivé podle názvu kdykoli a kdekoli v službě. Hello spolehlivé správce stavu zajišťuje, získejte odkaz na zpět. Není vhodné uložit odkazy tooreliable kolekci instancí třídy členské proměnné nebo vlastnosti. Musí dát zvláštní pozor tooensure nastavený hello odkaz tooan instance v případech, v průběhu životního cyklu služby hello. Hello spolehlivé správce stavu zpracuje tato práce pro uživatele a je optimalizovaný pro opakování návštěvách.

### <a name="transactional-and-asynchronous-operations"></a>Transakční a asynchronní operace
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Spolehlivé kolekce mají mnoho hello stejné operace, jejich `System.Collections.Generic` a `System.Collections.Concurrent` svými protějšky, s výjimkou LINQ. Operace na spolehlivé kolekce jsou asynchronní. Je to proto, že operace zápisu ke kolekcím, spolehlivé provádění vstupně-výstupní operace tooreplicate a zachovat data toodisk.

Spolehlivé operace kolekce jsou *transakcí*, takže můžete zachovat stav konzistentní napříč více spolehlivé kolekce a operace. Může například dequeue – pracovní položky z fronty spolehlivé, proveďte operaci na něm a uložit výsledek hello slovník spolehlivé, vše v rámci jedné transakce. To je považován za atomická operace, a zaručuje, že buď hello celé operace proběhne úspěšně nebo celou operaci hello vrátíte zpět. Pokud dojde k chybě po dequeue hello položku, ale před uložením hello výsledek, hello celá transakce bude vrácena zpět a hello položka zůstane ve frontě hello ke zpracování.

## <a name="run-hello-application"></a>Spuštění aplikace hello
Nemůžeme se teď vrátit toohello *HelloWorld* aplikace. Teď můžete sestavit a nasazení služeb. Po stisknutí klávesy **F5**, bude vaše aplikace vytvořené a nasazené tooyour místní cluster.

Po hello služby spuštění, spuštění, můžete zobrazit události trasování událostí pro Windows (ETW) hello vygenerované v **diagnostických událostí** okno. Upozorňujeme, že zobrazených událostí hello jsou z hello bezstavové služby i hello stavové služby v aplikaci hello. Je možné pozastavit datový proud hello kliknutím hello **pozastavit** tlačítko. Poté můžete prozkoumat hello podrobnosti zprávy rozbalením této zprávě.

> [!NOTE]
> Před spuštěním aplikace hello, ujistěte se, že máte místního vývojového clusteru se systémem. Podívejte se na hello [Příručka Začínáme](service-fabric-get-started.md) informace o nastavení vašeho místního prostředí.
> 
> 

![Zobrazení diagnostických událostí v sadě Visual Studio](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a>Další kroky
[Ladění aplikace Service Fabric v sadě Visual Studio](service-fabric-debugging-your-application.md)

[Začínáme: Service Fabric webového rozhraní API služby s vlastním hostování OWIN](service-fabric-reliable-services-communication-webapi.md)

[Další informace o spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md)

[Nasazení aplikace](service-fabric-deploy-remove-applications.md)

[Upgrade aplikace](service-fabric-application-upgrade.md)

[Referenční informace pro vývojáře pro spolehlivé služby](https://msdn.microsoft.com/library/azure/dn706529.aspx)

