---
title: "aaaHow toouse s cloudovými službami Azure diagnostics (.NET) | Microsoft Docs"
description: "Pomocí Azure diagnostics toogather dat z Azure cloud Services pro ladění, měření výkonu, monitorování, analýza provozu a další."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 89623a0e-4e78-4b67-a446-7d19a35a44be
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: robb
ms.openlocfilehash: 1525eac1e85955d8f05aa21a9805e0a80d0e4bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a><span data-ttu-id="43be5-103">Povolení Azure Diagnostics v cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="43be5-103">Enabling Azure Diagnostics in Azure Cloud Services</span></span>
<span data-ttu-id="43be5-104">V tématu [přehled Azure Diagnostics](../azure-diagnostics.md) pozadí na Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="43be5-104">See [Azure Diagnostics Overview](../azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-tooenable-diagnostics-in-a-worker-role"></a><span data-ttu-id="43be5-105">Jak tooEnable diagnostiky v roli pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="43be5-105">How tooEnable Diagnostics in a Worker Role</span></span>
<span data-ttu-id="43be5-106">Tento návod popisuje, jak hello tooimplement o roli pracovního procesu systému Azure, který vysílá telemetrická data pomocí .NET EventSource – třída.</span><span class="sxs-lookup"><span data-stu-id="43be5-106">This walkthrough describes how tooimplement an Azure worker role that emits telemetry data using hello .NET EventSource class.</span></span> <span data-ttu-id="43be5-107">Azure Diagnostics je použité toocollect hello telemetrická data a uloží jej v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="43be5-107">Azure Diagnostics is used toocollect hello telemetry data and store it in an Azure storage account.</span></span> <span data-ttu-id="43be5-108">Při vytváření role pracovního procesu, Visual Studio automaticky povolí 1.0 diagnostiky jako součást hello řešení v Azure SDK pro .NET 2.4 a starší.</span><span class="sxs-lookup"><span data-stu-id="43be5-108">When creating a worker role, Visual Studio automatically enables Diagnostics 1.0 as part of hello solution in Azure SDKs for .NET 2.4 and earlier.</span></span> <span data-ttu-id="43be5-109">Hello následující pokyny popisují hello proces pro vytváření hello role pracovního procesu, zakázání diagnostiky 1.0 z hello řešení a nasazení diagnostiky 1.2 nebo 1.3 tooyour role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="43be5-109">hello following instructions describe hello process for creating hello worker role, disabling Diagnostics 1.0 from hello solution, and deploying Diagnostics 1.2 or 1.3 tooyour worker role.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="43be5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="43be5-110">Prerequisites</span></span>
<span data-ttu-id="43be5-111">Tento článek předpokládá můžete mít předplatné Azure a jsou pomocí sady Visual Studio s hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="43be5-111">This article assumes you have an Azure subscription and are using Visual Studio with hello Azure SDK.</span></span> <span data-ttu-id="43be5-112">Pokud nemáte předplatné Azure, můžete zaregistrovat pro hello [bezplatné zkušební verze][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="43be5-112">If you do not have an Azure subscription, you can sign up for hello [Free Trial][Free Trial].</span></span> <span data-ttu-id="43be5-113">Zajistěte, aby příliš[nainstalovat a nakonfigurovat Azure PowerShell verze 0.8.7 nebo novější][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="43be5-113">Make sure too[Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-worker-role"></a><span data-ttu-id="43be5-114">Krok 1: Vytvoření Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="43be5-114">Step 1: Create a Worker Role</span></span>
1. <span data-ttu-id="43be5-115">Spusťte **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="43be5-115">Launch **Visual Studio**.</span></span>
2. <span data-ttu-id="43be5-116">Vytvoření **Azure Cloud Service** projekt z hello **cloudu** šablony, která je cílena rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="43be5-116">Create an **Azure Cloud Service** project from hello **Cloud** template that targets .NET Framework 4.5.</span></span>  <span data-ttu-id="43be5-117">Pojmenujte projekt hello "WadExample" a klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="43be5-117">Name hello project "WadExample" and click Ok.</span></span>
3. <span data-ttu-id="43be5-118">Vyberte **Role pracovního procesu** a klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="43be5-118">Select **Worker Role** and click Ok.</span></span> <span data-ttu-id="43be5-119">Vytvoří se Hello projektu.</span><span class="sxs-lookup"><span data-stu-id="43be5-119">hello project will be created.</span></span>
4. <span data-ttu-id="43be5-120">V **Průzkumníku řešení**, dvakrát klikněte na hello **WorkerRole1** vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="43be5-120">In **Solution Explorer**, double-click hello **WorkerRole1** properties file.</span></span>
5. <span data-ttu-id="43be5-121">V hello **konfigurace** kartě, zrušte zaškrtnutí **povolení diagnostiky** toodisable diagnostiky 1.0 (Azure SDK 2.4 a starší).</span><span class="sxs-lookup"><span data-stu-id="43be5-121">In hello **Configuration** tab, un-check **Enable Diagnostics** toodisable Diagnostics 1.0 (Azure SDK 2.4 and earlier).</span></span>
6. <span data-ttu-id="43be5-122">Sestavení vaší tooverify řešení, které máte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="43be5-122">Build your solution tooverify that you have no errors.</span></span>

### <a name="step-2-instrument-your-code"></a><span data-ttu-id="43be5-123">Krok 2: Instrumentace kódu</span><span class="sxs-lookup"><span data-stu-id="43be5-123">Step 2: Instrument your code</span></span>
<span data-ttu-id="43be5-124">Nahraďte obsah hello WorkerRole.cs hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="43be5-124">Replace hello contents of WorkerRole.cs with hello following code.</span></span> <span data-ttu-id="43be5-125">Hello třída SampleEventSourceWriter, dědí z hello [EventSource – třída][EventSource Class], implementuje čtyři metody protokolování: **SendEnums**, **MessageMethod** , **SetOther** a **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="43be5-125">hello class SampleEventSourceWriter, inherited from hello [EventSource Class][EventSource Class], implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="43be5-126">první parametr toohello Hello **WriteEvent** metoda definuje ID hello hello příslušné události.</span><span class="sxs-lookup"><span data-stu-id="43be5-126">hello first parameter toohello **WriteEvent** method defines hello ID for hello respective event.</span></span> <span data-ttu-id="43be5-127">Hello metoda Run implementuje nekonečnou smyčku, která volá všechny hello protokolování metody implementované v hello **SampleEventSourceWriter** třídy každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="43be5-127">hello Run method implements an infinite loop that calls each of hello logging methods implemented in hello **SampleEventSourceWriter** class every 10 seconds.</span></span>

```csharp
using Microsoft.WindowsAzure.ServiceRuntime;
using System;
using System.Diagnostics;
using System.Diagnostics.Tracing;
using System.Net;
using System.Threading;

namespace WorkerRole1
{
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums tooint for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through hello loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set hello maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see hello MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a><span data-ttu-id="43be5-128">Krok 3: Nasaďte své Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="43be5-128">Step 3: Deploy your Worker Role</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. <span data-ttu-id="43be5-129">Nasazení tooAzure vaše pracovní role z Visual Studia výběrem hello **WadExample** projekt v hello Průzkumníku řešení klikněte **publikovat** z hello **sestavení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="43be5-129">Deploy your worker role tooAzure from within Visual Studio by selecting hello **WadExample** project in hello Solution Explorer then **Publish** from hello **Build** menu.</span></span>
2. <span data-ttu-id="43be5-130">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="43be5-130">Choose your subscription.</span></span>
3. <span data-ttu-id="43be5-131">V hello **nastavením publikování v Microsoft Azure** dialogovém okně, vyberte **vytvořit novou...** .</span><span class="sxs-lookup"><span data-stu-id="43be5-131">In hello **Microsoft Azure Publish Settings** dialog, select **Create New…**.</span></span>
4. <span data-ttu-id="43be5-132">V hello **vytvoření cloudové služby a účet úložiště** dialogové okno, zadejte **název** (například "WadExample") a vyberte oblast nebo skupinu vztahů.</span><span class="sxs-lookup"><span data-stu-id="43be5-132">In hello **Create Cloud Service and Storage Account** dialog, enter a **Name** (for example, "WadExample") and select a region or affinity group.</span></span>
5. <span data-ttu-id="43be5-133">Sada hello **prostředí** příliš**pracovní**.</span><span class="sxs-lookup"><span data-stu-id="43be5-133">Set hello **Environment** too**Staging**.</span></span>
6. <span data-ttu-id="43be5-134">Upravit jakékoliv **nastavení** jako vhodné a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="43be5-134">Modify any other **Settings** as appropriate and click **Publish**.</span></span>
7. <span data-ttu-id="43be5-135">Po dokončení nasazení se ověřit v portálu Azure, která Cloudová služba se hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="43be5-135">After deployment has completed, verify in hello Azure portal that your cloud service is in a **Running** state.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-hello-extension"></a><span data-ttu-id="43be5-136">Krok 4: Vytvoření konfiguračního souboru diagnostiky a nainstalovat rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="43be5-136">Step 4: Create your Diagnostics configuration file and install hello extension</span></span>
1. <span data-ttu-id="43be5-137">Stáhněte si definice schématu souboru hello veřejné konfigurace tak, že spustíte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="43be5-137">Download hello public configuration file schema definition by executing hello following PowerShell command:</span></span>

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. <span data-ttu-id="43be5-138">Přidat tooyour souboru XML **WorkerRole1** projektu kliknutím pravým tlačítkem na hello **WorkerRole1** projektu a vyberte **přidat** -> **novou položku...**</span><span class="sxs-lookup"><span data-stu-id="43be5-138">Add an XML file tooyour **WorkerRole1** project by right-clicking on hello **WorkerRole1** project and select **Add** -> **New Item…**</span></span><span data-ttu-id="43be5-139"> -> **Visual C# položky** -> **Data** -> **souboru XML**.</span><span class="sxs-lookup"><span data-stu-id="43be5-139"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="43be5-140">Název souboru hello "WadExample.xml".</span><span class="sxs-lookup"><span data-stu-id="43be5-140">Name hello file "WadExample.xml".</span></span>

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. <span data-ttu-id="43be5-142">Hello WadConfig.xsd přidružte hello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="43be5-142">Associate hello WadConfig.xsd with hello configuration file.</span></span> <span data-ttu-id="43be5-143">Zajistěte, aby byl okno editor WadExample.xml hello hello aktivní okno.</span><span class="sxs-lookup"><span data-stu-id="43be5-143">Make sure hello WadExample.xml editor window is hello active window.</span></span> <span data-ttu-id="43be5-144">Stiskněte klávesu **F4** tooopen hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="43be5-144">Press **F4** tooopen hello **Properties** window.</span></span> <span data-ttu-id="43be5-145">Klikněte na tlačítko hello **schémata** vlastnost hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="43be5-145">Click hello **Schemas** property in hello **Properties** window.</span></span> <span data-ttu-id="43be5-146">Klikněte na tlačítko hello **...**</span><span class="sxs-lookup"><span data-stu-id="43be5-146">Click hello **…**</span></span> <span data-ttu-id="43be5-147">v hello **schémata** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="43be5-147">in hello **Schemas** property.</span></span> <span data-ttu-id="43be5-148">Klikněte na tlačítko hello **přidat...**</span><span class="sxs-lookup"><span data-stu-id="43be5-148">Click hello **Add…**</span></span> <span data-ttu-id="43be5-149">tlačítko a přejděte toohello umístění, kam jste uložili soubor XSD hello a vyberte hello WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="43be5-149">button and navigate toohello location where you saved hello XSD file and select hello file WadConfig.xsd.</span></span> <span data-ttu-id="43be5-150">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="43be5-150">Click **OK**.</span></span>

4. <span data-ttu-id="43be5-151">Nahraďte obsah hello hello WadExample.xml konfigurační soubor s hello následující XML a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="43be5-151">Replace hello contents of hello WadExample.xml configuration file with hello following XML and save hello file.</span></span> <span data-ttu-id="43be5-152">Tento konfigurační soubor definuje několik toocollect čítače výkonu: jeden pro využití procesoru a jeden pro využití paměti.</span><span class="sxs-lookup"><span data-stu-id="43be5-152">This configuration file defines a couple performance counters toocollect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="43be5-153">Potom hello konfigurace definuje hello čtyři události odpovídající metody toohello v hello SampleEventSourceWriter třídy.</span><span class="sxs-lookup"><span data-stu-id="43be5-153">Then hello configuration defines hello four events corresponding toohello methods in hello SampleEventSourceWriter class.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <WadCfg>
    <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
      <PerformanceCounters scheduledTransferPeriod="PT1M">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
        <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
      </PerformanceCounters>
      <EtwProviders>
        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
          <Event id="1" eventDestination="EnumsTable"/>
          <Event id="2" eventDestination="MessageTable"/>
          <Event id="3" eventDestination="SetOtherTable"/>
          <Event id="4" eventDestination="HighFreqTable"/>
          <DefaultEvents eventDestination="DefaultTable" />
        </EtwEventSourceProviderConfiguration>
      </EtwProviders>
    </DiagnosticMonitorConfiguration>
  </WadCfg>
</PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a><span data-ttu-id="43be5-154">Krok 5: Instalace diagnostiky na své Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="43be5-154">Step 5: Install Diagnostics on your Worker Role</span></span>
<span data-ttu-id="43be5-155">Hello rutiny Powershellu pro správu diagnostiky na web nebo worker role jsou: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension a AzureServiceDiagnosticsExtension odebrat.</span><span class="sxs-lookup"><span data-stu-id="43be5-155">hello PowerShell cmdlets for managing Diagnostics on a web or worker role are: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension, and Remove-AzureServiceDiagnosticsExtension.</span></span>

1. <span data-ttu-id="43be5-156">Otevřete prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="43be5-156">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="43be5-157">Spuštění skriptu tooinstall hello diagnostiky ve své role pracovního procesu (Nahraďte *StorageAccountKey* s hello klíč účtu úložiště pro váš účet úložiště wadexample a *config_path* s cestou hello toohello *WadExample.xml* souborů):</span><span class="sxs-lookup"><span data-stu-id="43be5-157">Execute hello script tooinstall Diagnostics on your worker role (replace *StorageAccountKey* with hello storage account key for your wadexample storage account and *config_path* with hello path toohello *WadExample.xml* file):</span></span>

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="43be5-158">Krok 6: Podívejte se na vaše data telemetrie</span><span class="sxs-lookup"><span data-stu-id="43be5-158">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="43be5-159">V sadě Visual Studio hello **Průzkumníka serveru**, přejděte na účet úložiště wadexample toohello.</span><span class="sxs-lookup"><span data-stu-id="43be5-159">In hello Visual Studio **Server Explorer**, navigate toohello wadexample storage account.</span></span> <span data-ttu-id="43be5-160">Po přibližně pět (5) minut spuštěn hello cloudové služby, měli byste vidět hello tabulky **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** a **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="43be5-160">After hello cloud service has been running about five (5) minutes, you should see hello tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="43be5-161">Dvakrát klikněte na hello tabulky tooview hello telemetrie, které se shromáždily.</span><span class="sxs-lookup"><span data-stu-id="43be5-161">Double-click one of hello tables tooview hello telemetry that has been collected.</span></span>

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="43be5-163">Schéma konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="43be5-163">Configuration File Schema</span></span>
<span data-ttu-id="43be5-164">soubor konfigurace diagnostiky Hello definuje hodnoty, které jsou používané tooinitialize konfigurace diagnostiky nastavení při spuštění diagnostiky agenta hello.</span><span class="sxs-lookup"><span data-stu-id="43be5-164">hello Diagnostics configuration file defines values that are used tooinitialize diagnostic configuration settings when hello diagnostics agent starts.</span></span> <span data-ttu-id="43be5-165">V tématu hello [nejnovější – odkaz schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.</span><span class="sxs-lookup"><span data-stu-id="43be5-165">See hello [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="43be5-166">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="43be5-166">Troubleshooting</span></span>
<span data-ttu-id="43be5-167">Pokud máte potíže s, najdete v části [řešení potíží s Azure Diagnostics](../azure-diagnostics-troubleshooting.md) nápovědu pro běžné problémy.</span><span class="sxs-lookup"><span data-stu-id="43be5-167">If you have trouble, see [Troubleshooting Azure Diagnostics](../azure-diagnostics-troubleshooting.md) for help with common problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43be5-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="43be5-168">Next Steps</span></span>
<span data-ttu-id="43be5-169">[Podívejte se do seznamu související Azure články diagnostiky virtuálního počítače](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello data shromažďujete, řešení problémů nebo Další informace o diagnostiky obecně.</span><span class="sxs-lookup"><span data-stu-id="43be5-169">[See a list of related Azure virtual-machine diagnostic articles](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
