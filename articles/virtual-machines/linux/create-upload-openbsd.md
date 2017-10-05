---
title: "Vytvořit a odeslat bitovou kopii virtuálního počítače OpenBSD do Azure | Microsoft Docs"
description: "Zjistěte, jak vytvořit a odeslat virtuální pevný disk (VHD), který obsahuje operační systém OpenBSD k vytvoření virtuálního počítače Azure pomocí rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: 716c07f6a738189d6cf2b3caafa16b753927d182
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-and-upload-an-openbsd-disk-image-to-azure"></a><span data-ttu-id="4e59f-103">Vytvořit a odeslat bitovou kopii disku OpenBSD do Azure</span><span class="sxs-lookup"><span data-stu-id="4e59f-103">Create and Upload an OpenBSD disk image to Azure</span></span>
<span data-ttu-id="4e59f-104">Tento článek ukazuje, jak vytvořit a odeslat virtuální pevný disk (VHD), který obsahuje OpenBSD operační systém.</span><span class="sxs-lookup"><span data-stu-id="4e59f-104">This article shows you how to create and upload a virtual hard disk (VHD) that contains the OpenBSD operating system.</span></span> <span data-ttu-id="4e59f-105">Po odeslání, můžete ho použít jako vlastní image pro vytvoření virtuálního počítače (VM) v Azure pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4e59f-105">After you upload it, you can use it as your own image to create a virtual machine (VM) in Azure through Azure CLI.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="4e59f-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4e59f-106">Prerequisites</span></span>
<span data-ttu-id="4e59f-107">Tento článek předpokládá, že máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="4e59f-107">This article assumes that you have the following items:</span></span>

* <span data-ttu-id="4e59f-108">**Předplatné Azure** – Pokud nemáte účet, můžete vytvořit jeden si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="4e59f-108">**An Azure subscription** - If you don't have an account, you can create one in just a couple of minutes.</span></span> <span data-ttu-id="4e59f-109">Pokud máte předplatné MSDN, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="4e59f-109">If you have an MSDN subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span> <span data-ttu-id="4e59f-110">Jinak, zjistěte, jak [vytvořit Bezplatný zkušební účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4e59f-110">Otherwise, learn how to [create a free trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span>  
* <span data-ttu-id="4e59f-111">**Azure CLI 2.0** – Ujistěte se, že máte nejnovější [Azure CLI 2.0](/cli/azure/install-azure-cli) nainstalován a přihlášení k účtu Azure s [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4e59f-111">**Azure CLI 2.0** - Make sure you have the latest [Azure CLI 2.0](/cli/azure/install-azure-cli) installed and logged in to your Azure account with [az login](/cli/azure/#login).</span></span>
* <span data-ttu-id="4e59f-112">**OpenBSD operační systém nainstalovaný v souboru VHD** -podporovaný operační systém OpenBSD (verze 6.1) musí být nainstalována na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="4e59f-112">**OpenBSD operating system installed in a .vhd file** - A supported OpenBSD operating system (6.1 version) must be installed to a virtual hard disk.</span></span> <span data-ttu-id="4e59f-113">Postup vytvoření souborů VHD existovat několik nástrojů.</span><span class="sxs-lookup"><span data-stu-id="4e59f-113">Multiple tools exist to create .vhd files.</span></span> <span data-ttu-id="4e59f-114">Řešení virtualizace, jako je například technologie Hyper-V můžete například použít k vytvoření souboru VHD a instalace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4e59f-114">For example, you can use a virtualization solution such as Hyper-V to create the .vhd file and install the operating system.</span></span> <span data-ttu-id="4e59f-115">Pokyny o tom, jak nainstalovat a používat technologie Hyper-V najdete v tématu [instalaci technologie Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e59f-115">For instructions about how to install and use Hyper-V, see [Install Hyper-V and create a virtual machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>


## <a name="prepare-openbsd-image-for-azure"></a><span data-ttu-id="4e59f-116">Příprava OpenBSD image pro Azure</span><span class="sxs-lookup"><span data-stu-id="4e59f-116">Prepare OpenBSD image for Azure</span></span>
<span data-ttu-id="4e59f-117">Na virtuálním počítači, kam jste nainstalovali operační systém OpenBSD 6.1, která přidá technologie Hyper-V podporují, proveďte následující postupy:</span><span class="sxs-lookup"><span data-stu-id="4e59f-117">On the VM where you installed the OpenBSD operating system 6.1, which added Hyper-V support, complete the following procedures:</span></span>

1. <span data-ttu-id="4e59f-118">Pokud během instalace není povolený DHCP, povolte službu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-118">If DHCP is not enabled during installation, enable the service as follows:</span></span>

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. <span data-ttu-id="4e59f-119">Nastavení konzoly sériového portu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-119">Set up a serial console as follows:</span></span>

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. <span data-ttu-id="4e59f-120">Instalace balíčku nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-120">Configure Package installation as follows:</span></span>

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. <span data-ttu-id="4e59f-121">Ve výchozím nastavení `root` je uživatel zakázán na virtuálních počítačích v Azure.</span><span class="sxs-lookup"><span data-stu-id="4e59f-121">By default, the `root` user is disabled on virtual machines in Azure.</span></span> <span data-ttu-id="4e59f-122">Uživatelé mohou spouštět příkazy se zvýšeným oprávněním pomocí `doas` příkaz OpenBSD virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4e59f-122">Users can run commands with elevated privileges by using the `doas` command on OpenBSD VM.</span></span> <span data-ttu-id="4e59f-123">Doas je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="4e59f-123">Doas is enabled by default.</span></span> <span data-ttu-id="4e59f-124">Další informace najdete v tématu [doas.conf](http://man.openbsd.org/doas.conf.5).</span><span class="sxs-lookup"><span data-stu-id="4e59f-124">For more information, see [doas.conf](http://man.openbsd.org/doas.conf.5).</span></span> 

5. <span data-ttu-id="4e59f-125">Nainstalujte a nakonfigurujte požadavky pro Azure Agent následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-125">Install and configure prerequisites for the Azure Agent as follows:</span></span>

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. <span data-ttu-id="4e59f-126">Nejnovější verzi agenta Azure vždy najdete na [Githubu](https://github.com/Azure/WALinuxAgent/releases).</span><span class="sxs-lookup"><span data-stu-id="4e59f-126">The latest release of the Azure agent can always be found on [Github](https://github.com/Azure/WALinuxAgent/releases).</span></span> <span data-ttu-id="4e59f-127">Nainstalujte agenta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-127">Install the agent as follows:</span></span>

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="4e59f-128">Po instalaci agenta Azure, je vhodné ověřit, zda je spuštěna následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-128">After you install Azure Agent, it's a good idea to verify that it's running as follows:</span></span>
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. <span data-ttu-id="4e59f-129">Zrušení zřízení systému a vyčistit ho a nastavit jej jako vhodný pro reprovisioning.</span><span class="sxs-lookup"><span data-stu-id="4e59f-129">Deprovision the system to clean it and make it suitable for reprovisioning.</span></span> <span data-ttu-id="4e59f-130">Následující příkaz taky odstraní poslední zřízené uživatelský účet a přidružená data:</span><span class="sxs-lookup"><span data-stu-id="4e59f-130">The following command also deletes the last provisioned user account and the associated data:</span></span>

    ```sh
    waagent -deprovision+user -force
    ```

<span data-ttu-id="4e59f-131">Nyní můžete vypnout virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4e59f-131">Now you can shut down your VM.</span></span>


## <a name="prepare-the-vhd"></a><span data-ttu-id="4e59f-132">Příprava virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="4e59f-132">Prepare the VHD</span></span>
<span data-ttu-id="4e59f-133">Formát VHDX není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="4e59f-133">The VHDX format is not supported in Azure, only **fixed VHD**.</span></span> <span data-ttu-id="4e59f-134">Můžete převést disk na pevnou Formát virtuálního pevného disku pomocí Správce technologie Hyper-V nebo prostředí Powershell [convert-VHD prostředí](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) rutiny.</span><span class="sxs-lookup"><span data-stu-id="4e59f-134">You can convert the disk to fixed VHD format using Hyper-V Manager or the Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet.</span></span> <span data-ttu-id="4e59f-135">Příkladem je jako následující.</span><span class="sxs-lookup"><span data-stu-id="4e59f-135">An example is as following.</span></span>

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a><span data-ttu-id="4e59f-136">Vytvořit prostředky úložiště a odeslat</span><span class="sxs-lookup"><span data-stu-id="4e59f-136">Create storage resources and upload</span></span>
<span data-ttu-id="4e59f-137">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4e59f-137">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4e59f-138">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="4e59f-138">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4e59f-139">Pokud chcete nahrát svůj disk VHD, vytvořit účet úložiště s [vytvořit účet úložiště az](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="4e59f-139">To upload your VHD, create a storage account with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="4e59f-140">Názvy účtů úložiště musí být jedinečný, takže zadejte vlastní název.</span><span class="sxs-lookup"><span data-stu-id="4e59f-140">Storage account names must be unique, so provide your own name.</span></span> <span data-ttu-id="4e59f-141">Následující příklad vytvoří účet úložiště s názvem *můj_účet_úložiště*:</span><span class="sxs-lookup"><span data-stu-id="4e59f-141">The following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

<span data-ttu-id="4e59f-142">Chcete-li řídit přístup k účtu úložiště, získat klíč úložiště s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-142">To control access to the storage account, obtain the storage key with [az storage account keys list](/cli/azure/storage/account/keys#list) as follows:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

<span data-ttu-id="4e59f-143">Logicky jednotlivé virtuální pevné disky můžete nahrávat na server, vytvořit kontejner v rámci účtu úložiště s [vytvořit kontejner úložiště az](/cli/azure/storage/container#create):</span><span class="sxs-lookup"><span data-stu-id="4e59f-143">To logically separate the VHDs you upload, create a container within the storage account with [az storage container create](/cli/azure/storage/container#create):</span></span>

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

<span data-ttu-id="4e59f-144">Nakonec nahrát svůj disk VHD s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-144">Finally, upload your VHD with [az storage blob upload](/cli/azure/storage/blob#upload) as follows:</span></span>

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a><span data-ttu-id="4e59f-145">Vytvoření virtuálního počítače z vaší virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="4e59f-145">Create VM from your VHD</span></span>
<span data-ttu-id="4e59f-146">Můžete vytvořit virtuální počítač s [ukázkový skript](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) nebo přímo pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="4e59f-146">You can create a VM with a [sample script](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) or directly with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="4e59f-147">Pokud chcete zadat OpenBSD virtuální pevný disk, který jste nahráli, použijte `--image` parametr následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-147">To specify the OpenBSD VHD you uploaded, use the `--image` parameter as follows:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

<span data-ttu-id="4e59f-148">Získat IP adresu pro virtuální počítač OpenBSD s [az virtuálních počítačů seznamu ip-addresses](/cli/azure/vm#list-ip-addresses) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e59f-148">Obtain the IP address for your OpenBSD VM with [az vm list-ip-addresses](/cli/azure/vm#list-ip-addresses) as follows:</span></span>

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

<span data-ttu-id="4e59f-149">Nyní můžete SSH k virtuálnímu počítači OpenBSD jako za normálních okolností:</span><span class="sxs-lookup"><span data-stu-id="4e59f-149">Now you can SSH to your OpenBSD VM as normal:</span></span>
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a><span data-ttu-id="4e59f-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e59f-150">Next steps</span></span>
<span data-ttu-id="4e59f-151">Pokud chcete získat další informace o podporu technologie Hyper-V na OpenBSD6.1, přečtěte si [OpenBSD 6.1](https://www.openbsd.org/61.html) a [hyperv.4](http://man.openbsd.org/hyperv.4).</span><span class="sxs-lookup"><span data-stu-id="4e59f-151">If you want to know more about Hyper-V support on OpenBSD6.1, read [OpenBSD 6.1](https://www.openbsd.org/61.html) and [hyperv.4](http://man.openbsd.org/hyperv.4).</span></span>

<span data-ttu-id="4e59f-152">Pokud chcete vytvořit virtuální počítač ze spravovaných disků, přečtěte si [disku az](/cli/azure/disk).</span><span class="sxs-lookup"><span data-stu-id="4e59f-152">If you want to create a VM from managed disk, read [az disk](/cli/azure/disk).</span></span> 