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
# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Povolení Azure Diagnostics v cloudové služby Azure
V tématu [přehled Azure Diagnostics](../azure-diagnostics.md) pozadí na Azure Diagnostics.

## <a name="how-tooenable-diagnostics-in-a-worker-role"></a>Jak tooEnable diagnostiky v roli pracovního procesu
Tento návod popisuje, jak hello tooimplement o roli pracovního procesu systému Azure, který vysílá telemetrická data pomocí .NET EventSource – třída. Azure Diagnostics je použité toocollect hello telemetrická data a uloží jej v účtu úložiště Azure. Při vytváření role pracovního procesu, Visual Studio automaticky povolí 1.0 diagnostiky jako součást hello řešení v Azure SDK pro .NET 2.4 a starší. Hello následující pokyny popisují hello proces pro vytváření hello role pracovního procesu, zakázání diagnostiky 1.0 z hello řešení a nasazení diagnostiky 1.2 nebo 1.3 tooyour role pracovního procesu.

### <a name="prerequisites"></a>Požadavky
Tento článek předpokládá můžete mít předplatné Azure a jsou pomocí sady Visual Studio s hello Azure SDK. Pokud nemáte předplatné Azure, můžete zaregistrovat pro hello [bezplatné zkušební verze][Free Trial]. Zajistěte, aby příliš[nainstalovat a nakonfigurovat Azure PowerShell verze 0.8.7 nebo novější][Install and configure Azure PowerShell version 0.8.7 or later].

### <a name="step-1-create-a-worker-role"></a>Krok 1: Vytvoření Role pracovního procesu
1. Spusťte **Visual Studio**.
2. Vytvoření **Azure Cloud Service** projekt z hello **cloudu** šablony, která je cílena rozhraní .NET Framework 4.5.  Pojmenujte projekt hello "WadExample" a klikněte na tlačítko Ok.
3. Vyberte **Role pracovního procesu** a klikněte na tlačítko Ok. Vytvoří se Hello projektu.
4. V **Průzkumníku řešení**, dvakrát klikněte na hello **WorkerRole1** vlastnosti souboru.
5. V hello **konfigurace** kartě, zrušte zaškrtnutí **povolení diagnostiky** toodisable diagnostiky 1.0 (Azure SDK 2.4 a starší).
6. Sestavení vaší tooverify řešení, které máte žádné chyby.

### <a name="step-2-instrument-your-code"></a>Krok 2: Instrumentace kódu
Nahraďte obsah hello WorkerRole.cs hello následující kód. Hello třída SampleEventSourceWriter, dědí z hello [EventSource – třída][EventSource Class], implementuje čtyři metody protokolování: **SendEnums**, **MessageMethod** , **SetOther** a **HighFreq**. první parametr toohello Hello **WriteEvent** metoda definuje ID hello hello příslušné události. Hello metoda Run implementuje nekonečnou smyčku, která volá všechny hello protokolování metody implementované v hello **SampleEventSourceWriter** třídy každých 10 sekund.

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


### <a name="step-3-deploy-your-worker-role"></a>Krok 3: Nasaďte své Role pracovního procesu

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

1. Nasazení tooAzure vaše pracovní role z Visual Studia výběrem hello **WadExample** projekt v hello Průzkumníku řešení klikněte **publikovat** z hello **sestavení** nabídky.
2. Vyberte předplatné.
3. V hello **nastavením publikování v Microsoft Azure** dialogovém okně, vyberte **vytvořit novou...** .
4. V hello **vytvoření cloudové služby a účet úložiště** dialogové okno, zadejte **název** (například "WadExample") a vyberte oblast nebo skupinu vztahů.
5. Sada hello **prostředí** příliš**pracovní**.
6. Upravit jakékoliv **nastavení** jako vhodné a klikněte na tlačítko **publikovat**.
7. Po dokončení nasazení se ověřit v portálu Azure, která Cloudová služba se hello **systémem** stavu.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-hello-extension"></a>Krok 4: Vytvoření konfiguračního souboru diagnostiky a nainstalovat rozšíření hello
1. Stáhněte si definice schématu souboru hello veřejné konfigurace tak, že spustíte následující příkaz prostředí PowerShell hello:

    ```powershell
    (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'
    ```
2. Přidat tooyour souboru XML **WorkerRole1** projektu kliknutím pravým tlačítkem na hello **WorkerRole1** projektu a vyberte **přidat** -> **novou položku...** -> **Visual C# položky** -> **Data** -> **souboru XML**. Název souboru hello "WadExample.xml".

   ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)
3. Hello WadConfig.xsd přidružte hello konfigurační soubor. Zajistěte, aby byl okno editor WadExample.xml hello hello aktivní okno. Stiskněte klávesu **F4** tooopen hello **vlastnosti** okno. Klikněte na tlačítko hello **schémata** vlastnost hello **vlastnosti** okno. Klikněte na tlačítko hello **...** v hello **schémata** vlastnost. Klikněte na tlačítko hello **přidat...** tlačítko a přejděte toohello umístění, kam jste uložili soubor XSD hello a vyberte hello WadConfig.xsd. Klikněte na **OK**.

4. Nahraďte obsah hello hello WadExample.xml konfigurační soubor s hello následující XML a uložte soubor hello. Tento konfigurační soubor definuje několik toocollect čítače výkonu: jeden pro využití procesoru a jeden pro využití paměti. Potom hello konfigurace definuje hello čtyři události odpovídající metody toohello v hello SampleEventSourceWriter třídy.

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Krok 5: Instalace diagnostiky na své Role pracovního procesu
Hello rutiny Powershellu pro správu diagnostiky na web nebo worker role jsou: Set-AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension a AzureServiceDiagnosticsExtension odebrat.

1. Otevřete prostředí Azure PowerShell.
2. Spuštění skriptu tooinstall hello diagnostiky ve své role pracovního procesu (Nahraďte *StorageAccountKey* s hello klíč účtu úložiště pro váš účet úložiště wadexample a *config_path* s cestou hello toohello *WadExample.xml* souborů):

```powershell
$storage_name = "wadexample"
$key = "<StorageAccountKey>"
$config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
$service_name="wadexample"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Krok 6: Podívejte se na vaše data telemetrie
V sadě Visual Studio hello **Průzkumníka serveru**, přejděte na účet úložiště wadexample toohello. Po přibližně pět (5) minut spuštěn hello cloudové služby, měli byste vidět hello tabulky **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** a **WADSetOtherTable**. Dvakrát klikněte na hello tabulky tooview hello telemetrie, které se shromáždily.

![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)

## <a name="configuration-file-schema"></a>Schéma konfiguračního souboru
soubor konfigurace diagnostiky Hello definuje hodnoty, které jsou používané tooinitialize konfigurace diagnostiky nastavení při spuštění diagnostiky agenta hello. V tématu hello [nejnovější – odkaz schématu](https://msdn.microsoft.com/library/azure/mt634524.aspx) platné hodnoty a příklady.

## <a name="troubleshooting"></a>Řešení potíží
Pokud máte potíže s, najdete v části [řešení potíží s Azure Diagnostics](../azure-diagnostics-troubleshooting.md) nápovědu pro běžné problémy.

## <a name="next-steps"></a>Další kroky
[Podívejte se do seznamu související Azure články diagnostiky virtuálního počítače](../monitoring-and-diagnostics/azure-diagnostics.md#cloud-services-using-azure-diagnostics) toochange hello data shromažďujete, řešení problémů nebo Další informace o diagnostiky obecně.

[EventSource Class]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Free Trial]: http://azure.microsoft.com/pricing/free-trial/
[Install and configure Azure PowerShell version 0.8.7 or later]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
