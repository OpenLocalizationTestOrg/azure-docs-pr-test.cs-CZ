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
# <a name="report-and-check-service-health"></a><span data-ttu-id="e57b5-103">Hlášení a kontrola stavu služeb</span><span class="sxs-lookup"><span data-stu-id="e57b5-103">Report and check service health</span></span>
<span data-ttu-id="e57b5-104">Pokud vaše služby dojde k potížím, možnost toorespond tooand oprava incidenty a výpadky závisí na vaší možnost toodetect hello problémy rychle.</span><span class="sxs-lookup"><span data-stu-id="e57b5-104">When your services encounter problems, your ability toorespond tooand fix incidents and outages depends on your ability toodetect hello issues quickly.</span></span> <span data-ttu-id="e57b5-105">Pokud sestavu problémy a chyby Správce stavu Azure Service Fabric toohello z kódu vaší služby, můžete použít standardní stavu, že Service Fabric nabízí toocheck hello stav nástroje pro monitorování.</span><span class="sxs-lookup"><span data-stu-id="e57b5-105">If you report problems and failures toohello Azure Service Fabric health manager from your service code, you can use standard health monitoring tools that Service Fabric provides toocheck hello health status.</span></span>

<span data-ttu-id="e57b5-106">Existují tři způsoby, můžete odesílat zprávy o stavu ze služby hello:</span><span class="sxs-lookup"><span data-stu-id="e57b5-106">There are three ways that you can report health from hello service:</span></span>

* <span data-ttu-id="e57b5-107">Použití [oddílu](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) nebo [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objekty.</span><span class="sxs-lookup"><span data-stu-id="e57b5-107">Use [Partition](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) or [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objects.</span></span>  
  <span data-ttu-id="e57b5-108">Můžete použít hello `Partition` a `CodePackageActivationContext` objekty tooreport hello stavu elementů, které jsou součástí aktuální kontext hello.</span><span class="sxs-lookup"><span data-stu-id="e57b5-108">You can use hello `Partition` and `CodePackageActivationContext` objects tooreport hello health of elements that are part of hello current context.</span></span> <span data-ttu-id="e57b5-109">Kód, který spouští jako součást repliku můžete například sestav stavu pouze o této repliky, hello oddíl, který patří do a hello aplikace, která je součástí.</span><span class="sxs-lookup"><span data-stu-id="e57b5-109">For example, code that runs as part of a replica can report health only on that replica, hello partition that it belongs to, and hello application that it is a part of.</span></span>
* <span data-ttu-id="e57b5-110">Použití `FabricClient`.</span><span class="sxs-lookup"><span data-stu-id="e57b5-110">Use `FabricClient`.</span></span>   
  <span data-ttu-id="e57b5-111">Můžete použít `FabricClient` tooreport stavu z kódu služby hello, pokud není hello clusteru [zabezpečené](service-fabric-cluster-security.md) nebo pokud služba hello je spuštěn s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="e57b5-111">You can use `FabricClient` tooreport health from hello service code if hello cluster is not [secure](service-fabric-cluster-security.md) or if hello service is running with admin privileges.</span></span> <span data-ttu-id="e57b5-112">Většina scénářích reálného světa nepoužívejte zabezpečená clustery, nebo zadejte oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="e57b5-112">Most real-world scenarios do not use unsecured clusters, or provide admin privileges.</span></span> <span data-ttu-id="e57b5-113">S `FabricClient`, stavu může podávat Každá entita, která je součástí clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e57b5-113">With `FabricClient`, you can report health on any entity that is a part of hello cluster.</span></span> <span data-ttu-id="e57b5-114">V ideálním případě by však kódu služby by měl odesílat pouze sestavy, které jsou související tooits vlastní stavu.</span><span class="sxs-lookup"><span data-stu-id="e57b5-114">Ideally, however, service code should only send reports that are related tooits own health.</span></span>
* <span data-ttu-id="e57b5-115">Použití hello rozhraní REST API na hello clusteru, aplikace, nasazení aplikace, služby, balíček služby, oddílu, repliky nebo úrovně uzlu.</span><span class="sxs-lookup"><span data-stu-id="e57b5-115">Use hello REST APIs at hello cluster, application, deployed application, service, service package, partition, replica, or node levels.</span></span> <span data-ttu-id="e57b5-116">To může být použité tooreport stav od do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e57b5-116">This can be used tooreport health from within a container.</span></span>

<span data-ttu-id="e57b5-117">Tento článek vás provede příklad, který hlásí stav z kódu služby hello.</span><span class="sxs-lookup"><span data-stu-id="e57b5-117">This article walks you through an example that reports health from hello service code.</span></span> <span data-ttu-id="e57b5-118">Hello příklad také ukazuje, jak hello nástroje poskytované subsystémem pro Service Fabric lze použít toocheck hello stav.</span><span class="sxs-lookup"><span data-stu-id="e57b5-118">hello example also shows how hello tools provided by Service Fabric can be used toocheck hello health status.</span></span> <span data-ttu-id="e57b5-119">Tento článek je určený toobe možnosti Service Fabric monitorování stavu toohello rychlý úvod.</span><span class="sxs-lookup"><span data-stu-id="e57b5-119">This article is intended toobe a quick introduction toohello health monitoring capabilities of Service Fabric.</span></span> <span data-ttu-id="e57b5-120">Podrobnější informace si můžete přečíst hello řadu podrobných článků o stavu, které začínají hello odkaz na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="e57b5-120">For more detailed information, you can read hello series of in-depth articles about health that start with hello link at hello end of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e57b5-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e57b5-121">Prerequisites</span></span>
<span data-ttu-id="e57b5-122">Musíte mít nainstalované tyto položky hello:</span><span class="sxs-lookup"><span data-stu-id="e57b5-122">You must have hello following installed:</span></span>

* <span data-ttu-id="e57b5-123">Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e57b5-123">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="e57b5-124">Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="e57b5-124">Service Fabric SDK</span></span>

## <a name="toocreate-a-local-secure-dev-cluster"></a><span data-ttu-id="e57b5-125">toocreate cluster místní zabezpečené vývojářů</span><span class="sxs-lookup"><span data-stu-id="e57b5-125">toocreate a local secure dev cluster</span></span>
* <span data-ttu-id="e57b5-126">Otevřete prostředí PowerShell s oprávněními správce a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="e57b5-126">Open PowerShell with admin privileges, and run hello following commands:</span></span>

![Příkazy, které zobrazují, jak toocreate cluster zabezpečené vývojářů](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="toodeploy-an-application-and-check-its-health"></a><span data-ttu-id="e57b5-128">toodeploy aplikaci a zkontrolujte jeho stav</span><span class="sxs-lookup"><span data-stu-id="e57b5-128">toodeploy an application and check its health</span></span>
1. <span data-ttu-id="e57b5-129">Otevřete Visual Studio jako správce.</span><span class="sxs-lookup"><span data-stu-id="e57b5-129">Open Visual Studio as an administrator.</span></span>
2. <span data-ttu-id="e57b5-130">Vytvoření projektu pomocí hello **stavové služby** šablony.</span><span class="sxs-lookup"><span data-stu-id="e57b5-130">Create a project by using hello **Stateful Service** template.</span></span>
   
    ![Vytvoření aplikace Service Fabric pomocí stavové služby](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. <span data-ttu-id="e57b5-132">Stiskněte klávesu **F5** toorun hello aplikace v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="e57b5-132">Press **F5** toorun hello application in debug mode.</span></span> <span data-ttu-id="e57b5-133">Hello aplikace je nasazená toohello místní cluster.</span><span class="sxs-lookup"><span data-stu-id="e57b5-133">hello application is deployed toohello local cluster.</span></span>
4. <span data-ttu-id="e57b5-134">Po hello aplikace spuštěna, klikněte pravým tlačítkem na ikonu Local Cluster Manager hello v oznamovací oblasti hello a vyberte možnost **spravovat místní Cluster** z hello místní nabídky tooopen Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="e57b5-134">After hello application is running, right-click hello Local Cluster Manager icon in hello notification area and select **Manage Local Cluster** from hello shortcut menu tooopen Service Fabric Explorer.</span></span>
   
    ![Otevření Service Fabric Explorer z oznamovací oblasti](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. <span data-ttu-id="e57b5-136">Stav aplikace Hello má být zobrazena jako tuto bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="e57b5-136">hello application health should be displayed as in this image.</span></span> <span data-ttu-id="e57b5-137">V tomto okamžiku musí být aplikace hello bez chyb v pořádku.</span><span class="sxs-lookup"><span data-stu-id="e57b5-137">At this time, hello application should be healthy with no errors.</span></span>
   
    ![V pořádku aplikace v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. <span data-ttu-id="e57b5-139">Můžete také zkontrolovat hello stavu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e57b5-139">You can also check hello health by using PowerShell.</span></span> <span data-ttu-id="e57b5-140">Můžete použít ```Get-ServiceFabricApplicationHealth``` toocheck stavu aplikace a můžete použít ```Get-ServiceFabricServiceHealth``` toocheck služby stavu.</span><span class="sxs-lookup"><span data-stu-id="e57b5-140">You can use ```Get-ServiceFabricApplicationHealth``` toocheck an application's health, and you can use ```Get-ServiceFabricServiceHealth``` toocheck a service's health.</span></span> <span data-ttu-id="e57b5-141">Sestava stavu pro hello stejná aplikace v prostředí PowerShell je na tomto obrázku Hello.</span><span class="sxs-lookup"><span data-stu-id="e57b5-141">hello health report for hello same application in PowerShell is in this image.</span></span>
   
    ![V pořádku aplikace v prostředí PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="tooadd-custom-health-events-tooyour-service-code"></a><span data-ttu-id="e57b5-143">tooadd vlastní stavu události tooyour služby kódu</span><span class="sxs-lookup"><span data-stu-id="e57b5-143">tooadd custom health events tooyour service code</span></span>
<span data-ttu-id="e57b5-144">šablony projektů Hello Service Fabric v sadě Visual Studio obsahovat ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="e57b5-144">hello Service Fabric project templates in Visual Studio contain sample code.</span></span> <span data-ttu-id="e57b5-145">Hello následující kroky ukazují, jak může hlásit události vlastní stavu z kódu vaší služby.</span><span class="sxs-lookup"><span data-stu-id="e57b5-145">hello following steps show how you can report custom health events from your service code.</span></span> <span data-ttu-id="e57b5-146">Tyto zprávy zobrazí automaticky v hello standardní nástroje pro sledování, že poskytuje Service Fabric, jako je Service Fabric Exploreru, zobrazení stavu Azure portálu a prostředí PowerShell stavu.</span><span class="sxs-lookup"><span data-stu-id="e57b5-146">Such reports show up automatically in hello standard tools for health monitoring that Service Fabric provides, such as Service Fabric Explorer, Azure portal health view, and PowerShell.</span></span>

1. <span data-ttu-id="e57b5-147">Znovu otevřete hello aplikace, kterou jste vytvořili dříve v sadě Visual Studio, nebo vytvořit novou aplikaci pomocí hello **stavové služby** šablony sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e57b5-147">Reopen hello application that you created previously in Visual Studio, or create a new application by using hello **Stateful Service** Visual Studio template.</span></span>
2. <span data-ttu-id="e57b5-148">Otevřete soubor Stateful1.cs hello a najde hello `myDictionary.TryGetValueAsync` volání v hello `RunAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="e57b5-148">Open hello Stateful1.cs file, and find hello `myDictionary.TryGetValueAsync` call in hello `RunAsync` method.</span></span> <span data-ttu-id="e57b5-149">Uvidíte, že tato metoda vrátí hodnotu `result` blokování protože hello klíče logiku v této aplikaci je tookeep počet spuštění text hello aktuální hodnota čítače hello.</span><span class="sxs-lookup"><span data-stu-id="e57b5-149">You can see that this method returns a `result` that holds hello current value of hello counter because hello key logic in this application is tookeep a count running.</span></span> <span data-ttu-id="e57b5-150">Pokud to byly reálné aplikaci a hello nedostatečná výsledek určený k selhání, je vhodnější tooflag tuto událost.</span><span class="sxs-lookup"><span data-stu-id="e57b5-150">If this were a real application, and if hello lack of result represented a failure, you would want tooflag that event.</span></span>
3. <span data-ttu-id="e57b5-151">tooreport událost stavu, pokud chybí hello výsledku představuje selhání, přidejte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="e57b5-151">tooreport a health event when hello lack of result represents a failure, add hello following steps.</span></span>
   
    <span data-ttu-id="e57b5-152">a.</span><span class="sxs-lookup"><span data-stu-id="e57b5-152">a.</span></span> <span data-ttu-id="e57b5-153">Přidat hello `System.Fabric.Health` obor názvů toohello Stateful1.cs souboru.</span><span class="sxs-lookup"><span data-stu-id="e57b5-153">Add hello `System.Fabric.Health` namespace toohello Stateful1.cs file.</span></span>
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    <span data-ttu-id="e57b5-154">b.</span><span class="sxs-lookup"><span data-stu-id="e57b5-154">b.</span></span> <span data-ttu-id="e57b5-155">Přidejte následující kód po hello hello `myDictionary.TryGetValueAsync` volání</span><span class="sxs-lookup"><span data-stu-id="e57b5-155">Add hello following code after hello `myDictionary.TryGetValueAsync` call</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    <span data-ttu-id="e57b5-156">Vytvoříme sestavy stavu repliky, protože je právě nahlásila stavové služby.</span><span class="sxs-lookup"><span data-stu-id="e57b5-156">We report replica health because it's being reported from a stateful service.</span></span> <span data-ttu-id="e57b5-157">Hello `HealthInformation` parametr ukládá informace o problému hello stavu, který je hlášena.</span><span class="sxs-lookup"><span data-stu-id="e57b5-157">hello `HealthInformation` parameter stores information about hello health issue that's being reported.</span></span>
   
    <span data-ttu-id="e57b5-158">Pokud jste vytvořili bezstavové služby, použijte následující kód hello</span><span class="sxs-lookup"><span data-stu-id="e57b5-158">If you had created a stateless service, use hello following code</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. <span data-ttu-id="e57b5-159">Pokud vaše služba je spuštěn s oprávněními správce, nebo pokud hello clusteru není [zabezpečené](service-fabric-cluster-security.md), můžete použít také `FabricClient` tooreport stavu, jak je znázorněno v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="e57b5-159">If your service is running with admin privileges or if hello cluster is not [secure](service-fabric-cluster-security.md), you can also use `FabricClient` tooreport health as shown in hello following steps.</span></span>  
   
    <span data-ttu-id="e57b5-160">a.</span><span class="sxs-lookup"><span data-stu-id="e57b5-160">a.</span></span> <span data-ttu-id="e57b5-161">Vytvoření hello `FabricClient` instance po hello `var myDictionary` deklarace.</span><span class="sxs-lookup"><span data-stu-id="e57b5-161">Create hello `FabricClient` instance after hello `var myDictionary` declaration.</span></span>
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    <span data-ttu-id="e57b5-162">b.</span><span class="sxs-lookup"><span data-stu-id="e57b5-162">b.</span></span> <span data-ttu-id="e57b5-163">Přidejte následující kód po hello hello `myDictionary.TryGetValueAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="e57b5-163">Add hello following code after hello `myDictionary.TryGetValueAsync` call.</span></span>
   
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
5. <span data-ttu-id="e57b5-164">Pojďme simulovat tuto chybu, abyste viděli, zobrazí v hello nástrojů pro monitorování stavu.</span><span class="sxs-lookup"><span data-stu-id="e57b5-164">Let's simulate this failure and see it show up in hello health monitoring tools.</span></span> <span data-ttu-id="e57b5-165">toosimulate hello selhání, komentář hello první řádek ve stavu hello reporting kód, který jste přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="e57b5-165">toosimulate hello failure, comment out hello first line in hello health reporting code that you added earlier.</span></span> <span data-ttu-id="e57b5-166">Po komentář první řádek hello hello kód bude vypadat jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="e57b5-166">After you comment out hello first line, hello code will look like hello following example.</span></span>
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   <span data-ttu-id="e57b5-167">Tento kód aktivuje se pokaždé, když sestava stavu hello `RunAsync` provede.</span><span class="sxs-lookup"><span data-stu-id="e57b5-167">This code fires hello health report each time `RunAsync` executes.</span></span> <span data-ttu-id="e57b5-168">Když provedete hello změnit, stiskněte klávesu **F5** toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e57b5-168">After you make hello change, press **F5** toorun hello application.</span></span>
6. <span data-ttu-id="e57b5-169">Po hello aplikace běží, otevřete Service Fabric Explorer toocheck hello stavu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e57b5-169">After hello application is running, open Service Fabric Explorer toocheck hello health of hello application.</span></span> <span data-ttu-id="e57b5-170">Tentokrát Service Fabric Explorer ukazuje, že je aplikace hello není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="e57b5-170">This time, Service Fabric Explorer shows that hello application is unhealthy.</span></span> <span data-ttu-id="e57b5-171">Důvodem je chyba hello, která byla nahlášena z kódu hello jsme přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="e57b5-171">This is because of hello error that was reported from hello code that we added previously.</span></span>
   
    ![Není v pořádku aplikace v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. <span data-ttu-id="e57b5-173">Pokud vyberete hello primární repliky v zobrazení stromu hello Service Fabric Explorer, zobrazí se **stav** příliš označuje k chybě.</span><span class="sxs-lookup"><span data-stu-id="e57b5-173">If you select hello primary replica in hello tree view of Service Fabric Explorer, you will see that **Health State** indicates an error, too.</span></span> <span data-ttu-id="e57b5-174">Service Fabric Explorer také zobrazuje podrobnosti, které byly přidány toohello sestava stavu hello `HealthInformation` parametr v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="e57b5-174">Service Fabric Explorer also displays hello health report details that were added toohello `HealthInformation` parameter in hello code.</span></span> <span data-ttu-id="e57b5-175">Zobrazí se text hello stejné sestavy stavu v prostředí PowerShell a hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e57b5-175">You can see hello same health reports in PowerShell and hello Azure portal.</span></span>
   
    ![Stav repliky v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

<span data-ttu-id="e57b5-177">Tato sestava zůstane v hello správce stavu, dokud je nahrazen jinou sestavu nebo dokud nebude tato replika se odstraní.</span><span class="sxs-lookup"><span data-stu-id="e57b5-177">This report remains in hello health manager until it is replaced by another report or until this replica is deleted.</span></span> <span data-ttu-id="e57b5-178">Protože jsme nenastavili `TimeToLive` pro tuto sestavu stavu v hello `HealthInformation` objektu hello sestavy nikdy nevyprší.</span><span class="sxs-lookup"><span data-stu-id="e57b5-178">Because we did not set `TimeToLive` for this health report in hello `HealthInformation` object, hello report never expires.</span></span>

<span data-ttu-id="e57b5-179">Doporučujeme vám, že by měl hlásí stav na hello nejvíce podrobné úrovni, které v tomto případě je hello repliky.</span><span class="sxs-lookup"><span data-stu-id="e57b5-179">We recommend that health should be reported on hello most granular level, which in this case is hello replica.</span></span> <span data-ttu-id="e57b5-180">Můžete také sestavy stavu `Partition`.</span><span class="sxs-lookup"><span data-stu-id="e57b5-180">You can also report health on `Partition`.</span></span>

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

<span data-ttu-id="e57b5-181">Stav tooreport na `Application`, `DeployedApplication`, a `DeployedServicePackage`, použijte `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="e57b5-181">tooreport health on `Application`, `DeployedApplication`, and `DeployedServicePackage`, use  `CodePackageActivationContext`.</span></span>

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a><span data-ttu-id="e57b5-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e57b5-182">Next steps</span></span>
* [<span data-ttu-id="e57b5-183">Podrobné informace o stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="e57b5-183">Deep dive on Service Fabric health</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="e57b5-184">REST API pro vytváření sestav stavu služby</span><span class="sxs-lookup"><span data-stu-id="e57b5-184">REST API for reporting service health</span></span>](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [<span data-ttu-id="e57b5-185">REST API pro vytváření sestav stavu aplikací</span><span class="sxs-lookup"><span data-stu-id="e57b5-185">REST API for reporting application health</span></span>](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

