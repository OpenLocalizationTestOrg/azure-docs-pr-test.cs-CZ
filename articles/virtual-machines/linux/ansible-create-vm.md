---
title: "Vytvořte základní virtuální počítač s Linuxem v Azure pomocí Ansible | Microsoft Docs"
description: "Další informace o použití Ansible k vytváření a správě základní virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: 526f3522334450564500c4ba0e401a683cae55f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a><span data-ttu-id="af2b3-103">Vytvoření základního virtuálního počítače v Azure pomocí Ansible</span><span class="sxs-lookup"><span data-stu-id="af2b3-103">Create a basic virtual machine in Azure with Ansible</span></span>
<span data-ttu-id="af2b3-104">Ansible umožňuje automatizovat nasazení a konfigurace prostředků ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="af2b3-104">Ansible allows you to automate the deployment and configuration of resources in your environment.</span></span> <span data-ttu-id="af2b3-105">Ansible můžete použít ke správě virtuálních počítačů (VM) v Azure, stejně jako jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="af2b3-105">You can use Ansible to manage your virtual machines (VMs) in Azure, the same as you would any other resource.</span></span> <span data-ttu-id="af2b3-106">Tento článek ukazuje, jak vytvořit základní virtuální počítač s Ansible.</span><span class="sxs-lookup"><span data-stu-id="af2b3-106">This article shows you how to create a basic VM with Ansible.</span></span> <span data-ttu-id="af2b3-107">Můžete si také přečíst postup [vytvořit úplný prostředí virtuálních počítačů s Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="af2b3-107">You can also learn how to [Create a complete VM environment with Ansible](ansible-create-complete-vm.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="af2b3-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="af2b3-108">Prerequisites</span></span>
<span data-ttu-id="af2b3-109">Ke správě prostředků Azure s Ansible, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="af2b3-109">To manage Azure resources with Ansible, you need the following:</span></span>

- <span data-ttu-id="af2b3-110">Ansible a moduly Azure Python SDK v systému hostitele.</span><span class="sxs-lookup"><span data-stu-id="af2b3-110">Ansible and the Azure Python SDK modules installed on your host system.</span></span>
    - <span data-ttu-id="af2b3-111">Nainstalujte Ansible [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), a [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span><span class="sxs-lookup"><span data-stu-id="af2b3-111">Install Ansible on [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), and [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span></span>
- <span data-ttu-id="af2b3-112">Přihlašovací údaje Azure a Ansible nakonfigurovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="af2b3-112">Azure credentials, and Ansible configured to use them.</span></span>
    - [<span data-ttu-id="af2b3-113">Vytvořit přihlašovací údaje Azure a nakonfigurovat Ansible</span><span class="sxs-lookup"><span data-stu-id="af2b3-113">Create Azure credentials and configure Ansible</span></span>](ansible-install-configure.md#create-azure-credentials)
- <span data-ttu-id="af2b3-114">Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="af2b3-114">Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="af2b3-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="af2b3-115">Run `az --version` to find the version.</span></span> 
    - <span data-ttu-id="af2b3-116">Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="af2b3-116">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> <span data-ttu-id="af2b3-117">Můžete také použít [cloudové prostředí](/azure/cloud-shell/quickstart) z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="af2b3-117">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>


## <a name="create-supporting-azure-resources"></a><span data-ttu-id="af2b3-118">Vytvoření Podpora prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="af2b3-118">Create supporting Azure resources</span></span>
<span data-ttu-id="af2b3-119">V tomto příkladu vytvoříme sadu runbook, která nasadí virtuální počítač do existující infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="af2b3-119">In this example, we create a runbook that deploys a VM into an existing infrastructure.</span></span> <span data-ttu-id="af2b3-120">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="af2b3-120">First, create resource group with [az group create](/cli/azure/vm#create).</span></span> <span data-ttu-id="af2b3-121">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="af2b3-121">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="af2b3-122">Vytvoření virtuální sítě pro virtuální počítač s [vytvoření sítě vnet az](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="af2b3-122">Create a virtual network for your VM with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="af2b3-123">Následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="af2b3-123">The following example creates a virtual network named *myVnet* and a subnet named *mySubnet*:</span></span>

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a><span data-ttu-id="af2b3-124">Vytvoření a spuštění Ansible playbook</span><span class="sxs-lookup"><span data-stu-id="af2b3-124">Create and run Ansible playbook</span></span>
<span data-ttu-id="af2b3-125">Vytvoření Ansible playbook s názvem **azure_create_vm.yml** a vložte následující obsah.</span><span class="sxs-lookup"><span data-stu-id="af2b3-125">Create an Ansible playbook named **azure_create_vm.yml** and paste the following contents.</span></span> <span data-ttu-id="af2b3-126">Tento příklad vytvoří jeden virtuální počítač a nakonfiguruje pověření SSH.</span><span class="sxs-lookup"><span data-stu-id="af2b3-126">This example creates a single VM and configures SSH credentials.</span></span> <span data-ttu-id="af2b3-127">Zadejte svoje vlastní veřejného klíče data v *key_data* spárujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="af2b3-127">Enter your own public key data in the *key_data* pair as follows:</span></span>

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys: 
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

<span data-ttu-id="af2b3-128">Pokud chcete vytvořit virtuální počítač s Ansible, spusťte playbook následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="af2b3-128">To create the VM with Ansible, run the playbook as follows:</span></span>

```bash
ansible-playbook azure_create_vm.yml
```

<span data-ttu-id="af2b3-129">Výstup bude vypadat podobně jako v následujícím příkladu, který ukazuje, že virtuální počítač se úspěšně vytvořil:</span><span class="sxs-lookup"><span data-stu-id="af2b3-129">The output looks similar to the following example that shows the VM has been successfully created:</span></span>

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a><span data-ttu-id="af2b3-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af2b3-130">Next steps</span></span>
<span data-ttu-id="af2b3-131">Tento příklad vytvoří virtuální počítač v existující skupinu prostředků a virtuální síť už nasazená.</span><span class="sxs-lookup"><span data-stu-id="af2b3-131">This example creates a VM in an existing resource group and with a virtual network already deployed.</span></span> <span data-ttu-id="af2b3-132">Podrobnější příklad o tom, jak použít Ansible k vytvoření doprovodné materiály, jako je například virtuální sítě a pravidel skupin zabezpečení sítě najdete v tématu [vytvořit úplný prostředí virtuálních počítačů s Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="af2b3-132">For a more detailed example on how to use Ansible to create supporting resources such as a virtual network and Network Security Group rules, see [Create a complete VM environment with Ansible](ansible-create-complete-vm.md).</span></span>