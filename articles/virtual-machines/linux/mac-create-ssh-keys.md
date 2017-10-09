---
title: "aaaCreate a používání SSH dvojice klíčů pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Jak toocreate a použití SSH pár veřejného a privátního klíče pro virtuální počítače s Linuxem v Azure tooimprove hello zabezpečení procesu ověřování hello."
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
ms.openlocfilehash: 7fb94841d34d5bc006f3134adf91102ddce5f174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Jak spárujte toocreate a používání veřejné a privátní klíč SSH pro virtuální počítače s Linuxem v Azure
Klíče dvojice zabezpečené shell (SSH) můžete vytvořit virtuální počítače (VM) v Azure, který používat klíče SSH k ověření, což eliminuje potřebu hello toolog hesla v. Tento článek ukazuje, jak tooquickly vygenerování a použití veřejného a privátního klíče souboru dvojici SSH verze 2 protokolu RSA pro virtuální počítače s Linuxem. Další kroky a další příklady najdete v tématu [podrobné kroky toocreate páry klíčů SSH a certifikáty](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>Vytvoření páru klíčů SSH
Použití hello `ssh-keygen` příkaz toocreate SSH veřejné a soukromé klíče soubory, které jsou ve výchozím nastavení, které jsou vytvořené v hello `~/.ssh` adresáře, ale můžete zadat jiné umístění a další přístupové heslo (heslo tooaccess hello souborem soukromého klíče) při výzva. Spusťte následující příkaz z prostředí Bash hello, když si odpovíte hello vyzve nahraďte svými vlastními informacemi.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-hello-ssh-key-pair"></a>Používat pár klíčů SSH hello
Hello veřejný klíč, který můžete umístit na virtuálním počítačům s Linuxem v Azure je ve výchozím nastavení uložená v `~/.ssh/id_rsa.pub`, pokud jste změnili umístění hello při jejich vytváření. Pokud používáte hello [Azure CLI 2.0](/cli/azure) toocreate virtuálního počítače, zadejte umístění hello tohoto veřejného klíče, pokud použijete hello [vytvořit virtuální počítač az](/cli/azure/vm#create) s hello `--ssh-key-path` možnost. Pokud zkopírujte a vložte obsah hello hello soubor veřejného klíče toouse hello portál Azure nebo šablony Resource Manageru, ujistěte se, že zkopírujete nemáte žádné další prázdný znak. Například pokud použijete OS X, můžete předat hello soubor veřejného klíče (ve výchozím nastavení, **~/.ssh/id_rsa.pub**) příliš**pbcopy** toocopy hello obsah (existují další programy Linux, které hello totéž, jako je například `xclip`).

Pokud s veřejnými klíči SSH teprve začínáte, můžete svůj veřejný klíč zobrazit spuštěním příkazu `cat`, jak je uvedeno níže, a nahrazením hodnoty `~/.ssh/id_rsa.pub` za umístění vašeho souboru veřejného klíče:

```bash
cat ~/.ssh/id_rsa.pub
```

S veřejným klíčem hello na vašem virtuálním počítači Azure, SSH tooyour virtuální počítač pomocí hello IP adresu nebo název DNS vašeho virtuálního počítače (mějte na paměti, tooreplace `azureuser` a `myvm.westus.cloudapp.azure.com` níže uživatelské jméno správce hello a hello plně kvalifikovaný název domény – nebo IP adresa):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Pokud jste zadali heslo, když jste vytvořili dvojici klíčů, zadejte heslo hello po zobrazení výzvy během procesu přihlášení hello. (Přidat hello server tooyour `~/.ssh/known_hosts` složce a nebudou vyzváni tooconnect znovu, dokud hello veřejný klíč na změny virtuálního počítače Azure nebo název serveru hello se odebere z `~/.ssh/known_hosts`.)

## <a name="next-steps"></a>Další kroky

Virtuální počítače vytvořené pomocí klíče SSH, jsou ve výchozím nastavení nakonfigurované s hesly zakázáno, toomake hrubou silou uhodnutí pokusí významně nákladnější a proto obtížná. Toto téma popisuje vytvoření jednoduchého páru klíčů SSH pro rychlé použití. Pokud potřebujete další pomoc při vytváření dvojici klíčů SSH nebo vyžadovat další certifikáty, najdete v části [podrobné kroky toocreate páry klíčů SSH a certifikáty](create-ssh-keys-detailed.md).

Můžete vytvořit virtuální počítače, které používají dvojici klíčů SSH pomocí hello portálu Azure, rozhraní příkazového řádku a šablony:

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí portálu Azure hello](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0)](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
