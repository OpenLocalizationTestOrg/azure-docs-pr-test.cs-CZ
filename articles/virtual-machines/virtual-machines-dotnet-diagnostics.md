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
# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Povolení diagnostiky ve virtuálních počítačích Azure
V tématu [přehled Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) pozadí na Azure Diagnostics.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Postup povolení diagnostiky ve virtuálním počítači
Této ukázce popisuje, jak provést vzdálenou instalaci diagnostiky pro virtuální počítač Azure z vývojovém počítači. Také zjistíte, jak implementovat aplikaci, která běží na virtuálním počítači Azure a vysílá telemetrická data pomocí .NET [EventSource – třída][EventSource Class]. Azure Diagnostics se používá k shromažďování telemetrie a uloží jej v účtu úložiště Azure.

### <a name="pre-requisites"></a>Požadavky
Této ukázce předpokládá můžete mít předplatné Azure a používáte Visual Studio 2017 sadou Azure SDK. Pokud nemáte předplatné Azure, můžete si zaregistrovat [bezplatné zkušební verze][Free Trial]. Zajistěte, abyste [nainstalovat a nakonfigurovat Azure PowerShell verze 0.8.7 nebo novější][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-virtual-machine"></a>Krok 1: Vytvoření virtuálního počítače
1. Ve svém vývojovém počítači spusťte Visual Studio 2017.
2. V sadě Visual Studio **Průzkumníka serveru** rozbalte **Azure**, klikněte pravým tlačítkem na **virtuální počítače** vyberte **vytvořit virtuální počítač**.
3. Vyberte předplatné Azure ve **zvolte předplatné** dialogové okno a klikněte na tlačítko **Další**.
4. Vyberte **Windows Server 2012 R2 Datacenter, června 2017** v **vyberte bitovou kopii virtuálního počítače** dialogové okno a klikněte na tlačítko **Další**.
5. V **základní nastavení virtuálního počítače**, nastavte název virtuálního počítače "wadexample". Nastavte správce uživatelské jméno a heslo a klikněte na tlačítko **Další**.
6. V **nastavení služby Cloud** dialogové okno Vytvořit novou cloudovou službu s názvem "wadexampleVM". Vytvořit nový účet úložiště s názvem "wadexample" a klikněte na **Další**.
7. Klikněte na možnost **Vytvořit**.

### <a name="step-2-create-your-application"></a>Krok 2: Vytvoření aplikace
1. Ve svém vývojovém počítači spusťte Visual Studio 2017.
2. Vytvořte nový Visual C# konzolovou aplikaci, která cílí na rozhraní .NET Framework 4.5. Název projektu "WadExampleVM".

   ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3. Nahraďte obsah souboru Program.cs následujícím kódem. Třída **SampleEventSourceWriter** implementuje čtyři metody protokolování: **SendEnums**, **MessageMethod**, **SetOther** a **HighFreq**. První parametr metody WriteEvent definuje ID pro příslušné události. Metoda Run implementuje nekonečnou smyčku, která volá všechny metody protokolování implementované v **SampleEventSourceWriter** třídy každých 10 sekund.

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
4. Uložte tento soubor a vyberte **sestavit řešení** z **sestavení** nabídky k vytvoření vašeho kódu.

### <a name="step-3-deploy-your-application"></a>Krok 3: Nasazení aplikace
1. Klikněte pravým tlačítkem na **WadExampleVM** projektu v **Průzkumníku řešení** a zvolte **otevřít složku v Průzkumníku souborů**.
2. Přejděte na *bin\Debug* složky a zkopírujte všechny soubory (WadExampleVM.*)
3. V **Průzkumníka serveru** klikněte pravým tlačítkem na virtuální počítač a vyberte **připojit pomocí vzdálené plochy**.
4. Po připojení k virtuálnímu počítači vytvořte složku s názvem WadExampleVM a vložit vaše aplikace soubory do složky.
5. Spusťte aplikaci WadExampleVM.exe. Měli byste vidět okno prázdné konzoly.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Krok 4: Vytvořte vlastní konfiguraci diagnostiky a nainstalovat rozšíření
1. Stáhněte si definici veřejné konfigurace souboru schématu na vývojovém počítači spuštěním následujícího příkazu Powershellu:

     (Get-AzureServiceAvailableExtension - Název_rozšíření 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-kódování utf8 - FilePath 'WadConfig.xsd.
2. Otevřete nový soubor XML v sadě Visual Studio, buď v projektu již otevřena nebo v sadě Visual Studio instanci s žádné otevřete projekty. V sadě Visual Studio, vyberte **přidat** -> **novou položku...** -> **Visual C# položky** -> **Data** -> **souboru XML**. Název souboru "WadExample.xml"
3. WadConfig.xsd přidružte konfiguračního souboru. Zajistěte, aby byl okno editoru WadExample.xml aktivní okno. Stiskněte klávesu **F4** otevřete **vlastnosti** okno. Klikněte na **schémata** vlastnost v **vlastnosti** okno. Klikněte **...** v **schémata** vlastnost. Klikněte na tlačítko **Přidat...** tlačítko a přejděte do umístění, kam jste uložili soubor XSD a vyberte soubor WadConfig.xsd. Klikněte na **OK**.
4. Nahraďte obsah souboru konfigurace WadExample.xml následující kód XML a uložte soubor. Tento konfigurační soubor definuje několik čítače výkonu ke shromažďování: jeden pro využití procesoru a jeden pro využití paměti. Konfigurace definuje čtyři události odpovídající metody ve třídě SampleEventSourceWriter.

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
Rutiny prostředí PowerShell pro správu diagnostiky na virtuálním počítači jsou: Set-AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension a AzureVMDiagnosticsExtension odebrat.

1. V počítači vývojáře otevřete prostředí Azure PowerShell.
2. Spustit skript, který chcete vzdáleně nainstalovat diagnostiky na vašem virtuálním počítači (Nahraďte `<user>` svým uživatelským jménem adresáře. Nahraďte `<StorageAccountKey>` s klíč účtu úložiště pro váš účet úložiště wadexamplevm):
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
V sadě Visual Studio **Průzkumníka serveru** přejděte na účet úložiště wadexample. Po virtuální počítač spuštěn asi 5 minut byste měli vidět tabulky **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** a **WADSetOtherTable**. Dvakrát klikněte na jednu z tabulky, které chcete zobrazit telemetrická data, která byla shromážděna.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Schéma konfiguračního souboru
Konfigurační soubor diagnostiky definuje hodnoty, které se používají k inicializaci nastavení diagnostiky konfigurace při spuštění diagnostiky agenta. Najdete v článku [nejnovější – odkaz schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.

## <a name="troubleshooting"></a>Řešení potíží
V tématu [řešení potíží s Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) Další informace.

## <a name="next-steps"></a>Další kroky
[Viz seznam virtuálních počítačů souvisejících článcích Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md#virtual-machines-using-azure-diagnostics) změnit shromažďujete data, řešení problémů nebo Další informace o diagnostiky obecně.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
