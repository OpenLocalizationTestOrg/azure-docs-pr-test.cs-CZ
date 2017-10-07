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
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a><span data-ttu-id="371c2-103">Virtuální počítače v šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="371c2-103">Virtual machines in an Azure Resource Manager template</span></span>

<span data-ttu-id="371c2-104">Tento článek popisuje aspekty šablony Azure Resource Manager, které se vztahují toovirtual počítače.</span><span class="sxs-lookup"><span data-stu-id="371c2-104">This article describes aspects of an Azure Resource Manager template that apply toovirtual machines.</span></span> <span data-ttu-id="371c2-105">Tento článek popisuje kompletní šablonu pro vytvoření virtuálního počítače; k tomu potřebujete definice prostředku pro účty úložiště, síťová rozhraní, veřejné IP adresy a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="371c2-105">This article doesn’t describe a complete template for creating a virtual machine; for that you need resource definitions for storage accounts, network interfaces, public IP addresses, and virtual networks.</span></span> <span data-ttu-id="371c2-106">Další informace o tom, jak tyto prostředky je možné definovat společně najdete v tématu hello [názorný Průvodce šablonou Resource Manageru](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="371c2-106">For more information about how these resources can be defined together, see hello [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="371c2-107">Existuje mnoho [šablony v galerii hello](https://azure.microsoft.com/documentation/templates/?term=VM) , zahrnout hello prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="371c2-107">There are many [templates in hello gallery](https://azure.microsoft.com/documentation/templates/?term=VM) that include hello VM resource.</span></span> <span data-ttu-id="371c2-108">Ne všechny elementy, které můžou být součástí šablony jsou popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="371c2-108">Not all elements that can be included in a template are described here.</span></span>

<span data-ttu-id="371c2-109">Tento příklad ukazuje oddíl typické prostředků šablony pro vytvoření zadaný počet virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="371c2-109">This example shows a typical resource section of a template for creating a specified number of VMs:</span></span>

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
><span data-ttu-id="371c2-110">Tento příklad spoléhá na účet úložiště, která byla dříve vytvořena.</span><span class="sxs-lookup"><span data-stu-id="371c2-110">This example relies on a storage account that was previously created.</span></span> <span data-ttu-id="371c2-111">Můžete vytvořit účet úložiště hello nasazením z šablony hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-111">You could create hello storage account by deploying it from hello template.</span></span> <span data-ttu-id="371c2-112">Příklad Hello také závisí na rozhraní sítě a jeho závislé prostředky, které by definované v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-112">hello example also relies on a network interface and its dependent resources that would be defined in hello template.</span></span> <span data-ttu-id="371c2-113">Tyto prostředky se nezobrazí v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-113">These resources are not shown in hello example.</span></span>
>
>

## <a name="api-version"></a><span data-ttu-id="371c2-114">Verze rozhraní API</span><span class="sxs-lookup"><span data-stu-id="371c2-114">API Version</span></span>

<span data-ttu-id="371c2-115">Když nasadíte prostředky pomocí šablony, máte toospecify verzi rozhraní API toouse hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-115">When you deploy resources using a template, you have toospecify a version of hello API toouse.</span></span> <span data-ttu-id="371c2-116">Příklad Hello ukazuje hello prostředek virtuálního počítače pomocí tohoto elementu apiVersion:</span><span class="sxs-lookup"><span data-stu-id="371c2-116">hello example shows hello virtual machine resource using this apiVersion element:</span></span>

```
"apiVersion": "2016-04-30-preview",
```

<span data-ttu-id="371c2-117">Hello verzi hello rozhraní API, které zadáte v šabloně ovlivňuje vlastnosti, které definujete v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-117">hello version of hello API you specify in your template affects which properties you can define in hello template.</span></span> <span data-ttu-id="371c2-118">Obecně byste měli vybrat hello nejnovější verzi rozhraní API, při vytváření šablony.</span><span class="sxs-lookup"><span data-stu-id="371c2-118">In general, you should select hello most recent API version when creating templates.</span></span> <span data-ttu-id="371c2-119">Pro existující šablony můžete rozhodnout, zda chcete toocontinue pomocí dřívější verze rozhraní API nebo aktualizaci šablony pro hello nejnovější verze tootake výhod nových funkcí.</span><span class="sxs-lookup"><span data-stu-id="371c2-119">For existing templates, you can decide whether you want toocontinue using an earlier API version, or update your template for hello latest version tootake advantage of new features.</span></span>

<span data-ttu-id="371c2-120">Použijte tyto příležitosti pro získání hello nejnovější verze rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="371c2-120">Use these opportunities for getting hello latest API versions:</span></span>

- <span data-ttu-id="371c2-121">Rozhraní API REST - [zobrazit seznam všech poskytovatelů prostředků](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span><span class="sxs-lookup"><span data-stu-id="371c2-121">REST API - [List all resource providers](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span></span>
- <span data-ttu-id="371c2-122">PowerShell – [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span><span class="sxs-lookup"><span data-stu-id="371c2-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span></span>
- <span data-ttu-id="371c2-123">Rozhraní příkazového řádku Azure 2.0 - [az zprostředkovatele zobrazit](https://docs.microsoft.com/cli/azure/provider#show)</span><span class="sxs-lookup"><span data-stu-id="371c2-123">Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)</span></span>

## <a name="parameters-and-variables"></a><span data-ttu-id="371c2-124">Parametry a proměnné</span><span class="sxs-lookup"><span data-stu-id="371c2-124">Parameters and variables</span></span>

<span data-ttu-id="371c2-125">[Parametry](../../resource-group-authoring-templates.md) usnadní vám toospecify hodnoty pro šablonu hello při každém spuštění.</span><span class="sxs-lookup"><span data-stu-id="371c2-125">[Parameters](../../resource-group-authoring-templates.md) make it easy for you toospecify values for hello template when you run it.</span></span> <span data-ttu-id="371c2-126">V příkladu hello se používá v této části Parametry:</span><span class="sxs-lookup"><span data-stu-id="371c2-126">This parameters section is used in hello example:</span></span>

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

<span data-ttu-id="371c2-127">Když nasadíte hello příklad šablony, zadejte hodnoty pro hello jméno a heslo účtu správce hello na každý virtuální počítač a hello počet toocreate virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="371c2-127">When you deploy hello example template, you enter values for hello name and password of hello administrator account on each VM and hello number of VMs toocreate.</span></span> <span data-ttu-id="371c2-128">Máte možnost hello zadání hodnot parametrů do samostatného souboru, který je spravován pomocí šablony hello nebo zadáním hodnot po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="371c2-128">You have hello option of specifying parameter values in a separate file that's managed with hello template, or providing values when prompted.</span></span>

<span data-ttu-id="371c2-129">[Proměnné](../../resource-group-authoring-templates.md) usnadní vám tooset hodnot v šabloně hello jsou opakovaně použít v celé jeho nebo můžou časem změnit.</span><span class="sxs-lookup"><span data-stu-id="371c2-129">[Variables](../../resource-group-authoring-templates.md) make it easy for you tooset up values in hello template that are used repeatedly throughout it or that can change over time.</span></span> <span data-ttu-id="371c2-130">Tato část proměnné se používá v příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="371c2-130">This variables section is used in hello example:</span></span>

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

<span data-ttu-id="371c2-131">Při nasazování šablony příklad hello hodnoty proměnné se používají pro hello název a identifikátor hello vytvořili účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="371c2-131">When you deploy hello example template, variable values are used for hello name and identifier of hello previously created storage account.</span></span> <span data-ttu-id="371c2-132">Proměnné jsou také použít tooprovide hello nastavení pro rozšíření diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-132">Variables are also used tooprovide hello settings for hello diagnostic extension.</span></span> <span data-ttu-id="371c2-133">Použití hello [osvědčené postupy pro vytváření šablon Azure Resource Manager](../../resource-manager-template-best-practices.md) toohelp můžete určit, jak mají toostructure hello parametrů a proměnných ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="371c2-133">Use hello [best practices for creating Azure Resource Manager templates](../../resource-manager-template-best-practices.md) toohelp you decide how you want toostructure hello parameters and variables in your template.</span></span>

## <a name="resource-loops"></a><span data-ttu-id="371c2-134">Smyčky prostředků</span><span class="sxs-lookup"><span data-stu-id="371c2-134">Resource loops</span></span>

<span data-ttu-id="371c2-135">Pokud potřebujete více než jeden virtuální počítač pro vaši aplikaci, můžete použít element kopírování v šabloně.</span><span class="sxs-lookup"><span data-stu-id="371c2-135">When you need more than one virtual machine for your application, you can use a copy element in a template.</span></span> <span data-ttu-id="371c2-136">Tento volitelný element projde vytváření hello počet virtuálních počítačů, které jste zadali jako parametr:</span><span class="sxs-lookup"><span data-stu-id="371c2-136">This optional element loops through creating hello number of VMs that you specified as a parameter:</span></span>

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

<span data-ttu-id="371c2-137">Všimněte si v příkladu hello, který hello index smyčky se také používá při zadání některé z hello hodnoty pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-137">Also, notice in hello example that hello loop index is used when specifying some of hello values for hello resource.</span></span> <span data-ttu-id="371c2-138">Například pokud jste zadali počet instancí názvů tři, hello hello disků operačního systému jsou myOSDisk1, myOSDisk2 a myOSDisk3:</span><span class="sxs-lookup"><span data-stu-id="371c2-138">For example, if you entered an instance count of three, hello names of hello operating system disks are myOSDisk1, myOSDisk2, and myOSDisk3:</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
><span data-ttu-id="371c2-139">Tento příklad používá spravované disky pro virtuální počítače hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-139">This example uses managed disks for hello virtual machines.</span></span>
>
>

<span data-ttu-id="371c2-140">Mějte na paměti, že vytváření smyčku pro jeden prostředek v šabloně hello může vyžadovat jste toouse hello smyčky při vytváření nebo přístup k dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="371c2-140">Keep in mind that creating a loop for one resource in hello template may require you toouse hello loop when creating or accessing other resources.</span></span> <span data-ttu-id="371c2-141">Víc virtuálních počítačů nelze použít například hello stejné síťové rozhraní, takže pokud vaše šablona projde vytváření tři virtuální počítače musí také cykly procesem vytvoření tři síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="371c2-141">For example, multiple VMs can’t use hello same network interface, so if your template loops through creating three VMs it must also loop through creating three network interfaces.</span></span> <span data-ttu-id="371c2-142">Při přiřazování tooa rozhraní sítě virtuálních počítačů, index smyčky hello je použité tooidentify ho:</span><span class="sxs-lookup"><span data-stu-id="371c2-142">When assigning a network interface tooa VM, hello loop index is used tooidentify it:</span></span>

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a><span data-ttu-id="371c2-143">Závislosti</span><span class="sxs-lookup"><span data-stu-id="371c2-143">Dependencies</span></span>

<span data-ttu-id="371c2-144">Většina prostředky závisí na jiné prostředky toowork správně.</span><span class="sxs-lookup"><span data-stu-id="371c2-144">Most resources depend on other resources toowork correctly.</span></span> <span data-ttu-id="371c2-145">Virtuální počítače musí být přidružený virtuální sítě a toodo, že tato služba vyžaduje síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="371c2-145">Virtual machines must be associated with a virtual network and toodo that it needs a network interface.</span></span> <span data-ttu-id="371c2-146">Hello [dependsOn](../../resource-group-define-dependencies.md) element je použité toomake se rozhraní sítě, hello je připraven toobe využít, než se vytvoří virtuální počítače hello:</span><span class="sxs-lookup"><span data-stu-id="371c2-146">hello [dependsOn](../../resource-group-define-dependencies.md) element is used toomake sure that hello network interface is ready toobe used before hello VMs are created:</span></span>

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

<span data-ttu-id="371c2-147">Správce prostředků nasadí současně všechny prostředky, které nejsou závislé na jiný prostředek nasazuje.</span><span class="sxs-lookup"><span data-stu-id="371c2-147">Resource Manager deploys in parallel any resources that are not dependent on another resource being deployed.</span></span> <span data-ttu-id="371c2-148">Buďte opatrní při nastavení závislostí, protože můžou nechtěně způsobit snížení nasazení tak, že zadáte nepotřebné závislosti.</span><span class="sxs-lookup"><span data-stu-id="371c2-148">Be careful when setting dependencies because you can inadvertently slow your deployment by specifying unnecessary dependencies.</span></span> <span data-ttu-id="371c2-149">Závislosti můžete zřetězené prostřednictvím více prostředků.</span><span class="sxs-lookup"><span data-stu-id="371c2-149">Dependencies can chain through multiple resources.</span></span> <span data-ttu-id="371c2-150">Například hello síťové rozhraní závisí na hello veřejnou IP adresu a virtuální síťové prostředky.</span><span class="sxs-lookup"><span data-stu-id="371c2-150">For example, hello network interface depends on hello public IP address and virtual network resources.</span></span>

<span data-ttu-id="371c2-151">Jak poznáte, pokud je třeba provést závislost?</span><span class="sxs-lookup"><span data-stu-id="371c2-151">How do you know if a dependency is required?</span></span> <span data-ttu-id="371c2-152">Podívejte se na hello hodnoty, které můžete zadat v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="371c2-152">Look at hello values you set in hello template.</span></span> <span data-ttu-id="371c2-153">Pokud element v definici prostředků virtuálního počítače hello odkazuje tooanother prostředků, která je nasazena v hello stejné šablony, musíte závislost.</span><span class="sxs-lookup"><span data-stu-id="371c2-153">If an element in hello virtual machine resource definition points tooanother resource that is deployed in hello same template, you need a dependency.</span></span> <span data-ttu-id="371c2-154">Virtuální počítač příklad určují profil sítě:</span><span class="sxs-lookup"><span data-stu-id="371c2-154">For example, your example virtual machine defines a network profile:</span></span>

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

<span data-ttu-id="371c2-155">tooset tato vlastnost musí existovat hello síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="371c2-155">tooset this property, hello network interface must exist.</span></span> <span data-ttu-id="371c2-156">Proto musíte závislost.</span><span class="sxs-lookup"><span data-stu-id="371c2-156">Therefore, you need a dependency.</span></span> <span data-ttu-id="371c2-157">Musíte taky tooset závislost, když jeden prostředek (podřízená) je definována v rámci jiný prostředek (nadřazené).</span><span class="sxs-lookup"><span data-stu-id="371c2-157">You also need tooset a dependency when one resource (a child) is defined within another resource (a parent).</span></span> <span data-ttu-id="371c2-158">Například nastavení pro diagnostiku hello a rozšíření vlastních skriptů jsou obě definované jako podřízené prostředky hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="371c2-158">For example, hello diagnostic settings and custom script extensions are both defined as child resources of hello virtual machine.</span></span> <span data-ttu-id="371c2-159">Nelze vytvořit dokud hello virtuální počítač existuje.</span><span class="sxs-lookup"><span data-stu-id="371c2-159">They cannot be created until hello virtual machine exists.</span></span> <span data-ttu-id="371c2-160">Proto i prostředky jsou označeny jako závislé na hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="371c2-160">Therefore, both resources are marked as dependent on hello virtual machine.</span></span>

## <a name="profiles"></a><span data-ttu-id="371c2-161">Profily</span><span class="sxs-lookup"><span data-stu-id="371c2-161">Profiles</span></span>

<span data-ttu-id="371c2-162">Několik elementy profil se používá při definování prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="371c2-162">Several profile elements are used when defining a virtual machine resource.</span></span> <span data-ttu-id="371c2-163">Některé jsou vyžadovány a některé jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="371c2-163">Some are required and some are optional.</span></span> <span data-ttu-id="371c2-164">Například hello položka hardwareProfile, osProfile, storageProfile a networkProfile elementy jsou požadovány, ale hello diagnosticsProfile je volitelný.</span><span class="sxs-lookup"><span data-stu-id="371c2-164">For example, hello hardwareProfile, osProfile, storageProfile, and networkProfile elements are required, but hello diagnosticsProfile is optional.</span></span> <span data-ttu-id="371c2-165">Tyto profily definovat nastavení, jako:</span><span class="sxs-lookup"><span data-stu-id="371c2-165">These profiles define settings such as:</span></span>
   
- [<span data-ttu-id="371c2-166">velikost</span><span class="sxs-lookup"><span data-stu-id="371c2-166">size</span></span>](sizes.md)
- <span data-ttu-id="371c2-167">[název](/architecture/best-practices/naming-conventions) a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="371c2-167">[name](/architecture/best-practices/naming-conventions) and credentials</span></span>
- <span data-ttu-id="371c2-168">disk a [nastavení operačního systému](cli-ps-findimage.md)</span><span class="sxs-lookup"><span data-stu-id="371c2-168">disk and [operating system settings](cli-ps-findimage.md)</span></span>
- [<span data-ttu-id="371c2-169">síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="371c2-169">network interface</span></span>](../../virtual-network/virtual-networks-multiple-nics.md) 
- <span data-ttu-id="371c2-170">Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="371c2-170">boot diagnostics</span></span>

## <a name="disks-and-images"></a><span data-ttu-id="371c2-171">Disky a obrázků</span><span class="sxs-lookup"><span data-stu-id="371c2-171">Disks and images</span></span>
   
<span data-ttu-id="371c2-172">V Azure, můžete soubory vhd představují [disky nebo bitové kopie](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="371c2-172">In Azure, vhd files can represent [disks or images](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="371c2-173">Když hello operační systém v souboru virtuálního pevného disku je specializovaná toobe konkrétní virtuální počítač, je tooas odkazuje na disk.</span><span class="sxs-lookup"><span data-stu-id="371c2-173">When hello operating system in a vhd file is specialized toobe a specific VM, it is referred tooas a disk.</span></span> <span data-ttu-id="371c2-174">Když je zobecněn hello operačního systému v souboru vhd toobe používat toocreate hodně virtuálních počítačů, je odkazované tooas bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="371c2-174">When hello operating system in a vhd file is generalized toobe used toocreate many VMs, it is referred tooas an image.</span></span>   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a><span data-ttu-id="371c2-175">Vytvoření nové virtuální počítače a nové disky z image platformy</span><span class="sxs-lookup"><span data-stu-id="371c2-175">Create new virtual machines and new disks from a platform image</span></span>

<span data-ttu-id="371c2-176">Když vytvoříte virtuální počítač, musíte rozhodnout, jaké toouse operačního systému.</span><span class="sxs-lookup"><span data-stu-id="371c2-176">When you create a VM, you must decide what operating system toouse.</span></span> <span data-ttu-id="371c2-177">Element imageReference element Hello je použité toodefine hello operační systém nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="371c2-177">hello imageReference element is used toodefine hello operating system of a new VM.</span></span> <span data-ttu-id="371c2-178">Hello příklad ukazuje definici pro operační systém Windows Server:</span><span class="sxs-lookup"><span data-stu-id="371c2-178">hello example shows a definition for a Windows Server operating system:</span></span>

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

<span data-ttu-id="371c2-179">Pokud chcete toocreate operačního systému Linux, můžete použít tuto definici:</span><span class="sxs-lookup"><span data-stu-id="371c2-179">If you want toocreate a Linux operating system, you might use this definition:</span></span>

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

<span data-ttu-id="371c2-180">Nastavení konfigurace pro disk operačního systému hello jsou přiřazeny hello osDisk element.</span><span class="sxs-lookup"><span data-stu-id="371c2-180">Configuration settings for hello operating system disk are assigned with hello osDisk element.</span></span> <span data-ttu-id="371c2-181">Hello příklad definuje nový disk spravované hello ukládání do mezipaměti v režimu sadu příliš**ReadWrite** a tento disk hello se vytváří z [image platformy](cli-ps-findimage.md):</span><span class="sxs-lookup"><span data-stu-id="371c2-181">hello example defines a new managed disk with hello caching mode set too**ReadWrite** and that hello disk is being created from a [platform image](cli-ps-findimage.md):</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a><span data-ttu-id="371c2-182">Vytvoření nové virtuální počítače z existujícího spravovaného disků</span><span class="sxs-lookup"><span data-stu-id="371c2-182">Create new virtual machines from existing managed disks</span></span>

<span data-ttu-id="371c2-183">Pokud chcete toocreate virtuální počítače z existujícího disků, odeberte element imageReference hello a hello osProfile elementy a definovat toto nastavení disku:</span><span class="sxs-lookup"><span data-stu-id="371c2-183">If you want toocreate virtual machines from existing disks, remove hello imageReference and hello osProfile elements and define these disk settings:</span></span>

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

### <a name="create-new-virtual-machines-from-a-managed-image"></a><span data-ttu-id="371c2-184">Vytvoření nových virtuálních počítačů ze spravovaných bitové kopie</span><span class="sxs-lookup"><span data-stu-id="371c2-184">Create new virtual machines from a managed image</span></span>

<span data-ttu-id="371c2-185">Pokud chcete virtuální počítač z bitové kopie spravované toocreate, změňte hello elementu imageReference element a definovat toto nastavení disku:</span><span class="sxs-lookup"><span data-stu-id="371c2-185">If you want toocreate a virtual machine from a managed image, change hello imageReference element and define these disk settings:</span></span>

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

### <a name="attach-data-disks"></a><span data-ttu-id="371c2-186">Připojte datových disků</span><span class="sxs-lookup"><span data-stu-id="371c2-186">Attach data disks</span></span>

<span data-ttu-id="371c2-187">Můžete přidat data toohello disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="371c2-187">You can optionally add data disks toohello VMs.</span></span> <span data-ttu-id="371c2-188">Hello [počet disků](sizes.md) závisí na velikosti hello disk operačního systému, který používáte.</span><span class="sxs-lookup"><span data-stu-id="371c2-188">hello [number of disks](sizes.md) depends on hello size of operating system disk that you use.</span></span> <span data-ttu-id="371c2-189">S hello nastavit velikost virtuálních počítačů hello tooStandard_DS1_v2 hello maximální počet datových disků, které nebylo možné přidat toohello je je dva.</span><span class="sxs-lookup"><span data-stu-id="371c2-189">With hello size of hello VMs set tooStandard_DS1_v2, hello maximum number of data disks that could be added toohello them is two.</span></span> <span data-ttu-id="371c2-190">V příkladu hello jeden spravovaný datový disk je přidáván tooeach virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="371c2-190">In hello example, one managed data disk is being added tooeach VM:</span></span>

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

## <a name="extensions"></a><span data-ttu-id="371c2-191">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="371c2-191">Extensions</span></span>

<span data-ttu-id="371c2-192">I když [rozšíření](extensions-features.md) jsou samostatné prostředku, jsou úzce vázanou tooVMs.</span><span class="sxs-lookup"><span data-stu-id="371c2-192">Although [extensions](extensions-features.md) are a separate resource, they are closely tied tooVMs.</span></span> <span data-ttu-id="371c2-193">Rozšíření mohou být přidány jako podřízené prostředkem hello virtuálního počítače nebo jako samostatnou prostředek.</span><span class="sxs-lookup"><span data-stu-id="371c2-193">Extensions can be added as a child resource of hello VM or as a separate resource.</span></span> <span data-ttu-id="371c2-194">Příklad Hello ukazuje hello [rozšíření diagnostiky](extensions-diagnostics-template.md) přidávané toohello virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="371c2-194">hello example shows hello [Diagnostics Extension](extensions-diagnostics-template.md) being added toohello VMs:</span></span>

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

<span data-ttu-id="371c2-195">Tento prostředek rozšíření používá proměnnou storageName hello a hello diagnostiky proměnné tooprovide hodnoty.</span><span class="sxs-lookup"><span data-stu-id="371c2-195">This extension resource uses hello storageName variable and hello diagnostic variables tooprovide values.</span></span> <span data-ttu-id="371c2-196">Pokud chcete toochange hello data, která se shromažďují v tomto rozšíření, můžete přidat další výkonu čítače toohello wadperfcounters proměnné.</span><span class="sxs-lookup"><span data-stu-id="371c2-196">If you want toochange hello data that is collected by this extension, you can add more performance counters toohello wadperfcounters variable.</span></span> <span data-ttu-id="371c2-197">Může také zvolit tooput hello diagnostická data do jiného úložiště účtu než kde jsou uloženy hello disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="371c2-197">You could also choose tooput hello diagnostics data into a different storage account than where hello VM disks are stored.</span></span>

<span data-ttu-id="371c2-198">Existuje mnoho rozšíření, které můžete nainstalovat na virtuálním počítači, avšak hello nejužitečnější je pravděpodobně hello [rozšíření vlastních skriptů](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="371c2-198">There are many extensions that you can install on a VM, but hello most useful is probably hello [Custom Script Extension](extensions-customscript.md).</span></span> <span data-ttu-id="371c2-199">V příkladu hello spustí skript prostředí PowerShell s názvem start.ps1 na každý virtuální počítač při prvním spuštění:</span><span class="sxs-lookup"><span data-stu-id="371c2-199">In hello example, a PowerShell script named start.ps1 runs on each VM when it first starts:</span></span>

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

<span data-ttu-id="371c2-200">skript start.ps1 Hello lze provádět mnoho úkoly konfigurace.</span><span class="sxs-lookup"><span data-stu-id="371c2-200">hello start.ps1 script can accomplish many configuration tasks.</span></span> <span data-ttu-id="371c2-201">Například nejsou inicializovány hello datových disků, které jsou přidány toohello virtuálních počítačů v příkladu hello; můžete použít vlastní skript tooinitialize je.</span><span class="sxs-lookup"><span data-stu-id="371c2-201">For example, hello data disks that are added toohello VMs in hello example are not initialized; you can use a custom script tooinitialize them.</span></span> <span data-ttu-id="371c2-202">Pokud máte více toodo spuštění úlohy, můžete hello start.ps1 souboru toocall jiné skripty prostředí PowerShell v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="371c2-202">If you have multiple startup tasks toodo, you can use hello start.ps1 file toocall other PowerShell scripts in Azure storage.</span></span> <span data-ttu-id="371c2-203">Příklad Hello používá prostředí PowerShell, ale můžete použít libovolnou skriptování metodu, která je dostupná na hello operačního systému, který používáte.</span><span class="sxs-lookup"><span data-stu-id="371c2-203">hello example uses PowerShell, but you can use any scripting method that is available on hello operating system that you are using.</span></span>

<span data-ttu-id="371c2-204">Můžete zobrazit stav hello hello nainstalovaná rozšíření z nastavení rozšíření hello hello portálu:</span><span class="sxs-lookup"><span data-stu-id="371c2-204">You can see hello status of hello installed extensions from hello Extensions settings in hello portal:</span></span>

![Načíst stav rozšíření](./media/template-description/virtual-machines-show-extensions.png)

<span data-ttu-id="371c2-206">Můžete také získat informace o rozšíření pomocí hello **Get-AzureRmVMExtension** prostředí PowerShell příkaz hello **get rozšíření virtuálního počítače** příkaz Azure CLI 2.0 nebo hello **získat informace o rozšíření**  Rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="371c2-206">You can also get extension information by using hello **Get-AzureRmVMExtension** PowerShell command, hello **vm extension get** Azure CLI 2.0 command, or hello **Get extension information** REST API.</span></span>

## <a name="deployments"></a><span data-ttu-id="371c2-207">Nasazení</span><span class="sxs-lookup"><span data-stu-id="371c2-207">Deployments</span></span>

<span data-ttu-id="371c2-208">Při nasazení šablony Azure sleduje hello prostředky, které jste nasadili jako skupina a automaticky přiřadí skupinu název toothis nasazení.</span><span class="sxs-lookup"><span data-stu-id="371c2-208">When you deploy a template, Azure tracks hello resources that you deployed as a group and automatically assigns a name toothis deployed group.</span></span> <span data-ttu-id="371c2-209">Název Hello hello nasazení je hello stejný jako název hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="371c2-209">hello name of hello deployment is hello same as hello name of hello template.</span></span>

<span data-ttu-id="371c2-210">Pokud jste zvědaví o hello stavu prostředků v hello nasazení, můžete okna skupina prostředků hello v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="371c2-210">If you are curious about hello status of resources in hello deployment, you can use hello Resource Group blade in hello Azure portal:</span></span>

![Získat informace o nasazení](./media/template-description/virtual-machines-deployment-info.png)
    
<span data-ttu-id="371c2-212">Není problém toouse hello stejné prostředky toocreate šablony nebo tooupdate existující prostředky.</span><span class="sxs-lookup"><span data-stu-id="371c2-212">It’s not a problem toouse hello same template toocreate resources or tooupdate existing resources.</span></span> <span data-ttu-id="371c2-213">Při použití šablony toodeploy příkazy, máte možnost toosay hello který [režimu](../../resource-group-template-deploy.md) chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="371c2-213">When you use commands toodeploy templates, you have hello opportunity toosay which [mode](../../resource-group-template-deploy.md) you want toouse.</span></span> <span data-ttu-id="371c2-214">režim Hello lze nastavit tooeither **Complete** nebo **přírůstkové**.</span><span class="sxs-lookup"><span data-stu-id="371c2-214">hello mode can be set tooeither **Complete** or **Incremental**.</span></span> <span data-ttu-id="371c2-215">Výchozí hodnota Hello je toodo přírůstkové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="371c2-215">hello default is toodo incremental updates.</span></span> <span data-ttu-id="371c2-216">Buďte opatrní při používání hello **Complete** režimu vzhledem k tomu, že omylem může odstranit prostředky.</span><span class="sxs-lookup"><span data-stu-id="371c2-216">Be careful when using hello **Complete** mode because you may accidentally delete resources.</span></span> <span data-ttu-id="371c2-217">Pokud nastavíte režim hello příliš**Complete**, odstraní všechny prostředky ve skupině prostředků hello, které nejsou v šabloně hello Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="371c2-217">When you set hello mode too**Complete**, Resource Manager deletes any resources in hello resource group that are not in hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="371c2-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="371c2-218">Next Steps</span></span>

- <span data-ttu-id="371c2-219">Vytvoření vlastní šablony pomocí [šablon pro tvorbu Azure Resource Manageru](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="371c2-219">Create your own template using [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>
- <span data-ttu-id="371c2-220">Nasazení šablony hello, který jste vytvořili pomocí [vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](ps-template.md).</span><span class="sxs-lookup"><span data-stu-id="371c2-220">Deploy hello template that you created using [Create a Windows virtual machine with a Resource Manager template](ps-template.md).</span></span>
- <span data-ttu-id="371c2-221">Zjistěte, jak toomanage hello virtuálních počítačů, které jste vytvořili kontrolou [vytvořit a spravovat virtuální počítače Windows hello modul Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="371c2-221">Learn how toomanage hello VMs that you created by reviewing [Create and manage Windows VMs with hello Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
