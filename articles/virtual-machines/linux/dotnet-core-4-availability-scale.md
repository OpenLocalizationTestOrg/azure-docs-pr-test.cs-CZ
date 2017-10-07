---
title: "aaaAvailability a škálování v šablonách Azure Resource Manageru | Microsoft Docs"
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
ms.openlocfilehash: 6f830ca0a64e6b65859312bdf31dc0af59e2b978
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-linux-vms"></a>Dostupnost a dosah v šablonách Azure Resource Manageru pro virtuální počítače s Linuxem

Dostupnost a dosah naleznete toouptime a hello možnost toomeet poptávky. Pokud aplikace musí být 99,9 % času hello, musí toohave architekturu, která umožňuje více souběžných výpočetní prostředky. Například místo jen jeden web, zahrnuje konfigurace s vyšší úrovní dostupnosti více instancí hello stejné vyrovnávání technologie před jejich lokality. V této konfiguraci jednu instanci aplikace hello může být kvůli údržbě vypnutá, zatímco hello zbývající dále toofunction. Škálování na hello druhé straně odkazuje tooan aplikace možnost tooserve vyžádání. S zatížení vyrovnáváním aplikace, přidání nebo odebrání instance z fondu hello umožňuje vyžádání toomeet tooscale aplikace.

Tento dokument podrobně popisuje, jak je hello Hudba úložiště Ukázka nasazení nakonfigurované pro dostupnost a škálovatelnost. Jsou vyznačené všechny závislosti a unikátní konfiguraci. Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru. úplnou šablonu Hello naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Skupina dostupnosti
Skupiny dostupnosti logicky platí virtuálních počítačů Azure pro fyzické hostitele a jiné infrastruktury komponenty, například napájení a fyzický síťový hardware. Skupiny dostupnosti Ujistěte se, že během údržby, selhání zařízení nebo jiné výpadek, ne všechny virtuální počítače, budou provedeny. Skupiny dostupnosti mohou být přidány tooan šablony Azure Resource Manageru pomocí hello Visual Studio nový Průvodce přidáním prostředku nebo vkládání platný kód JSON do šablony.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [sadu dostupnosti](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).

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

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [dostupnost nastavená přidružení virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
dostupnost Hello nastavit, jak je vidět z hello portálu Azure. Každý virtuální počítač a podrobnosti o konfiguraci hello jsou podrobně popsané v tomto poli.

![Skupina dostupnosti](./media/dotnet-core-4-availability-scale/aset.png)

Podrobné informace o dostupnosti sadách najdete v tématu [Správa dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

## <a name="network-load-balancer"></a>Nástroj pro vyrovnávání zatížení sítě
Vzhledem k tomu, skupinu dostupnosti aplikace odolnost proti chybám, Vyrovnávání zatížení zpřístupní velký počet instancí aplikace hello na jednu síťovou adresu. Několik instancí aplikace může být hostovaný na velký počet virtuálních počítačů, každý z nich připojený tooa nástroj pro vyrovnávání zatížení. Jako aplikace hello přistupuje, směrování pro vyrovnávání zatížení hello hello příchozí žádost mezi členy hello připojen. Nástroj pro vyrovnávání zatížení lze přidat pomocí hello Visual Studio nový Průvodce přidáním prostředku nebo vložením správně naformátovaný JSON prostředků do šablony Azure Resource Manager hello.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [nástroj pro vyrovnávání zatížení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

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

Protože hello vzorová aplikace je zveřejněné toohello internet s veřejnou IP adresu, tato adresa je přidružená Vyrovnávání zatížení hello. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [přidružení nástroj pro vyrovnávání zatížení sítě s veřejnou IP adresou](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

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

Z hello portálu Azure hello přehled nástroje pro vyrovnávání zatížení sítě zobrazuje hello přidružení hello veřejnou IP adresu.

![Nástroj pro vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/nlb.png)

## <a name="load-balancer-rule"></a>Pravidlo Vyrovnávání zatížení.
Při použití služby Vyrovnávání zatížení, konfigurovaná pravidla, které řídí, jak se data jsou rovnoměrně mezi prostředky hello určený. S hello ukázkovou aplikaci Store Hudba provoz dorazí na portu 80 hello veřejné IP adresy a je distribuován do portu 80 všech virtuálních počítačů. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [pravidlo Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).

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

Zobrazení hello sítě pravidlo nástroje pro vyrovnávání zatížení z portálu hello.

![Pravidlo Vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/lbrule.png)

## <a name="load-balancer-probe"></a>Sondu nástroje pro vyrovnávání zatížení
Nástroj pro vyrovnávání zatížení Hello také musí toomonitor každý virtuální počítač tak, aby se zpracovat požadavky jenom toorunning systémy. Toto monitorování probíhá podle konstantní zjišťování předem definovaný port. nasazení Hudba úložiště Hello je nakonfigurované tooprobe portu 80 pro všechny zahrnuté virtuálních počítačů. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [sběru dat služby Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).

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

sondu nástroje pro vyrovnávání zatížení Hello vidět z hello portálu Azure.

![Sondu nástroje pro vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/lbprobe.png)

## <a name="inbound-nat-rules"></a>Příchozí pravidla NAT
Pokud používáte nástroj pro vyrovnávání zatížení, třeba pravidla toobe zavést, které poskytují tooeach vyrovnáváním přístup bez zatížení virtuálního počítače. Například při vytváření připojení SSH pomocí každý virtuální počítač, tato komunikace by neměl být skupinu s vyrovnáváním zatížení, místo by měl být nakonfigurovaný předem určené cesty. Předem určené cesty se konfiguruje pomocí prostředek příchozí pravidlo NAT. Pomocí tohoto prostředku, příchozí komunikace může být namapované tooindividual virtuálních počítačů. 

S hello aplikaci Store Hudba je port od 5000 namapované tooport 22 na každém virtuálním počítači pro přístup SSH. Hello `copyindex()` funkce je použité tooincrement hello příchozí port, tak, aby se hello druhý virtuální počítač přijímá port pro příchozí 5001, hello třetí 5002 a tak dále. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [příchozí pravidla NAT](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

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

Jedním z příkladů příchozí pravidlo NAT jako hello zaznamenané v portálu Azure. Pro každý virtuální počítač v hello nasazení se vytvoří pravidlo SSH NAT.

![Příchozí pravidlo NAT](./media/dotnet-core-4-availability-scale/natrule.png)

Podrobnější informace o hello Vyrovnávání zatížení sítě Azure, najdete v části [pro infrastrukturu Azure služby Vyrovnávání zatížení](../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="deploy-multiple-vms"></a>Nasazení více virtuálních počítačů
Nakonec pro funkci tooeffectively sady dostupnosti nebo služby Vyrovnávání zatížení, jsou požadovány více virtuálních počítačů. Víc virtuálních počítačů můžete nasadit pomocí funkce kopírování šablony Azure Resource Manager hello. Pomocí funkce kopírování hello, není nutné toodefine omezený počet virtuálních počítačů, místo tuto hodnotu můžete dynamicky poskytovat hello během nasazení. funkci kopírování do Hello spotřebuje hello počet instancí toocreated a obslužných rutin nasazení hello správný počet virtuálních počítačů a přidružených prostředků.

V šabloně Hudba úložiště ukázkový text hello je definován parametr, který přebírá počet instancí. Toto číslo se používá v rámci hello šablonu při vytváření virtuálních počítačů a související prostředky.

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances toobe created behind load balancer."
  }
}
```

Na hello prostředek virtuálního počítače hello kopírovací smyčkou je zadaný název a používá toocontrol hello počet kopií výsledné hello počet instancí parametru.

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [funkci kopírování do virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300). 

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

aktuální iteraci Hello funkci kopírování do hello lze přistupovat pomocí hello `copyIndex()` funkce. Hodnota Hello hello kopie index funkce může být použité tooname virtuální počítače a další prostředky. Například pokud se nasazuje dvě instance virtuálního počítače, potřebují mít odlišné názvy. Hello `copyIndex()` funkci lze použít jako součást hello virtuálního počítače názvu toocreate jedinečný název. Příkladem hello `copyindex()` funkce použité pro účely pojmenování si můžete prohlédnout ve hello prostředek virtuálního počítače. Zde je název počítače hello zřetězení hello `vmName` parametr a hello `copyIndex()` funkce. 

Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [funkci kopírování do indexu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 

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

Hello `copyIndex` funkce je používán několikrát hello Hudba úložiště vzorové šablony. Prostředky a funkce využívá `copyIndex` zahrnout cokoli konkrétní tooa jednu instanci hello virtuálního počítače jako síťové rozhraní, pravidla nástroje pro vyrovnávání zatížení, a všechny závisí na funkce. 

Další informace o funkci kopírování do hello najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](../../resource-group-create-multiple.md).

## <a name="next-step"></a>Další krok
<hr>

[Krok 4 – nasazení aplikace pomocí šablony Azure Resource Manageru](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

