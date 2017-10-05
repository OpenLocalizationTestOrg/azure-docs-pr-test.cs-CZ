---
title: "Různé způsoby vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Další informace o různých způsobech vytvoření virtuálního počítače s Linuxem na platformě Azure, včetně odkazů na nástroje a kurzy pro jednotlivé metody."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 1eb90d44797d66f3e09811918ce5a7f4ad4287c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Různé způsoby vytvoření virtuálního počítače s Linuxem v Azure
V Azure máte flexibilitu vytvoření virtuálního počítače s Linuxem pomocí nástrojů a pracovních postupů, které vám vyhovují. Tento článek shrnuje tyto rozdíly a příklady vytvoření virtuálních počítačů s Linuxem.

## <a name="azure-cli"></a>Azure CLI
Virtuální počítače můžete v Azure vytvářet pomocí některé z následujících verzí rozhraní příkazového řádku:

- Azure CLI 1.0 – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků (tento článek)
- [Azure CLI 2.0](../windows/creation-choices.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

Rozhraní Azure CLI 1.0 je k dispozici napříč platformami jako balíček npm, balíček distribuce nebo kontejner Docker. Můžete si přečíst další informace o tom, [jak nainstalovat a nakonfigurovat Azure CLI](../../cli-install-nodejs.md). V následujících kurzech najdete příklady použití Azure CLI 1.0. V každém článku si přečtěte další podrobnosti o zobrazených příkazech rychlého startu CLI:

* [Vytvoření virtuálního počítače s Linuxem z Azure CLI pro vývoj a testování](quick-create-cli-nodejs.md)
  
  * Následující příklad vytvoří pomocí veřejného klíče s názvem počítače s CoreOS *azure_id_rsa.pub*:
    
    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md)
  
  * Následující příklad vytvoří virtuální počítač pomocí šablony uložené na GitHubu:
    
    ```azurecli
    azure group create --name myResourceGroup --location eastus 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [Vytvoření kompletního linuxového prostředí pomocí Azure CLI](create-cli-complete-nodejs.md)
  
  * Zahrnuje vytvoření nástroje pro vyrovnávání zatížení a více virtuálních počítačů ve skupině dostupnosti.
* [Přidání disku do virtuálního počítače s Linuxem](add-disk.md)
  
  * Následující příklad přidá *5* GB místa na disku na existující virtuální počítač s názvem *Můjvp*:
    
    ```azurecli
    azure vm disk attach-new \
        --resource-group myResourceGroup \
        --vm-name myVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>portál Azure
[Azure Portal](https://portal.azure.com) umožňuje rychle vytvořit virtuální počítač, protože není nutné nic instalovat na váš systém. Vytvoření virtuálního počítače pomocí portálu Azure Portal:

* [Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal](quick-create-portal.md) 

## <a name="operating-system-and-image-choices"></a>Operační systém a volba image
Při vytváření virtuálního počítače zvolíte image podle operačního systému, který chcete používat. Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje. Nebo nahrajte některou z vlastních imagí (viz [následující oddíl](#use-your-own-image)).

### <a name="azure-images"></a>Image dostupné v Azure
Pomocí příkazů rozhraní příkazového řádku `azure vm image` zobrazíte dostupné image podle vydavatele, vydání distribuce a sestavení.

Seznam dostupných vydavatelů zobrazíte takto:

```azurecli
azure vm image list-publishers --location eastus
```

Seznam dostupných produktů (nabídek) pro daného vydavatele zobrazíte takto:

```azurecli
azure vm image list-offers --location eastus --publisher Canonical
```

Seznam dostupných SKU (vydání distribuce) pro danou nabídku zobrazíte takto:

```azurecli
azure vm image list-skus --location eastus --publisher Canonical --offer UbuntuServer
```

Seznam všech dostupných imagí pro dané vydání zobrazíte takto:

```azurecli
azure vm image list --location eastus --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Příkazy `azure vm quick-create` a `azure vm create` mají aliasy, které můžete použít pro rychlý přístup k častějším distribucím a jejich nejnovějším vydáním. Použití aliasů je často rychlejší, než zadávání vydavatele, nabídky, SKU a verze při každém vytváření virtuálního počítače:

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
Pokud budete potřebovat image se zvláštními úpravami, můžete *zachytit* stávající virtuální počítač a použít image založenou na tomto virtuálním počítači. Můžete také nahrát místně vytvořenou image. Další informace o podporovaných distribucích a o tom, jak používat vlastní image, najdete v následujících článcích:

* [Distribuce schválené pro Azure](endorsed-distros.md)
* [Informace pro neschválené distribuce](create-upload-generic.md)
* [Nahrání a vytvoření virtuálního počítače s Linuxem z vlastní image disku](upload-vhd.md)
* [Jak zachytit virtuální počítač s Linuxem do šablony Resource Manageru](capture-image.md)
  
  * Příkazy rychlého startu pro zachycení stávajícího virtuálního počítače:
    
    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Další kroky
* Vytvořte virtuální počítač s Linuxem z webu [Azure Portal](quick-create-portal.md), přes [rozhraní příkazového řádku](quick-create-cli.md) nebo pomocí [šablony Azure Resource Manageru](../windows/cli-deploy-templates.md).
* Po vytvoření virtuálního počítače s Linuxem můžete [přidat datový disk](add-disk.md).
* Rychlé kroky pro [resetování hesla nebo klíčů SSH a správu uživatelů](using-vmaccess-extension.md)

