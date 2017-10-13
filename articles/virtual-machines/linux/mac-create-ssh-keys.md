---
title: "Vytvoření a použití páru klíčů SSH pro virtuální počítače s Linuxem v Azure | Dokumentace Microsoftu"
description: "Jak vytvořit a použít pár veřejného a privátního klíče SSH pro virtuální počítače s Linuxem v Azure k vylepšení zabezpečení procesu ověřování."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: iainfou
ms.openlocfilehash: 6fcdcc96c7762e2362aebf909ef25f4a5ab62f99
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Vytvoření a použití páru veřejného a privátního klíče SSH pro virtuální počítače s Linuxem v Azure
Pomocí páru klíčů SSH (Secure Shell) můžete v Azure vytvořit virtuální počítače, které k ověřování používají klíče SSH. Není potom potřeba používat k přihlašování hesla. Tento článek ukazuje, jak rychle vygenerovat a použít pár souborů veřejného a privátního klíče protokolu SSH verze 2 RSA pro virtuální počítače s Linuxem. Podrobný postup a další příklady najdete v tématu, které se věnuje [podrobným postupům vytvoření párů klíčů SSH a certifikátů](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>Vytvoření páru klíčů SSH
Pomocí příkazu `ssh-keygen` vytvořte soubory veřejného a privátního klíče SSH, které se automaticky vytvoří v adresáři `~/.ssh`, ale na vyzvání můžete zadat jiné umístění a dodatečné přístupové heslo (heslo pro přístup k souboru privátního klíče). Spusťte následující příkaz z prostředí Bash a na vyžádání zadejte vlastní údaje.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-the-ssh-key-pair"></a>Použití páru klíčů SSH
Veřejný klíč, který umístíte na virtuální počítač s Linuxem v Azure, se ve výchozím nastavení uloží do souboru `~/.ssh/id_rsa.pub`, pokud umístění při vytváření nezměníte. Pokud k vytvoření virtuálního počítače použijete [Azure CLI 2.0](/cli/azure), umístění veřejného klíče můžete zadat při použití příkazu [az vm create](/cli/azure/vm#create) pomocí možnosti `--ssh-key-path`. Pokud obsah souboru veřejného klíče kopírujete a vkládáte za účelem použití na webu Azure Portal nebo v šabloně Resource Manageru, ujistěte se, že nekopírujete žádné prázdné znaky. Pokud například používáte systém OS X, můžete soubor veřejného klíče (ve výchozím nastavení **~/.ssh/id_rsa.pub**) předat příkazu **pbcopy**, který jeho obsah zkopíruje (existují i další linuxové programy, které dělají to samé, například `xclip`).

Pokud s veřejnými klíči SSH teprve začínáte, můžete svůj veřejný klíč zobrazit spuštěním příkazu `cat`, jak je uvedeno níže, a nahrazením hodnoty `~/.ssh/id_rsa.pub` za umístění vašeho souboru veřejného klíče:

```bash
cat ~/.ssh/id_rsa.pub
```

Jakmile budete mít veřejný klíč na virtuálním počítači Azure, připojte se k virtuálnímu počítači pomocí SSH za použití jeho IP adresy nebo názvu DNS (nezapomeňte nahradit níže uvedené hodnoty `azureuser` a `myvm.westus.cloudapp.azure.com` uživatelským jménem správce a plně kvalifikovaným názvem domény nebo IP adresou):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Pokud jste při vytváření páru klíčů zadali přístupové heslo, zadejte jej na vyzvání během procesu přihlašování. (Server se přidá do vaší složky `~/.ssh/known_hosts` a nové připojení se nebude vyžadovat, dokud se nezmění veřejný klíč na virtuálním počítači Azure nebo se neodebere název serveru ze složky `~/.ssh/known_hosts`.)

## <a name="next-steps"></a>Další kroky

Virtuální počítače Azure vytvořené pomocí páru klíčů SSH jsou ve výchozím nastavení nakonfigurované se zakázaným heslem, aby byly pokusy o rozluštění hesla útokem hrubou silou výrazně nákladnější a tedy obtížnější. Toto téma popisuje vytvoření jednoduchého páru klíčů SSH pro rychlé použití. Pokud potřebujete další pomoc s vytvořením páru klíčů SSH nebo potřebujete další certifikáty, přečtěte si téma [Podrobný postup vytvoření párů klíčů SSH a certifikátů](create-ssh-keys-detailed.md).

Virtuální počítače používající pár klíčů SSH můžete vytvořit pomocí webu Azure Portal, rozhraní Azure CLI a šablon Azure:

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí webu Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
