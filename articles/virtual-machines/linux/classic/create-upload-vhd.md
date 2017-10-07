---
title: "aaaCreate a nahrání virtuálního pevného disku Linux tooAzure | Microsoft Docs"
description: "Vytvoření a odeslání Azure virtuálního pevného disku (VHD) obsahující operační systém Linux hello pomocí modelu nasazení Classic hello"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: iainfou
ms.openlocfilehash: 77b01316386c4a6eb68c129fa68d42f0a8996edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-hello-linux-operating-system"></a>Vytvoření a nahrání virtuálního pevného disku tohoto hello obsahuje operační systém Linux
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Můžete také [nahrajte image vlastní disku pomocí Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tento článek ukazuje, jak toocreate a nahrání virtuálního pevného disku (VHD), abyste ho mohli používat jako vlastní image toocreate virtuálních počítačů v Azure. Zjistěte, jak tooprepare hello operačního systému, abyste ji mohli používat toocreate více virtuálních počítačů na základě této bitové kopie. 


## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte hello následující položky:

* **Operační systém Linux nainstalován v souboru VHD** -instalaci [distribuce schválené pro Azure Linux](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (nebo v tématu [informace pro neschválené distribuce](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuálního disku ve formátu VHD hello. Několik nástrojů existují toocreate virtuálního počítače a virtuálního pevného disku:
  * Instalace a konfigurace [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) nebo [KVM](http://www.linux-kvm.org/page/RunningKVM), dbejte na to toouse virtuálního pevného disku jako formát obrázku. V případě potřeby můžete [převést bitovou kopii](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) pomocí `qemu-img convert`.
  * Můžete také použít technologie Hyper-V [ve Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) nebo [v systému Windows Server 2012 nebo 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Hello novější formát VHDX není podporovaný v Azure. Když vytvoříte virtuální počítač, zadejte hello Formát virtuálního pevného disku. V případě potřeby můžete převést pomocí tooVHD disky VHDX [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) nebo hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) rutiny prostředí PowerShell. Navíc Azure nepodporuje odesílání dynamické virtuální pevné disky, takže je třeba tooconvert takové toostatic disky VHD před nahráním. Můžete použít nástroje, jako [nástroje Azure virtuálního pevného disku pro přejděte](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert během procesu hello odesílání tooAzure dynamické disky.

* **Rozhraní příkazového řádku Azure** -instalace hello nejnovější [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello virtuálního pevného disku.

<a id="prepimage"> </a>

## <a name="step-1-prepare-hello-image-toobe-uploaded"></a>Krok 1: Příprava toobe hello image nahrát
Azure podporuje různé Linuxových distribucích (viz [distribuce schválené](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Následující články Hello provede jak tooprepare hello různé distribuce systému Linux, které jsou podporovány v Azure. Po dokončení kroků hello v následujících příručkách hello pocházet vraťte se sem, až budete mít soubor virtuálního pevného disku, který je připraven tooupload tooAzure:

* **[Na základě centOS distribuce](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Další - neschválené distribuce](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

> [!NOTE]
> Hello platformy Azure SLA se vztahuje toovirtual počítače běžící hello operační systém Linux jenom v případě, že distribuce schválené pro jednu z hello používá s podrobnosti konfigurace hello uvedeného v části "podporované verze na [Linux na Azure-Endorsed distribuce ](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Všechny Linuxových distribucích v galerii Azure image hello jsou potvrzená distribuce hello požadovanou konfigurací.
> 
> 

Viz také hello  **[poznámky k instalaci Linux](../create-upload-generic.md#general-linux-installation-notes)**  Příprava bitové kopie systému Linux na Azure další Obecné tipy pro.

<a id="connect"> </a>

## <a name="step-2-prepare-hello-connection-tooazure"></a>Krok 2: Příprava tooAzure připojení hello
Zajistěte, aby používáte hello rozhraní příkazového řádku Azure v modelu nasazení classic hello (`azure config mode asm`), potom se přihlaste v tooyour účtu:

```azurecli
azure login
```


<a id="upload"> </a>

## <a name="step-3-upload-hello-image-tooazure"></a>Krok 3: Nahrát tooAzure hello bitové kopie
Je nutné souboru virtuálního pevného disku na tooupload účet úložiště. Můžete vybrat buď existující účet úložiště nebo [vytvořte novou](../../../storage/common/storage-create-storage-account.md).

Použijte bitovou kopii hello hello rozhraní příkazového řádku Azure tooupload pomocí hello následující příkaz:

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

V předchozím příkladu hello:

* **BlobStorageURL** hello adresa URL pro účet úložiště hello plánování toouse
* **YourImagesFolder** je hello kontejneru v úložiště objektů blob místo toostore obrázků
* **VHDName** je hello štítek, který se zobrazí v portálu tooidentify hello virtuální pevný disk.
* **PathToVHDFile** je hello úplná cesta a název souboru VHD hello na váš počítač.

Hello následující příkaz ukazuje kompletní příklad:

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-hello-image"></a>Krok 4: Vytvoření virtuálního počítače z hello image
Vytvoříte virtuální počítač pomocí `azure vm create` v hello stejným způsobem jako regulární virtuální počítač. Zadejte název hello dáte bitové kopie v předchozím kroku hello. V následujícím příkladu hello, použijeme hello **myImage** uveden v předchozím kroku hello název bitové kopie:

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

toocreate vlastní virtuální počítače, zadejte vlastní uživatelské jméno + heslo, umístění, název DNS a název bitové kopie.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [referenční dokumentace rozhraní příkazového řádku Azure pro model nasazení Azure classic hello](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

[Step 1: Prepare hello image toobe uploaded]:#prepimage
[Step 2: Prepare hello connection tooAzure]:#connect
[Step 3: Upload hello image tooAzure]:#upload
