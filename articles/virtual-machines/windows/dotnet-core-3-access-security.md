---
title: "aaaAccess a zabezpečení v šablonách Azure pro virtuální počítače Windows | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: e671fc45-5e4d-40fd-aac5-290892713cc0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b8227ae745b3b0a22d136e98d18479f8b1504c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-windows-vms"></a>Přístup a zabezpečení v šablonách Azure Resource Manageru pro virtuální počítače Windows

Aplikace hostované v Azure pravděpodobně nutné toobe přístup přes hello internet nebo síť VPN nebo připojení Express Route s Azure. S ukázkou aplikace hello Hudba úložiště, je k dispozici na webu hello hello internet s veřejnou IP adresu. S přístupem k vytvoření by měly být zabezpečeny připojení toohello aplikací a přístupu toohello prostředky virtuálního počítače sami. Tento přístup zabezpečení je součástí skupinu zabezpečení sítě. 

Tento dokument podrobně popisuje, jak jsou zabezpečená hello aplikaci Store Hudba v šablony Azure Resource Manageru ukázkový hello. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru. úplnou šablonu Hello naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="public-ip-address"></a>Veřejná IP adresa
tooprovide veřejný přístup tooan prostředků Azure, prostředek veřejné IP adresy lze použít. Veřejnou IP adresu můžete nakonfigurovat statickou nebo dynamickou adresu IP. Pokud se používá s dynamickou adresou a hello virtuální počítač je zastaven a navrácena, odeberou se hello adresy. Když se počítač hello znovu spustí, může přiřadit jinou veřejnou IP adresu. tooprevent na IP adresu z změna, můžete použít vyhrazenou IP adresu. 

Šablony Azure Resource Manageru tooan pomocí hello Visual Studio nový Průvodce přidáním prostředku, lze přidat na veřejnou IP adresu nebo vložením platný kód JSON do šablony. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [veřejnou IP adresu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIpAddressName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "public-ip"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Veřejnou IP adresu lze přidružit k virtuální síťový adaptér, nebo nástroj pro vyrovnávání zatížení. V tomto příkladu protože hello Hudba úložiště webu vyrovnáváno zatížení napříč několika virtuálních počítačů, je hello veřejnou IP adresu připojené toohello Vyrovnávání zatížení.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [přidružení veřejné IP adresy pomocí nástroje pro vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Hello veřejnou IP adresu jako z pohledu hello portálu Azure. Všimněte si, že hello veřejná IP adresa je přidružená tooa nástroj pro vyrovnávání zatížení a není virtuální počítač. Nástroje pro vyrovnávání zatížení sítě jsou podrobně popsané v dokumentu další hello této řady.

![Veřejná IP adresa](./media/dotnet-core-3-access-security/pubip-win.png)

Další informace o Azure veřejné IP adresy najdete v tématu [IP adresách v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Skupina zabezpečení sítě
Jakmile přístup zavedených tooAzure prostředky, už musí být tento přístup omezený. Pro virtuální počítače Azure se provádí zabezpečený přístup pomocí skupiny zabezpečení sítě. S ukázkou aplikace hello úložiště Hudba všechny přístup toohello virtuální počítač je omezena kromě přes port 80 pro přístup protokolu http a port 3389 pro přístup k protokolu RDP. Šablony Azure Resource Manageru tooan pomocí hello Visual Studio nový Průvodce přidáním prostředku, lze přidat skupinu zabezpečení sítě, nebo vložením platný kód JSON do šablony.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [skupinu zabezpečení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57).

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('networkSecurityGroup')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-security-group"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
},
```

V tomto příkladu je skupina zabezpečení sítě hello spojený s objektu podsítě hello deklarované v prostředku hello virtuální sítě. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [přidružení skupiny zabezpečení sítě pomocí virtuální sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143).

```json
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
```

Zde je, jakou skupinu zabezpečení sítě hello vypadá z hello portálu Azure. Všimněte si, že skupina NSG můžete přidružit podsítě a / nebo síťové rozhraní. V takovém případě je hello NSG přidružené tooa podsíť. V této konfiguraci hello příchozí pravidla použít tooall virtuální počítače připojené toohello podsítě.

![Skupina zabezpečení sítě](./media/dotnet-core-3-access-security/nsg-win.png)

Podrobné informace na skupiny zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Další krok
<hr>

[Krok 3 – dostupnost a dosah v šablonách Azure Resource Manageru](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

