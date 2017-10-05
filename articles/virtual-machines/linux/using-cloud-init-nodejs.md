---
title: "Přizpůsobení virtuálního počítače s Linuxem během vytváření v Azure pomocí cloudu init | Microsoft Docs"
description: "Jak používat cloudové init k přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: v-livech
ms.openlocfilehash: 0b6150bca333188666935b3c9aa02c4b33690db9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation-with-the-azure-cli-10"></a><span data-ttu-id="2c6b4-103">Použít cloudové init k přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2c6b4-103">Use cloud-init to customize a Linux VM during creation with the Azure CLI 1.0</span></span>
<span data-ttu-id="2c6b4-104">Tento článek ukazuje, jak chcete-li skript cloudu init nastavit název hostitele, aktualizace nainstalované balíčky, a spravovat uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-104">This article shows how to make a cloud-init script to set the hostname, update installed packages, and manage user accounts.</span></span>  <span data-ttu-id="2c6b4-105">Skripty cloud init se označují jako při vytvoření virtuálního počítače z příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-105">The cloud-init scripts are called during the VM creation from Azure CLI.</span></span>  <span data-ttu-id="2c6b4-106">Tento článek vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="2c6b4-106">The article requires:</span></span>

* <span data-ttu-id="2c6b4-107">účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))</span><span class="sxs-lookup"><span data-stu-id="2c6b4-107">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="2c6b4-108">[rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) s přihlášením `azure login`.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-108">the [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="2c6b4-109">Rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager`azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-109">the Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="2c6b4-110">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="2c6b4-110">CLI versions to complete the task</span></span>
<span data-ttu-id="2c6b4-111">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="2c6b4-111">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="2c6b4-112">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="2c6b4-112">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="2c6b4-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="2c6b4-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="2c6b4-114">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="2c6b4-114">Quick Commands</span></span>
<span data-ttu-id="2c6b4-115">Vytvoření cloudové init.txt skript, který nastaví název hostitele, aktualizuje všechny balíčky a přidá uživatele sudo do systému Linux.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-115">Create a cloud-init.txt script that sets the hostname, updates all packages, and adds a sudo user to Linux.</span></span>

```sh
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
<span data-ttu-id="2c6b4-116">Vytvořte skupinu prostředků ke spuštění virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-116">Create a resource group to launch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="2c6b4-117">Vytvoření virtuálního počítače s Linuxem pomocí cloudu init konfigurace během spouštění.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-117">Create a Linux VM using cloud-init to configure it during boot.</span></span>

```azurecli
azure vm create \
  -g myResourceGroup \
  -n myVM \
  -l westus \
  -y Linux \
  -f myVMnic \
  -F myVNet \
  -P 10.0.0.0/22 \
  -j mySubnet \
  -k 10.0.0.0/24 \
  -Q canonical:ubuntuserver:14.04.2-LTS:latest \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -C cloud-init.txt
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="2c6b4-118">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="2c6b4-118">Detailed walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="2c6b4-119">Úvod</span><span class="sxs-lookup"><span data-stu-id="2c6b4-119">Introduction</span></span>
<span data-ttu-id="2c6b4-120">Při spuštění nového virtuálního počítače s Linuxem se zobrazuje standardní virtuální počítač s Linuxem se nic, vlastní nebo připraveno k vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-120">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="2c6b4-121">[Init cloudu](https://cloudinit.readthedocs.org) je standardní způsob vložení skriptu nebo konfigurace nastavení do tohoto virtuálního počítače s Linuxem, jako je spuštění pro první.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-121">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way to inject a script or configuration settings into that Linux VM as it is booting up for the first time.</span></span>

<span data-ttu-id="2c6b4-122">V Azure existují tři různé způsoby provést změny do virtuálního počítače s Linuxem, jako je právě nasazen nebo spuštěn.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-122">On Azure, there are a three different ways to make changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="2c6b4-123">Vložit skripty s použitím init cloudu.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-123">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="2c6b4-124">Vložit skripty s použitím Azure [rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2c6b4-124">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="2c6b4-125">Šablony Azure pomocí init cloudu.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-125">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="2c6b4-126">K pomocí šablony Azure [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2c6b4-126">An Azure template using [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="2c6b4-127">Chcete-li vložit skripty kdykoli po spuštění:</span><span class="sxs-lookup"><span data-stu-id="2c6b4-127">To inject scripts at any time after boot:</span></span>

* <span data-ttu-id="2c6b4-128">SSH ke spuštění příkazů přímo</span><span class="sxs-lookup"><span data-stu-id="2c6b4-128">SSH to run commands directly</span></span>
* <span data-ttu-id="2c6b4-129">Vložit skripty s použitím Azure [rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperativní nebo šablony Azure</span><span class="sxs-lookup"><span data-stu-id="2c6b4-129">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="2c6b4-130">Nástroje pro správu konfigurace, jako Ansible, Salt, Chef nebo Puppet.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-130">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="2c6b4-131">: Rozšíření VMAccess spustí skript jako kořenové stejným způsobem jako pomocí protokolu SSH můžete.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-131">: VMAccess Extension executes a script as root in the same way using SSH can.</span></span>  <span data-ttu-id="2c6b4-132">Však pomocí rozšíření virtuálního počítače umožňuje několik funkcí této nabídky Azure, které mohou být užitečné, v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-132">However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="2c6b4-133">Init cloudu dostupnosti pro virtuální počítač Azure rychlým vytvořením bitové kopie aliasy:</span><span class="sxs-lookup"><span data-stu-id="2c6b4-133">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="2c6b4-134">Alias</span><span class="sxs-lookup"><span data-stu-id="2c6b4-134">Alias</span></span> | <span data-ttu-id="2c6b4-135">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="2c6b4-135">Publisher</span></span> | <span data-ttu-id="2c6b4-136">Nabídka</span><span class="sxs-lookup"><span data-stu-id="2c6b4-136">Offer</span></span> | <span data-ttu-id="2c6b4-137">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="2c6b4-137">SKU</span></span> | <span data-ttu-id="2c6b4-138">Verze</span><span class="sxs-lookup"><span data-stu-id="2c6b4-138">Version</span></span> | <span data-ttu-id="2c6b4-139">init cloudu</span><span class="sxs-lookup"><span data-stu-id="2c6b4-139">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="2c6b4-140">CentOS</span><span class="sxs-lookup"><span data-stu-id="2c6b4-140">CentOS</span></span> |<span data-ttu-id="2c6b4-141">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="2c6b4-141">OpenLogic</span></span> |<span data-ttu-id="2c6b4-142">Centos</span><span class="sxs-lookup"><span data-stu-id="2c6b4-142">Centos</span></span> |<span data-ttu-id="2c6b4-143">7.2</span><span class="sxs-lookup"><span data-stu-id="2c6b4-143">7.2</span></span> |<span data-ttu-id="2c6b4-144">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2c6b4-144">latest</span></span> |<span data-ttu-id="2c6b4-145">Ne</span><span class="sxs-lookup"><span data-stu-id="2c6b4-145">no</span></span> |
| <span data-ttu-id="2c6b4-146">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2c6b4-146">CoreOS</span></span> |<span data-ttu-id="2c6b4-147">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2c6b4-147">CoreOS</span></span> |<span data-ttu-id="2c6b4-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="2c6b4-148">CoreOS</span></span> |<span data-ttu-id="2c6b4-149">Stable</span><span class="sxs-lookup"><span data-stu-id="2c6b4-149">Stable</span></span> |<span data-ttu-id="2c6b4-150">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2c6b4-150">latest</span></span> |<span data-ttu-id="2c6b4-151">Ano</span><span class="sxs-lookup"><span data-stu-id="2c6b4-151">yes</span></span> |
| <span data-ttu-id="2c6b4-152">Debian</span><span class="sxs-lookup"><span data-stu-id="2c6b4-152">Debian</span></span> |<span data-ttu-id="2c6b4-153">credativ</span><span class="sxs-lookup"><span data-stu-id="2c6b4-153">credativ</span></span> |<span data-ttu-id="2c6b4-154">Debian</span><span class="sxs-lookup"><span data-stu-id="2c6b4-154">Debian</span></span> |<span data-ttu-id="2c6b4-155">8</span><span class="sxs-lookup"><span data-stu-id="2c6b4-155">8</span></span> |<span data-ttu-id="2c6b4-156">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2c6b4-156">latest</span></span> |<span data-ttu-id="2c6b4-157">Ne</span><span class="sxs-lookup"><span data-stu-id="2c6b4-157">no</span></span> |
| <span data-ttu-id="2c6b4-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="2c6b4-158">openSUSE</span></span> |<span data-ttu-id="2c6b4-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="2c6b4-159">SUSE</span></span> |<span data-ttu-id="2c6b4-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="2c6b4-160">openSUSE</span></span> |<span data-ttu-id="2c6b4-161">13.2</span><span class="sxs-lookup"><span data-stu-id="2c6b4-161">13.2</span></span> |<span data-ttu-id="2c6b4-162">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2c6b4-162">latest</span></span> |<span data-ttu-id="2c6b4-163">Ne</span><span class="sxs-lookup"><span data-stu-id="2c6b4-163">no</span></span> |
| <span data-ttu-id="2c6b4-164">RHEL</span><span class="sxs-lookup"><span data-stu-id="2c6b4-164">RHEL</span></span> |<span data-ttu-id="2c6b4-165">Redhat</span><span class="sxs-lookup"><span data-stu-id="2c6b4-165">Redhat</span></span> |<span data-ttu-id="2c6b4-166">RHEL</span><span class="sxs-lookup"><span data-stu-id="2c6b4-166">RHEL</span></span> |<span data-ttu-id="2c6b4-167">7.2</span><span class="sxs-lookup"><span data-stu-id="2c6b4-167">7.2</span></span> |<span data-ttu-id="2c6b4-168">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2c6b4-168">latest</span></span> |<span data-ttu-id="2c6b4-169">Ne</span><span class="sxs-lookup"><span data-stu-id="2c6b4-169">no</span></span> |
| <span data-ttu-id="2c6b4-170">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="2c6b4-170">UbuntuLTS</span></span> |<span data-ttu-id="2c6b4-171">Canonical</span><span class="sxs-lookup"><span data-stu-id="2c6b4-171">Canonical</span></span> |<span data-ttu-id="2c6b4-172">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="2c6b4-172">UbuntuServer</span></span> |<span data-ttu-id="2c6b4-173">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="2c6b4-173">14.04.4-LTS</span></span> |<span data-ttu-id="2c6b4-174">nejnovější</span><span class="sxs-lookup"><span data-stu-id="2c6b4-174">latest</span></span> |<span data-ttu-id="2c6b4-175">Ano</span><span class="sxs-lookup"><span data-stu-id="2c6b4-175">yes</span></span> |

<span data-ttu-id="2c6b4-176">Microsoft ve spolupráci s našimi partnery získat cloudu init zahrnuté a práci v bitové kopie, které poskytují do Azure.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-176">Microsoft is working with our partners to get cloud-init included and working in the images that they provide to Azure.</span></span>

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a><span data-ttu-id="2c6b4-177">Přidání cloudu init skript k vytvoření virtuálních počítačů pomocí Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2c6b4-177">Adding a cloud-init script to the VM creation with the Azure CLI</span></span>
<span data-ttu-id="2c6b4-178">Spusťte skript cloudu init při vytváření virtuálního počítače v Azure, zadejte soubor init cloudu pomocí Azure CLI `--custom-data` přepínače.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-178">To launch a cloud-init script when creating a VM in Azure, specify the cloud-init file using the Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="2c6b4-179">Vytvořte skupinu prostředků ke spuštění virtuálních počítačů do.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-179">Create a resource group to launch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="2c6b4-180">Vytvoření virtuálního počítače s Linuxem pomocí cloudu init konfigurace během spouštění.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-180">Create a Linux VM using cloud-init to configure it during boot.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubnet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a><span data-ttu-id="2c6b4-181">Vytváření cloudu init skript, který nastaví název hostitele virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="2c6b4-181">Creating a cloud-init script to set the hostname of a Linux VM</span></span>
<span data-ttu-id="2c6b4-182">Jedním z nejjednodušší a nejdůležitější nastavení pro všechny virtuální počítač s Linuxem bude název hostitele.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-182">One of the simplest and most important settings for any Linux VM would be the hostname.</span></span> <span data-ttu-id="2c6b4-183">Jsme tento parametr můžete snadno nastavit pomocí init cloudových – pomocí tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-183">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="2c6b4-184">Příklad cloudu init skript s názvem `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-184">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```sh
#cloud-config
hostname: myservername
```

<span data-ttu-id="2c6b4-185">Při prvním spuštění virtuálního počítače, nastaví tento skript cloudu init název hostitele na `myservername`.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-185">During the initial startup of the VM, this cloud-init script sets the hostname to `myservername`.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_hostname.txt
```

<span data-ttu-id="2c6b4-186">Přihlášení a ověřte název hostitele nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-186">Login and verify the hostname of the new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a><span data-ttu-id="2c6b4-187">Vytváření cloudu init skriptu aktualizace Linux</span><span class="sxs-lookup"><span data-stu-id="2c6b4-187">Creating a cloud-init script to update Linux</span></span>
<span data-ttu-id="2c6b4-188">Pro zabezpečení budete chtít vaší virtuálního počítače s Ubuntu aktualizovat při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-188">For security, you want your Ubuntu VM to update on the first boot.</span></span>  <span data-ttu-id="2c6b4-189">Pomocí cloudu init, které jsme to udělat pomocí skriptu postupujte podle kroků, v závislosti na Linux distribuce, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-189">Using cloud-init we can do that with the follow script, depending on the Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a><span data-ttu-id="2c6b4-190">Ukázkový skript cloudu init `cloud_config_apt_upgrade.txt` pro Debian rodinu</span><span class="sxs-lookup"><span data-stu-id="2c6b4-190">Example cloud-init script `cloud_config_apt_upgrade.txt` for the Debian Family</span></span>
```sh
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="2c6b4-191">Po Linux, jsou aktualizovány všechny nainstalované balíčky prostřednictvím `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-191">After Linux has booted, all the installed packages are updated via `apt-get`.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_apt_upgrade.txt
```

<span data-ttu-id="2c6b4-192">Přihlášení a ověření, jsou aktualizovány všechny balíčky.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-192">Login and verify all packages are updated.</span></span>

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

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a><span data-ttu-id="2c6b4-193">Vytváření cloudu init skript k přidání uživatele do systému Linux</span><span class="sxs-lookup"><span data-stu-id="2c6b4-193">Creating a cloud-init script to add a user to Linux</span></span>
<span data-ttu-id="2c6b4-194">Jedním z první úlohy na žádné nové virtuální počítače Linux je chcete přidat uživatele pro sebe nebo nepoužívejte `root`.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-194">One of the first tasks on any new Linux VM is to add a user for yourself or to avoid using `root`.</span></span> <span data-ttu-id="2c6b4-195">SSH klíče jsou vhodné pro zabezpečení a použitelnost a jejich přidání do `~/.ssh/authorized_keys` souboru pomocí tohoto skriptu init cloudu.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-195">SSH keys are best practice for security and for usability and they are added to the `~/.ssh/authorized_keys` file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="2c6b4-196">Ukázkový skript cloudu init `cloud_config_add_users.txt` pro Debian řadu</span><span class="sxs-lookup"><span data-stu-id="2c6b4-196">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
```sh
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

<span data-ttu-id="2c6b4-197">Po Linux, jsou uvedené uživatele vytvořen a přidán do skupiny sudo.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-197">After Linux has booted, all the listed users are created and added to the sudo group.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_add_users.txt
```

<span data-ttu-id="2c6b4-198">Přihlášení a ověření nově vytvořeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-198">Login and verify the newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="2c6b4-199">Výstup</span><span class="sxs-lookup"><span data-stu-id="2c6b4-199">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="2c6b4-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c6b4-200">Next Steps</span></span>
<span data-ttu-id="2c6b4-201">Init cloudu se stává stále jednu standardní způsob, jak upravit virtuálním počítačům s Linuxem na spuštění.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-201">Cloud-init is becoming one standard way to modify your Linux VM on boot.</span></span> <span data-ttu-id="2c6b4-202">Azure má také rozšíření virtuálních počítačů, které umožňují upravit vaše LinuxVM na spouštěcí nebo když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-202">Azure also has VM extensions, which allow you to modify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="2c6b4-203">Například můžete použít Azure VMAccessExtension resetovat informace SSH nebo uživatele, když běží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-203">For example, you can use the Azure VMAccessExtension to reset SSH or user information while the VM is running.</span></span> <span data-ttu-id="2c6b4-204">S inicializací cloudu je zapotřebí restartování k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="2c6b4-204">With cloud-init, you would need a reboot to reset the password.</span></span>

[<span data-ttu-id="2c6b4-205">O rozšíření virtuálního počítače a funkce</span><span class="sxs-lookup"><span data-stu-id="2c6b4-205">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="2c6b4-206">Spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="2c6b4-206">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

