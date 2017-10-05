---
title: "Přesuňte soubory do a z virtuálních počítačů Linux Azure s spojovací bod služby | Microsoft Docs"
description: "Bezpečně přesunout soubory do a z virtuálního počítače s Linuxem v Azure pomocí spojovací bod služby a dvojici klíčů SSH."
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
ms.openlocfilehash: 736f7c11ec3de04f1ad52ee29d0a4c952c9b0545
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="move-files-to-and-from-a-linux-vm-using-scp"></a><span data-ttu-id="7506d-103">Přesunutí souborů do a z virtuálního počítače s Linuxem pomocí spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="7506d-103">Move files to and from a Linux VM using SCP</span></span>

<span data-ttu-id="7506d-104">Tento článek ukazuje, jak k přesunutí souborů z pracovní stanice až virtuální počítač Azure Linux nebo Linux virtuální počítač Azure na pracovní stanici, pomocí zabezpečené kopírování (SCP).</span><span class="sxs-lookup"><span data-stu-id="7506d-104">This article shows how to move files from your workstation up to an Azure Linux VM, or from an Azure Linux VM down to your workstation, using Secure Copy (SCP).</span></span> <span data-ttu-id="7506d-105">Přesouvání souborů mezi pracovní stanice a virtuální počítač s Linuxem, rychle a bezpečně, je důležité pro správu infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="7506d-105">Moving files between your workstation and a Linux VM, quickly and securely, is critical for managing your Azure infrastructure.</span></span> 

<span data-ttu-id="7506d-106">V tomto článku budete potřebovat Linux virtuálních počítačů nasadit v Azure pomocí [SSH soubory veřejného a privátního klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7506d-106">For this article, you need a Linux VM deployed in Azure using [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="7506d-107">Budete také potřebovat klientem spojovací bod služby v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="7506d-107">You also need an SCP client for your local computer.</span></span> <span data-ttu-id="7506d-108">Je postavená na SSH a součástí výchozí prostředí Bash většina Linux a počítače Mac a některé součásti pro Windows.</span><span class="sxs-lookup"><span data-stu-id="7506d-108">It is built on top of SSH and included in the default Bash shell of most Linux and Mac computers and some Windows shells.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="7506d-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="7506d-109">Quick commands</span></span>

<span data-ttu-id="7506d-110">Zkopírujte soubor až virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="7506d-110">Copy a file up to the Linux VM</span></span>

```bash
scp file azureuser@azurehost:directory/targetfile
```

<span data-ttu-id="7506d-111">Zkopírujte soubor dolů z virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="7506d-111">Copy a file down from the Linux VM</span></span>

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="7506d-112">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="7506d-112">Detailed walkthrough</span></span>

<span data-ttu-id="7506d-113">Jako příklady, jsme přesunout soubor konfigurace Azure až virtuálního počítače s Linuxem a vyžadování dolů adresář souboru protokolu, jak pomocí spojovací bod služby a SSH klíče.</span><span class="sxs-lookup"><span data-stu-id="7506d-113">As examples, we move an Azure configuration file up to a Linux VM and pull down a log file directory, both using SCP and SSH keys.</span></span>   

## <a name="ssh-key-pair-authentication"></a><span data-ttu-id="7506d-114">Ověřování pár klíčů SSH</span><span class="sxs-lookup"><span data-stu-id="7506d-114">SSH key pair authentication</span></span>

<span data-ttu-id="7506d-115">Spojovací bod služby používá SSH pro přenosové vrstvy.</span><span class="sxs-lookup"><span data-stu-id="7506d-115">SCP uses SSH for the transport layer.</span></span> <span data-ttu-id="7506d-116">SSH zpracovává ověřování na cílovém hostiteli a přesune soubor v tunelu šifrované ve výchozím nastavení provádí pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="7506d-116">SSH handles the authentication on the destination host, and it moves the file in an encrypted tunnel provided by default with SSH.</span></span> <span data-ttu-id="7506d-117">Pro ověřování SSH lze použít uživatelských jmen a hesel.</span><span class="sxs-lookup"><span data-stu-id="7506d-117">For SSH authentication, usernames and passwords can be used.</span></span> <span data-ttu-id="7506d-118">Veřejné a soukromé klíče ověřování SSH jsou však doporučujeme jako osvědčený postup zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7506d-118">However, SSH public and private key authentication are recommended as a security best practice.</span></span> <span data-ttu-id="7506d-119">Po ověření připojení SSH spojovací bod služby poté zahájí kopírování souboru.</span><span class="sxs-lookup"><span data-stu-id="7506d-119">Once SSH has authenticated the connection, SCP then begins copying the file.</span></span> <span data-ttu-id="7506d-120">Pomocí správně nakonfigurovaných `~/.ssh/config` a veřejného a privátního klíče SSH, spojovací bod služby připojení lze navázat jen pomocí názvu serveru (nebo IP adresa).</span><span class="sxs-lookup"><span data-stu-id="7506d-120">Using a properly configured `~/.ssh/config` and SSH public and private keys, the SCP connection can be established by just using a server name (or IP address).</span></span> <span data-ttu-id="7506d-121">Pokud máte pouze jeden klíč SSH, spojovací bod služby hledá v `~/.ssh/` adresář a používá je ve výchozím nastavení k přihlášení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="7506d-121">If you only have one SSH key, SCP looks for it in the `~/.ssh/` directory, and uses it by default to log in to the VM.</span></span>

<span data-ttu-id="7506d-122">Další informace o konfiguraci vašeho `~/.ssh/config` a veřejné a soukromé klíče SSH, najdete v části [vytvořit SSH klíče](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7506d-122">For more information on configuring your `~/.ssh/config` and SSH public and private keys, see [Create SSH keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="scp-a-file-to-a-linux-vm"></a><span data-ttu-id="7506d-123">Spojovací bod služby soubor, který chcete virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="7506d-123">SCP a file to a Linux VM</span></span>

<span data-ttu-id="7506d-124">V prvním příkladu jsme zkopírujte soubor konfigurace Azure až Linux virtuálního počítače, který se používá k nasazení automatizace.</span><span class="sxs-lookup"><span data-stu-id="7506d-124">For the first example, we copy an Azure configuration file up to a Linux VM that is used to deploy automation.</span></span> <span data-ttu-id="7506d-125">Protože tento soubor obsahuje přihlašovací údaje Azure API, které obsahují tajné klíče, je důležité zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="7506d-125">Because this file contains Azure API credentials, which include secrets, security is important.</span></span> <span data-ttu-id="7506d-126">Šifrované tunelového propojení SSH poskytované chrání obsah souboru.</span><span class="sxs-lookup"><span data-stu-id="7506d-126">The encrypted tunnel provided by SSH protects the contents of the file.</span></span>

<span data-ttu-id="7506d-127">Následující příkaz zkopíruje místní *.azure/config* souborů na virtuální počítač Azure s plně kvalifikovaný název domény *myserver.eastus.cloudapp.azure.com*. Uživatelské jméno správce ve virtuálním počítači Azure je *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="7506d-127">The following command copies the local *.azure/config* file to an Azure VM with FQDN *myserver.eastus.cloudapp.azure.com*. The admin user name on the Azure VM is *azureuser*.</span></span> <span data-ttu-id="7506d-128">Soubor je zaměřena na */home/azureuser/* adresáře.</span><span class="sxs-lookup"><span data-stu-id="7506d-128">The file is targeted to the */home/azureuser/* directory.</span></span> <span data-ttu-id="7506d-129">Nahraďte vlastními hodnotami v tomto příkazu.</span><span class="sxs-lookup"><span data-stu-id="7506d-129">Substitute your own values in this command.</span></span>

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a><span data-ttu-id="7506d-130">Spojovací bod služby adresáře z virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="7506d-130">SCP a directory from a Linux VM</span></span>

<span data-ttu-id="7506d-131">V tomto příkladu jsme zkopírujte adresář souborů protokolu z virtuálního počítače s Linuxem na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="7506d-131">For this example, we copy a directory of log files from the Linux VM down to your workstation.</span></span> <span data-ttu-id="7506d-132">Soubor protokolu může nebo nemusí obsahovat citlivá nebo tajný data.</span><span class="sxs-lookup"><span data-stu-id="7506d-132">A log file may or may not contain sensitive or secret data.</span></span> <span data-ttu-id="7506d-133">Použití spojovací bod služby zajišťuje ale, že jsou šifrovaná obsah souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="7506d-133">However, using SCP ensures the contents of the log files are encrypted.</span></span> <span data-ttu-id="7506d-134">Použití spojovací bod služby k přenosu souborů je nejjednodušší způsob, jak získat adresář protokolu a souborů na pracovní stanici při zároveň zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="7506d-134">Using SCP to transfer the files is the easiest way to get the log directory and files down to your workstation while also being secure.</span></span>

<span data-ttu-id="7506d-135">Následující příkaz zkopíruje soubory v */home/azureuser/logs/* adresář na virtuálním počítači Azure do místní TMP adresáře:</span><span class="sxs-lookup"><span data-stu-id="7506d-135">The following command copies files in the */home/azureuser/logs/* directory on the Azure VM to the local /tmp directory:</span></span>

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

<span data-ttu-id="7506d-136">`-r` Rozhraní příkazového řádku příznak dá pokyn spojovací bod služby k rekurzivnímu kopírování souborů a adresářů z bodu adresáře uvedené v příkazu.</span><span class="sxs-lookup"><span data-stu-id="7506d-136">The `-r` cli flag instructs SCP to recursively copy the files and directories from the point of the directory listed in the command.</span></span>  <span data-ttu-id="7506d-137">Také Všimněte si, že je podobná syntaxe příkazového řádku `cp` zkopírujte příkaz.</span><span class="sxs-lookup"><span data-stu-id="7506d-137">Also notice that the command-line syntax is similar to a `cp` copy command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7506d-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7506d-138">Next steps</span></span>

* [<span data-ttu-id="7506d-139">Spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess</span><span class="sxs-lookup"><span data-stu-id="7506d-139">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)