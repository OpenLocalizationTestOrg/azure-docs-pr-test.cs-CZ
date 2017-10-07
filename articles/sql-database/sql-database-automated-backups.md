---
title: "Automatická, geograficky redundantní zálohy databáze SQL aaaAzure | Microsoft Docs"
description: "SQL Database automaticky vytvoří zálohu místní databáze každých několik minut a používá Azure geograficky redundantní úložiště s přístupem pro čtení pro geografická redundance."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 8aff5356e8142707dd7cd2533a4aa5ea8fec866d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-automatic-sql-database-backups"></a>Další informace o automatické zálohování databáze SQL

Databáze SQL se automaticky vytvoří zálohy databáze a používá Azure přístup pro čtení geograficky redundantní úložiště (RA-GRS) tooprovide geografická redundance. Tyto zálohy se vytvoří automaticky a bez dalších poplatků. Nepotřebujete toodo cokoli toomake, k jejich dojít. Zálohování databáze jsou nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože se data chránit před náhodnou poškození nebo odstranění. Pokud chcete tookeep zálohy v vlastní kontejner úložiště můžete nakonfigurovat zásadu dlouhodobé uchovávání záloh. Další informace najdete v tématu [Dlouhodobé uchovávání](sql-database-long-term-retention.md).

## <a name="what-is-a-sql-database-backup"></a>Co je zálohování SQL Database?

Databáze SQL používá SQL Server technologie toocreate [úplné](https://msdn.microsoft.com/library/ms186289.aspx), [rozdílové](https://msdn.microsoft.com/library/ms175526.aspx), a [transakčního protokolu](https://msdn.microsoft.com/library/ms191429.aspx) zálohy. zálohování transakčního protokolu Hello obecně dojít každých 5 až 10 minut, s četností hello na základě hello úroveň výkonu a množství databázové aktivitě. Zálohování transakčního protokolu v případě zálohování úplné a rozdílové umožňují toorestore konkrétní v okamžiku toohello tooa databáze stejný server, který je hostitelem databáze hello. Při obnovování databáze hello služby obrázky, na které úplné, rozdíl a zálohování transakčního protokolu nemusí toobe obnovit.


Můžete použít tyto zálohy na:

* Obnovte databáze tooa v daném okamžiku v rámci doby uchování hello. Tato operace vytvoří novou databázi v hello stejný server jako původní databáze hello.
* Obnovte odstraněnou databázi toohello čas, který byl odstraněn nebo kdykoli během období uchování hello. Hello odstranit databázi lze obnovit pouze v hello stejný server, kde byl vytvořen hello původní databázi.
* Obnovte do databáze tooanother geografické oblasti. To vám umožní toorecover po havárii geografické při nelze získat přístup k serveru a databáze. Vytvoří novou databázi v jakékoli existující server kdekoli v hello, world. 
* Obnovte databázi ze zálohy konkrétní uložené v trezoru služeb zotavení Azure. To vám umožní toorestore starší verzi hello databáze toosatisfy žádost o dodržování předpisů nebo toorun starší verzi aplikace hello. V tématu [dlouhodobé uchovávání](sql-database-long-term-retention.md).
* tooperform obnovení, najdete v části [obnovit databázi ze zálohy](sql-database-recovery-using-backups.md).

> [!NOTE]
> V úložišti Azure termín hello *replikace* odkazuje toocopying soubory z jednoho umístění tooanother. SQL *replikace databáze* odkazuje tookeeping toomultiple sekundární databáze synchronizovat se službou primární databáze. 
> 

## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Kolik úložiště zálohy je zahrnuté zdarma?
Databáze SQL % too200 maximální zřízené databáze úložiště poskytuje úložiště zálohování bez dalších poplatků. Například pokud máte standardní DB instance s zřízené DB velikosti 250 GB, máte 500 GB úložiště zálohování bez dalších poplatků. Pokud vaše databáze překročí hello poskytuje úložiště záloh, můžete dobu uchování hello tooreduce kontaktováním podpory Azure. Další možností je toopay pro velmi zálohování úložiště, které se fakturuje hello standardní sazbou přístup pro čtení geograficky redundantní úložiště (RA-GRS). 

## <a name="how-often-do-backups-happen"></a>Jak často zálohám?
Úplná databáze zálohám týdně, rozdílové databáze obvykle zálohám každých několik hodin a transakčního protokolu, které se obecně zálohám každých 5 až 10 minut. ihned po vytvoření databáze, je naplánováno Hello první úplné zálohování. Obvykle dokončení do 30 minut, ale může trvat déle, pokud databáze hello značné velikosti. Například hello prvotní zálohování může trvat déle na obnovené databáze nebo kopii databáze. Po hello první úplné zálohování všechny další zálohy jsou naplánované automaticky a bezobslužně spravovat hello pozadí. Hello přesné načasování všechny zálohy databáze je dáno hello služby databáze SQL při vyvažování hello celkové zatížení systému. 

úložiště zálohování Hello geografická replikace dojde na základě plánu replikace Azure Storage hello.

## <a name="how-long-do-you-keep-my-backups"></a>Jak dlouho necháte Moje zálohování?
Každá záloha databáze SQL je doba uchování, která je založená na hello [vrstvy služeb](sql-database-service-tiers.md) hello databáze. Doba uchování Hello databáze v:


* Úroveň služby na úrovni Basic je 7 dní.
* Úroveň služby na úrovni Standard je 35 dní.
* Úroveň služeb Premium je 35 dnů.

Pokud jste starší verzi databáze z hello Standard nebo Premium služby úrovně tooBasic, hello zálohy budou uloženy sedm dní. Již nejsou k dispozici všechny stávající zálohy, které jsou starší než 7 dní. 

Pokud provádíte upgrade databáze z tooStandard vrstvy služby na úrovni Basic hello nebo Premium, SQL Database udržuje existující zálohy, dokud nedojde k jejich 35 dní starý. Když k nim dojde 35 dní udržuje nových záloh.

Pokud odstraníte databáze, databáze SQL uchová hello zálohy hello stejným způsobem, jako by tomu bylo v databázi online. Předpokládejme například, že odstraníte databáze Basic, který má dobu uchování o délce sedm dní. Zálohy, která je čtyři dny je uložit pro další tři dny.

> [!IMPORTANT]
> Pokud odstraníte který je hostitelem databáze systému SQL server Azure SQL hello, všechny databáze, které patří toohello server budou také odstraněny a nelze jej obnovit. Nelze obnovit odstraněné serveru.
> 

## <a name="how-tooextend-hello-backup-retention-period"></a>Jak tooextend hello zálohování doba uchování dat?
Pokud vaše aplikace vyžaduje, aby hello zálohy jsou k dispozici pro delší časové období, můžete rozšířit dobu uchování předdefinované hello konfigurace hello dlouhodobé uchovávání záloh zásad pro jednotlivé databáze (zleva doprava zásady). To vám umožní dobu uchování vytvořené it hello tooextend z 35 dní tooup too10 let. Další informace najdete v tématu [Dlouhodobé uchovávání](sql-database-long-term-retention.md).

Jakmile přidáte hello LTR zásad tooa databáze pomocí portálu Azure nebo rozhraní API, hello týdenní zálohování úplné databáze bude automaticky zkopírovaný tooyour vlastní úložiště služby Azure Backup. Pokud vaše databáze je šifrován TDE hello zálohy se šifrují automaticky v klidovém stavu.  Hello trezor služeb se automaticky odstraní vypršela platnost záloh na základě jejich časové razítko a hello zásad zleva doprava.  Proto není nutné plán zálohování hello toomanage nebo bez obav o vyčištění hello hello staré soubory. Hello obnovení podporuje rozhraní API trezoru záloh uložených v hello, pokud je k trezoru hello v hello stejnému předplatnému jako vaší databázi SQL. Hello portál Azure nebo tooaccess prostředí PowerShell můžete použít tyto zálohy.

> [!TIP]
> Jak tooguide, najdete v části [konfigurace a obnovení ze dlouhodobé uchovávání záloh Azure SQL Database](sql-database-long-term-backup-retention-configure.md)
>

## <a name="are-backups-encrypted"></a>Jsou zálohy šifrovat?

Když je povolené šifrování TDE pro Azure SQL database, jsou šifrované zálohování. Ve výchozím nastavení povolené šifrování TDE nastaveny všechny nové databáze Azure SQL. Další informace o šifrování TDE najdete v tématu [transparentní šifrování dat s Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database).

## <a name="next-steps"></a>Další kroky

- Zálohování databáze jsou nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože se data chránit před náhodnou poškození nebo odstranění. toolearn o hello jiných řešení kontinuity obchodních Azure SQL Database, najdete v části [obchodní kontinuity přehled](sql-database-business-continuity.md).
- toorestore tooa bodu v čase pomocí hello portál Azure, najdete v části [obnovit databáze tooa bodu v čase pomocí portálu Azure hello](sql-database-recovery-using-backups.md).
- toorestore tooa bodu v čase pomocí prostředí PowerShell, najdete v části [obnovit databáze tooa bodu v čase pomocí prostředí PowerShell](scripts/sql-database-restore-database-powershell.md).
- tooconfigure, spravovat a obnovit ze dlouhodobé uchovávání automatizované zálohování v trezoru služeb zotavení Azure pomocí hello Azure portálu, najdete v tématu [spravovat dlouhodobé uchovávání záloh pomocí portálu Azure hello](sql-database-long-term-backup-retention-configure.md).
- tooconfigure, spravovat a obnovit ze dlouhodobé uchovávání automatizované zálohování v trezoru služeb zotavení Azure pomocí prostředí PowerShell, najdete v části [spravovat pomocí prostředí PowerShell dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).
