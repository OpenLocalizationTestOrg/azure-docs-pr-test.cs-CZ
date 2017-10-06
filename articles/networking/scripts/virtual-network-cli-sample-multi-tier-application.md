---
title: "rozhraní příkazového řádku skriptu aaaAzure ukázkové – vytvoření sítě pro vícevrstvé aplikace | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuální sítě pro vícevrstvé aplikace."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: deeb3f459499cebd1b8ded6a299eb759d49cf08d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>Vytvoření sítě pro vícevrstvé aplikace

Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end. Podsíť front-end toohello provozu je omezená tooHTTP a SSH, při provozu toohello back-end podsíť je omezená tooMySQL, port 3306. Po spouštění skriptu hello budete mít dva virtuální počítače, jeden v jednotlivých podsítích, kterou můžete nasadit webový server a databáze MySQL software.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Ukázkový skript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě. Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](/cli/azure/network/vnet#create) | Vytvoří virtuální síť Azure a front-end podsítě. |
| [Vytvoření podsítě az sítě](/cli/azure/network/vnet/subnet#create) | Vytvoří podsíť back-end. |
| [Vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create) | Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu. |
| [Vytvoření az síťových adaptérů sítě](/cli/azure/network/nic#create) | Vytvoří rozhraní virtuální sítě a připojí je toohello virtuální sítě front-end a back-end podsítě. |
| [Vytvoření az sítě nsg](/cli/azure/network/nsg#create) | Vytvoří skupiny zabezpečení sítě (NSG), které jsou přidružené toohello front-end a back-end podsítě. |
| [Vytvoření pravidla nsg az sítě](/cli/azure/network/nsg/rule#create) |Vytvoří pravidla NSG, které povolí nebo blokuje specifické porty toospecific podsítě. |
| [Vytvoření virtuálního počítače az](/cli/azure/vm#create) | Vytvoří virtuální počítače a připojí tooeach síťový adaptér virtuálního počítače. Tento příkaz také určuje toouse bitové kopie virtuálního počítače hello a pověření pro správu. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).

Další síťové rozhraní příkazového řádku skriptu ukázky lze nalézt v hello [dokumentace přehled sítě Azure](../cli-samples.md)
