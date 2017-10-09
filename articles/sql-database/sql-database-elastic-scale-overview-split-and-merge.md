---
title: "aaaMoving data mezi databázemi upraveným cloudu | Microsoft Docs"
description: "Vysvětluje, jak toomanipulate horizontálních oddílů a přesunutí dat prostřednictvím automatických hostované služby pomocí Elastická databáze rozhraní API."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 204fd902-0397-4185-985a-dea3ed7c7d9f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 629dee06e22609e9b61edf93ba5c38d997132d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="moving-data-between-scaled-out-cloud-databases"></a>Přesun dat mezi cloudovými databázemi s horizontálním navýšením kapacity
Pokud jste Software jako služba developer a najednou aplikace zde nevyskytlo strávíte vyžádání, je třeba tooaccommodate hello růstu. Proto můžete přidat další databáze (horizontálních oddílů). Jak opětovné distribuci hello data toohello nové databáze a to bez přerušení hello integritu dat? Použití hello **nástroji pro sloučení rozdělení** toomove data z omezené databáze toohello nové databáze.  

Hello rozdělení sloučení nástroj spouští jako Azure webové služby. Správce nebo vývojáře používá hello nástroj toomove shardlets (data z horizontálního oddílu) mezi různých databází (horizontálních oddílů). Nástroj Hello používá metadata databáze služby toomaintain hello horizontálního oddílu mapy správy a zajistit konzistentní mapování.

![Přehled][1]

## <a name="download"></a>Ke stažení
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)

## <a name="documentation"></a>Dokumentace
1. [Kurz nástroje elastické databáze rozdělení sloučení](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
2. [Konfigurace zabezpečení rozdělení sloučení](sql-database-elastic-scale-split-merge-security-configuration.md)
3. [Aspekty zabezpečení rozdělení sloučení](sql-database-elastic-scale-split-merge-security-configuration.md)
4. [Správa mapování horizontálních oddílů](sql-database-elastic-scale-shard-map-management.md)
5. [Migrovat existující databáze tooscale na více systémů](sql-database-elastic-convert-to-use-elastic-tools.md)
6. [Nástroje elastické databáze](sql-database-elastic-scale-introduction.md)
7. [Glosář nástroje elastické databáze](sql-database-elastic-scale-glossary.md)

## <a name="why-use-hello-split-merge-tool"></a>Proč používat nástroj rozdělení sloučení hello?
**Flexibilita**

Aplikace potřebují toostretch flexibilně mimo hranice hello služby jedné databáze Azure SQL DB. Použijte hello nástroj toomove data jako potřebné toonew databáze při zachování integrity.

**Rozdělení toogrow** 

Je třeba tooincrease celkové kapacity toohandle výrazný růst. toodo tedy vytvořit další kapacitu, horizontálního dělení hello dat a rozděluje mezi přírůstkově více databází, dokud se splnění požadavků na kapacitu. Toto je typickým příkladem hello 'rozdělení' funkce. 

**Sloučení tooshrink**

Kapacita musí zmenšit z důvodu toohello sezónní povahy firmy. nástroj umožňuje Hello snižovat toofewer jednotek škálování, když obchodní zpomalí. funkce "sloučení" Hello v hello elastické škálování rozdělení sloučení služby obsahuje tento požadavek. 

**Spravovat hotspotům přesunutím shardlets**

S více klienty na databázi může hello přidělení shardlets tooshards vést toocapacity kritická místa na některé horizontálních oddílů. To vyžaduje znovu přidělování shardlets nebo přesunutí zaneprázdněn shardlets toonew nebo méně využívané horizontálních oddílů. 

## <a name="concepts--key-features"></a>Koncepty & klíčové funkce
**Zákazník hostované služby**

rozdělení korespondence Hello je dodávána jako zákazník hostované služby. Musíte nasadit a hostování hello služby v rámci vašeho předplatného Microsoft Azure. Hello balíček, který si stáhnout z NuGet obsahuje toocomplete šablony konfigurace s hello informace pro konkrétní nasazení. V tématu hello [rozdělení sloučení kurzu](sql-database-elastic-scale-configure-deploy-split-and-merge.md) podrobnosti. Vzhledem k tomu, že služba hello běží ve vašem předplatném Azure, můžete řídit a nakonfigurovat většinu aspektů zabezpečení služby hello. Hello výchozí šablony zahrnuje tooconfigure hello možností protokolu SSL, ověřování na základě certifikátu klienta, šifrování pro uložené přihlašovací údaje, DoS ochrana a omezení IP adres. Další informace naleznete na hello aspekty zabezpečení v následujícím dokumentu hello [konfigurace zabezpečení rozdělení sloučení](sql-database-elastic-scale-split-merge-security-configuration.md).

Výchozí Hello nasadit service běží s jeden pracovní proces a jednu webovou roli. Každá velikost virtuální počítač A1 hello používá ve službě Azure Cloud Services. Když tato nastavení nelze upravit, při nasazování balíčku hello, můžete je změnit po úspěšné nasazení ve hello spuštění cloudové služby (prostřednictvím hello portál Azure). Poznámka: Tato role pracovního procesu hello musí být nakonfigurované pro více než jednu instanci technických důvodů. 

**Mapa integrace horizontálního oddílu**

Hello rozdělení sloučení služba komunikuje s hello mapy horizontálního oddílu aplikace hello. Pokud používáte hello rozdělení sloučení služby toosplit nebo rozsahy sloučení nebo toomove shardlets mezi horizontálních oddílů, hello služby automaticky udržuje hello horizontálního oddílu mapy až toodate. toodo tedy hello služby připojí toohello horizontálního oddílu mapa správce databáze aplikace hello a udržuje rozsahy a mapování průběh rozdělení nebo sloučení nebo přesunutí požadavky. To zajišťuje, že hello horizontálního oddílu mapy vždy uvede aktuální zobrazení při operace sloučení rozdělení se děje. Rozdělení, sloučení a shardlet přesun operace jsou implementované přesunutím dávky shardlets z hello zdroj horizontálního oddílu toohello cíl horizontálních. Během hello shardlet přesun operaci hello shardlets subjektu toohello aktuální batch jsou označeny jako offline v mapě hello horizontálního oddílu a nejsou k dispozici pro závislé na data směrování připojení pomocí hello **OpenConnectionForKey** rozhraní API. 

**Konzistentní shardlet připojení**

Při přesunu dat pro novou dávku shardlets, jsou všechny zadané mapování horizontálních závislé na data směrování připojení toohello horizontálního oddílu ukládání hello shardlet borné a následné připojení z mapování horizontálních hello toohello rozhraní API jsou zablokované tyto shardlets při Přesun dat Hello probíhá v pořadí tooavoid nekonzistence. Připojení tooother shardlets na hello stejné ID horizontálního oddílu také získat ukončeny, ale bude úspěšné znovu okamžitě při opakování. Po přesunutí hello batch hello shardlets jsou označeny online znovu pro cíl hello horizontálního oddílu a hello zdrojová data se odebral ze horizontálních zdroj hello. Služba Hello projde tyto kroky pro každou dávku, dokud všechny shardlets byly přesunuty. To povede operace kill připojení tooseveral průběhu hello hello dokončení rozdělení nebo sloučení nebo přesunutí operace.  

**Správa shardlet dostupnosti**

Omezení připojení hello ukončení toohello aktuální dávku shardlets, jak je popsáno výše omezuje hello oboru nedostupnosti tooone dávku shardlets najednou. Toto je upřednostňovaný přes přístup kde hello dokončení horizontálního oddílu by zůstat offline pro všechny její shardlets průběhu hello operace rozdělení nebo sloučení. Hello velikost dávky, definován jako hello počet jedinečných shardlets toomove současně, je parametr konfigurace. Může být definováno pro každou operaci rozdělení a merge v závislosti na potřebách dostupnosti a výkonu aplikace hello. Všimněte si, že hello rozsah, který se v mapě horizontálního oddílu hello zamykají může být větší než zadaná velikost dávky hello. Je to proto, že služba hello vybere hello rozsah velikost tak, aby hello skutečný počet horizontálního dělení hodnoty klíče v datech hello přibližně odpovídá velikost dávky hello. Toto je důležité tooremember konkrétně pro klíče řídce vyplněná horizontálního dělení. 

**Metadata úložiště**

Služba rozdělení sloučení Hello používá databázi toomaintain, jeho stav a tookeep protokolů při zpracování požadavku. Hello uživatel vytvoří tato databáze v rámci svého předplatného a poskytuje hello připojovací řetězec pro ho v konfiguračním souboru hello pro nasazení služby hello. Správci z hello uživatele organizaci se může připojit i toothis databáze tooreview požadavek průběh a tooinvestigate podrobné informace o potenciální chyby.

**Sledování horizontálního dělení**

Hello rozdělení sloučení služby rozlišuje (1) horizontálně dělené tabulky, (2) referenční tabulky a (3) normální tabulky. Hello sémantika rozdělení nebo sloučení nebo přesunutí operace závisí na typu hello tabulky hello používá a jsou definovány takto: 

* **Horizontálně dělené tabulky**: operace přesunutí, sloučení a rozdělení přesunout shardlets z horizontálních tootarget zdroje. Po úspěšném dokončení hello celkové požadavků, tyto shardlets již neexistují u zdroje hello. Upozorňujeme, že hello cílové tabulky potřebovat tooexist na cílový hello horizontálního oddílu a nesmí obsahovat data v předchozí tooprocessing hello cílový rozsah hello operace. 
* **Referenční tabulky**: referenční tabulky, rozdělení hello sloučení a přesunout operace kopírování dat hello z horizontálního hello zdroj toohello cílového oddílu. Upozorňujeme však, že žádné změny dojít na hello horizontálního oddílu cíl pro danou tabulku, pokud všechny řádky, se již nachází v této tabulce na cíli hello. Tabulka Hello má toobe prázdná pro všechny referenční tabulky kopie operaci tooget zpracovat.
* **Ostatní tabulky**: ostatní tabulky mohou existovat na hello zdroj nebo cíl hello operace rozdělení a merge. Služba rozdělení sloučení Hello ignoruje tyto tabulky pro všechny přesuny dat nebo operace kopírování. Upozorňujeme však, že může narušovat tyto operace v případě omezení.

Hello informace na odkaz oproti horizontálně dělené tabulky jsou poskytovány hello **SchemaInfo** rozhraní API na mapě hello horizontálního oddílu. Hello následující příklad ukazuje použití hello tato rozhraní API na danou horizontálních mapy manager objekt smm: 

    // Create hello schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Hello tabulky 'region' a 'výhod, jsou definovány jako referenční tabulky a se zkopírují s operacemi rozdělení nebo sloučení nebo přesunutí. "zákazník" a "objednávky, pak jsou definovány jako horizontálně dělené tabulky. C_CUSTKEY a O_CUSTKEY slouží jako klíč hello horizontálního dělení. 

**Referenční integrita**

Služba rozdělení sloučení Hello analyzuje závislosti mezi tabulkami a využívá operace hello toostage relace cizích klíčů primární klíč pro přesun referenční tabulky a shardlets. Obecně platí referenční tabulky se zkopírují první v pořadí závislostí, pak shardlets zkopírují v pořadí podle jejich závislosti v rámci každé dávky. To je nezbytné, aby Primárníklíč-Cizíklíč omezení horizontálních cíl hello se uplatní jako dorazí hello nová data. 

**Konzistence mapy horizontálního oddílu a případné dokončení**

V hello přítomnost chyb služba rozdělení sloučení hello obnoví operations po všech výpadku a má za cíl toocomplete žádné v žádostech o průběhu. Však může existovat neopravitelné situacích, například hello cíl horizontálních dojde ke ztrátě nebo ohrožení zabezpečení nad rámec opravit. Za těchto okolností může některé shardlets, které se měly přesunout toobe stále tooreside na horizontálních zdroj hello. Hello služby zajišťuje mapování shardlet jsou po úspěšně zkopírovaný toohello cíl hello potřebná data jenom aktualizaci. Shardlets se odstraní pouze na zdroji hello, poté zkopírovat všechny, svá data byla úspěšně toohello cíl a odpovídající mapování hello bylo úspěšně aktualizováno. operace odstranění Hello se stane hello pozadí při hello rozsah již je online na horizontálního hello cílového oddílu. Hello rozdělení sloučení služby vždy zajišťuje správnost mapování hello uložené v mapě hello horizontálního oddílu.

## <a name="hello-split-merge-user-interface"></a>Hello sloučení rozdělení uživatelské rozhraní
balíček služby rozdělení sloučení Hello zahrnuje roli pracovního procesu a webové role. Hello webové role je použité toosubmit rozdělení sloučení požadavky interaktivní způsobem. hlavní součásti Hello hello uživatelského rozhraní jsou následující:

* Typ operace: typ operace hello je přepínač, který řídí hello druh operace provádí hello služby pro tento požadavek. Můžete zvolit hello rozdělení, sloučení a přesunout scénáře. Můžete také zrušit dříve odeslaný operaci. Můžete použít rozdělení, sloučení a přesunout požadavky pro rozsah horizontálního oddílu mapy. Seznam horizontálních mapuje pouze operace přesunutí podpory.
* Mapování horizontálních: hello další části žádost parametry titulní informace o mapování horizontálních hello a hostování mapu horizontálního oddílu hello databáze. Konkrétně potřebujete název hello tooprovide hello serveru Azure SQL Database a hostování hello shardmap databáze, přihlašovací údaje tooconnect toohello horizontálních mapovat databázi a nakonec hello název mapování horizontálních hello. V současné době operaci hello přijímá pouze jedinou sadu pověření. Tyto přihlašovací údaje musí dostatečná oprávnění toohave tooperform změny toohello horizontálního oddílu mapy, jakož i toohello uživatelská data na hello horizontálních oddílů.
* Rozsah zdrojových (rozdělení a sloučení): operace rozdělení a sloučení zpracovává rozsah pomocí klíče vysoké a nízké. toospecify operace s bez vazby vysoká hodnota klíče, zaškrtněte políčko "vysoká hodnota klíče je maximální" hello a hello vysoké klíčové pole ponechte prázdné. Hello rozsah hodnoty klíče, které zadáte do tooprecisely nutné neodpovídá mapování a jeho hranice na mapě horizontálního oddílu. Pokud nezadáte vůbec žádné hranice rozsahu hello služby automaticky odvození hello nejbližší rozsahu. Pomocí hello GetMappings.ps1 PowerShell skriptu tooretrieve hello aktuální mapování v mapě dané horizontálního oddílu.
* Rozdělení zdroj chování (rozdělení): pro rozdělení operace definovat rozsah zdrojových hello bodu toosplit hello. To provedete zadáním hello horizontálního dělení klíč, kterou má hello rozdělení toooccur. Přepínač použít hello zadejte, zda chcete hello dolní části toomove hello rozsah (s výjimkou hello rozdělení klíč), nebo jestli chcete toomove hello horní části (včetně klíče rozdělení hello).
* Zdroj Shardlet (přesunout): přesunout operace se liší od rozdělení nebo sloučení operace jako zdroj rozsah toodescribe hello nevyžadují. Zdroj pro přesun je jednoduše identifikována hodnota klíče horizontálního dělení hello plánování toomove.
* Cíl horizontálního oddílu (rozdělení): po hello informace jste zadali na hello zdroj rozdělení operaci, musíte toodefine místo, kam chcete zkopírovat hello data toobe tooby poskytnete hello databázi Azure SQL serveru a název databáze pro cíl hello.
* Cílový rozsah (sloučení): operace sloučení přesunout shardlets tooan stávající ID horizontálního oddílu. Tím, že poskytuje hello rozsah hranice hello existující oblast, která má být toomerge s identifikujete hello existující horizontálního oddílu.
* Velikost dávky: ovládací prvky velikost dávky hello hello počet shardlets, která přejde do režimu offline v době při přesunu dat hello. Toto je celočíselná hodnota, kde můžete použít nižší hodnoty po citlivé toolong doby výpadku pro shardlets. Vyšší hodnoty zvýší hello čas, který je daný shardlet offline však může zvýšit výkon.
* Id operace (zrušit): Pokud máte probíhající operace, která již nepotřebujete, můžete operaci hello zrušit zadáním jeho ID operace v tomto poli. ID operace hello můžete načíst z hello požadavek stav tabulky (viz oddíl 8.1) nebo z výstupu hello hello webového prohlížeče, kde jste odeslali žádost o hello.

## <a name="requirements-and-limitations"></a>Požadavky a omezení
Hello aktuální implementace služby rozdělení sloučení hello je subjektu toohello následující požadavky a omezení: 

* horizontálních oddílů Hello potřebovat tooexist a zaregistrovat v mapě horizontálního oddílu hello před provedením operace sloučení rozdělení na těchto horizontálních oddílů. 
* Služba Hello nevytváří tabulky a všechny další objekty databáze automaticky v rámci jeho operací. To znamená, které hello schématu pro všechny horizontálně dělené tabulky a odkazy na tabulky, třeba tooexist na hello cíl horizontálního oddílu předchozí tooany rozdělení nebo sloučení nebo přesunutí operaci. Horizontálně dělené tabulky jsou zejména požadované toobe v rozsahu hello, kde jsou nové shardlets toobe přidat operací rozdělení nebo sloučení nebo přesunutí prázdné. Jinak hodnota hello se nezdaří kontrolu konzistence počáteční hello horizontálního hello cílového oddílu. Všimněte si, že referenční data zkopírován pouze pokud hello Referenční tabulka prázdná a že nejsou žádné konzistence také zaručuje s ohledem tooother zápisu souběžných operací na hello referenční tabulky. Doporučujeme toto: při spuštění operace rozdělení nebo sloučení, žádné jiné operace zápisu provést změny toohello referenční tabulky.
* Služba Hello spoléhá na řádek identity vytvořit jedinečný index nebo klíč, který zahrnuje hello horizontálního dělení klíče tooimprove výkon a spolehlivost pro velké shardlets. To umožňuje hello služby toomove dat na i podrobnější než právě hello horizontálního dělení hodnotu klíče. To pomáhá tooreduce hello maximální množství místa protokolu a zámky, které jsou požadovány během operace hello. Můžete vytvořit jedinečný index nebo primární klíč, včetně hello horizontálního dělení klíče v dané tabulce, pokud chcete, aby toouse této tabulky s rozdělení nebo sloučení nebo přesunutí požadavky. Z důvodů výkonu hello horizontálního dělení klíč by měl být hello počáteční sloupec v hello klíč nebo hello index.
* Během hello během zpracování požadavku může být některá data shardlet přítomen hello zdroj a cíl hello horizontálního oddílu. Toto je nezbytné tooprotect proti selhání během pohybu shardlet hello. Hello integrace rozdělení sloučení s hello horizontálního oddílu mapy zajistí, že připojení prostřednictvím hello dat závislé směrování rozhraní API pomocí hello **OpenConnectionForKey** metoda na mapě hello horizontálního oddílu nejsou vidět všechny nekonzistentní zprostředkující stavy. Ale při připojování toohello zdroj nebo hello cíle horizontálních oddílů bez použití hello **OpenConnectionForKey** metoda, nekonzistentní zprostředkující stavy může být viditelné, pokud se děje rozdělení nebo sloučení nebo přesunutí požadavky. Tato připojení může zobrazovat částečné nebo duplicitní výsledky v závislosti na hello časování nebo hello horizontálního oddílu Základní hello připojení. Toto omezení aktuálně zahrnuje hello připojení elastické škálování Shard více – dotazy.
* Hello databázi metadat pro službu hello rozdělení sloučení nesmí sdílet mezi různé role. Například role hello rozdělení sloučení službu v pracovní potřebám toopoint tooa rozdílná metadata databáze než hello produkční role.

## <a name="billing"></a>Fakturace
Služba Hello rozdělení sloučení běží jako cloudová služba v rámci vašeho předplatného Microsoft Azure. Proto pro cloudové služby poplatky za instance tooyour služby hello. Pokud provádíte často rozdělení nebo sloučení nebo přesunutí operace, doporučujeme že odstranit rozdělení sloučení cloudové služby. Která šetří náklady pro spuštění nebo nasazena instance služby cloudu. Můžete znovu nasadit a spustit konfiguraci snadno spustitelného kdykoli budete potřebovat tooperform rozdělení nebo sloučení operace. 

## <a name="monitoring"></a>Monitorování
### <a name="status-tables"></a>Stav tabulky
Hello rozdělení sloučení služba poskytuje hello **stavem** tabulky v databázi úložiště hello metadat pro sledování dokončené a probíhající požadavků. Hello tabulka obsahuje řádek pro každý požadavek sloučení rozdělení, která byla odeslaná toothis instanci služby rozdělení sloučení hello. Nabízí následující informace pro každý požadavek hello:

* **Časové razítko**: hello čas a datum, když požadavek hello.
* **OperationId**: A identifikátor GUID, který jedinečně identifikuje hello požadavku. Tento požadavek může být také použít toocancel hello operace, i když je stále probíhající.
* **Stav**: hello aktuální stav žádosti hello. Probíhající požadavky také vypíše hello aktuální fáze, ve které hello je požadavek.
* **CancelRequest**: Příznak určující, zda text hello požadavek byl zrušen.
* **Průběh**: procento odhad dokončení operace hello. Hodnotu 50 označuje, že operace hello je přibližně 50 % dokončení.
* **Podrobnosti o**: hodnota ve formátu XML, která poskytuje podrobnější sestava průběhu. Sestava průběhu Hello je pravidelně aktualizovat, protože sady řádků se zkopírují ze zdroje tootarget. V případě selhání nebo výjimky tento sloupec také obsahuje podrobnější informace o selhání hello.

### <a name="azure-diagnostics"></a>Diagnostika Azure
Služba rozdělení sloučení Hello používá Azure Diagnostics podle Azure SDK 2.5 pro monitorování a Diagnostika. Jak je popsáno v tomto poli se řídit konfigurace diagnostiky hello: [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](../cloud-services/cloud-services-dotnet-diagnostics.md). Hello balíček ke stažení obsahuje dvě konfigurace diagnostiky – jeden pro hello webovou roli a jeden pro roli pracovního procesu hello. Tyto konfigurace diagnostiky pro službu hello postupujte podle pokynů hello od [základy cloudové služby ve službě Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Obsahuje hello Definice toolog čítače výkonu, protokoly služby IIS, protokoly událostí systému Windows a protokoly událostí aplikace rozdělení sloučení. 

## <a name="deploy-diagnostics"></a>Nasazení diagnostiky
tooenable monitorování a Diagnostika pomocí hello konfigurace diagnostiky pro webové a pracovní role hello poskytované hello balíček NuGet, spusťte následující příkazy pomocí Azure PowerShell hello: 

    $storage_name = "<YourAzureStorageAccount>" 

    $key = "<YourAzureStorageAccountKey" 

    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  


    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 


    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Můžete najít další informace o tooconfigure a nasadit nastavení diagnostiky zde: [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Načtení diagnostiky
Můžete snadno přístup k vaší diagnostiky z hello Průzkumníka serveru Visual Studia v hello Azure části stromu Průzkumníka serveru hello. Otevřete Visual Studio instance a v řádku nabídek hello klikněte na zobrazení a Průzkumníka serveru. Klikněte na tlačítko hello Azure – ikona tooconnect tooyour předplatného Azure. Pak přejděte tooAzure -> úložiště -> <your storage account> -> WADLogsTable-tabulky >. Další informace najdete v tématu [procházení prostředků úložiště pomocí Průzkumníka serveru](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

Hello WADLogsTable zvýrazněných v předchozí obrázek hello obsahuje hello podrobné události z protokolu aplikace hello rozdělení sloučení služby. Všimněte si, že hello výchozí konfigurace hello Stáhnout balíček byla zaměřena produkčním nasazení. Hello interval, kdy jsou protokoly a čítače načtený z instance služby hello je proto velké (5 minut). Pro testovací a vývojové musí nižší interval hello úpravou hello nastavení diagnostiky webové hello nebo tooyour role pracovního procesu hello. Klikněte pravým tlačítkem na roli hello v hello Průzkumníka serveru Visual Studia (viz výše) a upravte hello přenosu období v dialogovém okně hello nastavení konfigurace diagnostiky hello: 

![Konfigurace][3]

## <a name="performance"></a>Výkon
Obecně platí je lepší výkon toobe od hello vyšší, očekává více úrovní služeb původce ve službě Azure SQL Database. Vyšší vstupně-výstupní operace, procesoru a paměti přidělení pro vyšší úrovně služeb hello těžit hello hromadné kopírování a odstraňte operace, které hello používá služba rozdělení sloučení. Z tohoto důvodu zvýšit úroveň služby hello jen pro tyto databáze definované, omezenou dobu.

Služba Hello také provádí ověření dotazů v rámci jeho běžných operací. Tyto dotazy ověření zkontrolujte neočekávané přítomnosti dat v rozsahu cílového hello a zajistilo, že všechny operace rozdělení nebo sloučení nebo přesunutí spuštění z konzistentním stavu. Všechny tyto dotazy fungovat přes horizontálního dělení rozsahy klíčů definované hello rozsah hello operace a velikost dávky hello zadaný jako součást definice hello žádosti. Tyto dotazy se nejlépe provést, pokud indexu je přítomen, se hello horizontálního dělení klíč jako hello počáteční sloupec. 

Vlastnost jedinečnosti klíčem hello horizontálního dělení jako počáteční sloupec hello kromě toho vám umožní hello služby toouse optimalizované přístup, který omezuje spotřeby prostředků z hlediska místa protokolu a paměti. Tato vlastnost jedinečnosti je požadovaná toomove velikostí velkých objemů dat (obvykle vyšší než 1GB). 

## <a name="how-tooupgrade"></a>Jak tooupgrade
1. Postupujte podle kroků hello v [nasazení služby rozdělení sloučení](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Změňte cloudu konfigurační soubor služby pro nasazení rozdělení sloučení tooreflect hello nové konfigurace parametry. Nový požadovaný parametr je hello informace o hello certifikát použitý k šifrování. Snadný způsob toodo jde toocompare hello nový konfigurační soubor šablony ze stažení hello proti vaší stávající konfigurace. Ujistěte se, že přidáte hello nastavení "DataEncryptionPrimaryCertificateThumbprint" a "DataEncryptionPrimary" hello webové a roli pracovního procesu hello.
3. Před nasazením aktualizace tooAzure hello, ověřte, že všechny operace právě probíhající rozdělení sloučení byl dokončen. Snadno to provedete pomocí dotazu hello stavem a PendingWorkflows tabulky v databázi metadat sloučení rozdělení hello probíhající požadavkům.
4. Aktualizujte stávající nasazení cloudové služby pro rozdělení sloučení vašeho předplatného Azure s hello nový balíček a konfiguračním souboru aktualizované služby.

Není nutné tooprovision novou databázi metadat pro tooupgrade rozdělení sloučení. Nová verze Hello automaticky upgraduje vaše stávající databáze toohello nové verze metadat. 

## <a name="best-practices--troubleshooting"></a>Osvědčené postupy a řešení potíží
* Definování testovacím klientem a vykonávat vaše nejdůležitější rozdělení nebo sloučení nebo přesunutí operace s testovacím klientem hello napříč několika horizontálních oddílů. Zkontrolujte, že všechna metadata správně definován v mapě horizontálního oddílu a že hello operace není v rozporu omezení nebo cizí klíče.
* Velikost dat klienta testovací hello zachovat výše hello maximální velikost dat klientovi největší tooensure nejsou zjištění velikost dat souvisejících s problémy. To vám usnadní vyhodnocení na hello čas potřebný toomove jednoho klienta kolem horní mez. 
* Ujistěte se, že schéma umožňuje odstranění. Služba rozdělení sloučení Hello vyžaduje hello možnost tooremove data ze zdroje horizontálních hello po hello data byla úspěšně zkopírovaný toohello cíl. Například **odstranit aktivační události** můžete zabránit odstraňování hello dat na zdroji hello hello služby a může způsobit toofail operace.
* Hello horizontálního dělení klíč by měl být hello počáteční sloupec ve vaší primární klíč nebo jedinečný index definice. Zajistí hello nejlepší výkon pro hello rozdělení nebo sloučení ověření dotazů a pro hello skutečná data přesouvání a odstranění operací, které pracují vždy se rozsahy klíčů horizontálního dělení.
* Společně umístit služby rozdělení sloučení v oblasti a data center hello kde jsou umístěny databáze. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png

