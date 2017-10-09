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
# <a name="how-toocreate-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a><span data-ttu-id="ba569-103">Jak spárujte toocreate a používání veřejné a privátní klíč SSH pro virtuální počítače s Linuxem v Azure</span><span class="sxs-lookup"><span data-stu-id="ba569-103">How toocreate and use an SSH public and private key pair for Linux VMs in Azure</span></span>
<span data-ttu-id="ba569-104">Klíče dvojice zabezpečené shell (SSH) můžete vytvořit virtuální počítače (VM) v Azure, který používat klíče SSH k ověření, což eliminuje potřebu hello toolog hesla v.</span><span class="sxs-lookup"><span data-stu-id="ba569-104">With a secure shell (SSH) key pair, you can create virtual machines (VMs) in Azure that use SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span> <span data-ttu-id="ba569-105">Tento článek ukazuje, jak tooquickly vygenerování a použití veřejného a privátního klíče souboru dvojici SSH verze 2 protokolu RSA pro virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="ba569-105">This article shows you how tooquickly generate and use an SSH protocol version 2 RSA public and private key file pair for Linux VMs.</span></span> <span data-ttu-id="ba569-106">Další kroky a další příklady najdete v tématu [podrobné kroky toocreate páry klíčů SSH a certifikáty](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="ba569-106">For more detailed steps and additional examples, see [detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

## <a name="create-an-ssh-key-pair"></a><span data-ttu-id="ba569-107">Vytvoření páru klíčů SSH</span><span class="sxs-lookup"><span data-stu-id="ba569-107">Create an SSH key pair</span></span>
<span data-ttu-id="ba569-108">Použití hello `ssh-keygen` příkaz toocreate SSH veřejné a soukromé klíče soubory, které jsou ve výchozím nastavení, které jsou vytvořené v hello `~/.ssh` adresáře, ale můžete zadat jiné umístění a další přístupové heslo (heslo tooaccess hello souborem soukromého klíče) při výzva.</span><span class="sxs-lookup"><span data-stu-id="ba569-108">Use hello `ssh-keygen` command toocreate SSH public and private key files that are by default created in hello `~/.ssh` directory, but you can specify a different location and additional passphrase (a password tooaccess hello private key file) when prompted.</span></span> <span data-ttu-id="ba569-109">Spusťte následující příkaz z prostředí Bash hello, když si odpovíte hello vyzve nahraďte svými vlastními informacemi.</span><span class="sxs-lookup"><span data-stu-id="ba569-109">Run hello following command from a Bash shell, answering hello prompts with your own information.</span></span>

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-hello-ssh-key-pair"></a><span data-ttu-id="ba569-110">Používat pár klíčů SSH hello</span><span class="sxs-lookup"><span data-stu-id="ba569-110">Use hello SSH key pair</span></span>
<span data-ttu-id="ba569-111">Hello veřejný klíč, který můžete umístit na virtuálním počítačům s Linuxem v Azure je ve výchozím nastavení uložená v `~/.ssh/id_rsa.pub`, pokud jste změnili umístění hello při jejich vytváření.</span><span class="sxs-lookup"><span data-stu-id="ba569-111">hello public key that you place on your Linux VM in Azure is by default stored in `~/.ssh/id_rsa.pub`, unless you changed hello location when you created them.</span></span> <span data-ttu-id="ba569-112">Pokud používáte hello [Azure CLI 2.0](/cli/azure) toocreate virtuálního počítače, zadejte umístění hello tohoto veřejného klíče, pokud použijete hello [vytvořit virtuální počítač az](/cli/azure/vm#create) s hello `--ssh-key-path` možnost.</span><span class="sxs-lookup"><span data-stu-id="ba569-112">If you use hello [Azure CLI 2.0](/cli/azure) toocreate your VM, specify hello location of this public key when you use hello [az vm create](/cli/azure/vm#create) with hello `--ssh-key-path` option.</span></span> <span data-ttu-id="ba569-113">Pokud zkopírujte a vložte obsah hello hello soubor veřejného klíče toouse hello portál Azure nebo šablony Resource Manageru, ujistěte se, že zkopírujete nemáte žádné další prázdný znak.</span><span class="sxs-lookup"><span data-stu-id="ba569-113">If you copy and paste hello contents of hello public key file toouse in hello Azure portal or a Resource Manager template, make sure you don't copy any additional whitespace.</span></span> <span data-ttu-id="ba569-114">Například pokud použijete OS X, můžete předat hello soubor veřejného klíče (ve výchozím nastavení, **~/.ssh/id_rsa.pub**) příliš**pbcopy** toocopy hello obsah (existují další programy Linux, které hello totéž, jako je například `xclip`).</span><span class="sxs-lookup"><span data-stu-id="ba569-114">For example, if you use OS X, you can pipe hello public key file (by default, **~/.ssh/id_rsa.pub**) too**pbcopy** toocopy hello contents (there are other Linux programs that do hello same thing, such as `xclip`).</span></span>

<span data-ttu-id="ba569-115">Pokud s veřejnými klíči SSH teprve začínáte, můžete svůj veřejný klíč zobrazit spuštěním příkazu `cat`, jak je uvedeno níže, a nahrazením hodnoty `~/.ssh/id_rsa.pub` za umístění vašeho souboru veřejného klíče:</span><span class="sxs-lookup"><span data-stu-id="ba569-115">If you're not familiar with SSH public keys, you can see your public key by running `cat` as follows, replacing `~/.ssh/id_rsa.pub` with your own public key file location:</span></span>

```bash
cat ~/.ssh/id_rsa.pub
```

<span data-ttu-id="ba569-116">S veřejným klíčem hello na vašem virtuálním počítači Azure, SSH tooyour virtuální počítač pomocí hello IP adresu nebo název DNS vašeho virtuálního počítače (mějte na paměti, tooreplace `azureuser` a `myvm.westus.cloudapp.azure.com` níže uživatelské jméno správce hello a hello plně kvalifikovaný název domény – nebo IP adresa):</span><span class="sxs-lookup"><span data-stu-id="ba569-116">With hello public key on your Azure VM, SSH tooyour VM using hello IP address or DNS name of your VM (remember tooreplace `azureuser` and `myvm.westus.cloudapp.azure.com` below with hello admin username and hello fully qualified domain name -- or IP address):</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="ba569-117">Pokud jste zadali heslo, když jste vytvořili dvojici klíčů, zadejte heslo hello po zobrazení výzvy během procesu přihlášení hello.</span><span class="sxs-lookup"><span data-stu-id="ba569-117">If you provided a passphrase when you created your key pair, enter hello passphrase when prompted during hello login process.</span></span> <span data-ttu-id="ba569-118">(Přidat hello server tooyour `~/.ssh/known_hosts` složce a nebudou vyzváni tooconnect znovu, dokud hello veřejný klíč na změny virtuálního počítače Azure nebo název serveru hello se odebere z `~/.ssh/known_hosts`.)</span><span class="sxs-lookup"><span data-stu-id="ba569-118">(hello server is added tooyour `~/.ssh/known_hosts` folder, and you won't be asked tooconnect again until hello public key on your Azure VM changes or hello server name is removed from `~/.ssh/known_hosts`.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba569-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba569-119">Next steps</span></span>

<span data-ttu-id="ba569-120">Virtuální počítače vytvořené pomocí klíče SSH, jsou ve výchozím nastavení nakonfigurované s hesly zakázáno, toomake hrubou silou uhodnutí pokusí významně nákladnější a proto obtížná.</span><span class="sxs-lookup"><span data-stu-id="ba569-120">VMs created using SSH keys are by default configured with passwords disabled, toomake brute-forced guessing attempts vastly more expensive and therefore difficult.</span></span> <span data-ttu-id="ba569-121">Toto téma popisuje vytvoření jednoduchého páru klíčů SSH pro rychlé použití.</span><span class="sxs-lookup"><span data-stu-id="ba569-121">This topic describes creating a simple SSH key pair for quick usage.</span></span> <span data-ttu-id="ba569-122">Pokud potřebujete další pomoc při vytváření dvojici klíčů SSH nebo vyžadovat další certifikáty, najdete v části [podrobné kroky toocreate páry klíčů SSH a certifikáty](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="ba569-122">If you need more assistance in creating your SSH key pair or require additional certificates, see [Detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

<span data-ttu-id="ba569-123">Můžete vytvořit virtuální počítače, které používají dvojici klíčů SSH pomocí hello portálu Azure, rozhraní příkazového řádku a šablony:</span><span class="sxs-lookup"><span data-stu-id="ba569-123">You can create VMs that use your SSH key pair using hello Azure portal, CLI, and templates:</span></span>

* [<span data-ttu-id="ba569-124">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="ba569-124">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="ba569-125">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0)</span><span class="sxs-lookup"><span data-stu-id="ba569-125">Create a secure Linux VM using hello Azure CLI 2.0)</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="ba569-126">Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure</span><span class="sxs-lookup"><span data-stu-id="ba569-126">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
