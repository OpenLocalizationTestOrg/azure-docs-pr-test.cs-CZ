---
title: "Zálohování Azure: Obnovení stavu systému na Windows Server | Microsoft Docs"
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
ms.openlocfilehash: 320c85f8045d9b72cf7f430d2e2736ba8e5ec269
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="restore-system-state-to-windows-server"></a><span data-ttu-id="0b05a-103">Obnovení stavu systému na Windows Server</span><span class="sxs-lookup"><span data-stu-id="0b05a-103">Restore System State to Windows Server</span></span>

<span data-ttu-id="0b05a-104">Tento článek vysvětluje způsob obnovení zálohy stavu systému Windows Server z trezoru služby Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="0b05a-104">This article explains how to restore Windows Server System State backups from an Azure Recovery Services vault.</span></span> <span data-ttu-id="0b05a-105">Chcete-li obnovit stav systému, musíte mít zálohu stavu systému (vytvořili pomocí pokynů v [zálohování stavu systému](backup-azure-system-state.md#back-up-windows-server-system-state-preview)) a ujistěte se, že máte nainstalovanou [nejnovější verzi systému Microsoft Azure Recovery Services (MARS) Agent](http://aka.ms/azurebackup_agent).</span><span class="sxs-lookup"><span data-stu-id="0b05a-105">To restore System State, you must have a System State backup (created using the instructions in [Back up System State](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), and make sure you have installed the [latest version of the Microsoft Azure Recovery Services (MARS) agent](http://aka.ms/azurebackup_agent).</span></span> <span data-ttu-id="0b05a-106">Obnovení dat stavu systému Windows Server z trezoru služby Azure Recovery Services je dvoustupňový proces:</span><span class="sxs-lookup"><span data-stu-id="0b05a-106">Recovering Windows Server System State data from an Azure Recovery Services vault is a two-step process:</span></span>

1. <span data-ttu-id="0b05a-107">Obnovení stavu systému souborů z Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="0b05a-107">Restore System State as files from Azure Backup.</span></span> <span data-ttu-id="0b05a-108">Při obnovení stavu systému jako soubory ze zálohy Azure, můžete se buď:</span><span class="sxs-lookup"><span data-stu-id="0b05a-108">When restoring System State as files from Azure Backup, you can either:</span></span>
  * <span data-ttu-id="0b05a-109">Obnovení stavu systému na stejný server kterém byly provedeny zálohy, nebo</span><span class="sxs-lookup"><span data-stu-id="0b05a-109">Restore System State to the same server where the backups were taken, or</span></span>
  * <span data-ttu-id="0b05a-110">Obnovit stav systému souborů na alternativní server.</span><span class="sxs-lookup"><span data-stu-id="0b05a-110">Restore System State file to an alternate server.</span></span>

2. <span data-ttu-id="0b05a-111">Použít obnovené soubory stavu systému pro Windows Server.</span><span class="sxs-lookup"><span data-stu-id="0b05a-111">Apply the restored System State files to a Windows Server.</span></span>


## <a name="recover-system-state-files-to-the-same-server"></a><span data-ttu-id="0b05a-112">Obnovení stavu systému souborů na stejný server</span><span class="sxs-lookup"><span data-stu-id="0b05a-112">Recover System State files to the same server</span></span>
<span data-ttu-id="0b05a-113">Následující kroky popisují, jak vrátit konfiguraci systému Windows Server do předchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="0b05a-113">The following steps explain how to roll back your Windows Server configuration to a previous state.</span></span> <span data-ttu-id="0b05a-114">Vracení konfigurace serveru známé, stabilního stavu, může být velmi důležité.</span><span class="sxs-lookup"><span data-stu-id="0b05a-114">Rolling your server configuration back to a known, stable state, can be extremely valuable.</span></span> <span data-ttu-id="0b05a-115">Následující postup obnovení stavu systému serveru z trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="0b05a-115">The following steps restore the server's System State from a Recovery Services vault.</span></span> 

1. <span data-ttu-id="0b05a-116">Otevřete **Microsoft Azure Backup** modul snap-in.</span><span class="sxs-lookup"><span data-stu-id="0b05a-116">Open the **Microsoft Azure Backup** snap-in.</span></span> <span data-ttu-id="0b05a-117">Pokud si nejste jisti, kde nainstalovaný modul snap-in, hledání se počítač nebo server pro **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-117">If you don't know where the snap-in was installed, search the computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="0b05a-118">Desktopová aplikace by se ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="0b05a-118">The desktop app should appear in the search results.</span></span>

2. <span data-ttu-id="0b05a-119">Klikněte na tlačítko **obnovit Data** spusťte průvodce.</span><span class="sxs-lookup"><span data-stu-id="0b05a-119">Click **Recover Data** to start the wizard.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="0b05a-121">Na **Začínáme** , chcete-li obnovit data na stejném serveru nebo počítače, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-121">On the **Getting Started** pane, to restore the data to the same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Vyberte tuto možnost serveru k obnovení dat na stejný počítač](./media/backup-azure-restore-system-state/samemachine.png)

4. <span data-ttu-id="0b05a-123">Na **vyberte režimu obnovení** podokně vyberte **stav systému** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-123">On the **Select Recovery Mode** pane, choose **System State** and then click **Next**.</span></span>

    ![Procházet soubory](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. <span data-ttu-id="0b05a-125">V kalendáři v **vyberte svazek a datum** podokně, vyberte obnovení bodu.</span><span class="sxs-lookup"><span data-stu-id="0b05a-125">On the calendar in **Select Volume and Date** pane, select a recovery point.</span></span> 

    <span data-ttu-id="0b05a-126">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="0b05a-126">You can restore from any recovery point in time.</span></span> <span data-ttu-id="0b05a-127">Data v **tučné** označuje dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="0b05a-127">Dates in **bold** indicate the availability of at least one recovery point.</span></span> <span data-ttu-id="0b05a-128">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte konkrétní bod obnovení z **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="0b05a-128">Once you select a date, if multiple recovery points are available, choose the specific recovery point from the **Time** drop-down menu.</span></span>

    ![Svazek a datum](./media/backup-azure-restore-system-state/select-date.png)

6. <span data-ttu-id="0b05a-130">Po zadání bodu obnovení pro obnovení, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-130">Once you have chosen the recovery point to restore, click **Next**.</span></span>

    <span data-ttu-id="0b05a-131">Zálohování Azure připojí bod místní obnovení a používá je jako svazek obnovení.</span><span class="sxs-lookup"><span data-stu-id="0b05a-131">Azure Backup mounts the local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="0b05a-132">V podokně Další zadejte cíl pro obnovené soubory stavu systému a klikněte na tlačítko **Procházet** otevřete Průzkumníka Windows a soubory a složky, které chcete najít.</span><span class="sxs-lookup"><span data-stu-id="0b05a-132">On the next pane, specify the destination for the recovered System State files and click **Browse** to open Windows Explorer and find the files and folders you want.</span></span> <span data-ttu-id="0b05a-133">Možnost **vytvořit kopie tak, aby obě verze**, vytváří kopie jednotlivé soubory do stávajícího archivu soubor stavu systému místo vytvoření kopie celý archivu stavu systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-133">The option, **Create copies so that you have both versions**, creates copies of individual files in an existing System State file archive instead of creating the copy of the entire System State archive.</span></span>

    ![Možnosti obnovení](./media/backup-azure-restore-system-state/recover-as-files.png)

8. <span data-ttu-id="0b05a-135">Zkontrolujte podrobnosti o obnovení na **potvrzení** panelu a klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-135">Verify the details of recovery on the **Confirmation** pane and click **Recover**.</span></span>

   ![Klikněte na tlačítko Obnovit na vědomí akce obnovení](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. <span data-ttu-id="0b05a-137">Kopírování *WindowsImageBackup* adresář v cílovém umístění obnovení na svazek nekritické serveru.</span><span class="sxs-lookup"><span data-stu-id="0b05a-137">Copy the *WindowsImageBackup* directory in the Recovery destination to a non-critical volume of the server.</span></span> <span data-ttu-id="0b05a-138">Svazek operačního systému Windows je obvykle nepostradatelný svazek.</span><span class="sxs-lookup"><span data-stu-id="0b05a-138">Usually, the Windows OS volume is the critical volume.</span></span>

10. <span data-ttu-id="0b05a-139">Po obnovení je úspěšné, postupujte podle kroků v části [použít obnovit stav systému souborů pro Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), k dokončení procesu obnovení stavu systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-139">Once the recovery is successful, follow the steps in the section, [Apply restored System State files to the Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), to complete the System State recovery process.</span></span>

## <a name="recover-system-state-files-to-an-alternate-server"></a><span data-ttu-id="0b05a-140">Obnovení stavu systému souborů na alternativní server</span><span class="sxs-lookup"><span data-stu-id="0b05a-140">Recover System State files to an alternate server</span></span>

<span data-ttu-id="0b05a-141">Pokud váš Server systému Windows je poškozená nebo je nepřístupný a chcete ho obnovit stabilního stavu pomocí obnovení stavu systému Windows Server, můžete obnovit stav systému poškozená serveru z jiného serveru.</span><span class="sxs-lookup"><span data-stu-id="0b05a-141">If your Windows Server is corrupted or inaccessible, and you want to restore it to a stable state by recovering the Windows Server System State, you can restore the corrupted server's System State from another server.</span></span> <span data-ttu-id="0b05a-142">Použijte následující postup obnovení stavu systému na samostatný server.</span><span class="sxs-lookup"><span data-stu-id="0b05a-142">Use the following steps to the restore System State on a separate server.</span></span>  

<span data-ttu-id="0b05a-143">Zahrnuje technologiím použitým v těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="0b05a-143">The terminology used in these steps includes:</span></span>

- <span data-ttu-id="0b05a-144">*Zdrojový počítač* – původní počítač, ze kterého bylo provedeno zálohování a který není aktuálně k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0b05a-144">*Source machine* – The original machine from which the backup was taken and which is currently unavailable.</span></span>
- <span data-ttu-id="0b05a-145">*Cílový počítač* – počítače, do níž se obnovuje data.</span><span class="sxs-lookup"><span data-stu-id="0b05a-145">*Target machine* – The machine to which the data is being recovered.</span></span>
- <span data-ttu-id="0b05a-146">*Ukázka trezoru* – trezor služeb zotavení, ke kterému *zdrojový počítač* a *cílový počítač* jsou registrované.</span><span class="sxs-lookup"><span data-stu-id="0b05a-146">*Sample vault* – The Recovery Services vault to which the *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="0b05a-147">Zálohy vytvořené z jednoho počítače nelze obnovit na počítač se starší verzí operačního systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-147">Backups taken from one machine cannot be restored to a machine running an earlier version of the operating system.</span></span> <span data-ttu-id="0b05a-148">Například zálohy pořízené z počítače systému Windows Server 2016 nelze obnovit na Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="0b05a-148">For example, backups taken from a Windows Server 2016 machine can't be restored to Windows Server 2012 R2.</span></span> <span data-ttu-id="0b05a-149">Inverzní je ale možné.</span><span class="sxs-lookup"><span data-stu-id="0b05a-149">However, the inverse is possible.</span></span> <span data-ttu-id="0b05a-150">Zálohování z Windows serveru 2012 R2 můžete použít k obnovení systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="0b05a-150">You can use backups from Windows Server 2012 R2 to restore Windows Server 2016.</span></span>
>

1. <span data-ttu-id="0b05a-151">Otevřete **Microsoft Azure Backup** modul snap-in na *cílový počítač*.</span><span class="sxs-lookup"><span data-stu-id="0b05a-151">Open the **Microsoft Azure Backup** snap-in on the *Target machine*.</span></span>
2. <span data-ttu-id="0b05a-152">Ujistěte se, že *cílový počítač* a *zdrojový počítač* jsou registrované ke stejnému trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="0b05a-152">Ensure that the *Target machine* and the *Source machine* are registered to the same Recovery Services vault.</span></span>
3. <span data-ttu-id="0b05a-153">Klikněte na tlačítko **obnovit Data** inicializace pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="0b05a-153">Click **Recover Data** to initiate the workflow.</span></span>

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)

4. <span data-ttu-id="0b05a-155">Vyberte **jiný server**</span><span class="sxs-lookup"><span data-stu-id="0b05a-155">Select **Another server**</span></span>

    ![Jiný Server](./media/backup-azure-restore-system-state/anotherserver.png)

5. <span data-ttu-id="0b05a-157">Zadejte soubor s přihlašovacími údaji trezoru, která odpovídá *ukázka trezoru*.</span><span class="sxs-lookup"><span data-stu-id="0b05a-157">Provide the vault credential file that corresponds to the *Sample vault*.</span></span> <span data-ttu-id="0b05a-158">Pokud soubor s přihlašovacími údaji trezoru je neplatný (nebo vypršela platnost), stáhněte si nový soubor přihlašovacích údajů trezoru z *ukázka trezoru* na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0b05a-158">If the vault credential file is invalid (or expired), download a new vault credential file from the *Sample vault* in the Azure portal.</span></span> <span data-ttu-id="0b05a-159">Jakmile je zadaný soubor s přihlašovacími údaji trezoru, se zobrazí trezor služeb zotavení přidružený soubor s přihlašovacími údaji trezoru.</span><span class="sxs-lookup"><span data-stu-id="0b05a-159">Once the vault credential file is provided, the Recovery Services vault associated with the vault credential file appears.</span></span>

6. <span data-ttu-id="0b05a-160">V podokně vyberte Server Backup, vyberte *zdrojový počítač* ze seznamu zobrazených počítačů.</span><span class="sxs-lookup"><span data-stu-id="0b05a-160">On the Select Backup Server pane, select the *Source machine* from the list of displayed machines.</span></span>

    ![Seznam počítačů](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. <span data-ttu-id="0b05a-162">V podokně vyberte režimu obnovení, vyberte **stav systému** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-162">On the Select Recovery Mode pane, choose **System State** and click **Next**.</span></span> 

    ![Search](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. <span data-ttu-id="0b05a-164">V kalendáři v **vyberte svazek a datum** podokně, vyberte obnovení bodu.</span><span class="sxs-lookup"><span data-stu-id="0b05a-164">On the Calendar in the **Select Volume and Date** pane, select a recovery point.</span></span> <span data-ttu-id="0b05a-165">Můžete obnovit z libovolného obnovení bodu v čase.</span><span class="sxs-lookup"><span data-stu-id="0b05a-165">You can restore from any recovery point in time.</span></span> <span data-ttu-id="0b05a-166">Data v **tučné** označuje dostupnost alespoň jeden bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="0b05a-166">Dates in **bold** indicate the availability of at least one recovery point.</span></span> <span data-ttu-id="0b05a-167">Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte konkrétní bod obnovení z **čas** rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="0b05a-167">Once  you select a date, if multiple recovery points are available, choose the specific recovery point from the **Time** drop-down menu.</span></span> 

    ![Hledání položek](./media/backup-azure-restore-system-state/select-date.png)

9. <span data-ttu-id="0b05a-169">Po zadání bodu obnovení pro obnovení, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-169">Once you have chosen the recovery point to restore, click **Next**.</span></span>

10. <span data-ttu-id="0b05a-170">Na **vyberte režim obnovení stavu systému** podokně zadejte cíl, kde chcete soubory stavu systému obnovit a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-170">On the **Select System State Recovery Mode** pane, specify the destination where you want System State files to be recovered, then click **Next**.</span></span>

    ![Šifrování](./media/backup-azure-restore-system-state/recover-as-files.png)

    <span data-ttu-id="0b05a-172">Možnost **vytvořit kopie tak, aby obě verze**, vytváří kopie jednotlivé soubory do stávajícího archivu soubor stavu systému místo vytvoření kopie celý archivu stavu systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-172">The option, **Create copies so that you have both versions**, creates copies of individual files in an existing System State file archive instead of creating the copy of the entire System State archive.</span></span>

11. <span data-ttu-id="0b05a-173">Zkontrolujte podrobnosti o obnovení v podokně potvrzení a klikněte na tlačítko **obnovit**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-173">Verify the details of recovery on the Confirmation pane, and click **Recover**.</span></span> 

    ![Klikněte na tlačítko Obnovit potvrďte proces obnovení](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. <span data-ttu-id="0b05a-175">Kopírování *WindowsImageBackup* adresář nekritické svazku serveru (například D:\).</span><span class="sxs-lookup"><span data-stu-id="0b05a-175">Copy the *WindowsImageBackup* directory to a non-critical volume of the server (for example D:\).</span></span> <span data-ttu-id="0b05a-176">Svazek operačního systému Windows je obvykle nepostradatelný svazek.</span><span class="sxs-lookup"><span data-stu-id="0b05a-176">Usually the Windows OS volume is the critical volume.</span></span>

13. <span data-ttu-id="0b05a-177">Chcete-li dokončit proces obnovy, použijte následující části [použít obnovené soubory stavu systému v systému Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).</span><span class="sxs-lookup"><span data-stu-id="0b05a-177">To complete the recovery process, use the following section to [apply the restored System State files on a Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).</span></span>




## <a name="apply-restored-system-state-on-a-windows-server"></a><span data-ttu-id="0b05a-178">Použít obnovený stav systému v systému Windows Server</span><span class="sxs-lookup"><span data-stu-id="0b05a-178">Apply restored System State on a Windows Server</span></span>

<span data-ttu-id="0b05a-179">Jednou obnovily stavu systému jako soubory pomocí agenta služeb zotavení Azure, použijte nástroj zálohování systému Windows Server použít obnovené stavu systému na Windows Server.</span><span class="sxs-lookup"><span data-stu-id="0b05a-179">Once you have recovered System State as files using Azure Recovery Services Agent, use the Windows Server Backup utility to apply the recovered System State to Windows Server.</span></span> <span data-ttu-id="0b05a-180">Nástroj Zálohování systému Windows Server je již k dispozici na serveru.</span><span class="sxs-lookup"><span data-stu-id="0b05a-180">The Windows Server Backup utility is already available on the server.</span></span> <span data-ttu-id="0b05a-181">Následující kroky popisují, jak použít obnovené stav systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-181">The following steps explain how to apply the recovered System State.</span></span>

1. <span data-ttu-id="0b05a-182">Použijte následující příkazy k restartování serveru v *režimu obnovení adresářových služeb*.</span><span class="sxs-lookup"><span data-stu-id="0b05a-182">Use the following commands to reboot your server in *Directory Services Repair Mode*.</span></span> <span data-ttu-id="0b05a-183">V řádku se zvýšenými oprávněními příkaz:</span><span class="sxs-lookup"><span data-stu-id="0b05a-183">In an elevated command prompt:</span></span>

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. <span data-ttu-id="0b05a-184">Po restartování otevřete modul snap-in Zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="0b05a-184">After the reboot, open the Windows Server Backup snap-in.</span></span> <span data-ttu-id="0b05a-185">Pokud si nejste jisti, kde nainstalovaný modul snap-in, hledání se počítač nebo server pro **zálohování serveru**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-185">If you don't know where the snap-in was installed, search the computer or server for **Windows Server Backup**.</span></span>

    <span data-ttu-id="0b05a-186">Desktopová aplikace se zobrazí ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="0b05a-186">The desktop app appears in the search results.</span></span>

3. <span data-ttu-id="0b05a-187">V modulu snap-in, vyberte **místní záloha**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-187">In the snap-in, select **Local Backup**.</span></span>

    ![Vyberte místní zálohu k obnovení z tohoto umístění](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. <span data-ttu-id="0b05a-189">V konzole místní záloha v **podokna akce**, klikněte na tlačítko **obnovit** otevřete Průvodce obnovením.</span><span class="sxs-lookup"><span data-stu-id="0b05a-189">On the Local Backup console, in the **Actions Pane**, click **Recover** to open the Recovery Wizard.</span></span>

5. <span data-ttu-id="0b05a-190">Vyberte možnost, **zálohy uložené v jiném umístění**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-190">Select the option, **A backup stored in another location**, and click **Next**.</span></span>

   ![Vyberte k obnovení na jiný server](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. <span data-ttu-id="0b05a-192">Při zadávání typ umístění, vyberte **vzdálené sdílené složce** Pokud záloha stavu systému se obnovení provádělo na jiný server.</span><span class="sxs-lookup"><span data-stu-id="0b05a-192">When specifying the location type, select **Remote shared folder** if your System State backup was recovered to another server.</span></span> <span data-ttu-id="0b05a-193">Pokud stav systému byla obnovena místně, pak vyberte **místní jednotky**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-193">If your System State was recovered locally, then select **Local drives**.</span></span> 

    ![Vyberte, zda se má obnovení z místního serveru nebo jiné](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. <span data-ttu-id="0b05a-195">Zadejte cestu k *WindowsImageBackup* adresář, nebo zvolte místní jednotku, která obsahuje tento adresář (například D:\WindowsImageBackup), obnovit jako součást obnovení stavu systému souborů pomocí služeb zotavení Azure Agent a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-195">Enter the path to the *WindowsImageBackup* directory, or choose the local drive containing this directory (for example, D:\WindowsImageBackup), recovered as part of the System State files recovery using Azure Recovery Services Agent and click **Next**.</span></span>

    ![Cesta pro sdílený soubor](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. <span data-ttu-id="0b05a-197">Vyberte verzi stav systému, kterou chcete obnovit a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-197">Select the System State version that you want to restore, and click **Next**.</span></span>

9. <span data-ttu-id="0b05a-198">V podokně Vybrat typ obnovení, vyberte **stav systému** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-198">In the Select Recovery Type pane, select **System State** and click **Next**.</span></span>

10. <span data-ttu-id="0b05a-199">Pro umístění obnovení stavu systému, vyberte **původního umístění**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0b05a-199">For the location of the System State Recovery, select **Original Location**, and click **Next**.</span></span>

11. <span data-ttu-id="0b05a-200">Zkontrolujte podrobnosti o potvrzení, ověřte nastavení restartování a klikněte na tlačítko **obnovit** k applly obnovený stav systému souborů.</span><span class="sxs-lookup"><span data-stu-id="0b05a-200">Review the confirmation details, verify the reboot settings, and click **Recover** to applly the restored System State files.</span></span>

    ![spuštění obnovení stavu systému souborů](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a><span data-ttu-id="0b05a-202">Zvláštní upozornění pro obnovení stavu systému na serveru služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="0b05a-202">Special considerations for System State recovery on Active Directory server</span></span>

<span data-ttu-id="0b05a-203">Zálohování stavu systému zahrnuje data služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0b05a-203">System State backup includes Active Directory data.</span></span> <span data-ttu-id="0b05a-204">Použijte následující kroky k obnovení služby Active Directory Domain Services (AD DS) z jeho aktuální stav do předchozího stavu.</span><span class="sxs-lookup"><span data-stu-id="0b05a-204">Use the following steps to restore Active Directory Domain Service (AD DS) from its current state to a previous state.</span></span>

1. <span data-ttu-id="0b05a-205">Restartujte řadič domény v adresářových služeb obnovení režimu (DSRM).</span><span class="sxs-lookup"><span data-stu-id="0b05a-205">Restart the domain controller in Directory Services Restore Mode (DSRM).</span></span>
2. <span data-ttu-id="0b05a-206">Postupujte podle kroků [sem](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) použití rutiny zálohování serveru k obnovení služby AD DS.</span><span class="sxs-lookup"><span data-stu-id="0b05a-206">Follow the steps [here](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) to use Windows Server Backup cmdlets to recover AD DS.</span></span>


## <a name="troubleshoot-failed-system-state-restore"></a><span data-ttu-id="0b05a-207">Řešení potíží se nezdařilo obnovení stavu systému</span><span class="sxs-lookup"><span data-stu-id="0b05a-207">Troubleshoot failed System State restore</span></span>

<span data-ttu-id="0b05a-208">Pokud předchozí proces použití stavu systému úspěšně nedokončí, použijte prostředí Windows Recovery Environment (Win RE) k obnovení systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="0b05a-208">If the previous process of applying System State does not complete successfully, use the Windows Recovery Environment (Win RE) to recover your Windows Server.</span></span> <span data-ttu-id="0b05a-209">Následující kroky popisují, jak obnovit pomocí Win znovu.</span><span class="sxs-lookup"><span data-stu-id="0b05a-209">The following steps explain how to recover using Win RE.</span></span> <span data-ttu-id="0b05a-210">Tuto možnost použijte pouze v případě systému Windows Server není normálnímu spuštění po obnovení stavu systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-210">Use This option only if Windows Server does not boot normally after a System State restore.</span></span> <span data-ttu-id="0b05a-211">Následující proces vymaže data nesystémové, buďte opatrní.</span><span class="sxs-lookup"><span data-stu-id="0b05a-211">The following process erases non-system data, use caution.</span></span> 

1. <span data-ttu-id="0b05a-212">Spouštění systému Windows Server do prostředí Windows Recovery Environment (Windows RE).</span><span class="sxs-lookup"><span data-stu-id="0b05a-212">Boot your Windows Server into the Windows Recovery Environment (Win RE).</span></span>

2. <span data-ttu-id="0b05a-213">Vyberte Poradce při potížích ze tří dostupných možností.</span><span class="sxs-lookup"><span data-stu-id="0b05a-213">Select Troubleshoot from the three available options.</span></span>

    ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-1.png)

3. <span data-ttu-id="0b05a-215">Z **pokročilé možnosti** obrazovku, vyberte **příkazového řádku** a zadejte uživatelské jméno správce serveru a heslo.</span><span class="sxs-lookup"><span data-stu-id="0b05a-215">From the **Advanced Options** screen, select **Command Prompt** and provide the server administrator username and password.</span></span>

   ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-2.png)

4. <span data-ttu-id="0b05a-217">Zadejte uživatelské jméno správce serveru a heslo.</span><span class="sxs-lookup"><span data-stu-id="0b05a-217">Provide the server administrator username and password.</span></span>

    ![Otevření nabídky](./media/backup-azure-restore-system-state/winre-3.png)

5. <span data-ttu-id="0b05a-219">Když otevřete příkazový řádek v režimu správce, spusťte následující příkaz k získání verzí zálohování stavu systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-219">When you open the command prompt in administrator mode, run following command to get the System State backup versions.</span></span>

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-4.png)

6. <span data-ttu-id="0b05a-221">Spusťte následující příkaz získat všechny svazky, které jsou k dispozici v zálohování.</span><span class="sxs-lookup"><span data-stu-id="0b05a-221">Run the following command to get all volumes available in the backup.</span></span>

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-5.png)

7. <span data-ttu-id="0b05a-223">Následující příkaz obnoví všechny svazky, které jsou součástí zálohování stavu systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-223">The following command recovers all volumes that are part of the System State Backup.</span></span> <span data-ttu-id="0b05a-224">Všimněte si, že tento krok obnoví jenom nepostradatelné svazky, které jsou součástí stavu systému.</span><span class="sxs-lookup"><span data-stu-id="0b05a-224">Note that this step recovers only the critical volumes that are part of the System State.</span></span> <span data-ttu-id="0b05a-225">Všechna nesystémové data budou vymazána.</span><span class="sxs-lookup"><span data-stu-id="0b05a-225">All non-System data is erased.</span></span>

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![získání verze zálohování stavu systému](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a><span data-ttu-id="0b05a-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b05a-227">Next steps</span></span>
* <span data-ttu-id="0b05a-228">Teď, když jste obnovit soubory a složky, můžete [spravovat zálohy](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="0b05a-228">Now that you've recovered your files and folders, you can [manage your backups](backup-azure-manage-windows-server.md).</span></span>
