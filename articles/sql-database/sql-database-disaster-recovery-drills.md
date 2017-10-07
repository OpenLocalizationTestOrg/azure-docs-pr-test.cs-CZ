---
title: "aaaSQL projde obnovení po havárii databáze | Microsoft Docs"
description: "Další pokyny a doporučené postupy pro používání Azure SQL Database tooperform po havárii obnovení projde toohelp zachovat, zvláště kritické obchodní aplikace odolné toofailures a výpadky."
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
ms.openlocfilehash: bf17857a19fdebddf0d4f55e4db3a1b33efb4e8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performing-disaster-recovery-drill"></a><span data-ttu-id="8b443-103">Provedením postupu zotavení po havárii</span><span class="sxs-lookup"><span data-stu-id="8b443-103">Performing Disaster Recovery Drill</span></span>
<span data-ttu-id="8b443-104">Doporučujeme, aby se pravidelně provádí ověřování připravenosti aplikace pro pracovní postup obnovení.</span><span class="sxs-lookup"><span data-stu-id="8b443-104">It is recommended that validation of application readiness for recovery workflow is performed periodically.</span></span> <span data-ttu-id="8b443-105">Ověřování chování aplikace hello a důsledků při ztrátě nebo hello přerušení dat zahrnuje tento převzetí služeb při selhání je dobrým zvykem engineering.</span><span class="sxs-lookup"><span data-stu-id="8b443-105">Verifying hello application behavior and implications of data loss and/or hello disruption that failover involves is a good engineering practice.</span></span> <span data-ttu-id="8b443-106">Je také požadavek ve většině oborové standardy jako součást Certifikační kontinuity firmy.</span><span class="sxs-lookup"><span data-stu-id="8b443-106">It is also a requirement by most industry standards as part of business continuity certification.</span></span>

<span data-ttu-id="8b443-107">Provádění postupu zotavení po havárii zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="8b443-107">Performing a disaster recovery drill consists of:</span></span>

* <span data-ttu-id="8b443-108">Simulace výpadku datové vrstvy</span><span class="sxs-lookup"><span data-stu-id="8b443-108">Simulating data tier outage</span></span>
* <span data-ttu-id="8b443-109">Obnovení</span><span class="sxs-lookup"><span data-stu-id="8b443-109">Recovering</span></span>
* <span data-ttu-id="8b443-110">Ověření obnovení post integritu aplikací</span><span class="sxs-lookup"><span data-stu-id="8b443-110">Validate application integrity post recovery</span></span>

<span data-ttu-id="8b443-111">V závislosti na tom, jak můžete [určená aplikace pro kontinuitu podnikových procesů](sql-database-business-continuity.md), hello pracovního postupu tooexecute hello procházení se může lišit.</span><span class="sxs-lookup"><span data-stu-id="8b443-111">Depending on how you [designed your application for business continuity](sql-database-business-continuity.md), hello workflow tooexecute hello drill can vary.</span></span> <span data-ttu-id="8b443-112">Níže budeme popisují hello osvědčené postupy provádění postupu zotavení po havárii v kontextu hello databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="8b443-112">Below we describe hello best practices conducting a disaster recovery drill in hello context of Azure SQL Database.</span></span>

## <a name="geo-restore"></a><span data-ttu-id="8b443-113">Geografické obnovení</span><span class="sxs-lookup"><span data-stu-id="8b443-113">Geo-restore</span></span>
<span data-ttu-id="8b443-114">tooprevent hello potenciální ztrátě dat při provádění postupu zotavení po havárii, doporučujeme, abyste provádění hello procházení testovacím prostředí pomocí vytváření kopie hello produkčního prostředí a použití aplikace hello tooverify převzetí služeb při selhání pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="8b443-114">tooprevent hello potential data loss when conducting a disaster recovery drill, we recommend performing hello drill using a test environment by creating a copy of hello production environment and using it tooverify hello application’s failover workflow.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="8b443-115">Simulace výpadku</span><span class="sxs-lookup"><span data-stu-id="8b443-115">Outage simulation</span></span>
<span data-ttu-id="8b443-116">toosimulate hello výpadku, můžete odstranit nebo přejmenovat hello zdrojové databáze.</span><span class="sxs-lookup"><span data-stu-id="8b443-116">toosimulate hello outage, you can delete or rename hello source database.</span></span> <span data-ttu-id="8b443-117">To způsobí selhání připojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b443-117">This causes application connectivity failures.</span></span>

#### <a name="recovery"></a><span data-ttu-id="8b443-118">Obnovení</span><span class="sxs-lookup"><span data-stu-id="8b443-118">Recovery</span></span>
* <span data-ttu-id="8b443-119">Provést hello geografické obnovení databáze hello na jiném serveru, jak se popisuje [zde](sql-database-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="8b443-119">Perform hello geo-restore of hello database into a different server as described [here](sql-database-disaster-recovery.md).</span></span>
* <span data-ttu-id="8b443-120">Změna hello aplikace konfigurace tooconnect toohello obnovené databáze a postupujte podle hello [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) Průvodce toocomplete hello obnovení.</span><span class="sxs-lookup"><span data-stu-id="8b443-120">Change hello application configuration tooconnect toohello recovered database and follow hello [Configure a database after recovery](sql-database-disaster-recovery.md) guide toocomplete hello recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="8b443-121">Ověření</span><span class="sxs-lookup"><span data-stu-id="8b443-121">Validation</span></span>
* <span data-ttu-id="8b443-122">Dokončení hello procházet kontrolou hello aplikace integrity post obnovení (včetně připojovací řetězce, přihlášení, základní funkce testování nebo jinou ověření část postupy signoffs standardní aplikace).</span><span class="sxs-lookup"><span data-stu-id="8b443-122">Complete hello drill by verifying hello application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="geo-replication"></a><span data-ttu-id="8b443-123">Geografická replikace</span><span class="sxs-lookup"><span data-stu-id="8b443-123">Geo-replication</span></span>
<span data-ttu-id="8b443-124">Pro databáze, který je chráněný pomocí geografická replikace hello procházení zahrnuje cvičení sekundární databáze toohello plánované převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8b443-124">For a database that is protected using geo-replication hello drill exercise involves planned failover toohello secondary database.</span></span> <span data-ttu-id="8b443-125">Hello plánované převzetí služeb při selhání zajistí, hello primární a sekundární databáze hello zůstaly synchronizované při přepínání hello role.</span><span class="sxs-lookup"><span data-stu-id="8b443-125">hello planned failover ensures that hello primary and hello secondary databases remain in sync when hello roles are switched.</span></span> <span data-ttu-id="8b443-126">Na rozdíl od hello neplánované převzetí služeb při selhání, tato operace není způsobit ztrátu dat, takže hello procházení mohou být prováděny v provozním prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="8b443-126">Unlike hello unplanned failover, this operation does not result in data loss, so hello drill can be performed in hello production environment.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="8b443-127">Simulace výpadku</span><span class="sxs-lookup"><span data-stu-id="8b443-127">Outage simulation</span></span>
<span data-ttu-id="8b443-128">toosimulate hello výpadku, můžete zakázat hello webové aplikace nebo virtuální počítač připojen toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="8b443-128">toosimulate hello outage, you can disable hello web application or virtual machine connected toohello database.</span></span> <span data-ttu-id="8b443-129">Výsledkem hello chyby připojení pro klienty webového hello.</span><span class="sxs-lookup"><span data-stu-id="8b443-129">This results in hello connectivity failures for hello web clients.</span></span>

#### <a name="recovery"></a><span data-ttu-id="8b443-130">Obnovení</span><span class="sxs-lookup"><span data-stu-id="8b443-130">Recovery</span></span>
* <span data-ttu-id="8b443-131">Ujistěte se, že konfigurace aplikace hello v hello zotavení po Havárii oblast body toohello dřívějším sekundární který se stane hello plně dostupný nový primární.</span><span class="sxs-lookup"><span data-stu-id="8b443-131">Make sure hello application configuration in hello DR region points toohello former secondary which becomes hello fully accessible new primary.</span></span>
* <span data-ttu-id="8b443-132">Provedení [plánované převzetí služeb při selhání](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello sekundární databáze nové primární</span><span class="sxs-lookup"><span data-stu-id="8b443-132">Perform [planned failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello secondary database a new primary</span></span>
* <span data-ttu-id="8b443-133">Postupujte podle hello [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) Průvodce toocomplete hello obnovení.</span><span class="sxs-lookup"><span data-stu-id="8b443-133">Follow hello [Configure a database after recovery](sql-database-disaster-recovery.md) guide toocomplete hello recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="8b443-134">Ověření</span><span class="sxs-lookup"><span data-stu-id="8b443-134">Validation</span></span>
* <span data-ttu-id="8b443-135">Dokončení hello procházet kontrolou hello aplikace integrity post obnovení (včetně připojovací řetězce, přihlášení, základní funkce testování nebo jinou ověření část postupy signoffs standardní aplikace).</span><span class="sxs-lookup"><span data-stu-id="8b443-135">Complete hello drill by verifying hello application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b443-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b443-136">Next steps</span></span>
* <span data-ttu-id="8b443-137">toolearn o kontinuity obchodních scénářů, najdete v části [kontinuity scénáře](sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="8b443-137">toolearn about business continuity scenarios, see [Continuity scenarios](sql-database-business-continuity.md)</span></span>
* <span data-ttu-id="8b443-138">toolearn o zálohování Azure SQL Database automatizované, najdete v části [automatizované zálohování SQL Database](sql-database-automated-backups.md)</span><span class="sxs-lookup"><span data-stu-id="8b443-138">toolearn about Azure SQL Database automated backups, see [SQL Database automated backups](sql-database-automated-backups.md)</span></span>
* <span data-ttu-id="8b443-139">toolearn o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba hello](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="8b443-139">toolearn about using automated backups for recovery, see [restore a database from hello service-initiated backups](sql-database-recovery-using-backups.md)</span></span>
* <span data-ttu-id="8b443-140">toolearn o rychlejší možnosti obnovení, najdete v části [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="8b443-140">toolearn about faster recovery options, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>  
