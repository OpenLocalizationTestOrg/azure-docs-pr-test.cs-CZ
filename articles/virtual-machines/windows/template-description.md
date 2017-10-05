---
title: "Virtuální počítače v šablonu Azure Resource Manager | Microsoft Azure"
description: "Další informace o tom, jak je definován prostředek virtuálního počítače v šablonu Azure Resource Manager."
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
ms.openlocfilehash: d9b9121bc5e38396ba4def6c17f9b373c2b48056
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a><span data-ttu-id="d7b42-103">Virtuální počítače v šablonu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d7b42-103">Virtual machines in an Azure Resource Manager template</span></span>

<span data-ttu-id="d7b42-104">Tento článek popisuje aspekty šablony Azure Resource Manager, které se vztahují na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d7b42-104">This article describes aspects of an Azure Resource Manager template that apply to virtual machines.</span></span> <span data-ttu-id="d7b42-105">Tento článek popisuje kompletní šablonu pro vytvoření virtuálního počítače; k tomu potřebujete definice prostředku pro účty úložiště, síťová rozhraní, veřejné IP adresy a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d7b42-105">This article doesn’t describe a complete template for creating a virtual machine; for that you need resource definitions for storage accounts, network interfaces, public IP addresses, and virtual networks.</span></span> <span data-ttu-id="d7b42-106">Další informace o tom, jak tyto prostředky je možné definovat společně najdete v tématu [názorný Průvodce šablonou Resource Manageru](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="d7b42-106">For more information about how these resources can be defined together, see the [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="d7b42-107">Existuje mnoho [šablony v galerii](https://azure.microsoft.com/documentation/templates/?term=VM) , zahrnout prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7b42-107">There are many [templates in the gallery](https://azure.microsoft.com/documentation/templates/?term=VM) that include the VM resource.</span></span> <span data-ttu-id="d7b42-108">Ne všechny elementy, které můžou být součástí šablony jsou popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="d7b42-108">Not all elements that can be included in a template are described here.</span></span>

<span data-ttu-id="d7b42-109">Tento příklad ukazuje oddíl typické prostředků šablony pro vytvoření zadaný počet virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="d7b42-109">This example shows a typical resource section of a template for creating a specified number of VMs:</span></span>

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
><span data-ttu-id="d7b42-110">Tento příklad spoléhá na účet úložiště, která byla dříve vytvořena.</span><span class="sxs-lookup"><span data-stu-id="d7b42-110">This example relies on a storage account that was previously created.</span></span> <span data-ttu-id="d7b42-111">Můžete vytvořit účet úložiště po nasazení ze šablony.</span><span class="sxs-lookup"><span data-stu-id="d7b42-111">You could create the storage account by deploying it from the template.</span></span> <span data-ttu-id="d7b42-112">V příkladu se také závisí na rozhraní sítě a jeho závislé prostředky, které by definované v šabloně.</span><span class="sxs-lookup"><span data-stu-id="d7b42-112">The example also relies on a network interface and its dependent resources that would be defined in the template.</span></span> <span data-ttu-id="d7b42-113">Tyto prostředky se nezobrazí v příkladu.</span><span class="sxs-lookup"><span data-stu-id="d7b42-113">These resources are not shown in the example.</span></span>
>
>

## <a name="api-version"></a><span data-ttu-id="d7b42-114">Verze rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d7b42-114">API Version</span></span>

<span data-ttu-id="d7b42-115">Když nasadíte prostředky pomocí šablony, budete muset určit verzi rozhraní API používat.</span><span class="sxs-lookup"><span data-stu-id="d7b42-115">When you deploy resources using a template, you have to specify a version of the API to use.</span></span> <span data-ttu-id="d7b42-116">V příkladu prostředek virtuálního počítače pomocí tohoto elementu apiVersion:</span><span class="sxs-lookup"><span data-stu-id="d7b42-116">The example shows the virtual machine resource using this apiVersion element:</span></span>

```
"apiVersion": "2016-04-30-preview",
```

<span data-ttu-id="d7b42-117">Verze rozhraní API, které zadáte v šabloně ovlivňuje vlastnosti, které definujete v šabloně.</span><span class="sxs-lookup"><span data-stu-id="d7b42-117">The version of the API you specify in your template affects which properties you can define in the template.</span></span> <span data-ttu-id="d7b42-118">Obecně byste měli vybrat nejnovější verzi rozhraní API, při vytváření šablony.</span><span class="sxs-lookup"><span data-stu-id="d7b42-118">In general, you should select the most recent API version when creating templates.</span></span> <span data-ttu-id="d7b42-119">Pro existující šablony můžete rozhodnout, jestli chcete pokračovat, pomocí dřívější verze rozhraní API nebo aktualizaci šablony na nejnovější verzi, abyste mohli využívat nové funkce.</span><span class="sxs-lookup"><span data-stu-id="d7b42-119">For existing templates, you can decide whether you want to continue using an earlier API version, or update your template for the latest version to take advantage of new features.</span></span>

<span data-ttu-id="d7b42-120">Použijte tyto příležitosti pro získání nejnovější verze rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="d7b42-120">Use these opportunities for getting the latest API versions:</span></span>

- <span data-ttu-id="d7b42-121">Rozhraní API REST - [zobrazit seznam všech poskytovatelů prostředků](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span><span class="sxs-lookup"><span data-stu-id="d7b42-121">REST API - [List all resource providers](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span></span>
- <span data-ttu-id="d7b42-122">PowerShell – [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span><span class="sxs-lookup"><span data-stu-id="d7b42-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span></span>
- <span data-ttu-id="d7b42-123">Rozhraní příkazového řádku Azure 2.0 - [az zprostředkovatele zobrazit](https://docs.microsoft.com/cli/azure/provider#show)</span><span class="sxs-lookup"><span data-stu-id="d7b42-123">Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)</span></span>

## <a name="parameters-and-variables"></a><span data-ttu-id="d7b42-124">Parametry a proměnné</span><span class="sxs-lookup"><span data-stu-id="d7b42-124">Parameters and variables</span></span>

<span data-ttu-id="d7b42-125">[Parametry](../../resource-group-authoring-templates.md) usnadní zadejte hodnoty pro šablonu, když jej spustíte.</span><span class="sxs-lookup"><span data-stu-id="d7b42-125">[Parameters](../../resource-group-authoring-templates.md) make it easy for you to specify values for the template when you run it.</span></span> <span data-ttu-id="d7b42-126">V příkladu se používá v této části Parametry:</span><span class="sxs-lookup"><span data-stu-id="d7b42-126">This parameters section is used in the example:</span></span>

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

<span data-ttu-id="d7b42-127">Když nasadíte příklad šablony, zadejte hodnoty pro název a heslo účtu správce na každý virtuální počítač a počet virtuálních počítačů k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="d7b42-127">When you deploy the example template, you enter values for the name and password of the administrator account on each VM and the number of VMs to create.</span></span> <span data-ttu-id="d7b42-128">Máte možnost zadání hodnot parametrů do samostatného souboru, který je spravován pomocí šablony, nebo zadáním hodnot po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="d7b42-128">You have the option of specifying parameter values in a separate file that's managed with the template, or providing values when prompted.</span></span>

<span data-ttu-id="d7b42-129">[Proměnné](../../resource-group-authoring-templates.md) snadno nastavit hodnoty v šabloně jsou opakovaně použít v celé jeho nebo můžou časem změnit.</span><span class="sxs-lookup"><span data-stu-id="d7b42-129">[Variables](../../resource-group-authoring-templates.md) make it easy for you to set up values in the template that are used repeatedly throughout it or that can change over time.</span></span> <span data-ttu-id="d7b42-130">V příkladu se používá v této části proměnné:</span><span class="sxs-lookup"><span data-stu-id="d7b42-130">This variables section is used in the example:</span></span>

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

<span data-ttu-id="d7b42-131">Při nasazování šablony Příklad hodnoty proměnné se používají pro název a identifikátor dříve vytvořený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d7b42-131">When you deploy the example template, variable values are used for the name and identifier of the previously created storage account.</span></span> <span data-ttu-id="d7b42-132">Proměnné se taky používají k zadání nastavení pro rozšíření diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d7b42-132">Variables are also used to provide the settings for the diagnostic extension.</span></span> <span data-ttu-id="d7b42-133">Použití [osvědčené postupy pro vytváření šablon Azure Resource Manager](../../resource-manager-template-best-practices.md) vám pomohou rozhodnout, jak chcete struktury parametrů a proměnných ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="d7b42-133">Use the [best practices for creating Azure Resource Manager templates](../../resource-manager-template-best-practices.md) to help you decide how you want to structure the parameters and variables in your template.</span></span>

## <a name="resource-loops"></a><span data-ttu-id="d7b42-134">Smyčky prostředků</span><span class="sxs-lookup"><span data-stu-id="d7b42-134">Resource loops</span></span>

<span data-ttu-id="d7b42-135">Pokud potřebujete více než jeden virtuální počítač pro vaši aplikaci, můžete použít element kopírování v šabloně.</span><span class="sxs-lookup"><span data-stu-id="d7b42-135">When you need more than one virtual machine for your application, you can use a copy element in a template.</span></span> <span data-ttu-id="d7b42-136">Tento volitelný element projde vytváření počet virtuálních počítačů, které jste zadali jako parametr:</span><span class="sxs-lookup"><span data-stu-id="d7b42-136">This optional element loops through creating the number of VMs that you specified as a parameter:</span></span>

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

<span data-ttu-id="d7b42-137">Všimněte si také, v příkladu, index smyčky se používá při zadávání některé hodnoty pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="d7b42-137">Also, notice in the example that the loop index is used when specifying some of the values for the resource.</span></span> <span data-ttu-id="d7b42-138">Například pokud jste zadali počet instancí tří, názvy disků operačního systému jsou myOSDisk1, myOSDisk2 a myOSDisk3:</span><span class="sxs-lookup"><span data-stu-id="d7b42-138">For example, if you entered an instance count of three, the names of the operating system disks are myOSDisk1, myOSDisk2, and myOSDisk3:</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
><span data-ttu-id="d7b42-139">Tento příklad používá spravované disky pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d7b42-139">This example uses managed disks for the virtual machines.</span></span>
>
>

<span data-ttu-id="d7b42-140">Mějte na paměti, že vytváření smyčku pro jeden prostředek v šabloně, může vyžadovat použití smyčky při vytváření nebo přístup k dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="d7b42-140">Keep in mind that creating a loop for one resource in the template may require you to use the loop when creating or accessing other resources.</span></span> <span data-ttu-id="d7b42-141">Například víc virtuálních počítačů nemůžou použít stejný síťového rozhraní, pokud vaše šablona projde vytváření tři virtuální počítače musí také cykly procesem vytvoření tři síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d7b42-141">For example, multiple VMs can’t use the same network interface, so if your template loops through creating three VMs it must also loop through creating three network interfaces.</span></span> <span data-ttu-id="d7b42-142">Při přiřazování síťové rozhraní virtuálního počítače, index smyčky se používá k identifikaci:</span><span class="sxs-lookup"><span data-stu-id="d7b42-142">When assigning a network interface to a VM, the loop index is used to identify it:</span></span>

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a><span data-ttu-id="d7b42-143">Závislosti</span><span class="sxs-lookup"><span data-stu-id="d7b42-143">Dependencies</span></span>

<span data-ttu-id="d7b42-144">Většina prostředky závisí na jiné prostředky fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="d7b42-144">Most resources depend on other resources to work correctly.</span></span> <span data-ttu-id="d7b42-145">Virtuální počítače musí být přidružený k virtuální síti a k provádění potřebuje síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d7b42-145">Virtual machines must be associated with a virtual network and to do that it needs a network interface.</span></span> <span data-ttu-id="d7b42-146">[DependsOn](../../resource-group-define-dependencies.md) element se používá a ujistěte se, že síťové rozhraní je připravený k použití před vytvořením virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="d7b42-146">The [dependsOn](../../resource-group-define-dependencies.md) element is used to make sure that the network interface is ready to be used before the VMs are created:</span></span>

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

<span data-ttu-id="d7b42-147">Správce prostředků nasadí současně všechny prostředky, které nejsou závislé na jiný prostředek nasazuje.</span><span class="sxs-lookup"><span data-stu-id="d7b42-147">Resource Manager deploys in parallel any resources that are not dependent on another resource being deployed.</span></span> <span data-ttu-id="d7b42-148">Buďte opatrní při nastavení závislostí, protože můžou nechtěně způsobit snížení nasazení tak, že zadáte nepotřebné závislosti.</span><span class="sxs-lookup"><span data-stu-id="d7b42-148">Be careful when setting dependencies because you can inadvertently slow your deployment by specifying unnecessary dependencies.</span></span> <span data-ttu-id="d7b42-149">Závislosti můžete zřetězené prostřednictvím více prostředků.</span><span class="sxs-lookup"><span data-stu-id="d7b42-149">Dependencies can chain through multiple resources.</span></span> <span data-ttu-id="d7b42-150">Síťové rozhraní, například závisí na veřejnou IP adresu a virtuální síťové prostředky.</span><span class="sxs-lookup"><span data-stu-id="d7b42-150">For example, the network interface depends on the public IP address and virtual network resources.</span></span>

<span data-ttu-id="d7b42-151">Jak poznáte, pokud je třeba provést závislost?</span><span class="sxs-lookup"><span data-stu-id="d7b42-151">How do you know if a dependency is required?</span></span> <span data-ttu-id="d7b42-152">Podívejte se na hodnoty, které se nastavují v šabloně.</span><span class="sxs-lookup"><span data-stu-id="d7b42-152">Look at the values you set in the template.</span></span> <span data-ttu-id="d7b42-153">Pokud element body definice prostředků virtuálního počítače na jiný prostředek, který je nasazen do stejné šablony, musíte závislost.</span><span class="sxs-lookup"><span data-stu-id="d7b42-153">If an element in the virtual machine resource definition points to another resource that is deployed in the same template, you need a dependency.</span></span> <span data-ttu-id="d7b42-154">Virtuální počítač příklad určují profil sítě:</span><span class="sxs-lookup"><span data-stu-id="d7b42-154">For example, your example virtual machine defines a network profile:</span></span>

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

<span data-ttu-id="d7b42-155">Pokud chcete nastavit tuto vlastnost, musí existovat síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d7b42-155">To set this property, the network interface must exist.</span></span> <span data-ttu-id="d7b42-156">Proto musíte závislost.</span><span class="sxs-lookup"><span data-stu-id="d7b42-156">Therefore, you need a dependency.</span></span> <span data-ttu-id="d7b42-157">Musíte taky nastavit závislost, když jeden prostředek (podřízená) je definována v rámci jiný prostředek (nadřazené).</span><span class="sxs-lookup"><span data-stu-id="d7b42-157">You also need to set a dependency when one resource (a child) is defined within another resource (a parent).</span></span> <span data-ttu-id="d7b42-158">Například nastavení pro diagnostiku a rozšíření vlastních skriptů jsou obě definované jako podřízené prostředky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7b42-158">For example, the diagnostic settings and custom script extensions are both defined as child resources of the virtual machine.</span></span> <span data-ttu-id="d7b42-159">Nelze vytvořit dokud virtuální počítač existuje.</span><span class="sxs-lookup"><span data-stu-id="d7b42-159">They cannot be created until the virtual machine exists.</span></span> <span data-ttu-id="d7b42-160">Proto i prostředky jsou označené jako závislé na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="d7b42-160">Therefore, both resources are marked as dependent on the virtual machine.</span></span>

## <a name="profiles"></a><span data-ttu-id="d7b42-161">Profily</span><span class="sxs-lookup"><span data-stu-id="d7b42-161">Profiles</span></span>

<span data-ttu-id="d7b42-162">Několik elementy profil se používá při definování prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d7b42-162">Several profile elements are used when defining a virtual machine resource.</span></span> <span data-ttu-id="d7b42-163">Některé jsou vyžadovány a některé jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="d7b42-163">Some are required and some are optional.</span></span> <span data-ttu-id="d7b42-164">Například položka hardwareProfile, osProfile, storageProfile a networkProfile elementy jsou požadovány, ale diagnosticsProfile je volitelný.</span><span class="sxs-lookup"><span data-stu-id="d7b42-164">For example, the hardwareProfile, osProfile, storageProfile, and networkProfile elements are required, but the diagnosticsProfile is optional.</span></span> <span data-ttu-id="d7b42-165">Tyto profily definovat nastavení, jako:</span><span class="sxs-lookup"><span data-stu-id="d7b42-165">These profiles define settings such as:</span></span>
   
- [<span data-ttu-id="d7b42-166">velikost</span><span class="sxs-lookup"><span data-stu-id="d7b42-166">size</span></span>](sizes.md)
- <span data-ttu-id="d7b42-167">[název](/architecture/best-practices/naming-conventions) a přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="d7b42-167">[name](/architecture/best-practices/naming-conventions) and credentials</span></span>
- <span data-ttu-id="d7b42-168">disk a [nastavení operačního systému](cli-ps-findimage.md)</span><span class="sxs-lookup"><span data-stu-id="d7b42-168">disk and [operating system settings](cli-ps-findimage.md)</span></span>
- [<span data-ttu-id="d7b42-169">síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="d7b42-169">network interface</span></span>](../../virtual-network/virtual-networks-multiple-nics.md) 
- <span data-ttu-id="d7b42-170">Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="d7b42-170">boot diagnostics</span></span>

## <a name="disks-and-images"></a><span data-ttu-id="d7b42-171">Disky a obrázků</span><span class="sxs-lookup"><span data-stu-id="d7b42-171">Disks and images</span></span>
   
<span data-ttu-id="d7b42-172">V Azure, můžete soubory vhd představují [disky nebo bitové kopie](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7b42-172">In Azure, vhd files can represent [disks or images](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="d7b42-173">Pokud operační systém v souboru virtuálního pevného disku se specializuje na konkrétním virtuálním počítači, je jako disk uvedené.</span><span class="sxs-lookup"><span data-stu-id="d7b42-173">When the operating system in a vhd file is specialized to be a specific VM, it is referred to as a disk.</span></span> <span data-ttu-id="d7b42-174">Pokud operační systém v souboru virtuálního pevného disku je zobecněn, který se má použít k vytvoření hodně virtuálních počítačů, je jako obrázek uvedené.</span><span class="sxs-lookup"><span data-stu-id="d7b42-174">When the operating system in a vhd file is generalized to be used to create many VMs, it is referred to as an image.</span></span>   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a><span data-ttu-id="d7b42-175">Vytvoření nové virtuální počítače a nové disky z image platformy</span><span class="sxs-lookup"><span data-stu-id="d7b42-175">Create new virtual machines and new disks from a platform image</span></span>

<span data-ttu-id="d7b42-176">Když vytvoříte virtuální počítač, musíte rozhodnout, jaký operační systém používat.</span><span class="sxs-lookup"><span data-stu-id="d7b42-176">When you create a VM, you must decide what operating system to use.</span></span> <span data-ttu-id="d7b42-177">Element imageReference element se používá k definování operačního systému nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d7b42-177">The imageReference element is used to define the operating system of a new VM.</span></span> <span data-ttu-id="d7b42-178">Příklad ukazuje definici pro operační systém Windows Server:</span><span class="sxs-lookup"><span data-stu-id="d7b42-178">The example shows a definition for a Windows Server operating system:</span></span>

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

<span data-ttu-id="d7b42-179">Pokud chcete vytvořit operačního systému Linux, můžete použít tuto definici:</span><span class="sxs-lookup"><span data-stu-id="d7b42-179">If you want to create a Linux operating system, you might use this definition:</span></span>

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

<span data-ttu-id="d7b42-180">Nastavení konfigurace pro disk operačního systému jsou přiřazeny osDisk element.</span><span class="sxs-lookup"><span data-stu-id="d7b42-180">Configuration settings for the operating system disk are assigned with the osDisk element.</span></span> <span data-ttu-id="d7b42-181">V příkladu definuje ukládání do mezipaměti režim nastavený na nový disk spravované **ReadWrite** a zda je disk se vytváří z [image platformy](cli-ps-findimage.md):</span><span class="sxs-lookup"><span data-stu-id="d7b42-181">The example defines a new managed disk with the caching mode set to **ReadWrite** and that the disk is being created from a [platform image](cli-ps-findimage.md):</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a><span data-ttu-id="d7b42-182">Vytvoření nové virtuální počítače z existujícího spravovaného disků</span><span class="sxs-lookup"><span data-stu-id="d7b42-182">Create new virtual machines from existing managed disks</span></span>

<span data-ttu-id="d7b42-183">Pokud chcete vytvořit virtuální počítače z existujícího disků, odeberte elementu imageReference a osProfile elementy a definovat toto nastavení disku:</span><span class="sxs-lookup"><span data-stu-id="d7b42-183">If you want to create virtual machines from existing disks, remove the imageReference and the osProfile elements and define these disk settings:</span></span>

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

### <a name="create-new-virtual-machines-from-a-managed-image"></a><span data-ttu-id="d7b42-184">Vytvoření nových virtuálních počítačů ze spravovaných bitové kopie</span><span class="sxs-lookup"><span data-stu-id="d7b42-184">Create new virtual machines from a managed image</span></span>

<span data-ttu-id="d7b42-185">Pokud chcete vytvořit virtuální počítač z bitové kopie spravované, změňte element elementu imageReference a definovat toto nastavení disku:</span><span class="sxs-lookup"><span data-stu-id="d7b42-185">If you want to create a virtual machine from a managed image, change the imageReference element and define these disk settings:</span></span>

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

### <a name="attach-data-disks"></a><span data-ttu-id="d7b42-186">Připojte datových disků</span><span class="sxs-lookup"><span data-stu-id="d7b42-186">Attach data disks</span></span>

<span data-ttu-id="d7b42-187">Volitelně můžete přidat datových disků do virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7b42-187">You can optionally add data disks to the VMs.</span></span> <span data-ttu-id="d7b42-188">[Počet disků](sizes.md) závisí na velikosti disku operačního systému, který používáte.</span><span class="sxs-lookup"><span data-stu-id="d7b42-188">The [number of disks](sizes.md) depends on the size of operating system disk that you use.</span></span> <span data-ttu-id="d7b42-189">S velikostí virtuálních počítačů, nastavte na Standard_DS1_v2 je maximální počet datových disků, které nebylo možné přidat do je dva.</span><span class="sxs-lookup"><span data-stu-id="d7b42-189">With the size of the VMs set to Standard_DS1_v2, the maximum number of data disks that could be added to the them is two.</span></span> <span data-ttu-id="d7b42-190">V příkladu je přidáván jeden spravovaný datový disk pro každý virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="d7b42-190">In the example, one managed data disk is being added to each VM:</span></span>

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

## <a name="extensions"></a><span data-ttu-id="d7b42-191">Rozšíření</span><span class="sxs-lookup"><span data-stu-id="d7b42-191">Extensions</span></span>

<span data-ttu-id="d7b42-192">I když [rozšíření](extensions-features.md) jsou samostatné prostředků, úzce jsou svázané s virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="d7b42-192">Although [extensions](extensions-features.md) are a separate resource, they are closely tied to VMs.</span></span> <span data-ttu-id="d7b42-193">Rozšíření mohou být přidány jako podřízený prostředek virtuálního počítače nebo jako samostatnou prostředek.</span><span class="sxs-lookup"><span data-stu-id="d7b42-193">Extensions can be added as a child resource of the VM or as a separate resource.</span></span> <span data-ttu-id="d7b42-194">Příklad ukazuje [rozšíření diagnostiky](extensions-diagnostics-template.md) se přidává do virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="d7b42-194">The example shows the [Diagnostics Extension](extensions-diagnostics-template.md) being added to the VMs:</span></span>

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

<span data-ttu-id="d7b42-195">Tento prostředek rozšíření používá proměnnou storageName a diagnostiky proměnné zadat hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d7b42-195">This extension resource uses the storageName variable and the diagnostic variables to provide values.</span></span> <span data-ttu-id="d7b42-196">Pokud chcete změnit data, která se shromažďují v tomto rozšíření, můžete přidat další čítače výkonu wadperfcounters proměnné.</span><span class="sxs-lookup"><span data-stu-id="d7b42-196">If you want to change the data that is collected by this extension, you can add more performance counters to the wadperfcounters variable.</span></span> <span data-ttu-id="d7b42-197">Může se také rozhodnout put diagnostická data do jiného úložiště účtu než kde jsou uloženy disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d7b42-197">You could also choose to put the diagnostics data into a different storage account than where the VM disks are stored.</span></span>

<span data-ttu-id="d7b42-198">Existuje mnoho rozšíření, které můžete nainstalovat na virtuální počítač, ale je velmi užitečné pravděpodobně [rozšíření vlastních skriptů](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="d7b42-198">There are many extensions that you can install on a VM, but the most useful is probably the [Custom Script Extension](extensions-customscript.md).</span></span> <span data-ttu-id="d7b42-199">V příkladu spustí skript prostředí PowerShell s názvem start.ps1 na každý virtuální počítač při prvním spuštění:</span><span class="sxs-lookup"><span data-stu-id="d7b42-199">In the example, a PowerShell script named start.ps1 runs on each VM when it first starts:</span></span>

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

<span data-ttu-id="d7b42-200">Skript start.ps1 lze provádět mnoho úkoly konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d7b42-200">The start.ps1 script can accomplish many configuration tasks.</span></span> <span data-ttu-id="d7b42-201">Například nejsou inicializovány datových disků, které jsou přidány do virtuálních počítačů v příkladu; můžete použít vlastní skript k chybě při inicializaci je.</span><span class="sxs-lookup"><span data-stu-id="d7b42-201">For example, the data disks that are added to the VMs in the example are not initialized; you can use a custom script to initialize them.</span></span> <span data-ttu-id="d7b42-202">Pokud máte více spuštění úlohy uděláte, můžete použít soubor start.ps1 volat jiné skripty prostředí PowerShell v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="d7b42-202">If you have multiple startup tasks to do, you can use the start.ps1 file to call other PowerShell scripts in Azure storage.</span></span> <span data-ttu-id="d7b42-203">Tento příklad používá prostředí PowerShell, ale můžete použít libovolnou skriptování metodu, která je k dispozici v operačním systému, který používáte.</span><span class="sxs-lookup"><span data-stu-id="d7b42-203">The example uses PowerShell, but you can use any scripting method that is available on the operating system that you are using.</span></span>

<span data-ttu-id="d7b42-204">Stav nainstalovaného rozšíření z nastavení rozšíření na portálu můžete zobrazit:</span><span class="sxs-lookup"><span data-stu-id="d7b42-204">You can see the status of the installed extensions from the Extensions settings in the portal:</span></span>

![Načíst stav rozšíření](./media/template-description/virtual-machines-show-extensions.png)

<span data-ttu-id="d7b42-206">Můžete také získat informace o rozšíření pomocí **Get-AzureRmVMExtension** příkazu Powershellu **get rozšíření virtuálního počítače** příkaz Azure CLI 2.0 nebo **získat informace o rozšíření** ROZHRANÍ REST API.</span><span class="sxs-lookup"><span data-stu-id="d7b42-206">You can also get extension information by using the **Get-AzureRmVMExtension** PowerShell command, the **vm extension get** Azure CLI 2.0 command, or the **Get extension information** REST API.</span></span>

## <a name="deployments"></a><span data-ttu-id="d7b42-207">Nasazení</span><span class="sxs-lookup"><span data-stu-id="d7b42-207">Deployments</span></span>

<span data-ttu-id="d7b42-208">Při nasazení šablony Azure sleduje prostředky nasazené jako skupina a automaticky přiřadí název této skupiny nasazené.</span><span class="sxs-lookup"><span data-stu-id="d7b42-208">When you deploy a template, Azure tracks the resources that you deployed as a group and automatically assigns a name to this deployed group.</span></span> <span data-ttu-id="d7b42-209">Název nasazení je stejný jako název šablony.</span><span class="sxs-lookup"><span data-stu-id="d7b42-209">The name of the deployment is the same as the name of the template.</span></span>

<span data-ttu-id="d7b42-210">Pokud jste zvědaví o stavu prostředků v nasazení, můžete v okně skupiny prostředků na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="d7b42-210">If you are curious about the status of resources in the deployment, you can use the Resource Group blade in the Azure portal:</span></span>

![Získat informace o nasazení](./media/template-description/virtual-machines-deployment-info.png)
    
<span data-ttu-id="d7b42-212">Není problém používat stejné šablony vytvořit prostředky nebo aktualizovat existující prostředky.</span><span class="sxs-lookup"><span data-stu-id="d7b42-212">It’s not a problem to use the same template to create resources or to update existing resources.</span></span> <span data-ttu-id="d7b42-213">Při použití příkazů pro nasazení šablon, máte možnost k vyslovení který [režimu](../../resource-group-template-deploy.md) chcete použít.</span><span class="sxs-lookup"><span data-stu-id="d7b42-213">When you use commands to deploy templates, you have the opportunity to say which [mode](../../resource-group-template-deploy.md) you want to use.</span></span> <span data-ttu-id="d7b42-214">Režim může být nastaven na hodnotu **Complete** nebo **přírůstkové**.</span><span class="sxs-lookup"><span data-stu-id="d7b42-214">The mode can be set to either **Complete** or **Incremental**.</span></span> <span data-ttu-id="d7b42-215">Ve výchozím nastavení se přírůstkové aktualizace.</span><span class="sxs-lookup"><span data-stu-id="d7b42-215">The default is to do incremental updates.</span></span> <span data-ttu-id="d7b42-216">Buďte opatrní při používání **Complete** režimu vzhledem k tomu, že omylem může odstranit prostředky.</span><span class="sxs-lookup"><span data-stu-id="d7b42-216">Be careful when using the **Complete** mode because you may accidentally delete resources.</span></span> <span data-ttu-id="d7b42-217">Pokud nastavíte režim na **Complete**, odstraní všechny prostředky ve skupině prostředků, které nejsou v šabloně Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d7b42-217">When you set the mode to **Complete**, Resource Manager deletes any resources in the resource group that are not in the template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7b42-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7b42-218">Next Steps</span></span>

- <span data-ttu-id="d7b42-219">Vytvoření vlastní šablony pomocí [šablon pro tvorbu Azure Resource Manageru](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d7b42-219">Create your own template using [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>
- <span data-ttu-id="d7b42-220">Nasazení šablony, který jste vytvořili pomocí [vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru](ps-template.md).</span><span class="sxs-lookup"><span data-stu-id="d7b42-220">Deploy the template that you created using [Create a Windows virtual machine with a Resource Manager template](ps-template.md).</span></span>
- <span data-ttu-id="d7b42-221">Zjistěte, jak spravovat virtuální počítače, které jste vytvořili kontrolou [vytvořit a spravovat virtuální počítače Windows pomocí modulu Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d7b42-221">Learn how to manage the VMs that you created by reviewing [Create and manage Windows VMs with the Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
