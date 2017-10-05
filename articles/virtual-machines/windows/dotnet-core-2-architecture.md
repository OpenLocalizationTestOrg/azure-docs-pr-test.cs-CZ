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
# <a name="application-architecture-with-azure-resource-manager-templates-for-windows-vms"></a>Architektura aplikace pomocí šablony Azure Resource Manageru pro virtuální počítače Windows

Požadavky na výpočetní při vývoji nasazení Azure Resource Manager, musí být namapována k prostředků Azure a službám. Pokud aplikace obsahuje několik koncových bodů protokolu http, databáze a dat služby ukládání do mezipaměti, prostředky Azure tohoto hostitele každý všechny tyto komponenty musí být rationalized. Například ukázkovou aplikaci Hudba úložiště zahrnuje webovou aplikaci, která je hostovaná na virtuálním počítači a databázi SQL, který je hostován v databázi Azure SQL. 

Tento dokument podrobně popisuje, jak jsou nakonfigurované výpočetní prostředky úložiště Hudba v šablony Azure Resource Manageru ukázkový. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru. Úplnou šablonu naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="virtual-machine"></a>Virtuální počítač
Aplikaci Store Hudba zahrnuje webové aplikace, kde mohou zákazníci procházení a nákup Hudba. Existuje několik služeb Azure, které může být hostitelem webové aplikace v tomto příkladu se používá virtuální počítač. Pomocí šablony Hudba úložiště ukázkové nasazení virtuálního počítače, nainstalovat webový server a webu Hudba úložiště nainstalována a nakonfigurována. Z důvodu tohoto článku je podrobně popsán pouze nasazení virtuálního počítače. Konfigurace webového serveru a aplikace je podrobně popsaná v článku na novější.

Virtuální počítač můžete přidat do šablony pomocí průvodce Visual Studio, přidejte nový prostředek, nebo vložením platný kód JSON do šablony nasazení. Při nasazování virtuálního počítače, je také potřeba několik související prostředky. Když pomocí sady Visual Studio pro vytvoření šablony, vytvoří se pro vás tyto prostředky. Pokud ručně vytváření šablony, tyto prostředky je nutné vložit a nakonfigurovaná.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [virtuální počítač JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).

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

Po nasazení můžete zobrazit vlastnosti virtuálního počítače na portálu Azure.

![Virtuální počítač](./media/dotnet-core-2-architecture/vm-win.png)

## <a name="storage-account"></a>Účet úložiště
Účty úložiště mají mnoho možností úložiště a možnosti. V kontextu virtuálními počítači Azure účet úložiště obsahuje virtuální pevné disky virtuálního počítače a jakýchkoli dalších datových disků. Ukázky hudby úložiště obsahuje jeden účet úložiště pro uložení virtuálního pevného disku každého virtuálního počítače v nasazení. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [účet úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).

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

Účet úložiště je spojený s virtuální počítač v rámci deklaraci šablony správce prostředků virtuálního počítače. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [přidružení virtuálního počítače a účet úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).

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

Po nasazení účtu úložiště lze zobrazit na portálu Azure.

![Účet úložiště](./media/dotnet-core-2-architecture/storacct-win.png)

Kliknutím na do kontejneru objektů blob účet úložiště, lze je zobrazit souboru virtuálního pevného disku pro každý virtuální počítač nasadit pomocí šablony.

![Virtuální pevné disky](./media/dotnet-core-2-architecture/vhd-win.png)

Další informace o Azure Storage najdete v tématu [dokumentaci pro Azure Storage](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtual Network
Pokud virtuální počítač vyžaduje interní sítě, jako je například schopnost komunikovat s jinými virtuálními počítači a prostředky Azure, je třeba virtuální síti Azure.  Virtuální sítě není zpřístupněte virtuálního počítače přes internet. Veřejné připojení vyžaduje veřejnou IP adresu, která je podrobně popsán dále v této série.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [virtuální sítě a podsítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).

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

Z portálu Azure virtuální sítě vypadá jako na následujícím obrázku. Všimněte si, že všechny virtuální počítače nasazené pomocí šablony jsou připojené k virtuální síti.

![Virtual Network](./media/dotnet-core-2-architecture/vnet-win.png)

## <a name="network-interface"></a>Síťové rozhraní
 Síťové rozhraní připojí virtuální počítač k virtuální síti, konkrétně k podsíti, která byla definována ve virtuální síti. 

 Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [síťové rozhraní](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).

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

Každý prostředek virtuálního počítače obsahuje profil sítě. Síťové rozhraní je přidružený k virtuálnímu počítači v tomto profilu.  

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [síťového profilu virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Z portálu Azure síťové rozhraní vypadá jako na následujícím obrázku. Interní IP adresu a přidružení virtuálního počítače můžete zobrazit na prostředek rozhraní sítě.

![Síťové rozhraní](./media/dotnet-core-2-architecture/nic-win.png)

Další informace o virtuálních sítí Azure najdete v tématu [dokumentace Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL Database
Kromě hostování webu Hudba úložiště virtuálního počítače je nasazený databáze SQL Azure k hostování databáze hudba úložiště. Výhodou použití Azure SQL Database v tomto poli je, že druhá sada virtuálních počítačů se nevyžaduje a škálování a dostupnosti je integrovaná do služby.

Azure SQL database lze přidat pomocí Visual Studio přidat nový prostředek průvodce, nebo vložením platný kód JSON do šablony. Prostředek systému SQL Server obsahuje uživatelské jméno a heslo, které jsou udělena práva správce v instanci SQL. Navíc je přidání brány firewall zdroje SQL. Aplikace hostované v Azure jsou ve výchozím nastavení, moct připojit k instanci SQL. Takové SQL Server Management studio se připojit k instanci serveru SQL, brány firewall umožňující externí aplikace je potřeba nakonfigurovat. Výchozí konfigurace je z důvodu ukázku Hudba úložiště v pořádku. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).

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

Zobrazení systému SQL server a databáze MusicStore, jak je vidět na portálu Azure.

![SQL Server](./media/dotnet-core-2-architecture/sql-win.png)

Další informace o nasazení služby Azure SQL Database, najdete v části [dokumentace Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Další krok
<hr>

[Krok 2 – přístupu a zabezpečení v šablonách Azure Resource Manageru](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

