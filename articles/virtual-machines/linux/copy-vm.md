---
title: "virtuální počítač s Linuxem pomocí Azure CLI 2.0 aaaCopy | Microsoft Docs"
description: "Zjistěte, jak toocreate kopii virtuálním počítačům s Linuxem Azure pomocí Azure CLI 2.0 a spravované disky."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/10/2017
ms.author: cynthn
ms.openlocfilehash: ee34a4259dd0c1e7bf49312fe3fe3ba809bf69e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a>Vytvoří kopii virtuálního počítače s Linuxem pomocí Azure CLI 2.0 a spravované disky


Tento článek ukazuje, jak hello toocreate kopii Azure virtuálního počítače (VM) systémem Linux pomocí Azure CLI 2.0 a modelu nasazení Azure Resource Manager hello. Můžete také provést tyto kroky hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Můžete také [odesílání a vytvoření virtuálního počítače z disku VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Požadavky


-   Nainstalujte [rozhraní příkazového řádku Azure 2.0](/cli/azure/install-az-cli2)

-   Přihlaste se tooan účet Azure s [az přihlášení](/cli/azure/#login).

-   Máte virtuálním počítači Azure toouse jako zdroj hello vaší kopie.

## <a name="step-1-stop-hello-source-vm"></a>Krok 1: Zastavení hello zdrojového virtuálního počítače


Deallocate hello zdrojového virtuálního počítače pomocí [az OM deallocate](/cli/azure/vm#deallocate).
Hello následující příklad zruší přidělení hello virtuálního počítače s názvem **Můjvp** ve skupině prostředků hello **myResourceGroup**:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-hello-source-vm"></a>Krok 2: Kopírování hello zdrojového virtuálního počítače


toocopy virtuálního počítače, vytvořte kopii hello základní virtuální pevný disk. Tento proces vytvoří specializované virtuálního pevného disku jako spravované Disk, který obsahuje hello stejné konfigurace a nastavení, jako hello zdrojového virtuálního počítače.

Další informace o službě Azure Managed Disks najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md). 

1.  Seznam každý virtuální počítač a hello název jeho disk operačního systému s [seznamu virtuálních počítačů az](/cli/azure/vm#list). Hello následující příklad vypíše všechny virtuální počítače ve skupině prostředků s názvem **myResourceGroup**:
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    Hello výstup je podobné toohello následující ukázka:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Kopírování hello disku tak, že vytvoříte novou spravovat pomocí disku [vytvoření disku az](/cli/azure/disk#create). Hello následující příklad vytvoří disk s názvem **myCopiedDisk** z hello spravované disk s názvem **myDisk**:

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  Ověřit disky hello spravované nyní ve vaší skupině prostředků pomocí [seznam disků az](/cli/azure/disk#list). Hello následující příklad vypíše disky hello spravované v hello skupinu prostředků s názvem **myResourceGroup**:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  Přeskočit příliš["krok 3: nastavení virtuální sítě"](#step-3-set-up-a-virtual-network).


## <a name="step-3-set-up-a-virtual-network"></a>Krok 3: Nastavení virtuální sítě


Hello následující volitelné kroky vytvoření nové virtuální sítě, podsítě, veřejnou IP adresu a virtuální síťová karta (NIC).

Pokud kopírujete virtuální počítač pro řešení potíží s účely nebo další nasazení, není vhodné toouse virtuálního počítače ve stávající virtuální síť.

Pokud chcete toocreate infrastruktury virtuální sítě pro zkopírovaný virtuální počítače, postupujte podle hello vedle několika krocích. Pokud nechcete, aby toocreate virtuální síť, přeskočte příliš[krok 4: vytvoření virtuálního počítače](#step-4-create-a-vm).

1.  Vytvoření virtuální sítě hello pomocí [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Hello následující příklad vytvoří virtuální síť s názvem **myVnet** a podsíť s názvem **mySubnet**:

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  Vytvoření veřejné IP adresy pomocí [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create). Hello následující příklad vytvoří veřejnou IP adresu s názvem **myPublicIP** s názvem DNS hello **mypublicdns**. (název DNS hello musí být jedinečný, takže zadejte jedinečný název.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  Vytvořte pomocí seskupování hello [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create).
    Hello následující příklad vytvoří síťový adaptér s názvem **myNic** toho připojené toothe **mySubnet** podsítě:

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a>Krok 4: Vytvoření virtuálního počítače

Nyní můžete vytvořit virtuální počítač pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create).

Zadejte hello zkopírovat toouse spravované disk jako disk hello operačního systému (--připojit disk operačního systému), a to takto:

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Další kroky

toolearn jak toouse rozhraní příkazového řádku Azure toomanage nový virtuální počítač, najdete v části [příkazy příkazového řádku Azure CLI pro hello Azure Resource Manager](../azure-cli-arm-commands.md).
