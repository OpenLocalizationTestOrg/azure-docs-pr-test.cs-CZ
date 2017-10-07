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
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a>Konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh

Můžete nakonfigurovat trezoru služeb zotavení Azure hello, toostore zálohy databáze Azure SQL a potom obnovit databázi pomocí zálohy uchovávány v hello trezoru pomocí hello portál Azure nebo PowerShell.

## <a name="azure-portal"></a>portál Azure

Následující části zobrazit jak toouse hello Azure portálu tooconfigure hello služeb zotavení Azure trezoru, Zobrazit zálohy v trezoru hello a obnovení z trezoru hello Hello.

### <a name="configure-hello-vault-register-hello-server-and-select-databases"></a>Konfigurace hello trezoru, zaregistrujte hello server a vybrat databáze

Můžete [konfigurace zálohování tooretain automatizované trezoru služeb zotavení Azure](sql-database-long-term-retention.md) po dobu delší než doba uchování hello vaší vrstvy služeb. 

1. Otevřete hello **systému SQL Server** stránka serveru.

   ![stránka serveru SQL](./media/sql-database-get-started-portal/sql-server-blade.png)

2. Klikněte na **Dlouhodobé uchovávání záloh**.

   ![odkaz na dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. Na hello **dlouhodobé uchovávání záloh** stránky pro váš server, přečtěte si a přijměte podmínky verze preview hello (Pokud budete mít neudělali - nebo tato funkce je už ve verzi preview).

   ![Přijměte podmínky verze preview hello](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Vyberte tuto databázi v mřížce hello tooconfigure dlouhodobé uchovávání záloh a potom klikněte na **konfigurace** na panelu nástrojů hello.

   ![výběr databáze pro dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. Na hello **konfigurace** klikněte na tlačítko **konfigurovat požadované nastavení** pod **trezoru služby zotavení**.

   ![odkaz na konfiguraci trezoru](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. Na hello **trezoru služeb zotavení** vyberte existující trezor, pokud existuje. Pokud žádné trezoru služeb zotavení pro vaše předplatné nalezen, jinak hodnota klikněte tooexit hello toku a vytvoření trezoru služeb zotavení.

   ![Vytvoření trezoru propojení](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. Na hello **trezory služeb zotavení** klikněte na tlačítko **přidat**.

   ![Přidat odkaz na trezoru](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. Na hello **trezor služeb zotavení** stránky, zadejte platný název pro trezor služeb zotavení hello.

   ![název nového trezoru](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Vyberte předplatné a skupina prostředků a potom vyberte hello umístění pro trezor hello. Až budete hotovi, klikněte na **Vytvořit**.

   ![Vytvoření trezoru](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > Hello trezoru se musí nacházet v hello stejné oblasti jako logického serveru Azure SQL hello a musí použít hello stejné skupině prostředků jako hello logického serveru.
   >

10. Po vytvoření nového trezoru hello provést hello potřebné kroky tooreturn toohello **trezoru služeb zotavení** stránky.

11. Na hello **trezoru služeb zotavení** klikněte hello trezoru a pak klikněte na tlačítko **vyberte**.

   ![výběr existujícího trezoru](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. Na hello **konfigurace** stránky, zadejte platný název pro nové zásady uchovávání informací hello, hello výchozí zásady uchovávání informací podle potřeby upravit a pak klikněte na tlačítko **OK**.

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. Na hello **dlouhodobé uchovávání záloh** stránky pro vaši databázi, klikněte na tlačítko **Uložit** a pak klikněte na **OK** tooapply hello tooall zásady dlouhodobé uchovávání záloh, které jsou vybrané databáze.

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Klikněte na tlačítko **Uložit** tooenable dlouhodobé zálohování uchovávání pomocí této nové zásady toohello služeb zotavení Azure trezoru, který jste nakonfigurovali.

   ![definování zásady uchovávání informací](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> Po nakonfigurování zálohování zobrazí v trezoru hello během příštích 7 dnů. V tomto kurzu nepokračujte, dokud zálohování zobrazí v trezoru hello.
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a>Zobrazení záloh v dlouhodobé uchovávání pomocí portálu Azure

Zobrazit informace o zálohování databáze v [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md). 

1. V hello portálu Azure, otevřete svůj trezor služeb zotavení Azure pro zálohování databáze (přejděte příliš**všechny prostředky** a vyberte ho ze seznamu hello prostředky pro vaše předplatné) tooview hello množství úložiště používá databázi Zálohování v trezoru hello.

   ![zobrazení trezoru služby recovery services se zálohami](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Otevřete hello **databáze SQL** stránky pro vaši databázi.

   ![Nová stránka ukázkové databáze](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. Na panelu nástrojů hello, klikněte na tlačítko **obnovení**.

   ![panel nástrojů – obnovit](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Na stránce hello obnovení, klikněte na **dlouhodobé**.

5. V oblasti Azure trezoru záloh, klikněte na **vyberte zálohu** tooview hello k dispozici databáze záloh v dlouhodobé uchovávání záloh.

   ![zálohy v trezoru](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-hello-azure-portal"></a>Obnovte databázi ze zálohy v dlouhodobé uchovávání záloh pomocí hello portálu Azure

Hello databáze tooa novou databázi můžete obnovit ze zálohy v trezoru služeb zotavení Azure hello.

1. Na hello **Azure trezoru záloh** klikněte hello zálohování toorestore a pak klikněte na tlačítko **vyberte**.

   ![výběr zálohy v trezoru](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. V hello **název databáze** textové pole, zadejte název hello hello obnovit databáze.

   ![nový název databáze](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Klikněte na tlačítko **OK** toorestore databáze ze zálohy hello v hello trezoru toohello novou databázi.

4. Na panelu nástrojů hello klikněte na ikonu oznámení hello tooview hello stav úlohy obnovení hello.

   ![průběh úlohy obnovení z trezoru](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Po dokončení úlohy obnovení hello otevřete hello **databází SQL** stránky tooview hello nově obnovit databázi.

   ![obnovená databáze z trezoru](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Tady můžete připojit toohello obnovit databáze pomocí SQL Server Management Studio tooperform potřebné úkoly, jako třeba příliš[extrahovat bit dat z databáze toocopy hello obnovit do hello existující databázi nebo existující toodelete hello Název databáze a přejmenování hello obnovit databáze toohello existující databáze](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="powershell"></a>PowerShell

Hello následující části ukazují, jak zobrazit zálohy v trezoru hello toouse prostředí PowerShell tooconfigure hello služeb zotavení Azure trezoru a obnovení z trezoru hello.

### <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru služby Recovery Services

Použití hello [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate obnovení služeb trezoru.

> [!IMPORTANT]
> Hello trezoru se musí nacházet v hello stejné oblasti jako logického serveru Azure SQL hello a musí použít hello stejné skupině prostředků jako hello logického serveru.

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-toouse-hello-recovery-vault-for-its-long-term-retention-backups"></a>Nastavení trezoru server hello toouse obnovení pro jeho dlouhodobé uchovávání záloh

Použití hello [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) rutiny tooassociate dříve vytvořený trezor služeb zotavení s konkrétní server Azure SQL.

```PowerShell
# Set your server toouse hello vault toofor long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Vytvoření zásady uchovávání informací

Zásady uchovávání informací je, kde můžete nastavit dobu tookeep zálohu databáze. Použití hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) rutiny tooget hello výchozí uchování zásad toouse jako hello šablonu pro vytváření zásad. V této šabloně dobu uchování hello nastavena na 2 roky. Potom spustíte hello [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally vytvořit zásadu hello. 

> [!NOTE]
> Některé rutiny vyžadují, abyste nastavili hello trezoru kontextu dřív, než spustíte ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) tak, aby zobrazila tato rutina v několika související fragmenty kódu. Můžete nastavit hello kontextu, protože zásady hello je součástí hello trezoru. Můžete vytvořit více zásady uchovávání informací pro každý trezor a pak aplikovat hello požadovaných zásad toospecific databáze. 


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

### <a name="configure-a-database-toouse-hello-previously-defined-retention-policy"></a>Nakonfigurujte zásady uchovávání informací hello předtím definovaný toouse databáze

Použití hello [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) rutiny tooapply hello nové zásady tooa konkrétní databáze.

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a>Zobrazení informací o záloze a záloh v rámci dlouhodobého uchovávání

Zobrazit informace o zálohování databáze v [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md). 

Použijte následující informace o zálohování tooview rutiny hello:

- [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [Get-AzureRmRecoveryServicesBackupRecoveryPoint](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

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

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Obnovení databáze ze zálohy v rámci dlouhodobého uchovávání záloh

Obnovení ze zálohy dlouhodobé uchovávání používá hello [obnovení-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) rutiny.

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
> Zde se můžete připojit toohello obnovit databázi pomocí aplikace SQL Server Management Studio tooperform potřebné úkoly, jako je například tooextract bit dat z hello obnovit databáze toocopy do hello existující databázi nebo existující databázi toodelete hello a přejmenování Hello obnovené databáze toohello název existující databáze. V tématu [bodu v době obnovení](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Další kroky

- toolearn o generované služby Automatické zálohování, najdete v části [automatické zálohování](sql-database-automated-backups.md)
- toolearn o dlouhodobé uchovávání záloh, najdete v části [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md)
- toolearn o obnovení ze zálohy, najdete v části [obnovit ze zálohy](sql-database-recovery-using-backups.md)
