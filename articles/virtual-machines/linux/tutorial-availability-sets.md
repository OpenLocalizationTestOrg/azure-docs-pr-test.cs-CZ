---
title: "aaaAvailability nastaví kurz pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o hello skupiny dostupnosti pro virtuální počítače s Linuxem v Azure."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2a91e4a6057180035ec51410d9fffccaca343758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a>Jak nastaví toouse dostupnosti


V tomto kurzu se dozvíte, jak tooincrease hello dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti. Skupiny dostupnosti Ujistěte se, že hello virtuálních počítačů nasadit v Azure jsou distribuovány na více clusterů izolované hardwaru. To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, bude mít vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstanou dostupné a funkční z hlediska hello zákazníků použití.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače v nastavení dostupnosti
> * Zkontrolujte dostupné velikosti virtuálních počítačů


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Přehled sady dostupnosti

Skupiny dostupnosti je logické seskupení schopností, který můžete použít v Azure tooensure že hello prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure. Azure zajistí, že hello virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače. To zajistí, že hello události hardwaru nebo softwaru Azure chyby, bude mít vliv jenom podmnožinu virtuální počítače a aplikace celkový zůstane nahoru a bude pokračovat zákazníky k dispozici tooyour toobe. Pomocí nastavení dostupnosti je tooleverage nezbytné funkce, pokud chcete toobuild spolehlivé cloudové řešení.

Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze. S Azure, je vhodnější toodefine dvě sady dostupnosti před nasazením virtuálních počítačů: sadu jeden dostupnosti pro vrstvu "web" hello a jednu sadu dostupnosti pro vrstvu "databáze" hello. Při vytváření nového virtuálního počítače můžete určit hello dostupnosti jako virtuální počítač parametr toohello az vytvoření příkazu a Azure automaticky zajistí, že hello virtuálních počítačů, které vytvoříte v rámci hello k dispozici sady jsou izolovány napříč několika prostředcích fyzický hardware. To znamená, že pokud hello fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že hello ostatní instance webového serveru a databáze virtuální počítače zůstanou spuštěné bez problémů vzhledem k tomu, že jsou na jiný hardware.

Pokud chcete toodeploy spolehlivé virtuálních počítačů na základě řešení v rámci Azure byste měli vždycky používat skupiny dostupnosti.


## <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti

Můžete vytvořit pomocí sadu dostupnosti [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create). V tomto příkladu jsme nastavit počet aktualizací a chyby domén v obou hello *2* pro sadu s názvem dostupnosti hello *myAvailabilitySet* v hello *myResourceGroupAvailability*skupinu prostředků.

Vytvořte skupinu prostředků.

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Skupiny dostupnosti povolit tooisolate prostředky napříč "domén selhání" a "update domény". A **doména selhání** představuje kolekci izolované serveru + síť + úložiště prostředků. V předchozím příkladu hello jsme znamenat, že má být, že naše dostupnosti nastaven toobe rozložené mezi aspoň dva domén selhání při nasazení naše virtuálních počítačů. Také znamenat, že chceme, aby naše dostupnosti distribuované napříč dvěma **aktualizovat domény**.  Dvě domény aktualizace zajistěte, aby byly při Azure provádí aktualizace softwaru naše prostředky virtuálních počítačů izolované, brání veškerý software hello spuštění pod naše virtuálních počítačů z aktualizovaných v hello stejnou dobu.

## <a name="configure-virtual-network"></a>Konfigurace virtuální sítě
Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte hello podpora prostředky virtuální sítě. Další informace o virtuálních sítích najdete v tématu hello [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.

### <a name="create-network-resources"></a>Vytvoření síťové prostředky
Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Hello následující příklad vytvoří virtuální síť s názvem *myVnet* s podsítí s názvem *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
Virtuální síťové adaptéry jsou vytvořeny pomocí [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Hello následující příklad vytvoří tři virtuálních síťových karet. (Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro aplikace v rámci hello následující kroky). Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a přidat je nástroj pro vyrovnávání zatížení toohello:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupAvailability \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-vms-inside-an-availability-set"></a>Vytvořit virtuální počítače uvnitř skupiny dostupnosti

Virtuální počítače musí být vytvořen v rámci toomake sadu dostupnosti hello se, že jsou správně distribuovány mezi hello hardwaru. Nelze přidat sadu po vytvoření existující tooan dostupnosti virtuálních počítačů. 

Při vytváření virtuálního počítače pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) zadejte hello dostupnosti pomocí hello `--availability-set` parametr toospecify hello název sady dostupnosti hello.

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --nics myNic$i \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

Nyní je k dispozici dva virtuální počítače v rámci naší sady nově vytvořený dostupnosti. Protože jsou v hello stejné nastavení dostupnosti, Azure bude zajištěno, že hello virtuální počítače a všechny jejich prostředky (včetně datových disků) jsou rozmístěny v izolované fyzický hardware. Toto rozdělení pomáhá zajistit mnohem vyšší dostupnosti naše celkového řešení virtuálních počítačů.

Jednou z věcí, které se můžete setkat při přidávání virtuálních počítačů je, že konkrétní velikost virtuálního počítače již není k dispozici toouse v rámci vaší sady dostupnosti. Tento problém může dojít, když už není dostatek tooadd kapacity, které při zachování sada dostupnosti hello izolace pravidla hello vynucuje. Můžete zkontrolovat toosee jaké velikosti virtuálních počítačů jsou k dispozici toouse v rámci existující dostupnosti nastavit pomocí hello `--availability-set list-sizes` parametr.

## <a name="check-for-available-vm-sizes"></a>Zkontrolujte dostupné velikosti virtuálních počítačů 

Můžete přidat další virtuální počítače toohello dostupnosti později, ale potřebujete tooknow jaké velikosti virtuálních počítačů jsou k dispozici na hello hardwaru. Použití [seznam velikosti az virtuálních počítačů sady dostupnosti](/cli/azure/availability-set#list-sizes) toolist všech dostupných velikostí hello na hello hardwaru clusteru pro sadu dostupnosti hello.

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače v nastavení dostupnosti
> * Zkontrolujte dostupné velikosti virtuálních počítačů

Posunutí další kurz toolearn toohello o sady škálování virtuálního počítače.

> [!div class="nextstepaction"]
> [Vytvoření škálovací sady virtuálních počítačů](tutorial-create-vmss.md)

