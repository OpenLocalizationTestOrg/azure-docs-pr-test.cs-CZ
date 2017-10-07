---
title: "aaaAzure nejčastější dotazy týkající se databáze SQL | Microsoft Docs"
description: "Odpovědi toocommon dotazy zákazníků, požádejte o cloudu databáze a databáze SQL Azure, společnosti Microsoft systému správy relačních databází (RDBMS) a databáze jako služba v cloudu hello."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 1da12abc-0646-43ba-b564-e3b049a6487f
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 02/07/2017
ms.author: sashan;carlrab
ms.openlocfilehash: 04c02f96e6e91cf314221134ee0ef6d24217ef45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-faq"></a>SQL Database – Nejčastější dotazy

## <a name="what-is-hello-current-version-of-sql-database"></a>Co je aktuální verze hello SQL Database?
je Hello aktuální verze databáze SQL verze 12. Verze 11 verze byla vyřazena.

## <a name="what-is-hello-sla-for-sql-database"></a>Co je hello SLA pro databázi SQL?
Nemůžeme zaručit alespoň 99,99 % hello čas zákazníků bude mít připojení mezi jejich jednotlivé nebo elastické databáze Basic, Standard, nebo Premium Microsoft Azure SQL Database a naše internetovou bránu. Další informace najdete v tématu [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="how-do-i-reset-hello-password-for-hello-server-admin"></a>Jak resetovat heslo hello Dobrý den, správce serveru
V hello [portál Azure](https://portal.azure.com) klikněte na tlačítko **servery SQL**, vyberte hello server hello seznamu a pak klikněte na tlačítko **resetovat heslo**.

## <a name="how-do-i-manage-databases-and-logins"></a>Jak spravovat databází a přihlašovacích údajů?
V tématu [Správa databází a přihlašovacích údajů](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-tooaccess-a-server"></a>Jak mohu učinit se pouze autorizovaným IP adresy jsou povoleny tooaccess serveru?
V tématu [postupy: Konfigurace nastavení brány firewall pro službu SQL Database](sql-database-configure-firewall-settings.md).

## <a name="how-does-hello-usage-of-sql-database-show-up-on-my-bill"></a>Jak hello využití SQL Database objeví na Moje faktury?
Databáze SQL faktur na předvídatelný hodinové rychlost podle vrstvy služby hello + výkonu úrovně pro izolované databáze nebo Edtu za elastického fondu. Skutečné využití je počítaný a poměrně každou hodinu, takže vaše faktura může zobrazovat zlomků jednu hodinu. Například pokud databáze existuje 12 hodin za měsíc, vaše faktura zobrazuje využití 0,5 dní. Kromě toho jsou přerušená úrovně služeb + úroveň výkonu a Edtu na fond out v toomake faktury hello je snazší toosee hello počet dní databáze můžete použít pro každý za jeden měsíc.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Co v případě jedné databáze je aktivní méně než jednu hodinu nebo používá vyšší úroveň služby pro méně než jednu hodinu?
Fakturuje se pro každou hodinu, které existuje databáze pomocí hello nejvyšší úroveň služby + výkonu úrovně, použít danou dobu, bez ohledu na využití nebo zda byl aktivní hello databáze méně než jednu hodinu. Například pokud vytvoříte jedné databáze a odstranit ji pěti minutách vaše faktura odráží zdarma pro jednu databázi hodinu. 

Příklady

* Pokud chcete vytvořit základní databáze a pak okamžitě upgradovat tooStandard S1, budou se vám účtovat rychlostí hello Standard S1 pro hello první hodinu.
* Pokud upgradujete databázi z základní tooPremium na 10:00. a dokončení upgradu v 1:35 hodin na hello následující den budou se vám účtovat rychlostí Premium hello spouštění v 1:00 
* Pokud jste starší verzi databáze z Premium tooBasic ve 20:00 a dokončení ve 2:15, pak hello databáze bude účtován sazbou hello Premium až 3:00 hodin, po jejímž uplynutí je účtován základní tempem hello.

## <a name="how-does-elastic-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Jak využití elastického fondu zobrazit na můj fakturaci a co se stane, když změnit počet jednotek Edtu na fond?
Elastický fond účtuje zobrazit nahoru na vaší faktuře jako elastické jednotky Dtu (Edtu) v přírůstcích po hello zobrazený pod Edtu na fond na [hello stránce s cenami](https://azure.microsoft.com/pricing/details/sql-database/). Je bezplatná jednotlivé databáze pro elastické fondy. Fakturuje se pro každou hodinu, které fond existuje v hello nejvyšší eDTU, bez ohledu na využití nebo jestli byl fond hello aktivní méně než jednu hodinu. 

Příklady

* Pokud vytvoříte standardní elastický fond s 200 Edtu v 11:18:00, přidání pět fondu toohello databází, budeme vám účtovat 200 Edtu hello celou hodinu od v 11: 00 prostřednictvím hello zbytek hello den.
* Den 2 na 5:05:00. 1 databáze začne využívání 50 Edtu a má konstantní prostřednictvím hello den. Databáze 2 až 5 kolísá mezi 0 a 80 Edtu. Dni Dobrý den můžete přidat pět jiných databází, které využívají různých jednotek Edtu, které v průběhu dne hello. Den 2 je plný den účtovat podle 200 eDTU. 
* Den 3, ve 5: 00. můžete přidat další 15 databáze. Využití databáze se zvyšuje v rámci hello den toohello bodu, kde rozhodnete tooincrease Edtu pro fond hello z 200 too400 ve 20:05. Poplatky na úrovni eDTU hello 200 bylo platné až 20: 00 a zvyšuje too400 Edtu pro hello zbývající čtyři hodiny. 

## <a name="elastic-pool-billing-and-pricing-information"></a>Elastický fond fakturaci a informace o cenách
Elastické fondy fakturují za hello následující vlastnosti:

* Elastický fond se fakturuje po jeho vytvoření i v případě, že nejsou žádné databáze ve fondu hello.
* Elastický fond se fakturuje každou hodinu. To je hello stejné měření frekvence jako úrovně výkonu izolovaných databází.
* Pokud elastickém fondu se změněnou velikostí tooa nové není počet jednotek Edtu, pak hello fondu účtují podle toohello nové množství Edtu, dokud se nedokončí hello Změna velikosti operaci. To odpovídá hello stejný vzor jako změna hello úroveň výkonu izolovaných databází.
* cena Hello elastického fondu je založena na hello počet jednotek Edtu fondu hello. cena Hello elastického fondu je nezávislé na číslo hello a využití hello elastické databáze v něm.
* Cena se počítá podle (počet jednotek Edtu fondu) x (jednotkové ceny za eDTU).

Hello jednotkové ceny eDTU fondu elastické databáze je vyšší než hello jednotkové ceny DTU pro jednu databázi v hello stejnou úroveň služby. Podrobnosti najdete na stránce s [cenami služby SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). 

jednotky Edtu a služby vrstev hello toounderstand najdete v části [výkon a možnosti databáze SQL](sql-database-service-tiers.md).

## <a name="how-does-hello-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Jak se používá hello aktivní geografické replikace v elastický fond se zobrazí na Moje faktury?
Na rozdíl od izolovaných databází můžete pomocí [aktivní geografickou replikací](sql-database-geo-replication-overview.md) s elastické databáze nemá přímý vliv fakturace.  Se účtují poplatky pro hello Edtu zřízené pro každou fondů hello (fondu primární a sekundární fond)

## <a name="how-does-hello-use-of-hello-auditing-feature-impact-my-bill"></a>Jak hello používá z hello auditování dopad funkce Moje faktury?
Auditování je integrovaná do hello služby SQL Database bez jakýchkoli nákladů a je k dispozici tooBasic, standardní, Premium a Premium RS databáze. Ale toostore hello protokoly auditu, hello auditování funkce používá účet úložiště Azure a sazby za tabulky a fronty ve službě Azure Storage se vztahují na základě velikosti hello protokolu auditu.

## <a name="how-do-i-find-hello-right-service-tier-and-performance-level-for-single-databases-and-elastic-pools"></a>Jak najít hello správné služby vrstvy a úroveň výkonu pro izolované databáze i elastické fondy?
Existuje několik nástrojů k dispozici tooyou. 

* Pro místní databáze, použijte hello [DTU nastavení velikosti advisor](http://dtucalculator.azurewebsites.net/) toorecommend hello databáze a požadované Dtu a vyhodnotit více databází pro elastické fondy.
* Pokud jedné databáze by využívat výhody ve fondu, inteligentního modul Azure pokud ji vidí historie využití vzor, která vyžaduje se doporučuje fondu elastické databáze. V tématu [monitorování a správa elastického fondu pomocí portálu Azure hello](sql-database-elastic-pool-manage-portal.md). Podrobnosti o tom, jak toodo hello matematické sami najdete v tématu [cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool.md)
* toosee jestli potřebujete toodial jedné databáze nahoru nebo dolů, najdete v části [výkonu pokyny pro izolované databáze](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-hello-service-tier-or-performance-level-of-a-single-database"></a>Jak často můžete změnit úroveň výkonu a vrstvu služby hello služby jedné databáze?
Můžete změnit úroveň služby hello (mezi Basic, Standard, Premium a Premium RS) nebo často chcete hello úroveň výkonu v rámci vrstvu služby (například S1 tooS2). Pro starší verze databáze můžete změnit úroveň výkonu a vrstvu služby hello celkem čtyřikrát za období 24 hodin.

## <a name="how-often-can-i-adjust-hello-edtus-per-pool"></a>Jak často můžete upravit hello Edtu na fond?
Tolikrát, kolikrát chcete.

## <a name="how-long-does-it-take-toochange-hello-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Jak dlouho ukládá ji provést toochange hello výkon nebo vrstvě úroveň služby jedné databáze nebo přesun databáze do/z fondu elastické databáze?
Změna úrovně služby hello databáze a přesouvání a odhlašování fondu vyžaduje toobe databáze hello zkopírovali na platformě hello jako operaci na pozadí. Změna úrovně služby hello může trvat několik minut tooseveral hodin v závislosti na velikosti hello hello databází. V obou případech hello databáze zůstat online a dostupný při přesunutí hello. Podrobnosti o změně izolované databáze najdete v tématu [změnu hello úroveň služby databáze](sql-database-service-tiers.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Kdy použít jednu databázi oproti elastické databáze?
Obecně platí, elastické fondy navržených pro typické [vzor aplikace software jako služba (SaaS)](sql-database-design-patterns-multi-tenancy-saas-applications.md), kde je jedna databáze na zákazníka nebo klienta. Nákupu jednotlivé databáze a předimenzování toomeet hello proměnnou a ve špičce potřebují pro každou databázi není často nákladově efektivní. Fondy spravovat hello celkový výkon poskytovaný hello fondu a databáze hello škálovat nahoru a dolů automaticky. Inteligentní modul Azure doporučuje fond pro databáze při vzor používání zaručuje ho. Podrobnosti najdete v tématu [elastického fondu pokyny](sql-database-elastic-pool.md).

## <a name="what-does-it-mean-toohave-up-too200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Co znamená toohave too200 % maximální zřízené databáze úložiště pro úložiště pro zálohování?
Úložiště zálohování je hello úložiště přidružený k vaší zálohy automatizované databáze, které se používají pro [bodu-v-čas – obnovení](sql-database-recovery-using-backups.md#point-in-time-restore) a [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL Database poskytuje too200 % maximální zřízené databáze úložiště zálohování úložiště bez dalších poplatků. Například pokud máte standardní DB instance s zřízené DB velikosti 250 GB, jsou k dispozici 500 GB úložiště zálohování bez dalších poplatků. Pokud vaše databáze překročí hello poskytuje úložiště záloh, můžete vybrat dobu uchování hello tooreduce kontaktováním podpory Azure nebo platit za úložiště velmi zálohování hello účtovat standardní sazbou přístup pro čtení geograficky redundantní úložiště (RA-GRS). Další informace o fakturaci RA-GRS najdete v části Podrobnosti o cenách úložiště.

## <a name="im-moving-from-webbusiness-toohello-new-service-tiers-what-do-i-need-tooknow"></a>I mě přechod z webového nebo obchodní toohello nové úrovně služeb, co dělat, je potřeba tooknow?
Nyní jsou vyřazené databáze Azure SQL Web a Business. úrovně Basic, Standard, Premium, Premium RS a Elastická Hello nahraďte hello vyřazení databáze Web a Business. 

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-hello-same-azure-geography"></a>Co je očekávané replikace funkce lag při geografická replikace a databáze mezi dvěma oblastmi v rámci stejné hello Azure geography?
Nemůžeme se aktuálně podporují RPO pět sekund a hello replikace funkce lag musí být menší než při hello geo sekundární je hostován v hello, nedoporučuje se spárované oblast Azure a v hello stejné úrovně služby.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-hello-same-region-as-hello-primary-database"></a>Co je očekávané replikace funkce lag při geograficky sekundární ve hello stejné oblasti jako primární databáze hello?
Na základě na základě zkušeností data, není příliš mnoho rozdíl mezi uvnitř oblasti a prodleva mezi oblast replikace při hello Azure doporučeno spárované oblast se používá. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-hello-retry-logic-work-when-geo-replication-is-set-up"></a>Pokud je mezi dvěma oblastmi selhání sítě, jak logika opakovaných pokusů hello funguje při geografická replikace je nastaven?
Pokud dojde k odpojení, opakované každých 10 sekund toore-navázat připojení.

## <a name="what-can-i-do-tooguarantee-that-a-critical-change-on-hello-primary-database-is-replicated"></a>Jak tooguarantee, že se replikují na kritické změnu hello primární databáze?
je Hello geo sekundární repliku asynchronní a jsme nepokoušejte tookeep v úplnou synchronizaci s hello primární. Ale poskytujeme metoda tooforce synchronizace tooensure hello replikace důležitých změn (například aktualizace hesel). Vynucené synchronizace ovlivňuje výkon, protože blokuje hello volající vlákno, dokud se replikují všechny potvrzené transakce. Podrobnosti najdete v tématu [uložená procedura sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-toomonitor-hello-replication-lag-between-hello-primary-database-and-geo-secondary"></a>Jaké nástroje jsou k dispozici toomonitor hello replikace prodleva mezi hello primární databáze a také sekundární geograficky?
Zveřejňujeme hello v reálném čase replikace prodleva mezi hello primární databáze a geo sekundární prostřednictvím DMV. Podrobnosti najdete v tématu [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="toomove-a-database-tooa-different-server-in-hello-same-subscription"></a>toomove tooa databáze jiného serveru v hello stejného předplatného.
* V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **databází SQL**, vyberte databázi z hello seznamu a pak klikněte na tlačítko **kopie**. V tématu [zkopírujte Azure SQL database](sql-database-copy.md) další podrobnosti.

## <a name="toomove-a-database-between-subscriptions"></a>toomove databáze mezi předplatnými
* V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **servery SQL** a pak vyberte server hello, který je hostitelem databáze ze seznamu hello. Klikněte na tlačítko **přesunout**a pak vyberte hello prostředky toomove a toomove předplatné hello k.
