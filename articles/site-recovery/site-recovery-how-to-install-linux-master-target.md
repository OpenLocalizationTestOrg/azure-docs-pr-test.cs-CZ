---
title: "Jak nainstalovat Linux hlavní cílový server pro převzetí služeb při selhání z Azure do místní | Microsoft Docs"
description: "Před opětovnou ochranu virtuální počítač s Linuxem, potřebujete hlavní cílový server Linux. Zjistěte, jak k jeho instalaci."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 5341e3e56e0c366079958dd9a885f6ee3e8436cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="install-a-linux-master-target-server"></a><span data-ttu-id="2e772-104">Instalovat hlavní cílový server Linux</span><span class="sxs-lookup"><span data-stu-id="2e772-104">Install a Linux master target server</span></span>
<span data-ttu-id="2e772-105">Po selhání virtuálních počítačů, můžete můžete navrácení služeb po obnovení virtuálních počítačů k místní lokalitě.</span><span class="sxs-lookup"><span data-stu-id="2e772-105">After you fail over your virtual machines, you can fail back the virtual machines to the on-premises site.</span></span> <span data-ttu-id="2e772-106">Chcete-li navrácení služeb po obnovení, je potřeba znovu nastavte ochranu virtuálního počítače z Azure do místní lokality.</span><span class="sxs-lookup"><span data-stu-id="2e772-106">To fail back, you need to reprotect the virtual machine from Azure to the on-premises site.</span></span> <span data-ttu-id="2e772-107">Pro tento proces budete potřebovat místní hlavní cílový server příjem provozu.</span><span class="sxs-lookup"><span data-stu-id="2e772-107">For this process, you need an on-premises master target server to receive the traffic.</span></span> 

<span data-ttu-id="2e772-108">Pokud chráněný virtuální počítač je virtuálního počítače s Windows, musíte Windows hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="2e772-108">If your protected virtual machine is a Windows virtual machine, then you need a Windows master target.</span></span> <span data-ttu-id="2e772-109">Pro virtuální počítač s Linuxem budete potřebovat hlavního cíle Linuxu.</span><span class="sxs-lookup"><span data-stu-id="2e772-109">For a Linux virtual machine, you need a Linux master target.</span></span> <span data-ttu-id="2e772-110">Přečtěte si následující kroky a zjistěte, jak vytvořit a nainstalovat hlavního cíle Linuxu.</span><span class="sxs-lookup"><span data-stu-id="2e772-110">Read the following steps to learn how to create and install a Linux master target.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2e772-111">Od verze 9.10.0 hlavní cílový server, nejnovější hlavní cílový server můžete nainstalovat jenom na Ubuntu 16.04 server.</span><span class="sxs-lookup"><span data-stu-id="2e772-111">Starting with release of the 9.10.0 master target server, the latest master target server can be only installed on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="2e772-112">Nové instalace nejsou povoleny u CentOS6.6 servery.</span><span class="sxs-lookup"><span data-stu-id="2e772-112">New installations aren't allowed on  CentOS6.6 servers.</span></span> <span data-ttu-id="2e772-113">Však můžete nadále upgradu vaší starého hlavního cílové servery pomocí 9.10.0 verze.</span><span class="sxs-lookup"><span data-stu-id="2e772-113">However, you can continue to upgrade your old master target servers by using the 9.10.0 version.</span></span>

## <a name="overview"></a><span data-ttu-id="2e772-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="2e772-114">Overview</span></span>
<span data-ttu-id="2e772-115">Tento článek obsahuje pokyny k instalaci hlavního cíle Linuxu.</span><span class="sxs-lookup"><span data-stu-id="2e772-115">This article provides instructions for how to install a Linux master target.</span></span>

<span data-ttu-id="2e772-116">POST dotazy nebo připomínky můžete na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="2e772-116">Post comments or questions at the end of this article or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e772-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2e772-117">Prerequisites</span></span>

* <span data-ttu-id="2e772-118">Zvolte hostitele, do které chcete nasadit na hlavním cíli, určete, pokud navrácení služeb po obnovení bude do existující virtuální počítač místně nebo do nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2e772-118">To choose the host on which to deploy the master target, determine if the failback is going to be to an existing on-premises virtual machine or to a new virtual machine.</span></span> 
    * <span data-ttu-id="2e772-119">Pro existující virtuální počítač hostitele hlavního cíle mají mít přístup k úložišti dat virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2e772-119">For an existing virtual machine, the host of the master target should have access to the data stores of the virtual machine.</span></span>
    * <span data-ttu-id="2e772-120">Pokud na místním virtuálním počítači neexistuje, je navrácení služeb po obnovení virtuálního počítače vytvořit na stejném hostiteli jako hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="2e772-120">If the on-premises virtual machine does not exist, the failback virtual machine is created on the same host as the master target.</span></span> <span data-ttu-id="2e772-121">Můžete vybrat libovolného hostitele ESXi k instalaci na hlavním cíli.</span><span class="sxs-lookup"><span data-stu-id="2e772-121">You can choose any ESXi host to install the master target.</span></span>
* <span data-ttu-id="2e772-122">Hlavní cíl musí být v síti, který může komunikovat s procesovým serverem a konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="2e772-122">The master target should be on a network that can communicate with the process server and the configuration server.</span></span>
* <span data-ttu-id="2e772-123">Verze hlavního cíle musí být rovna nebo starší než verze procesového serveru a konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="2e772-123">The version of the master target must be equal to or earlier than the versions of the process server and the configuration server.</span></span> <span data-ttu-id="2e772-124">Například pokud je verze konfigurace serveru 9.4, verze hlavního cíle může být 9.4 nebo 9.3, ale není 9.5.</span><span class="sxs-lookup"><span data-stu-id="2e772-124">For example, if the version of the configuration server is 9.4, the version of the master target can be 9.4 or 9.3 but not 9.5.</span></span>
* <span data-ttu-id="2e772-125">Na hlavním cíli lze pouze virtuální počítač VMware, nikoli na fyzický server.</span><span class="sxs-lookup"><span data-stu-id="2e772-125">The master target can only be a VMware virtual machine and not a physical server.</span></span>

## <a name="create-the-master-target-according-to-the-sizing-guidelines"></a><span data-ttu-id="2e772-126">Vytvoření hlavního cíle podle pokynů pro změnu velikosti</span><span class="sxs-lookup"><span data-stu-id="2e772-126">Create the master target according to the sizing guidelines</span></span>

<span data-ttu-id="2e772-127">Vytvoření hlavního cíle podle následujících pokynů pro změnu velikosti:</span><span class="sxs-lookup"><span data-stu-id="2e772-127">Create the master target in accordance with the following sizing guidelines:</span></span>
- <span data-ttu-id="2e772-128">**Paměť RAM**: 6 GB nebo více</span><span class="sxs-lookup"><span data-stu-id="2e772-128">**RAM**: 6 GB or more</span></span>
- <span data-ttu-id="2e772-129">**Velikost disku operačního systému**: 100 GB nebo více (pro instalaci CentOS6.6)</span><span class="sxs-lookup"><span data-stu-id="2e772-129">**OS disk size**: 100 GB or more (to install CentOS6.6)</span></span>
- <span data-ttu-id="2e772-130">**Velikost disku Další jednotka pro uchování**: 1 TB</span><span class="sxs-lookup"><span data-stu-id="2e772-130">**Additional disk size for retention drive**: 1 TB</span></span>
- <span data-ttu-id="2e772-131">**Jader procesoru**: 4 jádra nebo více</span><span class="sxs-lookup"><span data-stu-id="2e772-131">**CPU cores**: 4 cores or more</span></span>

<span data-ttu-id="2e772-132">Jsou podporovány následující podporované Ubuntu jádra.</span><span class="sxs-lookup"><span data-stu-id="2e772-132">The following supported Ubuntu kernels are supported.</span></span>


|<span data-ttu-id="2e772-133">Řada jádra</span><span class="sxs-lookup"><span data-stu-id="2e772-133">Kernel Series</span></span>  |<span data-ttu-id="2e772-134">Podporovat až</span><span class="sxs-lookup"><span data-stu-id="2e772-134">Support up to</span></span>  |
|---------|---------|
|<span data-ttu-id="2e772-135">4.4</span><span class="sxs-lookup"><span data-stu-id="2e772-135">4.4</span></span>      |<span data-ttu-id="2e772-136">4.4.0-81-Generic</span><span class="sxs-lookup"><span data-stu-id="2e772-136">4.4.0-81-generic</span></span>         |
|<span data-ttu-id="2e772-137">4.8</span><span class="sxs-lookup"><span data-stu-id="2e772-137">4.8</span></span>      |<span data-ttu-id="2e772-138">4.8.0-56-Generic</span><span class="sxs-lookup"><span data-stu-id="2e772-138">4.8.0-56-generic</span></span>         |
|<span data-ttu-id="2e772-139">4.10</span><span class="sxs-lookup"><span data-stu-id="2e772-139">4.10</span></span>     |<span data-ttu-id="2e772-140">4.10.0-24-Generic</span><span class="sxs-lookup"><span data-stu-id="2e772-140">4.10.0-24-generic</span></span>        |


## <a name="deploy-the-master-target-server"></a><span data-ttu-id="2e772-141">Nasazení hlavního cílového serveru</span><span class="sxs-lookup"><span data-stu-id="2e772-141">Deploy the master target server</span></span>

### <a name="install-ubuntu-16042-minimal"></a><span data-ttu-id="2e772-142">Nainstalujte Ubuntu 16.04.2 minimální</span><span class="sxs-lookup"><span data-stu-id="2e772-142">Install Ubuntu 16.04.2 Minimal</span></span>

<span data-ttu-id="2e772-143">Proveďte následující kroky pro instalaci Ubuntu 16.04.2 64bitový operační systém.</span><span class="sxs-lookup"><span data-stu-id="2e772-143">Take the following the steps to install the Ubuntu 16.04.2 64-bit operating system.</span></span>

<span data-ttu-id="2e772-144">**Krok 1:** přejít na [stáhnout odkaz](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) a zvolte nejbližší zrcadlení z které stáhnout soubor ISO Ubuntu 16.04.2 minimální 64-bit.</span><span class="sxs-lookup"><span data-stu-id="2e772-144">**Step 1:** Go to the [download link](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) and choose the closest mirror from which download an Ubuntu 16.04.2 minimal 64-bit ISO.</span></span>

<span data-ttu-id="2e772-145">Ponechte soubor ISO Ubuntu 16.04.2 minimální 64-bit do jednotky DVD a spuštění systému.</span><span class="sxs-lookup"><span data-stu-id="2e772-145">Keep an Ubuntu 16.04.2 minimal 64-bit ISO in the DVD drive and start the system.</span></span>

<span data-ttu-id="2e772-146">**Krok 2:** vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-146">**Step 2:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Výběr jazyka](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

<span data-ttu-id="2e772-148">**Krok 3:** vyberte **nainstalovat Ubuntu Server**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-148">**Step 3:** Select **Install Ubuntu Server**, and then select **Enter**.</span></span>

![Vyberte možnost instalace Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

<span data-ttu-id="2e772-150">**Krok 4:** vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-150">**Step 4:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Vyberte angličtina jako upřednostňovaný jazyk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

<span data-ttu-id="2e772-152">**Krok 5:** vyberte příslušnou možnost **časové pásmo** seznam možností a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-152">**Step 5:** Select the appropriate option from the **Time Zone** options list, and then select **Enter**.</span></span>

![Vyberte správné časové pásmo](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

<span data-ttu-id="2e772-154">**Krok 6:** vyberte **ne** (výchozí možnost) a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-154">**Step 6:** Select **No** (the default option), and then select **Enter**.</span></span>


![Konfigurace klávesnice](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

<span data-ttu-id="2e772-156">**Krok 7:** vyberte **angličtinu (US)** jako země původu klávesnice, a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-156">**Step 7:** Select **English (US)** as the country of origin for the keyboard, and then select **Enter**.</span></span>

![Vyberte USA jako země původu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

<span data-ttu-id="2e772-158">**Krok 8:** vyberte **angličtinu (US)** rozložení klávesnice a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-158">**Step 8:** Select **English (US)** as the keyboard layout, and then select **Enter**.</span></span>

![Vyberte angličtinu jako rozložení klávesnice](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

<span data-ttu-id="2e772-160">**Krok 9:** zadejte název hostitele pro server v **Hostname** a pak vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="2e772-160">**Step 9:** Enter the hostname for your server in the **Hostname** box, and then select **Continue**.</span></span>

![Zadejte název hostitele pro server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

<span data-ttu-id="2e772-162">**Krok 10:** vytvoření uživatelského účtu, zadejte uživatelské jméno a potom vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="2e772-162">**Step 10:** To create a user account, enter the user name, and then select **Continue**.</span></span>

![Vytvoření uživatelského účtu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

<span data-ttu-id="2e772-164">**Krok 11:** zadejte heslo pro nový uživatelský účet a potom vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="2e772-164">**Step 11:** Enter the password for the new user account, and then select **Continue**.</span></span>

![Zadejte heslo](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

<span data-ttu-id="2e772-166">**Krok 12:** potvrďte heslo pro nového uživatele a pak vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="2e772-166">**Step 12:** Confirm the password for the new user, and then select **Continue**.</span></span>

![Potvrzení hesla](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

<span data-ttu-id="2e772-168">**Krok 13:** vyberte **ne** (výchozí možnost) a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-168">**Step 13:** Select **No** (the default option), and then select **Enter**.</span></span>

![Nastavit uživatele a hesla](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

<span data-ttu-id="2e772-170">**Krok 14:** Pokud časové pásmo, které se zobrazí správný, vyberte **Ano** (výchozí možnost) a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-170">**Step 14:** If the time zone that's displayed is correct, select **Yes** (the default option), and then select **Enter**.</span></span>

<span data-ttu-id="2e772-171">Chcete-li překonfigurovat časové pásmo, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="2e772-171">To reconfigure your time zone, select **No**.</span></span>

![Nakonfigurujte hodiny](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

<span data-ttu-id="2e772-173">**Krok 15:** dělicí metody možnosti, vyberte **na základě - použít celý disk**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-173">**Step 15:** From the partitioning method options, select **Guided - use entire disk**, and then select **Enter**.</span></span>

![Vyberte možnost použít pro dělicí metody](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

<span data-ttu-id="2e772-175">**Krok 16:** vyberte příslušný disk z **vyberte disk do oddílu** možnosti a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-175">**Step 16:** Select the appropriate disk from the **Select disk to partition** options, and then select **Enter**.</span></span>


![Vyberte disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

<span data-ttu-id="2e772-177">**Krok 17:** vyberte **Ano** k zápisu změn na disk a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-177">**Step 17:** Select **Yes** to write the changes to disk, and then select **Enter**.</span></span>

![Zápis změn na disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

<span data-ttu-id="2e772-179">**Krok 18:** vyberte možnost výchozí, vyberte **pokračovat**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-179">**Step 18:** Select the default option, select **Continue**, and then select **Enter**.</span></span>

![Vyberte možnost výchozí](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

<span data-ttu-id="2e772-181">**Krok 19:** vyberte příslušnou možnost pro správu upgrady systému a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-181">**Step 19:** Select the appropriate option for managing upgrades on your system, and then select **Enter**.</span></span>

![Vyberte, jak spravovat upgrady](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> <span data-ttu-id="2e772-183">Protože Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi Ubuntu, musíte zajistit, aby jádra zakázali upgrady pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2e772-183">Because the Azure Site Recovery master target server requires a very specific version of the Ubuntu, you need to ensure that the kernel upgrades are disabled for the virtual machine.</span></span> <span data-ttu-id="2e772-184">Pokud se povolí, nějaké regulární upgrady způsobit hlavní cílový server fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="2e772-184">If they are enabled, then any regular upgrades cause the master target server to malfunction.</span></span> <span data-ttu-id="2e772-185">Zkontrolujte, zda jste vybrali **žádné automatické aktualizace** možnost.</span><span class="sxs-lookup"><span data-stu-id="2e772-185">Make sure you select the **No automatic updates** option.</span></span>


<span data-ttu-id="2e772-186">**Krok 20:** vyberte výchozí možnosti.</span><span class="sxs-lookup"><span data-stu-id="2e772-186">**Step 20:** Select default options.</span></span> <span data-ttu-id="2e772-187">Pokud chcete openSSH pro připojení SSH, vyberte **OpenSSH server** a pak vyberte možnost **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="2e772-187">If you want openSSH for SSH connect, select the **OpenSSH server** option, and then select **Continue**.</span></span>

![Vyberte software](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

<span data-ttu-id="2e772-189">**Krok 21:** vyberte **Ano**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-189">**Step 21:** Select **Yes**, and then select **Enter**.</span></span>

![Isntall spouštěcí zavaděč GRUB](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

<span data-ttu-id="2e772-191">**Krok 22:** vyberte odpovídající zařízení, pro instalaci spouštěcí zavaděč (pokud možno **/dev/sda**) a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2e772-191">**Step 22:** Select the appropriate device for the boot loader installation (preferably **/dev/sda**), and then select **Enter**.</span></span>

![Vyberte zařízení, které spouštěcí zavaděč instalace](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

<span data-ttu-id="2e772-193">**Krok 23:** vyberte **pokračovat**a potom vyberte **Enter** k dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="2e772-193">**Step 23:** Select **Continue**, and then select **Enter** to finish the installation.</span></span>

![Dokončení instalace](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

<span data-ttu-id="2e772-195">Po dokončení instalace, přihlaste se k virtuálnímu počítači s novými pověřeními uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e772-195">After the installation has finished, sign in to the VM with the new user credentials.</span></span> <span data-ttu-id="2e772-196">(Odkazovat na **krok 10** Další informace.)</span><span class="sxs-lookup"><span data-stu-id="2e772-196">(Refer to **Step 10** for more information.)</span></span>

<span data-ttu-id="2e772-197">Proveďte kroky, které jsou popsány v následující snímek obrazovky nastavení hesla uživatele ROOT.</span><span class="sxs-lookup"><span data-stu-id="2e772-197">Take the steps that are described in the following screenshot to set the ROOT user password.</span></span> <span data-ttu-id="2e772-198">Přihlaste se jako KOŘENOVÉ uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e772-198">Then sign in as ROOT user.</span></span>

![Nastavení KOŘENOVÉ heslo uživatele](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-the-machine-for-configuration-as-a-master-target-server"></a><span data-ttu-id="2e772-200">Příprava pro konfiguraci počítače jako hlavní cílový server</span><span class="sxs-lookup"><span data-stu-id="2e772-200">Prepare the machine for configuration as a master target server</span></span>
<span data-ttu-id="2e772-201">V dalším kroku Příprava počítače pro konfiguraci jako hlavní cílový server.</span><span class="sxs-lookup"><span data-stu-id="2e772-201">Next, prepare the machine for configuration as a master target server.</span></span>

<span data-ttu-id="2e772-202">Chcete-li získat ID pro každý SCSI pevný disk ve virtuálním počítači Linux, povolte **disku. EnableUUID = TRUE** parametr.</span><span class="sxs-lookup"><span data-stu-id="2e772-202">To get the ID for each SCSI hard disk in a Linux virtual machine, enable the **disk.EnableUUID = TRUE** parameter.</span></span>

<span data-ttu-id="2e772-203">Chcete-li tento parametr, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2e772-203">To enable this parameter, take the following steps:</span></span>

1. <span data-ttu-id="2e772-204">Vypněte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2e772-204">Shut down your virtual machine.</span></span>

2. <span data-ttu-id="2e772-205">Pravým tlačítkem na položku pro virtuální počítač v levém podokně a pak vyberte **upravit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="2e772-205">Right-click the entry for the virtual machine in the left pane, and then select **Edit Settings**.</span></span>

3. <span data-ttu-id="2e772-206">Vyberte **možnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="2e772-206">Select the **Options** tab.</span></span>

4. <span data-ttu-id="2e772-207">V levém podokně vyberte **Upřesnit** > **Obecné**a pak vyberte **parametry konfigurace** tlačítko v pravé dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="2e772-207">In the left pane, select **Advanced** > **General**, and then select the **Configuration Parameters** button on the lower-right part of the screen.</span></span>

    ![Karta Možnosti](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    <span data-ttu-id="2e772-209">**Parametry konfigurace** možnost není k dispozici, když je počítač spuštěn.</span><span class="sxs-lookup"><span data-stu-id="2e772-209">The **Configuration Parameters** option is not available when the machine is running.</span></span> <span data-ttu-id="2e772-210">Chcete-li na této kartě aktivní, vypněte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2e772-210">To make this tab active, shut down the virtual machine.</span></span>

5. <span data-ttu-id="2e772-211">Zda řádek s **disku. EnableUUID** již existuje.</span><span class="sxs-lookup"><span data-stu-id="2e772-211">See whether a row with **disk.EnableUUID** already exists.</span></span>

    - <span data-ttu-id="2e772-212">Pokud hodnota existuje a je nastavená na **False**, změňte hodnotu na **True**.</span><span class="sxs-lookup"><span data-stu-id="2e772-212">If the value exists and is set to **False**, change the value to **True**.</span></span> <span data-ttu-id="2e772-213">(Hodnoty nejsou malá a velká písmena.)</span><span class="sxs-lookup"><span data-stu-id="2e772-213">(The values are not case-sensitive.)</span></span>

    - <span data-ttu-id="2e772-214">Pokud hodnota existuje a je nastavená na **True**, vyberte **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="2e772-214">If the value exists and is set to **True**, select **Cancel**.</span></span>

    - <span data-ttu-id="2e772-215">Pokud hodnota neexistuje, vyberte **přidat řádek**.</span><span class="sxs-lookup"><span data-stu-id="2e772-215">If the value does not exist, select **Add Row**.</span></span>

    - <span data-ttu-id="2e772-216">Ve sloupci Název přidat **disku. EnableUUID**a pak nastavte hodnotu na **TRUE**.</span><span class="sxs-lookup"><span data-stu-id="2e772-216">In the name column, add **disk.EnableUUID**, and then set the value to **TRUE**.</span></span>

    ![Kontroluje, zda disk. EnableUUID již existuje.](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a><span data-ttu-id="2e772-218">Zakázat upgrady jádra</span><span class="sxs-lookup"><span data-stu-id="2e772-218">Disable kernel upgrades</span></span>

<span data-ttu-id="2e772-219">Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi Ubuntu, zkontrolujte, zda jsou pro virtuální počítač vypnutá upgrady jádra.</span><span class="sxs-lookup"><span data-stu-id="2e772-219">Azure Site Recovery master target server requires a very specific version of the Ubuntu, ensure that the kernel upgrades are disabled for the virtual machine.</span></span>

<span data-ttu-id="2e772-220">Pokud upgrady jádra jsou povolené, jakékoli regulární upgrady způsobit hlavní cílový server fungovat správně.</span><span class="sxs-lookup"><span data-stu-id="2e772-220">If kernel upgrades are enabled, then any regular upgrades cause the master target server to malfunction.</span></span>

#### <a name="download-and-install-additional-packages"></a><span data-ttu-id="2e772-221">Stáhněte a nainstalujte další balíčky</span><span class="sxs-lookup"><span data-stu-id="2e772-221">Download and install additional packages</span></span>

> [!NOTE]
> <span data-ttu-id="2e772-222">Ujistěte se, že máte připojení k Internetu stáhněte a nainstalujte další balíčky.</span><span class="sxs-lookup"><span data-stu-id="2e772-222">Make sure that you have Internet connectivity to download and install additional packages.</span></span> <span data-ttu-id="2e772-223">Pokud nemáte připojení k Internetu, budete muset ručně najít tyto balíčky ot. / min a nainstalovat je.</span><span class="sxs-lookup"><span data-stu-id="2e772-223">If you don't have Internet connectivity, you need to manually find these RPM packages and install them.</span></span>

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-the-installer-for-setup"></a><span data-ttu-id="2e772-224">Získání Instalační program pro instalaci</span><span class="sxs-lookup"><span data-stu-id="2e772-224">Get the installer for setup</span></span>

<span data-ttu-id="2e772-225">Pokud hlavní cíl má připojení k Internetu, můžete použít následující kroky stáhnout instalační program.</span><span class="sxs-lookup"><span data-stu-id="2e772-225">If your master target has Internet connectivity, you can use the following steps to download the installer.</span></span> <span data-ttu-id="2e772-226">Jinak můžete zkopírujte instalační službu z procesového serveru a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="2e772-226">Otherwise, you can copy the installer from the process server and then install it.</span></span>

#### <a name="download-the-master-target-installation-packages"></a><span data-ttu-id="2e772-227">Stáhněte si instalační balíčky hlavní cíl</span><span class="sxs-lookup"><span data-stu-id="2e772-227">Download the master target installation packages</span></span>

<span data-ttu-id="2e772-228">[Stáhněte si nejnovější bits instalace hlavní cíl Linux](https://aka.ms/latestlinuxmobsvc).</span><span class="sxs-lookup"><span data-stu-id="2e772-228">[Download the latest Linux master target installation bits](https://aka.ms/latestlinuxmobsvc).</span></span>

<span data-ttu-id="2e772-229">Chcete-li stáhnout ji pomocí Linux, zadejte:</span><span class="sxs-lookup"><span data-stu-id="2e772-229">To download it by using Linux, type:</span></span>

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

<span data-ttu-id="2e772-230">Ujistěte se, stáhněte a rozbalte instalační program v domovském adresáři.</span><span class="sxs-lookup"><span data-stu-id="2e772-230">Make sure that you download and unzip the installer in your home directory.</span></span> <span data-ttu-id="2e772-231">Pokud rozbalte k **/usr/místní**, instalace selže.</span><span class="sxs-lookup"><span data-stu-id="2e772-231">If you unzip to **/usr/Local**, then the installation  fails.</span></span>


#### <a name="access-the-installer-from-the-process-server"></a><span data-ttu-id="2e772-232">Přístup k Instalační program z procesového serveru</span><span class="sxs-lookup"><span data-stu-id="2e772-232">Access the installer from the process server</span></span>

1. <span data-ttu-id="2e772-233">Na procesní server, přejděte na **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span><span class="sxs-lookup"><span data-stu-id="2e772-233">On the process server, go to **C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span></span>

2. <span data-ttu-id="2e772-234">Instalační program vyžaduje soubor zkopírovat z procesového serveru a uložte ho jako **latestlinuxmobsvc.tar.gz** v domovském adresáři.</span><span class="sxs-lookup"><span data-stu-id="2e772-234">Copy the required installer file from the process server, and save it as **latestlinuxmobsvc.tar.gz** in your home directory.</span></span>


### <a name="apply-custom-configuration-changes"></a><span data-ttu-id="2e772-235">Změny vlastní konfigurace</span><span class="sxs-lookup"><span data-stu-id="2e772-235">Apply custom configuration changes</span></span>

<span data-ttu-id="2e772-236">Chcete-li použít změny v vlastní konfigurace, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2e772-236">To apply custom configuration changes, use the following steps:</span></span>


1. <span data-ttu-id="2e772-237">Spusťte následující příkaz, který untar binárního souboru.</span><span class="sxs-lookup"><span data-stu-id="2e772-237">Run the following command to untar the binary.</span></span>
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Snímek obrazovky příkaz ke spuštění](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. <span data-ttu-id="2e772-239">Spusťte následující příkaz, který udělit oprávnění.</span><span class="sxs-lookup"><span data-stu-id="2e772-239">Run the following command to give permission.</span></span>
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. <span data-ttu-id="2e772-240">Spusťte následující příkaz pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="2e772-240">Run the following command to run the script.</span></span>
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> <span data-ttu-id="2e772-241">Spusťte skript jenom jednou na serveru.</span><span class="sxs-lookup"><span data-stu-id="2e772-241">Run the script only once on the server.</span></span> <span data-ttu-id="2e772-242">Vypněte server.</span><span class="sxs-lookup"><span data-stu-id="2e772-242">Shut down the server.</span></span> <span data-ttu-id="2e772-243">Restartujte server po přidat disk, jak je popsáno v následující části.</span><span class="sxs-lookup"><span data-stu-id="2e772-243">Then restart the server after you add a disk, as described in the next section.</span></span>

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a><span data-ttu-id="2e772-244">Přidejte uchování disk k virtuálnímu počítači hlavního cíle Linuxu</span><span class="sxs-lookup"><span data-stu-id="2e772-244">Add a retention disk to the Linux master target virtual machine</span></span>

<span data-ttu-id="2e772-245">Pomocí následujících kroků můžete vytvořit disku pro uchování:</span><span class="sxs-lookup"><span data-stu-id="2e772-245">Use the following steps to create a retention disk:</span></span>

1. <span data-ttu-id="2e772-246">Připojit nový disk 1 TB k virtuálnímu počítači hlavního cíle Linuxu a potom počítač spustit.</span><span class="sxs-lookup"><span data-stu-id="2e772-246">Attach a new 1-TB disk to the Linux master target virtual machine, and then start the machine.</span></span>

2. <span data-ttu-id="2e772-247">Použití **vícenásobný -udou** příkaz Další funkce multipath ID disku pro uchování.</span><span class="sxs-lookup"><span data-stu-id="2e772-247">Use the **multipath -ll** command to learn the multipath ID of the retention disk.</span></span>

    ```
    multipath -ll
    ```
    ![Vícenásobný ID disku pro uchování](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. <span data-ttu-id="2e772-249">Formátování disku a pak vytvořit systém souborů na nový disk.</span><span class="sxs-lookup"><span data-stu-id="2e772-249">Format the drive, and then create a file system on the new drive.</span></span>

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Vytvoření systému souborů na disku](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. <span data-ttu-id="2e772-251">Po vytvoření systému souborů, připojte disk uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="2e772-251">After you create the file system, mount the retention disk.</span></span>
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Připojení disku pro uchování](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. <span data-ttu-id="2e772-253">Vytvořte **fstab** položka připojit jednotka pro uchování pokaždé, když bude systém při spuštění.</span><span class="sxs-lookup"><span data-stu-id="2e772-253">Create the **fstab** entry to mount the retention drive every time the system starts.</span></span>
    ```
    vi /etc/fstab
    ```
    <span data-ttu-id="2e772-254">Vyberte **vložit** zahájíte úpravy souboru.</span><span class="sxs-lookup"><span data-stu-id="2e772-254">Select **Insert** to begin editing the file.</span></span> <span data-ttu-id="2e772-255">Vytvořte nový řádek a potom vložte následující text.</span><span class="sxs-lookup"><span data-stu-id="2e772-255">Create a new line, and then insert the following text.</span></span> <span data-ttu-id="2e772-256">Upravte vícenásobný ID disku na základě Identifikátoru zvýrazněná více cest z předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="2e772-256">Edit the disk multipath ID based on the highlighted multipath ID from the previous command.</span></span>

    <span data-ttu-id="2e772-257"> **/dev/mapper/ <Retention disks multipath id> /mnt/uchování ext4 rw 0 0**</span><span class="sxs-lookup"><span data-stu-id="2e772-257">**/dev/mapper/<Retention disks multipath id> /mnt/retention ext4 rw 0 0**</span></span>

    <span data-ttu-id="2e772-258">Vyberte **Esc**a pak zadejte **: QW** (zápisu a ukončení) zavřete okno editor.</span><span class="sxs-lookup"><span data-stu-id="2e772-258">Select **Esc**, and then type **:wq** (write and quit) to close the editor window.</span></span>

### <a name="install-the-master-target"></a><span data-ttu-id="2e772-259">Nainstalujte na hlavním cíli</span><span class="sxs-lookup"><span data-stu-id="2e772-259">Install the master target</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2e772-260">Verze hlavního cílového serveru musí být rovna nebo starší než verze procesového serveru a konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="2e772-260">The version of the master target server must be equal to or earlier than the versions of the process server and the configuration server.</span></span> <span data-ttu-id="2e772-261">Pokud není tato podmínka splněná, opětovné ochrany úspěšná, ale replikace selže.</span><span class="sxs-lookup"><span data-stu-id="2e772-261">If this condition is not met, reprotect succeeds, but replication fails.</span></span>


> [!NOTE]
> <span data-ttu-id="2e772-262">Než nainstalujete hlavní cílový server, zkontrolujte, zda **/etc/hosts** souboru na virtuální počítač obsahuje položky, které mapují názvem místního hostitele na IP adresy, které jsou přidruženy všechny síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="2e772-262">Before you install the master target server, check that the **/etc/hosts** file on the virtual machine contains entries that map the local hostname to the IP addresses that are associated with all network adapters.</span></span>

1. <span data-ttu-id="2e772-263">Kopírovat heslo z **C:\ProgramData\Microsoft Azure lokality Recovery\private\connection.passphrase** na konfiguračním serveru.</span><span class="sxs-lookup"><span data-stu-id="2e772-263">Copy the passphrase from **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** on the configuration server.</span></span> <span data-ttu-id="2e772-264">Potom uložte ho jako **passphrase.txt** ve stejném adresáři místní spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2e772-264">Then save it as **passphrase.txt** in the same local directory by running the following command:</span></span>

    ```
    echo <passphrase> >passphrase.txt
    ```
    <span data-ttu-id="2e772-265">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2e772-265">Example:</span></span> 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. <span data-ttu-id="2e772-266">Všimněte si, IP adresa konfiguračního serveru.</span><span class="sxs-lookup"><span data-stu-id="2e772-266">Note the configuration server's IP address.</span></span> <span data-ttu-id="2e772-267">Budete ho potřebovat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="2e772-267">You need it in the next step.</span></span>

3. <span data-ttu-id="2e772-268">Spusťte následující příkaz k instalaci hlavní cílový server a registrace serveru u konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="2e772-268">Run the following command to install the master target server and register the server with the configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    <span data-ttu-id="2e772-269">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2e772-269">Example:</span></span> 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    <span data-ttu-id="2e772-270">Počkejte na dokončení skriptu.</span><span class="sxs-lookup"><span data-stu-id="2e772-270">Wait until the script finishes.</span></span> <span data-ttu-id="2e772-271">Pokud se hlavní cíl zaregistruje úspěšně, se hlavní cíl je uvedený na **infrastruktura Site Recovery** stránce portálu.</span><span class="sxs-lookup"><span data-stu-id="2e772-271">If the master target registers sucessfully, the master target is listed on the **Site Recovery Infrastructure** page of the portal.</span></span>


#### <a name="install-the-master-target-by-using-interactive-installation"></a><span data-ttu-id="2e772-272">Instalaci se hlavní cíl pomocí interaktivní instalace</span><span class="sxs-lookup"><span data-stu-id="2e772-272">Install the master target by using interactive installation</span></span>

1. <span data-ttu-id="2e772-273">Spusťte následující příkaz k instalaci na hlavním cíli.</span><span class="sxs-lookup"><span data-stu-id="2e772-273">Run the following command to install the master target.</span></span> <span data-ttu-id="2e772-274">Pro roli agenta, vyberte **hlavního cíle**.</span><span class="sxs-lookup"><span data-stu-id="2e772-274">For the agent role, choose **Master Target**.</span></span>

    ```
    ./install
    ```

2. <span data-ttu-id="2e772-275">Vyberte výchozí umístění pro instalaci a potom vyberte **Enter** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="2e772-275">Choose the default location for installation, and then select **Enter** to continue.</span></span>

    ![Volba výchozí umístění pro instalaci hlavní cíl](./media/site-recovery-how-to-install-linux-master-target/image17.png)

<span data-ttu-id="2e772-277">Po dokončení instalace zaregistrujte konfigurační server pomocí příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="2e772-277">After the installation has finished, register the configuration server by using the command line.</span></span>

1. <span data-ttu-id="2e772-278">Všimněte si IP adresu konfiguračního serveru.</span><span class="sxs-lookup"><span data-stu-id="2e772-278">Note the IP address of the configuration server.</span></span> <span data-ttu-id="2e772-279">Budete ho potřebovat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="2e772-279">You need it in the next step.</span></span>

2. <span data-ttu-id="2e772-280">Spusťte následující příkaz k instalaci hlavní cílový server a registrace serveru u konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="2e772-280">Run the following command to install the master target server and register the server with the configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    <span data-ttu-id="2e772-281">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2e772-281">Example:</span></span> 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   <span data-ttu-id="2e772-282">Počkejte na dokončení skriptu.</span><span class="sxs-lookup"><span data-stu-id="2e772-282">Wait until the script finishes.</span></span> <span data-ttu-id="2e772-283">Pokud se hlavní cíl je úspěšně registrovaná, se hlavní cíl je uvedený na **infrastruktura Site Recovery** na portálu.</span><span class="sxs-lookup"><span data-stu-id="2e772-283">If the master target is registered succesfully, the master target is listed on the **Site Recovery Infrastructure** page of the portal.</span></span>


### <a name="upgrade-the-master-target"></a><span data-ttu-id="2e772-284">Upgrade na hlavním cíli</span><span class="sxs-lookup"><span data-stu-id="2e772-284">Upgrade the master target</span></span>

<span data-ttu-id="2e772-285">Spusťte instalační program.</span><span class="sxs-lookup"><span data-stu-id="2e772-285">Run the installer.</span></span> <span data-ttu-id="2e772-286">Automaticky zjišťuje, zda je agent nainstalovaný na hlavním cíli.</span><span class="sxs-lookup"><span data-stu-id="2e772-286">It automatically detects that the agent is installed on the master target.</span></span> <span data-ttu-id="2e772-287">Pokud chcete upgradovat, vyberte **Y**.  Po dokončení instalace, zkontrolujte verzi hlavního cíle nainstalován pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="2e772-287">To upgrade, select **Y**.  After the setup has been completed, check the version of the master target installed by using the following command.</span></span>

    ```
    cat /usr/local/.vx_version
    ```

<span data-ttu-id="2e772-288">Uvidíte, že **verze** pole obsahuje číslo verze se hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="2e772-288">You can see that the **Version** field gives the version number of the master target.</span></span>

### <a name="install-vmware-tools-on-the-master-target-server"></a><span data-ttu-id="2e772-289">Nainstalujte nástroje VMware na hlavním cílovém serveru</span><span class="sxs-lookup"><span data-stu-id="2e772-289">Install VMware tools on the master target server</span></span>

<span data-ttu-id="2e772-290">Musíte nainstalovat nástroje VMware na hlavním cíli, aby ho můžete zjistit datová úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e772-290">You need to install VMware tools on the master target so that it can discover the data stores.</span></span> <span data-ttu-id="2e772-291">Pokud nejsou nainstalovány nástroje, není v úložištích dat, uvedené na obrazovce opětovné ochrany.</span><span class="sxs-lookup"><span data-stu-id="2e772-291">If the tools are not installed, the reprotect screen isn't listed in the data stores.</span></span> <span data-ttu-id="2e772-292">Po instalaci nástroje VMware je potřeba restartovat.</span><span class="sxs-lookup"><span data-stu-id="2e772-292">After installation of the VMware tools, you need to restart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e772-293">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e772-293">Next steps</span></span>
<span data-ttu-id="2e772-294">Po instalaci a registraci hlavního cíle má finsihed, zobrazí se zobrazí na hlavním cíli **hlavního cíle** kapitoly **infrastruktura Site Recovery**, v části Přehled konfigurace serveru.</span><span class="sxs-lookup"><span data-stu-id="2e772-294">After the installation and registration of the master target has finsihed, you can see the master target appear on the **Master Target** section in **Site Recovery Infrastructure**, under the configuration server overview.</span></span>

<span data-ttu-id="2e772-295">Teď můžete pokračovat s [vytvoření](site-recovery-how-to-reprotect.md), za nímž následují navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="2e772-295">You can now proceed with [reprotection](site-recovery-how-to-reprotect.md), followed by failback.</span></span>

## <a name="common-issues"></a><span data-ttu-id="2e772-296">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="2e772-296">Common issues</span></span>

* <span data-ttu-id="2e772-297">Ujistěte se, že jste nezapínejte řešení Storage vMotion na žádné součásti správy, jako je hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="2e772-297">Make sure you do not turn on Storage vMotion on any management components such as a master target.</span></span> <span data-ttu-id="2e772-298">Pokud se hlavní cíl přesune po úspěšné opětovné ochrany, disků virtuálního počítače (VMDKs) nelze odpojit.</span><span class="sxs-lookup"><span data-stu-id="2e772-298">If the master target moves after a successful reprotect, the virtual machine disks (VMDKs) cannot be detached.</span></span> <span data-ttu-id="2e772-299">V takovém případě navrácení služeb po obnovení selže.</span><span class="sxs-lookup"><span data-stu-id="2e772-299">In this case, failback fails.</span></span>

* <span data-ttu-id="2e772-300">Hlavní cíl by neměl mít všechny snímky na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="2e772-300">The master target should not have any snapshots on the virtual machine.</span></span> <span data-ttu-id="2e772-301">Pokud existují snímky, navrácení služeb po obnovení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="2e772-301">If there are snapshots, failback fails.</span></span>

* <span data-ttu-id="2e772-302">Z důvodu některých vlastních konfigurací síťový adaptér v někteří zákazníci síťové rozhraní je zakázané během spouštění a nemůže inicializovat agenta hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="2e772-302">Due to some custom NIC configurations at some customers, the network interface is disabled during startup, and the master target agent cannot initialize.</span></span> <span data-ttu-id="2e772-303">Ujistěte se, že následující vlastnosti jsou správně nastaveny.</span><span class="sxs-lookup"><span data-stu-id="2e772-303">Make sure that the following properties are correctly set.</span></span> <span data-ttu-id="2e772-304">Zkontrolujte tyto vlastnosti v Ethernet karty souboru /etc/sysconfig/network-scripts/ifcfg-eth *.</span><span class="sxs-lookup"><span data-stu-id="2e772-304">Check these properties in the Ethernet card file's /etc/sysconfig/network-scripts/ifcfg-eth*.</span></span>
    * <span data-ttu-id="2e772-305">BOOTPROTO = dhcp</span><span class="sxs-lookup"><span data-stu-id="2e772-305">BOOTPROTO=dhcp</span></span>
    * <span data-ttu-id="2e772-306">ONBOOT = Ano</span><span class="sxs-lookup"><span data-stu-id="2e772-306">ONBOOT=yes</span></span>
