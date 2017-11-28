---
title: "aaaHow tooinstall převzetí služeb při selhání z Azure tooon místní hlavní cílový server Linux | Microsoft Docs"
description: "Před opětovnou ochranu virtuální počítač s Linuxem, potřebujete hlavní cílový server Linux. Zjistěte, jak tooinstall jeden."
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
ms.openlocfilehash: d7c55d115712b9862414979f89efb1f177c5f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-linux-master-target-server"></a><span data-ttu-id="a7ef4-104">Instalovat hlavní cílový server Linux</span><span class="sxs-lookup"><span data-stu-id="a7ef4-104">Install a Linux master target server</span></span>
<span data-ttu-id="a7ef4-105">Po selhání virtuálních počítačů, může selhat back hello virtuální počítače toohello místního webu.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-105">After you fail over your virtual machines, you can fail back hello virtual machines toohello on-premises site.</span></span> <span data-ttu-id="a7ef4-106">toofail zpět, je nutné tooreprotect hello virtuální počítač z Azure toohello místního webu.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-106">toofail back, you need tooreprotect hello virtual machine from Azure toohello on-premises site.</span></span> <span data-ttu-id="a7ef4-107">Pro tento proces budete potřebovat místní hlavní cílový server tooreceive hello provoz.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-107">For this process, you need an on-premises master target server tooreceive hello traffic.</span></span> 

<span data-ttu-id="a7ef4-108">Pokud chráněný virtuální počítač je virtuálního počítače s Windows, musíte Windows hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-108">If your protected virtual machine is a Windows virtual machine, then you need a Windows master target.</span></span> <span data-ttu-id="a7ef4-109">Pro virtuální počítač s Linuxem budete potřebovat hlavního cíle Linuxu.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-109">For a Linux virtual machine, you need a Linux master target.</span></span> <span data-ttu-id="a7ef4-110">Čtení hello následující kroky toolearn jak toocreate a nainstalujte systémem Linux hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-110">Read hello following steps toolearn how toocreate and install a Linux master target.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a7ef4-111">Od verze hello 9.10.0 hlavní cílový server, můžete pouze nainstalovány hello nejnovější hlavní cílový server na serveru Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-111">Starting with release of hello 9.10.0 master target server, hello latest master target server can be only installed on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="a7ef4-112">Nové instalace nejsou povoleny u CentOS6.6 servery.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-112">New installations aren't allowed on  CentOS6.6 servers.</span></span> <span data-ttu-id="a7ef4-113">Však můžete dál tooupgrade vaše starého hlavního cílové servery pomocí hello 9.10.0 verze.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-113">However, you can continue tooupgrade your old master target servers by using hello 9.10.0 version.</span></span>

## <a name="overview"></a><span data-ttu-id="a7ef4-114">Přehled</span><span class="sxs-lookup"><span data-stu-id="a7ef4-114">Overview</span></span>
<span data-ttu-id="a7ef4-115">Tento článek obsahuje pokyny, jak tooinstall systémem Linux hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-115">This article provides instructions for how tooinstall a Linux master target.</span></span>

<span data-ttu-id="a7ef4-116">POST dotazy nebo připomínky můžete na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="a7ef4-116">Post comments or questions at hello end of this article or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7ef4-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a7ef4-117">Prerequisites</span></span>

* <span data-ttu-id="a7ef4-118">určení toochoose hello hostitele na hello hlavní cíl které toodeploy, pokud hello navrácení služeb po obnovení budete toobe tooan existující místní virtuální počítač nebo tooa nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-118">toochoose hello host on which toodeploy hello master target, determine if hello failback is going toobe tooan existing on-premises virtual machine or tooa new virtual machine.</span></span> 
    * <span data-ttu-id="a7ef4-119">Pro existující virtuální počítač hostitele hello hello hlavní cíl musí mít přístup k úložišti dat toohello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-119">For an existing virtual machine, hello host of hello master target should have access toohello data stores of hello virtual machine.</span></span>
    * <span data-ttu-id="a7ef4-120">Pokud hello na místním virtuálním počítači neexistuje, je na stejné hostitele jako hlavní cíl hello hello vytvořit hello navrácení služeb po obnovení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-120">If hello on-premises virtual machine does not exist, hello failback virtual machine is created on hello same host as hello master target.</span></span> <span data-ttu-id="a7ef4-121">Můžete vybrat libovolného hostitele ESXi tooinstall hello hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-121">You can choose any ESXi host tooinstall hello master target.</span></span>
* <span data-ttu-id="a7ef4-122">Hello hlavní cíl musí být v síti, který může komunikovat s hello procesový server a hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-122">hello master target should be on a network that can communicate with hello process server and hello configuration server.</span></span>
* <span data-ttu-id="a7ef4-123">Hello verzi hello hlavní cíl musí být rovna tooor starší než verze hello hello procesový server a hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-123">hello version of hello master target must be equal tooor earlier than hello versions of hello process server and hello configuration server.</span></span> <span data-ttu-id="a7ef4-124">Například pokud hello verzi hello konfigurační server je 9.4, hello verzi hello hlavního cíle může být 9.4 nebo 9.3, ale není 9.5.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-124">For example, if hello version of hello configuration server is 9.4, hello version of hello master target can be 9.4 or 9.3 but not 9.5.</span></span>
* <span data-ttu-id="a7ef4-125">hlavní cíl Hello lze pouze virtuální počítač VMware, nikoli na fyzický server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-125">hello master target can only be a VMware virtual machine and not a physical server.</span></span>

## <a name="create-hello-master-target-according-toohello-sizing-guidelines"></a><span data-ttu-id="a7ef4-126">Vytvořit hlavní cíl hello toohello podle pokynů pro změnu velikosti</span><span class="sxs-lookup"><span data-stu-id="a7ef4-126">Create hello master target according toohello sizing guidelines</span></span>

<span data-ttu-id="a7ef4-127">Vytvořte hlavní cíl hello v souladu s hello následující pokyny k nastavení velikosti:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-127">Create hello master target in accordance with hello following sizing guidelines:</span></span>
- <span data-ttu-id="a7ef4-128">**Paměť RAM**: 6 GB nebo více</span><span class="sxs-lookup"><span data-stu-id="a7ef4-128">**RAM**: 6 GB or more</span></span>
- <span data-ttu-id="a7ef4-129">**Velikost disku operačního systému**: 100 GB nebo více (tooinstall CentOS6.6)</span><span class="sxs-lookup"><span data-stu-id="a7ef4-129">**OS disk size**: 100 GB or more (tooinstall CentOS6.6)</span></span>
- <span data-ttu-id="a7ef4-130">**Velikost disku Další jednotka pro uchování**: 1 TB</span><span class="sxs-lookup"><span data-stu-id="a7ef4-130">**Additional disk size for retention drive**: 1 TB</span></span>
- <span data-ttu-id="a7ef4-131">**Jader procesoru**: 4 jádra nebo více</span><span class="sxs-lookup"><span data-stu-id="a7ef4-131">**CPU cores**: 4 cores or more</span></span>

<span data-ttu-id="a7ef4-132">Následující Hello podporována Ubuntu jádra jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-132">hello following supported Ubuntu kernels are supported.</span></span>


|<span data-ttu-id="a7ef4-133">Řada jádra</span><span class="sxs-lookup"><span data-stu-id="a7ef4-133">Kernel Series</span></span>  |<span data-ttu-id="a7ef4-134">Podporovat až příliš</span><span class="sxs-lookup"><span data-stu-id="a7ef4-134">Support up too</span></span> |
|---------|---------|
|<span data-ttu-id="a7ef4-135">4.4</span><span class="sxs-lookup"><span data-stu-id="a7ef4-135">4.4</span></span>      |<span data-ttu-id="a7ef4-136">4.4.0-81-Generic</span><span class="sxs-lookup"><span data-stu-id="a7ef4-136">4.4.0-81-generic</span></span>         |
|<span data-ttu-id="a7ef4-137">4.8</span><span class="sxs-lookup"><span data-stu-id="a7ef4-137">4.8</span></span>      |<span data-ttu-id="a7ef4-138">4.8.0-56-Generic</span><span class="sxs-lookup"><span data-stu-id="a7ef4-138">4.8.0-56-generic</span></span>         |
|<span data-ttu-id="a7ef4-139">4.10</span><span class="sxs-lookup"><span data-stu-id="a7ef4-139">4.10</span></span>     |<span data-ttu-id="a7ef4-140">4.10.0-24-Generic</span><span class="sxs-lookup"><span data-stu-id="a7ef4-140">4.10.0-24-generic</span></span>        |


## <a name="deploy-hello-master-target-server"></a><span data-ttu-id="a7ef4-141">Nasazení hello hlavní cílový server</span><span class="sxs-lookup"><span data-stu-id="a7ef4-141">Deploy hello master target server</span></span>

### <a name="install-ubuntu-16042-minimal"></a><span data-ttu-id="a7ef4-142">Nainstalujte Ubuntu 16.04.2 minimální</span><span class="sxs-lookup"><span data-stu-id="a7ef4-142">Install Ubuntu 16.04.2 Minimal</span></span>

<span data-ttu-id="a7ef4-143">Trvat hello následující hello kroky tooinstall hello Ubuntu 16.04.2 64bitový operační systém.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-143">Take hello following hello steps tooinstall hello Ubuntu 16.04.2 64-bit operating system.</span></span>

<span data-ttu-id="a7ef4-144">**Krok 1:** přejděte toohello [stáhnout odkaz](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) a zvolte nejbližší zrcadlení hello z které stáhnout soubor ISO Ubuntu 16.04.2 minimální 64-bit.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-144">**Step 1:** Go toohello [download link](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) and choose hello closest mirror from which download an Ubuntu 16.04.2 minimal 64-bit ISO.</span></span>

<span data-ttu-id="a7ef4-145">Zachovat Ubuntu 16.04.2 minimální 64-bit ISO v jednotce hello DVD a spusťte hello systému.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-145">Keep an Ubuntu 16.04.2 minimal 64-bit ISO in hello DVD drive and start hello system.</span></span>

<span data-ttu-id="a7ef4-146">**Krok 2:** vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-146">**Step 2:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Výběr jazyka](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

<span data-ttu-id="a7ef4-148">**Krok 3:** vyberte **nainstalovat Ubuntu Server**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-148">**Step 3:** Select **Install Ubuntu Server**, and then select **Enter**.</span></span>

![Vyberte možnost instalace Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

<span data-ttu-id="a7ef4-150">**Krok 4:** vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-150">**Step 4:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Vyberte angličtina jako upřednostňovaný jazyk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

<span data-ttu-id="a7ef4-152">**Krok 5:** hello vyberte příslušnou možnost z hello **časové pásmo** seznam možností a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-152">**Step 5:** Select hello appropriate option from hello **Time Zone** options list, and then select **Enter**.</span></span>

![Vyberte hello správné časové pásmo](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

<span data-ttu-id="a7ef4-154">**Krok 6:** vyberte **ne** (hello výchozí možnost) a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-154">**Step 6:** Select **No** (hello default option), and then select **Enter**.</span></span>


![Konfigurace hello klávesnice](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

<span data-ttu-id="a7ef4-156">**Krok 7:** vyberte **angličtinu (US)** jako hello země původu hello klávesnice a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-156">**Step 7:** Select **English (US)** as hello country of origin for hello keyboard, and then select **Enter**.</span></span>

![Vyberte USA jako hello země původu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

<span data-ttu-id="a7ef4-158">**Krok 8:** vyberte **angličtinu (US)** jako hello rozložení klávesnice a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-158">**Step 8:** Select **English (US)** as hello keyboard layout, and then select **Enter**.</span></span>

![Vyberte angličtinu jako hello rozložení klávesnice](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

<span data-ttu-id="a7ef4-160">**Krok 9:** zadejte hello název hostitele pro server v hello **Hostname** a pak vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-160">**Step 9:** Enter hello hostname for your server in hello **Hostname** box, and then select **Continue**.</span></span>

![Zadejte název hostitele hello pro váš server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

<span data-ttu-id="a7ef4-162">**Krok 10:** toocreate uživatelský účet, zadejte hello uživatelské jméno a potom vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-162">**Step 10:** toocreate a user account, enter hello user name, and then select **Continue**.</span></span>

![Vytvoření uživatelského účtu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

<span data-ttu-id="a7ef4-164">**Krok 11:** zadejte heslo hello hello nový uživatelský účet a potom vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-164">**Step 11:** Enter hello password for hello new user account, and then select **Continue**.</span></span>

![Zadejte heslo hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

<span data-ttu-id="a7ef4-166">**Krok 12:** potvrďte hello heslo pro nového uživatele hello a pak vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-166">**Step 12:** Confirm hello password for hello new user, and then select **Continue**.</span></span>

![Potvrzení hesla hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

<span data-ttu-id="a7ef4-168">**Krok 13:** vyberte **ne** (hello výchozí možnost) a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-168">**Step 13:** Select **No** (hello default option), and then select **Enter**.</span></span>

![Nastavit uživatele a hesla](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

<span data-ttu-id="a7ef4-170">**Krok 14:** Pokud hello časové pásmo, které se zobrazí správný, vyberte **Ano** (hello výchozí možnost) a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-170">**Step 14:** If hello time zone that's displayed is correct, select **Yes** (hello default option), and then select **Enter**.</span></span>

<span data-ttu-id="a7ef4-171">tooreconfigure svým časovým pásmem, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-171">tooreconfigure your time zone, select **No**.</span></span>

![Nakonfigurujte hodiny hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

<span data-ttu-id="a7ef4-173">**Krok 15:** hello dělení možnosti metody, vyberte **na základě - použít celý disk**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-173">**Step 15:** From hello partitioning method options, select **Guided - use entire disk**, and then select **Enter**.</span></span>

![Vyberte hello dělení možnost – Metoda](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

<span data-ttu-id="a7ef4-175">**Krok 16:** vyberte hello příslušný disk z hello **vyberte disk toopartition** možnosti a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-175">**Step 16:** Select hello appropriate disk from hello **Select disk toopartition** options, and then select **Enter**.</span></span>


![Vyberte hello disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

<span data-ttu-id="a7ef4-177">**Krok 17:** vyberte **Ano** toowrite hello toodisk změny a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-177">**Step 17:** Select **Yes** toowrite hello changes toodisk, and then select **Enter**.</span></span>

![Zápis změn toodisk hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

<span data-ttu-id="a7ef4-179">**Krok 18:** vyberte hello výchozí možnost, vyberte **pokračovat**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-179">**Step 18:** Select hello default option, select **Continue**, and then select **Enter**.</span></span>

![Vyberte možnost Výchozí hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

<span data-ttu-id="a7ef4-181">**Krok 19:** vyberte příslušnou možnost hello pro správu upgrady systému a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-181">**Step 19:** Select hello appropriate option for managing upgrades on your system, and then select **Enter**.</span></span>

![Vyberte, jak toomanage upgradu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> <span data-ttu-id="a7ef4-183">Protože hello Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi hello Ubuntu, je třeba tooensure této hello jádra, které jsou zakázány upgrady pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-183">Because hello Azure Site Recovery master target server requires a very specific version of hello Ubuntu, you need tooensure that hello kernel upgrades are disabled for hello virtual machine.</span></span> <span data-ttu-id="a7ef4-184">Pokud se povolí, nějaké regulární upgrady způsobit hello hlavní cílový server toomalfunction.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-184">If they are enabled, then any regular upgrades cause hello master target server toomalfunction.</span></span> <span data-ttu-id="a7ef4-185">Zkontrolujte, zda jste vybrali hello **žádné automatické aktualizace** možnost.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-185">Make sure you select hello **No automatic updates** option.</span></span>


<span data-ttu-id="a7ef4-186">**Krok 20:** vyberte výchozí možnosti.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-186">**Step 20:** Select default options.</span></span> <span data-ttu-id="a7ef4-187">Pokud chcete openSSH pro připojení SSH, vyberte hello **OpenSSH server** a pak vyberte možnost **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-187">If you want openSSH for SSH connect, select hello **OpenSSH server** option, and then select **Continue**.</span></span>

![Vyberte software](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

<span data-ttu-id="a7ef4-189">**Krok 21:** vyberte **Ano**a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-189">**Step 21:** Select **Yes**, and then select **Enter**.</span></span>

![Isntall hello GRUB spouštěcí zavaděč](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

<span data-ttu-id="a7ef4-191">**Krok 22:** hello vyberte příslušné zařízení pro hello spouštěcí zavaděč instalace (pokud možno **/dev/sda**) a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-191">**Step 22:** Select hello appropriate device for hello boot loader installation (preferably **/dev/sda**), and then select **Enter**.</span></span>

![Vyberte zařízení, které spouštěcí zavaděč instalace](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

<span data-ttu-id="a7ef4-193">**Krok 23:** vyberte **pokračovat**a potom vyberte **Enter** toofinish hello instalace.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-193">**Step 23:** Select **Continue**, and then select **Enter** toofinish hello installation.</span></span>

![Dokončení instalace hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

<span data-ttu-id="a7ef4-195">Po dokončení instalace hello přihlaste toohello virtuálních počítačů s novými pověřeními uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-195">After hello installation has finished, sign in toohello VM with hello new user credentials.</span></span> <span data-ttu-id="a7ef4-196">(Odkazovat příliš**krok 10** Další informace.)</span><span class="sxs-lookup"><span data-stu-id="a7ef4-196">(Refer too**Step 10** for more information.)</span></span>

<span data-ttu-id="a7ef4-197">Trvat hello kroky, které jsou popsané v následující snímek obrazovky tooset hello KOŘENOVÉ hello heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-197">Take hello steps that are described in hello following screenshot tooset hello ROOT user password.</span></span> <span data-ttu-id="a7ef4-198">Přihlaste se jako KOŘENOVÉ uživatele.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-198">Then sign in as ROOT user.</span></span>

![Heslo uživatele ROOT hello sady](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-hello-machine-for-configuration-as-a-master-target-server"></a><span data-ttu-id="a7ef4-200">Příprava hello počítače pro konfiguraci jako hlavní cílový server</span><span class="sxs-lookup"><span data-stu-id="a7ef4-200">Prepare hello machine for configuration as a master target server</span></span>
<span data-ttu-id="a7ef4-201">V dalším kroku Příprava hello počítače pro konfiguraci jako hlavní cílový server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-201">Next, prepare hello machine for configuration as a master target server.</span></span>

<span data-ttu-id="a7ef4-202">tooget hello ID pro každý pevného disku SCSI na virtuálním počítači Linux povolit hello **disku. EnableUUID = TRUE** parametr.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-202">tooget hello ID for each SCSI hard disk in a Linux virtual machine, enable hello **disk.EnableUUID = TRUE** parameter.</span></span>

<span data-ttu-id="a7ef4-203">tooenable, které tento parametr hello proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-203">tooenable this parameter, take hello following steps:</span></span>

1. <span data-ttu-id="a7ef4-204">Vypněte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-204">Shut down your virtual machine.</span></span>

2. <span data-ttu-id="a7ef4-205">Klikněte pravým tlačítkem na položku hello pro hello virtuální počítač v levém podokně hello a potom vyberte **upravit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-205">Right-click hello entry for hello virtual machine in hello left pane, and then select **Edit Settings**.</span></span>

3. <span data-ttu-id="a7ef4-206">Vyberte hello **možnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-206">Select hello **Options** tab.</span></span>

4. <span data-ttu-id="a7ef4-207">V levém podokně hello vyberte **Upřesnit** > **Obecné**a potom vyberte hello **parametry konfigurace** tlačítko na hello pravé dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-207">In hello left pane, select **Advanced** > **General**, and then select hello **Configuration Parameters** button on hello lower-right part of hello screen.</span></span>

    ![Karta Možnosti](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    <span data-ttu-id="a7ef4-209">Hello **parametry konfigurace** možnost není k dispozici, když hello počítač běží.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-209">hello **Configuration Parameters** option is not available when hello machine is running.</span></span> <span data-ttu-id="a7ef4-210">toomake na této kartě aktivní, vypněte virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-210">toomake this tab active, shut down hello virtual machine.</span></span>

5. <span data-ttu-id="a7ef4-211">Zda řádek s **disku. EnableUUID** již existuje.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-211">See whether a row with **disk.EnableUUID** already exists.</span></span>

    - <span data-ttu-id="a7ef4-212">Pokud hodnota hello existuje a je nastaven příliš**False**, změňte hodnotu hello příliš**True**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-212">If hello value exists and is set too**False**, change hello value too**True**.</span></span> <span data-ttu-id="a7ef4-213">(hodnoty hello nejsou malá a velká písmena.)</span><span class="sxs-lookup"><span data-stu-id="a7ef4-213">(hello values are not case-sensitive.)</span></span>

    - <span data-ttu-id="a7ef4-214">Pokud hodnota hello existuje a je nastaven příliš**True**, vyberte **zrušit**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-214">If hello value exists and is set too**True**, select **Cancel**.</span></span>

    - <span data-ttu-id="a7ef4-215">Pokud hodnota hello neexistuje, vyberte **přidat řádek**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-215">If hello value does not exist, select **Add Row**.</span></span>

    - <span data-ttu-id="a7ef4-216">Ve sloupci Název hello přidat **disku. EnableUUID**a pak nastavte hodnotu hello příliš**TRUE**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-216">In hello name column, add **disk.EnableUUID**, and then set hello value too**TRUE**.</span></span>

    ![Kontroluje, zda disk. EnableUUID již existuje.](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a><span data-ttu-id="a7ef4-218">Zakázat upgrady jádra</span><span class="sxs-lookup"><span data-stu-id="a7ef4-218">Disable kernel upgrades</span></span>

<span data-ttu-id="a7ef4-219">Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi hello Ubuntu, zkontrolujte, zda jsou pro virtuální počítač hello vypnutá upgrady hello jádra.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-219">Azure Site Recovery master target server requires a very specific version of hello Ubuntu, ensure that hello kernel upgrades are disabled for hello virtual machine.</span></span>

<span data-ttu-id="a7ef4-220">Pokud upgrady jádra jsou povolené, jakékoli regulární upgrady způsobit hello hlavní cílový server toomalfunction.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-220">If kernel upgrades are enabled, then any regular upgrades cause hello master target server toomalfunction.</span></span>

#### <a name="download-and-install-additional-packages"></a><span data-ttu-id="a7ef4-221">Stáhněte a nainstalujte další balíčky</span><span class="sxs-lookup"><span data-stu-id="a7ef4-221">Download and install additional packages</span></span>

> [!NOTE]
> <span data-ttu-id="a7ef4-222">Ujistěte se, že máte toodownload připojení k Internetu a nainstalovat další balíčky.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-222">Make sure that you have Internet connectivity toodownload and install additional packages.</span></span> <span data-ttu-id="a7ef4-223">Pokud nemáte připojení k Internetu, je třeba toomanually tyto balíčky RPM najít a nainstalovat je.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-223">If you don't have Internet connectivity, you need toomanually find these RPM packages and install them.</span></span>

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-hello-installer-for-setup"></a><span data-ttu-id="a7ef4-224">Získat hello instalační program pro instalaci</span><span class="sxs-lookup"><span data-stu-id="a7ef4-224">Get hello installer for setup</span></span>

<span data-ttu-id="a7ef4-225">Pokud hlavní cíl se připojení k Internetu, můžete použít následující kroky toodownload hello instalačního programu hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-225">If your master target has Internet connectivity, you can use hello following steps toodownload hello installer.</span></span> <span data-ttu-id="a7ef4-226">Můžete, jinak zkopírujte instalační službu hello z hello procesový server a potom ji nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-226">Otherwise, you can copy hello installer from hello process server and then install it.</span></span>

#### <a name="download-hello-master-target-installation-packages"></a><span data-ttu-id="a7ef4-227">Stáhněte si instalační balíčky hello hlavní cíl</span><span class="sxs-lookup"><span data-stu-id="a7ef4-227">Download hello master target installation packages</span></span>

<span data-ttu-id="a7ef4-228">[Stáhnout hello nejnovější Linux hlavní cíl instalace bits](https://aka.ms/latestlinuxmobsvc).</span><span class="sxs-lookup"><span data-stu-id="a7ef4-228">[Download hello latest Linux master target installation bits](https://aka.ms/latestlinuxmobsvc).</span></span>

<span data-ttu-id="a7ef4-229">toodownload ho pomocí Linux, zadejte:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-229">toodownload it by using Linux, type:</span></span>

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

<span data-ttu-id="a7ef4-230">Ujistěte se, stáhněte a rozbalte instalační program hello v domovském adresáři.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-230">Make sure that you download and unzip hello installer in your home directory.</span></span> <span data-ttu-id="a7ef4-231">Pokud jste příliš rozbalte**/usr/místní**, hello instalace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-231">If you unzip too**/usr/Local**, then hello installation  fails.</span></span>


#### <a name="access-hello-installer-from-hello-process-server"></a><span data-ttu-id="a7ef4-232">Instalační program hello přístup ze serveru proces hello</span><span class="sxs-lookup"><span data-stu-id="a7ef4-232">Access hello installer from hello process server</span></span>

1. <span data-ttu-id="a7ef4-233">Na serveru proces hello přejděte příliš**C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-233">On hello process server, go too**C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span></span>

2. <span data-ttu-id="a7ef4-234">Hello instalační program vyžaduje soubor zkopírovat z hello procesový server a uložte ho jako **latestlinuxmobsvc.tar.gz** v domovském adresáři.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-234">Copy hello required installer file from hello process server, and save it as **latestlinuxmobsvc.tar.gz** in your home directory.</span></span>


### <a name="apply-custom-configuration-changes"></a><span data-ttu-id="a7ef4-235">Změny vlastní konfigurace</span><span class="sxs-lookup"><span data-stu-id="a7ef4-235">Apply custom configuration changes</span></span>

<span data-ttu-id="a7ef4-236">změny tooapply vlastní konfigurace, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-236">tooapply custom configuration changes, use hello following steps:</span></span>


1. <span data-ttu-id="a7ef4-237">Spusťte následující příkaz toountar hello binární hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-237">Run hello following command toountar hello binary.</span></span>
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Snímek obrazovky toorun příkaz hello](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. <span data-ttu-id="a7ef4-239">Spusťte následující příkaz toogive oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-239">Run hello following command toogive permission.</span></span>
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. <span data-ttu-id="a7ef4-240">Spusťte následující příkaz toorun hello skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-240">Run hello following command toorun hello script.</span></span>
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> <span data-ttu-id="a7ef4-241">Spusťte skript hello jenom jednou na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-241">Run hello script only once on hello server.</span></span> <span data-ttu-id="a7ef4-242">Vypněte hello server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-242">Shut down hello server.</span></span> <span data-ttu-id="a7ef4-243">Potom restartujte hello server po přidat disk, jak je popsáno v další části hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-243">Then restart hello server after you add a disk, as described in hello next section.</span></span>

### <a name="add-a-retention-disk-toohello-linux-master-target-virtual-machine"></a><span data-ttu-id="a7ef4-244">Přidat uchování disku toohello hlavní cílový virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="a7ef4-244">Add a retention disk toohello Linux master target virtual machine</span></span>

<span data-ttu-id="a7ef4-245">Použijte následující postup toocreate disku pro uchování hello:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-245">Use hello following steps toocreate a retention disk:</span></span>

1. <span data-ttu-id="a7ef4-246">Připojte nový hlavní cílový virtuální počítač Linux toohello disk 1 TB a potom hello počítač spustit.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-246">Attach a new 1-TB disk toohello Linux master target virtual machine, and then start hello machine.</span></span>

2. <span data-ttu-id="a7ef4-247">Použití hello **vícenásobný -udou** příkaz toolearn hello vícenásobný ID disku pro uchování hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-247">Use hello **multipath -ll** command toolearn hello multipath ID of hello retention disk.</span></span>

    ```
    multipath -ll
    ```
    ![Hello vícenásobný ID disku pro uchování hello](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. <span data-ttu-id="a7ef4-249">Formátování hello disku a pak vytvořit systém souborů na nový disk hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-249">Format hello drive, and then create a file system on hello new drive.</span></span>

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Vytvoření systému souborů na jednotce hello](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. <span data-ttu-id="a7ef4-251">Po vytvoření hello systému souborů, připojte disk uchování hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-251">After you create hello file system, mount hello retention disk.</span></span>
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Disku pro uchování hello připojení](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. <span data-ttu-id="a7ef4-253">Vytvoření hello **fstab** položka toomount hello jednotka pro uchování pokaždé, když hello systém při spuštění.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-253">Create hello **fstab** entry toomount hello retention drive every time hello system starts.</span></span>
    ```
    vi /etc/fstab
    ```
    <span data-ttu-id="a7ef4-254">Vyberte **vložit** toobegin úprav souboru hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-254">Select **Insert** toobegin editing hello file.</span></span> <span data-ttu-id="a7ef4-255">Vytvořte nový řádek a potom vložte následující text hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-255">Create a new line, and then insert hello following text.</span></span> <span data-ttu-id="a7ef4-256">Upravte ID vícenásobný hello disku podle hello zvýrazněná vícenásobný ID z předchozí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-256">Edit hello disk multipath ID based on hello highlighted multipath ID from hello previous command.</span></span>

    <span data-ttu-id="a7ef4-257"> **/dev/mapper/ <Retention disks multipath id> /mnt/uchování ext4 rw 0 0**</span><span class="sxs-lookup"><span data-stu-id="a7ef4-257">**/dev/mapper/<Retention disks multipath id> /mnt/retention ext4 rw 0 0**</span></span>

    <span data-ttu-id="a7ef4-258">Vyberte **Esc**a pak zadejte **: QW** (zápisu a ukončení) okno editor tooclose hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-258">Select **Esc**, and then type **:wq** (write and quit) tooclose hello editor window.</span></span>

### <a name="install-hello-master-target"></a><span data-ttu-id="a7ef4-259">Instalovat hlavní cíl hello</span><span class="sxs-lookup"><span data-stu-id="a7ef4-259">Install hello master target</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a7ef4-260">Hello verzi hello hlavní cílový server musí být rovna tooor starší než verze hello hello procesový server a hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-260">hello version of hello master target server must be equal tooor earlier than hello versions of hello process server and hello configuration server.</span></span> <span data-ttu-id="a7ef4-261">Pokud není tato podmínka splněná, opětovné ochrany úspěšná, ale replikace selže.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-261">If this condition is not met, reprotect succeeds, but replication fails.</span></span>


> [!NOTE]
> <span data-ttu-id="a7ef4-262">Než nainstalujete hello hlavní cílový server, zkontrolujte, že hello **/etc/hosts** soubor hello virtuálního počítače obsahuje položky, které mapují hello názvem místního hostitele toohello IP adresy, které jsou přidruženy všechny síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-262">Before you install hello master target server, check that hello **/etc/hosts** file on hello virtual machine contains entries that map hello local hostname toohello IP addresses that are associated with all network adapters.</span></span>

1. <span data-ttu-id="a7ef4-263">Kopírovat heslo hello z **C:\ProgramData\Microsoft Azure lokality Recovery\private\connection.passphrase** na hello konfiguračním serveru.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-263">Copy hello passphrase from **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** on hello configuration server.</span></span> <span data-ttu-id="a7ef4-264">Potom uložte ho jako **passphrase.txt** v hello hello stejného adresáře tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-264">Then save it as **passphrase.txt** in hello same local directory by running hello following command:</span></span>

    ```
    echo <passphrase> >passphrase.txt
    ```
    <span data-ttu-id="a7ef4-265">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-265">Example:</span></span> 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. <span data-ttu-id="a7ef4-266">Všimněte si, IP adresa serveru konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-266">Note hello configuration server's IP address.</span></span> <span data-ttu-id="a7ef4-267">Budete ho potřebovat v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-267">You need it in hello next step.</span></span>

3. <span data-ttu-id="a7ef4-268">Spusťte následující příkaz tooinstall hello hlavní cílový server hello a hello server zaregistrovat hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-268">Run hello following command tooinstall hello master target server and register hello server with hello configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    <span data-ttu-id="a7ef4-269">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-269">Example:</span></span> 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    <span data-ttu-id="a7ef4-270">Počkejte na dokončení skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-270">Wait until hello script finishes.</span></span> <span data-ttu-id="a7ef4-271">Pokud hlavní cíl hello zaregistruje úspěšně, hlavní cíl hello je uvedený na hello **infrastruktura Site Recovery** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-271">If hello master target registers sucessfully, hello master target is listed on hello **Site Recovery Infrastructure** page of hello portal.</span></span>


#### <a name="install-hello-master-target-by-using-interactive-installation"></a><span data-ttu-id="a7ef4-272">Instalovat hlavní cíl hello pomocí interaktivní instalace</span><span class="sxs-lookup"><span data-stu-id="a7ef4-272">Install hello master target by using interactive installation</span></span>

1. <span data-ttu-id="a7ef4-273">Spusťte následující příkaz tooinstall hello hlavní cíl hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-273">Run hello following command tooinstall hello master target.</span></span> <span data-ttu-id="a7ef4-274">Pro roli hello agenta, vyberte **hlavního cíle**.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-274">For hello agent role, choose **Master Target**.</span></span>

    ```
    ./install
    ```

2. <span data-ttu-id="a7ef4-275">Zvolte hello výchozí umístění pro instalaci a potom vyberte **Enter** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-275">Choose hello default location for installation, and then select **Enter** toocontinue.</span></span>

    ![Volba výchozí umístění pro instalaci hlavní cíl](./media/site-recovery-how-to-install-linux-master-target/image17.png)

<span data-ttu-id="a7ef4-277">Po dokončení instalace hello zaregistrujte konfigurační server hello pomocí příkazového řádku hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-277">After hello installation has finished, register hello configuration server by using hello command line.</span></span>

1. <span data-ttu-id="a7ef4-278">Všimněte si hello IP adresu hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-278">Note hello IP address of hello configuration server.</span></span> <span data-ttu-id="a7ef4-279">Budete ho potřebovat v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-279">You need it in hello next step.</span></span>

2. <span data-ttu-id="a7ef4-280">Spusťte následující příkaz tooinstall hello hlavní cílový server hello a hello server zaregistrovat hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-280">Run hello following command tooinstall hello master target server and register hello server with hello configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    <span data-ttu-id="a7ef4-281">Příklad:</span><span class="sxs-lookup"><span data-stu-id="a7ef4-281">Example:</span></span> 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   <span data-ttu-id="a7ef4-282">Počkejte na dokončení skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-282">Wait until hello script finishes.</span></span> <span data-ttu-id="a7ef4-283">Pokud hlavní cíl hello je úspěšně registrovaná, hlavní cíl hello je uvedený na hello **infrastruktura Site Recovery** hello portálu.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-283">If hello master target is registered succesfully, hello master target is listed on hello **Site Recovery Infrastructure** page of hello portal.</span></span>


### <a name="upgrade-hello-master-target"></a><span data-ttu-id="a7ef4-284">Hlavní cíl upgradu hello</span><span class="sxs-lookup"><span data-stu-id="a7ef4-284">Upgrade hello master target</span></span>

<span data-ttu-id="a7ef4-285">Spusťte instalační program hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-285">Run hello installer.</span></span> <span data-ttu-id="a7ef4-286">Automaticky rozpozná, že je tento hello agent nainstalovaný na hlavním cíli hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-286">It automatically detects that hello agent is installed on hello master target.</span></span> <span data-ttu-id="a7ef4-287">tooupgrade, vyberte **Y**.  Po dokončení instalace hello, zkontrolujte verzi hello hello hlavního cíle nainstalován pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-287">tooupgrade, select **Y**.  After hello setup has been completed, check hello version of hello master target installed by using hello following command.</span></span>

    ```
    cat /usr/local/.vx_version
    ```

<span data-ttu-id="a7ef4-288">Uvidíte, že hello **verze** pole obsahuje číslo verze hello hello hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-288">You can see that hello **Version** field gives hello version number of hello master target.</span></span>

### <a name="install-vmware-tools-on-hello-master-target-server"></a><span data-ttu-id="a7ef4-289">Nainstalujte nástroje VMware na hlavní cílový server hello</span><span class="sxs-lookup"><span data-stu-id="a7ef4-289">Install VMware tools on hello master target server</span></span>

<span data-ttu-id="a7ef4-290">Nástroje VMware tooinstall na hlavním cíli hello je nutné, aby ho můžete zjistit úložiště dat hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-290">You need tooinstall VMware tools on hello master target so that it can discover hello data stores.</span></span> <span data-ttu-id="a7ef4-291">Pokud nejsou nainstalovány nástroje hello, úvodní obrazovka opětovné ochrany se neuvádějí v úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-291">If hello tools are not installed, hello reprotect screen isn't listed in hello data stores.</span></span> <span data-ttu-id="a7ef4-292">Po instalaci nástroje VMware hello je třeba toorestart.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-292">After installation of hello VMware tools, you need toorestart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7ef4-293">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7ef4-293">Next steps</span></span>
<span data-ttu-id="a7ef4-294">Po hello instalace a registrace hlavního cíle hello má finsihed, uvidíte hello hlavní cíl se zobrazují v hello **hlavního cíle** kapitoly **infrastruktura Site Recovery**, v části hello Přehled konfigurace serveru.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-294">After hello installation and registration of hello master target has finsihed, you can see hello master target appear on hello **Master Target** section in **Site Recovery Infrastructure**, under hello configuration server overview.</span></span>

<span data-ttu-id="a7ef4-295">Teď můžete pokračovat s [vytvoření](site-recovery-how-to-reprotect.md), za nímž následují navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-295">You can now proceed with [reprotection](site-recovery-how-to-reprotect.md), followed by failback.</span></span>

## <a name="common-issues"></a><span data-ttu-id="a7ef4-296">Běžné problémy</span><span class="sxs-lookup"><span data-stu-id="a7ef4-296">Common issues</span></span>

* <span data-ttu-id="a7ef4-297">Ujistěte se, že jste nezapínejte řešení Storage vMotion na žádné součásti správy, jako je hlavní cíl.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-297">Make sure you do not turn on Storage vMotion on any management components such as a master target.</span></span> <span data-ttu-id="a7ef4-298">Pokud se hlavní cíl hello přesune po úspěšné opětovné ochrany, nelze odpojit hello disky virtuálního počítače (VMDKs).</span><span class="sxs-lookup"><span data-stu-id="a7ef4-298">If hello master target moves after a successful reprotect, hello virtual machine disks (VMDKs) cannot be detached.</span></span> <span data-ttu-id="a7ef4-299">V takovém případě navrácení služeb po obnovení selže.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-299">In this case, failback fails.</span></span>

* <span data-ttu-id="a7ef4-300">hlavní cíl Hello by neměl mít všechny snímky hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-300">hello master target should not have any snapshots on hello virtual machine.</span></span> <span data-ttu-id="a7ef4-301">Pokud existují snímky, navrácení služeb po obnovení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-301">If there are snapshots, failback fails.</span></span>

* <span data-ttu-id="a7ef4-302">Z důvodu toosome vlastní konfigurace síťový adaptér v někteří zákazníci hello síťové rozhraní je zakázané během spouštění a hello hlavní cíl agenta nelze inicializovat.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-302">Due toosome custom NIC configurations at some customers, hello network interface is disabled during startup, and hello master target agent cannot initialize.</span></span> <span data-ttu-id="a7ef4-303">Ujistěte se, že hello následující vlastnosti jsou správně nastaveny.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-303">Make sure that hello following properties are correctly set.</span></span> <span data-ttu-id="a7ef4-304">Zkontrolujte tyto vlastnosti v hello Ethernet karty souboru /etc/sysconfig/network-scripts/ifcfg-eth *.</span><span class="sxs-lookup"><span data-stu-id="a7ef4-304">Check these properties in hello Ethernet card file's /etc/sysconfig/network-scripts/ifcfg-eth*.</span></span>
    * <span data-ttu-id="a7ef4-305">BOOTPROTO = dhcp</span><span class="sxs-lookup"><span data-stu-id="a7ef4-305">BOOTPROTO=dhcp</span></span>
    * <span data-ttu-id="a7ef4-306">ONBOOT = Ano</span><span class="sxs-lookup"><span data-stu-id="a7ef4-306">ONBOOT=yes</span></span>
