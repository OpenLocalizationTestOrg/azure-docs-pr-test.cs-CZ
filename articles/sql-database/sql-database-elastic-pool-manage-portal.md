---
title: "Portál Azure: vytvoření a správa elastického fondu SQL Database | Microsoft Docs"
description: "Zjistěte, jak toouse hello portál Azure a SQL Database vestavěné inteligentní toomanage, sledování a optimální velikost výkon databáze toooptimize škálovatelné elastický fond a spravovat náklady."
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
ms.openlocfilehash: e0de952bc0c91177f64c04363630783d72435741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-hello-azure-portal"></a><span data-ttu-id="3abcb-103">Vytvoření a správa fondu elastické databáze s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3abcb-103">Create and manage an elastic pool with hello Azure portal</span></span>
<span data-ttu-id="3abcb-104">Toto téma ukazuje, jak toocreate a spravovat škálovatelné [elastické fondy](sql-database-elastic-pool.md) s hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3abcb-104">This topic shows you how toocreate and manage scalable [elastic pools](sql-database-elastic-pool.md) with hello Azure portal.</span></span> <span data-ttu-id="3abcb-105">Můžete také vytvořit a spravovat Azure elastický fond se [prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API nebo [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="3abcb-105">You can also create and manage an Azure elastic pool with [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="3abcb-106">Můžete také vytvořit a přesun databáze do a z elastické fondy pomocí [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="3abcb-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

## <a name="create-an-elastic-pool"></a><span data-ttu-id="3abcb-107">Vytvoření elastického fondu</span><span class="sxs-lookup"><span data-stu-id="3abcb-107">Create an elastic pool</span></span> 

<span data-ttu-id="3abcb-108">Elastický fond můžete vytvořit dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="3abcb-108">There are two ways you can create an elastic pool.</span></span> <span data-ttu-id="3abcb-109">Můžete provést od začátku Pokud znáte hello fondu instalaci nebo spuštění s doporučením ze služby hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-109">You can do it from scratch if you know hello pool setup you want, or start with a recommendation from hello service.</span></span> <span data-ttu-id="3abcb-110">SQL Database má vestavěné inteligentní, která doporučuje instalační program elastického fondu, pokud je cenově výhodnější založené na hello po telemetrii využití databází.</span><span class="sxs-lookup"><span data-stu-id="3abcb-110">SQL Database has built-in intelligence that recommends an elastic pool setup if it's more cost-efficient for you based on hello past usage telemetry for your databases.</span></span>

<span data-ttu-id="3abcb-111">Můžete vytvořit více fondů na serveru, ale nemůžete přidat databáze z různých serverů do hello stejného fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-111">You can create multiple pools on a server, but you can't add databases from different servers into hello same pool.</span></span> 

> [!NOTE]
> <span data-ttu-id="3abcb-112">Elastické fondy jsou obecně dostupné ve všech oblastech Azure s výjimkou oblasti Západní Indie, kde jsou aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="3abcb-112">Elastic pools are generally available (GA) in all Azure regions except West India where it is currently in preview.</span></span>  <span data-ttu-id="3abcb-113">Verze GA pro elastické fondy se v této oblasti objeví co nejdřív.</span><span class="sxs-lookup"><span data-stu-id="3abcb-113">GA of elastic pools in this region will occur as soon as possible.</span></span>
>

### <a name="step-1-create-an-elastic-pool"></a><span data-ttu-id="3abcb-114">Krok 1: Vytvoření fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="3abcb-114">Step 1: Create an elastic pool</span></span>

<span data-ttu-id="3abcb-115">Vytvoření fondu elastické databáze z existující **server** okna portálu hello je hello nejjednodušší způsob, jak toomove existující databáze do pružného fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-115">Creating an elastic pool from an existing **server** blade in hello portal is hello easiest way toomove existing databases into an elastic pool.</span></span>

> [!NOTE]
> <span data-ttu-id="3abcb-116">Můžete také vytvořit fondu elastické databáze vyhledávání **elastický fond SQL** v hello **Marketplace** nebo kliknutím na tlačítko **+ přidat** v hello **SQL elastické fondy**Procházet okna.</span><span class="sxs-lookup"><span data-stu-id="3abcb-116">You can also create an elastic pool by searching **SQL elastic pool** in hello **Marketplace** or clicking **+Add** in hello **SQL elastic pools** browse blade.</span></span> <span data-ttu-id="3abcb-117">Budete mít toospecify nového nebo existujícího serveru přes tento fond zřizování pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-117">You are able toospecify a new or existing server through this pool provisioning workflow.</span></span>
>
>

1. <span data-ttu-id="3abcb-118">V hello [portál Azure](http://portal.azure.com/), klikněte na tlačítko **další služby**  **>**  **servery SQL**a potom klikněte na hello serveru, který obsahuje hello databáze má tooadd tooan elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-118">In hello [Azure portal](http://portal.azure.com/), click **More services** **>** **SQL servers**, and then click hello server that contains hello databases you want tooadd tooan elastic pool.</span></span>
2. <span data-ttu-id="3abcb-119">Klikněte na **Nový fond**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-119">Click **New pool**.</span></span>

    ![Přidání serveru tooa fondu](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    <span data-ttu-id="3abcb-121">**- nebo -**</span><span class="sxs-lookup"><span data-stu-id="3abcb-121">**-OR-**</span></span>

    <span data-ttu-id="3abcb-122">Může se zobrazit zpráva, že jsou doporučené elastické fondy pro hello server.</span><span class="sxs-lookup"><span data-stu-id="3abcb-122">You may see a message saying there are recommended elastic pools for hello server.</span></span> <span data-ttu-id="3abcb-123">Klikněte na tlačítko hello zpráva toosee hello doporučuje vytvoření fondů založená na historické telemetrii využití databází, klikněte na tlačítko hello vrstvy toosee další podrobnosti a přizpůsobit hello fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-123">Click hello message toosee hello recommended pools based on historical database usage telemetry, and then click hello tier toosee more details and customize hello pool.</span></span> <span data-ttu-id="3abcb-124">V tématu [vysvětlení doporučení k fondům](#understand-elastic-pool-recommendations) dál v tomto tématu pro jak se provádí doporučení hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-124">See [Understand pool recommendations](#understand-elastic-pool-recommendations) later in this topic for how hello recommendation is made.</span></span>

    ![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. <span data-ttu-id="3abcb-126">Hello **elastický fond** se zobrazí okno, což je, kde zadáte hello nastavení fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-126">hello **elastic pool** blade appears, which is where you specify hello settings for your pool.</span></span> <span data-ttu-id="3abcb-127">Pokud jste klikli na **nový fond** v předchozím kroku hello hello cenová úroveň je **standardní** je vybrána ve výchozím nastavení a žádné databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-127">If you clicked **New pool** in hello previous step, hello pricing tier is **Standard** by default and no databases is selected.</span></span> <span data-ttu-id="3abcb-128">Můžete vytvořit fond prázdný, nebo zadejte existující databáze z tohoto serveru toomove do fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-128">You can create an empty pool, or specify a set of existing databases from that server toomove into hello pool.</span></span> <span data-ttu-id="3abcb-129">Pokud vytváříte doporučený fond, hello doporučená cenovou úroveň, nastavení výkonu a jsou předem seznam databází, ale můžete je stále změnit.</span><span class="sxs-lookup"><span data-stu-id="3abcb-129">If you are creating a recommended pool, hello recommended pricing tier, performance settings, and list of databases are prepopulated, but you can still change them.</span></span>

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. <span data-ttu-id="3abcb-131">Zadejte název pro hello elastického fondu nebo ponechte výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-131">Specify a name for hello elastic pool, or leave it as hello default.</span></span>

### <a name="step-2-choose-a-pricing-tier"></a><span data-ttu-id="3abcb-132">Krok 2: Zvolte cenovou úroveň</span><span class="sxs-lookup"><span data-stu-id="3abcb-132">Step 2: Choose a pricing tier</span></span>

<span data-ttu-id="3abcb-133">Hello cenová úroveň fondu určuje hello funkce dostupné toohello elastics v hello fondu a hello maximální počet jednotek Edtu (eDTU MAX) a úložiště (GB) k dispozici tooeach databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-133">hello pool's pricing tier determines hello features available toohello elastics in hello pool, and hello maximum number of eDTUs (eDTU MAX), and storage (GBs) available tooeach database.</span></span> <span data-ttu-id="3abcb-134">Podrobnosti viz Úrovně služeb</span><span class="sxs-lookup"><span data-stu-id="3abcb-134">For details, see Service Tiers.</span></span>

<span data-ttu-id="3abcb-135">Klikněte na tlačítko toochange hello cenovou úroveň pro fond hello **cenová úroveň**, klikněte na tlačítko hello cenové úrovně a pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-135">toochange hello pricing tier for hello pool, click **Pricing tier**, click hello pricing tier you want, and then click **Select**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3abcb-136">Pokud zvolíte hello cenová úroveň a potvrdíte svoji volbu kliknutím na **OK** v posledním kroku hello, nebude možné toochange hello cenovou úroveň fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-136">After you choose hello pricing tier and commit your changes by clicking **OK** in hello last step, you won't be able toochange hello pricing tier of hello pool.</span></span> <span data-ttu-id="3abcb-137">toochange hello cenovou úroveň existujícího elastického fondu, vytvoření fondu elastické databáze v hello požadovanou cenovou úrovní a migrace hello databáze toothis nový fond.</span><span class="sxs-lookup"><span data-stu-id="3abcb-137">toochange hello pricing tier for an existing elastic pool, create an elastic pool in hello desired pricing tier and migrate hello databases toothis new pool.</span></span>
>

![Výběr cenové úrovně](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-hello-pool"></a><span data-ttu-id="3abcb-139">Krok 3: Konfigurace fondu hello</span><span class="sxs-lookup"><span data-stu-id="3abcb-139">Step 3: Configure hello pool</span></span>

<span data-ttu-id="3abcb-140">Po nastavení hello cenové úrovně, klikněte na tlačítko Konfigurovat fond přidat databáze, sada fond hodnoty Edtu a úložiště (GB) a nastavíte hello min a max Edtu pro hello elastics ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-140">After setting hello pricing tier, click Configure pool where you add databases, set pool eDTUs and storage (pool GBs), and where you set hello min and max eDTUs for hello elastics in hello pool.</span></span>

1. <span data-ttu-id="3abcb-141">Klikněte na **Konfigurovat fond**</span><span class="sxs-lookup"><span data-stu-id="3abcb-141">Click **Configure pool**</span></span>
2. <span data-ttu-id="3abcb-142">Vyberte databáze hello chcete tooadd toohello fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-142">Select hello databases you want tooadd toohello pool.</span></span> <span data-ttu-id="3abcb-143">Tento krok je volitelný při vytváření fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-143">This step is optional while creating hello pool.</span></span> <span data-ttu-id="3abcb-144">Databáze lze přidat po vytvoření fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-144">Databases can be added after hello pool has been created.</span></span>
    <span data-ttu-id="3abcb-145">Klikněte na tlačítko tooadd databáze, **přidat databázi**, klikněte na tlačítko hello databáze má tooadd a pak klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3abcb-145">tooadd databases, click **Add database**, click hello databases that you want tooadd, and then click hello **Select** button.</span></span>

    ![Přidání databází](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    <span data-ttu-id="3abcb-147">Pokud pracujete s databází hello mají dostatek historických telemetrických dat, hello **Odhadované využití eDTU a GB** grafu a hello **skutečné využití eDTU** toohelp aktualizace pruhový graf provedete konfiguraci rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="3abcb-147">If hello databases you're working with have enough historical usage telemetry, hello **Estimated eDTU and GB usage** graph and hello **Actual eDTU usage** bar chart update toohelp you make configuration decisions.</span></span> <span data-ttu-id="3abcb-148">Navíc služba hello může zobrazovat toohelp zprávy doporučení je optimální velikost fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-148">Also, hello service may give you a recommendation message toohelp you right-size hello pool.</span></span> <span data-ttu-id="3abcb-149">Viz část [Dynamická doporučení](#understand-elastic-pool-recommendations).</span><span class="sxs-lookup"><span data-stu-id="3abcb-149">See [Dynamic Recommendations](#understand-elastic-pool-recommendations).</span></span>

3. <span data-ttu-id="3abcb-150">Pomocí ovládacích prvků hello na hello **konfigurace fondu** stránka tooexplore nastavení a konfigurovat fond.</span><span class="sxs-lookup"><span data-stu-id="3abcb-150">Use hello controls on hello **Configure pool** page tooexplore settings and configure your pool.</span></span> <span data-ttu-id="3abcb-151">V tématu [omezení Elastických fondů](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) další podrobnosti o omezeních pro jednotlivé úrovně služby a najdete v tématu [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool.md) podrobné pokyny k optimalizaci velikosti fondu elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-151">See [Elastic pools limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for more detail about limits for each service tier, and see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md) for detailed guidance on right-sizing an elastic pool.</span></span> <span data-ttu-id="3abcb-152">Další informace o nastavení fondu najdete v tématu [elastického fondu vlastnosti](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span><span class="sxs-lookup"><span data-stu-id="3abcb-152">For more information about pool settings, see [Elastic pool properties](sql-database-elastic-pool.md#database-properties-for-pooled-databases).</span></span>

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. <span data-ttu-id="3abcb-154">Klikněte na tlačítko **vyberte** v hello **konfigurace fondu** okno po změně nastavení.</span><span class="sxs-lookup"><span data-stu-id="3abcb-154">Click **Select** in hello **Configure Pool** blade after changing settings.</span></span>
5. <span data-ttu-id="3abcb-155">Klikněte na tlačítko **OK** toocreate hello fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-155">Click **OK** toocreate hello pool.</span></span>

## <a name="understand-elastic-pool-recommendations"></a><span data-ttu-id="3abcb-156">Vysvětlení doporučení k elastického fondu</span><span class="sxs-lookup"><span data-stu-id="3abcb-156">Understand elastic pool recommendations</span></span>

<span data-ttu-id="3abcb-157">Hello služba SQL Database vyhodnocuje historii využití a doporučí jeden nebo více fondů, pokud bude cenově výhodnější než použití izolované databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-157">hello SQL Database service evaluates usage history and recommends one or more pools when it is more cost-effective than using single databases.</span></span> <span data-ttu-id="3abcb-158">Každé doporučení je konfigurováno pro jedinečnou podmnožinu databází serveru hello, které co nejlépe vyhovovaly hello fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-158">Each recommendation is configured with a unique subset of hello server's databases that best fit hello pool.</span></span>

![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

<span data-ttu-id="3abcb-160">Hello doporučení fondu zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="3abcb-160">hello pool recommendation comprises:</span></span>

- <span data-ttu-id="3abcb-161">Cenovou úroveň pro hello fond (Basic, Standard, Premium nebo Premium RS)</span><span class="sxs-lookup"><span data-stu-id="3abcb-161">A pricing tier for hello pool (Basic, Standard, Premium, or Premium RS)</span></span>
- <span data-ttu-id="3abcb-162">vhodnou hodnotu **POOL eDTU** (označovanou také jako Max eDTU pro fond),</span><span class="sxs-lookup"><span data-stu-id="3abcb-162">Appropriate **POOL eDTUs** (also called Max eDTUs per pool)</span></span>
- <span data-ttu-id="3abcb-163">Hello **eDTU MAX** a **eDTU Min** na databázi</span><span class="sxs-lookup"><span data-stu-id="3abcb-163">hello **eDTU MAX** and **eDTU Min** per database</span></span>
- <span data-ttu-id="3abcb-164">Hello seznam doporučených databází pro fond hello</span><span class="sxs-lookup"><span data-stu-id="3abcb-164">hello list of recommended databases for hello pool</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3abcb-165">Služba Hello bere hello posledních 30 dnech telemetrie v úvahu při doporučujeme fondy.</span><span class="sxs-lookup"><span data-stu-id="3abcb-165">hello service takes hello last 30 days of telemetry into account when recommending pools.</span></span> <span data-ttu-id="3abcb-166">Pro databáze toobe, který zařazena mezi doporučené fondu elastické databáze musí existovat alespoň 7 dní.</span><span class="sxs-lookup"><span data-stu-id="3abcb-166">For a database toobe considered as a candidate for an elastic pool, it must exist for at least 7 days.</span></span> <span data-ttu-id="3abcb-167">Databáze, které již v elastickém fondu jsou, se nepovažují za kandidáty pro doporučení elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-167">Databases that are already in an elastic pool are not considered as candidates for elastic pool recommendations.</span></span>
>

<span data-ttu-id="3abcb-168">Hello služba vyhodnocuje potřebné prostředky a nákladů plynoucí z přesunutí hello jedné databáze na jednotlivých úrovních služby do fondů hello stejné úrovně.</span><span class="sxs-lookup"><span data-stu-id="3abcb-168">hello service evaluates resource needs and cost effectiveness of moving hello single databases in each service tier into pools of hello same tier.</span></span> <span data-ttu-id="3abcb-169">Například pro všechny databáze na serveru, které jsou v úrovni Standard, se zvažuje výhodnost jejich přesunu do elastického fondu také s úrovní Standard.</span><span class="sxs-lookup"><span data-stu-id="3abcb-169">For example, all Standard databases on a server are assessed for their fit into a Standard Elastic Pool.</span></span> <span data-ttu-id="3abcb-170">To znamená, že služba hello neprovádí vrstvě mezi úrovněmi, například přesun standardní databáze ve fondu Premium.</span><span class="sxs-lookup"><span data-stu-id="3abcb-170">This means hello service does not make cross-tier recommendations such as moving a Standard database into a Premium pool.</span></span>

<span data-ttu-id="3abcb-171">Po přidání databází toohello fondu, doporučení se dynamicky vygeneruje, na základě historie využití hello hello databází, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="3abcb-171">After adding databases toohello pool, recommendations are dynamically generated based on hello historical usage of hello databases you have selected.</span></span> <span data-ttu-id="3abcb-172">Tato doporučení jsou zobrazeny v hello eDTU a GB využití graf a v hlavičce doporučení hello horní části hello **konfigurace fondu** okno.</span><span class="sxs-lookup"><span data-stu-id="3abcb-172">These recommendations are shown in hello eDTU and GB usage chart and in a recommendation banner at hello top of hello **Configure pool** blade.</span></span> <span data-ttu-id="3abcb-173">Tato doporučení jsou určený tooassist můžete při vytváření fondu elastické databáze optimalizované pro vaše konkrétní databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-173">These recommendations are intended tooassist you in creating an elastic pool optimized for your specific databases.</span></span>

![dynamická doporučení](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## <a name="manage-and-monitor-an-elastic-pool"></a><span data-ttu-id="3abcb-175">Správa a sledování fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="3abcb-175">Manage and monitor an elastic pool</span></span>

<span data-ttu-id="3abcb-176">Můžete použít hello Azure portálu toomonitor a správa fondu elastické databáze a hello databáze ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-176">You can use hello Azure portal toomonitor and manage an elastic pool and hello databases in hello pool.</span></span> <span data-ttu-id="3abcb-177">Z portálu hello můžete sledovat využití hello fondu elastické databáze a databáze hello v tomto fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-177">From hello portal, you can monitor hello utilization of an elastic pool and hello databases within that pool.</span></span> <span data-ttu-id="3abcb-178">Můžete také nastavit sadu změn tooyour elastický fond a odeslat všechny změny v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="3abcb-178">You can also make a set of changes tooyour elastic pool and submit all changes at hello same time.</span></span> <span data-ttu-id="3abcb-179">Tyto změny zahrnují přidávání nebo odebírání databáze, změna nastavení služby elastického fondu nebo změna nastavení databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-179">These changes include adding or removing databases, changing your elastic pool settings, or changing your database settings.</span></span>

<span data-ttu-id="3abcb-180">Hello následující obrázek znázorňuje příklad elastickém fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-180">hello following graphic shows an example elastic pool.</span></span> <span data-ttu-id="3abcb-181">zobrazení Hello zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="3abcb-181">hello view includes:</span></span>

*  <span data-ttu-id="3abcb-182">Grafy pro sledování využití prostředků hello elastického fondu a databáze hello obsažené ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-182">Charts for monitoring resource usage of both hello elastic pool and hello databases contained in hello pool.</span></span>
*  <span data-ttu-id="3abcb-183">Hello **konfigurace** fondu tlačítko toomake změny toohello elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-183">hello **Configure** pool button toomake changes toohello elastic pool.</span></span>
*  <span data-ttu-id="3abcb-184">Hello **vytvořit databázi** tlačítko, která vytváří databázi a přidává ji toohello aktuální elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-184">hello **Create database** button that creates a database and adds it toohello current elastic pool.</span></span>
*  <span data-ttu-id="3abcb-185">Elastické úlohy, které vám pomůžou spravovat velké počty databází pomocí spouštění skriptů Transact SQL pro všechny databáze v seznamu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-185">Elastic jobs that help you manage large numbers of databases by running Transact SQL scripts against all databases in a list.</span></span>

![Zobrazení fondu][2]

<span data-ttu-id="3abcb-187">Můžete přejít tooa konkrétní fond toosee jeho využití prostředků.</span><span class="sxs-lookup"><span data-stu-id="3abcb-187">You can go tooa particular pool toosee its resource utilization.</span></span> <span data-ttu-id="3abcb-188">Ve výchozím nastavení hello fond je nakonfigurované tooshow využití úložiště a eDTU pro hello poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-188">By default, hello pool is configured tooshow storage and eDTU usage for hello last hour.</span></span> <span data-ttu-id="3abcb-189">Hello graf může být nakonfigurované tooshow jiné metriky v různých časových oken.</span><span class="sxs-lookup"><span data-stu-id="3abcb-189">hello chart can be configured tooshow different metrics over various time windows.</span></span>

1. <span data-ttu-id="3abcb-190">Vyberte toowork elastický fond s.</span><span class="sxs-lookup"><span data-stu-id="3abcb-190">Select an elastic pool toowork with.</span></span>
2. <span data-ttu-id="3abcb-191">V části **elastického fondu monitorování** je graf s názvem bez přípony **využití prostředků**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-191">Under **Elastic Pool Monitoring** is a chart labeled **Resource utilization**.</span></span> <span data-ttu-id="3abcb-192">Klikněte na tlačítko hello grafu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-192">Click hello chart.</span></span>

    ![Monitorování elastického fondu][3]

    <span data-ttu-id="3abcb-194">Hello **metrika** otevře se okno, zobrazuje podrobný přehled o hello zadat metriky přes hello zadaný časový interval.</span><span class="sxs-lookup"><span data-stu-id="3abcb-194">hello **Metric** blade opens, showing a detailed view of hello specified metrics over hello specified time window.</span></span>   

    ![Okno Metrika][9]

### <a name="toocustomize-hello-chart-display"></a><span data-ttu-id="3abcb-196">zobrazení grafu toocustomize hello</span><span class="sxs-lookup"><span data-stu-id="3abcb-196">toocustomize hello chart display</span></span>

<span data-ttu-id="3abcb-197">Graf hello a hello metriky okno toodisplay můžete upravit jiné metriky jako procento, procento vstupů/výstupů dat a protokolu vstupně-výstupní operace % využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="3abcb-197">You can edit hello chart and hello metric blade toodisplay other metrics such as CPU percentage, data IO percentage, and log IO percentage used.</span></span>

1. <span data-ttu-id="3abcb-198">V okně metriky hello, klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-198">On hello metric blade, click **Edit**.</span></span>

    ![Klikněte na tlačítko Upravit][6]

2. <span data-ttu-id="3abcb-200">V hello **upravit graf** okně vyberte časový interval (po hodině, ještě dnes, nebo za minulý týden), nebo klikněte na **vlastní** tooselect libovolné datum v rozsahu v hello poslední dva týdny.</span><span class="sxs-lookup"><span data-stu-id="3abcb-200">In hello **Edit Chart** blade, select a time range (past hour, today, or past week), or click **custom** tooselect any date range in hello last two weeks.</span></span> <span data-ttu-id="3abcb-201">Vyberte typ grafu hello (pruhu nebo čáry) a potom vyberte toomonitor prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-201">Select hello chart type (bar or line), then select hello resources toomonitor.</span></span>

   > [!Note]
   > <span data-ttu-id="3abcb-202">Pouze metriky s hello tutéž jednotku lze zobrazit v hello grafu v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="3abcb-202">Only metrics with hello same unit of measure can be displayed in hello chart at hello same time.</span></span> <span data-ttu-id="3abcb-203">Například pokud zvolíte možnost "eDTU procento" pak můžete zvolit pouze další metriky s procentem jako měrné jednotky hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-203">For example, if you select "eDTU percentage" then you can only select other metrics with percentage as hello unit of measure.</span></span>
   >

    ![Klikněte na tlačítko Upravit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. <span data-ttu-id="3abcb-205">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-205">Then click **OK**.</span></span>

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a><span data-ttu-id="3abcb-206">Spravovat a monitorovat databází v elastickém fondu</span><span class="sxs-lookup"><span data-stu-id="3abcb-206">Manage and monitor databases in an elastic pool</span></span>

<span data-ttu-id="3abcb-207">Jednotlivé databáze je možné monitorovat také pro potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="3abcb-207">Individual databases can also be monitored for potential trouble.</span></span>

1. <span data-ttu-id="3abcb-208">V části **elastické databáze monitorování**, je graf, který zobrazí metriky pro pět databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-208">Under **Elastic Database Monitoring**, there is a chart that displays metrics for five databases.</span></span> <span data-ttu-id="3abcb-209">Ve výchozím nastavení, hello graf zobrazuje hello top 5 databáze ve fondu hello podle využití eDTU průměrná v hello poslední hodinu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-209">By default, hello chart displays hello top 5 databases in hello pool by average eDTU usage in hello past hour.</span></span> <span data-ttu-id="3abcb-210">Klikněte na tlačítko hello grafu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-210">Click hello chart.</span></span>

    ![Monitorování elastického fondu][4]

2. <span data-ttu-id="3abcb-212">Hello **využití prostředků databáze** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="3abcb-212">hello **Database Resource Utilization** blade appears.</span></span> <span data-ttu-id="3abcb-213">To poskytuje podrobný přehled o využití hello databáze ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-213">This provides a detailed view of hello database usage in hello pool.</span></span> <span data-ttu-id="3abcb-214">Pomocí hello mřížky v dolní části okna hello hello, můžete vybrat všechny databáze ve fondu toodisplay hello jeho použití v grafu hello (až too5 databáze).</span><span class="sxs-lookup"><span data-stu-id="3abcb-214">Using hello grid in hello lower part of hello blade, you can select any databases in hello pool toodisplay its usage in hello chart (up too5 databases).</span></span> <span data-ttu-id="3abcb-215">Můžete také upravit hello metriky a časového okna zobrazí v grafu hello kliknutím **upravit graf**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-215">You can also customize hello metrics and time window displayed in hello chart by clicking **Edit chart**.</span></span>

    ![Okna využití prostředků databáze][8]

### <a name="toocustomize-hello-view"></a><span data-ttu-id="3abcb-217">zobrazení toocustomize hello</span><span class="sxs-lookup"><span data-stu-id="3abcb-217">toocustomize hello view</span></span>

1. <span data-ttu-id="3abcb-218">V hello **databáze využití prostředků** okně klikněte na tlačítko **upravit graf**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-218">In hello **Database resource utilization** blade, click **Edit chart**.</span></span>

    ![Klikněte na tlačítko Upravit graf](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. <span data-ttu-id="3abcb-220">V hello **upravit** grafu okně, vyberte časové rozmezí (za hodinu nebo za posledních 24 hodin) nebo klikněte na **vlastní** tooselect různých denně v hello po toodisplay 2 týdny.</span><span class="sxs-lookup"><span data-stu-id="3abcb-220">In hello **Edit** chart blade, select a time range (past hour or past 24 hours), or click **custom** tooselect a different day in hello past 2 weeks toodisplay.</span></span>

    ![Klikněte na tlačítko Vlastní](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. <span data-ttu-id="3abcb-222">Klikněte na tlačítko hello **porovnat databází tak, že** rozevírací tooselect jiné metriky toouse při porovnávání databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-222">Click hello **Compare databases by** dropdown tooselect a different metric toouse when comparing databases.</span></span>

    ![Upravit graf hello](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a><span data-ttu-id="3abcb-224">toomonitor tooselect databáze</span><span class="sxs-lookup"><span data-stu-id="3abcb-224">tooselect databases toomonitor</span></span>

<span data-ttu-id="3abcb-225">V seznamu hello databáze v hello **využití prostředků databáze** okno, můžete vyhledat konkrétní databáze hello stránky v seznamu hello zaměřením nebo zadáním v hello název databáze.</span><span class="sxs-lookup"><span data-stu-id="3abcb-225">In hello database list in hello **Database Resource Utilization** blade, you can find particular databases by looking through hello pages in hello list or by typing in hello name of a database.</span></span> <span data-ttu-id="3abcb-226">Použijte hello políčko tooselect hello databázi.</span><span class="sxs-lookup"><span data-stu-id="3abcb-226">Use hello checkbox tooselect hello database.</span></span>

![Vyhledejte toomonitor databáze][7]


## <a name="add-an-alert-tooan-elastic-pool-resource"></a><span data-ttu-id="3abcb-228">Přidat prostředek výstrahy tooan elastického fondu</span><span class="sxs-lookup"><span data-stu-id="3abcb-228">Add an alert tooan elastic pool resource</span></span>

<span data-ttu-id="3abcb-229">Můžete přidat tooan pravidla elastické se fond, který toopeople nebo výstrahu tooURL koncových bodů řetězce odeslání e-mailu, pokud se elastický fond hello dotkne prahovou hodnotu využití, které jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="3abcb-229">You can add rules tooan elastic pool that send email toopeople or alert strings tooURL endpoints when hello elastic pool hits a utilization threshold that you set up.</span></span>

<span data-ttu-id="3abcb-230">**tooadd prostředek tooany výstrahy:**</span><span class="sxs-lookup"><span data-stu-id="3abcb-230">**tooadd an alert tooany resource:**</span></span>

1. <span data-ttu-id="3abcb-231">Klikněte na tlačítko hello **využití prostředků** grafu tooopen hello **metrika** okně klikněte na tlačítko **přidat upozornění**a potom vyplňte informace hello v hello **přidat oznámení pravidlo** okno (**prostředků** je automaticky nastavení fondu hello toobe pracujete s).</span><span class="sxs-lookup"><span data-stu-id="3abcb-231">Click hello **Resource utilization** chart tooopen hello **Metric** blade, click **Add alert**, and then fill out hello information in hello **Add an alert rule** blade (**Resource** is automatically set up toobe hello pool you're working with).</span></span>
2. <span data-ttu-id="3abcb-232">Zadejte **název** a **popis** identifikující tooyou hello výstrah a příjemce hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-232">Type a **Name** and **Description** that identifies hello alert tooyou and hello recipients.</span></span>
3. <span data-ttu-id="3abcb-233">Vyberte **metrika** , které chcete tooalert hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-233">Choose a **Metric** that you want tooalert from hello list.</span></span>

    <span data-ttu-id="3abcb-234">Graf Hello dynamicky znázorňuje využití prostředků pro tuto metriky toohelp, že si vyberete prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-234">hello chart dynamically shows resource utilization for that metric toohelp you choose a threshold.</span></span>

4. <span data-ttu-id="3abcb-235">Vyberte **podmínku** (větší než menší než atd) a **prahová hodnota**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-235">Choose a **Condition** (greater than, less than, etc.) and a **Threshold**.</span></span>
5. <span data-ttu-id="3abcb-236">Vyberte **období** času, který hello metrika pravidlo je nutné splnit před hello výstrahy aktivační události.</span><span class="sxs-lookup"><span data-stu-id="3abcb-236">Choose a **Period** of time that hello metric rule must be satisfied before hello alert triggers.</span></span>
6. <span data-ttu-id="3abcb-237">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-237">Click **OK**.</span></span>

<span data-ttu-id="3abcb-238">Další informace najdete v tématu [vytvářet výstrahy, SQL Database na portálu Azure](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3abcb-238">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="3abcb-239">Přesun databáze do pružného fondu</span><span class="sxs-lookup"><span data-stu-id="3abcb-239">Move a database into an elastic pool</span></span>

<span data-ttu-id="3abcb-240">Můžete přidat nebo odebrat databáze z existující fond.</span><span class="sxs-lookup"><span data-stu-id="3abcb-240">You can add or remove databases from an existing pool.</span></span> <span data-ttu-id="3abcb-241">Hello databáze může být v jiných fondech.</span><span class="sxs-lookup"><span data-stu-id="3abcb-241">hello databases can be in other pools.</span></span> <span data-ttu-id="3abcb-242">Však lze přidat pouze databáze, které jsou na hello stejný logický server.</span><span class="sxs-lookup"><span data-stu-id="3abcb-242">However, you can only add databases that are on hello same logical server.</span></span>

1. <span data-ttu-id="3abcb-243">V okně hello hello fondu v části **elastické databáze** klikněte na tlačítko **konfigurace fondu**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-243">In hello blade for hello pool, under **Elastic databases** click **Configure pool**.</span></span>

    ![Klikněte na tlačítko Konfigurovat fond][1]

2. <span data-ttu-id="3abcb-245">V hello **konfigurace fondu** okně klikněte na tlačítko **přidat toopool**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-245">In hello **Configure pool** blade, click **Add toopool**.</span></span>

    ![Klikněte na tlačítko Přidat toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. <span data-ttu-id="3abcb-247">V hello **přidat databáze** okně, vyberte hello databáze nebo databáze tooadd toohello fondu.</span><span class="sxs-lookup"><span data-stu-id="3abcb-247">In hello **Add databases** blade, select hello database or databases tooadd toohello pool.</span></span> <span data-ttu-id="3abcb-248">Pak klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-248">Then click **Select**.</span></span>

    ![Vyberte tooadd databáze](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    <span data-ttu-id="3abcb-250">Hello **konfigurace fondu** okno teď seznamy hello databáze, které jste vybrali toobe přidali, se její stav nastavit příliš**čekající**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-250">hello **Configure pool** blade now lists hello database you selected toobe added, with its status set too**Pending**.</span></span>

    ![Přidání čekající fondu](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. <span data-ttu-id="3abcb-252">V hello **konfigurovat fond okno**, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-252">In hello **Configure pool blade**, click **Save**.</span></span>

    ![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a><span data-ttu-id="3abcb-254">Přesunutí databáze z fondu elastické databáze</span><span class="sxs-lookup"><span data-stu-id="3abcb-254">Move a database out of an elastic pool</span></span>

1. <span data-ttu-id="3abcb-255">V hello **konfigurace fondu** okně, vyberte hello databáze nebo databáze tooremove.</span><span class="sxs-lookup"><span data-stu-id="3abcb-255">In hello **Configure pool** blade, select hello database or databases tooremove.</span></span>

    ![výpis databáze](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. <span data-ttu-id="3abcb-257">Klikněte na tlačítko **odebrat z fondu**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-257">Click **Remove from pool**.</span></span>

    ![výpis databáze](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    <span data-ttu-id="3abcb-259">Hello **konfigurace fondu** okno teď seznamy hello databáze, které jste vybrali toobe odebrány s její stav nastavit příliš**čekající**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-259">hello **Configure pool** blade now lists hello database you selected toobe removed with its status set too**Pending**.</span></span>

    ![Náhled databáze přidávání a odebírání](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. <span data-ttu-id="3abcb-261">V hello **konfigurovat fond okno**, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3abcb-261">In hello **Configure pool blade**, click **Save**.</span></span>

    ![Kliknutí na Uložit](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="3abcb-263">Změňte nastavení výkonu elastického fondu</span><span class="sxs-lookup"><span data-stu-id="3abcb-263">Change performance settings of an elastic pool</span></span>

<span data-ttu-id="3abcb-264">Při sledování využití prostředků hello elastického fondu, může se stát, že některé změny jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="3abcb-264">As you monitor hello resource utilization of an elastic pool, you may discover that some adjustments are needed.</span></span> <span data-ttu-id="3abcb-265">Možná hello fondu musí změnu omezení hello výkon nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="3abcb-265">Maybe hello pool needs a change in hello performance or storage limits.</span></span> <span data-ttu-id="3abcb-266">Pravděpodobně budete chtít toochange hello nastavení databáze ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-266">Possibly you want toochange hello database settings in hello pool.</span></span> <span data-ttu-id="3abcb-267">Můžete změnit nastavení hello hello fondu na všechny čas tooget hello nejlepší kompromis výkonu a nákladů.</span><span class="sxs-lookup"><span data-stu-id="3abcb-267">You can change hello setup of hello pool at any time tooget hello best balance of performance and cost.</span></span> <span data-ttu-id="3abcb-268">V tématu [při fondu elastické databáze slouží?](sql-database-elastic-pool.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3abcb-268">See [When should an elastic pool be used?](sql-database-elastic-pool.md) for more information.</span></span>

<span data-ttu-id="3abcb-269">toochange hello Edtu nebo úložiště, omezuje na fond a Edtu na databázi:</span><span class="sxs-lookup"><span data-stu-id="3abcb-269">toochange hello eDTUs or storage limits per pool, and eDTUs per database:</span></span>

1. <span data-ttu-id="3abcb-270">Otevřete hello **konfigurace fondu** okno.</span><span class="sxs-lookup"><span data-stu-id="3abcb-270">Open hello **Configure pool** blade.</span></span>

    <span data-ttu-id="3abcb-271">V části **elastického fondu nastavení**, použijte buď nastavení fondu hello toochange posuvníku.</span><span class="sxs-lookup"><span data-stu-id="3abcb-271">Under **elastic pool settings**, use either slider toochange hello pool settings.</span></span>

    ![Využití elastického fondu prostředků](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. <span data-ttu-id="3abcb-273">Při změně nastavení hello zobrazuje zobrazení hello hello odhadované měsíční náklady na změny hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-273">When hello setting changes, hello display shows hello estimated monthly cost of hello change.</span></span>

    ![Aktualizace služby elastického fondu a měsíční náklady na nový](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="3abcb-275">Latence operací elastického fondu</span><span class="sxs-lookup"><span data-stu-id="3abcb-275">Latency of elastic pool operations</span></span>
* <span data-ttu-id="3abcb-276">Změna hello minimální počet jednotek Edtu na databázi nebo max Edtu na databázi obvykle dokončení během 5 minut nebo méně.</span><span class="sxs-lookup"><span data-stu-id="3abcb-276">Changing hello min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="3abcb-277">Změna hello Edtu na fond závisí na celkovém množství místo využité všechny databáze ve fondu hello hello.</span><span class="sxs-lookup"><span data-stu-id="3abcb-277">Changing hello eDTUs per pool depends on hello total amount of space used by all databases in hello pool.</span></span> <span data-ttu-id="3abcb-278">Změny průměrně trvají méně než 90 minut na každých 100 GB.</span><span class="sxs-lookup"><span data-stu-id="3abcb-278">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="3abcb-279">Například pokud celkové místo hello používá všechny databáze ve fondu hello je 200 GB, pak hello očekává, že latence pro změnu eDTU fondu hello na fondu je 3 hodiny nebo méně.</span><span class="sxs-lookup"><span data-stu-id="3abcb-279">For example, if hello total space used by all databases in hello pool is 200 GB, then hello expected latency for changing hello pool eDTU per pool is 3 hours or less.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3abcb-280">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3abcb-280">Next steps</span></span>

- <span data-ttu-id="3abcb-281">jaké fondu elastické databáze je, najdete v části toounderstand [elastického fondu SQL Database](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="3abcb-281">toounderstand what an elastic pool is, see [SQL Database elastic pool](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="3abcb-282">Pokyny k používání elastické fondy najdete v tématu [cenové a výkonové požadavky pro elastické fondy](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="3abcb-282">For guidance on using elastic pools, see [Price and performance considerations for elastic pools](sql-database-elastic-pool.md).</span></span>
- <span data-ttu-id="3abcb-283">toouse elastické úlohy toorun Transact-SQL skriptů pro libovolný počet databází ve fondu hello najdete [elastické úlohy přehled](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3abcb-283">toouse elastic jobs toorun Transact-SQL scripts against any number of databases in hello pool, see [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span>
- <span data-ttu-id="3abcb-284">najdete v části tooquery napříč libovolný počet databází ve fondu hello [elastické dotazu přehled](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3abcb-284">tooquery across any number of databases in hello pool, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
- <span data-ttu-id="3abcb-285">Transakce libovolný počet databází ve fondu hello, najdete v části [elastické transakce](sql-database-elastic-transactions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3abcb-285">For transactions any number of databases in hello pool, see [Elastic transactions](sql-database-elastic-transactions-overview.md).</span></span>


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
