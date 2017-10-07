---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vyrovnávat zatížení více webů s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vyrovnávat zatížení více webů toohello stejného virtuálního počítače"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 136da5d1783fb9f9dc87f1ffad8eec7b95c6bd7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a>Vyrovnávat zatížení více webů

Tento ukázkový skript vytvoří virtuální síť s dva virtuální počítače (VM), které jsou členy skupiny dostupnosti. Nástroj pro vyrovnávání zatížení přesměruje přenosy pro dva samostatné IP adres toohello dva virtuální počítače. Po spouštění skriptu hello můžete nasadit webový server softwaru toohello virtuální počítače a hostování více webů, každou s vlastní IP adresu.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků virtuální sítě, zatížení vyrovnávání a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](https://docs.microsoft.com/cli/azure/network/vnet#create) | Vytvoří virtuální síť Azure a podsíť. |
| [Vytvoření veřejné sítě az-ip](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS. |
| [Vytvoření sítě lb az](https://docs.microsoft.com/cli/azure/network/lb#create) | Vytvoří k nástroji pro vyrovnávání zatížení Azure. |
| [Vytvoření az sítě lb testu](https://docs.microsoft.com/cli/azure/network/lb/probe#create) | Vytvoří sondu nástroje pro vyrovnávání zatížení. Sondu nástroje pro vyrovnávání zatížení je použité toomonitor každý virtuální počítač v sadě nástroje pro vyrovnávání zatížení hello. Pokud žádné virtuální počítače bude nedostupné, není provoz směruje toohello virtuálních počítačů. |
| [Vytvořit pravidlo lb az sítě](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Vytvoří pravidlo Vyrovnávání zatížení. V této ukázce se vytvoří pravidlo pro port 80. HTTP přenos dorazí na hello nástroj pro vyrovnávání zatížení, je směrované tooport 80 jeden hello virtuálních počítačů v sadě nástroje pro vyrovnávání zatížení hello. |
| [Vytvoření az sítě lb ip front-endu-](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) | Vytvoření front-endovou IP adresy pro hello nástroj pro vyrovnávání zatížení. |
| [Vytvoření az sítě lb fond adres](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) | Vytvoří fond back-end adresy. |
| [Vytvoření az síťových adaptérů sítě](https://docs.microsoft.com/cli/azure/network/nic#create) | Vytvoří virtuální síťové karty a připojí jej toohello virtuální síť a podsíť. |
| [Vytvoření virtuálního počítače az sady dostupnosti.](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Vytvoří skupinu dostupnosti. Skupiny dostupnosti zajistěte doba provozu aplikací tak, že se hello virtuální počítače napříč fyzické prostředky tak, že pokud dojde k selhání, není provedena hello celou sadu. |
| [Seskupování síťových az ip-config vytvořit](https://docs.microsoft.com/cli/azure/network/nic/ip-config#create) | Vytvoří confiuration IP. Musíte mít hello Microsoft.Network/AllowMultipleIpConfigurationsPerNic funkce pro vaše předplatné povolený. Konfiguraci pouze jednoho může být určen jako hello primární konfiguraci IP adresy pro síťový adaptér, pomocí hello – příznak změnit na primární. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG. Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.  |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další síťové rozhraní příkazového řádku skriptu ukázky lze nalézt v hello [přehled sítě Azure dokumentaci](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
