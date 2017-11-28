---
title: "aaaStore zálohy databáze SQL Azure pro až roky too10 | Microsoft Docs"
description: "Zjistěte, jak Azure SQL Database podporuje ukládání záloh pro až too10 let."
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
ms.openlocfilehash: 5825ebd4e3bd66b59b13aea603d377ef814a1df3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="store-azure-sql-database-backups-for-up-too10-years"></a><span data-ttu-id="46543-103">Uložte zálohy databáze SQL Azure pro až too10 let</span><span class="sxs-lookup"><span data-stu-id="46543-103">Store Azure SQL Database backups for up too10 years</span></span>
<span data-ttu-id="46543-104">Mnoho aplikací mít regulačních, dodržování předpisů nebo jiné obchodní účely, které vyžadují tooretain zálohy databáze nad rámec hello 7-35 dní od Azure SQL Database [automatické zálohování](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="46543-104">Many applications have regulatory, compliance, or other business purposes that require you tooretain database backups beyond hello 7-35 days provided by Azure SQL Database [automatic backups](sql-database-automated-backups.md).</span></span> <span data-ttu-id="46543-105">Pomocí funkce dlouhodobé uchovávání záloh hello můžete uložit zálohování databáze SQL v trezoru služeb zotavení Azure pro až too10 let.</span><span class="sxs-lookup"><span data-stu-id="46543-105">By using hello long-term backup retention feature, you can store your SQL database backups in an Azure Recovery Services vault for up too10 years.</span></span> <span data-ttu-id="46543-106">Můžete uložit až too1 000 databází na jeden trezor.</span><span class="sxs-lookup"><span data-stu-id="46543-106">You can store up too1,000 databases per vault.</span></span> <span data-ttu-id="46543-107">Pak můžete vybrat jakékoli zálohy v trezoru toorestore hello ho jako novou databázi.</span><span class="sxs-lookup"><span data-stu-id="46543-107">You then can select any backup in hello vault toorestore it as a new database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="46543-108">Dlouhodobé uchovávání záloh je momentálně ve verzi preview a je k dispozici v následujících oblastech hello: Austrálie – východ, Austrálie – jihovýchod, Brazílie – Jih, střed USA, východní Asie, východní USA, Východ USA 2, Indie – střed, Indie – Jih, Japonsko – východ, Japonsko – Západ, Sever střední USA, severní Evropa, střed USA – Jih, jihovýchodní Asie, západní Evropa a západní USA.</span><span class="sxs-lookup"><span data-stu-id="46543-108">Long-term backup retention is currently in preview and available in hello following regions: Australia East, Australia Southeast, Brazil South, Central US, East Asia, East US, East US 2, India Central, India South, Japan East, Japan West, North Central US, North Europe, South Central US, Southeast Asia, West Europe, and West US.</span></span>
>

> [!NOTE]
> <span data-ttu-id="46543-109">Zálohu databáze too200 jednomu trezoru můžete povolit v období 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="46543-109">You can enable up too200 databases per vault during a 24-hour period.</span></span> <span data-ttu-id="46543-110">Doporučujeme vám, že používáte samostatné úložiště pro každý server toominimize hello dopad tento limit.</span><span class="sxs-lookup"><span data-stu-id="46543-110">We recommend that you use a separate vault for each server toominimize hello impact of this limit.</span></span> 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a><span data-ttu-id="46543-111">Jak funguje dlouhodobé uchovávání záloh databáze SQL</span><span class="sxs-lookup"><span data-stu-id="46543-111">How SQL Database long-term backup retention works</span></span>

<span data-ttu-id="46543-112">S dlouhodobé uchovávání záloh můžete databázový server SQL přidružit trezoru služeb zotavení Azure.</span><span class="sxs-lookup"><span data-stu-id="46543-112">With long-term backup retention, you can associate a SQL database server with an Azure Recovery Services vault.</span></span> 

* <span data-ttu-id="46543-113">Musíte vytvořit trezor hello v hello stejného předplatného Azure, který vytvořili hello SQL serveru a v hello stejné zeměpisné oblasti nebo skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="46543-113">You must create hello vault in hello same Azure subscription that created hello SQL server and in hello same geographic region and resource group.</span></span> 
* <span data-ttu-id="46543-114">Nakonfigurujete zásady uchovávání informací pro všechny databáze.</span><span class="sxs-lookup"><span data-stu-id="46543-114">You then configure a retention policy for any database.</span></span> <span data-ttu-id="46543-115">Hello zásad příčiny hello týdenní úplná databáze zálohy toobe zkopíruje toohello trezor služeb zotavení a uchovávají po dobu uchovávání hello (až roky too10).</span><span class="sxs-lookup"><span data-stu-id="46543-115">hello policy causes hello weekly full database backups toobe copied toohello Recovery Services vault and retained for hello specified retention period (up too10 years).</span></span> 
* <span data-ttu-id="46543-116">Pak můžete obnovit databáze hello z jakéhokoli z těchto zálohy tooa novou databázi v libovolném serveru v odběru hello.</span><span class="sxs-lookup"><span data-stu-id="46543-116">You can then restore hello database from any of these backups tooa new database in any server in hello subscription.</span></span> <span data-ttu-id="46543-117">Úložiště Azure vytvoří kopii z existující zálohy a kopírování hello nemá žádný vliv výkon na existující databázi hello.</span><span class="sxs-lookup"><span data-stu-id="46543-117">Azure storage creates a copy from existing backups, and hello copy has no performance impact on hello existing database.</span></span>

> [!TIP]
> <span data-ttu-id="46543-118">Jak tooguide, najdete v části [konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="46543-118">For a how-tooguide, see [Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="enable-long-term-backup-retention"></a><span data-ttu-id="46543-119">Povolit dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="46543-119">Enable long-term backup retention</span></span>

<span data-ttu-id="46543-120">tooconfigure dlouhodobé uchovávání záloh pro databázi:</span><span class="sxs-lookup"><span data-stu-id="46543-120">tooconfigure long-term backup retention for a database:</span></span>

1. <span data-ttu-id="46543-121">Vytvoření trezoru služeb zotavení Azure služby v hello stejnou oblast, předplatné a prostředků skupinu jako databázový server SQL.</span><span class="sxs-lookup"><span data-stu-id="46543-121">Create an Azure Recovery Services vault in hello same region, subscription, and resource group as your SQL database server.</span></span> 
2. <span data-ttu-id="46543-122">Registrace trezoru toohello server hello.</span><span class="sxs-lookup"><span data-stu-id="46543-122">Register hello server toohello vault.</span></span>
3. <span data-ttu-id="46543-123">Vytvoření zásady ochrany služeb zotavení Azure.</span><span class="sxs-lookup"><span data-stu-id="46543-123">Create an Azure Recovery Services protection policy.</span></span>
4. <span data-ttu-id="46543-124">Použijte hello ochrany zásad toohello databáze, které vyžadují dlouhodobé uchovávání záloh.</span><span class="sxs-lookup"><span data-stu-id="46543-124">Apply hello protection policy toohello databases that require long-term backup retention.</span></span>

<span data-ttu-id="46543-125">tooconfigure, spravovat a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure, proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="46543-125">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of hello following:</span></span>

* <span data-ttu-id="46543-126">Pomocí portálu Azure hello: klikněte na tlačítko **dlouhodobé uchovávání záloh**, vyberte databázi a pak klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="46543-126">Using hello Azure portal: Click **Long-term backup retention**, select a database, and then click **Configure**.</span></span> 

   ![Vyberte databázi pro dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* <span data-ttu-id="46543-128">Pomocí prostředí PowerShell: Přejděte příliš[konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="46543-128">Using PowerShell: Go too[Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="restore-a-database-thats-stored-with-hello-long-term-backup-retention-feature"></a><span data-ttu-id="46543-129">Obnovit databázi, která je uložena s hello dlouhodobé uchovávání záloh funkcí</span><span class="sxs-lookup"><span data-stu-id="46543-129">Restore a database that's stored with hello long-term backup retention feature</span></span>

<span data-ttu-id="46543-130">toorecover ze zálohování na dlouhodobé uchovávání záloh:</span><span class="sxs-lookup"><span data-stu-id="46543-130">toorecover from a long-term backup retention backup:</span></span>

1. <span data-ttu-id="46543-131">Kde je uložena záloha hello trezor hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="46543-131">List hello vault where hello backup is stored.</span></span>
2. <span data-ttu-id="46543-132">Kontejner hello seznamu, který je namapované tooyour logického serveru.</span><span class="sxs-lookup"><span data-stu-id="46543-132">List hello container that is mapped tooyour logical server.</span></span>
3. <span data-ttu-id="46543-133">Seznam hello zdroj dat v rámci hello trezoru, který je namapované tooyour databáze.</span><span class="sxs-lookup"><span data-stu-id="46543-133">List hello data source within hello vault that is mapped tooyour database.</span></span>
4. <span data-ttu-id="46543-134">Seznam hello bodů obnovení, které jsou k dispozici toorestore.</span><span class="sxs-lookup"><span data-stu-id="46543-134">List hello recovery points that are available toorestore.</span></span>
5. <span data-ttu-id="46543-135">Obnovení databáze hello z hello obnovení bodu toohello cílového serveru v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="46543-135">Restore hello database from hello recovery point toohello target server within your subscription.</span></span>

<span data-ttu-id="46543-136">tooconfigure, spravovat a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure, proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="46543-136">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of hello following:</span></span>

* <span data-ttu-id="46543-137">Pomocí portálu Azure hello: přejděte příliš[spravovat dlouhodobé uchovávání záloh pomocí portálu Azure hello](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="46543-137">Using hello Azure portal: Go too[Manage long-term backup retention using hello Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="46543-138">Pomocí prostředí PowerShell: Přejděte příliš[spravovat pomocí prostředí PowerShell dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="46543-138">Using PowerShell: Go too[Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="get-pricing-for-long-term-backup-retention"></a><span data-ttu-id="46543-139">Získat ceny pro dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="46543-139">Get pricing for long-term backup retention</span></span>

<span data-ttu-id="46543-140">Dlouhodobé uchovávání záloh databáze SQL je účtován podle toohello [služby Azure backup ceny sazby](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="46543-140">Long-term backup retention of a SQL database is charged according toohello [Azure backup services pricing rates](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="46543-141">Po hello server databáze SQL je registrovaný toohello trezoru, vám budou účtovat hello celkové úložiště, který je používán hello týdenní zálohy uložené v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="46543-141">After hello SQL database server is registered toohello vault, you are charged for hello total storage that's used by hello weekly backups stored in hello vault.</span></span>

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a><span data-ttu-id="46543-142">Zobrazit dostupné zálohy, které jsou uložené v dlouhodobé uchovávání záloh</span><span class="sxs-lookup"><span data-stu-id="46543-142">View available backups that are stored in long-term backup retention</span></span>

<span data-ttu-id="46543-143">tooconfigure, spravovat a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure pomocí hello portálu Azure, proveďte jednu z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="46543-143">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault by using hello Azure portal, do either of hello following:</span></span>

* <span data-ttu-id="46543-144">Pomocí portálu Azure hello: přejděte příliš[spravovat dlouhodobé uchovávání záloh pomocí portálu Azure hello](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="46543-144">Using hello Azure portal: Go too[Manage long-term backup retention using hello Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="46543-145">Pomocí prostředí PowerShell: Přejděte příliš[spravovat pomocí prostředí PowerShell dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="46543-145">Using PowerShell: Go too[Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="disable-long-term-retention"></a><span data-ttu-id="46543-146">Zakázat dlouhodobé uchovávání</span><span class="sxs-lookup"><span data-stu-id="46543-146">Disable long-term retention</span></span>

<span data-ttu-id="46543-147">služby zotavení Hello automaticky zpracovává hello čištění záloh podle hello zadané zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="46543-147">hello recovery service automatically handles hello cleanup of backups based on hello provided retention policy.</span></span> 

<span data-ttu-id="46543-148">toostop odesílání hello zálohy pro konkrétní databázi toohello trezoru, odeberte hello zásady uchovávání informací pro tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="46543-148">toostop sending hello backups for a specific database toohello vault, remove hello retention policy for that database.</span></span>
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> <span data-ttu-id="46543-149">Hello zálohování, které jsou již v trezoru hello jsou poškozena.</span><span class="sxs-lookup"><span data-stu-id="46543-149">hello backups that are already in hello vault are unaffected.</span></span> <span data-ttu-id="46543-150">Jsou automaticky odstraněny službou obnovení hello když vyprší platnost dobu uchování.</span><span class="sxs-lookup"><span data-stu-id="46543-150">They are automatically deleted by hello recovery service when their retention period expires.</span></span>

## <a name="long-term-backup-retention-faq"></a><span data-ttu-id="46543-151">Dlouhodobé uchovávání záloh – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="46543-151">Long-term backup retention FAQ</span></span>

<span data-ttu-id="46543-152">**Můžete ručně odstranit konkrétní zálohy v trezoru hello?**</span><span class="sxs-lookup"><span data-stu-id="46543-152">**Can I manually delete specific backups in hello vault?**</span></span>

<span data-ttu-id="46543-153">Aktuálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="46543-153">Not currently.</span></span> <span data-ttu-id="46543-154">Hello trezoru záloh automaticky vyčistí, pokud vypršela doba uchování hello.</span><span class="sxs-lookup"><span data-stu-id="46543-154">hello vault automatically cleans up backups when hello retention period has expired.</span></span>

<span data-ttu-id="46543-155">**Můžete zaregistrovat my server toostore zálohy toomore než jeden trezor?**</span><span class="sxs-lookup"><span data-stu-id="46543-155">**Can I register my server toostore backups toomore than one vault?**</span></span>

<span data-ttu-id="46543-156">Ne, můžete uložit aktuálně jeden trezor záloh tooonly najednou.</span><span class="sxs-lookup"><span data-stu-id="46543-156">No, you can currently store backups tooonly one vault at a time.</span></span>

<span data-ttu-id="46543-157">**Může mít trezoru a server v různých předplatných?**</span><span class="sxs-lookup"><span data-stu-id="46543-157">**Can I have a vault and server in different subscriptions?**</span></span>

<span data-ttu-id="46543-158">Ne, aktuálně hello trezoru a server musí být v hello stejné předplatném nebo skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="46543-158">No, currently hello vault and server must be in hello same subscription and resource group.</span></span>

<span data-ttu-id="46543-159">**Můžete použít k trezoru, vytvořené v oblasti, která se liší od oblasti svému serveru?**</span><span class="sxs-lookup"><span data-stu-id="46543-159">**Can I use a vault that I created in a region that's different from my server’s region?**</span></span>

<span data-ttu-id="46543-160">Ne, hello trezoru a server musí být v hello stejné oblasti toominimize zkopírujte čas a náklady na provoz.</span><span class="sxs-lookup"><span data-stu-id="46543-160">No, hello vault and server must be in hello same region toominimize copy time and avoid traffic charges.</span></span>

<span data-ttu-id="46543-161">**Kolik databáze můžete ukládat do jednoho trezoru?**</span><span class="sxs-lookup"><span data-stu-id="46543-161">**How many databases can I store in one vault?**</span></span>

<span data-ttu-id="46543-162">V současné době podporujeme až too1 000 databází na jeden trezor.</span><span class="sxs-lookup"><span data-stu-id="46543-162">Currently, we support up too1,000 databases per vault.</span></span> 

<span data-ttu-id="46543-163">**Kolik trezorů můžete vytvořit na jedno předplatné?**</span><span class="sxs-lookup"><span data-stu-id="46543-163">**How many vaults can I create per subscription?**</span></span>

<span data-ttu-id="46543-164">Můžete vytvořit až too25 trezory jedno předplatné.</span><span class="sxs-lookup"><span data-stu-id="46543-164">You can create up too25 vaults per subscription.</span></span>

<span data-ttu-id="46543-165">**Kolik databází můžete nakonfigurovat za den za trezoru?**</span><span class="sxs-lookup"><span data-stu-id="46543-165">**How many databases can I configure per day per vault?**</span></span>

<span data-ttu-id="46543-166">Můžete nastavit 200 databáze za den za trezor.</span><span class="sxs-lookup"><span data-stu-id="46543-166">You can set up 200 databases per day per vault.</span></span>

<span data-ttu-id="46543-167">**Funguje s elastické fondy dlouhodobé uchovávání záloh?**</span><span class="sxs-lookup"><span data-stu-id="46543-167">**Does long-term backup retention work with elastic pools?**</span></span>

<span data-ttu-id="46543-168">Ano.</span><span class="sxs-lookup"><span data-stu-id="46543-168">Yes.</span></span> <span data-ttu-id="46543-169">Všechny databáze ve fondu hello se dá nakonfigurovat s hello zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="46543-169">Any database in hello pool can be configured with hello retention policy.</span></span>

<span data-ttu-id="46543-170">**Můžete zvolit hello čas, kdy je vytvořeno hello zálohování?**</span><span class="sxs-lookup"><span data-stu-id="46543-170">**Can I choose hello time at which hello backup is created?**</span></span>

<span data-ttu-id="46543-171">Ne, databáze SQL řídí vlivu na výkon hello toominimize plán zálohování hello vašich databází.</span><span class="sxs-lookup"><span data-stu-id="46543-171">No, SQL Database controls hello backup schedule toominimize hello performance impact on your databases.</span></span>

<span data-ttu-id="46543-172">**Je nutné transparentní šifrování dat pro databázi povoleno. Můžete použít ho k trezoru hello?**</span><span class="sxs-lookup"><span data-stu-id="46543-172">**I have transparent data encryption enabled for my database. Can I use it with hello vault?**</span></span> 

<span data-ttu-id="46543-173">Ano, je podporováno transparentní šifrování dat.</span><span class="sxs-lookup"><span data-stu-id="46543-173">Yes, transparent data encryption is supported.</span></span> <span data-ttu-id="46543-174">Hello databázi lze obnovit z trezoru hello i v případě hello původní databáze již existuje.</span><span class="sxs-lookup"><span data-stu-id="46543-174">You can restore hello database from hello vault even if hello original database no longer exists.</span></span>

<span data-ttu-id="46543-175">**Co se stane s hello zálohy v trezoru hello, pokud je pozastavená Moje předplatné?**</span><span class="sxs-lookup"><span data-stu-id="46543-175">**What happens with hello backups in hello vault if my subscription is suspended?**</span></span> 

<span data-ttu-id="46543-176">Pokud je předplatné pozastavené, jsme zachovat hello existující databáze a zálohování.</span><span class="sxs-lookup"><span data-stu-id="46543-176">If your subscription is suspended, we retain hello existing databases and backups.</span></span> <span data-ttu-id="46543-177">Nových záloh nejsou zkopírovaný toohello trezoru.</span><span class="sxs-lookup"><span data-stu-id="46543-177">New backups are not copied toohello vault.</span></span> <span data-ttu-id="46543-178">Po hello předplatné znovu aktivujete, služba hello obnoví kopírování toohello trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="46543-178">After you reactivate hello subscription, hello service resumes copying backups toohello vault.</span></span> <span data-ttu-id="46543-179">Operace obnovení přístupné toohello pomocí hello zálohování, které byly zkopírovány existuje před pozastavením hello předplatné se změní na svůj trezor.</span><span class="sxs-lookup"><span data-stu-id="46543-179">Your vault becomes accessible toohello restore operations by using hello backups that were copied there before hello subscription was suspended.</span></span> 

<span data-ttu-id="46543-180">**Lze získat přístup záložní soubory databáze SQL toohello tak I stáhnout nebo obnovení je toohello SQL serveru?**</span><span class="sxs-lookup"><span data-stu-id="46543-180">**Can I get access toohello SQL database backup files so I can download or restore them toohello SQL server?**</span></span>

<span data-ttu-id="46543-181">Ne, aktuálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="46543-181">No, not currently.</span></span>

<span data-ttu-id="46543-182">**Je možné toohave vícenásobné plány (denně, týdně, měsíčně, ročně) v rámci zásady uchovávání informací SQL.**</span><span class="sxs-lookup"><span data-stu-id="46543-182">**Is it possible toohave multiple schedules (daily, weekly, monthly, yearly) within a SQL retention policy.**</span></span>

<span data-ttu-id="46543-183">Ne, víc plány jsou aktuálně dostupné jen pro zálohy virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="46543-183">No, multiple schedules are currently available only for virtual machine backups.</span></span>

<span data-ttu-id="46543-184">**Co když nastavíme dlouhodobé uchovávání záloh na databázi, která se nachází aktivní geografickou replikací sekundární databáze?**</span><span class="sxs-lookup"><span data-stu-id="46543-184">**What if we set up long-term backup retention on a database that is an active geo-replication secondary database?**</span></span>

<span data-ttu-id="46543-185">Protože jsme nemáte trvat zálohy na replikách, je aktuálně žádná možnost pro dlouhodobé uchovávání zálohování na sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="46543-185">Because we don't take backups on replicas, there is currently no option for long-term backup retention on secondary databases.</span></span> <span data-ttu-id="46543-186">Ale je důležité pro uživatele tooset až dlouhodobé uchovávání záloh na sekundární databázi aktivní geografickou replikaci z těchto důvodů:</span><span class="sxs-lookup"><span data-stu-id="46543-186">However, it is important for users tooset up long-term backup retention on an active geo-replication secondary database for these reasons:</span></span>
* <span data-ttu-id="46543-187">Pokud dojde převzetí služeb při selhání a hello databáze se stane primární databázi, jsme trvat úplné zálohování, který je nahraný toovault.</span><span class="sxs-lookup"><span data-stu-id="46543-187">When a failover happens and hello database becomes a primary database, we take a full backup, which is uploaded toovault.</span></span>
* <span data-ttu-id="46543-188">Neexistuje žádné další náklady toohello zákazníka pro nastavení dlouhodobé uchovávání záloh na sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="46543-188">There is no extra cost toohello customer for setting up long-term backup retention on a secondary database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46543-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="46543-189">Next steps</span></span>
<span data-ttu-id="46543-190">Protože zálohy databáze chránit data před náhodným poškození nebo odstranění, jsou nedílnou součást vámi vyžádaných žádné kontinuity podnikových procesů a strategie zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="46543-190">Because database backups protect data from accidental corruption or deletion, they're an essential part of any business continuity and disaster recovery strategy.</span></span> <span data-ttu-id="46543-191">toolearn o hello jiných řešení kontinuity podnikových procesů SQL Database, najdete v části [obchodní kontinuity přehled](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="46543-191">toolearn about hello other SQL Database business-continuity solutions, see [Business continuity overview](sql-database-business-continuity.md).</span></span>
