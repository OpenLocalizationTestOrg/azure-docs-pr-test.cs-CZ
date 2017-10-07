---
title: "aaaCreate kopii virtuálním počítačům s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate kopii virtuálního počítače Azure Linux s hello Azure CLI 1.0 v modelu nasazení Resource Manager hello"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 997a2c8109e7083ececd76fd1013e9ed4d3e6afd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-hello-azure-cli-10"></a>Vytvořit kopii virtuální počítač s Linuxem v Azure s hello Azure CLI 1.0
Tento článek ukazuje, jak hello toocreate kopie vašeho virtuálních počítačů (VM) Azure spuštěné Linux pomocí modelu nasazení Resource Manager. Nejdřív zkopírujte přes hello operačního systému a datové disky tooa nový kontejner, pak nastavíte hello síťové prostředky a vytvořit nový virtuální počítač hello.

Můžete také [odesílání a vytvořit virtuální počítač z bitové kopie disku vlastní](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- Rozhraní příkazového řádku Azure 1.0 – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="before-you-begin"></a>Než začnete
Ujistěte se, že splňujete následující požadavky, než začnete hello kroky hello:

* Máte hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) stažen a nainstalován na váš počítač. 
* Musíte taky některé informace o existující virtuální počítač Azure Linux:

| Informace o zdroji virtuálních počítačů | Kde tooget ho |
| --- | --- |
| název virtuálního počítače |`azure vm list` |
| Název skupiny prostředků |`azure vm list` |
| Umístění |`azure vm list` |
| Název účtu úložiště |`azure storage account list -g <resourceGroup>` |
| Název kontejneru |`azure storage container list -a <sourcestorageaccountname>` |
| Název zdrojového souboru virtuálního pevného disku virtuálního počítače |`azure storage blob list --container <containerName>` |

* Budete potřebovat toomake některé možnosti o nový virtuální počítač:   <br> -Název kontejneru   <br> -Název virtuálního počítače.   <br> Velikost virtuálního počítače-   <br> Název - vNet   <br> -Název podsítě.   <br> Název - IP   <br> Název - NIC

## <a name="login-and-set-your-subscription"></a>Přihlášení a nastavte předplatné
1. Přihlášení toohello rozhraní příkazového řádku.

    ```azurecli
    azure login
    ```
2. Ujistěte se, že jste v režimu Resource Manager.

    ```azurecli
    azure config mode arm
    ```
3. Nastavit hello správné předplatné. Můžete použít "účet azure seznam" toosee všech vašich předplatných.

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-hello-vm"></a>Zastavit hello virtuálních počítačů
Zastavte a navrátit hello zdrojového virtuálního počítače. Můžete použít "seznam virtuálních počítačů azure" tooget seznam všech hello virtuální počítače ve vašem předplatném a jejich názvy skupin prostředků.

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-hello-vhd"></a>Zkopírujte hello virtuálního pevného disku
Hello virtuální pevný disk můžete zkopírovat z hello zdroj úložiště toohello cíl pomocí hello `azure storage blob copy start`. V tomto příkladu přidáme toocopy hello virtuálního pevného disku toohello stejný účet úložiště, ale jiný kontejner.

toocopy hello virtuálního pevného disku tooanother kontejneru v hello stejný účet úložiště, zadejte:

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-hello-virtual-network-for-your-new-vm"></a>Nastavení hello virtuální sítě pro nový virtuální počítač
Nastavení virtuální sítě a síťovou kartu pro nový virtuální počítač. 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-hello-new-vm"></a>Vytvoření nového virtuálního počítače hello
Nyní můžete vytvořit virtuální počítač z nahraný virtuální disk [pomocí šablony správce prostředků](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) nebo prostřednictvím hello rozhraní příkazového řádku zadáním hello URI tooyour zkopírovaných disku zadáním:

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a>Další kroky
toolearn jak toouse rozhraní příkazového řádku Azure toomanage nového virtuálního počítače najdete v části [příkazy příkazového řádku Azure CLI pro hello Azure Resource Manager](../azure-cli-arm-commands.md).

