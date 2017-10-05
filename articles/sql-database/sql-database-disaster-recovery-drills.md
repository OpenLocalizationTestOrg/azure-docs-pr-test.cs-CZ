---
title: "Cvičení obnovení po havárii databáze SQL | Microsoft Docs"
description: "Další pokyny a osvědčené postupy pro používání Azure SQL Database k provedení projde obnovení po havárii zajistit, aby byl váš zvláště důležitých podnikových aplikací odolné selhání a výpadky."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: 1b1d65a41a794a566287dcffe3581ac58e2a965f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="performing-disaster-recovery-drill"></a><span data-ttu-id="f542e-103">Provedením postupu zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="f542e-103">Performing Disaster Recovery Drill</span></span>
<span data-ttu-id="f542e-104">Doporučujeme, aby se pravidelně provádí ověřování připravenosti aplikace pro pracovní postup obnovení.</span><span class="sxs-lookup"><span data-stu-id="f542e-104">It is recommended that validation of application readiness for recovery workflow is performed periodically.</span></span> <span data-ttu-id="f542e-105">Ověření chování aplikace a dopad ztráty dat nebo přerušení zahrnuje tento převzetí služeb při selhání je dobrým zvykem engineering.</span><span class="sxs-lookup"><span data-stu-id="f542e-105">Verifying the application behavior and implications of data loss and/or the disruption that failover involves is a good engineering practice.</span></span> <span data-ttu-id="f542e-106">Je také požadavek ve většině oborové standardy jako součást Certifikační kontinuity firmy.</span><span class="sxs-lookup"><span data-stu-id="f542e-106">It is also a requirement by most industry standards as part of business continuity certification.</span></span>

<span data-ttu-id="f542e-107">Provádění postupu zotavení po havárii zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="f542e-107">Performing a disaster recovery drill consists of:</span></span>

* <span data-ttu-id="f542e-108">Simulace výpadku datové vrstvy</span><span class="sxs-lookup"><span data-stu-id="f542e-108">Simulating data tier outage</span></span>
* <span data-ttu-id="f542e-109">Obnovení</span><span class="sxs-lookup"><span data-stu-id="f542e-109">Recovering</span></span>
* <span data-ttu-id="f542e-110">Ověření obnovení post integritu aplikací</span><span class="sxs-lookup"><span data-stu-id="f542e-110">Validate application integrity post recovery</span></span>

<span data-ttu-id="f542e-111">V závislosti na tom, jak můžete [určená aplikace pro kontinuitu podnikových procesů](sql-database-business-continuity.md), pracovní postup spuštění podrobné se může lišit.</span><span class="sxs-lookup"><span data-stu-id="f542e-111">Depending on how you [designed your application for business continuity](sql-database-business-continuity.md), the workflow to execute the drill can vary.</span></span> <span data-ttu-id="f542e-112">Níže budeme popisují doporučené postupy provádění postupu zotavení po havárii v kontextu databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="f542e-112">Below we describe the best practices conducting a disaster recovery drill in the context of Azure SQL Database.</span></span>

## <a name="geo-restore"></a><span data-ttu-id="f542e-113">Geografické obnovení</span><span class="sxs-lookup"><span data-stu-id="f542e-113">Geo-restore</span></span>
<span data-ttu-id="f542e-114">Aby se zabránilo potenciální ztrátě dat při provádění postupu zotavení po havárii, doporučujeme provádění procházení testovacím prostředí pomocí vytvořit kopii produkčního prostředí a používat jej ověření pracovní postup převzetí služeb při selhání aplikace.</span><span class="sxs-lookup"><span data-stu-id="f542e-114">To prevent the potential data loss when conducting a disaster recovery drill, we recommend performing the drill using a test environment by creating a copy of the production environment and using it to verify the application’s failover workflow.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="f542e-115">Simulace výpadku</span><span class="sxs-lookup"><span data-stu-id="f542e-115">Outage simulation</span></span>
<span data-ttu-id="f542e-116">Pro simulaci se výpadek, můžete odstranit nebo přejmenovat zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="f542e-116">To simulate the outage, you can delete or rename the source database.</span></span> <span data-ttu-id="f542e-117">To způsobí selhání připojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="f542e-117">This causes application connectivity failures.</span></span>

#### <a name="recovery"></a><span data-ttu-id="f542e-118">Obnovení</span><span class="sxs-lookup"><span data-stu-id="f542e-118">Recovery</span></span>
* <span data-ttu-id="f542e-119">Provést geografické obnovení databáze do jiného serveru, jak se popisuje [zde](sql-database-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="f542e-119">Perform the geo-restore of the database into a different server as described [here](sql-database-disaster-recovery.md).</span></span>
* <span data-ttu-id="f542e-120">Změna konfigurace aplikace pro připojení k obnovené databáze a postupujte podle [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) průvodce dokončí obnovení.</span><span class="sxs-lookup"><span data-stu-id="f542e-120">Change the application configuration to connect to the recovered database and follow the [Configure a database after recovery](sql-database-disaster-recovery.md) guide to complete the recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="f542e-121">Ověření</span><span class="sxs-lookup"><span data-stu-id="f542e-121">Validation</span></span>
* <span data-ttu-id="f542e-122">Dokončete podrobné sestavy pomocí ověření obnovení post integrity aplikace (včetně připojovací řetězce, přihlášení, základní funkce testování nebo jinou ověření část postupy signoffs standardní aplikace).</span><span class="sxs-lookup"><span data-stu-id="f542e-122">Complete the drill by verifying the application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="geo-replication"></a><span data-ttu-id="f542e-123">Geografická replikace</span><span class="sxs-lookup"><span data-stu-id="f542e-123">Geo-replication</span></span>
<span data-ttu-id="f542e-124">Pro databáze, který je chráněný pomocí geografická replikace zahrnuje cvičení procházení plánované převzetí služeb při selhání pro sekundární databázi.</span><span class="sxs-lookup"><span data-stu-id="f542e-124">For a database that is protected using geo-replication the drill exercise involves planned failover to the secondary database.</span></span> <span data-ttu-id="f542e-125">Plánované převzetí služeb při selhání zaručuje, že primární a sekundární databáze zachovány synchronizované při přepínání role.</span><span class="sxs-lookup"><span data-stu-id="f542e-125">The planned failover ensures that the primary and the secondary databases remain in sync when the roles are switched.</span></span> <span data-ttu-id="f542e-126">Na rozdíl od neplánované převzetí služeb při selhání tato operace není způsobit ztrátu dat, takže podrobné sestavy lze provést v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f542e-126">Unlike the unplanned failover, this operation does not result in data loss, so the drill can be performed in the production environment.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="f542e-127">Simulace výpadku</span><span class="sxs-lookup"><span data-stu-id="f542e-127">Outage simulation</span></span>
<span data-ttu-id="f542e-128">Pro simulaci se výpadek, můžete zakázat webové aplikace nebo virtuální počítač připojen k databázi.</span><span class="sxs-lookup"><span data-stu-id="f542e-128">To simulate the outage, you can disable the web application or virtual machine connected to the database.</span></span> <span data-ttu-id="f542e-129">Pro klienty webového výsledkem chyby připojení.</span><span class="sxs-lookup"><span data-stu-id="f542e-129">This results in the connectivity failures for the web clients.</span></span>

#### <a name="recovery"></a><span data-ttu-id="f542e-130">Obnovení</span><span class="sxs-lookup"><span data-stu-id="f542e-130">Recovery</span></span>
* <span data-ttu-id="f542e-131">Zajistěte, aby konfigurace aplikace v oblasti zotavení po Havárii odkazuje na sekundární dřívějším, který se stane plně dostupný nový primární.</span><span class="sxs-lookup"><span data-stu-id="f542e-131">Make sure the application configuration in the DR region points to the former secondary which becomes the fully accessible new primary.</span></span>
* <span data-ttu-id="f542e-132">Provedení [plánované převzetí služeb při selhání](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) aby sekundární databázi nový primární</span><span class="sxs-lookup"><span data-stu-id="f542e-132">Perform [planned failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) to make the secondary database a new primary</span></span>
* <span data-ttu-id="f542e-133">Postupujte podle [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) průvodce dokončí obnovení.</span><span class="sxs-lookup"><span data-stu-id="f542e-133">Follow the [Configure a database after recovery](sql-database-disaster-recovery.md) guide to complete the recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="f542e-134">Ověření</span><span class="sxs-lookup"><span data-stu-id="f542e-134">Validation</span></span>
* <span data-ttu-id="f542e-135">Dokončete podrobné sestavy pomocí ověření obnovení post integrity aplikace (včetně připojovací řetězce, přihlášení, základní funkce testování nebo jinou ověření část postupy signoffs standardní aplikace).</span><span class="sxs-lookup"><span data-stu-id="f542e-135">Complete the drill by verifying the application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f542e-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f542e-136">Next steps</span></span>
* <span data-ttu-id="f542e-137">Další informace o obchodních scénářů kontinuity najdete v tématu [kontinuity scénáře](sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="f542e-137">To learn about business continuity scenarios, see [Continuity scenarios](sql-database-business-continuity.md)</span></span>
* <span data-ttu-id="f542e-138">Další informace o Azure SQL Database automatizované zálohování najdete v tématu [automatizované zálohování SQL Database](sql-database-automated-backups.md)</span><span class="sxs-lookup"><span data-stu-id="f542e-138">To learn about Azure SQL Database automated backups, see [SQL Database automated backups](sql-database-automated-backups.md)</span></span>
* <span data-ttu-id="f542e-139">Další informace o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="f542e-139">To learn about using automated backups for recovery, see [restore a database from the service-initiated backups](sql-database-recovery-using-backups.md)</span></span>
* <span data-ttu-id="f542e-140">Další informace o možnosti rychlejší obnovení najdete v tématu [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f542e-140">To learn about faster recovery options, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>  
