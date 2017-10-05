---
title: "Instalaci služby Mobility (VMware nebo fyzických do Azure) | Microsoft Docs"
description: "Informace o instalaci agenta služby Mobility místní počítače chránit."
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
ms.openlocfilehash: 848284f37ae2470a169d8f8a8c9c0bb5b926abe3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-mobility-service-vmware-or-physical-to-azure"></a><span data-ttu-id="98fb7-103">Instalaci služby Mobility (VMware nebo fyzických do Azure)</span><span class="sxs-lookup"><span data-stu-id="98fb7-103">Install Mobility Service (VMware or physical to Azure)</span></span>
<span data-ttu-id="98fb7-104">Služba Mobility Azure Site Recovery zaznamenává datové zápisy na počítači a předává je na procesní server.</span><span class="sxs-lookup"><span data-stu-id="98fb7-104">Azure Site Recovery Mobility Service captures data writes on a computer, and then forwards them to the process server.</span></span> <span data-ttu-id="98fb7-105">Nasazení služby Mobility do jednotlivých počítačů (virtuálních počítačů VMware nebo fyzických serverů), který chcete replikovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="98fb7-105">Deploy Mobility Service to every computer (VMware VM or physical server) that you want to replicate to Azure.</span></span> <span data-ttu-id="98fb7-106">Služba Mobility můžete nasadit na servery, které chcete chránit pomocí následujících metod:</span><span class="sxs-lookup"><span data-stu-id="98fb7-106">You can deploy Mobility Service to the servers that you want to protect by using the following methods:</span></span>


* [<span data-ttu-id="98fb7-107">Instalace služby Mobility pomocí nástroje pro nasazení softwaru jako je System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="98fb7-107">Install Mobility Service by using software deployment tools like System Center Configuration Manager</span></span>](site-recovery-install-mobility-service-using-sccm.md)
* [<span data-ttu-id="98fb7-108">Instalace služby Mobility pomocí Azure Automation a konfigurace požadovaného stavu (DSC automatizace)</span><span class="sxs-lookup"><span data-stu-id="98fb7-108">Install Mobility Service by using Azure Automation and Desired State Configuration (Automation DSC)</span></span>](site-recovery-automate-mobility-service-install.md)
* [<span data-ttu-id="98fb7-109">Nainstalujte službu Mobility ručně pomocí grafického uživatelského rozhraní (GUI)</span><span class="sxs-lookup"><span data-stu-id="98fb7-109">Install Mobility Service manually by using the graphical user interface (GUI)</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [<span data-ttu-id="98fb7-110">Ruční instalace služby Mobility na příkazovém řádku</span><span class="sxs-lookup"><span data-stu-id="98fb7-110">Install Mobility Service manually at a command prompt</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [<span data-ttu-id="98fb7-111">Instalaci služby Mobility pomocí nabízené instalace z Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="98fb7-111">Install Mobility Service by push installation from Azure Site Recovery</span></span>](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> <span data-ttu-id="98fb7-112">Počínaje verzí 9.7.0.0, na virtuální počítače s Windows (VM), služba Mobility instalační program nainstaluje také k dispozici nejnovější [agenta virtuálního počítače Azure](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span><span class="sxs-lookup"><span data-stu-id="98fb7-112">Beginning with version 9.7.0.0, on Windows virtual machines (VMs), the Mobility Service installer also installs the latest available [Azure VM agent](../virtual-machines/windows/extensions-features.md#azure-vm-agent).</span></span> <span data-ttu-id="98fb7-113">Při počítače při selhání do Azure, že počítač splňuje instalace agenta požadované pro používání všech rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="98fb7-113">When a computer fails over to Azure, the computer meets the agent installation prerequisite for using any VM extension.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98fb7-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="98fb7-114">Prerequisites</span></span>
<span data-ttu-id="98fb7-115">Než nainstalujete službu Mobility ručně na vašem serveru, proveďte následující požadované kroky:</span><span class="sxs-lookup"><span data-stu-id="98fb7-115">Complete these prerequisite steps before you manually install Mobility Service on your server:</span></span>
1. <span data-ttu-id="98fb7-116">Přihlaste se k konfigurační server a pak otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="98fb7-116">Sign in to your configuration server, and then open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="98fb7-117">Změňte adresář na složku Koš a pak vytvořte soubor přístupové heslo:</span><span class="sxs-lookup"><span data-stu-id="98fb7-117">Change the directory to the bin folder, and then create a passphrase file:</span></span>

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. <span data-ttu-id="98fb7-118">Přístupové heslo soubor uložte na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="98fb7-118">Store the passphrase file in a secure location.</span></span> <span data-ttu-id="98fb7-119">Můžete použít soubor během instalace služby Mobility.</span><span class="sxs-lookup"><span data-stu-id="98fb7-119">You use the file during the Mobility Service installation.</span></span>
4. <span data-ttu-id="98fb7-120">Instalační služba mobility pro všechny podporované operační systémy jsou ve složce %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository.</span><span class="sxs-lookup"><span data-stu-id="98fb7-120">Mobility Service installers for all supported operating systems are in the %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository folder.</span></span>

### <a name="mobility-service-installer-to-operating-system-mapping"></a><span data-ttu-id="98fb7-121">Mapování mobility služby Instalační služby operačního systému</span><span class="sxs-lookup"><span data-stu-id="98fb7-121">Mobility Service installer-to-operating system mapping</span></span>

| <span data-ttu-id="98fb7-122">Název šablony souboru instalačního programu</span><span class="sxs-lookup"><span data-stu-id="98fb7-122">Installer file template name</span></span>| <span data-ttu-id="98fb7-123">Operační systém</span><span class="sxs-lookup"><span data-stu-id="98fb7-123">Operating system</span></span> |
|---|--|
|<span data-ttu-id="98fb7-124">Microsoft automatické obnovení systému\_uživatelský Agent\*Windows\*release.exe</span><span class="sxs-lookup"><span data-stu-id="98fb7-124">Microsoft-ASR\_UA\*Windows\*release.exe</span></span> | <span data-ttu-id="98fb7-125">Windows Server 2008 R2 SP1 (64 bitů)</span><span class="sxs-lookup"><span data-stu-id="98fb7-125">Windows Server 2008 R2 SP1 (64-bit)</span></span> </br> <span data-ttu-id="98fb7-126">Windows Server 2012 (64 bitů)</span><span class="sxs-lookup"><span data-stu-id="98fb7-126">Windows Server 2012 (64-bit)</span></span> </br> <span data-ttu-id="98fb7-127">Windows Server 2012 R2 (64 bitů)</span><span class="sxs-lookup"><span data-stu-id="98fb7-127">Windows Server 2012 R2 (64-bit)</span></span> |
|<span data-ttu-id="98fb7-128">Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL6 64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="98fb7-128">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>| <span data-ttu-id="98fb7-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="98fb7-129">Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> </br> <span data-ttu-id="98fb7-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="98fb7-130">CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (64-bit only)</span></span> |
|<span data-ttu-id="98fb7-131">Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL7-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="98fb7-131">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span> | <span data-ttu-id="98fb7-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="98fb7-132">Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (64-bit only)</span></span> </br> <span data-ttu-id="98fb7-133">CentOS 7.0, 7.1, 7.2 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="98fb7-133">CentOS 7.0, 7.1, 7.2 (64-bit only)</span></span></br> <span data-ttu-id="98fb7-134">CentOs 7.3 (pouze migrace)</span><span class="sxs-lookup"><span data-stu-id="98fb7-134">CentOs 7.3 (migration only)</span></span> |
|<span data-ttu-id="98fb7-135">Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="98fb7-135">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>| <span data-ttu-id="98fb7-136">SUSE Linux Enterprise Server 11 SP3 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="98fb7-136">SUSE Linux Enterprise Server 11 SP3 (64-bit only)</span></span>|
|<span data-ttu-id="98fb7-137">Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="98fb7-137">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>| <span data-ttu-id="98fb7-138">SUSE Linux Enterprise Server 11 SP4 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="98fb7-138">SUSE Linux Enterprise Server 11 SP4 (64-bit only)</span></span>|
|<span data-ttu-id="98fb7-139">Microsoft automatické obnovení systému\_uživatelský Agent\*OL6-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="98fb7-139">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span> | <span data-ttu-id="98fb7-140">Oracle Enterprise Linux 6.4, 6.5 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="98fb7-140">Oracle Enterprise Linux 6.4, 6.5 (64-bit only)</span></span>|
|<span data-ttu-id="98fb7-141">Microsoft automatické obnovení systému\_uživatelský Agent\*UBUNTU 14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="98fb7-141">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span> | <span data-ttu-id="98fb7-142">Ubuntu Linux 14.04 (pouze 64bitové)</span><span class="sxs-lookup"><span data-stu-id="98fb7-142">Ubuntu Linux 14.04 (64-bit only)</span></span>|


## <a name="install-mobility-service-manually-by-using-the-gui"></a><span data-ttu-id="98fb7-143">Nainstalujte službu Mobility ručně pomocí grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="98fb7-143">Install Mobility Service manually by using the GUI</span></span>

>[!IMPORTANT]
> <span data-ttu-id="98fb7-144">Pokud používáte **konfigurační Server** k replikaci **virtuální počítače Azure IaaS** z jedné Azure předplatné nebo oblasti do jiné pak **pomocí instalace z příkazového řádku, na základě** – metoda</span><span class="sxs-lookup"><span data-stu-id="98fb7-144">If you are using a **Configuration Server** to replicate **Azure IaaS virtual machines** from one Azure Subscription/Region to another then **use the Command-line based installation** method</span></span>

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a><span data-ttu-id="98fb7-145">Ruční instalace služby Mobility na příkazovém řádku</span><span class="sxs-lookup"><span data-stu-id="98fb7-145">Install Mobility Service manually at a command prompt</span></span>

### <a name="command-line-installation-on-a-windows-computer"></a><span data-ttu-id="98fb7-146">Instalace z příkazového řádku na počítači se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="98fb7-146">Command-line installation on a Windows computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a><span data-ttu-id="98fb7-147">Instalace z příkazového řádku na počítač se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="98fb7-147">Command-line installation on a Linux computer</span></span>
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a><span data-ttu-id="98fb7-148">Instalaci služby Mobility pomocí nabízené instalace z Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="98fb7-148">Install Mobility Service by push installation from Azure Site Recovery</span></span>
<span data-ttu-id="98fb7-149">Nabízenou instalaci služby Mobility pomocí Site Recovery proveďte všechny cílové počítače musí splňovat následující požadavky.</span><span class="sxs-lookup"><span data-stu-id="98fb7-149">To do a push installation of Mobility Service by using Site Recovery, all target computers must meet the following prerequisites.</span></span>

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
<span data-ttu-id="98fb7-150">Po instalaci služby Mobility na portálu Azure vyberte **replikovat** tlačítko spustit ochranu těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="98fb7-150">After Mobility Service is installed, in the Azure portal, select the **Replicate** button to start protecting these VMs.</span></span>

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a><span data-ttu-id="98fb7-151">Odinstalujte službu Mobility na počítači s Windows serverem</span><span class="sxs-lookup"><span data-stu-id="98fb7-151">Uninstall Mobility Service on a Windows Server computer</span></span>
<span data-ttu-id="98fb7-152">Odinstalace služby Mobility na počítači s Windows serverem, použijte jednu z následujících metod.</span><span class="sxs-lookup"><span data-stu-id="98fb7-152">Use one of the following methods to uninstall Mobility Service on a Windows Server computer.</span></span>

### <a name="uninstall-by-using-the-gui"></a><span data-ttu-id="98fb7-153">Odinstalovat pomocí grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="98fb7-153">Uninstall by using the GUI</span></span>
1. <span data-ttu-id="98fb7-154">V Ovládacích panelech vyberte **programy**.</span><span class="sxs-lookup"><span data-stu-id="98fb7-154">In Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="98fb7-155">Vyberte **Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server**a potom vyberte **odinstalovat**.</span><span class="sxs-lookup"><span data-stu-id="98fb7-155">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="98fb7-156">Odinstalujte na příkazovém řádku</span><span class="sxs-lookup"><span data-stu-id="98fb7-156">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="98fb7-157">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="98fb7-157">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="98fb7-158">Odinstalace služby Mobility, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="98fb7-158">To uninstall Mobility Service, run the following command:</span></span>

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a><span data-ttu-id="98fb7-159">Odinstalujte službu Mobility na počítač se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="98fb7-159">Uninstall Mobility Service on a Linux computer</span></span>
1. <span data-ttu-id="98fb7-160">Na serveru systému Linux, přihlaste se jako **kořenové** uživatele.</span><span class="sxs-lookup"><span data-stu-id="98fb7-160">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="98fb7-161">V terminálu přejděte na /user/local/ASR.</span><span class="sxs-lookup"><span data-stu-id="98fb7-161">In a terminal, go to /user/local/ASR.</span></span>
3. <span data-ttu-id="98fb7-162">Odinstalace služby Mobility, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="98fb7-162">To uninstall Mobility Service, run the following command:</span></span>

```
uninstall.sh -Y
```
