---
title: "Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 1.0 | Dokumentace Microsoftu"
description: "Vytvoření virtuálního počítače s Linuxem v Azure pomocí Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
ms.assetid: facb1115-2b4e-4ef3-9905-330e42beb686
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: 
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2016
ms.author: v-livech
ms.openlocfilehash: ec1b34e4f539d2e95bb1f99fca3a6a0ec682ef51
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-linux-vm-using-the-azure-cli-10"></a><span data-ttu-id="d837f-103">Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d837f-103">Create a Linux VM using the Azure CLI 1.0</span></span>

<span data-ttu-id="d837f-104">Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem na platformě Azure pomocí příkazu `azure vm quick-create` v rozhraní příkazového řádku (CLI) Azure.</span><span class="sxs-lookup"><span data-stu-id="d837f-104">This article shows how to quickly deploy a Linux virtual machine (VM) on Azure by using the `azure vm quick-create` command in the Azure command-line interface (CLI).</span></span> <span data-ttu-id="d837f-105">Příkaz `quick-create` nasadí virtuální počítač se základní zabezpečenou infrastrukturou, který můžete použít k rychlému vytvoření prototypu nebo otestování konceptu.</span><span class="sxs-lookup"><span data-stu-id="d837f-105">The `quick-create` command deploys a VM inside a basic, secure infrastructure that you can use to prototype or test a concept rapidly.</span></span>

> [!NOTE]
<span data-ttu-id="d837f-106">Informace o vytvoření virtuálního počítače pomocí Azure CLI 2.0 najdete v tématu [Vytvoření virtuálního počítače pomocí Azure CLI](../windows/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d837f-106">To create a VM using the Azure CLI 2.0, see [Create a VM with the Azure CLI](../windows/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d837f-107">Virtuální počítač s Linuxem můžete rychle nasadit také pomocí webu [Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d837f-107">You can also quickly deploy a Linux VM by using the [Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d837f-108">Článek vyžaduje [SSH soubory veřejného a privátního klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d837f-108">The article requires an [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="quick-commands"></a><span data-ttu-id="d837f-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="d837f-109">Quick commands</span></span>

<span data-ttu-id="d837f-110">Následující příklad ukazuje, jak nasadit virtuální počítač s CoreOS a připojit klíč SSH (Secure Shell). Vaše argumenty ale mohou být jiné:</span><span class="sxs-lookup"><span data-stu-id="d837f-110">The following example shows how to deploy a CoreOS VM and attach your Secure Shell (SSH) key (your arguments might be different):</span></span>

```azurecli
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="d837f-111">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="d837f-111">Detailed walkthrough</span></span>

<span data-ttu-id="d837f-112">V následujícím textu najdete podrobný návod, jak postupovat při nasazení virtuálního počítače UbuntuLTS, s názorným vysvětlením jednotlivých kroků.</span><span class="sxs-lookup"><span data-stu-id="d837f-112">The following walkthrough has an UbuntuLTS VM being deployed, step by step, with explanations of what each step is doing.</span></span>

## <a name="vm-quick-create-aliases"></a><span data-ttu-id="d837f-113">Aliasy quick-create pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="d837f-113">VM quick-create aliases</span></span>

<span data-ttu-id="d837f-114">Rychlým způsobem, jak zvolit distribuci, je použít aliasy rozhraní příkazového řádku Azure namapované na nejběžnější distribuce operačních systémů.</span><span class="sxs-lookup"><span data-stu-id="d837f-114">A quick way to choose a distribution is to use the Azure CLI aliases mapped to the most common OS distributions.</span></span> <span data-ttu-id="d837f-115">Tyto aliasy jsou uvedené v následující tabulce (pro rozhraní příkazového řádku Azure verze 0.10).</span><span class="sxs-lookup"><span data-stu-id="d837f-115">The following table lists the aliases (as of Azure CLI version 0.10).</span></span> <span data-ttu-id="d837f-116">Všechna nasazení, která využívají `quick-create`, standardně směřují na virtuální počítače zálohované úložištěm SSD (solid-state drive), které nabízí rychlejší zřizování a vysoce výkonný přístup na disk.</span><span class="sxs-lookup"><span data-stu-id="d837f-116">All deployments that use `quick-create` default to VMs that are backed by solid-state drive (SSD) storage, which offers faster provisioning and high-performance disk access.</span></span> <span data-ttu-id="d837f-117">(Tyto aliasy představují jenom nepatrnou část dostupných distribucí na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="d837f-117">(These aliases represent a tiny portion of the available distributions on Azure.</span></span> <span data-ttu-id="d837f-118">Další image můžete vyhledat v Azure Marketplace pomocí [hledání image v PowerShellu](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [na webu](https://azure.microsoft.com/marketplace/virtual-machines/) nebo můžete [nahrát vlastní image](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)</span><span class="sxs-lookup"><span data-stu-id="d837f-118">Find more images in the Azure Marketplace by [searching for an image in PowerShell](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [on the web](https://azure.microsoft.com/marketplace/virtual-machines/), or [upload your own custom image](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)</span></span>

| <span data-ttu-id="d837f-119">Alias</span><span class="sxs-lookup"><span data-stu-id="d837f-119">Alias</span></span> | <span data-ttu-id="d837f-120">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="d837f-120">Publisher</span></span> | <span data-ttu-id="d837f-121">Nabídka</span><span class="sxs-lookup"><span data-stu-id="d837f-121">Offer</span></span> | <span data-ttu-id="d837f-122">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="d837f-122">SKU</span></span> | <span data-ttu-id="d837f-123">Verze</span><span class="sxs-lookup"><span data-stu-id="d837f-123">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="d837f-124">CentOS</span><span class="sxs-lookup"><span data-stu-id="d837f-124">CentOS</span></span> |<span data-ttu-id="d837f-125">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="d837f-125">OpenLogic</span></span> |<span data-ttu-id="d837f-126">CentOS</span><span class="sxs-lookup"><span data-stu-id="d837f-126">CentOS</span></span> |<span data-ttu-id="d837f-127">7.2</span><span class="sxs-lookup"><span data-stu-id="d837f-127">7.2</span></span> |<span data-ttu-id="d837f-128">nejnovější</span><span class="sxs-lookup"><span data-stu-id="d837f-128">latest</span></span> |
| <span data-ttu-id="d837f-129">CoreOS</span><span class="sxs-lookup"><span data-stu-id="d837f-129">CoreOS</span></span> |<span data-ttu-id="d837f-130">CoreOS</span><span class="sxs-lookup"><span data-stu-id="d837f-130">CoreOS</span></span> |<span data-ttu-id="d837f-131">CoreOS</span><span class="sxs-lookup"><span data-stu-id="d837f-131">CoreOS</span></span> |<span data-ttu-id="d837f-132">Stable</span><span class="sxs-lookup"><span data-stu-id="d837f-132">Stable</span></span> |<span data-ttu-id="d837f-133">nejnovější</span><span class="sxs-lookup"><span data-stu-id="d837f-133">latest</span></span> |
| <span data-ttu-id="d837f-134">Debian</span><span class="sxs-lookup"><span data-stu-id="d837f-134">Debian</span></span> |<span data-ttu-id="d837f-135">credativ</span><span class="sxs-lookup"><span data-stu-id="d837f-135">credativ</span></span> |<span data-ttu-id="d837f-136">Debian</span><span class="sxs-lookup"><span data-stu-id="d837f-136">Debian</span></span> |<span data-ttu-id="d837f-137">8</span><span class="sxs-lookup"><span data-stu-id="d837f-137">8</span></span> |<span data-ttu-id="d837f-138">nejnovější</span><span class="sxs-lookup"><span data-stu-id="d837f-138">latest</span></span> |
| <span data-ttu-id="d837f-139">openSUSE</span><span class="sxs-lookup"><span data-stu-id="d837f-139">openSUSE</span></span> |<span data-ttu-id="d837f-140">SUSE</span><span class="sxs-lookup"><span data-stu-id="d837f-140">SUSE</span></span> |<span data-ttu-id="d837f-141">openSUSE</span><span class="sxs-lookup"><span data-stu-id="d837f-141">openSUSE</span></span> |<span data-ttu-id="d837f-142">13.2</span><span class="sxs-lookup"><span data-stu-id="d837f-142">13.2</span></span> |<span data-ttu-id="d837f-143">nejnovější</span><span class="sxs-lookup"><span data-stu-id="d837f-143">latest</span></span> |
| <span data-ttu-id="d837f-144">RHEL</span><span class="sxs-lookup"><span data-stu-id="d837f-144">RHEL</span></span> |<span data-ttu-id="d837f-145">Red Hat</span><span class="sxs-lookup"><span data-stu-id="d837f-145">Red Hat</span></span> |<span data-ttu-id="d837f-146">RHEL</span><span class="sxs-lookup"><span data-stu-id="d837f-146">RHEL</span></span> |<span data-ttu-id="d837f-147">7.2</span><span class="sxs-lookup"><span data-stu-id="d837f-147">7.2</span></span> |<span data-ttu-id="d837f-148">nejnovější</span><span class="sxs-lookup"><span data-stu-id="d837f-148">latest</span></span> |
| <span data-ttu-id="d837f-149">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="d837f-149">UbuntuLTS</span></span> |<span data-ttu-id="d837f-150">Canonical</span><span class="sxs-lookup"><span data-stu-id="d837f-150">Canonical</span></span> |<span data-ttu-id="d837f-151">Ubuntu Server</span><span class="sxs-lookup"><span data-stu-id="d837f-151">Ubuntu Server</span></span> |<span data-ttu-id="d837f-152">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="d837f-152">14.04.4-LTS</span></span> |<span data-ttu-id="d837f-153">nejnovější</span><span class="sxs-lookup"><span data-stu-id="d837f-153">latest</span></span> |

<span data-ttu-id="d837f-154">V následujících částech se používá alias `UbuntuLTS` pro možnost **ImageURN** (`-Q`) k nasazení serveru Ubuntu 14.04.4 LTS.</span><span class="sxs-lookup"><span data-stu-id="d837f-154">The following sections use the `UbuntuLTS` alias for the **ImageURN** option (`-Q`) to deploy an Ubuntu 14.04.4 LTS Server.</span></span>

<span data-ttu-id="d837f-155">V předchozím příkladu `quick-create` se příznak `-M` využíval jenom k identifikaci veřejného klíče SSH pro odeslání a hesla SSH byla zakázaná, takže se zobrazí výzva k zadání následujících argumentů:</span><span class="sxs-lookup"><span data-stu-id="d837f-155">The previous `quick-create` example only called out the `-M` flag to identify the SSH public key to upload while disabling SSH passwords, so you are prompted for the following arguments:</span></span>

* <span data-ttu-id="d837f-156">název skupiny prostředků (pro první skupinu prostředků Azure to obvykle může být libovolný řetězec)</span><span class="sxs-lookup"><span data-stu-id="d837f-156">resource group name (any string is typically fine for your first Azure resource group)</span></span>
* <span data-ttu-id="d837f-157">název virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d837f-157">VM name</span></span>
* <span data-ttu-id="d837f-158">umístění (vhodné výchozí hodnoty jsou `westus` nebo `westeurope`)</span><span class="sxs-lookup"><span data-stu-id="d837f-158">location (`westus` or `westeurope` are good defaults)</span></span>
* <span data-ttu-id="d837f-159">linux (aby se v Azure vědělo, který operační systém chcete)</span><span class="sxs-lookup"><span data-stu-id="d837f-159">linux (to let Azure know which OS you want)</span></span>
* <span data-ttu-id="d837f-160">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="d837f-160">username</span></span>

<span data-ttu-id="d837f-161">V následujícím příkladu jsou všechny tyto hodnoty zadané, takže už není potřeba zobrazovat žádné další výzvy.</span><span class="sxs-lookup"><span data-stu-id="d837f-161">The following example specifies all the values so that no further prompting is required.</span></span> <span data-ttu-id="d837f-162">Pokud jako soubor veřejného klíče ve formátu ssh-rsa používáte `~/.ssh/id_rsa.pub`, funguje tak, jak je:</span><span class="sxs-lookup"><span data-stu-id="d837f-162">So long as you have an `~/.ssh/id_rsa.pub` as a ssh-rsa format public key file, it works as is:</span></span>

```azurecli
azure vm quick-create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --admin-username myAdminUser \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --image-urn UbuntuLTS
```

<span data-ttu-id="d837f-163">Výstup by měl vypadat jako následující výstupní blok:</span><span class="sxs-lookup"><span data-stu-id="d837f-163">The output should look like the following output block:</span></span>

```azurecli
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a><span data-ttu-id="d837f-164">Přihlášení k novému virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="d837f-164">Log in to the new VM</span></span>
<span data-ttu-id="d837f-165">Přihlaste se do vašeho virtuálního počítače pomocí veřejné IP adresy, která je uvedená ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="d837f-165">Log in to your VM by using the public IP address listed in the output.</span></span> <span data-ttu-id="d837f-166">Můžete také využít uvedený plně kvalifikovaný název domény:</span><span class="sxs-lookup"><span data-stu-id="d837f-166">You can also use the fully qualified domain name (FQDN) that's listed:</span></span>

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

<span data-ttu-id="d837f-167">Proces přihlášení by měl vypadat podobně jako následující výstupní blok:</span><span class="sxs-lookup"><span data-stu-id="d837f-167">The login process should look something like the following output block:</span></span>

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a><span data-ttu-id="d837f-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d837f-168">Next steps</span></span>
<span data-ttu-id="d837f-169">Příkaz `azure vm quick-create` představuje způsob, jak rychle nasadit virtuální počítač, abyste se mohli přihlásit k prostředí Bash a začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="d837f-169">The `azure vm quick-create` command is the way to quickly deploy a VM so you can log in to a bash shell and get working.</span></span> <span data-ttu-id="d837f-170">Použití `vm quick-create` ale neposkytuje větší možnosti kontroly ani neumožňuje vytvářet složitější prostředí.</span><span class="sxs-lookup"><span data-stu-id="d837f-170">However, using `vm quick-create` does not give you extensive control nor does it enable you to create a more complex environment.</span></span>  <span data-ttu-id="d837f-171">Pokud budete chtít nasadit virtuální počítač s Linuxem přizpůsobený vaší infrastruktuře, můžete postupovat podle některého z těchto článků:</span><span class="sxs-lookup"><span data-stu-id="d837f-171">To deploy a Linux VM that's customized for your infrastructure, you can follow any of these articles:</span></span>

* <span data-ttu-id="d837f-172">[Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d837f-172">[Create your own custom environment for a Linux VM using Azure CLI commands directly](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* [<span data-ttu-id="d837f-173">Vytvoření virtuálního počítače s Linuxem se zabezpečením SSH na platformě Azure pomocí šablon</span><span class="sxs-lookup"><span data-stu-id="d837f-173">Create an SSH Secured Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

<span data-ttu-id="d837f-174">K [rychlému vytvoření linuxového virtuálního počítače jako hostitele Docker můžete také využít ovladač Azure `docker-machine` s různými příkazy](docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d837f-174">You can also [use the `docker-machine` Azure driver with various commands to quickly create a Linux VM as a docker host](docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
