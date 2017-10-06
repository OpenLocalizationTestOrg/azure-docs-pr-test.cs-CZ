---
title: "aaaInstall služba Mobility (VMware nebo fyzických tooAzure) | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello tooprotect agenta služby Mobility místní počítače."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: f7836e6b35d3838bae1eff927838ce4b245b9f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-mobility-service-vmware-or-physical-tooazure"></a><span data-ttu-id="6c6a3-103">Instalaci služby Mobility (VMware nebo fyzických tooAzure)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-103">Install Mobility Service (VMware or physical tooAzure)</span></span>
<span data-ttu-id="6c6a3-104">Služba Mobility Azure Site Recovery zaznamenává datové zápisy na počítači a předává je toohello procesový server.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-104">Azure Site Recovery Mobility Service captures data writes on a computer, and then forwards them toohello process server.</span></span> <span data-ttu-id="6c6a3-105">Nasaďte službu Mobility počítače tooevery (virtuálního počítače VMware nebo fyzických serverů), které chcete tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-105">Deploy Mobility Service tooevery computer (VMware VM or physical server) that you want tooreplicate tooAzure.</span></span> <span data-ttu-id="6c6a3-106">Můžete nasadit službu Mobility toohello serverů, které má tooprotect pomocí hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="6c6a3-106">You can deploy Mobility Service toohello servers that you want tooprotect by using hello following methods:</span></span>


* [<span data-ttu-id="6c6a3-107">Instalace služby Mobility pomocí nástroje pro nasazení softwaru jako je System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="6c6a3-107">Install Mobility Service by using software deployment tools like System Center Configuration Manager</span></span>](site-recovery-install-mobility-service-using-sccm.md)
* [<span data-ttu-id="6c6a3-108">Instalace služby Mobility pomocí Azure Automation a konfigurace požadovaného stavu (DSC automatizace)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-108">Install Mobility Service by using Azure Automation and Desired State Configuration (Automation DSC)</span></span>](site-recovery-automate-mobility-service-install.md)
* [<span data-ttu-id="6c6a3-109">Nainstalujte službu Mobility ručně pomocí hello grafické uživatelské rozhraní (GUI)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-109">Install Mobility Service manually by using hello graphical user interface (GUI)</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [<span data-ttu-id="6c6a3-110">Ruční instalace služby Mobility na příkazovém řádku</span><span class="sxs-lookup"><span data-stu-id="6c6a3-110">Install Mobility Service manually at a command prompt</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [<span data-ttu-id="6c6a3-111">Instalaci služby Mobility pomocí nabízené instalace z Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="6c6a3-111">Install Mobility Service by push installation from Azure Site Recovery</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> <span data-ttu-id="6c6a3-112">Počínaje verzí 9.7.0.0, na virtuální počítače (VM) s Windows hello služba Mobility instalační program nainstaluje také hello nejnovější dostupné [agenta virtuálního počítače Azure](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span><span class="sxs-lookup"><span data-stu-id="6c6a3-112">Beginning with version 9.7.0.0, on Windows virtual machines (VMs), hello Mobility Service installer also installs hello latest available [Azure VM agent](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span></span> <span data-ttu-id="6c6a3-113">Když se počítač nepovede přes tooAzure, hello počítač splňuje instalace agenta hello požadovaných pro používání všech rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-113">When a computer fails over tooAzure, hello computer meets hello agent installation prerequisite for using any VM extension.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c6a3-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6c6a3-114">Prerequisites</span></span>
<span data-ttu-id="6c6a3-115">Než nainstalujete službu Mobility ručně na vašem serveru, proveďte následující požadované kroky:</span><span class="sxs-lookup"><span data-stu-id="6c6a3-115">Complete these prerequisite steps before you manually install Mobility Service on your server:</span></span>
1. <span data-ttu-id="6c6a3-116">Přihlaste se tooyour konfigurační server a pak otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-116">Sign in tooyour configuration server, and then open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="6c6a3-117">Změnit složku Koš toohello hello a pak vytvořte soubor přístupové heslo:</span><span class="sxs-lookup"><span data-stu-id="6c6a3-117">Change hello directory toohello bin folder, and then create a passphrase file:</span></span>

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. <span data-ttu-id="6c6a3-118">Uložte soubor hello přístupové heslo v zabezpečeném umístění.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-118">Store hello passphrase file in a secure location.</span></span> <span data-ttu-id="6c6a3-119">Použijete soubor hello během hello instalace služby Mobility.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-119">You use hello file during hello Mobility Service installation.</span></span>
4. <span data-ttu-id="6c6a3-120">Instalační služba mobility pro všechny podporované operační systémy jsou ve složce %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository hello.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-120">Mobility Service installers for all supported operating systems are in hello %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository folder.</span></span>

### <a name="mobility-service-installer-to-operating-system-mapping"></a><span data-ttu-id="6c6a3-121">Mapování mobility služby Instalační služby operačního systému</span><span class="sxs-lookup"><span data-stu-id="6c6a3-121">Mobility Service installer-to-operating system mapping</span></span>

| <span data-ttu-id="6c6a3-122">Název šablony souboru instalačního programu</span><span class="sxs-lookup"><span data-stu-id="6c6a3-122">Installer file template name</span></span>| <span data-ttu-id="6c6a3-123">Operační systém</span><span class="sxs-lookup"><span data-stu-id="6c6a3-123">Operating system</span></span> |
|---|--|
|<span data-ttu-id="6c6a3-124">Microsoft automatické obnovení systému\_uživatelský Agent\*Windows\*release.exe</span><span class="sxs-lookup"><span data-stu-id="6c6a3-124">Microsoft-ASR\_UA\*Windows\*release.exe</span></span> | <span data-ttu-id="6c6a3-125">Windows Server 2008 R2 SP1 (64 bitů)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-125">Windows Server 2008 R2 SP1 (64-bit)</span></span> </br> <span data-ttu-id="6c6a3-126">Windows Server 2012 (64 bitů)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-126">Windows Server 2012 (64-bit)</span></span> </br> <span data-ttu-id="6c6a3-127">Windows Server 2012 R2 (64 bitů)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-127">Windows Server 2012 R2 (64-bit)</span></span> |
|<span data-ttu-id="6c6a3-128">Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL6 64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="6c6a3-128">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>| <span data-ttu-id="6c6a3-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> </br> <span data-ttu-id="6c6a3-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> |
|<span data-ttu-id="6c6a3-131">Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL7-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="6c6a3-131">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span> | <span data-ttu-id="6c6a3-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (64-bit only)</span></span> </br> <span data-ttu-id="6c6a3-133">CentOS 7.0, 7.1, 7.2 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-133">CentOS 7.0, 7.1, 7.2 (64-bit only)</span></span></br> <span data-ttu-id="6c6a3-134">CentOs 7.3 (pouze migrace)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-134">CentOs 7.3 (migration only)</span></span> |
|<span data-ttu-id="6c6a3-135">Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="6c6a3-135">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>| <span data-ttu-id="6c6a3-136">SUSE Linux Enterprise Server 11 SP3 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-136">SUSE Linux Enterprise Server 11 SP3 (64-bit only)</span></span>|
|<span data-ttu-id="6c6a3-137">Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="6c6a3-137">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>| <span data-ttu-id="6c6a3-138">SUSE Linux Enterprise Server 11 SP4 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-138">SUSE Linux Enterprise Server 11 SP4 (64-bit only)</span></span>|
|<span data-ttu-id="6c6a3-139">Microsoft automatické obnovení systému\_uživatelský Agent\*OL6-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="6c6a3-139">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span> | <span data-ttu-id="6c6a3-140">Oracle Enterprise Linux 6.4, 6.5 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-140">Oracle Enterprise Linux 6.4, 6.5 (64-bit only)</span></span>|
|<span data-ttu-id="6c6a3-141">Microsoft automatické obnovení systému\_uživatelský Agent\*UBUNTU 14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="6c6a3-141">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span> | <span data-ttu-id="6c6a3-142">Ubuntu Linux 14.04 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="6c6a3-142">Ubuntu Linux 14.04 (64-bit only)</span></span>|


## <a name="install-mobility-service-manually-by-using-hello-gui"></a><span data-ttu-id="6c6a3-143">Nainstalujte službu Mobility ručně pomocí grafického uživatelského rozhraní hello</span><span class="sxs-lookup"><span data-stu-id="6c6a3-143">Install Mobility Service manually by using hello GUI</span></span>

>[!IMPORTANT]
> <span data-ttu-id="6c6a3-144">Pokud používáte **konfigurační Server** tooreplicate **virtuální počítače Azure IaaS** z jednoho předplatného Azure nebo oblast tooanother pak **pomocí instalace z příkazového řádku založené na hello**  – metoda</span><span class="sxs-lookup"><span data-stu-id="6c6a3-144">If you are using a **Configuration Server** tooreplicate **Azure IaaS virtual machines** from one Azure Subscription/Region tooanother then **use hello Command-line based installation** method</span></span>

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a><span data-ttu-id="6c6a3-145">Ruční instalace služby Mobility na příkazovém řádku</span><span class="sxs-lookup"><span data-stu-id="6c6a3-145">Install Mobility Service manually at a command prompt</span></span>

### <a name="command-line-installation-on-a-windows-computer"></a><span data-ttu-id="6c6a3-146">Instalace z příkazového řádku na počítači se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="6c6a3-146">Command-line installation on a Windows computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a><span data-ttu-id="6c6a3-147">Instalace z příkazového řádku na počítač se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="6c6a3-147">Command-line installation on a Linux computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a><span data-ttu-id="6c6a3-148">Instalaci služby Mobility pomocí nabízené instalace z Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="6c6a3-148">Install Mobility Service by push installation from Azure Site Recovery</span></span>
<span data-ttu-id="6c6a3-149">toodo nabízenou instalaci služby Mobility pomocí Site Recovery, všechny cílové počítače musí splňovat následující požadavky hello.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-149">toodo a push installation of Mobility Service by using Site Recovery, all target computers must meet hello following prerequisites.</span></span>

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
<span data-ttu-id="6c6a3-150">Po instalaci služby Mobility, v hello portál Azure, vyberte hello **replikovat** tlačítko toostart ochranu těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-150">After Mobility Service is installed, in hello Azure portal, select hello **Replicate** button toostart protecting these VMs.</span></span>

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a><span data-ttu-id="6c6a3-151">Odinstalujte službu Mobility na počítači s Windows serverem</span><span class="sxs-lookup"><span data-stu-id="6c6a3-151">Uninstall Mobility Service on a Windows Server computer</span></span>
<span data-ttu-id="6c6a3-152">Použijte jeden z následujících metod toouninstall služba Mobility na počítači s Windows serverem hello.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-152">Use one of hello following methods toouninstall Mobility Service on a Windows Server computer.</span></span>

### <a name="uninstall-by-using-hello-gui"></a><span data-ttu-id="6c6a3-153">Odinstalovat pomocí grafického uživatelského rozhraní hello</span><span class="sxs-lookup"><span data-stu-id="6c6a3-153">Uninstall by using hello GUI</span></span>
1. <span data-ttu-id="6c6a3-154">V Ovládacích panelech vyberte **programy**.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-154">In Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="6c6a3-155">Vyberte **Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server**a potom vyberte **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-155">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="6c6a3-156">Odinstalujte na příkazovém řádku</span><span class="sxs-lookup"><span data-stu-id="6c6a3-156">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="6c6a3-157">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-157">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="6c6a3-158">toouninstall služby Mobility, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6c6a3-158">toouninstall Mobility Service, run hello following command:</span></span>

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a><span data-ttu-id="6c6a3-159">Odinstalujte službu Mobility na počítač se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="6c6a3-159">Uninstall Mobility Service on a Linux computer</span></span>
1. <span data-ttu-id="6c6a3-160">Na serveru systému Linux, přihlaste se jako **kořenové** uživatele.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-160">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="6c6a3-161">V terminálu přejděte příliš/uživatel/místní nebo automatické obnovení systému.</span><span class="sxs-lookup"><span data-stu-id="6c6a3-161">In a terminal, go too/user/local/ASR.</span></span>
3. <span data-ttu-id="6c6a3-162">toouninstall služby Mobility, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6c6a3-162">toouninstall Mobility Service, run hello following command:</span></span>

```
uninstall.sh -Y
```
