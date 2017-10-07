---
title: "aaaAzure přehled agenta virtuálního počítače systému Linux | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace agenta pro Linux (příkaz waagent) toomanage virtuální počítač interakci s Kontroleru prostředků infrastruktury Azure."
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
ms.openlocfilehash: 4e08c84d9205f4db7aae6fd1568ec1f15fba395c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-and-using-hello-azure-linux-agent"></a><span data-ttu-id="a1360-103">Informace o používání hello Azure Linux Agent</span><span class="sxs-lookup"><span data-stu-id="a1360-103">Understanding and using hello Azure Linux Agent</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a><span data-ttu-id="a1360-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="a1360-104">Introduction</span></span>
<span data-ttu-id="a1360-105">Hello Microsoft Azure Linux Agent (příkaz waagent) spravuje Linux & FreeBSD zřizování a virtuálních počítačů interakci s hello Kontroleru prostředků infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="a1360-105">hello Microsoft Azure Linux Agent (waagent) manages Linux & FreeBSD provisioning, and VM interaction with hello Azure Fabric Controller.</span></span> <span data-ttu-id="a1360-106">Poskytuje následující funkce pro nasazení systému Linux a FreeBSD IaaS hello:</span><span class="sxs-lookup"><span data-stu-id="a1360-106">It provides hello following functionality for Linux and FreeBSD IaaS deployments:</span></span>

> [!NOTE]
> <span data-ttu-id="a1360-107">Viz hello Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a1360-107">See hello Azure Linux agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) for additional details.</span></span>
> 
> 

* <span data-ttu-id="a1360-108">**Zřizování bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="a1360-108">**Image Provisioning**</span></span>
  
  * <span data-ttu-id="a1360-109">Vytvoření uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="a1360-109">Creation of a user account</span></span>
  * <span data-ttu-id="a1360-110">Konfigurace typů ověřování SSH</span><span class="sxs-lookup"><span data-stu-id="a1360-110">Configuring SSH authentication types</span></span>
  * <span data-ttu-id="a1360-111">Nasazení veřejné klíče SSH a páry klíčů</span><span class="sxs-lookup"><span data-stu-id="a1360-111">Deployment of SSH public keys and key pairs</span></span>
  * <span data-ttu-id="a1360-112">Název hostitele hello nastavení</span><span class="sxs-lookup"><span data-stu-id="a1360-112">Setting hello host name</span></span>
  * <span data-ttu-id="a1360-113">Hello hostitele název toohello platforma pro publikování DNS</span><span class="sxs-lookup"><span data-stu-id="a1360-113">Publishing hello host name toohello platform DNS</span></span>
  * <span data-ttu-id="a1360-114">Generování sestav platforma toohello otisk prstu klíče SSH hostitele</span><span class="sxs-lookup"><span data-stu-id="a1360-114">Reporting SSH host key fingerprint toohello platform</span></span>
  * <span data-ttu-id="a1360-115">Správa prostředků disku</span><span class="sxs-lookup"><span data-stu-id="a1360-115">Resource Disk Management</span></span>
  * <span data-ttu-id="a1360-116">Formátování a připojení hello prostředků disku</span><span class="sxs-lookup"><span data-stu-id="a1360-116">Formatting and mounting hello resource disk</span></span>
  * <span data-ttu-id="a1360-117">Konfigurace velikosti odkládacího souboru</span><span class="sxs-lookup"><span data-stu-id="a1360-117">Configuring swap space</span></span>
* <span data-ttu-id="a1360-118">**Sítě**</span><span class="sxs-lookup"><span data-stu-id="a1360-118">**Networking**</span></span>
  
  * <span data-ttu-id="a1360-119">Spravuje kompatibility tooimprove trasy se servery DHCP, platformy</span><span class="sxs-lookup"><span data-stu-id="a1360-119">Manages routes tooimprove compatibility with platform DHCP servers</span></span>
  * <span data-ttu-id="a1360-120">Zajišťuje hello stabilitu hello název síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="a1360-120">Ensures hello stability of hello network interface name</span></span>
* <span data-ttu-id="a1360-121">**Jádra**</span><span class="sxs-lookup"><span data-stu-id="a1360-121">**Kernel**</span></span>
  
  * <span data-ttu-id="a1360-122">Nakonfiguruje virtuální technologie NUMA (zakázat jádra < 2.6.37)</span><span class="sxs-lookup"><span data-stu-id="a1360-122">Configures virtual NUMA (disable for kernel <2.6.37)</span></span>
  * <span data-ttu-id="a1360-123">Využívá šifrování technologie Hyper-V pro /dev/random</span><span class="sxs-lookup"><span data-stu-id="a1360-123">Consumes Hyper-V entropy for /dev/random</span></span>
  * <span data-ttu-id="a1360-124">Nakonfiguruje SCSI vypršení časových limitů pro zařízení hello kořenové, (který může být vzdálený)</span><span class="sxs-lookup"><span data-stu-id="a1360-124">Configures SCSI timeouts for hello root device (which could be remote)</span></span>
* <span data-ttu-id="a1360-125">**Diagnostika**</span><span class="sxs-lookup"><span data-stu-id="a1360-125">**Diagnostics**</span></span>
  
  * <span data-ttu-id="a1360-126">Konzole přesměrování toohello sériového portu</span><span class="sxs-lookup"><span data-stu-id="a1360-126">Console redirection toohello serial port</span></span>
* <span data-ttu-id="a1360-127">**Nasazení SCVMM**</span><span class="sxs-lookup"><span data-stu-id="a1360-127">**SCVMM Deployments**</span></span>
  
  * <span data-ttu-id="a1360-128">Zjišťuje a bootstraps hello VMM agenta pro Linux v prostředí System Center Virtual Machine Manager 2012 R2</span><span class="sxs-lookup"><span data-stu-id="a1360-128">Detects and bootstraps hello VMM agent for Linux when running in a System Center Virtual Machine Manager 2012 R2 environment</span></span>
* <span data-ttu-id="a1360-129">**Rozšíření virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="a1360-129">**VM Extension**</span></span>
  
  * <span data-ttu-id="a1360-130">Vložit součásti autorem Microsoftu a partnerů do automation tooenable software a konfigurace virtuálního počítače s Linuxem (IaaS)</span><span class="sxs-lookup"><span data-stu-id="a1360-130">Inject component authored by Microsoft and Partners into Linux VM (IaaS) tooenable software and configuration automation</span></span>
  * <span data-ttu-id="a1360-131">Odkaz na implementaci rozšíření virtuálního počítače na [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span><span class="sxs-lookup"><span data-stu-id="a1360-131">VM Extension reference implementation on [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)</span></span>

## <a name="communication"></a><span data-ttu-id="a1360-132">Komunikace</span><span class="sxs-lookup"><span data-stu-id="a1360-132">Communication</span></span>
<span data-ttu-id="a1360-133">prostřednictvím dvou kanálů dojde k Hello toku informací z agenta toohello hello platforem:</span><span class="sxs-lookup"><span data-stu-id="a1360-133">hello information flow from hello platform toohello agent occurs via two channels:</span></span>

* <span data-ttu-id="a1360-134">Při spouštění počítače připojit DVD pro nasazení IaaS.</span><span class="sxs-lookup"><span data-stu-id="a1360-134">A boot-time attached DVD for IaaS deployments.</span></span> <span data-ttu-id="a1360-135">Tento disk DVD zahrnuje kompatibilní se standardem OVF konfigurační soubor, který obsahuje všechny informace o zřizování než skutečná keypairs SSH hello.</span><span class="sxs-lookup"><span data-stu-id="a1360-135">This DVD includes an OVF-compliant configuration file that includes all provisioning information other than hello actual SSH keypairs.</span></span>
* <span data-ttu-id="a1360-136">Koncový bod TCP vystavení rozhraní REST API používá tooobtain nasazení a konfiguraci topologie.</span><span class="sxs-lookup"><span data-stu-id="a1360-136">A TCP endpoint exposing a REST API used tooobtain deployment and topology configuration.</span></span>

## <a name="requirements"></a><span data-ttu-id="a1360-137">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a1360-137">Requirements</span></span>
<span data-ttu-id="a1360-138">Hello následující systémy, které jsou známé toowork s hello Azure Linux Agent:</span><span class="sxs-lookup"><span data-stu-id="a1360-138">hello following systems have been tested and are known toowork with hello Azure Linux Agent:</span></span>

> [!NOTE]
> <span data-ttu-id="a1360-139">Tento seznam se můžou lišit od hello oficiální seznam podporovaných systémů na hello platforma Microsoft Azure podle postupu popsaného tady: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span><span class="sxs-lookup"><span data-stu-id="a1360-139">This list may differ from hello official list of supported systems on hello Microsoft Azure Platform, as described here: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)</span></span>
> 
> 

* <span data-ttu-id="a1360-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="a1360-140">CoreOS</span></span>
* <span data-ttu-id="a1360-141">CentOS 6.3 +</span><span class="sxs-lookup"><span data-stu-id="a1360-141">CentOS 6.3+</span></span>
* <span data-ttu-id="a1360-142">Red Hat Enterprise Linux 6.7 +</span><span class="sxs-lookup"><span data-stu-id="a1360-142">Red Hat Enterprise Linux 6.7+</span></span>
* <span data-ttu-id="a1360-143">Debian 7.0 +</span><span class="sxs-lookup"><span data-stu-id="a1360-143">Debian 7.0+</span></span>
* <span data-ttu-id="a1360-144">Ubuntu 12.04 +</span><span class="sxs-lookup"><span data-stu-id="a1360-144">Ubuntu 12.04+</span></span>
* <span data-ttu-id="a1360-145">openSUSE 12.3 +</span><span class="sxs-lookup"><span data-stu-id="a1360-145">openSUSE 12.3+</span></span>
* <span data-ttu-id="a1360-146">SLES 11 SP3 +</span><span class="sxs-lookup"><span data-stu-id="a1360-146">SLES 11 SP3+</span></span>
* <span data-ttu-id="a1360-147">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="a1360-147">Oracle Linux 6.4+</span></span>

<span data-ttu-id="a1360-148">Jiné podporované systémy:</span><span class="sxs-lookup"><span data-stu-id="a1360-148">Other Supported Systems:</span></span>

* <span data-ttu-id="a1360-149">FreeBSD 10 + (Azure Linux Agent v2.0.10 +)</span><span class="sxs-lookup"><span data-stu-id="a1360-149">FreeBSD 10+ (Azure Linux Agent v2.0.10+)</span></span>

<span data-ttu-id="a1360-150">Hello agenta systému Linux, závisí na některé balíčky systému v pořadí toofunction správně:</span><span class="sxs-lookup"><span data-stu-id="a1360-150">hello Linux agent depends on some system packages in order toofunction properly:</span></span>

* <span data-ttu-id="a1360-151">Python 2.6 +</span><span class="sxs-lookup"><span data-stu-id="a1360-151">Python 2.6+</span></span>
* <span data-ttu-id="a1360-152">OpenSSL 1.0 +</span><span class="sxs-lookup"><span data-stu-id="a1360-152">OpenSSL 1.0+</span></span>
* <span data-ttu-id="a1360-153">OpenSSH 5.3 +</span><span class="sxs-lookup"><span data-stu-id="a1360-153">OpenSSH 5.3+</span></span>
* <span data-ttu-id="a1360-154">Nástroje systému souborů: čtvrceny sfdisk, fdisk, mkfs,</span><span class="sxs-lookup"><span data-stu-id="a1360-154">Filesystem utilities: sfdisk, fdisk, mkfs, parted</span></span>
* <span data-ttu-id="a1360-155">Nástroje pro hesla: chpasswd, sudo</span><span class="sxs-lookup"><span data-stu-id="a1360-155">Password tools: chpasswd, sudo</span></span>
* <span data-ttu-id="a1360-156">Nástroje pro zpracování textu: menšit grep</span><span class="sxs-lookup"><span data-stu-id="a1360-156">Text processing tools: sed, grep</span></span>
* <span data-ttu-id="a1360-157">Síťové nástroje: trasy protokolu ip</span><span class="sxs-lookup"><span data-stu-id="a1360-157">Network tools: ip-route</span></span>
* <span data-ttu-id="a1360-158">Podpora jádra pro připojení UDF systémy.</span><span class="sxs-lookup"><span data-stu-id="a1360-158">Kernel support for mounting UDF filesystems.</span></span>

## <a name="installation"></a><span data-ttu-id="a1360-159">Instalace</span><span class="sxs-lookup"><span data-stu-id="a1360-159">Installation</span></span>
<span data-ttu-id="a1360-160">Instalace pomocí ot. / min nebo bázi DEB balíček z úložiště balíčků vaší distribuce je hello upřednostňovaný způsob instalace a upgrade hello Azure Linux Agent.</span><span class="sxs-lookup"><span data-stu-id="a1360-160">Installation using an RPM or a DEB package from your distribution's package repository is hello preferred method of installing and upgrading hello Azure Linux Agent.</span></span> <span data-ttu-id="a1360-161">Všechny hello [schválené distribuční zprostředkovatelé](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) balíček Azure Linux agent hello integrovat do svých bitové kopie a úložiště.</span><span class="sxs-lookup"><span data-stu-id="a1360-161">All hello [endorsed distribution providers](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) integrate hello Azure Linux agent package into their images and repositories.</span></span>

<span data-ttu-id="a1360-162">Naleznete v dokumentaci toohello v hello [Azure Linux Agent úložišti na Githubu](https://github.com/Azure/WALinuxAgent) pro Upřesnit možnosti instalace, například při instalaci z umístění zdroje nebo toocustom nebo předpony.</span><span class="sxs-lookup"><span data-stu-id="a1360-162">Refer toohello documentation in hello [Azure Linux Agent repo on GitHub](https://github.com/Azure/WALinuxAgent) for advanced installation options, such as installing from source or toocustom locations or prefixes.</span></span>

## <a name="command-line-options"></a><span data-ttu-id="a1360-163">Možnosti příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="a1360-163">Command Line Options</span></span>
### <a name="flags"></a><span data-ttu-id="a1360-164">Příznaky</span><span class="sxs-lookup"><span data-stu-id="a1360-164">Flags</span></span>
* <span data-ttu-id="a1360-165">verbose: zvýšit podrobnost zadaný příkaz</span><span class="sxs-lookup"><span data-stu-id="a1360-165">verbose: Increase verbosity of specified command</span></span>
* <span data-ttu-id="a1360-166">Vynutit: přeskočit interaktivní potvrzení pro některé příkazy</span><span class="sxs-lookup"><span data-stu-id="a1360-166">force: Skip interactive confirmation for some commands</span></span>

### <a name="commands"></a><span data-ttu-id="a1360-167">Příkazy</span><span class="sxs-lookup"><span data-stu-id="a1360-167">Commands</span></span>
* <span data-ttu-id="a1360-168">Nápověda: uvádí hello podporované příkazy a značky.</span><span class="sxs-lookup"><span data-stu-id="a1360-168">help: Lists hello supported commands and flags.</span></span>
* <span data-ttu-id="a1360-169">deprovision: Pokus tooclean hello systému a nastavit jej jako vhodný pro zřizování znovu.</span><span class="sxs-lookup"><span data-stu-id="a1360-169">deprovision: Attempt tooclean hello system and make it suitable for re-provisioning.</span></span> <span data-ttu-id="a1360-170">Tuto operaci odstraněny hello následující:</span><span class="sxs-lookup"><span data-stu-id="a1360-170">This operation deleted hello following:</span></span>
  
  * <span data-ttu-id="a1360-171">Všechny klíče SSH hostitele (Pokud Provisioning.RegenerateSshHostKeyPair 'y' v konfiguračním souboru hello)</span><span class="sxs-lookup"><span data-stu-id="a1360-171">All SSH host keys (if Provisioning.RegenerateSshHostKeyPair is 'y' in hello configuration file)</span></span>
  * <span data-ttu-id="a1360-172">Konfigurace názvový server v /etc/resolv.conf</span><span class="sxs-lookup"><span data-stu-id="a1360-172">Nameserver configuration in /etc/resolv.conf</span></span>
  * <span data-ttu-id="a1360-173">Kořenové heslo z/etc/shadow (Pokud Provisioning.DeleteRootPassword 'y' v konfiguračním souboru hello)</span><span class="sxs-lookup"><span data-stu-id="a1360-173">Root password from /etc/shadow (if Provisioning.DeleteRootPassword is 'y' in hello configuration file)</span></span>
  * <span data-ttu-id="a1360-174">V mezipaměti klienta zapůjčení DHCP</span><span class="sxs-lookup"><span data-stu-id="a1360-174">Cached DHCP client leases</span></span>
  * <span data-ttu-id="a1360-175">Resetování hostitele toolocalhost.localdomain název</span><span class="sxs-lookup"><span data-stu-id="a1360-175">Resets host name toolocalhost.localdomain</span></span>

> [!WARNING]
> <span data-ttu-id="a1360-176">Zrušení zřízení nezaručí, že této bitové kopie hello je nezaškrtnuté všech citlivých informací a je určená pro opětovnou distribuci.</span><span class="sxs-lookup"><span data-stu-id="a1360-176">Deprovisioning does not guarantee that hello image is cleared of all sensitive information and suitable for redistribution.</span></span>
> 
> 

* <span data-ttu-id="a1360-177">deprovision + uživatele: provede vše pod - deprovision (výše) a taky odstraní poslední účet zřízení uživatele hello (získaný z /var/lib/waagent) a související data.</span><span class="sxs-lookup"><span data-stu-id="a1360-177">deprovision+user: Performs everything under -deprovision (above) and also deletes hello last provisioned user account (obtained from /var/lib/waagent) and associated data.</span></span> <span data-ttu-id="a1360-178">Tento parametr je při jeho rušení obrázek, který byl dříve zřizování v Azure, může být zachycen a znovu použít.</span><span class="sxs-lookup"><span data-stu-id="a1360-178">This parameter is when de-provisioning an image that was previously provisioning on Azure so it may be captured and re-used.</span></span>
* <span data-ttu-id="a1360-179">verze: Zobrazí hello verzi příkaz waagent</span><span class="sxs-lookup"><span data-stu-id="a1360-179">version: Displays hello version of waagent</span></span>
* <span data-ttu-id="a1360-180">serialconsole: nakonfiguruje GRUB toomark ttyS0 (hello první sériového portu) jako konzola spouštěcí hello.</span><span class="sxs-lookup"><span data-stu-id="a1360-180">serialconsole: Configures GRUB toomark ttyS0 (hello first serial port) as hello boot console.</span></span> <span data-ttu-id="a1360-181">To zajišťuje, že jsou protokoly spuštění jádra odeslané toothe sériového portu a k dispozici pro ladění.</span><span class="sxs-lookup"><span data-stu-id="a1360-181">This ensures that kernel bootup logs are sent toothe serial port and made available for debugging.</span></span>
* <span data-ttu-id="a1360-182">Démon: příkaz waagent spustit jako démon toomanage interakci s platformou hello.</span><span class="sxs-lookup"><span data-stu-id="a1360-182">daemon: Run waagent as a daemon toomanage interaction with hello platform.</span></span> <span data-ttu-id="a1360-183">Tento argument je zadaný toowaagent ve skriptu init příkaz waagent hello.</span><span class="sxs-lookup"><span data-stu-id="a1360-183">This argument is specified toowaagent in hello waagent init script.</span></span>
* <span data-ttu-id="a1360-184">spustit: Spusťte příkaz waagent jako proces na pozadí</span><span class="sxs-lookup"><span data-stu-id="a1360-184">start: Run waagent as a background process</span></span>

## <a name="configuration"></a><span data-ttu-id="a1360-185">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a1360-185">Configuration</span></span>
<span data-ttu-id="a1360-186">Konfigurační soubor (nebo etc/waagent.conf) ovládacích prvků hello akce příkaz waagent.</span><span class="sxs-lookup"><span data-stu-id="a1360-186">A configuration file (/etc/waagent.conf) controls hello actions of waagent.</span></span> <span data-ttu-id="a1360-187">Ukázkový soubor konfigurace je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="a1360-187">A sample configuration file is shown below:</span></span>

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

<span data-ttu-id="a1360-188">Hello různé možnosti konfigurace jsou podrobně popsány v níže.</span><span class="sxs-lookup"><span data-stu-id="a1360-188">hello various configuration options are described in detail below.</span></span> <span data-ttu-id="a1360-189">Možnosti konfigurace jsou tři typy; Logická hodnota, řetězec nebo celé číslo.</span><span class="sxs-lookup"><span data-stu-id="a1360-189">Configuration options are of three types; Boolean, String or Integer.</span></span> <span data-ttu-id="a1360-190">Možnosti konfigurace Boolean Hello lze zadat jako "y" nebo "n".</span><span class="sxs-lookup"><span data-stu-id="a1360-190">hello Boolean configuration options can be specified as "y" or "n".</span></span> <span data-ttu-id="a1360-191">Hello speciální – klíčové slovo "Žádný" může být použita pro některé řetězec typ konfigurace položky podle popisu níže.</span><span class="sxs-lookup"><span data-stu-id="a1360-191">hello special keyword "None" may be used for some string type configuration entries as detailed below.</span></span>

<span data-ttu-id="a1360-192">**Provisioning.Enabled:**</span><span class="sxs-lookup"><span data-stu-id="a1360-192">**Provisioning.Enabled:**</span></span>  
<span data-ttu-id="a1360-193">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-193">Type: Boolean</span></span>  
<span data-ttu-id="a1360-194">Výchozí: y</span><span class="sxs-lookup"><span data-stu-id="a1360-194">Default: y</span></span>

<span data-ttu-id="a1360-195">To umožňuje hello uživatele tooenable nebo zakázat hello zřizování funkce v agentovi hello.</span><span class="sxs-lookup"><span data-stu-id="a1360-195">This allows hello user tooenable or disable hello provisioning functionality in hello agent.</span></span> <span data-ttu-id="a1360-196">Platné hodnoty jsou "y" nebo "n".</span><span class="sxs-lookup"><span data-stu-id="a1360-196">Valid values are "y" or "n".</span></span> <span data-ttu-id="a1360-197">Pokud zřizování je zakázaná, hostitele a uživatelské klíče SSH hello obrázku jsou zachovány a veškeré zadané v hello Azure zřizování rozhraní API konfigurace je ignorována.</span><span class="sxs-lookup"><span data-stu-id="a1360-197">If provisioning is disabled, SSH host and user keys in hello image are preserved and any configuration specified in hello Azure provisioning API is ignored.</span></span>

> [!NOTE]
> <span data-ttu-id="a1360-198">Hello `Provisioning.Enabled` výchozí hodnoty parametrů příliš "n" Image Ubuntu cloudu, který použít cloudové init pro zřizování.</span><span class="sxs-lookup"><span data-stu-id="a1360-198">hello `Provisioning.Enabled` parameter defaults too"n" on Ubuntu Cloud Images that use cloud-init for provisioning.</span></span>
> 
> 

<span data-ttu-id="a1360-199">**Provisioning.DeleteRootPassword:**</span><span class="sxs-lookup"><span data-stu-id="a1360-199">**Provisioning.DeleteRootPassword:**</span></span>  
<span data-ttu-id="a1360-200">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-200">Type: Boolean</span></span>  
<span data-ttu-id="a1360-201">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="a1360-201">Default: n</span></span>

<span data-ttu-id="a1360-202">Pokud sada hello kořenové heslo v souboru hello/etc/stínové vymazáním během hello procesu zřizování.</span><span class="sxs-lookup"><span data-stu-id="a1360-202">If set, hello root password in hello /etc/shadow file is erased during hello provisioning process.</span></span>

<span data-ttu-id="a1360-203">**Provisioning.RegenerateSshHostKeyPair:**</span><span class="sxs-lookup"><span data-stu-id="a1360-203">**Provisioning.RegenerateSshHostKeyPair:**</span></span>  
<span data-ttu-id="a1360-204">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-204">Type: Boolean</span></span>  
<span data-ttu-id="a1360-205">Výchozí: y</span><span class="sxs-lookup"><span data-stu-id="a1360-205">Default: y</span></span>

<span data-ttu-id="a1360-206">Pokud sadu, všechny hostitele páry klíčů SSH (ecdsa, dsa a rsa), se odstraní při zřizování z etc/ssh/hello.</span><span class="sxs-lookup"><span data-stu-id="a1360-206">If set, all SSH host key pairs (ecdsa, dsa and rsa) are deleted during hello provisioning process from /etc/ssh/.</span></span> <span data-ttu-id="a1360-207">A je generována jeden nový pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="a1360-207">And a single fresh key pair is generated.</span></span>

<span data-ttu-id="a1360-208">typ šifrování Hello pro nový pár klíčů hello je možné konfigurovat pomocí hello Provisioning.SshHostKeyPairType položku.</span><span class="sxs-lookup"><span data-stu-id="a1360-208">hello encryption type for hello fresh key pair is configurable by hello Provisioning.SshHostKeyPairType entry.</span></span> <span data-ttu-id="a1360-209">Upozorňujeme, že některé distribuce bude znovu vytvořit páry klíčů SSH pro všechny chybějící typy šifrování při restartování démon procesu SSH hello (třeba po restartování).</span><span class="sxs-lookup"><span data-stu-id="a1360-209">Please note that some distributions will re-create SSH key pairs for any missing encryption types when hello SSH daemon is restarted (for example, upon a reboot).</span></span>

<span data-ttu-id="a1360-210">**Provisioning.SshHostKeyPairType:**</span><span class="sxs-lookup"><span data-stu-id="a1360-210">**Provisioning.SshHostKeyPairType:**</span></span>  
<span data-ttu-id="a1360-211">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="a1360-211">Type: String</span></span>  
<span data-ttu-id="a1360-212">Výchozí: rsa</span><span class="sxs-lookup"><span data-stu-id="a1360-212">Default: rsa</span></span>

<span data-ttu-id="a1360-213">To je možné nastavit tooan šifrovací algoritmus typ, který je podporován proces démon programu SSH hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a1360-213">This can be set tooan encryption algorithm type that is supported by hello SSH daemon on hello virtual machine.</span></span> <span data-ttu-id="a1360-214">Hello obvykle podporované hodnoty jsou "rsa", "dsa" a "ecdsa".</span><span class="sxs-lookup"><span data-stu-id="a1360-214">hello typically supported values are "rsa", "dsa" and "ecdsa".</span></span> <span data-ttu-id="a1360-215">Všimněte si, že "putty.exe" v systému Windows nepodporuje "ecdsa".</span><span class="sxs-lookup"><span data-stu-id="a1360-215">Note that "putty.exe" on Windows does not support "ecdsa".</span></span> <span data-ttu-id="a1360-216">Ano Pokud máte v úmyslu toouse putty.exe na Windows tooconnect tooa Linux nasazení, použijte "rsa" nebo "dsa".</span><span class="sxs-lookup"><span data-stu-id="a1360-216">So, if you intend toouse putty.exe on Windows tooconnect tooa Linux deployment, please use "rsa" or "dsa".</span></span>

<span data-ttu-id="a1360-217">**Provisioning.MonitorHostName:**</span><span class="sxs-lookup"><span data-stu-id="a1360-217">**Provisioning.MonitorHostName:**</span></span>  
<span data-ttu-id="a1360-218">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-218">Type: Boolean</span></span>  
<span data-ttu-id="a1360-219">Výchozí: y</span><span class="sxs-lookup"><span data-stu-id="a1360-219">Default: y</span></span>

<span data-ttu-id="a1360-220">Pokud nastavíte, příkaz waagent bude monitorovat hello Linux virtuálního počítače pro název hostitele změny (jak vrácené příkazem "název hostitele" hello) a automaticky aktualizovat hello konfigurace sítí v hello image tooreflect hello změn.</span><span class="sxs-lookup"><span data-stu-id="a1360-220">If set, waagent will monitor hello Linux virtual machine for hostname changes (as returned by hello "hostname" command) and automatically update hello networking configuration in hello image tooreflect hello change.</span></span> <span data-ttu-id="a1360-221">V názvu hello toopush pořadí změnit toohello servery DNS, sítě bude restartována v hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a1360-221">In order toopush hello name change toohello DNS servers, networking will be restarted in hello virtual machine.</span></span> <span data-ttu-id="a1360-222">To způsobí Stručný postup ke ztrátě připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="a1360-222">This will result in brief loss of Internet connectivity.</span></span>

<span data-ttu-id="a1360-223">**Provisioning.DecodeCustomData**</span><span class="sxs-lookup"><span data-stu-id="a1360-223">**Provisioning.DecodeCustomData**</span></span>  
<span data-ttu-id="a1360-224">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-224">Type: Boolean</span></span>  
<span data-ttu-id="a1360-225">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="a1360-225">Default: n</span></span>

<span data-ttu-id="a1360-226">Pokud nastavíte, příkaz waagent bude dekódovat CustomData z formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="a1360-226">If set, waagent will decode CustomData from Base64.</span></span>

<span data-ttu-id="a1360-227">**Provisioning.ExecuteCustomData**</span><span class="sxs-lookup"><span data-stu-id="a1360-227">**Provisioning.ExecuteCustomData**</span></span>  
<span data-ttu-id="a1360-228">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-228">Type: Boolean</span></span>  
<span data-ttu-id="a1360-229">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="a1360-229">Default: n</span></span>

<span data-ttu-id="a1360-230">Pokud nastavíte, příkaz waagent provede CustomData po zřízení.</span><span class="sxs-lookup"><span data-stu-id="a1360-230">If set, waagent will execute CustomData after provisioning.</span></span>

<span data-ttu-id="a1360-231">**Provisioning.PasswordCryptId**</span><span class="sxs-lookup"><span data-stu-id="a1360-231">**Provisioning.PasswordCryptId**</span></span>  
<span data-ttu-id="a1360-232">Typ: řetězec</span><span class="sxs-lookup"><span data-stu-id="a1360-232">Type:String</span></span>  
<span data-ttu-id="a1360-233">Výchozí: 6</span><span class="sxs-lookup"><span data-stu-id="a1360-233">Default:6</span></span>

<span data-ttu-id="a1360-234">Algoritmus používaný crypt při generování hodnoty hash hesla.</span><span class="sxs-lookup"><span data-stu-id="a1360-234">Algorithm used by crypt when generating password hash.</span></span>  
 <span data-ttu-id="a1360-235">1 - ALGORITMUS MD5</span><span class="sxs-lookup"><span data-stu-id="a1360-235">1 - MD5</span></span>  
 <span data-ttu-id="a1360-236">2a - Blowfish</span><span class="sxs-lookup"><span data-stu-id="a1360-236">2a - Blowfish</span></span>  
 <span data-ttu-id="a1360-237">5 - SHA-256</span><span class="sxs-lookup"><span data-stu-id="a1360-237">5 - SHA-256</span></span>  
 <span data-ttu-id="a1360-238">6 - SHA-512</span><span class="sxs-lookup"><span data-stu-id="a1360-238">6 - SHA-512</span></span>  

<span data-ttu-id="a1360-239">**Provisioning.PasswordCryptSaltLength**</span><span class="sxs-lookup"><span data-stu-id="a1360-239">**Provisioning.PasswordCryptSaltLength**</span></span>  
<span data-ttu-id="a1360-240">Typ: řetězec</span><span class="sxs-lookup"><span data-stu-id="a1360-240">Type:String</span></span>  
<span data-ttu-id="a1360-241">Výchozí: 10</span><span class="sxs-lookup"><span data-stu-id="a1360-241">Default:10</span></span>

<span data-ttu-id="a1360-242">Délka náhodných salt používá při generování hodnoty hash hesla.</span><span class="sxs-lookup"><span data-stu-id="a1360-242">Length of random salt used when generating password hash.</span></span>

<span data-ttu-id="a1360-243">**ResourceDisk.Format:**</span><span class="sxs-lookup"><span data-stu-id="a1360-243">**ResourceDisk.Format:**</span></span>  
<span data-ttu-id="a1360-244">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-244">Type: Boolean</span></span>  
<span data-ttu-id="a1360-245">Výchozí: y</span><span class="sxs-lookup"><span data-stu-id="a1360-245">Default: y</span></span>

<span data-ttu-id="a1360-246">Pokud nastavíte, hello prostředků disku poskytované hello platformy bude formátu a připojené pomocí příkaz waagent, pokud je typ systému souborů hello požadoval uživatel hello v "ResourceDisk.Filesystem" než "ntfs".</span><span class="sxs-lookup"><span data-stu-id="a1360-246">If set, hello resource disk provided by hello platform will be formatted and mounted by waagent if hello filesystem type requested by hello user in "ResourceDisk.Filesystem" is anything other than "ntfs".</span></span> <span data-ttu-id="a1360-247">Jeden oddíl typu Linux (83) bude k dispozici na disku hello.</span><span class="sxs-lookup"><span data-stu-id="a1360-247">A single partition of type Linux (83) will be made available on hello disk.</span></span> <span data-ttu-id="a1360-248">Všimněte si, že tento oddíl nebude naformátovaný, pokud ho můžete úspěšně připojit.</span><span class="sxs-lookup"><span data-stu-id="a1360-248">Note that this partition will not be formatted if it can be successfully mounted.</span></span>

<span data-ttu-id="a1360-249">**ResourceDisk.Filesystem:**</span><span class="sxs-lookup"><span data-stu-id="a1360-249">**ResourceDisk.Filesystem:**</span></span>  
<span data-ttu-id="a1360-250">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="a1360-250">Type: String</span></span>  
<span data-ttu-id="a1360-251">Výchozí: ext4</span><span class="sxs-lookup"><span data-stu-id="a1360-251">Default: ext4</span></span>

<span data-ttu-id="a1360-252">Určuje typ systému souborů hello hello prostředků disku.</span><span class="sxs-lookup"><span data-stu-id="a1360-252">This specifies hello filesystem type for hello resource disk.</span></span> <span data-ttu-id="a1360-253">Podporované hodnoty se liší podle distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a1360-253">Supported values vary by Linux distribution.</span></span> <span data-ttu-id="a1360-254">Pokud je řetězec hello X, potom mkfs. X by měla být k dispozici na bitovou kopii systému Linux hello.</span><span class="sxs-lookup"><span data-stu-id="a1360-254">If hello string is X, then mkfs.X should be present on hello Linux image.</span></span> <span data-ttu-id="a1360-255">Bitové kopie SLES 11 by měl obvykle používají 'ext3'.</span><span class="sxs-lookup"><span data-stu-id="a1360-255">SLES 11 images should typically use 'ext3'.</span></span> <span data-ttu-id="a1360-256">Obrázky FreeBSD tady by měl použít 'ufs2'.</span><span class="sxs-lookup"><span data-stu-id="a1360-256">FreeBSD images should use 'ufs2' here.</span></span>

<span data-ttu-id="a1360-257">**ResourceDisk.MountPoint:**</span><span class="sxs-lookup"><span data-stu-id="a1360-257">**ResourceDisk.MountPoint:**</span></span>  
<span data-ttu-id="a1360-258">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="a1360-258">Type: String</span></span>  
<span data-ttu-id="a1360-259">Výchozí hodnota: / mnt nebo prostředků</span><span class="sxs-lookup"><span data-stu-id="a1360-259">Default: /mnt/resource</span></span> 

<span data-ttu-id="a1360-260">Toto nastavení určuje hello cestu, kde je připojena hello prostředků disku.</span><span class="sxs-lookup"><span data-stu-id="a1360-260">This specifies hello path at which hello resource disk is mounted.</span></span> <span data-ttu-id="a1360-261">Všimněte si, je tento disk hello prostředků *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a1360-261">Note that hello resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span>

<span data-ttu-id="a1360-262">**ResourceDisk.MountOptions**</span><span class="sxs-lookup"><span data-stu-id="a1360-262">**ResourceDisk.MountOptions**</span></span>  
<span data-ttu-id="a1360-263">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="a1360-263">Type: String</span></span>  
<span data-ttu-id="a1360-264">Výchozí: žádná</span><span class="sxs-lookup"><span data-stu-id="a1360-264">Default: None</span></span>

<span data-ttu-id="a1360-265">Určuje disku připojení možnosti toobe předán toohello připojení -o příkaz.</span><span class="sxs-lookup"><span data-stu-id="a1360-265">Specifies disk mount options toobe passed toohello mount -o command.</span></span> <span data-ttu-id="a1360-266">Toto je čárkami oddělený seznam hodnot, např.</span><span class="sxs-lookup"><span data-stu-id="a1360-266">This is a comma separated list of values, ex.</span></span> <span data-ttu-id="a1360-267">'nodev, nosuid'.</span><span class="sxs-lookup"><span data-stu-id="a1360-267">'nodev,nosuid'.</span></span> <span data-ttu-id="a1360-268">V tématu mount(8) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="a1360-268">See mount(8) for details.</span></span>

<span data-ttu-id="a1360-269">**ResourceDisk.EnableSwap:**</span><span class="sxs-lookup"><span data-stu-id="a1360-269">**ResourceDisk.EnableSwap:**</span></span>  
<span data-ttu-id="a1360-270">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-270">Type: Boolean</span></span>  
<span data-ttu-id="a1360-271">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="a1360-271">Default: n</span></span>

<span data-ttu-id="a1360-272">Pokud nastavit odkládacího souboru (/ swapfile) je vytvořen na prostředku disku hello a přidat systému toohello velikosti odkládacího souboru.</span><span class="sxs-lookup"><span data-stu-id="a1360-272">If set, a swap file (/swapfile) is created on hello resource disk and added toohello system swap space.</span></span>

<span data-ttu-id="a1360-273">**ResourceDisk.SwapSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="a1360-273">**ResourceDisk.SwapSizeMB:**</span></span>  
<span data-ttu-id="a1360-274">Typ: celé číslo</span><span class="sxs-lookup"><span data-stu-id="a1360-274">Type: Integer</span></span>  
<span data-ttu-id="a1360-275">Výchozí: 0</span><span class="sxs-lookup"><span data-stu-id="a1360-275">Default: 0</span></span>

<span data-ttu-id="a1360-276">velikost Hello hello odkládacího souboru v megabajtech.</span><span class="sxs-lookup"><span data-stu-id="a1360-276">hello size of hello swap file in megabytes.</span></span>

<span data-ttu-id="a1360-277">**Logs.Verbose:**</span><span class="sxs-lookup"><span data-stu-id="a1360-277">**Logs.Verbose:**</span></span>  
<span data-ttu-id="a1360-278">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-278">Type: Boolean</span></span>  
<span data-ttu-id="a1360-279">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="a1360-279">Default: n</span></span>

<span data-ttu-id="a1360-280">Pokud je boosted sady podrobností protokolu.</span><span class="sxs-lookup"><span data-stu-id="a1360-280">If set, log verbosity is boosted.</span></span> <span data-ttu-id="a1360-281">Příkaz Waagent protokoly too/var/log/waagent.log a využívá protokoly toorotate hello systému logrotate funkce.</span><span class="sxs-lookup"><span data-stu-id="a1360-281">Waagent logs too/var/log/waagent.log and leverages hello system logrotate functionality toorotate logs.</span></span>

<span data-ttu-id="a1360-282">**OPERAČNÍ SYSTÉM. EnableRDMA**</span><span class="sxs-lookup"><span data-stu-id="a1360-282">**OS.EnableRDMA**</span></span>  
<span data-ttu-id="a1360-283">Typ: logická hodnota</span><span class="sxs-lookup"><span data-stu-id="a1360-283">Type: Boolean</span></span>  
<span data-ttu-id="a1360-284">Výchozí: n</span><span class="sxs-lookup"><span data-stu-id="a1360-284">Default: n</span></span>

<span data-ttu-id="a1360-285">Pokud nastavíte, hello agent bude pokus tooinstall a pak můžete načíst ovladač jádra RDMA, která odpovídá hello verzi firmwaru hello na hello základní hardware.</span><span class="sxs-lookup"><span data-stu-id="a1360-285">If set, hello agent will attempt tooinstall and then load an RDMA kernel driver that matches hello version of hello firmware on hello underlying hardware.</span></span>

<span data-ttu-id="a1360-286">**OPERAČNÍ SYSTÉM. RootDeviceScsiTimeout:**</span><span class="sxs-lookup"><span data-stu-id="a1360-286">**OS.RootDeviceScsiTimeout:**</span></span>  
<span data-ttu-id="a1360-287">Typ: celé číslo</span><span class="sxs-lookup"><span data-stu-id="a1360-287">Type: Integer</span></span>  
<span data-ttu-id="a1360-288">Výchozí: 300</span><span class="sxs-lookup"><span data-stu-id="a1360-288">Default: 300</span></span>

<span data-ttu-id="a1360-289">Tím se nakonfiguruje hello SCSI vypršení časového limitu v sekundách na disku a datové jednotky hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="a1360-289">This configures hello SCSI timeout in seconds on hello OS disk and data drives.</span></span> <span data-ttu-id="a1360-290">Pokud není nastavena, hello systému, které budou použity výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a1360-290">If not set, hello system defaults are used.</span></span>

<span data-ttu-id="a1360-291">**OPERAČNÍ SYSTÉM. OpensslPath:**</span><span class="sxs-lookup"><span data-stu-id="a1360-291">**OS.OpensslPath:**</span></span>  
<span data-ttu-id="a1360-292">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="a1360-292">Type: String</span></span>  
<span data-ttu-id="a1360-293">Výchozí: žádná</span><span class="sxs-lookup"><span data-stu-id="a1360-293">Default: None</span></span>

<span data-ttu-id="a1360-294">To může být použité toospecify alternativní cestu pro binární toouse hello openssl pro kryptografické operace.</span><span class="sxs-lookup"><span data-stu-id="a1360-294">This can be used toospecify an alternate path for hello openssl binary toouse for cryptographic operations.</span></span>

<span data-ttu-id="a1360-295">**HttpProxy.Host HttpProxy.Port**</span><span class="sxs-lookup"><span data-stu-id="a1360-295">**HttpProxy.Host, HttpProxy.Port**</span></span>  
<span data-ttu-id="a1360-296">Typ: Řetězec</span><span class="sxs-lookup"><span data-stu-id="a1360-296">Type: String</span></span>  
<span data-ttu-id="a1360-297">Výchozí: žádná</span><span class="sxs-lookup"><span data-stu-id="a1360-297">Default: None</span></span>

<span data-ttu-id="a1360-298">Pokud nastavíte, hello agent použije tento proxy server tooaccess hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="a1360-298">If set, hello agent will use this proxy server tooaccess hello internet.</span></span> 

## <a name="ubuntu-cloud-images"></a><span data-ttu-id="a1360-299">Ubuntu cloudu obrázků</span><span class="sxs-lookup"><span data-stu-id="a1360-299">Ubuntu Cloud Images</span></span>
<span data-ttu-id="a1360-300">Všimněte si, že Ubuntu cloudu Image využívat [cloudu init](https://launchpad.net/ubuntu/+source/cloud-init) tooperform mnoho úlohy konfigurace, které by jinak spravovány nástrojem hello Azure Linux Agent.</span><span class="sxs-lookup"><span data-stu-id="a1360-300">Note that Ubuntu Cloud Images utilize [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) tooperform many configuration tasks that would otherwise be managed by hello Azure Linux Agent.</span></span>  <span data-ttu-id="a1360-301">Je třeba počítat hello následující rozdíly:</span><span class="sxs-lookup"><span data-stu-id="a1360-301">Please note hello following differences:</span></span>

* <span data-ttu-id="a1360-302">**Provisioning.Enabled** příliš "n" Image Ubuntu cloudu, využívající cloudu init tooperform zřizování úlohy výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a1360-302">**Provisioning.Enabled** defaults too"n" on Ubuntu Cloud Images that use cloud-init tooperform provisioning tasks.</span></span>
* <span data-ttu-id="a1360-303">Hello následující konfigurační parametry nemají vliv na cloudu bitové kopie Ubuntu, použít cloudové init toomanage hello prostředků disku a velikost odkládacího souboru místo:</span><span class="sxs-lookup"><span data-stu-id="a1360-303">hello following configuration parameters have no effect on Ubuntu Cloud Images that use cloud-init toomanage hello resource disk and swap space:</span></span>
  
  * <span data-ttu-id="a1360-304">**ResourceDisk.Format**</span><span class="sxs-lookup"><span data-stu-id="a1360-304">**ResourceDisk.Format**</span></span>
  * <span data-ttu-id="a1360-305">**ResourceDisk.Filesystem**</span><span class="sxs-lookup"><span data-stu-id="a1360-305">**ResourceDisk.Filesystem**</span></span>
  * <span data-ttu-id="a1360-306">**ResourceDisk.MountPoint**</span><span class="sxs-lookup"><span data-stu-id="a1360-306">**ResourceDisk.MountPoint**</span></span>
  * <span data-ttu-id="a1360-307">**ResourceDisk.EnableSwap**</span><span class="sxs-lookup"><span data-stu-id="a1360-307">**ResourceDisk.EnableSwap**</span></span>
  * <span data-ttu-id="a1360-308">**ResourceDisk.SwapSizeMB**</span><span class="sxs-lookup"><span data-stu-id="a1360-308">**ResourceDisk.SwapSizeMB**</span></span>
* <span data-ttu-id="a1360-309">Zobrazit hello následující prostředky tooconfigure hello prostředků disku přípojného bodu a záměna prostoru Ubuntu cloudu Image při zřizování:</span><span class="sxs-lookup"><span data-stu-id="a1360-309">Please see hello following resources tooconfigure hello resource disk mount point and swap space on Ubuntu Cloud Images during provisioning:</span></span>
  
  * [<span data-ttu-id="a1360-310">Ubuntu Wiki: Konfigurace oddílů Swap</span><span class="sxs-lookup"><span data-stu-id="a1360-310">Ubuntu Wiki: Configure Swap Partitions</span></span>](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [<span data-ttu-id="a1360-311">Vložení vlastní Data do virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="a1360-311">Injecting Custom Data into an Azure Virtual Machine</span></span>](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

