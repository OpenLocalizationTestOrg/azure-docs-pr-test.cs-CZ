---
title: "Přístup a zabezpečení v šablonách Azure pro virtuální počítače Windows | Microsoft Docs"
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
ms.openlocfilehash: ad1b5c4763cf56f681a50bb1bccc825311bbfdf5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-windows-vms"></a>Přístup a zabezpečení v šablonách Azure Resource Manageru pro virtuální počítače Windows

Aplikace hostované v Azure pravděpodobně muset být přístup přes internet nebo síť VPN nebo připojení Express Route s Azure. Pomocí aplikace Ukázky hudby úložiště je webový server k dispozici na Internetu s veřejnou IP adresu. S přístupem k vytvoření by měly být zabezpečeny připojení k aplikaci a přístup k prostředkům virtuálního počítače, sami. Tento přístup zabezpečení je součástí skupinu zabezpečení sítě. 

Tento dokument podrobně popisuje, jak jsou zabezpečená aplikaci Store Hudba v ukázkové šablony Azure Resource Manager. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru. Úplnou šablonu naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="public-ip-address"></a>Veřejná IP adresa
Pokud chcete zadat veřejný přístup k prostředku Azure, lze prostředek veřejné IP adresy. Veřejnou IP adresu můžete nakonfigurovat statickou nebo dynamickou adresu IP. Pokud se používá s dynamickou adresou a virtuální počítač je zastaven a navrácena, odeberou se adresy. Když se počítač spustí znovu, může přiřadit jinou veřejnou IP adresu. Abyste zabránili změna IP adresy, lze použít vyhrazenou IP adresu. 

Veřejnou IP adresu lze přidat do šablony Azure Resource Manager pomocí Visual Studio nový Průvodce přidáním prostředku nebo vložením platný kód JSON do šablony. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [veřejnou IP adresu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).

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

Veřejnou IP adresu lze přidružit k virtuální síťový adaptér, nebo nástroj pro vyrovnávání zatížení. V tomto příkladu protože web Hudba úložiště je vyrovnáváno zatížení napříč několika virtuálních počítačů, je veřejná IP adresa připojený k vyrovnávání zatížení.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [přidružení veřejné IP adresy pomocí nástroje pro vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

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

Veřejnou IP adresu jako zaznamenané z portálu Azure. Všimněte si, že veřejná IP adresa je přidružená k vyrovnávání zatížení a není virtuální počítač. Nástroje pro vyrovnávání zatížení sítě jsou podrobně popsané v další dokument Tato řada.

![Veřejná IP adresa](./media/dotnet-core-3-access-security/pubip-win.png)

Další informace o Azure veřejné IP adresy najdete v tématu [IP adresách v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Skupina zabezpečení sítě
Po vytvoření přístup k prostředkům Azure tento přístup by měla být omezená. Pro virtuální počítače Azure se provádí zabezpečený přístup pomocí skupiny zabezpečení sítě. Pomocí aplikace Ukázky hudby úložiště je veškerý přístup k virtuálnímu počítači s omezeným přístupem, s výjimkou přes port 80 pro přístup protokolu http a port 3389 pro přístup k protokolu RDP. Skupina zabezpečení sítě lze přidat do šablony Azure Resource Manager pomocí Visual Studio nový Průvodce přidáním prostředku nebo vložením platný kód JSON do šablony.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [skupinu zabezpečení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57).

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

Skupina zabezpečení sítě v tomto příkladu je spojený s objektu podsítě, které jsou deklarované v rámci virtuální sítě prostředku. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [přidružení skupiny zabezpečení sítě pomocí virtuální sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143).

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

Zde je, jak skupinu zabezpečení sítě vypadá z portálu Azure. Všimněte si, že skupina NSG můžete přidružit podsítě a / nebo síťové rozhraní. V takovém případě je přidružena k podsíti NSG. V této konfiguraci příchozích pravidel platí pro všechny virtuální počítače připojené k podsíti.

![Skupina zabezpečení sítě](./media/dotnet-core-3-access-security/nsg-win.png)

Podrobné informace na skupiny zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Další krok
<hr>

[Krok 3 – dostupnost a dosah v šablonách Azure Resource Manageru](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

