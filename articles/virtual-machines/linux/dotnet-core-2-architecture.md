---
title: "aaaDeploying Linux výpočetní prostředky pomocí šablon Azure Resource Manageru | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 1c4d419e-ba0e-45e4-a9dd-7ee9975a86f9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0bc26805860fed47923d46fc84f357060f68a951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-architecture-with-azure-resource-manager-templates-for-linux-vms"></a>Architektura aplikace pomocí šablony Azure Resource Manageru pro virtuální počítače s Linuxem

Při vývoji nasazení Azure Resource Manager, třeba požadavky na výpočetní služby a prostředky tooAzure toobe namapované. Pokud aplikace obsahuje několik koncových bodů protokolu http, databáze a dat služby ukládání do mezipaměti, hello prostředky Azure, které jsou hostiteli každou z těchto součástí je potřeba toobe rationalized. Například hello ukázková Hudba úložiště aplikace zahrnuje webovou aplikaci, která je hostovaná na virtuálním počítači a databázi SQL, který je hostován v databázi Azure SQL. 

Tento dokument podrobně popisuje, jak jsou nakonfigurované hello Hudba úložiště výpočetní prostředky v šablony Azure Resource Manageru ukázkový hello. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru. úplnou šablonu Hello naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux). 

## <a name="virtual-machine"></a>Virtuální počítač
Hello aplikaci Store Hudba zahrnuje webové aplikace, kde mohou zákazníci procházení a nákup Hudba. Existuje několik služeb Azure, které může být hostitelem webové aplikace v tomto příkladu se používá virtuální počítač. Pomocí šablony Hudba úložiště ukázkové hello nasazení virtuálního počítače, nainstalovat webový server a hello Hudba úložiště webu nainstalovaný a nakonfigurovaný. Pro hello zájmu v tomto článku je podrobně popsán pouze hello nasazení virtuálního počítače. Konfigurace Hello hello webového serveru a aplikace hello je podrobně popsaná v článku na novější.

Virtuální počítač se dá přidat šablonu tooa pomocí hello Visual Studio přidat nový prostředek průvodce, nebo vložením platný kód JSON do šablony nasazení hello. Při nasazování virtuálního počítače, je také potřeba několik související prostředky. Když pomocí sady Visual Studio toocreate hello šablony, vytvoří se pro vás tyto prostředky. Pokud ručně vytváření hello šablony, tyto prostředky je nutné toobe vložit a nakonfigurovaná.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [virtuální počítač JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295).

```json
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

Po nasazení hello vlastnosti virtuálního počítače si můžete prohlédnout ve hello portálu Azure.

![Virtuální počítač](./media/dotnet-core-2-architecture/vm.png)

## <a name="storage-account"></a>Účet úložiště
Účty úložiště mají mnoho možností úložiště a možnosti. Hello kontextu virtuálními počítači Azure účet úložiště obsahuje virtuální pevné disky hello hello virtuálního počítače a jakýchkoli dalších datových disků. Ukázka Hello Hudba úložiště zahrnuje jednu úložiště účet toohold hello virtuálního pevného disku každého virtuálního počítače v hello nasazení. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [účet úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109).

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

Účet úložiště je spojený s virtuální počítač v rámci deklaraci šablony Resource Manageru hello hello virtuálního počítače. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [přidružení virtuálního počítače a účet úložiště](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).

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

Po nasazení účtu úložiště hello lze zobrazit v hello portálu Azure.

![Účet úložiště](./media/dotnet-core-2-architecture/storacct.png)

Kliknutím na do kontejneru objektů blob účet úložiště hello, hello souboru virtuálního pevného disku pro každý virtuální počítač nasadit pomocí šablony hello je možné zobrazit.

![Virtuální pevné disky](./media/dotnet-core-2-architecture/vhd.png)

Další informace o Azure Storage najdete v tématu [dokumentaci pro Azure Storage](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtual Network
Pokud virtuální počítač vyžaduje interní sítě, jako je například hello možnost toocommunicate s jinými virtuálními počítači a prostředky Azure, je třeba virtuální síti Azure.  Virtuální sítě není zpřístupněte hello virtuálního počítače přes hello internet. Veřejné připojení vyžaduje veřejnou IP adresu, která je podrobně popsán dále v této série.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [virtuální sítě a podsítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136).

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

Z hello portálu Azure virtuální sítě hello vypadá hello následující obrázek. Všimněte si, že jsou všechny virtuální počítače nasazené pomocí šablony hello toohello připojené virtuální sítě.

![Virtual Network](./media/dotnet-core-2-architecture/vnet.png)

## <a name="network-interface"></a>Síťové rozhraní
 Síťové rozhraní připojí virtuální síť tooa virtuálního počítače, konkrétně tooa podsíť, která byla definována ve virtuální síti hello. 

 Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [síťové rozhraní](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceNamePrefix'), copyindex())]",
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
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'SSH-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig1",
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
              "id": "[concat(variables('lbID'),'/inboundNatRules/SSH-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Každý prostředek virtuálního počítače obsahuje profil sítě. Hello síťové rozhraní je spojeno s hello virtuálního počítače v tomto profilu.  

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [síťového profilu virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350).

```json
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

Z hello portálu Azure vypadá hello síťové rozhraní hello následující obrázek. Hello interní IP adresu a hello přidružení virtuálního počítače můžete zobrazit na hello síťového rozhraní prostředku.

![Síťové rozhraní](./media/dotnet-core-2-architecture/nic.png)

Další informace o virtuálních sítí Azure najdete v tématu [dokumentace Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure SQL Database
Kromě toho tooa virtuální počítač hostování hello Hudba úložiště webu, Azure SQL Database je nasazené toohost hello Hudba úložiště databáze. Hello výhod používání Azure SQL Database v tomto poli je, že druhá sada virtuálních počítačů se nevyžaduje a škálování a dostupnosti je integrovaná do služby hello.

Azure SQL database lze přidat pomocí hello Visual Studio přidat nový prostředek průvodce, nebo vložením platný kód JSON do šablony. Hello prostředků systému SQL Server obsahuje uživatelské jméno a heslo, které jsou udělena práva správce v instanci SQL hello. Navíc je přidání brány firewall zdroje SQL. Aplikace hostované v Azure jsou ve výchozím nastavení může tooconnect k instanci SQL hello. externí aplikace tooallow takové SQL Server Management studio tooconnect toohello instance SQL, brány firewall hello musí toobe nakonfigurované. Hello výchozí konfiguraci pro hello zájmu hello Hudba úložiště ukázku, je v pořádku. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401).

```json
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicStoreSqlName')]",
  "location": "[resourceGroup().location]",

  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('sqlAdminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicStoreSqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Zobrazení hello SQL server a databáze MusicStore, jak je vidět v hello portálu Azure.

![SQL Server](./media/dotnet-core-2-architecture/sql.png)

Další informace o nasazení služby Azure SQL Database, najdete v části [dokumentace Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Další krok
<hr>

[Krok 2 – přístupu a zabezpečení v šablonách Azure Resource Manageru](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

