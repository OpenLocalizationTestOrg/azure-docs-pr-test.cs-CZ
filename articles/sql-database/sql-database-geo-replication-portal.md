---
title: "Portál Azure: geografická replikace databáze SQL | Microsoft Docs"
description: "Konfigurace geografická replikace pro Azure SQL Database v hello portál Azure a inicializovat převzetí služeb při selhání"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d0b29822-714f-4633-a5ab-fb1a09d43ced
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/06/2016
ms.author: carlrab
ms.openlocfilehash: 09cbbdb040f36c42593e3be87ce6db2238f36656
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-hello-azure-portal-and-initiate-failover"></a><span data-ttu-id="4f597-103">Konfigurace aktivní geografickou replikací pro Azure SQL Database v hello portál Azure a inicializovat převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="4f597-103">Configure active geo-replication for Azure SQL Database in hello Azure portal and initiate failover</span></span>

<span data-ttu-id="4f597-104">Tento článek ukazuje, jak tooconfigure active geografická replikace pro databázi SQL v hello [portál Azure](http://portal.azure.com) a tooinitiate převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="4f597-104">This article shows you how tooconfigure active geo-replication for SQL Database in hello [Azure portal](http://portal.azure.com) and tooinitiate failover.</span></span>

<span data-ttu-id="4f597-105">převzetí služeb při selhání tooinitiate hello portál Azure, najdete v části [zahájení plánovaném nebo neplánovaném převzetí služeb při selhání pro databázi SQL Azure s hello portál Azure](sql-database-geo-replication-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4f597-105">tooinitiate failover with hello Azure portal, see [Initiate a planned or unplanned failover for Azure SQL Database with hello Azure portal](sql-database-geo-replication-portal.md).</span></span>

<span data-ttu-id="4f597-106">tooconfigure aktivní geografickou replikaci pomocí hello portálu Azure, je třeba hello následujících prostředků:</span><span class="sxs-lookup"><span data-stu-id="4f597-106">tooconfigure active geo-replication by using hello Azure portal, you need hello following resource:</span></span>

* <span data-ttu-id="4f597-107">Azure SQL database: hello primární databázi, které chcete tooreplicate tooa jiné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="4f597-107">An Azure SQL database: hello primary database that you want tooreplicate tooa different geographical region.</span></span>

> [!Note]
<span data-ttu-id="4f597-108">Aktivní geografickou replikaci musí být mezi databází v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="4f597-108">Active geo-replication must be between databases in hello same subscription.</span></span>

## <a name="add-a-secondary-database"></a><span data-ttu-id="4f597-109">Přidání sekundární databáze</span><span class="sxs-lookup"><span data-stu-id="4f597-109">Add a secondary database</span></span>
<span data-ttu-id="4f597-110">Hello následujících kroků vytvořte novou sekundární databázi ve spolupráci se geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="4f597-110">hello following steps create a new secondary database in a geo-replication partnership.</span></span>  

<span data-ttu-id="4f597-111">tooadd sekundární databáze, musí být spoluvlastník nebo vlastníka předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="4f597-111">tooadd a secondary database, you must be hello subscription owner or co-owner.</span></span>

<span data-ttu-id="4f597-112">sekundární databáze Hello má stejný název jako primární databáze hello hello a má, ve výchozím nastavení, hello stejné úrovně služeb.</span><span class="sxs-lookup"><span data-stu-id="4f597-112">hello secondary database has hello same name as hello primary database and has, by default, hello same service level.</span></span> <span data-ttu-id="4f597-113">Hello sekundární databáze může být jedné databáze nebo databáze ve fondu elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="4f597-113">hello secondary database can be a single database or a database in an elastic pool.</span></span> <span data-ttu-id="4f597-114">Další informace najdete v tématu [úrovních služeb](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="4f597-114">For more information, see [Service tiers](sql-database-service-tiers.md).</span></span>
<span data-ttu-id="4f597-115">Po hello sekundární se vytvoří a nasadí, data začne replikace z hello primární databáze toohello nové sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="4f597-115">After hello secondary is created and seeded, data begins replicating from hello primary database toohello new secondary database.</span></span>

> [!NOTE]
> <span data-ttu-id="4f597-116">Pokud už existuje databáze partnera hello (například. v důsledku ukončení předchozí relace geografická replikace) hello příkaz se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4f597-116">If hello partner database already exists (for example, as a result of terminating a previous geo-replication relationship) hello command fails.</span></span>
> 

1. <span data-ttu-id="4f597-117">V hello [portál Azure](http://portal.azure.com), procházet toohello databáze, které chcete tooset službu geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="4f597-117">In hello [Azure portal](http://portal.azure.com), browse toohello database that you want tooset up for geo-replication.</span></span>
2. <span data-ttu-id="4f597-118">Na stránce databáze SQL hello vyberte **geografická replikace**a pak vyberte hello oblast toocreate hello sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="4f597-118">On hello SQL database page, select **geo-replication**, and then select hello region toocreate hello secondary database.</span></span> <span data-ttu-id="4f597-119">Můžete vybrat všechny oblasti, než hello oblasti hostování hello primární databázi, ale doporučujeme hello [spárované oblast](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="4f597-119">You can select any region other than hello region hosting hello primary database, but we recommend hello [paired region](../best-practices-availability-paired-regions.md).</span></span>
   
    ![Konfigurace geografické replikace](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. <span data-ttu-id="4f597-121">Vyberte nebo nakonfigurovat hello server a cenovou úroveň pro hello sekundární databáze.</span><span class="sxs-lookup"><span data-stu-id="4f597-121">Select or configure hello server and pricing tier for hello secondary database.</span></span>
   
    ![Konfigurace sekundární](./media/sql-database-geo-replication-portal/create-secondary.png)
4. <span data-ttu-id="4f597-123">Volitelně můžete přidat sekundární databázi elastického fondu tooan.</span><span class="sxs-lookup"><span data-stu-id="4f597-123">Optionally, you can add a secondary database tooan elastic pool.</span></span> <span data-ttu-id="4f597-124">Klikněte na tlačítko toocreate hello sekundární databáze ve fondu, **elastický fond** a vyberte fond na cílovém serveru hello.</span><span class="sxs-lookup"><span data-stu-id="4f597-124">toocreate hello secondary database in a pool, click **elastic pool** and select a pool on hello target server.</span></span> <span data-ttu-id="4f597-125">Fond musí již existují na cílový server hello.</span><span class="sxs-lookup"><span data-stu-id="4f597-125">A pool must already exist on hello target server.</span></span> <span data-ttu-id="4f597-126">Tento pracovní postup nevytvoří fondu.</span><span class="sxs-lookup"><span data-stu-id="4f597-126">This workflow does not create a pool.</span></span>
5. <span data-ttu-id="4f597-127">Klikněte na tlačítko **vytvořit** tooadd hello sekundární.</span><span class="sxs-lookup"><span data-stu-id="4f597-127">Click **Create** tooadd hello secondary.</span></span>
6. <span data-ttu-id="4f597-128">Hello sekundární databáze byla vytvořena a zahájí se synchronizace replik indexů proces hello.</span><span class="sxs-lookup"><span data-stu-id="4f597-128">hello secondary database is created and hello seeding process begins.</span></span>
   
    ![Konfigurace sekundární](./media/sql-database-geo-replication-portal/seeding0.png)
7. <span data-ttu-id="4f597-130">Po dokončení synchronizace replik indexů proces hello hello sekundární databáze zobrazí její stav.</span><span class="sxs-lookup"><span data-stu-id="4f597-130">When hello seeding process is complete, hello secondary database displays its status.</span></span>
   
    ![Synchronizace replik indexů dokončení](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a><span data-ttu-id="4f597-132">Zahájení převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="4f597-132">Initiate a failover</span></span>

<span data-ttu-id="4f597-133">Hello sekundární databáze může být vypnuté toobecome hello primární.</span><span class="sxs-lookup"><span data-stu-id="4f597-133">hello secondary database can be switched toobecome hello primary.</span></span>  

1. <span data-ttu-id="4f597-134">V hello [portál Azure](http://portal.azure.com), procházet toohello primární databázi ve spolupráci se geografická replikace hello.</span><span class="sxs-lookup"><span data-stu-id="4f597-134">In hello [Azure portal](http://portal.azure.com), browse toohello primary database in hello geo-replication partnership.</span></span>
2. <span data-ttu-id="4f597-135">V okně hello SQL Database, vyberte **všechna nastavení** > **geografická replikace**.</span><span class="sxs-lookup"><span data-stu-id="4f597-135">On hello SQL Database blade, select **All settings** > **geo-replication**.</span></span>
3. <span data-ttu-id="4f597-136">V hello **SEKUNDÁRNÍCH** seznamu, vyberte hello databáze chcete toobecome hello nový primární a klikněte na tlačítko **převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="4f597-136">In hello **SECONDARIES** list, select hello database you want toobecome hello new primary and click **Failover**.</span></span>
   
    ![převzetí služeb při selhání](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. <span data-ttu-id="4f597-138">Klikněte na tlačítko **Ano** toobegin hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="4f597-138">Click **Yes** toobegin hello failover.</span></span>

<span data-ttu-id="4f597-139">příkaz Hello okamžitě přepínačů hello sekundární databáze do primární role hello.</span><span class="sxs-lookup"><span data-stu-id="4f597-139">hello command immediately switches hello secondary database into hello primary role.</span></span> 

<span data-ttu-id="4f597-140">Není k dispozici na krátkou dobu, během které obě databáze jsou k dispozici (v pořadí hello 0 too25 sekund), při přepínání hello role.</span><span class="sxs-lookup"><span data-stu-id="4f597-140">There is a short period during which both databases are unavailable (on hello order of 0 too25 seconds) while hello roles are switched.</span></span> <span data-ttu-id="4f597-141">Pokud hello primární databáze má více sekundární databáze, příkaz hello automaticky změní konfiguraci hello nový primární jiných sekundárních tooconnect toohello.</span><span class="sxs-lookup"><span data-stu-id="4f597-141">If hello primary database has multiple secondary databases, hello command automatically reconfigures hello other secondaries tooconnect toohello new primary.</span></span> <span data-ttu-id="4f597-142">Hello celou operaci lze provést méně než minutu toocomplete za normálních okolností.</span><span class="sxs-lookup"><span data-stu-id="4f597-142">hello entire operation should take less than a minute toocomplete under normal circumstances.</span></span> 

> [!NOTE]
> <span data-ttu-id="4f597-143">Tento příkaz je určená pro rychlé obnovení databáze hello v případě výpadku.</span><span class="sxs-lookup"><span data-stu-id="4f597-143">This command is designed for quick recovery of hello database in case of an outage.</span></span> <span data-ttu-id="4f597-144">Aktivuje převzetí služeb při selhání bez synchronizace dat (Vynucené převzetí služeb při selhání).</span><span class="sxs-lookup"><span data-stu-id="4f597-144">It triggers failover without data synchronization (forced failover).</span></span>  <span data-ttu-id="4f597-145">Pokud primární hello je online a potvrzením transakce při vydání příkazu hello ztrátu dat může dojít.</span><span class="sxs-lookup"><span data-stu-id="4f597-145">If hello primary is online and committing transactions when hello command is issued some data loss may occur.</span></span> 
> 
> 

## <a name="remove-secondary-database"></a><span data-ttu-id="4f597-146">Odebrání sekundární databáze</span><span class="sxs-lookup"><span data-stu-id="4f597-146">Remove secondary database</span></span>
<span data-ttu-id="4f597-147">Tato operace trvale ukončí hello toohello replikace do sekundární databáze a změny hello role hello sekundární tooa regulární databáze pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="4f597-147">This operation permanently terminates hello replication toohello secondary database, and changes hello role of hello secondary tooa regular read-write database.</span></span> <span data-ttu-id="4f597-148">Pokud je přerušený hello připojení toohello sekundární databáze, hello příkazu úspěšné, ale hello sekundární nemá ne stane pro čtení a zápis do doby, po obnovení připojení.</span><span class="sxs-lookup"><span data-stu-id="4f597-148">If hello connectivity toohello secondary database is broken, hello command succeeds but hello secondary does not become read-write until after connectivity is restored.</span></span>  

1. <span data-ttu-id="4f597-149">V hello [portál Azure](http://portal.azure.com), procházet toohello primární databázi ve spolupráci se geografická replikace hello.</span><span class="sxs-lookup"><span data-stu-id="4f597-149">In hello [Azure portal](http://portal.azure.com), browse toohello primary database in hello geo-replication partnership.</span></span>
2. <span data-ttu-id="4f597-150">Na stránce databáze SQL hello vyberte **geografická replikace**.</span><span class="sxs-lookup"><span data-stu-id="4f597-150">On hello SQL database page, select **geo-replication**.</span></span>
3. <span data-ttu-id="4f597-151">V hello **SEKUNDÁRNÍCH** seznamu, vyberte hello databáze chcete tooremove z hello geografická replikace partnerství.</span><span class="sxs-lookup"><span data-stu-id="4f597-151">In hello **SECONDARIES** list, select hello database you want tooremove from hello geo-replication partnership.</span></span>
4. <span data-ttu-id="4f597-152">Klikněte na tlačítko **zastavení replikace**.</span><span class="sxs-lookup"><span data-stu-id="4f597-152">Click **Stop Replication**.</span></span>
   
    ![Odeberte sekundární](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. <span data-ttu-id="4f597-154">Otevře se okno pro potvrzení.</span><span class="sxs-lookup"><span data-stu-id="4f597-154">A confirmation window opens.</span></span> <span data-ttu-id="4f597-155">Klikněte na tlačítko **Ano** tooremove hello databáze z hello geografická replikace partnerství.</span><span class="sxs-lookup"><span data-stu-id="4f597-155">Click **Yes** tooremove hello database from hello geo-replication partnership.</span></span> <span data-ttu-id="4f597-156">(Nastavit tooa pro čtení a zápis databáze nejsou součástí žádné replikace.)</span><span class="sxs-lookup"><span data-stu-id="4f597-156">(Set it tooa read-write database not part of any replication.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f597-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f597-157">Next steps</span></span>
* <span data-ttu-id="4f597-158">toolearn Další informace o aktivní geografickou replikaci, najdete v části [aktivní geografickou replikací](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4f597-158">toolearn more about active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md).</span></span>
* <span data-ttu-id="4f597-159">Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="4f597-159">For a business continuity overview and scenarios, see [Business continuity overview](sql-database-business-continuity.md).</span></span>

