---
title: "Úvod do systému Linux v Azure | Microsoft Docs"
description: "Další informace o použití virtuální počítače s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: python
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: b13bf305-87bf-4df3-815e-e8f6337aa6ea
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: szark
ms.openlocfilehash: 7bd0c5549a2e1f592681760d5ef464b9570ca4ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-linux-on-azure"></a><span data-ttu-id="e601c-103">Úvod do Linuxu na Azure</span><span class="sxs-lookup"><span data-stu-id="e601c-103">Introduction to Linux on Azure</span></span>
<span data-ttu-id="e601c-104">Toto téma obsahuje přehled některých aspektů pomocí virtuální počítače s Linuxem v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="e601c-104">This topic provides an overview of some aspects of using Linux virtual machines in the Azure cloud.</span></span> <span data-ttu-id="e601c-105">Virtuální počítač s Linuxem nasazení je jednoduchý proces pomocí bitovou kopii z galerie.</span><span class="sxs-lookup"><span data-stu-id="e601c-105">Deploying a Linux virtual machine is a straightforward process using an image from the gallery.</span></span>

## <a name="authentication-usernames-passwords-and-ssh-keys"></a><span data-ttu-id="e601c-106">Ověřování: Uživatelská jména, hesla a klíče SSH</span><span class="sxs-lookup"><span data-stu-id="e601c-106">Authentication: Usernames, Passwords and SSH Keys</span></span>
<span data-ttu-id="e601c-107">Když vytváříte virtuální počítač s Linuxem pomocí portálu Azure, budete vyzváni k zadání buď uživatelské jméno a heslo nebo veřejný klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="e601c-107">When creating a Linux virtual machine using the Azure portal, you are asked to provide a either username and password or an SSH public key.</span></span> <span data-ttu-id="e601c-108">Volba uživatelské jméno pro nasazení virtuální počítač s Linuxem v Azure se vztahují následující omezení: názvy účtů systému (UID < 100), který už ve virtuálním počítači nejsou povoleny, root, třeba.</span><span class="sxs-lookup"><span data-stu-id="e601c-108">The choice of a username for deploying a Linux virtual machine on Azure is subject to the following constraint: names of system accounts (UID <100) already present in the virtual machine are not allowed, 'root' for example.</span></span>

* <span data-ttu-id="e601c-109">V tématu [vytvoření virtuálního počítače se systémem Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e601c-109">See [Create a Virtual Machine Running Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="e601c-110">V tématu [postup používání SSH s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e601c-110">See [How to Use SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="obtaining-superuser-privileges-using-sudo"></a><span data-ttu-id="e601c-111">Získání oprávnění superuživatele pomocí`sudo`</span><span class="sxs-lookup"><span data-stu-id="e601c-111">Obtaining Superuser Privileges Using `sudo`</span></span>
<span data-ttu-id="e601c-112">Uživatelský účet, který je zadán během nasazení instance virtuálních počítačů v Azure je privilegovaný účet.</span><span class="sxs-lookup"><span data-stu-id="e601c-112">The user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="e601c-113">Tento účet je nakonfigurovat pomocí Azure Linux Agent moct zvýšení oprávnění pomocí root (superuživatel účet) `sudo` nástroj.</span><span class="sxs-lookup"><span data-stu-id="e601c-113">This account is configured by the Azure Linux Agent to be able to elevate privileges to root (superuser account) using the `sudo` utility.</span></span> <span data-ttu-id="e601c-114">Po přihlášení pomocí tohoto uživatelského účtu, budete moci spustit příkazy jako kořenová pomocí syntaxe příkazu:</span><span class="sxs-lookup"><span data-stu-id="e601c-114">Once logged in using this user account, you will be able to run commands as root using the command syntax:</span></span>

    # sudo <COMMAND>

<span data-ttu-id="e601c-115">Volitelně můžete získat pomocí prostředí kořenové **sudo -s**.</span><span class="sxs-lookup"><span data-stu-id="e601c-115">You can optionally obtain a root shell using **sudo -s**.</span></span>

* <span data-ttu-id="e601c-116">V tématu [pomocí oprávnění kořenového na virtuální počítače s Linuxem v Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e601c-116">See [Using root privileges on Linux virtual machines in Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="firewall-configuration"></a><span data-ttu-id="e601c-117">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="e601c-117">Firewall Configuration</span></span>
<span data-ttu-id="e601c-118">Azure poskytuje filtr příchozího paketu, který omezuje připojení k portům zadaný na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e601c-118">Azure provides an inbound packet filter that restricts connectivity to ports specified in the Azure portal.</span></span> <span data-ttu-id="e601c-119">Ve výchozím nastavení je jenom povolených port SSH.</span><span class="sxs-lookup"><span data-stu-id="e601c-119">By default, the only allowed port is SSH.</span></span> <span data-ttu-id="e601c-120">Přístup k další porty na virtuální počítač Linux vám může otevře nakonfigurováním koncových bodů na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="e601c-120">You may open up access to additional ports on your Linux virtual machine by configuring endpoints in the Azure portal:</span></span>

* <span data-ttu-id="e601c-121">Přejděte na téma: [jak nastavit koncové body k virtuálnímu počítači](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e601c-121">See: [How to Set Up Endpoints to a Virtual Machine](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="e601c-122">Nepovolujte Linux obrázků v galerii Azure *iptables* brány firewall ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e601c-122">The Linux images in the Azure Gallery do not enable the *iptables* firewall by default.</span></span> <span data-ttu-id="e601c-123">V případě potřeby lze nakonfigurovat bránu firewall k poskytnutí další filtrování.</span><span class="sxs-lookup"><span data-stu-id="e601c-123">If desired, the firewall may be configured to provide additional filtering.</span></span>

## <a name="hostname-changes"></a><span data-ttu-id="e601c-124">Název hostitele změny</span><span class="sxs-lookup"><span data-stu-id="e601c-124">Hostname Changes</span></span>
<span data-ttu-id="e601c-125">Když nasadíte původně instanci bitovou kopii systému Linux, musíte zadat název hostitele pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e601c-125">When you initially deploy an instance of a Linux image, you are required to provide a host name for the virtual machine.</span></span> <span data-ttu-id="e601c-126">Jakmile je virtuální počítač spuštěn, tento název hostitele je publikována na servery DNS platformy, aby více virtuálních počítačů, které jsou vzájemně propojené můžete provádět vyhledávání IP adresy pomocí názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="e601c-126">Once the virtual machine is running, this hostname is published to the platform DNS servers so that multiple virtual machines connected to each other can perform IP address lookups using hostnames.</span></span>

<span data-ttu-id="e601c-127">Pokud název hostitele změny se požaduje po nasazení virtuálního počítače, použijte příkaz</span><span class="sxs-lookup"><span data-stu-id="e601c-127">If hostname changes are desired after a virtual machine has been deployed, please use the command</span></span>

    # sudo hostname <newname>

<span data-ttu-id="e601c-128">Azure Linux Agent zahrnuje funkci automaticky zjišťovat tato změna názvu a odpovídajícím způsobem konfigurace virtuálního počítače zachovat tuto změnu a publikovat tuto změnu na servery DNS platformy.</span><span class="sxs-lookup"><span data-stu-id="e601c-128">The Azure Linux Agent includes functionality to automatically detect this name change and appropriately configure the virtual machine to persist this change and publish this change to the platform DNS servers.</span></span>

* [<span data-ttu-id="e601c-129">Uživatelská příručka k Azure Linux Agent</span><span class="sxs-lookup"><span data-stu-id="e601c-129">Azure Linux Agent User Guide</span></span>](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a><span data-ttu-id="e601c-130">Init cloudu</span><span class="sxs-lookup"><span data-stu-id="e601c-130">Cloud-Init</span></span>
<span data-ttu-id="e601c-131">**Ubuntu** a **CoreOS** bitové kopie využívat cloudové init na Azure, které poskytuje další funkce pro zavádění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e601c-131">**Ubuntu** and **CoreOS** images utilize cloud-init on Azure, which provides additional capabilities for bootstrapping a virtual machine.</span></span>

* [<span data-ttu-id="e601c-132">Postup vložení vlastních dat</span><span class="sxs-lookup"><span data-stu-id="e601c-132">How to Inject Custom Data</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="e601c-133">Vlastní Data a inicializací cloudu na platformě Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e601c-133">Custom Data and Cloud-Init on Microsoft Azure</span></span>](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [<span data-ttu-id="e601c-134">Vytvoření Azure Swap oddíly používající Init cloudu</span><span class="sxs-lookup"><span data-stu-id="e601c-134">Create Azure Swap Partitions Using Cloud-Init</span></span>](https://wiki.ubuntu.com/AzureSwapPartitions)
* [<span data-ttu-id="e601c-135">Jak používat CoreOS v Azure</span><span class="sxs-lookup"><span data-stu-id="e601c-135">How to Use CoreOS on Azure</span></span>](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a><span data-ttu-id="e601c-136">Zachycení Image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e601c-136">Virtual Machine Image Capture</span></span>
<span data-ttu-id="e601c-137">Azure umožňuje zaznamenat stav z existujícího virtuálního počítače do bitové kopie, které lze následně použít k nasazení virtuálního počítače další instance.</span><span class="sxs-lookup"><span data-stu-id="e601c-137">Azure provides the ability to capture the state of an existing virtual machine into an image that can subsequently be used to deploy additional virtual machine instances.</span></span> <span data-ttu-id="e601c-138">Může být Azure Linux Agent používá k vrácení zpět některé přizpůsobení, která byla provedena během procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="e601c-138">The Azure Linux Agent may be used to rollback some of the customization that was performed during the provisioning process.</span></span> <span data-ttu-id="e601c-139">Může postupujte podle následujících kroků a zachytit virtuální počítač jako obrázek:</span><span class="sxs-lookup"><span data-stu-id="e601c-139">You may follow the steps below to capture a virtual machine as an image:</span></span>

1. <span data-ttu-id="e601c-140">Spustit **příkaz waagent-deprovision** zrušit zřizování přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="e601c-140">Run **waagent -deprovision** to undo provisioning customization.</span></span> <span data-ttu-id="e601c-141">Nebo **příkaz waagent-deprovision + uživatele** volitelně odstranit uživatelský účet zadaný během zřizování a všechna přidružená data.</span><span class="sxs-lookup"><span data-stu-id="e601c-141">Or **waagent -deprovision+user** to optionally delete the user account specified during provisioning and all associated data.</span></span>
2. <span data-ttu-id="e601c-142">Vypnout nižší nebo vypnutí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e601c-142">Shut down/power off the virtual machine.</span></span>
3. <span data-ttu-id="e601c-143">Klikněte na tlačítko **zaznamenat** v portálu Azure nebo pomocí prostředí PowerShell nebo rozhraní příkazového řádku nástroje zachytit virtuální počítač jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="e601c-143">Click **Capture** in the Azure portal or use the PowerShell or CLI tools to capture the virtual machine as an image.</span></span>
   
   * <span data-ttu-id="e601c-144">Přejděte na téma: [jak zachytit Linuxový virtuální počítač pro použití jako šablonu](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e601c-144">See: [How to Capture a Linux Virtual Machine to Use as a Template](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

## <a name="attaching-disks"></a><span data-ttu-id="e601c-145">Připojení disků</span><span class="sxs-lookup"><span data-stu-id="e601c-145">Attaching Disks</span></span>
<span data-ttu-id="e601c-146">Každý virtuální počítač má dočasného, místní *prostředků disku* připojen.</span><span class="sxs-lookup"><span data-stu-id="e601c-146">Each virtual machine has a temporary, local *resource disk* attached.</span></span> <span data-ttu-id="e601c-147">Protože data na prostředku disku nesmí být trvanlivý po restartování počítače, se často používá aplikace a procesů spuštěných ve virtuálním počítači pro přechodná a **dočasné** úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="e601c-147">Because data on a resource disk may not be durable across reboots, it is often used by applications and processes running in the virtual machine for transient and **temporary** storage of data.</span></span> <span data-ttu-id="e601c-148">Slouží také k ukládání stránky nebo Prohodit soubory operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e601c-148">It is also used to store the page or swap files for the operating system.</span></span>

<span data-ttu-id="e601c-149">V systému Linux, je prostředek disku obvykle spravuje Azure Linux Agent a automaticky připojit k **/mnt nebo prostředků** (nebo **/mnt** Ubuntu Image).</span><span class="sxs-lookup"><span data-stu-id="e601c-149">On Linux, the resource disk is typically managed by the Azure Linux Agent and automatically mounted to **/mnt/resource** (or **/mnt** on Ubuntu images).</span></span>

> [!NOTE]
> <span data-ttu-id="e601c-150">Všimněte si, že je disk prostředků **dočasné** na disku a může se odstraní a naformátována, když je virtuální počítač restartovat.</span><span class="sxs-lookup"><span data-stu-id="e601c-150">Note that the resource disk is a **temporary** disk, and might be deleted and reformatted when the VM is rebooted.</span></span>
> 
> 

<span data-ttu-id="e601c-151">V systému Linux datový disk může být s názvem jádrem jako `/dev/sdc`, a uživatelé budou potřebovat k oddílu, formátování a připojte tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="e601c-151">On Linux the data disk might be named by the kernel as `/dev/sdc`, and users will need to partition, format and mount that resource.</span></span> <span data-ttu-id="e601c-152">To je popsané v tomto kurzu krok za krokem: [jak připojit datový Disk k virtuálnímu počítači](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e601c-152">This is covered step-by-step in the tutorial: [How to Attach a Data Disk to a Virtual Machine](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

* <span data-ttu-id="e601c-153">**Viz také:** [konfigurace softwaru diskového pole RAID v systému Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [konfigurace LVM na virtuální počítač s Linuxem v Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e601c-153">**See also:** [Configure Software RAID on Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [Configure LVM on a Linux VM in Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

