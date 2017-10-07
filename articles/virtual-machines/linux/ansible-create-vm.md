---
title: "aaaUse Ansible toocreate základní virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Ansible toocreate a spravovat základní virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: ffe278b3f846924ff9c4d026120565146f951152
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Vytvoření základního virtuálního počítače v Azure pomocí Ansible
Ansible umožňuje tooautomate hello nasazení a konfigurace prostředků ve vašem prostředí. Můžete použít Ansible toomanage virtuálních počítačů (VM) ve službě Azure, hello stejné jako jiný prostředek. Tento článek ukazuje, jak toocreate základní virtuální počítač s Ansible. Můžete si také přečíst jak příliš[vytvořit úplný prostředí virtuálních počítačů s Ansible](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Požadavky
toomanage Azure prostředky s Ansible, je nutné hello následující:

- Ansible a hello moduly Azure Python SDK v systému hostitele.
    - Nainstalujte Ansible [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), a [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)
- Přihlašovací údaje Azure a Ansible nakonfigurované toouse je.
    - [Vytvořit přihlašovací údaje Azure a nakonfigurovat Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI verze verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. 
    - Pokud potřebujete tooupgrade, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). Můžete také použít [cloudové prostředí](/azure/cloud-shell/quickstart) z prohlížeče.


## <a name="create-supporting-azure-resources"></a>Vytvoření Podpora prostředků Azure
V tomto příkladu vytvoříme sadu runbook, která nasadí virtuální počítač do existující infrastruktury. Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/vm#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvoření virtuální sítě pro virtuální počítač s [vytvoření sítě vnet az](/cli/azure/network/vnet#create). Hello následující příklad vytvoří virtuální síť s názvem *myVnet* a podsíť s názvem *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Vytvoření a spuštění Ansible playbook
Vytvoření Ansible playbook s názvem **azure_create_vm.yml** a hello vložte následující obsah. Tento příklad vytvoří jeden virtuální počítač a nakonfiguruje pověření SSH. Zadejte svoje vlastní data veřejného klíče v hello *key_data* spárujte následujícím způsobem:

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

toocreate hello virtuálního počítače s Ansible, spusťte hello playbook následujícím způsobem:

```bash
ansible-playbook azure_create_vm.yml
```

výstup Hello vypadá podobně jako toohello následující příklad, který zobrazuje hello že virtuálního počítače byla úspěšně vytvořena:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a>Další kroky
Tento příklad vytvoří virtuální počítač v existující skupinu prostředků a virtuální síť už nasazená. Podrobnější příklad, jak toouse Ansible toocreate podpůrné prostředkům, například virtuální sítě a pravidel skupin zabezpečení sítě najdete v části [vytvořit úplný prostředí virtuálních počítačů s Ansible](ansible-create-complete-vm.md).
