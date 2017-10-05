---
title: "Připojení RedHat Linux virtuálního počítače do Azure Active Directory DS | Microsoft Docs"
description: "Postup připojení k existující Red Hat Enterprise Linux 7 virtuální počítač do služby Azure Active Directory Domain Services."
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
ms.openlocfilehash: 2e46a0f3c9bdbe267d121b4bf62e25d5d411fcc2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="join-a-redhat-linux-vm-to-an-azure-active-directory-domain-service"></a><span data-ttu-id="cb01e-103">Připojení RedHat Linux virtuálního počítače do domény služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb01e-103">Join a RedHat Linux VM to an Azure Active Directory Domain Service</span></span>

<span data-ttu-id="cb01e-104">Tento článek ukazuje, jak připojit virtuální počítač Red Hat Enterprise Linux (RHEL) 7 k spravované doméně služby Azure Active Directory Domain Services (AADDS).</span><span class="sxs-lookup"><span data-stu-id="cb01e-104">This article shows you how to join a Red Hat Enterprise Linux (RHEL) 7 virtual machine to an Azure Active Directory Domain Services (AADDS) managed domain.</span></span>  <span data-ttu-id="cb01e-105">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="cb01e-105">The requirements are:</span></span>

- [<span data-ttu-id="cb01e-106">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="cb01e-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)

- [<span data-ttu-id="cb01e-107">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="cb01e-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

- [<span data-ttu-id="cb01e-108">Azure Active Directory Domain Services řadiče domény</span><span class="sxs-lookup"><span data-stu-id="cb01e-108">an Azure Active Directory Domain Services DC</span></span>](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a><span data-ttu-id="cb01e-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="cb01e-109">Quick Commands</span></span>

<span data-ttu-id="cb01e-110">_Nahradí všechny příklady s vlastním nastavením._</span><span class="sxs-lookup"><span data-stu-id="cb01e-110">_Replace any examples with your own settings._</span></span>

### <a name="switch-the-azure-cli-to-classic-deployment-mode"></a><span data-ttu-id="cb01e-111">Přepínač příkazového řádku azure k nasazení classic</span><span class="sxs-lookup"><span data-stu-id="cb01e-111">Switch the azure-cli to classic deployment mode</span></span>

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a><span data-ttu-id="cb01e-112">Hledat verze RHELU a bitové kopie</span><span class="sxs-lookup"><span data-stu-id="cb01e-112">Search for a RHEL version and image</span></span>

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a><span data-ttu-id="cb01e-113">Vytvoření Redhat Linux virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cb01e-113">Create a Redhat Linux VM</span></span>

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

### <a name="ssh-to-the-vm"></a><span data-ttu-id="cb01e-114">Přístup k virtuálnímu počítači přes SSH</span><span class="sxs-lookup"><span data-stu-id="cb01e-114">SSH to the VM</span></span>

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a><span data-ttu-id="cb01e-115">Balíčky aktualizací YUM</span><span class="sxs-lookup"><span data-stu-id="cb01e-115">Update YUM packages</span></span>

```bash
sudo yum update
```

### <a name="install-packages-needed"></a><span data-ttu-id="cb01e-116">Instalovat balíčky potřeby</span><span class="sxs-lookup"><span data-stu-id="cb01e-116">Install packages needed</span></span>

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

<span data-ttu-id="cb01e-117">Teď, když požadované balíčky jsou nainstalovány na virtuální počítač Linux, dalším úkolem je připojení virtuálního počítače k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="cb01e-117">Now that the required packages are installed on the Linux virtual machine, the next task is to join the virtual machine to the managed domain.</span></span>

### <a name="discover-the-aad-domain-services-managed-domain"></a><span data-ttu-id="cb01e-118">Zjistit spravované doméně služby AAD Domain Services</span><span class="sxs-lookup"><span data-stu-id="cb01e-118">Discover the AAD Domain Services managed domain</span></span>

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a><span data-ttu-id="cb01e-119">Inicializace protokolu kerberos</span><span class="sxs-lookup"><span data-stu-id="cb01e-119">Initialize kerberos</span></span>

<span data-ttu-id="cb01e-120">Ujistěte se, že zadáváte uživatel, který patří do skupiny "Administrators AAD řadič domény.</span><span class="sxs-lookup"><span data-stu-id="cb01e-120">Ensure that you specify a user who belongs to the 'AAD DC Administrators' group.</span></span> <span data-ttu-id="cb01e-121">Pouze tito uživatelé se může připojit počítače k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="cb01e-121">Only these users can join computers to the managed domain.</span></span>

```bash
kinit ahmet@mydomain.com
```

### <a name="join-the-machine-to-the-domain"></a><span data-ttu-id="cb01e-122">Připojení počítače k doméně</span><span class="sxs-lookup"><span data-stu-id="cb01e-122">Join the machine to the domain</span></span>

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-the-machine-is-joined-to-the-domain"></a><span data-ttu-id="cb01e-123">Ověřte, zda že je počítač připojený k doméně</span><span class="sxs-lookup"><span data-stu-id="cb01e-123">Verify the machine is joined to the domain</span></span>

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="cb01e-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb01e-124">Next Steps</span></span>

* [<span data-ttu-id="cb01e-125">Red Hat aktualizace infrastruktury (RHUI) pro na vyžádání Red Hat Enterprise Linux virtuálních počítačů v Azure</span><span class="sxs-lookup"><span data-stu-id="cb01e-125">Red Hat Update Infrastructure (RHUI) for on-demand Red Hat Enterprise Linux VMs in Azure</span></span>](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="cb01e-126">Nastavení pro virtuální počítače ve službě Správce prostředků Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="cb01e-126">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="cb01e-127">Nasazení a správě virtuálních počítačů pomocí šablon Azure Resource Manageru a rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="cb01e-127">Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI</span></span>](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
