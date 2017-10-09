---
title: aaaReport a kontrola stavu s Azure Service Fabric | Microsoft Docs
description: "Zjistěte, jak toosend stavu sestavy z vašeho kódu služby a jak poskytuje toocheck hello stav služby pomocí nástroje pro sledování stavu hello této Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: mfussell
editor: 
ms.assetid: 7c712c22-d333-44bc-b837-d0b3603d9da8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: dekapur
ms.openlocfilehash: bcb838fefe3f2054447e1731d709e455560260e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="report-and-check-service-health"></a>Hlášení a kontrola stavu služeb
Pokud vaše služby dojde k potížím, možnost toorespond tooand oprava incidenty a výpadky závisí na vaší možnost toodetect hello problémy rychle. Pokud sestavu problémy a chyby Správce stavu Azure Service Fabric toohello z kódu vaší služby, můžete použít standardní stavu, že Service Fabric nabízí toocheck hello stav nástroje pro monitorování.

Existují tři způsoby, můžete odesílat zprávy o stavu ze služby hello:

* Použití [oddílu](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) nebo [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objekty.  
  Můžete použít hello `Partition` a `CodePackageActivationContext` objekty tooreport hello stavu elementů, které jsou součástí aktuální kontext hello. Kód, který spouští jako součást repliku můžete například sestav stavu pouze o této repliky, hello oddíl, který patří do a hello aplikace, která je součástí.
* Použití `FabricClient`.   
  Můžete použít `FabricClient` tooreport stavu z kódu služby hello, pokud není hello clusteru [zabezpečené](service-fabric-cluster-security.md) nebo pokud služba hello je spuštěn s oprávněními správce. Většina scénářích reálného světa nepoužívejte zabezpečená clustery, nebo zadejte oprávnění správce. S `FabricClient`, stavu může podávat Každá entita, která je součástí clusteru hello. V ideálním případě by však kódu služby by měl odesílat pouze sestavy, které jsou související tooits vlastní stavu.
* Použití hello rozhraní REST API na hello clusteru, aplikace, nasazení aplikace, služby, balíček služby, oddílu, repliky nebo úrovně uzlu. To může být použité tooreport stav od do kontejneru.

Tento článek vás provede příklad, který hlásí stav z kódu služby hello. Hello příklad také ukazuje, jak hello nástroje poskytované subsystémem pro Service Fabric lze použít toocheck hello stav. Tento článek je určený toobe možnosti Service Fabric monitorování stavu toohello rychlý úvod. Podrobnější informace si můžete přečíst hello řadu podrobných článků o stavu, které začínají hello odkaz na konci hello tohoto článku.

## <a name="prerequisites"></a>Požadavky
Musíte mít nainstalované tyto položky hello:

* Visual Studio 2015 nebo Visual Studio 2017
* Service Fabric SDK

## <a name="toocreate-a-local-secure-dev-cluster"></a>toocreate cluster místní zabezpečené vývojářů
* Otevřete prostředí PowerShell s oprávněními správce a spusťte následující příkazy hello:

![Příkazy, které zobrazují, jak toocreate cluster zabezpečené vývojářů](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="toodeploy-an-application-and-check-its-health"></a>toodeploy aplikaci a zkontrolujte jeho stav
1. Otevřete Visual Studio jako správce.
2. Vytvoření projektu pomocí hello **stavové služby** šablony.
   
    ![Vytvoření aplikace Service Fabric pomocí stavové služby](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. Stiskněte klávesu **F5** toorun hello aplikace v režimu ladění. Hello aplikace je nasazená toohello místní cluster.
4. Po hello aplikace spuštěna, klikněte pravým tlačítkem na ikonu Local Cluster Manager hello v oznamovací oblasti hello a vyberte možnost **spravovat místní Cluster** z hello místní nabídky tooopen Service Fabric Exploreru.
   
    ![Otevření Service Fabric Explorer z oznamovací oblasti](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. Stav aplikace Hello má být zobrazena jako tuto bitovou kopii. V tomto okamžiku musí být aplikace hello bez chyb v pořádku.
   
    ![V pořádku aplikace v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. Můžete také zkontrolovat hello stavu pomocí prostředí PowerShell. Můžete použít ```Get-ServiceFabricApplicationHealth``` toocheck stavu aplikace a můžete použít ```Get-ServiceFabricServiceHealth``` toocheck služby stavu. Sestava stavu pro hello stejná aplikace v prostředí PowerShell je na tomto obrázku Hello.
   
    ![V pořádku aplikace v prostředí PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="tooadd-custom-health-events-tooyour-service-code"></a>tooadd vlastní stavu události tooyour služby kódu
šablony projektů Hello Service Fabric v sadě Visual Studio obsahovat ukázkový kód. Hello následující kroky ukazují, jak může hlásit události vlastní stavu z kódu vaší služby. Tyto zprávy zobrazí automaticky v hello standardní nástroje pro sledování, že poskytuje Service Fabric, jako je Service Fabric Exploreru, zobrazení stavu Azure portálu a prostředí PowerShell stavu.

1. Znovu otevřete hello aplikace, kterou jste vytvořili dříve v sadě Visual Studio, nebo vytvořit novou aplikaci pomocí hello **stavové služby** šablony sady Visual Studio.
2. Otevřete soubor Stateful1.cs hello a najde hello `myDictionary.TryGetValueAsync` volání v hello `RunAsync` metoda. Uvidíte, že tato metoda vrátí hodnotu `result` blokování protože hello klíče logiku v této aplikaci je tookeep počet spuštění text hello aktuální hodnota čítače hello. Pokud to byly reálné aplikaci a hello nedostatečná výsledek určený k selhání, je vhodnější tooflag tuto událost.
3. tooreport událost stavu, pokud chybí hello výsledku představuje selhání, přidejte hello následující kroky.
   
    a. Přidat hello `System.Fabric.Health` obor názvů toohello Stateful1.cs souboru.
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    b. Přidejte následující kód po hello hello `myDictionary.TryGetValueAsync` volání
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Vytvoříme sestavy stavu repliky, protože je právě nahlásila stavové služby. Hello `HealthInformation` parametr ukládá informace o problému hello stavu, který je hlášena.
   
    Pokud jste vytvořili bezstavové služby, použijte následující kód hello
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. Pokud vaše služba je spuštěn s oprávněními správce, nebo pokud hello clusteru není [zabezpečené](service-fabric-cluster-security.md), můžete použít také `FabricClient` tooreport stavu, jak je znázorněno v hello následující kroky.  
   
    a. Vytvoření hello `FabricClient` instance po hello `var myDictionary` deklarace.
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    b. Přidejte následující kód po hello hello `myDictionary.TryGetValueAsync` volání.
   
    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.Context.PartitionId,
            this.Context.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```
5. Pojďme simulovat tuto chybu, abyste viděli, zobrazí v hello nástrojů pro monitorování stavu. toosimulate hello selhání, komentář hello první řádek ve stavu hello reporting kód, který jste přidali dříve. Po komentář první řádek hello hello kód bude vypadat jako hello následující ukázka.
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   Tento kód aktivuje se pokaždé, když sestava stavu hello `RunAsync` provede. Když provedete hello změnit, stiskněte klávesu **F5** toorun hello aplikace.
6. Po hello aplikace běží, otevřete Service Fabric Explorer toocheck hello stavu aplikace hello. Tentokrát Service Fabric Explorer ukazuje, že je aplikace hello není v pořádku. Důvodem je chyba hello, která byla nahlášena z kódu hello jsme přidali dříve.
   
    ![Není v pořádku aplikace v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. Pokud vyberete hello primární repliky v zobrazení stromu hello Service Fabric Explorer, zobrazí se **stav** příliš označuje k chybě. Service Fabric Explorer také zobrazuje podrobnosti, které byly přidány toohello sestava stavu hello `HealthInformation` parametr v kódu hello. Zobrazí se text hello stejné sestavy stavu v prostředí PowerShell a hello portálu Azure.
   
    ![Stav repliky v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Tato sestava zůstane v hello správce stavu, dokud je nahrazen jinou sestavu nebo dokud nebude tato replika se odstraní. Protože jsme nenastavili `TimeToLive` pro tuto sestavu stavu v hello `HealthInformation` objektu hello sestavy nikdy nevyprší.

Doporučujeme vám, že by měl hlásí stav na hello nejvíce podrobné úrovni, které v tomto případě je hello repliky. Můžete také sestavy stavu `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Stav tooreport na `Application`, `DeployedApplication`, a `DeployedServicePackage`, použijte `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Další kroky
* [Podrobné informace o stavu Service Fabric](service-fabric-health-introduction.md)
* [REST API pro vytváření sestav stavu služby](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [REST API pro vytváření sestav stavu aplikací](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

