---
title: "aaaDifferent způsoby toocreate virtuálního počítače s Linuxem v Azure | Microsoft Azure"
description: "Přečtěte si hello různé způsoby toocreate virtuální počítač s Linuxem v Azure, včetně tootools odkazy a kurzy pro každou metodu."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 250e92c063c87a8c1279097dc2264777d95478d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-linux-vm"></a>Různé způsoby toocreate virtuálního počítače s Linuxem
Budete mít hello flexibilitu v Azure toocreate Linux virtuálního počítače (VM) pomocí nástroje a pracovní postupy možnost tooyou. Tento článek obsahuje souhrn tyto rozdíly a příklady pro vytváření virtuálních počítačů Linux, včetně hello 2.0 rozhraní příkazového řádku Azure. Můžete také zobrazit možnosti vytvoření včetně hello [Azure CLI 1.0](creation-choices-nodejs.md).

Hello [Azure CLI 2.0](/cli/azure/install-az-cli2) napříč platformami pomocí balíčku npm, zadaný distro balíčky nebo kontejner Docker je k dispozici. Nainstalujte hello sestavení nejvhodnější pro vaše prostředí a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login)

* [Vytvoření virtuálního počítače s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure](quick-create-cli.md)
  
  * Pomocí příkazu [az group create](/cli/azure/group#create) vytvořte skupinu prostředků *myResourceGroup*: 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) s názvem *Můjvp* pomocí nejnovější hello *UbuntuLTS* bitovou kopii a generovat klíče SSH, pokud už neexistují v *~/.ssh*:

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md)
  
  * Hello následující příklad používá [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) toocreate virtuálního počítače ze šablony uložené na Githubu:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [Vytvoření a přizpůsobení virtuálního počítače s Linuxem pomocí cloud-init](tutorial-automate-vm-deployment.md)

* [Vytvoření vysoce dostupné aplikace s vyrovnáváním zatížení na více virtuálních počítačích s Linuxem](tutorial-load-balancer.md)


## <a name="azure-portal"></a>portál Azure
Hello [portál Azure](https://portal.azure.com) vám umožní tooquickly vytvoření virtuálního počítače, protože není nic tooinstall ve vašem systému. Pomocí portálu Azure toocreate hello hello virtuálních počítačů:

* [Vytvoření virtuálního počítače s Linuxem pomocí portálu Azure hello](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a>Operační systém a volba image
Při vytváření virtuálního počítače, zvolíte image podle operačního systému, které chcete toorun hello. Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje. Nebo nahrát některou ze svých vlastních imagí (viz [hello následující části](#use-your-own-image)).

### <a name="azure-images"></a>Image dostupné v Azure
Použití hello [image virtuálního počítače az](/cli/azure/vm/image) toosee příkazy, které jsou dostupné vydavatele, verzi distro a sestavení.

Seznam dostupných vydavatelů:

```azurecli
az vm image list-publishers --location eastus
```

Seznam dostupných produktů (nabídek) pro daného vydavatele:

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

Seznam dostupných SKU (vydání distribuce) pro danou nabídku:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

Seznam všech dostupných imagí pro dané vydání:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

Další příklady vyhledávání a použití dostupných imagí najdete v tématu [vyhledání a výběr imagí virtuálních počítačů Azure pomocí rozhraní příkazového řádku Azure hello](cli-ps-findimage.md).

Hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz má aliasy, můžete přístup tooquickly hello častější distribucích a o jejich nejnovější verze. Použití aliasy je často rychlejší než zadání hello vydavatele, nabídky, SKU a verzi pokaždé, když vytvoření virtuálního počítače:

| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |nejnovější |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |
| Debian |credativ |Debian |8 |nejnovější |
| openSUSE |SUSE |openSUSE |13.2 |nejnovější |
| RHEL |Redhat |RHEL |7.2 |nejnovější |
| SLES |SLES |SLES |12-SP1 |nejnovější |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |nejnovější |

### <a name="use-your-own-image"></a>Použití vlastní image
Pokud budete potřebovat image se zvláštními úpravami, můžete zachytit stávající virtuální počítač a použít image založenou na tomto virtuálním počítači. Můžete také nahrát místně vytvořenou image. Další informace o podporovaných distribucích a jak toouse vlastní Image, najdete v části hello následující články:

* [Distribuce schválené pro Azure](endorsed-distros.md)
* [Informace pro neschválené distribuce](create-upload-generic.md)
* [Jak toocreate bitové kopie ze stávajícího virtuálního počítače Azure](tutorial-custom-images.md).
  
  * Příklad úvodní příkazy toocreate bitové kopie ze stávajícího virtuálního počítače Azure:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a>Další kroky
* Vytvoření virtuálního počítače s Linuxem pomocí hello [rozhraní příkazového řádku](quick-create-cli.md), z hello [portál](quick-create-portal.md), nebo pomocí [šablony Azure Resource Manageru](../windows/cli-deploy-templates.md).
* Po vytvoření virtuálního počítače s Linuxem [si přečtěte o discích a úložišti Azure](tutorial-manage-disks.md).
* Rychlé kroky příliš[resetování hesla nebo klíčů SSH a spravujte uživatele](using-vmaccess-extension.md).
