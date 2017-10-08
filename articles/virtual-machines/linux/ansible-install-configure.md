---
title: "aaaInstall a konfigurace Ansible pro použití s virtuálními počítači Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall a nakonfigurujte Ansible pro správu prostředků Azure na SLES, Ubuntu a CentOS"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2017
ms.author: iainfou
ms.openlocfilehash: b33d1893909b6134a5474617c9af2d6e4f627c05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-ansible-toomanage-virtual-machines-in-azure"></a><span data-ttu-id="c41d4-103">Instalace a konfigurace Ansible toomanage virtuální počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="c41d4-103">Install and configure Ansible toomanage virtual machines in Azure</span></span>
<span data-ttu-id="c41d4-104">Tento článek podrobně popisuje, jak tooinstall Ansible a požadované moduly Azure Python SDK pro některé hello nejběžnější distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c41d4-104">This article details how tooinstall Ansible and required Azure Python SDK modules for some of hello most common Linux distros.</span></span> <span data-ttu-id="c41d4-105">Můžete nainstalovat Ansible na jiné distribucích úpravou hello nainstalované balíčky toofit konkrétní platformu.</span><span class="sxs-lookup"><span data-stu-id="c41d4-105">You can install Ansible on other distros by adjusting hello installed packages toofit your particular platform.</span></span> <span data-ttu-id="c41d4-106">toocreate Azure prostředkům zabezpečené způsobem, můžete si také přečíst jak toocreate a zadejte přihlašovací údaje pro Ansible toouse.</span><span class="sxs-lookup"><span data-stu-id="c41d4-106">toocreate Azure resources in a secure manner, you also learn how toocreate and define credentials for Ansible toouse.</span></span> 

<span data-ttu-id="c41d4-107">Další možnosti instalace a kroky pro další platformy najdete v tématu hello [Průvodce instalací Ansible](https://docs.ansible.com/ansible/intro_installation.html).</span><span class="sxs-lookup"><span data-stu-id="c41d4-107">For more installation options and steps for additional platforms, see hello [Ansible install guide](https://docs.ansible.com/ansible/intro_installation.html).</span></span>


## <a name="install-ansible"></a><span data-ttu-id="c41d4-108">Nainstalujte Ansible</span><span class="sxs-lookup"><span data-stu-id="c41d4-108">Install Ansible</span></span>
<span data-ttu-id="c41d4-109">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="c41d4-109">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="c41d4-110">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAnsible* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="c41d4-110">hello following example creates a resource group named *myResourceGroupAnsible* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

<span data-ttu-id="c41d4-111">Teď vytvořte virtuální počítač a nainstalujte Ansible pro jednu z následujících distribucích hello:</span><span class="sxs-lookup"><span data-stu-id="c41d4-111">Now create a VM and install Ansible for one of hello following distros:</span></span>

- [<span data-ttu-id="c41d4-112">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="c41d4-112">Ubuntu 16.04 LTS</span></span>](#ubuntu1604-lts)
- [<span data-ttu-id="c41d4-113">CentOS 7.3</span><span class="sxs-lookup"><span data-stu-id="c41d4-113">CentOS 7.3</span></span>](#centos-73)
- [<span data-ttu-id="c41d4-114">SLES 12.2 SP2</span><span class="sxs-lookup"><span data-stu-id="c41d4-114">SLES 12.2 SP2</span></span>](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="c41d4-115">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="c41d4-115">Ubuntu 16.04 LTS</span></span>
<span data-ttu-id="c41d4-116">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="c41d4-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="c41d4-117">Hello následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="c41d4-117">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="c41d4-118">SSH tooyour virtuální počítač pomocí hello `publicIpAddress` si poznamenali v hello operace vytvoření výstup hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="c41d4-118">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="c41d4-119">Na virtuální počítač nainstalujte hello požadované balíčky pro moduly Azure Python SDK hello a Ansible následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c41d4-119">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Azure SDKs via pip
pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via apt
sudo apt-get install -y software-properties-common
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update && sudo apt-get install -y ansible
```

<span data-ttu-id="c41d4-120">Nyní přesunout příliš[přihlašovací údaje Azure vytvořit](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="c41d4-120">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="centos-73"></a><span data-ttu-id="c41d4-121">CentOS 7.3</span><span class="sxs-lookup"><span data-stu-id="c41d4-121">CentOS 7.3</span></span>
<span data-ttu-id="c41d4-122">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="c41d4-122">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="c41d4-123">Hello následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="c41d4-123">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="c41d4-124">SSH tooyour virtuální počítač pomocí hello `publicIpAddress` si poznamenali v hello operace vytvoření výstup hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="c41d4-124">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="c41d4-125">Na virtuální počítač nainstalujte hello požadované balíčky pro moduly Azure Python SDK hello a Ansible následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c41d4-125">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

<span data-ttu-id="c41d4-126">Nyní přesunout příliš[přihlašovací údaje Azure vytvořit](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="c41d4-126">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="sles-122-sp2"></a><span data-ttu-id="c41d4-127">SLES 12.2 SP2</span><span class="sxs-lookup"><span data-stu-id="c41d4-127">SLES 12.2 SP2</span></span>
<span data-ttu-id="c41d4-128">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="c41d4-128">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="c41d4-129">Hello následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="c41d4-129">hello following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="c41d4-130">SSH tooyour virtuální počítač pomocí hello `publicIpAddress` si poznamenali v hello operace vytvoření výstup hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="c41d4-130">SSH tooyour VM using hello `publicIpAddress` noted in hello output from hello VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="c41d4-131">Na virtuální počítač nainstalujte hello požadované balíčky pro moduly Azure Python SDK hello a Ansible následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c41d4-131">On your VM, install hello required packages for hello Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

<span data-ttu-id="c41d4-132">Nyní přesunout příliš[přihlašovací údaje Azure vytvořit](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="c41d4-132">Now move on too[Create Azure credentials](#create-azure-credentials).</span></span>


## <a name="create-azure-credentials"></a><span data-ttu-id="c41d4-133">Vytvořit přihlašovací údaje Azure</span><span class="sxs-lookup"><span data-stu-id="c41d4-133">Create Azure credentials</span></span>
<span data-ttu-id="c41d4-134">Ansible komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="c41d4-134">Ansible communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="c41d4-135">Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například Ansible.</span><span class="sxs-lookup"><span data-stu-id="c41d4-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Ansible.</span></span> <span data-ttu-id="c41d4-136">Můžete řídit a definovat hello oprávnění objektu služby hello toowhat operace můžete provádět v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="c41d4-136">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span> <span data-ttu-id="c41d4-137">tooimprove zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.</span><span class="sxs-lookup"><span data-stu-id="c41d4-137">tooimprove security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="c41d4-138">Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstup hello pověření, které potřebuje Ansible:</span><span class="sxs-lookup"><span data-stu-id="c41d4-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that Ansible needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="c41d4-139">Příklad výstupu hello z předchozích příkazů hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c41d4-139">An example of hello output from hello preceding commands is as follows:</span></span>

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

<span data-ttu-id="c41d4-140">tooauthenticate tooAzure, musíte taky tooobtain ID vašeho předplatného Azure s [az účet zobrazit](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="c41d4-140">tooauthenticate tooAzure, you also need tooobtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="c41d4-141">V dalším kroku hello používáte hello výstup z těchto dvou příkazů.</span><span class="sxs-lookup"><span data-stu-id="c41d4-141">You use hello output from these two commands in hello next step.</span></span>


## <a name="create-ansible-credentials-file"></a><span data-ttu-id="c41d4-142">Vytvořte soubor s přihlašovacími údaji Ansible</span><span class="sxs-lookup"><span data-stu-id="c41d4-142">Create Ansible credentials file</span></span>
<span data-ttu-id="c41d4-143">tooprovide pověření tooAnsible, definujte proměnné prostředí, nebo vytvořit soubor místní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c41d4-143">tooprovide credentials tooAnsible, you define environment variables or create a local credentials file.</span></span> <span data-ttu-id="c41d4-144">Další informace o tom, najdete v části přihlašovací údaje Ansible toodefine, [poskytování pověření tooAzure moduly](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span><span class="sxs-lookup"><span data-stu-id="c41d4-144">For more information about how toodefine Ansible credentials, see [Providing Credentials tooAzure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span></span> 

<span data-ttu-id="c41d4-145">Vývojové prostředí, vytvářet *pověření* souboru Ansible na hostiteli virtuálního počítače takto:</span><span class="sxs-lookup"><span data-stu-id="c41d4-145">For a development environment, create a *credentials* file for Ansible on your host VM as follows:</span></span>

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

<span data-ttu-id="c41d4-146">Hello *pověření* samotný soubor kombinuje ID předplatného hello výstup hello vytvoření objektu služby.</span><span class="sxs-lookup"><span data-stu-id="c41d4-146">hello *credentials* file itself combines hello subscription ID with hello output of creating a service principal.</span></span> <span data-ttu-id="c41d4-147">Výstup z předchozí hello [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) příkaz je hello stejné pořadí podle potřeby pro *client_id*, *tajný klíč*, a *klienta* .</span><span class="sxs-lookup"><span data-stu-id="c41d4-147">Output from hello previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is hello same order as needed for *client_id*, *secret*, and *tenant*.</span></span> <span data-ttu-id="c41d4-148">Následující příklad Hello *pověření* soubor znázorňuje tyto hodnoty odpovídající předchozí výstup hello.</span><span class="sxs-lookup"><span data-stu-id="c41d4-148">hello following example *credentials* file shows these values matching hello previous output.</span></span> <span data-ttu-id="c41d4-149">Zadejte vlastní hodnoty následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c41d4-149">Enter your own values as follows:</span></span>

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a><span data-ttu-id="c41d4-150">Použití proměnných prostředí Ansible</span><span class="sxs-lookup"><span data-stu-id="c41d4-150">Use Ansible environment variables</span></span>
<span data-ttu-id="c41d4-151">Pokud chcete toouse nástroje, například Ansible věž nebo volaných, můžete definovat proměnné prostředí následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c41d4-151">If you are going toouse tools such as Ansible Tower or Jenkins, you can define environment variables as follows.</span></span> <span data-ttu-id="c41d4-152">Tyto proměnné kombinovat s výstup hello vytvoření služby hlavní hello ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="c41d4-152">These variables combine hello subscription ID with hello output from creating a service principal.</span></span> <span data-ttu-id="c41d4-153">Výstup z předchozí hello [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) příkaz je hello stejné pořadí podle potřeby pro *AZURE_CLIENT_ID*, *AZURE_SECRET*, a *AZURE_ KLIENTA*.</span><span class="sxs-lookup"><span data-stu-id="c41d4-153">Output from hello previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is hello same order as needed for *AZURE_CLIENT_ID*, *AZURE_SECRET*, and *AZURE_TENANT*.</span></span> 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a><span data-ttu-id="c41d4-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c41d4-154">Next steps</span></span>
<span data-ttu-id="c41d4-155">Nyní máte Ansible a hello požadované moduly Azure Python SDK nainstalovat a přihlašovací údaje definované pro Ansible toouse.</span><span class="sxs-lookup"><span data-stu-id="c41d4-155">You now have Ansible and hello required Azure Python SDK modules installed, and credentials defined for Ansible toouse.</span></span> <span data-ttu-id="c41d4-156">Zjistěte, jak příliš[vytvoření virtuálního počítače s Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="c41d4-156">Learn how too[create a VM with Ansible](ansible-create-vm.md).</span></span> <span data-ttu-id="c41d4-157">Můžete si také přečíst jak příliš[kompletní virtuální počítače Azure můžete vytvořit a podpůrné prostředky s Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="c41d4-157">You can also learn how too[create a complete Azure VM and supporting resources with Ansible](ansible-create-complete-vm.md).</span></span>
