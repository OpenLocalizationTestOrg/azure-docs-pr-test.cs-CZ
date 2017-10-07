---
title: "aaaVirtual počítačů v šablonu Azure Resource Manager | Microsoft Azure"
description: "Další informace o tom, jak je definován prostředek virtuálního počítače hello v šablonu Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f63ab5cc-45b8-43aa-a4e7-69dc42adbb99
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: davidmu
ms.openlocfilehash: 94adcbe5bf44be72ffc1b920461aed15c4fc025f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a>Virtuální počítače v šablonu Azure Resource Manager

Tento článek popisuje aspekty šablony Azure Resource Manager, které se vztahují toovirtual počítače. Tento článek popisuje kompletní šablonu pro vytvoření virtuálního počítače; k tomu potřebujete definice prostředku pro účty úložiště, síťová rozhraní, veřejné IP adresy a virtuální sítě. Další informace o tom, jak tyto prostředky je možné definovat společně najdete v tématu hello [názorný Průvodce šablonou Resource Manageru](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Existuje mnoho [šablony v galerii hello](https://azure.microsoft.com/documentation/templates/?term=VM) , zahrnout hello prostředků virtuálního počítače. Ne všechny elementy, které můžou být součástí šablony jsou popsané v tomto poli.

Tento příklad ukazuje oddíl typické prostředků šablony pro vytvoření zadaný počet virtuálních počítačů:

```json
"resources": [
  { 
    "apiVersion": "2016-04-30-preview", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[concat('myVM', copyindex())]", 
    "location": "[resourceGroup().location]",
    "copy": {
      "name": "virtualMachineLoop", 
      "count": "[parameters('numberOfInstances')]"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/myNIC', copyindex())]" 
    ], 
    "properties": { 
      "hardwareProfile": { 
        "vmSize": "Standard_DS1" 
      }, 
      "osProfile": { 
        "computername": "[concat('myVM', copyindex())]", 
        "adminUsername": "[parameters('adminUsername')]", 
        "adminPassword": "[parameters('adminPassword')]" 
      }, 
      "storageProfile": { 
        "imageReference": { 
          "publisher": "MicrosoftWindowsServer", 
          "offer": "WindowsServer", 
          "sku": "2012-R2-Datacenter", 
          "version": "latest" 
        }, 
        "osDisk": { 
          "name": "[concat('myOSDisk', copyindex())]",
          "caching": "ReadWrite", 
          "createOption": "FromImage" 
        },
        "dataDisks": [
          {
            "name": "[concat('myDataDisk', copyindex())]",
            "diskSizeGB": "100",
            "lun": 0,
            "createOption": "Empty"
          }
        ] 
      }, 
      "networkProfile": { 
        "networkInterfaces": [ 
          { 
            "id": "[resourceId('Microsoft.Network/networkInterfaces',
              concat('myNIC', copyindex()))]" 
          } 
        ] 
      },
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('https://', variables('storageName'), '.blob.core.windows.net')]"
        }
      } 
    },
    "resources": [ 
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
      {
        "name": "MyCustomScriptExtension",
        "type": "extensions",
        "apiVersion": "2016-03-30",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
        ],
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.7",
          "autoUpgradeMinorVersion": true,
          "settings": {
            "fileUris": [
              "[concat('https://', variables('storageName'),
                '.blob.core.windows.net/customscripts/start.ps1')]" 
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
          }
        }
      } 
    ]
  } 
]
``` 

> [!NOTE] 
>Tento příklad spoléhá na účet úložiště, která byla dříve vytvořena. Můžete vytvořit účet úložiště hello nasazením z šablony hello. Příklad Hello také závisí na rozhraní sítě a jeho závislé prostředky, které by definované v šabloně hello. Tyto prostředky se nezobrazí v příkladu hello.
>
>

## <a name="api-version"></a>Verze rozhraní API

Když nasadíte prostředky pomocí šablony, máte toospecify verzi rozhraní API toouse hello. Příklad Hello ukazuje hello prostředek virtuálního počítače pomocí tohoto elementu apiVersion:

```
"apiVersion": "2016-04-30-preview",
```

Hello verzi hello rozhraní API, které zadáte v šabloně ovlivňuje vlastnosti, které definujete v šabloně hello. Obecně byste měli vybrat hello nejnovější verzi rozhraní API, při vytváření šablony. Pro existující šablony můžete rozhodnout, zda chcete toocontinue pomocí dřívější verze rozhraní API nebo aktualizaci šablony pro hello nejnovější verze tootake výhod nových funkcí.

Použijte tyto příležitosti pro získání hello nejnovější verze rozhraní API:

- Rozhraní API REST - [zobrazit seznam všech poskytovatelů prostředků](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)
- PowerShell – [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)
- Rozhraní příkazového řádku Azure 2.0 - [az zprostředkovatele zobrazit](https://docs.microsoft.com/cli/azure/provider#show)

## <a name="parameters-and-variables"></a>Parametry a proměnné

[Parametry](../../resource-group-authoring-templates.md) usnadní vám toospecify hodnoty pro šablonu hello při každém spuštění. V příkladu hello se používá v této části Parametry:

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

Když nasadíte hello příklad šablony, zadejte hodnoty pro hello jméno a heslo účtu správce hello na každý virtuální počítač a hello počet toocreate virtuálních počítačů. Máte možnost hello zadání hodnot parametrů do samostatného souboru, který je spravován pomocí šablony hello nebo zadáním hodnot po zobrazení výzvy.

[Proměnné](../../resource-group-authoring-templates.md) usnadní vám tooset hodnot v šabloně hello jsou opakovaně použít v celé jeho nebo můžou časem změnit. Tato část proměnné se používá v příkladu hello:

```
"variables": { 
  "storageName": "mystore1",
  "accountid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name,
  '/providers/','Microsoft.Storage/storageAccounts/', variables('storageName'))]", 
  "wadlogs": "<WadCfg> 
    <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> 
      <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> 
      <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > 
        <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" />
      </WindowsEventLog>", 
  "wadperfcounters": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\">
      <PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\">
        <annotation displayName=\"Threads\" locale=\"en-us\"/>
      </PerformanceCounterConfiguration>
    </PerformanceCounters>", 
  "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters'), 
    '<Metrics resourceId=\"')]", 
  "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name , 
    '/providers/', 'Microsoft.Compute/virtualMachines/')]", 
  "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/>
    <MetricAggregation scheduledTransferPeriod=\"PT1M\"/>
    </Metrics></DiagnosticMonitorConfiguration>
    </WadCfg>"
}, 
```

Při nasazování šablony příklad hello hodnoty proměnné se používají pro hello název a identifikátor hello vytvořili účet úložiště. Proměnné jsou také použít tooprovide hello nastavení pro rozšíření diagnostiky hello. Použití hello [osvědčené postupy pro vytváření šablon Azure Resource Manager](../../resource-manager-template-best-practices.md) toohelp můžete určit, jak mají toostructure hello parametrů a proměnných ve vaší šabloně.

## <a name="resource-loops"></a>Smyčky prostředků

Pokud potřebujete více než jeden virtuální počítač pro vaši aplikaci, můžete použít element kopírování v šabloně. Tento volitelný element projde vytváření hello počet virtuálních počítačů, které jste zadali jako parametr:

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

Všimněte si v příkladu hello, který hello index smyčky se také používá při zadání některé z hello hodnoty pro prostředek hello. Například pokud jste zadali počet instancí názvů tři, hello hello disků operačního systému jsou myOSDisk1, myOSDisk2 a myOSDisk3:

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
>Tento příklad používá spravované disky pro virtuální počítače hello.
>
>

Mějte na paměti, že vytváření smyčku pro jeden prostředek v šabloně hello může vyžadovat jste toouse hello smyčky při vytváření nebo přístup k dalším prostředkům. Víc virtuálních počítačů nelze použít například hello stejné síťové rozhraní, takže pokud vaše šablona projde vytváření tři virtuální počítače musí také cykly procesem vytvoření tři síťových rozhraní. Při přiřazování tooa rozhraní sítě virtuálních počítačů, index smyčky hello je použité tooidentify ho:

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a>Závislosti

Většina prostředky závisí na jiné prostředky toowork správně. Virtuální počítače musí být přidružený virtuální sítě a toodo, že tato služba vyžaduje síťové rozhraní. Hello [dependsOn](../../resource-group-define-dependencies.md) element je použité toomake se rozhraní sítě, hello je připraven toobe využít, než se vytvoří virtuální počítače hello:

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

Správce prostředků nasadí současně všechny prostředky, které nejsou závislé na jiný prostředek nasazuje. Buďte opatrní při nastavení závislostí, protože můžou nechtěně způsobit snížení nasazení tak, že zadáte nepotřebné závislosti. Závislosti můžete zřetězené prostřednictvím více prostředků. Například hello síťové rozhraní závisí na hello veřejnou IP adresu a virtuální síťové prostředky.

Jak poznáte, pokud je třeba provést závislost? Podívejte se na hello hodnoty, které můžete zadat v šabloně hello. Pokud element v definici prostředků virtuálního počítače hello odkazuje tooanother prostředků, která je nasazena v hello stejné šablony, musíte závislost. Virtuální počítač příklad určují profil sítě:

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

tooset tato vlastnost musí existovat hello síťové rozhraní. Proto musíte závislost. Musíte taky tooset závislost, když jeden prostředek (podřízená) je definována v rámci jiný prostředek (nadřazené). Například nastavení pro diagnostiku hello a rozšíření vlastních skriptů jsou obě definované jako podřízené prostředky hello virtuálního počítače. Nelze vytvořit dokud hello virtuální počítač existuje. Proto i prostředky jsou označeny jako závislé na hello virtuálního počítače.

## <a name="profiles"></a>Profily

Několik elementy profil se používá při definování prostředek virtuálního počítače. Některé jsou vyžadovány a některé jsou volitelné. Například hello položka hardwareProfile, osProfile, storageProfile a networkProfile elementy jsou požadovány, ale hello diagnosticsProfile je volitelný. Tyto profily definovat nastavení, jako:
   
- [velikost](sizes.md)
- [název](/architecture/best-practices/naming-conventions) a přihlašovací údaje
- disk a [nastavení operačního systému](cli-ps-findimage.md)
- [síťové rozhraní](../../virtual-network/virtual-networks-multiple-nics.md) 
- Diagnostika spouštění

## <a name="disks-and-images"></a>Disky a obrázků
   
V Azure, můžete soubory vhd představují [disky nebo bitové kopie](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Když hello operační systém v souboru virtuálního pevného disku je specializovaná toobe konkrétní virtuální počítač, je tooas odkazuje na disk. Když je zobecněn hello operačního systému v souboru vhd toobe používat toocreate hodně virtuálních počítačů, je odkazované tooas bitovou kopii.   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a>Vytvoření nové virtuální počítače a nové disky z image platformy

Když vytvoříte virtuální počítač, musíte rozhodnout, jaké toouse operačního systému. Element imageReference element Hello je použité toodefine hello operační systém nový virtuální počítač. Hello příklad ukazuje definici pro operační systém Windows Server:

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

Pokud chcete toocreate operačního systému Linux, můžete použít tuto definici:

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

Nastavení konfigurace pro disk operačního systému hello jsou přiřazeny hello osDisk element. Hello příklad definuje nový disk spravované hello ukládání do mezipaměti v režimu sadu příliš**ReadWrite** a tento disk hello se vytváří z [image platformy](cli-ps-findimage.md):

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a>Vytvoření nové virtuální počítače z existujícího spravovaného disků

Pokud chcete toocreate virtuální počítače z existujícího disků, odeberte element imageReference hello a hello osProfile elementy a definovat toto nastavení disku:

```
"osDisk": { 
  "osType": "Windows",
  "managedDisk": { 
    "id": "[resourceId('Microsoft.Compute/disks', [concat('myOSDisk', copyindex())])]" 
  }, 
  "caching": "ReadWrite",
  "createOption": "Attach" 
},
```

### <a name="create-new-virtual-machines-from-a-managed-image"></a>Vytvoření nových virtuálních počítačů ze spravovaných bitové kopie

Pokud chcete virtuální počítač z bitové kopie spravované toocreate, změňte hello elementu imageReference element a definovat toto nastavení disku:

```
"storageProfile": { 
  "imageReference": {
    "id": "[resourceId('Microsoft.Compute/images', 'myImage')]"
  },
  "osDisk": { 
    "name": "[concat('myOSDisk', copyindex())]",
    "osType": "Windows",
    "caching": "ReadWrite", 
    "createOption": "FromImage" 
  }
},
```

### <a name="attach-data-disks"></a>Připojte datových disků

Můžete přidat data toohello disky virtuálních počítačů. Hello [počet disků](sizes.md) závisí na velikosti hello disk operačního systému, který používáte. S hello nastavit velikost virtuálních počítačů hello tooStandard_DS1_v2 hello maximální počet datových disků, které nebylo možné přidat toohello je je dva. V příkladu hello jeden spravovaný datový disk je přidáván tooeach virtuálních počítačů:

```
"dataDisks": [
  {
    "name": "[concat('myDataDisk', copyindex())]",
    "diskSizeGB": "100",
    "lun": 0, 
    "caching": "ReadWrite",
    "createOption": "Empty"
  }
],
```

## <a name="extensions"></a>Rozšíření

I když [rozšíření](extensions-features.md) jsou samostatné prostředku, jsou úzce vázanou tooVMs. Rozšíření mohou být přidány jako podřízené prostředkem hello virtuálního počítače nebo jako samostatnou prostředek. Příklad Hello ukazuje hello [rozšíření diagnostiky](extensions-diagnostics-template.md) přidávané toohello virtuální počítače:

```
{ 
  "name": "Microsoft.Insights.VMDiagnosticsSettings", 
  "type": "extensions", 
  "location": "[resourceGroup().location]", 
  "apiVersion": "2016-03-30", 
  "dependsOn": [ 
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
  ], 
  "properties": { 
    "publisher": "Microsoft.Azure.Diagnostics", 
    "type": "IaaSDiagnostics", 
    "typeHandlerVersion": "1.5", 
    "autoUpgradeMinorVersion": true, 
    "settings": { 
      "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
      variables('wadmetricsresourceid'), 
      concat('myVM', copyindex()),
      variables('wadcfgxend')))]", 
      "storageAccount": "[variables('storageName')]" 
    }, 
    "protectedSettings": { 
      "storageAccountName": "[variables('storageName')]", 
      "storageAccountKey": "[listkeys(variables('accountid'), 
        '2015-06-15').key1]", 
      "storageAccountEndPoint": "https://core.windows.net" 
    } 
  } 
},
```

Tento prostředek rozšíření používá proměnnou storageName hello a hello diagnostiky proměnné tooprovide hodnoty. Pokud chcete toochange hello data, která se shromažďují v tomto rozšíření, můžete přidat další výkonu čítače toohello wadperfcounters proměnné. Může také zvolit tooput hello diagnostická data do jiného úložiště účtu než kde jsou uloženy hello disky virtuálních počítačů.

Existuje mnoho rozšíření, které můžete nainstalovat na virtuálním počítači, avšak hello nejužitečnější je pravděpodobně hello [rozšíření vlastních skriptů](extensions-customscript.md). V příkladu hello spustí skript prostředí PowerShell s názvem start.ps1 na každý virtuální počítač při prvním spuštění:

```
{
  "name": "MyCustomScriptExtension",
  "type": "extensions",
  "apiVersion": "2016-03-30",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
  ],
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "[concat('https://', variables('storageName'),
          '.blob.core.windows.net/customscripts/start.ps1')]" 
      ],
      "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
    }
  }
}
```

skript start.ps1 Hello lze provádět mnoho úkoly konfigurace. Například nejsou inicializovány hello datových disků, které jsou přidány toohello virtuálních počítačů v příkladu hello; můžete použít vlastní skript tooinitialize je. Pokud máte více toodo spuštění úlohy, můžete hello start.ps1 souboru toocall jiné skripty prostředí PowerShell v úložišti Azure. Příklad Hello používá prostředí PowerShell, ale můžete použít libovolnou skriptování metodu, která je dostupná na hello operačního systému, který používáte.

Můžete zobrazit stav hello hello nainstalovaná rozšíření z nastavení rozšíření hello hello portálu:

![Načíst stav rozšíření](./media/template-description/virtual-machines-show-extensions.png)

Můžete také získat informace o rozšíření pomocí hello **Get-AzureRmVMExtension** prostředí PowerShell příkaz hello **get rozšíření virtuálního počítače** příkaz Azure CLI 2.0 nebo hello **získat informace o rozšíření**  Rozhraní REST API.

## <a name="deployments"></a>Nasazení

Při nasazení šablony Azure sleduje hello prostředky, které jste nasadili jako skupina a automaticky přiřadí skupinu název toothis nasazení. Název Hello hello nasazení je hello stejný jako název hello hello šablony.

Pokud jste zvědaví o hello stavu prostředků v hello nasazení, můžete okna skupina prostředků hello v hello portálu Azure:

![Získat informace o nasazení](./media/template-description/virtual-machines-deployment-info.png)
    
Není problém toouse hello stejné prostředky toocreate šablony nebo tooupdate existující prostředky. Při použití šablony toodeploy příkazy, máte možnost toosay hello který [režimu](../../resource-group-template-deploy.md) chcete toouse. režim Hello lze nastavit tooeither **Complete** nebo **přírůstkové**. Výchozí hodnota Hello je toodo přírůstkové aktualizace. Buďte opatrní při používání hello **Complete** režimu vzhledem k tomu, že omylem může odstranit prostředky. Pokud nastavíte režim hello příliš**Complete**, odstraní všechny prostředky ve skupině prostředků hello, které nejsou v šabloně hello Resource Manager.

## <a name="next-steps"></a>Další kroky

- Vytvoření vlastní šablony pomocí [šablon pro tvorbu Azure Resource Manageru](../../resource-group-authoring-templates.md).
- Nasazení šablony hello, který jste vytvořili pomocí [vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](ps-template.md).
- Zjistěte, jak toomanage hello virtuálních počítačů, které jste vytvořili kontrolou [vytvořit a spravovat virtuální počítače Windows hello modul Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
