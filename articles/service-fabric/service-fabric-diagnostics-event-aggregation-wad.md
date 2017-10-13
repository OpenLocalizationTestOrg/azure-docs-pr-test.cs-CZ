---
title: "Azure Service Fabric událostí agregace se systémem Windows Azure Diagnostics | Microsoft Docs"
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
ms.openlocfilehash: 5773361fdec4cb8ee54fa2856f6aa969d5dac4e9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Seskupení událostí a kolekce s použitím Windows Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Když používáte cluster služby Azure Service Fabric, je vhodné shromažďovat protokoly ze všech uzlů v centrálním umístění. S protokoly v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v aplikací a služeb spuštěných v daném clusteru.

Jeden způsob, jak nahrát a shromažďování protokolů je použití rozšíření Windows Azure Diagnostics (WAD), který odešle protokoly do služby Azure Storage a má také možnost odeslat protokoly služby Azure Application Insights nebo Event Hubs. Externího procesu můžete také použít ke čtení události z úložiště a umístit je o analýzy platformy produkt, například [analýzy protokolů OMS](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu.

## <a name="prerequisites"></a>Požadavky
Tyto nástroje se používají k provádění některých operací v tomto dokumentu:

* [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (související s Azure Cloud Services, ale má správné informace a příklady)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Klient Azure Resource Manager](https://github.com/projectkudu/ARMClient)
* [Šablona Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a>Protokol a událostí zdroje

### <a name="service-fabric-platform-events"></a>Události platformy Service Fabric
Jak je popsáno v [v tomto článku](service-fabric-diagnostics-event-generation-infra.md), Service Fabric nastaví můžete s kanály, několik protokolování se na pole, ve kterých jsou následující kanály snadno nakonfigurované s WAD k odeslání, sledování a diagnostická data do úložiště tabulky nebo jinde:
  * Provozní události: vyšší úrovně operace, které provádí platformy Service Fabric. Mezi příklady patří vytvoření aplikací a služeb, uzel změny stavu a informace o upgradu. Tyto jsou vygenerované jako protokoly trasování událostí pro Windows (ETW)
  * [Programování událostí modelu Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
  * [Spolehlivé služby programovací model události](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a>Události aplikace
 Události vygenerované z kódu vaší aplikací a služeb a zapisují pomocí pomocná třída EventSource, součástí šablony sady Visual Studio. Další informace o tom, jak psát EventSource protokoly z vaší aplikace, najdete v části [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>Nasazení rozšíření diagnostiky
Prvním krokem při shromažďování protokolů je k nasazení rozšíření diagnostiky na všech virtuálních počítačích v clusteru Service Fabric. Rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je do účtu úložiště, který určíte. Kroky trochu záviset na tom, zda používáte portál Azure nebo Azure Resource Manager. Kroky také lišit v závislosti na tom, jestli nasazení je součástí vytváření clusteru nebo je pro cluster, který již existuje. Podívejme se na postup pro jednotlivé scénáře.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Nasazení rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu Azure
Nasazení rozšíření diagnostiky pro virtuální počítače v clusteru jako součást vytváření clusteru, použijte panel nastavení diagnostiky vidět na následujícím obrázku – Ujistěte se, že diagnostiky je nastavená na **na** (výchozí nastavení). Po vytvoření clusteru, nelze změnit tato nastavení pomocí portálu.

![Nastavení diagnostiky Azure na portálu pro vytvoření clusteru](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

Při vytváření clusteru pomocí portálu, důrazně doporučujeme, abyste si stáhli šablony **předtím, než kliknete na tlačítko OK** k vytvoření clusteru. Podrobnosti najdete v části [nastavit cluster Service Fabric pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Šablonu, kterou chcete provést změny později, budete potřebovat, protože nemůže provést některé změny pomocí portálu.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Nasazení rozšíření diagnostiky jako součást vytváření clusteru pomocí Azure Resource Manager
K vytvoření clusteru pomocí Správce prostředků, musíte přidat konfiguraci diagnostiky JSON šablony Resource Manageru úplné clusteru před vytvořením clusteru. Poskytujeme šablony Resource Manageru ukázkový pět VM cluster s konfigurací diagnostiky přidána jako součást naše ukázky šablony Resource Manageru. Zobrazí se v tomto umístění v galerii Azure Samples: [pěti uzly clusteru s ukázkou šablony správce prostředků diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

Pokud chcete zobrazit nastavení diagnostiky v šabloně Resource Manager, otevřete soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**. Pokud chcete vytvořit cluster pomocí této šablony, vyberte **nasadit do Azure** tlačítko k dispozici na předchozí odkaz.

Alternativně můžete stáhnout ukázkový Resource Manager, provést změny a vytvořte cluster se změněné šablony pomocí `New-AzureRmResourceGroupDeployment` příkazu v okně prostředí Azure PowerShell. Zobrazit následující kód pro parametry, které můžete předat do příkazu. Podrobné informace o tom, jak nasazení skupiny prostředků pomocí prostředí PowerShell najdete v článku [nasazení skupiny prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Nasazení rozšíření diagnostiky k existujícímu clusteru
Pokud máte existující cluster, který nemá diagnostiky nasazené, nebo pokud chcete upravit existující konfigurace, můžete přidat nebo aktualizovat. Úprava šablony Resource Manageru, který se používá k vytvoření stávajícího clusteru nebo stáhnout šablonu z portálu, jak je popsáno výše. Upravte soubor template.json provedením následujících úloh.

Přidáte nový prostředek úložiště do šablony přidáním do oddílu prostředků.

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

 V dalším kroku přidejte do části Parametry hned za definice účet úložiště, mezi `supportLogStorageAccountName` a `vmNodeType0Name`. Nahraďte zástupný text *sem zadejte název účtu úložiště* s názvem účtu úložiště.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Potom aktualizovat `VirtualMachineProfile` souboru template.json přidáním následujícího kódu v rámci pole rozšíření. Nezapomeňte přidat čárka na začátku nebo konci, v závislosti na tom, kde je vložen.

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

Jakmile upravíte soubor template.json, jak je popsáno, znovu publikujte šablony Resource Manageru. Pokud byl exportován šablony, spuštěním souboru deploy.ps1 znovu publikuje uzamkl šablony. Poté, co nasadíte, ujistěte se, že **ProvisioningState** je **úspěšné**.

## <a name="collect-health-and-load-events"></a>Shromažďování stavu a načtení události

Počínaje 5.4 verzi Service Fabric, stavu a zatížení metriky události jsou k dispozici pro kolekci. Tyto události podle událostí generovaných systému nebo v kódu pomocí stavu nebo zatížení rozhraní API pro vytváření sestav, jako [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) nebo [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). To umožňuje pro agregaci a zobrazení stavu systému v čase a pro výstrahy na základě stavu nebo zatížení událostí. Chcete-li zobrazit tyto události v sadě Visual Studio prohlížeč diagnostických událostí přidat "Microsoft-ServiceFabric:4:0x4000000000000008" do seznamu zprostředkovatelů trasování událostí pro Windows.

Shromažďovat události, upravte šablonu Resource Manager, aby patří

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

Od verze 5.7 verzi Service Fabric [reverse proxy](service-fabric-reverseproxy.md) události jsou k dispozici pro kolekci.
Reverzní proxy server vysílá události do dvou kanálů, jeden obsahující chybové události odrážející zpracování chyb a další tak obsahující podrobné události týkající se všechny žádosti požadavku zpracovat reverzní proxy server. 

1. Shromažďovat události chyb: Chcete-li zobrazit tyto události v sadě Visual Studio prohlížeč diagnostických událostí přidat "Microsoft-ServiceFabric:4:0x4000000000000010" do seznamu zprostředkovatelů trasování událostí pro Windows.
Shromažďovat události z Azure clusterů, upravte šablonu Resource Manager, aby patří

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

2. Shromažďování všech požadavků zpracování události: V sadě Visual Studio na diagnostiky prohlížeče událostí, aktualizace Microsoft-ServiceFabric položku v seznamu zprostředkovatele trasování událostí pro Windows a "Microsoft-ServiceFabric:4:0x4000000000000020".
Azure Service Fabric clusterů upravte zahrnout šablona resource Manageru

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
> Doporučujeme uvážlivě umožňuje shromažďování událostí z tohoto kanálu, jak to shromažďuje všechny přenosy přes reverzní proxy server a můžete rychle spotřebovávají kapacitu úložiště.

Pro Azure Service Fabric clusterů jsou události ze všech uzlů shromažďovat a agregovat do SystemEventTable.
Podrobné řešení potíží s události reverzní proxy server, naleznete [reverzní proxy server diagnostiky průvodce](service-fabric-reverse-proxy-diagnostics.md).

## <a name="collect-from-new-eventsource-channels"></a>Shromáždit z nové EventSource kanály

K aktualizaci diagnostiky pro shromažďování protokolů z nové EventSource kanály, které představují novou aplikaci se o nasazení, provést stejné kroky podle předchozích pokynů pro nastavení diagnostiky pro existující clusteru.

Aktualizace `EtwEventSourceProviderConfiguration` v souboru template.json pro přidání položek nové kanály EventSource před použitím konfigurace aktualizací pomocí `New-AzureRmResourceGroupDeployment` příkaz prostředí PowerShell. Název zdroje událostí je definován jako součást kódu v souboru ServiceEventSource.cs generovaný Visual Studio.

Například pokud je váš zdroj událostí Moje Eventsource, přidejte následující kód do tabulky s názvem MyDestinationTableName umístit události z Moje Eventsource.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Pokud chcete shromáždit čítače výkonu nebo protokoly událostí, upravte šablony Resource Manageru pomocí příklady součástí [vytvoření virtuálního počítače s Windows pomocí monitorování a Diagnostika pomocí šablony Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Potom se znovu publikujte šablony Resource Manageru.

## <a name="collect-performance-counters"></a>Shromažďování čítačů výkonu

Ke shromažďování metrik výkonu z clusteru, přidejte čítače výkonu pro vaše "WadCfg > DiagnosticMonitorConfiguration" v šabloně Resource Manageru pro váš cluster. V tématu [čítače výkonu služby Fabric](service-fabric-diagnostics-event-generation-perf.md) pro čítače výkonu doporučujeme shromažďování.

Například Zde jsme nastavit jeden čítač výkonu vzorkovat každých 15 sekund (to se dá změnit a dodržuje formát "PT\<čas >\<jednotky >", například by PT3M ukázkové v intervalech tříminutové) a přenášená k příslušné úložiště tabulka každou minutu.

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
  
Pokud používáte jímky Application Insights, jak je popsáno v následující části a chcete tyto metriky objeví ve službě Application Insights, pak nezapomeňte přidat název podřízený v části "jímky", jak je uvedeno výše. Kromě toho zvažte vytvoření samostatné tabulky k odeslání čítače výkonu, tak jejich nemáte velkého množství lidí dat pocházejících z dalších kanálů protokolování, které jste povolili.


## <a name="send-logs-to-application-insights"></a>Odeslání protokolů s Application Insights

Odeslání dat monitorování a Diagnostika Statistika aplikace (AI) lze provést v rámci konfigurace WAD. Pokud se rozhodnete použít AI pro analýzu událostí a vizualizace, přečtěte si [analýza události a vizualizace s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) nastavit jímky AI jako součást vaší "WadCfg".

## <a name="next-steps"></a>Další kroky

Jakmile jste správně nakonfigurovali Azure diagnostics, zobrazí se data v tabulkách úložiště z protokolů trasování událostí pro Windows a EventSource. Pokud se rozhodnete použít OMS, Kibana nebo jakékoli jiné data analýzy a vizualizace platformě, která není přímo nakonfigurovanou v šabloně Resource Manager, ujistěte se, zda je nastavení platformou vaší volby pro čtení dat z těchto tabulek úložiště. Díky tomuto pro OMS je relativně jednoduchá a je vysvětleno v [událostí a protokolu analýzu prostřednictvím OMS](service-fabric-diagnostics-event-analysis-oms.md). Application Insights je bit ve speciálním případě v tomto smyslu získáte vzhledem k tomu může být nakonfigurovaný jako součást konfigurace rozšíření diagnostiky, takže [příslušném článku](service-fabric-diagnostics-event-analysis-appinsights.md) Pokud se rozhodnete použít AI.

>[!NOTE]
>Aktuálně neexistuje žádný způsob, jak filtrovat nebo je naopak události, které se odesílají do tabulky. Pokud nemáte implementujte proces odebrání události v tabulce, tabulka pořád roste s tím. V současné době je příkladem data výmazu dat služby spuštěné [sledovací zařízení ukázka](https://github.com/Azure-Samples/service-fabric-watchdog-service), a se doporučuje, když napsat jeden pro sami taky Pokud dobrý důvody pro ukládání protokolů nad rámec 30 nebo 90 den časový rámec.

* [Zjistěte, jak shromažďování čítače výkonu nebo protokoly pomocí rozšíření diagnostiky](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Analýza události a vizualizace s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Analýza události a vizualizace s OMS](service-fabric-diagnostics-event-analysis-oms.md)