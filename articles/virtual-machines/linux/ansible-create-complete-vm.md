---
title: "K vytvoření kompletní virtuální počítač s Linuxem v Azure použijte Ansible | Microsoft Docs"
description: "Další informace o použití Ansible k vytváření a správě dokončení prostředí Linux virtuálního počítače v Azure"
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
ms.openlocfilehash: b2fcc288b40c12a9b3f966156ee2eedf4acca313
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a><span data-ttu-id="150ba-103">Vytvořte dokončení prostředí Linux virtuálního počítače v Azure s Ansible</span><span class="sxs-lookup"><span data-stu-id="150ba-103">Create a complete Linux virtual machine environment in Azure with Ansible</span></span>
<span data-ttu-id="150ba-104">Ansible umožňuje automatizovat nasazení a konfigurace prostředků ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="150ba-104">Ansible allows you to automate the deployment and configuration of resources in your environment.</span></span> <span data-ttu-id="150ba-105">Ansible můžete použít ke správě virtuálních počítačů (VM) v Azure, stejně jako jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="150ba-105">You can use Ansible to manage your virtual machines (VMs) in Azure, the same as you would any other resource.</span></span> <span data-ttu-id="150ba-106">Tento článek ukazuje, jak vytvořit úplný prostředí Linux a podpůrné prostředky s Ansible.</span><span class="sxs-lookup"><span data-stu-id="150ba-106">This article shows you how to create a complete Linux environment and supporting resources with Ansible.</span></span> <span data-ttu-id="150ba-107">Můžete si také přečíst postup [vytvořit základní virtuální počítač s Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="150ba-107">You can also learn how to [Create a basic VM with Ansible](ansible-create-vm.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="150ba-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="150ba-108">Prerequisites</span></span>
<span data-ttu-id="150ba-109">Ke správě prostředků Azure s Ansible, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="150ba-109">To manage Azure resources with Ansible, you need the following:</span></span>

- <span data-ttu-id="150ba-110">Ansible a moduly Azure Python SDK v systému hostitele.</span><span class="sxs-lookup"><span data-stu-id="150ba-110">Ansible and the Azure Python SDK modules installed on your host system.</span></span>
    - <span data-ttu-id="150ba-111">Nainstalujte Ansible [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), a [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span><span class="sxs-lookup"><span data-stu-id="150ba-111">Install Ansible on [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), and [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span></span>
- <span data-ttu-id="150ba-112">Přihlašovací údaje Azure a Ansible nakonfigurovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="150ba-112">Azure credentials, and Ansible configured to use them.</span></span>
    - [<span data-ttu-id="150ba-113">Vytvořit přihlašovací údaje Azure a nakonfigurovat Ansible</span><span class="sxs-lookup"><span data-stu-id="150ba-113">Create Azure credentials and configure Ansible</span></span>](ansible-install-configure.md#create-azure-credentials)
- <span data-ttu-id="150ba-114">Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="150ba-114">Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="150ba-115">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="150ba-115">Run `az --version` to find the version.</span></span> 
    - <span data-ttu-id="150ba-116">Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="150ba-116">If you need to upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> <span data-ttu-id="150ba-117">Můžete také použít [cloudové prostředí](/azure/cloud-shell/quickstart) z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="150ba-117">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>


## <a name="create-virtual-network"></a><span data-ttu-id="150ba-118">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="150ba-118">Create virtual network</span></span>
<span data-ttu-id="150ba-119">V následující části v playbook Ansible vytvoří virtuální síť s názvem *myVnet* v *10.0.0.0/16* adresní prostor:</span><span class="sxs-lookup"><span data-stu-id="150ba-119">The following section in an Ansible playbook creates a virtual network named *myVnet* in the *10.0.0.0/16* address space:</span></span>

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

<span data-ttu-id="150ba-120">Chcete-li přidat podsíť, v následující části vytvoří podsíť s názvem *mySubnet* v *myVnet* virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="150ba-120">To add a subnet, the following section creates a subnet named *mySubnet* in the *myVnet* virtual network:</span></span>

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a><span data-ttu-id="150ba-121">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="150ba-121">Create public IP address</span></span>
<span data-ttu-id="150ba-122">Pro přístup k prostředkům přes Internet, vytvořte a přiřaďte veřejnou IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="150ba-122">To access resources across the Internet, create and assign a public IP address to your VM.</span></span> <span data-ttu-id="150ba-123">V následující části v playbook Ansible vytvoří veřejnou IP adresu s názvem *myPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="150ba-123">The following section in an Ansible playbook creates a public IP address named *myPublicIP*:</span></span>

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a><span data-ttu-id="150ba-124">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="150ba-124">Create Network Security Group</span></span>
<span data-ttu-id="150ba-125">Skupiny zabezpečení sítě řídí tok síťový provoz do/z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="150ba-125">Network Security Groups control the flow of network traffic in and out of your VM.</span></span> <span data-ttu-id="150ba-126">V následující části v playbook Ansible vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* a definuje pravidlo umožňující přenos SSH na TCP port 22:</span><span class="sxs-lookup"><span data-stu-id="150ba-126">The following section in an Ansible playbook creates a network security group named *myNetworkSecurityGroup* and defines a rule to allow SSH traffic on TCP port 22:</span></span>

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: TCP
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-virtual-network-interface-card"></a><span data-ttu-id="150ba-127">Vytvořit virtuální síťová karta</span><span class="sxs-lookup"><span data-stu-id="150ba-127">Create virtual network interface card</span></span>
<span data-ttu-id="150ba-128">Virtuální síťová karta (NIC) připojí k dané virtuální síti, veřejnou IP adresu a skupinu zabezpečení sítě virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="150ba-128">A virtual network interface card (NIC) connects your VM to a given virtual network, public IP address, and network security group.</span></span> <span data-ttu-id="150ba-129">V následující části v playbook Ansible vytvoří virtuální síťový adaptér s názvem *myNIC* připojené k virtuální síťové prostředky, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="150ba-129">The following section in an Ansible playbook creates a virtual NIC named *myNIC* connected to the virtual networking resources you have created:</span></span>

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```


## <a name="create-virtual-machine"></a><span data-ttu-id="150ba-130">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="150ba-130">Create virtual machine</span></span>
<span data-ttu-id="150ba-131">Posledním krokem je vytvoření virtuálního počítače a použít všechny prostředky, které jsou vytvořené.</span><span class="sxs-lookup"><span data-stu-id="150ba-131">The final step is to create a VM and use all the resources created.</span></span> <span data-ttu-id="150ba-132">V následující části v playbook Ansible vytvoří virtuální počítač s názvem *Můjvp* a připojí virtuální síťový adaptér s názvem *myNIC*.</span><span class="sxs-lookup"><span data-stu-id="150ba-132">The following section in an Ansible playbook creates a VM named *myVM* and attaches the virtual NIC named *myNIC*.</span></span> <span data-ttu-id="150ba-133">Zadejte svoje vlastní veřejného klíče data v *key_data* spárujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="150ba-133">Enter your own public key data in the *key_data* pair as follows:</span></span>

```yaml
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
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.3'
      version: latest
```

## <a name="complete-ansible-playbook"></a><span data-ttu-id="150ba-134">Dokončení Ansible playbook</span><span class="sxs-lookup"><span data-stu-id="150ba-134">Complete Ansible playbook</span></span>
<span data-ttu-id="150ba-135">Tyto části sdružujícího vytvořit Ansible playbook s názvem *azure_create_complete_vm.yml* a vložte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="150ba-135">To bring all these sections together, create an Ansible playbook named *azure_create_complete_vm.yml* and paste the following contents:</span></span>

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
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
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

<span data-ttu-id="150ba-136">Ansible musí nasadit všechny prostředky do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="150ba-136">Ansible needs a resource group to deploy all your resources into.</span></span> <span data-ttu-id="150ba-137">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="150ba-137">Create a resource group with [az group create](/cli/azure/vm#create).</span></span> <span data-ttu-id="150ba-138">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="150ba-138">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="150ba-139">Pokud chcete vytvořit úplný prostředí virtuálních počítačů s Ansible, spusťte playbook následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="150ba-139">To create the complete VM environment with Ansible, run the playbook as follows:</span></span>

```bash
ansible-playbook azure_create_complete_vm.yml
```

<span data-ttu-id="150ba-140">Výstup bude vypadat podobně jako v následujícím příkladu, který ukazuje, že virtuální počítač se úspěšně vytvořil:</span><span class="sxs-lookup"><span data-stu-id="150ba-140">The output looks similar to the following example that shows the VM has been successfully created:</span></span>

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create virtual network] *********************************************
changed: [localhost]

TASK [Add subnet] *********************************************************
changed: [localhost]

TASK [Create public IP address] *******************************************
changed: [localhost]

TASK [Create Network Security Group that allows SSH] **********************
changed: [localhost]

TASK [Create virtual network inteface card] *******************************
changed: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=7    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a><span data-ttu-id="150ba-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="150ba-141">Next steps</span></span>
<span data-ttu-id="150ba-142">Tento příklad vytvoří kompletní prostředí virtuálních počítačů, včetně požadované prostředky virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="150ba-142">This example creates a complete VM environment including the required virtual networking resources.</span></span> <span data-ttu-id="150ba-143">Více přímé příklad k vytvoření virtuálního počítače do stávající síťové prostředky s výchozími možnostmi najdete v tématu [vytvoření virtuálního počítače](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="150ba-143">For a more direct example to create a VM into existing network resources with default options, see [Create a VM](ansible-create-vm.md).</span></span>