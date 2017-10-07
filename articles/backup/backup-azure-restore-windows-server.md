---
title: "aaaRestore data v Azure tooa systému Windows Server nebo počítač se systémem Windows | Microsoft Docs"
description: "Zjistěte, jak toorestore data uložená v Azure tooa systému Windows Server nebo počítač se systémem Windows."
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
ms.openlocfilehash: 11335495e448986a74f1733406f87e04331641d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a><span data-ttu-id="bcd94-103">Obnovit soubory tooa Windows server nebo klientský počítač systému Windows pomocí modelu nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="bcd94-103">Restore files tooa Windows server or Windows client machine using Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bcd94-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bcd94-104">Azure portal</span></span>](backup-azure-restore-windows-server.md)
> * [<span data-ttu-id="bcd94-105">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="bcd94-105">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
>
>

<span data-ttu-id="bcd94-106">Tento článek vysvětluje, jak toorestore data z trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="bcd94-106">This article explains how toorestore data from a backup vault.</span></span> <span data-ttu-id="bcd94-107">toorestore data, která používáte Průvodce obnovení dat hello v agentovi Microsoft Azure Recovery Services (MARS) hello.</span><span class="sxs-lookup"><span data-stu-id="bcd94-107">toorestore data, you use hello Recover Data wizard in hello Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="bcd94-108">Při obnovování dat, je možné:</span><span class="sxs-lookup"><span data-stu-id="bcd94-108">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="bcd94-109">Obnovení dat toohello stejný počítač, ze které hello zálohy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="bcd94-109">Restore data toohello same machine from which hello backups were taken.</span></span>
* <span data-ttu-id="bcd94-110">Obnovení dat tooan alternativní počítače.</span><span class="sxs-lookup"><span data-stu-id="bcd94-110">Restore data tooan alternate machine.</span></span>

<span data-ttu-id="bcd94-111">V lednu 2017 společnost Microsoft vydala agenta MARS toohello aktualizace verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="bcd94-111">In January 2017, Microsoft released a Preview update toohello MARS agent.</span></span> <span data-ttu-id="bcd94-112">Společně s oprav chyb, tato aktualizace umožňuje rychlé obnovení, což vám umožní toomount obnovení zapisovatelného snímku bodu jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-112">Along with bug fixes, this update enables Instant Restore, which allows you toomount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="bcd94-113">Pak můžete zkoumat hello obnovení svazku a zkopírujte soubory tooa místního počítače a následné obnovení souborů.</span><span class="sxs-lookup"><span data-stu-id="bcd94-113">You can then explore hello recovery volume and copy files tooa local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="bcd94-114">Hello [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) je povinný, pokud chcete, aby toouse rychlé obnovení dat toorestore.</span><span class="sxs-lookup"><span data-stu-id="bcd94-114">hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want toouse Instant Restore toorestore data.</span></span> <span data-ttu-id="bcd94-115">Hello zálohovaná data musí být chráněna v trezorů v uvedené v článku podpory hello národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="bcd94-115">Also hello backup data must be protected in vaults in locales listed in hello support article.</span></span> <span data-ttu-id="bcd94-116">Poraďte se hello [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) hello nejnovější seznam národních prostředí, které podporují rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-116">Consult hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for hello latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="bcd94-117">Rychlé obnovení je **není** aktuálně k dispozici ve všech národních prostředí.</span><span class="sxs-lookup"><span data-stu-id="bcd94-117">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="bcd94-118">Rychlé obnovení je k dispozici pro použití v trezory služeb zotavení v hello portál Azure a trezory Backup portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="bcd94-118">Instant Restore is available for use in Recovery Services vaults in hello Azure portal and Backup vaults in hello classic portal.</span></span> <span data-ttu-id="bcd94-119">Pokud chcete toouse rychlé obnovení, stažení aktualizace MARS hello a postupujte podle hello postupy, které zmínili, rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-119">If you want toouse Instant Restore, download hello MARS update, and follow hello procedures that mention Instant Restore.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a><span data-ttu-id="bcd94-120">Rychlé obnovení dat toohello toorecover použít stejný počítač</span><span class="sxs-lookup"><span data-stu-id="bcd94-120">Use Instant Restore toorecover data toohello same machine</span></span>

<span data-ttu-id="bcd94-121">Pokud jste omylem odstranili toorestore souboru a chcete ho toohello stejného počítače (z které hello je odebrán zálohování), hello následující kroky vám pomůže obnovit hello data.</span><span class="sxs-lookup"><span data-stu-id="bcd94-121">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="bcd94-122">Otevřete hello **Microsoft Azure Backup** přichycení v.</span><span class="sxs-lookup"><span data-stu-id="bcd94-122">Open hello **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="bcd94-123">Pokud si nejste jisti, kam se nainstaloval hello modul snap-in, vyhledávat hello počítači nebo serveru pro **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-123">If you don't know where hello snap in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="bcd94-124">aplikace na ploše Hello by se zobrazit ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="bcd94-124">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="bcd94-125">Klikněte na tlačítko **obnovit Data** toostart hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="bcd94-125">Click **Recover Data** toostart hello wizard.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="bcd94-127">Na hello **Začínáme** podokně, toorestore hello data toohello stejný server nebo počítač, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-127">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Vyberte tento server možnost toorestore hello data toohello stejný počítač](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="bcd94-129">Na hello **vyberte režimu obnovení** podokně vyberte **jednotlivých souborů a složek** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-129">On hello **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Procházet soubory](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="bcd94-131">Na hello **vyberte svazek a datum** podokně, vyberte hello svazku, který obsahuje hello soubory nebo složky, které chcete toorestore.</span><span class="sxs-lookup"><span data-stu-id="bcd94-131">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="bcd94-132">Na hello kalendáři vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-132">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="bcd94-133">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="bcd94-133">You can restore from any recovery point in time.</span></span> <span data-ttu-id="bcd94-134">Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-134">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="bcd94-135">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="bcd94-135">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Svazek a datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="bcd94-137">Jakmile jste vybrali toorestore bodu obnovení hello, klikněte na možnost **připojit**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-137">Once you have chosen hello recovery point toorestore, click **Mount**.</span></span>

    <span data-ttu-id="bcd94-138">Zálohování Azure připojí bodu místní obnovení hello a používá je jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-138">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="bcd94-139">Na hello **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** tooopen Průzkumníka Windows a najít hello soubory a složky chcete.</span><span class="sxs-lookup"><span data-stu-id="bcd94-139">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="bcd94-141">V Průzkumníku Windows, kopie hello soubory nebo složky toorestore a vložit je tooany umístění místní toohello serveru nebo počítači.</span><span class="sxs-lookup"><span data-stu-id="bcd94-141">In Windows Explorer, copy hello files and/or folders you want toorestore and paste them tooany location local toohello server or computer.</span></span> <span data-ttu-id="bcd94-142">Můžete otevřít nebo stream hello soubory přímo z hello obnovení svazku a ověření hello správné verze se obnoví.</span><span class="sxs-lookup"><span data-stu-id="bcd94-142">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Kopírovat a vkládat soubory a složky z připojeného svazku toolocal umístění](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="bcd94-144">Po dokončení obnovení hello soubory nebo složky na hello **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-144">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="bcd94-145">Pak klikněte na tlačítko **Ano** tooconfirm, které chcete toounmount hello svazku.</span><span class="sxs-lookup"><span data-stu-id="bcd94-145">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Odpojení hello svazku a potvrďte](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="bcd94-147">Pokud neklikejte na odpojení, zůstane hello obnovení svazku připojené 6 hodin od času hello, pokud byla připojena.</span><span class="sxs-lookup"><span data-stu-id="bcd94-147">If you do not click Unmount, hello Recovery Volume will remain mounted for 6 hours from hello time when it was mounted.</span></span> <span data-ttu-id="bcd94-148">Čas připojení hello je však rozšířené maximálně 24 hodin, v případě probíhající kopírování souborů.</span><span class="sxs-lookup"><span data-stu-id="bcd94-148">However, hello mount time is extended upto a maximum of 24 hours in case of an ongoing file-copy.</span></span> <span data-ttu-id="bcd94-149">Žádná operace zálohování se spustí hello svazek je připojen.</span><span class="sxs-lookup"><span data-stu-id="bcd94-149">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="bcd94-150">Všechny naplánované operace zálohování toorun během hello doby, kdy je připojen hello svazek, se spustí po obnovení svazku hello odpojené.</span><span class="sxs-lookup"><span data-stu-id="bcd94-150">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a><span data-ttu-id="bcd94-151">Použít rychlé obnovení toorestore data tooan alternativní počítač</span><span class="sxs-lookup"><span data-stu-id="bcd94-151">Use Instant Restore toorestore data tooan alternate machine</span></span>
<span data-ttu-id="bcd94-152">Pokud dojde ke ztrátě celý server, stále můžete obnovit data z Azure Backup tooa jiný počítač.</span><span class="sxs-lookup"><span data-stu-id="bcd94-152">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="bcd94-153">Následující kroky Hello znázorňují hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bcd94-153">hello following steps illustrate hello workflow.</span></span>


<span data-ttu-id="bcd94-154">zahrnuje Hello terminologie použitá v těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="bcd94-154">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="bcd94-155">*Zdrojový počítač* – pořízení hello původní počítač, ze které hello zálohy a který není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="bcd94-155">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="bcd94-156">*Cílový počítač* – hello počítač toowhich hello data obnovena.</span><span class="sxs-lookup"><span data-stu-id="bcd94-156">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="bcd94-157">*Ukázka trezoru* – hello toowhich trezoru služeb zotavení hello *zdrojový počítač* a *cílový počítač* jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="bcd94-157">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="bcd94-158">Zálohování nemůže být starší verzí operačního systému hello obnovené tooa cílový počítač.</span><span class="sxs-lookup"><span data-stu-id="bcd94-158">Backups can't be restored tooa target machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="bcd94-159">Například převzat ze systému Windows 7, počítač může být obnovena záloha v systému Windows 8 nebo novější, počítače.</span><span class="sxs-lookup"><span data-stu-id="bcd94-159">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="bcd94-160">Zálohy z počítače se systémem Windows 8 nemůže být počítač obnovený tooa Windows 7.</span><span class="sxs-lookup"><span data-stu-id="bcd94-160">A backup taken from a Windows 8 computer cannot be restored tooa Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="bcd94-161">Otevřete hello **Microsoft Azure Backup** přichycení v na hello *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="bcd94-161">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>

2. <span data-ttu-id="bcd94-162">Ujistěte se, hello *cílový počítač* a hello *zdrojový počítač* jsou registrované toohello trezoru stejné služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-162">Ensure hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>

3. <span data-ttu-id="bcd94-163">Klikněte na tlačítko **obnovit Data** tooopen hello **Průvodce obnovení dat**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-163">Click **Recover Data** tooopen hello **Recover Data wizard**.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="bcd94-165">Na hello **Začínáme** podokně, vyberte **jiný server**</span><span class="sxs-lookup"><span data-stu-id="bcd94-165">On hello **Getting Started** pane, select **Another server**</span></span>

    ![Jiný Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="bcd94-167">Zadejte soubor s přihlašovacími údaji trezoru hello odpovídající toohello *ukázka trezoru*a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-167">Provide hello vault credential file that corresponds toohello *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="bcd94-168">Pokud soubor s přihlašovacími údaji trezoru hello je neplatný (nebo vypršela platnost), stáhněte si nový soubor s přihlašovacími údaji trezoru z hello *ukázka trezoru* v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bcd94-168">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="bcd94-169">Po zadání přihlašovacích údajů platné úložiště, zobrazí se název hello hello odpovídající úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="bcd94-169">Once you provide a valid vault credential, hello name of hello corresponding Backup Vault appears.</span></span>


6. <span data-ttu-id="bcd94-170">Na hello **vyberte zálohování serveru** podokně, vyberte hello *zdrojový počítač* hello seznamu zobrazených počítačů a zadejte přístupové heslo hello.</span><span class="sxs-lookup"><span data-stu-id="bcd94-170">On hello **Select Backup Server** pane, select hello *Source machine* from hello list of displayed machines and provide hello passphrase.</span></span> <span data-ttu-id="bcd94-171">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-171">Then click **Next**.</span></span>

    ![Seznam počítačů](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="bcd94-173">Na hello **vyberte režimu obnovení** podokně, vyberte **jednotlivých souborů a složek** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-173">On hello **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Search](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="bcd94-175">Na hello **vyberte svazek a datum** podokně, vyberte hello svazku, který obsahuje hello soubory nebo složky, které chcete toorestore.</span><span class="sxs-lookup"><span data-stu-id="bcd94-175">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="bcd94-176">Na hello kalendáři vyberte bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-176">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="bcd94-177">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="bcd94-177">You can restore from any recovery point in time.</span></span> <span data-ttu-id="bcd94-178">Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-178">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="bcd94-179">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="bcd94-179">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Hledání položek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="bcd94-181">Klikněte na tlačítko **připojit** toolocally přípojného hello obnovení bodu jako svazek obnovení na vaše *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="bcd94-181">Click **Mount** toolocally mount hello recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="bcd94-182">Na hello **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** tooopen Průzkumníka Windows a najít hello soubory a složky chcete.</span><span class="sxs-lookup"><span data-stu-id="bcd94-182">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="bcd94-184">V Průzkumníku Windows, zkopírujte hello soubory nebo složky ze svazku obnovení hello a vložte je tooyour *cílový počítač* umístění.</span><span class="sxs-lookup"><span data-stu-id="bcd94-184">In Windows Explorer, copy hello files and/or folders from hello recovery volume and paste them tooyour *Target machine* location.</span></span> <span data-ttu-id="bcd94-185">Můžete otevřít nebo stream hello soubory přímo z hello obnovení svazku a ověření hello správné verze se obnoví.</span><span class="sxs-lookup"><span data-stu-id="bcd94-185">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="bcd94-187">Po dokončení obnovení hello soubory nebo složky na hello **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-187">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="bcd94-188">Pak klikněte na tlačítko **Ano** tooconfirm, které chcete toounmount hello svazku.</span><span class="sxs-lookup"><span data-stu-id="bcd94-188">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="bcd94-190">Pokud neklikejte na odpojení, zůstane hello obnovení svazku připojené 6 hodin od času hello, pokud byla připojena.</span><span class="sxs-lookup"><span data-stu-id="bcd94-190">If you do not click Unmount, hello Recovery Volume will remain mounted for 6 hours from hello time when it was mounted.</span></span> <span data-ttu-id="bcd94-191">Čas připojení hello je však rozšířené maximálně 24 hodin, v případě probíhající kopírování souborů.</span><span class="sxs-lookup"><span data-stu-id="bcd94-191">However, hello mount time is extended upto a maximum of 24 hours in case of an ongoing file-copy.</span></span> <span data-ttu-id="bcd94-192">Žádná operace zálohování se spustí hello svazek je připojen.</span><span class="sxs-lookup"><span data-stu-id="bcd94-192">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="bcd94-193">Všechny naplánované operace zálohování toorun během hello doby, kdy je připojen hello svazek, se spustí po obnovení svazku hello odpojené.</span><span class="sxs-lookup"><span data-stu-id="bcd94-193">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >

## <a name="troubleshooting"></a><span data-ttu-id="bcd94-194">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="bcd94-194">Troubleshooting</span></span>
<span data-ttu-id="bcd94-195">Pokud nelze Azure Backup úspěšně připojit svazek hello obnovení ani po několika minutách kliknutí na možnost **připojit** nebo selže toomount hello obnovení svazku s jedné nebo více chybám, postupujte podle hello kroků toobegin obnovení normálně.</span><span class="sxs-lookup"><span data-stu-id="bcd94-195">If Azure Backup does not successfully mount hello recovery volume even after several minutes of clicking **Mount** or fails toomount hello recovery volume with one or more errors, follow hello steps below toobegin recovering normally.</span></span>

1.  <span data-ttu-id="bcd94-196">Proces probíhající připojení hello zrušte, v případě, že po spuštění pro několik minut.</span><span class="sxs-lookup"><span data-stu-id="bcd94-196">Cancel hello ongoing mount process in case it has been running for several minutes.</span></span>

2.  <span data-ttu-id="bcd94-197">Ujistěte se, že jste na nejnovější verzi agenta Azure Backup hello hello.</span><span class="sxs-lookup"><span data-stu-id="bcd94-197">Ensure that you are on hello latest version of hello Azure Backup agent.</span></span> <span data-ttu-id="bcd94-198">toofind informace hello verze agenta Azure Backup, klikněte na **o Microsoft agenta služeb zotavení Azure** na hello **akce** podokně služby Microsoft Azure Backup konzoly a ujistěte se, že hello  **Verze** číslo je rovna tooor vyšší než verze hello uvedený v [v tomto článku](https://go.microsoft.com/fwlink/?linkid=229525).</span><span class="sxs-lookup"><span data-stu-id="bcd94-198">toofind out hello version information of Azure Backup agent, click on **About Microsoft Azure Recovery Services Agent** on hello **Actions** pane of Microsoft Azure Backup console and ensure that hello **Version** number is equal tooor higher than hello version mentioned in [this article](https://go.microsoft.com/fwlink/?linkid=229525).</span></span> <span data-ttu-id="bcd94-199">Můžete stáhnout nejnovější verzi hello [sem](https://go.microsoft.com/fwLink/?LinkID=288905)</span><span class="sxs-lookup"><span data-stu-id="bcd94-199">You can download hello latest version from [here](https://go.microsoft.com/fwLink/?LinkID=288905)</span></span>

3.  <span data-ttu-id="bcd94-200">Přejděte příliš**Správce zařízení** -> **řadiče úložiště** a ujistěte se, že je možné najít **iniciátor iSCSI společnosti Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-200">Go too**Device Manager** -> **Storage Controllers** and ensure that you can locate **Microsoft iSCSI Initiator**.</span></span> <span data-ttu-id="bcd94-201">Pokud abyste ji mohli najít, přejděte přímo toostep 7 níže.</span><span class="sxs-lookup"><span data-stu-id="bcd94-201">If you can locate it, directly go toostep 7 below.</span></span> 

4.  <span data-ttu-id="bcd94-202">Pokud služba iniciátoru iSCSI společnosti Microsoft nelze najít, jak je uvedeno v kroku 3, zkontrolujte toosee, zda můžete najít položku v části **Správce zařízení** -> **řadiče úložiště** názvem  **Neznámé zařízení** s ID hardwaru **ROOT\ISCSIPRT**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-202">If you cannot locate Microsoft iSCSI Initiator service as mentioned in step 3, check toosee if you can find an entry under **Device Manager** -> **Storage Controllers** called **Unknown Device** with Hardware ID **ROOT\ISCSIPRT**.</span></span>

5.  <span data-ttu-id="bcd94-203">Klikněte pravým tlačítkem na **neznámé zařízení** a vyberte **aktualizace ovladače**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-203">Right click on **Unknown Device** and select **Update Driver Software**.</span></span>

6.  <span data-ttu-id="bcd94-204">Aktualizovat ovladač hello výběrem možnosti hello příliš **vyhledání automaticky aktualizovaný ovladač softwaru**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-204">Update hello driver by selecting hello option too **Search automatically for updated driver software**.</span></span> <span data-ttu-id="bcd94-205">Dokončení hello aktualizace by se měl změnit **neznámé zařízení** příliš**iniciátor iSCSI společnosti Microsoft** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="bcd94-205">Completion of hello update should change **Unknown Device** too**Microsoft iSCSI Initiator** as shown below.</span></span> 

    ![Šifrování](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  <span data-ttu-id="bcd94-207">Přejděte příliš**Správce úloh** -> **služby (místní počítač)** -> **služba iniciátoru iSCSI společnosti Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-207">Go too**Task Manager** -> **Services (Local)** -> **Microsoft iSCSI Initiator Service**.</span></span> 

    ![Šifrování](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  <span data-ttu-id="bcd94-209">Restartujte službu iniciátoru iSCSI společnosti Microsoft hello kliknutím pravým tlačítkem na službu hello, kliknutím na **Zastavit** a další kliknutím pravým tlačítkem na znovu a kliknete na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="bcd94-209">Restart hello Microsoft iSCSI Initiator service by right-clicking on hello service, clicking on **Stop** and further right clicking again and clicking on **Start**.</span></span>

9.  <span data-ttu-id="bcd94-210">Opakujte obnovení pomocí rychlé obnovení.</span><span class="sxs-lookup"><span data-stu-id="bcd94-210">Retry recovering using Instant Restore.</span></span> 

<span data-ttu-id="bcd94-211">Pokud se obnovení hello stále nezdaří, restartujte server nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="bcd94-211">If hello recovery still fails, reboot your server/client.</span></span> <span data-ttu-id="bcd94-212">Pokud restartování není žádoucí nebo hello obnovení stále nedaří i po restartování serveru hello, pokuste obnovení provést z alternativní počítače a obraťte se na podporu Azure tak, že přejdete příliš[portálu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a odeslání žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="bcd94-212">If a reboot is not desirable or hello recovery still fails even after rebooting hello server, try recovering from an Alternate Machine, and contact Azure Support by going too[Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and submitting a support request.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bcd94-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bcd94-213">Next steps</span></span>
* <span data-ttu-id="bcd94-214">Teď, když jste obnovit soubory a složky, můžete [spravovat zálohy](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="bcd94-214">Now that you've recovered your files and folders, you can [manage your backups](backup-azure-manage-windows-server.md).</span></span>
