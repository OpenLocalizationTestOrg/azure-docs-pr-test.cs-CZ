---
title: "aaaFailover skupin a aktivní geografickou replikaci - Azure SQL Database | Microsoft Docs"
description: "Automatické převzetí služeb při selhání skupiny s aktivní geografickou replikací umožňuje toosetup 4 repliky databáze v některém z hello datových centrech Azure a autoomatically převzetí služeb při selhání v případě hello výpadku."
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2a29f657-82fb-4283-9a83-e14a144bfd93
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 07/10/2017
ms.author: sashan
ms.openlocfilehash: 7a39af8505624c0f19c5abccff051af836b1f9bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-failover-groups-and-active-geo-replication"></a>Přehled: Převzetí služeb při selhání skupiny a aktivní geografickou replikaci
Aktivní geografickou replikací vám umožní tooconfigure až toofour čitelný sekundární databáze v hello stejný nebo jiný datového centra umístění (oblastí). Sekundární databáze jsou k dispozici pro dotazování a převzetí služeb při selhání v případě hello data center výpadku nebo hello nemohou tooconnect toohello primární databáze. Hello převzetí služeb při selhání musí ručně zahájit hello aplikace hello uživatele. Po převzetí služeb při selhání má hello nový primární koncový bod jiné připojení. 

> [!NOTE]
> Aktivní geografickou replikací je k dispozici pro všechny databáze na všech úrovních služby ve všech oblastech.
>  

Automatické převzetí služeb při selhání skupiny Azure SQL Database (-preview) je tooautomatically funkce určená SQL Database spravovat relace geografická replikace, připojení a převzetí služeb při selhání ve velkém měřítku. Pomocí něho hello zákazníkům nárůst hello možnost tooautomatically obnovit více souvisejících databáze v sekundární oblasti hello po závažné regionální chyby nebo další neplánované události, které služba SQL Database za následek úplné nebo částečné ztrátě hello dostupnost v primární oblasti hello. Kromě toho jde pomocí úlohy hello čitelný sekundární databáze toooffload jen pro čtení.  Protože skupiny automatické převzetí služeb při selhání zahrnují více databází musí být nakonfigurované na primárním serveru hello. Primární a sekundární servery musí být v hello stejného předplatného. Automatické převzetí služeb při selhání skupiny podporují replikace všech databází v hello skupiny tooonly jednu sekundární server v jiné oblasti. Aktivní geografickou replikaci, bez skupiny automatické převzetí služeb při selhání umožňuje až too4 sekundární databáze v libovolné oblasti.

Pokud používáte aktivní geografickou replikaci a u příčiny selže primární databázi, nebo jednoduše potřeby toobe do režimu offline, můžete spustit převzetí služeb při selhání tooany sekundárních databází. Při převzetí služeb při selhání je aktivovaná tooone hello sekundární databáze, jsou všechny ostatní sekundární repliky automaticky propojené toohello nový primární. Pokud používáte – automatické převzetí služeb při selhání skupiny obnovení databáze (v preview) toomanage a všechny výpadek, který má dopad na jeden nebo několik hello databází ve skupině hello za následek automatické převzetí služeb při selhání. Můžete nakonfigurovat zásady hello-automatické převzetí služeb při selhání, který bude nejlépe vyhovovat potřebám vaší aplikace, nebo můžete odhlásit a použít ruční aktivaci. Kromě toho skupiny automatické převzetí služeb při selhání (v preview) nabízí pro čtení a zápis a koncových bodů naslouchání jen pro čtení, které zůstanou beze změny během převzetí služeb při selhání. Při použití aktivace ruční nebo automatické převzetí služeb při selhání, převzetí služeb při selhání přepne všechny sekundární databáze v tooprimary skupiny hello. Po dokončení převzetí služeb při selhání hello databáze je záznam DNS hello automaticky aktualizovány tooredirect hello koncových bodů toohello novou oblast.

Můžete spravovat replikace a převzetí služeb při selhání může jedna databáze nebo sada databází na serveru nebo ve fondu elastické databáze používá aktivní geografickou replikaci. Můžete to, že pomocí hello [portál Azure](sql-database-geo-replication-portal.md), [prostředí PowerShell](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md), nebo hello [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). Po převzetí služeb při selhání Ujistěte se, že hello požadavky na ověřování pro server a databáze jsou nakonfigurovány na nový primární hello. Podrobnosti najdete v tématu [zabezpečení SQL Database po zotavení po havárii](sql-database-geo-replication-security-config.md). To platí i tooactive geografická replikace automatické převzetí služeb při selhání skupiny a (v preview).

Aktivní geografickou replikací využívá hello [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) technologie SQL Server tooasynchronously replikují potvrzené transakce na hello primární databáze tooa sekundární databáze pomocí izolace snímku čtení potvrdit (RCSI). Automatické převzetí služeb při selhání skupiny poskytují hello skupiny sémantiku nad aktivní geografickou replikaci, ale hello, který se používá stejný mechanismus asynchronní replikaci. V libovolném časovém okamžiku hello sekundární databáze může být mírně za hello primární databázi, záruku, že data sekundární hello toonever mít částečné transakce. Mezi oblastmi redundance umožňuje obnovit tooquickly aplikace z trvalé ztrátě celého datového centra nebo částí datacentru způsobené přírodní katastrofy, lidské chyby závažné nebo škodlivý jednání. Hello konkrétní plánovaný bod obnovení dat lze nalézt na [přehled kontinuity podnikových procesů](sql-database-business-continuity.md).

Hello následující obrázek znázorňuje příklad aktivní geografickou replikací nakonfigurované s v oblasti severní centrální nám hello primární a sekundární v oblasti hello jihu USA.

![geografická replikace relace](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Protože jsou čitelné hello sekundární databáze, může se jednat použité toooffload jen pro čtení úlohy, jako například úlohy sestav. Pokud používáte aktivní geografickou replikaci, je možné toocreate hello sekundární databáze v hello nezvyšuje stejné oblasti s hello primární, ale aplikace hello odolnost toocatastrophic selhání. Pokud používáte skupiny automatické převzetí služeb při selhání (v preview), je vždy vytvořit sekundární databáze v jiné oblasti.

Kromě toho toodisaster obnovení active geografická replikace mohou být používány hello následující scénáře:

* **Migrace databáze**: toomigrate aktivní geografickou replikaci databáze z jednoho serveru tooanother online můžete použít s minimální výpadky.
* **Upgrady aplikací**: můžete vytvořit další sekundární jako kopii back selhání během upgradů aplikací.

tooachieve skutečné provozní kontinuitu, přidání redundance databáze mezi datovými centry je jenom část hello řešení. Aplikaci (služba) na kompletní obnovení po závažné chybě vyžaduje obnovení všechny součásti, které tvoří hello služby a všechny závislé služby. Všechny tyto komponenty příklady hello klientský software (například prohlížeč s vlastní JavaScript), webové servery front-end, úložiště a DNS. Je důležité, aby všechny součásti jsou odolné toohello stejné chyby a bude k dispozici v cíli času obnovení (RTO) hello vaší aplikace. Proto nutné tooidentify všechny závislé služby a pochopit hello záruky a možnosti, které poskytují. Pak je třeba provést tooensure přiměřené kroky, které hello funkcí služby během převzetí služeb při selhání hello služeb, na kterých závisí. Další informace o návrhu řešení pro zotavení po havárii, najdete v části [návrhu cloudové řešení pro obnovení po havárii pomocí aktivní geografickou replikaci](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Možnosti aktivní geografickou replikaci
funkce aktivní geografickou replikací Hello poskytuje hello následující základní možnosti:
* **Automatické asynchronní replikaci**: přidáním tooan existující databázi lze vytvořit pouze sekundární databáze. Hello sekundární lze vytvořit v jakékoli serveru Azure SQL Database. Po vytvoření se zobrazí v sekundární databáze hello hello daty zkopírovanými z primární databáze hello. Tento proces se označuje jako synchronizace replik indexů. Po sekundární databáze byla vytvořena a nasadí, aktualizace toohello primární databáze jsou asynchronně replikuje toohello sekundární databáze automaticky. Asynchronní replikaci znamená, že transakce jsou potvrzeno u primární databáze hello předtím, než budou toohello replikované sekundární databáze. 
* **Čitelný sekundární databáze**: aplikace můžete přístup k sekundární databázi pro operace jen pro čtení pomocí hello stejné nebo objekty zabezpečení různých použité pro přístup k primární databázi hello. Hello sekundární databáze pracovat v izolaci snímku režimu tooensure replikace z hello aktualizací hello primární (protokol opětovného přehrání) není zpožděn dotazy spouštěné na sekundární hello.

   > [!NOTE]
   > Hello protokolu opakování zpožděno hello sekundární databáze případné aktualizace schématu přijímá z hello primární, které vyžadují zámek schématu hello sekundární databáze. 
   > 

* **Více čitelný sekundární repliky**: dvě nebo více sekundárních databází zvýšit redundance a úroveň ochrany pro primární databáze hello a aplikace. Existuje více sekundární databáze, i nadále aplikace hello chráněn, i když dojde k selhání jednoho hello sekundární databáze. Pokud existuje pouze jedna sekundární databáze, a se nezdaří, je vystaven aplikace hello toohigher riziko, dokud nebude vytvořena nová sekundární databáze.

   > [!NOTE]
   > Pokud používáte aktivní geografickou replikací toobuild globálně distribuované aplikace a potřebujete přístup jen pro čtení tooprovide toodata v oblastech více než 4, můžete vytvořit sekundární sekundární (Tento proces se označuje jako řetězení). Tímto způsobem můžete dosáhnout prakticky neomezené škálování replikace databáze. Kromě toho řetězení snižuje zatížení hello replikace z primární databáze hello. kompromis Hello je hello vyšší replikace funkce lag na hello listu většinu sekundární databáze. . 
   >

* **Podpora fondu elastické databáze**: aktivní geografickou replikací je možné nakonfigurovat pro všechny databáze v každém elastického fondu. Hello sekundární databáze může být v jiném elastického fondu. Pro normální databáze lze hello sekundární fondu elastické databáze a naopak stejné hello, pokud jsou hello úrovně služeb. 
* **Úroveň výkonu konfigurovat sekundární databáze hello**: sekundární databáze se dají vytvořit s nižší úroveň výkonu než hello primární. Primární i sekundární databáze jsou požadované toohave hello stejnou úroveň služby. Tato možnost není doporučeno pro aplikace s aktivitou write vysoké databáze, protože hello vyšší replikace funkce lag zvyšuje hello riziko ztráty významné dat po selhání. Kromě toho po ovlivní výkon aplikace hello převzetí služeb při selhání až hello nové primární je vyšší výkon upgradovaný tooa úroveň. Hello protokolu vstupně-výstupní operace procento grafu na portál Azure poskytuje vhodný způsob tooestimate hello minimální výkonu úroveň hello sekundární, který je požadovaný toosustain hello replikace zatížení. Například, pokud je primární databáze P6 (1 000 DTU) a protokol vstupně-výstupní operace v procentech je 50 % hello sekundární potřebuje toobe nejméně P4 (500 DTU). Můžete také načíst hello protokolu vstupně-výstupní operace dat pomocí [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) nebo [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) databáze zobrazení.  Další informace o hello úrovně výkonu databáze SQL najdete v tématu [výkon a možnosti databáze SQL](sql-database-service-tiers.md). 
* **Uživatelem řízená převzetí služeb při selhání a navrácení služeb po obnovení**: sekundární databáze může být explicitně primární roli vypnuté toohello kdykoli hello aplikace nebo uživatel hello. Během skutečné výpadku hello "neplánované" možnost by měl použít, které okamžitě zvýší úroveň primární sekundární toobe hello. Při selhání primární hello obnoví a je k dispozici znovu, hello systém automaticky značky hello obnovit primární jako sekundární a převeďte ho do aktuální s nový primární hello. Z důvodu toohello asynchronní povaze replikace malé množství dat může dojít ke ztrátě během neplánované převzetí služeb při selhání primárního selže ještě před replikací hello nejnovější změny toohello sekundární. Při selhání primárního serveru s více sekundárních hello systému automaticky změní konfiguraci relace replikace hello a odkazy hello zbývající sekundární repliky toohello nově povýší primární bez nutnosti zásahu uživatele. Po výpadku hello, která způsobila, že převzetí služeb při selhání hello zmírnit, může být žádoucí tooreturn hello aplikace toohello primární oblasti. toodo, že příkaz hello převzetí služeb při selhání by měla být volána s hello "plánované" možnost. 
* **Udržování synchronizace přihlašovacích údajů a pravidel brány firewall**: doporučujeme používat [databáze pravidla brány firewall](sql-database-firewall-configure.md) pro geograficky replikované databáze, tato pravidla je možné replikovat s hello databáze tooensure všechny sekundární databáze mají Hello stejná pravidla brány firewall jako primární hello. Tento přístup eliminuje potřebu hello zákazníci toomanually konfiguraci a údržbu pravidla brány firewall na servery, které hostují jak hello primární a sekundární databáze. Podobně [obsažené uživatelů databáze](sql-database-manage-logins.md) pro data přístup zajišťuje primární i sekundární databáze mají vždy hello stejné uživatelské přihlašovací údaje, takže během převzetí služeb při selhání není k dispozici žádné přerušení kvůli toomismatches s přihlášení a hesla. Uveďte hello [Azure Active Directory](../active-directory/active-directory-whatis.md), zákazníci mohou spravovat uživatele přístup tooboth primární a sekundární databáze a odstraňuje hello potřebné pro zcela Správa přihlašovací údaje do databáze.

## <a name="auto-failover-group-capabilities"></a>Možnosti skupiny-automatické převzetí služeb při selhání

Funkce skupiny-automatické převzetí služeb při selhání poskytuje výkonné abstrakce aktivní geografickou replikací podporou úrovně replikační skupiny a automatické převzetí služeb při selhání. Kromě toho se odeberou hello nezbytné toochange hello SQL připojovací řetězec po převzetí služeb při selhání tím, že poskytuje další naslouchací proces hello koncových bodů. 

* **Převzetí služeb při selhání skupiny**: jednu nebo více skupin převzetí služeb při selhání můžete vytvořit mezi dvěma servery v různých oblastech (primární a sekundární servery). Každá skupina může obsahovat jednu nebo několik databází, které se obnoví jako jednotku, v případě, že všechny nebo některé primární databáze k dispozici z důvodu tooan výpadku v primární oblasti hello.  
* **Primární server**: server, který je hostitelem hello primární databází ve skupině hello převzetí služeb při selhání.
* **Sekundární server**: server, který je hostitelem hello sekundární databází ve skupině hello převzetí služeb při selhání. sekundární server Hello nemůže být v hello stejné oblasti jako primární server hello.
* **Přidání skupiny databází toofailover**: můžete umístit několik databází v rámci serveru nebo v rámci elastického fondu do hello stejné skupiny převzetí služeb při selhání. Pokud chcete přidat skupinu samostatné databáze toohello, automaticky vytvoří sekundární databáze pomocí hello stejnou edici a úroveň výkonu. Pokud hello primární databází v elastickém fondu se hello sekundární je automaticky vytvořen ve hello elastický fond s hello stejný název. Pokud přidáte databázi, která už má sekundární databáze v sekundární server hello, geografická replikace zdědí hello skupiny.

   > [!NOTE]
   > Při přidávání databázi, která už má sekundární databáze na serveru, který není součástí skupiny hello převzetí služeb při selhání, se vytvoří nový sekundární v sekundární server hello. 
   >

* **Naslouchací proces pro čtení a zápis převzetí služeb při selhání skupiny**: záznam DNS CNAME je vytvořen jako  **&lt;název skupiny pro převzetí služeb při selhání&gt;. database.windows.net** který odkazuje toohello aktuální adresa URL primární server. Umožňuje hello aplikace SQL pro čtení a zápis tootransparently připojení toohello primární databázi, jakmile hello primární změny po převzetí služeb při selhání. 
* **Převzetí služeb při selhání jen pro čtení naslouchací proces skupiny**: záznam DNS CNAME je vytvořen jako  **&lt;název skupiny pro převzetí služeb při selhání&gt;. secondary.database.windows.net** odkazující na adresu URL toohello sekundární server. Umožňuje hello jen pro čtení aplikace SQL připojit tootransparently toohello sekundární databáze pomocí hello zadaná pravidla Vyrovnávání zatížení. Volitelně můžete zadat, pokud chcete hello jen pro čtení provoz toobe automaticky znovu směrován toohello primární server po hello sekundární server není k dispozici.
* **Zásady automatické převzetí služeb při selhání**: ve výchozím nastavení je hello převzetí služeb při selhání skupiny nakonfigurovaná pomocí zásad automatické převzetí služeb při selhání. Jakmile bude zjištěno selhání hello se aktivuje Hello systému převzetí služeb při selhání. Pokud chcete pracovní postup převzetí služeb při selhání hello toocontrol z hello aplikace, můžete vypnout automatické převzetí služeb při selhání. 
* **Ruční převzetí služeb při selhání**: převzetí služeb při selhání můžete spustit ručně kdykoli bez ohledu na automatické převzetí služeb při selhání konfigurace hello. Pokud zásady automatické převzetí služeb při selhání není nakonfigurované ruční převzetí služeb při selhání je požadovaná toorecover databází ve skupině hello převzetí služeb při selhání. Můžete zahájit vynucené nebo popisný převzetí služeb při selhání (úplná synchronizace dat). Hello pozdější může být použité toorelocate hello aktivní server toohello primární oblasti. Po dokončení převzetí služeb při selhání hello záznamy DNS jsou automaticky aktualizovány tooensure připojení toohello správný server.
* **Období odkladu ztráty dat.**: protože hello primárních a sekundárních databází se synchronizují pomocí asynchronní replikaci hello převzetí služeb při selhání může dojít ke ztrátě dat. Tooreflect zásady hello automatické převzetí služeb při selhání můžete přizpůsobit ztrátě toodata tolerance vaší aplikace. Nakonfigurováním **GracePeriodWithDataLossHours**, můžete řídit, jak dlouho hello systému čeká před zahájením hello převzetí služeb při selhání, je pravděpodobně tooresult ztráty dat. 

   > [!NOTE]
   > Když systém zjistí, že hello databází ve skupině hello je stále online (například výpadek hello pouze ovlivněn rovině řízení služby hello), ihned aktivuje hello převzetí služeb při selhání se úplná synchronizace dat (popisný převzetí služeb při selhání) bez ohledu na hodnotu hello nastavuje **GracePeriodWithDataLossHours**. To zaručuje, že nedochází ke ztrátě dat během obnovení hello. období odkladu Hello se projeví pouze v případě, že popisný převzetí služeb při selhání není možné. Pokud hello výpadku zmírnit před vypršením platnosti hello období odkladu, hello převzetí služeb při selhání není aktivována.
   >

* **Více skupin převzetí služeb při selhání**: můžete nakonfigurovat více skupin převzetí služeb při selhání pro hello stejného páru servery toocontrol hello škálování převzetí služeb při selhání. Každá skupina převezme nezávisle. Pokud vaše aplikace víceklientské používá elastické fondy, můžete v každém fondu databáze této schopnosti toomix primární a sekundární. Tímto způsobem můžete snížit dopad hello výpadku tooonly polovinu hello klientům.

## <a name="best-practices-of-building-highly-available-service"></a>Osvědčené postupy vytváření službu s vysokou dostupností

toobuild vysokou dostupností službu, která používá Azure SQL database hello zákazníků postupujte podle těchto pokynů:
- **Použití převzetí služeb při selhání skupiny**: jednu nebo více skupin převzetí služeb při selhání můžete vytvořit mezi dvěma servery v různých oblastech (primární a sekundární servery). Každá skupina může obsahovat jednu nebo několik databází, které se obnoví jako jednotku, v případě, že všechny nebo některé primární databáze k dispozici z důvodu tooan výpadku v primární oblasti hello. převzetí služeb při selhání skupiny Hello vytvoří geograficky sekundární databáze s hello stejné služby cíl, stejně jako hello primární. Pokud přidáte s hello stejné služby cíle na úrovni jako je nakonfigurována existující geografická replikace vztah toohello převzetí služeb při selhání skupiny Ujistěte se, hello geograficky – sekundární hello primární.
- **Použijte naslouchací proces pro čtení a zápis pro pracovní vytížení OLTP**: při provádění operace použití OLTP  **&lt;název skupiny pro převzetí služeb při selhání&gt;. database.windows.net** jak bude adresa URL serveru hello a hello připojení automaticky směrovanou toohello primární. Tato adresa URL nedojde ke změně po převzetí služeb při selhání hello.  
Poznámka: hello převzetí služeb při selhání budete muset aktualizovat hello DNS záznam tak hello připojení klienta bude přesměrované toohello nové primární až po hello klientské mezipaměti DNS se aktualizují.
- **Použijte jen pro čtení naslouchací proces pro pracovní vytížení jen pro čtení**: Pokud máte logicky izolované jen pro čtení zatížení, která je typu prošlostí odolný vůči chybám toocertain dat, můžete použít hello sekundární databáze v aplikaci hello. Použijte jen pro čtení relací  **&lt;název skupiny pro převzetí služeb při selhání&gt;. secondary.database.windows.net** jako adresa URL serveru hello a hello připojení se automaticky směrovanou toohello sekundární. Dále je doporučeno, abyste určili v připojovacím řetězci číst záměr pomocí **ApplicationIntent = jen pro čtení**. 
- **Připravit pro snížení výkonu**: je nezávislé na zbytek hello aplikace hello nebo dalších služeb používaných SQL rozhodovací převzetí služeb při selhání. Hello aplikace může být "smíšený" s některé součásti v jedné oblasti a některé v jiném. snížení hello tooavoid, ujistěte se, nasazení redundantního aplikace hello v oblasti hello zotavení po Havárii a postupujte podle pokynů hello v tomto článku.  
Poznámka: aplikace hello v oblasti zotavení po Havárii hello nemá toouse různých připojovací řetězec.  
- **Připravit na ztrátu dat**: Pokud je zjištěn výpadek, SQL automaticky aktivuje převzetí služeb při selhání pro čtení a zápis, pokud je nejlepší naše znalostí nulové ztráty toohello data. Jinak hodnota čekací dobu hello jste zadali ve **GracePeriodWithDataLossHours**. Pokud jste zadali **GracePeriodWithDataLossHours**, připravit ztráty dat. Obecně platí během výpadky, Azure upřednostňuje dostupnost. Pokud nechcete ztrátě dat, ujistěte se, že tooset **GracePeriodWithDataLossHours** dostatečně velký počet tooa, jako je například 24 hodin. 


## <a name="upgrading-or-downgrading-a-primary-database"></a>Upgrade nebo přechod na starší verzi primární databáze
Můžete upgradovat nebo starší verzi úroveň výkonu různých tooa primární databáze (v rámci hello stejné úrovně služby) bez odpojení žádné sekundární databáze. Při upgradu, doporučujeme nejprve upgradovat hello sekundární databáze a pak upgradovat primární hello. Když přechod na starší verzi, změnit pořadí hello: nejdřív starší verzi hello primární a poté downgradovat sekundární hello. Když upgradujete nebo starší verzi hello databáze tooa jinou službu vrstvy se vynucuje tohoto doporučení. 

> [!NOTE]
> Pokud jste vytvořili sekundární databáze v rámci konfigurace skupiny hello převzetí služeb při selhání není doporučené toodowngrade hello sekundární databáze. Toto je tooensure datové vrstvy má dostatečnou kapacitu tooprocess běžné zatížení po aktivaci převzetí služeb při selhání. 
>

## <a name="preventing-hello-loss-of-critical-data"></a>Brání hello ke ztrátě důležitých dat
Z důvodu toohello vysoké latenci sítě WAN průběžná kopie používá mechanismus asynchronní replikaci. Asynchronní replikaci lze ztrátu dat nevyhnutelné když dojde k chybě. Nedošlo ke ztrátě dat však mohou vyžadovat některé aplikace. tooprotect tyto důležité aktualizace, vývojář aplikací můžete volat hello [uložená procedura sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) postup systému ihned po potvrzení transakce hello. Volání metody **uložená procedura sp_wait_for_database_copy_sync** bloky hello volání přístup z více vláken, dokud hello poslední potvrzené transakce byla přenášených toohello sekundární databáze. Však není čekat na hello přenášených transakce toobe přehrány a potvrzeno u sekundární hello. **uložená procedura sp_wait_for_database_copy_sync** je odkaz oboru tooa konkrétní průběžná kopie. Tento postup můžete volat každý uživatel s hello připojení práva toohello primární databáze.

> [!NOTE]
> **uložená procedura sp_wait_for_database_copy_sync** zabrání ztrátě dat. po převzetí služeb při selhání, ale jeho nebude zaručit úplnou synchronizaci pro přístup pro čtení. Hello prodlevy způsobené **uložená procedura sp_wait_for_database_copy_sync** volání procedur může být významný a závisí na velikosti hello hello transakčního protokolu v době hello hello volání. 
> 

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Prostřednictvím kódu programu Správa skupin převzetí služeb při selhání a aktivní geografickou replikaci
Jak je popsáno dříve, skupiny automatické převzetí služeb při selhání (v preview) a aktivní geografická replikace můžete také spravovat programově pomocí prostředí Azure PowerShell a hello REST API. Hello následující tabulky popisují hello sadu příkazů, které jsou k dispozici.

**Rozhraní API Azure Resource Manager a na základě rolí zabezpečení**: aktivní geografickou replikací zahrnuje sadu rozhraní API Správce Azure Resource Manager pro správu, včetně hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) a [Azure Rutiny prostředí PowerShell](https://docs.microsoft.com/powershell/azure/overview). Tato rozhraní API vyžadují hello použití skupin prostředků a podporují zabezpečení na základě role (RBAC). Další informace o přístupu tooimplement rolí najdete v tématu [řízení přístupu](../active-directory/role-based-access-control-what-is.md).

> [!NOTE]
> Mnoho nových funkcí aktivní geografickou replikaci podporují jenom pomocí [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) na základě [REST API služby Azure SQL](https://msdn.microsoft.com/library/azure/mt163571.aspx) a [rutiny prostředí PowerShell databáze SQL Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx). Hello [(klasické) rozhraní API REST](https://msdn.microsoft.com/library/azure/dn505719.aspx) a [rutiny Azure SQL Database (klasické)](https://msdn.microsoft.com/library/azure/dn546723.aspx) jsou podporovány pro zpětnou kompatibilitu, takže pomocí rozhraní API založené na Azure Resource Manager doporučují hello. 
> 

## <a name="manage-sql-database-failover-using-using-transact-sql"></a>Spravovat SQL databáze převzetí služeb při selhání pomocí jazyka Transact-SQL

| Příkaz | Popis |
| --- | --- |
| [Příkaz ALTER DATABASE (databáze Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx) |Použít přidat sekundární SERVER ON argument toocreate sekundární databáze pro databázi a spustí replikaci dat |
| [Příkaz ALTER DATABASE (databáze Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx) |Použít převzetí služeb při selhání nebo FORCE_FAILOVER_ALLOW_DATA_LOSS tooswitch sekundární databáze toobe primární tooinitiate převzetí služeb při selhání |
| [Příkaz ALTER DATABASE (databáze Azure SQL)](https://msdn.microsoft.com/library/mt574871.aspx) |Použití odebrat sekundární SERVER ON tooterminate replikaci dat mezi SQL Database a hello zadat sekundární databáze. |
| [Sys.geo_replication_links (Azure SQL Database)](https://msdn.microsoft.com/library/mt575501.aspx) |Vrací informace o všechna existující replikační připojení pro každou databázi na hello logického serveru Azure SQL Database. |
| [Sys.dm_geo_replication_link_status (Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx) |Získá text hello, čas poslední replikace, poslední replikace funkce lag a další informace o hello propojení replikace pro danou databázi SQL. |
| [Sys.dm_operation_status (Azure SQL Database)](https://msdn.microsoft.com/library/dn270022.aspx) |Zobrazuje stav hello pro všechny databázové operace, včetně hello stav odkazů replikace hello. |
| [uložená procedura sp_wait_for_database_copy_sync (Azure SQL Database)](https://msdn.microsoft.com/library/dn467644.aspx) |dokud všechny potvrzené transakce jsou replikována a potvrzena hello aktivní sekundární databáze způsobí, že toowait aplikace hello. |
|  | |

## <a name="manage-sql-database-failover-using-using-powershell"></a>Spravovat SQL databáze převzetí služeb při selhání pomocí pomocí prostředí PowerShell

| Rutina | Popis |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Získá jednu nebo více databází. |
| [Nové AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Sekundární databáze pro databázi vytvoří a spustí replikaci dat. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |Přepne sekundární databáze toobe primární tooinitiate převzetí služeb při selhání. |
| [Odebrat AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Ukončí replikaci dat mezi SQL Database a hello zadaná sekundární databáze. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Získá hello geografická replikace propojení mezi Azure SQL Database a skupinu prostředků nebo SQL Server. |
| [Nové AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Tento příkaz vytvoří skupinu převzetí služeb při selhání a zaregistruje ho na primární a sekundární servery|
| [Odebrat AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Odebere skupinu hello převzetí služeb při selhání ze serveru hello a odstraní všechny sekundární databáze, které jsou zahrnuté v hello skupinu |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Načte konfiguraci skupiny hello převzetí služeb při selhání |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Upraví konfiguraci hello hello převzetí služeb při selhání skupiny |
| [Přepínač AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Převzetí služeb při selhání aktivační události hello převzetí služeb při selhání skupiny toohello sekundární server |
|  | |

> [!IMPORTANT]
> Ukázkové skripty, najdete v části [konfigurace a převzetí služeb při selhání jedné databáze používá aktivní geografickou replikaci](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [konfigurace a převzetí služeb při selhání ve fondu databáze používá aktivní geografickou replikaci](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), a [Konfigurace a převzetí služeb při selhání převzetí služeb při selhání skupiny pro jednu databázi (preview)] (skripty nebo sql-database-setup-geodr-failover-database-failover-skupin-powershell.md.
>

## <a name="manage-sql-database-failover-using-hello-rest-api"></a>Správa SQL databáze převzetí služeb při selhání pomocí hello REST API
| Rozhraní API | Popis |
| --- | --- |
| [Vytvoření nebo aktualizace databáze (createMode = obnovit)](/rest/api/sql/Databases/CreateOrUpdate) |Vytvoří, aktualizuje nebo obnoví primární nebo sekundární databáze. |
| [Získat vytvořit nebo aktualizovat stav databáze](/rest/api/sql/Databases/CreateOrUpdate) |Vrátí stav hello během operace vytvoření. |
| [Nastavit sekundární databáze jako primární (plánované převzetí služeb při selhání)](https://docs.microsoft.com/rest/api/sql/replicationlinkss#ReplicationLinks_Failover) |Nastaví které repliky databáze je primární podle převzetí služeb při selhání z hello aktuální primární repliku databáze. |
| [Nastavit sekundární databáze jako primární (neplánované převzetí služeb při selhání)](https://docs.microsoft.com/rest/api/sql/replicationlinks#ReplicationLinks_FailoverAllowDataLoss) |Nastaví které repliky databáze je primární podle převzetí služeb při selhání z hello aktuální primární repliku databáze. Tato operace může způsobit ztrátu dat. |
| [Získání odkazu replikace](/rest/api/sql/replicationlinks/get) |Získá konkrétní replikace odkaz pro danou databázi SQL ve spolupráci se geografická replikace. Načítá hello informace zobrazené v zobrazení katalogu sys.geo_replication_links hello. |
| [Propojení replikace - seznamu databáze](/rest/api/sql/replicationlinks/listbydatabase) | Získá všechny odkazy replikace pro danou databázi SQL ve spolupráci se geografická replikace. Načítá hello informace zobrazené v zobrazení katalogu sys.geo_replication_links hello. |
| [Odstranit propojení replikace](/rest/api/sql/databases/delete) | Odstraní propojení replikace databáze. Nelze provést během převzetí služeb při selhání. |
| [Vytvořit nebo aktualizovat skupinu převzetí služeb při selhání] / rest/api/sql nebo failovergroups/createorupdate) | Vytvoří nebo aktualizuje skupinu převzetí služeb při selhání |
| [Odstranit skupinu převzetí služeb při selhání](/rest/api/sql/failovergroups/delete) | Odebere skupinu hello převzetí služeb při selhání ze serveru hello |
| [Převzetí služeb při selhání (plánovaná)](/rest/api/sql/failovergroups/failover) | Selhání z hello aktuální primární server toothis server. |
| [Vynucené převzetí služeb při selhání povolit ztrátě dat.](/rest/api/sql/failovergroups/forcefailoverallowdataloss) |ails přes z hello aktuální primární server toothis server. Tato operace může způsobit ztrátu dat. |
| [Get převzetí služeb při selhání skupiny](/rest/api/sql/failovergroups/get) | Načte skupinu převzetí služeb při selhání. |
| [Seznam skupin převzetí služeb při selhání serverem](/rest/api/sql/failovergroups/listbyserver) | Seznam skupin hello převzetí služeb při selhání na serveru. |
| [Převzetí služeb při selhání skupiny aktualizací](/rest/api/sql/failovergroups/update) | Aktualizuje skupinu převzetí služeb při selhání. |
|  | |

## <a name="next-steps"></a>Další kroky
* Ukázkové skripty najdete v části:
   - [Konfigurace a převzetí služeb při selhání jedné databáze používá aktivní geografickou replikaci](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
   - [Konfigurace a převzetí služeb při selhání ve fondu databáze používá aktivní geografickou replikaci](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
   - [Konfigurace a převzetí služeb při selhání a převzetí služeb při selhání skupiny pro jednu databázi (preview)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md)
* toolearn o zálohování Azure SQL Database automatizované, najdete v části [automatizované zálohování SQL Database](sql-database-automated-backups.md).
* toolearn o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba hello](sql-database-recovery-using-backups.md).
* toolearn o požadavky na ověřování pro nový primární server a databázi, najdete v části [zabezpečení SQL Database po zotavení po havárii](sql-database-geo-replication-security-config.md).

