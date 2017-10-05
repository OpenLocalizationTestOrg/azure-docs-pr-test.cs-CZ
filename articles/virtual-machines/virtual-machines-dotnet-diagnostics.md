---
title: "Jak používat Azure diagnostics ve virtuálních počítačích | Microsoft Docs"
description: "Pomocí Azure diagnostics ke shromažďování dat z virtuálních počítačů Azure pro ladění, měření výkonu, monitorování, analýza provozu a další."
services: virtual-machines
documentationcenter: .net
author: davidmu1
manager: 
editor: 
ms.assetid: dfaabc7a-23e7-4af0-8369-f504d2915b3d
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/16/2016
ms.author: davidmu
ms.openlocfilehash: 8ff6b9825212359617b748aba1c78ed789b130dd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a><span data-ttu-id="d4379-103">Povolení diagnostiky ve virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="d4379-103">Enabling Diagnostics in Azure Virtual Machines</span></span>
<span data-ttu-id="d4379-104">V tématu [přehled Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) pozadí na Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="d4379-104">See [Azure Diagnostics Overview](../monitoring-and-diagnostics/azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a><span data-ttu-id="d4379-105">Postup povolení diagnostiky ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="d4379-105">How to Enable Diagnostics in a Virtual Machine</span></span>
<span data-ttu-id="d4379-106">Této ukázce popisuje, jak provést vzdálenou instalaci diagnostiky pro virtuální počítač Azure z vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="d4379-106">This walk through describes how to remotely install Diagnostics to an Azure virtual machine from a development computer.</span></span> <span data-ttu-id="d4379-107">Také zjistíte, jak implementovat aplikaci, která běží na virtuálním počítači Azure a vysílá telemetrická data pomocí .NET [EventSource – třída][EventSource Class].</span><span class="sxs-lookup"><span data-stu-id="d4379-107">You also learn how to implement an application that runs on that Azure virtual machine and emits telemetry data using the .NET [EventSource Class][EventSource Class].</span></span> <span data-ttu-id="d4379-108">Azure Diagnostics se používá k shromažďování telemetrie a uloží jej v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d4379-108">Azure Diagnostics is used to collect the telemetry and store it in an Azure storage account.</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="d4379-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d4379-109">Pre-requisites</span></span>
<span data-ttu-id="d4379-110">Této ukázce předpokládá můžete mít předplatné Azure a používáte Visual Studio 2017 sadou Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="d4379-110">This walk through assumes you have an Azure subscription and are using Visual Studio 2017 with the Azure SDK.</span></span> <span data-ttu-id="d4379-111">Pokud nemáte předplatné Azure, můžete si zaregistrovat [bezplatné zkušební verze][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="d4379-111">If you do not have an Azure subscription, you can sign up for the [Free Trial][Free Trial].</span></span> <span data-ttu-id="d4379-112">Zajistěte, abyste [nainstalovat a nakonfigurovat Azure PowerShell verze 0.8.7 nebo novější][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="d4379-112">Make sure to [Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-virtual-machine"></a><span data-ttu-id="d4379-113">Krok 1: Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d4379-113">Step 1: Create a Virtual Machine</span></span>
1. <span data-ttu-id="d4379-114">Ve svém vývojovém počítači spusťte Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d4379-114">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="d4379-115">V sadě Visual Studio **Průzkumníka serveru** rozbalte **Azure**, klikněte pravým tlačítkem na **virtuální počítače** vyberte **vytvořit virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="d4379-115">In the Visual Studio **Server Explorer** expand **Azure**, right-click **Virtual Machines** then select **Create Virtual Machine**.</span></span>
3. <span data-ttu-id="d4379-116">Vyberte předplatné Azure ve **zvolte předplatné** dialogové okno a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d4379-116">Select your Azure subscription in the **Choose a Subscription** dialog and click **Next**.</span></span>
4. <span data-ttu-id="d4379-117">Vyberte **Windows Server 2012 R2 Datacenter, června 2017** v **vyberte bitovou kopii virtuálního počítače** dialogové okno a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d4379-117">Select **Windows Server 2012 R2 Datacenter, June 2017** in the **Select a Virtual Machine Image** dialog and click **Next**.</span></span>
5. <span data-ttu-id="d4379-118">V **základní nastavení virtuálního počítače**, nastavte název virtuálního počítače "wadexample".</span><span class="sxs-lookup"><span data-stu-id="d4379-118">In the **Virtual Machine Basic Settings**, set the virtual machine name to "wadexample".</span></span> <span data-ttu-id="d4379-119">Nastavte správce uživatelské jméno a heslo a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d4379-119">Set your Administrator user name and password and click **Next**.</span></span>
6. <span data-ttu-id="d4379-120">V **nastavení služby Cloud** dialogové okno Vytvořit novou cloudovou službu s názvem "wadexampleVM".</span><span class="sxs-lookup"><span data-stu-id="d4379-120">In the **Cloud Service Settings** dialog create a new cloud service named "wadexampleVM".</span></span> <span data-ttu-id="d4379-121">Vytvořit nový účet úložiště s názvem "wadexample" a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d4379-121">Create a new Storage account named "wadexample" and click **Next**.</span></span>
7. <span data-ttu-id="d4379-122">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d4379-122">Click **Create**.</span></span>

### <a name="step-2-create-your-application"></a><span data-ttu-id="d4379-123">Krok 2: Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="d4379-123">Step 2: Create your Application</span></span>
1. <span data-ttu-id="d4379-124">Ve svém vývojovém počítači spusťte Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d4379-124">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="d4379-125">Vytvořte nový Visual C# konzolovou aplikaci, která cílí na rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="d4379-125">Create a new Visual C# Console Application that targets .NET Framework 4.5.</span></span> <span data-ttu-id="d4379-126">Název projektu "WadExampleVM".</span><span class="sxs-lookup"><span data-stu-id="d4379-126">Name the project "WadExampleVM".</span></span>

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. <span data-ttu-id="d4379-128">Nahraďte obsah souboru Program.cs následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="d4379-128">Replace the contents of Program.cs with the following code.</span></span> <span data-ttu-id="d4379-129">Třída **SampleEventSourceWriter** implementuje čtyři metody protokolování: **SendEnums**, **MessageMethod**, **SetOther** a **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="d4379-129">The class **SampleEventSourceWriter** implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="d4379-130">První parametr metody WriteEvent definuje ID pro příslušné události.</span><span class="sxs-lookup"><span data-stu-id="d4379-130">The first parameter to the WriteEvent method defines the ID for the respective event.</span></span> <span data-ttu-id="d4379-131">Metoda Run implementuje nekonečnou smyčku, která volá všechny metody protokolování implementované v **SampleEventSourceWriter** třídy každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="d4379-131">The Run method implements an infinite loop that calls each of the logging methods implemented in the **SampleEventSourceWriter** class every 10 seconds.</span></span>

    ```csharp
     using System;
     using System.Diagnostics;
     using System.Diagnostics.Tracing;
     using System.Threading;

     namespace WadExampleVM
     {
       sealed class SampleEventSourceWriter : EventSource {
         public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
         public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); } // Cast enums to int for efficient logging.
         public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
         public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
         public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }
       }

       enum MyColor {
         Red,
         Blue,
         Green
       }

       [Flags]
       enum MyFlags {
         Flag1 = 1,
         Flag2 = 2,
         Flag3 = 4
       }

       class Program
       {
         static void Main(string[] args) {
         Trace.TraceInformation("My application entry point called");

         int value = 0;

         while (true) {
             Thread.Sleep(10000);
             Trace.TraceInformation("Working");

             // Emit several events every time we go through the loop
             for (int i = 0; i < 6; i++) {
                 SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
             }

             for (int i = 0; i < 3; i++) {
                 SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                 SampleEventSourceWriter.Log.SetOther(true, 123456789);
             }

             if (value == int.MaxValue) value = 0;
             SampleEventSourceWriter.Log.HighFreq(value++);
         }

        }
      }
     }
     ```
4. <span data-ttu-id="d4379-132">Uložte tento soubor a vyberte **sestavit řešení** z **sestavení** nabídky k vytvoření vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="d4379-132">Save the file and select **Build Solution** from the **Build** menu to build your code.</span></span>

### <a name="step-3-deploy-your-application"></a><span data-ttu-id="d4379-133">Krok 3: Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="d4379-133">Step 3: Deploy your Application</span></span>
1. <span data-ttu-id="d4379-134">Klikněte pravým tlačítkem na **WadExampleVM** projektu v **Průzkumníku řešení** a zvolte **otevřít složku v Průzkumníku souborů**.</span><span class="sxs-lookup"><span data-stu-id="d4379-134">Right-click on the **WadExampleVM** project in **Solution Explorer** and choose **Open Folder in File Explorer**.</span></span>
2. <span data-ttu-id="d4379-135">Přejděte na *bin\Debug* složky a zkopírujte všechny soubory (WadExampleVM.*)</span><span class="sxs-lookup"><span data-stu-id="d4379-135">Navigate to the *bin\Debug* folder and copy all the files (WadExampleVM.*)</span></span>
3. <span data-ttu-id="d4379-136">V **Průzkumníka serveru** klikněte pravým tlačítkem na virtuální počítač a vyberte **připojit pomocí vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="d4379-136">In **Server Explorer** right-click on the virtual machine and choose **Connect using Remote Desktop**.</span></span>
4. <span data-ttu-id="d4379-137">Po připojení k virtuálnímu počítači vytvořte složku s názvem WadExampleVM a vložit vaše aplikace soubory do složky.</span><span class="sxs-lookup"><span data-stu-id="d4379-137">Once connected to the VM create a folder named WadExampleVM and paste your application files into the folder.</span></span>
5. <span data-ttu-id="d4379-138">Spusťte aplikaci WadExampleVM.exe.</span><span class="sxs-lookup"><span data-stu-id="d4379-138">Launch the application WadExampleVM.exe.</span></span> <span data-ttu-id="d4379-139">Měli byste vidět okno prázdné konzoly.</span><span class="sxs-lookup"><span data-stu-id="d4379-139">You should see a blank console window.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a><span data-ttu-id="d4379-140">Krok 4: Vytvořte vlastní konfiguraci diagnostiky a nainstalovat rozšíření</span><span class="sxs-lookup"><span data-stu-id="d4379-140">Step 4: Create your Diagnostics configuration and install the Extension</span></span>
1. <span data-ttu-id="d4379-141">Stáhněte si definici veřejné konfigurace souboru schématu na vývojovém počítači spuštěním následujícího příkazu Powershellu:</span><span class="sxs-lookup"><span data-stu-id="d4379-141">Download the public configuration file schema definition to your development computer by executing the following PowerShell command:</span></span>

     <span data-ttu-id="d4379-142">(Get-AzureServiceAvailableExtension - Název_rozšíření 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-kódování utf8 - FilePath 'WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="d4379-142">(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'</span></span>
2. <span data-ttu-id="d4379-143">Otevřete nový soubor XML v sadě Visual Studio, buď v projektu již otevřena nebo v sadě Visual Studio instanci s žádné otevřete projekty.</span><span class="sxs-lookup"><span data-stu-id="d4379-143">Open a new XML file in Visual Studio, either in a project you already have open or in a Visual Studio instance with no open projects.</span></span> <span data-ttu-id="d4379-144">V sadě Visual Studio, vyberte **přidat** -> **novou položku...**</span><span class="sxs-lookup"><span data-stu-id="d4379-144">In Visual Studio, select **Add** -> **New Item…**</span></span><span data-ttu-id="d4379-145"> -> **Visual C# položky** -> **Data** -> **souboru XML**.</span><span class="sxs-lookup"><span data-stu-id="d4379-145"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="d4379-146">Název souboru "WadExample.xml"</span><span class="sxs-lookup"><span data-stu-id="d4379-146">Name the file "WadExample.xml"</span></span>
3. <span data-ttu-id="d4379-147">WadConfig.xsd přidružte konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="d4379-147">Associate the WadConfig.xsd with the configuration file.</span></span> <span data-ttu-id="d4379-148">Zajistěte, aby byl okno editoru WadExample.xml aktivní okno.</span><span class="sxs-lookup"><span data-stu-id="d4379-148">Make sure the WadExample.xml editor window is the active window.</span></span> <span data-ttu-id="d4379-149">Stiskněte klávesu **F4** otevřete **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="d4379-149">Press **F4** to open the **Properties** window.</span></span> <span data-ttu-id="d4379-150">Klikněte na **schémata** vlastnost v **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="d4379-150">Click on the **Schemas** property in the **Properties** window.</span></span> <span data-ttu-id="d4379-151">Klikněte **...**</span><span class="sxs-lookup"><span data-stu-id="d4379-151">Click the **…**</span></span> <span data-ttu-id="d4379-152">v **schémata** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d4379-152">in the **Schemas** property.</span></span> <span data-ttu-id="d4379-153">Klikněte na tlačítko **Přidat...**</span><span class="sxs-lookup"><span data-stu-id="d4379-153">Click the **Add…**</span></span> <span data-ttu-id="d4379-154">tlačítko a přejděte do umístění, kam jste uložili soubor XSD a vyberte soubor WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="d4379-154">button and navigate to the location where you saved the XSD file and select the file WadConfig.xsd.</span></span> <span data-ttu-id="d4379-155">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4379-155">Click **OK**.</span></span>
4. <span data-ttu-id="d4379-156">Nahraďte obsah souboru konfigurace WadExample.xml následující kód XML a uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="d4379-156">Replace the contents of the WadExample.xml configuration file with the following XML and save the file.</span></span> <span data-ttu-id="d4379-157">Tento konfigurační soubor definuje několik čítače výkonu ke shromažďování: jeden pro využití procesoru a jeden pro využití paměti.</span><span class="sxs-lookup"><span data-stu-id="d4379-157">This configuration file defines a couple performance counters to collect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="d4379-158">Konfigurace definuje čtyři události odpovídající metody ve třídě SampleEventSourceWriter.</span><span class="sxs-lookup"><span data-stu-id="d4379-158">Then the configuration defines the four events corresponding to the methods in the SampleEventSourceWriter class.</span></span>

```
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a><span data-ttu-id="d4379-159">Krok 5: Vzdálenou instalaci diagnostiky na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="d4379-159">Step 5: Remotely install Diagnostics on your Azure Virtual Machine</span></span>
<span data-ttu-id="d4379-160">Rutiny prostředí PowerShell pro správu diagnostiky na virtuálním počítači jsou: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension a AzureVMDiagnosticsExtension odebrat.</span><span class="sxs-lookup"><span data-stu-id="d4379-160">The PowerShell cmdlets for managing Diagnostics on a VM are: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension, and Remove-AzureVMDiagnosticsExtension.</span></span>

1. <span data-ttu-id="d4379-161">V počítači vývojáře otevřete prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d4379-161">On your developer computer, open Azure PowerShell.</span></span>
2. <span data-ttu-id="d4379-162">Spustit skript, který chcete vzdáleně nainstalovat diagnostiky na vašem virtuálním počítači (Nahraďte `<user>` svým uživatelským jménem adresáře.</span><span class="sxs-lookup"><span data-stu-id="d4379-162">Execute the script to remotely install Diagnostics on your VM (Replace `<user>` with your user directory name.</span></span> <span data-ttu-id="d4379-163">Nahraďte `<StorageAccountKey>` s klíč účtu úložiště pro váš účet úložiště wadexamplevm):</span><span class="sxs-lookup"><span data-stu-id="d4379-163">Replace `<StorageAccountKey>` with the storage account key for your wadexamplevm storage account):</span></span>
```
     $storage_name = "wadexamplevm"
     $key = "<StorageAccountKey>"
     $config_path="c:\users\<user>\documents\visual studio 2017\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
     $service_name="wadexamplevm"
     $vm_name="WadExample"
     $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
     $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
     $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
     $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM
```
### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="d4379-164">Krok 6: Podívejte se na vaše data telemetrie</span><span class="sxs-lookup"><span data-stu-id="d4379-164">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="d4379-165">V sadě Visual Studio **Průzkumníka serveru** přejděte na účet úložiště wadexample.</span><span class="sxs-lookup"><span data-stu-id="d4379-165">In the Visual Studio **Server Explorer** navigate to the wadexample storage account.</span></span> <span data-ttu-id="d4379-166">Po virtuální počítač spuštěn asi 5 minut byste měli vidět tabulky **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** a **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="d4379-166">After the VM has been running about 5 minutes you should see the tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="d4379-167">Dvakrát klikněte na jednu z tabulky, které chcete zobrazit telemetrická data, která byla shromážděna.</span><span class="sxs-lookup"><span data-stu-id="d4379-167">Double-click on one of the tables to view the telemetry that has been collected.</span></span>

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="d4379-169">Schéma konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="d4379-169">Configuration file schema</span></span>
<span data-ttu-id="d4379-170">Konfigurační soubor diagnostiky definuje hodnoty, které se používají k inicializaci nastavení diagnostiky konfigurace při spuštění diagnostiky agenta.</span><span class="sxs-lookup"><span data-stu-id="d4379-170">The Diagnostics configuration file defines values that are used to initialize diagnostic configuration settings when the diagnostics agent starts.</span></span> <span data-ttu-id="d4379-171">Najdete v článku [nejnovější – odkaz schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.</span><span class="sxs-lookup"><span data-stu-id="d4379-171">See the [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d4379-172">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d4379-172">Troubleshooting</span></span>
<span data-ttu-id="d4379-173">V tématu [řešení potíží s Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="d4379-173">See [Troubleshooting Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4379-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d4379-174">Next steps</span></span>
<span data-ttu-id="d4379-175">[Viz seznam virtuálních počítačů souvisejících článcích Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) změnit shromažďujete data, řešení problémů nebo Další informace o diagnostiky obecně.</span><span class="sxs-lookup"><span data-stu-id="d4379-175">[See a list of virtual machine related Azure Diagnostics articles](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) to change the data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
