---
title: "aaaDeploying Windows výpočetní prostředky pomocí šablon Azure Resource Manageru | Microsoft Docs"
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
ms.openlocfilehash: dee228a492b08053713829e156e5b5ba304d7588
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="05870-103">Architektura aplikace pomocí šablony Azure Resource Manageru pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="05870-103">Application architecture with Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="05870-104">Při vývoji nasazení Azure Resource Manager, třeba požadavky na výpočetní služby a prostředky tooAzure toobe namapované.</span><span class="sxs-lookup"><span data-stu-id="05870-104">When developing an Azure Resource Manager deployment, compute requirements need toobe mapped tooAzure resources and services.</span></span> <span data-ttu-id="05870-105">Pokud aplikace obsahuje několik koncových bodů protokolu http, databáze a dat služby ukládání do mezipaměti, hello prostředky Azure, které jsou hostiteli každou z těchto součástí je potřeba toobe rationalized.</span><span class="sxs-lookup"><span data-stu-id="05870-105">If an application consists of several http endpoints, a database, and a data caching service, hello Azure resources that host each of these components needs toobe rationalized.</span></span> <span data-ttu-id="05870-106">Například hello ukázková Hudba úložiště aplikace zahrnuje webovou aplikaci, která je hostovaná na virtuálním počítači a databázi SQL, který je hostován v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="05870-106">For instance, hello sample Music Store application includes a web application that is hosted on a virtual machine, and a SQL database, which is hosted in Azure SQL database.</span></span> 

<span data-ttu-id="05870-107">Tento dokument podrobně popisuje, jak jsou nakonfigurované hello Hudba úložiště výpočetní prostředky v šablony Azure Resource Manageru ukázkový hello.</span><span class="sxs-lookup"><span data-stu-id="05870-107">This document details how hello Music Store compute resources are configured in hello sample Azure Resource Manager template.</span></span> <span data-ttu-id="05870-108">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="05870-108">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="05870-109">Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="05870-109">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="05870-110">úplnou šablonu Hello naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="05870-110">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="virtual-machine"></a><span data-ttu-id="05870-111">Virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="05870-111">Virtual Machine</span></span>
<span data-ttu-id="05870-112">Hello aplikaci Store Hudba zahrnuje webové aplikace, kde mohou zákazníci procházení a nákup Hudba.</span><span class="sxs-lookup"><span data-stu-id="05870-112">hello Music Store application includes a web application where customers can browse and purchase music.</span></span> <span data-ttu-id="05870-113">Existuje několik služeb Azure, které může být hostitelem webové aplikace v tomto příkladu se používá virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="05870-113">While there are several Azure services that can host web applications, for this example, a Virtual Machine is used.</span></span> <span data-ttu-id="05870-114">Pomocí šablony Hudba úložiště ukázkové hello nasazení virtuálního počítače, nainstalovat webový server a hello Hudba úložiště webu nainstalovaný a nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="05870-114">Using hello sample Music Store template, a virtual machine is deployed, a web server install, and hello Music Store website installed and configured.</span></span> <span data-ttu-id="05870-115">Pro hello zájmu v tomto článku je podrobně popsán pouze hello nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="05870-115">For hello sake of this article, only hello virtual machine deployment is detailed.</span></span> <span data-ttu-id="05870-116">Konfigurace Hello hello webového serveru a aplikace hello je podrobně popsaná v článku na novější.</span><span class="sxs-lookup"><span data-stu-id="05870-116">hello configuration of hello web server and hello application is detailed in a later article.</span></span>

<span data-ttu-id="05870-117">Virtuální počítač se dá přidat šablonu tooa pomocí hello Visual Studio přidat nový prostředek průvodce, nebo vložením platný kód JSON do šablony nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="05870-117">A virtual machine can be added tooa template using hello Visual Studio Add New Resource wizard, or by inserting valid JSON into hello deployment template.</span></span> <span data-ttu-id="05870-118">Při nasazování virtuálního počítače, je také potřeba několik související prostředky.</span><span class="sxs-lookup"><span data-stu-id="05870-118">When deploying a virtual machine, several related resources are also needed.</span></span> <span data-ttu-id="05870-119">Když pomocí sady Visual Studio toocreate hello šablony, vytvoří se pro vás tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="05870-119">If using Visual Studio toocreate hello template, these resources are created for you.</span></span> <span data-ttu-id="05870-120">Pokud ručně vytváření hello šablony, tyto prostředky je nutné toobe vložit a nakonfigurovaná.</span><span class="sxs-lookup"><span data-stu-id="05870-120">If manually constructing hello template, these resources need toobe inserted and configured.</span></span>

<span data-ttu-id="05870-121">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [virtuální počítač JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).</span><span class="sxs-lookup"><span data-stu-id="05870-121">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).</span></span>

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

<span data-ttu-id="05870-122">Po nasazení hello vlastnosti virtuálního počítače si můžete prohlédnout ve hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="05870-122">Once deployed, hello virtual machine properties can be seen in hello Azure portal.</span></span>

![Virtuální počítač](./media/dotnet-core-2-architecture/vm-win.png)

## <a name="storage-account"></a><span data-ttu-id="05870-124">Účet úložiště</span><span class="sxs-lookup"><span data-stu-id="05870-124">Storage Account</span></span>
<span data-ttu-id="05870-125">Účty úložiště mají mnoho možností úložiště a možnosti.</span><span class="sxs-lookup"><span data-stu-id="05870-125">Storage accounts have many storage options and capabilities.</span></span> <span data-ttu-id="05870-126">Hello kontextu virtuálními počítači Azure účet úložiště obsahuje virtuální pevné disky hello hello virtuálního počítače a jakýchkoli dalších datových disků.</span><span class="sxs-lookup"><span data-stu-id="05870-126">For hello context of Azure Virtual machines, a storage account holds hello virtual hard drives of hello virtual machine and any additional data disks.</span></span> <span data-ttu-id="05870-127">Ukázka Hello Hudba úložiště zahrnuje jednu úložiště účet toohold hello virtuálního pevného disku každého virtuálního počítače v hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="05870-127">hello Music Store sample includes one storage account toohold hello virtual hard drive of each virtual machine in hello deployment.</span></span> 

<span data-ttu-id="05870-128">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [účet úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).</span><span class="sxs-lookup"><span data-stu-id="05870-128">Follow this link toosee hello JSON sample within hello Resource Manager template – [Storage Account](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).</span></span>

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

<span data-ttu-id="05870-129">Účet úložiště je spojený s virtuální počítač v rámci deklaraci šablony Resource Manageru hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="05870-129">A storage account is associate with a virtual machine inside hello Resource Manager template declaration of hello virtual machine.</span></span> 

<span data-ttu-id="05870-130">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [přidružení virtuálního počítače a účet úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).</span><span class="sxs-lookup"><span data-stu-id="05870-130">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine and Storage Account association](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).</span></span>

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

<span data-ttu-id="05870-131">Po nasazení účtu úložiště hello lze zobrazit v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="05870-131">After deployment, hello storage account can be viewed in hello Azure portal.</span></span>

![Účet úložiště](./media/dotnet-core-2-architecture/storacct-win.png)

<span data-ttu-id="05870-133">Kliknutím na do kontejneru objektů blob účet úložiště hello, hello souboru virtuálního pevného disku pro každý virtuální počítač nasadit pomocí šablony hello je možné zobrazit.</span><span class="sxs-lookup"><span data-stu-id="05870-133">Clicking into hello storage account blob container, hello virtual hard drive file for each virtual machine deployed with hello template can be seen.</span></span>

![Virtuální pevné disky](./media/dotnet-core-2-architecture/vhd-win.png)

<span data-ttu-id="05870-135">Další informace o Azure Storage najdete v tématu [dokumentaci pro Azure Storage](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="05870-135">For more information on Azure Storage, see [Azure Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>

## <a name="virtual-network"></a><span data-ttu-id="05870-136">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="05870-136">Virtual Network</span></span>
<span data-ttu-id="05870-137">Pokud virtuální počítač vyžaduje interní sítě, jako je například hello možnost toocommunicate s jinými virtuálními počítači a prostředky Azure, je třeba virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="05870-137">If a virtual machine requires internal networking such as hello ability toocommunicate with other virtual machines and Azure resources, an Azure Virtual Network is required.</span></span>  <span data-ttu-id="05870-138">Virtuální sítě není zpřístupněte hello virtuálního počítače přes hello internet.</span><span class="sxs-lookup"><span data-stu-id="05870-138">A virtual network does not make hello virtual machine accessible over hello internet.</span></span> <span data-ttu-id="05870-139">Veřejné připojení vyžaduje veřejnou IP adresu, která je podrobně popsán dále v této série.</span><span class="sxs-lookup"><span data-stu-id="05870-139">Public connectivity requires a public IP address, which is detailed later in this series.</span></span>

<span data-ttu-id="05870-140">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [virtuální sítě a podsítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).</span><span class="sxs-lookup"><span data-stu-id="05870-140">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Network and Subnets](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).</span></span>

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

<span data-ttu-id="05870-141">Z hello portálu Azure virtuální sítě hello vypadá hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="05870-141">From hello Azure portal, hello virtual network looks like hello following image.</span></span> <span data-ttu-id="05870-142">Všimněte si, že jsou všechny virtuální počítače nasazené pomocí šablony hello toohello připojené virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="05870-142">Notice that all virtual machines deployed with hello template are attached toohello virtual network.</span></span>

![Virtual Network](./media/dotnet-core-2-architecture/vnet-win.png)

## <a name="network-interface"></a><span data-ttu-id="05870-144">Síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="05870-144">Network Interface</span></span>
 <span data-ttu-id="05870-145">Síťové rozhraní připojí virtuální síť tooa virtuálního počítače, konkrétně tooa podsíť, která byla definována ve virtuální síti hello.</span><span class="sxs-lookup"><span data-stu-id="05870-145">A network interface connects a virtual machine tooa virtual network, more specifically tooa subnet that has been defined in hello virtual network.</span></span> 

 <span data-ttu-id="05870-146">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [síťové rozhraní](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).</span><span class="sxs-lookup"><span data-stu-id="05870-146">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Interface](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).</span></span>

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

<span data-ttu-id="05870-147">Každý prostředek virtuálního počítače obsahuje profil sítě.</span><span class="sxs-lookup"><span data-stu-id="05870-147">Each virtual machine resource includes a network profile.</span></span> <span data-ttu-id="05870-148">Hello síťové rozhraní je spojeno s hello virtuálního počítače v tomto profilu.</span><span class="sxs-lookup"><span data-stu-id="05870-148">hello network interface is associated with hello virtual machine in this profile.</span></span>  

<span data-ttu-id="05870-149">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [síťového profilu virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).</span><span class="sxs-lookup"><span data-stu-id="05870-149">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine Network Profile](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).</span></span>

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

<span data-ttu-id="05870-150">Z hello portálu Azure vypadá hello síťové rozhraní hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="05870-150">From hello Azure portal, hello network interface looks like hello following image.</span></span> <span data-ttu-id="05870-151">Hello interní IP adresu a hello přidružení virtuálního počítače můžete zobrazit na hello síťového rozhraní prostředku.</span><span class="sxs-lookup"><span data-stu-id="05870-151">hello internal IP address and hello virtual machine association can be seen on hello network interface resource.</span></span>

![Síťové rozhraní](./media/dotnet-core-2-architecture/nic-win.png)

<span data-ttu-id="05870-153">Další informace o virtuálních sítí Azure najdete v tématu [dokumentace Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="05870-153">For more information on Azure Virtual Networks, see [Azure Virtual Network documentation](https://azure.microsoft.com/documentation/services/virtual-network/).</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="05870-154">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="05870-154">Azure SQL Database</span></span>
<span data-ttu-id="05870-155">Kromě toho tooa virtuální počítač hostování hello Hudba úložiště webu, Azure SQL Database je nasazené toohost hello Hudba úložiště databáze.</span><span class="sxs-lookup"><span data-stu-id="05870-155">In addition tooa virtual machine hosting hello Music Store website, an Azure SQL Database is deployed toohost hello Music Store database.</span></span> <span data-ttu-id="05870-156">Hello výhod používání Azure SQL Database v tomto poli je, že druhá sada virtuálních počítačů se nevyžaduje a škálování a dostupnosti je integrovaná do služby hello.</span><span class="sxs-lookup"><span data-stu-id="05870-156">hello advantage of using Azure SQL Database here is that a second set of virtual machines is not required, and scale and availability is built into hello service.</span></span>

<span data-ttu-id="05870-157">Azure SQL database lze přidat pomocí hello Visual Studio přidat nový prostředek průvodce, nebo vložením platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="05870-157">An Azure SQL database can be added using hello Visual Studio Add New Resource wizard, or by inserting valid JSON into a template.</span></span> <span data-ttu-id="05870-158">Hello prostředků systému SQL Server obsahuje uživatelské jméno a heslo, které jsou udělena práva správce v instanci SQL hello.</span><span class="sxs-lookup"><span data-stu-id="05870-158">hello SQL Server resource includes a user name and password that is granted administrative rights on hello SQL instance.</span></span> <span data-ttu-id="05870-159">Navíc je přidání brány firewall zdroje SQL.</span><span class="sxs-lookup"><span data-stu-id="05870-159">Also, a SQL firewall resource is added.</span></span> <span data-ttu-id="05870-160">Aplikace hostované v Azure jsou ve výchozím nastavení může tooconnect k instanci SQL hello.</span><span class="sxs-lookup"><span data-stu-id="05870-160">By default, applications hosted in Azure are able tooconnect with hello SQL instance.</span></span> <span data-ttu-id="05870-161">externí aplikace tooallow takové SQL Server Management studio tooconnect toohello instance SQL, brány firewall hello musí toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="05870-161">tooallow external application such a SQL Server Management studio tooconnect toohello SQL instance, hello firewall needs toobe configured.</span></span> <span data-ttu-id="05870-162">Hello výchozí konfiguraci pro hello zájmu hello Hudba úložiště ukázku, je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="05870-162">For hello sake of hello Music Store demo, hello default configuration is fine.</span></span> 

<span data-ttu-id="05870-163">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).</span><span class="sxs-lookup"><span data-stu-id="05870-163">Follow this link toosee hello JSON sample within hello Resource Manager template – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).</span></span>

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

<span data-ttu-id="05870-164">Zobrazení hello SQL server a databáze MusicStore, jak je vidět v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="05870-164">A view of hello SQL server and MusicStore database as seen in hello Azure portal.</span></span>

![SQL Server](./media/dotnet-core-2-architecture/sql-win.png)

<span data-ttu-id="05870-166">Další informace o nasazení služby Azure SQL Database, najdete v části [dokumentace Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="05870-166">For more information on deploying Azure SQL Database, see [Azure SQL Database documentation](https://azure.microsoft.com/documentation/services/sql-database/).</span></span>

## <a name="next-step"></a><span data-ttu-id="05870-167">Další krok</span><span class="sxs-lookup"><span data-stu-id="05870-167">Next step</span></span>
<hr>

[<span data-ttu-id="05870-168">Krok 2 – přístupu a zabezpečení v šablonách Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="05870-168">Step 2 - Access and Security in Azure Resource Manager Templates</span></span>](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

