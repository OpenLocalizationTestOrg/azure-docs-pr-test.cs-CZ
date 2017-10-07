---
title: aaaIntroduction tooLinux v Azure | Microsoft Docs
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
ms.openlocfilehash: 3a931447ee23ce7000174ca314c3e10abc6b8e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toolinux-on-azure"></a><span data-ttu-id="62bd2-103">Úvod tooLinux v Azure</span><span class="sxs-lookup"><span data-stu-id="62bd2-103">Introduction tooLinux on Azure</span></span>
<span data-ttu-id="62bd2-104">Toto téma obsahuje přehled některých aspektů pomocí virtuální počítače s Linuxem v hello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="62bd2-104">This topic provides an overview of some aspects of using Linux virtual machines in hello Azure cloud.</span></span> <span data-ttu-id="62bd2-105">Virtuální počítač s Linuxem nasazení je jednoduchý proces pomocí bitovou kopii z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="62bd2-105">Deploying a Linux virtual machine is a straightforward process using an image from hello gallery.</span></span>

## <a name="authentication-usernames-passwords-and-ssh-keys"></a><span data-ttu-id="62bd2-106">Ověřování: Uživatelská jména, hesla a klíče SSH</span><span class="sxs-lookup"><span data-stu-id="62bd2-106">Authentication: Usernames, Passwords and SSH Keys</span></span>
<span data-ttu-id="62bd2-107">Při vytváření virtuální počítač s Linuxem pomocí hello portál Azure, se zobrazí výzva tooprovide buď uživatelské jméno a heslo nebo veřejný klíč SSH.</span><span class="sxs-lookup"><span data-stu-id="62bd2-107">When creating a Linux virtual machine using hello Azure portal, you are asked tooprovide a either username and password or an SSH public key.</span></span> <span data-ttu-id="62bd2-108">Hello volba uživatelského jména pro nasazení virtuální počítač s Linuxem v Azure je subjektu toohello následující omezení: názvy účtů systému (UID < 100) již existuje v hello virtuálního počítače nejsou povoleny, "kořenový" třeba.</span><span class="sxs-lookup"><span data-stu-id="62bd2-108">hello choice of a username for deploying a Linux virtual machine on Azure is subject toohello following constraint: names of system accounts (UID <100) already present in hello virtual machine are not allowed, 'root' for example.</span></span>

* <span data-ttu-id="62bd2-109">V tématu [vytvoření virtuálního počítače se systémem Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="62bd2-109">See [Create a Virtual Machine Running Linux](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>
* <span data-ttu-id="62bd2-110">V tématu [jak tooUse SSH s Linuxem v Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="62bd2-110">See [How tooUse SSH with Linux on Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="obtaining-superuser-privileges-using-sudo"></a><span data-ttu-id="62bd2-111">Získání oprávnění superuživatele pomocí`sudo`</span><span class="sxs-lookup"><span data-stu-id="62bd2-111">Obtaining Superuser Privileges Using `sudo`</span></span>
<span data-ttu-id="62bd2-112">Hello uživatelský účet, který je zadán během nasazení instance virtuálního počítače na platformě Azure je privilegovaný účet.</span><span class="sxs-lookup"><span data-stu-id="62bd2-112">hello user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="62bd2-113">Tento účet je nakonfiguroval hello Azure Linux Agent toobe možné tooelevate oprávnění tooroot (superuživatel účtu) pomocí hello `sudo` nástroj.</span><span class="sxs-lookup"><span data-stu-id="62bd2-113">This account is configured by hello Azure Linux Agent toobe able tooelevate privileges tooroot (superuser account) using hello `sudo` utility.</span></span> <span data-ttu-id="62bd2-114">Po přihlášení pomocí tohoto uživatelského účtu, bude možné toorun příkazy jako kořenová pomocí syntaxe příkazu hello:</span><span class="sxs-lookup"><span data-stu-id="62bd2-114">Once logged in using this user account, you will be able toorun commands as root using hello command syntax:</span></span>

    # sudo <COMMAND>

<span data-ttu-id="62bd2-115">Volitelně můžete získat pomocí prostředí kořenové **sudo -s**.</span><span class="sxs-lookup"><span data-stu-id="62bd2-115">You can optionally obtain a root shell using **sudo -s**.</span></span>

* <span data-ttu-id="62bd2-116">V tématu [pomocí oprávnění kořenového na virtuální počítače s Linuxem v Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="62bd2-116">See [Using root privileges on Linux virtual machines in Azure](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="firewall-configuration"></a><span data-ttu-id="62bd2-117">Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="62bd2-117">Firewall Configuration</span></span>
<span data-ttu-id="62bd2-118">Azure poskytuje filtr příchozího paketu, který omezuje tooports připojení zadaný v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="62bd2-118">Azure provides an inbound packet filter that restricts connectivity tooports specified in hello Azure portal.</span></span> <span data-ttu-id="62bd2-119">Ve výchozím nastavení, hello pouze povolené port je SSH.</span><span class="sxs-lookup"><span data-stu-id="62bd2-119">By default, hello only allowed port is SSH.</span></span> <span data-ttu-id="62bd2-120">Může dojít k otevření až přístupové porty tooadditional na virtuální počítač Linux nakonfigurováním koncové body v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="62bd2-120">You may open up access tooadditional ports on your Linux virtual machine by configuring endpoints in hello Azure portal:</span></span>

* <span data-ttu-id="62bd2-121">Přejděte na téma: [jak tooSet až koncové body tooa virtuálního počítače](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="62bd2-121">See: [How tooSet Up Endpoints tooa Virtual Machine](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="62bd2-122">Hello Linux obrázků v Azure Gallery hello nepovolujte hello *iptables* brány firewall ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="62bd2-122">hello Linux images in hello Azure Gallery do not enable hello *iptables* firewall by default.</span></span> <span data-ttu-id="62bd2-123">V případě potřeby lze nakonfigurovat bránu firewall hello tooprovide další filtrování.</span><span class="sxs-lookup"><span data-stu-id="62bd2-123">If desired, hello firewall may be configured tooprovide additional filtering.</span></span>

## <a name="hostname-changes"></a><span data-ttu-id="62bd2-124">Název hostitele změny</span><span class="sxs-lookup"><span data-stu-id="62bd2-124">Hostname Changes</span></span>
<span data-ttu-id="62bd2-125">Když nasadíte původně instanci bitovou kopii systému Linux, jste požadované tooprovide název hostitele pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="62bd2-125">When you initially deploy an instance of a Linux image, you are required tooprovide a host name for hello virtual machine.</span></span> <span data-ttu-id="62bd2-126">Po hello virtuální počítač spuštěn, je tento název hostitele serverů DNS publikované toohello platformy, tak, aby více virtuálních počítačů připojený tooeach jiné můžete provádět vyhledávání IP adresy pomocí názvy hostitelů.</span><span class="sxs-lookup"><span data-stu-id="62bd2-126">Once hello virtual machine is running, this hostname is published toohello platform DNS servers so that multiple virtual machines connected tooeach other can perform IP address lookups using hostnames.</span></span>

<span data-ttu-id="62bd2-127">Pokud název hostitele změny se požaduje po nasazení virtuálního počítače, použijte příkaz hello</span><span class="sxs-lookup"><span data-stu-id="62bd2-127">If hostname changes are desired after a virtual machine has been deployed, please use hello command</span></span>

    # sudo hostname <newname>

<span data-ttu-id="62bd2-128">Hello Azure Linux Agent zahrnuje funkci tooautomatically, zjištění této změny názvu a odpovídajícím způsobem konfigurace toopersist hello virtuální počítač tato změna a publikování serverů DNS platformy toohello tuto změnu.</span><span class="sxs-lookup"><span data-stu-id="62bd2-128">hello Azure Linux Agent includes functionality tooautomatically detect this name change and appropriately configure hello virtual machine toopersist this change and publish this change toohello platform DNS servers.</span></span>

* [<span data-ttu-id="62bd2-129">Uživatelská příručka k Azure Linux Agent</span><span class="sxs-lookup"><span data-stu-id="62bd2-129">Azure Linux Agent User Guide</span></span>](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a><span data-ttu-id="62bd2-130">Init cloudu</span><span class="sxs-lookup"><span data-stu-id="62bd2-130">Cloud-Init</span></span>
<span data-ttu-id="62bd2-131">**Ubuntu** a **CoreOS** bitové kopie využívat cloudové init na Azure, které poskytuje další funkce pro zavádění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="62bd2-131">**Ubuntu** and **CoreOS** images utilize cloud-init on Azure, which provides additional capabilities for bootstrapping a virtual machine.</span></span>

* [<span data-ttu-id="62bd2-132">Jak tooInject vlastní Data</span><span class="sxs-lookup"><span data-stu-id="62bd2-132">How tooInject Custom Data</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="62bd2-133">Vlastní Data a inicializací cloudu na platformě Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="62bd2-133">Custom Data and Cloud-Init on Microsoft Azure</span></span>](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [<span data-ttu-id="62bd2-134">Vytvoření Azure Swap oddíly používající Init cloudu</span><span class="sxs-lookup"><span data-stu-id="62bd2-134">Create Azure Swap Partitions Using Cloud-Init</span></span>](https://wiki.ubuntu.com/AzureSwapPartitions)
* [<span data-ttu-id="62bd2-135">Jak tooUse CoreOS v Azure</span><span class="sxs-lookup"><span data-stu-id="62bd2-135">How tooUse CoreOS on Azure</span></span>](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a><span data-ttu-id="62bd2-136">Zachycení Image virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="62bd2-136">Virtual Machine Image Capture</span></span>
<span data-ttu-id="62bd2-137">Azure poskytuje možnost hello toocapture hello stav existující virtuální počítač do bitové kopie, kterou lze následně instance používané toodeploy dalších virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="62bd2-137">Azure provides hello ability toocapture hello state of an existing virtual machine into an image that can subsequently be used toodeploy additional virtual machine instances.</span></span> <span data-ttu-id="62bd2-138">Hello Azure Linux Agent může být použité toorollback některé hello přizpůsobení, která byla provedena během procesu zřizování hello.</span><span class="sxs-lookup"><span data-stu-id="62bd2-138">hello Azure Linux Agent may be used toorollback some of hello customization that was performed during hello provisioning process.</span></span> <span data-ttu-id="62bd2-139">Hello kroků toocapture virtuální počítač může postupujte jako obrázek:</span><span class="sxs-lookup"><span data-stu-id="62bd2-139">You may follow hello steps below toocapture a virtual machine as an image:</span></span>

1. <span data-ttu-id="62bd2-140">Spustit **příkaz waagent-deprovision** tooundo zřizování přizpůsobení.</span><span class="sxs-lookup"><span data-stu-id="62bd2-140">Run **waagent -deprovision** tooundo provisioning customization.</span></span> <span data-ttu-id="62bd2-141">Nebo **příkaz waagent-deprovision + uživatele** toooptionally odstranit hello uživatelský účet zadaný během zřizování a všechna přidružená data.</span><span class="sxs-lookup"><span data-stu-id="62bd2-141">Or **waagent -deprovision+user** toooptionally delete hello user account specified during provisioning and all associated data.</span></span>
2. <span data-ttu-id="62bd2-142">Vypnout nižší nebo vypnutí hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="62bd2-142">Shut down/power off hello virtual machine.</span></span>
3. <span data-ttu-id="62bd2-143">Klikněte na tlačítko **zaznamenat** v hello Azure hello portálu nebo pomocí prostředí PowerShell nebo rozhraní příkazového řádku nástroje toocapture hello virtuálního počítače jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="62bd2-143">Click **Capture** in hello Azure portal or use hello PowerShell or CLI tools toocapture hello virtual machine as an image.</span></span>
   
   * <span data-ttu-id="62bd2-144">Přejděte na téma: [jak tooCapture tooUse Linuxový virtuální počítač jako šablonu](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="62bd2-144">See: [How tooCapture a Linux Virtual Machine tooUse as a Template](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

## <a name="attaching-disks"></a><span data-ttu-id="62bd2-145">Připojení disků</span><span class="sxs-lookup"><span data-stu-id="62bd2-145">Attaching Disks</span></span>
<span data-ttu-id="62bd2-146">Každý virtuální počítač má dočasného, místní *prostředků disku* připojen.</span><span class="sxs-lookup"><span data-stu-id="62bd2-146">Each virtual machine has a temporary, local *resource disk* attached.</span></span> <span data-ttu-id="62bd2-147">Protože data na prostředku disku nesmí být trvanlivý po restartování počítače, se často používá aplikace a procesů spuštěných ve virtuálním počítači hello přechodná a **dočasné** úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="62bd2-147">Because data on a resource disk may not be durable across reboots, it is often used by applications and processes running in hello virtual machine for transient and **temporary** storage of data.</span></span> <span data-ttu-id="62bd2-148">Je také hello použité toostore stránky nebo swap soubory hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="62bd2-148">It is also used toostore hello page or swap files for hello operating system.</span></span>

<span data-ttu-id="62bd2-149">V systému Linux, je obvykle spravuje hello Azure Linux Agent a automaticky připojit příliš hello prostředků disku**/mnt nebo prostředků** (nebo **/mnt** Ubuntu Image).</span><span class="sxs-lookup"><span data-stu-id="62bd2-149">On Linux, hello resource disk is typically managed by hello Azure Linux Agent and automatically mounted too**/mnt/resource** (or **/mnt** on Ubuntu images).</span></span>

> [!NOTE]
> <span data-ttu-id="62bd2-150">Všimněte si, je tento disk hello prostředků **dočasné** na disku a může se odstraní a naformátována při hello virtuálních počítačů po restartu.</span><span class="sxs-lookup"><span data-stu-id="62bd2-150">Note that hello resource disk is a **temporary** disk, and might be deleted and reformatted when hello VM is rebooted.</span></span>
> 
> 

<span data-ttu-id="62bd2-151">V systému Linux hello datový disk může být s názvem podle hello jádra jako `/dev/sdc`, a uživatelé budou potřebovat toopartition, formátování a připojte tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="62bd2-151">On Linux hello data disk might be named by hello kernel as `/dev/sdc`, and users will need toopartition, format and mount that resource.</span></span> <span data-ttu-id="62bd2-152">To je zahrnutý v kurzu hello krok za krokem: [jak tooAttach tooa datový Disk virtuálního počítače](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="62bd2-152">This is covered step-by-step in hello tutorial: [How tooAttach a Data Disk tooa Virtual Machine](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

* <span data-ttu-id="62bd2-153">**Viz také:** [konfigurace softwaru diskového pole RAID v systému Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [konfigurace LVM na virtuální počítač s Linuxem v Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="62bd2-153">**See also:** [Configure Software RAID on Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [Configure LVM on a Linux VM in Azure](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

