---
title: "Konfigurace dlouhodobé uchovávání záloh – Azure SQL database | Microsoft Docs"
description: "Zjistěte, jak uložit automatizované zálohování do trezoru služeb zotavení Azure a obnovit z trezoru služeb zotavení Azure"
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
ms.openlocfilehash: ed9f74a59f0ca512e2758c6db4c5c9075030f859
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a><span data-ttu-id="639f7-103">Konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="639f7-103">Configure and restore from Azure SQL Database long-term backup retention</span></span>

<span data-ttu-id="639f7-104">Můžete nakonfigurovat na trezor služeb zotavení Azure uložte zálohy databáze Azure SQL a poté obnovte databázi pomocí zálohy uchovávány v úložišti pomocí portálu Azure nebo Powershellu.</span><span class="sxs-lookup"><span data-stu-id="639f7-104">You can configure the Azure Recovery Services vault to store Azure SQL database backups and then recover a database using backups retained in the vault using the Azure portal or PowerShell.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="639f7-105">portál Azure</span><span class="sxs-lookup"><span data-stu-id="639f7-105">Azure portal</span></span>

<span data-ttu-id="639f7-106">Následující části ukazují, jak pomocí portálu Azure můžete nakonfigurovat trezor služeb zotavení Azure, Zobrazit zálohy v trezoru a obnovení z trezoru.</span><span class="sxs-lookup"><span data-stu-id="639f7-106">The following sections show you how to use the Azure portal to configure the Azure Recovery Services vault, view backups in the vault, and restore from the vault.</span></span>

### <a name="configure-the-vault-register-the-server-and-select-databases"></a><span data-ttu-id="639f7-107">Konfigurace trezoru, registraci serveru a vyberte databáze</span><span class="sxs-lookup"><span data-stu-id="639f7-107">Configure the vault, register the server, and select databases</span></span>

<span data-ttu-id="639f7-108">Můžete [nakonfigurovat trezoru služeb zotavení Azure uchovávat automatizované zálohování](sql-database-long-term-retention.md) po dobu delší než doba uchování pro vaše vrstvu služeb.</span><span class="sxs-lookup"><span data-stu-id="639f7-108">You [configure an Azure Recovery Services vault to retain automated backups](sql-database-long-term-retention.md) for a period longer than the retention period for your service tier.</span></span> 

1. <span data-ttu-id="639f7-109">Otevřete **systému SQL Server** stránka serveru.</span><span class="sxs-lookup"><span data-stu-id="639f7-109">Open the **SQL Server** page for your server.</span></span>

   ![stránka serveru SQL](./media/sql-database-get-started-portal/sql-server-blade.png)

2. <span data-ttu-id="639f7-111">Klikněte na **Dlouhodobé uchovávání záloh**.</span><span class="sxs-lookup"><span data-stu-id="639f7-111">Click **Long-term backup retention**.</span></span>

   ![odkaz na dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. <span data-ttu-id="639f7-113">Na **dlouhodobé uchovávání záloh** stránky pro váš server, přečtěte si a přijměte podmínky verze preview (Pokud budete mít neudělali - nebo tato funkce je už ve verzi preview).</span><span class="sxs-lookup"><span data-stu-id="639f7-113">On the **Long-term backup retention** page for your server, review and accept the preview terms (unless you have already done so - or this feature is no longer in preview).</span></span>

   ![přijetí podmínek verze preview](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. <span data-ttu-id="639f7-115">Konfigurace dlouhodobé uchovávání záloh, vyberte tuto databázi v mřížce a pak klikněte na tlačítko **konfigurace** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="639f7-115">To configure long-term backup retention, select that database in the grid and then click **Configure** on the toolbar.</span></span>

   ![výběr databáze pro dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. <span data-ttu-id="639f7-117">Na **konfigurace** klikněte na tlačítko **konfigurovat požadované nastavení** pod **trezoru služby zotavení**.</span><span class="sxs-lookup"><span data-stu-id="639f7-117">On the **Configure** page, click **Configure required settings** under **Recovery service vault**.</span></span>

   ![odkaz na konfiguraci trezoru](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. <span data-ttu-id="639f7-119">Na **trezoru služeb zotavení** vyberte existující trezor, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="639f7-119">On the **Recovery services vault** page, select an existing vault, if any.</span></span> <span data-ttu-id="639f7-120">Pokud se pro vaše předplatné nenašel žádný trezor služby Recovery Services, kliknutím proces ukončete a vytvořte trezor služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="639f7-120">Otherwise, if no recovery services vault found for your subscription, click to exit the flow and create a recovery services vault.</span></span>

   ![Vytvoření trezoru propojení](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. <span data-ttu-id="639f7-122">Na **trezory služeb zotavení** klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="639f7-122">On the **Recovery Services vaults** page, click **Add**.</span></span>

   ![Přidat odkaz na trezoru](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. <span data-ttu-id="639f7-124">Na **trezor služeb zotavení** stránky, zadejte platný název pro trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="639f7-124">On the **Recovery Services vault** page, provide a valid name for the Recovery Services vault.</span></span>

   ![název nového trezoru](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. <span data-ttu-id="639f7-126">Vyberte svoje předplatné a skupinu prostředků a potom vyberte umístění trezoru.</span><span class="sxs-lookup"><span data-stu-id="639f7-126">Select your subscription and resource group, and then select the location for the vault.</span></span> <span data-ttu-id="639f7-127">Až budete hotovi, klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="639f7-127">When done, click **Create**.</span></span>

   ![Vytvoření trezoru](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > <span data-ttu-id="639f7-129">Trezor musí být umístěn ve stejné oblasti Azure jako logický server SQL Azure a musí používat stejnou skupinu prostředků jako logický server.</span><span class="sxs-lookup"><span data-stu-id="639f7-129">The vault must be located in the same region as the Azure SQL logical server, and must use the same resource group as the logical server.</span></span>
   >

10. <span data-ttu-id="639f7-130">Po vytvoření nového trezoru provést nezbytné kroky se vrátíte do **trezoru služeb zotavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="639f7-130">After the new vault is created, execute the necessary steps to return to the **Recovery services vault** page.</span></span>

11. <span data-ttu-id="639f7-131">Na **trezoru služeb zotavení** , kliknutím na trezor a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="639f7-131">On the **Recovery services vault** page, click the vault and then click **Select**.</span></span>

   ![výběr existujícího trezoru](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. <span data-ttu-id="639f7-133">Na **konfigurace** stránky, zadejte platný název pro nové zásady uchovávání informací, úpravy výchozích zásad uchovávání informací podle potřeby a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="639f7-133">On the **Configure** page, provide a valid name for the new retention policy, modify the default retention policy as appropriate, and then click **OK**.</span></span>

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. <span data-ttu-id="639f7-135">Na **dlouhodobé uchovávání záloh** stránky pro vaši databázi, klikněte na tlačítko **Uložit** a pak klikněte na **OK** uplatňovat zásady dlouhodobé uchovávání záloh na všech vybraných databázích.</span><span class="sxs-lookup"><span data-stu-id="639f7-135">On the **Long-term backup retention** page for your database, click **Save** and then click **OK** to apply the long-term backup retention policy to all selected databases.</span></span>

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. <span data-ttu-id="639f7-137">Kliknutím na **Uložit** povolte nakonfigurované dlouhodobé uchovávání záloh v trezoru služby Azure Recovery Services pomocí této nové zásady.</span><span class="sxs-lookup"><span data-stu-id="639f7-137">Click **Save** to enable long-term backup retention using this new policy to the Azure Recovery Services vault that you configured.</span></span>

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> <span data-ttu-id="639f7-139">Po nakonfigurování se zálohy v trezoru objeví během příštích sedm dnů.</span><span class="sxs-lookup"><span data-stu-id="639f7-139">Once configured, backups show up in the vault within next seven days.</span></span> <span data-ttu-id="639f7-140">Nepokračujte v tomto kurzu, dokud se zálohy neobjeví v trezoru.</span><span class="sxs-lookup"><span data-stu-id="639f7-140">Do not continue this tutorial until backups show up in the vault.</span></span>
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a><span data-ttu-id="639f7-141">Zobrazení záloh v dlouhodobé uchovávání pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="639f7-141">View backups in long-term retention using Azure portal</span></span>

<span data-ttu-id="639f7-142">Zobrazit informace o zálohování databáze v [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="639f7-142">View information about your database backups in [long-term backup retention](sql-database-long-term-retention.md).</span></span> 

1. <span data-ttu-id="639f7-143">Na portálu Azure otevřete svůj trezor služeb zotavení Azure pro zálohování databáze (přejděte na **všechny prostředky** a vyberte ji ze seznamu prostředků pro vaše předplatné) zobrazíte velikost úložiště používané v zálohování databáze trezor.</span><span class="sxs-lookup"><span data-stu-id="639f7-143">In the Azure portal, open your Azure Recovery Services vault for your database backups (go to **All resources** and select it from the list of resources for your subscription) to view the amount of storage used by your database backups in the vault.</span></span>

   ![zobrazení trezoru služby recovery services se zálohami](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. <span data-ttu-id="639f7-145">Otevřete **databáze SQL** stránky pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="639f7-145">Open the **SQL database** page for your database.</span></span>

   ![Nová stránka ukázkové databáze](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. <span data-ttu-id="639f7-147">Na panelu nástrojů klikněte na **Obnovit**.</span><span class="sxs-lookup"><span data-stu-id="639f7-147">On the toolbar, click **Restore**.</span></span>

   ![panel nástrojů – obnovit](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. <span data-ttu-id="639f7-149">Na stránce obnovení klikněte na **dlouhodobé**.</span><span class="sxs-lookup"><span data-stu-id="639f7-149">On the Restore page, click **Long-term**.</span></span>

5. <span data-ttu-id="639f7-150">V části Zálohy v trezoru Azure klikněte na **Zvolit zálohu** a zobrazte dostupné zálohy databáze v rámci dlouhodobého uchovávání záloh.</span><span class="sxs-lookup"><span data-stu-id="639f7-150">Under Azure vault backups, click **Select a backup** to view the available database backups in long-term backup retention.</span></span>

   ![zálohy v trezoru](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-the-azure-portal"></a><span data-ttu-id="639f7-152">Obnovte databázi ze zálohy v dlouhodobé uchovávání záloh pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="639f7-152">Restore a database from a backup in long-term backup retention using the Azure portal</span></span>

<span data-ttu-id="639f7-153">Obnovení databáze pro novou databázi ze zálohy v trezoru služeb zotavení Azure.</span><span class="sxs-lookup"><span data-stu-id="639f7-153">You restore the database to a new database from a backup in the Azure Recovery Services vault.</span></span>

1. <span data-ttu-id="639f7-154">Na **Azure trezoru záloh** stránky, klikněte na zálohu k obnovení a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="639f7-154">On the **Azure vault backups** page, click the backup to restore and then click **Select**.</span></span>

   ![výběr zálohy v trezoru](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. <span data-ttu-id="639f7-156">Do textového pole **Název databáze** zadejte název obnovené databáze.</span><span class="sxs-lookup"><span data-stu-id="639f7-156">In the **Database name** text box, provide the name for the restored database.</span></span>

   ![nový název databáze](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. <span data-ttu-id="639f7-158">Kliknutím na **OK** obnovte databázi ze zálohy v trezoru do nové databáze.</span><span class="sxs-lookup"><span data-stu-id="639f7-158">Click **OK** to restore your database from the backup in the vault to the new database.</span></span>

4. <span data-ttu-id="639f7-159">Pokud chcete zobrazit stav úlohy obnovení, na panelu nástrojů klikněte na ikonu oznámení.</span><span class="sxs-lookup"><span data-stu-id="639f7-159">On the toolbar, click the notification icon to view the status of the restore job.</span></span>

   ![průběh úlohy obnovení z trezoru](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. <span data-ttu-id="639f7-161">Po dokončení úlohy obnovení, otevřete **databází SQL** zobrazit nově obnovenou databázi.</span><span class="sxs-lookup"><span data-stu-id="639f7-161">When the restore job is completed, open the **SQL databases** page to view the newly restored database.</span></span>

   ![obnovená databáze z trezoru](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> <span data-ttu-id="639f7-163">Odtud se můžete pomocí aplikace SQL Server Management Studio připojit k obnovené databázi a provádět požadované úlohy, jako je například [extrakce části dat z obnovené databáze a zkopírování do existující databáze nebo odstranění existující databáze a přejmenování obnovené databáze na název existující databáze](sql-database-recovery-using-backups.md#point-in-time-restore).</span><span class="sxs-lookup"><span data-stu-id="639f7-163">From here, you can connect to the restored database using SQL Server Management Studio to perform needed tasks, such as to [extract a bit of data from the restored database to copy into the existing database or to delete the existing database and rename the restored database to the existing database name](sql-database-recovery-using-backups.md#point-in-time-restore).</span></span>
>

## <a name="powershell"></a><span data-ttu-id="639f7-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="639f7-164">PowerShell</span></span>

<span data-ttu-id="639f7-165">Následující části ukazují, jak pomocí prostředí PowerShell nakonfigurovat trezor služeb zotavení Azure, zobrazení v trezoru a obnovení z trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="639f7-165">The following sections show you how to use PowerShell to configure the Azure Recovery Services vault, view backups in the vault, and restore from the vault.</span></span>

### <a name="create-a-recovery-services-vault"></a><span data-ttu-id="639f7-166">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="639f7-166">Create a recovery services vault</span></span>

<span data-ttu-id="639f7-167">Použití [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) k vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="639f7-167">Use the [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) to create a recovery services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="639f7-168">Trezor musí být umístěn ve stejné oblasti Azure jako logický server SQL Azure a musí používat stejnou skupinu prostředků jako logický server.</span><span class="sxs-lookup"><span data-stu-id="639f7-168">The vault must be located in the same region as the Azure SQL logical server, and must use the same resource group as the logical server.</span></span>

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-for-its-long-term-retention-backups"></a><span data-ttu-id="639f7-169">Nastavení serveru na použití obnovení trezoru pro jeho dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="639f7-169">Set your server to use the recovery vault for its long-term retention backups</span></span>

<span data-ttu-id="639f7-170">Použití [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) rutiny přidružit trezoru služeb zotavení dříve vytvořenou pro konkrétní server Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="639f7-170">Use the [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet to associate a previously created recovery services vault with a specific Azure SQL server.</span></span>

```PowerShell
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a><span data-ttu-id="639f7-171">Vytvoření zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="639f7-171">Create a retention policy</span></span>

<span data-ttu-id="639f7-172">Zásada uchovávání informací stanoví, kde můžete nastavit dobu uchování zálohy databáze.</span><span class="sxs-lookup"><span data-stu-id="639f7-172">A retention policy is where you set how long to keep a database backup.</span></span> <span data-ttu-id="639f7-173">Použít [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) rutiny výchozí zásady uchovávání informací použít jako šablonu pro vytváření zásad.</span><span class="sxs-lookup"><span data-stu-id="639f7-173">Use the [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet to get the default retention policy to use as the template for creating policies.</span></span> <span data-ttu-id="639f7-174">V této šabloně je nastavit dobu uchování na 2 roky.</span><span class="sxs-lookup"><span data-stu-id="639f7-174">In this template, the retention period is set for 2 years.</span></span> <span data-ttu-id="639f7-175">Potom spustíte [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) nakonec vytvoření zásad.</span><span class="sxs-lookup"><span data-stu-id="639f7-175">Next, run the [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) to finally create the policy.</span></span> 

> [!NOTE]
> <span data-ttu-id="639f7-176">Některé rutiny vyžadují, abyste nastavili trezoru rámci dřív, než spustíte ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) tak, aby zobrazila tato rutina v několika související fragmenty kódu.</span><span class="sxs-lookup"><span data-stu-id="639f7-176">Some cmdlets require that you set the vault context before running ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) so you see this cmdlet in a few related snippets.</span></span> <span data-ttu-id="639f7-177">Můžete nastavit kontext, protože zásady je součástí trezoru.</span><span class="sxs-lookup"><span data-stu-id="639f7-177">You set the context because the policy is part of the vault.</span></span> <span data-ttu-id="639f7-178">Můžete vytvořit více zásad uchovávání pro každý trezor a pak použít požadovanou zásadu u konkrétních databází.</span><span class="sxs-lookup"><span data-stu-id="639f7-178">You can create multiple retention policies for each vault and then apply the desired policy to specific databases.</span></span> 


```PowerShell
# Retrieve the default retention policy for the AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention value to two years (you can set to any time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-to-use-the-previously-defined-retention-policy"></a><span data-ttu-id="639f7-179">Konfigurace databáze na používání dříve definované zásady uchovávání</span><span class="sxs-lookup"><span data-stu-id="639f7-179">Configure a database to use the previously defined retention policy</span></span>

<span data-ttu-id="639f7-180">Použití [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) rutiny použijí nové zásady k určité databázi.</span><span class="sxs-lookup"><span data-stu-id="639f7-180">Use the [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet to apply the new policy to a specific database.</span></span>

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a><span data-ttu-id="639f7-181">Zobrazení informací o záloze a záloh v rámci dlouhodobého uchovávání</span><span class="sxs-lookup"><span data-stu-id="639f7-181">View backup info, and backups in long-term retention</span></span>

<span data-ttu-id="639f7-182">Zobrazit informace o zálohování databáze v [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="639f7-182">View information about your database backups in [long-term backup retention](sql-database-long-term-retention.md).</span></span> 

<span data-ttu-id="639f7-183">Chcete-li zobrazit informace o zálohování pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="639f7-183">Use the following cmdlets to view backup information:</span></span>

- [<span data-ttu-id="639f7-184">Get-AzureRmRecoveryServicesBackupContainer</span><span class="sxs-lookup"><span data-stu-id="639f7-184">Get-AzureRmRecoveryServicesBackupContainer</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [<span data-ttu-id="639f7-185">Get-AzureRmRecoveryServicesBackupItem</span><span class="sxs-lookup"><span data-stu-id="639f7-185">Get-AzureRmRecoveryServicesBackupItem</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [<span data-ttu-id="639f7-186">Get-AzureRmRecoveryServicesBackupRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="639f7-186">Get-AzureRmRecoveryServicesBackupRecoveryPoint</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

```PowerShell
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set the vault context to the vault we want to restore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# the following commands find the container associated with the server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore

# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a><span data-ttu-id="639f7-187">Obnovení databáze ze zálohy v rámci dlouhodobého uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="639f7-187">Restore a database from a backup in long-term backup retention</span></span>

<span data-ttu-id="639f7-188">Obnovení ze zálohy dlouhodobé uchovávání používá [obnovení-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) rutiny.</span><span class="sxs-lookup"><span data-stu-id="639f7-188">Restoring from long-term backup retention uses the [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.</span></span>

```PowerShell
# Restore the most recent backup: $availableBackups[0]
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
> <span data-ttu-id="639f7-189">Tady můžete připojit k obnovené databázi pomocí SQL Server Management Studio potřebné úkoly, například za účelem extrahování bit dat z obnovené databáze chcete zkopírovat do existující databáze nebo odstranit existující databázi a přejmenujte obnovenou databáze na název existující databáze.</span><span class="sxs-lookup"><span data-stu-id="639f7-189">From here, you can connect to the restored database using SQL Server Management Studio to perform needed tasks, such as to extract a bit of data from the restored database to copy into the existing database or to delete the existing database and rename the restored database to the existing database name.</span></span> <span data-ttu-id="639f7-190">V tématu [bodu v době obnovení](sql-database-recovery-using-backups.md#point-in-time-restore).</span><span class="sxs-lookup"><span data-stu-id="639f7-190">See [point in time restore](sql-database-recovery-using-backups.md#point-in-time-restore).</span></span>

## <a name="next-steps"></a><span data-ttu-id="639f7-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="639f7-191">Next steps</span></span>

- <span data-ttu-id="639f7-192">Další informace o automatických zálohách generovaných službou najdete u popisu [automatických záloh](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="639f7-192">To learn about service-generated automatic backups, see [automatic backups](sql-database-automated-backups.md)</span></span>
- <span data-ttu-id="639f7-193">Další informace o dlouhodobém uchovávání záloh najdete v části [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md)</span><span class="sxs-lookup"><span data-stu-id="639f7-193">To learn about long-term backup retention, see [long-term backup retention](sql-database-long-term-retention.md)</span></span>
- <span data-ttu-id="639f7-194">Další informace o obnovování ze záloh najdete v části [obnovení ze zálohy](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="639f7-194">To learn about restoring from backups, see [restore from backup](sql-database-recovery-using-backups.md)</span></span>
