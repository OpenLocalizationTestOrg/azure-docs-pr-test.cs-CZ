---
title: "aaaAzure Service Fabric událostí agregace s Windows Azure Diagnostics | Microsoft Docs"
description: "Další informace o agregaci a shromažďování událostí pomocí WAD pro monitorování a Diagnostika Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 4827ce66620e61c5b4a8682db55952333113188a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Seskupení událostí a kolekce s použitím Windows Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Když používáte cluster služby Azure Service Fabric, je vhodné toocollect hello protokoly ze všech uzlů hello v centrálním umístění. S protokoly hello v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v hello aplikací a služeb spuštěných v daném clusteru.

Jedním ze způsobů tooupload a shromáždit protokoly je toouse hello Windows Azure Diagnostics (WAD) přípony, který odešle tooAzure protokoly úložiště a také obsahuje hello možnost toosend protokoly tooAzure Application Insights nebo Event Hubs. Můžete také použít externího procesu tooread hello události z úložiště a umístit je o analýzy platformy produkt, například [analýzy protokolů OMS](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu.

## <a name="prerequisites"></a>Požadavky
Tyto nástroje jsou použité tooperform některé hello operací v tomto dokumentu:

* [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (týkající se tooAzure cloudové služby, ale má správné informace a příklady)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Klient Azure Resource Manager](https://github.com/projectkudu/ARMClient)
* [Šablona Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a>Protokol a událostí zdroje

### <a name="service-fabric-platform-events"></a>Události platformy Service Fabric
Jak je popsáno v [v tomto článku](service-fabric-diagnostics-event-generation-infra.md), Service Fabric nastaví můžete s kanály, několik protokolování se na pole, která hello následující kanály jsou snadno nakonfigurované WAD toosend monitorování a Diagnostika data tooa tabulku úložiště nebo jinde:
  * Provozní události: vyšší úrovně operace, které hello provede platformy Service Fabric. Mezi příklady patří vytvoření aplikací a služeb, uzel změny stavu a informace o upgradu. Tyto jsou vygenerované jako protokoly trasování událostí pro Windows (ETW)
  * [Programování událostí modelu Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
  * [Spolehlivé služby programovací model události](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a>Události aplikace
 Události vygenerované z vašich aplikací a služeb kódu a zapsané pomocí hello EventSource pomocná třída součástí hello šablony sady Visual Studio. Další informace o tom, jak toowrite EventSource protokolů z vaší aplikace naleznete v tématu [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Nasazení rozšíření diagnostiky hello
Hello prvním krokem při shromažďování protokolů je rozšíření diagnostiky hello toodeploy na každém hello virtuálních počítačů v clusteru Service Fabric hello. Hello rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je toohello účet úložiště, který určíte. kroky Hello trochu záviset na tom, zda používáte hello portál Azure nebo Azure Resource Manager. Hello kroky také lišit v závislosti na tom, jestli je součástí vytváření clusteru hello nasazení, nebo je pro cluster, který již existuje. Podívejme se na hello kroky pro jednotlivé scénáře.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Nasazení hello rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu Azure
toodeploy hello diagnostiky rozšíření toohello virtuálních počítačů v clusteru hello jako součást vytváření clusteru, použijte panel nastavení diagnostiky hello uvedené v následující hello obrázek – ověřte, zda diagnostiky příliš**na** (hello výchozí nastavení) . Po vytvoření clusteru hello, nelze změnit tato nastavení pomocí portálu hello.

![Nastavení Azure Diagnostics hello portálu pro vytvoření clusteru](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

Při vytváření clusteru pomocí portálu hello, důrazně doporučujeme, abyste si stáhli hello šablony **předtím, než kliknete na tlačítko OK** toocreate hello clusteru. Podrobnosti najdete příliš[nastavit cluster Service Fabric pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Změny toomake hello šablony budete potřebovat později, protože nemůže provést některé změny pomocí portálu hello.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Nasazení hello rozšíření diagnostiky jako součást vytváření clusteru pomocí Azure Resource Manager
toocreate cluster pomocí Resource Manageru, potřebujete tooadd hello diagnostiky konfigurace JSON toohello úplné clusteru šablony Resource Manageru před vytvořením clusteru hello. Poskytujeme šablony Resource Manageru ukázkový pět VM cluster s konfigurací diagnostiky přidat tooit jako součást naše ukázky šablony Resource Manageru. Zobrazí se v tomto umístění v galerii Azure Samples hello: [pěti uzly clusteru s ukázkou šablony správce prostředků diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

nastavení diagnostiky hello toosee v hello šablony Resource Manageru, otevřete hello soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**. toocreate cluster pomocí této šablony, vyberte hello **nasazení tooAzure** tlačítko k dispozici na předchozí odkaz hello.

Alternativně můžete stáhnout ukázkový hello Resource Manager, zkontrolujte změny tooit a vytvořte cluster se změněné šablony hello pomocí hello `New-AzureRmResourceGroupDeployment` příkazu v okně prostředí Azure PowerShell. Viz následující kód pro hello parametry, které se předávají v příkazu toohello hello. Podrobné informace o tom, jak toodeploy prostředek skupiny pomocí prostředí PowerShell, najdete v článku hello [nasazení skupiny prostředků pomocí šablony Azure Resource Manager hello](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a>Nasazení hello diagnostiky rozšíření tooan stávajícího clusteru
Pokud máte existující cluster, který nemá diagnostiky nasazené, nebo pokud chcete toomodify existující konfigurace, můžete přidat nebo aktualizovat. Upravit šablonu hello Resource Manager, který je použité toocreate hello existující cluster nebo stáhnout hello šablonu z portálu hello, jak je popsáno výše. Upravte soubor template.json hello provedením hello následující úlohy.

Přidáte novou šablonu toohello prostředků úložiště přidáním toohello oddílu prostředků.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Dál přidejte toohello parametry části bezprostředně za hello Definice účet úložiště, mezi `supportLogStorageAccountName` a `vmNodeType0Name`. Nahraďte zástupný text hello *sem zadejte název účtu úložiště* hello název účtu úložiště hello.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for hello application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for hello storage account that contains application diagnostics data from hello cluster"
      }
    },
```
Aktualizujte hello `VirtualMachineProfile` části souboru template.json hello přidáním hello následující kód do pole rozšíření hello. Být jisti tooadd čárka na začátku hello nebo hello end, v závislosti na tom, kde je vložen.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

Po úpravě souboru template.json hello, jak je popsáno, znovu publikujte šablony Resource Manageru hello. Pokud byl exportován hello šablony, znovu publikuje spouštění souboru deploy.ps1 hello uzamkl hello šablony. Poté, co nasadíte, ujistěte se, že **ProvisioningState** je **úspěšné**.

## <a name="collect-health-and-load-events"></a>Shromažďování stavu a načtení události

Od verze hello 5.4 Service Fabric, stavu a zatížení metriky události jsou k dispozici pro kolekci. Tyto události podle událostí generovaných hello systému nebo v kódu pomocí hello stavu nebo zatížení rozhraní API pro vytváření sestav, jako [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) nebo [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). To umožňuje pro agregaci a zobrazení stavu systému v čase a pro výstrahy na základě stavu nebo zatížení událostí. Přidat tyto události v sadě Visual Studio prohlížeč diagnostických událostí tooview "Microsoft-ServiceFabric:4:0x4000000000000008" toohello seznamu zprostředkovatelů trasování událostí pro Windows.

události hello toocollect, upravte tooinclude šablony Resource Manageru hello

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="collect-reverse-proxy-events"></a>Shromažďování událostí reverzní proxy server

Od verze hello 5.7 Service Fabric [reverse proxy](service-fabric-reverseproxy.md) události jsou k dispozici pro kolekci.
Reverzní proxy server vysílá události do dvou kanálů, jednu obsahující chybu události odrážející selhání zpracování požadavku a další jeden obsahující podrobné události týkající se všech požadavků hello zpracovat reverzní proxy server hello hello. 

1. Shromažďovat události chyb: tooview přidat tyto události v sadě Visual Studio prohlížeč diagnostických událostí "Microsoft-ServiceFabric:4:0x4000000000000010" toohello seznamu zprostředkovatelů trasování událostí pro Windows.
toocollect hello události z Azure clusterů, upravte tooinclude šablony Resource Manageru hello

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387920",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

2. Shromažďování všech požadavků zpracování události: V sadě Visual Studio na diagnostiky prohlížeče událostí, položku hello Microsoft ServiceFabric update v hello seznam zprostředkovatelů trasování událostí pro Windows příliš "Microsoft-ServiceFabric:4:0x4000000000000020".
Azure Service Fabric clusterů upravte tooinclude šablony správce prostředků hello

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387936",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```
> Doporučujeme toojudiciously Povolit shromažďování událostí z tohoto kanálu, jako to shromažďuje všechny přenosy přes reverzní proxy server hello a můžete rychle spotřebovávají kapacitu úložiště.

Pro Azure Service Fabric clusterů hello události ze všech uzlů hello jsou shromažďovány a agregovat do hello SystemEventTable.
Podrobné řešení potíží s událostí reverzní proxy server hello odkazovat hello [reverzní proxy server diagnostiky průvodce](service-fabric-reverse-proxy-diagnostics.md).

## <a name="collect-from-new-eventsource-channels"></a>Shromáždit z nové EventSource kanály

tooupdate diagnostiky toocollect protokoly z nové EventSource kanály, které představují novou aplikaci, která jste o toodeploy, provést hello stejný postup jako výše popsané hello nastavení diagnostiky pro existující cluster.

Aktualizovat hello `EtwEventSourceProviderConfiguration` kapitoly položky tooadd souboru template.json hello hello nové EventSource kanály než použijete hello konfigurace aktualizace pomocí hello `New-AzureRmResourceGroupDeployment` příkaz prostředí PowerShell. Název Hello hello zdroje událostí je definován jako součást kódu v souboru generovaný Visual Studio ServiceEventSource.cs hello.

Například pokud váš zdroj událostí s názvem Moje Eventsource, přidejte následující kód tooplace hello události z Moje Eventsource do tabulky s názvem MyDestinationTableName hello.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

čítače výkonu toocollect nebo protokoly událostí, změna šablony Resource Manageru hello pomocí hello příklady [vytvoření virtuálního počítače s Windows pomocí monitorování a Diagnostika pomocí šablony Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Potom se znovu publikujte šablony Resource Manageru hello.

## <a name="collect-performance-counters"></a>Shromažďování čítačů výkonu

metriky výkonu toocollect z clusteru, přidejte tooyour čítače výkonu hello "> WadCfg DiagnosticMonitorConfiguration" v hello šablony Resource Manageru pro váš cluster. V tématu [čítače výkonu služby Fabric](service-fabric-diagnostics-event-generation-perf.md) pro čítače výkonu doporučujeme shromažďování.

Například Zde jsme nastavit jeden čítač výkonu vzorkovat každých 15 sekund (to se dá změnit a způsobem hello formát "PT\<čas >\<jednotky >", například by PT3M ukázkové na tři minutách) a přenáší toohello příslušné úložiště tabulka každou minutu.

  ```json
  "PerformanceCounters": {
      "scheduledTransferPeriod": "PT1M",
      "PerformanceCounterConfiguration": [
          {
              "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
              "sampleRate": "PT15S",
              "unit": "Percent",
              "annotation": [
              ],
              "sinks": ""
          }
      ]
  }
  ```
  
Pokud používáte jímky Application Insights, jak je popsáno v následující části hello a chcete tyto metriky tooshow si ve službě Application Insights, zkontrolujte že tooadd hello podřízený název v části "jímky" hello, jak je uvedeno výše. Kromě toho, zvažte vytvoření samostatné tabulky toosend čítače výkonu, tak jejich nemáte velkého množství lidí si hello dat pocházejících z hello jinými protokolování kanály, které jste povolili.


## <a name="send-logs-tooapplication-insights"></a>Odeslání protokolů tooApplication statistiky

Odesílání dat tooApplication monitorování a Diagnostika přehledy (AI) lze provést v rámci konfigurace WAD hello. Pokud se rozhodnete toouse AI pro analýzu událostí a vizualizace, přečtěte si [analýza události a vizualizace s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) tooset až jímky AI jako součást vaší "WadCfg".

## <a name="next-steps"></a>Další kroky

Jakmile jste správně nakonfigurovali Azure diagnostics, zobrazí se data v tabulkách úložiště z EventSource protokoly a trasování událostí pro Windows hello. Pokud si zvolíte toouse OMS, Kibana nebo jakékoli jiné data analýzy a vizualizace platformě, která není nakonfigurovaná přímo hello šablony Resource Manageru, proveďte v hello data z těchto tabulek úložiště zda tooset až hello platforma tooread vašeho výběru. Díky tomuto pro OMS je relativně jednoduchá a je vysvětleno v [událostí a protokolu analýzu prostřednictvím OMS](service-fabric-diagnostics-event-analysis-oms.md). Application Insights je bit ve speciálním případě v tomto smyslu, protože může být nakonfigurovaný jako součást konfigurace rozšíření diagnostiky hello, takže získáte toohello [příslušném článku](service-fabric-diagnostics-event-analysis-appinsights.md) Pokud si zvolíte toouse AI.

>[!NOTE]
>Aktuálně neexistuje žádný způsob, jak toofilter nebo výmazu dat hello události, které jsou odesílány toohello tabulky. Pokud nemáte implementovat zpracování tooremove událostí z tabulky hello, bude hello tabulky toogrow. V současné době je příkladem data výmazu dat služby spuštěné v hello [sledovací zařízení ukázka](https://github.com/Azure-Samples/service-fabric-watchdog-service), a se doporučuje, když napsat jeden pro sami taky Pokud dobrý důvody pro vás toostore protokoly nad rámec 30 nebo 90 den časový rámec.

* [Zjistěte, jak čítače výkonu toocollect nebo protokoly pomocí hello rozšíření diagnostiky](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Analýza události a vizualizace s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Analýza události a vizualizace s OMS](service-fabric-diagnostics-event-analysis-oms.md)