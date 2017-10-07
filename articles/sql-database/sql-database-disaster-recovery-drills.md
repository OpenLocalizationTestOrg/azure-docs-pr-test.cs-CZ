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
# <a name="performing-disaster-recovery-drill"></a>Provedením postupu zotavení po havárii
Doporučujeme, aby se pravidelně provádí ověřování připravenosti aplikace pro pracovní postup obnovení. Ověřování chování aplikace hello a důsledků při ztrátě nebo hello přerušení dat zahrnuje tento převzetí služeb při selhání je dobrým zvykem engineering. Je také požadavek ve většině oborové standardy jako součást Certifikační kontinuity firmy.

Provádění postupu zotavení po havárii zahrnuje:

* Simulace výpadku datové vrstvy
* Obnovení
* Ověření obnovení post integritu aplikací

V závislosti na tom, jak můžete [určená aplikace pro kontinuitu podnikových procesů](sql-database-business-continuity.md), hello pracovního postupu tooexecute hello procházení se může lišit. Níže budeme popisují hello osvědčené postupy provádění postupu zotavení po havárii v kontextu hello databáze SQL Azure.

## <a name="geo-restore"></a>Geografické obnovení
tooprevent hello potenciální ztrátě dat při provádění postupu zotavení po havárii, doporučujeme, abyste provádění hello procházení testovacím prostředí pomocí vytváření kopie hello produkčního prostředí a použití aplikace hello tooverify převzetí služeb při selhání pracovního postupu.

#### <a name="outage-simulation"></a>Simulace výpadku
toosimulate hello výpadku, můžete odstranit nebo přejmenovat hello zdrojové databáze. To způsobí selhání připojení aplikace.

#### <a name="recovery"></a>Obnovení
* Provést hello geografické obnovení databáze hello na jiném serveru, jak se popisuje [zde](sql-database-disaster-recovery.md).
* Změna hello aplikace konfigurace tooconnect toohello obnovené databáze a postupujte podle hello [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) Průvodce toocomplete hello obnovení.

#### <a name="validation"></a>Ověření
* Dokončení hello procházet kontrolou hello aplikace integrity post obnovení (včetně připojovací řetězce, přihlášení, základní funkce testování nebo jinou ověření část postupy signoffs standardní aplikace).

## <a name="geo-replication"></a>Geografická replikace
Pro databáze, který je chráněný pomocí geografická replikace hello procházení zahrnuje cvičení sekundární databáze toohello plánované převzetí služeb při selhání. Hello plánované převzetí služeb při selhání zajistí, hello primární a sekundární databáze hello zůstaly synchronizované při přepínání hello role. Na rozdíl od hello neplánované převzetí služeb při selhání, tato operace není způsobit ztrátu dat, takže hello procházení mohou být prováděny v provozním prostředí hello.

#### <a name="outage-simulation"></a>Simulace výpadku
toosimulate hello výpadku, můžete zakázat hello webové aplikace nebo virtuální počítač připojen toohello databáze. Výsledkem hello chyby připojení pro klienty webového hello.

#### <a name="recovery"></a>Obnovení
* Ujistěte se, že konfigurace aplikace hello v hello zotavení po Havárii oblast body toohello dřívějším sekundární který se stane hello plně dostupný nový primární.
* Provedení [plánované převzetí služeb při selhání](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello sekundární databáze nové primární
* Postupujte podle hello [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) Průvodce toocomplete hello obnovení.

#### <a name="validation"></a>Ověření
* Dokončení hello procházet kontrolou hello aplikace integrity post obnovení (včetně připojovací řetězce, přihlášení, základní funkce testování nebo jinou ověření část postupy signoffs standardní aplikace).

## <a name="next-steps"></a>Další kroky
* toolearn o kontinuity obchodních scénářů, najdete v části [kontinuity scénáře](sql-database-business-continuity.md)
* toolearn o zálohování Azure SQL Database automatizované, najdete v části [automatizované zálohování SQL Database](sql-database-automated-backups.md)
* toolearn o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba hello](sql-database-recovery-using-backups.md)
* toolearn o rychlejší možnosti obnovení, najdete v části [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)  
