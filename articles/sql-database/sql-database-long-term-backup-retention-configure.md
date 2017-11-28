---
title: "Konfigurace dlouhodobé uchovávání záloh – Azure SQL database | Microsoft Docs"
description: "Zjistěte, jak toostore automatizované zálohování v hello trezoru služeb zotavení Azure a toorestore z hello trezoru služeb zotavení Azure"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: carlrab
ms.openlocfilehash: 603f4dd21cee4407d46f749655aba8f9ef3322c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a><span data-ttu-id="3cbb3-103">Konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="3cbb3-103">Configure and restore from Azure SQL Database long-term backup retention</span></span>

<span data-ttu-id="3cbb3-104">Můžete nakonfigurovat trezoru služeb zotavení Azure hello, toostore zálohy databáze Azure SQL a potom obnovit databázi pomocí zálohy uchovávány v hello trezoru pomocí hello portál Azure nebo PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-104">You can configure hello Azure Recovery Services vault toostore Azure SQL database backups and then recover a database using backups retained in hello vault using hello Azure portal or PowerShell.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="3cbb3-105">portál Azure</span><span class="sxs-lookup"><span data-stu-id="3cbb3-105">Azure portal</span></span>

<span data-ttu-id="3cbb3-106">Následující části zobrazit jak toouse hello Azure portálu tooconfigure hello služeb zotavení Azure trezoru, Zobrazit zálohy v trezoru hello a obnovení z trezoru hello Hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-106">hello following sections show you how toouse hello Azure portal tooconfigure hello Azure Recovery Services vault, view backups in hello vault, and restore from hello vault.</span></span>

### <a name="configure-hello-vault-register-hello-server-and-select-databases"></a><span data-ttu-id="3cbb3-107">Konfigurace hello trezoru, zaregistrujte hello server a vybrat databáze</span><span class="sxs-lookup"><span data-stu-id="3cbb3-107">Configure hello vault, register hello server, and select databases</span></span>

<span data-ttu-id="3cbb3-108">Můžete [konfigurace zálohování tooretain automatizované trezoru služeb zotavení Azure](sql-database-long-term-retention.md) po dobu delší než doba uchování hello vaší vrstvy služeb.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-108">You [configure an Azure Recovery Services vault tooretain automated backups](sql-database-long-term-retention.md) for a period longer than hello retention period for your service tier.</span></span> 

1. <span data-ttu-id="3cbb3-109">Otevřete hello **systému SQL Server** stránka serveru.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-109">Open hello **SQL Server** page for your server.</span></span>

   ![stránka serveru SQL](./media/sql-database-get-started-portal/sql-server-blade.png)

2. <span data-ttu-id="3cbb3-111">Klikněte na **Dlouhodobé uchovávání záloh**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-111">Click **Long-term backup retention**.</span></span>

   ![odkaz na dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. <span data-ttu-id="3cbb3-113">Na hello **dlouhodobé uchovávání záloh** stránky pro váš server, přečtěte si a přijměte podmínky verze preview hello (Pokud budete mít neudělali - nebo tato funkce je už ve verzi preview).</span><span class="sxs-lookup"><span data-stu-id="3cbb3-113">On hello **Long-term backup retention** page for your server, review and accept hello preview terms (unless you have already done so - or this feature is no longer in preview).</span></span>

   ![Přijměte podmínky verze preview hello](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. <span data-ttu-id="3cbb3-115">Vyberte tuto databázi v mřížce hello tooconfigure dlouhodobé uchovávání záloh a potom klikněte na **konfigurace** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-115">tooconfigure long-term backup retention, select that database in hello grid and then click **Configure** on hello toolbar.</span></span>

   ![výběr databáze pro dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. <span data-ttu-id="3cbb3-117">Na hello **konfigurace** klikněte na tlačítko **konfigurovat požadované nastavení** pod **trezoru služby zotavení**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-117">On hello **Configure** page, click **Configure required settings** under **Recovery service vault**.</span></span>

   ![odkaz na konfiguraci trezoru](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. <span data-ttu-id="3cbb3-119">Na hello **trezoru služeb zotavení** vyberte existující trezor, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-119">On hello **Recovery services vault** page, select an existing vault, if any.</span></span> <span data-ttu-id="3cbb3-120">Pokud žádné trezoru služeb zotavení pro vaše předplatné nalezen, jinak hodnota klikněte tooexit hello toku a vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-120">Otherwise, if no recovery services vault found for your subscription, click tooexit hello flow and create a recovery services vault.</span></span>

   ![Vytvoření trezoru propojení](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. <span data-ttu-id="3cbb3-122">Na hello **trezory služeb zotavení** klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-122">On hello **Recovery Services vaults** page, click **Add**.</span></span>

   ![Přidat odkaz na trezoru](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. <span data-ttu-id="3cbb3-124">Na hello **trezor služeb zotavení** stránky, zadejte platný název pro trezor služeb zotavení hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-124">On hello **Recovery Services vault** page, provide a valid name for hello Recovery Services vault.</span></span>

   ![název nového trezoru](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. <span data-ttu-id="3cbb3-126">Vyberte předplatné a skupina prostředků a potom vyberte hello umístění pro trezor hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-126">Select your subscription and resource group, and then select hello location for hello vault.</span></span> <span data-ttu-id="3cbb3-127">Až budete hotovi, klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-127">When done, click **Create**.</span></span>

   ![Vytvoření trezoru](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > <span data-ttu-id="3cbb3-129">Hello trezoru se musí nacházet v hello stejné oblasti jako logického serveru Azure SQL hello a musí použít hello stejné skupině prostředků jako hello logického serveru.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-129">hello vault must be located in hello same region as hello Azure SQL logical server, and must use hello same resource group as hello logical server.</span></span>
   >

10. <span data-ttu-id="3cbb3-130">Po vytvoření nového trezoru hello provést hello potřebné kroky tooreturn toohello **trezoru služeb zotavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-130">After hello new vault is created, execute hello necessary steps tooreturn toohello **Recovery services vault** page.</span></span>

11. <span data-ttu-id="3cbb3-131">Na hello **trezoru služeb zotavení** klikněte hello trezoru a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-131">On hello **Recovery services vault** page, click hello vault and then click **Select**.</span></span>

   ![výběr existujícího trezoru](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. <span data-ttu-id="3cbb3-133">Na hello **konfigurace** stránky, zadejte platný název pro nové zásady uchovávání informací hello, hello výchozí zásady uchovávání informací podle potřeby upravit a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-133">On hello **Configure** page, provide a valid name for hello new retention policy, modify hello default retention policy as appropriate, and then click **OK**.</span></span>

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. <span data-ttu-id="3cbb3-135">Na hello **dlouhodobé uchovávání záloh** stránky pro vaši databázi, klikněte na tlačítko **Uložit** a pak klikněte na **OK** tooapply hello tooall zásady dlouhodobé uchovávání záloh, které jsou vybrané databáze.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-135">On hello **Long-term backup retention** page for your database, click **Save** and then click **OK** tooapply hello long-term backup retention policy tooall selected databases.</span></span>

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. <span data-ttu-id="3cbb3-137">Klikněte na tlačítko **Uložit** tooenable dlouhodobé zálohování uchovávání pomocí této nové zásady toohello služeb zotavení Azure trezoru, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-137">Click **Save** tooenable long-term backup retention using this new policy toohello Azure Recovery Services vault that you configured.</span></span>

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> <span data-ttu-id="3cbb3-139">Po nakonfigurování zálohování zobrazí v trezoru hello během příštích 7 dnů.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-139">Once configured, backups show up in hello vault within next seven days.</span></span> <span data-ttu-id="3cbb3-140">V tomto kurzu nepokračujte, dokud zálohování zobrazí v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-140">Do not continue this tutorial until backups show up in hello vault.</span></span>
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a><span data-ttu-id="3cbb3-141">Zobrazení záloh v dlouhodobé uchovávání pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3cbb3-141">View backups in long-term retention using Azure portal</span></span>

<span data-ttu-id="3cbb3-142">Zobrazit informace o zálohování databáze v [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="3cbb3-142">View information about your database backups in [long-term backup retention](sql-database-long-term-retention.md).</span></span> 

1. <span data-ttu-id="3cbb3-143">V hello portálu Azure, otevřete svůj trezor služeb zotavení Azure pro zálohování databáze (přejděte příliš**všechny prostředky** a vyberte ho ze seznamu hello prostředky pro vaše předplatné) tooview hello množství úložiště používá databázi Zálohování v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-143">In hello Azure portal, open your Azure Recovery Services vault for your database backups (go too**All resources** and select it from hello list of resources for your subscription) tooview hello amount of storage used by your database backups in hello vault.</span></span>

   ![zobrazení trezoru služby recovery services se zálohami](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. <span data-ttu-id="3cbb3-145">Otevřete hello **databáze SQL** stránky pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-145">Open hello **SQL database** page for your database.</span></span>

   ![Nová stránka ukázkové databáze](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. <span data-ttu-id="3cbb3-147">Na panelu nástrojů hello, klikněte na tlačítko **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-147">On hello toolbar, click **Restore**.</span></span>

   ![panel nástrojů – obnovit](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. <span data-ttu-id="3cbb3-149">Na stránce hello obnovení, klikněte na **dlouhodobé**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-149">On hello Restore page, click **Long-term**.</span></span>

5. <span data-ttu-id="3cbb3-150">V oblasti Azure trezoru záloh, klikněte na **vyberte zálohu** tooview hello k dispozici databáze záloh v dlouhodobé uchovávání záloh.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-150">Under Azure vault backups, click **Select a backup** tooview hello available database backups in long-term backup retention.</span></span>

   ![zálohy v trezoru](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-hello-azure-portal"></a><span data-ttu-id="3cbb3-152">Obnovte databázi ze zálohy v dlouhodobé uchovávání záloh pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3cbb3-152">Restore a database from a backup in long-term backup retention using hello Azure portal</span></span>

<span data-ttu-id="3cbb3-153">Hello databáze tooa novou databázi můžete obnovit ze zálohy v trezoru služeb zotavení Azure hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-153">You restore hello database tooa new database from a backup in hello Azure Recovery Services vault.</span></span>

1. <span data-ttu-id="3cbb3-154">Na hello **Azure trezoru záloh** klikněte hello zálohování toorestore a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-154">On hello **Azure vault backups** page, click hello backup toorestore and then click **Select**.</span></span>

   ![výběr zálohy v trezoru](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. <span data-ttu-id="3cbb3-156">V hello **název databáze** textové pole, zadejte název hello hello obnovit databáze.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-156">In hello **Database name** text box, provide hello name for hello restored database.</span></span>

   ![nový název databáze](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. <span data-ttu-id="3cbb3-158">Klikněte na tlačítko **OK** toorestore databáze ze zálohy hello v hello trezoru toohello novou databázi.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-158">Click **OK** toorestore your database from hello backup in hello vault toohello new database.</span></span>

4. <span data-ttu-id="3cbb3-159">Na panelu nástrojů hello klikněte na ikonu oznámení hello tooview hello stav úlohy obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-159">On hello toolbar, click hello notification icon tooview hello status of hello restore job.</span></span>

   ![průběh úlohy obnovení z trezoru](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. <span data-ttu-id="3cbb3-161">Po dokončení úlohy obnovení hello otevřete hello **databází SQL** stránky tooview hello nově obnovit databázi.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-161">When hello restore job is completed, open hello **SQL databases** page tooview hello newly restored database.</span></span>

   ![obnovená databáze z trezoru](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> <span data-ttu-id="3cbb3-163">Tady můžete připojit toohello obnovit databáze pomocí SQL Server Management Studio tooperform potřebné úkoly, jako třeba příliš[extrahovat bit dat z databáze toocopy hello obnovit do hello existující databázi nebo existující toodelete hello Název databáze a přejmenování hello obnovit databáze toohello existující databáze](sql-database-recovery-using-backups.md#point-in-time-restore).</span><span class="sxs-lookup"><span data-stu-id="3cbb3-163">From here, you can connect toohello restored database using SQL Server Management Studio tooperform needed tasks, such as too[extract a bit of data from hello restored database toocopy into hello existing database or toodelete hello existing database and rename hello restored database toohello existing database name](sql-database-recovery-using-backups.md#point-in-time-restore).</span></span>
>

## <a name="powershell"></a><span data-ttu-id="3cbb3-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3cbb3-164">PowerShell</span></span>

<span data-ttu-id="3cbb3-165">Hello následující části ukazují, jak zobrazit zálohy v trezoru hello toouse prostředí PowerShell tooconfigure hello služeb zotavení Azure trezoru a obnovení z trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-165">hello following sections show you how toouse PowerShell tooconfigure hello Azure Recovery Services vault, view backups in hello vault, and restore from hello vault.</span></span>

### <a name="create-a-recovery-services-vault"></a><span data-ttu-id="3cbb3-166">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="3cbb3-166">Create a recovery services vault</span></span>

<span data-ttu-id="3cbb3-167">Použití hello [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate obnovení služeb trezoru.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-167">Use hello [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate a recovery services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3cbb3-168">Hello trezoru se musí nacházet v hello stejné oblasti jako logického serveru Azure SQL hello a musí použít hello stejné skupině prostředků jako hello logického serveru.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-168">hello vault must be located in hello same region as hello Azure SQL logical server, and must use hello same resource group as hello logical server.</span></span>

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-toouse-hello-recovery-vault-for-its-long-term-retention-backups"></a><span data-ttu-id="3cbb3-169">Nastavení trezoru server hello toouse obnovení pro jeho dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="3cbb3-169">Set your server toouse hello recovery vault for its long-term retention backups</span></span>

<span data-ttu-id="3cbb3-170">Použití hello [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) rutiny tooassociate dříve vytvořený trezor služeb zotavení s konkrétní server Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-170">Use hello [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet tooassociate a previously created recovery services vault with a specific Azure SQL server.</span></span>

```PowerShell
# Set your server toouse hello vault toofor long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a><span data-ttu-id="3cbb3-171">Vytvoření zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="3cbb3-171">Create a retention policy</span></span>

<span data-ttu-id="3cbb3-172">Zásady uchovávání informací je, kde můžete nastavit dobu tookeep zálohu databáze.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-172">A retention policy is where you set how long tookeep a database backup.</span></span> <span data-ttu-id="3cbb3-173">Použití hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) rutiny tooget hello výchozí uchování zásad toouse jako hello šablonu pro vytváření zásad.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-173">Use hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet tooget hello default retention policy toouse as hello template for creating policies.</span></span> <span data-ttu-id="3cbb3-174">V této šabloně dobu uchování hello nastavena na 2 roky.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-174">In this template, hello retention period is set for 2 years.</span></span> <span data-ttu-id="3cbb3-175">Potom spustíte hello [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally vytvořit zásadu hello.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-175">Next, run hello [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally create hello policy.</span></span> 

> [!NOTE]
> <span data-ttu-id="3cbb3-176">Některé rutiny vyžadují, abyste nastavili hello trezoru kontextu dřív, než spustíte ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) tak, aby zobrazila tato rutina v několika související fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-176">Some cmdlets require that you set hello vault context before running ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) so you see this cmdlet in a few related snippets.</span></span> <span data-ttu-id="3cbb3-177">Můžete nastavit hello kontextu, protože zásady hello je součástí hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-177">You set hello context because hello policy is part of hello vault.</span></span> <span data-ttu-id="3cbb3-178">Můžete vytvořit více zásady uchovávání informací pro každý trezor a pak aplikovat hello požadovaných zásad toospecific databáze.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-178">You can create multiple retention policies for each vault and then apply hello desired policy toospecific databases.</span></span> 


```PowerShell
# Retrieve hello default retention policy for hello AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set hello retention value tootwo years (you can set tooany time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set hello vault context toohello vault you are creating hello policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create hello new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-toouse-hello-previously-defined-retention-policy"></a><span data-ttu-id="3cbb3-179">Nakonfigurujte zásady uchovávání informací hello předtím definovaný toouse databáze</span><span class="sxs-lookup"><span data-stu-id="3cbb3-179">Configure a database toouse hello previously defined retention policy</span></span>

<span data-ttu-id="3cbb3-180">Použití hello [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) rutiny tooapply hello nové zásady tooa konkrétní databáze.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-180">Use hello [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet tooapply hello new policy tooa specific database.</span></span>

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a><span data-ttu-id="3cbb3-181">Zobrazení informací o záloze a záloh v rámci dlouhodobého uchovávání</span><span class="sxs-lookup"><span data-stu-id="3cbb3-181">View backup info, and backups in long-term retention</span></span>

<span data-ttu-id="3cbb3-182">Zobrazit informace o zálohování databáze v [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="3cbb3-182">View information about your database backups in [long-term backup retention](sql-database-long-term-retention.md).</span></span> 

<span data-ttu-id="3cbb3-183">Použijte následující informace o zálohování tooview rutiny hello:</span><span class="sxs-lookup"><span data-stu-id="3cbb3-183">Use hello following cmdlets tooview backup information:</span></span>

- [<span data-ttu-id="3cbb3-184">Get-AzureRmRecoveryServicesBackupContainer</span><span class="sxs-lookup"><span data-stu-id="3cbb3-184">Get-AzureRmRecoveryServicesBackupContainer</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [<span data-ttu-id="3cbb3-185">Get-AzureRmRecoveryServicesBackupItem</span><span class="sxs-lookup"><span data-stu-id="3cbb3-185">Get-AzureRmRecoveryServicesBackupItem</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [<span data-ttu-id="3cbb3-186">Get-AzureRmRecoveryServicesBackupRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="3cbb3-186">Get-AzureRmRecoveryServicesBackupRecoveryPoint</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

```PowerShell
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set hello vault context toohello vault we want toorestore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# hello following commands find hello container associated with hello server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get hello long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore

# Get all available backups for hello previously indicated database
# Optionally, set hello -StartDate and -EndDate parameters tooreturn backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a><span data-ttu-id="3cbb3-187">Obnovení databáze ze zálohy v rámci dlouhodobého uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="3cbb3-187">Restore a database from a backup in long-term backup retention</span></span>

<span data-ttu-id="3cbb3-188">Obnovení ze zálohy dlouhodobé uchovávání používá hello [obnovení-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-188">Restoring from long-term backup retention uses hello [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.</span></span>

```PowerShell
# Restore hello most recent backup: $availableBackups[0]
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$restoredDatabaseName = "{new-database-name}"
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDb = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel
$restoredDb
```


> [!NOTE]
> <span data-ttu-id="3cbb3-189">Zde se můžete připojit toohello obnovit databázi pomocí aplikace SQL Server Management Studio tooperform potřebné úkoly, jako je například tooextract bit dat z hello obnovit databáze toocopy do hello existující databázi nebo existující databázi toodelete hello a přejmenování Hello obnovené databáze toohello název existující databáze.</span><span class="sxs-lookup"><span data-stu-id="3cbb3-189">From here, you can connect toohello restored database using SQL Server Management Studio tooperform needed tasks, such as tooextract a bit of data from hello restored database toocopy into hello existing database or toodelete hello existing database and rename hello restored database toohello existing database name.</span></span> <span data-ttu-id="3cbb3-190">V tématu [bodu v době obnovení](sql-database-recovery-using-backups.md#point-in-time-restore).</span><span class="sxs-lookup"><span data-stu-id="3cbb3-190">See [point in time restore](sql-database-recovery-using-backups.md#point-in-time-restore).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cbb3-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3cbb3-191">Next steps</span></span>

- <span data-ttu-id="3cbb3-192">toolearn o generované služby Automatické zálohování, najdete v části [automatické zálohování](sql-database-automated-backups.md)</span><span class="sxs-lookup"><span data-stu-id="3cbb3-192">toolearn about service-generated automatic backups, see [automatic backups](sql-database-automated-backups.md)</span></span>
- <span data-ttu-id="3cbb3-193">toolearn o dlouhodobé uchovávání záloh, najdete v části [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md)</span><span class="sxs-lookup"><span data-stu-id="3cbb3-193">toolearn about long-term backup retention, see [long-term backup retention](sql-database-long-term-retention.md)</span></span>
- <span data-ttu-id="3cbb3-194">toolearn o obnovení ze zálohy, najdete v části [obnovit ze zálohy](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="3cbb3-194">toolearn about restoring from backups, see [restore from backup](sql-database-recovery-using-backups.md)</span></span>
