---
title: "Instalace a konfigurace Ansible pro použití s virtuálními počítači Azure | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat Ansible pro správu prostředků Azure na SLES, Ubuntu a CentOS"
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
ms.openlocfilehash: 52b763274437961dccfc862c8a45fbd57ea9fc4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a><span data-ttu-id="484f9-103">Instalace a konfigurace Ansible ke správě virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="484f9-103">Install and configure Ansible to manage virtual machines in Azure</span></span>
<span data-ttu-id="484f9-104">Tento článek podrobně popisuje postup instalace Ansible a požadované moduly Azure Python SDK pro některé z nejběžnějších distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="484f9-104">This article details how to install Ansible and required Azure Python SDK modules for some of the most common Linux distros.</span></span> <span data-ttu-id="484f9-105">Ansible můžete nainstalovat na jiné distribucích úpravou nainstalované balíčky podle vaší konkrétní platformu.</span><span class="sxs-lookup"><span data-stu-id="484f9-105">You can install Ansible on other distros by adjusting the installed packages to fit your particular platform.</span></span> <span data-ttu-id="484f9-106">Vytváření prostředků Azure zabezpečeným způsobem, můžete také zjistěte, jak vytvořit a definovat přihlašovací údaje pro Ansible používat.</span><span class="sxs-lookup"><span data-stu-id="484f9-106">To create Azure resources in a secure manner, you also learn how to create and define credentials for Ansible to use.</span></span> 

<span data-ttu-id="484f9-107">Další možnosti instalace a kroky pro další platformy najdete v tématu [Průvodce instalací Ansible](https://docs.ansible.com/ansible/intro_installation.html).</span><span class="sxs-lookup"><span data-stu-id="484f9-107">For more installation options and steps for additional platforms, see the [Ansible install guide](https://docs.ansible.com/ansible/intro_installation.html).</span></span>


## <a name="install-ansible"></a><span data-ttu-id="484f9-108">Nainstalujte Ansible</span><span class="sxs-lookup"><span data-stu-id="484f9-108">Install Ansible</span></span>
<span data-ttu-id="484f9-109">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="484f9-109">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="484f9-110">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAnsible* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="484f9-110">The following example creates a resource group named *myResourceGroupAnsible* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

<span data-ttu-id="484f9-111">Teď vytvořte virtuální počítač a nainstalujte Ansible pro jednu z následujících distribucích:</span><span class="sxs-lookup"><span data-stu-id="484f9-111">Now create a VM and install Ansible for one of the following distros:</span></span>

- [<span data-ttu-id="484f9-112">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="484f9-112">Ubuntu 16.04 LTS</span></span>](#ubuntu1604-lts)
- [<span data-ttu-id="484f9-113">CentOS 7.3</span><span class="sxs-lookup"><span data-stu-id="484f9-113">CentOS 7.3</span></span>](#centos-73)
- [<span data-ttu-id="484f9-114">SLES 12.2 SP2</span><span class="sxs-lookup"><span data-stu-id="484f9-114">SLES 12.2 SP2</span></span>](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="484f9-115">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="484f9-115">Ubuntu 16.04 LTS</span></span>
<span data-ttu-id="484f9-116">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="484f9-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="484f9-117">Následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="484f9-117">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="484f9-118">SSH pro virtuální počítač pomocí `publicIpAddress` uvedeno ve výstupu z virtuálního počítače vytvořit operace:</span><span class="sxs-lookup"><span data-stu-id="484f9-118">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="484f9-119">Na vašem virtuálním počítači nainstalujte požadované balíčky pro moduly Azure Python SDK a Ansible následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="484f9-119">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

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

<span data-ttu-id="484f9-120">Nyní se přesunout na [přihlašovací údaje Azure vytvořit](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="484f9-120">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="centos-73"></a><span data-ttu-id="484f9-121">CentOS 7.3</span><span class="sxs-lookup"><span data-stu-id="484f9-121">CentOS 7.3</span></span>
<span data-ttu-id="484f9-122">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="484f9-122">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="484f9-123">Následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="484f9-123">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="484f9-124">SSH pro virtuální počítač pomocí `publicIpAddress` uvedeno ve výstupu z virtuálního počítače vytvořit operace:</span><span class="sxs-lookup"><span data-stu-id="484f9-124">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="484f9-125">Na vašem virtuálním počítači nainstalujte požadované balíčky pro moduly Azure Python SDK a Ansible následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="484f9-125">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

<span data-ttu-id="484f9-126">Nyní se přesunout na [přihlašovací údaje Azure vytvořit](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="484f9-126">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


### <a name="sles-122-sp2"></a><span data-ttu-id="484f9-127">SLES 12.2 SP2</span><span class="sxs-lookup"><span data-stu-id="484f9-127">SLES 12.2 SP2</span></span>
<span data-ttu-id="484f9-128">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="484f9-128">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="484f9-129">Následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:</span><span class="sxs-lookup"><span data-stu-id="484f9-129">The following example creates a VM named *myVMAnsible*:</span></span>

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="484f9-130">SSH pro virtuální počítač pomocí `publicIpAddress` uvedeno ve výstupu z virtuálního počítače vytvořit operace:</span><span class="sxs-lookup"><span data-stu-id="484f9-130">SSH to your VM using the `publicIpAddress` noted in the output from the VM create operation:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="484f9-131">Na vašem virtuálním počítači nainstalujte požadované balíčky pro moduly Azure Python SDK a Ansible následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="484f9-131">On your VM, install the required packages for the Azure Python SDK modules and Ansible as follows:</span></span>

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

<span data-ttu-id="484f9-132">Nyní se přesunout na [přihlašovací údaje Azure vytvořit](#create-azure-credentials).</span><span class="sxs-lookup"><span data-stu-id="484f9-132">Now move on to [Create Azure credentials](#create-azure-credentials).</span></span>


## <a name="create-azure-credentials"></a><span data-ttu-id="484f9-133">Vytvořit přihlašovací údaje Azure</span><span class="sxs-lookup"><span data-stu-id="484f9-133">Create Azure credentials</span></span>
<span data-ttu-id="484f9-134">Ansible komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="484f9-134">Ansible communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="484f9-135">Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například Ansible.</span><span class="sxs-lookup"><span data-stu-id="484f9-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Ansible.</span></span> <span data-ttu-id="484f9-136">Můžete řídit a definovat oprávnění, jaké operace objektu služby můžete provádět v Azure.</span><span class="sxs-lookup"><span data-stu-id="484f9-136">You control and define the permissions as to what operations the service principal can perform in Azure.</span></span> <span data-ttu-id="484f9-137">Pokud chcete zvýšit zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.</span><span class="sxs-lookup"><span data-stu-id="484f9-137">To improve security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="484f9-138">Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstupní přihlašovací údaje, které potřebuje Ansible:</span><span class="sxs-lookup"><span data-stu-id="484f9-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output the credentials that Ansible needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="484f9-139">Příklad výstupu z předchozích příkazů vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="484f9-139">An example of the output from the preceding commands is as follows:</span></span>

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

<span data-ttu-id="484f9-140">K ověření do Azure, musíte také získat svoje ID předplatného Azure s [az účet zobrazit](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="484f9-140">To authenticate to Azure, you also need to obtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="484f9-141">V dalším kroku použijete výstup z těchto dvou příkazů.</span><span class="sxs-lookup"><span data-stu-id="484f9-141">You use the output from these two commands in the next step.</span></span>


## <a name="create-ansible-credentials-file"></a><span data-ttu-id="484f9-142">Vytvořte soubor s přihlašovacími údaji Ansible</span><span class="sxs-lookup"><span data-stu-id="484f9-142">Create Ansible credentials file</span></span>
<span data-ttu-id="484f9-143">K zadání přihlašovacích údajů k Ansible, můžete definovat proměnné prostředí nebo vytvoření přihlašovacích údajů místního souboru.</span><span class="sxs-lookup"><span data-stu-id="484f9-143">To provide credentials to Ansible, you define environment variables or create a local credentials file.</span></span> <span data-ttu-id="484f9-144">Další informace o tom, jak definovat Ansible pověření najdete v tématu [poskytování pověření moduly Azure](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span><span class="sxs-lookup"><span data-stu-id="484f9-144">For more information about how to define Ansible credentials, see [Providing Credentials to Azure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).</span></span> 

<span data-ttu-id="484f9-145">Vývojové prostředí, vytvářet *pověření* souboru Ansible na hostiteli virtuálního počítače takto:</span><span class="sxs-lookup"><span data-stu-id="484f9-145">For a development environment, create a *credentials* file for Ansible on your host VM as follows:</span></span>

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

<span data-ttu-id="484f9-146">*Pověření* samotný soubor kombinuje ID předplatného s výstupem vytvoření objektu služby.</span><span class="sxs-lookup"><span data-stu-id="484f9-146">The *credentials* file itself combines the subscription ID with the output of creating a service principal.</span></span> <span data-ttu-id="484f9-147">Výstup z předchozí [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) příkaz je stejné pořadí podle potřeby pro *client_id*, *tajný klíč*, a *klienta*.</span><span class="sxs-lookup"><span data-stu-id="484f9-147">Output from the previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is the same order as needed for *client_id*, *secret*, and *tenant*.</span></span> <span data-ttu-id="484f9-148">Následující příklad *pověření* soubor znázorňuje tyto hodnoty odpovídající předchozí výstup.</span><span class="sxs-lookup"><span data-stu-id="484f9-148">The following example *credentials* file shows these values matching the previous output.</span></span> <span data-ttu-id="484f9-149">Zadejte vlastní hodnoty následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="484f9-149">Enter your own values as follows:</span></span>

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a><span data-ttu-id="484f9-150">Použití proměnných prostředí Ansible</span><span class="sxs-lookup"><span data-stu-id="484f9-150">Use Ansible environment variables</span></span>
<span data-ttu-id="484f9-151">Pokud budete používat nástroje, například Ansible věž nebo volaných, můžete definovat proměnné prostředí následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="484f9-151">If you are going to use tools such as Ansible Tower or Jenkins, you can define environment variables as follows.</span></span> <span data-ttu-id="484f9-152">Tyto proměnné kombinovat s výstupem vytvoření služby Hlavní ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="484f9-152">These variables combine the subscription ID with the output from creating a service principal.</span></span> <span data-ttu-id="484f9-153">Výstup z předchozí [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) příkaz je stejné pořadí podle potřeby pro *AZURE_CLIENT_ID*, *AZURE_SECRET*, a *AZURE_TENANT*.</span><span class="sxs-lookup"><span data-stu-id="484f9-153">Output from the previous [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) command is the same order as needed for *AZURE_CLIENT_ID*, *AZURE_SECRET*, and *AZURE_TENANT*.</span></span> 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a><span data-ttu-id="484f9-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="484f9-154">Next steps</span></span>
<span data-ttu-id="484f9-155">Nyní máte Ansible a požadované moduly Azure Python SDK nainstalovat a přihlašovací údaje definované pro Ansible používat.</span><span class="sxs-lookup"><span data-stu-id="484f9-155">You now have Ansible and the required Azure Python SDK modules installed, and credentials defined for Ansible to use.</span></span> <span data-ttu-id="484f9-156">Zjistěte, jak [vytvoření virtuálního počítače s Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="484f9-156">Learn how to [create a VM with Ansible](ansible-create-vm.md).</span></span> <span data-ttu-id="484f9-157">Můžete si také přečíst postup [kompletní virtuální počítače Azure můžete vytvořit a podpůrné prostředky s Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="484f9-157">You can also learn how to [create a complete Azure VM and supporting resources with Ansible](ansible-create-complete-vm.md).</span></span>