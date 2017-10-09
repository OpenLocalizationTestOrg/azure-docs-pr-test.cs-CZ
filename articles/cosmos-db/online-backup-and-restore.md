---
title: "aaaOnline zálohování a obnovení s Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak tooperform automatické zálohování a obnovení na databázi Azure Cosmos DB."
keywords: "zálohování a obnovení, zálohování online"
services: cosmos-db
documentationcenter: 
author: RahulPrasad16
manager: jhubbard
editor: monicar
ms.assetid: 98eade4a-7ef4-4667-b167-6603ecd80b79
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/11/2017
ms.author: raprasa
ms.openlocfilehash: a0b464c95681dfc7b5462b02bf04c2c43d6bc16f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Automatické online zálohování a obnovení databáze Cosmos Azure
Azure Cosmos DB automaticky provede zálohování všech dat v pravidelných intervalech. Automatické zálohování Hello se provádějí bez ovlivnění hello výkon nebo dostupnost databázové operace. Všechny zálohy se ukládají odděleně v jiné službě úložiště a tyto zálohy jsou globálně replikovat pro odolnost proti místní havárie. Automatické zálohování Hello jsou určené pro scénáře, když jste omylem odstranit kontejner vaší Comos DB a později vyžadují obnovení dat nebo řešení zotavení po havárii.  

Tento článek začíná rychlé rekapitulace hello data redundance a dostupnosti v Cosmos DB a pak popisuje zálohování. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Vysoká dostupnost s Cosmos DB - rekapitulace
Cosmos DB je navrženou toobe [globálně distribuované](distribute-data-globally.md) – umožňuje tooscale propustnosti nad několika oblastmi Azure společně s zásad řízené převzetí služeb při selhání a transparentní rozhraní API více funkci. Jako nabídku databáze systému [99,99 % dostupnost SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), všech zápisů hello v Cosmos DB jsou trvale potvrzena toolocal disky podle kvora replik v rámci místního datového centra dříve, než potvrdil toohello klienta. Upozorňujeme, že hello vysokou dostupnost databáze Cosmos spoléhá na místní úložiště a nezávisí na žádné technologie externího úložiště. Kromě toho pokud databázový účet je přidružen více než jedné oblasti Azure, replikují se v jiných oblastech také vaše zápisy. tooscale daty propustnost a přístup v nízkou latenci, může mít mnoho pro čtení oblasti spojené s vaším účtem databáze podle potřeby. V každé oblasti pro čtení je spolehlivě hello (replikované) dat zachová pro sady replik.  

Jak je znázorněno v následujícím diagramu hello, je jediný kontejner Cosmos DB [vodorovně oddílů](partition-data.md). "Oddíl" je označený jako kruh v následujícím diagramu hello a každý oddíl je zpřístupnit pomocí sady replik. Toto je místní distribuční hello v rámci jedné oblasti Azure (označen hello osy X). Dále každý oddíl (s jeho odpovídající sady replik) je pak globálně distribuované nad několika oblastmi spojené s vaším účtem databáze (například v tento obrázek hello tři oblasti – Východ USA, západní USA a střed). Hello "oddílu sada" je globálně distribuované entity, která obsahuje více kopií vašich dat v jednotlivých oblastech (označen osy hello Y). Můžete přiřadit prioritu toohello oblasti spojené s vaším účtem databáze a databáze Cosmos bude transparentně další oblasti toohello převzetí služeb při selhání v případě havárie. Můžete také ručně simulovat převzetí služeb při selhání tootest hello začátku do konce dostupnosti vaší aplikace.  

Hello následující obrázek ukazuje hello vysoký stupeň redundance s Cosmos DB.

![Vysoký stupeň redundance s Cosmos DB](./media/online-backup-and-restore/redundancy.png)

![Vysoký stupeň redundance s Cosmos DB](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Úplné, automatické, online zálohování
Bohužel se I odstraněn Moje kontejner nebo databáze. S Cosmos databáze nejen vaše data, ale hello zálohování dat také probíhají vysoce redundantní a odolné tooregional havárie. Tyto automatizované zálohy jsou aktuálně prováděné přibližně každé čtyři hodiny a nejnovější 2 zálohy jsou uložené za všech okolností. Pokud jsou hello data ať už náhodně vyřazeny nebo je poškozen, prosím [požádejte podporu Azure](https://azure.microsoft.com/support/options/) v rámci 8 hodin. 

bez ovlivnění hello výkon nebo dostupnost databázové operace jsou provedeny Hello zálohy. Cosmos DB přebírá zálohování hello hello pozadí bez použití vašeho zřízené RUs nebo které mají vliv na výkon hello a bez ovlivnění dostupnosti hello vaší databáze. 

Na rozdíl od vaše data, která je uložena v rámci Cosmos DB automatické zálohování hello ukládají ve službě Azure Blob Storage. tooguarantee hello nízkou latenci nebo efektivní nahrávání, hello snímek zálohy se nahraný tooan instanci Azure Blob storage ve stejné oblasti jako hello aktuální zápisu oblast účtu databáze Cosmos DB hello. V zájmu odolnosti proti místní po havárii každý snímek zálohovaných dat do Azure Blob Storage opět replikují přes tooanother oblast geograficky redundantní úložiště (GRS). Hello následující diagram ukazuje, že je hello celý Cosmos DB kontejneru (s všechny tři primární oddíly v západní USA, v tomto příkladu) zálohovány v vzdáleného účtu úložiště objektů Blob Azure v západní USA a GRS replikovat tooEast nás. 

Hello následující obrázek ukazuje pravidelné úplné zálohy všech Cosmos DB entit ve službě Azure Storage GRS.

![Pravidelné úplné zálohy všechny entity Cosmos DB v GRS Azure Storage](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Doba uchovávání záloh
Jak je popsáno výše, Azure Cosmos DB trvá snímky dat každé čtyři hodiny a uchovává hello posledních dvou snímky všech oddílů po dobu 30 dnů. Podle našich předpisy jsou snímky vymazány po 90 dnech.

Pokud chcete toomaintain vlastní snímky, můžete použít možnost tooJSON export hello v hello Azure Cosmos DB [nástroj pro migraci dat](import-data.md#export-to-json-file) tooschedule další zálohy. 

## <a name="restoring-a-database-from-an-online-backup"></a>Obnovení databáze z online zálohování
Pokud omylem odstraníte databáze nebo kolekce, můžete [souboru lístek podpory](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) nebo [volání podpory Azure](https://azure.microsoft.com/support/options/) toorestore hello data z posledního automatické zálohování hello. Pokud budete potřebovat toorestore databáze z důvodu problému s poškozením dat, najdete v části [zpracování poškození dat](#handling-data-corruption) podle potřeby tootake další kroky tooprevent hello poškozená data ze zálohy průbojnými hello. Pro konkrétní snímek vašeho zálohování toobe obnovit Cosmos DB vyžaduje, aby hello data byla k dispozici pro dobu trvání hello hello cyklus zálohování tohoto snímku.

## <a name="handling-data-corruption"></a>Zpracování poškození dat
Azure Cosmos DB zachová hello posledních dvou zálohování všech oddílů v systému hello. Tento model funguje dobře, když kontejner (kolekce dokumentů, graf, tabulku) nebo v databázi je omylem odstranit, protože jeden z poslední verze hello může být obnovena. Však v hello tom případě, když uživatelé mohou zavést kvůli problému s poškozením dat, Azure Cosmos DB může být vědomi hello poškození dat, a je možné, že hello poškození může mít procházejí hello zálohy. Jakmile se detekuje poškození, byste měli odstranit kontejner hello poškozený (kolekce nebo grafu nebo tabulky) tak, aby zálohování jsou chráněny před přepsáním s poškozenými daty. Vzhledem k tomu, že poslední zálohy hello může být staré čtyři hodiny, můžete použít hello uživatele [změnit informačního kanálu](change-feed.md) toocapture a úložiště hello poslední čtyři hodiny vhodné dat před odstraněním hello kontejneru.

## <a name="next-steps"></a>Další kroky

najdete v databázi v několika datových centrech tooreplicate [distribuci dat globálně pomocí Cosmos DB](distribute-data-globally.md). 

toofile kontaktujte podporu Azure [souboru lístek z portálu Azure hello](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

