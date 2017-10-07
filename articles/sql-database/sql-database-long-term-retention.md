---
title: "aaaStore zálohy databáze SQL Azure pro až roky too10 | Microsoft Docs"
description: "Zjistěte, jak Azure SQL Database podporuje ukládání záloh pro až too10 let."
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 5825ebd4e3bd66b59b13aea603d377ef814a1df3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="store-azure-sql-database-backups-for-up-too10-years"></a>Uložte zálohy databáze SQL Azure pro až too10 let
Mnoho aplikací mít regulačních, dodržování předpisů nebo jiné obchodní účely, které vyžadují tooretain zálohy databáze nad rámec hello 7-35 dní od Azure SQL Database [automatické zálohování](sql-database-automated-backups.md). Pomocí funkce dlouhodobé uchovávání záloh hello můžete uložit zálohování databáze SQL v trezoru služeb zotavení Azure pro až too10 let. Můžete uložit až too1 000 databází na jeden trezor. Pak můžete vybrat jakékoli zálohy v trezoru toorestore hello ho jako novou databázi.

> [!IMPORTANT]
> Dlouhodobé uchovávání záloh je momentálně ve verzi preview a je k dispozici v následujících oblastech hello: Austrálie – východ, Austrálie – jihovýchod, Brazílie – Jih, střed USA, východní Asie, východní USA, Východ USA 2, Indie – střed, Indie – Jih, Japonsko – východ, Japonsko – Západ, Sever střední USA, severní Evropa, střed USA – Jih, jihovýchodní Asie, západní Evropa a západní USA.
>

> [!NOTE]
> Zálohu databáze too200 jednomu trezoru můžete povolit v období 24 hodin. Doporučujeme vám, že používáte samostatné úložiště pro každý server toominimize hello dopad tento limit. 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a>Jak funguje dlouhodobé uchovávání záloh databáze SQL

S dlouhodobé uchovávání záloh můžete databázový server SQL přidružit trezoru služeb zotavení Azure. 

* Musíte vytvořit trezor hello v hello stejného předplatného Azure, který vytvořili hello SQL serveru a v hello stejné zeměpisné oblasti nebo skupině prostředků. 
* Nakonfigurujete zásady uchovávání informací pro všechny databáze. Hello zásad příčiny hello týdenní úplná databáze zálohy toobe zkopíruje toohello trezor služeb zotavení a uchovávají po dobu uchovávání hello (až roky too10). 
* Pak můžete obnovit databáze hello z jakéhokoli z těchto zálohy tooa novou databázi v libovolném serveru v odběru hello. Úložiště Azure vytvoří kopii z existující zálohy a kopírování hello nemá žádný vliv výkon na existující databázi hello.

> [!TIP]
> Jak tooguide, najdete v části [konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).

## <a name="enable-long-term-backup-retention"></a>Povolit dlouhodobé uchovávání záloh

tooconfigure dlouhodobé uchovávání záloh pro databázi:

1. Vytvoření trezoru služeb zotavení Azure služby v hello stejnou oblast, předplatné a prostředků skupinu jako databázový server SQL. 
2. Registrace trezoru toohello server hello.
3. Vytvoření zásady ochrany služeb zotavení Azure.
4. Použijte hello ochrany zásad toohello databáze, které vyžadují dlouhodobé uchovávání záloh.

tooconfigure, spravovat a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure, proveďte jednu z následujících hello:

* Pomocí portálu Azure hello: klikněte na tlačítko **dlouhodobé uchovávání záloh**, vyberte databázi a pak klikněte na tlačítko **konfigurace**. 

   ![Vyberte databázi pro dlouhodobé uchovávání záloh](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* Pomocí prostředí PowerShell: Přejděte příliš[konfigurace a obnovení z Azure SQL Database dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-a-database-thats-stored-with-hello-long-term-backup-retention-feature"></a>Obnovit databázi, která je uložena s hello dlouhodobé uchovávání záloh funkcí

toorecover ze zálohování na dlouhodobé uchovávání záloh:

1. Kde je uložena záloha hello trezor hello seznamu.
2. Kontejner hello seznamu, který je namapované tooyour logického serveru.
3. Seznam hello zdroj dat v rámci hello trezoru, který je namapované tooyour databáze.
4. Seznam hello bodů obnovení, které jsou k dispozici toorestore.
5. Obnovení databáze hello z hello obnovení bodu toohello cílového serveru v rámci vašeho předplatného.

tooconfigure, spravovat a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure, proveďte jednu z následujících hello:

* Pomocí portálu Azure hello: přejděte příliš[spravovat dlouhodobé uchovávání záloh pomocí portálu Azure hello](sql-database-long-term-backup-retention-configure.md). 

* Pomocí prostředí PowerShell: Přejděte příliš[spravovat pomocí prostředí PowerShell dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).

## <a name="get-pricing-for-long-term-backup-retention"></a>Získat ceny pro dlouhodobé uchovávání záloh

Dlouhodobé uchovávání záloh databáze SQL je účtován podle toohello [služby Azure backup ceny sazby](https://azure.microsoft.com/pricing/details/backup/).

Po hello server databáze SQL je registrovaný toohello trezoru, vám budou účtovat hello celkové úložiště, který je používán hello týdenní zálohy uložené v trezoru hello.

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a>Zobrazit dostupné zálohy, které jsou uložené v dlouhodobé uchovávání záloh

tooconfigure, spravovat a obnovit databázi z dlouhodobé uchovávání záloh automatizované zálohování v trezoru služeb zotavení Azure pomocí hello portálu Azure, proveďte jednu z následujících hello:

* Pomocí portálu Azure hello: přejděte příliš[spravovat dlouhodobé uchovávání záloh pomocí portálu Azure hello](sql-database-long-term-backup-retention-configure.md). 

* Pomocí prostředí PowerShell: Přejděte příliš[spravovat pomocí prostředí PowerShell dlouhodobé uchovávání záloh](sql-database-long-term-backup-retention-configure.md).

## <a name="disable-long-term-retention"></a>Zakázat dlouhodobé uchovávání

služby zotavení Hello automaticky zpracovává hello čištění záloh podle hello zadané zásady uchovávání informací. 

toostop odesílání hello zálohy pro konkrétní databázi toohello trezoru, odeberte hello zásady uchovávání informací pro tuto databázi.
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> Hello zálohování, které jsou již v trezoru hello jsou poškozena. Jsou automaticky odstraněny službou obnovení hello když vyprší platnost dobu uchování.

## <a name="long-term-backup-retention-faq"></a>Dlouhodobé uchovávání záloh – nejčastější dotazy

**Můžete ručně odstranit konkrétní zálohy v trezoru hello?**

Aktuálně nepodporuje. Hello trezoru záloh automaticky vyčistí, pokud vypršela doba uchování hello.

**Můžete zaregistrovat my server toostore zálohy toomore než jeden trezor?**

Ne, můžete uložit aktuálně jeden trezor záloh tooonly najednou.

**Může mít trezoru a server v různých předplatných?**

Ne, aktuálně hello trezoru a server musí být v hello stejné předplatném nebo skupině prostředků.

**Můžete použít k trezoru, vytvořené v oblasti, která se liší od oblasti svému serveru?**

Ne, hello trezoru a server musí být v hello stejné oblasti toominimize zkopírujte čas a náklady na provoz.

**Kolik databáze můžete ukládat do jednoho trezoru?**

V současné době podporujeme až too1 000 databází na jeden trezor. 

**Kolik trezorů můžete vytvořit na jedno předplatné?**

Můžete vytvořit až too25 trezory jedno předplatné.

**Kolik databází můžete nakonfigurovat za den za trezoru?**

Můžete nastavit 200 databáze za den za trezor.

**Funguje s elastické fondy dlouhodobé uchovávání záloh?**

Ano. Všechny databáze ve fondu hello se dá nakonfigurovat s hello zásady uchovávání informací.

**Můžete zvolit hello čas, kdy je vytvořeno hello zálohování?**

Ne, databáze SQL řídí vlivu na výkon hello toominimize plán zálohování hello vašich databází.

**Je nutné transparentní šifrování dat pro databázi povoleno. Můžete použít ho k trezoru hello?** 

Ano, je podporováno transparentní šifrování dat. Hello databázi lze obnovit z trezoru hello i v případě hello původní databáze již existuje.

**Co se stane s hello zálohy v trezoru hello, pokud je pozastavená Moje předplatné?** 

Pokud je předplatné pozastavené, jsme zachovat hello existující databáze a zálohování. Nových záloh nejsou zkopírovaný toohello trezoru. Po hello předplatné znovu aktivujete, služba hello obnoví kopírování toohello trezoru záloh. Operace obnovení přístupné toohello pomocí hello zálohování, které byly zkopírovány existuje před pozastavením hello předplatné se změní na svůj trezor. 

**Lze získat přístup záložní soubory databáze SQL toohello tak I stáhnout nebo obnovení je toohello SQL serveru?**

Ne, aktuálně nepodporuje.

**Je možné toohave vícenásobné plány (denně, týdně, měsíčně, ročně) v rámci zásady uchovávání informací SQL.**

Ne, víc plány jsou aktuálně dostupné jen pro zálohy virtuálních počítačů.

**Co když nastavíme dlouhodobé uchovávání záloh na databázi, která se nachází aktivní geografickou replikací sekundární databáze?**

Protože jsme nemáte trvat zálohy na replikách, je aktuálně žádná možnost pro dlouhodobé uchovávání zálohování na sekundární databáze. Ale je důležité pro uživatele tooset až dlouhodobé uchovávání záloh na sekundární databázi aktivní geografickou replikaci z těchto důvodů:
* Pokud dojde převzetí služeb při selhání a hello databáze se stane primární databázi, jsme trvat úplné zálohování, který je nahraný toovault.
* Neexistuje žádné další náklady toohello zákazníka pro nastavení dlouhodobé uchovávání záloh na sekundární databáze.

## <a name="next-steps"></a>Další kroky
Protože zálohy databáze chránit data před náhodným poškození nebo odstranění, jsou nedílnou součást vámi vyžádaných žádné kontinuity podnikových procesů a strategie zotavení po havárii. toolearn o hello jiných řešení kontinuity podnikových procesů SQL Database, najdete v části [obchodní kontinuity přehled](sql-database-business-continuity.md).
