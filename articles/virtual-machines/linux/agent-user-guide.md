---
title: "Virtuální počítač Azure Linux Agent přehled | Microsoft Docs"
description: "Informace o instalaci a konfiguraci agenta systému Linux (příkaz waagent) ke správě virtuálního počítače interakci s Kontroleru prostředků infrastruktury Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: szark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 486ad6bb148583a957fb82b7954ff94f853b12cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-and-using-the-azure-linux-agent"></a><span data-ttu-id="b68e6-103">Informace o používání Azure Linux Agent</span><span class="sxs-lookup"><span data-stu-id="b68e6-103">Understanding and using the Azure Linux Agent</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a><span data-ttu-id="b68e6-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="b68e6-104">Introduction</span></span>
<span data-ttu-id="b68e6-105">Microsoft Azure Linux Agent (příkaz waagent) spravuje Linux & FreeBSD zřizování a virtuálních počítačů interakci s Kontroleru prostředků infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="b68e6-105">The Microsoft Azure Linux Agent (waagent) manages Linux & FreeBSD provisioning, and VM interaction with the Azure Fabric Controller.</span></span> <span data-ttu-id="b68e6-106">Poskytuje následující funkce pro systémy Linux a FreeBSD IaaS nasazení:</span><span class="sxs-lookup"><span data-stu-id="b68e6-106">It provides the following functionality for Linux and FreeBSD IaaS deployments:</span></span>

> [!NOTE]
> <span data-ttu-id="b68e6-107">Viz Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="b68e6-107">See the Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) for additional details.</span></span>
> 
> 

* <span data-ttu-id="b68e6-108">**Zřizování bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="b68e6-108">**Image Provisioning**</span></span>
  
  * <span data-ttu-id="b68e6-109">Vytvoření uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="b68e6-109">Creation of a user account</span></span>
  * <span data-ttu-id="b68e6-110">Konfigurace typů ověřování SSH</span><span class="sxs-lookup"><span data-stu-id="b68e6-110">Configuring SSH authentication types</span></span>
  * <span data-ttu-id="b68e6-111">Nasazení veřejné klíče SSH a páry klíčů</span><span class="sxs-lookup"><span data-stu-id="b68e6-111">Deployment of SSH public keys and key pairs</span></span>
  * <span data-ttu-id="b68e6-112">Nastavení názvu hostitele</span><span class="sxs-lookup"><span data-stu-id="b68e6-112">Setting the host name</span></span>
  * <span data-ttu-id="b68e6-113">Publikování název hostitele pro platformu DNS</span><span class="sxs-lookup"><span data-stu-id="b68e6-113">Publishing the host name to the platform DNS</span></span>
  * <span data-ttu-id="b68e6-114">Vytváření sestav otisk klíče SSH hostitele pro platformu</span><span class="sxs-lookup"><span data-stu-id="b68e6-114">Reporting SSH host key fingerprint to the platform</span></span>
  * <span data-ttu-id="b68e6-115">Správa prostředků disku</span><span class="sxs-lookup"><span data-stu-id="b68e6-115">Resource Disk Management</span></span>
  * <span data-ttu-id="b68e6-116">Formátování a připojování prostředků disku</span><span class="sxs-lookup"><span data-stu-id="b68e6-116">Formatting and mounting the resource disk</span></span>
  * <span data-ttu-id="b68e6-117">Konfigurace velikosti odkládacího souboru</span><span class="sxs-lookup"><span data-stu-id="b68e6-117">Configuring swap space</span></span>
* <span data-ttu-id="b68e6-118">**Sítě**</span><span class="sxs-lookup"><span data-stu-id="b68e6-118">**Networking**</span></span>
  
  * <span data-ttu-id="b68e6-119">Spravuje trasy zlepšit kompatibilitu s servery DHCP platformy</span><span class="sxs-lookup"><span data-stu-id="b68e6-119">Manages routes to improve compatibility with platform DHCP servers</span></span>
  * <span data-ttu-id="b68e6-120">Zajišťuje stabilitu název síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="b68e6-120">Ensures the stability of the network interface name</span></span>
* <span data-ttu-id="b68e6-121">**Jádra**</span><span class="sxs-lookup"><span data-stu-id="b68e6-121">**Kernel**</span></span>
  
  * <span data-ttu-id="b68e6-122">Nakonfiguruje virtuální technologie NUMA (zakázat jádra < 2.6.37)</span><span class="sxs-lookup"><span data-stu-id="b68e6-122">Configures virtual NUMA (disable for kernel <2.6.37)</span></span>
  * <span data-ttu-id="b68e6-123">Využívá šifrování technologie Hyper-V pro /dev/random</span><span class="sxs-lookup"><span data-stu-id="b68e6-123">Consumes Hyper-V entropy for /dev/random</span></span>
  * <span data-ttu-id="b68e6-124">Nakonfiguruje SCSI vypršení časových limitů pro kořenové zařízení (který může být vzdálený)</span><span class="sxs-lookup"><span data-stu-id="b68e6-124">Configures SCSI timeouts for the root device (which could be remote)</span></span>
* <span data-ttu-id="b68e6-125">**Diagnostika**</span><span class="sxs-lookup"><span data-stu-id="b68e6-125">**Diagnostics**</span></span>
  
  * <span data-ttu-id="b68e6-126">Přesměrování konzoly sériového portu</span><span class="sxs-lookup"><span data-stu-id="b68e6-126">Console redirection to the serial port</span></span>
* <span data-ttu-id="b68e6-127">**Nasazení SCVMM**</span><span class="sxs-lookup"><span data-stu-id="b68e6-127">**SCVMM Deployments**</span></span>
  
  * <span data-ttu-id="b68e6-128">Zjišťuje a bootstraps agenta nástroje VMM pro Linux v prostředí System Center Virtual Machine Manager 2012 R2</span><span class="sxs-lookup"><span data-stu-id="b68e6-128">Detects and bootstraps the VMM agent for Linux when running in a System Center Virtual Machine Manager 2012 R2 environment</span></span>
* <span data-ttu-id="b68e6-129">**Rozšíření virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="b68e6-129">**VM Extension**</span></span>
  
  * <span data-ttu-id="b68e6-130">Vložit součásti autorem je do virtuálního počítače s Linuxem (IaaS) Chcete-li povolit softwaru a konfigurace automatizace Microsoftu a partnerů.</span><span class="sxs-lookup"><span data-stu-id="b68e6-130">Inject component authored by Microsoft and Partners into Linux VM (IaaS) to enable software and configuration automation</span></span>
  * <span data-ttu-id="b68e6-131">Odkaz na implementaci rozšíření virtuálního počítače na [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span><span class="sxs-lookup"><span data-stu-id="b68e6-131">VM Extension reference implementation on [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span></span>

## <a name="communication"></a><span data-ttu-id="b68e6-132">Komunikace</span><span class="sxs-lookup"><span data-stu-id="b68e6-132">Communication</span></span>
<span data-ttu-id="b68e6-133">Informace o toku od platformy agenta dojde k prostřednictvím dva kanály:</span><span class="sxs-lookup"><span data-stu-id="b68e6-133">The information flow from the platform to the agent occurs via two channels:</span></span>

* <span data-ttu-id="b68e6-134">Při spouštění počítače připojit DVD pro nasazení IaaS.</span><span class="sxs-lookup"><span data-stu-id="b68e6-134">A boot-time attached DVD for IaaS deployments.</span></span> <span data-ttu-id="b68e6-135">Tento disk DVD zahrnuje kompatibilní se standardem OVF konfigurační soubor, který obsahuje všechny informace o zřizování než skutečná keypairs SSH.</span><span class="sxs-lookup"><span data-stu-id="b68e6-135">This DVD includes an OVF-compliant configuration file that includes all provisioning information other than the actual SSH keypairs.</span></span>
* <span data-ttu-id="b68e6-136">Koncový bod TCP vystavení rozhraní REST API použít k získání nasazení a konfiguraci topologie.</span><span class="sxs-lookup"><span data-stu-id="b68e6-136">A TCP endpoint exposing a REST API used to obtain deployment and topology configuration.</span></span>

## <a name="requirements"></a><span data-ttu-id="b68e6-137">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b68e6-137">Requirements</span></span>
<span data-ttu-id="b68e6-138">Tyto systémy byly testovány a jsou známé pro práci s Azure Linux Agent:</span><span class="sxs-lookup"><span data-stu-id="b68e6-138">The following systems have been tested and are known to work with the Azure Linux Agent:</span></span>

> [!NOTE]
> <span data-ttu-id="b68e6-139">Tento seznam se můžou lišit od oficiálního seznam podporovaných systémů na platformě Microsoft Azure podle postupu popsaného tady: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span><span class="sxs-lookup"><span data-stu-id="b68e6-139">This list may differ from the official list of supported systems on the Microsoft Azure Platform, as described here: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span></span>
> 
> 

* <span data-ttu-id="b68e6-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="b68e6-140">CoreOS</span></span>
* <span data-ttu-id="b68e6-141">CentOS 6.3 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-141">CentOS 6.3+</span></span>
* <span data-ttu-id="b68e6-142">Red Hat Enterprise Linux 6.7 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-142">Red Hat Enterprise Linux 6.7+</span></span>
* <span data-ttu-id="b68e6-143">Debian 7.0 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-143">Debian 7.0+</span></span>
* <span data-ttu-id="b68e6-144">Ubuntu 12.04 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-144">Ubuntu 12.04+</span></span>
* <span data-ttu-id="b68e6-145">openSUSE 12.3 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-145">openSUSE 12.3+</span></span>
* <span data-ttu-id="b68e6-146">SLES 11 SP3 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-146">SLES 11 SP3+</span></span>
* <span data-ttu-id="b68e6-147">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-147">Oracle Linux 6.4+</span></span>

<span data-ttu-id="b68e6-148">Jiné podporované systémy:</span><span class="sxs-lookup"><span data-stu-id="b68e6-148">Other Supported Systems:</span></span>

* <span data-ttu-id="b68e6-149">FreeBSD 10 + (Azure Linux Agent v2.0.10 +)</span><span class="sxs-lookup"><span data-stu-id="b68e6-149">FreeBSD 10+ (Azure Linux Agent v2.0.10+)</span></span>

<span data-ttu-id="b68e6-150">Agenta systému Linux, aby mohl správně fungovat závisí na některé balíčky systému:</span><span class="sxs-lookup"><span data-stu-id="b68e6-150">The Linux agent depends on some system packages in order to function properly:</span></span>

* <span data-ttu-id="b68e6-151">Python 2.6 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-151">Python 2.6+</span></span>
* <span data-ttu-id="b68e6-152">OpenSSL 1.0 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-152">OpenSSL 1.0+</span></span>
* <span data-ttu-id="b68e6-153">OpenSSH 5.3 +</span><span class="sxs-lookup"><span data-stu-id="b68e6-153">OpenSSH 5.3+</span></span>
* <span data-ttu-id="b68e6-154">Nástroje systému souborů: čtvrceny sfdisk, fdisk, mkfs,</span><span class="sxs-lookup"><span data-stu-id="b68e6-154">Filesystem utilities: sfdisk, fdisk, mkfs, parted</span></span>
* <span data-ttu-id="b68e6-155">Nástroje pro hesla: chpasswd, sudo</span><span class="sxs-lookup"><span data-stu-id="b68e6-155">Password tools: chpasswd, sudo</span></span>
* <span data-ttu-id="b68e6-156">Nástroje pro zpracování textu: menšit grep</span><span class="sxs-lookup"><span data-stu-id="b68e6-156">Text processing tools: sed, grep</span></span>
* <span data-ttu-id="b68e6-157">Síťové nástroje: trasy protokolu ip</span><span class="sxs-lookup"><span data-stu-id="b68e6-157">Network tools: ip-route</span></span>
* <span data-ttu-id="b68e6-158">Podpora jádra pro připojení UDF systémy.</span><span class="sxs-lookup"><span data-stu-id="b68e6-158">Kernel support for mounting UDF filesystems.</span></span>

## <a name="installation"></a><span data-ttu-id="b68e6-159">Instalace</span><span class="sxs-lookup"><span data-stu-id="b68e6-159">Installation</span></span>
<span data-ttu-id="b68e6-160">Instalace pomocí ot. / min nebo bázi DEB balíček z úložiště balíčků vaší distribuce je upřednostňovaný způsob instalace a upgrade Azure Linux Agent.</span><span class="sxs-lookup"><span data-stu-id="b68e6-160">Installation using an RPM or a DEB package from your distribution's package repository is the preferred method of installing and upgrading the Azure Linux Agent.</span></span> <span data-ttu-id="b68e6-161">Všechny [schválené distribuční zprostředkovatelé](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) balíček Azure Linux agent integrovat do svých bitové kopie a úložiště.</span><span class="sxs-lookup"><span data-stu-id="b68e6-161">All the [endorsed distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrate the Azure Linux agent package into their images and repositories.</span></span>

<span data-ttu-id="b68e6-162">V dokumentaci v [Azure Linux Agent úložišti na Githubu](https://github.com/Azure/WALinuxAgent) pro Upřesnit možnosti instalace, například při instalaci ze zdroje nebo vlastní umístění nebo předpony.</span><span class="sxs-lookup"><span data-stu-id="b68e6-162">Refer to the documentation in the [Azure Linux Agent repo on GitHub](https://github.com/Azure/WALinuxAgent) for advanced installation options, such as installing from source or to custom locations or prefixes.</span></span>

## <a name="command-line-options"></a><span data-ttu-id="b68e6-163">Možnosti příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="b68e6-163">Command Line Options</span></span>
### <a name="flags"></a><span data-ttu-id="b68e6-164">Příznaky</span><span class="sxs-lookup"><span data-stu-id="b68e6-164">Flags</span></span>
* <span data-ttu-id="b68e6-165">verbose: zvýšit podrobnost zadaný příkaz</span><span class="sxs-lookup"><span data-stu-id="b68e6-165">verbose: Increase verbosity of specified command</span></span>
* <span data-ttu-id="b68e6-166">Vynutit: přeskočit interaktivní potvrzení pro některé příkazy</span><span class="sxs-lookup"><span data-stu-id="b68e6-166">force: Skip interactive confirmation for some commands</span></span>

### <a name="commands"></a><span data-ttu-id="b68e6-167">Příkazy</span><span class="sxs-lookup"><span data-stu-id="b68e6-167">Commands</span></span>
* <span data-ttu-id="b68e6-168">Nápověda: seznam podporovaných příkazů a příznaky.</span><span class="sxs-lookup"><span data-stu-id="b68e6-168">help: Lists the supported commands and flags.</span></span>
* <span data-ttu-id="b68e6-169">deprovision: Pokus o vyčištění systému a nastavit jej jako vhodný pro zřizování znovu.</span><span class="sxs-lookup"><span data-stu-id="b68e6-169">deprovision: Attempt to clean the system and make it suitable for re-provisioning.</span></span> <span data-ttu-id="b68e6-170">Tuto operaci odstraněny následující:</span><span class="sxs-lookup"><span data-stu-id="b68e6-170">This operation deleted the following:</span></span>
  
  * <span data-ttu-id="b68e6-171">Všechny klíče SSH hostitele (Pokud Provisioning.RegenerateSshHostKeyPair 'y' v konfiguračním souboru)</span><span class="sxs-lookup"><span data-stu-id="b68e6-171">All SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in the configuration file)</span></span>
  * <span data-ttu-id="b68e6-172">Konfigurace názvový server v /etc/resolv.conf</span><span class="sxs-lookup"><span data-stu-id="b68e6-172">Nameserver configuration in /etc/resolv.conf</span></span>
  * <span data-ttu-id="b68e6-173">Kořenové heslo z/etc/shadow (Pokud Provisioning.DeleteRootPassword 'y' v konfiguračním souboru)</span><span class="sxs-lookup"><span data-stu-id="b68e6-173">Root password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in the configuration file)</span></span>
  * <span data-ttu-id="b68e6-174">V mezipaměti klienta zapůjčení DHCP</span><span class="sxs-lookup"><span data-stu-id="b68e6-174">Cached DHCP client leases</span></span>
  * <span data-ttu-id="b68e6-175">Resetuje název hostitele na localhost.localdomain</span><span class="sxs-lookup"><span data-stu-id="b68e6-175">Resets host name to localhost.localdomain</span></span>

> [!WARNING]
> <span data-ttu-id="b68e6-176">Zrušení zřízení nezaručuje, že bitová kopie je nezaškrtnuté všech citlivých informací a je určená pro opětovnou distribuci.</span><span class="sxs-lookup"><span data-stu-id="b68e6-176">Deprovisioning does not guarantee that the image is cleared of all sensitive information and suitable for redistribution.</span></span>
> 
> 

* <span data-ttu-id="b68e6-177">deprovision + uživatele: provede vše pod - deprovision (výše) a taky odstraní poslední účet zřízení uživatele (získaný z /var/lib/waagent) a související data.</span><span class="sxs-lookup"><span data-stu-id="b68e6-177">deprovision+user: Performs everything under -deprovision (above) and also deletes the last provisioned user account (obtained from /var/lib/waagent) and associated data.</span></span> <span data-ttu-id="b68e6-178">Tento parametr je při jeho rušení obrázek, který byl dříve zřizování v Azure, může být zachycen a znovu použít.</span><span class="sxs-lookup"><span data-stu-id="b68e6-178">This parameter is when de-provisioning an image that was previously provisioning on Azure so it may be captured and re-used.</span></span>
* <span data-ttu-id="b68e6-179">verze: Zobrazí verzi příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="b68e6-179">version: Displays the version of waagent</span></span>
* <span data-ttu-id="b68e6-180">serialconsole: nakonfiguruje GRUB označit ttyS0 (první sériového portu) jako spouštěcí konzoly.</span><span class="sxs-lookup"><span data-stu-id="b68e6-180">serialconsole: Configures GRUB to mark ttyS0 (the first serial port) as the boot console.</span></span> <span data-ttu-id="b68e6-181">To zajišťuje, že jsou protokoly spuštění jádra posílají do sériového portu a k dispozici pro ladění.</span><span class="sxs-lookup"><span data-stu-id="b68e6-181">This ensures that kernel bootup logs are sent to the serial port and made available for debugging.</span></span>
* <span data-ttu-id="b68e6-182">Démon: Spusťte příkaz waagent jako démon ke správě interakci s platformou.</span><span class="sxs-lookup"><span data-stu-id="b68e6-182">daemon: Run waagent as a daemon to manage interaction with the platform.</span></span> <span data-ttu-id="b68e6-183">Tento argument je určena k příkaz waagent ve skriptu příkaz waagent init.</span><span class="sxs-lookup"><span data-stu-id="b68e6-183">This argument is specified to waagent in the waagent init script.</span></span>
* <span data-ttu-id="b68e6-184">spustit: Spusťte příkaz waagent jako proces na pozadí</span><span class="sxs-lookup"><span data-stu-id="b68e6-184">start: Run waagent as a background process</span></span>

## <a name="configuration"></a><span data-ttu-id="b68e6-185">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="b68e6-185">Configuration</span></span>
<span data-ttu-id="b68e6-186">Konfigurační soubor (nebo etc/waagent.conf) akce příkaz waagent ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="b68e6-186">A configuration file (/etc/waagent.conf) controls the actions of waagent.</span></span> <span data-ttu-id="b68e6-187">Ukázkový soubor konfigurace je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="b68e6-187">A sample configuration file is shown below:</span></span>

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

<span data-ttu-id="b68e6-188">Možnosti konfigurace jsou podrobně popsány v níže.</span><span class="sxs-lookup"><span data-stu-id="b68e6-188">The various configuration options are described in detail below.</span></span> <span data-ttu-id="b68e6-189">Možnosti konfigurace jsou tři typy; Logická hodnota, řetězec nebo celé číslo.</span><span class="sxs-lookup"><span data-stu-id="b68e6-189">Configuration options are of three types; Boolean, String or Integer.</span></span> <span data-ttu-id="b68e6-190">Možnosti konfigurace Boolean lze zadat jako "y" nebo "n".</span><span class="sxs-lookup"><span data-stu-id="b68e6-190">The Boolean configuration options can be specified as "y" or "n".</span></span> <span data-ttu-id="b68e6-191">Speciální klíčové slovo "Žádný" může být použita pro některé řetězec typ konfigurace položky podle popisu níže.</span><span class="sxs-lookup"><span data-stu-id="b68e6-191">The special keyword "None" may be used for some string type configuration entries as detailed below.</span></span>

<span data-ttu-id="b68e6-192">**Provisioning.Enabled:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-192">**Provisioning.Enabled:**</span></span>  
<span data-ttu-id="b68e6-193">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-193">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-194">Výchozí: y</span><span class="sxs-lookup"><span data-stu-id="b68e6-194">Default: y</span></span>

<span data-ttu-id="b68e6-195">To umožňuje uživatelům povolit nebo zakázat funkci zřizování v agentovi.</span><span class="sxs-lookup"><span data-stu-id="b68e6-195">This allows the user to enable or disable the provisioning functionality in the agent.</span></span> <span data-ttu-id="b68e6-196">Platné hodnoty jsou "y" nebo "n".</span><span class="sxs-lookup"><span data-stu-id="b68e6-196">Valid values are "y" or "n".</span></span> <span data-ttu-id="b68e6-197">Pokud zřizování je zakázaná, hostitele a uživatel klíčů SSH na obrázku jsou zachovány a veškeré zadané ve službě Azure zřizování rozhraní API konfigurace je ignorována.</span><span class="sxs-lookup"><span data-stu-id="b68e6-197">If provisioning is disabled, SSH host and user keys in the image are preserved and any configuration specified in the Azure provisioning API is ignored.</span></span>

> [!NOTE]
> <span data-ttu-id="b68e6-198">`Provisioning.Enabled` Parametr výchozí hodnota je "n" Image Ubuntu cloudu, který použít cloudové init pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="b68e6-198">The `Provisioning.Enabled` parameter defaults to "n" on Ubuntu Cloud Images that use cloud-init for provisioning.</span></span>
> 
> 

<span data-ttu-id="b68e6-199">**Provisioning.DeleteRootPassword:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-199">**Provisioning.DeleteRootPassword:**</span></span>  
<span data-ttu-id="b68e6-200">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-200">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-201">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="b68e6-201">Default: n</span></span>

<span data-ttu-id="b68e6-202">Pokud je sada, kořenové heslo v souboru/etc/stínové vymazat během procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="b68e6-202">If set, the root password in the /etc/shadow file is erased during the provisioning process.</span></span>

<span data-ttu-id="b68e6-203">**Provisioning.RegenerateSshHostKeyPair:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-203">**Provisioning.RegenerateSshHostKeyPair:**</span></span>  
<span data-ttu-id="b68e6-204">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-204">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-205">Výchozí: y</span><span class="sxs-lookup"><span data-stu-id="b68e6-205">Default: y</span></span>

<span data-ttu-id="b68e6-206">Pokud během procesu zřizování z etc/ssh/se odstraní sady, všechny hostitele páry klíčů SSH (ecdsa, dsa a rsa).</span><span class="sxs-lookup"><span data-stu-id="b68e6-206">If set, all SSH host key pairs (ecdsa, dsa and rsa) are deleted during the provisioning process from /etc/ssh/.</span></span> <span data-ttu-id="b68e6-207">A je generována jeden nový pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="b68e6-207">And a single fresh key pair is generated.</span></span>

<span data-ttu-id="b68e6-208">Typ šifrování pro nový pár klíčů lze konfigurovat v položce Provisioning.SshHostKeyPairType.</span><span class="sxs-lookup"><span data-stu-id="b68e6-208">The encryption type for the fresh key pair is configurable by the Provisioning.SshHostKeyPairType entry.</span></span> <span data-ttu-id="b68e6-209">Upozorňujeme, že některé distribuce bude znovu vytvořit páry klíčů SSH pro všechny chybějící typy šifrování při restartování démon procesu SSH (například při restartování).</span><span class="sxs-lookup"><span data-stu-id="b68e6-209">Please note that some distributions will re-create SSH key pairs for any missing encryption types when the SSH daemon is restarted (for example, upon a reboot).</span></span>

<span data-ttu-id="b68e6-210">**Provisioning.SshHostKeyPairType:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-210">**Provisioning.SshHostKeyPairType:**</span></span>  
<span data-ttu-id="b68e6-211">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="b68e6-211">Type: String</span></span>  
<span data-ttu-id="b68e6-212">Výchozí: rsa</span><span class="sxs-lookup"><span data-stu-id="b68e6-212">Default: rsa</span></span>

<span data-ttu-id="b68e6-213">To je možné nastavit na typ algoritmus šifrování, který je podporován proces démon programu SSH na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="b68e6-213">This can be set to an encryption algorithm type that is supported by the SSH daemon on the virtual machine.</span></span> <span data-ttu-id="b68e6-214">Obvykle podporované hodnoty jsou "rsa", "dsa" a "ecdsa".</span><span class="sxs-lookup"><span data-stu-id="b68e6-214">The typically supported values are "rsa", "dsa" and "ecdsa".</span></span> <span data-ttu-id="b68e6-215">Všimněte si, že "putty.exe" v systému Windows nepodporuje "ecdsa".</span><span class="sxs-lookup"><span data-stu-id="b68e6-215">Note that "putty.exe" on Windows does not support "ecdsa".</span></span> <span data-ttu-id="b68e6-216">Ano Pokud máte v úmyslu použít pro připojení k nasazení Linux putty.exe v systému Windows, použijte "rsa" nebo "dsa".</span><span class="sxs-lookup"><span data-stu-id="b68e6-216">So, if you intend to use putty.exe on Windows to connect to a Linux deployment, please use "rsa" or "dsa".</span></span>

<span data-ttu-id="b68e6-217">**Provisioning.MonitorHostName:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-217">**Provisioning.MonitorHostName:**</span></span>  
<span data-ttu-id="b68e6-218">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-218">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-219">Výchozí: y</span><span class="sxs-lookup"><span data-stu-id="b68e6-219">Default: y</span></span>

<span data-ttu-id="b68e6-220">Pokud nastavíte, příkaz waagent bude monitorovat Linux virtuálního počítače pro název hostitele změny (jak vrácené příkazem "název hostitele") a automaticky aktualizovat v bitové kopii, aby odrážely změny konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="b68e6-220">If set, waagent will monitor the Linux virtual machine for hostname changes (as returned by the "hostname" command) and automatically update the networking configuration in the image to reflect the change.</span></span> <span data-ttu-id="b68e6-221">Pro vkládání změnu názvu na servery DNS, sítě restartuje ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="b68e6-221">In order to push the name change to the DNS servers, networking will be restarted in the virtual machine.</span></span> <span data-ttu-id="b68e6-222">To způsobí Stručný postup ke ztrátě připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="b68e6-222">This will result in brief loss of Internet connectivity.</span></span>

<span data-ttu-id="b68e6-223">**Provisioning.DecodeCustomData**</span><span class="sxs-lookup"><span data-stu-id="b68e6-223">**Provisioning.DecodeCustomData**</span></span>  
<span data-ttu-id="b68e6-224">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-224">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-225">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="b68e6-225">Default: n</span></span>

<span data-ttu-id="b68e6-226">Pokud nastavíte, příkaz waagent bude dekódovat CustomData z formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="b68e6-226">If set, waagent will decode CustomData from Base64.</span></span>

<span data-ttu-id="b68e6-227">**Provisioning.ExecuteCustomData**</span><span class="sxs-lookup"><span data-stu-id="b68e6-227">**Provisioning.ExecuteCustomData**</span></span>  
<span data-ttu-id="b68e6-228">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-228">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-229">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="b68e6-229">Default: n</span></span>

<span data-ttu-id="b68e6-230">Pokud nastavíte, příkaz waagent provede CustomData po zřízení.</span><span class="sxs-lookup"><span data-stu-id="b68e6-230">If set, waagent will execute CustomData after provisioning.</span></span>

<span data-ttu-id="b68e6-231">**Provisioning.PasswordCryptId**</span><span class="sxs-lookup"><span data-stu-id="b68e6-231">**Provisioning.PasswordCryptId**</span></span>  
<span data-ttu-id="b68e6-232">Typ: řetězec</span><span class="sxs-lookup"><span data-stu-id="b68e6-232">Type:String</span></span>  
<span data-ttu-id="b68e6-233">Výchozí: 6</span><span class="sxs-lookup"><span data-stu-id="b68e6-233">Default:6</span></span>

<span data-ttu-id="b68e6-234">Algoritmus používaný crypt při generování hodnoty hash hesla.</span><span class="sxs-lookup"><span data-stu-id="b68e6-234">Algorithm used by crypt when generating password hash.</span></span>  
 <span data-ttu-id="b68e6-235">1 - ALGORITMUS MD5</span><span class="sxs-lookup"><span data-stu-id="b68e6-235">1 - MD5</span></span>  
 <span data-ttu-id="b68e6-236">2a - Blowfish</span><span class="sxs-lookup"><span data-stu-id="b68e6-236">2a - Blowfish</span></span>  
 <span data-ttu-id="b68e6-237">5 - SHA-256</span><span class="sxs-lookup"><span data-stu-id="b68e6-237">5 - SHA-256</span></span>  
 <span data-ttu-id="b68e6-238">6 - SHA-512</span><span class="sxs-lookup"><span data-stu-id="b68e6-238">6 - SHA-512</span></span>  

<span data-ttu-id="b68e6-239">**Provisioning.PasswordCryptSaltLength**</span><span class="sxs-lookup"><span data-stu-id="b68e6-239">**Provisioning.PasswordCryptSaltLength**</span></span>  
<span data-ttu-id="b68e6-240">Typ: řetězec</span><span class="sxs-lookup"><span data-stu-id="b68e6-240">Type:String</span></span>  
<span data-ttu-id="b68e6-241">Výchozí: 10</span><span class="sxs-lookup"><span data-stu-id="b68e6-241">Default:10</span></span>

<span data-ttu-id="b68e6-242">Délka náhodných salt používá při generování hodnoty hash hesla.</span><span class="sxs-lookup"><span data-stu-id="b68e6-242">Length of random salt used when generating password hash.</span></span>

<span data-ttu-id="b68e6-243">**ResourceDisk.Format:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-243">**ResourceDisk.Format:**</span></span>  
<span data-ttu-id="b68e6-244">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-244">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-245">Výchozí: y</span><span class="sxs-lookup"><span data-stu-id="b68e6-245">Default: y</span></span>

<span data-ttu-id="b68e6-246">Pokud nastavíte, prostředků disku poskytované platformou bude formátu a připojené pomocí příkaz waagent, pokud je typ systému souborů požadované uživatelem v "ResourceDisk.Filesystem" jakoukoli jinou hodnotu než "ntfs".</span><span class="sxs-lookup"><span data-stu-id="b68e6-246">If set, the resource disk provided by the platform will be formatted and mounted by waagent if the filesystem type requested by the user in "ResourceDisk.Filesystem" is anything other than "ntfs".</span></span> <span data-ttu-id="b68e6-247">Jeden oddíl typu Linux (83) bude k dispozici na disku.</span><span class="sxs-lookup"><span data-stu-id="b68e6-247">A single partition of type Linux (83) will be made available on the disk.</span></span> <span data-ttu-id="b68e6-248">Všimněte si, že tento oddíl nebude naformátovaný, pokud ho můžete úspěšně připojit.</span><span class="sxs-lookup"><span data-stu-id="b68e6-248">Note that this partition will not be formatted if it can be successfully mounted.</span></span>

<span data-ttu-id="b68e6-249">**ResourceDisk.Filesystem:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-249">**ResourceDisk.Filesystem:**</span></span>  
<span data-ttu-id="b68e6-250">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="b68e6-250">Type: String</span></span>  
<span data-ttu-id="b68e6-251">Výchozí: ext4</span><span class="sxs-lookup"><span data-stu-id="b68e6-251">Default: ext4</span></span>

<span data-ttu-id="b68e6-252">Určuje typ systému souborů pro prostředek disku.</span><span class="sxs-lookup"><span data-stu-id="b68e6-252">This specifies the filesystem type for the resource disk.</span></span> <span data-ttu-id="b68e6-253">Podporované hodnoty se liší podle distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="b68e6-253">Supported values vary by Linux distribution.</span></span> <span data-ttu-id="b68e6-254">Pokud je řetězec X, potom mkfs. X by měla být k dispozici na bitovou kopii systému Linux.</span><span class="sxs-lookup"><span data-stu-id="b68e6-254">If the string is X, then mkfs.X should be present on the Linux image.</span></span> <span data-ttu-id="b68e6-255">Bitové kopie SLES 11 by měl obvykle používají 'ext3'.</span><span class="sxs-lookup"><span data-stu-id="b68e6-255">SLES 11 images should typically use 'ext3'.</span></span> <span data-ttu-id="b68e6-256">Obrázky FreeBSD tady by měl použít 'ufs2'.</span><span class="sxs-lookup"><span data-stu-id="b68e6-256">FreeBSD images should use 'ufs2' here.</span></span>

<span data-ttu-id="b68e6-257">**ResourceDisk.MountPoint:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-257">**ResourceDisk.MountPoint:**</span></span>  
<span data-ttu-id="b68e6-258">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="b68e6-258">Type: String</span></span>  
<span data-ttu-id="b68e6-259">Výchozí hodnota: / mnt nebo prostředků</span><span class="sxs-lookup"><span data-stu-id="b68e6-259">Default: /mnt/resource</span></span> 

<span data-ttu-id="b68e6-260">Určuje cestu, kde je prostředek disk připojený.</span><span class="sxs-lookup"><span data-stu-id="b68e6-260">This specifies the path at which the resource disk is mounted.</span></span> <span data-ttu-id="b68e6-261">Všimněte si, že je disk prostředků *dočasné* na disku a může vyprázdněny, když je virtuální počítač zrušit.</span><span class="sxs-lookup"><span data-stu-id="b68e6-261">Note that the resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span>

<span data-ttu-id="b68e6-262">**ResourceDisk.MountOptions**</span><span class="sxs-lookup"><span data-stu-id="b68e6-262">**ResourceDisk.MountOptions**</span></span>  
<span data-ttu-id="b68e6-263">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="b68e6-263">Type: String</span></span>  
<span data-ttu-id="b68e6-264">Výchozí: žádná</span><span class="sxs-lookup"><span data-stu-id="b68e6-264">Default: None</span></span>

<span data-ttu-id="b68e6-265">Určuje možnosti připojení disku má být předán příkazu -o připojení.</span><span class="sxs-lookup"><span data-stu-id="b68e6-265">Specifies disk mount options to be passed to the mount -o command.</span></span> <span data-ttu-id="b68e6-266">Toto je čárkami oddělený seznam hodnot, např.</span><span class="sxs-lookup"><span data-stu-id="b68e6-266">This is a comma separated list of values, ex.</span></span> <span data-ttu-id="b68e6-267">'nodev, nosuid'.</span><span class="sxs-lookup"><span data-stu-id="b68e6-267">'nodev,nosuid'.</span></span> <span data-ttu-id="b68e6-268">V tématu mount(8) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="b68e6-268">See mount(8) for details.</span></span>

<span data-ttu-id="b68e6-269">**ResourceDisk.EnableSwap:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-269">**ResourceDisk.EnableSwap:**</span></span>  
<span data-ttu-id="b68e6-270">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-270">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-271">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="b68e6-271">Default: n</span></span>

<span data-ttu-id="b68e6-272">Pokud nastavení odkládacího souboru (/ swapfile) je vytvořen na prostředku disku a přidat do odkládacího souboru v systému.</span><span class="sxs-lookup"><span data-stu-id="b68e6-272">If set, a swap file (/swapfile) is created on the resource disk and added to the system swap space.</span></span>

<span data-ttu-id="b68e6-273">**ResourceDisk.SwapSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-273">**ResourceDisk.SwapSizeMB:**</span></span>  
<span data-ttu-id="b68e6-274">Typ: celé číslo</span><span class="sxs-lookup"><span data-stu-id="b68e6-274">Type: Integer</span></span>  
<span data-ttu-id="b68e6-275">Výchozí: 0</span><span class="sxs-lookup"><span data-stu-id="b68e6-275">Default: 0</span></span>

<span data-ttu-id="b68e6-276">Velikost odkládacího souboru v megabajtech.</span><span class="sxs-lookup"><span data-stu-id="b68e6-276">The size of the swap file in megabytes.</span></span>

<span data-ttu-id="b68e6-277">**Logs.Verbose:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-277">**Logs.Verbose:**</span></span>  
<span data-ttu-id="b68e6-278">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-278">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-279">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="b68e6-279">Default: n</span></span>

<span data-ttu-id="b68e6-280">Pokud je boosted sady podrobností protokolu.</span><span class="sxs-lookup"><span data-stu-id="b68e6-280">If set, log verbosity is boosted.</span></span> <span data-ttu-id="b68e6-281">Příkaz Waagent /var/log/waagent.log v protokolech a využívá funkce logrotate systému otočení protokoly.</span><span class="sxs-lookup"><span data-stu-id="b68e6-281">Waagent logs to /var/log/waagent.log and leverages the system logrotate functionality to rotate logs.</span></span>

<span data-ttu-id="b68e6-282">**OPERAČNÍ SYSTÉM. EnableRDMA**</span><span class="sxs-lookup"><span data-stu-id="b68e6-282">**OS.EnableRDMA**</span></span>  
<span data-ttu-id="b68e6-283">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="b68e6-283">Type: Boolean</span></span>  
<span data-ttu-id="b68e6-284">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="b68e6-284">Default: n</span></span>

<span data-ttu-id="b68e6-285">Pokud nastavíte, agent se pokusí nainstalovat a pak můžete načíst ovladač jádra RDMA, která odpovídá verzi firmwaru v základní hardware.</span><span class="sxs-lookup"><span data-stu-id="b68e6-285">If set, the agent will attempt to install and then load an RDMA kernel driver that matches the version of the firmware on the underlying hardware.</span></span>

<span data-ttu-id="b68e6-286">**OPERAČNÍ SYSTÉM. RootDeviceScsiTimeout:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-286">**OS.RootDeviceScsiTimeout:**</span></span>  
<span data-ttu-id="b68e6-287">Typ: celé číslo</span><span class="sxs-lookup"><span data-stu-id="b68e6-287">Type: Integer</span></span>  
<span data-ttu-id="b68e6-288">Výchozí: 300</span><span class="sxs-lookup"><span data-stu-id="b68e6-288">Default: 300</span></span>

<span data-ttu-id="b68e6-289">Tím se nakonfiguruje SCSI časový limit v sekundách na disku a datové jednotky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b68e6-289">This configures the SCSI timeout in seconds on the OS disk and data drives.</span></span> <span data-ttu-id="b68e6-290">Pokud není nastavena, systém, které budou použity výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b68e6-290">If not set, the system defaults are used.</span></span>

<span data-ttu-id="b68e6-291">**OPERAČNÍ SYSTÉM. OpensslPath:**</span><span class="sxs-lookup"><span data-stu-id="b68e6-291">**OS.OpensslPath:**</span></span>  
<span data-ttu-id="b68e6-292">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="b68e6-292">Type: String</span></span>  
<span data-ttu-id="b68e6-293">Výchozí: žádná</span><span class="sxs-lookup"><span data-stu-id="b68e6-293">Default: None</span></span>

<span data-ttu-id="b68e6-294">Tímto lze zadat alternativní cestu ke openssl binární pro kryptografické operace.</span><span class="sxs-lookup"><span data-stu-id="b68e6-294">This can be used to specify an alternate path for the openssl binary to use for cryptographic operations.</span></span>

<span data-ttu-id="b68e6-295">**HttpProxy.Host HttpProxy.Port**</span><span class="sxs-lookup"><span data-stu-id="b68e6-295">**HttpProxy.Host, HttpProxy.Port**</span></span>  
<span data-ttu-id="b68e6-296">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="b68e6-296">Type: String</span></span>  
<span data-ttu-id="b68e6-297">Výchozí: žádná</span><span class="sxs-lookup"><span data-stu-id="b68e6-297">Default: None</span></span>

<span data-ttu-id="b68e6-298">Pokud nastavíte, agent použije tento proxy server pro přístup k Internetu.</span><span class="sxs-lookup"><span data-stu-id="b68e6-298">If set, the agent will use this proxy server to access the internet.</span></span> 

## <a name="ubuntu-cloud-images"></a><span data-ttu-id="b68e6-299">Ubuntu cloudu obrázků</span><span class="sxs-lookup"><span data-stu-id="b68e6-299">Ubuntu Cloud Images</span></span>
<span data-ttu-id="b68e6-300">Všimněte si, že Ubuntu cloudu Image využívat [cloudu init](https://launchpad.net/ubuntu/+source/cloud-init) mnoho úkoly konfigurace, které by jinak spravovány nástrojem Azure Linux Agent.</span><span class="sxs-lookup"><span data-stu-id="b68e6-300">Note that Ubuntu Cloud Images utilize [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) to perform many configuration tasks that would otherwise be managed by the Azure Linux Agent.</span></span>  <span data-ttu-id="b68e6-301">Je třeba počítat následující rozdíly:</span><span class="sxs-lookup"><span data-stu-id="b68e6-301">Please note the following differences:</span></span>

* <span data-ttu-id="b68e6-302">**Provisioning.Enabled** výchozí hodnota je "n" Image Ubuntu cloudu, který použít cloudové init k provádění úloh pro zřízení.</span><span class="sxs-lookup"><span data-stu-id="b68e6-302">**Provisioning.Enabled** defaults to "n" on Ubuntu Cloud Images that use cloud-init to perform provisioning tasks.</span></span>
* <span data-ttu-id="b68e6-303">Následující konfigurační parametry nemají vliv na cloudu bitové kopie Ubuntu, použít cloudové init ke správě prostředků disku a záměna prostoru:</span><span class="sxs-lookup"><span data-stu-id="b68e6-303">The following configuration parameters have no effect on Ubuntu Cloud Images that use cloud-init to manage the resource disk and swap space:</span></span>
  
  * <span data-ttu-id="b68e6-304">**ResourceDisk.Format**</span><span class="sxs-lookup"><span data-stu-id="b68e6-304">**ResourceDisk.Format**</span></span>
  * <span data-ttu-id="b68e6-305">**ResourceDisk.Filesystem**</span><span class="sxs-lookup"><span data-stu-id="b68e6-305">**ResourceDisk.Filesystem**</span></span>
  * <span data-ttu-id="b68e6-306">**ResourceDisk.MountPoint**</span><span class="sxs-lookup"><span data-stu-id="b68e6-306">**ResourceDisk.MountPoint**</span></span>
  * <span data-ttu-id="b68e6-307">**ResourceDisk.EnableSwap**</span><span class="sxs-lookup"><span data-stu-id="b68e6-307">**ResourceDisk.EnableSwap**</span></span>
  * <span data-ttu-id="b68e6-308">**ResourceDisk.SwapSizeMB**</span><span class="sxs-lookup"><span data-stu-id="b68e6-308">**ResourceDisk.SwapSizeMB**</span></span>
* <span data-ttu-id="b68e6-309">Podrobnosti najdete v následujících zdrojích nakonfigurovat prostředek disku přípojného bodu a záměna prostoru Ubuntu cloudu Image při zřizování:</span><span class="sxs-lookup"><span data-stu-id="b68e6-309">Please see the following resources to configure the resource disk mount point and swap space on Ubuntu Cloud Images during provisioning:</span></span>
  
  * [<span data-ttu-id="b68e6-310">Ubuntu Wiki: Konfigurace oddílů Swap</span><span class="sxs-lookup"><span data-stu-id="b68e6-310">Ubuntu Wiki: Configure Swap Partitions</span></span>](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [<span data-ttu-id="b68e6-311">Vložení vlastní Data do virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="b68e6-311">Injecting Custom Data into an Azure Virtual Machine</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

