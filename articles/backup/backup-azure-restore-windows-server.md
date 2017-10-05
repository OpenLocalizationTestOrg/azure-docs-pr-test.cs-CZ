---
title: "Obnovení dat v Azure pro Windows Server nebo počítač se systémem Windows | Microsoft Docs"
description: "Zjistěte, jak obnovit data uložená v Azure a Windows Server nebo počítač se systémem Windows."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/16/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 231dd61f95267b3a504ed70e9b3a5abc470b69b2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a><span data-ttu-id="524b3-103">Obnovení souborů na serveru Windows nebo klientském počítači s využitím modelu nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="524b3-103">Restore files to a Windows server or Windows client machine using Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="524b3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="524b3-104">Azure portal</span></span>](backup-azure-restore-windows-server.md)
> * [<span data-ttu-id="524b3-105">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="524b3-105">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
>
>

<span data-ttu-id="524b3-106">Tento článek vysvětluje, jak obnovit data z úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="524b3-106">This article explains how to restore data from a backup vault.</span></span> <span data-ttu-id="524b3-107">Chcete-li obnovit data, použijte Průvodce obnovení dat v agentovi nástroje Microsoft Azure Recovery Services (MARS).</span><span class="sxs-lookup"><span data-stu-id="524b3-107">To restore data, you use the Recover Data wizard in the Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="524b3-108">Při obnovování dat, je možné:</span><span class="sxs-lookup"><span data-stu-id="524b3-108">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="524b3-109">Obnovení dat na stejný počítač, ve kterém byly provedeny zálohy.</span><span class="sxs-lookup"><span data-stu-id="524b3-109">Restore data to the same machine from which the backups were taken.</span></span>
* <span data-ttu-id="524b3-110">Obnovte data do alternativní počítače.</span><span class="sxs-lookup"><span data-stu-id="524b3-110">Restore data to an alternate machine.</span></span>

<span data-ttu-id="524b3-111">V lednu 2017 společnost Microsoft vydala Preview aktualizace agenta MARS.</span><span class="sxs-lookup"><span data-stu-id="524b3-111">In January 2017, Microsoft released a Preview update to the MARS agent.</span></span> <span data-ttu-id="524b3-112">Společně s oprav chyb tato aktualizace umožňuje rychlé obnovení, která umožňuje připojit obnovení zapisovatelného snímku bodu jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-112">Along with bug fixes, this update enables Instant Restore, which allows you to mount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="524b3-113">Pak můžete zkoumat obnovení svazku a zkopírujte soubory do místního počítače a následné obnovení souborů.</span><span class="sxs-lookup"><span data-stu-id="524b3-113">You can then explore the recovery volume and copy files to a local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="524b3-114">[Ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) je povinný, pokud chcete obnovit data pomocí funkce Rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-114">The [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want to use Instant Restore to restore data.</span></span> <span data-ttu-id="524b3-115">Zálohovaná data musí být chráněna v trezorů v národní prostředí, které jsou uvedené v článku podpory.</span><span class="sxs-lookup"><span data-stu-id="524b3-115">Also the backup data must be protected in vaults in locales listed in the support article.</span></span> <span data-ttu-id="524b3-116">Obrátit [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) nejnovější seznam národních prostředí, které podporují rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-116">Consult the [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for the latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="524b3-117">Rychlé obnovení je **není** aktuálně k dispozici ve všech národních prostředí.</span><span class="sxs-lookup"><span data-stu-id="524b3-117">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="524b3-118">Rychlé obnovení je k dispozici pro použití v trezory služeb zotavení v portálu Azure a trezory Backup na portálu classic.</span><span class="sxs-lookup"><span data-stu-id="524b3-118">Instant Restore is available for use in Recovery Services vaults in the Azure portal and Backup vaults in the classic portal.</span></span> <span data-ttu-id="524b3-119">Pokud chcete použít rychlé obnovení, stažení aktualizace MARS a postupujte podle pokynů, které zmínili, rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-119">If you want to use Instant Restore, download the MARS update, and follow the procedures that mention Instant Restore.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a><span data-ttu-id="524b3-120">Obnovit data do stejného počítače pomocí funkce Rychlé obnovení</span><span class="sxs-lookup"><span data-stu-id="524b3-120">Use Instant Restore to recover data to the same machine</span></span>

<span data-ttu-id="524b3-121">Pokud jste omylem odstranit soubor a chcete obnovit do stejného počítače (ze kterého dochází k zálohování), následující kroky vám pomohou obnovit data.</span><span class="sxs-lookup"><span data-stu-id="524b3-121">If you accidentally deleted a file and wish to restore it to the same machine (from which the backup is taken), the following steps will help you recover the data.</span></span>

1. <span data-ttu-id="524b3-122">Otevřete **Microsoft Azure Backup** přichycení v.</span><span class="sxs-lookup"><span data-stu-id="524b3-122">Open the **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="524b3-123">Pokud si nejste jisti, kde byl nainstalován modul snap-in, hledání se počítač nebo server pro **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="524b3-123">If you don't know where the snap in was installed, search the computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="524b3-124">Desktopová aplikace by se ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="524b3-124">The desktop app should appear in the search results.</span></span>

2. <span data-ttu-id="524b3-125">Klikněte na tlačítko **obnovit Data** spusťte průvodce.</span><span class="sxs-lookup"><span data-stu-id="524b3-125">Click **Recover Data** to start the wizard.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="524b3-127">Na **Začínáme** , chcete-li obnovit data na stejném serveru nebo počítače, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="524b3-127">On the **Getting Started** pane, to restore the data to the same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Vyberte tuto možnost serveru k obnovení dat na stejný počítač](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="524b3-129">Na **vyberte režimu obnovení** podokně vyberte **jednotlivých souborů a složek** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="524b3-129">On the **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Procházet soubory](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="524b3-131">Na **vyberte svazek a datum** podokně, vyberte svazek, který obsahuje soubory nebo složky, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="524b3-131">On the **Select Volume and Date** pane, select the volume that contains the files and/or folders you want to restore.</span></span>

    <span data-ttu-id="524b3-132">V kalendáři vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-132">On the calendar, select a recovery point.</span></span> <span data-ttu-id="524b3-133">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="524b3-133">You can restore from any recovery point in time.</span></span> <span data-ttu-id="524b3-134">Data v **tučné** označuje dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-134">Dates in **bold** indicate the availability of at least one recovery point.</span></span> <span data-ttu-id="524b3-135">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte konkrétní bod obnovení z **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="524b3-135">Once you select a date, if multiple recovery points are available, choose the specific recovery point from the **Time** drop-down menu.</span></span>

    ![Svazek a datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="524b3-137">Po zadání bodu obnovení pro obnovení, klikněte na tlačítko **připojit**.</span><span class="sxs-lookup"><span data-stu-id="524b3-137">Once you have chosen the recovery point to restore, click **Mount**.</span></span>

    <span data-ttu-id="524b3-138">Zálohování Azure připojí bod místní obnovení a používá je jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-138">Azure Backup mounts the local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="524b3-139">Na **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** otevřete Průzkumníka Windows a soubory a složky, které chcete najít.</span><span class="sxs-lookup"><span data-stu-id="524b3-139">On the **Browse and Recover Files** pane, click **Browse** to open Windows Explorer and find the files and folders you want.</span></span>

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="524b3-141">V Průzkumníku Windows zkopírujte soubory nebo složky, kterou chcete obnovit a vložit do libovolného umístění místní k serveru nebo počítači.</span><span class="sxs-lookup"><span data-stu-id="524b3-141">In Windows Explorer, copy the files and/or folders you want to restore and paste them to any location local to the server or computer.</span></span> <span data-ttu-id="524b3-142">Můžete otevřít nebo stream soubory přímo ze svazku obnovení a ověřte, že se obnoví správné verze.</span><span class="sxs-lookup"><span data-stu-id="524b3-142">You can open or stream the files directly from the recovery volume and verify the correct versions are recovered.</span></span>

    ![Zkopírujte a vložte souborů a složek z připojeného svazku do místního umístění](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="524b3-144">Po dokončení obnovení souborů a složek, na **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="524b3-144">When you are finished restoring the files and/or folders, on the **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="524b3-145">Pak klikněte na tlačítko **Ano** potvrďte, že chcete odpojit svazek.</span><span class="sxs-lookup"><span data-stu-id="524b3-145">Then click **Yes** to confirm that you want to unmount the volume.</span></span>

    ![Odpojte Image svazku a potvrďte](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="524b3-147">Pokud neklikejte na odpojení, zůstane svazek obnovení připojené 6 hodin od okamžiku, kdy byla připojena.</span><span class="sxs-lookup"><span data-stu-id="524b3-147">If you do not click Unmount, the Recovery Volume will remain mounted for 6 hours from the time when it was mounted.</span></span> <span data-ttu-id="524b3-148">Čas připojení je však rozšířené maximálně 24 hodin, v případě probíhající kopírování souborů.</span><span class="sxs-lookup"><span data-stu-id="524b3-148">However, the mount time is extended upto a maximum of 24 hours in case of an ongoing file-copy.</span></span> <span data-ttu-id="524b3-149">Žádná operace zálohování se spustí připojený svazek.</span><span class="sxs-lookup"><span data-stu-id="524b3-149">No backup operations will run while the volume is mounted.</span></span> <span data-ttu-id="524b3-150">Naplánované spuštění v době, kdy je svazek připojený, zálohování se spustí po obnovení svazku je odpojené.</span><span class="sxs-lookup"><span data-stu-id="524b3-150">Any backup operation scheduled to run during the time when the volume is mounted, will run after the recovery volume is unmounted.</span></span>
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a><span data-ttu-id="524b3-151">K obnovení dat alternativní počítači použít rychlé obnovení</span><span class="sxs-lookup"><span data-stu-id="524b3-151">Use Instant Restore to restore data to an alternate machine</span></span>
<span data-ttu-id="524b3-152">Pokud dojde ke ztrátě celý server, můžete stále obnovit data z Azure Backup na jiný počítač.</span><span class="sxs-lookup"><span data-stu-id="524b3-152">If your entire server is lost, you can still recover data from Azure Backup to a different machine.</span></span> <span data-ttu-id="524b3-153">Následující kroky popisují pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="524b3-153">The following steps illustrate the workflow.</span></span>


<span data-ttu-id="524b3-154">Zahrnuje technologiím použitým v těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="524b3-154">The terminology used in these steps includes:</span></span>

* <span data-ttu-id="524b3-155">*Zdrojový počítač* – původní počítač, ze kterého bylo provedeno zálohování a který není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="524b3-155">*Source machine* – The original machine from which the backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="524b3-156">*Cílový počítač* – počítače, do níž se obnovuje data.</span><span class="sxs-lookup"><span data-stu-id="524b3-156">*Target machine* – The machine to which the data is being recovered.</span></span>
* <span data-ttu-id="524b3-157">*Ukázka trezoru* – trezor služeb zotavení, ke kterému *zdrojový počítač* a *cílový počítač* jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="524b3-157">*Sample vault* – The Recovery Services vault to which the *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="524b3-158">Zálohování nelze obnovit do cílového počítače, který používá starší verzi operačního systému.</span><span class="sxs-lookup"><span data-stu-id="524b3-158">Backups can't be restored to a target machine running an earlier version of the operating system.</span></span> <span data-ttu-id="524b3-159">Například převzat ze systému Windows 7, počítač může být obnovena záloha v systému Windows 8 nebo novější, počítače.</span><span class="sxs-lookup"><span data-stu-id="524b3-159">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="524b3-160">Zálohy z počítače se systémem Windows 8 není možné obnovit do počítače Windows 7.</span><span class="sxs-lookup"><span data-stu-id="524b3-160">A backup taken from a Windows 8 computer cannot be restored to a Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="524b3-161">Otevřete **Microsoft Azure Backup** přichycení v na *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="524b3-161">Open the **Microsoft Azure Backup** snap in on the *Target machine*.</span></span>

2. <span data-ttu-id="524b3-162">Ujistěte se, *cílový počítač* a *zdrojový počítač* jsou registrované ke stejnému trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="524b3-162">Ensure the *Target machine* and the *Source machine* are registered to the same Recovery Services vault.</span></span>

3. <span data-ttu-id="524b3-163">Klikněte na tlačítko **obnovit Data** otevřete **Průvodce obnovení dat**.</span><span class="sxs-lookup"><span data-stu-id="524b3-163">Click **Recover Data** to open the **Recover Data wizard**.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="524b3-165">Na **Začínáme** podokně, vyberte **jiný server**</span><span class="sxs-lookup"><span data-stu-id="524b3-165">On the **Getting Started** pane, select **Another server**</span></span>

    ![Jiný Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="524b3-167">Zadejte soubor s přihlašovacími údaji trezoru, která odpovídá *ukázka trezoru*a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="524b3-167">Provide the vault credential file that corresponds to the *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="524b3-168">Pokud soubor s přihlašovacími údaji trezoru je neplatný (nebo vypršela platnost), stáhněte si nový soubor přihlašovacích údajů trezoru z *ukázka trezoru* na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="524b3-168">If the vault credential file is invalid (or expired), download a new vault credential file from the *Sample vault* in the Azure portal.</span></span> <span data-ttu-id="524b3-169">Po zadání přihlašovacích údajů platné úložiště, zobrazí se název odpovídajícího úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="524b3-169">Once you provide a valid vault credential, the name of the corresponding Backup Vault appears.</span></span>


6. <span data-ttu-id="524b3-170">Na **vyberte zálohování serveru** podokně, vyberte *zdrojový počítač* ze seznamu zobrazených počítačů a zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="524b3-170">On the **Select Backup Server** pane, select the *Source machine* from the list of displayed machines and provide the passphrase.</span></span> <span data-ttu-id="524b3-171">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="524b3-171">Then click **Next**.</span></span>

    ![Seznam počítačů](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="524b3-173">Na **vyberte režimu obnovení** podokně, vyberte **jednotlivých souborů a složek** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="524b3-173">On the **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Search](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="524b3-175">Na **vyberte svazek a datum** podokně, vyberte svazek, který obsahuje soubory nebo složky, kterou chcete obnovit.</span><span class="sxs-lookup"><span data-stu-id="524b3-175">On the **Select Volume and Date** pane, select the volume that contains the files and/or folders you want to restore.</span></span>

    <span data-ttu-id="524b3-176">V kalendáři vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-176">On the calendar, select a recovery point.</span></span> <span data-ttu-id="524b3-177">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="524b3-177">You can restore from any recovery point in time.</span></span> <span data-ttu-id="524b3-178">Data v **tučné** označuje dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-178">Dates in **bold** indicate the availability of at least one recovery point.</span></span> <span data-ttu-id="524b3-179">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte konkrétní bod obnovení z **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="524b3-179">Once you select a date, if multiple recovery points are available, choose the specific recovery point from the **Time** drop-down menu.</span></span>

    ![Hledání položek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="524b3-181">Klikněte na tlačítko **připojit** místně připojit bod obnovení jako svazek obnovení na vaše *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="524b3-181">Click **Mount** to locally mount the recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="524b3-182">Na **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** otevřete Průzkumníka Windows a soubory a složky, které chcete najít.</span><span class="sxs-lookup"><span data-stu-id="524b3-182">On the **Browse and Recover Files** pane, click **Browse** to open Windows Explorer and find the files and folders you want.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="524b3-184">V Průzkumníku Windows, zkopírujte soubory nebo složky ze svazku obnovení a vložte je do vašeho *cílový počítač* umístění.</span><span class="sxs-lookup"><span data-stu-id="524b3-184">In Windows Explorer, copy the files and/or folders from the recovery volume and paste them to your *Target machine* location.</span></span> <span data-ttu-id="524b3-185">Můžete otevřít nebo stream soubory přímo ze svazku obnovení a ověřte, že se obnoví správné verze.</span><span class="sxs-lookup"><span data-stu-id="524b3-185">You can open or stream the files directly from the recovery volume and verify the correct versions are recovered.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="524b3-187">Po dokončení obnovení souborů a složek, na **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="524b3-187">When you are finished restoring the files and/or folders, on the **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="524b3-188">Pak klikněte na tlačítko **Ano** potvrďte, že chcete odpojit svazek.</span><span class="sxs-lookup"><span data-stu-id="524b3-188">Then click **Yes** to confirm that you want to unmount the volume.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="524b3-190">Pokud neklikejte na odpojení, zůstane svazek obnovení připojené 6 hodin od okamžiku, kdy byla připojena.</span><span class="sxs-lookup"><span data-stu-id="524b3-190">If you do not click Unmount, the Recovery Volume will remain mounted for 6 hours from the time when it was mounted.</span></span> <span data-ttu-id="524b3-191">Čas připojení je však rozšířené maximálně 24 hodin, v případě probíhající kopírování souborů.</span><span class="sxs-lookup"><span data-stu-id="524b3-191">However, the mount time is extended upto a maximum of 24 hours in case of an ongoing file-copy.</span></span> <span data-ttu-id="524b3-192">Žádná operace zálohování se spustí připojený svazek.</span><span class="sxs-lookup"><span data-stu-id="524b3-192">No backup operations will run while the volume is mounted.</span></span> <span data-ttu-id="524b3-193">Naplánované spuštění v době, kdy je svazek připojený, zálohování se spustí po obnovení svazku je odpojené.</span><span class="sxs-lookup"><span data-stu-id="524b3-193">Any backup operation scheduled to run during the time when the volume is mounted, will run after the recovery volume is unmounted.</span></span>
    >

## <a name="troubleshooting"></a><span data-ttu-id="524b3-194">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="524b3-194">Troubleshooting</span></span>
<span data-ttu-id="524b3-195">Pokud nelze zálohy Azure úspěšně připojit svazek obnovení ani po několika minutách kliknutí na možnost **připojit** nebo nepodaří připojit svazek obnovení jedné nebo více chybám, použijte následující postup zahájíte obnovení normálně.</span><span class="sxs-lookup"><span data-stu-id="524b3-195">If Azure Backup does not successfully mount the recovery volume even after several minutes of clicking **Mount** or fails to mount the recovery volume with one or more errors, follow the steps below to begin recovering normally.</span></span>

1.  <span data-ttu-id="524b3-196">Zrušte proces probíhající připojení v případě, že po spuštění pro několik minut.</span><span class="sxs-lookup"><span data-stu-id="524b3-196">Cancel the ongoing mount process in case it has been running for several minutes.</span></span>

2.  <span data-ttu-id="524b3-197">Ujistěte se, že jste na nejnovější verzi agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="524b3-197">Ensure that you are on the latest version of the Azure Backup agent.</span></span> <span data-ttu-id="524b3-198">Chcete-li zjistit informace o verzi agenta Azure Backup, klikněte na **o Microsoft agenta služeb zotavení Azure** na **akce** podokně služby Microsoft Azure Backup konzoly a ujistěte se, že **Verze** číslo je rovna nebo vyšší než verze uvedené v [v tomto článku](https://go.microsoft.com/fwlink/?linkid=229525).</span><span class="sxs-lookup"><span data-stu-id="524b3-198">To find out the version information of Azure Backup agent, click on **About Microsoft Azure Recovery Services Agent** on the **Actions** pane of Microsoft Azure Backup console and ensure that the **Version** number is equal to or higher than the version mentioned in [this article](https://go.microsoft.com/fwlink/?linkid=229525).</span></span> <span data-ttu-id="524b3-199">Můžete si stáhnout nejnovější verzi z [sem](https://go.microsoft.com/fwLink/?LinkID=288905)</span><span class="sxs-lookup"><span data-stu-id="524b3-199">You can download the latest version from [here](https://go.microsoft.com/fwLink/?LinkID=288905)</span></span>

3.  <span data-ttu-id="524b3-200">Přejděte na **Správce zařízení** -> **řadiče úložiště** a ujistěte se, že je možné najít **iniciátor iSCSI společnosti Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="524b3-200">Go to **Device Manager** -> **Storage Controllers** and ensure that you can locate **Microsoft iSCSI Initiator**.</span></span> <span data-ttu-id="524b3-201">Pokud ji můžete najít, přímo přejděte ke kroku 7 níže.</span><span class="sxs-lookup"><span data-stu-id="524b3-201">If you can locate it, directly go to step 7 below.</span></span> 

4.  <span data-ttu-id="524b3-202">Pokud služba iniciátoru iSCSI společnosti Microsoft nelze najít, jak je uvedeno v kroku 3, zkontrolujte, budete moci najít položku v části **Správce zařízení** -> **řadiče úložiště** názvem  **Neznámé zařízení** s ID hardwaru **ROOT\ISCSIPRT**.</span><span class="sxs-lookup"><span data-stu-id="524b3-202">If you cannot locate Microsoft iSCSI Initiator service as mentioned in step 3, check to see if you can find an entry under **Device Manager** -> **Storage Controllers** called **Unknown Device** with Hardware ID **ROOT\ISCSIPRT**.</span></span>

5.  <span data-ttu-id="524b3-203">Klikněte pravým tlačítkem na **neznámé zařízení** a vyberte **aktualizace ovladače**.</span><span class="sxs-lookup"><span data-stu-id="524b3-203">Right click on **Unknown Device** and select **Update Driver Software**.</span></span>

6.  <span data-ttu-id="524b3-204">Aktualizovat ovladač tak, že vyberete možnost **vyhledání automaticky aktualizovaný ovladač softwaru**.</span><span class="sxs-lookup"><span data-stu-id="524b3-204">Update the driver by selecting the option to  **Search automatically for updated driver software**.</span></span> <span data-ttu-id="524b3-205">Dokončení aktualizace by se měl změnit **neznámé zařízení** k **iniciátor iSCSI společnosti Microsoft** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="524b3-205">Completion of the update should change **Unknown Device** to **Microsoft iSCSI Initiator** as shown below.</span></span> 

    ![Šifrování](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  <span data-ttu-id="524b3-207">Přejděte na **Správce úloh** -> **služby (místní počítač)** -> **služba iniciátoru iSCSI společnosti Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="524b3-207">Go to **Task Manager** -> **Services (Local)** -> **Microsoft iSCSI Initiator Service**.</span></span> 

    ![Šifrování](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  <span data-ttu-id="524b3-209">Restartujte službu iniciátoru iSCSI společnosti Microsoft kliknutím pravým tlačítkem na službu, kliknutím na **Zastavit** a další kliknutím pravým tlačítkem na znovu a kliknete na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="524b3-209">Restart the Microsoft iSCSI Initiator service by right-clicking on the service, clicking on **Stop** and further right clicking again and clicking on **Start**.</span></span>

9.  <span data-ttu-id="524b3-210">Opakujte obnovení pomocí rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="524b3-210">Retry recovering using Instant Restore.</span></span> 

<span data-ttu-id="524b3-211">Pokud se obnovení stále nedaří, restartujte server nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="524b3-211">If the recovery still fails, reboot your server/client.</span></span> <span data-ttu-id="524b3-212">Pokud restartování není žádoucí nebo obnovení stále nedaří i po restartování serveru, pokuste obnovení provést z alternativní počítače a obraťte se na podporu Azure přechodem na [portálu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a odeslání žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="524b3-212">If a reboot is not desirable or the recovery still fails even after rebooting the server, try recovering from an Alternate Machine, and contact Azure Support by going to [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and submitting a support request.</span></span>

## <a name="next-steps"></a><span data-ttu-id="524b3-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="524b3-213">Next steps</span></span>
* <span data-ttu-id="524b3-214">Teď, když jste obnovit soubory a složky, můžete [spravovat zálohy](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="524b3-214">Now that you've recovered your files and folders, you can [manage your backups](backup-azure-manage-windows-server.md).</span></span>
