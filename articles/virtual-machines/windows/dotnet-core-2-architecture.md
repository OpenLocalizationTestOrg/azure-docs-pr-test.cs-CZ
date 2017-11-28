---
title: "Nasazení Windows výpočetní prostředky pomocí šablony Azure Resource Manageru | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b026fe81-1bc1-4899-ac32-886091671498
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8a8b888195e52ea9669922a6a00a873025f3c375
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="e35aa-103">Architektura aplikace pomocí šablony Azure Resource Manageru pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="e35aa-103">Application architecture with Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="e35aa-104">Požadavky na výpočetní při vývoji nasazení Azure Resource Manager, musí být namapována k prostředků Azure a službám.</span><span class="sxs-lookup"><span data-stu-id="e35aa-104">When developing an Azure Resource Manager deployment, compute requirements need to be mapped to Azure resources and services.</span></span> <span data-ttu-id="e35aa-105">Pokud aplikace obsahuje několik koncových bodů protokolu http, databáze a dat služby ukládání do mezipaměti, prostředky Azure tohoto hostitele každý všechny tyto komponenty musí být rationalized.</span><span class="sxs-lookup"><span data-stu-id="e35aa-105">If an application consists of several http endpoints, a database, and a data caching service, the Azure resources that host each of these components needs to be rationalized.</span></span> <span data-ttu-id="e35aa-106">Například ukázkovou aplikaci Hudba úložiště zahrnuje webovou aplikaci, která je hostovaná na virtuálním počítači a databázi SQL, který je hostován v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="e35aa-106">For instance, the sample Music Store application includes a web application that is hosted on a virtual machine, and a SQL database, which is hosted in Azure SQL database.</span></span> 

<span data-ttu-id="e35aa-107">Tento dokument podrobně popisuje, jak jsou nakonfigurované výpočetní prostředky úložiště Hudba v šablony Azure Resource Manageru ukázkový.</span><span class="sxs-lookup"><span data-stu-id="e35aa-107">This document details how the Music Store compute resources are configured in the sample Azure Resource Manager template.</span></span> <span data-ttu-id="e35aa-108">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e35aa-108">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="e35aa-109">Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="e35aa-109">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="e35aa-110">Úplnou šablonu naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="e35aa-110">The complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="virtual-machine"></a><span data-ttu-id="e35aa-111">Virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="e35aa-111">Virtual Machine</span></span>
<span data-ttu-id="e35aa-112">Aplikaci Store Hudba zahrnuje webové aplikace, kde mohou zákazníci procházení a nákup Hudba.</span><span class="sxs-lookup"><span data-stu-id="e35aa-112">The Music Store application includes a web application where customers can browse and purchase music.</span></span> <span data-ttu-id="e35aa-113">Existuje několik služeb Azure, které může být hostitelem webové aplikace v tomto příkladu se používá virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e35aa-113">While there are several Azure services that can host web applications, for this example, a Virtual Machine is used.</span></span> <span data-ttu-id="e35aa-114">Pomocí šablony Hudba úložiště ukázkové nasazení virtuálního počítače, nainstalovat webový server a webu Hudba úložiště nainstalována a nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="e35aa-114">Using the sample Music Store template, a virtual machine is deployed, a web server install, and the Music Store website installed and configured.</span></span> <span data-ttu-id="e35aa-115">Z důvodu tohoto článku je podrobně popsán pouze nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e35aa-115">For the sake of this article, only the virtual machine deployment is detailed.</span></span> <span data-ttu-id="e35aa-116">Konfigurace webového serveru a aplikace je podrobně popsaná v článku na novější.</span><span class="sxs-lookup"><span data-stu-id="e35aa-116">The configuration of the web server and the application is detailed in a later article.</span></span>

<span data-ttu-id="e35aa-117">Virtuální počítač můžete přidat do šablony pomocí průvodce Visual Studio, přidejte nový prostředek, nebo vložením platný kód JSON do šablony nasazení.</span><span class="sxs-lookup"><span data-stu-id="e35aa-117">A virtual machine can be added to a template using the Visual Studio Add New Resource wizard, or by inserting valid JSON into the deployment template.</span></span> <span data-ttu-id="e35aa-118">Při nasazování virtuálního počítače, je také potřeba několik související prostředky.</span><span class="sxs-lookup"><span data-stu-id="e35aa-118">When deploying a virtual machine, several related resources are also needed.</span></span> <span data-ttu-id="e35aa-119">Když pomocí sady Visual Studio pro vytvoření šablony, vytvoří se pro vás tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="e35aa-119">If using Visual Studio to create the template, these resources are created for you.</span></span> <span data-ttu-id="e35aa-120">Pokud ručně vytváření šablony, tyto prostředky je nutné vložit a nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="e35aa-120">If manually constructing the template, these resources need to be inserted and configured.</span></span>

<span data-ttu-id="e35aa-121">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [virtuální počítač JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).</span><span class="sxs-lookup"><span data-stu-id="e35aa-121">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).</span></span>

```json
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

<span data-ttu-id="e35aa-122">Po nasazení můžete zobrazit vlastnosti virtuálního počítače na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e35aa-122">Once deployed, the virtual machine properties can be seen in the Azure portal.</span></span>

![Virtuální počítač](./media/dotnet-core-2-architecture/vm-win.png)

## <a name="storage-account"></a><span data-ttu-id="e35aa-124">Účet úložiště</span><span class="sxs-lookup"><span data-stu-id="e35aa-124">Storage Account</span></span>
<span data-ttu-id="e35aa-125">Účty úložiště mají mnoho možností úložiště a možnosti.</span><span class="sxs-lookup"><span data-stu-id="e35aa-125">Storage accounts have many storage options and capabilities.</span></span> <span data-ttu-id="e35aa-126">V kontextu virtuálními počítači Azure účet úložiště obsahuje virtuální pevné disky virtuálního počítače a jakýchkoli dalších datových disků.</span><span class="sxs-lookup"><span data-stu-id="e35aa-126">For the context of Azure Virtual machines, a storage account holds the virtual hard drives of the virtual machine and any additional data disks.</span></span> <span data-ttu-id="e35aa-127">Ukázky hudby úložiště obsahuje jeden účet úložiště pro uložení virtuálního pevného disku každého virtuálního počítače v nasazení.</span><span class="sxs-lookup"><span data-stu-id="e35aa-127">The Music Store sample includes one storage account to hold the virtual hard drive of each virtual machine in the deployment.</span></span> 

<span data-ttu-id="e35aa-128">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [účet úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).</span><span class="sxs-lookup"><span data-stu-id="e35aa-128">Follow this link to see the JSON sample within the Resource Manager template – [Storage Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

<span data-ttu-id="e35aa-129">Účet úložiště je spojený s virtuální počítač v rámci deklaraci šablony správce prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e35aa-129">A storage account is associate with a virtual machine inside the Resource Manager template declaration of the virtual machine.</span></span> 

<span data-ttu-id="e35aa-130">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [přidružení virtuálního počítače a účet úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).</span><span class="sxs-lookup"><span data-stu-id="e35aa-130">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine and Storage Account association](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).</span></span>

```json
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

<span data-ttu-id="e35aa-131">Po nasazení účtu úložiště lze zobrazit na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e35aa-131">After deployment, the storage account can be viewed in the Azure portal.</span></span>

![Účet úložiště](./media/dotnet-core-2-architecture/storacct-win.png)

<span data-ttu-id="e35aa-133">Kliknutím na do kontejneru objektů blob účet úložiště, lze je zobrazit souboru virtuálního pevného disku pro každý virtuální počítač nasadit pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="e35aa-133">Clicking into the storage account blob container, the virtual hard drive file for each virtual machine deployed with the template can be seen.</span></span>

![Virtuální pevné disky](./media/dotnet-core-2-architecture/vhd-win.png)

<span data-ttu-id="e35aa-135">Další informace o Azure Storage najdete v tématu [dokumentaci pro Azure Storage](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="e35aa-135">For more information on Azure Storage, see [Azure Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>

## <a name="virtual-network"></a><span data-ttu-id="e35aa-136">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="e35aa-136">Virtual Network</span></span>
<span data-ttu-id="e35aa-137">Pokud virtuální počítač vyžaduje interní sítě, jako je například schopnost komunikovat s jinými virtuálními počítači a prostředky Azure, je třeba virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="e35aa-137">If a virtual machine requires internal networking such as the ability to communicate with other virtual machines and Azure resources, an Azure Virtual Network is required.</span></span>  <span data-ttu-id="e35aa-138">Virtuální sítě není zpřístupněte virtuálního počítače přes internet.</span><span class="sxs-lookup"><span data-stu-id="e35aa-138">A virtual network does not make the virtual machine accessible over the internet.</span></span> <span data-ttu-id="e35aa-139">Veřejné připojení vyžaduje veřejnou IP adresu, která je podrobně popsán dále v této série.</span><span class="sxs-lookup"><span data-stu-id="e35aa-139">Public connectivity requires a public IP address, which is detailed later in this series.</span></span>

<span data-ttu-id="e35aa-140">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [virtuální sítě a podsítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).</span><span class="sxs-lookup"><span data-stu-id="e35aa-140">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Network and Subnets](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

<span data-ttu-id="e35aa-141">Z portálu Azure virtuální sítě vypadá jako na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e35aa-141">From the Azure portal, the virtual network looks like the following image.</span></span> <span data-ttu-id="e35aa-142">Všimněte si, že všechny virtuální počítače nasazené pomocí šablony jsou připojené k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="e35aa-142">Notice that all virtual machines deployed with the template are attached to the virtual network.</span></span>

![Virtual Network](./media/dotnet-core-2-architecture/vnet-win.png)

## <a name="network-interface"></a><span data-ttu-id="e35aa-144">Síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="e35aa-144">Network Interface</span></span>
 <span data-ttu-id="e35aa-145">Síťové rozhraní připojí virtuální počítač k virtuální síti, konkrétně k podsíti, která byla definována ve virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="e35aa-145">A network interface connects a virtual machine to a virtual network, more specifically to a subnet that has been defined in the virtual network.</span></span> 

 <span data-ttu-id="e35aa-146">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [síťové rozhraní](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).</span><span class="sxs-lookup"><span data-stu-id="e35aa-146">Follow this link to see the JSON sample within the Resource Manager template – [Network Interface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

<span data-ttu-id="e35aa-147">Každý prostředek virtuálního počítače obsahuje profil sítě.</span><span class="sxs-lookup"><span data-stu-id="e35aa-147">Each virtual machine resource includes a network profile.</span></span> <span data-ttu-id="e35aa-148">Síťové rozhraní je přidružený k virtuálnímu počítači v tomto profilu.</span><span class="sxs-lookup"><span data-stu-id="e35aa-148">The network interface is associated with the virtual machine in this profile.</span></span>  

<span data-ttu-id="e35aa-149">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [síťového profilu virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).</span><span class="sxs-lookup"><span data-stu-id="e35aa-149">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine Network Profile](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).</span></span>

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

<span data-ttu-id="e35aa-150">Z portálu Azure síťové rozhraní vypadá jako na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="e35aa-150">From the Azure portal, the network interface looks like the following image.</span></span> <span data-ttu-id="e35aa-151">Interní IP adresu a přidružení virtuálního počítače můžete zobrazit na prostředek rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="e35aa-151">The internal IP address and the virtual machine association can be seen on the network interface resource.</span></span>

![Síťové rozhraní](./media/dotnet-core-2-architecture/nic-win.png)

<span data-ttu-id="e35aa-153">Další informace o virtuálních sítí Azure najdete v tématu [dokumentace Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="e35aa-153">For more information on Azure Virtual Networks, see [Azure Virtual Network documentation](https://azure.microsoft.com/documentation/services/virtual-network/).</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="e35aa-154">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e35aa-154">Azure SQL Database</span></span>
<span data-ttu-id="e35aa-155">Kromě hostování webu Hudba úložiště virtuálního počítače je nasazený databáze SQL Azure k hostování databáze hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="e35aa-155">In addition to a virtual machine hosting the Music Store website, an Azure SQL Database is deployed to host the Music Store database.</span></span> <span data-ttu-id="e35aa-156">Výhodou použití Azure SQL Database v tomto poli je, že druhá sada virtuálních počítačů se nevyžaduje a škálování a dostupnosti je integrovaná do služby.</span><span class="sxs-lookup"><span data-stu-id="e35aa-156">The advantage of using Azure SQL Database here is that a second set of virtual machines is not required, and scale and availability is built into the service.</span></span>

<span data-ttu-id="e35aa-157">Azure SQL database lze přidat pomocí Visual Studio přidat nový prostředek průvodce, nebo vložením platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="e35aa-157">An Azure SQL database can be added using the Visual Studio Add New Resource wizard, or by inserting valid JSON into a template.</span></span> <span data-ttu-id="e35aa-158">Prostředek systému SQL Server obsahuje uživatelské jméno a heslo, které jsou udělena práva správce v instanci SQL.</span><span class="sxs-lookup"><span data-stu-id="e35aa-158">The SQL Server resource includes a user name and password that is granted administrative rights on the SQL instance.</span></span> <span data-ttu-id="e35aa-159">Navíc je přidání brány firewall zdroje SQL.</span><span class="sxs-lookup"><span data-stu-id="e35aa-159">Also, a SQL firewall resource is added.</span></span> <span data-ttu-id="e35aa-160">Aplikace hostované v Azure jsou ve výchozím nastavení, moct připojit k instanci SQL.</span><span class="sxs-lookup"><span data-stu-id="e35aa-160">By default, applications hosted in Azure are able to connect with the SQL instance.</span></span> <span data-ttu-id="e35aa-161">Takové SQL Server Management studio se připojit k instanci serveru SQL, brány firewall umožňující externí aplikace je potřeba nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="e35aa-161">To allow external application such a SQL Server Management studio to connect to the SQL instance, the firewall needs to be configured.</span></span> <span data-ttu-id="e35aa-162">Výchozí konfigurace je z důvodu ukázku Hudba úložiště v pořádku.</span><span class="sxs-lookup"><span data-stu-id="e35aa-162">For the sake of the Music Store demo, the default configuration is fine.</span></span> 

<span data-ttu-id="e35aa-163">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).</span><span class="sxs-lookup"><span data-stu-id="e35aa-163">Follow this link to see the JSON sample within the Resource Manager template – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).</span></span>

```json
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

<span data-ttu-id="e35aa-164">Zobrazení systému SQL server a databáze MusicStore, jak je vidět na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e35aa-164">A view of the SQL server and MusicStore database as seen in the Azure portal.</span></span>

![SQL Server](./media/dotnet-core-2-architecture/sql-win.png)

<span data-ttu-id="e35aa-166">Další informace o nasazení služby Azure SQL Database, najdete v části [dokumentace Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="e35aa-166">For more information on deploying Azure SQL Database, see [Azure SQL Database documentation](https://azure.microsoft.com/documentation/services/sql-database/).</span></span>

## <a name="next-step"></a><span data-ttu-id="e35aa-167">Další krok</span><span class="sxs-lookup"><span data-stu-id="e35aa-167">Next step</span></span>
<hr>

[<span data-ttu-id="e35aa-168">Krok 2 – přístupu a zabezpečení v šablonách Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e35aa-168">Step 2 - Access and Security in Azure Resource Manager Templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

