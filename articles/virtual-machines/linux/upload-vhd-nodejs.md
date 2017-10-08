---
title: "aaaUpload vlastní image Linux s 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření a nahrání virtuálního pevného disku (VHD) tooAzure s vlastní image Linux pomocí modelu nasazení Resource Manager hello a hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
ms.openlocfilehash: 0bbd232debee1e632233d1c010e388dbc1548bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-hello-azure-cli-10"></a>Nahrání a vytvoření virtuálního počítače s Linuxem z image vlastní disku pomocí hello Azure CLI 1.0
Tento článek ukazuje, jak tooupload tooAzure virtuální pevný disk (VHD) pomocí hello modelu nasazení Resource Manager a vytvořit virtuální počítače s Linuxem z této vlastní image. Tato funkce vám umožní tooinstall a konfigurovat požadavky na tooyour distro Linux a pak použít tento virtuální pevný disk tooquickly vytvoření Azure virtuálních počítačů (VM).


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="quick-commands"></a>Rychlé příkazy
Pokud je třeba tooquickly dosáhnout hello, následující části Podrobnosti hello hello základní příkazy tooupload tooAzure virtuálních počítačů. Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#requirements).

Ujistěte se, že máte [hello Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:

```azurecli
azure config mode arm
```

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myimages`.

Nejprve vytvořte skupinu prostředků. Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `WestUs` umístění:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

Vytvořte virtuální disky toohold účet úložiště. Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount`:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Zobrazí seznam hello přístupové klíče pro účet úložiště. Poznamenejte si `key1`:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Vytvořte kontejner v rámci účtu úložiště pomocí klíče hello úložiště, který jste obdrželi. Hello následující příklad vytvoří kontejner s názvem `myimages` pomocí hello úložiště klíčů hodnoty z `key1`:

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Nakonec nahrajte váš kontejner toohello virtuálního pevného disku, kterou jste vytvořili. Zadejte místní cestu tooyour hello virtuální pevný disk v části `/path/to/disk/mydisk.vhd`:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Nyní můžete vytvořit virtuální počítač z nahraný virtuální disk [pomocí šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Můžete také použít hello rozhraní příkazového řádku zadáním hello URI tooyour disku (`--image-urn`). Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí hello předtím nahrála virtuální disk:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Hello cílový účet úložiště obsahuje toobe hello stejné jako kde jste nahráli virtuální disk. Můžete také potřebovat toospecify nebo zodpovědět vyzve k zadání, všechny hello další parametry, které vyžadují hello `azure vm create` příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH. Další informace o hello [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Požadavky
toocomplete hello následující kroky, budete potřebovat:

* **Operační systém Linux nainstalován v souboru VHD** -nainstalovat [distribuce schválené pro Azure Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuálního disku v hello virtuálního pevného disku formát. Několik nástrojů existují toocreate virtuálního počítače a virtuálního pevného disku:
  * Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), dbejte na to toouse virtuálního pevného disku jako formát obrázku. V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.
  * Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Hello novější formát VHDX není podporovaný v Azure. Když vytvoříte virtuální počítač, zadejte hello Formát virtuálního pevného disku. V případě potřeby můžete převést pomocí tooVHD disky VHDX [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell. Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je třeba tooconvert takové toostatic disky VHD před nahráním. Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert během procesu hello odesílání tooAzure dynamické disky.
> 
> 

* Virtuální počítače vytvořené pomocí vlastní image se musí nacházet v hello stejný účet úložiště jako samotný obraz hello
  * Vytvoření úložiště účet a kontejner toohold vlastní image a vytvořené virtuální počítače
  * Po vytvoření všechny virtuální počítače, můžete bezpečně odstranit image

Ujistěte se, že máte [hello Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení a použití režimu Resource Manager:

```azurecli
azure config mode arm
```

Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami. Názvy parametrů příklad zahrnuté `myResourceGroup`, `mystorageaccount`, a `myimages`.

<a id="prepimage"> </a>

## <a name="prepare-hello-image-toobe-uploaded"></a>Příprava toobe hello image nahrát
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


## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Skupiny prostředků logicky seskupit všechny prostředky Azure toosupport hello virtuálních počítačů, jako je například hello virtuální sítě a úložiště. Další informace o [skupin prostředků Azure zde](../../azure-resource-manager/resource-group-overview.md). Před nahráním image vlastní disku a vytváření virtuálních počítačů, je nutné nejprve toocreate skupinu prostředků. 

Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `WestUS` umístění:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
Virtuální počítače jsou uloženy jako objekty BLOB stránky v rámci účtu úložiště. Další informace o [zde úložiště objektů blob Azure](../../storage/common/storage-introduction.md#blob-storage). Můžete vytvořit účet úložiště pro image vlastní disku a virtuální počítače. Všechny virtuální počítače, které vytvoříte z vaší vlastní disku image nutné toobe v hello stejný účet úložiště jako této bitové kopie.

Hello následující příklad vytvoří účet úložiště s názvem `mystorageaccount` ve skupině prostředků hello vytvořili:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Vypsat klíče účtu úložiště
Vygeneruje Azure dva 512bitové přístupové klíče pro každý účet úložiště. Tyto přístupové klíče se používají při ověřování účtu úložiště toohello, jako je například toocarry se operace zápisu. Další informace o [Správa přístup toostorage zde](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Můžete zobrazit přístupové klíče s hello `azure storage account keys list` příkaz.

Zobrazit hello přístupové klíče pro účet úložiště hello, kterou jste vytvořili:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
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
V hello stejným způsobem, abyste vytvořili různé adresáře toologically uspořádat do místního systému souborů, vytvoření kontejnerů v rámci účtu tooorganize úložiště virtuálních disků a bitové kopie. Účet úložiště může obsahovat libovolný počet kontejnerů. 

Hello následující příklad vytvoří kontejner s názvem `myimages`, zadání hello přístupový klíč získaných v předchozím kroku hello (`key1`):

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Nahrání virtuálního pevného disku
Nyní můžete ve skutečnosti nahrát vlastní disku image. S všechny virtuální disky, které jsou používány virtuálními počítači, můžete nahrát a uložení bitové kopie disku vlastní jako objekt blob stránky.

Zadejte přístupový klíč, hello kontejneru, kterou jste vytvořili v předchozím kroku hello a pak hello image vlastní disku toohello cestu v místním počítači:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Vytvoření virtuálního počítače z vlastní image
Když vytvoříte virtuální počítače z image vlastní disk, zadejte hello bitové kopie disku toohello identifikátor URI. Ujistěte se, že hello cílového úložiště účet shoduje se uloží vlastní disku image. Můžete vytvořit virtuální počítač pomocí šablony Azure CLI nebo Resource Manager JSON hello.

### <a name="create-a-vm-using-hello-azure-cli"></a>Vytvoření virtuálního počítače pomocí hello rozhraní příkazového řádku Azure
Zadejte hello `--image-urn` parametr s hello `azure vm create` příkaz toopoint tooyour vlastní disku image. Ujistěte se, že `--storage-account-name` odpovídá hello účet úložiště, kde je uložena bitová kopie vaše vlastní disku. Nemáte toouse hello stejném kontejneru jako hello toostore bitové kopie disku vlastní virtuální počítače. Zkontrolujte, zda toocreate žádné další kontejnery v hello stejným způsobem, jak hello dřívějších krocích před nahráním vaše vlastní disku Image.

Hello následující příklad vytvoří virtuální počítač s názvem `myVM` z bitové kopie disku vlastní:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Stále nutné toospecify, nebo se zodpovědět vyzve k zadání, všechny hello další parametry, které vyžadují hello `azure vm create` příkaz například virtuální sítě, veřejnou IP adresu, uživatelské jméno a klíče SSH. Další informace o hello [dostupné parametry příkazového řádku Resource Manager](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Vytvoření virtuálního počítače pomocí šablony JSON
Šablony Azure Resource Manageru jsou soubory JavaScript Object Notation (JSON), které definují hello prostředí, které chcete toobuild. Hello šablony jsou rozděleny u zprostředkovatelů prostředků toodifferent například výpočetní nebo sítích. Můžete použít existující šablony nebo napsat vlastní. Další informace o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md).

V rámci hello `Microsoft.Compute/virtualMachines` poskytovatele šablony, máte `storageProfile` uzlu, který obsahuje podrobnosti o hello konfiguraci pro virtuální počítač. tooedit Hello dva hlavní parametry jsou hello `image` a `vhd` identifikátory URI, který bod image vlastní disku tooyour a virtuální disk nového virtuálního počítače. Následující Hello ukazuje příklad hello JSON pro použití image vlastní disku:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Můžete použít [toocreate tento existující šablony virtuálního počítače z vlastní image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) nebo přečtěte si o [vlastních šablon Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md). 

Jakmile máte nakonfigurované šablony, vytvoříte virtuální počítače pomocí hello `azure group deployment create` příkaz. Zadejte hello URI šablony JSON s hello `--template-uri` parametr:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Pokud máte soubor JSON, který je uložený místně v počítači, můžete použít hello `--template-file` parametr místo:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Další kroky
Po připravené a nahrát vlastní virtuální disk, si můžete přečíst více o [pomocí Resource Manageru a šablony](../../azure-resource-manager/resource-group-overview.md). Můžete také příliš[přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nové virtuální počítače. Pokud máte aplikace spuštěné na virtuálních počítačů, je nutné, aby tooaccess, nezapomeňte příliš[otevřete porty a koncové body](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

