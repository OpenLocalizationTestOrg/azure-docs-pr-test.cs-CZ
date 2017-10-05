---
title: "Uložte zálohy databáze SQL Azure pro až 10 let | Microsoft Docs"
description: "Zjistěte, jak Azure SQL Database podporuje ukládání záloh až 10 let."
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 25e651203f804fbf32d632b5f83145a3f3f72a7f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a><span data-ttu-id="4bf5b-103">Uložte zálohy databáze SQL Azure až 10 let.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-103">Store Azure SQL Database backups for up to 10 years</span></span>
<span data-ttu-id="4bf5b-104">Mnoho aplikací mít regulačních, dodržování předpisů nebo jiné obchodní účely, které vyžadují, abyste uchování záloh databáze nad rámec 7-35 dní, poskytuje Azure SQL Database [automatické zálohování](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-104">Many applications have regulatory, compliance, or other business purposes that require you to retain database backups beyond the 7-35 days provided by Azure SQL Database [automatic backups](sql-database-automated-backups.md).</span></span> <span data-ttu-id="4bf5b-105">Pomocí funkce dlouhodobé uchovávání záloh, můžete uložit zálohování databáze SQL v trezoru služeb zotavení Azure až 10 let.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-105">By using the long-term backup retention feature, you can store your SQL database backups in an Azure Recovery Services vault for up to 10 years.</span></span> <span data-ttu-id="4bf5b-106">Můžete uložit až 1 000 databází na jeden trezor.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-106">You can store up to 1,000 databases per vault.</span></span> <span data-ttu-id="4bf5b-107">Pak můžete vybrat jakékoli zálohy v trezoru obnovit jako novou databázi.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-107">You then can select any backup in the vault to restore it as a new database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4bf5b-108">Dlouhodobé uchovávání záloh je momentálně ve verzi preview a je k dispozici v následujících oblastech: Austrálie – východ, Austrálie – jihovýchod, Brazílie – Jih, střed USA, východní Asie, východní USA, Východ USA 2, Indie – střed, Indie – Jih, Japonsko – východ, Japonsko – Západ, Sever střední USA, severní Evropa, střed USA – Jih, jihovýchodní Asie, západní Evropa a západní USA.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-108">Long-term backup retention is currently in preview and available in the following regions: Australia East, Australia Southeast, Brazil South, Central US, East Asia, East US, East US 2, India Central, India South, Japan East, Japan West, North Central US, North Europe, South Central US, Southeast Asia, West Europe, and West US.</span></span>
>

> [!NOTE]
> <span data-ttu-id="4bf5b-109">Až 200 databáze jednomu trezoru můžete povolit v období 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-109">You can enable up to 200 databases per vault during a 24-hour period.</span></span> <span data-ttu-id="4bf5b-110">Doporučujeme použít samostatné úložiště pro každý server pro minimalizaci dopadů toto omezení.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-110">We recommend that you use a separate vault for each server to minimize the impact of this limit.</span></span> 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a><span data-ttu-id="4bf5b-111">Jak funguje dlouhodobé uchovávání záloh databáze SQL</span><span class="sxs-lookup"><span data-stu-id="4bf5b-111">How SQL Database long-term backup retention works</span></span>

<span data-ttu-id="4bf5b-112">S dlouhodobé uchovávání záloh můžete databázový server SQL přidružit trezoru služeb zotavení Azure.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-112">With long-term backup retention, you can associate a SQL database server with an Azure Recovery Services vault.</span></span> 

* <span data-ttu-id="4bf5b-113">Ve stejném předplatném Azure, který vytvořili systému SQL server a ve stejné zeměpisné oblasti a skupina prostředků je třeba vytvořit trezor.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-113">You must create the vault in the same Azure subscription that created the SQL server and in the same geographic region and resource group.</span></span> 
* <span data-ttu-id="4bf5b-114">Nakonfigurujete zásady uchovávání informací pro všechny databáze.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-114">You then configure a retention policy for any database.</span></span> <span data-ttu-id="4bf5b-115">Zásady způsobí, že týdenní zálohy databáze úplné zkopírován do trezoru služeb zotavení a uchovávají po dobu uchovávání (až 10 let).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-115">The policy causes the weekly full database backups to be copied to the Recovery Services vault and retained for the specified retention period (up to 10 years).</span></span> 
* <span data-ttu-id="4bf5b-116">Potom můžete obnovit databázi z jakéhokoli z těchto zálohování pro novou databázi v libovolném serveru v odběru.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-116">You can then restore the database from any of these backups to a new database in any server in the subscription.</span></span> <span data-ttu-id="4bf5b-117">Úložiště Azure vytvoří kopii z existující zálohy a o kopírování nemá žádný vliv výkon na existující databázi.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-117">Azure storage creates a copy from existing backups, and the copy has no performance impact on the existing database.</span></span>

> [!TIP]
> <span data-ttu-id="4bf5b-118">Postupy: informace najdete v tématu [konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-118">For a how-to guide, see [Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="enable-long-term-backup-retention"></a><span data-ttu-id="4bf5b-119">Povolit dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="4bf5b-119">Enable long-term backup retention</span></span>

<span data-ttu-id="4bf5b-120">Postup konfigurace dlouhodobé uchovávání záloh pro databázi:</span><span class="sxs-lookup"><span data-stu-id="4bf5b-120">To configure long-term backup retention for a database:</span></span>

1. <span data-ttu-id="4bf5b-121">Vytvoření trezoru služeb zotavení Azure služby ve stejné oblasti, předplatné a skupina prostředků jako databázový server SQL.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-121">Create an Azure Recovery Services vault in the same region, subscription, and resource group as your SQL database server.</span></span> 
2. <span data-ttu-id="4bf5b-122">Registraci serveru do trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-122">Register the server to the vault.</span></span>
3. <span data-ttu-id="4bf5b-123">Vytvoření zásady ochrany služeb zotavení Azure.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-123">Create an Azure Recovery Services protection policy.</span></span>
4. <span data-ttu-id="4bf5b-124">Použijte zásady ochrany pro databáze, které vyžadují dlouhodobé uchovávání záloh.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-124">Apply the protection policy to the databases that require long-term backup retention.</span></span>

<span data-ttu-id="4bf5b-125">Ke konfiguraci, správě a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure, proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="4bf5b-125">To configure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of the following:</span></span>

* <span data-ttu-id="4bf5b-126">Pomocí portálu Azure: klikněte na tlačítko **dlouhodobé uchovávání záloh**, vyberte databázi a pak klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-126">Using the Azure portal: Click **Long-term backup retention**, select a database, and then click **Configure**.</span></span> 

   ![Vyberte databázi pro dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* <span data-ttu-id="4bf5b-128">Pomocí prostředí PowerShell: Přejděte na [konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-128">Using PowerShell: Go to [Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="restore-a-database-thats-stored-with-the-long-term-backup-retention-feature"></a><span data-ttu-id="4bf5b-129">Obnovit databázi, která je uložena s funkci dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="4bf5b-129">Restore a database that's stored with the long-term backup retention feature</span></span>

<span data-ttu-id="4bf5b-130">Obnovení ze zálohy dlouhodobé uchovávání záloh:</span><span class="sxs-lookup"><span data-stu-id="4bf5b-130">To recover from a long-term backup retention backup:</span></span>

1. <span data-ttu-id="4bf5b-131">Zobrazí seznam v úložišti, kde je uložena záloha.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-131">List the vault where the backup is stored.</span></span>
2. <span data-ttu-id="4bf5b-132">Zobrazí seznam kontejneru, který je namapovaný k logickému serveru.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-132">List the container that is mapped to your logical server.</span></span>
3. <span data-ttu-id="4bf5b-133">Zobrazí seznam zdroj dat v úložišti, který je namapovaný k vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-133">List the data source within the vault that is mapped to your database.</span></span>
4. <span data-ttu-id="4bf5b-134">Zobrazí seznam bodů obnovení, které jsou k dispozici pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-134">List the recovery points that are available to restore.</span></span>
5. <span data-ttu-id="4bf5b-135">Obnovte databázi z bodu obnovení na cílový server v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-135">Restore the database from the recovery point to the target server within your subscription.</span></span>

<span data-ttu-id="4bf5b-136">Ke konfiguraci, správě a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure, proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="4bf5b-136">To configure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of the following:</span></span>

* <span data-ttu-id="4bf5b-137">Pomocí portálu Azure: přejděte na [spravovat pomocí portálu Azure dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-137">Using the Azure portal: Go to [Manage long-term backup retention using the Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="4bf5b-138">Pomocí prostředí PowerShell: Přejděte na [spravovat pomocí prostředí PowerShell dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-138">Using PowerShell: Go to [Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="get-pricing-for-long-term-backup-retention"></a><span data-ttu-id="4bf5b-139">Získat ceny pro dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="4bf5b-139">Get pricing for long-term backup retention</span></span>

<span data-ttu-id="4bf5b-140">Dlouhodobé uchovávání záloh databáze SQL je účtován podle požadavků [služby Azure backup ceny sazby](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-140">Long-term backup retention of a SQL database is charged according to the [Azure backup services pricing rates](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="4bf5b-141">Po databáze serveru SQL je registrovaný k úložišti, vám budou účtovat celkové úložiště, který je používán týdenní zálohy uložené v trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-141">After the SQL database server is registered to the vault, you are charged for the total storage that's used by the weekly backups stored in the vault.</span></span>

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a><span data-ttu-id="4bf5b-142">Zobrazit dostupné zálohy, které jsou uložené v dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="4bf5b-142">View available backups that are stored in long-term backup retention</span></span>

<span data-ttu-id="4bf5b-143">Ke konfiguraci, správě a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure pomocí portálu Azure, proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="4bf5b-143">To configure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault by using the Azure portal, do either of the following:</span></span>

* <span data-ttu-id="4bf5b-144">Pomocí portálu Azure: přejděte na [spravovat pomocí portálu Azure dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-144">Using the Azure portal: Go to [Manage long-term backup retention using the Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="4bf5b-145">Pomocí prostředí PowerShell: Přejděte na [spravovat pomocí prostředí PowerShell dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-145">Using PowerShell: Go to [Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="disable-long-term-retention"></a><span data-ttu-id="4bf5b-146">Zakázat dlouhodobé uchovávání</span><span class="sxs-lookup"><span data-stu-id="4bf5b-146">Disable long-term retention</span></span>

<span data-ttu-id="4bf5b-147">Služba obnovení automaticky zpracovává čištění na základě zásad zadané uchovávání záloh.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-147">The recovery service automatically handles the cleanup of backups based on the provided retention policy.</span></span> 

<span data-ttu-id="4bf5b-148">Chcete-li zastavit odesílání zálohy pro konkrétní databázi do trezoru, odeberte zásady uchovávání informací pro tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-148">To stop sending the backups for a specific database to the vault, remove the retention policy for that database.</span></span>
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> <span data-ttu-id="4bf5b-149">Zálohování, které jsou již v trezoru jsou poškozena.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-149">The backups that are already in the vault are unaffected.</span></span> <span data-ttu-id="4bf5b-150">Jsou automaticky odstraněny službou obnovení po dobu uchování vyprší.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-150">They are automatically deleted by the recovery service when their retention period expires.</span></span>

## <a name="long-term-backup-retention-faq"></a><span data-ttu-id="4bf5b-151">Dlouhodobé uchovávání záloh – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="4bf5b-151">Long-term backup retention FAQ</span></span>

<span data-ttu-id="4bf5b-152">**Můžete ručně odstranit konkrétní zálohy v trezoru?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-152">**Can I manually delete specific backups in the vault?**</span></span>

<span data-ttu-id="4bf5b-153">Aktuálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-153">Not currently.</span></span> <span data-ttu-id="4bf5b-154">Trezor záloh automaticky vyčistí, pokud vypršela doba uchování.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-154">The vault automatically cleans up backups when the retention period has expired.</span></span>

<span data-ttu-id="4bf5b-155">**Můžete zaregistrovat svůj server a uložte zálohy do více než jednoho trezoru?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-155">**Can I register my server to store backups to more than one vault?**</span></span>

<span data-ttu-id="4bf5b-156">Ne, můžete uložit aktuálně pouze jeden trezor záloh v čase.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-156">No, you can currently store backups to only one vault at a time.</span></span>

<span data-ttu-id="4bf5b-157">**Může mít trezoru a server v různých předplatných?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-157">**Can I have a vault and server in different subscriptions?**</span></span>

<span data-ttu-id="4bf5b-158">Ne, aktuálně trezoru a server musí být ve stejném předplatném a skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-158">No, currently the vault and server must be in the same subscription and resource group.</span></span>

<span data-ttu-id="4bf5b-159">**Můžete použít k trezoru, vytvořené v oblasti, která se liší od oblasti svému serveru?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-159">**Can I use a vault that I created in a region that's different from my server’s region?**</span></span>

<span data-ttu-id="4bf5b-160">Ne, trezoru a server musí být ve stejné oblasti minimalizovat dobu kopírování a zamezit tak poplatky za provozu.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-160">No, the vault and server must be in the same region to minimize copy time and avoid traffic charges.</span></span>

<span data-ttu-id="4bf5b-161">**Kolik databáze můžete ukládat do jednoho trezoru?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-161">**How many databases can I store in one vault?**</span></span>

<span data-ttu-id="4bf5b-162">V současné době podporujeme až 1 000 databází na jeden trezor.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-162">Currently, we support up to 1,000 databases per vault.</span></span> 

<span data-ttu-id="4bf5b-163">**Kolik trezorů můžete vytvořit na jedno předplatné?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-163">**How many vaults can I create per subscription?**</span></span>

<span data-ttu-id="4bf5b-164">Můžete vytvořit až pro 25 trezorů na jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-164">You can create up to 25 vaults per subscription.</span></span>

<span data-ttu-id="4bf5b-165">**Kolik databází můžete nakonfigurovat za den za trezoru?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-165">**How many databases can I configure per day per vault?**</span></span>

<span data-ttu-id="4bf5b-166">Můžete nastavit 200 databáze za den za trezor.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-166">You can set up 200 databases per day per vault.</span></span>

<span data-ttu-id="4bf5b-167">**Funguje s elastické fondy dlouhodobé uchovávání záloh?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-167">**Does long-term backup retention work with elastic pools?**</span></span>

<span data-ttu-id="4bf5b-168">Ano.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-168">Yes.</span></span> <span data-ttu-id="4bf5b-169">Všechny databáze ve fondu můžete nakonfigurovat zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-169">Any database in the pool can be configured with the retention policy.</span></span>

<span data-ttu-id="4bf5b-170">**Můžete vybrat v době, kdy je vytvoření zálohy?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-170">**Can I choose the time at which the backup is created?**</span></span>

<span data-ttu-id="4bf5b-171">Ne, databáze SQL určuje plán zálohování pro minimalizaci vlivu na výkon vašich databází.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-171">No, SQL Database controls the backup schedule to minimize the performance impact on your databases.</span></span>

<span data-ttu-id="4bf5b-172">**Je nutné transparentní šifrování dat pro databázi povoleno. Můžete použít ho k trezoru?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-172">**I have transparent data encryption enabled for my database. Can I use it with the vault?**</span></span> 

<span data-ttu-id="4bf5b-173">Ano, je podporováno transparentní šifrování dat.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-173">Yes, transparent data encryption is supported.</span></span> <span data-ttu-id="4bf5b-174">Databázi můžete obnovit z trezoru i v případě, že původní databázi již existuje.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-174">You can restore the database from the vault even if the original database no longer exists.</span></span>

<span data-ttu-id="4bf5b-175">**Co se stane s zálohy v trezoru, pokud je pozastavená Moje předplatné?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-175">**What happens with the backups in the vault if my subscription is suspended?**</span></span> 

<span data-ttu-id="4bf5b-176">Pokud je předplatné pozastavené, jsme zachovat stávající databáze a zálohování.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-176">If your subscription is suspended, we retain the existing databases and backups.</span></span> <span data-ttu-id="4bf5b-177">Nových záloh nejsou zkopírovány do trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-177">New backups are not copied to the vault.</span></span> <span data-ttu-id="4bf5b-178">Po předplatné znovu aktivujete, službu obnoví kopírování zálohování do trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-178">After you reactivate the subscription, the service resumes copying backups to the vault.</span></span> <span data-ttu-id="4bf5b-179">Svůj trezor bude přístupný pro operace obnovení pomocí zálohování, které byly zkopírovány existuje před pozastavením předplatné.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-179">Your vault becomes accessible to the restore operations by using the backups that were copied there before the subscription was suspended.</span></span> 

<span data-ttu-id="4bf5b-180">**Můžete získat přístup k záložní soubory databáze SQL, tak I stáhnout nebo obnovit je do systému SQL server?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-180">**Can I get access to the SQL database backup files so I can download or restore them to the SQL server?**</span></span>

<span data-ttu-id="4bf5b-181">Ne, aktuálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-181">No, not currently.</span></span>

<span data-ttu-id="4bf5b-182">**Je možné, že více plánů (denně, týdně, měsíčně, ročně) v rámci zásady uchovávání informací SQL.**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-182">**Is it possible to have multiple schedules (daily, weekly, monthly, yearly) within a SQL retention policy.**</span></span>

<span data-ttu-id="4bf5b-183">Ne, víc plány jsou aktuálně dostupné jen pro zálohy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-183">No, multiple schedules are currently available only for virtual machine backups.</span></span>

<span data-ttu-id="4bf5b-184">**Co když nastavíme dlouhodobé uchovávání záloh na databázi, která se nachází aktivní geografickou replikací sekundární databáze?**</span><span class="sxs-lookup"><span data-stu-id="4bf5b-184">**What if we set up long-term backup retention on a database that is an active geo-replication secondary database?**</span></span>

<span data-ttu-id="4bf5b-185">Protože jsme nemáte trvat zálohy na replikách, je aktuálně žádná možnost pro dlouhodobé uchovávání zálohování na sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-185">Because we don't take backups on replicas, there is currently no option for long-term backup retention on secondary databases.</span></span> <span data-ttu-id="4bf5b-186">Je ale důležité pro uživatele nastavit dlouhodobé uchovávání záloh na sekundární databázi aktivní geografickou replikaci z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="4bf5b-186">However, it is important for users to set up long-term backup retention on an active geo-replication secondary database for these reasons:</span></span>
* <span data-ttu-id="4bf5b-187">Pokud dojde převzetí služeb při selhání a databáze se stane primární databázi, jsme trvat úplné zálohování, což je nahrán do trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-187">When a failover happens and the database becomes a primary database, we take a full backup, which is uploaded to vault.</span></span>
* <span data-ttu-id="4bf5b-188">Existuje nejsou zpoplatněné zákazník pro nastavení dlouhodobé uchovávání záloh na sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-188">There is no extra cost to the customer for setting up long-term backup retention on a secondary database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4bf5b-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4bf5b-189">Next steps</span></span>
<span data-ttu-id="4bf5b-190">Protože zálohy databáze chránit data před náhodným poškození nebo odstranění, jsou nedílnou součást vámi vyžádaných žádné kontinuity podnikových procesů a strategie zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="4bf5b-190">Because database backups protect data from accidental corruption or deletion, they're an essential part of any business continuity and disaster recovery strategy.</span></span> <span data-ttu-id="4bf5b-191">Další informace o jiných řešení kontinuity podnikových procesů databáze SQL najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="4bf5b-191">To learn about the other SQL Database business-continuity solutions, see [Business continuity overview](sql-database-business-continuity.md).</span></span>
