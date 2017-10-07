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
# <a name="configure-active-geo-replication-for-azure-sql-database-in-hello-azure-portal-and-initiate-failover"></a>Konfigurace aktivní geografickou replikací pro Azure SQL Database v hello portál Azure a inicializovat převzetí služeb při selhání

Tento článek ukazuje, jak tooconfigure active geografická replikace pro databázi SQL v hello [portál Azure](http://portal.azure.com) a tooinitiate převzetí služeb při selhání.

převzetí služeb při selhání tooinitiate hello portál Azure, najdete v části [zahájení plánovaném nebo neplánovaném převzetí služeb při selhání pro databázi SQL Azure s hello portál Azure](sql-database-geo-replication-portal.md).

tooconfigure aktivní geografickou replikaci pomocí hello portálu Azure, je třeba hello následujících prostředků:

* Azure SQL database: hello primární databázi, které chcete tooreplicate tooa jiné zeměpisné oblasti.

> [!Note]
Aktivní geografickou replikaci musí být mezi databází v hello stejného předplatného.

## <a name="add-a-secondary-database"></a>Přidání sekundární databáze
Hello následujících kroků vytvořte novou sekundární databázi ve spolupráci se geografická replikace.  

tooadd sekundární databáze, musí být spoluvlastník nebo vlastníka předplatného hello.

sekundární databáze Hello má stejný název jako primární databáze hello hello a má, ve výchozím nastavení, hello stejné úrovně služeb. Hello sekundární databáze může být jedné databáze nebo databáze ve fondu elastické databáze. Další informace najdete v tématu [úrovních služeb](sql-database-service-tiers.md).
Po hello sekundární se vytvoří a nasadí, data začne replikace z hello primární databáze toohello nové sekundární databáze.

> [!NOTE]
> Pokud už existuje databáze partnera hello (například. v důsledku ukončení předchozí relace geografická replikace) hello příkaz se nezdaří.
> 

1. V hello [portál Azure](http://portal.azure.com), procházet toohello databáze, které chcete tooset službu geografická replikace.
2. Na stránce databáze SQL hello vyberte **geografická replikace**a pak vyberte hello oblast toocreate hello sekundární databázi. Můžete vybrat všechny oblasti, než hello oblasti hostování hello primární databázi, ale doporučujeme hello [spárované oblast](../best-practices-availability-paired-regions.md).
   
    ![Konfigurace geografické replikace](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Vyberte nebo nakonfigurovat hello server a cenovou úroveň pro hello sekundární databáze.
   
    ![Konfigurace sekundární](./media/sql-database-geo-replication-portal/create-secondary.png)
4. Volitelně můžete přidat sekundární databázi elastického fondu tooan. Klikněte na tlačítko toocreate hello sekundární databáze ve fondu, **elastický fond** a vyberte fond na cílovém serveru hello. Fond musí již existují na cílový server hello. Tento pracovní postup nevytvoří fondu.
5. Klikněte na tlačítko **vytvořit** tooadd hello sekundární.
6. Hello sekundární databáze byla vytvořena a zahájí se synchronizace replik indexů proces hello.
   
    ![Konfigurace sekundární](./media/sql-database-geo-replication-portal/seeding0.png)
7. Po dokončení synchronizace replik indexů proces hello hello sekundární databáze zobrazí její stav.
   
    ![Synchronizace replik indexů dokončení](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Zahájení převzetí služeb při selhání

Hello sekundární databáze může být vypnuté toobecome hello primární.  

1. V hello [portál Azure](http://portal.azure.com), procházet toohello primární databázi ve spolupráci se geografická replikace hello.
2. V okně hello SQL Database, vyberte **všechna nastavení** > **geografická replikace**.
3. V hello **SEKUNDÁRNÍCH** seznamu, vyberte hello databáze chcete toobecome hello nový primární a klikněte na tlačítko **převzetí služeb při selhání**.
   
    ![převzetí služeb při selhání](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Klikněte na tlačítko **Ano** toobegin hello převzetí služeb při selhání.

příkaz Hello okamžitě přepínačů hello sekundární databáze do primární role hello. 

Není k dispozici na krátkou dobu, během které obě databáze jsou k dispozici (v pořadí hello 0 too25 sekund), při přepínání hello role. Pokud hello primární databáze má více sekundární databáze, příkaz hello automaticky změní konfiguraci hello nový primární jiných sekundárních tooconnect toohello. Hello celou operaci lze provést méně než minutu toocomplete za normálních okolností. 

> [!NOTE]
> Tento příkaz je určená pro rychlé obnovení databáze hello v případě výpadku. Aktivuje převzetí služeb při selhání bez synchronizace dat (Vynucené převzetí služeb při selhání).  Pokud primární hello je online a potvrzením transakce při vydání příkazu hello ztrátu dat může dojít. 
> 
> 

## <a name="remove-secondary-database"></a>Odebrání sekundární databáze
Tato operace trvale ukončí hello toohello replikace do sekundární databáze a změny hello role hello sekundární tooa regulární databáze pro čtení a zápis. Pokud je přerušený hello připojení toohello sekundární databáze, hello příkazu úspěšné, ale hello sekundární nemá ne stane pro čtení a zápis do doby, po obnovení připojení.  

1. V hello [portál Azure](http://portal.azure.com), procházet toohello primární databázi ve spolupráci se geografická replikace hello.
2. Na stránce databáze SQL hello vyberte **geografická replikace**.
3. V hello **SEKUNDÁRNÍCH** seznamu, vyberte hello databáze chcete tooremove z hello geografická replikace partnerství.
4. Klikněte na tlačítko **zastavení replikace**.
   
    ![Odeberte sekundární](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Otevře se okno pro potvrzení. Klikněte na tlačítko **Ano** tooremove hello databáze z hello geografická replikace partnerství. (Nastavit tooa pro čtení a zápis databáze nejsou součástí žádné replikace.)

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o aktivní geografickou replikaci, najdete v části [aktivní geografickou replikací](sql-database-geo-replication-overview.md).
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).

