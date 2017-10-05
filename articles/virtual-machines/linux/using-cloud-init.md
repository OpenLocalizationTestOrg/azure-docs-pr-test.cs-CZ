---
title: "Použít cloudové init k přizpůsobení virtuálního počítače s Linuxem | Microsoft Docs"
description: "Jak používat cloudové init k přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: a7a6daad34525683579e25b9591ed28f2bf29c04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation"></a><span data-ttu-id="7cbff-103">Slouží k přizpůsobení virtuálního počítače s Linuxem během vytváření init cloudu</span><span class="sxs-lookup"><span data-stu-id="7cbff-103">Use cloud-init to customize a Linux VM during creation</span></span>
<span data-ttu-id="7cbff-104">Tento článek ukazuje, jak provádět skript cloudu init nastavit název hostitele, aktualizace nainstalované balíčky a správa uživatelských účtů s Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="7cbff-104">This article shows you how to make a cloud-init script to set the hostname, update installed packages, and manage user accounts with the Azure CLI 2.0.</span></span> <span data-ttu-id="7cbff-105">Skripty cloud init se označují jako při vytvoření virtuálního počítače (VM) z příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="7cbff-105">The cloud-init scripts are called when you create a virtual machine (VM) from Azure CLI.</span></span> <span data-ttu-id="7cbff-106">Podrobnější přehled o instalaci aplikací, zápisu konfigurační soubory a vložení klíče z Key Vault, najdete v části [v tomto kurzu](tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="7cbff-106">For a more in-depth overview on installing applications, writing configuration files, and injecting keys from Key Vault, see [this tutorial](tutorial-automate-vm-deployment.md).</span></span> <span data-ttu-id="7cbff-107">K provedení těchto kroků můžete také využít [Azure CLI 1.0](using-cloud-init-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="7cbff-107">You can also perform these steps with the [Azure CLI 1.0](using-cloud-init-nodejs.md).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="7cbff-108">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="7cbff-108">Quick commands</span></span>
<span data-ttu-id="7cbff-109">Vytvoření cloudové init.txt skript, který nastaví název hostitele, aktualizuje všechny balíčky a přidá uživatele sudo do systému Linux.</span><span class="sxs-lookup"><span data-stu-id="7cbff-109">Create a cloud-init.txt script that sets the hostname, updates all packages, and adds a sudo user to Linux.</span></span>

```yaml
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```

<span data-ttu-id="7cbff-110">Vytvořte skupinu prostředků ke spuštění virtuálních počítačů do s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="7cbff-110">Create a resource group to launch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="7cbff-111">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="7cbff-111">The following example creates the resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="7cbff-112">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění s `--custom-data` parametr.</span><span class="sxs-lookup"><span data-stu-id="7cbff-112">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot with the `--custom-data` parameter.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="7cbff-113">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="7cbff-113">Detailed walkthrough</span></span>
<span data-ttu-id="7cbff-114">Při spuštění nového virtuálního počítače s Linuxem se zobrazuje standardní virtuální počítač s Linuxem se nic, vlastní nebo připraveno k vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="7cbff-114">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="7cbff-115">[Init cloudu](https://cloudinit.readthedocs.org) je standardní způsob vložení skriptu nebo konfigurace nastavení do tohoto virtuálního počítače s Linuxem, jako je spuštění pro první.</span><span class="sxs-lookup"><span data-stu-id="7cbff-115">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way to inject a script or configuration settings into that Linux VM as it is booting up for the first time.</span></span>

<span data-ttu-id="7cbff-116">V Azure, existuje několik různých způsobů provést změny do virtuálního počítače s Linuxem, jako je právě nasazen nebo spuštěn.</span><span class="sxs-lookup"><span data-stu-id="7cbff-116">On Azure, there are a few different ways to make changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="7cbff-117">Vložit skripty s použitím init cloudu.</span><span class="sxs-lookup"><span data-stu-id="7cbff-117">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="7cbff-118">Vložit skripty s použitím Azure [rozšíření VMAccess](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="7cbff-118">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md).</span></span>
* <span data-ttu-id="7cbff-119">Šablony Azure pomocí init cloudu.</span><span class="sxs-lookup"><span data-stu-id="7cbff-119">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="7cbff-120">K pomocí šablony Azure [CustomScriptExtention](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="7cbff-120">An Azure template using [CustomScriptExtention](extensions-customscript.md).</span></span>

<span data-ttu-id="7cbff-121">Chcete-li vložit skripty kdykoli po spuštění:</span><span class="sxs-lookup"><span data-stu-id="7cbff-121">To inject scripts at any time after boot:</span></span>

* <span data-ttu-id="7cbff-122">SSH ke spuštění příkazů přímo</span><span class="sxs-lookup"><span data-stu-id="7cbff-122">SSH to run commands directly</span></span>
* <span data-ttu-id="7cbff-123">Vložit skripty s použitím Azure [rozšíření VMAccess](using-vmaccess-extension.md), imperativní nebo šablony Azure</span><span class="sxs-lookup"><span data-stu-id="7cbff-123">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="7cbff-124">Nástroje pro správu konfigurace, jako Ansible, Salt, Chef nebo Puppet.</span><span class="sxs-lookup"><span data-stu-id="7cbff-124">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="7cbff-125">Rozšíření VMAccess spustí skript jako kořenové stejným způsobem jako pomocí protokolu SSH můžete.</span><span class="sxs-lookup"><span data-stu-id="7cbff-125">VMAccess Extension executes a script as root in the same way using SSH can.</span></span> <span data-ttu-id="7cbff-126">Však pomocí rozšíření virtuálního počítače umožňuje několik funkcí této nabídky Azure, které mohou být užitečné, v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="7cbff-126">However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="7cbff-127">Init cloudu dostupnosti pro virtuální počítač Azure rychlým vytvořením bitové kopie aliasy:</span><span class="sxs-lookup"><span data-stu-id="7cbff-127">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="7cbff-128">Alias</span><span class="sxs-lookup"><span data-stu-id="7cbff-128">Alias</span></span> | <span data-ttu-id="7cbff-129">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="7cbff-129">Publisher</span></span> | <span data-ttu-id="7cbff-130">Nabídka</span><span class="sxs-lookup"><span data-stu-id="7cbff-130">Offer</span></span> | <span data-ttu-id="7cbff-131">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="7cbff-131">SKU</span></span> | <span data-ttu-id="7cbff-132">Verze</span><span class="sxs-lookup"><span data-stu-id="7cbff-132">Version</span></span> | <span data-ttu-id="7cbff-133">init cloudu</span><span class="sxs-lookup"><span data-stu-id="7cbff-133">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="7cbff-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="7cbff-134">CentOS</span></span> |<span data-ttu-id="7cbff-135">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="7cbff-135">OpenLogic</span></span> |<span data-ttu-id="7cbff-136">Centos</span><span class="sxs-lookup"><span data-stu-id="7cbff-136">Centos</span></span> |<span data-ttu-id="7cbff-137">7.2</span><span class="sxs-lookup"><span data-stu-id="7cbff-137">7.2</span></span> |<span data-ttu-id="7cbff-138">nejnovější</span><span class="sxs-lookup"><span data-stu-id="7cbff-138">latest</span></span> |<span data-ttu-id="7cbff-139">Ne</span><span class="sxs-lookup"><span data-stu-id="7cbff-139">no</span></span> |
| <span data-ttu-id="7cbff-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="7cbff-140">CoreOS</span></span> |<span data-ttu-id="7cbff-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="7cbff-141">CoreOS</span></span> |<span data-ttu-id="7cbff-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="7cbff-142">CoreOS</span></span> |<span data-ttu-id="7cbff-143">Stable</span><span class="sxs-lookup"><span data-stu-id="7cbff-143">Stable</span></span> |<span data-ttu-id="7cbff-144">nejnovější</span><span class="sxs-lookup"><span data-stu-id="7cbff-144">latest</span></span> |<span data-ttu-id="7cbff-145">Ano</span><span class="sxs-lookup"><span data-stu-id="7cbff-145">yes</span></span> |
| <span data-ttu-id="7cbff-146">Debian</span><span class="sxs-lookup"><span data-stu-id="7cbff-146">Debian</span></span> |<span data-ttu-id="7cbff-147">credativ</span><span class="sxs-lookup"><span data-stu-id="7cbff-147">credativ</span></span> |<span data-ttu-id="7cbff-148">Debian</span><span class="sxs-lookup"><span data-stu-id="7cbff-148">Debian</span></span> |<span data-ttu-id="7cbff-149">8</span><span class="sxs-lookup"><span data-stu-id="7cbff-149">8</span></span> |<span data-ttu-id="7cbff-150">nejnovější</span><span class="sxs-lookup"><span data-stu-id="7cbff-150">latest</span></span> |<span data-ttu-id="7cbff-151">Ne</span><span class="sxs-lookup"><span data-stu-id="7cbff-151">no</span></span> |
| <span data-ttu-id="7cbff-152">openSUSE</span><span class="sxs-lookup"><span data-stu-id="7cbff-152">openSUSE</span></span> |<span data-ttu-id="7cbff-153">SUSE</span><span class="sxs-lookup"><span data-stu-id="7cbff-153">SUSE</span></span> |<span data-ttu-id="7cbff-154">openSUSE</span><span class="sxs-lookup"><span data-stu-id="7cbff-154">openSUSE</span></span> |<span data-ttu-id="7cbff-155">13.2</span><span class="sxs-lookup"><span data-stu-id="7cbff-155">13.2</span></span> |<span data-ttu-id="7cbff-156">nejnovější</span><span class="sxs-lookup"><span data-stu-id="7cbff-156">latest</span></span> |<span data-ttu-id="7cbff-157">Ne</span><span class="sxs-lookup"><span data-stu-id="7cbff-157">no</span></span> |
| <span data-ttu-id="7cbff-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="7cbff-158">RHEL</span></span> |<span data-ttu-id="7cbff-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="7cbff-159">Redhat</span></span> |<span data-ttu-id="7cbff-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="7cbff-160">RHEL</span></span> |<span data-ttu-id="7cbff-161">7.2</span><span class="sxs-lookup"><span data-stu-id="7cbff-161">7.2</span></span> |<span data-ttu-id="7cbff-162">nejnovější</span><span class="sxs-lookup"><span data-stu-id="7cbff-162">latest</span></span> |<span data-ttu-id="7cbff-163">Ne</span><span class="sxs-lookup"><span data-stu-id="7cbff-163">no</span></span> |
| <span data-ttu-id="7cbff-164">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="7cbff-164">UbuntuLTS</span></span> |<span data-ttu-id="7cbff-165">Canonical</span><span class="sxs-lookup"><span data-stu-id="7cbff-165">Canonical</span></span> |<span data-ttu-id="7cbff-166">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="7cbff-166">UbuntuServer</span></span> |<span data-ttu-id="7cbff-167">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="7cbff-167">14.04.4-LTS</span></span> |<span data-ttu-id="7cbff-168">nejnovější</span><span class="sxs-lookup"><span data-stu-id="7cbff-168">latest</span></span> |<span data-ttu-id="7cbff-169">Ano</span><span class="sxs-lookup"><span data-stu-id="7cbff-169">yes</span></span> |

<span data-ttu-id="7cbff-170">Pracujeme s našimi partnery získat cloudu init zahrnuté a práci v bitové kopie, které poskytují do Azure.</span><span class="sxs-lookup"><span data-stu-id="7cbff-170">We are working with our partners to get cloud-init included and working in the images that they provide to Azure.</span></span>

## <a name="add-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a><span data-ttu-id="7cbff-171">Přidat skript cloudu init vytvoření virtuálních počítačů pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7cbff-171">Add a cloud-init script to the VM creation with the Azure CLI</span></span>
<span data-ttu-id="7cbff-172">Spusťte skript cloudu init při vytváření virtuálního počítače v Azure, zadejte soubor init cloudu pomocí Azure CLI `--custom-data` přepínače.</span><span class="sxs-lookup"><span data-stu-id="7cbff-172">To launch a cloud-init script when creating a VM in Azure, specify the cloud-init file using the Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="7cbff-173">Vytvořte skupinu prostředků ke spuštění virtuálních počítačů do s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="7cbff-173">Create a resource group to launch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="7cbff-174">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="7cbff-174">The following example creates the resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="7cbff-175">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění.</span><span class="sxs-lookup"><span data-stu-id="7cbff-175">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a><span data-ttu-id="7cbff-176">Vytvoření skriptu cloudu init nastavit název hostitele virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="7cbff-176">Create a cloud-init script to set the hostname of a Linux VM</span></span>
<span data-ttu-id="7cbff-177">Jedním z nejjednodušší a nejdůležitější nastavení pro všechny virtuální počítač s Linuxem bude název hostitele.</span><span class="sxs-lookup"><span data-stu-id="7cbff-177">One of the simplest and most important settings for any Linux VM would be the hostname.</span></span> <span data-ttu-id="7cbff-178">Jsme tento parametr můžete snadno nastavit pomocí init cloudových – pomocí tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="7cbff-178">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="7cbff-179">Příklad cloudu init skript s názvem `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="7cbff-179">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```yaml
#cloud-config
hostname: myservername
```

<span data-ttu-id="7cbff-180">Při prvním spuštění virtuálního počítače, nastaví tento skript cloudu init název hostitele na *NázevServeru*.</span><span class="sxs-lookup"><span data-stu-id="7cbff-180">During the initial startup of the VM, this cloud-init script sets the hostname to *myservername*.</span></span> <span data-ttu-id="7cbff-181">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění.</span><span class="sxs-lookup"><span data-stu-id="7cbff-181">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="7cbff-182">Přihlášení a ověřte název hostitele nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7cbff-182">Login and verify the hostname of the new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a><span data-ttu-id="7cbff-183">Vytvoření skriptu init cloudu</span><span class="sxs-lookup"><span data-stu-id="7cbff-183">Create a cloud-init script</span></span>
<span data-ttu-id="7cbff-184">Pro zabezpečení budete chtít vaší virtuálního počítače s Ubuntu aktualizovat při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="7cbff-184">For security, you want your Ubuntu VM to update on the first boot.</span></span> <span data-ttu-id="7cbff-185">Pomocí cloudu init, které jsme to udělat pomocí skriptu postupujte podle kroků, v závislosti na Linux distribuce, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="7cbff-185">Using cloud-init we can do that with the follow script, depending on the Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a><span data-ttu-id="7cbff-186">Ukázkový skript cloudu init `cloud_config_apt_upgrade.txt` pro Debian rodinu</span><span class="sxs-lookup"><span data-stu-id="7cbff-186">Example cloud-init script `cloud_config_apt_upgrade.txt` for the Debian Family</span></span>
```yaml
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="7cbff-187">Po Linux, jsou aktualizovány všechny nainstalované balíčky prostřednictvím **výstižný get**.</span><span class="sxs-lookup"><span data-stu-id="7cbff-187">After Linux has booted, all the installed packages are updated via **apt-get**.</span></span> <span data-ttu-id="7cbff-188">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění.</span><span class="sxs-lookup"><span data-stu-id="7cbff-188">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_apt_upgrade.txt
```

<span data-ttu-id="7cbff-189">Přihlášení a ověření, jsou aktualizovány všechny balíčky.</span><span class="sxs-lookup"><span data-stu-id="7cbff-189">Login and verify all packages are updated.</span></span>

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="create-a-cloud-init-script-to-add-a-user-to-linux"></a><span data-ttu-id="7cbff-190">Vytvoření skriptu cloudu init přidání uživatele do systému Linux</span><span class="sxs-lookup"><span data-stu-id="7cbff-190">Create a cloud-init script to add a user to Linux</span></span>
<span data-ttu-id="7cbff-191">Jedním z první úlohy na žádné nové virtuální počítače Linux je chcete přidat uživatele pro sebe nebo nepoužívejte *kořenové*.</span><span class="sxs-lookup"><span data-stu-id="7cbff-191">One of the first tasks on any new Linux VM is to add a user for yourself or to avoid using *root*.</span></span> <span data-ttu-id="7cbff-192">SSH klíče jsou vhodné pro zabezpečení a použitelnost a jejich přidání do *~/.ssh/authorized_keys* souboru pomocí tohoto skriptu init cloudu.</span><span class="sxs-lookup"><span data-stu-id="7cbff-192">SSH keys are best practice for security and for usability and they are added to the *~/.ssh/authorized_keys* file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="7cbff-193">Ukázkový skript cloudu init `cloud_config_add_users.txt` pro Debian řadu</span><span class="sxs-lookup"><span data-stu-id="7cbff-193">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
```yaml
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

<span data-ttu-id="7cbff-194">Po Linux, jsou uvedené uživatele vytvořen a přidán do skupiny sudo.</span><span class="sxs-lookup"><span data-stu-id="7cbff-194">After Linux has booted, all the listed users are created and added to the sudo group.</span></span> <span data-ttu-id="7cbff-195">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init konfigurace během spouštění.</span><span class="sxs-lookup"><span data-stu-id="7cbff-195">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

<span data-ttu-id="7cbff-196">Přihlášení a ověření nově vytvořeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="7cbff-196">Login and verify the newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="7cbff-197">Výstup</span><span class="sxs-lookup"><span data-stu-id="7cbff-197">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="7cbff-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7cbff-198">Next steps</span></span>
<span data-ttu-id="7cbff-199">Init cloudu se stává stále jednu standardní způsob, jak upravit virtuálním počítačům s Linuxem na spuštění.</span><span class="sxs-lookup"><span data-stu-id="7cbff-199">Cloud-init is becoming one standard way to modify your Linux VM on boot.</span></span> <span data-ttu-id="7cbff-200">Azure má také rozšíření virtuálních počítačů, které umožňují upravit vaše LinuxVM na spouštěcí nebo když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="7cbff-200">Azure also has VM extensions, which allow you to modify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="7cbff-201">Například můžete použít Azure VMAccessExtension resetovat informace SSH nebo uživatele, když běží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7cbff-201">For example, you can use the Azure VMAccessExtension to reset SSH or user information while the VM is running.</span></span> <span data-ttu-id="7cbff-202">S inicializací cloudu je zapotřebí restartování k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="7cbff-202">With cloud-init, you would need a reboot to reset the password.</span></span>

[<span data-ttu-id="7cbff-203">O rozšíření virtuálního počítače a funkce</span><span class="sxs-lookup"><span data-stu-id="7cbff-203">About virtual machine extensions and features</span></span>](extensions-features.md)

[<span data-ttu-id="7cbff-204">Spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="7cbff-204">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension</span></span>](using-vmaccess-extension.md)