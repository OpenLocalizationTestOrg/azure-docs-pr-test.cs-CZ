---
title: "aaaHow toouse ve virtuálních počítačích Azure diagnostics | Microsoft Docs"
description: "Použití Azure diagnostics toogather data z virtuálních počítačů Azure pro ladění, měření výkonu, monitorování, analýza provozu a další."
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
ms.openlocfilehash: 54cdfd30d7bbbb71af449826e90234faf5ecdf44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a><span data-ttu-id="4231e-103">Povolení diagnostiky ve virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="4231e-103">Enabling Diagnostics in Azure Virtual Machines</span></span>
<span data-ttu-id="4231e-104">V tématu [přehled Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) pozadí na Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="4231e-104">See [Azure Diagnostics Overview](../monitoring-and-diagnostics/azure-diagnostics.md) for a background on Azure Diagnostics.</span></span>

## <a name="how-tooenable-diagnostics-in-a-virtual-machine"></a><span data-ttu-id="4231e-105">Jak tooEnable diagnostiky ve virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="4231e-105">How tooEnable Diagnostics in a Virtual Machine</span></span>
<span data-ttu-id="4231e-106">Této ukázce popisuje, jak tooremotely nainstalovat tooan diagnostiky virtuálního počítače Azure z vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="4231e-106">This walk through describes how tooremotely install Diagnostics tooan Azure virtual machine from a development computer.</span></span> <span data-ttu-id="4231e-107">Také zjistíte, jak hello tooimplement aplikace, která běží na virtuálním počítači Azure a vysílá telemetrická data pomocí .NET [EventSource – třída][EventSource Class].</span><span class="sxs-lookup"><span data-stu-id="4231e-107">You also learn how tooimplement an application that runs on that Azure virtual machine and emits telemetry data using hello .NET [EventSource Class][EventSource Class].</span></span> <span data-ttu-id="4231e-108">Azure Diagnostics je použité toocollect hello telemetrie a uloží jej v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4231e-108">Azure Diagnostics is used toocollect hello telemetry and store it in an Azure storage account.</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="4231e-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4231e-109">Pre-requisites</span></span>
<span data-ttu-id="4231e-110">Této ukázce se předpokládá můžete mít předplatné Azure a používáte Visual Studio 2017 s hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="4231e-110">This walk through assumes you have an Azure subscription and are using Visual Studio 2017 with hello Azure SDK.</span></span> <span data-ttu-id="4231e-111">Pokud nemáte předplatné Azure, můžete zaregistrovat pro hello [bezplatné zkušební verze][Free Trial].</span><span class="sxs-lookup"><span data-stu-id="4231e-111">If you do not have an Azure subscription, you can sign up for hello [Free Trial][Free Trial].</span></span> <span data-ttu-id="4231e-112">Zajistěte, aby příliš[nainstalovat a nakonfigurovat Azure PowerShell verze 0.8.7 nebo novější][Install and configure Azure PowerShell version 0.8.7 or later].</span><span class="sxs-lookup"><span data-stu-id="4231e-112">Make sure too[Install and configure Azure PowerShell version 0.8.7 or later][Install and configure Azure PowerShell version 0.8.7 or later].</span></span>

### <a name="step-1-create-a-virtual-machine"></a><span data-ttu-id="4231e-113">Krok 1: Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4231e-113">Step 1: Create a Virtual Machine</span></span>
1. <span data-ttu-id="4231e-114">Ve svém vývojovém počítači spusťte Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4231e-114">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="4231e-115">V sadě Visual Studio hello **Průzkumníka serveru** rozbalte **Azure**, klikněte pravým tlačítkem na **virtuální počítače** vyberte **vytvořit virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="4231e-115">In hello Visual Studio **Server Explorer** expand **Azure**, right-click **Virtual Machines** then select **Create Virtual Machine**.</span></span>
3. <span data-ttu-id="4231e-116">Vyberte předplatné Azure v hello **zvolte předplatné** dialogové okno a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4231e-116">Select your Azure subscription in hello **Choose a Subscription** dialog and click **Next**.</span></span>
4. <span data-ttu-id="4231e-117">Vyberte **Windows Server 2012 R2 Datacenter, června 2017** v hello **vyberte bitovou kopii virtuálního počítače** dialogové okno a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4231e-117">Select **Windows Server 2012 R2 Datacenter, June 2017** in hello **Select a Virtual Machine Image** dialog and click **Next**.</span></span>
5. <span data-ttu-id="4231e-118">V hello **základní nastavení virtuálního počítače**, nastavte název virtuálního počítače hello příliš "wadexample".</span><span class="sxs-lookup"><span data-stu-id="4231e-118">In hello **Virtual Machine Basic Settings**, set hello virtual machine name too"wadexample".</span></span> <span data-ttu-id="4231e-119">Nastavte správce uživatelské jméno a heslo a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4231e-119">Set your Administrator user name and password and click **Next**.</span></span>
6. <span data-ttu-id="4231e-120">V hello **nastavení služby Cloud** dialogové okno Vytvořit novou cloudovou službu s názvem "wadexampleVM".</span><span class="sxs-lookup"><span data-stu-id="4231e-120">In hello **Cloud Service Settings** dialog create a new cloud service named "wadexampleVM".</span></span> <span data-ttu-id="4231e-121">Vytvořit nový účet úložiště s názvem "wadexample" a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="4231e-121">Create a new Storage account named "wadexample" and click **Next**.</span></span>
7. <span data-ttu-id="4231e-122">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4231e-122">Click **Create**.</span></span>

### <a name="step-2-create-your-application"></a><span data-ttu-id="4231e-123">Krok 2: Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="4231e-123">Step 2: Create your Application</span></span>
1. <span data-ttu-id="4231e-124">Ve svém vývojovém počítači spusťte Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4231e-124">On your development computer, launch Visual Studio 2017.</span></span>
2. <span data-ttu-id="4231e-125">Vytvořte nový Visual C# konzolovou aplikaci, která cílí na rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="4231e-125">Create a new Visual C# Console Application that targets .NET Framework 4.5.</span></span> <span data-ttu-id="4231e-126">Pojmenujte projekt hello "WadExampleVM".</span><span class="sxs-lookup"><span data-stu-id="4231e-126">Name hello project "WadExampleVM".</span></span>

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. <span data-ttu-id="4231e-128">Nahraďte obsah souboru Program.cs hello hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="4231e-128">Replace hello contents of Program.cs with hello following code.</span></span> <span data-ttu-id="4231e-129">Hello třída **SampleEventSourceWriter** implementuje čtyři metody protokolování: **SendEnums**, **MessageMethod**, **SetOther** a  **HighFreq**.</span><span class="sxs-lookup"><span data-stu-id="4231e-129">hello class **SampleEventSourceWriter** implements four logging methods: **SendEnums**, **MessageMethod**, **SetOther** and **HighFreq**.</span></span> <span data-ttu-id="4231e-130">první parametr toohello Hello WriteEvent metoda definuje ID hello hello příslušné události.</span><span class="sxs-lookup"><span data-stu-id="4231e-130">hello first parameter toohello WriteEvent method defines hello ID for hello respective event.</span></span> <span data-ttu-id="4231e-131">Hello metoda Run implementuje nekonečnou smyčku, která volá všechny hello protokolování metody implementované v hello **SampleEventSourceWriter** třídy každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="4231e-131">hello Run method implements an infinite loop that calls each of hello logging methods implemented in hello **SampleEventSourceWriter** class every 10 seconds.</span></span>

    ```csharp
     using System;
     using System.Diagnostics;
     using System.Diagnostics.Tracing;
     using System.Threading;

     namespace WadExampleVM
     {
       sealed class SampleEventSourceWriter : EventSource {
         public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
         public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); } // Cast enums tooint for efficient logging.
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

             // Emit several events every time we go through hello loop
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
4. <span data-ttu-id="4231e-132">Uložte soubor hello a vyberte **sestavit řešení** z hello **sestavení** nabídky toobuild vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="4231e-132">Save hello file and select **Build Solution** from hello **Build** menu toobuild your code.</span></span>

### <a name="step-3-deploy-your-application"></a><span data-ttu-id="4231e-133">Krok 3: Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="4231e-133">Step 3: Deploy your Application</span></span>
1. <span data-ttu-id="4231e-134">Klikněte pravým tlačítkem na hello **WadExampleVM** projektu v **Průzkumníku řešení** a zvolte **otevřít složku v Průzkumníku souborů**.</span><span class="sxs-lookup"><span data-stu-id="4231e-134">Right-click on hello **WadExampleVM** project in **Solution Explorer** and choose **Open Folder in File Explorer**.</span></span>
2. <span data-ttu-id="4231e-135">Přejděte toohello *bin\Debug* složky a zkopírujte všechny hello soubory (WadExampleVM.*)</span><span class="sxs-lookup"><span data-stu-id="4231e-135">Navigate toohello *bin\Debug* folder and copy all hello files (WadExampleVM.*)</span></span>
3. <span data-ttu-id="4231e-136">V **Průzkumníka serveru** klikněte pravým tlačítkem na virtuální počítač hello a zvolte **připojit pomocí vzdálené plochy**.</span><span class="sxs-lookup"><span data-stu-id="4231e-136">In **Server Explorer** right-click on hello virtual machine and choose **Connect using Remote Desktop**.</span></span>
4. <span data-ttu-id="4231e-137">Po připojení toohello virtuálního počítače vytvořte složku s názvem WadExampleVM a vložte soubory aplikací do složky hello.</span><span class="sxs-lookup"><span data-stu-id="4231e-137">Once connected toohello VM create a folder named WadExampleVM and paste your application files into hello folder.</span></span>
5. <span data-ttu-id="4231e-138">Spuštění aplikace hello WadExampleVM.exe.</span><span class="sxs-lookup"><span data-stu-id="4231e-138">Launch hello application WadExampleVM.exe.</span></span> <span data-ttu-id="4231e-139">Měli byste vidět okno prázdné konzoly.</span><span class="sxs-lookup"><span data-stu-id="4231e-139">You should see a blank console window.</span></span>

### <a name="step-4-create-your-diagnostics-configuration-and-install-hello-extension"></a><span data-ttu-id="4231e-140">Krok 4: Vytvořte vlastní konfiguraci diagnostiky a instalace hello rozšíření</span><span class="sxs-lookup"><span data-stu-id="4231e-140">Step 4: Create your Diagnostics configuration and install hello Extension</span></span>
1. <span data-ttu-id="4231e-141">Stáhněte si hello veřejné konfigurace souboru schématu definice tooyour vývojovém počítači tak, že spustíte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="4231e-141">Download hello public configuration file schema definition tooyour development computer by executing hello following PowerShell command:</span></span>

     <span data-ttu-id="4231e-142">(Get-AzureServiceAvailableExtension - Název_rozšíření 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-kódování utf8 - FilePath 'WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="4231e-142">(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'</span></span>
2. <span data-ttu-id="4231e-143">Otevřete nový soubor XML v sadě Visual Studio, buď v projektu již otevřena nebo v sadě Visual Studio instanci s žádné otevřete projekty.</span><span class="sxs-lookup"><span data-stu-id="4231e-143">Open a new XML file in Visual Studio, either in a project you already have open or in a Visual Studio instance with no open projects.</span></span> <span data-ttu-id="4231e-144">V sadě Visual Studio, vyberte **přidat** -> **novou položku...**</span><span class="sxs-lookup"><span data-stu-id="4231e-144">In Visual Studio, select **Add** -> **New Item…**</span></span><span data-ttu-id="4231e-145"> -> **Visual C# položky** -> **Data** -> **souboru XML**.</span><span class="sxs-lookup"><span data-stu-id="4231e-145"> -> **Visual C# items** -> **Data** -> **XML File**.</span></span> <span data-ttu-id="4231e-146">Název souboru hello "WadExample.xml"</span><span class="sxs-lookup"><span data-stu-id="4231e-146">Name hello file "WadExample.xml"</span></span>
3. <span data-ttu-id="4231e-147">Hello WadConfig.xsd přidružte hello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="4231e-147">Associate hello WadConfig.xsd with hello configuration file.</span></span> <span data-ttu-id="4231e-148">Zajistěte, aby byl okno editor WadExample.xml hello hello aktivní okno.</span><span class="sxs-lookup"><span data-stu-id="4231e-148">Make sure hello WadExample.xml editor window is hello active window.</span></span> <span data-ttu-id="4231e-149">Stiskněte klávesu **F4** tooopen hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="4231e-149">Press **F4** tooopen hello **Properties** window.</span></span> <span data-ttu-id="4231e-150">Klikněte na hello **schémata** vlastnost hello **vlastnosti** okno.</span><span class="sxs-lookup"><span data-stu-id="4231e-150">Click on hello **Schemas** property in hello **Properties** window.</span></span> <span data-ttu-id="4231e-151">Klikněte na tlačítko hello **...**</span><span class="sxs-lookup"><span data-stu-id="4231e-151">Click hello **…**</span></span> <span data-ttu-id="4231e-152">v hello **schémata** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4231e-152">in hello **Schemas** property.</span></span> <span data-ttu-id="4231e-153">Klikněte na tlačítko hello **přidat...**</span><span class="sxs-lookup"><span data-stu-id="4231e-153">Click hello **Add…**</span></span> <span data-ttu-id="4231e-154">tlačítko a přejděte toohello umístění, kam jste uložili soubor XSD hello a vyberte hello WadConfig.xsd.</span><span class="sxs-lookup"><span data-stu-id="4231e-154">button and navigate toohello location where you saved hello XSD file and select hello file WadConfig.xsd.</span></span> <span data-ttu-id="4231e-155">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4231e-155">Click **OK**.</span></span>
4. <span data-ttu-id="4231e-156">Nahraďte obsah hello hello WadExample.xml konfigurační soubor s hello následující XML a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="4231e-156">Replace hello contents of hello WadExample.xml configuration file with hello following XML and save hello file.</span></span> <span data-ttu-id="4231e-157">Tento konfigurační soubor definuje několik toocollect čítače výkonu: jeden pro využití procesoru a jeden pro využití paměti.</span><span class="sxs-lookup"><span data-stu-id="4231e-157">This configuration file defines a couple performance counters toocollect: one for CPU utilization and one for memory utilization.</span></span> <span data-ttu-id="4231e-158">Potom hello konfigurace definuje hello čtyři události odpovídající metody toohello v hello SampleEventSourceWriter třídy.</span><span class="sxs-lookup"><span data-stu-id="4231e-158">Then hello configuration defines hello four events corresponding toohello methods in hello SampleEventSourceWriter class.</span></span>

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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a><span data-ttu-id="4231e-159">Krok 5: Vzdálenou instalaci diagnostiky na virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="4231e-159">Step 5: Remotely install Diagnostics on your Azure Virtual Machine</span></span>
<span data-ttu-id="4231e-160">Hello rutiny Powershellu pro správu diagnostiky na virtuálním počítači jsou: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension a AzureVMDiagnosticsExtension odebrat.</span><span class="sxs-lookup"><span data-stu-id="4231e-160">hello PowerShell cmdlets for managing Diagnostics on a VM are: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension, and Remove-AzureVMDiagnosticsExtension.</span></span>

1. <span data-ttu-id="4231e-161">V počítači vývojáře otevřete prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4231e-161">On your developer computer, open Azure PowerShell.</span></span>
2. <span data-ttu-id="4231e-162">Spuštění instalace tooremotely skriptu hello diagnostiky na vašem virtuálním počítači (Nahraďte `<user>` svým uživatelským jménem adresáře.</span><span class="sxs-lookup"><span data-stu-id="4231e-162">Execute hello script tooremotely install Diagnostics on your VM (Replace `<user>` with your user directory name.</span></span> <span data-ttu-id="4231e-163">Nahraďte `<StorageAccountKey>` s hello klíč účtu úložiště pro váš účet úložiště wadexamplevm):</span><span class="sxs-lookup"><span data-stu-id="4231e-163">Replace `<StorageAccountKey>` with hello storage account key for your wadexamplevm storage account):</span></span>
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
### <a name="step-6-look-at-your-telemetry-data"></a><span data-ttu-id="4231e-164">Krok 6: Podívejte se na vaše data telemetrie</span><span class="sxs-lookup"><span data-stu-id="4231e-164">Step 6: Look at your telemetry data</span></span>
<span data-ttu-id="4231e-165">V sadě Visual Studio hello **Průzkumníka serveru** přejděte účet úložiště wadexample toohello.</span><span class="sxs-lookup"><span data-stu-id="4231e-165">In hello Visual Studio **Server Explorer** navigate toohello wadexample storage account.</span></span> <span data-ttu-id="4231e-166">Po hello byl spuštěný virtuální počítač asi 5 minut byste měli vidět hello tabulky **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**,  **WADPerformanceCountersTable** a **WADSetOtherTable**.</span><span class="sxs-lookup"><span data-stu-id="4231e-166">After hello VM has been running about 5 minutes you should see hello tables **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** and **WADSetOtherTable**.</span></span> <span data-ttu-id="4231e-167">Dvakrát klikněte na jednu z hello tabulky tooview hello telemetrie, které se shromáždily.</span><span class="sxs-lookup"><span data-stu-id="4231e-167">Double-click on one of hello tables tooview hello telemetry that has been collected.</span></span>

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a><span data-ttu-id="4231e-169">Schéma konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="4231e-169">Configuration file schema</span></span>
<span data-ttu-id="4231e-170">soubor konfigurace diagnostiky Hello definuje hodnoty, které jsou používané tooinitialize konfigurace diagnostiky nastavení při spuštění diagnostiky agenta hello.</span><span class="sxs-lookup"><span data-stu-id="4231e-170">hello Diagnostics configuration file defines values that are used tooinitialize diagnostic configuration settings when hello diagnostics agent starts.</span></span> <span data-ttu-id="4231e-171">V tématu hello [nejnovější – odkaz schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.</span><span class="sxs-lookup"><span data-stu-id="4231e-171">See hello [latest schema reference](https://msdn.microsoft.com/library/azure/mt634524.aspx) for valid values and examples.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4231e-172">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4231e-172">Troubleshooting</span></span>
<span data-ttu-id="4231e-173">V tématu [řešení potíží s Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4231e-173">See [Troubleshooting Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4231e-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4231e-174">Next steps</span></span>
<span data-ttu-id="4231e-175">[Viz seznam virtuálních počítačů souvisejících článcích Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello data shromažďujete, řešení problémů nebo Další informace o diagnostiky obecně.</span><span class="sxs-lookup"><span data-stu-id="4231e-175">[See a list of virtual machine related Azure Diagnostics articles](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello data you are collecting, troubleshoot problems or learn more about diagnostics in general.</span></span>

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
