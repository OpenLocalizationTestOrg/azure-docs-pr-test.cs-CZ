---
title: "toocustomize aaaUsing init cloudu virtuálního počítače s Linuxem během vytváření v Azure | Microsoft Docs"
description: "Jak cloud init toocustomize toouse a virtuální počítač s Linuxem během vytváření s hello Azure CLI 1.0"
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
ms.openlocfilehash: b9f480bd04029956d0593bbef931795733cbc2f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation-with-hello-azure-cli-10"></a><span data-ttu-id="9c7f0-103">Použít během vytváření s hello Azure CLI 1.0 toocustomize init cloudu virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9c7f0-103">Use cloud-init toocustomize a Linux VM during creation with hello Azure CLI 1.0</span></span>
<span data-ttu-id="9c7f0-104">Tento článek ukazuje, jak toomake tooset skriptu cloudu init hello název hostitele, aktualizace nainstalované balíčky a správa uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-104">This article shows how toomake a cloud-init script tooset hello hostname, update installed packages, and manage user accounts.</span></span>  <span data-ttu-id="9c7f0-105">Hello cloudu init skripty se nazývají během hello vytvoření virtuálního počítače z příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-105">hello cloud-init scripts are called during hello VM creation from Azure CLI.</span></span>  <span data-ttu-id="9c7f0-106">článek Hello vyžaduje:</span><span class="sxs-lookup"><span data-stu-id="9c7f0-106">hello article requires:</span></span>

* <span data-ttu-id="9c7f0-107">účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))</span><span class="sxs-lookup"><span data-stu-id="9c7f0-107">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="9c7f0-108">Hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) přihlášení `azure login`.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-108">hello [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="9c7f0-109">Hello rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-109">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="9c7f0-110">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9c7f0-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="9c7f0-111">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="9c7f0-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="9c7f0-112">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="9c7f0-112">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="9c7f0-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="9c7f0-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="9c7f0-114">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="9c7f0-114">Quick Commands</span></span>
<span data-ttu-id="9c7f0-115">Vytvoření cloudové init.txt skript, který nastaví hello název hostitele, aktualizuje všechny balíčky a přidá tooLinux uživatele sudo.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-115">Create a cloud-init.txt script that sets hello hostname, updates all packages, and adds a sudo user tooLinux.</span></span>

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
<span data-ttu-id="9c7f0-116">Vytvořte virtuální počítače do toolaunch skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-116">Create a resource group toolaunch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="9c7f0-117">Vytvoření virtuálního počítače s Linuxem pomocí cloudu init tooconfigure ji během spouštění.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-117">Create a Linux VM using cloud-init tooconfigure it during boot.</span></span>

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

## <a name="detailed-walkthrough"></a><span data-ttu-id="9c7f0-118">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="9c7f0-118">Detailed walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="9c7f0-119">Úvod</span><span class="sxs-lookup"><span data-stu-id="9c7f0-119">Introduction</span></span>
<span data-ttu-id="9c7f0-120">Při spuštění nového virtuálního počítače s Linuxem se zobrazuje standardní virtuální počítač s Linuxem se nic, vlastní nebo připraveno k vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-120">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="9c7f0-121">[Init cloudu](https://cloudinit.readthedocs.org) je standardní způsob tooinject skriptu nebo konfigurace nastavení do tohoto virtuálního počítače s Linuxem, jako je spuštění pro službu hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-121">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way tooinject a script or configuration settings into that Linux VM as it is booting up for hello first time.</span></span>

<span data-ttu-id="9c7f0-122">V Azure, existují tři různé způsoby toomake změny do virtuálního počítače s Linuxem jako je právě nasazen nebo spuštěn.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-122">On Azure, there are a three different ways toomake changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="9c7f0-123">Vložit skripty s použitím init cloudu.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-123">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="9c7f0-124">Vložit skripty s použitím hello Azure [rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9c7f0-124">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="9c7f0-125">Šablony Azure pomocí init cloudu.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-125">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="9c7f0-126">K pomocí šablony Azure [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9c7f0-126">An Azure template using [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="9c7f0-127">skripty tooinject kdykoli po spuštění:</span><span class="sxs-lookup"><span data-stu-id="9c7f0-127">tooinject scripts at any time after boot:</span></span>

* <span data-ttu-id="9c7f0-128">SSH toorun přímo příkazy</span><span class="sxs-lookup"><span data-stu-id="9c7f0-128">SSH toorun commands directly</span></span>
* <span data-ttu-id="9c7f0-129">Vložit skripty s použitím hello Azure [rozšíření VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperativní nebo šablony Azure</span><span class="sxs-lookup"><span data-stu-id="9c7f0-129">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="9c7f0-130">Nástroje pro správu konfigurace, jako Ansible, Salt, Chef nebo Puppet.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-130">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="9c7f0-131">: Rozšíření VMAccess spustí skript jako kořenový v hello stejný způsobem pomocí protokolu SSH můžete.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-131">: VMAccess Extension executes a script as root in hello same way using SSH can.</span></span>  <span data-ttu-id="9c7f0-132">Však pomocí rozšíření virtuálního počítače hello povolí několik funkcí této nabídky Azure, které mohou být užitečné, v závislosti na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-132">However, using hello VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="9c7f0-133">Init cloudu dostupnosti pro virtuální počítač Azure rychlým vytvořením bitové kopie aliasy:</span><span class="sxs-lookup"><span data-stu-id="9c7f0-133">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="9c7f0-134">Alias</span><span class="sxs-lookup"><span data-stu-id="9c7f0-134">Alias</span></span> | <span data-ttu-id="9c7f0-135">Vydavatel</span><span class="sxs-lookup"><span data-stu-id="9c7f0-135">Publisher</span></span> | <span data-ttu-id="9c7f0-136">Nabídka</span><span class="sxs-lookup"><span data-stu-id="9c7f0-136">Offer</span></span> | <span data-ttu-id="9c7f0-137">Skladová jednotka (SKU)</span><span class="sxs-lookup"><span data-stu-id="9c7f0-137">SKU</span></span> | <span data-ttu-id="9c7f0-138">Verze</span><span class="sxs-lookup"><span data-stu-id="9c7f0-138">Version</span></span> | <span data-ttu-id="9c7f0-139">init cloudu</span><span class="sxs-lookup"><span data-stu-id="9c7f0-139">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="9c7f0-140">CentOS</span><span class="sxs-lookup"><span data-stu-id="9c7f0-140">CentOS</span></span> |<span data-ttu-id="9c7f0-141">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="9c7f0-141">OpenLogic</span></span> |<span data-ttu-id="9c7f0-142">Centos</span><span class="sxs-lookup"><span data-stu-id="9c7f0-142">Centos</span></span> |<span data-ttu-id="9c7f0-143">7.2</span><span class="sxs-lookup"><span data-stu-id="9c7f0-143">7.2</span></span> |<span data-ttu-id="9c7f0-144">nejnovější</span><span class="sxs-lookup"><span data-stu-id="9c7f0-144">latest</span></span> |<span data-ttu-id="9c7f0-145">Ne</span><span class="sxs-lookup"><span data-stu-id="9c7f0-145">no</span></span> |
| <span data-ttu-id="9c7f0-146">CoreOS</span><span class="sxs-lookup"><span data-stu-id="9c7f0-146">CoreOS</span></span> |<span data-ttu-id="9c7f0-147">CoreOS</span><span class="sxs-lookup"><span data-stu-id="9c7f0-147">CoreOS</span></span> |<span data-ttu-id="9c7f0-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="9c7f0-148">CoreOS</span></span> |<span data-ttu-id="9c7f0-149">Stable</span><span class="sxs-lookup"><span data-stu-id="9c7f0-149">Stable</span></span> |<span data-ttu-id="9c7f0-150">nejnovější</span><span class="sxs-lookup"><span data-stu-id="9c7f0-150">latest</span></span> |<span data-ttu-id="9c7f0-151">Ano</span><span class="sxs-lookup"><span data-stu-id="9c7f0-151">yes</span></span> |
| <span data-ttu-id="9c7f0-152">Debian</span><span class="sxs-lookup"><span data-stu-id="9c7f0-152">Debian</span></span> |<span data-ttu-id="9c7f0-153">credativ</span><span class="sxs-lookup"><span data-stu-id="9c7f0-153">credativ</span></span> |<span data-ttu-id="9c7f0-154">Debian</span><span class="sxs-lookup"><span data-stu-id="9c7f0-154">Debian</span></span> |<span data-ttu-id="9c7f0-155">8</span><span class="sxs-lookup"><span data-stu-id="9c7f0-155">8</span></span> |<span data-ttu-id="9c7f0-156">nejnovější</span><span class="sxs-lookup"><span data-stu-id="9c7f0-156">latest</span></span> |<span data-ttu-id="9c7f0-157">Ne</span><span class="sxs-lookup"><span data-stu-id="9c7f0-157">no</span></span> |
| <span data-ttu-id="9c7f0-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="9c7f0-158">openSUSE</span></span> |<span data-ttu-id="9c7f0-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="9c7f0-159">SUSE</span></span> |<span data-ttu-id="9c7f0-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="9c7f0-160">openSUSE</span></span> |<span data-ttu-id="9c7f0-161">13.2</span><span class="sxs-lookup"><span data-stu-id="9c7f0-161">13.2</span></span> |<span data-ttu-id="9c7f0-162">nejnovější</span><span class="sxs-lookup"><span data-stu-id="9c7f0-162">latest</span></span> |<span data-ttu-id="9c7f0-163">Ne</span><span class="sxs-lookup"><span data-stu-id="9c7f0-163">no</span></span> |
| <span data-ttu-id="9c7f0-164">RHEL</span><span class="sxs-lookup"><span data-stu-id="9c7f0-164">RHEL</span></span> |<span data-ttu-id="9c7f0-165">Redhat</span><span class="sxs-lookup"><span data-stu-id="9c7f0-165">Redhat</span></span> |<span data-ttu-id="9c7f0-166">RHEL</span><span class="sxs-lookup"><span data-stu-id="9c7f0-166">RHEL</span></span> |<span data-ttu-id="9c7f0-167">7.2</span><span class="sxs-lookup"><span data-stu-id="9c7f0-167">7.2</span></span> |<span data-ttu-id="9c7f0-168">nejnovější</span><span class="sxs-lookup"><span data-stu-id="9c7f0-168">latest</span></span> |<span data-ttu-id="9c7f0-169">Ne</span><span class="sxs-lookup"><span data-stu-id="9c7f0-169">no</span></span> |
| <span data-ttu-id="9c7f0-170">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="9c7f0-170">UbuntuLTS</span></span> |<span data-ttu-id="9c7f0-171">Canonical</span><span class="sxs-lookup"><span data-stu-id="9c7f0-171">Canonical</span></span> |<span data-ttu-id="9c7f0-172">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="9c7f0-172">UbuntuServer</span></span> |<span data-ttu-id="9c7f0-173">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="9c7f0-173">14.04.4-LTS</span></span> |<span data-ttu-id="9c7f0-174">nejnovější</span><span class="sxs-lookup"><span data-stu-id="9c7f0-174">latest</span></span> |<span data-ttu-id="9c7f0-175">Ano</span><span class="sxs-lookup"><span data-stu-id="9c7f0-175">yes</span></span> |

<span data-ttu-id="9c7f0-176">Společnost Microsoft se práce s našich partnerů tooget cloudu inicializací zahrnuté a práci v hello bitové kopie, aby umožňovala tooAzure.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-176">Microsoft is working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span>

## <a name="adding-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a><span data-ttu-id="9c7f0-177">Přidání cloudu init skriptu toohello vytvoření virtuálních počítačů s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="9c7f0-177">Adding a cloud-init script toohello VM creation with hello Azure CLI</span></span>
<span data-ttu-id="9c7f0-178">toolaunch skript cloudu init při vytváření virtuálního počítače v Azure, zadejte soubor cloudu init hello pomocí rozhraní příkazového řádku Azure hello `--custom-data` přepínače.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-178">toolaunch a cloud-init script when creating a VM in Azure, specify hello cloud-init file using hello Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="9c7f0-179">Vytvořte virtuální počítače do toolaunch skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-179">Create a resource group toolaunch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="9c7f0-180">Vytvoření virtuálního počítače s Linuxem pomocí cloudu init tooconfigure ji během spouštění.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-180">Create a Linux VM using cloud-init tooconfigure it during boot.</span></span>

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

## <a name="creating-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a><span data-ttu-id="9c7f0-181">Vytváření cloudu init skriptu tooset hello název hostitele virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9c7f0-181">Creating a cloud-init script tooset hello hostname of a Linux VM</span></span>
<span data-ttu-id="9c7f0-182">Jedním z nejdůležitějších kroků nastavení pro všechny virtuální počítač s Linuxem a hello nejjednodušší by hello název hostitele.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-182">One of hello simplest and most important settings for any Linux VM would be hello hostname.</span></span> <span data-ttu-id="9c7f0-183">Jsme tento parametr můžete snadno nastavit pomocí init cloudových – pomocí tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-183">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="9c7f0-184">Příklad cloudu init skript s názvem `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-184">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```sh
#cloud-config
hostname: myservername
```

<span data-ttu-id="9c7f0-185">Během hello počáteční spouštění hello virtuálních počítačů, tento skript cloudu init nastaví název hostitele hello příliš`myservername`.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-185">During hello initial startup of hello VM, this cloud-init script sets hello hostname too`myservername`.</span></span>

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

<span data-ttu-id="9c7f0-186">Přihlášení a ověřte název hostitele hello hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-186">Login and verify hello hostname of hello new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-tooupdate-linux"></a><span data-ttu-id="9c7f0-187">Vytváření cloudu init skriptu tooupdate Linux</span><span class="sxs-lookup"><span data-stu-id="9c7f0-187">Creating a cloud-init script tooupdate Linux</span></span>
<span data-ttu-id="9c7f0-188">Pro zabezpečení budete chtít vaší tooupdate virtuálního počítače s Ubuntu na hello při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-188">For security, you want your Ubuntu VM tooupdate on hello first boot.</span></span>  <span data-ttu-id="9c7f0-189">Pomocí cloudu init můžeme udělat, aby s hello podle skriptu, v závislosti na distribuční hello Linux, který používáte.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-189">Using cloud-init we can do that with hello follow script, depending on hello Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a><span data-ttu-id="9c7f0-190">Ukázkový skript cloudu init `cloud_config_apt_upgrade.txt` pro hello Debian rodiny</span><span class="sxs-lookup"><span data-stu-id="9c7f0-190">Example cloud-init script `cloud_config_apt_upgrade.txt` for hello Debian Family</span></span>
```sh
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="9c7f0-191">Po Linux, jsou aktualizovány všechny balíčky hello nainstalovaná prostřednictvím `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-191">After Linux has booted, all hello installed packages are updated via `apt-get`.</span></span>

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

<span data-ttu-id="9c7f0-192">Přihlášení a ověření, jsou aktualizovány všechny balíčky.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-192">Login and verify all packages are updated.</span></span>

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

## <a name="creating-a-cloud-init-script-tooadd-a-user-toolinux"></a><span data-ttu-id="9c7f0-193">Vytváření cloudu init skriptu tooadd tooLinux uživatele</span><span class="sxs-lookup"><span data-stu-id="9c7f0-193">Creating a cloud-init script tooadd a user tooLinux</span></span>
<span data-ttu-id="9c7f0-194">Jedním z první úlohy hello na žádné nové virtuální počítače Linux je tooadd uživatele pro sebe nebo pomocí tooavoid `root`.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-194">One of hello first tasks on any new Linux VM is tooadd a user for yourself or tooavoid using `root`.</span></span> <span data-ttu-id="9c7f0-195">SSH klíče jsou vhodné pro zabezpečení a použitelnost a přidají se toohello `~/.ssh/authorized_keys` souboru pomocí tohoto skriptu init cloudu.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-195">SSH keys are best practice for security and for usability and they are added toohello `~/.ssh/authorized_keys` file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="9c7f0-196">Ukázkový skript cloudu init `cloud_config_add_users.txt` pro Debian řadu</span><span class="sxs-lookup"><span data-stu-id="9c7f0-196">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
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

<span data-ttu-id="9c7f0-197">Po Linux, všichni uživatelé hello uvedené jsou skupiny vytvořené a přidané toohello sudo.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-197">After Linux has booted, all hello listed users are created and added toohello sudo group.</span></span>

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

<span data-ttu-id="9c7f0-198">Přihlášení a ověření hello nově vytvořeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-198">Login and verify hello newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="9c7f0-199">Výstup</span><span class="sxs-lookup"><span data-stu-id="9c7f0-199">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="9c7f0-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c7f0-200">Next Steps</span></span>
<span data-ttu-id="9c7f0-201">Init cloudu se stává stále jednu standardní způsob toomodify virtuálním počítačům s Linuxem na spuštění.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-201">Cloud-init is becoming one standard way toomodify your Linux VM on boot.</span></span> <span data-ttu-id="9c7f0-202">Azure má také rozšíření virtuálních počítačů, které umožňují toomodify vaše LinuxVM na spouštěcí nebo když je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-202">Azure also has VM extensions, which allow you toomodify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="9c7f0-203">Například můžete hello Azure VMAccessExtension tooreset SSH nebo informace o uživateli při hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-203">For example, you can use hello Azure VMAccessExtension tooreset SSH or user information while hello VM is running.</span></span> <span data-ttu-id="9c7f0-204">S inicializací cloudu bude třeba heslo hello tooreset restartování.</span><span class="sxs-lookup"><span data-stu-id="9c7f0-204">With cloud-init, you would need a reboot tooreset hello password.</span></span>

[<span data-ttu-id="9c7f0-205">O rozšíření virtuálního počítače a funkce</span><span class="sxs-lookup"><span data-stu-id="9c7f0-205">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="9c7f0-206">Spravovat uživatele, SSH a zkontrolujte nebo hello oprava disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="9c7f0-206">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

