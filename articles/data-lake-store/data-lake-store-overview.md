---
title: aaaOverview Azure Data Lake Store | Microsoft Docs
description: "Co je Azure Data Lake Store a hello hodnotu, kterou poskytuje oproti jiným úložištím dat"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b3475057-9427-4492-a3af-25a802a23a79
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 5a60a6b86a51c44647cf4ee168fb333d1c37b1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-data-lake-store"></a>Přehled Azure Data Lake Store
Azure Data Lake Store je celopodnikové, flexibilně škálovatelné úložiště pro analytické úlohy s velkými objemy dat. Azure Data Lake umožňuje toocapture data jakékoli velikosti, typu a rychlosti příjmu do jediného místa pro provozní a zjišťovací analýzy.

> [!TIP]
> Použití hello [Data Lake Store studijní](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) toostart zkoumat hello služby Azure Data Lake Store.
> 
> 

Azure Data Lake Store je přístupná z Hadoop (dostupné s clusterem HDInsight) pomocí hello rozhraní REST API webhdfs, které kompatibilní. Je speciálně navrženého tooenable analýzy hello uložená data a je přizpůsobená pro scénáře analýzy dat výkonu. Předinstalované hello, obsahuje všechny funkce hello podnikové úrovni – zabezpečení, spravovatelnosti, škálovatelnosti, spolehlivosti a dostupnosti – pro případy reálného podnikového použití nezbytné.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Mezi klíčové funkce hello hello Azure Data Lake patří následující hello.

### <a name="built-for-hadoop"></a>Sestaveno pro Hadoop
Hello Azure Data Lake store je systém souborů Apache Hadoop kompatibilní s Hadoop Distributed File System (HDFS) a pracuje s ekosystémem Hadoop hello.  Vaše stávající aplikace HDInsight nebo služby, které používají hello rozhraní API webhdfs, které lze snadno integrovat s Data Lake Store. Data Lake Store taky aplikacím zpřístupňuje rozhraní REST kompatibilní s WebHDFS.

Data uložená v Data Lake Store se dají snadno analyzovat pomocí analytických rámců Hadoop, jako je například MapReduce nebo Hive. Clustery Microsoft Azure HDInsight můžete zřídit a nakonfigurovat toodirectly přístup k datům uloženým v Data Lake Store.

### <a name="unlimited-storage-petabyte-files"></a>Neomezené úložiště, petabajtové soubory
Služba Azure Data Lake Store poskytuje neomezené úložiště a je vhodná pro ukládání nejrůznějších dat k analýze. Neuplatňuje žádné omezení velikosti účtů, velikosti souborů nebo hello množství dat, které můžou být uložené v data lake. Jednotlivé soubory rozsahu kilobajtů toopetabytes velikost, takže je služba skvělou volbou toostore libovolného typu dat.. Data se ukládají spolehlivě tím, že více kopií a není omezen na hello doba, pro které hello data se uloží v hello data lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Optimalizace výkonu pro analýzu velkých objemů dat
Azure Data Lake Store je sestavena pro spouštění rozsáhlých analytických systémů, které vyžadují obrovskou propustnost tooquery a analýze velkých objemů dat. Hello data lake šíří části souborů přes počet jednotlivých serverů úložiště. Tím se zlepšuje hello číst propustnost při čtení souboru hello paralelně pro provádění analýz dat..

### <a name="enterprise-ready-highly-available-and-secure"></a>Připraveno pro podniky: Vysoká dostupnost a zabezpečení
Azure Data Lake Store poskytuje dostupnost a spolehlivost odpovídající standardům odvětví. Vaše datové prostředky se ukládají odolným způsobem díky vytváření redundantních kopií tooguard před neočekávaným selháním. Podniky můžou v rámci svých řešení využít Azure Data Lake jako důležitou součást svojí stávající datové platformy.

Data Lake Store taky zajišťuje zabezpečení podnikové úrovni hello uložená data. Další informace najdete v tématu [Zabezpečení dat v Azure Data Lake Store](#DataLakeStoreSecurity).

### <a name="all-data"></a>Všechna data
Služba Azure Data Lake Store dokáže ukládat libovolná data v nativním formátu tak, jak jsou, bez nutnosti předchozí transformace. Data Lake Store nevyžadují toobe schéma definované před načtením dat hello zároveň je nechává toohello jednotlivé analytické framework toointerpret hello dat a definovat schéma během hello hello analýzy. Je možné toostore soubory libovolných velikostí a formátů umožňuje pro Data Lake Store toohandle strukturovaná, částečně strukturovaná i nestrukturovaná data.

Kontejnery na data Azure Data Lake Store jsou v podstatě složky a soubory. Můžete pracovat hello uložená data pomocí sady SDK, portálu Azure a prostředí Azure Powershell. Tak dlouho, dokud vložíte do úložiště hello používání těchto rozhraní a příslušných kontejnerů hello vaše data, můžete ukládat jakýkoli typ dat.. Data Lake Store neprovede žádné zvláštní zpracování dat na základě typu dat, který by závisel hello.

## <a name="DataLakeStoreSecurity"></a>Zabezpečení dat v Azure Data Lake Storu
Azure Data Lake Store využívá Azure Active Directory k ověřování a seznamy řízení přístupu (ACL) toomanage přístup tooyour data.

| Funkce | Popis |
| --- | --- |
| Authentication |Azure Data Lake Store se integruje s Azure Active Directory (AAD) pro správu identit a přístupu pro všechny hello data uložená v Azure Data Lake Store. V důsledku hello integraci, výhody Azure Data Lake všechny funkce AAD, včetně vícefaktorového ověřování, podmíněného přístupu, řízení přístupu na základě role, sledování využití aplikací, sledování a výstrah zabezpečení atd. Azure Data Lake Store podporuje hello protokol OAuth 2.0 pro ověřování s v rozhraní REST hello. |
| Řízení přístupu |Azure Data Lake Store zajišťuje řízení přístupu tím podporuje oprávnění ve stylu POSIX vystavené hello protokolu WebHDFS. Seznamy ACL v hello Data Lake Store Public Preview (hello aktuální verzi), může být povolena na hello kořenová složka, podsložky a jednotlivé soubory. Další informace o fungování seznamů řízení přístupu v souvislosti s Data Lake Storem najdete v tématu [Řízení přístupu v Data Lake Storu](data-lake-store-access-control.md). |
| Šifrování |Data Lake Store taky zajišťuje šifrování dat, který je uložený v účtu hello. Nastavení šifrování hello je zadat při vytváření účtu Data Lake Store. Můžete pokusit toohave vaše data zašifrovaná nebo zvolit žádné šifrování. Další informace o tom, tooprovide konfigurace související se šifrování, najdete v části [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md). |

Chcete toolearn více informací o zabezpečení dat v Data Lake Store. Postupujte podle níže uvedených odkazů hello.

* Pokyny najdete v části toosecure dat v Data Lake Store, [zabezpečení dat v Azure Data Lake Store](data-lake-store-secure-data.md).
* Dáváte přednost videu? [Přehrát toto video](https://mix.office.com/watch/1q2mgzh9nn5lx) na tom, jak toosecure data uložená v Data Lake Store.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Aplikace kompatibilní s Azure Data Lake Store
Azure Data Lake Store je kompatibilní s součástí typu nejvíce open source v ekosystému Hadoop hello. Výborně se taky integruje s jinými službami Azure. Díky tomu je služba Data Lake Store ideální volbou pro ukládání vašich dat. Postupujte podle hello najdete pod odkazy níže toolearn informace o tom, jak používat Data Lake Store službu se součástmi typu open source, jakož i jinými službami Azure.

* Seznam aplikací typu Open Source, které spolupracují se službou Azure Data Lake Store, najdete v tématu [Aplikace a služby kompatibilní s Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md).
* V tématu [integraci s jinými službami Azure](data-lake-store-integrate-with-other-services.md) toounderstand použití Data Lake Store pomocí jiných Azure services tooenable širší rozsah scénáře.
* V tématu [scénáře použití Data Lake Store](data-lake-store-data-scenarios.md) toolearn jak ukládat toouse Data Lake v scénáře, jako je příjem dat, zpracování dat, stahování dat a vizualizace dat.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Co je systém souborů Azure Data Lake Store (adl://)?
Data Lake Store je přístupná prostřednictvím nového systému souborů hello, hello AzureDataLakeFilesystem (adl: / /), v prostředích Hadoop (dostupné s clusterem HDInsight). Aplikace a služby, které používají adl: / /, můžou mít tootake výhod další optimalizace výkonu, které nejsou ve WebHDFS aktuálně dostupná. V důsledku toho Data Lake Store poskytuje hello flexibilitu tooeither využít nejlepší výkon hello s hello doporučená možnost použití adl: / / nebo zachovat stávající kód a trvalého toouse hello rozhraní API webhdfs, které přímo. Azure HDInsight plně využívá hello AzureDataLakeFilesystem tooprovide hello nejlepší výkon v Data Lake Store.

Přistupujete k datům v hello Data Lake Store pomocí `adl://<data_lake_store_name>.azuredatalakestore.net`. Další informace o tom, jak tooaccess hello data v hello Data Lake Store najdete v tématu [data uložena zobrazit vlastnosti hello](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Jak můžu začít používat Azure Data Lake Store?
V tématu [Začínáme s Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md), na tom, jak hello tooprovision Data Lake Store pomocí portálu Azure. Po zřízení Azure Data Lake, dozvíte, jak nabídky velkých objemů dat toouse například Azure Data Lake Analytics nebo Azure HDInsight s Data Lake Store. Můžete také vytvořit účet Azure Data Lake Store toocreate aplikace .NET a provádět operace, jako je například nahrávání dat, stahování dat atd.

* [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Začínáme s Azure Data Lake Storem pomocí sady .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="data-lake-store-videos"></a>Videa Data Lake Store
Pokud dáváte přednost, sledování videa toolearn, Data Lake Store nabízí videa pro celou řadu funkcí.

* [Vytvoření účtu Azure Data Lake Store](https://mix.office.com/watch/1k1cycy4l4gen)
* [Použití Průzkumníku dat tooManage hello dat v Azure Data Lake Store](https://mix.office.com/watch/icletrxrh6pc)
* [Připojení Azure Data Lake Analytics tooAzure Data Lake Store](https://mix.office.com/watch/qwji0dc9rx9k)
* [Přístup k Azure Data Lake Storu prostřednictvím Data Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)
* [Připojení Azure HDInsight tooAzure Data Lake Store](https://mix.office.com/watch/l93xri2yhtp2)
* [Přístup k Azure Data Lake Storu prostřednictvím aplikací Hive a Pig](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Použití DistCp (Hadoop Distributed Copy) toocopy data tooand z Azure Data Lake Store](https://mix.office.com/watch/1liuojvdx6sie)
* [Použití Apache Sqoop toomove dat mezi relačními zdroji a Azure Data Lake Store](https://mix.office.com/watch/1butcdjxmu114)
* [Orchestrace dat pomocí Azure Data Factory pro Azure Data Lake Store](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Zabezpečení dat v hello Azure Data Lake Store](https://mix.office.com/watch/1q2mgzh9nn5lx)

