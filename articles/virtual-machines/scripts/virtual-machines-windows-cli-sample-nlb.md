---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s Windows Server 2016 s vyrovnáváním zatížení sítě | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Windows Server 2016 s vyrovnáváním zatížení sítě"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: aaaac0c2cc32ce0cac21417926399d848bd6fa09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a>Vyrovnávání zatížení přenosů mezi vysoce dostupné virtuální počítače

Tento ukázkový skript vytvoří vše potřebné vyrovnáváním toorun několik Ubuntu virtuální počítače nakonfigurované v s vysokou dostupností a načtení konfigurace. Po spouštění skriptu hello, budete mít tři virtuální počítače připojené k tooan Azure skupiny dostupnosti a je přístupný prostřednictvím Vyrovnávání zatížení Azure.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nlb/create-windows-vm-nlb.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, skupinu dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](https://docs.microsoft.com/cli/azure/network/vnet#create) | Vytvoří virtuální síť Azure a podsíť. |
| [Vytvoření veřejné sítě az-ip](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS. |
| [Vytvoření sítě lb az](https://docs.microsoft.com/cli/azure/network/lb#create) | Vytvoří k síť Azure pro vyrovnávání zatížení (sítě NLB). |
| [Vytvoření az sítě lb testu](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Vytvoří kontrolu Vyrovnávání zatížení sítě. Test Vyrovnávání zatížení sítě je použité toomonitor každý virtuální počítač v sadě hello Vyrovnávání zatížení sítě. Pokud žádné virtuální počítače bude nedostupné, není provoz směruje toohello virtuálních počítačů. |
| [Vytvořit pravidlo lb az sítě](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Vytvoří pravidlo Vyrovnávání zatížení sítě. V této ukázce se vytvoří pravidlo pro port 80. HTTP přenos dorazí na hello Vyrovnávání zatížení sítě, je směrované tooport 80 jeden hello virtuálních počítačů v sadě hello Vyrovnávání zatížení sítě. |
| [Vytvoření az sítě lb příchozí--pravidlo nat](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) | Vytvoří pravidlo Vyrovnávání zatížení sítě překlad adres (NAT).  Pravidla NAT namapovat port hello port tooa Vyrovnávání zatížení sítě na virtuálním počítači. V této ukázce se vytvoří pravidlo NAT pro SSH provoz tooeach virtuálních počítačů v sadě hello Vyrovnávání zatížení sítě.  |
| [Vytvoření az sítě nsg](https://docs.microsoft.com/cli/azure/network/nsg#create) | Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi hello Internetu a hello virtuálního počítače. |
| [Vytvoření pravidla nsg az sítě](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Vytvoří tooallow pravidla NSG příchozí přenosy. V této ukázce je otevřen port 22 pro SSH provoz. |
| [Vytvoření az síťových adaptérů sítě](https://docs.microsoft.com/cli/azure/network/nic#create) | Vytvoří virtuální síťové karty a připojí jej toohello virtuální sítě, podsítě a NSG. |
| [Vytvoření virtuálního počítače az sady dostupnosti.](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Vytvoří skupinu dostupnosti. Skupiny dostupnosti zajistěte doba provozu aplikací tak, že se hello virtuální počítače napříč fyzické prostředky tak, že pokud dojde k selhání, není provedena hello celou sadu. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG. Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.  |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
