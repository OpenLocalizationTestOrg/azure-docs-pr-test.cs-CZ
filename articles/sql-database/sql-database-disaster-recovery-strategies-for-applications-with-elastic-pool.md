---
title: "řešení zotavení po havárii aaaDesign – Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak toodesign cloudové řešení pro zotavení po havárii výběrem vzor hello správné převzetí služeb při selhání."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2db99057-0c79-4fb0-a7f1-d1c057ec787f
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: sashan;carlrab
ms.openlocfilehash: 9d9eca7570c7a01c992d0b33d449721709b471c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>Strategie zotavení po havárii pro aplikace pomocí fondů elastické databáze SQL
V průběhu let hello jsme se dozvěděli, cloudové služby nejsou spolehlivá a dojít závažné incidenty. Databáze SQL poskytuje několik možností tooprovide pro kontinuitu podnikových procesů hello vaší aplikace, když dojde k tyto incidenty. [Elastické fondy](sql-database-elastic-pool.md) a jedné databáze podporují hello stejný druh možnosti obnovení po havárii. Tento článek popisuje několik strategie zotavení po Havárii pro elastických fondů, které využívají tyto funkce kontinuity obchodních databáze SQL.

Tento článek používá následující kanonický vzor aplikací SaaS ISV hello:

<i>Moderní cloudové webovou aplikaci zřídí jedna databáze SQL pro každý koncový uživatel. Hello ISV má mnoho zákazníků a proto používá mnoho databází, označuje jako databáze klienta. Protože hello klienta databáze obvykle mají vzorce nepředvídatelné aktivity, používá hello ISV elastický fond toomake hello databáze nákladů na velmi předvídatelný přes dlouhou dobu. Hello elastického fondu také usnadňuje správu výkonu hello, pokud aktivity uživatelů hello špičky. Kromě toho toohello klienta databází hello aplikace také používá několik databází toomanage uživatelské profily, zabezpečení, shromažďování vzorce používání atd. Dostupnost hello jednotlivých klientů, které nemá negativní vliv na dostupnost hello aplikace jako celek. Ale hello dostupnost a výkon databáze správy je velmi důležitá pro funkce aplikace hello a pokud jsou databáze správy hello offline hello celá aplikace je offline.</i>  

Tento článek popisuje zahrnující celou řadu scénářů z náklady citlivé spuštění aplikace tooones požadavkům přísné dostupnosti strategie zotavení po Havárii.

## <a name="scenario-1-cost-sensitive-startup"></a>Scénář 1. Náklady citlivé spuštění
<i>Jsem obchodní spuštění a mě náklady velmi citlivé.  Chci, aby toosimplify nasazení a Správa aplikace hello a mám omezené SLA pro jednotlivé zákazníky. Ale být tooensure hello aplikace jako celek se nikdy offline.</i>

toosatisfy hello jednoduchost požadavek, nasaďte všechny databáze klienta do jednoho elastického fondu v hello oblast Azure podle svého výběru a nasaďte správu databází jako geograficky replikované izolované databáze. Pro zotavení po havárii hello klientů použijte geografické obnovení, který se dodává bez dalších poplatků. dostupnost hello tooensure hello správu databází, geograficky replikovat, je oblast tooanother pomocí skupinu automatické převzetí služeb při selhání (v preview) (krok 1). probíhající náklady Hello hello konfiguraci obnovení po havárii v tomto scénáři je rovna toohello celkové náklady na sekundární databází hello. Tato konfigurace je znázorněna v diagramu další hello.

![Obrázek 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Pokud dojde k výpadku v hello primární oblasti, toobring kroky obnovení hello aplikace online jsou zobrazené ve další diagram hello.

* převzetí služeb při selhání skupiny Hello zahájí automatické převzetí služeb při selhání hello správy databáze toohello zotavení po Havárii oblasti. aplikace Hello je automaticky opětovného připojení toohello nové primární a všechny nové účty a vytvoří se v oblasti hello zotavení po Havárii databází klienta. stávající zákazníky služby Hello zobrazit tato data není dočasně k dispozici.
* Vytvoření fondu elastické hello pomocí hello stejnou konfiguraci jako původní fond hello (2).
* Použijte geografické obnovení toocreate kopie databáze klienta hello (3). Můžete zvážit spouštění jednotlivých obnovení hello připojeními hello koncového uživatele nebo používat některé další schéma s prioritou specifické pro aplikaci.


V tomto okamžiku aplikace je zpět do online režimu v oblasti hello zotavení po Havárii, ale někteří zákazníci zaznamenat zpoždění při přístupu k jejich datům.

![Obrázek 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Pokud hello výpadek byl dočasné, je možné, že primární oblasti hello budou obnovena Azure předtím, než všechny databáze obnovení hello jsou dokončeny v oblasti hello zotavení po Havárii. V takovém případě orchestraci přesunutí hello aplikace back toohello primární oblasti. proces Hello trvá hello kroky zobrazené v diagramu další hello.

* Zrušte všechny zbývající geografické obnovení žádosti.   
* Selhání hello správu databází toohello primární oblasti (5). Po obnovení hello oblast mají základní hello staré barvy automaticky stane sekundární repliky. Nyní se přepínat role znovu. 
* Změňte aplikace hello připojovací řetězec toopoint back toohello primární oblasti. Nyní všechny nové účty a databáze klienta se vytvoří v primární oblasti hello. Některé stávající zákazníky služby zobrazit tato data není dočasně k dispozici.   
* Nastavte všechny databáze v hello zotavení po Havárii fondu jen tooread tooensure, který nemůže být upravena v oblasti hello zotavení po Havárii (6). 
* Pro každou databázi v hello zotavení po Havárii fondu, který se změnil od obnovení hello přejmenovat nebo odstranit hello příslušné databáze ve fondu primární hello (7). 
* Kopírování hello aktualizovat databáze z hello zotavení po Havárii fondu toohello primární fondu (8). 
* Odstranění fondu hello zotavení po Havárii (9)

Aplikace je nyní online v hello primární oblasti s všechny databáze klienta k dispozici ve fondu primární hello.

![Obrázek 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

klíč Hello **těžit** tuto strategii je nízké náklady probíhající pro redundanci dat vrstvy. Zálohy jsou automaticky provedenou hello služby SQL Database s bez přepsání aplikace a bez dalších poplatků.  Hello náklady nabíhají jenom v případě, že jsou obnoveny hello elastické databáze. Hello **kompromis** je, že hello dokončete obnovení všech databází klienta trvá déle. Hello doba závisí na hello celkový počet obnovení spustíte v hello zotavení po Havárii oblast a celkové velikosti hello klienta databází. I když některé klienty obnovení určit prioritu před jinými, vzájemně neslučitelných s hello všechny ostatní obnovení, které jsou zahájeny v hello stejné oblasti jako služba hello řeší a omezí generovaný toominimize hello celkového vlivu na hello zákazníků existující databáze. Kromě toho hello obnovení databází hello klienta nelze spustit, dokud se vytvoří nový elastický fond hello v oblasti hello zotavení po Havárii.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scénář 2. Vyspělá aplikace vrstvené službou
<i>Jsem vyspělá aplikace SaaS s nabídek vrstvené služby a jiné smlouvy SLA pro zkušební zákazníků a platícího zákazníků. Pro zákazníky hello zkušební verze je nutné tooreduce hello náklady co nejvíc. Zkušební verze zákazníkům může trvat výpadek ale být tooreduce jeho pravděpodobnost. Žádné výpadky hello platícího zákazníků, je riziko letu. Takže chci, aby se, že platící zákazníky jsou vždy možné tooaccess toomake svá data.</i> 

Tento scénář samostatnou hello zkušební klienty z toosupport placené klienty jejich umístěním do samostatné elastické fondy. zkušební verze zákazníci Hello mají nižší eDTU na klienta a nižší smlouvy SLA s delší dobu obnovení. Hello platící zákazníky jsou ve fondu s vyšší eDTU na klienta a vyšší SLA. tooguarantee hello nejnižší čas obnovení, jsou databáze klienta hello platící zákazníky využívající geograficky replikované. Tato konfigurace je znázorněna v diagramu další hello. 

![Obrázek 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Jako hello prvního scénáře jsou velmi aktivní hello správy databáze, takže použití jedné geograficky replikované databáze pro něj (1). Tím se zajistí hello předvídatelný výkon pro nové předplatné zákazníka, profil aktualizace a jiné operace správy. Hello oblast, ve kterém jsou umístěny základní barvy hello hello správu databází je hello primární oblasti a hello oblast, ve kterém jsou umístěny hello sekundárních databází správu hello je oblast hello zotavení po Havárii.

Hello databáze klienta platící zákazníky mít aktivní databáze v hello "placené" fondu zřízené v primární oblasti hello. Zřídit sekundární fond s hello stejný název v oblasti hello zotavení po Havárii. Každý klient je toohello geograficky replikované sekundární fondu (2). To umožňuje rychlé obnovení všech databází klienta převzetí služeb při selhání. 

Pokud dojde k výpadku v hello primární oblasti, toobring kroky obnovení hello aplikace online jsou zobrazené v diagramu další hello:

![Obrázek 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Okamžitě převzetí služeb při selhání hello správu databází toohello zotavení po Havárii oblast (3).
* Změňte aplikace hello připojovací řetězec toopoint toohello zotavení po Havárii oblast. Nyní všechny nové účty a databáze klienta se vytvoří v oblasti hello zotavení po Havárii. Hello stávající zákazníky služby zkušební zobrazit tato data není dočasně k dispozici.
* Převzetí služeb při selhání hello placené klienta databází toohello fondu v hello zotavení po Havárii oblast tooimmediately obnovit jejich dostupnost (4). Vzhledem k tomu, že hello převzetí služeb při selhání je rychlý Změna úrovně, zvažte optimalizaci kde jednotlivé převzetí služeb při selhání hello jsou aktivovány na vyžádání připojení hello koncového uživatele. 
* Pokud velikost vašeho sekundární fondu eDTU byl nižší než hello primární, protože sekundární databáze hello pouze požadované hello kapacity tooprocess hello změnu protokolů při jejich byly sekundární repliky, okamžitě zvýšení kapacity fondu hello teď tooaccommodate hello úplné úlohy, které obsahují všechny klienty (5). 
* Vytvořit nový elastický fond hello s hello stejný název a text hello stejné konfigurace v oblasti hello zotavení po Havárii pro databáze hello zkušební zákazníků (6). 
* Jakmile je vytvořen fond hello zákazníků zkušební verzi, použijte geografické obnovení toorestore hello jednotlivé zkušební verzi klienta databáze do nového fondu hello (7). Zvažte aktivován hello jednotlivých obnovení koncového uživatele připojeními hello nebo používat některé další schéma s prioritou specifické pro aplikaci.

Aplikace je v tomto okamžiku zpět do online režimu v oblasti hello zotavení po Havárii. Mají přístup všechny platící zákazníky tootheir data během zkušební zákazníků hello zaznamenat zpoždění při přístupu k jejich datům.

Pokud budou obnovena hello primární oblasti Azure *po* jste obnovili aplikace hello v oblasti hello zotavení po Havárii můžete pokračovat v používání aplikace hello v této oblasti nebo si můžete toofail back toohello primární oblasti. Pokud je obnovit primární oblast hello *před* dokončení procesu hello převzetí služeb při selhání, zvažte selhání zpět hned. navrácení služeb po obnovení Hello učiní hello kroky, které jsou zobrazené v diagramu další hello: 

![Obrázek 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Zrušte všechny zbývající geografické obnovení žádosti.   
* Selhání hello správu databází (8). Po obnovení hello oblast hello původního primárního automaticky stane hello sekundární. Nyní bude text hello primární znovu.  
* Selhání hello placené databáze klienta (9). Po obnovení hello oblast je podobně původní základní barvy hello automaticky stane hello sekundární repliky. Nyní budou základní hello barvy znovu. 
* Sada hello obnovit zkušební verzi databáze, které se změnily v oblasti zotavení po Havárii hello jen tooread (10).
* Pro každou databázi v hello zkušební zákazníků zotavení po Havárii fondu, který změnil od obnovení hello přejmenovat nebo odstranit hello příslušné databáze ve hello zkušební zákazníkům primární fondu (11). 
* Kopírování hello aktualizovat databáze z hello zotavení po Havárii fondu toohello primární fondu (12). 
* Odstranění fondu hello zotavení po Havárii (13) 

> [!NOTE]
> operace převzetí služeb při selhání Hello je asynchronní. toominimize hello obnovení čas, kdy je důležité, možné provést příkaz převzetí služeb při selhání hello klienta databáze, a to v dávkách aspoň 20 databází. 
> 
> 

klíč Hello **těžit** této strategie je, že zajišťuje hello nejvyšší SLA pro hello platícího zákazníků. Také zaručuje, že hello nové zkušební verze jsou odblokuje, jakmile je vytvořen fond zotavení po Havárii zkušební hello. Hello **kompromis** je, že tato instalace zvyšuje hello celkové náklady na hello klienta databází hello náklady hello sekundární fond zotavení po Havárii pro placené zákazníků. Kromě toho pokud hello sekundární fondu má různou velikost, prostředí platící zákazníky hello nižší výkon po převzetí služeb při selhání, dokud hello fondu v oblasti hello zotavení po Havárii dokončení upgradu. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scénář 3. Geograficky distribuovaná aplikace s vrstvami služby
<i>Je nutné vyspělá aplikace SaaS s nabídkami vrstvené služby. I má toooffer velmi agresivní toomy SLA placené zákazníků a minimalizovat riziko hello dopad, když dojde k výpadku, protože i stručný přerušení může způsobit nespokojenosti zákazníka. Je důležité, že hello platícího zákazníci mohou mít přístup svá data. zkušební verze Hello jsou zdarma a SLA není nabídnuta během zkušební doby hello.</i> 

toosupport tento scénář, použijte tři samostatné elastické fondy. Zřízení dvě stejná velikost fondy s vysokou Edtu na databázi ve dvou různých oblastech toocontain hello placené databáze klienta zákazníků. Hello třetí fond, který obsahuje hello zkušební klienty může mít nižší počet jednotek Edtu na databázi a zřídit v jednom ze dvou oblastí hello.

tooguarantee hello nejnižší obnovení době výpadky, jsou databáze klienta hello platící zákazníky využívající geograficky replikované s 50 % hello primární databází v každé z hello dvou oblastech. Podobně každou oblast má 50 % hello sekundární databáze. Tímto způsobem, pokud oblast je v režimu offline, jen 50 % hello placené zákazníků databáze dopad a mají toofail. Hello jiných databází zůstanou beze změn. Tato konfigurace je znázorněno v následujícím diagramu hello:

![Obrázek 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Jako v předchozích případech hello, jsou velmi aktivní hello správu databází, je nakonfigurujte jako izolované geograficky replikované databáze (1). Tím se zajistí hello předvídatelný výkon hello nová předplatná zákazníka, profil aktualizace a jiné operace správy. Oblasti A je hello primární oblasti pro správu databáze hello a oblast hello B se používá pro obnovení hello správu databází.

databáze Hello platící zákazníky využívající klienta jsou taky geograficky replikované, ale s základní barvy a sekundárních rozdělit mezi oblast A a B (2) oblast. Tímto způsobem hello klienta primární databází vliv výpadku hello přebírány toohello jiné oblasti a bude k dispozici. Hello jiných polovinu hello klienta databází nejsou vůbec dopad. 

Hello další diagram znázorňuje tootake kroky obnovení hello, pokud dojde k výpadku v oblasti A.

![Obrázek 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Okamžitě převzetí služeb při selhání hello správu databází tooregion B (3).
* Změnit aplikace hello připojení řetězec toopoint toohello správu databází v oblasti upravit B. hello správu databází toomake hello nové účty a databáze klienta se vytvoří v oblasti B a jsou nalezeny hello existující databáze klienta jako také. Hello stávající zákazníky služby zkušební zobrazit tato data není dočasně k dispozici.
* Převzetí služeb při selhání klienta hello placené databáze toopool 2 v oblasti B tooimmediately obnovit jejich dostupnost (4). Vzhledem k tomu, že hello převzetí služeb při selhání je rychlý Změna úrovně, zvažte optimalizaci kde jednotlivé převzetí služeb při selhání hello jsou aktivovány na vyžádání připojení hello koncového uživatele. 
* Od teď fond 2 obsahuje pouze primární databáze, hello celkový počet úloh ve fondu zvyšuje hello a můžete okamžitě zvětšete jeho velikost eDTU (5). 
* Vytvořit nový elastický fond hello s hello stejný název a text hello stejné konfigurace v oblasti hello B pro databáze hello zkušební zákazníků (6). 
* Po použití geografické obnovení toorestore hello jednotlivé zkušební verzi klienta databáze je vytvořen fond hello do fondu hello (7). Můžete zvážit spouštění jednotlivých obnovení hello připojeními hello koncového uživatele nebo používat některé další schéma s prioritou specifické pro aplikaci.

> [!NOTE]
> operace převzetí služeb při selhání Hello je asynchronní. Doba obnovení hello toominimize, je důležité, možné provést příkaz převzetí služeb při selhání hello klienta databáze, a to v dávkách aspoň 20 databází. 
> 

V tomto okamžiku aplikace je zpět do online režimu v oblasti B. Mají přístup všechny platící zákazníky tootheir data během zkušební zákazníků hello zaznamenat zpoždění při přístupu k jejich datům.

Když je obnovena oblasti A musíte toodecide Pokud chcete toouse oblast B pro zkušební zákazníků nebo navrácení služeb po obnovení toousing hello zkušební zákazníkům fond v oblasti A. Jedno kritérium může být % hello od obnovení hello upraven databází zkušební verzi klienta. Bez ohledu na to, že rozhodnutí potřebujete placené hello klientům toore vyrovnávání mezi dvěma fondy. Hello další obrázek znázorňuje proces hello, pokud databáze zkušební verzi klienta hello nezdaří back tooregion A.  

![Obrázek 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Zrušte všechny zbývající geografické obnovení žádosti o tootrial zotavení po Havárii fondu.   
* Selhání databáze správy hello (8). Po obnovení hello oblast hello původního primárního se stala automaticky hello sekundární. Nyní bude text hello primární znovu.  
* Vyberte, které databáze placené klienta nezdaří back toopool 1 a inicializovat převzetí služeb při selhání tootheir sekundárních (9). Po obnovení hello oblast jsou všechny databáze ve fondu 1 automaticky sekundární repliky. Nyní 50 % z nich stanou základní barvy znovu. 
* Snížení velikosti hello původní eDTU fondu 2 toohello (10).
* Všechny sady obnovíte zkušební verze databáze v oblasti hello B jen tooread (11).
* Pro každou databázi ve hello zkušební zotavení po Havárii fondu, který se změnil od obnovení hello přejmenovat nebo odstranit hello příslušné databáze ve hello zkušební primární fondu (12). 
* Kopírování hello aktualizovat databáze z hello zotavení po Havárii fondu toohello primární fondu (13). 
* Odstranění fondu hello zotavení po Havárii (14) 

klíč Hello **výhody** této strategie jsou:

* Podporuje hello nejvíce agresivní SLA pro hello platícího zákazníků, protože zajišťuje, že výpadek nemůže mít vliv na více než 50 % hello klienta databáze. 
* Zaručuje, že jsou nové zkušební verze hello odblokuje co nejrychleji hello záznam pro zotavení po Havárii fondu se vytvoří během obnovení hello. 
* Umožňuje efektivnější využití kapacity fondu hello jako 50 % sekundární databází ve fondu 1 a fond 2 zaručena toobe active méně než hello primární databáze.

Hello hlavní **kompromis** jsou:

* Hello operace CRUD s databázemi, správu hello mít nižší latence pro hello koncoví uživatelé připojený tooregion A než pro hello koncoví uživatelé připojený tooregion B, jako jsou spustit pro primární hello hello správu databází.
* To vyžaduje složitější návrhu hello správu databáze. Každý záznam klienta má například značky < location vyžadující toobe změnit během převzetí služeb při selhání a navrácení služeb po obnovení.  
* Hello platícího zákazníků může zaznamenat snížení výkonu než obvykle, dokud hello fondu v oblasti B dokončení upgradu. 

## <a name="summary"></a>Souhrn
Tento článek se zaměřuje na hello strategie obnovení po havárii pro databázové vrstvy hello používá víceklientské aplikace SaaS ISV. Hello strategie zvolíte je podle potřeb hello hello aplikace, jako je například hello firmy modelu hello SLA má toooffer tooyour zákazníků, rozpočet omezení atd. Každý popsané strategie jsou podrobněji popsány dále hello výhody a kompromis tak může provést informované rozhodnutí. Konkrétní aplikace pravděpodobně také dalšími součástmi Azure. Proto zkontrolujte své firmy kontinuity pokyny a orchestraci hello obnovení hello databázové vrstvy s nimi. toolearn Další informace o správě obnovení databáze aplikace v Azure, najdete v příliš[návrhu cloudové řešení pro zotavení po havárii](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Další kroky
* toolearn o zálohování Azure SQL Database automatizované, najdete v části [automatizované zálohování SQL Database](sql-database-automated-backups.md).
* Přehled kontinuity obchodních a scénářů najdete v tématu [obchodní kontinuity přehled](sql-database-business-continuity.md).
* toolearn o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba hello](sql-database-recovery-using-backups.md).
* toolearn o rychlejší možnosti obnovení, najdete v části [aktivní geografickou replikací](sql-database-geo-replication-overview.md).
* toolearn o použití automatizované zálohování pro archivaci, najdete v části [kopírování databáze](sql-database-copy.md).

