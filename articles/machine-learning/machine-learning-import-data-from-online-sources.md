---
title: "aaaImport data do nástroje Machine Learning Studio ze zdroje dat online | Microsoft Docs"
description: "Jak tooimport data školení Azure Machine Learning Studio z různých zdrojů online."
keywords: "Importujte dat, formát dat, datové typy, zdroje dat, Cvičná data"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;garye
ms.openlocfilehash: aae6907cdd0b4dc373ae08c2569caa276c198b49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-hello-import-data-module"></a>Importovat data do Azure Machine Learning Studio z různých zdrojů dat online s modulem importovat Data hello
Tento článek popisuje hello podporu pro import dat online z různých zdrojů a informace hello potřeby toomove data z těchto zdrojů do experimentu Azure Machine Learning.

> [!NOTE]
> Tento článek poskytuje obecné informace o hello [importovat Data] [ import-data] modulu. Podrobnější informace o typech hello dat dostanete formáty, parametry a odpovědi toocommon otázky, najdete v části hello modulu referenční téma pro hello [importovat Data] [ import-data] modulu.
> 
> 

<!-- -->

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Úvod
Pomocí hello [importovat Data] [ import-data] modul, můžete přistupujete k datům z několika zdrojů dat online spuštěného experimentu [Azure Machine Learning Studio](https://studio.azureml.net/Home):

* Adresa URL webového pomocí protokolu HTTP
* Hadoop pomocí HiveQL
* Úložiště objektů blob v Azure
* Tabulky Azure
* Azure SQL database nebo SQL Server na virtuálním počítači Azure
* Místní databáze systému SQL Server
* Datový kanál poskytovatele, aktuálně OData
* Azure CosmosDB (dříve nazývané DocumentDB)

Přidat zdroje dat online tooaccess v experimentu Studio hello [importovat Data] [ import-data] tooyour modulu, vyberte hello **zdroj dat**a pak zadejte potřebné parametry hello tooaccess hello data. Hello online datových zdrojů, které jsou podporovány je uvedeno v tabulce hello. Tato tabulka shrnuje hello formáty souborů, které jsou podporovány a parametry, které jsou používané tooaccess hello data.

Všimněte si, že vzhledem k tomu, že tato data školení přistupuje spuštěného experimentu, je k dispozici pouze v tomto experimentu. Pro srovnání data, která byla uložena v modulu datové sady jsou k dispozici tooany experimentu v pracovním prostoru.

> [!IMPORTANT]
> V současné době hello [importovat Data] [ import-data] a [Export dat] [ export-data] moduly lze číst a zapisovat data pouze z úložiště Azure, které jsou vytvořené pomocí hello Model nasazení Classic. Jinými slovy hello nový typ účtu úložiště objektů Blob Azure, která nabízí nástroje pro úroveň přístupu horkého úložiště nebo úroveň přístupu studeného úložiště ještě není podporovaný. 
> 
> Obecně platí, všechny účty úložiště Azure, že jste mohli vytvořit předtím, než jsou k dispozici možnost této služby by neměl mít vliv. 
> Pokud potřebujete toocreate nový účet, vyberte **Classic** pro hello nasazení modelu, nebo pomocí Správce prostředků a vyberte **obecné účely** místo **úložiště objektů Blob** pro **Účet typu**. 
> 
> Další informace najdete v tématu [Azure Blob Storage: horká a studená vrstvy úložiště](../storage/blobs/storage-blob-storage-tiers.md).
> 
> 

## <a name="supported-online-data-sources"></a>Podporované zdroje dat online
Azure Machine Learning **importovat Data** modul podporuje hello následující zdroje dat:

| Zdroj dat | Popis | Parametry |
| --- | --- | --- |
| Adresa URL webové prostřednictvím protokolu HTTP |Čte data hodnot oddělených čárkami (CSV), hodnoty oddělené tabulátory (TSV), formát souboru atribut vztah (ARFF) a formáty Support Vector počítače (SVM-light) z jakékoli webové adresy URL, která používá protokol HTTP |<b>Adresa URL</b>: Určuje hello celý název souboru hello, včetně adres URL hello lokality a název souboru hello, s jakoukoli příponou. <br/><br/><b>Formát dat</b>: Určuje jeden z datových hello podporované formáty: CSV, TSV, ARFF nebo SVM-light. Pokud hello data obsahuje řádek záhlaví, je použít tooassign názvy sloupců. |
| Hadoop/HDFS |Čte data z distribuovaného úložiště v Hadoop. Zadáte hello data, která chcete pomocí HiveQL, jako SQL dotazovací jazyk. HiveQL lze také použít tooaggregate dat a provádět data filtrování předtím, než přidáte hello data tooMachine Learning Studio. |<b>Databázový dotaz Hive</b>: Určuje dotaz Hive hello používá toogenerate hello data.<br/><br/><b>Identifikátor URI serveru HCatalog </b> : Zadaný hello názvem vašeho clusteru formátu hello  *&lt;název clusteru&gt;. azurehdinsight.net.*<br/><br/><b>Název uživatelského účtu Hadoop</b>: Určuje název používá tooprovision hello clusteru hello Hadoop uživatelský účet.<br/><br/><b>Heslo uživatelského účtu Hadoop</b> : Určuje hello pověření použitá při zřizování clusteru hello. Další informace najdete v tématu [vytvoření Hadoop clusterů v HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Umístění výstupu dat</b>: Určuje, zda text hello data jsou uložena v Hadoop distributed file system (HDFS) nebo v Azure. <br/><ul>Pokud ukládáte výstupní data v HDFS, zadejte identifikátor URI serveru HDFS hello. (Být se název clusteru HDInsight toouse hello bez hello předponu HTTPS://). <br/><br/>Pokud výstupní data ukládáte v Azure, musíte zadat název účtu úložiště Azure hello, přístupový klíč k úložišti a název kontejneru úložiště.</ul> |
| Databáze SQL |Čte data, která je uložená v databázi Azure SQL nebo v databázi systému SQL Server spuštěna na virtuálním počítači Azure. |<b>Název databázového serveru</b>: Určuje název hello hello serveru, na které hello databáze běží.<br/><ul>V případě Azure SQL Database zadejte název serveru hello, aby se vygenerovala. Obvykle má hello formuláře  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>V případě systému SQL server, který je hostitelem je Azure virtuálního počítače zadejte *tcp:&lt;název DNS virtuálního počítače&gt;, 1433*</ul><br/><b>Název databáze </b>: Určuje název hello hello databáze na serveru hello. <br/><br/><b>Název uživatelského účtu serveru</b>: Určuje uživatelské jméno pro účet, který má přístupová oprávnění pro databázi hello. <br/><br/><b>Heslo uživatelského účtu serveru</b>: Určuje hello heslo pro uživatelský účet hello.<br/><br/><b>Přijměte všechny certifikát serveru</b>: pomocí této možnosti (méně bezpečné), pokud chcete, aby tooskip kontrola certifikát webu hello před číst vaše data.<br/><br/><b>Databázový dotaz</b>: Zadejte příkaz SQL, který popisuje hello data, která chcete tooread. |
| Místní databáze SQL |Čte data, která je uložená v místní databázi. |<b>Brána data gateway</b>: Určuje název hello hello Brána pro správu dat nainstalovat do počítače, kde má přístup k databázi SQL serveru. Informace o nastavení hello brány najdete v tématu [provést pokročilou analýzu pomocí Azure Machine Learning pomocí dat z SQL serveru místní](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Název databázového serveru</b>: Určuje název hello hello serveru, na které hello databáze běží.<br/><br/><b>Název databáze </b>: Určuje název hello hello databáze na serveru hello. <br/><br/><b>Název uživatelského účtu serveru</b>: Určuje uživatelské jméno pro účet, který má přístupová oprávnění pro databázi hello. <br/><br/><b>Uživatelské jméno a heslo</b>: klikněte na tlačítko <b>zadejte hodnoty</b> tooenter svoje přihlašovací údaje databáze. Můžete použít integrované ověřování systému Windows nebo ověřování systému SQL Server v závislosti na konfiguraci vaší místní SQL Server.<br/><br/><b>Databázový dotaz</b>: Zadejte příkaz SQL, který popisuje hello data, která chcete tooread. |
| Tabulky Azure |Čte data z hello služby Table v Azure Storage.<br/><br/>Pokud si přečíst velké objemy dat zřídka, použijte hello služby Azure Table. Poskytuje flexibilní, nerelačních (NoSQL), řešení nesmírně škálovatelná služba, nenákladné a vysoce dostupné úložiště. |Hello možnosti v hello **importovat Data** změnit v závislosti na tom, zda přistupujete k informací veřejného nebo soukromého skladování účet, který vyžaduje přihlašovací údaje. To je dáno hello <b>typ ověřování</b> která může mít hodnotu "PublicOrSAS" nebo "Account", z nichž každá má svou vlastní sadu parametrů. <br/><br/><b>Veřejné nebo sdíleného přístupového podpisu (SAS) URI</b>: hello parametry jsou:<br/><br/><ul><b>Tabulka URI</b>: Určuje hello veřejné nebo SAS adresa URL pro tabulku hello.<br/><br/><b>Určuje hello tooscan řádků pro názvy vlastností</b>: hello hodnoty jsou <i>TopN</i> tooscan hello zadaný počet řádků, nebo <i>ScanAll</i> tooget všechny řádky v tabulce hello. <br/><br/>Pokud jsou hello data homogenní a předvídatelné, je vhodné vybrat *TopN* a zadejte číslo pro N. Pro rozsáhlé tabulky to může způsobit rychlejší doby čtení.<br/><br/>Pokud jsou hello data strukturovaná se sadami vlastností, které se liší na základě hloubky hello a pozice hello tabulky, zvolte hello *ScanAll* možnost tooscan všechny řádky. Tím se zajistí hello integrity výsledné vlastnost a převod metadat.<br/><br/></ul><b>Účet úložiště privátního</b>: hello parametry jsou: <br/><br/><ul><b>Název účtu</b>: Určuje název hello hello účtu, který obsahuje tooread tabulky hello.<br/><br/><b>Klíč účtu</b>: Určuje klíč úložiště hello spojené s účtem hello.<br/><br/><b>Název tabulky</b> : Určuje název hello hello tabulku, která obsahuje hello data tooread.<br/><br/><b>Řádky tooscan pro názvy vlastností</b>: hello hodnoty jsou <i>TopN</i> tooscan hello zadaný počet řádků, nebo <i>ScanAll</i> tooget všechny řádky v tabulce hello.<br/><br/>Pokud jsou hello data homogenní a předvídatelné, doporučujeme vybrat *TopN* a zadejte číslo pro N. Pro rozsáhlé tabulky to může způsobit rychlejší doby čtení.<br/><br/>Pokud jsou hello data strukturovaná se sadami vlastností, které se liší na základě hloubky hello a pozice hello tabulky, zvolte hello *ScanAll* možnost tooscan všechny řádky. Tím se zajistí hello integrity výsledné vlastnost a převod metadat.<br/><br/> |
| Azure Blob Storage |Čte data uložená v hello služby objektů Blob v Azure Storage, včetně obrázků, nestrukturovaných textu nebo binárních dat.<br/><br/>Můžete použít hello objektu Blob služby toopublicly zveřejněte data nebo data aplikací tooprivately úložiště. Data můžete přístup odkudkoli pomocí připojení HTTP a HTTPS. |Hello možnosti v hello **importovat Data** modulu změnit v závislosti na tom, zda přistupujete k informací veřejného nebo soukromého skladování účet, který vyžaduje přihlašovací údaje. To je dáno hello <b>typ ověřování</b> která může mít hodnotu "PublicOrSAS" nebo "Účet".<br/><br/><b>Veřejné nebo sdíleného přístupového podpisu (SAS) URI</b>: hello parametry jsou:<br/><br/><ul><b>Identifikátor URI</b>: Určuje hello veřejné nebo SAS adresa URL pro objekt blob úložiště hello.<br/><br/><b>Formát souboru</b>: Určuje formát hello hello dat v hello služby objektů Blob. Hello podporované formáty jsou sdíleného svazku clusteru, TSV a ARFF.<br/><br/></ul><b>Účet úložiště privátního</b>: hello parametry jsou: <br/><br/><ul><b>Název účtu</b>: Určuje název hello hello účtu, který obsahuje objekt blob hello chcete tooread.<br/><br/><b>Klíč účtu</b>: Určuje klíč úložiště hello spojené s účtem hello.<br/><br/><b>Cesta toocontainer, adresáře nebo objekt blob </b> : Určuje název hello hello objektů blob, který obsahuje hello data tooread.<br/><br/><b>Formát souboru BLOB</b>: Určuje formát hello hello dat ve službě blob hello. Hello podporované datové formáty jsou sdíleného svazku clusteru, TSV, ARFF, CSV s zadané kódování a Excel. <br/><br/><ul>Pokud hello formátu CSV nebo TSV, být tooindicate se, zda text hello soubor obsahuje řádek záhlaví.<br/><br/>Můžete použít data z Excelu. možnost tooread hello ze sešitů aplikace Excel. V hello <i>formát dat aplikace Excel</i> možnost, zda se jestli hello dat je v oblasti sešitu aplikace Excel, nebo v tabulce aplikace Excel. V hello <i>listu aplikace Excel nebo vložené tabulky </i>, zadejte název hello hello list nebo tabulka, která chcete tooread z.</ul><br/> |
| Zprostředkovatel dat informačního kanálu |Čte data z podporovaných informačního kanálu zprostředkovatele. Aktuálně jedinou formátu Open Data Protocol (OData) hello je podporována. |<b>Data obsahu typu</b>: Určuje formát hello OData.<br/><br/><b>Adresa URL zdroje</b>: Určuje úplnou adresu URL hello pro hello datový kanál. <br/>Například hello následující adresu URL čtení z ukázková databáze Northwind hello: http://services.odata.org/northwind/northwind.svc/ |

## <a name="next-steps"></a>Další kroky

[Nasazení webové služby Azure ML, které používají Data importu a exportu dat moduly](machine-learning-web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
