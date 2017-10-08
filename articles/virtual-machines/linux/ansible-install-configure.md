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
# <a name="install-and-configure-ansible-toomanage-virtual-machines-in-azure"></a>Instalace a konfigurace Ansible toomanage virtuální počítače v Azure
Tento článek podrobně popisuje, jak tooinstall Ansible a požadované moduly Azure Python SDK pro některé hello nejběžnější distribucích systému Linux. Můžete nainstalovat Ansible na jiné distribucích úpravou hello nainstalované balíčky toofit konkrétní platformu. toocreate Azure prostředkům zabezpečené způsobem, můžete si také přečíst jak toocreate a zadejte přihlašovací údaje pro Ansible toouse. 

Další možnosti instalace a kroky pro další platformy najdete v tématu hello [Průvodce instalací Ansible](https://docs.ansible.com/ansible/intro_installation.html).


## <a name="install-ansible"></a>Nainstalujte Ansible
Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAnsible* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

Teď vytvořte virtuální počítač a nainstalujte Ansible pro jednu z následujících distribucích hello:

- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [CentOS 7.3](#centos-73)
- [SLES 12.2 SP2](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS
Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour virtuální počítač pomocí hello `publicIpAddress` si poznamenali v hello operace vytvoření výstup hello virtuálních počítačů:

```bash
ssh azureuser@<publicIpAddress>
```

Na virtuální počítač nainstalujte hello požadované balíčky pro moduly Azure Python SDK hello a Ansible následujícím způsobem:

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

Nyní přesunout příliš[přihlašovací údaje Azure vytvořit](#create-azure-credentials).


### <a name="centos-73"></a>CentOS 7.3
Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour virtuální počítač pomocí hello `publicIpAddress` si poznamenali v hello operace vytvoření výstup hello virtuálních počítačů:

```bash
ssh azureuser@<publicIpAddress>
```

Na virtuální počítač nainstalujte hello požadované balíčky pro moduly Azure Python SDK hello a Ansible následujícím způsobem:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

Nyní přesunout příliš[přihlašovací údaje Azure vytvořit](#create-azure-credentials).


### <a name="sles-122-sp2"></a>SLES 12.2 SP2
Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour virtuální počítač pomocí hello `publicIpAddress` si poznamenali v hello operace vytvoření výstup hello virtuálních počítačů:

```bash
ssh azureuser@<publicIpAddress>
```

Na virtuální počítač nainstalujte hello požadované balíčky pro moduly Azure Python SDK hello a Ansible následujícím způsobem:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

Nyní přesunout příliš[přihlašovací údaje Azure vytvořit](#create-azure-credentials).


## <a name="create-azure-credentials"></a>Vytvořit přihlašovací údaje Azure
Ansible komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby. Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například Ansible. Můžete řídit a definovat hello oprávnění objektu služby hello toowhat operace můžete provádět v rámci Azure. tooimprove zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.

Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstup hello pověření, které potřebuje Ansible:

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

Příklad výstupu hello z předchozích příkazů hello vypadá takto:

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

tooauthenticate tooAzure, musíte taky tooobtain ID vašeho předplatného Azure s [az účet zobrazit](/cli/azure/account#show):

```azurecli
az account show --query [id] --output tsv
```

V dalším kroku hello používáte hello výstup z těchto dvou příkazů.


## <a name="create-ansible-credentials-file"></a>Vytvořte soubor s přihlašovacími údaji Ansible
tooprovide pověření tooAnsible, definujte proměnné prostředí, nebo vytvořit soubor místní přihlašovací údaje. Další informace o tom, najdete v části přihlašovací údaje Ansible toodefine, [poskytování pověření tooAzure moduly](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules). 

Vývojové prostředí, vytvářet *pověření* souboru Ansible na hostiteli virtuálního počítače takto:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Hello *pověření* samotný soubor kombinuje ID předplatného hello výstup hello vytvoření objektu služby. Výstup z předchozí hello [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) příkaz je hello stejné pořadí podle potřeby pro *client_id*, *tajný klíč*, a *klienta* . Následující příklad Hello *pověření* soubor znázorňuje tyto hodnoty odpovídající předchozí výstup hello. Zadejte vlastní hodnoty následujícím způsobem:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a>Použití proměnných prostředí Ansible
Pokud chcete toouse nástroje, například Ansible věž nebo volaných, můžete definovat proměnné prostředí následujícím způsobem. Tyto proměnné kombinovat s výstup hello vytvoření služby hlavní hello ID předplatného. Výstup z předchozí hello [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) příkaz je hello stejné pořadí podle potřeby pro *AZURE_CLIENT_ID*, *AZURE_SECRET*, a *AZURE_ KLIENTA*. 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Další kroky
Nyní máte Ansible a hello požadované moduly Azure Python SDK nainstalovat a přihlašovací údaje definované pro Ansible toouse. Zjistěte, jak příliš[vytvoření virtuálního počítače s Ansible](ansible-create-vm.md). Můžete si také přečíst jak příliš[kompletní virtuální počítače Azure můžete vytvořit a podpůrné prostředky s Ansible](ansible-create-complete-vm.md).
