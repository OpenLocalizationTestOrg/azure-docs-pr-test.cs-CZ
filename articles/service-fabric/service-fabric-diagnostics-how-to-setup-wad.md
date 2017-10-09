---
title: "aaaCollect protokolů pomocí Azure Diagnostics | Microsoft Docs"
description: "Tento článek popisuje, jak tooset si Azure Diagnostics toocollect protokolů z clusteru Service Fabric běžící v Azure."
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
ms.openlocfilehash: afbcfbe972b1847ef33bf0539b4398794b1bd56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Shromažďování protokolů pomocí Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

Když používáte cluster služby Azure Service Fabric, je vhodné toocollect hello protokoly ze všech uzlů hello v centrálním umístění. S protokoly hello v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v hello aplikací a služeb spuštěných v daném clusteru.

Jedním ze způsobů tooupload a shromáždit protokoly je toouse hello Azure Diagnostics rozšíření, které nahrávání protokolů tooAzure úložiště, Azure Application Insights nebo Azure Event Hubs. Hello protokoly nejsou přímo v úložišti nebo ve službě Event Hubs to užitečné. Ale můžete použít externího procesu tooread hello události z úložiště a umístit je v produktu, jako [analýzy protokolů](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) se dodává s komplexní protokolu vyhledávání a analýzy služby předdefinované.

## <a name="prerequisites"></a>Požadavky
Pomocí těchto nástrojů tooperform některé hello operací v tomto dokumentu:

* [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (týkající se tooAzure cloudové služby, ale má správné informace a příklady)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Klient Azure Resource Manager](https://github.com/projectkudu/ARMClient)
* [Šablona Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-toocollect"></a>Můžete chtít toocollect zdroje protokolu
* **Protokoly Service Fabric**: nevydává hello platformy toostandard trasování událostí pro Windows (ETW) a EventSource kanály. Protokoly mohou být jedním z několika typů:
  * Provozní události: protokoly pro operace, které hello provede platformy Service Fabric. Mezi příklady patří vytvoření aplikací a služeb, uzel změny stavu a informace o upgradu.
  * [Programování událostí modelu Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
  * [Spolehlivé služby programovací model události](service-fabric-reliable-services-diagnostics.md)
* **Události aplikace**: události vygenerované z kódu vaší služby a zapsané pomocí hello EventSource pomocná třída součástí šablony sady Visual Studio hello. Další informace o tom, jak toowrite protokolů z vaší aplikace naleznete v tématu [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Nasazení rozšíření diagnostiky hello
Hello prvním krokem při shromažďování protokolů je rozšíření diagnostiky hello toodeploy na každém hello virtuálních počítačů v clusteru Service Fabric hello. Hello rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je toohello účet úložiště, který určíte. kroky Hello trochu záviset na tom, zda používáte hello portál Azure nebo Azure Resource Manager. Hello kroky také lišit v závislosti na tom, jestli je součástí vytváření clusteru hello nasazení, nebo je pro cluster, který již existuje. Podívejme se na hello kroky pro jednotlivé scénáře.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-hello-portal"></a>Nasazení hello rozšíření diagnostiky jako součást vytváření clusteru prostřednictvím portálu hello
toodeploy hello diagnostiky rozšíření toohello virtuálních počítačů v clusteru hello jako součást vytváření clusteru, použijte panel nastavení diagnostiky hello znázorňuje následující obrázek hello. tooenable shromažďování událostí Reliable Actors nebo spolehlivé služby, ověřte, zda diagnostiky příliš**na** (hello výchozí nastavení). Po vytvoření clusteru hello, nelze změnit tato nastavení pomocí portálu hello.

![Nastavení Azure Diagnostics hello portálu pro vytvoření clusteru](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Hello tým podpory Azure *vyžaduje* podpora protokolů toohelp vyřešte všechny žádosti o podporu, které vytvoříte. Tyto protokoly se shromažďují v reálném čase a jsou uložené v jednom z hello účty úložiště vytvořené ve skupině prostředků hello. nastavení diagnostiky Hello nakonfigurovat událostí na úrovni aplikace. Tyto události zahrnují [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) události, [spolehlivé služby](service-fabric-reliable-services-diagnostics.md) události a některé úrovni systému toobe události Service Fabric uložené ve službě Azure Storage.

Produkty, jako [Elasticsearch](https://www.elastic.co/guide/index.html) nebo vlastního procesu můžete získat hello události z účtu úložiště hello. Aktuálně neexistuje žádný způsob, jak toofilter nebo výmazu dat hello události, které jsou odesílány toohello tabulky. Pokud nemáte implementovat zpracování tooremove událostí z tabulky hello, bude hello tabulky toogrow.

Při vytváření clusteru pomocí portálu hello, důrazně doporučujeme, abyste si stáhli hello šablony **předtím, než kliknete na tlačítko OK** toocreate hello clusteru. Podrobnosti najdete příliš[nastavit cluster Service Fabric pomocí šablony Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Změny toomake hello šablony budete potřebovat později, protože nemůže provést některé změny pomocí portálu hello.

Šablony můžete exportovat z portálu hello pomocí hello následující kroky. Tyto šablony však může být obtížnější toouse, protože mohou mít hodnoty null, kterým chybí požadované informace.

1. Otevřete vaší skupiny prostředků.
2. Vyberte **nastavení** panel nastavení toodisplay hello.
3. Vyberte **nasazení** panelu Historie nasazení toodisplay hello.
4. Vyberte podrobnosti o nasazení toodisplay hello hello nasazení.
5. Vyberte **exportovat šablonu** toodisplay hello šablony panelu.
6. Vyberte **uložit toofile** tooexport soubor .zip, který obsahuje šablony hello, parametr a soubory prostředí PowerShell.

Po exportu hello soubory, musíte toomake změna. Upravte souboru parameters.JSON tímto kódem hello a odeberte hello **adminPassword** elementu. Při spuštění skriptu nasazení hello způsobí výzva k zadání hesla hello. Když spouštíte skript nasazení hello, můžete mít hodnoty null parametru toofix.

toouse hello stáhnout šablony tooupdate konfigurace:

1. Rozbalte složku tooa hello obsah v místním počítači.
2. Upravte hello obsah tooreflect hello novou konfiguraci.
3. Spusťte PowerShell a změňte toohello složky, které jste extrahovali obsah hello.
4. Spustit **deploy.ps1** a vyplňte hello ID odběru, název skupiny prostředků hello (hello použijte stejný název tooupdate hello konfigurace) a název jedinečný nasazení.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Nasazení hello rozšíření diagnostiky jako součást vytváření clusteru pomocí Azure Resource Manager
toocreate cluster pomocí Resource Manageru, potřebujete tooadd hello diagnostiky konfigurace JSON toohello úplné clusteru šablony Resource Manageru před vytvořením clusteru hello. Poskytujeme šablony Resource Manageru ukázkový pět VM cluster s konfigurací diagnostiky přidat tooit jako součást naše ukázky šablony Resource Manageru. Zobrazí se v tomto umístění v galerii Azure Samples hello: [pěti uzly clusteru s ukázkou šablony správce prostředků diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).

nastavení diagnostiky hello toosee v hello šablony Resource Manageru, otevřete hello soubor azuredeploy.json a vyhledejte **IaaSDiagnostics**. toocreate cluster pomocí této šablony, vyberte hello **nasazení tooAzure** tlačítko k dispozici na předchozí odkaz hello.

Alternativně můžete stáhnout ukázkový hello Resource Manager, zkontrolujte změny tooit a vytvořte cluster se změněné šablony hello pomocí hello `New-AzureRmResourceGroupDeployment` příkazu v okně prostředí Azure PowerShell. Viz následující kód pro hello parametry, které se předávají v příkazu toohello hello. Podrobné informace o tom, jak toodeploy prostředek skupiny pomocí prostředí PowerShell, najdete v článku hello [nasazení skupiny prostředků pomocí šablony Azure Resource Manager hello](../azure-resource-manager/resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

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

## <a name="update-diagnostics-toocollect-health-and-load-events"></a>Aktualizovat stav a zatížení události toocollect diagnostiky

Od verze hello 5.4 Service Fabric, stavu a zatížení metriky události jsou k dispozici pro kolekci. Tyto události podle událostí generovaných hello systému nebo v kódu pomocí hello stavu nebo zatížení rozhraní API pro vytváření sestav, jako [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) nebo [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). To umožňuje pro agregaci a zobrazení stavu systému v čase a pro výstrahy na základě stavu nebo zatížení událostí. Přidat tyto události v sadě Visual Studio prohlížeč diagnostických událostí tooview "Microsoft-ServiceFabric:4:0x4000000000000008" toohello seznamu zprostředkovatelů trasování událostí pro Windows.

události hello toocollect, upravte tooinclude šablony správce prostředků hello

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

## <a name="update-diagnostics-toocollect-and-upload-logs-from-new-eventsource-channels"></a>Aktualizujte toocollect diagnostiky a odeslat protokoly z nové EventSource kanály
tooupdate diagnostiky toocollect protokoly z nové EventSource kanály, které představují novou aplikaci, která jste o toodeploy, provádět hello stejné kroky jako hello [předchozí části](#deploywadarm) pro nastavení diagnostiky pro existující cluster.

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

## <a name="next-steps"></a>Další kroky
toounderstand podrobněji událostech, které je vhodné vyhledat při řešení potíží, najdete v části hello diagnostických událostí vygenerované pro [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) a [spolehlivé služby](service-fabric-reliable-services-diagnostics.md).

## <a name="related-articles"></a>Související články
* [Zjistěte, jak čítače výkonu toocollect nebo protokoly pomocí hello rozšíření diagnostiky](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Řešení Service Fabric v analýzy protokolů](../log-analytics/log-analytics-service-fabric.md)

