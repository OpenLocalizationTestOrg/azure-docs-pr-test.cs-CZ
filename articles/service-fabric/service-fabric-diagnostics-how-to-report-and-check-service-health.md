---
title: "Vytvoření sestavy a zkontrolujte stav s Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak chcete zasílat zprávy o stavu z vašeho kódu služby a jak zkontrolovat stav služby pomocí nástroje pro sledování stavu, které poskytuje Azure Service Fabric."
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
ms.openlocfilehash: 83981d5bec14c06c509f1a8a4153dc23298f5ce0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="report-and-check-service-health"></a><span data-ttu-id="16ea6-103">Hlášení a kontrola stavu služeb</span><span class="sxs-lookup"><span data-stu-id="16ea6-103">Report and check service health</span></span>
<span data-ttu-id="16ea6-104">Pokud vaše služby dojde k potížím, budete moci reagovat na a opravte incidentů a výpadky závisí na budete moci rychle zjistit problémy.</span><span class="sxs-lookup"><span data-stu-id="16ea6-104">When your services encounter problems, your ability to respond to and fix incidents and outages depends on your ability to detect the issues quickly.</span></span> <span data-ttu-id="16ea6-105">Pokud hlášení problémů a selhání do Azure Service Fabric správce stavu z vašeho kódu služby můžete použít standardní stavu monitorování nástroje, které Service Fabric nabízí zkontrolovat stav.</span><span class="sxs-lookup"><span data-stu-id="16ea6-105">If you report problems and failures to the Azure Service Fabric health manager from your service code, you can use standard health monitoring tools that Service Fabric provides to check the health status.</span></span>

<span data-ttu-id="16ea6-106">Existují tři způsoby, můžete odesílat zprávy o stavu ze služby:</span><span class="sxs-lookup"><span data-stu-id="16ea6-106">There are three ways that you can report health from the service:</span></span>

* <span data-ttu-id="16ea6-107">Použití [oddílu](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) nebo [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objekty.</span><span class="sxs-lookup"><span data-stu-id="16ea6-107">Use [Partition](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) or [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objects.</span></span>  
  <span data-ttu-id="16ea6-108">Můžete použít `Partition` a `CodePackageActivationContext` objekty hlášení stavu elementů, které jsou součástí aktuální kontext.</span><span class="sxs-lookup"><span data-stu-id="16ea6-108">You can use the `Partition` and `CodePackageActivationContext` objects to report the health of elements that are part of the current context.</span></span> <span data-ttu-id="16ea6-109">Například kód, který spouští jako součást repliku můžete sestavy stavu pouze repliky, oddílu, který patří do a aplikace, která je součástí.</span><span class="sxs-lookup"><span data-stu-id="16ea6-109">For example, code that runs as part of a replica can report health only on that replica, the partition that it belongs to, and the application that it is a part of.</span></span>
* <span data-ttu-id="16ea6-110">Použití `FabricClient`.</span><span class="sxs-lookup"><span data-stu-id="16ea6-110">Use `FabricClient`.</span></span>   
  <span data-ttu-id="16ea6-111">Můžete použít `FabricClient` stav sestavy z kódu služby clusteru, není-li [zabezpečené](service-fabric-cluster-security.md) nebo pokud je služba spuštěna s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="16ea6-111">You can use `FabricClient` to report health from the service code if the cluster is not [secure](service-fabric-cluster-security.md) or if the service is running with admin privileges.</span></span> <span data-ttu-id="16ea6-112">Většina scénářích reálného světa nepoužívejte zabezpečená clustery, nebo zadejte oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="16ea6-112">Most real-world scenarios do not use unsecured clusters, or provide admin privileges.</span></span> <span data-ttu-id="16ea6-113">S `FabricClient`, stavu může podávat Každá entita, která je součástí clusteru.</span><span class="sxs-lookup"><span data-stu-id="16ea6-113">With `FabricClient`, you can report health on any entity that is a part of the cluster.</span></span> <span data-ttu-id="16ea6-114">V ideálním případě by však kódu služby by měl odesílat pouze sestavy, které se vztahují k vlastní stavu.</span><span class="sxs-lookup"><span data-stu-id="16ea6-114">Ideally, however, service code should only send reports that are related to its own health.</span></span>
* <span data-ttu-id="16ea6-115">Pomocí rozhraní API REST v clusteru, aplikace, nasazení aplikace, služby, balíček služby, oddíl, repliky nebo úrovně uzlu.</span><span class="sxs-lookup"><span data-stu-id="16ea6-115">Use the REST APIs at the cluster, application, deployed application, service, service package, partition, replica, or node levels.</span></span> <span data-ttu-id="16ea6-116">To může používají k hlášení stavu z do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="16ea6-116">This can be used to report health from within a container.</span></span>

<span data-ttu-id="16ea6-117">Tento článek vás provede příklad, který hlásí stav z kódu služby.</span><span class="sxs-lookup"><span data-stu-id="16ea6-117">This article walks you through an example that reports health from the service code.</span></span> <span data-ttu-id="16ea6-118">Tento příklad také ukazuje, jak lze pomocí nástroje poskytované subsystémem pro Service Fabric zkontrolujte stav.</span><span class="sxs-lookup"><span data-stu-id="16ea6-118">The example also shows how the tools provided by Service Fabric can be used to check the health status.</span></span> <span data-ttu-id="16ea6-119">Tento článek je určený jako rychlý úvod do stavu možnosti Service Fabric monitorování.</span><span class="sxs-lookup"><span data-stu-id="16ea6-119">This article is intended to be a quick introduction to the health monitoring capabilities of Service Fabric.</span></span> <span data-ttu-id="16ea6-120">Podrobnější informace si můžete přečíst řadu podrobných článků o stavu, které začínají na odkaz na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="16ea6-120">For more detailed information, you can read the series of in-depth articles about health that start with the link at the end of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16ea6-121">Požadavky</span><span class="sxs-lookup"><span data-stu-id="16ea6-121">Prerequisites</span></span>
<span data-ttu-id="16ea6-122">Musíte mít nainstalované tyto položky:</span><span class="sxs-lookup"><span data-stu-id="16ea6-122">You must have the following installed:</span></span>

* <span data-ttu-id="16ea6-123">Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="16ea6-123">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="16ea6-124">Service Fabric SDK</span><span class="sxs-lookup"><span data-stu-id="16ea6-124">Service Fabric SDK</span></span>

## <a name="to-create-a-local-secure-dev-cluster"></a><span data-ttu-id="16ea6-125">K vytvoření clusteru místní zabezpečené vývojářů</span><span class="sxs-lookup"><span data-stu-id="16ea6-125">To create a local secure dev cluster</span></span>
* <span data-ttu-id="16ea6-126">Otevřete prostředí PowerShell s oprávněními správce a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="16ea6-126">Open PowerShell with admin privileges, and run the following commands:</span></span>

![Příkazy, které ukazují, jak vytvořit cluster zabezpečené vývojářů](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a><span data-ttu-id="16ea6-128">Pokud chcete nasadit aplikaci a zkontrolovat jeho stav</span><span class="sxs-lookup"><span data-stu-id="16ea6-128">To deploy an application and check its health</span></span>
1. <span data-ttu-id="16ea6-129">Otevřete Visual Studio jako správce.</span><span class="sxs-lookup"><span data-stu-id="16ea6-129">Open Visual Studio as an administrator.</span></span>
2. <span data-ttu-id="16ea6-130">Vytvoření projektu pomocí **stavové služby** šablony.</span><span class="sxs-lookup"><span data-stu-id="16ea6-130">Create a project by using the **Stateful Service** template.</span></span>
   
    ![Vytvoření aplikace Service Fabric pomocí stavové služby](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. <span data-ttu-id="16ea6-132">Stiskněte klávesu **F5** ke spuštění aplikace v režimu ladění.</span><span class="sxs-lookup"><span data-stu-id="16ea6-132">Press **F5** to run the application in debug mode.</span></span> <span data-ttu-id="16ea6-133">Aplikace je nasazený na místním clusteru.</span><span class="sxs-lookup"><span data-stu-id="16ea6-133">The application is deployed to the local cluster.</span></span>
4. <span data-ttu-id="16ea6-134">Jakmile je aplikace spuštěna, klikněte pravým tlačítkem na ikonu Local Cluster Manager v oznamovací oblasti a vyberte **spravovat místní Cluster** z místní nabídky k otevření Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="16ea6-134">After the application is running, right-click the Local Cluster Manager icon in the notification area and select **Manage Local Cluster** from the shortcut menu to open Service Fabric Explorer.</span></span>
   
    ![Otevření Service Fabric Explorer z oznamovací oblasti](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. <span data-ttu-id="16ea6-136">Stav aplikace má být zobrazena jako tuto bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="16ea6-136">The application health should be displayed as in this image.</span></span> <span data-ttu-id="16ea6-137">V tomto okamžiku by aplikace měla být v pořádku bez chyb.</span><span class="sxs-lookup"><span data-stu-id="16ea6-137">At this time, the application should be healthy with no errors.</span></span>
   
    ![V pořádku aplikace v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. <span data-ttu-id="16ea6-139">Můžete také zkontrolovat stav pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16ea6-139">You can also check the health by using PowerShell.</span></span> <span data-ttu-id="16ea6-140">Můžete použít ```Get-ServiceFabricApplicationHealth``` ke kontrole stavu aplikace a vy můžete použít ```Get-ServiceFabricServiceHealth``` ke kontrole stavu služby.</span><span class="sxs-lookup"><span data-stu-id="16ea6-140">You can use ```Get-ServiceFabricApplicationHealth``` to check an application's health, and you can use ```Get-ServiceFabricServiceHealth``` to check a service's health.</span></span> <span data-ttu-id="16ea6-141">Sestava stavu pro stejnou aplikaci v prostředí PowerShell je na tomto obrázku.</span><span class="sxs-lookup"><span data-stu-id="16ea6-141">The health report for the same application in PowerShell is in this image.</span></span>
   
    ![V pořádku aplikace v prostředí PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a><span data-ttu-id="16ea6-143">Přidání události vlastní stavu do kódu služby</span><span class="sxs-lookup"><span data-stu-id="16ea6-143">To add custom health events to your service code</span></span>
<span data-ttu-id="16ea6-144">Šablony projektů Service Fabric v sadě Visual Studio obsahovat ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="16ea6-144">The Service Fabric project templates in Visual Studio contain sample code.</span></span> <span data-ttu-id="16ea6-145">Následující postup ukazuje, jak může hlásit události vlastní stavu z kódu vaší služby.</span><span class="sxs-lookup"><span data-stu-id="16ea6-145">The following steps show how you can report custom health events from your service code.</span></span> <span data-ttu-id="16ea6-146">Tyto zprávy zobrazí automaticky v standardní nástroje pro sledování stavu, že poskytuje Service Fabric, jako je Service Fabric Exploreru, zobrazení stavu Azure portálu a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16ea6-146">Such reports show up automatically in the standard tools for health monitoring that Service Fabric provides, such as Service Fabric Explorer, Azure portal health view, and PowerShell.</span></span>

1. <span data-ttu-id="16ea6-147">Otevřete aplikaci, která jste předtím vytvořili v sadě Visual Studio, nebo vytvořit novou aplikaci pomocí **stavové služby** šablony sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16ea6-147">Reopen the application that you created previously in Visual Studio, or create a new application by using the **Stateful Service** Visual Studio template.</span></span>
2. <span data-ttu-id="16ea6-148">Otevřete soubor Stateful1.cs a najít `myDictionary.TryGetValueAsync` volání v `RunAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="16ea6-148">Open the Stateful1.cs file, and find the `myDictionary.TryGetValueAsync` call in the `RunAsync` method.</span></span> <span data-ttu-id="16ea6-149">Uvidíte, že tato metoda vrátí hodnotu `result` , protože logice klíče v této aplikaci je udržovat počet spuštění udržuje aktuální hodnotu čítače.</span><span class="sxs-lookup"><span data-stu-id="16ea6-149">You can see that this method returns a `result` that holds the current value of the counter because the key logic in this application is to keep a count running.</span></span> <span data-ttu-id="16ea6-150">Pokud to byly reálné aplikaci a nedostatek výsledek určený k selhání, je vhodné příznak tuto událost.</span><span class="sxs-lookup"><span data-stu-id="16ea6-150">If this were a real application, and if the lack of result represented a failure, you would want to flag that event.</span></span>
3. <span data-ttu-id="16ea6-151">K hlášení o stavu událost při chybějícím výsledek představuje selhání, přidáte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="16ea6-151">To report a health event when the lack of result represents a failure, add the following steps.</span></span>
   
    <span data-ttu-id="16ea6-152">a.</span><span class="sxs-lookup"><span data-stu-id="16ea6-152">a.</span></span> <span data-ttu-id="16ea6-153">Přidat `System.Fabric.Health` oboru názvů do souboru Stateful1.cs.</span><span class="sxs-lookup"><span data-stu-id="16ea6-153">Add the `System.Fabric.Health` namespace to the Stateful1.cs file.</span></span>
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    <span data-ttu-id="16ea6-154">b.</span><span class="sxs-lookup"><span data-stu-id="16ea6-154">b.</span></span> <span data-ttu-id="16ea6-155">Přidejte následující kód po `myDictionary.TryGetValueAsync` volání</span><span class="sxs-lookup"><span data-stu-id="16ea6-155">Add the following code after the `myDictionary.TryGetValueAsync` call</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    <span data-ttu-id="16ea6-156">Vytvoříme sestavy stavu repliky, protože je právě nahlásila stavové služby.</span><span class="sxs-lookup"><span data-stu-id="16ea6-156">We report replica health because it's being reported from a stateful service.</span></span> <span data-ttu-id="16ea6-157">`HealthInformation` Parametr ukládá informace o stavu problém, který je hlášena.</span><span class="sxs-lookup"><span data-stu-id="16ea6-157">The `HealthInformation` parameter stores information about the health issue that's being reported.</span></span>
   
    <span data-ttu-id="16ea6-158">Pokud jste vytvořili bezstavové služby, použijte následující kód</span><span class="sxs-lookup"><span data-stu-id="16ea6-158">If you had created a stateless service, use the following code</span></span>
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. <span data-ttu-id="16ea6-159">Pokud vaše služba je spuštěn s oprávněními správce, nebo pokud clusteru není [zabezpečené](service-fabric-cluster-security.md), můžete použít také `FabricClient` stav sestavy, jak je znázorněno v následujícím postupu.</span><span class="sxs-lookup"><span data-stu-id="16ea6-159">If your service is running with admin privileges or if the cluster is not [secure](service-fabric-cluster-security.md), you can also use `FabricClient` to report health as shown in the following steps.</span></span>  
   
    <span data-ttu-id="16ea6-160">a.</span><span class="sxs-lookup"><span data-stu-id="16ea6-160">a.</span></span> <span data-ttu-id="16ea6-161">Vytvořte `FabricClient` instance po `var myDictionary` deklarace.</span><span class="sxs-lookup"><span data-stu-id="16ea6-161">Create the `FabricClient` instance after the `var myDictionary` declaration.</span></span>
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    <span data-ttu-id="16ea6-162">b.</span><span class="sxs-lookup"><span data-stu-id="16ea6-162">b.</span></span> <span data-ttu-id="16ea6-163">Přidejte následující kód po `myDictionary.TryGetValueAsync` volání.</span><span class="sxs-lookup"><span data-stu-id="16ea6-163">Add the following code after the `myDictionary.TryGetValueAsync` call.</span></span>
   
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
5. <span data-ttu-id="16ea6-164">Pojďme simulovat tuto chybu, abyste viděli, zobrazí v nástroje pro sledování stavu.</span><span class="sxs-lookup"><span data-stu-id="16ea6-164">Let's simulate this failure and see it show up in the health monitoring tools.</span></span> <span data-ttu-id="16ea6-165">Simulace selhání komentář v prvním řádku kódu generování sestav stavu, které jste přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="16ea6-165">To simulate the failure, comment out the first line in the health reporting code that you added earlier.</span></span> <span data-ttu-id="16ea6-166">Po komentář první řádek kód bude vypadat jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="16ea6-166">After you comment out the first line, the code will look like the following example.</span></span>
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   <span data-ttu-id="16ea6-167">Tento kód sestava stavu aktivuje se pokaždé, když `RunAsync` provede.</span><span class="sxs-lookup"><span data-stu-id="16ea6-167">This code fires the health report each time `RunAsync` executes.</span></span> <span data-ttu-id="16ea6-168">Po provedení změny, stiskněte klávesu **F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="16ea6-168">After you make the change, press **F5** to run the application.</span></span>
6. <span data-ttu-id="16ea6-169">Jakmile je aplikace spuštěna, otevřete Service Fabric Explorer ke kontrole stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="16ea6-169">After the application is running, open Service Fabric Explorer to check the health of the application.</span></span> <span data-ttu-id="16ea6-170">Tentokrát Service Fabric Explorer ukazuje, že aplikace je není v pořádku.</span><span class="sxs-lookup"><span data-stu-id="16ea6-170">This time, Service Fabric Explorer shows that the application is unhealthy.</span></span> <span data-ttu-id="16ea6-171">To je kvůli chybě, která byla nahlášena z kódu jsme přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="16ea6-171">This is because of the error that was reported from the code that we added previously.</span></span>
   
    ![Není v pořádku aplikace v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. <span data-ttu-id="16ea6-173">Pokud vyberete primární repliky v zobrazení stromu Service Fabric Explorer, zobrazí se **stav** příliš označuje k chybě.</span><span class="sxs-lookup"><span data-stu-id="16ea6-173">If you select the primary replica in the tree view of Service Fabric Explorer, you will see that **Health State** indicates an error, too.</span></span> <span data-ttu-id="16ea6-174">Také zobrazuje podrobnosti o stavu sestavy, které byly přidány do Service Fabric Explorer `HealthInformation` parametr v kódu.</span><span class="sxs-lookup"><span data-stu-id="16ea6-174">Service Fabric Explorer also displays the health report details that were added to the `HealthInformation` parameter in the code.</span></span> <span data-ttu-id="16ea6-175">Zobrazí se stejné sestavy stavu v prostředí PowerShell a portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="16ea6-175">You can see the same health reports in PowerShell and the Azure portal.</span></span>
   
    ![Stav repliky v Service Fabric Exploreru](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

<span data-ttu-id="16ea6-177">Tato sestava zůstane v správce stavu, dokud je nahrazen jinou sestavu nebo dokud nebude tato replika se odstraní.</span><span class="sxs-lookup"><span data-stu-id="16ea6-177">This report remains in the health manager until it is replaced by another report or until this replica is deleted.</span></span> <span data-ttu-id="16ea6-178">Protože jsme nenastavili `TimeToLive` pro tuto sestavu stavu v `HealthInformation` objektu sestavy nikdy nevyprší.</span><span class="sxs-lookup"><span data-stu-id="16ea6-178">Because we did not set `TimeToLive` for this health report in the `HealthInformation` object, the report never expires.</span></span>

<span data-ttu-id="16ea6-179">Doporučujeme vám, že by měl na nejvíce podrobné úrovni, které v tomto případě je replika hlásí stav.</span><span class="sxs-lookup"><span data-stu-id="16ea6-179">We recommend that health should be reported on the most granular level, which in this case is the replica.</span></span> <span data-ttu-id="16ea6-180">Můžete také sestavy stavu `Partition`.</span><span class="sxs-lookup"><span data-stu-id="16ea6-180">You can also report health on `Partition`.</span></span>

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

<span data-ttu-id="16ea6-181">Stav sestavy na `Application`, `DeployedApplication`, a `DeployedServicePackage`, použijte `CodePackageActivationContext`.</span><span class="sxs-lookup"><span data-stu-id="16ea6-181">To report health on `Application`, `DeployedApplication`, and `DeployedServicePackage`, use  `CodePackageActivationContext`.</span></span>

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a><span data-ttu-id="16ea6-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16ea6-182">Next steps</span></span>
* [<span data-ttu-id="16ea6-183">Podrobné informace o stavu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="16ea6-183">Deep dive on Service Fabric health</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="16ea6-184">REST API pro vytváření sestav stavu služby</span><span class="sxs-lookup"><span data-stu-id="16ea6-184">REST API for reporting service health</span></span>](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [<span data-ttu-id="16ea6-185">REST API pro vytváření sestav stavu aplikací</span><span class="sxs-lookup"><span data-stu-id="16ea6-185">REST API for reporting application health</span></span>](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

