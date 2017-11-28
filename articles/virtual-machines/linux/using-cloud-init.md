---
title: "toocustomize aaaUse init cloudu virtuálního počítače s Linuxem | Microsoft Docs"
description: "Jak cloud init toocustomize toouse a virtuální počítač s Linuxem během vytváření s hello 2.0 rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: e7297e162fc73f0da42f195bec2fcbe23b310c1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation"></a><span data-ttu-id="2d6a9-103">Použít během vytváření toocustomize init cloudu virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="2d6a9-103">Use cloud-init toocustomize a Linux VM during creation</span></span>
<span data-ttu-id="2d6a9-104">Tento článek ukazuje, jak toomake tooset skriptu cloudu init hello název hostitele, aktualizace nainstalované balíčky a spravovat uživatelské účty s hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-104">This article shows you how toomake a cloud-init script tooset hello hostname, update installed packages, and manage user accounts with hello Azure CLI 2.0.</span></span> <span data-ttu-id="2d6a9-105">Hello cloudu init skriptů jsou volány při vytvoření virtuálního počítače (VM) z příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-105">hello cloud-init scripts are called when you create a virtual machine (VM) from Azure CLI.</span></span> <span data-ttu-id="2d6a9-106">Podrobnější přehled o instalaci aplikací, zápisu konfigurační soubory a vložení klíče z Key Vault, najdete v části [v tomto kurzu](tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="2d6a9-106">For a more in-depth overview on installing applications, writing configuration files, and injecting keys from Key Vault, see [this tutorial](tutorial-automate-vm-deployment.md).</span></span> <span data-ttu-id="2d6a9-107">Můžete také provést tyto kroky hello [Azure CLI 1.0](using-cloud-init-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2d6a9-107">You can also perform these steps with hello [Azure CLI 1.0](using-cloud-init-nodejs.md).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="2d6a9-108">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="2d6a9-108">Quick commands</span></span>
<span data-ttu-id="2d6a9-109">Vytvoření cloudové init.txt skript, který nastaví hello název hostitele, aktualizuje všechny balíčky a přidá tooLinux uživatele sudo.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-109">Create a cloud-init.txt script that sets hello hostname, updates all packages, and adds a sudo user tooLinux.</span></span>

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

<span data-ttu-id="2d6a9-110">Vytvoření toolaunch skupiny prostředků virtuálních počítačů do s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="2d6a9-110">Create a resource group toolaunch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="2d6a9-111">Hello následující příklad vytvoří hello skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="2d6a9-111">hello following example creates hello resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="2d6a9-112">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění s hello `--custom-data` parametr.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-112">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot with hello `--custom-data` parameter.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="2d6a9-113">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="2d6a9-113">Detailed walkthrough</span></span>
<span data-ttu-id="2d6a9-114">Při spuštění nového virtuálního počítače s Linuxem se zobrazuje standardní virtuální počítač s Linuxem se nic, vlastní nebo připraveno k vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-114">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="2d6a9-115">[Init cloudu](https://cloudinit.readthedocs.org) je standardní způsob tooinject skriptu nebo konfigurace nastavení do tohoto virtuálního počítače s Linuxem, jako je spuštění pro službu hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-115">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way tooinject a script or configuration settings into that Linux VM as it is booting up for hello first time.</span></span>

<span data-ttu-id="2d6a9-116">V Azure, je několik různých způsobů toomake změny do virtuálního počítače s Linuxem jak je právě nasazen nebo spuštěn.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-116">On Azure, there are a few different ways toomake changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="2d6a9-117">Vložit skripty s použitím init cloudu.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-117">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="2d6a9-118">Vložit skripty s použitím hello Azure [rozšíření VMAccess](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="2d6a9-118">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md).</span></span>
* <span data-ttu-id="2d6a9-119">Šablony Azure pomocí init cloudu.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-119">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="2d6a9-120">K pomocí šablony Azure [CustomScriptExtention](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="2d6a9-120">An Azure template using [CustomScriptExtention](extensions-customscript.md).</span></span>

<span data-ttu-id="2d6a9-121">skripty tooinject kdykoli po spuštění:</span><span class="sxs-lookup"><span data-stu-id="2d6a9-121">tooinject scripts at any time after boot:</span></span>

* <span data-ttu-id="2d6a9-122">SSH toorun přímo příkazy</span><span class="sxs-lookup"><span data-stu-id="2d6a9-122">SSH toorun commands directly</span></span>
* <span data-ttu-id="2d6a9-123">Vložit skripty s použitím hello Azure [rozšíření VMAccess](using-vmaccess-extension.md), imperativní nebo šablony Azure</span><span class="sxs-lookup"><span data-stu-id="2d6a9-123">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="2d6a9-124">Nástroje pro správu konfigurace, jako Ansible, Salt, Chef nebo Puppet.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-124">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="2d6a9-125">Rozšíření VMAccess provádí skript jako kořenový v hello stejný způsobem pomocí protokolu SSH můžete.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-125">VMAccess Extension executes a script as root in hello same way using SSH can.</span></span> <span data-ttu-id="2d6a9-126">Však pomocí rozšíření virtuálního počítače hello povolí několik funkcí této nabídky Azure, které mohou být užitečné, v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-126">However, using hello VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="2d6a9-127">Init cloudu dostupnosti pro virtuální počítač Azure rychlým vytvořením bitové kopie aliasy:</span><span class="sxs-lookup"><span data-stu-id="2d6a9-127">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="2d6a9-128">Alias</span><span class="sxs-lookup"><span data-stu-id="2d6a9-128">Alias</span></span> | <span data-ttu-id="2d6a9-129">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="2d6a9-129">Publisher</span></span> | <span data-ttu-id="2d6a9-130">Nabídka</span><span class="sxs-lookup"><span data-stu-id="2d6a9-130">Offer</span></span> | <span data-ttu-id="2d6a9-131">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="2d6a9-131">SKU</span></span> | <span data-ttu-id="2d6a9-132">Verze</span><span class="sxs-lookup"><span data-stu-id="2d6a9-132">Version</span></span> | <span data-ttu-id="2d6a9-133">init cloudu</span><span class="sxs-lookup"><span data-stu-id="2d6a9-133">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="2d6a9-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="2d6a9-134">CentOS</span></span> |<span data-ttu-id="2d6a9-135">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="2d6a9-135">OpenLogic</span></span> |<span data-ttu-id="2d6a9-136">Centos</span><span class="sxs-lookup"><span data-stu-id="2d6a9-136">Centos</span></span> |<span data-ttu-id="2d6a9-137">7.2</span><span class="sxs-lookup"><span data-stu-id="2d6a9-137">7.2</span></span> |<span data-ttu-id="2d6a9-138">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2d6a9-138">latest</span></span> |<span data-ttu-id="2d6a9-139">Ne</span><span class="sxs-lookup"><span data-stu-id="2d6a9-139">no</span></span> |
| <span data-ttu-id="2d6a9-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2d6a9-140">CoreOS</span></span> |<span data-ttu-id="2d6a9-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2d6a9-141">CoreOS</span></span> |<span data-ttu-id="2d6a9-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2d6a9-142">CoreOS</span></span> |<span data-ttu-id="2d6a9-143">Stable</span><span class="sxs-lookup"><span data-stu-id="2d6a9-143">Stable</span></span> |<span data-ttu-id="2d6a9-144">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2d6a9-144">latest</span></span> |<span data-ttu-id="2d6a9-145">Ano</span><span class="sxs-lookup"><span data-stu-id="2d6a9-145">yes</span></span> |
| <span data-ttu-id="2d6a9-146">Debian</span><span class="sxs-lookup"><span data-stu-id="2d6a9-146">Debian</span></span> |<span data-ttu-id="2d6a9-147">credativ</span><span class="sxs-lookup"><span data-stu-id="2d6a9-147">credativ</span></span> |<span data-ttu-id="2d6a9-148">Debian</span><span class="sxs-lookup"><span data-stu-id="2d6a9-148">Debian</span></span> |<span data-ttu-id="2d6a9-149">8</span><span class="sxs-lookup"><span data-stu-id="2d6a9-149">8</span></span> |<span data-ttu-id="2d6a9-150">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2d6a9-150">latest</span></span> |<span data-ttu-id="2d6a9-151">Ne</span><span class="sxs-lookup"><span data-stu-id="2d6a9-151">no</span></span> |
| <span data-ttu-id="2d6a9-152">openSUSE</span><span class="sxs-lookup"><span data-stu-id="2d6a9-152">openSUSE</span></span> |<span data-ttu-id="2d6a9-153">SUSE</span><span class="sxs-lookup"><span data-stu-id="2d6a9-153">SUSE</span></span> |<span data-ttu-id="2d6a9-154">openSUSE</span><span class="sxs-lookup"><span data-stu-id="2d6a9-154">openSUSE</span></span> |<span data-ttu-id="2d6a9-155">13.2</span><span class="sxs-lookup"><span data-stu-id="2d6a9-155">13.2</span></span> |<span data-ttu-id="2d6a9-156">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2d6a9-156">latest</span></span> |<span data-ttu-id="2d6a9-157">Ne</span><span class="sxs-lookup"><span data-stu-id="2d6a9-157">no</span></span> |
| <span data-ttu-id="2d6a9-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="2d6a9-158">RHEL</span></span> |<span data-ttu-id="2d6a9-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="2d6a9-159">Redhat</span></span> |<span data-ttu-id="2d6a9-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="2d6a9-160">RHEL</span></span> |<span data-ttu-id="2d6a9-161">7.2</span><span class="sxs-lookup"><span data-stu-id="2d6a9-161">7.2</span></span> |<span data-ttu-id="2d6a9-162">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2d6a9-162">latest</span></span> |<span data-ttu-id="2d6a9-163">Ne</span><span class="sxs-lookup"><span data-stu-id="2d6a9-163">no</span></span> |
| <span data-ttu-id="2d6a9-164">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="2d6a9-164">UbuntuLTS</span></span> |<span data-ttu-id="2d6a9-165">Canonical</span><span class="sxs-lookup"><span data-stu-id="2d6a9-165">Canonical</span></span> |<span data-ttu-id="2d6a9-166">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="2d6a9-166">UbuntuServer</span></span> |<span data-ttu-id="2d6a9-167">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="2d6a9-167">14.04.4-LTS</span></span> |<span data-ttu-id="2d6a9-168">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2d6a9-168">latest</span></span> |<span data-ttu-id="2d6a9-169">Ano</span><span class="sxs-lookup"><span data-stu-id="2d6a9-169">yes</span></span> |

<span data-ttu-id="2d6a9-170">Snažíme se práce s našich partnerů tooget cloudu inicializací zahrnuté a práci v hello bitové kopie, aby umožňovala tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-170">We are working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span>

## <a name="add-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a><span data-ttu-id="2d6a9-171">Přidejte cloudu init skriptu toohello vytvoření virtuálních počítačů s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2d6a9-171">Add a cloud-init script toohello VM creation with hello Azure CLI</span></span>
<span data-ttu-id="2d6a9-172">toolaunch skript cloudu init při vytváření virtuálního počítače v Azure, zadejte soubor cloudu init hello pomocí rozhraní příkazového řádku Azure hello `--custom-data` přepínače.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-172">toolaunch a cloud-init script when creating a VM in Azure, specify hello cloud-init file using hello Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="2d6a9-173">Vytvoření toolaunch skupiny prostředků virtuálních počítačů do s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="2d6a9-173">Create a resource group toolaunch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="2d6a9-174">Hello následující příklad vytvoří hello skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="2d6a9-174">hello following example creates hello resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="2d6a9-175">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-175">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a><span data-ttu-id="2d6a9-176">Vytvoření cloudu init skriptu tooset hello název hostitele virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="2d6a9-176">Create a cloud-init script tooset hello hostname of a Linux VM</span></span>
<span data-ttu-id="2d6a9-177">Jedním z nejdůležitějších kroků nastavení pro všechny virtuální počítač s Linuxem a hello nejjednodušší by hello název hostitele.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-177">One of hello simplest and most important settings for any Linux VM would be hello hostname.</span></span> <span data-ttu-id="2d6a9-178">Jsme tento parametr můžete snadno nastavit pomocí init cloudových – pomocí tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-178">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="2d6a9-179">Příklad cloudu init skript s názvem `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-179">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```yaml
#cloud-config
hostname: myservername
```

<span data-ttu-id="2d6a9-180">Během hello počáteční spouštění hello virtuálních počítačů, tento skript cloudu init nastaví název hostitele hello příliš*NázevServeru*.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-180">During hello initial startup of hello VM, this cloud-init script sets hello hostname too*myservername*.</span></span> <span data-ttu-id="2d6a9-181">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-181">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="2d6a9-182">Přihlášení a ověřte název hostitele hello hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-182">Login and verify hello hostname of hello new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a><span data-ttu-id="2d6a9-183">Vytvoření skriptu init cloudu</span><span class="sxs-lookup"><span data-stu-id="2d6a9-183">Create a cloud-init script</span></span>
<span data-ttu-id="2d6a9-184">Pro zabezpečení budete chtít vaší tooupdate virtuálního počítače s Ubuntu na hello při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-184">For security, you want your Ubuntu VM tooupdate on hello first boot.</span></span> <span data-ttu-id="2d6a9-185">Pomocí cloudu init můžeme udělat, aby s hello podle skriptu, v závislosti na distribuční hello Linux, který používáte.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-185">Using cloud-init we can do that with hello follow script, depending on hello Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a><span data-ttu-id="2d6a9-186">Ukázkový skript cloudu init `cloud_config_apt_upgrade.txt` pro hello Debian rodiny</span><span class="sxs-lookup"><span data-stu-id="2d6a9-186">Example cloud-init script `cloud_config_apt_upgrade.txt` for hello Debian Family</span></span>
```yaml
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="2d6a9-187">Po Linux, jsou aktualizovány všechny balíčky hello nainstalovaná prostřednictvím **výstižný get**.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-187">After Linux has booted, all hello installed packages are updated via **apt-get**.</span></span> <span data-ttu-id="2d6a9-188">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-188">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_apt_upgrade.txt
```

<span data-ttu-id="2d6a9-189">Přihlášení a ověření, jsou aktualizovány všechny balíčky.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-189">Login and verify all packages are updated.</span></span>

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
hello following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 tooremove and 0 not upgraded.
```

## <a name="create-a-cloud-init-script-tooadd-a-user-toolinux"></a><span data-ttu-id="2d6a9-190">Vytvoření cloudu init skriptu tooadd tooLinux uživatele</span><span class="sxs-lookup"><span data-stu-id="2d6a9-190">Create a cloud-init script tooadd a user tooLinux</span></span>
<span data-ttu-id="2d6a9-191">Jedním z první úlohy hello na žádné nové virtuální počítače Linux je tooadd uživatele pro sebe nebo pomocí tooavoid *kořenové*.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-191">One of hello first tasks on any new Linux VM is tooadd a user for yourself or tooavoid using *root*.</span></span> <span data-ttu-id="2d6a9-192">SSH klíče jsou vhodné pro zabezpečení a použitelnost a přidají se toohello *~/.ssh/authorized_keys* souboru pomocí tohoto skriptu init cloudu.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-192">SSH keys are best practice for security and for usability and they are added toohello *~/.ssh/authorized_keys* file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="2d6a9-193">Ukázkový skript cloudu init `cloud_config_add_users.txt` pro Debian řadu</span><span class="sxs-lookup"><span data-stu-id="2d6a9-193">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
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

<span data-ttu-id="2d6a9-194">Po Linux, všichni uživatelé hello uvedené jsou skupiny vytvořené a přidané toohello sudo.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-194">After Linux has booted, all hello listed users are created and added toohello sudo group.</span></span> <span data-ttu-id="2d6a9-195">Vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) pomocí cloudu init tooconfigure ji během spouštění.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-195">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

<span data-ttu-id="2d6a9-196">Přihlášení a ověření hello nově vytvořeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-196">Login and verify hello newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="2d6a9-197">Výstup</span><span class="sxs-lookup"><span data-stu-id="2d6a9-197">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="2d6a9-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2d6a9-198">Next steps</span></span>
<span data-ttu-id="2d6a9-199">Init cloudu se stává stále jednu standardní způsob toomodify virtuálním počítačům s Linuxem na spuštění.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-199">Cloud-init is becoming one standard way toomodify your Linux VM on boot.</span></span> <span data-ttu-id="2d6a9-200">Azure má také rozšíření virtuálních počítačů, které umožňují toomodify vaše LinuxVM na spouštěcí nebo když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-200">Azure also has VM extensions, which allow you toomodify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="2d6a9-201">Například můžete hello Azure VMAccessExtension tooreset SSH nebo informace o uživateli při hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-201">For example, you can use hello Azure VMAccessExtension tooreset SSH or user information while hello VM is running.</span></span> <span data-ttu-id="2d6a9-202">S inicializací cloudu bude třeba heslo hello tooreset restartování.</span><span class="sxs-lookup"><span data-stu-id="2d6a9-202">With cloud-init, you would need a reboot tooreset hello password.</span></span>

[<span data-ttu-id="2d6a9-203">O rozšíření virtuálního počítače a funkce</span><span class="sxs-lookup"><span data-stu-id="2d6a9-203">About virtual machine extensions and features</span></span>](extensions-features.md)

[<span data-ttu-id="2d6a9-204">Spravovat uživatele, SSH a zkontrolujte nebo hello oprava disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="2d6a9-204">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md)
