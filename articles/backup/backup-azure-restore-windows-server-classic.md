---
title: "hello aaaRestore data tooa systému Windows Server nebo klienta Windows Azure pomocí modelu nasazení classic | Microsoft Docs"
description: "Zjistěte, jak toorestore z Windows serveru nebo klienta Windows."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 4d1458d5233c4f55004ecfa95cbf7b3b18a03dde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-hello-classic-deployment-model"></a><span data-ttu-id="185cb-103">Obnovit soubory tooa Windows server nebo klientský počítač systému Windows pomocí modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="185cb-103">Restore files tooa Windows server or Windows client machine using hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="185cb-104">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="185cb-104">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
> * [<span data-ttu-id="185cb-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="185cb-105">Azure portal</span></span>](backup-azure-restore-windows-server.md)
>
>

<span data-ttu-id="185cb-106">Tento článek vysvětluje, jak toorecover data ze zálohy trezoru a obnovte ji tooa serveru nebo počítači.</span><span class="sxs-lookup"><span data-stu-id="185cb-106">This article explains how toorecover data from a backup vault and restore it tooa server or computer.</span></span> <span data-ttu-id="185cb-107">Počínaje března 2017, můžete již nemohou vytvářet záloh na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-107">Starting in March 2017, you can no longer create backup vaults in hello classic portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="185cb-108">Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup.</span><span class="sxs-lookup"><span data-stu-id="185cb-108">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="185cb-109">Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="185cb-109">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="185cb-110">Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="185cb-110">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="185cb-111">**15. října 2017**, už nebude trezory Backup toocreate možné toouse prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="185cb-111">**October 15, 2017**, you will no longer be able toouse PowerShell toocreate Backup vaults.</span></span> <br/> <span data-ttu-id="185cb-112">**Od 1. listopadu 2017**:</span><span class="sxs-lookup"><span data-stu-id="185cb-112">**Starting November 1, 2017**:</span></span>
>- <span data-ttu-id="185cb-113">Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="185cb-113">Any remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="185cb-114">Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-114">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="185cb-115">Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="185cb-115">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

<span data-ttu-id="185cb-116">toorestore data, která používáte Průvodce obnovení dat hello v agentovi Microsoft Azure Recovery Services (MARS) hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-116">toorestore data, you use hello Recover Data wizard in hello Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="185cb-117">Při obnovování dat, je možné:</span><span class="sxs-lookup"><span data-stu-id="185cb-117">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="185cb-118">Obnovení dat toohello stejný počítač, ze které hello zálohy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="185cb-118">Restore data toohello same machine from which hello backups were taken.</span></span>
* <span data-ttu-id="185cb-119">Obnovení dat tooan alternativní počítače.</span><span class="sxs-lookup"><span data-stu-id="185cb-119">Restore data tooan alternate machine.</span></span>

<span data-ttu-id="185cb-120">V lednu 2017 společnost Microsoft vydala agenta MARS toohello aktualizace verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="185cb-120">In January 2017, Microsoft released a Preview update toohello MARS agent.</span></span> <span data-ttu-id="185cb-121">Společně s oprav chyb, tato aktualizace umožňuje rychlé obnovení, což vám umožní toomount obnovení zapisovatelného snímku bodu jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-121">Along with bug fixes, this update enables Instant Restore, which allows you toomount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="185cb-122">Pak můžete zkoumat hello obnovení svazku a zkopírujte soubory tooa místního počítače a následné obnovení souborů.</span><span class="sxs-lookup"><span data-stu-id="185cb-122">You can then explore hello recovery volume and copy files tooa local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="185cb-123">Hello [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) je povinný, pokud chcete, aby toouse rychlé obnovení dat toorestore.</span><span class="sxs-lookup"><span data-stu-id="185cb-123">hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want toouse Instant Restore toorestore data.</span></span> <span data-ttu-id="185cb-124">Hello zálohovaná data musí být chráněna v trezorů v uvedené v článku podpory hello národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="185cb-124">Also hello backup data must be protected in vaults in locales listed in hello support article.</span></span> <span data-ttu-id="185cb-125">Poraďte se hello [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) hello nejnovější seznam národních prostředí, které podporují rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-125">Consult hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for hello latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="185cb-126">Rychlé obnovení je **není** aktuálně k dispozici ve všech národních prostředí.</span><span class="sxs-lookup"><span data-stu-id="185cb-126">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="185cb-127">Rychlé obnovení je k dispozici pro použití v trezory služeb zotavení v hello portál Azure a trezory Backup portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-127">Instant Restore is available for use in Recovery Services vaults in hello Azure portal and Backup vaults in hello classic portal.</span></span> <span data-ttu-id="185cb-128">Pokud chcete toouse rychlé obnovení, stažení aktualizace MARS hello a postupujte podle hello postupy, které zmínili, rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-128">If you want toouse Instant Restore, download hello MARS update, and follow hello procedures that mention Instant Restore.</span></span>


## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a><span data-ttu-id="185cb-129">Rychlé obnovení dat toohello toorecover použít stejný počítač</span><span class="sxs-lookup"><span data-stu-id="185cb-129">Use Instant Restore toorecover data toohello same machine</span></span>

<span data-ttu-id="185cb-130">Pokud jste omylem odstranili toorestore souboru a chcete ho toohello stejného počítače (z které hello je odebrán zálohování), hello následující kroky vám pomůže obnovit hello data.</span><span class="sxs-lookup"><span data-stu-id="185cb-130">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="185cb-131">Otevřete hello **Microsoft Azure Backup** přichycení v.</span><span class="sxs-lookup"><span data-stu-id="185cb-131">Open hello **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="185cb-132">Pokud si nejste jisti, kam se nainstaloval hello modul snap-in, vyhledávat hello počítači nebo serveru pro **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="185cb-132">If you don't know where hello snap in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="185cb-133">aplikace na ploše Hello by se zobrazit ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-133">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="185cb-134">Klikněte na tlačítko **obnovit Data** toostart hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="185cb-134">Click **Recover Data** toostart hello wizard.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="185cb-136">Na hello **Začínáme** podokně, toorestore hello data toohello stejný server nebo počítač, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="185cb-136">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Vyberte tento server možnost toorestore hello data toohello stejný počítač](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="185cb-138">Na hello **vyberte režimu obnovení** podokně vyberte **jednotlivých souborů a složek** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="185cb-138">On hello **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Procházet soubory](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="185cb-140">Na hello **vyberte svazek a datum** podokně, vyberte hello svazku, který obsahuje hello soubory nebo složky, které chcete toorestore.</span><span class="sxs-lookup"><span data-stu-id="185cb-140">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="185cb-141">Na hello kalendáři vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-141">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="185cb-142">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="185cb-142">You can restore from any recovery point in time.</span></span> <span data-ttu-id="185cb-143">Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-143">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="185cb-144">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="185cb-144">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Svazek a datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="185cb-146">Jakmile jste vybrali toorestore bodu obnovení hello, klikněte na možnost **připojit**.</span><span class="sxs-lookup"><span data-stu-id="185cb-146">Once you have chosen hello recovery point toorestore, click **Mount**.</span></span>

    <span data-ttu-id="185cb-147">Zálohování Azure připojí bodu místní obnovení hello a používá je jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-147">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="185cb-148">Na hello **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** tooopen Průzkumníka Windows a najít hello soubory a složky chcete.</span><span class="sxs-lookup"><span data-stu-id="185cb-148">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="185cb-150">V Průzkumníku Windows, kopie hello soubory nebo složky toorestore a vložit je tooany umístění místní toohello serveru nebo počítači.</span><span class="sxs-lookup"><span data-stu-id="185cb-150">In Windows Explorer, copy hello files and/or folders you want toorestore and paste them tooany location local toohello server or computer.</span></span> <span data-ttu-id="185cb-151">Můžete otevřít nebo stream hello soubory přímo z hello obnovení svazku a ověření hello správné verze se obnoví.</span><span class="sxs-lookup"><span data-stu-id="185cb-151">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Kopírovat a vkládat soubory a složky z připojeného svazku toolocal umístění](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="185cb-153">Po dokončení obnovení hello soubory nebo složky na hello **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="185cb-153">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="185cb-154">Pak klikněte na tlačítko **Ano** tooconfirm, které chcete toounmount hello svazku.</span><span class="sxs-lookup"><span data-stu-id="185cb-154">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Odpojení hello svazku a potvrďte](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="185cb-156">Pokud neklikejte na odpojení, zůstane hello obnovení svazku připojené šest hodin od času hello, pokud byla připojena.</span><span class="sxs-lookup"><span data-stu-id="185cb-156">If you do not click Unmount, hello Recovery Volume will remain mounted for six hours from hello time when it was mounted.</span></span> <span data-ttu-id="185cb-157">Žádná operace zálohování se spustí hello svazek je připojen.</span><span class="sxs-lookup"><span data-stu-id="185cb-157">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="185cb-158">Všechny naplánované operace zálohování toorun během hello doby, kdy je připojen hello svazek, se spustí po obnovení svazku hello odpojené.</span><span class="sxs-lookup"><span data-stu-id="185cb-158">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="recover-data-toohello-same-machine"></a><span data-ttu-id="185cb-159">Obnovení dat toohello stejný počítač</span><span class="sxs-lookup"><span data-stu-id="185cb-159">Recover data toohello same machine</span></span>
<span data-ttu-id="185cb-160">Pokud jste omylem odstranili toorestore souboru a chcete ho toohello stejného počítače (z které hello je odebrán zálohování), hello následující kroky vám pomůže obnovit hello data.</span><span class="sxs-lookup"><span data-stu-id="185cb-160">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="185cb-161">Otevřete hello **Microsoft Azure Backup** přichycení v.</span><span class="sxs-lookup"><span data-stu-id="185cb-161">Open hello **Microsoft Azure Backup** snap in.</span></span>
2. <span data-ttu-id="185cb-162">Klikněte na tlačítko **obnovit Data** tooinitiate hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="185cb-162">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)
3. <span data-ttu-id="185cb-164">Vyberte hello  **tento server (*yourmachinename*) ** hello toorestore možnost zálohovat soubor na hello stejný počítač.</span><span class="sxs-lookup"><span data-stu-id="185cb-164">Select hello **This server (*yourmachinename*)** option toorestore hello backed up file on hello same machine.</span></span>

    ![Stejný počítač](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. <span data-ttu-id="185cb-166">Zvolte příliš**Procházet soubory** nebo **vyhledávání souborů**.</span><span class="sxs-lookup"><span data-stu-id="185cb-166">Choose too**Browse for files** or **Search for files**.</span></span>

    <span data-ttu-id="185cb-167">Ponechte výchozí možnost hello Pokud máte v plánu toorestore jeden nebo více souborů, jejichž cesta je známá.</span><span class="sxs-lookup"><span data-stu-id="185cb-167">Leave hello default option if you plan toorestore one or more files whose path is known.</span></span> <span data-ttu-id="185cb-168">Pokud nejste jisti o hello struktura složek, ale chcete toosearch pro soubor, vyberte hello **vyhledávání souborů** možnost.</span><span class="sxs-lookup"><span data-stu-id="185cb-168">If you are not sure about hello folder structure but would like toosearch for a file, pick hello **Search for files** option.</span></span> <span data-ttu-id="185cb-169">Hello za účelem v této části budeme pokračovat s hello výchozí možnost.</span><span class="sxs-lookup"><span data-stu-id="185cb-169">For hello purpose of this section, we will proceed with hello default option.</span></span>

    ![Procházet soubory](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. <span data-ttu-id="185cb-171">Vyberte svazek hello, ze kterého chcete soubor toorestore hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-171">Select hello volume from which you wish toorestore hello file.</span></span>

    <span data-ttu-id="185cb-172">Můžete obnovit z libovolného bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="185cb-172">You can restore from any point in time.</span></span> <span data-ttu-id="185cb-173">Data, které jsou v **tučné** v ovládacím prvku Kalendář hello znamenat hello dostupnost bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-173">Dates which appear in **bold** in hello calendar control indicate hello availability of a restore point.</span></span> <span data-ttu-id="185cb-174">Jakmile je vybrat datum, na základě vašeho plánu zálohování (a hello úspěch operace zálohování), můžete vybrat bod v čase, ze hello **čas** rozevírací nabídku.</span><span class="sxs-lookup"><span data-stu-id="185cb-174">Once a date is selected, based on your backup schedule (and hello success of a backup operation), you can select a point in time from hello **Time** drop down.</span></span>

    ![Svazek a datum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. <span data-ttu-id="185cb-176">Vyberte položky toorecover hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-176">Select hello items toorecover.</span></span> <span data-ttu-id="185cb-177">Můžete vybrat víc chcete toorestore složek nebo souborů.</span><span class="sxs-lookup"><span data-stu-id="185cb-177">You can multi-select folders/files you wish toorestore.</span></span>

    ![Výběr souborů](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. <span data-ttu-id="185cb-179">Zadejte parametry obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-179">Specify hello recovery parameters.</span></span>

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * <span data-ttu-id="185cb-181">Máte možnost obnovení původního umístění toohello (v které hello soubor nebo složku, budou přepsána) nebo tooanother umístění v hello stejný počítač.</span><span class="sxs-lookup"><span data-stu-id="185cb-181">You have an option of restoring toohello original location (in which hello file/folder would be overwritten) or tooanother location in hello same machine.</span></span>
   * <span data-ttu-id="185cb-182">Pokud hello soubor nebo složku, toorestore existuje v cílovém umístění hello, můžete vytvořit kopie (hello dvě verze stejného souboru), přepsat hello soubory v cílovém umístění hello nebo přeskočit obnovení hello hello souborů, které existují v cílovém hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-182">If hello file/folder you wish toorestore exists in hello target location, you can create copies (two versions of hello same file), overwrite hello files in hello target location, or skip hello recovery of hello files which exist in hello target.</span></span>
   * <span data-ttu-id="185cb-183">Důrazně doporučujeme ponechat hello výchozí možnost obnovení hello seznamy ACL v hello soubory, které se obnovuje.</span><span class="sxs-lookup"><span data-stu-id="185cb-183">It is highly recommended that you leave hello default option of restoring hello ACLs on hello files which are being recovered.</span></span>
8. <span data-ttu-id="185cb-184">Jakmile jsou k dispozici tyto vstupy, klikněte na možnost **Další**.</span><span class="sxs-lookup"><span data-stu-id="185cb-184">Once these inputs are provided, click **Next**.</span></span> <span data-ttu-id="185cb-185">Postup obnovení Hello, která obnoví hello soubory toothis počítač, začne.</span><span class="sxs-lookup"><span data-stu-id="185cb-185">hello recovery workflow, which restores hello files toothis machine, will begin.</span></span>

## <a name="recover-tooan-alternate-machine"></a><span data-ttu-id="185cb-186">Obnovit tooan alternativní počítač</span><span class="sxs-lookup"><span data-stu-id="185cb-186">Recover tooan alternate machine</span></span>
<span data-ttu-id="185cb-187">Pokud dojde ke ztrátě celý server, stále můžete obnovit data z Azure Backup tooa jiný počítač.</span><span class="sxs-lookup"><span data-stu-id="185cb-187">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="185cb-188">Následující kroky Hello znázorňují hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="185cb-188">hello following steps illustrate hello workflow.</span></span>  

<span data-ttu-id="185cb-189">zahrnuje Hello terminologie použitá v těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="185cb-189">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="185cb-190">*Zdrojový počítač* – pořízení hello původní počítač, ze které hello zálohy a který není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="185cb-190">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="185cb-191">*Cílový počítač* – hello počítač toowhich hello data obnovena.</span><span class="sxs-lookup"><span data-stu-id="185cb-191">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="185cb-192">*Ukázka trezoru* – hello zálohy trezoru toowhich hello *zdrojový počítač* a *cílový počítač* jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="185cb-192">*Sample vault* – hello Backup vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="185cb-193">Zálohy vytvořené z počítače nelze obnovit v počítači, který běží starší verze operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-193">Backups taken from a machine cannot be restored on a machine which is running an earlier version of hello operating system.</span></span> <span data-ttu-id="185cb-194">Například pokud zálohy jsou převzaty z počítače s Windows 7, může být obnovena do systému Windows 8 nebo novější verze počítače.</span><span class="sxs-lookup"><span data-stu-id="185cb-194">For example, if backups are taken from a Windows 7 machine, it can be restored on a Windows 8 or above machine.</span></span> <span data-ttu-id="185cb-195">Naopak hello jsou však nemá hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="185cb-195">However, hello vice-versa does not hold true.</span></span>
>
>

1. <span data-ttu-id="185cb-196">Otevřete hello **Microsoft Azure Backup** přichycení v na hello *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="185cb-196">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>
2. <span data-ttu-id="185cb-197">Ujistěte se, že hello *cílový počítač* a hello *zdrojový počítač* jsou registrované toohello stejné úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="185cb-197">Ensure that hello *Target machine* and hello *Source machine* are registered toohello same backup vault.</span></span>
3. <span data-ttu-id="185cb-198">Klikněte na tlačítko **obnovit Data** tooinitiate hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="185cb-198">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)
4. <span data-ttu-id="185cb-200">Vyberte **jiný server**</span><span class="sxs-lookup"><span data-stu-id="185cb-200">Select **Another server**</span></span>

    ![Jiný Server](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. <span data-ttu-id="185cb-202">Zadejte soubor s přihlašovacími údaji trezoru hello odpovídající toohello *ukázka trezoru*.</span><span class="sxs-lookup"><span data-stu-id="185cb-202">Provide hello vault credential file that corresponds toohello *Sample vault*.</span></span> <span data-ttu-id="185cb-203">Pokud soubor s přihlašovacími údaji trezoru hello je neplatný (nebo vypršela platnost) stáhnout nový soubor s přihlašovacími údaji trezoru z hello *ukázka trezoru* v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="185cb-203">If hello vault credential file is invalid (or expired) download a new vault credential file from hello *Sample vault* in hello Azure classic portal.</span></span> <span data-ttu-id="185cb-204">Jakmile je zadaný soubor s přihlašovacími údaji trezoru hello, se zobrazí úložiště záloh hello proti soubor s přihlašovacími údaji trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-204">Once hello vault credential file is provided, hello backup vault against hello vault credential file is displayed.</span></span>
6. <span data-ttu-id="185cb-205">Vyberte hello *zdrojový počítač* hello seznamu zobrazených počítačů.</span><span class="sxs-lookup"><span data-stu-id="185cb-205">Select hello *Source machine* from hello list of displayed machines.</span></span>

    ![Seznam počítačů](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. <span data-ttu-id="185cb-207">Vyberte buď hello **vyhledávání souborů** nebo **Procházet soubory** možnost.</span><span class="sxs-lookup"><span data-stu-id="185cb-207">Select either hello **Search for files** or **Browse for files** option.</span></span> <span data-ttu-id="185cb-208">Hello za účelem v této části, použijeme hello **vyhledávání souborů** možnost.</span><span class="sxs-lookup"><span data-stu-id="185cb-208">For hello purpose of this section, we will use hello **Search for files** option.</span></span>

    ![Search](./media/backup-azure-restore-windows-server-classic/search.png)
8. <span data-ttu-id="185cb-210">Vyberte svazek hello a datum na další obrazovce hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-210">Select hello volume and date in hello next screen.</span></span> <span data-ttu-id="185cb-211">Vyhledávání pro název složky nebo souboru hello chcete toorestore.</span><span class="sxs-lookup"><span data-stu-id="185cb-211">Search for hello folder/file name you want toorestore.</span></span>

    ![Hledání položek](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. <span data-ttu-id="185cb-213">Vyberte hello umístění, kde hello soubory musí toobe obnovit.</span><span class="sxs-lookup"><span data-stu-id="185cb-213">Select hello location where hello files need toobe restored.</span></span>

    ![Obnovení umístění](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. <span data-ttu-id="185cb-215">Zadejte šifrovací přístupové heslo hello, který jste zadali během *zdrojový počítač* registrace příliš*ukázka trezoru*.</span><span class="sxs-lookup"><span data-stu-id="185cb-215">Provide hello encryption passphrase that was provided during *Source machine’s* registration too*Sample vault*.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. <span data-ttu-id="185cb-217">Jakmile je k dispozici vstup hello, klikněte na možnost **obnovit**, které aktivační události hello obnovení hello zálohovat soubory toohello cíle zadaného.</span><span class="sxs-lookup"><span data-stu-id="185cb-217">Once hello input is provided, click **Recover**, which triggers hello restore of hello backed up files toohello destination provided.</span></span>

## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a><span data-ttu-id="185cb-218">Použít rychlé obnovení toorestore data tooan alternativní počítač</span><span class="sxs-lookup"><span data-stu-id="185cb-218">Use Instant Restore toorestore data tooan alternate machine</span></span>
<span data-ttu-id="185cb-219">Pokud dojde ke ztrátě celý server, stále můžete obnovit data z Azure Backup tooa jiný počítač.</span><span class="sxs-lookup"><span data-stu-id="185cb-219">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="185cb-220">Následující kroky Hello znázorňují hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="185cb-220">hello following steps illustrate hello workflow.</span></span>

<span data-ttu-id="185cb-221">zahrnuje Hello terminologie použitá v těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="185cb-221">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="185cb-222">*Zdrojový počítač* – pořízení hello původní počítač, ze které hello zálohy a který není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="185cb-222">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="185cb-223">*Cílový počítač* – hello počítač toowhich hello data obnovena.</span><span class="sxs-lookup"><span data-stu-id="185cb-223">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="185cb-224">*Ukázka trezoru* – hello toowhich trezoru služeb zotavení hello *zdrojový počítač* a *cílový počítač* jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="185cb-224">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="185cb-225">Zálohování nemůže být starší verzí operačního systému hello obnovené tooa cílový počítač.</span><span class="sxs-lookup"><span data-stu-id="185cb-225">Backups can't be restored tooa target machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="185cb-226">Například převzat ze systému Windows 7, počítač může být obnovena záloha v systému Windows 8 nebo novější, počítače.</span><span class="sxs-lookup"><span data-stu-id="185cb-226">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="185cb-227">Zálohy z počítače se systémem Windows 8 nemůže být počítač obnovený tooa Windows 7.</span><span class="sxs-lookup"><span data-stu-id="185cb-227">A backup taken from a Windows 8 computer cannot be restored tooa Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="185cb-228">Otevřete hello **Microsoft Azure Backup** přichycení v na hello *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="185cb-228">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>

2. <span data-ttu-id="185cb-229">Ujistěte se, hello *cílový počítač* a hello *zdrojový počítač* jsou registrované toohello trezoru stejné služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="185cb-229">Ensure hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>

3. <span data-ttu-id="185cb-230">Klikněte na tlačítko **obnovit Data** tooopen hello **Průvodce obnovení dat**.</span><span class="sxs-lookup"><span data-stu-id="185cb-230">Click **Recover Data** tooopen hello **Recover Data wizard**.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="185cb-232">Na hello **Začínáme** podokně, vyberte **jiný server**</span><span class="sxs-lookup"><span data-stu-id="185cb-232">On hello **Getting Started** pane, select **Another server**</span></span>

    ![Jiný Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="185cb-234">Zadejte soubor s přihlašovacími údaji trezoru hello odpovídající toohello *ukázka trezoru*a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="185cb-234">Provide hello vault credential file that corresponds toohello *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="185cb-235">Pokud soubor s přihlašovacími údaji trezoru hello je neplatný (nebo vypršela platnost), stáhněte si nový soubor s přihlašovacími údaji trezoru z hello *ukázka trezoru* v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="185cb-235">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="185cb-236">Po zadání přihlašovacích údajů platné úložiště, zobrazí se název hello hello odpovídající úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="185cb-236">Once you provide a valid vault credential, hello name of hello corresponding Backup Vault appears.</span></span>

6. <span data-ttu-id="185cb-237">Na hello **vyberte zálohování serveru** podokně, vyberte hello *zdrojový počítač* hello seznamu zobrazených počítačů a zadejte přístupové heslo hello.</span><span class="sxs-lookup"><span data-stu-id="185cb-237">On hello **Select Backup Server** pane, select hello *Source machine* from hello list of displayed machines and provide hello passphrase.</span></span> <span data-ttu-id="185cb-238">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="185cb-238">Then click **Next**.</span></span>

    ![Seznam počítačů](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="185cb-240">Na hello **vyberte režimu obnovení** podokně, vyberte **jednotlivých souborů a složek** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="185cb-240">On hello **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Search](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="185cb-242">Na hello **vyberte svazek a datum** podokně, vyberte hello svazku, který obsahuje hello soubory nebo složky, které chcete toorestore.</span><span class="sxs-lookup"><span data-stu-id="185cb-242">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="185cb-243">Na hello kalendáři vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-243">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="185cb-244">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="185cb-244">You can restore from any recovery point in time.</span></span> <span data-ttu-id="185cb-245">Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="185cb-245">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="185cb-246">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="185cb-246">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Hledání položek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="185cb-248">Klikněte na tlačítko **připojit** toolocally přípojného hello obnovení bodu jako svazek obnovení na vaše *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="185cb-248">Click **Mount** toolocally mount hello recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="185cb-249">Na hello **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** tooopen Průzkumníka Windows a najít hello soubory a složky chcete.</span><span class="sxs-lookup"><span data-stu-id="185cb-249">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="185cb-251">V Průzkumníku Windows, zkopírujte hello soubory nebo složky ze svazku obnovení hello a vložte je tooyour *cílový počítač* umístění.</span><span class="sxs-lookup"><span data-stu-id="185cb-251">In Windows Explorer, copy hello files and/or folders from hello recovery volume and paste them tooyour *Target machine* location.</span></span> <span data-ttu-id="185cb-252">Můžete otevřít nebo stream hello soubory přímo z hello obnovení svazku a ověření hello správné verze se obnoví.</span><span class="sxs-lookup"><span data-stu-id="185cb-252">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="185cb-254">Po dokončení obnovení hello soubory nebo složky na hello **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="185cb-254">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="185cb-255">Pak klikněte na tlačítko **Ano** tooconfirm, které chcete toounmount hello svazku.</span><span class="sxs-lookup"><span data-stu-id="185cb-255">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="185cb-257">Pokud neklikejte na odpojení, zůstane hello obnovení svazku připojené šest hodin od času hello, pokud byla připojena.</span><span class="sxs-lookup"><span data-stu-id="185cb-257">If you do not click Unmount, hello Recovery Volume will remain mounted for six hours from hello time when it was mounted.</span></span> <span data-ttu-id="185cb-258">Žádná operace zálohování se spustí hello svazek je připojen.</span><span class="sxs-lookup"><span data-stu-id="185cb-258">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="185cb-259">Všechny naplánované operace zálohování toorun během hello doby, kdy je připojen hello svazek, se spustí po obnovení svazku hello odpojené.</span><span class="sxs-lookup"><span data-stu-id="185cb-259">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="next-steps"></a><span data-ttu-id="185cb-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="185cb-260">Next steps</span></span>
* [<span data-ttu-id="185cb-261">Azure Backup – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="185cb-261">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
* <span data-ttu-id="185cb-262">Navštivte hello [fóru služby Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span><span class="sxs-lookup"><span data-stu-id="185cb-262">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span></span>

## <a name="learn-more"></a><span data-ttu-id="185cb-263">Další informace</span><span class="sxs-lookup"><span data-stu-id="185cb-263">Learn more</span></span>
* [<span data-ttu-id="185cb-264">Přehled služby Azure Backup</span><span class="sxs-lookup"><span data-stu-id="185cb-264">Azure Backup Overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [<span data-ttu-id="185cb-265">Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="185cb-265">Backup Azure virtual machines</span></span>](backup-azure-vms-introduction.md)
* [<span data-ttu-id="185cb-266">Zálohování se úlohy Microsoft</span><span class="sxs-lookup"><span data-stu-id="185cb-266">Backup up Microsoft workloads</span></span>](backup-azure-dpm-introduction.md)
