---
title: "aaaAdd monitorování & diagnostiky tooan virtuální počítač Azure | Microsoft Docs"
description: "Pomocí toocreate šablony správce prostředků Azure nového virtuálního počítače Windows Azure diagnostics rozšíření."
services: virtual-machines-windows
documentationcenter: 
author: sbtron
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8cde8fe7-977b-43d2-be74-ad46dc946058
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: saurabh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8a831421a0f9d38c09d51cf8c2e6dff913c77ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>Monitorování a Diagnostika pomocí šablony virtuálního počítače s Windows a Azure Resource Manager
Hello rozšíření diagnostiky Azure poskytuje hello monitorování a Diagnostika možnosti v systému Windows na základě virtuální počítač Azure. Tyto funkce na virtuálním počítači hello můžete povolit, přiložením rozšíření hello jako součást hello šablony azure resource Manageru. V tématu [vytváření šablon Azure Resource Manager pomocí rozšíření virtuálního počítače](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) Další informace o včetně všechna rozšíření v rámci šablony virtuálního počítače. Tento článek popisuje, jak můžete přidat šablonu virtuálního počítače windows rozšíření tooa aplikace hello Azure Diagnostics.  

## <a name="add-hello-azure-diagnostics-extension-toohello-vm-resource-definition"></a>Přidat definici prostředků hello Azure Diagnostics rozšíření toohello virtuálních počítačů
rozšíření diagnostiky hello tooenable na virtuální počítač Windows musíte rozšíření hello tooadd jako prostředek virtuálního počítače v hello šablona Resource Manageru.

Pro jednoduché Resource Manager na bázi virtuálního počítače přidejte hello rozšíření konfigurace toohello *prostředky* pole pro hello virtuálního počítače: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Další běžné konvence je přidat konfigurace rozšíření hello v hello kořenový uzel prostředků šablony hello místo definice prostředků uzlu hello virtuálního počítače. S tímto přístupem je nutné zadat hierarchický vztah mezi hello rozšíření a hello virtuální počítač s hello tooexplicitly *název* a *typ* hodnoty. Například: 

    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

rozšíření Hello je vždy přidružený hello virtuálního počítače, můžete buď přímo přímo definovat uzlu prostředků hello virtuálního počítače nebo zadejte na základní úrovni hello a použít tooassociate hierarchické konvence pojmenování hello je s virtuálním hello počítač.

Konfigurace je pro rozšíření hello sady škálování virtuálního počítače zadaná v hello *extensionProfile* vlastnost hello *VirtualMachineProfile*.

Hello *vydavatele* vlastnosti s hodnotou hello **Microsoft.Azure.Diagnostics** a hello *typ* vlastnosti s hodnotou hello **IaaSDiagnostics** jednoznačně rozšíření Azure Diagnostics hello.

Hello hodnotu hello *název* vlastnost lze použít toorefer toohello rozšíření ve skupině prostředků hello. Nastavení se konkrétně příliš**Microsoft.Insights.VMDiagnosticsSettings** ho povolí toobe snadno identifikovaný hello zajistit, portálu Azure, který hello monitorování grafy se zobrazí správně v hello portálu Azure.

Hello *typeHandlerVersion* Určuje verzi hello hello rozšíření chcete toouse. Nastavení *autoUpgradeMinorVersion* příliš malé verze**true** zajistí, že dostanete nejnovější dílčí verzi hello hello rozšíření, která je k dispozici. Důrazně doporučujeme vždy nastavit *autoUpgradeMinorVersion* tooalways být **true** , abyste měli vždy toouse hello rozšíření na nejnovější dostupné diagnostiky se všemi hello nové funkce a chyb opravy. 

Hello *nastavení* prvek obsahuje vlastnosti konfigurace hello rozšíření, která můžete nastavit a čtení zpět z rozšíření hello (někdy označují tooas veřejné konfigurace). Hello *xmlcfg* vlastnost obsahuje konfigurační soubor xml na základě protokolů diagnostiky hello, čítačích výkonu atd, které bude shromažďována agentem hello diagnostiky. V tématu [schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/dn782207.aspx) Další informace o schématu xml hello, sám sebe. Běžnou praxí je toostore hello skutečné xml konfigurace jako base64 a proměnnou v hello šablony Azure Resource Manageru a pak zřetězení zakódovat je hodnota hello tooset *xmlcfg*. Části hello na [proměnné konfigurace diagnostiky](#diagnostics-configuration-variables) toounderstand informace o tom, jak toostore hello kód xml v proměnné. Hello *storageAccount* vlastnost určuje název hello hello úložiště účet toowhich diagnostická data budou přeneseny. 

Hello vlastnosti v *protectedSettings* (někdy označují tooas privátní konfigurace) můžete nastavit, ale nemůže přečíst zpět po nastaveno. Hello jen pro zápis povaha *protectedSettings* je vhodná pro ukládání tajné klíče jako klíč účtu úložiště hello zápis hello diagnostická data.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Zadání účtu úložiště diagnostiky jako parametry
Hello diagnostiky rozšíření fragmentu kódu json výše předpokládá dva parametry *existingdiagnosticsStorageAccountName* a *existingdiagnosticsStorageResourceGroup* toospecify hello diagnostiky účet úložiště, kde bude uložena diagnostická data. Zadání účtu úložiště diagnostiky hello jako parametr je účet úložiště diagnostiky hello snadno toochange různých prostředích například může být vhodné toouse účet úložiště různé diagnostiky pro testování a jinou pro vaše produkčním nasazení.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "hello name of an existing storage account toowhich diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "hello resource group for hello storage account specified in existingdiagnosticsStorageAccountName"
              }
        }

Je nejlepší postup toospecify účtu úložiště diagnostiky v jiné skupině prostředků než hello skupinu prostředků pro virtuální počítač hello. Skupinu prostředků lze považovat za toobe jednotka nasazení se vlastní životnost, virtuální počítač můžete nasadit a znovu nasadit jako nové aktualizace konfigurace probíhají tooit ale může být vhodné toocontinue ukládání dat diagnostiky hello v hello stejný účet úložiště mezi tyto nasazení virtuálních počítačů. S hello účet úložiště ve umožňuje jiného prostředku hello úložiště účet tooaccept data z různých nasazení virtuálních počítačů, takže je easy tootroubleshoot problémy napříč hello různých verzí.

> [!NOTE]
> Pokud vytvoříte šablonu virtuálního počítače windows ze sady Visual Studio hello výchozí účet úložiště může být nastaveno toouse hello stejný účet úložiště, kde je načtený hello pevný disk virtuálního počítače. Toto je počáteční nastavení toosimplify hello virtuálních počítačů. Je potřeba zvážit znovu hello šablony toouse jiný účet úložiště, může být předán jako parametr. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Proměnné konfigurace diagnostiky
Definuje Hello diagnostiky rozšíření fragmentu kódu json výše *accountid* proměnné toosimplify získávání hello klíč účtu úložiště pro úložiště diagnostiky hello:   

    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


Hello *xmlcfg* vlastnost pro rozšíření diagnostiky hello je definovaná pomocí více proměnných, které jsou zřetězeny společně. Hello hodnoty těchto proměnných jsou ve formátu xml, proto potřebují toobe správně uvozeny zpětným lomítkem při nastavování hello json proměnné.

Hello následující text popisuje hello diagnostiky konfigurace xml, který shromažďuje čítače úrovně výkonu standardní systému spolu se některé protokoly událostí systému windows a protokolů diagnostiky infrastruktury. Je uvozena a zda je správně naformátovaná tak, aby hello konfigurace lze přímo vložit do části hello proměnných šablony. V tématu hello [schéma konfigurace diagnostiky](https://msdn.microsoft.com/library/azure/dn782207.aspx) více lidského čitelný příklad hello konfigurační soubor xml.

        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Hello metriky definice xml uzlu v hello výše konfigurace je důležité konfigurační element jako definuje, jak se čítače výkonu hello definované dříve v hello xml v *PerformanceCounter* uzlu budou seskupeny a uložené. 

> [!IMPORTANT]
> Tyto metriky jednotky hello monitorování grafů a výstrahy v hello portálu Azure.  Hello **metriky** uzel s hello *resourceID* a **MetricAggregation** musí obsahovat hello konfigurace diagnostiky pro virtuální počítač, pokud chcete, aby toosee hello virtuálních počítačů data monitorování v hello portálu Azure. 
> 
> 

Hello následuje příklad xml hello metriky definice: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

Hello *resourceID* atribut jednoznačně identifikuje hello virtuálního počítače v rámci vašeho předplatného. Ujistěte se, zda text hello subscription() toouse a resourceGroup() funguje tak, aby hello šablony automaticky aktualizuje tyto hodnoty podle hello předplatném nebo skupině prostředků, které nasazujete.

Pokud vytvoříte více virtuálních počítačů ve smyčce, pak bude mít toopopulate hello *resourceID* hodnotu s toocorrectly funkce copyIndex() rozlišení jednotlivých jednotlivých virtuálních počítačů. Hello *xmlCfg* hodnota může být aktualizovaný toosupport to následujícím způsobem:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Hello hodnotu MetricAggregation *PT1H* a *PT1M* označily agregace za minutu a agregace přes hodinu.

## <a name="wadmetrics-tables-in-storage"></a>WADMetrics tabulek v úložišti
Konfigurace metriky Hello výše bude generovat tabulky ve vašem účtu úložiště diagnostiky s hello následující zásady vytváření názvů:

* **WADMetrics** : standardní předponou pro všechny tabulky WADMetrics
* **PT1H** nebo **PT1M** : označuje, že tabulka hello obsahuje agregovaná data více než 1 hodina nebo 1 minuta
* **P10D** : označuje, že tabulka hello budou obsahovat data pro 10 dní od hello tabulky při spuštění, shromažďování dat
* **V2S** : řetězcová konstanta
* **RRRRMMDD** : hello datum, na které hello tabulky shromažďování dat

Příklad: *WADMetricsPT1HP10DV2S20151108* budou obsahovat data metriky agregovat přes hodinu 10 dní od 11. listopadu 2015    

Každá tabulka WADMetrics bude obsahovat hello následující sloupce:

* **PartitionKey**: hello partitionkey vytvořeným v závislosti na hello *resourceID* hodnotu toouniquely identifikovat hello prostředků virtuálního počítače. pro např: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
* **RowKey** : formát hello způsobem <Descending time tick>:<Performance Counter Name>. Hello sestupném značek výpočet doby je maximální doba rysky minus hello čas začátku hello hello agregace období. Například Pokud hello ukázka období spustit na 10. listopadu 2015 a 00:00Hrs UTC pak hello výpočtu by: DateTime.MaxValue.Ticks - (nové DateTime(2015,11,10,0,0,0,DateTimeKind.Utc). Rysky). Pro klíč řádku hello výkonu hello paměti bajty k dispozici čítače bude vypadat jako: 2519551871999999999__:005CMemory:005CAvailable:0020 bajtů
* **Název_čítače** : je hello název čítače výkonu hello. To odpovídá hello *counterSpecifier* definované v hello xml konfigurace.
* **Maximální** : hello maximální hodnota čítače výkonu hello období hello agregace.
* **Minimální** : hello minimální hodnota čítače výkonu hello období hello agregace.
* **Celkový počet** : hello součet všech hodnot čítač výkonu hello hlášené období hello agregace.
* **Počet** : Celkový počet hodnot hello hlášené pro čítač výkonu hello.
* **Průměrná** : hello průměr (celkem nebo počet) hodnota čítače výkonu hello období hello agregace.

## <a name="next-steps"></a>Další kroky
* Pro dokončení vzorové šablony z virtuálního počítače s Windows pomocí rozšíření diagnostiky najdete v části [201-vm monitorování--rozšíření diagnostiky](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* Nasazení pomocí šablony správce prostředků hello [prostředí Azure PowerShell](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [příkazového řádku Azure](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md)

