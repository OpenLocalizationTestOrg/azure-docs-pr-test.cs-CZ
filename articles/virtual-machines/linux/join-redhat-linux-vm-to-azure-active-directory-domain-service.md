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
# <a name="join-a-redhat-linux-vm-tooan-azure-active-directory-domain-service"></a><span data-ttu-id="fd146-103">Připojení virtuálního počítače s Linuxem RedHat tooan Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="fd146-103">Join a RedHat Linux VM tooan Azure Active Directory Domain Service</span></span>

<span data-ttu-id="fd146-104">Tento článek ukazuje, jak toojoin tooan virtuální počítač Red Hat Enterprise Linux (RHEL) 7 Azure Active Directory Domain Services (AADDS) spravované domény.</span><span class="sxs-lookup"><span data-stu-id="fd146-104">This article shows you how toojoin a Red Hat Enterprise Linux (RHEL) 7 virtual machine tooan Azure Active Directory Domain Services (AADDS) managed domain.</span></span>  <span data-ttu-id="fd146-105">Hello požadavky jsou:</span><span class="sxs-lookup"><span data-stu-id="fd146-105">hello requirements are:</span></span>

- [<span data-ttu-id="fd146-106">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="fd146-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)

- [<span data-ttu-id="fd146-107">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="fd146-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

- [<span data-ttu-id="fd146-108">Azure Active Directory Domain Services řadiče domény</span><span class="sxs-lookup"><span data-stu-id="fd146-108">an Azure Active Directory Domain Services DC</span></span>](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="fd146-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="fd146-109">Quick Commands</span></span>

<span data-ttu-id="fd146-110">_Nahradí všechny příklady s vlastním nastavením._</span><span class="sxs-lookup"><span data-stu-id="fd146-110">_Replace any examples with your own settings._</span></span>

### <a name="switch-hello-azure-cli-tooclassic-deployment-mode"></a><span data-ttu-id="fd146-111">Přepnutí režimu nasazení tooclassic hello rozhraní příkazového řádku azure</span><span class="sxs-lookup"><span data-stu-id="fd146-111">Switch hello azure-cli tooclassic deployment mode</span></span>

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a><span data-ttu-id="fd146-112">Hledat verze RHELU a bitové kopie</span><span class="sxs-lookup"><span data-stu-id="fd146-112">Search for a RHEL version and image</span></span>

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a><span data-ttu-id="fd146-113">Vytvoření Redhat Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fd146-113">Create a Redhat Linux VM</span></span>

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

### <a name="ssh-toohello-vm"></a><span data-ttu-id="fd146-114">SSH toohello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fd146-114">SSH toohello VM</span></span>

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a><span data-ttu-id="fd146-115">Balíčky aktualizací YUM</span><span class="sxs-lookup"><span data-stu-id="fd146-115">Update YUM packages</span></span>

```bash
sudo yum update
```

### <a name="install-packages-needed"></a><span data-ttu-id="fd146-116">Instalovat balíčky potřeby</span><span class="sxs-lookup"><span data-stu-id="fd146-116">Install packages needed</span></span>

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

<span data-ttu-id="fd146-117">Teď, když hello požadované balíčky jsou nainstalovány na hello Linux virtuálního počítače, dalším úkolem hello je toojoin hello virtuálního počítače toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="fd146-117">Now that hello required packages are installed on hello Linux virtual machine, hello next task is toojoin hello virtual machine toohello managed domain.</span></span>

### <a name="discover-hello-aad-domain-services-managed-domain"></a><span data-ttu-id="fd146-118">Zjistit hello služby AAD Domain Services spravované doméně</span><span class="sxs-lookup"><span data-stu-id="fd146-118">Discover hello AAD Domain Services managed domain</span></span>

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a><span data-ttu-id="fd146-119">Inicializace protokolu kerberos</span><span class="sxs-lookup"><span data-stu-id="fd146-119">Initialize kerberos</span></span>

<span data-ttu-id="fd146-120">Ujistěte se, že zadáváte uživatel, který patří skupině toohello 'AAD řadič domény Administrators'.</span><span class="sxs-lookup"><span data-stu-id="fd146-120">Ensure that you specify a user who belongs toohello 'AAD DC Administrators' group.</span></span> <span data-ttu-id="fd146-121">Pouze tito uživatelé mohou připojit počítače toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="fd146-121">Only these users can join computers toohello managed domain.</span></span>

```bash
kinit ahmet@mydomain.com
```

### <a name="join-hello-machine-toohello-domain"></a><span data-ttu-id="fd146-122">Připojení k doméně toohello počítač hello</span><span class="sxs-lookup"><span data-stu-id="fd146-122">Join hello machine toohello domain</span></span>

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-hello-machine-is-joined-toohello-domain"></a><span data-ttu-id="fd146-123">Ověřte, zda text hello počítač je připojený k toohello domény</span><span class="sxs-lookup"><span data-stu-id="fd146-123">Verify hello machine is joined toohello domain</span></span>

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="fd146-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd146-124">Next Steps</span></span>

* [<span data-ttu-id="fd146-125">Red Hat aktualizace infrastruktury (RHUI) pro na vyžádání Red Hat Enterprise Linux virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="fd146-125">Red Hat Update Infrastructure (RHUI) for on-demand Red Hat Enterprise Linux VMs in Azure</span></span>](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="fd146-126">Nastavení pro virtuální počítače ve službě Správce prostředků Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="fd146-126">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="fd146-127">Nasazení a správě virtuálních počítačů pomocí šablon Azure Resource Manageru a hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="fd146-127">Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI</span></span>](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
