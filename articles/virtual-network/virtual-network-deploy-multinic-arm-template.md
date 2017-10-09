---
title: "aaaCreate virtuálního počítače s více síťovými kartami - šablony Azure Resource Manageru | Microsoft Docs"
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
ms.openlocfilehash: f5d9ffcbd40c72dc18ae6de38e739eb5e45bf669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a><span data-ttu-id="56338-103">Vytvoření virtuálního počítače s více síťovými kartami pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="56338-103">Create a VM with multiple NICs using a template</span></span>
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="56338-104">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="56338-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="56338-105">Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="56338-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="56338-106">Hello následující postup použijte skupinu prostředků s názvem *IaaSStory* pro hello webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery hello DB.</span><span class="sxs-lookup"><span data-stu-id="56338-106">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56338-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="56338-107">Prerequisites</span></span>
<span data-ttu-id="56338-108">Před vytvořením hello servery DB, musíte toocreate hello *IaaSStory* skupina prostředků se všechny hello potřebné prostředky pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="56338-108">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="56338-109">dokončení těchto prostředků, toocreate hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="56338-109">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="56338-110">Přejděte příliš[stránku hello šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="56338-110">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="56338-111">V stránku hello šablony, toohello napravo od **nadřazené skupiny prostředků**, klikněte na tlačítko **nasazení tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="56338-111">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="56338-112">V případě potřeby změňte hodnoty parametrů hello a potom postupujte podle kroků hello ve skupině prostředků hello toodeploy portálu Azure preview hello.</span><span class="sxs-lookup"><span data-stu-id="56338-112">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56338-113">Ujistěte se, že vaše názvy účtů úložiště jsou jedinečné.</span><span class="sxs-lookup"><span data-stu-id="56338-113">Make sure your storage account names are unique.</span></span> <span data-ttu-id="56338-114">Názvy účtů úložiště duplicitní nemůže mít v Azure.</span><span class="sxs-lookup"><span data-stu-id="56338-114">You cannot have duplicate storage account names in Azure.</span></span>
> 

## <a name="understand-hello-deployment-template"></a><span data-ttu-id="56338-115">Pochopení hello nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="56338-115">Understand hello deployment template</span></span>
<span data-ttu-id="56338-116">Před nasazením hello šablony aplikace této dokumentace, ujistěte se, že chápete, co dělá.</span><span class="sxs-lookup"><span data-stu-id="56338-116">Before you deploy hello template provided with this documentation, make sure you understand what it does.</span></span> <span data-ttu-id="56338-117">Následující kroky Hello poskytují dobrý přehled o hello šablony:</span><span class="sxs-lookup"><span data-stu-id="56338-117">hello following steps provide a good overview of hello template:</span></span>

1. <span data-ttu-id="56338-118">Přejděte příliš[stránku hello šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="56338-118">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="56338-119">Klikněte na tlačítko **azuredeploy.json** soubor šablony tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="56338-119">Click **azuredeploy.json** tooopen hello template file.</span></span>
3. <span data-ttu-id="56338-120">Všimněte si hello *osType* parametr uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="56338-120">Notice hello *osType* parameter listed below.</span></span> <span data-ttu-id="56338-121">Tento parametr je použité tooselect co toouse bitové kopie virtuálního počítače pro server databáze hello, společně s více operačního systému související nastavení.</span><span class="sxs-lookup"><span data-stu-id="56338-121">This parameter is used tooselect what VM image toouse for hello database server, along with multiple operating system related settings.</span></span>

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS toouse for VMs: Windows or Ubuntu."
      }
    },
    ```

4. <span data-ttu-id="56338-122">Posuňte se dolů toohello seznam proměnných a zkontrolujte hello Definice hello **dbVMSetting** proměnné, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="56338-122">Scroll down toohello list of variables, and check hello definition for hello **dbVMSetting** variables, listed below.</span></span> <span data-ttu-id="56338-123">Jeden z elementů pole hello obsažené v hello obdrží **dbVMSettings** proměnné.</span><span class="sxs-lookup"><span data-stu-id="56338-123">It receives one of hello array elements contained in hello **dbVMSettings** variable.</span></span> <span data-ttu-id="56338-124">Pokud jste obeznámeni s terminologie vývoj softwaru, můžete si prohlédnout hello **dbVMSettings** proměnné jako zatřiďovací tabulku nebo slovník.</span><span class="sxs-lookup"><span data-stu-id="56338-124">If you are familiar with software development terminology, you can view hello **dbVMSettings** variable as a hash table, or a dictionary.</span></span>

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. <span data-ttu-id="56338-125">Předpokládejme, že se že rozhodnete toodeploy Windows virtuální počítače se systémem SQL v hello back-end.</span><span class="sxs-lookup"><span data-stu-id="56338-125">Suppose you decide toodeploy Windows VMs running SQL in hello back-end.</span></span> <span data-ttu-id="56338-126">Potom hello hodnotu **osType** by *Windows*a hello **dbVMSetting** proměnná by obsahovat element hello uvedené níže, který představuje první hodnota hello v hello **dbVMSettings** proměnné.</span><span class="sxs-lookup"><span data-stu-id="56338-126">Then hello value for **osType** would be *Windows*, and hello **dbVMSetting** variable would contain hello element listed below, which represents hello first value in hello **dbVMSettings** variable.</span></span>

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

6. <span data-ttu-id="56338-127">Všimněte si hello **vmSize** obsahuje hodnotu hello *Standard_DS3*.</span><span class="sxs-lookup"><span data-stu-id="56338-127">Notice hello **vmSize** contains hello value *Standard_DS3*.</span></span> <span data-ttu-id="56338-128">Pouze určité velikosti virtuálních počítačů povolit pro použití hello několik síťových adaptérů.</span><span class="sxs-lookup"><span data-stu-id="56338-128">Only certain VM sizes allow for hello use of multiple NICs.</span></span> <span data-ttu-id="56338-129">Můžete ověřit, které velikosti virtuálních počítačů podporují několik síťových adaptérů a čtení hello [velikosti virtuálních počítačů Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [velikosti virtuálního počítače s Linuxem](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) články.</span><span class="sxs-lookup"><span data-stu-id="56338-129">You can verify which VM sizes support multiple NICs by reading hello [Windows VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Linux VM sizes](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) articles.</span></span>

7. <span data-ttu-id="56338-130">Posuňte se dolů příliš**prostředky** a Všimněte si hello první prvek.</span><span class="sxs-lookup"><span data-stu-id="56338-130">Scroll down too**resources** and notice hello first element.</span></span> <span data-ttu-id="56338-131">Popisuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="56338-131">It describes a storage account.</span></span> <span data-ttu-id="56338-132">Tento účet úložiště bude použité toomaintain hello datové disky používané každou databázi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="56338-132">This storage account will be used toomaintain hello data disks used by each database VM.</span></span> <span data-ttu-id="56338-133">V tomto scénáři každou databázi virtuálních počítačů má disk s operačním systémem, který je uložen v pravidelných úložiště a dvě datové disky uložené v úložiště SSD (premium).</span><span class="sxs-lookup"><span data-stu-id="56338-133">In this scenario, each database VM has an OS disk stored in regular storage, and two data disks stored in SSD (premium) storage.</span></span>

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

8. <span data-ttu-id="56338-134">Posuňte se dolů toohello další prostředků, jak je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="56338-134">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="56338-135">Tento prostředek představuje hello síťový adaptér používá pro přístup k databázi v každé databázi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="56338-135">This resource represents hello NIC used for database access in each database VM.</span></span> <span data-ttu-id="56338-136">Všimněte si použití hello hello **kopie** funkce v tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="56338-136">Notice hello use of hello **copy** function in this resource.</span></span> <span data-ttu-id="56338-137">Hello šablona vám umožní toodeploy jako hodně virtuálních počítačů tak, jak chcete, podle hello **dbCount** parametr.</span><span class="sxs-lookup"><span data-stu-id="56338-137">hello template allows you toodeploy as many VMs as you want, based on hello **dbCount** parameter.</span></span> <span data-ttu-id="56338-138">Proto je nutné toocreate hello stejnou úroveň síťové karty pro přístup k databázi, jednu pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="56338-138">Therefore you need toocreate hello same amount of NICs for database access, one for each VM.</span></span>

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

9. <span data-ttu-id="56338-139">Posuňte se dolů toohello další prostředků, jak je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="56338-139">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="56338-140">Tento prostředek představuje hello síťový adaptér používá pro správu v každé databázi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="56338-140">This resource represents hello NIC used for management in each database VM.</span></span> <span data-ttu-id="56338-141">Znovu budete potřebovat jeden z tyto síťové adaptéry pro každou databázi virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="56338-141">Once again, you need one of these NICs for each database VM.</span></span> <span data-ttu-id="56338-142">Všimněte si hello **skupinu zabezpečení sítě** elementu propojení skupinu NSG, který umožňuje přístup toothis tooRDP/SSH seskupování jenom.</span><span class="sxs-lookup"><span data-stu-id="56338-142">Notice hello **networkSecurityGroup** element, linking an NSG that allows access tooRDP/SSH toothis NIC only.</span></span>

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

10. <span data-ttu-id="56338-143">Posuňte se dolů toohello další prostředků, jak je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="56338-143">Scroll down toohello next resource, as listed below.</span></span> <span data-ttu-id="56338-144">Tento prostředek představuje toobe sadu dostupnosti sdílí všechny virtuální počítače databáze.</span><span class="sxs-lookup"><span data-stu-id="56338-144">This resource represents an availability set toobe shared by all database VMs.</span></span> <span data-ttu-id="56338-145">Tímto způsobem můžete zaručit, že bude vždy jeden virtuální počítač v hello nastavit během údržby.</span><span class="sxs-lookup"><span data-stu-id="56338-145">That way, you guarantee that there will always be one VM in hello set running during maintenance.</span></span>

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

11. <span data-ttu-id="56338-146">Posuňte se dolů toohello další prostředků.</span><span class="sxs-lookup"><span data-stu-id="56338-146">Scroll down toohello next resource.</span></span> <span data-ttu-id="56338-147">Tento prostředek představuje hello databáze virtuálních počítačů, jak je vidět v hello nejprve několika řádků, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="56338-147">This resource represents hello database VMs, as seen in hello first few lines listed below.</span></span> <span data-ttu-id="56338-148">Všimněte si použití hello hello **kopie** znovu fungovat, zajistíte, že jsou vytvořeny víc virtuálních počítačů podle hello **dbCount** parametr.</span><span class="sxs-lookup"><span data-stu-id="56338-148">Notice hello use of hello **copy** function again, ensuring that multiple VMs are created based on hello **dbCount** parameter.</span></span> <span data-ttu-id="56338-149">Všimněte si také hello **dependsOn** kolekce.</span><span class="sxs-lookup"><span data-stu-id="56338-149">Also notice hello **dependsOn** collection.</span></span> <span data-ttu-id="56338-150">Zobrazí se seznam dva síťové adaptéry je nutné toobe vytvořil před hello virtuální počítač nasazen, společně s hello skupinu dostupnosti a účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="56338-150">It lists two NICs being necessary toobe created before hello VM is deployed, along with hello availability set, and hello storage account.</span></span>

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

12. <span data-ttu-id="56338-151">Přejděte dolů v toohello prostředků virtuálních počítačů hello **networkProfile** elementu, jak je uvedeno dále.</span><span class="sxs-lookup"><span data-stu-id="56338-151">Scroll down in hello VM resource toohello **networkProfile** element, as listed below.</span></span> <span data-ttu-id="56338-152">Všimněte si, že existují dva síťové adaptéry se referenční informace pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="56338-152">Notice that there are two NICs being reference for each VM.</span></span> <span data-ttu-id="56338-153">Když vytvoříte několik síťových adaptérů pro virtuální počítač, je nutné nastavit hello **primární** vlastnost hello síťové adaptéry příliš*true*, a příliš hello rest*false*.</span><span class="sxs-lookup"><span data-stu-id="56338-153">When you create multiple NICs for a VM, you must set hello **primary** property of one of hello NICs too*true*, and hello rest too*false*.</span></span>

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

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a><span data-ttu-id="56338-154">Nasazení šablony ARM hello pomocí klikněte na tlačítko toodeploy</span><span class="sxs-lookup"><span data-stu-id="56338-154">Deploy hello ARM template by using click toodeploy</span></span>

> [!IMPORTANT]
> <span data-ttu-id="56338-155">Ujistěte se, postupujte hello [předpoklady](#Pre-requisites) kroky před hello pokynů níže.</span><span class="sxs-lookup"><span data-stu-id="56338-155">Make sure you follow hello [pre-requisites](#Pre-requisites) steps before following hello instructions below.</span></span>
> 

<span data-ttu-id="56338-156">Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="56338-156">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="56338-157">toodeploy pomocí této šablony, klikněte na toodeploy, postupujte podle [tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello napravo od **skupina prostředků back-end (viz dokumentace)** klikněte na tlačítko **nasazení tooAzure**, nahraďte výchozí hodnoty parametrů v případě potřeby Hello a postupujte podle pokynů hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="56338-157">toodeploy this template using click toodeploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello right of **Backend resource group (see documentation)** click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

<span data-ttu-id="56338-158">Hello obrázek níže ukazuje hello obsah hello novou skupinu prostředků, po nasazení.</span><span class="sxs-lookup"><span data-stu-id="56338-158">hello figure below shows hello contents of hello new resource group, after deployment.</span></span>

![Skupina prostředků back-end](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="56338-160">Nasazení šablony hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="56338-160">Deploy hello template by using PowerShell</span></span>
<span data-ttu-id="56338-161">Šablona hello toodeploy jste stáhli pomocí prostředí PowerShell, instalace a konfigurace prostředí PowerShell pomocí kroků hello v hello [instalace a konfigurace prostředí PowerShell](/powershell/azure/overview) článek a pak dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="56338-161">toodeploy hello template you downloaded by using PowerShell, install and configure PowerShell by completing hello steps in hello [Install and configure PowerShell](/powershell/azure/overview) article and then complete hello following steps:</span></span>

<span data-ttu-id="56338-162">Spustit hello  **`New-AzureRmResourceGroup`**  hello rutiny toocreate skupinu prostředků pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="56338-162">Run hello **`New-AzureRmResourceGroup`** cmdlet toocreate a resource group using hello template.</span></span>

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

<span data-ttu-id="56338-163">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56338-163">Expected output:</span></span>

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

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="56338-164">Nasazení šablony hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="56338-164">Deploy hello template by using hello Azure CLI</span></span>
<span data-ttu-id="56338-165">šablony hello toodeploy pomocí hello rozhraní příkazového řádku Azure, postupujte podle kroků hello níže.</span><span class="sxs-lookup"><span data-stu-id="56338-165">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="56338-166">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="56338-166">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="56338-167">Spustit hello  **`azure config mode`**  příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="56338-167">Run hello **`azure config mode`** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="56338-168">Hello očekávaný výstup následovně:</span><span class="sxs-lookup"><span data-stu-id="56338-168">hello expected output follows:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="56338-169">Otevřete hello [soubor parametrů](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), vyberte její obsah a uložte ji tooa soubor v počítači.</span><span class="sxs-lookup"><span data-stu-id="56338-169">Open hello [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), select its contents, and save it tooa file in your computer.</span></span> <span data-ttu-id="56338-170">V tomto příkladu jsme příliš uložili soubor parametrů hello*Parameters.JSON tímto kódem*.</span><span class="sxs-lookup"><span data-stu-id="56338-170">For this example, we saved hello parameters file too*parameters.json*.</span></span>
4. <span data-ttu-id="56338-171">Spustit hello  **`azure group deployment create`**  rutiny toodeploy hello nové sítě VNet pomocí šablony hello a parametr soubory jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="56338-171">Run hello **`azure group deployment create`** cmdlet toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="56338-172">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="56338-172">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    <span data-ttu-id="56338-173">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="56338-173">Expected output:</span></span>
   
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

