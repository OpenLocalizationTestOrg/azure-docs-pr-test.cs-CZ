---
title: "aaaMove soubory tooand z virtuálních počítačů Linux Azure s spojovací bod služby | Microsoft Docs"
description: "Bezpečně přesunout tooand soubory z virtuálního počítače s Linuxem v Azure pomocí spojovací bod služby a dvojici klíčů SSH."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 9e4dce667aa581df74aec3d45a6ba5e52f777d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-files-tooand-from-a-linux-vm-using-scp"></a><span data-ttu-id="9acb9-103">Přesun tooand soubory z virtuálního počítače s Linuxem pomocí spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="9acb9-103">Move files tooand from a Linux VM using SCP</span></span>

<span data-ttu-id="9acb9-104">Tento článek ukazuje, jak toomove soubory z pracovní stanici si tooan virtuální počítač Azure s Linuxem nebo Linux virtuálního počítače Azure dolů tooyour pracovní stanici, pomocí zabezpečené kopírování (SCP).</span><span class="sxs-lookup"><span data-stu-id="9acb9-104">This article shows how toomove files from your workstation up tooan Azure Linux VM, or from an Azure Linux VM down tooyour workstation, using Secure Copy (SCP).</span></span> <span data-ttu-id="9acb9-105">Přesouvání souborů mezi pracovní stanice a virtuální počítač s Linuxem, rychle a bezpečně, je důležité pro správu infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="9acb9-105">Moving files between your workstation and a Linux VM, quickly and securely, is critical for managing your Azure infrastructure.</span></span> 

<span data-ttu-id="9acb9-106">V tomto článku budete potřebovat Linux virtuálních počítačů nasadit v Azure pomocí [SSH soubory veřejného a privátního klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9acb9-106">For this article, you need a Linux VM deployed in Azure using [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="9acb9-107">Budete také potřebovat klientem spojovací bod služby v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="9acb9-107">You also need an SCP client for your local computer.</span></span> <span data-ttu-id="9acb9-108">Je postavená na SSH a součástí prostředí Bash výchozí hello většina Linux a počítače Mac a některé součásti pro Windows.</span><span class="sxs-lookup"><span data-stu-id="9acb9-108">It is built on top of SSH and included in hello default Bash shell of most Linux and Mac computers and some Windows shells.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="9acb9-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="9acb9-109">Quick commands</span></span>

<span data-ttu-id="9acb9-110">Zkopírujte soubor do toohello virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9acb9-110">Copy a file up toohello Linux VM</span></span>

```bash
scp file azureuser@azurehost:directory/targetfile
```

<span data-ttu-id="9acb9-111">Zkopírujte soubor dolů z hello virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9acb9-111">Copy a file down from hello Linux VM</span></span>

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="9acb9-112">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="9acb9-112">Detailed walkthrough</span></span>

<span data-ttu-id="9acb9-113">Jako příklady, můžeme přesunout soubor konfigurace Azure až tooa virtuálního počítače s Linuxem a stahují adresář souboru protokolu, jak pomocí klíče spojovací bod služby a SSH.</span><span class="sxs-lookup"><span data-stu-id="9acb9-113">As examples, we move an Azure configuration file up tooa Linux VM and pull down a log file directory, both using SCP and SSH keys.</span></span>   

## <a name="ssh-key-pair-authentication"></a><span data-ttu-id="9acb9-114">Ověřování pár klíčů SSH</span><span class="sxs-lookup"><span data-stu-id="9acb9-114">SSH key pair authentication</span></span>

<span data-ttu-id="9acb9-115">Spojovací bod služby používá SSH pro hello přenosové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="9acb9-115">SCP uses SSH for hello transport layer.</span></span> <span data-ttu-id="9acb9-116">SSH popisovače hello ověřování na cílovém hostiteli hello a přesune soubor hello v tunelu šifrované ve výchozím nastavení provádí pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="9acb9-116">SSH handles hello authentication on hello destination host, and it moves hello file in an encrypted tunnel provided by default with SSH.</span></span> <span data-ttu-id="9acb9-117">Pro ověřování SSH lze použít uživatelských jmen a hesel.</span><span class="sxs-lookup"><span data-stu-id="9acb9-117">For SSH authentication, usernames and passwords can be used.</span></span> <span data-ttu-id="9acb9-118">Veřejné a soukromé klíče ověřování SSH jsou však doporučujeme jako osvědčený postup zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9acb9-118">However, SSH public and private key authentication are recommended as a security best practice.</span></span> <span data-ttu-id="9acb9-119">Po ověření hello připojení SSH spojovací bod služby poté zahájí kopírování souboru hello.</span><span class="sxs-lookup"><span data-stu-id="9acb9-119">Once SSH has authenticated hello connection, SCP then begins copying hello file.</span></span> <span data-ttu-id="9acb9-120">Pomocí správně nakonfigurovaných `~/.ssh/config` a veřejného a privátního klíče SSH, hello spojovací bod služby připojení lze navázat jen pomocí názvu serveru (nebo IP adresa).</span><span class="sxs-lookup"><span data-stu-id="9acb9-120">Using a properly configured `~/.ssh/config` and SSH public and private keys, hello SCP connection can be established by just using a server name (or IP address).</span></span> <span data-ttu-id="9acb9-121">Pokud máte pouze jeden klíč SSH, spojovací bod služby hledá pro něj v hello `~/.ssh/` adresář a používá je ve výchozím nastavení toolog v toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9acb9-121">If you only have one SSH key, SCP looks for it in hello `~/.ssh/` directory, and uses it by default toolog in toohello VM.</span></span>

<span data-ttu-id="9acb9-122">Další informace o konfiguraci vašeho `~/.ssh/config` a veřejné a soukromé klíče SSH, najdete v části [vytvořit SSH klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9acb9-122">For more information on configuring your `~/.ssh/config` and SSH public and private keys, see [Create SSH keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="scp-a-file-tooa-linux-vm"></a><span data-ttu-id="9acb9-123">Spojovací bod služby tooa soubor virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9acb9-123">SCP a file tooa Linux VM</span></span>

<span data-ttu-id="9acb9-124">Hello prvním příkladu jsme zkopírujte soubor konfigurace Azure až tooa Linux virtuálního počítače, který je použité toodeploy automation.</span><span class="sxs-lookup"><span data-stu-id="9acb9-124">For hello first example, we copy an Azure configuration file up tooa Linux VM that is used toodeploy automation.</span></span> <span data-ttu-id="9acb9-125">Protože tento soubor obsahuje přihlašovací údaje Azure API, které obsahují tajné klíče, je důležité zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9acb9-125">Because this file contains Azure API credentials, which include secrets, security is important.</span></span> <span data-ttu-id="9acb9-126">Hello šifrované tunelového propojení SSH poskytované chrání obsah hello hello souboru.</span><span class="sxs-lookup"><span data-stu-id="9acb9-126">hello encrypted tunnel provided by SSH protects hello contents of hello file.</span></span>

<span data-ttu-id="9acb9-127">Následující příkaz kopie hello místní Hello *.azure/config* souboru tooan virtuální počítač Azure s plně kvalifikovaný název domény *myserver.eastus.cloudapp.azure.com*. uživatelské jméno správce hello na hello virtuální počítač Azure je *azureuser* .</span><span class="sxs-lookup"><span data-stu-id="9acb9-127">hello following command copies hello local *.azure/config* file tooan Azure VM with FQDN *myserver.eastus.cloudapp.azure.com*. hello admin user name on hello Azure VM is *azureuser*.</span></span> <span data-ttu-id="9acb9-128">Hello soubor je cílové toohello */home/azureuser/* adresáře.</span><span class="sxs-lookup"><span data-stu-id="9acb9-128">hello file is targeted toohello */home/azureuser/* directory.</span></span> <span data-ttu-id="9acb9-129">Nahraďte vlastními hodnotami v tomto příkazu.</span><span class="sxs-lookup"><span data-stu-id="9acb9-129">Substitute your own values in this command.</span></span>

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a><span data-ttu-id="9acb9-130">Spojovací bod služby adresáře z virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9acb9-130">SCP a directory from a Linux VM</span></span>

<span data-ttu-id="9acb9-131">V tomto příkladu jsme zkopírujte adresář souborů protokolu z hello virtuálního počítače s Linuxem dolů tooyour pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="9acb9-131">For this example, we copy a directory of log files from hello Linux VM down tooyour workstation.</span></span> <span data-ttu-id="9acb9-132">Soubor protokolu může nebo nemusí obsahovat citlivá nebo tajný data.</span><span class="sxs-lookup"><span data-stu-id="9acb9-132">A log file may or may not contain sensitive or secret data.</span></span> <span data-ttu-id="9acb9-133">Použití spojovací bod služby zajišťuje ale, že jsou šifrovaná hello obsah souborů protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="9acb9-133">However, using SCP ensures hello contents of hello log files are encrypted.</span></span> <span data-ttu-id="9acb9-134">Pomocí souborů hello tootransfer spojovací bod služby je nejjednodušší způsob, jak tooget hello hello adresář protokolu a soubory dolů pracovní stanice tooyour při zároveň zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="9acb9-134">Using SCP tootransfer hello files is hello easiest way tooget hello log directory and files down tooyour workstation while also being secure.</span></span>

<span data-ttu-id="9acb9-135">Hello následující příkaz zkopíruje soubory v hello */home/azureuser/logs/* v hello virtuálního počítače Azure toohello místní TMP adresáře:</span><span class="sxs-lookup"><span data-stu-id="9acb9-135">hello following command copies files in hello */home/azureuser/logs/* directory on hello Azure VM toohello local /tmp directory:</span></span>

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

<span data-ttu-id="9acb9-136">Hello `-r` rozhraní příkazového řádku příznak dá pokyn spojovací bod služby toorecursively kopie hello souborů a adresářů z bodu hello hello adresáře uvedené v příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="9acb9-136">hello `-r` cli flag instructs SCP toorecursively copy hello files and directories from hello point of hello directory listed in hello command.</span></span>  <span data-ttu-id="9acb9-137">Všimněte si, že hello syntaxe příkazového řádku je také podobné tooa `cp` zkopírujte příkaz.</span><span class="sxs-lookup"><span data-stu-id="9acb9-137">Also notice that hello command-line syntax is similar tooa `cp` copy command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9acb9-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9acb9-138">Next steps</span></span>

* [<span data-ttu-id="9acb9-139">Spravovat uživatele, SSH a zkontrolujte nebo hello oprava disky na virtuální počítače Azure s Linuxem pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="9acb9-139">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
