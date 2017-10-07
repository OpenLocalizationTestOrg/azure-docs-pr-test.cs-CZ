---
title: "virtuální počítač s Linuxem v Azure z nespravovaných aaaConvert disky toomanaged disků - disků spravované Azure | Microsoft Docs"
description: "Jak tooconvert virtuálního počítače s Linuxem z nespravovaných disky toomanaged disky pomocí Azure CLI 2.0 v modelu nasazení Resource Manager hello"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 1b94da11deab46f344e28ab4491cf220506b6347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a>Převést virtuální počítač s Linuxem z nespravovaných disky toomanaged disků

Pokud máte existující virtuální počítače s Linuxem (VM) používající nespravované disky, můžete převést disky toouse spravované hello virtuálních počítačů prostřednictvím hello [Azure spravované disky](../windows/managed-disks-overview.md) služby. Tento proces převede disk hello operačního systému a všechny připojené datových disků.

Tento článek ukazuje, jak hello tooconvert virtuálních počítačů pomocí rozhraní příkazového řádku Azure. Pokud potřebujete tooinstall nebo ho upgradovat, přečtěte si téma [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Než začnete

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Převést virtuální počítače jednou instancí
Tato část popisuje, jak tooconvert jedné instance virtuální počítače Azure z nespravovaných disky toomanaged disky. (Pokud jsou vaše virtuální počítače v nastavení dostupnosti, viz další část hello.) Můžete použít tento proces tooconvert hello virtuální počítače z nespravované premium (SSD) disky toopremium spravované disky nebo standard (HDD) nespravované disky toostandard spravované disků.

1. Deallocate hello virtuálních počítačů pomocí [az OM deallocate](/cli/azure/vm#deallocate). Hello následující příklad zruší přidělení hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Převést disky toomanaged hello virtuálních počítačů pomocí [převést virtuální počítač az](/cli/azure/vm#convert). Hello následující proces převede hello virtuálního počítače s názvem `myVM`, včetně disku hello operačního systému a všechny datové disky:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Spustit hello virtuálních počítačů po hello převod toomanaged disky pomocí [spuštění virtuálního počítače az](/cli/azure/vm#start). Následující příklad spustí Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Převést virtuální počítače v nastavení dostupnosti

Pokud hello virtuálních počítačů, které chcete tooconvert toomanaged disky jsou v nastavení dostupnosti, je nutné nejprve tooconvert hello dostupnost sady tooa spravovat sady dostupnosti.

Všechny virtuální počítače ve skupině dostupnosti hello musí být navrácena před převodem hello sady dostupnosti. Plán tooconvert všechny disky toomanaged virtuálních počítačů po hello dostupnosti nastavit sám sebe byl převeden tooa spravovat sady dostupnosti. Pak spusťte všechny virtuální počítače hello a pokračovat v činnosti jako normální.

1. Zobrazí seznam všech virtuálních počítačů ve skupině dostupnosti, nastavit pomocí [seznamu skupinu dostupnosti virtuálních počítačů az](/cli/azure/vm/availability-set#list). Hello následující příklad vypíše všechny virtuální počítače ve skupině s názvem dostupnosti hello `myAvailabilitySet` v hello skupinu prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Deallocate všechny virtuální počítače hello pomocí [az OM deallocate](/cli/azure/vm#deallocate). Hello následující příklad zruší přidělení hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Převést hello dostupnosti pomocí [převést skupinu dostupnosti virtuálních počítačů az](/cli/azure/vm/availability-set#convert). Hello následující příklad převede sadu s názvem dostupnosti hello `myAvailabilitySet` v hello skupinu prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Převeďte všechny disky toomanaged hello virtuálních počítačů pomocí [převést virtuální počítač az](/cli/azure/vm#convert). Hello následující proces převede hello virtuálního počítače s názvem `myVM`, včetně disku hello operačního systému a všechny datové disky:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Spuštění všech virtuálních počítačů hello po hello převod toomanaged disky pomocí [spuštění virtuálního počítače az](/cli/azure/vm#start). Následující příklad spustí Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Další kroky
Další informace o možnostech úložiště najdete v tématu [přehled Azure spravované disky](../windows/managed-disks-overview.md).
