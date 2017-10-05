---
title: "Jak používat s cloudovými službami Azure diagnostics (.NET) | Microsoft Docs"
description: "Pomocí Azure diagnostics ke shromažďování dat od cloudových služeb Azure pro ladění, měření výkonu, monitorování, analýza provozu a další."
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
ms.openlocfilehash: 333d2f26ce043a167fb84858c8327cb39e868ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a><span data-ttu-id="ad581-103">Povolení Azure Diagnostics v cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="ad581-103">Enabling Azure Diagnostics in Azure Cloud Services</span></span>
<span data-ttu-id="ad581-104">V tématu [přehled Azure Diagnostics](../azure-diagnostics.md) pozadí na Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="ad581-104">See [Azure Diagnostics Overview](../azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-to-enable-diagnostics-in-a-worker-role"></a><span data-ttu-id="ad581-105">Postup povolení diagnostiky v roli pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="ad581-105">How to Enable Diagnostics in a Worker Role</span></span>
<span data-ttu-id="ad581-106">Tento návod popisuje, jak implementovat o roli pracovního procesu systému Azure, který vysílá telemetrická data pomocí .NET EventSource – třída.</span><span class="sxs-lookup"><span data-stu-id="ad581-106">This walkthrough describes how to implement an Azure worker role that emits telemetry data using the .NET EventSource class.</span></span> <span data-ttu-id="ad581-107">Azure Diagnostics se používá k shromažďování telemetrických dat a uložit ho v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ad581-107">Azure Diagnostics is used to collect the telemetry data and store it in an Azure storage account.</span></span> <span data-ttu-id="ad581-108">Při vytváření role pracovního procesu, Visual Studio automaticky povolí 1.0 diagnostiky jako součást řešení v Azure SDK pro .NET 2.4 a starší.</span><span class="sxs-lookup"><span data-stu-id="ad581-108">When creating a worker role, Visual Studio automatically enables Diagnostics 1.0 as part of the solution in Azure SDKs for .NET 2.4 and earlier.</span></span> <span data-ttu-id="ad581-109">Následující pokyny popisují proces pro vytvoření role pracovního procesu, zakázání diagnostiky 1.0 z řešení a nasazení diagnostiky 1.2 nebo 1.3 pro své role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="ad581-109">The following instructions describe the process for creating the worker role, disabling Diagnostics 1.0 from the solution, and deploying Diagnostics 1.2 or 1.3 to your worker role.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ad581-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ad581-110">Prerequisites</span></span>
<span data-ttu-id="ad581-111">Tento článek předpokládá můžete mít předplatné Azure a se sadou Azure SDK pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad581-111">This article assumes you have an Azure subscription and are using Visual Studio with the Azure SDK.</span></span> <span data-ttu-id="ad581-112">Pokud nemáte předplatné Azure, můžete si zaregistrovat [bezplatné zkušební verze][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="ad581-112">If you do not have an Azure subscription, you can sign up for the [Free Trial][Free Trial].</span></span> <span data-ttu-id="ad581-113">Zajistěte, abyste [nainstalovat a nakonfigurovat Azure PowerShell verze 0.8.7 nebo novější][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="ad581-113">Make sure to [Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-worker-role"></a><span data-ttu-id="ad581-114">Krok 1: Vytvoření Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="ad581-114">Step 1: Create a Worker Role</span></span>
1. <span data-ttu-id="ad581-115">Spusťte **sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="ad581-115">Launch **Visual Studio**.</span></span>
2. <span data-ttu-id="ad581-116">Vytvoření **Cloudová služba Azure** projekt **cloudu** šablony, která je cílena rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="ad581-116">Create an **Azure Cloud Service** project from the **Cloud** template that targets .NET Framework 4.5.</span></span>  <span data-ttu-id="ad581-117">Název projektu "WadExample" a klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="ad581-117">Name the project "WadExample" and click Ok.</span></span>
3. <span data-ttu-id="ad581-118">Vyberte **Role pracovního procesu** a klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="ad581-118">Select **Worker Role** and click Ok.</span></span> <span data-ttu-id="ad581-119">Vytvoří se projektu.</span><span class="sxs-lookup"><span data-stu-id="ad581-119">The project will be created.</span></span>
4. <span data-ttu-id="ad581-120">V **Průzkumníku řešení**, dvakrát klikněte **WorkerRole1** vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="ad581-120">In **Solution Explorer**, double-click the **WorkerRole1** properties file.</span></span>
5. <span data-ttu-id="ad581-121">V **konfigurace** kartě, zrušte zaškrtnutí **povolení diagnostiky** zakázat diagnostiky 1.0 (Azure SDK 2.4 a starší).</span><span class="sxs-lookup"><span data-stu-id="ad581-121">In the **Configuration** tab, un-check **Enable Diagnostics** to disable Diagnostics 1.0 (Azure SDK 2.4 and earlier).</span></span>
6. <span data-ttu-id="ad581-122">Vytvoření vlastního řešení Chcete-li ověřit, že máte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="ad581-122">Build your solution to verify that you have no errors.</span></span>

### <a name="step-2-instrument-your-code"></a><span data-ttu-id="ad581-123">Krok 2: Instrumentace kódu</span><span class="sxs-lookup"><span data-stu-id="ad581-123">Step 2: Instrument your code</span></span>
<span data-ttu-id="ad581-124">Obsah WorkerRole.cs nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="ad581-124">Replace the contents of WorkerRole.cs with the following code.</span></span> <span data-ttu-id="ad581-125">Třída SampleEventSourceWriter, dědí z [EventSource – třída][EventSource Class], implementuje čtyři metody protokolování: **SendEnums**, **MessageMethod**, **SetOther** a **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="ad581-125">The class SampleEventSourceWriter, inherited from the [EventSource Class][EventSource Class], implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="ad581-126">První parametr **WriteEvent** metoda definuje ID pro příslušné události.</span><span class="sxs-lookup"><span data-stu-id="ad581-126">The first parameter to the **WriteEvent** method defines the ID for the respective event.</span></span> <span data-ttu-id="ad581-127">Metoda Run implementuje nekonečnou smyčku, která volá všechny metody protokolování implementované v **SampleEventSourceWriter** třídy každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="ad581-127">The Run method implements an infinite loop that calls each of the logging methods implemented in the **SampleEventSourceWriter** class every 10 seconds.</span></span>

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
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
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

                // Emit several events every time we go through the loop
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
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
}
```


### <a name="step-3-deploy-your-worker-role"></a><span data-ttu-id="ad581-128">Krok 3: Nasaďte své Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="ad581-128">Step 3: Deploy your Worker Role</span></span>

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. <span data-ttu-id="ad581-129">Nasaďte své role pracovního procesu Azure z Visual Studia výběrem **WadExample** projekt v Průzkumníku řešení klikněte **publikovat** z **sestavení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="ad581-129">Deploy your worker role to Azure from within Visual Studio by selecting the **WadExample** project in the Solution Explorer then **Publish** from the **Build** menu.</span></span>
2. <span data-ttu-id="ad581-130">Vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="ad581-130">Choose your subscription.</span></span>
3. <span data-ttu-id="ad581-131">V **nastavením publikování v Microsoft Azure** dialogovém okně, vyberte **vytvořit novou...** .</span><span class="sxs-lookup"><span data-stu-id="ad581-131">In the **Microsoft Azure Publish Settings** dialog, select **Create New…**.</span></span>
4. <span data-ttu-id="ad581-132">V **vytvoření cloudové služby a účet úložiště** dialogové okno, zadejte **název** (například "WadExample") a vyberte oblast nebo skupinu vztahů.</span><span class="sxs-lookup"><span data-stu-id="ad581-132">In the **Create Cloud Service and Storage Account** dialog, enter a **Name** (for example, "WadExample") and select a region or affinity group.</span></span>
5. <span data-ttu-id="ad581-133">Nastavte **prostředí** k **pracovní**.</span><span class="sxs-lookup"><span data-stu-id="ad581-133">Set the **Environment** to **Staging**.</span></span>
6. <span data-ttu-id="ad581-134">Upravit jakékoliv **nastavení** jako vhodné a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ad581-134">Modify any other **Settings** as appropriate and click **Publish**.</span></span>
7. <span data-ttu-id="ad581-135">Po dokončení nasazení, ověřte na portálu Azure, která Cloudová služba se **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="ad581-135">After deployment has completed, verify in the Azure portal that your cloud service is in a **Running** state.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a><span data-ttu-id="ad581-136">Krok 4: Vytvoření konfiguračního souboru diagnostiky a nainstalovat rozšíření</span><span class="sxs-lookup"><span data-stu-id="ad581-136">Step 4: Create your Diagnostics configuration file and install the extension</span></span>
1. <span data-ttu-id="ad581-137">Stáhněte si definici veřejné konfigurace souboru schématu spuštěním následujícího příkazu Powershellu:</span><span class="sxs-lookup"><span data-stu-id="ad581-137">Download the public configuration file schema definition by executing the following PowerShell command:</span></span>

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. <span data-ttu-id="ad581-138">Přidání souboru XML pro vaše **WorkerRole1** kliknutím pravým tlačítkem na projekt **WorkerRole1** projektu a vyberte **přidat** -> **novou položku...**</span><span class="sxs-lookup"><span data-stu-id="ad581-138">Add an XML file to your **WorkerRole1** project by right-clicking on the **WorkerRole1** project and select **Add** -> **New Item…**</span></span><span data-ttu-id="ad581-139"> -> **Visual C# položky** -> **Data** -> **souboru XML**.</span><span class="sxs-lookup"><span data-stu-id="ad581-139"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="ad581-140">Název souboru "WadExample.xml".</span><span class="sxs-lookup"><span data-stu-id="ad581-140">Name the file "WadExample.xml".</span></span>

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. <span data-ttu-id="ad581-142">WadConfig.xsd přidružte konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="ad581-142">Associate the WadConfig.xsd with the configuration file.</span></span> <span data-ttu-id="ad581-143">Zajistěte, aby byl okno editoru WadExample.xml aktivní okno.</span><span class="sxs-lookup"><span data-stu-id="ad581-143">Make sure the WadExample.xml editor window is the active window.</span></span> <span data-ttu-id="ad581-144">Stiskněte klávesu **F4** otevřete **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="ad581-144">Press **F4** to open the **Properties** window.</span></span> <span data-ttu-id="ad581-145">Klikněte na tlačítko **schémata** vlastnost **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="ad581-145">Click the **Schemas** property in the **Properties** window.</span></span> <span data-ttu-id="ad581-146">Klikněte **...**</span><span class="sxs-lookup"><span data-stu-id="ad581-146">Click the **…**</span></span> <span data-ttu-id="ad581-147">v **schémata** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ad581-147">in the **Schemas** property.</span></span> <span data-ttu-id="ad581-148">Klikněte na tlačítko **Přidat...**</span><span class="sxs-lookup"><span data-stu-id="ad581-148">Click the **Add…**</span></span> <span data-ttu-id="ad581-149">tlačítko a přejděte do umístění, kam jste uložili soubor XSD a vyberte soubor WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="ad581-149">button and navigate to the location where you saved the XSD file and select the file WadConfig.xsd.</span></span> <span data-ttu-id="ad581-150">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ad581-150">Click **OK**.</span></span>

4. <span data-ttu-id="ad581-151">Nahraďte obsah souboru konfigurace WadExample.xml následující kód XML a uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="ad581-151">Replace the contents of the WadExample.xml configuration file with the following XML and save the file.</span></span> <span data-ttu-id="ad581-152">Tento konfigurační soubor definuje několik čítače výkonu ke shromažďování: jeden pro využití procesoru a jeden pro využití paměti.</span><span class="sxs-lookup"><span data-stu-id="ad581-152">This configuration file defines a couple performance counters to collect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="ad581-153">Konfigurace definuje čtyři události odpovídající metody ve třídě SampleEventSourceWriter.</span><span class="sxs-lookup"><span data-stu-id="ad581-153">Then the configuration defines the four events corresponding to the methods in the SampleEventSourceWriter class.</span></span>

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a><span data-ttu-id="ad581-154">Krok 5: Instalace diagnostiky na své Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="ad581-154">Step 5: Install Diagnostics on your Worker Role</span></span>
<span data-ttu-id="ad581-155">Rutiny prostředí PowerShell pro správu diagnostiky na web nebo worker role jsou: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension a AzureServiceDiagnosticsExtension odebrat.</span><span class="sxs-lookup"><span data-stu-id="ad581-155">The PowerShell cmdlets for managing Diagnostics on a web or worker role are: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension, and Remove-AzureServiceDiagnosticsExtension.</span></span>

1. <span data-ttu-id="ad581-156">Otevřete prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ad581-156">Open Azure PowerShell.</span></span>
2. <span data-ttu-id="ad581-157">Spustit skript, který chcete nainstalovat diagnostiky na své role pracovního procesu (Nahraďte *StorageAccountKey* s klíč účtu úložiště pro váš účet úložiště wadexample a *config_path* cestou k *WadExample.xml* souborů):</span><span class="sxs-lookup"><span data-stu-id="ad581-157">Execute the script to install Diagnostics on your worker role (replace *StorageAccountKey* with the storage account key for your wadexample storage account and *config_path* with the path to the *WadExample.xml* file):</span></span>

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="ad581-158">Krok 6: Podívejte se na vaše data telemetrie</span><span class="sxs-lookup"><span data-stu-id="ad581-158">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="ad581-159">V sadě Visual Studio **Průzkumníka serveru**, přejděte na účet úložiště wadexample.</span><span class="sxs-lookup"><span data-stu-id="ad581-159">In the Visual Studio **Server Explorer**, navigate to the wadexample storage account.</span></span> <span data-ttu-id="ad581-160">Po přibližně pět (5) minut spuštění cloudové služby, měli byste vidět tabulky **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** a **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="ad581-160">After the cloud service has been running about five (5) minutes, you should see the tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="ad581-161">Dvakrát klikněte na jednu z tabulky, které chcete zobrazit telemetrická data, která byla shromážděna.</span><span class="sxs-lookup"><span data-stu-id="ad581-161">Double-click one of the tables to view the telemetry that has been collected.</span></span>

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="ad581-163">Schéma konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="ad581-163">Configuration File Schema</span></span>
<span data-ttu-id="ad581-164">Konfigurační soubor diagnostiky definuje hodnoty, které se používají k inicializaci nastavení diagnostiky konfigurace při spuštění diagnostiky agenta.</span><span class="sxs-lookup"><span data-stu-id="ad581-164">The Diagnostics configuration file defines values that are used to initialize diagnostic configuration settings when the diagnostics agent starts.</span></span> <span data-ttu-id="ad581-165">Najdete v článku [nejnovější – odkaz schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.</span><span class="sxs-lookup"><span data-stu-id="ad581-165">See the [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ad581-166">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ad581-166">Troubleshooting</span></span>
<span data-ttu-id="ad581-167">Pokud máte potíže s, najdete v části [řešení potíží s Azure Diagnostics](../azure-diagnostics-troubleshooting.md) nápovědu pro běžné problémy.</span><span class="sxs-lookup"><span data-stu-id="ad581-167">If you have trouble, see [Troubleshooting Azure Diagnostics](../azure-diagnostics-troubleshooting.md) for help with common problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad581-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad581-168">Next Steps</span></span>
<span data-ttu-id="ad581-169">[Podívejte se do seznamu související Azure články diagnostiky virtuálního počítače](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) změnit shromažďujete data, řešení problémů nebo Další informace o diagnostiky obecně.</span><span class="sxs-lookup"><span data-stu-id="ad581-169">[See a list of related Azure virtual-machine diagnostic articles](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) to change the data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
