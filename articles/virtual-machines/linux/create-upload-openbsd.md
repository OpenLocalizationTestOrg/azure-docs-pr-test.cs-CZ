---
title: "aaaCreate a nahrání virtuálního počítače OpenBSD bitové kopie tooAzure | Microsoft Docs"
description: "Zjistěte, jak toocreate a nahrajte virtuální pevný disk (VHD), který obsahuje hello toocreate OpenBSD operačního systému virtuálního počítače Azure pomocí rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: kyliel
ms.openlocfilehash: 0524f45ea1ecec37e55cbe9d54438a571a831de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-an-openbsd-disk-image-tooazure"></a>Vytvoření a odeslání tooAzure bitové kopie disku OpenBSD
Tento článek ukazuje, jak toocreate a nahrajte virtuální pevný disk (VHD), který obsahuje hello OpenBSD operačního systému. Po odeslání, můžete jej jako vlastní image toocreate virtuální počítač (VM) v Azure pomocí rozhraní příkazového řádku Azure.


## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte hello následující položky:

* **Předplatné Azure** – Pokud nemáte účet, můžete vytvořit jeden si během několika minut. Pokud máte předplatné MSDN, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Jinak, zjistěte, jak příliš[vytvořit Bezplatný zkušební účet](https://azure.microsoft.com/pricing/free-trial/).  
* **Azure CLI 2.0** -zajistěte, aby byla hello nejnovější [Azure CLI 2.0](/cli/azure/install-azure-cli) nainstalován a přihlášení tooyour účet Azure s [az přihlášení](/cli/azure/#login).
* **OpenBSD operační systém nainstalovaný v souboru VHD** -podporovaný operační systém OpenBSD (verze 6.1) musí být nainstalované tooa virtuální pevný disk. Několik nástrojů existovat toocreate soubory VHD. Můžete například použít řešení virtualizace jako soubor VHD hello toocreate technologie Hyper-V a nainstalujte hello operační systém. Pokyny o tom, jak tooinstall a použití technologie Hyper-V, najdete v části [instalaci technologie Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).


## <a name="prepare-openbsd-image-for-azure"></a>Příprava OpenBSD image pro Azure
Na hello virtuálních počítačů, které jste nainstalovali hello OpenBSD operačního systému 6.1, která přidá podporu technologie Hyper-V, proveďte následující postupy hello:

1. Pokud během instalace není povolený DHCP, povolte hello služby následujícím způsobem:

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. Nastavení konzoly sériového portu následujícím způsobem:

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. Instalace balíčku nakonfigurujte následujícím způsobem:

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. Ve výchozím nastavení, hello `root` je uživatel zakázán na virtuálních počítačích v Azure. Uživatelé mohou spouštět příkazy se zvýšeným oprávněním pomocí hello `doas` příkaz OpenBSD virtuálního počítače. Doas je ve výchozím nastavení povolené. Další informace najdete v tématu [doas.conf](http://man.openbsd.org/doas.conf.5). 

5. Nainstalujte a nakonfigurujte předpoklady pro hello Azure Agent následovně:

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. Hello nejnovější verzi agenta Azure hello vždy najdete na [Githubu](https://github.com/Azure/WALinuxAgent/releases). Nainstalujte agenta hello následujícím způsobem:

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > Po instalaci agenta Azure, je vhodné tooverify, který používá následujícím způsobem:
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. Deprovision hello systému tooclean ho a zkontrolujte ji vhodný pro reprovisioning. Hello následující příkaz dojde také k odstranění hello poslední zřízené uživatelský účet a hello související data:

    ```sh
    waagent -deprovision+user -force
    ```

Nyní můžete vypnout virtuální počítač.


## <a name="prepare-hello-vhd"></a>Příprava hello virtuálního pevného disku
Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**. Můžete převést hello disku toofixed Formát virtuálního pevného disku pomocí Správce technologie Hyper-V nebo prostředí Powershell text hello [convert-VHD prostředí](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) rutiny. Příkladem je jako následující.

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a>Vytvořit prostředky úložiště a odeslat
Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

tooupload svůj disk VHD, vytvořit účet úložiště s [vytvořit účet úložiště az](/cli/azure/storage/account#create). Názvy účtů úložiště musí být jedinečný, takže zadejte vlastní název. Hello následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště*:

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

toocontrol přístup k účtu úložiště toohello, získejte hello klíč úložiště s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list) následujícím způsobem:

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

samostatné hello toologically virtuální pevné disky nahrajete, vytvořit kontejner v rámci účtu úložiště hello s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create):

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

Nakonec nahrát svůj disk VHD s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload) následujícím způsobem:

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a>Vytvoření virtuálního počítače z vaší virtuálního pevného disku
Můžete vytvořit virtuální počítač s [ukázkový skript](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) nebo přímo pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create). toospecify hello OpenBSD virtuální pevný disk, který jste nahráli, použijte hello `--image` parametr následujícím způsobem:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Získat hello IP adresu pro virtuální počítač OpenBSD s [az virtuálních počítačů seznamu ip-addresses](/cli/azure/vm#list-ip-addresses) následujícím způsobem:

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

Nyní můžete SSH tooyour OpenBSD virtuální počítač jako za normálních okolností:
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a>Další kroky
Pokud chcete další informace o Hyper-V podporují na OpenBSD6.1 tooknow, přečtěte si [OpenBSD 6.1](https://www.openbsd.org/61.html) a [hyperv.4](http://man.openbsd.org/hyperv.4).

Pokud chcete, aby toocreate virtuálního počítače ze spravovaných disků, přečtěte si [disku az](/cli/azure/disk). 
