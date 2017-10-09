---
title: "aaaUpload nebo kopírování vlastní virtuální počítač s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Nahrát, nebo můžete zkopírovat vlastní virtuální počítač pomocí modelu nasazení Resource Manager hello a hello 2.0 rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/06/2017
ms.author: cynthn
ms.openlocfilehash: 79af897120a6ba7f4a427ba6c7d0c31b7b870bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a>Vytvoření virtuálního počítače s Linuxem z vlastní disk s hello 2.0 rozhraní příkazového řádku Azure

<!-- rename toocreate-vm-specialized -->

Tento článek ukazuje, jak tooupload přizpůsobené virtuální pevný disk (VHD) nebo zkopírovat existující virtuální pevný disk v Azure a vytvoření nových Linux virtuálních počítačů (VM) z vlastní disku hello. Můžete instalovat a konfigurovat požadavky na tooyour distro Linux a pak použít tento virtuální pevný disk tooquickly vytvořit nový virtuální počítač Azure.

Pokud toocreate chcete z přizpůsobené disku víc virtuálních počítačů, měli byste vytvořit bitovou kopii z virtuálního počítače nebo virtuální pevný disk. Další informace najdete v tématu [vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku hello](tutorial-custom-images.md).

Máte dvě možnosti:
* [Nahrání virtuálního pevného disku](#option-1-upload-a-specialized-vhd)
* [Zkopírujte existující virtuální počítač Azure](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Rychlé příkazy

Při vytváření nového virtuálního počítače pomocí [az virtuální počítač vytvořit](/cli/azure/vm#create) z přizpůsobené nebo specializované disku můžete **připojit** hello disku (– připojit disk operačního systému) místo zadávání vlastního nebo marketplace image (--bitové kopie). Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí hello spravovaného disku s názvem *myManagedDisk* vytvořené z vaší vlastní virtuální pevný disk:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Požadavky
toocomplete hello následující kroky, budete potřebovat:

* Virtuální počítač Linux, který je připravena pro použití v Azure. Hello [hello Příprava virtuálního počítače](#prepare-the-vm) tohoto článku popisuje, jak toofind distro konkrétní informace o instalaci hello Azure Linux Agent (příkaz waagent), který je nutné pro virtuální počítač toowork hello správně v Azure a jste toobe možné tooconnect tooit pomocí protokolu SSH.
* soubor VHD Hello z existující [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuální disk ve formátu virtuálního pevného disku hello. Několik nástrojů existují toocreate virtuálního počítače a virtuálního pevného disku:
  * Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), dbejte na to toouse virtuálního pevného disku jako formát obrázku. V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí **qemu img převést**.
  * Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Hello novější formát VHDX není podporovaný v Azure. Když vytvoříte virtuální počítač, zadejte hello Formát virtuálního pevného disku. V případě potřeby můžete převést pomocí tooVHD disky VHDX [qemu img převést](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell. Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je třeba tooconvert takové toostatic disky VHD před nahráním. Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert během procesu hello odesílání tooAzure dynamické disky.
> 
> 


* Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté *myResourceGroup*, *můj_účet_úložiště*, a *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-hello-vm"></a>Příprava hello virtuálních počítačů

Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Následující články Hello provede jak tooprepare hello různé distribuce systému Linux, které jsou podporovány v Azure:

* [Na základě centOS distribuce](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Další - neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Viz také hello [poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.

> [!NOTE]
> Hello [platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) platí tooVMs běží Linux, jen pokud jeden z hello distribuce schválené se používá s podrobnosti konfigurace hello uvedeného v části "podporované verze v [Linux na schválené pro Azure Distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>Možnost 1: Nahrání virtuálního pevného disku

Můžete nahrát vlastní virtuální pevný disk, ke které máte spuštěné v místním počítači nebo který jste exportovali z jiného cloudu. toouse hello virtuálního pevného disku toocreate nový virtuální počítač Azure, budete potřebovat tooupload hello virtuálního pevného disku tooa úložiště účtu a vytvoření spravovaného disku ze hello virtuálního pevného disku. 

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Před nahráním váš vlastní disk a vytváření virtuálních počítačů, musíte nejprve toocreate skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).

Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění: [disky spravované Azure – přehled](../windows/managed-disks-overview.md)
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>vytvořit účet úložiště

Vytvoření účtu úložiště vlastní disk a virtuální počítače s [vytvořit účet úložiště az](/cli/azure/storage/account#create). 

Hello následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště* ve skupině prostředků hello vytvořili:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Vypsat klíče účtu úložiště
Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště. Tyto přístupové klíče se používají při ověřování účtu úložiště toohello, jako je provádění operace zápisu. Další informace o [Správa přístup toostorage zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Zobrazit přístupové klíče hello s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).

Zobrazit hello přístupové klíče pro účet úložiště hello, kterou jste vytvořili:

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

výstup Hello je podobná:

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
Poznamenejte si **key1** jak ji budete používat toointeract se svým účtem úložiště v dalších krocích hello.

### <a name="create-a-storage-container"></a>Vytvoření kontejneru úložiště
V hello stejným způsobem, abyste vytvořili různé adresáře toologically uspořádat do místního systému souborů, vytvoření kontejnerů v rámci tooorganize účet úložiště vaše disky. Účet úložiště může obsahovat libovolný počet kontejnerů. Vytvořit kontejner s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create).

Hello následující příklad vytvoří kontejner s názvem *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-hello-vhd"></a>Nahrát hello virtuálního pevného disku
Teď nahrát vlastní disk s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload). Nahrání a ukládat vlastní disk jako objekt blob stránky.

Zadejte přístupový klíč, hello kontejneru, kterou jste vytvořili v předchozím kroku hello a pak hello cesta toohello vlastní disku v místním počítači:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
Odesílání hello virtuálního pevného disku může chvíli trvat.

### <a name="create-a-managed-disk"></a>Vytvoření spravovaného disku


Vytvoření spravovaného disku pomocí virtuálního pevného disku hello [vytvoření disku az](/cli/azure/disk#create). Hello následující příklad vytvoří spravované disk s názvem *myManagedDisk* z hello virtuálního pevného disku, který jste nahráli tooyour s názvem účtu úložiště a kontejneru:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>Možnost 2: Zkopírujte existující virtuální počítač

Můžete také vytvořit hello vlastní virtuální počítač v Azure a pak kopie disku hello operačního systému a jeho připojení tooa nový virtuální počítač toocreate jiná kopie. To je v pořádku pro testování, ale pokud chcete toouse existující virtuální počítač Azure jako hello model pro více nové virtuální počítače, ve skutečnosti měli byste vytvořit **image** místo. Další informace o vytvoření bitové kopie ze stávajícího virtuálního počítače Azure najdete v tématu [vytvořit vlastní image virtuálního počítače Azure pomocí hello rozhraní příkazového řádku](tutorial-custom-images.md)

### <a name="create-a-snapshot"></a>Vytvoření snímku

Tento příklad vytvoří snímek virtuálního počítače s názvem *Můjvp* ve skupině prostředků *myResourceGroup* a vytvoří snímek s názvem *osDiskSnapshot*.

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-hello-managed-disk"></a>Vytvoření spravovaného disku hello

Vytvoření nového spravovaného disku ze snímku hello.

Získání ID hello hello snímku. V tomto příkladu je název snímku hello *osDiskSnapshot* a je v hello *myResourceGroup* skupinu prostředků.

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

Vytvoření hello spravovaného disku. V tomto příkladu vytvoříme spravované disk s názvem *myManagedDisk* ze naše snímek, který je 128 GB velikost ve standardní úložiště.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-hello-vm"></a>Vytvoření hello virtuálních počítačů

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojte (--připojit disk operačního systému) hello spravované disk jako disk hello operačního systému. Hello následující příklad vytvoří virtuální počítač s názvem *myNewVM* pomocí hello spravovaného disku vytvořené z vaší nahraný virtuální pevný disk:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

Musí být schopný tooSSH do hello virtuálních počítačů pomocí přihlašovacích údajů hello z hello zdrojového virtuálního počítače. 

## <a name="next-steps"></a>Další kroky
Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md). Můžete také příliš[přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nové virtuální počítače. Pokud máte aplikace spuštěné na virtuálních počítačů, je nutné, aby tooaccess, nezapomeňte příliš[otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

