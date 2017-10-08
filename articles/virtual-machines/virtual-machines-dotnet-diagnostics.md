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
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Povolení diagnostiky ve virtuálních počítačích Azure
V tématu [přehled Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) pozadí na Azure Diagnostics.

## <a name="how-tooenable-diagnostics-in-a-virtual-machine"></a>Jak tooEnable diagnostiky ve virtuálním počítači
Této ukázce popisuje, jak tooremotely nainstalovat tooan diagnostiky virtuálního počítače Azure z vývojovém počítači. Také zjistíte, jak hello tooimplement aplikace, která běží na virtuálním počítači Azure a vysílá telemetrická data pomocí .NET [EventSource – třída][EventSource Class]. Azure Diagnostics je použité toocollect hello telemetrie a uloží jej v účtu úložiště Azure.

### <a name="pre-requisites"></a>Požadavky
Této ukázce se předpokládá můžete mít předplatné Azure a používáte Visual Studio 2017 s hello Azure SDK. Pokud nemáte předplatné Azure, můžete zaregistrovat pro hello [bezplatné zkušební verze][Free Trial]. Zajistěte, aby příliš[nainstalovat a nakonfigurovat Azure PowerShell verze 0.8.7 nebo novější][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-virtual-machine"></a>Krok 1: Vytvoření virtuálního počítače
1. Ve svém vývojovém počítači spusťte Visual Studio 2017.
2. V sadě Visual Studio hello **Průzkumníka serveru** rozbalte **Azure**, klikněte pravým tlačítkem na **virtuální počítače** vyberte **vytvořit virtuální počítač**.
3. Vyberte předplatné Azure v hello **zvolte předplatné** dialogové okno a klikněte na tlačítko **Další**.
4. Vyberte **Windows Server 2012 R2 Datacenter, června 2017** v hello **vyberte bitovou kopii virtuálního počítače** dialogové okno a klikněte na tlačítko **Další**.
5. V hello **základní nastavení virtuálního počítače**, nastavte název virtuálního počítače hello příliš "wadexample". Nastavte správce uživatelské jméno a heslo a klikněte na tlačítko **Další**.
6. V hello **nastavení služby Cloud** dialogové okno Vytvořit novou cloudovou službu s názvem "wadexampleVM". Vytvořit nový účet úložiště s názvem "wadexample" a klikněte na **Další**.
7. Klikněte na možnost **Vytvořit**.

### <a name="step-2-create-your-application"></a>Krok 2: Vytvoření aplikace
1. Ve svém vývojovém počítači spusťte Visual Studio 2017.
2. Vytvořte nový Visual C# konzolovou aplikaci, která cílí na rozhraní .NET Framework 4.5. Pojmenujte projekt hello "WadExampleVM".

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. Nahraďte obsah souboru Program.cs hello hello následující kód. Hello třída **SampleEventSourceWriter** implementuje čtyři metody protokolování: **SendEnums**, **MessageMethod**, **SetOther** a  **HighFreq**. první parametr toohello Hello WriteEvent metoda definuje ID hello hello příslušné události. Hello metoda Run implementuje nekonečnou smyčku, která volá všechny hello protokolování metody implementované v hello **SampleEventSourceWriter** třídy každých 10 sekund.

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
4. Uložte soubor hello a vyberte **sestavit řešení** z hello **sestavení** nabídky toobuild vašeho kódu.

### <a name="step-3-deploy-your-application"></a>Krok 3: Nasazení aplikace
1. Klikněte pravým tlačítkem na hello **WadExampleVM** projektu v **Průzkumníku řešení** a zvolte **otevřít složku v Průzkumníku souborů**.
2. Přejděte toohello *bin\Debug* složky a zkopírujte všechny hello soubory (WadExampleVM.*)
3. V **Průzkumníka serveru** klikněte pravým tlačítkem na virtuální počítač hello a zvolte **připojit pomocí vzdálené plochy**.
4. Po připojení toohello virtuálního počítače vytvořte složku s názvem WadExampleVM a vložte soubory aplikací do složky hello.
5. Spuštění aplikace hello WadExampleVM.exe. Měli byste vidět okno prázdné konzoly.

### <a name="step-4-create-your-diagnostics-configuration-and-install-hello-extension"></a>Krok 4: Vytvořte vlastní konfiguraci diagnostiky a instalace hello rozšíření
1. Stáhněte si hello veřejné konfigurace souboru schématu definice tooyour vývojovém počítači tak, že spustíte následující příkaz prostředí PowerShell hello:

     (Get-AzureServiceAvailableExtension - Název_rozšíření 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-kódování utf8 - FilePath 'WadConfig.xsd.
2. Otevřete nový soubor XML v sadě Visual Studio, buď v projektu již otevřena nebo v sadě Visual Studio instanci s žádné otevřete projekty. V sadě Visual Studio, vyberte **přidat** -> **novou položku...** -> **Visual C# položky** -> **Data** -> **souboru XML**. Název souboru hello "WadExample.xml"
3. Hello WadConfig.xsd přidružte hello konfigurační soubor. Zajistěte, aby byl okno editor WadExample.xml hello hello aktivní okno. Stiskněte klávesu **F4** tooopen hello **vlastnosti** okno. Klikněte na hello **schémata** vlastnost hello **vlastnosti** okno. Klikněte na tlačítko hello **...** v hello **schémata** vlastnost. Klikněte na tlačítko hello **přidat...** tlačítko a přejděte toohello umístění, kam jste uložili soubor XSD hello a vyberte hello WadConfig.xsd. Klikněte na **OK**.
4. Nahraďte obsah hello hello WadExample.xml konfigurační soubor s hello následující XML a uložte soubor hello. Tento konfigurační soubor definuje několik toocollect čítače výkonu: jeden pro využití procesoru a jeden pro využití paměti. Potom hello konfigurace definuje hello čtyři události odpovídající metody toohello v hello SampleEventSourceWriter třídy.

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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Krok 5: Vzdálenou instalaci diagnostiky na virtuální počítač Azure
Hello rutiny Powershellu pro správu diagnostiky na virtuálním počítači jsou: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension a AzureVMDiagnosticsExtension odebrat.

1. V počítači vývojáře otevřete prostředí Azure PowerShell.
2. Spuštění instalace tooremotely skriptu hello diagnostiky na vašem virtuálním počítači (Nahraďte `<user>` svým uživatelským jménem adresáře. Nahraďte `<StorageAccountKey>` s hello klíč účtu úložiště pro váš účet úložiště wadexamplevm):
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
### <a name="step-6-look-at-your-telemetry-data"></a>Krok 6: Podívejte se na vaše data telemetrie
V sadě Visual Studio hello **Průzkumníka serveru** přejděte účet úložiště wadexample toohello. Po hello byl spuštěný virtuální počítač asi 5 minut byste měli vidět hello tabulky **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**,  **WADPerformanceCountersTable** a **WADSetOtherTable**. Dvakrát klikněte na jednu z hello tabulky tooview hello telemetrie, které se shromáždily.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Schéma konfiguračního souboru
soubor konfigurace diagnostiky Hello definuje hodnoty, které jsou používané tooinitialize konfigurace diagnostiky nastavení při spuštění diagnostiky agenta hello. V tématu hello [nejnovější – odkaz schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.

## <a name="troubleshooting"></a>Řešení potíží
V tématu [řešení potíží s Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) Další informace.

## <a name="next-steps"></a>Další kroky
[Viz seznam virtuálních počítačů souvisejících článcích Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) toochange hello data shromažďujete, řešení problémů nebo Další informace o diagnostiky obecně.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
