---
title: "Portál Azure: vytvoření a správa elastického fondu SQL Database | Microsoft Docs"
description: "Zjistěte, jak pomocí portálu Azure a SQL Database vestavěné inteligentní pro správu, monitorování a optimální velikost škálovatelné elastického fondu Správa nákladů a optimalizace výkonu databáze."
keywords: 
services: sql-database
documentationcenter: 
author: ninarn
manager: jhubbard
editor: cgronlun
ms.assetid: 3dc9b7a3-4b10-423a-8e44-9174aca5cf3d
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 06/06/2016
ms.author: ninarn
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 4ffd1db31f42967dc7f07aa979898dddbb333641
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-the-azure-portal"></a><span data-ttu-id="61450-103">Vytvoření a správa fondu elastické databáze pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="61450-103">Create and manage an elastic pool with the Azure portal</span></span>
<span data-ttu-id="61450-104">Toto téma ukazuje, jak vytvořit a spravovat škálovatelné [elastické fondy](sql-database-elastic-pool.md) pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="61450-104">This topic shows you how to create and manage scalable [elastic pools](sql-database-elastic-pool.md) with the Azure portal.</span></span> <span data-ttu-id="61450-105">Můžete také vytvořit a spravovat Azure elastický fond se [prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md), rozhraní API REST nebo [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="61450-105">You can also create and manage an Azure elastic pool with [PowerShell](sql-database-elastic-pool-manage-powershell.md), the REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="61450-106">Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="61450-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

## <a name="create-an-elastic-pool"></a><span data-ttu-id="61450-107">Vytvoření elastického fondu</span><span class="sxs-lookup"><span data-stu-id="61450-107">Create an elastic pool</span></span> 

<span data-ttu-id="61450-108">Elastický fond můžete vytvořit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="61450-108">There are two ways you can create an elastic pool.</span></span> <span data-ttu-id="61450-109">Pokud znáte cílovou konfiguraci fondu, můžete ho vytvořit od začátku sami, nebo můžete začít s fondem, který vám doporučí služba.</span><span class="sxs-lookup"><span data-stu-id="61450-109">You can do it from scratch if you know the pool setup you want, or start with a recommendation from the service.</span></span> <span data-ttu-id="61450-110">SQL Database má vestavěné inteligentní, která doporučuje instalační program elastického fondu, pokud je cenově výhodnější založené na posledních telemetrii využití pro své databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-110">SQL Database has built-in intelligence that recommends an elastic pool setup if it's more cost-efficient for you based on the past usage telemetry for your databases.</span></span>

<span data-ttu-id="61450-111">Můžete vytvořit více fondů na serveru, ale nemůžete přidat databáze z různých serverů do stejného fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-111">You can create multiple pools on a server, but you can't add databases from different servers into the same pool.</span></span> 

> [!NOTE]
> <span data-ttu-id="61450-112">Elastické fondy jsou obecně dostupné ve všech oblastech Azure s výjimkou oblasti Západní Indie, kde jsou aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="61450-112">Elastic pools are generally available (GA) in all Azure regions except West India where it is currently in preview.</span></span>  <span data-ttu-id="61450-113">Verze GA pro elastické fondy se v této oblasti objeví co nejdřív.</span><span class="sxs-lookup"><span data-stu-id="61450-113">GA of elastic pools in this region will occur as soon as possible.</span></span>
>

### <a name="step-1-create-an-elastic-pool"></a><span data-ttu-id="61450-114">Krok 1: Vytvoření fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="61450-114">Step 1: Create an elastic pool</span></span>

<span data-ttu-id="61450-115">Vytvoření fondu elastické databáze z existující **server** okna portálu je nejjednodušší způsob, jak přesunout existující databáze do pružného fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-115">Creating an elastic pool from an existing **server** blade in the portal is the easiest way to move existing databases into an elastic pool.</span></span>

> [!NOTE]
> <span data-ttu-id="61450-116">Můžete také vytvořit fondu elastické databáze vyhledávání **elastický fond SQL** v **Marketplace** nebo kliknutím na tlačítko **+ přidat** v **SQL elastické fondy** Procházet okna.</span><span class="sxs-lookup"><span data-stu-id="61450-116">You can also create an elastic pool by searching **SQL elastic pool** in the **Marketplace** or clicking **+Add** in the **SQL elastic pools** browse blade.</span></span> <span data-ttu-id="61450-117">Budete moci zadat nové nebo existující server prostřednictvím tohoto fondu zřizování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="61450-117">You are able to specify a new or existing server through this pool provisioning workflow.</span></span>
>
>

1. <span data-ttu-id="61450-118">V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **další služby**  **>**  **servery SQL**a potom klikněte na server obsahující databáze, kterou chcete přidat do fondu elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-118">In the [Azure portal](http://portal.azure.com/), click **More services** **>** **SQL servers**, and then click the server that contains the databases you want to add to an elastic pool.</span></span>
2. <span data-ttu-id="61450-119">Klikněte na **Nový fond**.</span><span class="sxs-lookup"><span data-stu-id="61450-119">Click **New pool**.</span></span>

    ![Přidejte fond na server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    <span data-ttu-id="61450-121">**- nebo -**</span><span class="sxs-lookup"><span data-stu-id="61450-121">**-OR-**</span></span>

    <span data-ttu-id="61450-122">Může se zobrazit zpráva, že jsou doporučené elastické fondy pro server.</span><span class="sxs-lookup"><span data-stu-id="61450-122">You may see a message saying there are recommended elastic pools for the server.</span></span> <span data-ttu-id="61450-123">Kliknutím na zprávu zobrazíte doporučení k vytvoření fondů založená na historické telemetrii využití databází. Pak kliknutím na úroveň zobrazte další podrobnosti a možnosti přizpůsobení fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-123">Click the message to see the recommended pools based on historical database usage telemetry, and then click the tier to see more details and customize the pool.</span></span> <span data-ttu-id="61450-124">Podrobnější informace o doporučeních najdete v části [Vysvětlení doporučení k fondům](#understand-elastic-pool-recommendations) dále v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="61450-124">See [Understand pool recommendations](#understand-elastic-pool-recommendations) later in this topic for how the recommendation is made.</span></span>

    ![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. <span data-ttu-id="61450-126">**Elastický fond** se zobrazí okno, což je, kde můžete určit nastavení fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-126">The **elastic pool** blade appears, which is where you specify the settings for your pool.</span></span> <span data-ttu-id="61450-127">Pokud jste klikli na **nový fond** v předchozím kroku, cenová úroveň je **standardní** je vybrána ve výchozím nastavení a žádné databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-127">If you clicked **New pool** in the previous step, the pricing tier is **Standard** by default and no databases is selected.</span></span> <span data-ttu-id="61450-128">Můžete vytvořit prázdný fond nebo zadat sadu existujících databází z daného serveru, které se mají přesunout do fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-128">You can create an empty pool, or specify a set of existing databases from that server to move into the pool.</span></span> <span data-ttu-id="61450-129">Pokud vytváříte doporučený fond, předběžně vyplní doporučené cenovou úroveň, nastavení výkonu a seznam databází, ale můžete je stále změnit.</span><span class="sxs-lookup"><span data-stu-id="61450-129">If you are creating a recommended pool, the recommended pricing tier, performance settings, and list of databases are prepopulated, but you can still change them.</span></span>

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. <span data-ttu-id="61450-131">Zadejte název elastického fondu nebo ponechte výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="61450-131">Specify a name for the elastic pool, or leave it as the default.</span></span>

### <a name="step-2-choose-a-pricing-tier"></a><span data-ttu-id="61450-132">Krok 2: Zvolte cenovou úroveň</span><span class="sxs-lookup"><span data-stu-id="61450-132">Step 2: Choose a pricing tier</span></span>

<span data-ttu-id="61450-133">Cenová úroveň fondu určuje funkce, které jsou k dispozici pro elastics ve fondu a maximální počet jednotek Edtu (eDTU MAX) a úložiště (GB) k dispozici pro každou databázi.</span><span class="sxs-lookup"><span data-stu-id="61450-133">The pool's pricing tier determines the features available to the elastics in the pool, and the maximum number of eDTUs (eDTU MAX), and storage (GBs) available to each database.</span></span> <span data-ttu-id="61450-134">Podrobnosti viz Úrovně služeb</span><span class="sxs-lookup"><span data-stu-id="61450-134">For details, see Service Tiers.</span></span>

<span data-ttu-id="61450-135">Chcete-li změnit cenovou úroveň fondu, klikněte na **Cenová úroveň**, vyberte požadovanou cenovou úroveň a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="61450-135">To change the pricing tier for the pool, click **Pricing tier**, click the pricing tier you want, and then click **Select**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61450-136">Až vyberete cenovou úroveň a potvrdíte svoji volbu kliknutím na **OK** v posledním kroku, nebude už možné cenovou úroveň fondu změnit.</span><span class="sxs-lookup"><span data-stu-id="61450-136">After you choose the pricing tier and commit your changes by clicking **OK** in the last step, you won't be able to change the pricing tier of the pool.</span></span> <span data-ttu-id="61450-137">Pokud chcete změnit cenovou úroveň existujícího elastického fondu, vytvoření fondu elastické databáze v s požadovanou cenovou úrovní a migrace databáze do tohoto nového fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-137">To change the pricing tier for an existing elastic pool, create an elastic pool in the desired pricing tier and migrate the databases to this new pool.</span></span>
>

![Výběr cenové úrovně](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-the-pool"></a><span data-ttu-id="61450-139">Krok 3: Konfigurace fondu</span><span class="sxs-lookup"><span data-stu-id="61450-139">Step 3: Configure the pool</span></span>

<span data-ttu-id="61450-140">Po nastavení cenové úrovně, klikněte na tlačítko Konfigurovat fond přidat databáze, sada fond hodnoty Edtu a úložiště (GB) a nastavíte min a max Edtu pro elastics ve fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-140">After setting the pricing tier, click Configure pool where you add databases, set pool eDTUs and storage (pool GBs), and where you set the min and max eDTUs for the elastics in the pool.</span></span>

1. <span data-ttu-id="61450-141">Klikněte na **Konfigurovat fond**</span><span class="sxs-lookup"><span data-stu-id="61450-141">Click **Configure pool**</span></span>
2. <span data-ttu-id="61450-142">Vyberte databáze, které chcete přidat do fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-142">Select the databases you want to add to the pool.</span></span> <span data-ttu-id="61450-143">Tento krok je při vytváření fondu volitelný.</span><span class="sxs-lookup"><span data-stu-id="61450-143">This step is optional while creating the pool.</span></span> <span data-ttu-id="61450-144">Databáze můžete přidat i po vytvoření fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-144">Databases can be added after the pool has been created.</span></span>
    <span data-ttu-id="61450-145">Klikněte na možnost **Přidat databázi**, pak na databáze, které chcete přidat a potom na tlačítko **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="61450-145">To add databases, click **Add database**, click the databases that you want to add, and then click the **Select** button.</span></span>

    ![Přidání databází](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    <span data-ttu-id="61450-147">Pokud databáze, s kterými pracujete, mají dostatek historických telemetrických dat, graf **Odhadované eDTU a využití GB** a pruhový graf **Skutečné využití eDTU** se aktualizují, aby vám pomohly s rozhodováním o hodnotách konfigurace.</span><span class="sxs-lookup"><span data-stu-id="61450-147">If the databases you're working with have enough historical usage telemetry, the **Estimated eDTU and GB usage** graph and the **Actual eDTU usage** bar chart update to help you make configuration decisions.</span></span> <span data-ttu-id="61450-148">Služba vám také může zobrazovat doporučení s cílem nastavit optimální velikost fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-148">Also, the service may give you a recommendation message to help you right-size the pool.</span></span> <span data-ttu-id="61450-149">Viz část [Dynamická doporučení](#understand-elastic-pool-recommendations).</span><span class="sxs-lookup"><span data-stu-id="61450-149">See [Dynamic Recommendations](#understand-elastic-pool-recommendations).</span></span>

3. <span data-ttu-id="61450-150">Pomocí ovládacích prvků na stránce **Konfigurace fondu** můžete zkontrolovat nastavení a konfigurovat fond.</span><span class="sxs-lookup"><span data-stu-id="61450-150">Use the controls on the **Configure pool** page to explore settings and configure your pool.</span></span> <span data-ttu-id="61450-151">V tématu [omezení Elastických fondů](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) další podrobnosti o omezeních pro jednotlivé úrovně služby a najdete v tématu [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool.md) podrobné pokyny k optimalizaci velikosti fondu elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-151">See [Elastic pools limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for more detail about limits for each service tier, and see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md) for detailed guidance on right-sizing an elastic pool.</span></span> <span data-ttu-id="61450-152">Další informace o nastavení fondu najdete v tématu [elastického fondu vlastnosti](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span><span class="sxs-lookup"><span data-stu-id="61450-152">For more information about pool settings, see [Elastic pool properties](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span></span>

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. <span data-ttu-id="61450-154">Po úpravě nastavení v okně **Konfigurace fondu** klikněte na tlačítko **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="61450-154">Click **Select** in the **Configure Pool** blade after changing settings.</span></span>
5. <span data-ttu-id="61450-155">Kliknutím na tlačítko **OK** vytvořte fond.</span><span class="sxs-lookup"><span data-stu-id="61450-155">Click **OK** to create the pool.</span></span>

## <a name="understand-elastic-pool-recommendations"></a><span data-ttu-id="61450-156">Vysvětlení doporučení k elastického fondu</span><span class="sxs-lookup"><span data-stu-id="61450-156">Understand elastic pool recommendations</span></span>

<span data-ttu-id="61450-157">Služba SQL Database vyhodnocuje historii využití a doporučí použití jednoho nebo několika fondů, jakmile to začne být cenově výhodnější než použití izolované databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-157">The SQL Database service evaluates usage history and recommends one or more pools when it is more cost-effective than using single databases.</span></span> <span data-ttu-id="61450-158">Každé doporučení je konfigurováno pro jedinečnou podmnožinu databází serveru, která je pro fond nejvhodnější.</span><span class="sxs-lookup"><span data-stu-id="61450-158">Each recommendation is configured with a unique subset of the server's databases that best fit the pool.</span></span>

![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

<span data-ttu-id="61450-160">Doporučení fondu zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="61450-160">The pool recommendation comprises:</span></span>

- <span data-ttu-id="61450-161">Cenovou úroveň pro fond (Basic, Standard, Premium nebo Premium RS)</span><span class="sxs-lookup"><span data-stu-id="61450-161">A pricing tier for the pool (Basic, Standard, Premium, or Premium RS)</span></span>
- <span data-ttu-id="61450-162">vhodnou hodnotu **POOL eDTU** (označovanou také jako Max eDTU pro fond),</span><span class="sxs-lookup"><span data-stu-id="61450-162">Appropriate **POOL eDTUs** (also called Max eDTUs per pool)</span></span>
- <span data-ttu-id="61450-163">hodnoty **eDTU MAX** a **eDTU MIN** pro každou databázi,</span><span class="sxs-lookup"><span data-stu-id="61450-163">The **eDTU MAX** and **eDTU Min** per database</span></span>
- <span data-ttu-id="61450-164">seznam doporučených databází pro fond.</span><span class="sxs-lookup"><span data-stu-id="61450-164">The list of recommended databases for the pool</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61450-165">Při vytváření doporučení bere služba v úvahu telemetrická data za posledních 30 dní.</span><span class="sxs-lookup"><span data-stu-id="61450-165">The service takes the last 30 days of telemetry into account when recommending pools.</span></span> <span data-ttu-id="61450-166">Aby databáze mohla být zařazena mezi doporučené fondu elastické databáze musí existovat alespoň 7 dní.</span><span class="sxs-lookup"><span data-stu-id="61450-166">For a database to be considered as a candidate for an elastic pool, it must exist for at least 7 days.</span></span> <span data-ttu-id="61450-167">Databáze, které již v elastickém fondu jsou, se nepovažují za kandidáty pro doporučení elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-167">Databases that are already in an elastic pool are not considered as candidates for elastic pool recommendations.</span></span>
>

<span data-ttu-id="61450-168">Služba vyhodnocuje potřebné prostředky a cenovou výhodnost přesunu jednotlivých databází v každé úrovni služby do fondu ve stejné úrovni.</span><span class="sxs-lookup"><span data-stu-id="61450-168">The service evaluates resource needs and cost effectiveness of moving the single databases in each service tier into pools of the same tier.</span></span> <span data-ttu-id="61450-169">Například pro všechny databáze na serveru, které jsou v úrovni Standard, se zvažuje výhodnost jejich přesunu do elastického fondu také s úrovní Standard.</span><span class="sxs-lookup"><span data-stu-id="61450-169">For example, all Standard databases on a server are assessed for their fit into a Standard Elastic Pool.</span></span> <span data-ttu-id="61450-170">To znamená, že služba nikdy nenavrhne přesun databáze mezi úrovněmi, například přesun databáze s úrovní Standard do fondu s úrovní Premium.</span><span class="sxs-lookup"><span data-stu-id="61450-170">This means the service does not make cross-tier recommendations such as moving a Standard database into a Premium pool.</span></span>

<span data-ttu-id="61450-171">Po přidání databází do fondu, doporučení se dynamicky vygeneruje, na základě historie využití databází, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="61450-171">After adding databases to the pool, recommendations are dynamically generated based on the historical usage of the databases you have selected.</span></span> <span data-ttu-id="61450-172">Tato doporučení jsou zobrazeny v eDTU a GB využití graf a v hlavičce doporučení v horní části **konfigurace fondu** okno.</span><span class="sxs-lookup"><span data-stu-id="61450-172">These recommendations are shown in the eDTU and GB usage chart and in a recommendation banner at the top of the **Configure pool** blade.</span></span> <span data-ttu-id="61450-173">Tato doporučení jsou určeny k usnadnění vytváření fondu elastické databáze optimalizované pro vaše konkrétní databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-173">These recommendations are intended to assist you in creating an elastic pool optimized for your specific databases.</span></span>

![dynamická doporučení](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a><span data-ttu-id="61450-175">Správa a sledování fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="61450-175">Manage and monitor an elastic pool</span></span>

<span data-ttu-id="61450-176">Na portálu Azure můžete použít ke sledování a správě fondu elastické databáze a databáze ve fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-176">You can use the Azure portal to monitor and manage an elastic pool and the databases in the pool.</span></span> <span data-ttu-id="61450-177">Z portálu můžete sledovat využití fondu elastické databáze a databáze v tomto fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-177">From the portal, you can monitor the utilization of an elastic pool and the databases within that pool.</span></span> <span data-ttu-id="61450-178">Můžete také nastavit sadu změn do pružného fondu a odeslat všechny změny ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="61450-178">You can also make a set of changes to your elastic pool and submit all changes at the same time.</span></span> <span data-ttu-id="61450-179">Tyto změny zahrnují přidávání nebo odebírání databáze, změna nastavení služby elastického fondu nebo změna nastavení databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-179">These changes include adding or removing databases, changing your elastic pool settings, or changing your database settings.</span></span>

<span data-ttu-id="61450-180">Následující obrázek znázorňuje příklad elastickém fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-180">The following graphic shows an example elastic pool.</span></span> <span data-ttu-id="61450-181">Zobrazení zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="61450-181">The view includes:</span></span>

*  <span data-ttu-id="61450-182">Grafy pro sledování využití prostředků elastický fond a databází obsažené ve fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-182">Charts for monitoring resource usage of both the elastic pool and the databases contained in the pool.</span></span>
*  <span data-ttu-id="61450-183">**Konfigurace** tlačítka fondu změny do elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-183">The **Configure** pool button to make changes to the elastic pool.</span></span>
*  <span data-ttu-id="61450-184">**Vytvořit databázi** tlačítko, které vytvoří databázi a přidává ji k aktuální elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-184">The **Create database** button that creates a database and adds it to the current elastic pool.</span></span>
*  <span data-ttu-id="61450-185">Elastické úlohy, které vám pomůžou spravovat velké počty databází pomocí spouštění skriptů Transact SQL pro všechny databáze v seznamu.</span><span class="sxs-lookup"><span data-stu-id="61450-185">Elastic jobs that help you manage large numbers of databases by running Transact SQL scripts against all databases in a list.</span></span>

![Zobrazení fondu][2]

<span data-ttu-id="61450-187">Můžete přejít na konkrétní fond zobrazíte jeho využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="61450-187">You can go to a particular pool to see its resource utilization.</span></span> <span data-ttu-id="61450-188">Ve výchozím nastavení je fond konfigurována pro zobrazení využití úložiště a eDTU za poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="61450-188">By default, the pool is configured to show storage and eDTU usage for the last hour.</span></span> <span data-ttu-id="61450-189">Graf lze konfigurovat zobrazíte jiné metriky v různých časových oken.</span><span class="sxs-lookup"><span data-stu-id="61450-189">The chart can be configured to show different metrics over various time windows.</span></span>

1. <span data-ttu-id="61450-190">Vyberte fondu elastické databáze pro práci s.</span><span class="sxs-lookup"><span data-stu-id="61450-190">Select an elastic pool to work with.</span></span>
2. <span data-ttu-id="61450-191">V části **elastického fondu monitorování** je graf s názvem bez přípony **využití prostředků**.</span><span class="sxs-lookup"><span data-stu-id="61450-191">Under **Elastic Pool Monitoring** is a chart labeled **Resource utilization**.</span></span> <span data-ttu-id="61450-192">Klikněte na graf.</span><span class="sxs-lookup"><span data-stu-id="61450-192">Click the chart.</span></span>

    ![Monitorování elastického fondu][3]

    <span data-ttu-id="61450-194">**Metrika** otevře se okno, zobrazuje podrobný přehled o metriku zadané v průběhu zadaný časový interval.</span><span class="sxs-lookup"><span data-stu-id="61450-194">The **Metric** blade opens, showing a detailed view of the specified metrics over the specified time window.</span></span>   

    ![Okno Metrika][9]

### <a name="to-customize-the-chart-display"></a><span data-ttu-id="61450-196">Chcete-li přizpůsobit zobrazení grafu</span><span class="sxs-lookup"><span data-stu-id="61450-196">To customize the chart display</span></span>

<span data-ttu-id="61450-197">Můžete upravit graf a v okně metriky zobrazíte další metriky jako procento, procento vstupů/výstupů dat a protokolu vstupně-výstupní operace % využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="61450-197">You can edit the chart and the metric blade to display other metrics such as CPU percentage, data IO percentage, and log IO percentage used.</span></span>

1. <span data-ttu-id="61450-198">V okně metriky, klikněte na **upravit**.</span><span class="sxs-lookup"><span data-stu-id="61450-198">On the metric blade, click **Edit**.</span></span>

    ![Klikněte na tlačítko Upravit][6]

2. <span data-ttu-id="61450-200">V **upravit graf** okně vyberte časový interval (po hodině, ještě dnes, nebo za minulý týden), nebo klikněte na **vlastní** a vybrat libovolnou oblast datum v poslední dva týdny.</span><span class="sxs-lookup"><span data-stu-id="61450-200">In the **Edit Chart** blade, select a time range (past hour, today, or past week), or click **custom** to select any date range in the last two weeks.</span></span> <span data-ttu-id="61450-201">Vyberte typ grafu (pruhu nebo čáry) a potom vyberte zdroje, které chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="61450-201">Select the chart type (bar or line), then select the resources to monitor.</span></span>

   > [!Note]
   > <span data-ttu-id="61450-202">Pouze metriky se stejnou jednotkou míry lze zobrazit v grafu ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="61450-202">Only metrics with the same unit of measure can be displayed in the chart at the same time.</span></span> <span data-ttu-id="61450-203">Například pokud zvolíte možnost "eDTU procento" pak můžete zvolit pouze další metriky s procentem jako měrné jednotky.</span><span class="sxs-lookup"><span data-stu-id="61450-203">For example, if you select "eDTU percentage" then you can only select other metrics with percentage as the unit of measure.</span></span>
   >

    ![Klikněte na tlačítko Upravit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. <span data-ttu-id="61450-205">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="61450-205">Then click **OK**.</span></span>

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a><span data-ttu-id="61450-206">Spravovat a monitorovat databází v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="61450-206">Manage and monitor databases in an elastic pool</span></span>

<span data-ttu-id="61450-207">Jednotlivé databáze je možné monitorovat také pro potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="61450-207">Individual databases can also be monitored for potential trouble.</span></span>

1. <span data-ttu-id="61450-208">V části **elastické databáze monitorování**, je graf, který zobrazí metriky pro pět databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-208">Under **Elastic Database Monitoring**, there is a chart that displays metrics for five databases.</span></span> <span data-ttu-id="61450-209">Ve výchozím nastavení grafu zobrazí top 5 databáze ve fondu podle využití eDTU průměrná za poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="61450-209">By default, the chart displays the top 5 databases in the pool by average eDTU usage in the past hour.</span></span> <span data-ttu-id="61450-210">Klikněte na graf.</span><span class="sxs-lookup"><span data-stu-id="61450-210">Click the chart.</span></span>

    ![Monitorování elastického fondu][4]

2. <span data-ttu-id="61450-212">**Využití prostředků databáze** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="61450-212">The **Database Resource Utilization** blade appears.</span></span> <span data-ttu-id="61450-213">To poskytuje podrobný přehled o využití databáze ve fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-213">This provides a detailed view of the database usage in the pool.</span></span> <span data-ttu-id="61450-214">Pomocí mřížky v dolní části okna, můžete vybrat všechny databáze ve fondu se mají zobrazit jeho použití v grafu (až 5 databáze).</span><span class="sxs-lookup"><span data-stu-id="61450-214">Using the grid in the lower part of the blade, you can select any databases in the pool to display its usage in the chart (up to 5 databases).</span></span> <span data-ttu-id="61450-215">Můžete taky přizpůsobit okno metriky a času zobrazené v grafu kliknutím **upravit graf**.</span><span class="sxs-lookup"><span data-stu-id="61450-215">You can also customize the metrics and time window displayed in the chart by clicking **Edit chart**.</span></span>

    ![Okna využití prostředků databáze][8]

### <a name="to-customize-the-view"></a><span data-ttu-id="61450-217">Chcete-li přizpůsobit zobrazení</span><span class="sxs-lookup"><span data-stu-id="61450-217">To customize the view</span></span>

1. <span data-ttu-id="61450-218">V **databáze využití prostředků** okně klikněte na tlačítko **upravit graf**.</span><span class="sxs-lookup"><span data-stu-id="61450-218">In the **Database resource utilization** blade, click **Edit chart**.</span></span>

    ![Klikněte na tlačítko Upravit graf](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. <span data-ttu-id="61450-220">V **upravit** grafu okně, vyberte časové rozmezí (za hodinu nebo za posledních 24 hodin) nebo klikněte na **vlastní** chcete vybrat jiný den v posledních 2 týdny k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="61450-220">In the **Edit** chart blade, select a time range (past hour or past 24 hours), or click **custom** to select a different day in the past 2 weeks to display.</span></span>

    ![Klikněte na tlačítko Vlastní](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. <span data-ttu-id="61450-222">Klikněte na tlačítko **porovnat databází tak, že** rozevírací seznam a vyberte jiné metriky pro použití při porovnávání databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-222">Click the **Compare databases by** dropdown to select a different metric to use when comparing databases.</span></span>

    ![Upravit graf](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a><span data-ttu-id="61450-224">Chcete-li vybrat databáze k monitorování</span><span class="sxs-lookup"><span data-stu-id="61450-224">To select databases to monitor</span></span>

<span data-ttu-id="61450-225">V seznamu databáze **využití prostředků databáze** okno, můžete vyhledat konkrétní databáze tak, že vyhledá prostřednictvím stránky v seznamu nebo zadáním názvu databáze.</span><span class="sxs-lookup"><span data-stu-id="61450-225">In the database list in the **Database Resource Utilization** blade, you can find particular databases by looking through the pages in the list or by typing in the name of a database.</span></span> <span data-ttu-id="61450-226">Vyberte databázi, použijte zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="61450-226">Use the checkbox to select the database.</span></span>

![Vyhledejte databáze k monitorování][7]


## <a name="add-an-alert-to-an-elastic-pool-resource"></a><span data-ttu-id="61450-228">Přidání oznámení na prostředek elastického fondu</span><span class="sxs-lookup"><span data-stu-id="61450-228">Add an alert to an elastic pool resource</span></span>

<span data-ttu-id="61450-229">Pravidla můžete přidat do pružného fondu, který odeslání e-mailu na osoby nebo výstrahu řetězce adresy URL koncových bodů, pokud se elastický fond dotkne prahovou hodnotu využití, které jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="61450-229">You can add rules to an elastic pool that send email to people or alert strings to URL endpoints when the elastic pool hits a utilization threshold that you set up.</span></span>

<span data-ttu-id="61450-230">**Postup přidání upozornění k jakémukoli prostředku:**</span><span class="sxs-lookup"><span data-stu-id="61450-230">**To add an alert to any resource:**</span></span>

1. <span data-ttu-id="61450-231">Klikněte na tlačítko **využití prostředků** graf a otevřete **metrika** okně klikněte na tlačítko **přidat upozornění**a pak zadejte informace do **přidání pravidla výstrahy** okno (**prostředků** je automaticky nastavena si být fondu pracujete s).</span><span class="sxs-lookup"><span data-stu-id="61450-231">Click the **Resource utilization** chart to open the **Metric** blade, click **Add alert**, and then fill out the information in the **Add an alert rule** blade (**Resource** is automatically set up to be the pool you're working with).</span></span>
2. <span data-ttu-id="61450-232">Zadejte **název** a **popis** identifikující výstrahy pro vás i příjemce.</span><span class="sxs-lookup"><span data-stu-id="61450-232">Type a **Name** and **Description** that identifies the alert to you and the recipients.</span></span>
3. <span data-ttu-id="61450-233">Vyberte **metrika** , kterou chcete výstrahu ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="61450-233">Choose a **Metric** that you want to alert from the list.</span></span>

    <span data-ttu-id="61450-234">Graf zobrazuje dynamicky využití prostředků pro tuto metriku, které vám pomohou zvolit prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="61450-234">The chart dynamically shows resource utilization for that metric to help you choose a threshold.</span></span>

4. <span data-ttu-id="61450-235">Vyberte **podmínku** (větší než menší než atd) a **prahová hodnota**.</span><span class="sxs-lookup"><span data-stu-id="61450-235">Choose a **Condition** (greater than, less than, etc.) and a **Threshold**.</span></span>
5. <span data-ttu-id="61450-236">Vyberte **období** času, který metriky pravidlo je nutné splnit před výstrahy aktivačních událostí.</span><span class="sxs-lookup"><span data-stu-id="61450-236">Choose a **Period** of time that the metric rule must be satisfied before the alert triggers.</span></span>
6. <span data-ttu-id="61450-237">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="61450-237">Click **OK**.</span></span>

<span data-ttu-id="61450-238">Další informace najdete v tématu [vytvářet výstrahy, SQL Database na portálu Azure](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="61450-238">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="61450-239">Přesun databáze do pružného fondu</span><span class="sxs-lookup"><span data-stu-id="61450-239">Move a database into an elastic pool</span></span>

<span data-ttu-id="61450-240">Můžete přidat nebo odebrat databáze z existující fond.</span><span class="sxs-lookup"><span data-stu-id="61450-240">You can add or remove databases from an existing pool.</span></span> <span data-ttu-id="61450-241">Databáze může být v jiných fondech.</span><span class="sxs-lookup"><span data-stu-id="61450-241">The databases can be in other pools.</span></span> <span data-ttu-id="61450-242">Ale můžete přidat pouze databáze, které jsou na stejného logického serveru.</span><span class="sxs-lookup"><span data-stu-id="61450-242">However, you can only add databases that are on the same logical server.</span></span>

1. <span data-ttu-id="61450-243">V okně pro fond v části **elastické databáze** klikněte na tlačítko **konfigurace fondu**.</span><span class="sxs-lookup"><span data-stu-id="61450-243">In the blade for the pool, under **Elastic databases** click **Configure pool**.</span></span>

    ![Klikněte na tlačítko Konfigurovat fond][1]

2. <span data-ttu-id="61450-245">V **konfigurace fondu** okně klikněte na tlačítko **přidat do fondu**.</span><span class="sxs-lookup"><span data-stu-id="61450-245">In the **Configure pool** blade, click **Add to pool**.</span></span>

    ![Klikněte na tlačítko Přidat do fondu](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. <span data-ttu-id="61450-247">V **přidat databáze** okně vyberte databázi nebo databáze pro přidání do fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-247">In the **Add databases** blade, select the database or databases to add to the pool.</span></span> <span data-ttu-id="61450-248">Pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="61450-248">Then click **Select**.</span></span>

    ![Vyberte databáze pro přidání](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    <span data-ttu-id="61450-250">**Konfigurace fondu** okno teď zobrazuje databáze vybrané pro přidání, s její stav nastavit na **čekající**.</span><span class="sxs-lookup"><span data-stu-id="61450-250">The **Configure pool** blade now lists the database you selected to be added, with its status set to **Pending**.</span></span>

    ![Přidání čekající fondu](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. <span data-ttu-id="61450-252">V **konfigurovat fond okno**, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="61450-252">In the **Configure pool blade**, click **Save**.</span></span>

    ![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="61450-254">Přesunutí databáze z fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="61450-254">Move a database out of an elastic pool</span></span>

1. <span data-ttu-id="61450-255">V **konfigurace fondu** okně vyberte databázi nebo databáze k odebrání.</span><span class="sxs-lookup"><span data-stu-id="61450-255">In the **Configure pool** blade, select the database or databases to remove.</span></span>

    ![výpis databáze](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. <span data-ttu-id="61450-257">Klikněte na tlačítko **odebrat z fondu**.</span><span class="sxs-lookup"><span data-stu-id="61450-257">Click **Remove from pool**.</span></span>

    ![výpis databáze](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    <span data-ttu-id="61450-259">**Konfigurace fondu** okno teď uvádí databázi vybrané k odstranění s její stav nastavit na **čekající**.</span><span class="sxs-lookup"><span data-stu-id="61450-259">The **Configure pool** blade now lists the database you selected to be removed with its status set to **Pending**.</span></span>

    ![Náhled databáze přidávání a odebírání](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. <span data-ttu-id="61450-261">V **konfigurovat fond okno**, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="61450-261">In the **Configure pool blade**, click **Save**.</span></span>

    ![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="61450-263">Změňte nastavení výkonu elastického fondu</span><span class="sxs-lookup"><span data-stu-id="61450-263">Change performance settings of an elastic pool</span></span>

<span data-ttu-id="61450-264">Při sledování využití prostředků elastického fondu, může se stát, že některé změny jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="61450-264">As you monitor the resource utilization of an elastic pool, you may discover that some adjustments are needed.</span></span> <span data-ttu-id="61450-265">Možná fondu musí změnu omezení výkon nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="61450-265">Maybe the pool needs a change in the performance or storage limits.</span></span> <span data-ttu-id="61450-266">Pravděpodobně budete chtít změnit nastavení databáze ve fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-266">Possibly you want to change the database settings in the pool.</span></span> <span data-ttu-id="61450-267">Můžete změnit nastavení fondu kdykoli získat nejlepší kompromis výkonu a nákladů.</span><span class="sxs-lookup"><span data-stu-id="61450-267">You can change the setup of the pool at any time to get the best balance of performance and cost.</span></span> <span data-ttu-id="61450-268">V tématu [při fondu elastické databáze slouží?](sql-database-elastic-pool.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="61450-268">See [When should an elastic pool be used?](sql-database-elastic-pool.md) for more information.</span></span>

<span data-ttu-id="61450-269">Chcete-li změnit omezení Edtu nebo úložiště na každý fond a Edtu na databázi:</span><span class="sxs-lookup"><span data-stu-id="61450-269">To change the eDTUs or storage limits per pool, and eDTUs per database:</span></span>

1. <span data-ttu-id="61450-270">Otevřete **konfigurace fondu** okno.</span><span class="sxs-lookup"><span data-stu-id="61450-270">Open the **Configure pool** blade.</span></span>

    <span data-ttu-id="61450-271">V části **elastického fondu nastavení**, použijte buď posuvníku Chcete-li změnit nastavení fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-271">Under **elastic pool settings**, use either slider to change the pool settings.</span></span>

    ![Využití elastického fondu prostředků](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. <span data-ttu-id="61450-273">Při změně nastavení zobrazení ukazuje odhadované měsíční náklady změny.</span><span class="sxs-lookup"><span data-stu-id="61450-273">When the setting changes, the display shows the estimated monthly cost of the change.</span></span>

    ![Aktualizace služby elastického fondu a měsíční náklady na nový](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="61450-275">Latence operací elastického fondu</span><span class="sxs-lookup"><span data-stu-id="61450-275">Latency of elastic pool operations</span></span>
* <span data-ttu-id="61450-276">Změna minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.</span><span class="sxs-lookup"><span data-stu-id="61450-276">Changing the min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="61450-277">Změna Edtu na fond, závisí na celkovém množství místo využité všechny databáze ve fondu.</span><span class="sxs-lookup"><span data-stu-id="61450-277">Changing the eDTUs per pool depends on the total amount of space used by all databases in the pool.</span></span> <span data-ttu-id="61450-278">Změny průměrně trvají méně než 90 minut na každých 100 GB.</span><span class="sxs-lookup"><span data-stu-id="61450-278">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="61450-279">Například pokud celkové místo používá všechny databáze ve fondu je 200 GB, pak očekávané latence pro změnu fondu eDTU na fond je 3 hodiny nebo méně.</span><span class="sxs-lookup"><span data-stu-id="61450-279">For example, if the total space used by all databases in the pool is 200 GB, then the expected latency for changing the pool eDTU per pool is 3 hours or less.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61450-280">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61450-280">Next steps</span></span>

- <span data-ttu-id="61450-281">Abyste zjistili, co je, najdete v elastickém fondu [elastického fondu SQL Database](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="61450-281">To understand what an elastic pool is, see [SQL Database elastic pool](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="61450-282">Pokyny k používání elastické fondy najdete v tématu [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="61450-282">For guidance on using elastic pools, see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="61450-283">Pokud chcete použít ke spuštění skriptů jazyka Transact-SQL pro libovolný počet databází ve fondu elastické úlohy, najdete v části [elastické úlohy přehled](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="61450-283">To use elastic jobs to run Transact-SQL scripts against any number of databases in the pool, see [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span>
- <span data-ttu-id="61450-284">K dotazování mezi libovolný počet databází ve fondu, najdete v části [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="61450-284">To query across any number of databases in the pool, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
- <span data-ttu-id="61450-285">Transakce libovolný počet databází ve fondu, najdete v části [elastické transakce](sql-database-elastic-transactions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="61450-285">For transactions any number of databases in the pool, see [Elastic transactions](sql-database-elastic-transactions-overview.md).</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
