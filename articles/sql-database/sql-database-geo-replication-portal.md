---
title: "Portál Azure: geografická replikace databáze SQL | Microsoft Docs"
description: "Konfigurace geografická replikace pro Azure SQL Database v portálu Azure a inicializovat převzetí služeb při selhání"
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
ms.openlocfilehash: db90fad2fe397f0c8466db6bdc1bd8c8d1cf8f15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a><span data-ttu-id="32136-103">Konfigurace aktivní geografickou replikací pro Azure SQL Database v portálu Azure a inicializovat převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="32136-103">Configure active geo-replication for Azure SQL Database in the Azure portal and initiate failover</span></span>

<span data-ttu-id="32136-104">V tomto článku se dozvíte, jak nakonfigurovat aktivní geografickou replikací pro databázi SQL v [portál Azure](http://portal.azure.com) a k zahájení převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="32136-104">This article shows you how to configure active geo-replication for SQL Database in the [Azure portal](http://portal.azure.com) and to initiate failover.</span></span>

<span data-ttu-id="32136-105">K zahájení převzetí služeb při selhání pomocí portálu Azure, najdete v části [zahájení plánovaném nebo neplánovaném převzetí služeb při selhání pro Azure SQL Database pomocí portálu Azure](sql-database-geo-replication-portal.md).</span><span class="sxs-lookup"><span data-stu-id="32136-105">To initiate failover with the Azure portal, see [Initiate a planned or unplanned failover for Azure SQL Database with the Azure portal](sql-database-geo-replication-portal.md).</span></span>

<span data-ttu-id="32136-106">Chcete-li nakonfigurovat aktivní geografickou replikaci pomocí portálu Azure, je třeba v následujícím zdroji:</span><span class="sxs-lookup"><span data-stu-id="32136-106">To configure active geo-replication by using the Azure portal, you need the following resource:</span></span>

* <span data-ttu-id="32136-107">Azure SQL database: primární databázi, kterou chcete replikovat do jiné zeměpisné oblasti.</span><span class="sxs-lookup"><span data-stu-id="32136-107">An Azure SQL database: The primary database that you want to replicate to a different geographical region.</span></span>

> [!Note]
<span data-ttu-id="32136-108">Aktivní geografickou replikaci musí být mezi databázemi ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="32136-108">Active geo-replication must be between databases in the same subscription.</span></span>

## <a name="add-a-secondary-database"></a><span data-ttu-id="32136-109">Přidání sekundární databáze</span><span class="sxs-lookup"><span data-stu-id="32136-109">Add a secondary database</span></span>
<span data-ttu-id="32136-110">Následující postup vytvoření nové sekundární databáze ve spolupráci se geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="32136-110">The following steps create a new secondary database in a geo-replication partnership.</span></span>  

<span data-ttu-id="32136-111">Pokud chcete přidat sekundární databáze, musí být spoluvlastník nebo vlastníka předplatného.</span><span class="sxs-lookup"><span data-stu-id="32136-111">To add a secondary database, you must be the subscription owner or co-owner.</span></span>

<span data-ttu-id="32136-112">Sekundární databáze má stejný název jako primární databáze a ve výchozím nastavení, má stejnou úroveň služby.</span><span class="sxs-lookup"><span data-stu-id="32136-112">The secondary database has the same name as the primary database and has, by default, the same service level.</span></span> <span data-ttu-id="32136-113">Sekundární databáze může být jedné databáze nebo databáze ve fondu elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="32136-113">The secondary database can be a single database or a database in an elastic pool.</span></span> <span data-ttu-id="32136-114">Další informace najdete v tématu [úrovních služeb](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="32136-114">For more information, see [Service tiers](sql-database-service-tiers.md).</span></span>
<span data-ttu-id="32136-115">Po sekundární se vytvoří a nasadí, data začne replikace z primární databáze do nové sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="32136-115">After the secondary is created and seeded, data begins replicating from the primary database to the new secondary database.</span></span>

> [!NOTE]
> <span data-ttu-id="32136-116">Pokud už existuje databáze partnera (například. v důsledku ukončení předchozí relace geografická replikace) příkaz se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="32136-116">If the partner database already exists (for example, as a result of terminating a previous geo-replication relationship) the command fails.</span></span>
> 

1. <span data-ttu-id="32136-117">V [portál Azure](http://portal.azure.com), přejděte do databáze, kterou chcete nastavit pro geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="32136-117">In the [Azure portal](http://portal.azure.com), browse to the database that you want to set up for geo-replication.</span></span>
2. <span data-ttu-id="32136-118">Na stránce databáze SQL, vyberte **geografická replikace**a potom vyberte oblast, chcete-li vytvořit sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="32136-118">On the SQL database page, select **geo-replication**, and then select the region to create the secondary database.</span></span> <span data-ttu-id="32136-119">Můžete vybrat všechny oblasti jiné než oblasti hostování primární databázi, ale doporučujeme, abyste [spárované oblast](../best-practices-availability-paired-regions.md).</span><span class="sxs-lookup"><span data-stu-id="32136-119">You can select any region other than the region hosting the primary database, but we recommend the [paired region](../best-practices-availability-paired-regions.md).</span></span>
   
    ![Konfigurace geografické replikace](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. <span data-ttu-id="32136-121">Vyberte nebo nakonfigurovat server a cenovou úroveň pro sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="32136-121">Select or configure the server and pricing tier for the secondary database.</span></span>
   
    ![Konfigurace sekundární](./media/sql-database-geo-replication-portal/create-secondary.png)
4. <span data-ttu-id="32136-123">Volitelně můžete přidat sekundární databáze do pružného fondu.</span><span class="sxs-lookup"><span data-stu-id="32136-123">Optionally, you can add a secondary database to an elastic pool.</span></span> <span data-ttu-id="32136-124">Chcete-li vytvořit sekundární databáze ve fondu, klikněte na tlačítko **elastický fond** a vyberte fond na cílovém serveru.</span><span class="sxs-lookup"><span data-stu-id="32136-124">To create the secondary database in a pool, click **elastic pool** and select a pool on the target server.</span></span> <span data-ttu-id="32136-125">Fond musí již existovat na cílovém serveru.</span><span class="sxs-lookup"><span data-stu-id="32136-125">A pool must already exist on the target server.</span></span> <span data-ttu-id="32136-126">Tento pracovní postup nevytvoří fondu.</span><span class="sxs-lookup"><span data-stu-id="32136-126">This workflow does not create a pool.</span></span>
5. <span data-ttu-id="32136-127">Klikněte na tlačítko **vytvořit** přidat sekundární.</span><span class="sxs-lookup"><span data-stu-id="32136-127">Click **Create** to add the secondary.</span></span>
6. <span data-ttu-id="32136-128">Sekundární databáze byla vytvořena a zahájí se proces synchronizace replik indexů.</span><span class="sxs-lookup"><span data-stu-id="32136-128">The secondary database is created and the seeding process begins.</span></span>
   
    ![Konfigurace sekundární](./media/sql-database-geo-replication-portal/seeding0.png)
7. <span data-ttu-id="32136-130">Po dokončení procesu synchronizace replik indexů sekundární databázi zobrazí její stav.</span><span class="sxs-lookup"><span data-stu-id="32136-130">When the seeding process is complete, the secondary database displays its status.</span></span>
   
    ![Synchronizace replik indexů dokončení](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a><span data-ttu-id="32136-132">Zahájení převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="32136-132">Initiate a failover</span></span>

<span data-ttu-id="32136-133">Sekundární databáze je možné přepnout stane primárním serverem.</span><span class="sxs-lookup"><span data-stu-id="32136-133">The secondary database can be switched to become the primary.</span></span>  

1. <span data-ttu-id="32136-134">V [portál Azure](http://portal.azure.com), přejděte do primární databáze ve spolupráci se geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="32136-134">In the [Azure portal](http://portal.azure.com), browse to the primary database in the geo-replication partnership.</span></span>
2. <span data-ttu-id="32136-135">V okně databáze SQL vyberte **všechna nastavení** > **geografická replikace**.</span><span class="sxs-lookup"><span data-stu-id="32136-135">On the SQL Database blade, select **All settings** > **geo-replication**.</span></span>
3. <span data-ttu-id="32136-136">V **SEKUNDÁRNÍCH** vyberte databáze, kterou chcete stane nový primární a klikněte na tlačítko **převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="32136-136">In the **SECONDARIES** list, select the database you want to become the new primary and click **Failover**.</span></span>
   
    ![převzetí služeb při selhání](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. <span data-ttu-id="32136-138">Klikněte na tlačítko **Ano** zahájíte převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="32136-138">Click **Yes** to begin the failover.</span></span>

<span data-ttu-id="32136-139">Příkaz okamžitě přepínačů sekundární databázi do primární role.</span><span class="sxs-lookup"><span data-stu-id="32136-139">The command immediately switches the secondary database into the primary role.</span></span> 

<span data-ttu-id="32136-140">Není k dispozici na krátkou dobu, během které obě databáze jsou k dispozici (v řádu 0 až 25 sekund), při přepínání role.</span><span class="sxs-lookup"><span data-stu-id="32136-140">There is a short period during which both databases are unavailable (on the order of 0 to 25 seconds) while the roles are switched.</span></span> <span data-ttu-id="32136-141">Pokud primární databáze má více sekundárních databází, příkaz automaticky změní konfiguraci dalších sekundární databáze k připojení k nové primární.</span><span class="sxs-lookup"><span data-stu-id="32136-141">If the primary database has multiple secondary databases, the command automatically reconfigures the other secondaries to connect to the new primary.</span></span> <span data-ttu-id="32136-142">Celou operaci by měl trvat méně než minutu za normálních okolností.</span><span class="sxs-lookup"><span data-stu-id="32136-142">The entire operation should take less than a minute to complete under normal circumstances.</span></span> 

> [!NOTE]
> <span data-ttu-id="32136-143">Tento příkaz je určená pro rychlé obnovení databáze v případě výpadku.</span><span class="sxs-lookup"><span data-stu-id="32136-143">This command is designed for quick recovery of the database in case of an outage.</span></span> <span data-ttu-id="32136-144">Aktivuje převzetí služeb při selhání bez synchronizace dat (Vynucené převzetí služeb při selhání).</span><span class="sxs-lookup"><span data-stu-id="32136-144">It triggers failover without data synchronization (forced failover).</span></span>  <span data-ttu-id="32136-145">Pokud primární je online a může dojít při vydání příkazu ke ztrátě dat. zapisování transakcí.</span><span class="sxs-lookup"><span data-stu-id="32136-145">If the primary is online and committing transactions when the command is issued some data loss may occur.</span></span> 
> 
> 

## <a name="remove-secondary-database"></a><span data-ttu-id="32136-146">Odebrání sekundární databáze</span><span class="sxs-lookup"><span data-stu-id="32136-146">Remove secondary database</span></span>
<span data-ttu-id="32136-147">Tato operace trvale ukončí replikace do sekundární databáze a změn roli sekundární regulární databáze pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="32136-147">This operation permanently terminates the replication to the secondary database, and changes the role of the secondary to a regular read-write database.</span></span> <span data-ttu-id="32136-148">Pokud se přeruší připojení k sekundární databázi, příkaz úspěšné, ale je obnovit sekundární nemá ne stane pro čtení a zápis až po připojení.</span><span class="sxs-lookup"><span data-stu-id="32136-148">If the connectivity to the secondary database is broken, the command succeeds but the secondary does not become read-write until after connectivity is restored.</span></span>  

1. <span data-ttu-id="32136-149">V [portál Azure](http://portal.azure.com), přejděte do primární databáze ve spolupráci se geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="32136-149">In the [Azure portal](http://portal.azure.com), browse to the primary database in the geo-replication partnership.</span></span>
2. <span data-ttu-id="32136-150">Na stránce databáze SQL, vyberte **geografická replikace**.</span><span class="sxs-lookup"><span data-stu-id="32136-150">On the SQL database page, select **geo-replication**.</span></span>
3. <span data-ttu-id="32136-151">V **SEKUNDÁRNÍCH** seznamu, vyberte databázi, kterou chcete odebrat z partnerství geografická replikace.</span><span class="sxs-lookup"><span data-stu-id="32136-151">In the **SECONDARIES** list, select the database you want to remove from the geo-replication partnership.</span></span>
4. <span data-ttu-id="32136-152">Klikněte na tlačítko **zastavení replikace**.</span><span class="sxs-lookup"><span data-stu-id="32136-152">Click **Stop Replication**.</span></span>
   
    ![Odeberte sekundární](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. <span data-ttu-id="32136-154">Otevře se okno pro potvrzení.</span><span class="sxs-lookup"><span data-stu-id="32136-154">A confirmation window opens.</span></span> <span data-ttu-id="32136-155">Klikněte na tlačítko **Ano** odebrání partnerství geografická replikace databáze.</span><span class="sxs-lookup"><span data-stu-id="32136-155">Click **Yes** to remove the database from the geo-replication partnership.</span></span> <span data-ttu-id="32136-156">(Nastavte ho na databázi pro čtení a zápis není součástí žádné replikace.)</span><span class="sxs-lookup"><span data-stu-id="32136-156">(Set it to a read-write database not part of any replication.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="32136-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32136-157">Next steps</span></span>
* <span data-ttu-id="32136-158">Další informace o aktivní geografickou replikaci, najdete v části [aktivní geografickou replikací](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32136-158">To learn more about active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md).</span></span>
* <span data-ttu-id="32136-159">Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="32136-159">For a business continuity overview and scenarios, see [Business continuity overview](sql-database-business-continuity.md).</span></span>

