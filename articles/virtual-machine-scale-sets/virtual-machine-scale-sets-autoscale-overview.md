---
title: "škálování aaaAutomatic a virtuálního počítače sady škálování | Microsoft Docs"
description: "Další informace o použití diagnostiky a škálování prostředky tooautomatically škálování virtuálních počítačů v sadě škálování."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: adegeo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 25f54b693e3c991577238242008c262023ed479c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-automatic-scaling-and-virtual-machine-scale-sets"></a>Jak toouse automatické škálování a sady škálování virtuálního počítače
Automatické škálování virtuálních počítačů v sadě škálování je hello vytvoření nebo odstranění počítačů v hello nastavit jako potřeby toomatch požadavky na výkon. S růstem hello objem práce aplikace může vyžadovat další prostředky tooenable ho tooeffectively provádět úlohy.

Automatické škálování je automatizovaný proces, pomáhá usnadňují režie na správu. Protože se sníží režie, není nutné toocontinually sledování výkonu systému nebo rozhodnout, jak toomanage prostředky. Škálování je elastické proces. Další prostředky se dá přidat jako zvyšuje zatížení hello. A jako vyžádání snížení prostředky můžete odebrané toominimize náklady a zachovat úrovně výkonu.

Nastavte automatické škálování na škále nastavit pomocí šablony, prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo hello portál Azure Azure Resource Manager.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Nastavení škálování pomocí šablony Resource Manageru
Namísto nasazení a samostatně spravovat každý prostředek vaší aplikace, použijte šablonu, která nasadí všechny prostředky v rámci jediné koordinované operace. V šabloně hello prostředky aplikace jsou definované a jsou zadány parametry nasazení pro různá prostředí. Šablona Hello se skládá z JSON a výrazy, můžete použít hodnoty tooconstruct pro vaše nasazení. toolearn víc, podívejte se na [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

V šabloně hello zadejte element kapacity hello:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

Kapacita identifikuje hello počet virtuálních počítačů v sadě hello. Můžete ručně změnit hello kapacity nasazení šablony s jinou hodnotou. Pokud nasazujete kapacitou hello tooonly změny šablony, můžete zahrnout pouze element SKU hello s kapacitou hello aktualizovat.

Hello kapacity škálovací sady můžete být automaticky upravována pomocí kombinace hello **autoscaleSettings** prostředků a hello rozšíření diagnostiky.

### <a name="configure-hello-azure-diagnostics-extension"></a>Konfigurace rozšíření Azure Diagnostics hello
Automatické škálování provést pouze pokud se kolekce metriky úspěšné na každém virtuálním počítači v sadě škálování hello. Hello rozšíření diagnostiky Azure poskytuje možnosti pro monitorování a Diagnostika hello vyhovujících potřebám kolekce metriky hello hello škálování prostředku. Rozšíření hello můžete nainstalovat v rámci šablony Resource Manageru hello.

Tento příklad ukazuje hello proměnné, které se používají v rozšíření diagnostiky hello šablony tooconfigure hello:

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

Parametry jsou k dispozici při nasazení šablony hello. V tomto příkladu, hello název účtu úložiště hello (které jsou uložená data) a hello je zadaný název sady škálování hello (kdy se shromažďují data). Také v tomto příkladu systému Windows Server se shromažďují pouze hello čítače výkonu počtu vláken. Všechny hello čítače výkonu k dispozici v systému Windows nebo Linux lze použít toocollect diagnostické informace a můžou být součástí konfigurace rozšíření hello.

Tento příklad ukazuje definici rozšíření hello hello v šabloně hello:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "Microsoft.Insights.VMDiagnosticsSettings",
      "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
          "storageAccount": "[variables('diagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
          "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
          "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
          "storageAccountEndPoint": "https://core.windows.net"
        }
      }
    }
  ]
}
```

Po spuštění rozšíření diagnostiky hello hello data ukládají a shromažďují v tabulce v hello účet úložiště, který určíte. V hello **WADPerformanceCounters** tabulky, můžete najít hello shromažďovaných dat:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-hello-autoscalesettings-resource"></a>Konfigurace prostředků autoScaleSettings hello
prostředek autoscaleSettings Hello používá hello informace z toodecide rozšíření diagnostiky hello zda tooincrease nebo snížení hello počet virtuálních počítačů v hello škálování nastavit.

Tento příklad ukazuje hello konfigurace hello prostředku v šabloně hello:

```json
{
  "type": "Microsoft.Insights/autoscaleSettings",
  "apiVersion": "2015-04-01",
  "name": "[concat(parameters('resourcePrefix'),'as1')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
  ],
  "properties": {
    "enabled": true,
    "name": "[concat(parameters('resourcePrefix'),'as1')]",
    "profiles": [
      {
        "name": "Profile1",
        "capacity": {
          "minimum": "1",
          "maximum": "10",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 650
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 550
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ],
    "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
  }
}
```

V předchozím příkladu hello vytvoří se dvě pravidla pro definování hello automatické škálování akce. první pravidlo Hello definuje akce škálování hello a druhé pravidlo hello definuje hello škálování v akci. Tyto hodnoty jsou k dispozici v pravidlech hello:

| Pravidlo | Popis |
| ---- | ----------- |
| metricName        | Tato hodnota je hello stejné jako čítače výkonu hello jste definovali v hello wadperfcounter proměnnou pro rozšíření diagnostiky hello. V předchozím příkladu hello se používá čítač počtu vláken hello.    |
| metricResourceUri | Tato hodnota je identifikátor prostředku hello škálovací sadu virtuálních počítačů hello. Tento identifikátor obsahuje hello název skupiny prostředků hello hello název zprostředkovatele prostředků hello a hello název tooscale sada škálování hello. |
| Časovými úseky         | Tato hodnota je hello členitost hello metriky, které jsou shromážděny. V předchozím příkladu hello data jsou shromažďována v intervalu jedné minuty. Tato hodnota se používá s hodnota timeWindow. |
| statistiky         | Tato hodnota určuje, jak hello metriky jsou kombinované tooaccommodate hello automatického škálování akce. Hello možné hodnoty jsou: průměr, minimum, maximum. |
| Hodnota timeWindow        | Tato hodnota je rozsah hello času, ve kterém se shromažďují instance data. Musí být mezi 5 minutami a 12 hodin. |
| Agregace času   | Tato hodnota určuje, jak by měla být kombinovány hello data, která se shromažďují v čase. Hello výchozí hodnota je průměr. Hello možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet. |
| Operátor          | Tato hodnota je hello operátor, který je použité toocompare hello metriky dat a hello prahovou hodnotu. Hello možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual. |
| Prahová hodnota         | Tato hodnota je hello hodnotu, která aktivuje hello akce škálování. Být jisti tooprovide dostatečná rozdíl mezi hello prahové hodnoty pro hello **Škálováním na více systémů** a **škálování v** akce. Pokud jste nastavili hello stejné hodnoty pro obě akce, připravená na hello systému konstantní změna, která zabraňuje v jeho implementaci akce škálování. Například nebude fungovat nastavení obou too600 vláken v předchozím příkladu hello. |
| Směr         | Tato hodnota určuje hello akce, která se provede, když je dosaženo hello prahovou hodnotu. Hello možné hodnoty jsou zvýšení nebo snížení. |
| type              | Tato hodnota je hello typ akce, který má vzniknout a musí být nastaven tooChangeCount. |
| hodnota             | Tato hodnota je hello počet virtuálních počítačů, které jsou přidány tooor odebrána z hello škálovací sadu. Tato hodnota musí být 1 nebo vyšší. |
| cooldown          | Tato hodnota je hello množství toowait času od poslední akce škálování hello předtím, než dojde k hello další akce. Tato hodnota musí být mezi minutu a jeden týden. |

V závislosti na hello čítače výkonu který používáte, některé hello elementů v konfiguraci šablony hello fungují jinak. V předchozím příkladu hello čítače výkonu hello je počet vláken, hello prahová hodnota je 650 pro akci škálování a hello prahová hodnota je 550 pro hello škálování v akci. Pokud používáte čítače, jako je % času procesoru, hello prahová hodnota nastavená toohello procento využití procesoru, který určuje škálování akce.

Když se aktivuje škálování akce, jako je například vysoké zatížení, je vyšší kapacita hello sady hello podle hello hodnotu v šabloně hello. Například v škálování nastavit kde kapacity hello je nastavený too3 a hodnotu akce škálování hello too1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Když hello aktuální zatížení příčiny hello vlákno průměrný počet toogo nad prahovou hodnotou hello 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

A **Škálováním na více systémů** akce se aktivuje, že příčiny hello kapacitu toobe sadu hello zvýšenou o hodnotu jedna:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

Hello výsledkem je, že virtuální počítač se přidá toohello škálovací sadu:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Po dobu cooldown pět minut je-li hello průměrný počet vláken na počítačích hello více než 600 jiný počítač přidání toohello sady. Je-li počet vláken průměrná hello níže 550, kapacity hello škálovací sady hello se sníží o jedna a počítač je odebrán ze sady hello.

## <a name="set-up-scaling-using-azure-powershell"></a>Nastavení škálování pomocí Azure PowerShell

Příklady toosee pomocí prostředí PowerShell tooset až automatické škálování, podívejte se na [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Nastavení škálování pomocí rozhraní příkazového řádku Azure

toosee příklady použití Azure CLI tooset až automatické škálování, podívejte se na [Azure monitorování napříč platformami CLI rychlý start ukázky](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-hello-azure-portal"></a>Nastavení škálování pomocí hello portálu Azure

Příklad použití toosee hello Azure portálu tooset až automatické škálování, vyhledejte v [vytvoření sady škálování virtuálního počítače pomocí portálu Azure hello](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Prozkoumat škálování akce

* **Azure Portal**  
Teď můžete získat omezené množství informací pomocí portálu hello.

* **Průzkumník prostředků Azure**  
Tento nástroj je nejlepší pro zkoumání hello aktuální stav škálovací sady hello. Postupujte podle této cestě a měli byste vidět sadu zobrazení instance hello hello měřítka, kterou jste vytvořili:  
**Odběry > {vaše předplatné} > Skupinyprostředků > {vaší skupiny prostředků} > poskytovatelé > Microsoft.Compute > virtualMachineScaleSets > {škálovací sadu} > virtuálních počítačů**

* **Azure PowerShell**  
Použijte tento příkaz tooget některé informace:

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* Připojte toohello jumpbox virtuálního počítače stejně, jako by všechny ostatní počítače a pak můžete vzdáleně přistupovat hello virtuálních počítačů v hello škálování sadu toomonitor jednotlivých procesů.

## <a name="next-steps"></a>Další kroky
* Podívejte se na [automaticky škálovat počítače ve Škálovací sadě virtuálního počítače](virtual-machine-scale-sets-windows-autoscale.md) toosee příklad jak toocreate škálování nastavit s nakonfigurovat automatické škálování.

* Najít příklady Azure monitorování monitorování funkcí v [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md)

* Další informace o funkcích oznámení v [použití automatického škálování akce toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).

* Další informace o tom příliš[protokoly auditu použití toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

* Další informace o [pokročilé scénáře škálování](virtual-machine-scale-sets-advanced-autoscale.md).
