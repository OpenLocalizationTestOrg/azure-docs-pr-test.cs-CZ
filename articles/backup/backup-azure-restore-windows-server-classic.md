---
title: "Obnovení dat do systému Windows Server nebo klienta Windows z Azure pomocí modelu nasazení classic | Microsoft Docs"
description: "Zjistěte, jak obnovit ze systému Windows Server nebo klienta Windows."
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
ms.openlocfilehash: 300b2b17b44e21ed446fd63d572a2461e2fc1343
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-the-classic-deployment-model"></a><span data-ttu-id="0dca2-103">Obnovení souborů na serveru Windows nebo klientském počítači s využitím klasického modelu nasazení</span><span class="sxs-lookup"><span data-stu-id="0dca2-103">Restore files to a Windows server or Windows client machine using the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0dca2-104">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="0dca2-104">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
> * [<span data-ttu-id="0dca2-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0dca2-105">Azure portal</span></span>](backup-azure-restore-windows-server.md)
>
>

<span data-ttu-id="0dca2-106">Tento článek vysvětluje, jak obnovit data z úložiště záloh a obnovení serveru nebo počítači.</span><span class="sxs-lookup"><span data-stu-id="0dca2-106">This article explains how to recover data from a backup vault and restore it to a server or computer.</span></span> <span data-ttu-id="0dca2-107">Počínaje března 2017, můžete již nemohou vytvářet záloh na portálu classic.</span><span class="sxs-lookup"><span data-stu-id="0dca2-107">Starting in March 2017, you can no longer create backup vaults in the classic portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0dca2-108">Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="0dca2-108">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="0dca2-109">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="0dca2-109">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="0dca2-110">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="0dca2-110">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="0dca2-111">Od **15. října 2017** už nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="0dca2-111">**October 15, 2017**, you will no longer be able to use PowerShell to create Backup vaults.</span></span> <br/> <span data-ttu-id="0dca2-112">**Od 1. listopadu 2017**:</span><span class="sxs-lookup"><span data-stu-id="0dca2-112">**Starting November 1, 2017**:</span></span>
>- <span data-ttu-id="0dca2-113">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="0dca2-113">Any remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="0dca2-114">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="0dca2-114">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="0dca2-115">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0dca2-115">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

<span data-ttu-id="0dca2-116">Chcete-li obnovit data, použijte Průvodce obnovení dat v agentovi nástroje Microsoft Azure Recovery Services (MARS).</span><span class="sxs-lookup"><span data-stu-id="0dca2-116">To restore data, you use the Recover Data wizard in the Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="0dca2-117">Při obnovování dat, je možné:</span><span class="sxs-lookup"><span data-stu-id="0dca2-117">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="0dca2-118">Obnovení dat na stejný počítač, ve kterém byly provedeny zálohy.</span><span class="sxs-lookup"><span data-stu-id="0dca2-118">Restore data to the same machine from which the backups were taken.</span></span>
* <span data-ttu-id="0dca2-119">Obnovte data do alternativní počítače.</span><span class="sxs-lookup"><span data-stu-id="0dca2-119">Restore data to an alternate machine.</span></span>

<span data-ttu-id="0dca2-120">V lednu 2017 společnost Microsoft vydala Preview aktualizace agenta MARS.</span><span class="sxs-lookup"><span data-stu-id="0dca2-120">In January 2017, Microsoft released a Preview update to the MARS agent.</span></span> <span data-ttu-id="0dca2-121">Společně s oprav chyb tato aktualizace umožňuje rychlé obnovení, která umožňuje připojit obnovení zapisovatelného snímku bodu jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-121">Along with bug fixes, this update enables Instant Restore, which allows you to mount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="0dca2-122">Pak můžete zkoumat obnovení svazku a zkopírujte soubory do místního počítače a následné obnovení souborů.</span><span class="sxs-lookup"><span data-stu-id="0dca2-122">You can then explore the recovery volume and copy files to a local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="0dca2-123">[Ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) je povinný, pokud chcete obnovit data pomocí funkce Rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-123">The [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want to use Instant Restore to restore data.</span></span> <span data-ttu-id="0dca2-124">Zálohovaná data musí být chráněna v trezorů v národní prostředí, které jsou uvedené v článku podpory.</span><span class="sxs-lookup"><span data-stu-id="0dca2-124">Also the backup data must be protected in vaults in locales listed in the support article.</span></span> <span data-ttu-id="0dca2-125">Obrátit [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) nejnovější seznam národních prostředí, které podporují rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-125">Consult the [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for the latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="0dca2-126">Rychlé obnovení je **není** aktuálně k dispozici ve všech národních prostředí.</span><span class="sxs-lookup"><span data-stu-id="0dca2-126">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="0dca2-127">Rychlé obnovení je k dispozici pro použití v trezory služeb zotavení v portálu Azure a trezory Backup na portálu classic.</span><span class="sxs-lookup"><span data-stu-id="0dca2-127">Instant Restore is available for use in Recovery Services vaults in the Azure portal and Backup vaults in the classic portal.</span></span> <span data-ttu-id="0dca2-128">Pokud chcete použít rychlé obnovení, stažení aktualizace MARS a postupujte podle pokynů, které zmínili, rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-128">If you want to use Instant Restore, download the MARS update, and follow the procedures that mention Instant Restore.</span></span>


## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a><span data-ttu-id="0dca2-129">Obnovit data do stejného počítače pomocí funkce Rychlé obnovení</span><span class="sxs-lookup"><span data-stu-id="0dca2-129">Use Instant Restore to recover data to the same machine</span></span>

<span data-ttu-id="0dca2-130">Pokud jste omylem odstranit soubor a chcete obnovit do stejného počítače (ze kterého dochází k zálohování), následující kroky vám pomohou obnovit data.</span><span class="sxs-lookup"><span data-stu-id="0dca2-130">If you accidentally deleted a file and wish to restore it to the same machine (from which the backup is taken), the following steps will help you recover the data.</span></span>

1. <span data-ttu-id="0dca2-131">Otevřete **Microsoft Azure Backup** přichycení v.</span><span class="sxs-lookup"><span data-stu-id="0dca2-131">Open the **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="0dca2-132">Pokud si nejste jisti, kde byl nainstalován modul snap-in, hledání se počítač nebo server pro **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-132">If you don't know where the snap in was installed, search the computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="0dca2-133">Desktopová aplikace by se ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="0dca2-133">The desktop app should appear in the search results.</span></span>

2. <span data-ttu-id="0dca2-134">Klikněte na tlačítko **obnovit Data** spusťte průvodce.</span><span class="sxs-lookup"><span data-stu-id="0dca2-134">Click **Recover Data** to start the wizard.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="0dca2-136">Na **Začínáme** , chcete-li obnovit data na stejném serveru nebo počítače, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-136">On the **Getting Started** pane, to restore the data to the same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Vyberte tuto možnost serveru k obnovení dat na stejný počítač](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="0dca2-138">Na **vyberte režimu obnovení** podokně vyberte **jednotlivých souborů a složek** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-138">On the **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Procházet soubory](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="0dca2-140">Na **vyberte svazek a datum** podokně, vyberte svazek, který obsahuje soubory nebo složky, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="0dca2-140">On the **Select Volume and Date** pane, select the volume that contains the files and/or folders you want to restore.</span></span>

    <span data-ttu-id="0dca2-141">V kalendáři vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-141">On the calendar, select a recovery point.</span></span> <span data-ttu-id="0dca2-142">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="0dca2-142">You can restore from any recovery point in time.</span></span> <span data-ttu-id="0dca2-143">Data v **tučné** označuje dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-143">Dates in **bold** indicate the availability of at least one recovery point.</span></span> <span data-ttu-id="0dca2-144">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte konkrétní bod obnovení z **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="0dca2-144">Once you select a date, if multiple recovery points are available, choose the specific recovery point from the **Time** drop-down menu.</span></span>

    ![Svazek a datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="0dca2-146">Po zadání bodu obnovení pro obnovení, klikněte na tlačítko **připojit**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-146">Once you have chosen the recovery point to restore, click **Mount**.</span></span>

    <span data-ttu-id="0dca2-147">Zálohování Azure připojí bod místní obnovení a používá je jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-147">Azure Backup mounts the local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="0dca2-148">Na **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** otevřete Průzkumníka Windows a soubory a složky, které chcete najít.</span><span class="sxs-lookup"><span data-stu-id="0dca2-148">On the **Browse and Recover Files** pane, click **Browse** to open Windows Explorer and find the files and folders you want.</span></span>

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="0dca2-150">V Průzkumníku Windows zkopírujte soubory nebo složky, kterou chcete obnovit a vložit do libovolného umístění místní k serveru nebo počítači.</span><span class="sxs-lookup"><span data-stu-id="0dca2-150">In Windows Explorer, copy the files and/or folders you want to restore and paste them to any location local to the server or computer.</span></span> <span data-ttu-id="0dca2-151">Můžete otevřít nebo stream soubory přímo ze svazku obnovení a ověřte, že se obnoví správné verze.</span><span class="sxs-lookup"><span data-stu-id="0dca2-151">You can open or stream the files directly from the recovery volume and verify the correct versions are recovered.</span></span>

    ![Zkopírujte a vložte souborů a složek z připojeného svazku do místního umístění](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="0dca2-153">Po dokončení obnovení souborů a složek, na **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-153">When you are finished restoring the files and/or folders, on the **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="0dca2-154">Pak klikněte na tlačítko **Ano** potvrďte, že chcete odpojit svazek.</span><span class="sxs-lookup"><span data-stu-id="0dca2-154">Then click **Yes** to confirm that you want to unmount the volume.</span></span>

    ![Odpojte Image svazku a potvrďte](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="0dca2-156">Pokud neklikejte na odpojení, zůstane svazek obnovení připojené šest hodin od okamžiku, kdy byla připojena.</span><span class="sxs-lookup"><span data-stu-id="0dca2-156">If you do not click Unmount, the Recovery Volume will remain mounted for six hours from the time when it was mounted.</span></span> <span data-ttu-id="0dca2-157">Žádná operace zálohování se spustí připojený svazek.</span><span class="sxs-lookup"><span data-stu-id="0dca2-157">No backup operations will run while the volume is mounted.</span></span> <span data-ttu-id="0dca2-158">Naplánované spuštění v době, kdy je svazek připojený, zálohování se spustí po obnovení svazku je odpojené.</span><span class="sxs-lookup"><span data-stu-id="0dca2-158">Any backup operation scheduled to run during the time when the volume is mounted, will run after the recovery volume is unmounted.</span></span>
    >


## <a name="recover-data-to-the-same-machine"></a><span data-ttu-id="0dca2-159">Obnovení dat na stejný počítač</span><span class="sxs-lookup"><span data-stu-id="0dca2-159">Recover data to the same machine</span></span>
<span data-ttu-id="0dca2-160">Pokud jste omylem odstranit soubor a chcete obnovit do stejného počítače (ze kterého dochází k zálohování), následující kroky vám pomohou obnovit data.</span><span class="sxs-lookup"><span data-stu-id="0dca2-160">If you accidentally deleted a file and wish to restore it to the same machine (from which the backup is taken), the following steps will help you recover the data.</span></span>

1. <span data-ttu-id="0dca2-161">Otevřete **Microsoft Azure Backup** přichycení v.</span><span class="sxs-lookup"><span data-stu-id="0dca2-161">Open the **Microsoft Azure Backup** snap in.</span></span>
2. <span data-ttu-id="0dca2-162">Klikněte na tlačítko **obnovit Data** inicializace pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="0dca2-162">Click **Recover Data** to initiate the workflow.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)
3. <span data-ttu-id="0dca2-164">Vyberte  **tento server (*yourmachinename*) ** možnost obnovení zálohy souboru ve stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="0dca2-164">Select the **This server (*yourmachinename*)** option to restore the backed up file on the same machine.</span></span>

    ![Stejný počítač](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. <span data-ttu-id="0dca2-166">Zvolit **Procházet soubory** nebo **vyhledávání souborů**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-166">Choose to **Browse for files** or **Search for files**.</span></span>

    <span data-ttu-id="0dca2-167">Ponechte výchozí možnost, pokud máte v plánu obnovení jeden nebo více souborů, jejichž cesta je známá.</span><span class="sxs-lookup"><span data-stu-id="0dca2-167">Leave the default option if you plan to restore one or more files whose path is known.</span></span> <span data-ttu-id="0dca2-168">Pokud nejste jisti o struktura složek, ale chcete hledat soubor, vyberte **vyhledávání souborů** možnost.</span><span class="sxs-lookup"><span data-stu-id="0dca2-168">If you are not sure about the folder structure but would like to search for a file, pick the **Search for files** option.</span></span> <span data-ttu-id="0dca2-169">Pro účely této části se budeme pokračovat v výchozí možnost.</span><span class="sxs-lookup"><span data-stu-id="0dca2-169">For the purpose of this section, we will proceed with the default option.</span></span>

    ![Procházet soubory](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. <span data-ttu-id="0dca2-171">Vyberte svazek, ze kterého chcete soubor obnovit.</span><span class="sxs-lookup"><span data-stu-id="0dca2-171">Select the volume from which you wish to restore the file.</span></span>

    <span data-ttu-id="0dca2-172">Můžete obnovit z libovolného bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="0dca2-172">You can restore from any point in time.</span></span> <span data-ttu-id="0dca2-173">Data, které jsou v **tučné** v ovládacím prvku kalendář označuje dostupnost bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-173">Dates which appear in **bold** in the calendar control indicate the availability of a restore point.</span></span> <span data-ttu-id="0dca2-174">Jakmile je vybrat datum, na základě plán zálohování (a úspěch operace zálohování), můžete vybrat bod v čase, ze **čas** rozevírací nabídku.</span><span class="sxs-lookup"><span data-stu-id="0dca2-174">Once a date is selected, based on your backup schedule (and the success of a backup operation), you can select a point in time from the **Time** drop down.</span></span>

    ![Svazek a datum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. <span data-ttu-id="0dca2-176">Vyberte položky, které chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="0dca2-176">Select the items to recover.</span></span> <span data-ttu-id="0dca2-177">Můžete vybrat víc složek nebo souborů, které chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="0dca2-177">You can multi-select folders/files you wish to restore.</span></span>

    ![Výběr souborů](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. <span data-ttu-id="0dca2-179">Zadejte parametry obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-179">Specify the recovery parameters.</span></span>

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * <span data-ttu-id="0dca2-181">Máte možnost obnovení do původního umístění (ve kterém soubor nebo složku, budou přepsána) nebo do jiného umístění ve stejném počítači.</span><span class="sxs-lookup"><span data-stu-id="0dca2-181">You have an option of restoring to the original location (in which the file/folder would be overwritten) or to another location in the same machine.</span></span>
   * <span data-ttu-id="0dca2-182">Pokud v cílovém umístění existuje soubor nebo složku, kterou chcete obnovit, můžete vytvořit kopie (dvě verze stejného souboru), přepisovat soubory v cílovém umístění nebo přeskočit obnovení souborů, které existují v cíl.</span><span class="sxs-lookup"><span data-stu-id="0dca2-182">If the file/folder you wish to restore exists in the target location, you can create copies (two versions of the same file), overwrite the files in the target location, or skip the recovery of the files which exist in the target.</span></span>
   * <span data-ttu-id="0dca2-183">Důrazně doporučujeme ponechat výchozí možnost obnovení seznamy ACL v souborech, které se obnovuje.</span><span class="sxs-lookup"><span data-stu-id="0dca2-183">It is highly recommended that you leave the default option of restoring the ACLs on the files which are being recovered.</span></span>
8. <span data-ttu-id="0dca2-184">Jakmile jsou k dispozici tyto vstupy, klikněte na možnost **Další**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-184">Once these inputs are provided, click **Next**.</span></span> <span data-ttu-id="0dca2-185">Pracovní postup obnovení, který obnoví soubory k tomuto počítači, bude zahájena.</span><span class="sxs-lookup"><span data-stu-id="0dca2-185">The recovery workflow, which restores the files to this machine, will begin.</span></span>

## <a name="recover-to-an-alternate-machine"></a><span data-ttu-id="0dca2-186">Obnovit na alternativní počítače</span><span class="sxs-lookup"><span data-stu-id="0dca2-186">Recover to an alternate machine</span></span>
<span data-ttu-id="0dca2-187">Pokud dojde ke ztrátě celý server, můžete stále obnovit data z Azure Backup na jiný počítač.</span><span class="sxs-lookup"><span data-stu-id="0dca2-187">If your entire server is lost, you can still recover data from Azure Backup to a different machine.</span></span> <span data-ttu-id="0dca2-188">Následující kroky popisují pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="0dca2-188">The following steps illustrate the workflow.</span></span>  

<span data-ttu-id="0dca2-189">Zahrnuje technologiím použitým v těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="0dca2-189">The terminology used in these steps includes:</span></span>

* <span data-ttu-id="0dca2-190">*Zdrojový počítač* – původní počítač, ze kterého bylo provedeno zálohování a který není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0dca2-190">*Source machine* – The original machine from which the backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="0dca2-191">*Cílový počítač* – počítače, do níž se obnovuje data.</span><span class="sxs-lookup"><span data-stu-id="0dca2-191">*Target machine* – The machine to which the data is being recovered.</span></span>
* <span data-ttu-id="0dca2-192">*Ukázka trezoru* – úložiště záloh, ke kterému *zdrojový počítač* a *cílový počítač* jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="0dca2-192">*Sample vault* – The Backup vault to which the *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="0dca2-193">Zálohy vytvořené z počítače nelze obnovit v počítači, který běží starší verze operačního systému.</span><span class="sxs-lookup"><span data-stu-id="0dca2-193">Backups taken from a machine cannot be restored on a machine which is running an earlier version of the operating system.</span></span> <span data-ttu-id="0dca2-194">Například pokud zálohy jsou převzaty z počítače s Windows 7, může být obnovena do systému Windows 8 nebo novější verze počítače.</span><span class="sxs-lookup"><span data-stu-id="0dca2-194">For example, if backups are taken from a Windows 7 machine, it can be restored on a Windows 8 or above machine.</span></span> <span data-ttu-id="0dca2-195">Naopak jsou však nemá hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="0dca2-195">However, the vice-versa does not hold true.</span></span>
>
>

1. <span data-ttu-id="0dca2-196">Otevřete **Microsoft Azure Backup** přichycení v na *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="0dca2-196">Open the **Microsoft Azure Backup** snap in on the *Target machine*.</span></span>
2. <span data-ttu-id="0dca2-197">Ujistěte se, že *cílový počítač* a *zdrojový počítač* jsou registrované na stejné úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="0dca2-197">Ensure that the *Target machine* and the *Source machine* are registered to the same backup vault.</span></span>
3. <span data-ttu-id="0dca2-198">Klikněte na tlačítko **obnovit Data** inicializace pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="0dca2-198">Click **Recover Data** to initiate the workflow.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)
4. <span data-ttu-id="0dca2-200">Vyberte **jiný server**</span><span class="sxs-lookup"><span data-stu-id="0dca2-200">Select **Another server**</span></span>

    ![Jiný Server](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. <span data-ttu-id="0dca2-202">Zadejte soubor s přihlašovacími údaji trezoru, která odpovídá *ukázka trezoru*.</span><span class="sxs-lookup"><span data-stu-id="0dca2-202">Provide the vault credential file that corresponds to the *Sample vault*.</span></span> <span data-ttu-id="0dca2-203">Pokud soubor s přihlašovacími údaji trezoru je neplatný (nebo vypršela platnost), stáhněte si nový soubor přihlašovacích údajů trezoru z *ukázka trezoru* na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="0dca2-203">If the vault credential file is invalid (or expired) download a new vault credential file from the *Sample vault* in the Azure classic portal.</span></span> <span data-ttu-id="0dca2-204">Jakmile je zadaný soubor s přihlašovacími údaji trezoru, se zobrazí úložiště záloh proti soubor s přihlašovacími údaji trezoru.</span><span class="sxs-lookup"><span data-stu-id="0dca2-204">Once the vault credential file is provided, the backup vault against the vault credential file is displayed.</span></span>
6. <span data-ttu-id="0dca2-205">Vyberte *zdrojový počítač* ze seznamu zobrazených počítačů.</span><span class="sxs-lookup"><span data-stu-id="0dca2-205">Select the *Source machine* from the list of displayed machines.</span></span>

    ![Seznam počítačů](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. <span data-ttu-id="0dca2-207">Vyberte buď **vyhledávání souborů** nebo **Procházet soubory** možnost.</span><span class="sxs-lookup"><span data-stu-id="0dca2-207">Select either the **Search for files** or **Browse for files** option.</span></span> <span data-ttu-id="0dca2-208">Pro účely této části se budeme používat **vyhledávání souborů** možnost.</span><span class="sxs-lookup"><span data-stu-id="0dca2-208">For the purpose of this section, we will use the **Search for files** option.</span></span>

    ![Search](./media/backup-azure-restore-windows-server-classic/search.png)
8. <span data-ttu-id="0dca2-210">Vyberte svazek a datum na další obrazovce.</span><span class="sxs-lookup"><span data-stu-id="0dca2-210">Select the volume and date in the next screen.</span></span> <span data-ttu-id="0dca2-211">Vyhledávání pro název složka či soubor, který chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="0dca2-211">Search for the folder/file name you want to restore.</span></span>

    ![Hledání položek](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. <span data-ttu-id="0dca2-213">Vyberte umístění, kde je nutné obnovit soubory.</span><span class="sxs-lookup"><span data-stu-id="0dca2-213">Select the location where the files need to be restored.</span></span>

    ![Obnovení umístění](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. <span data-ttu-id="0dca2-215">Zadejte šifrovací heslo, které jste zadali během *zdrojový počítač* registraci *ukázka trezoru*.</span><span class="sxs-lookup"><span data-stu-id="0dca2-215">Provide the encryption passphrase that was provided during *Source machine’s* registration to *Sample vault*.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. <span data-ttu-id="0dca2-217">Jakmile je k dispozici vstup, klikněte na **obnovit**, která aktivuje obnovení do cílového umístění zadané zálohované soubory.</span><span class="sxs-lookup"><span data-stu-id="0dca2-217">Once the input is provided, click **Recover**, which triggers the restore of the backed up files to the destination provided.</span></span>

## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a><span data-ttu-id="0dca2-218">K obnovení dat alternativní počítači použít rychlé obnovení</span><span class="sxs-lookup"><span data-stu-id="0dca2-218">Use Instant Restore to restore data to an alternate machine</span></span>
<span data-ttu-id="0dca2-219">Pokud dojde ke ztrátě celý server, můžete stále obnovit data z Azure Backup na jiný počítač.</span><span class="sxs-lookup"><span data-stu-id="0dca2-219">If your entire server is lost, you can still recover data from Azure Backup to a different machine.</span></span> <span data-ttu-id="0dca2-220">Následující kroky popisují pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="0dca2-220">The following steps illustrate the workflow.</span></span>

<span data-ttu-id="0dca2-221">Zahrnuje technologiím použitým v těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="0dca2-221">The terminology used in these steps includes:</span></span>

* <span data-ttu-id="0dca2-222">*Zdrojový počítač* – původní počítač, ze kterého bylo provedeno zálohování a který není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0dca2-222">*Source machine* – The original machine from which the backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="0dca2-223">*Cílový počítač* – počítače, do níž se obnovuje data.</span><span class="sxs-lookup"><span data-stu-id="0dca2-223">*Target machine* – The machine to which the data is being recovered.</span></span>
* <span data-ttu-id="0dca2-224">*Ukázka trezoru* – trezor služeb zotavení, ke kterému *zdrojový počítač* a *cílový počítač* jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="0dca2-224">*Sample vault* – The Recovery Services vault to which the *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="0dca2-225">Zálohování nelze obnovit do cílového počítače, který používá starší verzi operačního systému.</span><span class="sxs-lookup"><span data-stu-id="0dca2-225">Backups can't be restored to a target machine running an earlier version of the operating system.</span></span> <span data-ttu-id="0dca2-226">Například převzat ze systému Windows 7, počítač může být obnovena záloha v systému Windows 8 nebo novější, počítače.</span><span class="sxs-lookup"><span data-stu-id="0dca2-226">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="0dca2-227">Zálohy z počítače se systémem Windows 8 není možné obnovit do počítače Windows 7.</span><span class="sxs-lookup"><span data-stu-id="0dca2-227">A backup taken from a Windows 8 computer cannot be restored to a Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="0dca2-228">Otevřete **Microsoft Azure Backup** přichycení v na *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="0dca2-228">Open the **Microsoft Azure Backup** snap in on the *Target machine*.</span></span>

2. <span data-ttu-id="0dca2-229">Ujistěte se, *cílový počítač* a *zdrojový počítač* jsou registrované ke stejnému trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-229">Ensure the *Target machine* and the *Source machine* are registered to the same Recovery Services vault.</span></span>

3. <span data-ttu-id="0dca2-230">Klikněte na tlačítko **obnovit Data** otevřete **Průvodce obnovení dat**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-230">Click **Recover Data** to open the **Recover Data wizard**.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="0dca2-232">Na **Začínáme** podokně, vyberte **jiný server**</span><span class="sxs-lookup"><span data-stu-id="0dca2-232">On the **Getting Started** pane, select **Another server**</span></span>

    ![Jiný Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="0dca2-234">Zadejte soubor s přihlašovacími údaji trezoru, která odpovídá *ukázka trezoru*a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-234">Provide the vault credential file that corresponds to the *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="0dca2-235">Pokud soubor s přihlašovacími údaji trezoru je neplatný (nebo vypršela platnost), stáhněte si nový soubor přihlašovacích údajů trezoru z *ukázka trezoru* na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0dca2-235">If the vault credential file is invalid (or expired), download a new vault credential file from the *Sample vault* in the Azure portal.</span></span> <span data-ttu-id="0dca2-236">Po zadání přihlašovacích údajů platné úložiště, zobrazí se název odpovídajícího úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="0dca2-236">Once you provide a valid vault credential, the name of the corresponding Backup Vault appears.</span></span>

6. <span data-ttu-id="0dca2-237">Na **vyberte zálohování serveru** podokně, vyberte *zdrojový počítač* ze seznamu zobrazených počítačů a zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="0dca2-237">On the **Select Backup Server** pane, select the *Source machine* from the list of displayed machines and provide the passphrase.</span></span> <span data-ttu-id="0dca2-238">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-238">Then click **Next**.</span></span>

    ![Seznam počítačů](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="0dca2-240">Na **vyberte režimu obnovení** podokně, vyberte **jednotlivých souborů a složek** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-240">On the **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Search](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="0dca2-242">Na **vyberte svazek a datum** podokně, vyberte svazek, který obsahuje soubory nebo složky, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="0dca2-242">On the **Select Volume and Date** pane, select the volume that contains the files and/or folders you want to restore.</span></span>

    <span data-ttu-id="0dca2-243">V kalendáři vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-243">On the calendar, select a recovery point.</span></span> <span data-ttu-id="0dca2-244">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="0dca2-244">You can restore from any recovery point in time.</span></span> <span data-ttu-id="0dca2-245">Data v **tučné** označuje dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="0dca2-245">Dates in **bold** indicate the availability of at least one recovery point.</span></span> <span data-ttu-id="0dca2-246">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte konkrétní bod obnovení z **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="0dca2-246">Once you select a date, if multiple recovery points are available, choose the specific recovery point from the **Time** drop-down menu.</span></span>

    ![Hledání položek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="0dca2-248">Klikněte na tlačítko **připojit** místně připojit bod obnovení jako svazek obnovení na vaše *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="0dca2-248">Click **Mount** to locally mount the recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="0dca2-249">Na **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** otevřete Průzkumníka Windows a soubory a složky, které chcete najít.</span><span class="sxs-lookup"><span data-stu-id="0dca2-249">On the **Browse and Recover Files** pane, click **Browse** to open Windows Explorer and find the files and folders you want.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="0dca2-251">V Průzkumníku Windows, zkopírujte soubory nebo složky ze svazku obnovení a vložte je do vašeho *cílový počítač* umístění.</span><span class="sxs-lookup"><span data-stu-id="0dca2-251">In Windows Explorer, copy the files and/or folders from the recovery volume and paste them to your *Target machine* location.</span></span> <span data-ttu-id="0dca2-252">Můžete otevřít nebo stream soubory přímo ze svazku obnovení a ověřte, že se obnoví správné verze.</span><span class="sxs-lookup"><span data-stu-id="0dca2-252">You can open or stream the files directly from the recovery volume and verify the correct versions are recovered.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="0dca2-254">Po dokončení obnovení souborů a složek, na **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="0dca2-254">When you are finished restoring the files and/or folders, on the **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="0dca2-255">Pak klikněte na tlačítko **Ano** potvrďte, že chcete odpojit svazek.</span><span class="sxs-lookup"><span data-stu-id="0dca2-255">Then click **Yes** to confirm that you want to unmount the volume.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="0dca2-257">Pokud neklikejte na odpojení, zůstane svazek obnovení připojené šest hodin od okamžiku, kdy byla připojena.</span><span class="sxs-lookup"><span data-stu-id="0dca2-257">If you do not click Unmount, the Recovery Volume will remain mounted for six hours from the time when it was mounted.</span></span> <span data-ttu-id="0dca2-258">Žádná operace zálohování se spustí připojený svazek.</span><span class="sxs-lookup"><span data-stu-id="0dca2-258">No backup operations will run while the volume is mounted.</span></span> <span data-ttu-id="0dca2-259">Naplánované spuštění v době, kdy je svazek připojený, zálohování se spustí po obnovení svazku je odpojené.</span><span class="sxs-lookup"><span data-stu-id="0dca2-259">Any backup operation scheduled to run during the time when the volume is mounted, will run after the recovery volume is unmounted.</span></span>
    >


## <a name="next-steps"></a><span data-ttu-id="0dca2-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0dca2-260">Next steps</span></span>
* [<span data-ttu-id="0dca2-261">Azure Backup – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="0dca2-261">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
* <span data-ttu-id="0dca2-262">Přejděte [fórum Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span><span class="sxs-lookup"><span data-stu-id="0dca2-262">Visit the [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span></span>

## <a name="learn-more"></a><span data-ttu-id="0dca2-263">Další informace</span><span class="sxs-lookup"><span data-stu-id="0dca2-263">Learn more</span></span>
* [<span data-ttu-id="0dca2-264">Přehled služby Azure Backup</span><span class="sxs-lookup"><span data-stu-id="0dca2-264">Azure Backup Overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [<span data-ttu-id="0dca2-265">Zálohování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="0dca2-265">Backup Azure virtual machines</span></span>](backup-azure-vms-introduction.md)
* [<span data-ttu-id="0dca2-266">Zálohování se úlohy Microsoft</span><span class="sxs-lookup"><span data-stu-id="0dca2-266">Backup up Microsoft workloads</span></span>](backup-azure-dpm-introduction.md)
