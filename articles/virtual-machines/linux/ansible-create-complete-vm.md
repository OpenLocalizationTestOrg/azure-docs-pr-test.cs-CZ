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
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Vytvořte dokončení prostředí Linux virtuálního počítače v Azure s Ansible
Ansible umožňuje tooautomate hello nasazení a konfigurace prostředků ve vašem prostředí. Můžete použít Ansible toomanage virtuálních počítačů (VM) ve službě Azure, hello stejné jako jiný prostředek. Tento článek ukazuje, jak toocreate dokončení prostředí Linux a podpůrné prostředky s Ansible. Můžete si také přečíst jak příliš[vytvořit základní virtuální počítač s Ansible](ansible-create-vm.md).


## <a name="prerequisites"></a>Požadavky
toomanage Azure prostředky s Ansible, je nutné hello následující:

- Ansible a hello moduly Azure Python SDK v systému hostitele.
    - Nainstalujte Ansible [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), a [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)
- Přihlašovací údaje Azure a Ansible nakonfigurované toouse je.
    - [Vytvořit přihlašovací údaje Azure a nakonfigurovat Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI verze verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. 
    - Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). Můžete také použít [cloudové prostředí](/azure/cloud-shell/quickstart) z prohlížeče.


## <a name="create-virtual-network"></a>Vytvoření virtuální sítě
Hello následující části v playbook Ansible vytvoří virtuální síť s názvem *myVnet* v hello *10.0.0.0/16* adresní prostor:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

tooadd podsíť, následující části hello vytvoří podsíť s názvem *mySubnet* v hello *myVnet* virtuální sítě:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Vytvoření veřejné IP adresy
tooaccess prostředkům v rámci hello Internetu, vytvořte a přiřaďte tooyour veřejnou adresu IP virtuálního počítače. Hello následující části v playbook Ansible vytvoří veřejnou IP adresu s názvem *myPublicIP*:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Vytvořit skupinu zabezpečení sítě
Skupiny zabezpečení sítě řídit tok hello síťový provoz do/z virtuálního počítače. Hello následující části v playbook Ansible vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* a definuje přenosem pravidlo tooallow SSH na TCP port 22:

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


## <a name="create-virtual-network-interface-card"></a>Vytvořit virtuální síťová karta
Virtuální síťová karta (NIC) připojí vaše tooa virtuálních počítačů daného virtuální sítě, veřejnou IP adresu a skupinu zabezpečení sítě. Hello následující části v playbook Ansible vytvoří virtuální síťový adaptér s názvem *myNIC* připojení virtuálních síťových prostředků toohello jste vytvořili:

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


## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
poslední krok Hello je toocreate virtuálního počítače a použít všechny prostředky hello vytvořené. Hello následující části v playbook Ansible vytvoří virtuální počítač s názvem *Můjvp* a připojí hello virtuální síťový adaptér s názvem *myNIC*. Zadejte svoje vlastní data veřejného klíče v hello *key_data* spárujte následujícím způsobem:

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

## <a name="complete-ansible-playbook"></a>Dokončení Ansible playbook
toobring tyto části společně, vytvořit Ansible playbook s názvem *azure_create_complete_vm.yml* a hello vložte následující obsah:

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

Ansible musí všechny prostředky do toodeploy skupiny prostředků. Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/vm#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

toocreate hello dokončení VM prostředí s Ansible, spusťte hello playbook následujícím způsobem:

```bash
ansible-playbook azure_create_complete_vm.yml
```

výstup Hello vypadá podobně jako toohello následující příklad, který zobrazuje hello že virtuálního počítače byla úspěšně vytvořena:

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

## <a name="next-steps"></a>Další kroky
Tento příklad vytvoří kompletní prostředí virtuálních počítačů včetně hello požadované virtuálních síťových prostředků. Více přímé příklad toocreate virtuálního počítače do stávající síťové prostředky s výchozími možnostmi najdete v části [vytvoření virtuálního počítače](ansible-create-vm.md).
