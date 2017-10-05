---
title: "Shromažďování protokolů pomocí Azure Diagnostics | Microsoft Docs"
description: "Tento článek popisuje, jak nastavení Azure Diagnostics pro shromažďování protokolů z clusteru Service Fabric běžící v Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 9f7e1fa5-6543-4efd-b53f-39510f18df56
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 190a8a393f2e7d74a792db4efa81f94a18b02221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Shromažďování protokolů pomocí Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

Když používáte cluster služby Azure Service Fabric, je vhodné shromažďovat protokoly ze všech uzlů v centrálním umístění. S protokoly v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v aplikací a služeb spuštěných v daném clusteru.

Jeden způsob, jak nahrát a shromažďování protokolů je použití rozšíření diagnostiky Azure, který odešle protokoly do služby Azure Storage, Azure Application Insights nebo Azure Event Hubs. Protokoly nejsou přímo v úložišti nebo ve službě Event Hubs to užitečné. Ale externího procesu můžete použít ke čtení událostí z úložiště a umístit je v produktu, jako [analýzy protokolů](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) se dodává s komplexní protokolu vyhledávání a analýzy služby předdefinované.

## <a name="prerequisites"></a>Požadavky
Pomocí těchto nástrojů provést některé operace, které v tomto dokumentu:

* [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (související s Azure Cloud Services, ale má správné informace a příklady)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Klient Azure Resource Manager](https://github.com/projectkudu/ARMClient)
* [Šablona Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-to-collect"></a>Protokol zdroje, které můžete chtít shromažďovat
* **Protokoly Service Fabric**: vygenerované ze platformy na standardní trasování událostí pro Windows (ETW) a EventSource kanály. Protokoly mohou být jedním z několika typů:
  * Provozní události: protokoly pro operace, které provádí platformy Service Fabric. Mezi příklady patří vytvoření aplikací a služeb, uzel změny stavu a informace o upgradu.
  * [Programování událostí modelu Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
  * [Spolehlivé služby programovací model události](service-fabric-reliable-services-diagnostics.md)
* **Události aplikace**: události vygenerované z kódu vaší služby a zapsané pomocí pomocná třída EventSource, součástí šablony sady Visual Studio. Další informace o tom, jak psát protokoly z vaší aplikace, najdete v části [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>Nasazení rozšíření diagnostiky
Prvním krokem při shromažďování protokolů je k nasazení rozšíření diagnostiky na všech virtuálních počítačích v clusteru Service Fabric. Rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je do účtu úložiště, který určíte. Kroky trochu záviset na tom, zda používáte portál Azure nebo Azure Resource Manager. Kroky také lišit v závislosti na tom, jestli nasazení je součástí vytváření clusteru nebo je pro cluster, který již existuje. Podívejme se na postup pro jednotlivé scénáře.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Nasazení rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu
Nasazení rozšíření diagnostiky pro virtuální počítače v clusteru jako součást vytváření clusteru, použijte panel nastavení této diagnostiky vidět na následujícím obrázku. Povolit Reliable Actors nebo spolehlivé služby shromažďování událostí, ujistěte se, že diagnostiky je nastavená na **na** (výchozí nastavení). Po vytvoření clusteru, nelze změnit tato nastavení pomocí portálu.

![Nastavení diagnostiky Azure na portálu pro vytvoření clusteru](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Tým podpory Azure *vyžaduje* podporovat protokoly pomáhající při řešení žádosti o podporu, které vytvoříte. Tyto protokoly se shromažďují v reálném čase a jsou uložené v jednom z účty úložiště vytvořené ve skupině prostředků. Nastavení diagnostiky konfigurace událostí na úrovni aplikace. Tyto události zahrnují [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) události, [spolehlivé služby](service-fabric-reliable-services-diagnostics.md) události a některé události Service Fabric úrovni systému se neukládají v Azure Storage.

Produkty, jako [Elasticsearch](https://www.elastic.co/guide/index.html) nebo vlastního procesu můžete získat události z účtu úložiště. Aktuálně neexistuje žádný způsob, jak filtrovat nebo je naopak události, které se odesílají do tabulky. Pokud nemáte implementujte proces odebrání události v tabulce, tabulka pořád roste s tím.

Při vytváření clusteru pomocí portálu, důrazně doporučujeme, abyste si stáhli šablony **předtím, než kliknete na tlačítko OK** k vytvoření clusteru. Podrobnosti najdete v části [nastavit cluster Service Fabric pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Šablonu, kterou chcete provést změny později, budete potřebovat, protože nemůže provést některé změny pomocí portálu.

Pomocí následujících kroků můžete exportovat šablony z portálu. Tyto šablony však může být obtížné použít, protože mohou mít hodnoty null, kterým chybí požadované informace.

1. Otevřete vaší skupiny prostředků.
2. Vyberte **nastavení** zobrazíte panel nastavení.
3. Vyberte **nasazení** zobrazíte panel Historie nasazení.
4. Vyberte nasazení zobrazit podrobnosti o nasazení.
5. Vyberte **exportovat šablonu** zobrazíte panel šablony.
6. Vyberte **uložit do souboru** exportovat soubor .zip, který obsahuje šablony, parametr a soubory prostředí PowerShell.

Po exportu soubory, budete muset provedete změny. Upravte souboru parameters.JSON tímto kódem a odeberte **adminPassword** elementu. To způsobí výzva k zadání hesla, když se spustí skript nasazení. Když spouštíte skript nasazení, můžete chtít opravit hodnoty null parametru.

Použití stažené šabloně aktualizovat konfiguraci:

1. Extrahujte obsah do složky v místním počítači.
2. Upravte obsah, aby odráželo novou konfiguraci.
3. Spusťte PowerShell a přejděte do složky, které jste extrahovali obsah.
4. Spustit **deploy.ps1** a zadat ID předplatného, název skupiny prostředků (použijte stejný název se aktualizovat konfiguraci) a název jedinečný nasazení.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Nasazení rozšíření diagnostiky jako součást vytváření clusteru pomocí Azure Resource Manager
K vytvoření clusteru pomocí Správce prostředků, musíte přidat konfiguraci diagnostiky JSON šablony Resource Manageru úplné clusteru před vytvořením clusteru. Poskytujeme šablony Resource Manageru ukázkový pět VM cluster s konfigurací diagnostiky přidána jako součást naše ukázky šablony Resource Manageru. Zobrazí se v tomto umístění v galerii Azure Samples: [pěti uzly clusteru s ukázkou šablony správce prostředků diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).

Pokud chcete zobrazit nastavení diagnostiky v šabloně Resource Manager, otevřete soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**. Pokud chcete vytvořit cluster pomocí této šablony, vyberte **nasadit do Azure** tlačítko k dispozici na předchozí odkaz.

Alternativně můžete stáhnout ukázkový Resource Manager, provést změny a vytvořte cluster se změněné šablony pomocí `New-AzureRmResourceGroupDeployment` příkazu v okně prostředí Azure PowerShell. Zobrazit následující kód pro parametry, které můžete předat do příkazu. Podrobné informace o tom, jak nasazení skupiny prostředků pomocí prostředí PowerShell najdete v článku [nasazení skupiny prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

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

## <a name="update-diagnostics-to-collect-health-and-load-events"></a>Aktualizovat diagnostiky pro shromažďování stavu a načtení události

Počínaje 5.4 verzi Service Fabric, stavu a zatížení metriky události jsou k dispozici pro kolekci. Tyto události podle událostí generovaných systému nebo v kódu pomocí stavu nebo zatížení rozhraní API pro vytváření sestav, jako [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) nebo [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). To umožňuje pro agregaci a zobrazení stavu systému v čase a pro výstrahy na základě stavu nebo zatížení událostí. Chcete-li zobrazit tyto události v sadě Visual Studio prohlížeč diagnostických událostí přidat "Microsoft-ServiceFabric:4:0x4000000000000008" do seznamu zprostředkovatelů trasování událostí pro Windows.

Shromažďovat události, upravte zahrnout šablona resource Manageru

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

## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Aktualizovat diagnostiky ke sběru a odeslat protokoly z nové EventSource kanály
Chcete-li aktualizovat diagnostiky pro shromažďování protokolů z nové EventSource kanály, které představují novou aplikaci, která se chystáte nasadit, proveďte stejný postup jako v [předchozí části](#deploywadarm) pro nastavení diagnostiky pro existující cluster.

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

## <a name="next-steps"></a>Další kroky
Chcete-li podrobněji pochopit, jaké události je vhodné vyhledat při řešení potíží, najdete v části diagnostických událostí vygenerované pro [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) a [spolehlivé služby](service-fabric-reliable-services-diagnostics.md).

## <a name="related-articles"></a>Související články
* [Zjistěte, jak shromažďování čítače výkonu nebo protokoly pomocí rozšíření diagnostiky](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Řešení Service Fabric v analýzy protokolů](../log-analytics/log-analytics-service-fabric.md)

