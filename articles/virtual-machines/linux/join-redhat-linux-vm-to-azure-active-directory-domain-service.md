---
title: "virtuální počítač s Linuxem RedHat tooan Azure Active Directory DS aaaJoin | Microsoft Docs"
description: "Jak toojoin existující virtuální počítač Red Hat Enterprise Linux 7 tooan Azure Active Directory Domain Services."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2016
ms.author: v-livech
ms.openlocfilehash: f3ba3c764e253191753f1cc5fc8c3b85c53818af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-redhat-linux-vm-tooan-azure-active-directory-domain-service"></a>Připojení virtuálního počítače s Linuxem RedHat tooan Azure Active Directory Domain Services

Tento článek ukazuje, jak toojoin tooan virtuální počítač Red Hat Enterprise Linux (RHEL) 7 Azure Active Directory Domain Services (AADDS) spravované domény.  Hello požadavky jsou:

- [Účet Azure](https://azure.microsoft.com/pricing/free-trial/)

- [Soubory veřejného a privátního klíče SSH](mac-create-ssh-keys.md)

- [Azure Active Directory Domain Services řadiče domény](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Rychlé příkazy

_Nahradí všechny příklady s vlastním nastavením._

### <a name="switch-hello-azure-cli-tooclassic-deployment-mode"></a>Přepnutí režimu nasazení tooclassic hello rozhraní příkazového řádku azure

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a>Hledat verze RHELU a bitové kopie

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a>Vytvoření Redhat Linux virtuálního počítače

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p myPassword \
-e 22 \
-t "~/.ssh/id_rsa.pub" \
-z "Small" \
-l "West US"
```

### <a name="ssh-toohello-vm"></a>SSH toohello virtuálních počítačů

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a>Balíčky aktualizací YUM

```bash
sudo yum update
```

### <a name="install-packages-needed"></a>Instalovat balíčky potřeby

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

Teď, když hello požadované balíčky jsou nainstalovány na hello Linux virtuálního počítače, dalším úkolem hello je toojoin hello virtuálního počítače toohello spravované domény.

### <a name="discover-hello-aad-domain-services-managed-domain"></a>Zjistit hello služby AAD Domain Services spravované doméně

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a>Inicializace protokolu kerberos

Ujistěte se, že zadáváte uživatel, který patří skupině toohello 'AAD řadič domény Administrators'. Pouze tito uživatelé mohou připojit počítače toohello spravované domény.

```bash
kinit ahmet@mydomain.com
```

### <a name="join-hello-machine-toohello-domain"></a>Připojení k doméně toohello počítač hello

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-hello-machine-is-joined-toohello-domain"></a>Ověřte, zda text hello počítač je připojený k toohello domény

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a>Další kroky

* [Red Hat aktualizace infrastruktury (RHUI) pro na vyžádání Red Hat Enterprise Linux virtuálních počítačů v Azure](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Nastavení pro virtuální počítače ve službě Správce prostředků Azure Key Vault](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Nasazení a správě virtuálních počítačů pomocí šablon Azure Resource Manageru a hello rozhraní příkazového řádku Azure](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
