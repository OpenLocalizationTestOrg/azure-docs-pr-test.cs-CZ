---
title: "úložiště objektů blob aaaUse pro službu IIS a tabulka úložiště pro události v Azure Log Analytics | Microsoft Docs"
description: "Analýzy protokolů může číst hello protokoly služby Azure, které zápis tootable úložiště diagnostiky nebo protokoly služby IIS zapisovat tooblob úložiště."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff3de04dc8cb6729c1443372ec31a0e8dc47f273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a>Používat úložiště objektů blob v Azure pro službu IIS a Azure úložiště table pro události se analýzy protokolů

Analýzy protokolů můžete číst hello protokoly pro následující služby, které zápisu diagnostiky tootable úložiště nebo IIS protokoly úložiště napsané tooblob hello:

* Service Fabric clusterů (Preview)
* Virtuální počítače
* Web/role pracovního procesu

Před analýzy protokolů můžete shromažďovat data pro tyto prostředky, musí být povolena Azure diagnostics.

Jakmile diagnostiky jsou povolené, můžete použít hello portál Azure nebo PowerShell konfigurace analýzy protokolů toocollect hello protokoly.

Azure Diagnostics je rozšířením Azure, která vám umožní toocollect diagnostických dat z role pracovního procesu, webovou roli nebo virtuální počítač spuštěný v Azure. Hello data je uložený v účtu úložiště Azure a můžete pak shromáždit pomocí analýzy protokolů.

Pro analýzy protokolů toocollect musí být tyto protokoly Azure Diagnostics hello protokoly v hello následující umístění:

| Typ protokolu | Typ prostředku | Umístění |
| --- | --- | --- |
| Protokoly IIS |Virtuální počítače <br> Webové role <br> Role pracovního procesu |wad logfiles služby iis (úložiště objektů Blob) |
| Syslog |Virtuální počítače |LinuxsyslogVer2v0 (tabulky úložiště) |
| Provozní události služby prostředků infrastruktury |Uzly Service Fabric |WADServiceFabricSystemEventTable |
| Události služby Fabric spolehlivé objektu Actor |Uzly Service Fabric |WADServiceFabricReliableActorEventTable |
| Události spolehlivé služby Service Fabric |Uzly Service Fabric |WADServiceFabricReliableServiceEventTable |
| Protokoly událostí systému Windows |Uzly Service Fabric <br> Virtuální počítače <br> Webové role <br> Role pracovního procesu |WADWindowsEventLogsTable (Table Storage) |
| Protokoly systému Windows trasování událostí pro Windows |Uzly Service Fabric <br> Virtuální počítače <br> Webové role <br> Role pracovního procesu |WADETWEventTable (Table Storage) |

> [!NOTE]
> Protokoly služby IIS z webů Azure nejsou aktuálně podporovány.
>
>

Pro virtuální počítače, máte možnost hello instalace hello [analýzy protokolů agenta](log-analytics-azure-vm-extension.md) do další přehledy tooenable virtuálního počítače. Kromě toho protokoly služby IIS možné tooanalyze toobeing a protokoly událostí, můžete provádět další analýzu, včetně sledování změn konfigurace, vyhodnocení SQL a vyhodnocení aktualizací.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Povolit Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekce
Hello použijte následující postup tooenable Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekce pomocí portálu Microsoft Azure hello.

### <a name="tooenable-azure-diagnostics-in-a-virtual-machine-with-hello-azure-portal"></a>tooenable Azure diagnostics ve virtuálním počítači s hello portálu Azure
1. Když vytvoříte virtuální počítač, nainstalujte hello agenta virtuálního počítače. Pokud hello virtuálního počítače již existuje, ověřte, že hello agenta virtuálního počítače je již nainstalována.

   * V hello portálu Azure, přejděte toohello virtuálního počítače, vyberte **volitelné konfiguraci**, pak **diagnostiky** a nastavte **stav** příliš**na**.

     Po dokončení zpracování se hello virtuálních počítačů má rozšíření diagnostiky Azure hello nainstalovaná a spuštěná. Toto rozšíření je zodpovědná za shromažďování dat diagnostiky.
2. Povolí monitorování a konfigurace protokolování událostí na existující virtuální počítač. Můžete povolit diagnostiku v hello úroveň virtuálního počítače. tooenable diagnostiky a potom konfiguraci protokolování událostí, provádět hello následující kroky:

   1. Vyberte hello virtuálních počítačů.
   2. Klikněte na tlačítko **monitorování**.
   3. Klikněte na tlačítko **diagnostiky**.
   4. Sada hello **stav** příliš**ON**.
   5. Vyberte každý protokol diagnostiky, které chcete toocollect.
   6. Klikněte na **OK**.

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Povolit Azure diagnostics ve webové roli pro shromažďování protokolů a událostí služby IIS
Odkazovat příliš[jak tooEnable diagnostiky v cloudové službě](../cloud-services/cloud-services-dotnet-diagnostics.md) obecné pokyny k povolení Azure diagnostics. níže uvedené pokyny Hello tyto informace používat a přizpůsobit pro použití s analýzy protokolů.

S Azure diagnostics povoleno:

* Protokoly služby IIS jsou uloženy ve výchozím nastavení, s daty protokolu přenést v intervalu přenos scheduledTransferPeriod hello.
* Ve výchozím nastavení se nepřenesou protokoly událostí systému Windows.

### <a name="tooenable-diagnostics"></a>tooenable diagnostiky
tooenable protokoly událostí systému Windows nebo toochange hello scheduledTransferPeriod, nakonfigurujte Azure Diagnostics pomocí konfigurační soubor XML hello (diagnostics.wadcfg), jak je znázorněno v [krok 4: vytvoření konfiguračního souboru diagnostiky a instalace hello rozšíření](../cloud-services/cloud-services-dotnet-diagnostics.md)

Hello následující příklad konfigurační soubor shromažďuje protokoly služby IIS a všechny události z hello aplikace a systémové protokoly:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant tooWeb roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Zajistěte, aby vaše ConfigurationSettings určovala účtu úložiště, jako hello následující ukázka:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

Hello **AccountName** a **AccountKey** hodnoty se nacházejí v hello portál Azure hello panelu účet úložiště, v části Spravovat přístupové klíče. Hello protokol pro hello připojovací řetězec musí být **https**.

Po aktualizované konfigurace diagnostiky hello tooyour cloudové služby a zapisuje diagnostiky tooAzure úložiště, pak jsou připravené tooconfigure analýzy protokolů.

## <a name="use-hello-azure-portal-toocollect-logs-from-azure-storage"></a>Použití protokolů Azure portálu toocollect hello ze služby Azure Storage
Hello Azure tooconfigure portálu analýzy protokolů toocollect hello protokoly můžete použít k hello následující služby Azure:

* Clusterů Service Fabric
* Virtuální počítače
* Web/role pracovního procesu

V hello portálu Azure přejděte tooyour pracovní prostor analýzy protokolů a provádět hello následující úlohy:

1. Klikněte na tlačítko *protokoly účtů úložiště*
2. Klikněte na tlačítko hello *přidat* úloh
3. Vyberte účet úložiště hello, který obsahuje hello protokolů diagnostiky
   * Tento účet může být účet úložiště classic nebo účet úložiště Azure Resource Manager
4. Vyberte hello chcete toocollect protokoly pro datový typ
   * Volby Hello jsou protokoly služby IIS. Události; Syslog (Linux); Protokoly trasování událostí pro Windows. Události služby prostředků infrastruktury
5. Hodnota Hello zdroje se automaticky vyplní podle hello datový typ a nelze změnit
6. Klikněte na tlačítko OK toosave hello konfigurace

Opakujte kroky 2 až 6 pro další úložiště účtů a datové typy, které chcete toocollect analýzy protokolů.

V přibližně 30 minut jsou možné toosee data z účtu úložiště hello v analýzy protokolů. Zobrazí se pouze data, která je zapsán toostorage po použití konfigurace hello. Analýzy protokolů číst hello už existující data z účtu úložiště hello.

> [!NOTE]
> portál Hello neověřuje, že hello zdroj existuje v účtu úložiště hello nebo pokud probíhá zápis nová data.
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Povolit Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekce pomocí prostředí PowerShell
Hello použijte kroky [tooindex konfigurace analýzy protokolů Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse prostředí PowerShell tooread z Azure diagnostiky, které jsou napsané tootable úložiště.

Pomocí Azure PowerShell můžete přesněji určit hello události, které jsou zapsány tooAzure úložiště.
Další informace najdete v tématu [povolení diagnostiky v Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).

Můžete povolit a aktualizovat Azure diagnostics pomocí hello následující skript prostředí PowerShell.
Tento skript můžete také použít s vlastní konfiguraci protokolování.
Upravte účet úložiště hello skriptu tooset hello, název služby a název virtuálního počítače.
skript Hello používá rutiny pro klasické virtuální počítače.

Zkontrolujte hello následující ukázka skriptu, zkopírujte jej, upravte ho požadovaným způsobem, uložit hello ukázka jako soubor skriptu prostředí PowerShell a potom spusťte hello skriptu.

```
    #Connect tooAzure
    Add-AzureAccount

    # settings toochange:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert tooconfig format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of hello extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Další kroky
* [Shromažďovat protokoly a metriky pro služby Azure](log-analytics-azure-storage.md) podporovaných službami Azure.
* [Povolit řešení](log-analytics-add-solutions.md) tooprovide vhled do dat hello.
* [Použijte vyhledávací dotazy](log-analytics-log-searches.md) tooanalyze hello data.
