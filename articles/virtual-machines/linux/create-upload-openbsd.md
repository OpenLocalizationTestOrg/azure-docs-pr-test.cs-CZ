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
# <a name="create-and-upload-an-openbsd-disk-image-tooazure"></a><span data-ttu-id="c0e4d-103">Vytvoření a odeslání tooAzure bitové kopie disku OpenBSD</span><span class="sxs-lookup"><span data-stu-id="c0e4d-103">Create and Upload an OpenBSD disk image tooAzure</span></span>
<span data-ttu-id="c0e4d-104">Tento článek ukazuje, jak toocreate a nahrajte virtuální pevný disk (VHD), který obsahuje hello OpenBSD operačního systému.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-104">This article shows you how toocreate and upload a virtual hard disk (VHD) that contains hello OpenBSD operating system.</span></span> <span data-ttu-id="c0e4d-105">Po odeslání, můžete jej jako vlastní image toocreate virtuální počítač (VM) v Azure pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-105">After you upload it, you can use it as your own image toocreate a virtual machine (VM) in Azure through Azure CLI.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c0e4d-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0e4d-106">Prerequisites</span></span>
<span data-ttu-id="c0e4d-107">Tento článek předpokládá, že máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-107">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="c0e4d-108">**Předplatné Azure** – Pokud nemáte účet, můžete vytvořit jeden si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-108">**An Azure subscription** - If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="c0e4d-109">Pokud máte předplatné MSDN, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-109">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="c0e4d-110">Jinak, zjistěte, jak příliš[vytvořit Bezplatný zkušební účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-110">Otherwise, learn how too[create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="c0e4d-111">**Azure CLI 2.0** -zajistěte, aby byla hello nejnovější [Azure CLI 2.0](/cli/azure/install-azure-cli) nainstalován a přihlášení tooyour účet Azure s [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-111">**Azure CLI 2.0** - Make sure you have hello latest [Azure CLI 2.0](/cli/azure/install-azure-cli) installed and logged in tooyour Azure account with [az login](/cli/azure/#login).</span></span>
* <span data-ttu-id="c0e4d-112">**OpenBSD operační systém nainstalovaný v souboru VHD** -podporovaný operační systém OpenBSD (verze 6.1) musí být nainstalované tooa virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-112">**OpenBSD operating system installed in a .vhd file** - A supported OpenBSD operating system (6.1 version) must be installed tooa virtual hard disk.</span></span> <span data-ttu-id="c0e4d-113">Několik nástrojů existovat toocreate soubory VHD.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-113">Multiple tools exist toocreate .vhd files.</span></span> <span data-ttu-id="c0e4d-114">Můžete například použít řešení virtualizace jako soubor VHD hello toocreate technologie Hyper-V a nainstalujte hello operační systém.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-114">For example, you can use a virtualization solution such as Hyper-V toocreate hello .vhd file and install hello operating system.</span></span> <span data-ttu-id="c0e4d-115">Pokyny o tom, jak tooinstall a použití technologie Hyper-V, najdete v části [instalaci technologie Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-115">For instructions about how tooinstall and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>


## <a name="prepare-openbsd-image-for-azure"></a><span data-ttu-id="c0e4d-116">Příprava OpenBSD image pro Azure</span><span class="sxs-lookup"><span data-stu-id="c0e4d-116">Prepare OpenBSD image for Azure</span></span>
<span data-ttu-id="c0e4d-117">Na hello virtuálních počítačů, které jste nainstalovali hello OpenBSD operačního systému 6.1, která přidá podporu technologie Hyper-V, proveďte následující postupy hello:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-117">On hello VM where you installed hello OpenBSD operating system 6.1, which added Hyper-V support, complete hello following procedures:</span></span>

1. <span data-ttu-id="c0e4d-118">Pokud během instalace není povolený DHCP, povolte hello služby následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-118">If DHCP is not enabled during installation, enable hello service as follows:</span></span>

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. <span data-ttu-id="c0e4d-119">Nastavení konzoly sériového portu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-119">Set up a serial console as follows:</span></span>

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. <span data-ttu-id="c0e4d-120">Instalace balíčku nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-120">Configure Package installation as follows:</span></span>

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. <span data-ttu-id="c0e4d-121">Ve výchozím nastavení, hello `root` je uživatel zakázán na virtuálních počítačích v Azure.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-121">By default, hello `root` user is disabled on virtual machines in Azure.</span></span> <span data-ttu-id="c0e4d-122">Uživatelé mohou spouštět příkazy se zvýšeným oprávněním pomocí hello `doas` příkaz OpenBSD virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-122">Users can run commands with elevated privileges by using hello `doas` command on OpenBSD VM.</span></span> <span data-ttu-id="c0e4d-123">Doas je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-123">Doas is enabled by default.</span></span> <span data-ttu-id="c0e4d-124">Další informace najdete v tématu [doas.conf](http://man.openbsd.org/doas.conf.5).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-124">For more information, see [doas.conf](http://man.openbsd.org/doas.conf.5).</span></span> 

5. <span data-ttu-id="c0e4d-125">Nainstalujte a nakonfigurujte předpoklady pro hello Azure Agent následovně:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-125">Install and configure prerequisites for hello Azure Agent as follows:</span></span>

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. <span data-ttu-id="c0e4d-126">Hello nejnovější verzi agenta Azure hello vždy najdete na [Githubu](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-126">hello latest release of hello Azure agent can always be found on [Github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="c0e4d-127">Nainstalujte agenta hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-127">Install hello agent as follows:</span></span>

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="c0e4d-128">Po instalaci agenta Azure, je vhodné tooverify, který používá následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-128">After you install Azure Agent, it's a good idea tooverify that it's running as follows:</span></span>
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. <span data-ttu-id="c0e4d-129">Deprovision hello systému tooclean ho a zkontrolujte ji vhodný pro reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-129">Deprovision hello system tooclean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="c0e4d-130">Hello následující příkaz dojde také k odstranění hello poslední zřízené uživatelský účet a hello související data:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-130">hello following command also deletes hello last provisioned user account and hello associated data:</span></span>

    ```sh
    waagent -deprovision+user -force
    ```

<span data-ttu-id="c0e4d-131">Nyní můžete vypnout virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-131">Now you can shut down your VM.</span></span>


## <a name="prepare-hello-vhd"></a><span data-ttu-id="c0e4d-132">Příprava hello virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="c0e4d-132">Prepare hello VHD</span></span>
<span data-ttu-id="c0e4d-133">Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-133">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span> <span data-ttu-id="c0e4d-134">Můžete převést hello disku toofixed Formát virtuálního pevného disku pomocí Správce technologie Hyper-V nebo prostředí Powershell text hello [convert-VHD prostředí](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) rutiny.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-134">You can convert hello disk toofixed VHD format using Hyper-V Manager or hello Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet.</span></span> <span data-ttu-id="c0e4d-135">Příkladem je jako následující.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-135">An example is as following.</span></span>

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a><span data-ttu-id="c0e4d-136">Vytvořit prostředky úložiště a odeslat</span><span class="sxs-lookup"><span data-stu-id="c0e4d-136">Create storage resources and upload</span></span>
<span data-ttu-id="c0e4d-137">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-137">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="c0e4d-138">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-138">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="c0e4d-139">tooupload svůj disk VHD, vytvořit účet úložiště s [vytvořit účet úložiště az](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-139">tooupload your VHD, create a storage account with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="c0e4d-140">Názvy účtů úložiště musí být jedinečný, takže zadejte vlastní název.</span><span class="sxs-lookup"><span data-stu-id="c0e4d-140">Storage account names must be unique, so provide your own name.</span></span> <span data-ttu-id="c0e4d-141">Hello následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště*:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-141">hello following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

<span data-ttu-id="c0e4d-142">toocontrol přístup k účtu úložiště toohello, získejte hello klíč úložiště s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-142">toocontrol access toohello storage account, obtain hello storage key with [az storage account keys list](/cli/azure/storage/account/keys#list) as follows:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

<span data-ttu-id="c0e4d-143">samostatné hello toologically virtuální pevné disky nahrajete, vytvořit kontejner v rámci účtu úložiště hello s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create):</span><span class="sxs-lookup"><span data-stu-id="c0e4d-143">toologically separate hello VHDs you upload, create a container within hello storage account with [az storage container create](/cli/azure/storage/container#create):</span></span>

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

<span data-ttu-id="c0e4d-144">Nakonec nahrát svůj disk VHD s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-144">Finally, upload your VHD with [az storage blob upload](/cli/azure/storage/blob#upload) as follows:</span></span>

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a><span data-ttu-id="c0e4d-145">Vytvoření virtuálního počítače z vaší virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="c0e4d-145">Create VM from your VHD</span></span>
<span data-ttu-id="c0e4d-146">Můžete vytvořit virtuální počítač s [ukázkový skript](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) nebo přímo pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-146">You can create a VM with a [sample script](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) or directly with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="c0e4d-147">toospecify hello OpenBSD virtuální pevný disk, který jste nahráli, použijte hello `--image` parametr následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-147">toospecify hello OpenBSD VHD you uploaded, use hello `--image` parameter as follows:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="c0e4d-148">Získat hello IP adresu pro virtuální počítač OpenBSD s [az virtuálních počítačů seznamu ip-addresses](/cli/azure/vm#list-ip-addresses) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-148">Obtain hello IP address for your OpenBSD VM with [az vm list-ip-addresses](/cli/azure/vm#list-ip-addresses) as follows:</span></span>

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

<span data-ttu-id="c0e4d-149">Nyní můžete SSH tooyour OpenBSD virtuální počítač jako za normálních okolností:</span><span class="sxs-lookup"><span data-stu-id="c0e4d-149">Now you can SSH tooyour OpenBSD VM as normal:</span></span>
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a><span data-ttu-id="c0e4d-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0e4d-150">Next steps</span></span>
<span data-ttu-id="c0e4d-151">Pokud chcete další informace o Hyper-V podporují na OpenBSD6.1 tooknow, přečtěte si [OpenBSD 6.1](https://www.openbsd.org/61.html) a [hyperv.4](http://man.openbsd.org/hyperv.4).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-151">If you want tooknow more about Hyper-V support on OpenBSD6.1, read [OpenBSD 6.1](https://www.openbsd.org/61.html) and [hyperv.4](http://man.openbsd.org/hyperv.4).</span></span>

<span data-ttu-id="c0e4d-152">Pokud chcete, aby toocreate virtuálního počítače ze spravovaných disků, přečtěte si [disku az](/cli/azure/disk).</span><span class="sxs-lookup"><span data-stu-id="c0e4d-152">If you want toocreate a VM from managed disk, read [az disk](/cli/azure/disk).</span></span> 
