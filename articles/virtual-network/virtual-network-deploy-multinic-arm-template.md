---
title: "Vytvoření virtuálního počítače s více síťovými kartami - šablony Azure Resource Manageru | Microsoft Docs"
description: "Vytvoření virtuálního počítače s více síťovými kartami pomocí šablony Azure Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85bfa264c6cf2b0586816a47b3ab72f3aee8ec96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a><span data-ttu-id="c8d08-103">Vytvoření virtuálního počítače s více síťovými kartami pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="c8d08-103">Create a VM with multiple NICs using a template</span></span>
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="c8d08-104">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c8d08-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="c8d08-105">Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c8d08-105">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="c8d08-106">Následující postup použijte skupinu prostředků s názvem *IaaSStory* pro webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery DB.</span><span class="sxs-lookup"><span data-stu-id="c8d08-106">The following steps use a resource group named *IaaSStory* for the WEB servers and a resource group named *IaaSStory-BackEnd* for the DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8d08-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c8d08-107">Prerequisites</span></span>
<span data-ttu-id="c8d08-108">Před vytvořením servery DB, je potřeba vytvořit *IaaSStory* skupina prostředků se všechny potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="c8d08-108">Before you can create the DB servers, you need to create the *IaaSStory* resource group with all the necessary resources for this scenario.</span></span> <span data-ttu-id="c8d08-109">Pokud chcete vytvořit tyto prostředky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c8d08-109">To create these resources, complete the following steps:</span></span>

1. <span data-ttu-id="c8d08-110">Přejděte na [na stránku šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="c8d08-110">Navigate to [the template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="c8d08-111">Na stránce šablony napravo od **nadřazené skupiny prostředků**, klikněte na tlačítko **nasadit do Azure**.</span><span class="sxs-lookup"><span data-stu-id="c8d08-111">In the template page, to the right of **Parent resource group**, click **Deploy to Azure**.</span></span>
3. <span data-ttu-id="c8d08-112">V případě potřeby změňte hodnoty parametrů a potom postupujte podle kroků v portálu Azure preview nasazení skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="c8d08-112">If needed, change the parameter values, then follow the steps in the Azure preview portal to deploy the resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8d08-113">Ujistěte se, že vaše názvy účtů úložiště jsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="c8d08-113">Make sure your storage account names are unique.</span></span> <span data-ttu-id="c8d08-114">Názvy účtů úložiště duplicitní nemůže mít v Azure.</span><span class="sxs-lookup"><span data-stu-id="c8d08-114">You cannot have duplicate storage account names in Azure.</span></span>
> 

## <a name="understand-the-deployment-template"></a><span data-ttu-id="c8d08-115">Pochopení šablony nasazení</span><span class="sxs-lookup"><span data-stu-id="c8d08-115">Understand the deployment template</span></span>
<span data-ttu-id="c8d08-116">Před nasazením šablony dodávané s tuto dokumentaci, ujistěte se, že chápete, jak funguje.</span><span class="sxs-lookup"><span data-stu-id="c8d08-116">Before you deploy the template provided with this documentation, make sure you understand what it does.</span></span> <span data-ttu-id="c8d08-117">Následující kroky poskytují dobrý přehled o šabloně:</span><span class="sxs-lookup"><span data-stu-id="c8d08-117">The following steps provide a good overview of the template:</span></span>

1. <span data-ttu-id="c8d08-118">Přejděte na [na stránku šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="c8d08-118">Navigate to [the template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="c8d08-119">Klikněte na tlačítko **azuredeploy.json** otevřete soubor šablony.</span><span class="sxs-lookup"><span data-stu-id="c8d08-119">Click **azuredeploy.json** to open the template file.</span></span>
3. <span data-ttu-id="c8d08-120">Upozornění *osType* parametr uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="c8d08-120">Notice the *osType* parameter listed below.</span></span> <span data-ttu-id="c8d08-121">Tento parametr slouží k výběru jaké image virtuálního počítače používat pro databázový server, společně s více operačního systému související nastavení.</span><span class="sxs-lookup"><span data-stu-id="c8d08-121">This parameter is used to select what VM image to use for the database server, along with multiple operating system related settings.</span></span>

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS to use for VMs: Windows or Ubuntu."
      }
    },
    ```

4. <span data-ttu-id="c8d08-122">Posuňte se dolů a seznam proměnných a zkontrolujte definici **dbVMSetting** proměnné, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="c8d08-122">Scroll down to the list of variables, and check the definition for the **dbVMSetting** variables, listed below.</span></span> <span data-ttu-id="c8d08-123">Obdrží jeden prvků pole, které jsou součástí **dbVMSettings** proměnné.</span><span class="sxs-lookup"><span data-stu-id="c8d08-123">It receives one of the array elements contained in the **dbVMSettings** variable.</span></span> <span data-ttu-id="c8d08-124">Pokud jste obeznámeni s terminologie vývoj softwaru, můžete zobrazit **dbVMSettings** proměnné jako zatřiďovací tabulku nebo slovník.</span><span class="sxs-lookup"><span data-stu-id="c8d08-124">If you are familiar with software development terminology, you can view the **dbVMSettings** variable as a hash table, or a dictionary.</span></span>

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. <span data-ttu-id="c8d08-125">Předpokládejme, že se rozhodnete nasadit virtuální počítače Windows se systémem SQL v back-end.</span><span class="sxs-lookup"><span data-stu-id="c8d08-125">Suppose you decide to deploy Windows VMs running SQL in the back-end.</span></span> <span data-ttu-id="c8d08-126">Pak hodnota **osType** by *Windows*a **dbVMSetting** proměnná by obsahovat element uvedené níže, který představuje první hodnota v **dbVMSettings** proměnné.</span><span class="sxs-lookup"><span data-stu-id="c8d08-126">Then the value for **osType** would be *Windows*, and the **dbVMSetting** variable would contain the element listed below, which represents the first value in the **dbVMSettings** variable.</span></span>

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. <span data-ttu-id="c8d08-127">Upozornění **vmSize** obsahuje hodnotu *Standard_DS3*.</span><span class="sxs-lookup"><span data-stu-id="c8d08-127">Notice the **vmSize** contains the value *Standard_DS3*.</span></span> <span data-ttu-id="c8d08-128">Povolit jenom určité velikosti virtuálních počítačů použít několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="c8d08-128">Only certain VM sizes allow for the use of multiple NICs.</span></span> <span data-ttu-id="c8d08-129">Můžete ověřit, které velikosti virtuálních počítačů ve čtení podporovat několik síťových adaptérů [velikosti virtuálních počítačů Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [velikosti virtuálního počítače s Linuxem](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) články.</span><span class="sxs-lookup"><span data-stu-id="c8d08-129">You can verify which VM sizes support multiple NICs by reading the [Windows VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux VM sizes](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) articles.</span></span>

7. <span data-ttu-id="c8d08-130">Přejděte dolů k položce **prostředky** a Všimněte si první prvek.</span><span class="sxs-lookup"><span data-stu-id="c8d08-130">Scroll down to **resources** and notice the first element.</span></span> <span data-ttu-id="c8d08-131">Popisuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c8d08-131">It describes a storage account.</span></span> <span data-ttu-id="c8d08-132">Tento účet úložiště se použije k udržování datové disky používané každou databázi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c8d08-132">This storage account will be used to maintain the data disks used by each database VM.</span></span> <span data-ttu-id="c8d08-133">V tomto scénáři každou databázi virtuálních počítačů má disk s operačním systémem, který je uložen v pravidelných úložiště a dvě datové disky uložené v úložiště SSD (premium).</span><span class="sxs-lookup"><span data-stu-id="c8d08-133">In this scenario, each database VM has an OS disk stored in regular storage, and two data disks stored in SSD (premium) storage.</span></span>

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. <span data-ttu-id="c8d08-134">Posuňte se dolů a další zdroje, jak je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="c8d08-134">Scroll down to the next resource, as listed below.</span></span> <span data-ttu-id="c8d08-135">Tento prostředek představuje síťová karta použitá pro přístup k databázi v každé databázi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c8d08-135">This resource represents the NIC used for database access in each database VM.</span></span> <span data-ttu-id="c8d08-136">Všimněte si použití **kopie** funkce v tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="c8d08-136">Notice the use of the **copy** function in this resource.</span></span> <span data-ttu-id="c8d08-137">Šablona umožňuje nasadit jako hodně virtuálních počítačů tak, jak chcete, založené na **dbCount** parametr.</span><span class="sxs-lookup"><span data-stu-id="c8d08-137">The template allows you to deploy as many VMs as you want, based on the **dbCount** parameter.</span></span> <span data-ttu-id="c8d08-138">Proto musíte vytvořit stejnou úroveň síťové karty pro přístup k databázi, jednu pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c8d08-138">Therefore you need to create the same amount of NICs for database access, one for each VM.</span></span>

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. <span data-ttu-id="c8d08-139">Posuňte se dolů a další zdroje, jak je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="c8d08-139">Scroll down to the next resource, as listed below.</span></span> <span data-ttu-id="c8d08-140">Tento prostředek představuje síťový adaptér používá pro správu v každé databázi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c8d08-140">This resource represents the NIC used for management in each database VM.</span></span> <span data-ttu-id="c8d08-141">Znovu budete potřebovat jeden z tyto síťové adaptéry pro každou databázi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c8d08-141">Once again, you need one of these NICs for each database VM.</span></span> <span data-ttu-id="c8d08-142">Upozornění **skupinu zabezpečení sítě** elementu, skupinu NSG, které umožňuje přístup k protokolu RDP/SSH pro tuto síťovou kartu pouze propojení.</span><span class="sxs-lookup"><span data-stu-id="c8d08-142">Notice the **networkSecurityGroup** element, linking an NSG that allows access to RDP/SSH to this NIC only.</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. <span data-ttu-id="c8d08-143">Posuňte se dolů a další zdroje, jak je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="c8d08-143">Scroll down to the next resource, as listed below.</span></span> <span data-ttu-id="c8d08-144">Tento prostředek představuje sadu ke sdílení všechny virtuální počítače databáze dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c8d08-144">This resource represents an availability set to be shared by all database VMs.</span></span> <span data-ttu-id="c8d08-145">Tímto způsobem můžete zaručit, že bude vždy jeden virtuální počítač v sadě během údržby.</span><span class="sxs-lookup"><span data-stu-id="c8d08-145">That way, you guarantee that there will always be one VM in the set running during maintenance.</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. <span data-ttu-id="c8d08-146">Posuňte se dolů a další prostředků.</span><span class="sxs-lookup"><span data-stu-id="c8d08-146">Scroll down to the next resource.</span></span> <span data-ttu-id="c8d08-147">Tento prostředek představuje databázi virtuálních počítačů, tak v prvním několika řádků, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="c8d08-147">This resource represents the database VMs, as seen in the first few lines listed below.</span></span> <span data-ttu-id="c8d08-148">Všimněte si použití **kopie** funkce znovu, zajistíte, že se vytvářejí na základě několika virtuálními počítači **dbCount** parametr.</span><span class="sxs-lookup"><span data-stu-id="c8d08-148">Notice the use of the **copy** function again, ensuring that multiple VMs are created based on the **dbCount** parameter.</span></span> <span data-ttu-id="c8d08-149">Všimněte si také **dependsOn** kolekce.</span><span class="sxs-lookup"><span data-stu-id="c8d08-149">Also notice the **dependsOn** collection.</span></span> <span data-ttu-id="c8d08-150">Zobrazí se seznam dva síťové adaptéry je nutné vytvořit před nasazením virtuálního počítače, společně s skupiny dostupnosti a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c8d08-150">It lists two NICs being necessary to be created before the VM is deployed, along with the availability set, and the storage account.</span></span>

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. <span data-ttu-id="c8d08-151">Přejděte dolů v prostředek virtuálního počítače, který **networkProfile** elementu, jak je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="c8d08-151">Scroll down in the VM resource to the **networkProfile** element, as listed below.</span></span> <span data-ttu-id="c8d08-152">Všimněte si, že existují dva síťové adaptéry se referenční informace pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c8d08-152">Notice that there are two NICs being reference for each VM.</span></span> <span data-ttu-id="c8d08-153">Když vytvoříte několik síťových adaptérů pro virtuální počítač, je nutné nastavit **primární** vlastnost jedné síťové karty na *true*a zbytek na *false*.</span><span class="sxs-lookup"><span data-stu-id="c8d08-153">When you create multiple NICs for a VM, you must set the **primary** property of one of the NICs to *true*, and the rest to *false*.</span></span>

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a><span data-ttu-id="c8d08-154">Nasazení šablony ARM pomocí metody Click to Deploy</span><span class="sxs-lookup"><span data-stu-id="c8d08-154">Deploy the ARM template by using click to deploy</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8d08-155">Použijte [předpoklady](#Pre-requisites) kroky před níže uvedených pokynů.</span><span class="sxs-lookup"><span data-stu-id="c8d08-155">Make sure you follow the [pre-requisites](#Pre-requisites) steps before following the instructions below.</span></span>
> 

<span data-ttu-id="c8d08-156">Ukázková šablona, která je k dispozici ve veřejném úložišti, používá soubor parametrů obsahující výchozí hodnoty, které se použijí k vygenerování výše popsaného scénáře.</span><span class="sxs-lookup"><span data-stu-id="c8d08-156">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="c8d08-157">K nasazení této šablony metody click to deploy, postupujte podle [tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), napravo od **skupina prostředků back-end (viz dokumentace)** klikněte na tlačítko **nasadit do Azure**, nahradí výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů v portálu.</span><span class="sxs-lookup"><span data-stu-id="c8d08-157">To deploy this template using click to deploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), to the right of **Backend resource group (see documentation)** click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

<span data-ttu-id="c8d08-158">Následující obrázek znázorňuje obsah novou skupinu prostředků, po nasazení.</span><span class="sxs-lookup"><span data-stu-id="c8d08-158">The figure below shows the contents of the new resource group, after deployment.</span></span>

![Skupina prostředků back-end](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a><span data-ttu-id="c8d08-160">Nasazení šablony pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8d08-160">Deploy the template by using PowerShell</span></span>
<span data-ttu-id="c8d08-161">Pokud chcete nasadit šablonu stáhnout pomocí prostředí PowerShell, instalace a konfigurace prostředí PowerShell pomocí kroků v [instalace a konfigurace prostředí PowerShell](/powershell/azure/overview) článku a potom proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c8d08-161">To deploy the template you downloaded by using PowerShell, install and configure PowerShell by completing the steps in the [Install and configure PowerShell](/powershell/azure/overview) article and then complete the following steps:</span></span>

<span data-ttu-id="c8d08-162">Spustit  **`New-AzureRmResourceGroup`**  vytvořte skupinu prostředků pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="c8d08-162">Run the **`New-AzureRmResourceGroup`** cmdlet to create a resource group using the template.</span></span>

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

<span data-ttu-id="c8d08-163">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c8d08-163">Expected output:</span></span>

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a><span data-ttu-id="c8d08-164">Nasazení šablony pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c8d08-164">Deploy the template by using the Azure CLI</span></span>
<span data-ttu-id="c8d08-165">Pokud chcete nasadit šablonu pomocí rozhraní příkazového řádku Azure, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="c8d08-165">To deploy the template by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="c8d08-166">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c8d08-166">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="c8d08-167">Spuštěním příkazu **`azure config mode`** přepněte do režimu Resource Manager, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="c8d08-167">Run the **`azure config mode`** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="c8d08-168">Očekávaný výstup zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="c8d08-168">The expected output follows:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="c8d08-169">Otevřete [soubor parametrů](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), vyberte svůj obsah a uložte je do souboru v počítači.</span><span class="sxs-lookup"><span data-stu-id="c8d08-169">Open the [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), select its contents, and save it to a file in your computer.</span></span> <span data-ttu-id="c8d08-170">V tomto příkladu jsme uložili soubor parametrů do souboru *parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="c8d08-170">For this example, we saved the parameters file to *parameters.json*.</span></span>
4. <span data-ttu-id="c8d08-171">Spuštěním rutiny **`azure group deployment create`** nasadíte novou síť VNet pomocí šablony a souborů parametrů, které jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="c8d08-171">Run the **`azure group deployment create`** cmdlet to deploy the new VNet by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="c8d08-172">Seznam uvedený za výstupem vysvětluje použité parametry.</span><span class="sxs-lookup"><span data-stu-id="c8d08-172">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    <span data-ttu-id="c8d08-173">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c8d08-173">Expected output:</span></span>
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

