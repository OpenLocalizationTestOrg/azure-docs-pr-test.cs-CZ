---
title: "Převeďte virtuální počítač s Linuxem v Azure z nespravovaných disků na spravované disky - disky spravované Azure | Microsoft Docs"
description: "Jak převést virtuální počítač s Linuxem z disků nespravované na spravované disky pomocí Azure CLI 2.0 v modelu nasazení Resource Manager"
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
ms.openlocfilehash: 94f8e3330fb2d6547811315fcfdb8ced338e0247
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Převeďte virtuální počítač s Linuxem z disků nespravované na spravované disky

Pokud máte existující virtuální počítače s Linuxem (VM) používající nespravované disky, můžete převést virtuální počítače použít spravované disky prostřednictvím [Azure spravované disky](../windows/managed-disks-overview.md) služby. Tento proces převede disk operačního systému a všechny připojené datových disků.

Tento článek ukazuje, jak převést virtuální počítače pomocí rozhraní příkazového řádku Azure. Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Než začnete

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Převést virtuální počítače jednou instancí
Tato část popisuje jak převést virtuální počítače Azure jednou instancí z nespravovaných disků na spravované disky. (Pokud jsou vaše virtuální počítače v nastavení dostupnosti, najdete v další části.) Tento proces můžete převést virtuální počítače z disků disky na disky premium spravované nebo z standard (HDD) nespravované premium (SSD) nespravované na spravované standardní disky.

1. Zrušit přidělení virtuálního počítače pomocí [az OM deallocate](/cli/azure/vm#deallocate). Následující příklad zruší přidělení virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Převeďte virtuální počítač na spravované disky pomocí [převést virtuální počítač az](/cli/azure/vm#convert). Následující proces převede virtuální počítač s názvem `myVM`, včetně disku operačního systému a všechny datové disky:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Spusťte virtuální počítač po převodu na spravované disky pomocí [spuštění virtuálního počítače az](/cli/azure/vm#start). Následující příklad spustí virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Převést virtuální počítače v nastavení dostupnosti

Pokud virtuální počítače, které chcete převést na spravované disky jsou v nastavení dostupnosti, je nutné nejprve převést skupinu dostupnosti do skupiny spravované dostupnosti.

Všechny virtuální počítače v sadě dostupnosti musí být navrácena předtím, než převedete sadu dostupnosti. Naplánujte převeďte všechny virtuální počítače na spravovaného disky po samotné sadu dostupnosti byl převeden na sadu spravovaných dostupnosti. Potom spusťte všechny virtuální počítače a pokračovat normální.

1. Zobrazí seznam všech virtuálních počítačů ve skupině dostupnosti, nastavit pomocí [seznamu skupinu dostupnosti virtuálních počítačů az](/cli/azure/vm/availability-set#list). Následující příklad vypíše všechny virtuální počítače ve skupině s názvem dostupnosti `myAvailabilitySet` ve skupině prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Deallocate všechny virtuální počítače pomocí [az OM deallocate](/cli/azure/vm#deallocate). Následující příklad zruší přidělení virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Převést skupinu dostupnosti pomocí [převést skupinu dostupnosti virtuálních počítačů az](/cli/azure/vm/availability-set#convert). Následující příklad převede skupinu dostupnosti s názvem `myAvailabilitySet` ve skupině prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Převeďte všechny virtuální počítače na spravovaného disky pomocí [převést virtuální počítač az](/cli/azure/vm#convert). Následující proces převede virtuální počítač s názvem `myVM`, včetně disku operačního systému a všechny datové disky:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Spusťte všechny virtuální počítače po převodu na spravované disky pomocí [spuštění virtuálního počítače az](/cli/azure/vm#start). Následující příklad spustí virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Další kroky
Další informace o možnostech úložiště najdete v tématu [přehled Azure spravované disky](../windows/managed-disks-overview.md).
