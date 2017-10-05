---
title: "Kurz sady dostupnosti pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Další informace o skupiny dostupnosti pro virtuální počítače s Linuxem v Azure."
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
ms.openlocfilehash: 63fe3f165864f06228604cac56d06cc061ab25f5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-availability-sets"></a>Jak používat skupiny dostupnosti


V tomto kurzu se dozvíte, jak zvýšit dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti. Skupiny dostupnosti Ujistěte se, že virtuálních počítačů nasadit v Azure jsou distribuovány na více clusterů izolované hardwaru. To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, bude mít vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstanou dostupné a funkční z perspektivy zákazníky ho pomocí.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače v nastavení dostupnosti
> * Zkontrolujte dostupné velikosti virtuálních počítačů


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Přehled sady dostupnosti

Skupiny dostupnosti je logické seskupení funkci, která můžete použít v Azure k zajištění, že prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure. Azure zajistí, že virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače. To zajistí, že v případě hardwaru nebo softwaru Azure selhání, bude mít vliv jenom podmnožinu virtuální počítače a aplikace celkové zůstane nahoru a nadále k dispozici pro vaše zákazníky. Pomocí nastavení dostupnosti je nezbytné schopnost využít, pokud chcete vytvářet spolehlivé cloudové řešení.

Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze. S Azure, by chcete definovat dvě sady dostupnosti před nasazením virtuálních počítačů: jednu sadu dostupnosti pro vrstvu "web" a jednu sadu dostupnosti pro vrstvu "databáze". Při vytváření nového virtuálního počítače můžete určit skupiny dostupnosti jako parametr pro virtuální počítač az vytvoření příkazu a Azure automaticky zajistí, že virtuální počítače, vytvořte v rámci sady k dispozici jsou izolované napříč několika prostředcích fyzický hardware. To znamená, že pokud fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že ostatní instance webového serveru a databáze virtuální počítače zůstanou spuštěné bez problémů vzhledem k tomu, že jsou na jiný hardware.

Pokud chcete nasadit spolehlivé řešení založená na virtuálních počítačů v rámci Azure byste měli vždycky používat skupiny dostupnosti.


## <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti

Můžete vytvořit pomocí sadu dostupnosti [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create). V tomto příkladu jsme nastavit počet aktualizací a chyby domén v *2* pro skupinu dostupnosti s názvem *myAvailabilitySet* v *myResourceGroupAvailability* Skupina prostředků.

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

Skupiny dostupnosti umožňují izolovat prostředky napříč "domén selhání" a "update domény". A **doména selhání** představuje kolekci izolované serveru + síť + úložiště prostředků. V předchozím příkladu jsme znamenat, že chceme, aby naše dostupnosti být distribuován do alespoň dva domén selhání při nasazení naše virtuálních počítačů. Také znamenat, že chceme, aby naše dostupnosti distribuované napříč dvěma **aktualizovat domény**.  Dvě domény aktualizace zajistěte, aby při Azure provádí aktualizace softwaru naše prostředky virtuálních počítačů izolované, brání veškerý software, které jsou spuštěné pod naše virtuálních počítačů ze aktualizaci ve stejnou dobu.

## <a name="configure-virtual-network"></a>Konfigurace virtuální sítě
Před nasazením některé virtuální počítače a nástroj pro vyrovnávání můžete otestovat, vytvořte doprovodné materiály virtuální sítě. Další informace o virtuálních sítích najdete v tématu [spravovat virtuální sítě Azure](tutorial-virtual-network.md) kurzu.

### <a name="create-network-resources"></a>Vytvoření síťové prostředky
Vytvoření virtuální sítě s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Následující příklad vytvoří virtuální síť s názvem *myVnet* s podsítí s názvem *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
Virtuální síťové adaptéry jsou vytvořeny pomocí [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create). Následující příklad vytvoří tři virtuálních síťových karet. (Jeden virtuální síťovou kartu pro každý virtuální počítač vytvoříte pro vaši aplikaci v následujících krocích). Můžete kdykoli vytvořit další virtuální síťové karty a virtuální počítače a jejich přidání do Vyrovnávání zatížení:

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

Virtuální počítače musí být vytvořen v rámci skupinu dostupnosti a ujistěte se, že jsou správně rozmístěny v hardwaru. Nelze přidat existující virtuální počítač po vytvoření sadu dostupnosti. 

Při vytváření virtuálního počítače pomocí [az virtuální počítač vytvořit](/cli/azure/vm#create) zadáte skupinu dostupnosti pomocí `--availability-set` parametr zadat název skupiny dostupnosti.

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

Nyní je k dispozici dva virtuální počítače v rámci naší sady nově vytvořený dostupnosti. Protože jsou ve stejné skupině dostupnosti, Azure zajistí, že virtuální počítače a jejich prostředky (včetně datových disků) jsou rozmístěny v izolované fyzický hardware. Toto rozdělení pomáhá zajistit mnohem vyšší dostupnosti naše celkového řešení virtuálních počítačů.

Jednou z věcí, které se můžete setkat při přidávání virtuálních počítačů je, že konkrétní velikost virtuálního počítače již není k dispozici pro použití v rámci vaší sady dostupnosti. Tento problém může dojít, když už není dostatečnou kapacitu pro přidání při zachování pravidla izolace vynucuje skupiny dostupnosti. Můžete zkontrolovat, uvidíte, jaké velikosti virtuálních počítačů jsou k dispozici pro použití v rámci existující dostupnosti nastavit pomocí `--availability-set list-sizes` parametr.

## <a name="check-for-available-vm-sizes"></a>Zkontrolujte dostupné velikosti virtuálních počítačů 

Můžete přidat další virtuální počítače pro skupinu dostupnosti později, ale musíte vědět, jaké velikosti virtuálních počítačů jsou k dispozici na hardware. Použití [seznam velikosti az virtuálních počítačů sady dostupnosti](/cli/azure/availability-set#list-sizes) seznam všech dostupných velikostí na hardwaru clusteru pro skupinu dostupnosti.

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

Přechodu na v dalším kurzu se dozvíte o sady škálování virtuálního počítače.

> [!div class="nextstepaction"]
> [Vytvoření škálovací sady virtuálních počítačů](tutorial-create-vmss.md)

