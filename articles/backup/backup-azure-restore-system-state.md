---
title: "Zálohování Azure: Tooa obnovení stavu systému Windows Server | Microsoft Docs"
description: "Krok podle kroku vysvětlení pro obnovení stavu systému Windows Server ze zálohy v Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/18/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: a45506507f53e2744350d3b6b2e52f1db415de4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-system-state-toowindows-server"></a><span data-ttu-id="e1080-103">Obnovit stav systému tooWindows serveru</span><span class="sxs-lookup"><span data-stu-id="e1080-103">Restore System State tooWindows Server</span></span>

<span data-ttu-id="e1080-104">Tento článek vysvětluje, jak toorestore zálohy stavu systému Windows Server ze služeb zotavení Azure trezoru.</span><span class="sxs-lookup"><span data-stu-id="e1080-104">This article explains how toorestore Windows Server System State backups from an Azure Recovery Services vault.</span></span> <span data-ttu-id="e1080-105">toorestore stav systému, musíte mít zálohu stavu systému (vytvořili pomocí pokynů hello v [zálohování stavu systému](backup-azure-system-state.md#back-up-windows-server-system-state-preview)) a ujistěte se, že máte nainstalovanou hello [nejnovější verzi Microsoft Azure Recovery hello Agent služeb (MARS)](http://aka.ms/azurebackup_agent).</span><span class="sxs-lookup"><span data-stu-id="e1080-105">toorestore System State, you must have a System State backup (created using hello instructions in [Back up System State](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), and make sure you have installed hello [latest version of hello Microsoft Azure Recovery Services (MARS) agent](http://aka.ms/azurebackup_agent).</span></span> <span data-ttu-id="e1080-106">Obnovení dat stavu systému Windows Server z trezoru služby Azure Recovery Services je dvoustupňový proces:</span><span class="sxs-lookup"><span data-stu-id="e1080-106">Recovering Windows Server System State data from an Azure Recovery Services vault is a two-step process:</span></span>

1. <span data-ttu-id="e1080-107">Obnovení stavu systému souborů z Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="e1080-107">Restore System State as files from Azure Backup.</span></span> <span data-ttu-id="e1080-108">Při obnovení stavu systému jako soubory ze zálohy Azure, můžete se buď:</span><span class="sxs-lookup"><span data-stu-id="e1080-108">When restoring System State as files from Azure Backup, you can either:</span></span>
  * <span data-ttu-id="e1080-109">Obnovit stav systému toohello stejný server, kde byly provedeny hello zálohy, nebo</span><span class="sxs-lookup"><span data-stu-id="e1080-109">Restore System State toohello same server where hello backups were taken, or</span></span>
  * <span data-ttu-id="e1080-110">Obnovit stav systému souborů tooan alternativní server.</span><span class="sxs-lookup"><span data-stu-id="e1080-110">Restore System State file tooan alternate server.</span></span>

2. <span data-ttu-id="e1080-111">Použijte hello obnovit stav systému souborů tooa systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="e1080-111">Apply hello restored System State files tooa Windows Server.</span></span>


## <a name="recover-system-state-files-toohello-same-server"></a><span data-ttu-id="e1080-112">Obnovení stavu systému souborů toohello stejný server</span><span class="sxs-lookup"><span data-stu-id="e1080-112">Recover System State files toohello same server</span></span>
<span data-ttu-id="e1080-113">Hello následující kroky popisují, jak tooroll zpět předchozí stav vašeho systému Windows Server konfigurace tooa.</span><span class="sxs-lookup"><span data-stu-id="e1080-113">hello following steps explain how tooroll back your Windows Server configuration tooa previous state.</span></span> <span data-ttu-id="e1080-114">Vrácení zpět tooa konfigurace označuje, stabilního stavu váš server může být velmi důležité.</span><span class="sxs-lookup"><span data-stu-id="e1080-114">Rolling your server configuration back tooa known, stable state, can be extremely valuable.</span></span> <span data-ttu-id="e1080-115">Hello následujících kroků obnovení hello stavu systému serveru z trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="e1080-115">hello following steps restore hello server's System State from a Recovery Services vault.</span></span> 

1. <span data-ttu-id="e1080-116">Otevřete hello **Microsoft Azure Backup** modul snap-in.</span><span class="sxs-lookup"><span data-stu-id="e1080-116">Open hello **Microsoft Azure Backup** snap-in.</span></span> <span data-ttu-id="e1080-117">Pokud si nejste jisti, kde nainstalovaný modul snap-in hello, vyhledávat hello počítači nebo serveru pro **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="e1080-117">If you don't know where hello snap-in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="e1080-118">aplikace na ploše Hello by se zobrazit ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-118">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="e1080-119">Klikněte na tlačítko **obnovit Data** toostart hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="e1080-119">Click **Recover Data** toostart hello wizard.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="e1080-121">Na hello **Začínáme** podokně, toorestore hello data toohello stejný server nebo počítač, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-121">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Vyberte tento server možnost toorestore hello data toohello stejný počítač](./media/backup-azure-restore-system-state/samemachine.png)

4. <span data-ttu-id="e1080-123">Na hello **vyberte režimu obnovení** podokně vyberte **stav systému** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-123">On hello **Select Recovery Mode** pane, choose **System State** and then click **Next**.</span></span>

    ![Procházet soubory](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. <span data-ttu-id="e1080-125">V kalendáři hello v **vyberte svazek a datum** podokně, vyberte obnovení bodu.</span><span class="sxs-lookup"><span data-stu-id="e1080-125">On hello calendar in **Select Volume and Date** pane, select a recovery point.</span></span> 

    <span data-ttu-id="e1080-126">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="e1080-126">You can restore from any recovery point in time.</span></span> <span data-ttu-id="e1080-127">Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="e1080-127">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="e1080-128">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="e1080-128">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Svazek a datum](./media/backup-azure-restore-system-state/select-date.png)

6. <span data-ttu-id="e1080-130">Jakmile jste vybrali toorestore bodu obnovení hello, klikněte na možnost **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-130">Once you have chosen hello recovery point toorestore, click **Next**.</span></span>

    <span data-ttu-id="e1080-131">Zálohování Azure připojí bodu místní obnovení hello a používá je jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="e1080-131">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="e1080-132">V podokně Další hello zadejte hello cíl pro hello obnovit stav systému souborů a klikněte na tlačítko **Procházet** tooopen Průzkumníka Windows a najít hello soubory a složky chcete.</span><span class="sxs-lookup"><span data-stu-id="e1080-132">On hello next pane, specify hello destination for hello recovered System State files and click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span> <span data-ttu-id="e1080-133">Hello možnost **vytvořit kopie tak, aby obě verze**, vytváří kopie jednotlivé soubory do stávajícího archivu soubor stavu systému místo vytvoření kopie hello hello celý archivu stavu systému.</span><span class="sxs-lookup"><span data-stu-id="e1080-133">hello option, **Create copies so that you have both versions**, creates copies of individual files in an existing System State file archive instead of creating hello copy of hello entire System State archive.</span></span>

    ![Možnosti obnovení](./media/backup-azure-restore-system-state/recover-as-files.png)

8. <span data-ttu-id="e1080-135">Ověřte podrobnosti o hello obnovení na hello **potvrzení** panelu a klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="e1080-135">Verify hello details of recovery on hello **Confirmation** pane and click **Recover**.</span></span>

   ![Klikněte na tlačítko Obnovit tooacknowledge hello obnovit akce](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. <span data-ttu-id="e1080-137">Kopírování hello *WindowsImageBackup* adresář v hello nekritické svazek tooa obnovení cílového serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-137">Copy hello *WindowsImageBackup* directory in hello Recovery destination tooa non-critical volume of hello server.</span></span> <span data-ttu-id="e1080-138">Hello svazku operačního systému Windows je obvykle hello nepostradatelný svazek.</span><span class="sxs-lookup"><span data-stu-id="e1080-138">Usually, hello Windows OS volume is hello critical volume.</span></span>

10. <span data-ttu-id="e1080-139">Po úspěšné obnovení hello postupujte podle kroků hello v části hello [použít obnovit stav systému souborů toohello systému Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello proces obnovení stavu systému.</span><span class="sxs-lookup"><span data-stu-id="e1080-139">Once hello recovery is successful, follow hello steps in hello section, [Apply restored System State files toohello Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello System State recovery process.</span></span>

## <a name="recover-system-state-files-tooan-alternate-server"></a><span data-ttu-id="e1080-140">Obnovení stavu systému souborů tooan alternativní server</span><span class="sxs-lookup"><span data-stu-id="e1080-140">Recover System State files tooan alternate server</span></span>

<span data-ttu-id="e1080-141">Pokud váš Server systému Windows je poškozená nebo je nepřístupný, a chcete toorestore ho hello tooa stabilního stavu tím, že obnovení stavu systému Windows Server, můžete obnovit stav systému hello poškozená serveru z jiného serveru.</span><span class="sxs-lookup"><span data-stu-id="e1080-141">If your Windows Server is corrupted or inaccessible, and you want toorestore it tooa stable state by recovering hello Windows Server System State, you can restore hello corrupted server's System State from another server.</span></span> <span data-ttu-id="e1080-142">Pomocí následujících kroků toohello obnovení stavu systému na samostatný server hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-142">Use hello following steps toohello restore System State on a separate server.</span></span>  

<span data-ttu-id="e1080-143">zahrnuje Hello terminologie použitá v těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="e1080-143">hello terminology used in these steps includes:</span></span>

- <span data-ttu-id="e1080-144">*Zdrojový počítač* – pořízení hello původní počítač, ze které hello zálohy a který není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e1080-144">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
- <span data-ttu-id="e1080-145">*Cílový počítač* – hello počítač toowhich hello data obnovena.</span><span class="sxs-lookup"><span data-stu-id="e1080-145">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
- <span data-ttu-id="e1080-146">*Ukázka trezoru* – hello toowhich trezoru služeb zotavení hello *zdrojový počítač* a *cílový počítač* jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="e1080-146">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="e1080-147">Zálohy vytvořené z jednoho počítače nelze počítač obnovený tooa starší verzí operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-147">Backups taken from one machine cannot be restored tooa machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="e1080-148">Zálohy vytvořené ze systému Windows Server 2016 počítač nemůže být třeba obnovit tooWindows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="e1080-148">For example, backups taken from a Windows Server 2016 machine can't be restored tooWindows Server 2012 R2.</span></span> <span data-ttu-id="e1080-149">Inverzní hello je ale možné.</span><span class="sxs-lookup"><span data-stu-id="e1080-149">However, hello inverse is possible.</span></span> <span data-ttu-id="e1080-150">Můžete použít zálohování z Windows serveru 2012 R2 toorestore systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="e1080-150">You can use backups from Windows Server 2012 R2 toorestore Windows Server 2016.</span></span>
>

1. <span data-ttu-id="e1080-151">Otevřete hello **Microsoft Azure Backup** modul snap-in na hello *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="e1080-151">Open hello **Microsoft Azure Backup** snap-in on hello *Target machine*.</span></span>
2. <span data-ttu-id="e1080-152">Ujistěte se, že hello *cílový počítač* a hello *zdrojový počítač* jsou registrované toohello trezoru stejné služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="e1080-152">Ensure that hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>
3. <span data-ttu-id="e1080-153">Klikněte na tlačítko **obnovit Data** tooinitiate hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="e1080-153">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)

4. <span data-ttu-id="e1080-155">Vyberte **jiný server**</span><span class="sxs-lookup"><span data-stu-id="e1080-155">Select **Another server**</span></span>

    ![Jiný Server](./media/backup-azure-restore-system-state/anotherserver.png)

5. <span data-ttu-id="e1080-157">Zadejte soubor s přihlašovacími údaji trezoru hello odpovídající toohello *ukázka trezoru*.</span><span class="sxs-lookup"><span data-stu-id="e1080-157">Provide hello vault credential file that corresponds toohello *Sample vault*.</span></span> <span data-ttu-id="e1080-158">Pokud soubor s přihlašovacími údaji trezoru hello je neplatný (nebo vypršela platnost), stáhněte si nový soubor s přihlašovacími údaji trezoru z hello *ukázka trezoru* v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e1080-158">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="e1080-159">Jakmile je zadaný soubor s přihlašovacími údaji trezoru hello, zobrazí se trezor služeb zotavení hello přidružený soubor s přihlašovacími údaji trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-159">Once hello vault credential file is provided, hello Recovery Services vault associated with hello vault credential file appears.</span></span>

6. <span data-ttu-id="e1080-160">V podokně vyberte Server Backup hello vyberte hello *zdrojový počítač* hello seznamu zobrazených počítačů.</span><span class="sxs-lookup"><span data-stu-id="e1080-160">On hello Select Backup Server pane, select hello *Source machine* from hello list of displayed machines.</span></span>

    ![Seznam počítačů](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. <span data-ttu-id="e1080-162">V podokně hello vyberte režimu obnovení, vyberte **stav systému** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-162">On hello Select Recovery Mode pane, choose **System State** and click **Next**.</span></span> 

    ![Search](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. <span data-ttu-id="e1080-164">Na hello kalendáře v hello **vyberte svazek a datum** podokně, vyberte obnovení bodu.</span><span class="sxs-lookup"><span data-stu-id="e1080-164">On hello Calendar in hello **Select Volume and Date** pane, select a recovery point.</span></span> <span data-ttu-id="e1080-165">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="e1080-165">You can restore from any recovery point in time.</span></span> <span data-ttu-id="e1080-166">Data v **tučné** znamenat hello dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="e1080-166">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="e1080-167">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte hello konkrétní bod obnovení z hello **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="e1080-167">Once  you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span> 

    ![Hledání položek](./media/backup-azure-restore-system-state/select-date.png)

9. <span data-ttu-id="e1080-169">Jakmile jste vybrali toorestore bodu obnovení hello, klikněte na možnost **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-169">Once you have chosen hello recovery point toorestore, click **Next**.</span></span>

10. <span data-ttu-id="e1080-170">Na hello **vyberte režim obnovení stavu systému** podokně zadejte cíl hello místo, kam chcete stav systému souborů toobe obnovit a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-170">On hello **Select System State Recovery Mode** pane, specify hello destination where you want System State files toobe recovered, then click **Next**.</span></span>

    ![Šifrování](./media/backup-azure-restore-system-state/recover-as-files.png)

    <span data-ttu-id="e1080-172">Hello možnost **vytvořit kopie tak, aby obě verze**, vytváří kopie jednotlivé soubory do stávajícího archivu soubor stavu systému místo vytvoření kopie hello hello celý archivu stavu systému.</span><span class="sxs-lookup"><span data-stu-id="e1080-172">hello option, **Create copies so that you have both versions**, creates copies of individual files in an existing System State file archive instead of creating hello copy of hello entire System State archive.</span></span>

11. <span data-ttu-id="e1080-173">Ověřte podrobnosti o hello obnovení v podokně hello potvrzení a klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="e1080-173">Verify hello details of recovery on hello Confirmation pane, and click **Recover**.</span></span> 

    ![Klikněte na tlačítko Obnovit hello, tooconfirm proces obnovení hello](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. <span data-ttu-id="e1080-175">Kopírování hello *WindowsImageBackup* directory tooa nekritické svazku hello serveru (například D:\).</span><span class="sxs-lookup"><span data-stu-id="e1080-175">Copy hello *WindowsImageBackup* directory tooa non-critical volume of hello server (for example D:\).</span></span> <span data-ttu-id="e1080-176">Obvykle je hello svazku operačního systému Windows hello nepostradatelný svazek.</span><span class="sxs-lookup"><span data-stu-id="e1080-176">Usually hello Windows OS volume is hello critical volume.</span></span>

13. <span data-ttu-id="e1080-177">proces obnovení hello toocomplete, použijte následující hello části příliš[použít hello obnovit stav systému souborů v systému Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).</span><span class="sxs-lookup"><span data-stu-id="e1080-177">toocomplete hello recovery process, use hello following section too[apply hello restored System State files on a Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).</span></span>




## <a name="apply-restored-system-state-on-a-windows-server"></a><span data-ttu-id="e1080-178">Použít obnovený stav systému v systému Windows Server</span><span class="sxs-lookup"><span data-stu-id="e1080-178">Apply restored System State on a Windows Server</span></span>

<span data-ttu-id="e1080-179">Jakmile jste obnovili stavu systému jako soubory pomocí agenta služeb zotavení Azure, použijte hello zálohování serveru nástroj tooapply hello obnovit tooWindows stav systému serveru.</span><span class="sxs-lookup"><span data-stu-id="e1080-179">Once you have recovered System State as files using Azure Recovery Services Agent, use hello Windows Server Backup utility tooapply hello recovered System State tooWindows Server.</span></span> <span data-ttu-id="e1080-180">Hello nástroj zálohování systému Windows Server je již k dispozici na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-180">hello Windows Server Backup utility is already available on hello server.</span></span> <span data-ttu-id="e1080-181">Hello následující kroky popisují, jak tooapply hello obnovit stav systému.</span><span class="sxs-lookup"><span data-stu-id="e1080-181">hello following steps explain how tooapply hello recovered System State.</span></span>

1. <span data-ttu-id="e1080-182">Použití hello následující příkazy tooreboot serveru v *režimu obnovení adresářových služeb*.</span><span class="sxs-lookup"><span data-stu-id="e1080-182">Use hello following commands tooreboot your server in *Directory Services Repair Mode*.</span></span> <span data-ttu-id="e1080-183">V řádku se zvýšenými oprávněními příkaz:</span><span class="sxs-lookup"><span data-stu-id="e1080-183">In an elevated command prompt:</span></span>

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. <span data-ttu-id="e1080-184">Po restartování hello otevřete modul snap-in Zálohování serveru Windows hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-184">After hello reboot, open hello Windows Server Backup snap-in.</span></span> <span data-ttu-id="e1080-185">Pokud si nejste jisti, kde nainstalovaný modul snap-in hello, vyhledávat hello počítači nebo serveru pro **zálohování serveru**.</span><span class="sxs-lookup"><span data-stu-id="e1080-185">If you don't know where hello snap-in was installed, search hello computer or server for **Windows Server Backup**.</span></span>

    <span data-ttu-id="e1080-186">aplikace na ploše Hello se zobrazí ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-186">hello desktop app appears in hello search results.</span></span>

3. <span data-ttu-id="e1080-187">V modulu snap-in hello, vyberte **místní záloha**.</span><span class="sxs-lookup"><span data-stu-id="e1080-187">In hello snap-in, select **Local Backup**.</span></span>

    ![Vyberte místní záloha toorestore odtud](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. <span data-ttu-id="e1080-189">V konzole hello místní záloha v hello **podokna akce**, klikněte na tlačítko **obnovit** tooopen hello Průvodce obnovením.</span><span class="sxs-lookup"><span data-stu-id="e1080-189">On hello Local Backup console, in hello **Actions Pane**, click **Recover** tooopen hello Recovery Wizard.</span></span>

5. <span data-ttu-id="e1080-190">Vyberte možnost hello **zálohy uložené v jiném umístění**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-190">Select hello option, **A backup stored in another location**, and click **Next**.</span></span>

   ![Vyberte jiný server toorecover tooa](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. <span data-ttu-id="e1080-192">Při zadávání typ umístění hello, vyberte **vzdálené sdílené složce** Pokud záloha stavu systému byla obnovená tooanother serveru.</span><span class="sxs-lookup"><span data-stu-id="e1080-192">When specifying hello location type, select **Remote shared folder** if your System State backup was recovered tooanother server.</span></span> <span data-ttu-id="e1080-193">Pokud stav systému byla obnovena místně, pak vyberte **místní jednotky**.</span><span class="sxs-lookup"><span data-stu-id="e1080-193">If your System State was recovered locally, then select **Local drives**.</span></span> 

    ![Vyberte zda toorecovery z místního serveru nebo jiné](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. <span data-ttu-id="e1080-195">Zadejte cestu toohello hello *WindowsImageBackup* adresář, nebo zvolte hello místní disk obsahující tento adresář (například D:\WindowsImageBackup), obnovit jako součást obnovení souborů hello stavu systému pomocí Azure Recovery Služba agenta a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-195">Enter hello path toohello *WindowsImageBackup* directory, or choose hello local drive containing this directory (for example, D:\WindowsImageBackup), recovered as part of hello System State files recovery using Azure Recovery Services Agent and click **Next**.</span></span>

    ![Cesta toohello sdílený soubor](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. <span data-ttu-id="e1080-197">Vyberte hello stav systému verze má toorestore a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-197">Select hello System State version that you want toorestore, and click **Next**.</span></span>

9. <span data-ttu-id="e1080-198">V podokně hello vybrat typ obnovení, vyberte **stav systému** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-198">In hello Select Recovery Type pane, select **System State** and click **Next**.</span></span>

10. <span data-ttu-id="e1080-199">Pro umístění hello hello obnovení stavu systému, vyberte **původního umístění**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="e1080-199">For hello location of hello System State Recovery, select **Original Location**, and click **Next**.</span></span>

11. <span data-ttu-id="e1080-200">Zkontrolujte podrobnosti o potvrzení hello, ověřte nastavení restartování hello a klikněte na tlačítko **obnovit** tooapplly hello obnovit stav systému souborů.</span><span class="sxs-lookup"><span data-stu-id="e1080-200">Review hello confirmation details, verify hello reboot settings, and click **Recover** tooapplly hello restored System State files.</span></span>

    ![hello spuštění obnovení stavu systému souborů](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a><span data-ttu-id="e1080-202">Zvláštní upozornění pro obnovení stavu systému na serveru služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1080-202">Special considerations for System State recovery on Active Directory server</span></span>

<span data-ttu-id="e1080-203">Zálohování stavu systému zahrnuje data služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e1080-203">System State backup includes Active Directory data.</span></span> <span data-ttu-id="e1080-204">Pomocí následujících kroků toorestore služba Active Directory Domain Services (AD DS) z jeho aktuální stav předchozí tooa stavu hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-204">Use hello following steps toorestore Active Directory Domain Service (AD DS) from its current state tooa previous state.</span></span>

1. <span data-ttu-id="e1080-205">Restartujte řadič domény hello v adresářových služeb obnovení režimu (DSRM).</span><span class="sxs-lookup"><span data-stu-id="e1080-205">Restart hello domain controller in Directory Services Restore Mode (DSRM).</span></span>
2. <span data-ttu-id="e1080-206">Postupujte podle kroků hello [sem](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse zálohování serveru rutiny toorecover služby AD DS.</span><span class="sxs-lookup"><span data-stu-id="e1080-206">Follow hello steps [here](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse Windows Server Backup cmdlets toorecover AD DS.</span></span>


## <a name="troubleshoot-failed-system-state-restore"></a><span data-ttu-id="e1080-207">Řešení potíží se nezdařilo obnovení stavu systému</span><span class="sxs-lookup"><span data-stu-id="e1080-207">Troubleshoot failed System State restore</span></span>

<span data-ttu-id="e1080-208">Pokud předchozí proces hello použití stavu systému úspěšně nedokončí, můžete použijte modul snap-in prostředí Windows Recovery Environment (Win RE) toorecover hello systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="e1080-208">If hello previous process of applying System State does not complete successfully, use hello Windows Recovery Environment (Win RE) toorecover your Windows Server.</span></span> <span data-ttu-id="e1080-209">Hello následující kroky popisují, jak pomocí prostředí Windows RE toorecover.</span><span class="sxs-lookup"><span data-stu-id="e1080-209">hello following steps explain how toorecover using Win RE.</span></span> <span data-ttu-id="e1080-210">Tuto možnost použijte pouze v případě systému Windows Server není normálnímu spuštění po obnovení stavu systému.</span><span class="sxs-lookup"><span data-stu-id="e1080-210">Use This option only if Windows Server does not boot normally after a System State restore.</span></span> <span data-ttu-id="e1080-211">Hello postupu vymaže data nesystémové, buďte opatrní.</span><span class="sxs-lookup"><span data-stu-id="e1080-211">hello following process erases non-system data, use caution.</span></span> 

1. <span data-ttu-id="e1080-212">Spouštění systému Windows Server do hello prostředí Windows Recovery Environment (Win RE).</span><span class="sxs-lookup"><span data-stu-id="e1080-212">Boot your Windows Server into hello Windows Recovery Environment (Win RE).</span></span>

2. <span data-ttu-id="e1080-213">Vyberte Poradce při potížích ze tří dostupných možností hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-213">Select Troubleshoot from hello three available options.</span></span>

    ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-1.png)

3. <span data-ttu-id="e1080-215">Z hello **pokročilé možnosti** obrazovku, vyberte **příkazového řádku** a zadejte uživatelské jméno správce serveru hello a heslo.</span><span class="sxs-lookup"><span data-stu-id="e1080-215">From hello **Advanced Options** screen, select **Command Prompt** and provide hello server administrator username and password.</span></span>

   ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-2.png)

4. <span data-ttu-id="e1080-217">Zadejte uživatelské jméno správce serveru hello a heslo.</span><span class="sxs-lookup"><span data-stu-id="e1080-217">Provide hello server administrator username and password.</span></span>

    ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-3.png)

5. <span data-ttu-id="e1080-219">Když otevřete hello příkazový řádek v režimu správce, spusťte následující příkaz tooget hello stav systému zálohování verze.</span><span class="sxs-lookup"><span data-stu-id="e1080-219">When you open hello command prompt in administrator mode, run following command tooget hello System State backup versions.</span></span>

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-4.png)

6. <span data-ttu-id="e1080-221">Spusťte následující příkaz tooget hello všechny svazky, které jsou k dispozici v hello zálohování.</span><span class="sxs-lookup"><span data-stu-id="e1080-221">Run hello following command tooget all volumes available in hello backup.</span></span>

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-5.png)

7. <span data-ttu-id="e1080-223">Hello následující příkaz obnoví všechny svazky, které jsou součástí hello zálohování stavu systému.</span><span class="sxs-lookup"><span data-stu-id="e1080-223">hello following command recovers all volumes that are part of hello System State Backup.</span></span> <span data-ttu-id="e1080-224">Všimněte si, že tento krok obnoví pouze hello důležitých svazků, které jsou součástí stavu systému hello.</span><span class="sxs-lookup"><span data-stu-id="e1080-224">Note that this step recovers only hello critical volumes that are part of hello System State.</span></span> <span data-ttu-id="e1080-225">Všechna nesystémové data budou vymazána.</span><span class="sxs-lookup"><span data-stu-id="e1080-225">All non-System data is erased.</span></span>

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a><span data-ttu-id="e1080-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1080-227">Next steps</span></span>
* <span data-ttu-id="e1080-228">Teď, když jste obnovit soubory a složky, můžete [spravovat zálohy](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="e1080-228">Now that you've recovered your files and folders, you can [manage your backups](backup-azure-manage-windows-server.md).</span></span>
