---
title: "aaaCapture bitové kopie virtuálního počítače s Linuxem v Azure pomocí rozhraní příkazového řádku 2.0 | Microsoft Docs"
description: "Vytvořte bitovou kopii virtuálnímu počítači Azure toouse velkokapacitního nasazení pomocí hello 2.0 rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9558332a86186b282775097428df462709373012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-image-of-a-virtual-machine-or-vhd"></a>Jak toocreate bitové kopie virtuálního počítače nebo virtuální pevný disk

<!-- generalize, image - extended version of hello tutorial-->

toocreate více kopií toouse virtuální počítač (VM) v Azure, vytvořte bitovou kopii hello virtuálního počítače nebo hello virtuálního pevného disku operačního systému. toocreate bitovou kopii, je nutné odebrat informace osobního účtu, takže je bezpečnější toodeploy vícekrát. V hello postupu zrušení zřízení existující virtuální počítač, navrácení a vytvořit bitovou kopii. Můžete použít tento toocreate bitové kopie virtuálních počítačů v libovolné skupině prostředků v rámci vašeho předplatného.

Pokud chcete použít pro zálohování nebo ladění toocreate kopii vaše stávající virtuální počítač s Linuxem nebo nahrát specializované Linux virtuální pevný disk z virtuálního počítače místní, přečtěte si téma [nahrát a vytvoření virtuálního počítače s Linuxem z bitové kopie disku vlastní](upload-vhd.md).  

Můžete také použít **balírna** toocreate vlastní konfigurace. Další informace o používání balírna najdete v tématu [jak Image toouse balírna toocreate Linux virtuálního počítače v nástroji Azure](build-image-with-packer.md).


## <a name="before-you-begin"></a>Než začnete
Ujistěte se, že splňujete hello následující požadavky:

* Je nutné virtuální počítač Azure vytvořené v modelu nasazení Resource Manager hello pomocí spravovaných disků. Pokud jste nevytvořili virtuální počítač s Linuxem, můžete použít hello [portál](quick-create-portal.md), hello [rozhraní příkazového řádku Azure](quick-create-cli.md), nebo [šablony Resource Manageru](create-ssh-secured-vm-from-template.md). Nakonfigurujte hello virtuální počítač podle potřeby. Například [přidat datových disků](add-disk.md), aktualizace a instalovat aplikace. 

* Musíte taky toohave hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a být přihlášeni pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

## <a name="quick-commands"></a>Rychlé příkazy

Zjednodušené verzi tohoto tématu pro testování, vyhodnocení nebo získávání informací o virtuálních počítačů v Azure, naleznete v části [vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku hello](tutorial-custom-images.md).


## <a name="step-1-deprovision-hello-vm"></a>Krok 1: Zrušení zřízení hello virtuálních počítačů
Můžete zrušit jejich zřízení hello virtuální počítač pomocí hello agenta virtuálního počítače Azure, toodelete počítače konkrétní soubory a data. Použití hello `waagent` s hello *-deprovision + uživatele* parametr na svůj zdroj virtuálního počítače s Linuxem. Další informace najdete v tématu hello [Azure Linux Agent uživatelská příručka](../windows/agent-user-guide.md).

1. Připojte tooyour virtuálního počítače s Linuxem pomocí klienta SSH.
2. V okně hello SSH zadejte následující příkaz hello:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > Spusťte pouze na virtuálním počítači tento příkaz, že máte v úmyslu toocapture jako obrázek. Nezaručuje se této bitové kopie hello vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci. Hello *+ uživatele* parametr také odebere poslední účet zřízení uživatele hello. Pokud chcete, aby tookeep přihlašovací údaje hello virtuálních počítačů, použijte *-deprovision* tooleave hello uživatelský účet na místě.
 
3. Typ **y** toocontinue. Můžete přidat hello **-force** parametr tooavoid tento krok potvrzení.
4. Po dokončení příkazu hello zadejte **ukončete**. Tento krok zavře klient SSH hello.

## <a name="step-2-create-vm-image"></a>Krok 2: Vytvoření image virtuálního počítače
Použijte hello Azure CLI 2.0 toomark hello virtuálního počítače jako zobecněn a zachycení bitové kopie hello. Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.

1. Deallocate hello virtuální počítač, který jste se zrušit [az OM deallocate](/cli//azure/vm#deallocate). Hello následující příklad zruší přidělení hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. Označit hello virtuálního počítače jako zobecněn s [az virtuálních počítačů zobecní](/cli//azure/vm#generalize). Následující příklad značky hello hello virtuálních počítačů s názvem Hello *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup* jako zobecněn:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Nyní vytvořit bitovou kopii hello prostředků virtuálního počítače s [vytvoření bitové kopie az](/cli//azure/image#create). Hello následující příklad vytvoří bitovou kopii s názvem *myImage* v hello skupinu prostředků s názvem *myResourceGroup* pomocí hello prostředků virtuálního počítače s názvem *Můjvp*:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > Hello bitové kopie je vytvořen v hello stejné skupině prostředků jako vašeho zdrojového virtuálního počítače. Virtuální počítače můžete vytvořit v libovolné skupině prostředků v rámci vašeho předplatného z této bitové kopie. Z hlediska správy můžete toocreate určité skupiny zdrojů pro vaše prostředky virtuálních počítačů a bitové kopie.

## <a name="step-3-create-a-vm-from-hello-captured-image"></a>Krok 3: Vytvoření virtuálního počítače z hello zachycení image
Vytvoření virtuálního počítače pomocí bitové kopie hello jste vytvořili pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem *myVMDeployed* z hello image s názvem *myImage*:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-hello-vm-in-another-resource-group"></a>Vytváření hello virtuálních počítačů v jiné skupině prostředků 

Virtuální počítače můžete vytvořit z image v libovolné skupině prostředků v rámci vašeho předplatného. toocreate virtuální počítač v jiné skupině prostředků než hello image, zadejte hello úplné prostředků ID tooyour bitovou kopii. Použití [seznamu obrázků az](/cli/azure/image#list) tooview seznam bitové kopie. Hello výstup je podobné toohello následující ukázka:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

Hello následující příklad používá [vytvořit virtuální počítač az](/cli/azure/vm#create) toocreate virtuální počítač v jiné skupině prostředků než hello zdrojové bitové kopie zadáním hello ID prostředku bitové kopie:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-hello-deployment"></a>Krok 4: Ověření nasazení hello

Nyní SSH toohello virtuálního počítače jste vytvořili tooverify hello nasazení a spuštění pomocí hello nového virtuálního počítače. tooconnect pomocí protokolu SSH, najít hello IP adresu nebo plně kvalifikovaný název domény vašeho virtuálního počítače s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Další kroky
Můžete vytvořit víc virtuálních počítačů z vaší zdrojové bitové kopie virtuálního počítače. Pokud budete potřebovat image tooyour toomake změny: 

- Vytvořte virtuální počítač z bitové kopie.
- Nastavit všechny aktualizace nebo změny konfigurace.
- Postupujte podle hello kroky opakujte toodeprovision, navrácení, generalize a vytvořte bitovou kopii.
- Použijte tuto novou bitovou kopii pro budoucí nasazení. V případě potřeby odstraňte původní image hello.

Další informace týkající se správy virtuálních počítačů s hello rozhraní příkazového řádku najdete v tématu [Azure CLI 2.0](/cli/azure/overview).
