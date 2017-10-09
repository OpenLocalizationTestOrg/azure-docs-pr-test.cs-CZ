---
title: "aaaCreate vlastní Image virtuálních počítačů s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – vytvoření vlastní image virtuálního počítače pomocí hello rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/21/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 217a993c0c1d48939b74108ac6c5f7a1a619416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-hello-cli"></a>Vytvořit vlastní image virtuálního počítače Azure pomocí hello rozhraní příkazového řádku

Vlastní Image jsou jako obrázky marketplace, ale můžete vytvořit sami. Vlastní Image může být použité toobootstrap konfigurace, jako je například předběžného načítání aplikace, konfigurace aplikace a další konfigurace operačního systému. V tomto kurzu vytvoříte svoji vlastní image virtuálního počítače v Azure. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Zrušení zřízení a generalize virtuální počítače
> * Vytvoření vlastní image
> * Vytvoření virtuálního počítače z vlastní image
> * Zobrazí seznam všech hello bitové kopie v rámci vašeho předplatného
> * Odstranit bitovou kopii


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Než začnete

Následující postup Hello podrobnosti, jak tootake existující virtuální počítač a zapněte ji do opakovaně použitelné vlastní image, které můžete použít toocreate nové instance virtuálního počítače.

Příklad hello toocomplete v tomto kurzu, musí mít existující virtuální počítač. V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) můžete vytvořit za vás. Při absolvování hello kurzu, nahraďte hello skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.

## <a name="create-a-custom-image"></a>Vytvoření vlastní image

toocreate bitové kopie virtuálního počítače, musíte tooprepare hello virtuálních počítačů zrušení zřízení, rušení přidělení a pak označením hello zdrojového virtuálního počítače jako zobecněn. Jednou hello, kterou virtuální počítač připravený, můžete vytvořit bitovou kopii.

### <a name="deprovision-hello-vm"></a>Zrušení zřízení hello virtuálních počítačů 

Zrušení zřízení umožňuje zobecnit hello virtuálních počítačů odebráním informace specifické pro počítač. Díky této generalizace je možné toodeploy mnoho virtuálních počítačů z jedné bitové kopie. Během zrušení zřízení, název hostitele hello resetovat příliš*localhost.localdomain*. Klíče hostitele SSH, konfigurace názvový server, kořenové heslo a uložené v mezipaměti zapůjčení DHCP se také odstraní.

toodeprovision hello virtuálních počítačů, použijte agenta virtuálního počítače Azure hello (příkaz waagent). agent virtuálního počítače Azure Hello je nainstalován na hello virtuálních počítačů a spravuje zřizování a interakci s hello Kontroleru prostředků infrastruktury Azure. Další informace najdete v tématu hello [Azure Linux Agent uživatelská příručka](agent-user-guide.md).

Připojit tooyour virtuálních počítačů pomocí protokolu SSH a spuštění hello příkaz toodeprovision hello virtuálních počítačů. S hello `+user` argument, hello poslední zřízené uživatelský účet a všechna související data se také odstraní. Nahraďte hello příklad IP adresu hello veřejnou IP adresu vašeho virtuálního počítače.

SSH toohello virtuálních počítačů.
```bash
ssh azureuser@52.174.34.95
```
Zrušení zřízení hello virtuálních počítačů.

```bash
sudo waagent -deprovision+user -force
```
Zavřete relace SSH hello.

```bash
exit
```

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a>Deallocate a označit hello jako zobecněný virtuální počítač

toocreate bitovou kopii, hello virtuálních počítačů musí toobe navrácena. Deallocate – hello virtuální počítač pomocí [az OM deallocate](/cli//azure/vm#deallocate). 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

Nakonec nastavit stav hello hello virtuálního počítače jako zobecněn s [az virtuálních počítačů zobecní](/cli//azure/vm#generalize) tak hello platformy Azure zná hello zobecněný virtuální počítač. Bitovou kopii lze vytvořit pouze z zobecněný virtuální počítač.
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-hello-image"></a>Vytvoření bitové kopie hello

Nyní můžete vytvořit image hello virtuálních počítačů pomocí [vytvoření bitové kopie az](/cli//azure/image#create). Hello následující příklad vytvoří bitovou kopii s názvem *myImage* z virtuálního počítače s názvem *Můjvp*.
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-hello-image"></a>Vytvořit virtuální počítače z hello image

Teď, když máte image, můžete vytvořit jeden nebo více nové virtuální počítače pomocí bitové kopie hello [vytvořit virtuální počítač az](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem *myVMfromImage* z hello image s názvem *myImage*.

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Správy bitových kopií 

Zde jsou některé příklady běžné úlohy správy bitové kopie a jak toocomplete je pomocí hello rozhraní příkazového řádku Azure.

Zobrazí seznam všech Image podle názvu ve formátu tabulky.

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

Odstraňte bitovou kopii. Tento příklad odstranění hello image s názvem *myOldImage* z hello *myResourceGroup*.

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili vlastní image virtuálního počítače. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Zrušení zřízení a generalize virtuální počítače
> * Vytvoření vlastní image
> * Vytvoření virtuálního počítače z vlastní image
> * Zobrazí seznam všech hello bitové kopie v rámci vašeho předplatného
> * Odstranit bitovou kopii

Posunutí další kurz toolearn toohello o vysoce dostupných virtuálních počítačů.

> [!div class="nextstepaction"]
> [Vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md).

