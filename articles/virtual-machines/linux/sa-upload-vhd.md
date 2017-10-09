---
title: "aaaUpload vlastní disk Linux s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření a nahrání virtuálního pevného disku (VHD) tooAzure, pomocí modelu nasazení Resource Manager hello a hello 2.0 rozhraní příkazového řádku Azure"
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
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: cf8556ab4dfe6c4e5eff4e99fe1ddc44393f774c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a>Nahrání a vytvoření virtuálního počítače s Linuxem z vlastní disk s hello 2.0 rozhraní příkazového řádku Azure
Tento článek ukazuje, jak tooupload virtuální pevný disk (VHD) tooan úložiště Azure účet s hello Azure CLI 2.0 a vytvořit virtuální počítače s Linuxem z tento vlastní disk. Můžete také provést tyto kroky hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tato funkce vám umožní tooinstall a konfigurovat požadavky na tooyour distro Linux a pak použít tento virtuální pevný disk tooquickly vytvoření Azure virtuálních počítačů (VM).

Toto téma používá účty úložiště pro hello konečné virtuální pevné disky, ale můžete také provést tyto kroky, pomocí [discích spravovaných](upload-vhd.md). 

## <a name="quick-commands"></a>Rychlé příkazy
Pokud je třeba tooquickly dosáhnout hello, následující části Podrobnosti hello hello základní příkazy tooupload tooAzure virtuálního pevného disku. Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#requirements).

Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `mydisks`.

Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `WestUs` umístění:

```azurecli
az group create --name myResourceGroup --location westus
```

Vytvoření toohold účet úložiště virtuální disky s [vytvořit účet úložiště az](/cli/azure/storage/account#create). Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Seznam hello přístupové klíče pro účet úložiště s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list). Poznamenejte si `key1`:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Vytvořit kontejner v rámci účtu úložiště pomocí klíč hello úložiště získat s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create). Hello následující příklad vytvoří kontejner s názvem `mydisks` pomocí hello úložiště klíčů hodnoty z `key1`:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Nakonec nahrát váš kontejner toohello virtuálního pevného disku jste vytvořili pomocí [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload). Zadejte místní cestu tooyour hello virtuální pevný disk v části `/path/to/disk/mydisk.vhd`:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

Zadejte hello URI tooyour disk (`--image`) s [vytvořit virtuální počítač az](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí hello předtím nahrála virtuální disk:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Hello cílový účet úložiště obsahuje toobe hello stejné jako kde jste nahráli virtuální disk. Můžete také potřebovat toospecify nebo zodpovědět vyzve k zadání, všechny hello další parametry, které vyžadují hello **vytvořit virtuální počítač az** příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH. Další informace o hello [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Požadavky
toocomplete hello následující kroky, budete potřebovat:

* **Operační systém Linux nainstalován v souboru VHD** -nainstalovat [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuálního disku v hello virtuálního pevného disku formát. Několik nástrojů existují toocreate virtuálního počítače a virtuálního pevného disku:
  * Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), dbejte na to toouse virtuálního pevného disku jako formát obrázku. V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.
  * Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Hello novější formát VHDX není podporovaný v Azure. Když vytvoříte virtuální počítač, zadejte hello Formát virtuálního pevného disku. V případě potřeby můžete převést pomocí tooVHD disky VHDX [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell. Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je třeba tooconvert takové toostatic disky VHD před nahráním. Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert během procesu hello odesílání tooAzure dynamické disky.
> 
> 

* Virtuální počítače vytvořené z vlastní disk se musí nacházet v hello stejný účet úložiště jako samotném disku hello
  * Vytvoření úložiště účet a kontejner toohold vlastní disku a vytvořené virtuální počítače
  * Po vytvoření všechny virtuální počítače, můžete bezpečně odstranit disk

Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `mydisks`.

<a id="prepimage"> </a>

## <a name="prepare-hello-disk-toobe-uploaded"></a>Příprava toobe disku hello nahrát
Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Následující články Hello provede jak tooprepare hello různé distribuce systému Linux, které jsou podporovány v Azure:

* **[Na základě centOS distribuce](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Další - neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Viz také hello  **[poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes)**  Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.

> [!NOTE]
> Hello [platformy Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) platí tooVMs běží Linux, jen pokud jeden z hello distribuce schválené se používá s podrobnosti konfigurace hello uvedeného v části "podporované verze v [Linux na schválené pro Azure Distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Skupiny prostředků logicky seskupit všechny prostředky Azure toosupport hello virtuálních počítačů, jako je například hello virtuální sítě a úložiště. Další informace o skupin prostředků, najdete v části [skupiny zdrojů přehled](../../azure-resource-manager/resource-group-overview.md). Před nahráním váš vlastní disk a vytváření virtuálních počítačů, musíte nejprve toocreate skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).

Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Vytvoření účtu úložiště vlastní disk a virtuální počítače s [vytvořit účet úložiště az](/cli/azure/storage/account#create). Všechny virtuální počítače s nespravované disky, které vytvoříte z toobe třeba vaše vlastní disku v hello stejný účet úložiště jako disk. 

Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount` ve skupině prostředků hello vytvořili:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Vypsat klíče účtu úložiště
Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště. Tyto přístupové klíče se používají při ověřování účtu úložiště toohello, jako je například toocarry se operace zápisu. Další informace o [Správa přístup toostorage zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Zobrazit přístupové klíče hello s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).

Zobrazit hello přístupové klíče pro účet úložiště hello, kterou jste vytvořili:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
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
Poznamenejte si `key1` jak ji budete používat toointeract se svým účtem úložiště v dalších krocích hello.

## <a name="create-a-storage-container"></a>Vytvoření kontejneru úložiště
V hello stejným způsobem, abyste vytvořili různé adresáře toologically uspořádat do místního systému souborů, vytvoření kontejnerů v rámci tooorganize účet úložiště vaše disky. Účet úložiště může obsahovat libovolný počet kontejnerů. Vytvořit kontejner s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create).

Hello následující příklad vytvoří kontejner s názvem `mydisks`:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>Nahrání virtuálního pevného disku
Teď nahrát vlastní disk s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload). Nahrání a ukládat vlastní disk jako objekt blob stránky.

Zadejte přístupový klíč, hello kontejneru, kterou jste vytvořili v předchozím kroku hello a pak hello cesta toohello vlastní disku v místním počítači:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-hello-vm"></a>Vytvoření hello virtuálních počítačů
toocreate virtuální počítač s nespravované disky, zadejte hello URI tooyour disk (`--image`) s [vytvořit virtuální počítač az](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí hello předtím nahrála virtuální disk:

Zadejte hello `--image` parametr s [vytvořit virtuální počítač az](/cli/azure/vm#create) toopoint tooyour vlastní disku. Ujistěte se, že `--storage-account` odpovídá hello účet úložiště, kde je uložený váš vlastní disk. Nemáte toouse hello stejném kontejneru jako hello vlastní disku toostore virtuální počítače. Zkontrolujte, zda toocreate žádné další kontejnery v hello stejným způsobem, jak hello dřívějších krocích před nahráním váš vlastní disk.

Hello následující příklad vytvoří virtuální počítač s názvem `myVM` z vlastní disku:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Stále nutné toospecify, nebo se zodpovědět vyzve k zadání, všechny hello další parametry, které vyžadují hello **vytvořit virtuální počítač az** příkaz například uživatelské jméno a klíče SSH.


## <a name="resource-manager-template"></a>Šablona Resource Manageru
Šablony Azure Resource Manageru jsou soubory JavaScript Object Notation (JSON), které definují hello prostředí, které chcete toobuild. Hello šablony jsou rozděleny u zprostředkovatelů prostředků toodifferent například výpočetní nebo sítích. Můžete použít existující šablony nebo napsat vlastní. Další informace o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).

V rámci hello `Microsoft.Compute/virtualMachines` poskytovatele šablony, máte `storageProfile` uzlu, který obsahuje podrobnosti o hello konfiguraci pro virtuální počítač. tooedit Hello dva hlavní parametry jsou hello `image` a `vhd` identifikátory URI, který bod vlastní tooyour a virtuální disk nového virtuálního počítače. Následující Hello ukazuje příklad hello JSON pro použití vlastního disku:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Můžete použít [toocreate tento existující šablony virtuálního počítače z vlastní image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) nebo přečtěte si o [vlastních šablon Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md). 

Jakmile máte šablonu nakonfigurována, použijte [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) toocreate virtuální počítače. Zadejte hello URI šablony JSON s hello `--template-uri` parametr:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Pokud máte soubor JSON, který je uložený místně v počítači, můžete použít hello `--template-file` parametr místo:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Další kroky
Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md). Můžete také příliš[přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nové virtuální počítače. Pokud máte aplikace spuštěné na virtuálních počítačů, je nutné, aby tooaccess, nezapomeňte příliš[otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

