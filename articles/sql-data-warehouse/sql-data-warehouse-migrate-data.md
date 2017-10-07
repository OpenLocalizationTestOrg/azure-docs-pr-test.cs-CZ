---
title: "aaaMigrate tooSQL vaše data datového skladu | Microsoft Docs"
description: "Tipy pro migraci vaše data tooAzure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: d78f954a-f54c-4aa4-9040-919bc6414887
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/29/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: fe4c6b7e82094c59c45e06be6da225fee1b707ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data"></a>Migrace dat
Data lze přesunout z různých zdrojů do SQL Data Warehouse pomocí různých nástrojů.  Kopírování ADF, SSIS a všechny bcp můžou být použité tooachieve tohoto cíle. Při rostoucí hello množství dat, musí však rozmyslete rozdělení hello proces migrace dat do kroků. To umožňuje hello možnost toooptimize každý krok pro výkon i pro odolnost tooensure smooth dat migrace.

Tento článek popisuje nejprve hello jednoduché migrační scénáře ADF Copy, SSIS a bcp. Následně vypadá trochu hlouběji do jak lze optimalizovat hello migrace.

## <a name="azure-data-factory-adf-copy"></a>Azure Data Factory (ADF) kopie
[Kopírování ADF] [ ADF Copy] je součástí [Azure Data Factory][Azure Data Factory]. Můžete kopírovat ADF tooexport datové soubory tooflat umístěný v místním úložišti plochých souborů tooremote uchovávat v Azure blob storage nebo přímo do SQL Data Warehouse.

Pokud vaše data se spustí v plochých souborů, pak musíte nejdřív tootransfer se objekt blob úložiště tooAzure před zahájením zatížení do SQL Data Warehouse. Jakmile hello data se přenáší do úložiště objektů blob v Azure můžete toouse [ADF kopie] [ ADF Copy] znovu toopush hello dat do SQL Data Warehouse.

PolyBase také nabízí možnost vysoce výkonné pro načítání dat hello. Ale, která znamená, pomocí dvou nástrojů místo jeden. Pokud jste třeba nejlepší výkon hello pak pomocí funkce PolyBase. Pokud chcete, aby prostředí jednotný nástroj (a hello dat není masivní) ADF je vaše odpověď.


> 
> 

HEAD přes toohello následujícího článku pro některé dobře [ADF ukázky][ADF samples].

## <a name="integration-services"></a>Integrační služby
Integration Services (SSIS) je výkonný a flexibilní extrahovat transformace a načítání (ETL) nástroj, který podporuje komplexní pracovní postupy, transformaci dat a několik možností načítání data. Použití služby SSIS toosimply přenos dat tooAzure nebo v rámci širší migrace.

> [!NOTE]
> SSIS můžete exportovat tooUTF-8 bez hello značka pořadí bajtů v souboru hello. tooconfigure, to je nutné nejprve použít hello odvodit sloupec součástí tooconvert hello textová data v hello datového toku toouse hello 65001 UTF-8 znaková stránka. Jakmile byly převedeny hello sloupců, zapisovat hello data toohello plochý soubor cílový adaptér zajistit, že 65001 také byla vybrána jako hello znaková stránka pro soubor hello.
> 
> 

SSIS připojí tooSQL datového skladu, stejně jako se bude připojovat tooa nasazení systému SQL Server. Připojení se ale potřebovat toobe Správci připojení ADO.NET. Můžete také dbát na tooconfigure hello "Použijte příkaz bulk insert Pokud je k dispozici" nastavení toomaximize propustnost. Naleznete toohello [ADO.NET cílový adaptér] [ ADO.NET destination adapter] článku toolearn Další informace o této vlastnosti

> [!NOTE]
> Připojení tooAzure SQL Data Warehouse pomocí OLEDB není podporováno.
> 
> 

Kromě toho je vždy hello možnost, že balíček může selhat z důvodu toothrottling nebo síťové problémy. Návrh balíčků tak, že lze obnovit v hello bodem selhání, bez opakovaného fungovat dokončení před selháním hello.

Další informace naleznete hello [SSIS dokumentaci][SSIS documentation].

## <a name="bcp"></a>bcp
BCP je nástroj příkazového řádku, která je určená pro export a import dat plochý soubor. Některé transformace může probíhat během exportu dat. jednoduché transformace tooperform pomocí dotazu tooselect a transformovat hello data. Po exportu, pak dají načíst přímo do databáze SQL Data Warehouse hello cíl hello hello plochých souborů.

> [!NOTE]
> Často je text hello tooencapsulate vhodné transformace použít během data exportovat v zobrazení zdroje systému hello. To zajišťuje, že se uchovávají hello logiku a hello proces opakovatelných.
> 
> 

Výhody bcp jsou:

* Jednoduchost. příkazy BCP jsou jednoduché toobuild a provést
* Znovu spustit zatížení proces. Jednou exportovaný hello zatížení může být provedeny v libovolném počtu

Omezení bcp jsou:

* BCP pracuje s tabulce pouze plochých souborů. Nefunguje s soubory, jako jsou xml nebo JSON
* Funkce transformace dat jsou omezené toohello export jenom stage a jednoduchý ve své podstatě
* BCP nebyla přizpůsobena toobe robustní při načítání dat přes hello Internetu. Nestability sítě může způsobit chybu zatížení.
* využívá BCP hello schématu aplikace hello cílové databáze předchozí toohello zatížení

Další informace najdete v tématu [pomocí bcp tooload dat do SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].

## <a name="optimizing-data-migration"></a>Optimalizace migrace dat
Proces migrace dat SQLDW můžete efektivně rozdělit na tři samostatné kroky:

1. Export zdroje dat
2. Přenos dat tooAzure
3. Načtení do hello cílová SQLDW databáze

Každý krok může být jednotlivě optimalizované toocreate proces migrace robustní, znovu spustit a odolné, který maximalizuje výkon při každém kroku.

## <a name="optimizing-data-load"></a>Optimalizace načtení dat
Prohlížení tyto v obráceném pořadí na chvíli; Hello nejrychlejší způsob, jak tooload dat je prostřednictvím PolyBase. Optimalizace pro proces zatížení PolyBase umístí požadavky na hello předchozích kroků, je nejlepší toounderstand to předem. Jsou:

1. Kódování dat souborů
2. Formátu dat souborů
3. Umístění datových souborů

### <a name="encoding"></a>Encoding
PolyBase vyžaduje toobe datové soubory ve formátu UTF-8 nebo UTF-16FE. 



### <a name="format-of-data-files"></a>Formátu dat souborů
PolyBase vyžaduje pevnou řádek ukončovací \n nebo nový řádek. Datové soubory musí odpovídat toothis standard. Nejsou k dispozici žádné omezení konců řetězec nebo sloupec.

Budete mít toodefine každý sloupec v souboru hello jako součást externí tabulku v PolyBase. Ujistěte se, že všechny sloupce exportovaný jsou povinné a že hello typy v souladu s toohello požadované standardy.

Zpět naleznete toohello [migrovat schéma] najdete v článku o podporované datové typy.

### <a name="location-of-data-files"></a>Umístění datových souborů
SQL Data Warehouse používá výhradně PolyBase tooload dat z Azure Blob Storage. V důsledku toho hello dat musí nejprve přenesení do úložiště objektů blob.

## <a name="optimizing-data-transfer"></a>Optimalizace přenos dat
Jedna z částí nejpomalejší hello migrace dat je hello přenos dat tooAzure hello. Pouze šířku pásma sítě může být problém, ale také spolehlivost sítě může být vážnou překážkou průběh. Ve výchozím nastavení je tooAzure přenášení dat přes internet proto hello případné chyby přenos, ke kterým dochází přiměřeně pravděpodobně hello. Tyto chyby však může vyžadovat opětovné odeslání celé nebo částečně toobe data.

Naštěstí máte několik možností tooimprove hello rychlost a pružnost tohoto procesu:

### <a name="expressrouteexpressroute"></a>[ExpressRoute][ExpressRoute]
Může být vhodné pomocí tooconsider [ExpressRoute] [ ExpressRoute] toospeed až hello přenosu. [ExpressRoute] [ ExpressRoute] poskytuje vám tooAzure vytvořeného privátní připojení tak hello připojení nejde přes hello veřejného Internetu. Toto je rozhodně není povinný krok. Ale ho bude při nabízení data tooAzure z místního zlepšit propustnost nebo společném umístění.

Hello výhody použití [ExpressRoute] [ ExpressRoute] jsou:

1. Větší spolehlivost
2. Vyšší rychlost sítě
3. Nižší latenci sítě
4. vyšší zabezpečení sítě

[ExpressRoute] [ ExpressRoute] je výhodné pro různé scénáře, ne jenom hello migrace.

Zajímá vás to? Další informace a ceny prosím naleznete hello [dokumentace ExpressRoute][ExpressRoute documentation].

### <a name="azure-import-and-export-service"></a>Azure Import a Export služby
Hello Azure Import a Export služby je proces přenosu dat určená pro velké (GB ++) toomassive (TB ++) přenosů dat do Azure. Se týká psaní toodisks vaše data a jejich přesouvání tooan datového centra Azure. obsah disku Hello budou načteny potom do objektů BLOB služby Azure Storage vaším jménem.

Souhrnné zobrazení procesu exportu importovat hello vypadá takto:

1. Konfigurace Azure Blob Storage kontejneru tooreceive hello dat
2. Export toolocal úložiště dat
3. Zkopírujte hello data too3.5 palec SATA II/III jednotky pevného disku pomocí hello [Azure Import/Export nástroj]
4. Vytvoření úlohy importu pomocí hello Azure Import a Export služba poskytující soubory deníku hello vyprodukované hello [Azure Import/Export nástroj]
5. Dodávat hello disky určenou Azure datové centrum
6. Vaše data jsou přenášená tooyour kontejneru Azure Blob Storage
7. Načtení dat hello do SQLDW pomocí PolyBase

### <a name="azcopyazcopy-utility"></a>[AZCopy][AZCopy] nástroj
Hello [AZCopy][AZCopy] nástroj je skvělý nástroj pro získávání dat přenesených do objektů BLOB služby Azure Storage. Je určený pro malé (MB ++) toovery velké (GB ++) přenosy dat. [AZCopy] byl také navrženou tooprovide dobrý odolné propustnost při přenosu dat tooAzure a tak je, že je služba skvělou volbou pro krok přenos dat hello. Jednou přenášená načtením hello dat do SQL Data Warehouse pomocí PolyBase. AZCopy taky můžete začlenit do vaší služby SSIS balíčky pomocí "Spustit proces" úlohy.

toouse AZCopy, bude nejprve nutné toodownload a nainstalujte ji. Je [produkční verzi] [ production version] a [verze preview] [ preview version] k dispozici.

tooupload soubor ze systému souborů, které budete potřebovat příkaz jako hello jeden níže:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Proces vysoké úrovně souhrn může být:

1. Konfigurace úložiště Azure blob kontejneru tooreceive hello dat
2. Export toolocal úložiště dat
3. AZCopy daty v kontejneru Azure Blob Storage hello
4. Načtení hello dat do SQL Data Warehouse pomocí PolyBase

Úplné dokumentaci k dispozici: [AZCopy][AZCopy].

## <a name="optimizing-data-export"></a>Optimalizace export dat
Tooensuring, hello export vyhovuje toohello požadavky definované polybase můžete kromě toho můžete hledat toooptimize hello export proces hello tooimprove hello dat další.



### <a name="data-compression"></a>Komprese dat
PolyBase čte data komprimované gzip. Pokud jsou možné toocompress datové soubory toogzip pak můžete minimalizovat hello množství dat, které se instaluje přes síť hello.

### <a name="multiple-files"></a>Více souborů
Rozdělení rozsáhlé tabulky do několika souborů umožňuje nejenom tooimprove exportovat rychlost, také pomáhá s přenosu re-startability a hello celkové možnosti správy dat hello jednou v úložišti objektů blob Azure hello. Jeden z hello mnoho dobrý funkce PolyBase je, že bude číst všechny hello soubory do složky a s nimi zacházet jako jedna tabulka. Proto je vhodné tooisolate hello souborů pro každou tabulku do vlastní složky.

PolyBase také podporuje funkci označuje jako "rekurzivní složky traversal". Tuto funkci můžete používat toofurther vylepšit hello organizace vaší tooimprove exportovaná data vaší dat správy.

toolearn Další informace o načtení dat pomocí funkce PolyBase, najdete v části [použijte PolyBase tooload dat do SQL Data Warehouse][Use PolyBase tooload data into SQL Data Warehouse].

## <a name="next-steps"></a>Další kroky
Další informace o migraci najdete v tématu [migrace vašeho řešení tooSQL datového skladu][Migrate your solution tooSQL Data Warehouse].
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[AZCopy]:../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution tooSQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp tooload data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase tooload data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
