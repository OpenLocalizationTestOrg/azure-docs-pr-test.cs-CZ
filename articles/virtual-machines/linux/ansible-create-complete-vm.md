---
title: "aaaUse Ansible toocreate dokončení virtuálního počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Ansible toocreate a spravovat dokončení prostředí Linux virtuálního počítače v Azure"
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
ms.openlocfilehash: 970b0427f39fc23240f9faab868196ca4f444e0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a><span data-ttu-id="ce635-103">Vytvořte dokončení prostředí Linux virtuálního počítače v Azure s Ansible</span><span class="sxs-lookup"><span data-stu-id="ce635-103">Create a complete Linux virtual machine environment in Azure with Ansible</span></span>
<span data-ttu-id="ce635-104">Ansible umožňuje tooautomate hello nasazení a konfigurace prostředků ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce635-104">Ansible allows you tooautomate hello deployment and configuration of resources in your environment.</span></span> <span data-ttu-id="ce635-105">Můžete použít Ansible toomanage virtuálních počítačů (VM) ve službě Azure, hello stejné jako jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="ce635-105">You can use Ansible toomanage your virtual machines (VMs) in Azure, hello same as you would any other resource.</span></span> <span data-ttu-id="ce635-106">Tento článek ukazuje, jak toocreate dokončení prostředí Linux a podpůrné prostředky s Ansible.</span><span class="sxs-lookup"><span data-stu-id="ce635-106">This article shows you how toocreate a complete Linux environment and supporting resources with Ansible.</span></span> <span data-ttu-id="ce635-107">Můžete si také přečíst jak příliš[vytvořit základní virtuální počítač s Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="ce635-107">You can also learn how too[Create a basic VM with Ansible](ansible-create-vm.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ce635-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce635-108">Prerequisites</span></span>
<span data-ttu-id="ce635-109">toomanage Azure prostředky s Ansible, je nutné hello následující:</span><span class="sxs-lookup"><span data-stu-id="ce635-109">toomanage Azure resources with Ansible, you need hello following:</span></span>

- <span data-ttu-id="ce635-110">Ansible a hello moduly Azure Python SDK v systému hostitele.</span><span class="sxs-lookup"><span data-stu-id="ce635-110">Ansible and hello Azure Python SDK modules installed on your host system.</span></span>
    - <span data-ttu-id="ce635-111">Nainstalujte Ansible [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), a [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span><span class="sxs-lookup"><span data-stu-id="ce635-111">Install Ansible on [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), and [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span></span>
- <span data-ttu-id="ce635-112">Přihlašovací údaje Azure a Ansible nakonfigurované toouse je.</span><span class="sxs-lookup"><span data-stu-id="ce635-112">Azure credentials, and Ansible configured toouse them.</span></span>
    - [<span data-ttu-id="ce635-113">Vytvořit přihlašovací údaje Azure a nakonfigurovat Ansible</span><span class="sxs-lookup"><span data-stu-id="ce635-113">Create Azure credentials and configure Ansible</span></span>](ansible-install-configure.md#create-azure-credentials)
- <span data-ttu-id="ce635-114">Azure CLI verze verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ce635-114">Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ce635-115">Spustit `az --version` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="ce635-115">Run `az --version` toofind hello version.</span></span> 
    - <span data-ttu-id="ce635-116">Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ce635-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> <span data-ttu-id="ce635-117">Můžete také použít [cloudové prostředí](/azure/cloud-shell/quickstart) z prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ce635-117">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>


## <a name="create-virtual-network"></a><span data-ttu-id="ce635-118">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="ce635-118">Create virtual network</span></span>
<span data-ttu-id="ce635-119">Hello následující části v playbook Ansible vytvoří virtuální síť s názvem *myVnet* v hello *10.0.0.0/16* adresní prostor:</span><span class="sxs-lookup"><span data-stu-id="ce635-119">hello following section in an Ansible playbook creates a virtual network named *myVnet* in hello *10.0.0.0/16* address space:</span></span>

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

<span data-ttu-id="ce635-120">tooadd podsíť, následující části hello vytvoří podsíť s názvem *mySubnet* v hello *myVnet* virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="ce635-120">tooadd a subnet, hello following section creates a subnet named *mySubnet* in hello *myVnet* virtual network:</span></span>

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a><span data-ttu-id="ce635-121">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="ce635-121">Create public IP address</span></span>
<span data-ttu-id="ce635-122">tooaccess prostředkům v rámci hello Internetu, vytvořte a přiřaďte tooyour veřejnou adresu IP virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ce635-122">tooaccess resources across hello Internet, create and assign a public IP address tooyour VM.</span></span> <span data-ttu-id="ce635-123">Hello následující části v playbook Ansible vytvoří veřejnou IP adresu s názvem *myPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="ce635-123">hello following section in an Ansible playbook creates a public IP address named *myPublicIP*:</span></span>

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a><span data-ttu-id="ce635-124">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="ce635-124">Create Network Security Group</span></span>
<span data-ttu-id="ce635-125">Skupiny zabezpečení sítě řídit tok hello síťový provoz do/z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ce635-125">Network Security Groups control hello flow of network traffic in and out of your VM.</span></span> <span data-ttu-id="ce635-126">Hello následující části v playbook Ansible vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* a definuje přenosem pravidlo tooallow SSH na TCP port 22:</span><span class="sxs-lookup"><span data-stu-id="ce635-126">hello following section in an Ansible playbook creates a network security group named *myNetworkSecurityGroup* and defines a rule tooallow SSH traffic on TCP port 22:</span></span>

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


## <a name="create-virtual-network-interface-card"></a><span data-ttu-id="ce635-127">Vytvořit virtuální síťová karta</span><span class="sxs-lookup"><span data-stu-id="ce635-127">Create virtual network interface card</span></span>
<span data-ttu-id="ce635-128">Virtuální síťová karta (NIC) připojí vaše tooa virtuálních počítačů daného virtuální sítě, veřejnou IP adresu a skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="ce635-128">A virtual network interface card (NIC) connects your VM tooa given virtual network, public IP address, and network security group.</span></span> <span data-ttu-id="ce635-129">Hello následující části v playbook Ansible vytvoří virtuální síťový adaptér s názvem *myNIC* připojení virtuálních síťových prostředků toohello jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="ce635-129">hello following section in an Ansible playbook creates a virtual NIC named *myNIC* connected toohello virtual networking resources you have created:</span></span>

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


## <a name="create-virtual-machine"></a><span data-ttu-id="ce635-130">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="ce635-130">Create virtual machine</span></span>
<span data-ttu-id="ce635-131">poslední krok Hello je toocreate virtuálního počítače a použít všechny prostředky hello vytvořené.</span><span class="sxs-lookup"><span data-stu-id="ce635-131">hello final step is toocreate a VM and use all hello resources created.</span></span> <span data-ttu-id="ce635-132">Hello následující části v playbook Ansible vytvoří virtuální počítač s názvem *Můjvp* a připojí hello virtuální síťový adaptér s názvem *myNIC*.</span><span class="sxs-lookup"><span data-stu-id="ce635-132">hello following section in an Ansible playbook creates a VM named *myVM* and attaches hello virtual NIC named *myNIC*.</span></span> <span data-ttu-id="ce635-133">Zadejte svoje vlastní data veřejného klíče v hello *key_data* spárujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ce635-133">Enter your own public key data in hello *key_data* pair as follows:</span></span>

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

## <a name="complete-ansible-playbook"></a><span data-ttu-id="ce635-134">Dokončení Ansible playbook</span><span class="sxs-lookup"><span data-stu-id="ce635-134">Complete Ansible playbook</span></span>
<span data-ttu-id="ce635-135">toobring tyto části společně, vytvořit Ansible playbook s názvem *azure_create_complete_vm.yml* a hello vložte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="ce635-135">toobring all these sections together, create an Ansible playbook named *azure_create_complete_vm.yml* and paste hello following contents:</span></span>

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

<span data-ttu-id="ce635-136">Ansible musí všechny prostředky do toodeploy skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="ce635-136">Ansible needs a resource group toodeploy all your resources into.</span></span> <span data-ttu-id="ce635-137">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="ce635-137">Create a resource group with [az group create](/cli/azure/vm#create).</span></span> <span data-ttu-id="ce635-138">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="ce635-138">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="ce635-139">toocreate hello dokončení VM prostředí s Ansible, spusťte hello playbook následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ce635-139">toocreate hello complete VM environment with Ansible, run hello playbook as follows:</span></span>

```bash
ansible-playbook azure_create_complete_vm.yml
```

<span data-ttu-id="ce635-140">výstup Hello vypadá podobně jako toohello následující příklad, který zobrazuje hello že virtuálního počítače byla úspěšně vytvořena:</span><span class="sxs-lookup"><span data-stu-id="ce635-140">hello output looks similar toohello following example that shows hello VM has been successfully created:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ce635-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce635-141">Next steps</span></span>
<span data-ttu-id="ce635-142">Tento příklad vytvoří kompletní prostředí virtuálních počítačů včetně hello požadované virtuálních síťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="ce635-142">This example creates a complete VM environment including hello required virtual networking resources.</span></span> <span data-ttu-id="ce635-143">Více přímé příklad toocreate virtuálního počítače do stávající síťové prostředky s výchozími možnostmi najdete v části [vytvoření virtuálního počítače](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="ce635-143">For a more direct example toocreate a VM into existing network resources with default options, see [Create a VM](ansible-create-vm.md).</span></span>
