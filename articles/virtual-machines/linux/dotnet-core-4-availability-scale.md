---
title: "Dostupnost a dosah v šablonách Azure Resource Manageru | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8fcfea79-f017-4658-8c51-74242fcfb7f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0c0250b8152ed31b9a5d8b42ae139c9b38da0984
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-linux-vms"></a>Dostupnost a dosah v šablonách Azure Resource Manageru pro virtuální počítače s Linuxem

Dostupnost a dosah viz provozu a schopnost potřeby. Pokud aplikace musí být 99,9 % času, musí mít architekturu, která umožňuje více souběžných výpočetní prostředky. Například místo jen jeden web, konfigurace s vyšší úrovní dostupnosti obsahuje více instancí stejného webu, vyrovnávání technologie před jejich. V této konfiguraci jednu instanci aplikace můžete ukončena kvůli údržbě, zatímco zbývající i nadále fungovat. Škálování na straně druhé odkazuje aplikace schopnost poskytovat vyžádání. S zatížení vyrovnáváním aplikace, přidání nebo odebrání instance z fondu umožňuje aplikaci škálovat podle požadavků.

Tento dokument podrobně popisuje konfiguraci nasazení ukázkové Hudba úložiště pro dostupnost a škálovatelnost. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru. Úplnou šablonu naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Skupina dostupnosti
Skupiny dostupnosti logicky platí virtuálních počítačů Azure pro fyzické hostitele a jiné infrastruktury komponenty, například napájení a fyzický síťový hardware. Skupiny dostupnosti Ujistěte se, že během údržby, selhání zařízení nebo jiné výpadek, ne všechny virtuální počítače, budou provedeny. Skupiny dostupnosti mohou být přidány do šablony Azure Resource Manager pomocí Visual Studio nový Průvodce přidáním prostředku nebo vkládání platný kód JSON do šablony.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [sadu dostupnosti](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Skupiny dostupnosti je deklarován jako vlastnost prostředku virtuálního počítače. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [dostupnost nastavená přidružení virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Dostupnost nastavit, jak je vidět na portálu Azure. Každý virtuální počítač a údaje o konfiguraci jsou podrobně popsané v tomto poli.

![Skupina dostupnosti](./media/dotnet-core-4-availability-scale/aset.png)

Podrobné informace o dostupnosti sadách najdete v tématu [Správa dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

## <a name="network-load-balancer"></a>Nástroj pro vyrovnávání zatížení sítě
Vzhledem k tomu, skupinu dostupnosti aplikace odolnost proti chybám, Vyrovnávání zatížení zpřístupní velký počet instancí aplikace na jednu síťovou adresu. Několik instancí aplikace může být hostovaný na velký počet virtuálních počítačů, každý z nich připojený ke službě Vyrovnávání zatížení. Protože přistupuje k aplikaci, nástroje pro vyrovnávání zatížení směrování příchozího požadavku napříč připojení členové. Nástroj pro vyrovnávání zatížení lze přidat pomocí Visual Studio nový Průvodce přidáním prostředku nebo vložením správně naformátovaný JSON prostředků do šablony Azure Resource Manager.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [nástroj pro vyrovnávání zatížení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Protože ukázkové aplikace je přístup k Internetu s veřejnou IP adresu, tato adresa je přidružena s nástrojem pro vyrovnávání zatížení. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [přidružení nástroj pro vyrovnávání zatížení sítě s veřejnou IP adresou](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Z portálu Azure zobrazuje přehled nástroje pro vyrovnávání zatížení sítě přidružení veřejnou IP adresu.

![Nástroj pro vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/nlb.png)

## <a name="load-balancer-rule"></a>Pravidlo Vyrovnávání zatížení.
Při použití služby Vyrovnávání zatížení, jsou nakonfigurované pravidla, které řídí, jak se data jsou rovnoměrně mezi předpokládaných prostředků. S ukázkovou aplikaci Store Hudba provoz dorazí na portu 80 veřejnou IP adresu a je distribuován do portu 80 všech virtuálních počítačů. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [pravidlo Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).

```json
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Zobrazení pravidlo Vyrovnávání zatížení sítě z portálu.

![Pravidlo Vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/lbrule.png)

## <a name="load-balancer-probe"></a>Sondu nástroje pro vyrovnávání zatížení
Vyrovnávání zatížení musí také monitorovat každý virtuální počítač tak, aby se zpracovat požadavky pouze na systémy. Toto monitorování probíhá podle konstantní zjišťování předem definovaný port. Hudba úložiště nasazení se konfiguruje testovat na zahrnuté všechny virtuální počítače, port 80. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [sběru dat služby Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).

```json
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Sondu nástroje pro vyrovnávání zatížení vidět na portálu Azure.

![Sondu nástroje pro vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/lbprobe.png)

## <a name="inbound-nat-rules"></a>Příchozí pravidla NAT
Pokud používáte nástroj pro vyrovnávání zatížení, pravidla potřebovat k uvedení do místo, které poskytují vyrovnáváním přístup bez zatížení pro každý virtuální počítač. Například při vytváření připojení SSH pomocí každý virtuální počítač, tato komunikace by neměl být skupinu s vyrovnáváním zatížení, místo by měl být nakonfigurovaný předem určené cesty. Předem určené cesty se konfiguruje pomocí prostředek příchozí pravidlo NAT. Pomocí tohoto prostředku, příchozí komunikaci lze mapovat na jednotlivé virtuální počítače. 

Se k aplikaci Store Hudba je port od 5000 namapovat port 22 na každém virtuálním počítači pro přístup SSH. `copyindex()` Funkce slouží k zvýšit příchozí port, tak, aby druhý virtuální počítač přijímá port pro příchozí 5001, třetí 5002 a tak dále. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [příchozí pravidla NAT](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Jedním z příkladů příchozí pravidlo NAT, jak je vidět na portálu Azure. Pro každý virtuální počítač v nasazení se vytvoří pravidlo SSH NAT.

![Příchozí pravidlo NAT](./media/dotnet-core-4-availability-scale/natrule.png)

Podrobnější informace o vyrovnávání zatížení sítě Azure, najdete v části [pro infrastrukturu Azure služby Vyrovnávání zatížení](../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="deploy-multiple-vms"></a>Nasazení více virtuálních počítačů
Nakonec sadu dostupnosti nebo nástroj pro vyrovnávání zatížení efektivně fungovat, jsou požadovány více virtuálních počítačů. Víc virtuálních počítačů můžete nasadit pomocí funkce kopírování šablony Azure Resource Manager. Pomocí funkce kopírování, není nutné definovat omezený počet virtuálních počítačů, místo tuto hodnotu můžete dynamicky poskytovat v době nasazení. Funkci kopírování spotřebuje počet instancí pro vytvoření a nasazení správný počet virtuálních počítačů a přidružených prostředků obslužné rutiny.

V šabloně Hudba úložiště ukázkové je definován parametr, který přebírá počet instancí. Toto číslo se používá v rámci šablony při vytváření virtuálních počítačů a související prostředky.

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

Na prostředek virtuálního počítače je kopírovací smyčkou zadaný název a číslo instance parametru použít k řízení počtu kopií výsledné.

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [funkci kopírování do virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300). 

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

Na aktuální iteraci funkce kopii lze přistupovat pomocí `copyIndex()` funkce. Hodnota indexu kopírování slouží k názvu virtuální počítače a další prostředky. Například pokud se nasazuje dvě instance virtuálního počítače, potřebují mít odlišné názvy. `copyIndex()` Funkci lze použít jako součást název virtuálního počítače vytvoří jedinečný název. Příkladem `copyindex()` funkce použité pro účely pojmenování si můžete prohlédnout ve prostředek virtuálního počítače. Zde, název počítače je tvořen `vmName` parametr a `copyIndex()` funkce. 

Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [funkci kopírování do indexu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 

```json
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

`copyIndex` Funkce se používá v šabloně Hudba úložiště ukázkové několikrát. Prostředky a funkce využívá `copyIndex` obsahovat vše specifické pro jednu instanci virtuálního počítače jako síťové rozhraní, pravidla nástroje pro vyrovnávání zatížení, a všechny závisí na funkce. 

Další informace o funkci kopírování najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](../../resource-group-create-multiple.md).

## <a name="next-step"></a>Další krok
<hr>

[Krok 4 – nasazení aplikace pomocí šablony Azure Resource Manageru](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

