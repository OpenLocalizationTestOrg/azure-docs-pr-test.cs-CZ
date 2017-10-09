---
title: aaaCopy dat z Azure BLOB Storage do Data Lake Store | Microsoft Docs
description: "Použít AdlCopy nástroj toocopy dat z Azure úložiště objektů BLOB tooData Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: a3d4172eaefe7395cdef2fff72691bd70f642b78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-from-azure-storage-blobs-toodata-lake-store"></a>Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store
> [!div class="op_single_selector"]
> * [Pomocí DistCp](data-lake-store-copy-data-wasb-distcp.md)
> * [Pomocí AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake Store poskytuje nástroj příkazového řádku, [AdlCopy](http://aka.ms/downloadadlcopy), toocopy data z hello následující zdroje:

* Z Azure úložiště objektů BLOB do Data Lake Store. Nelze použít AdlCopy toocopy dat z Data Lake Store tooAzure úložiště objektů BLOB.
* Mezi dvěma účty Azure Data Lake Store.

Navíc můžete použít nástroj AdlCopy hello ve dvou různých režimech:

* **Samostatné**, kde nástroj hello používá Data Lake Store prostředky tooperform hello úloh.
* **Pomocí účtu Data Lake Analytics**, kde hello jednotky přiřazen účet Data Lake Analytics tooyour jsou použité tooperform hello kopírování. Můžete chtít toouse tuto možnost při prohlížení tooperform hello kopírování úlohy předvídatelný způsobem.

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Objektů BLOB služby Azure Storage** kontejneru určitými daty.
* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Účtu Azure Data Lake Analytics (volitelné)** -najdete v části [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) pokyny, jak toocreate Data Lake ukládání účtu.
* **Nástroj AdlCopy**. Nainstalujte nástroj AdlCopy hello z [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-hello-adlcopy-tool"></a>Syntaxe nástroje AdlCopy hello
Pomocí následující syntaxe toowork nástrojem AdlCopy hello hello

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

Parametry Hello v syntaxi hello jsou následující:

| Možnost | Popis |
| --- | --- |
| Zdroj |Určuje umístění hello hello zdroje dat v objektu blob úložiště Azure hello. Hello zdroj může být kontejner objektů blob, objekt blob nebo jiný účet Data Lake Store. |
| Cíle |Určuje toocopy cílové hello Data Lake Store k. |
| SourceKey |Určuje hello úložiště přístupový klíč pro zdrojový objekt blob úložiště Azure hello. To je potřeba, pouze pokud je zdroj hello kontejner objektů blob nebo objekt blob. |
| Účet |**Volitelné**. Použijte, pokud chcete úlohu kopírování toouse Azure Data Lake Analytics účet toorun hello. Pokud použijete možnost /Account hello v syntaxi hello ale nezadávejte účet Data Lake Analytics, AdlCopy používá výchozí účet toorun hello úlohy. Navíc pokud použijete tuto možnost, musíte přidat zdroj hello (Azure Storage Blob) a cíl (Azure Data Lake Store) jako zdroje dat pro váš účet Data Lake Analytics. |
| Jednotky |Určuje hello počet jednotek Data Lake Analytics, které se použijí pro úlohu kopírování hello. Tato možnost je povinná, pokud používáte hello **/účet** možnost účet Data Lake Analytics toospecify hello. |
| vzor |Určuje vzor regulárního výrazu, která určuje, které toocopy objektů BLOB nebo soubory. AdlCopy používá odpovídající malá a velká písmena. výchozím způsobem Hello používá při žádné vzor je zadán, je toocopy všechny položky. Zadání více vzorů souborů není podporováno. |

## <a name="use-adlcopy-as-standalone-toocopy-data-from-an-azure-storage-blob"></a>Použít AdlCopy (jako samostatná) toocopy data z objektu blob Azure Storage
1. Otevřete příkazový řádek a přejděte toohello directory AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.
2. Spusťte následující příkaz toocopy hello konkrétní objekt blob z hello zdrojový kontejner tooa Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Například:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] Hello syntaxi výše určuje hello souboru toobe zkopírovaný tooa složku v účtu Data Lake Store hello. Nástroj AdlCopy vytvoří složku, pokud název hello zadaná složka neexistuje.

    Bude výzvami tooenter hello přihlašovací údaje pro hello předplatné Azure, ve kterém máte účtu Data Lake Store. Zobrazí se výstup podobný toohello následující:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. Všechny objekty BLOB hello můžete také zkopírovat z jednoho kontejneru typu toohello Data Lake Store účtu pomocí hello následující příkaz:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Například:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>Otázky výkonu

Pokud kopírujete z účtu úložiště objektů Blob Azure, můžete být omezeny během kopírování na straně úložiště objektů blob hello. To způsobí snížení výkonu hello kopírování úlohy. toolearn Další informace o omezení hello Azure Blob Storage, najdete v části omezení Azure Storage v [předplatného Azure a omezení služby](../azure-subscription-service-limits.md).

## <a name="use-adlcopy-as-standalone-toocopy-data-from-another-data-lake-store-account"></a>Použít data toocopy AdlCopy (jako samostatná) z jiného účtu Data Lake Store
Můžete také použít AdlCopy toocopy data mezi dvěma účty Data Lake Store.

1. Otevřete příkazový řádek a přejděte toohello directory AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.
2. Spusťte následující příkaz toocopy hello konkrétní soubor z jednoho tooanother účet Data Lake Store.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Například:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > Hello syntaxi výše určuje hello souboru toobe zkopírovaný tooa složku v účtu Data Lake Store cílové hello. Nástroj AdlCopy vytvoří složku, pokud název hello zadaná složka neexistuje.
   >
   >

    Bude výzvami tooenter hello přihlašovací údaje pro hello předplatné Azure, ve kterém máte účtu Data Lake Store. Zobrazí se výstup podobný toohello následující:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. Hello následující příkaz zkopíruje všechny soubory ze složky ve složce tooa hello zdroje Data Lake Store účtu v cílovém umístění hello účtu Data Lake Store.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>Otázky výkonu

Při použití AdlCopy jako samostatný nástroj, hello kopírování se spouští na sdílené, spravované prostředky Azure. Hello výkonu, které může dojít v tomto prostředí závisí na zatížení systému a dostupné prostředky. Tento režim je nejvhodnější pro malé přenosy na základě ad hoc. Žádné parametry potřebovat toobe přizpůsobená při použití AdlCopy jako samostatný nástroj.

## <a name="use-adlcopy-with-data-lake-analytics-account-toocopy-data"></a>Použít data toocopy AdlCopy (s účtem Data Lake Analytics)
Můžete také vaše Data Lake Analytics účet toorun hello AdlCopy úlohy toocopy dat z Azure úložiště objektů BLOB tooData Lake Store. Obvykle použijete tuto možnost při toobe hello data přesunout je v dosahu hello gigabajtech a terabajtů a chcete, aby propustnost lepší a předvídatelný výkon.

toouse vašeho účtu Data Lake Analytics pomocí AdlCopy toocopy z objektu Blob Azure Storage, zdroj hello (Azure Storage Blob) je nutné přidat jako zdroj dat pro váš účet Data Lake Analytics. Pokyny k přidání účtu Data Lake Analytics tooyour další datové zdroje najdete v tématu [zdrojů dat účtu Správa Data Lake Analytics](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).

> [!NOTE]
> Pokud kopírujete z účtu Azure Data Lake Store jako zdroj hello pomocí účtu Data Lake Analytics, není nutné tooassociate hello účtu Data Lake Store s hello účtu Data Lake Analytics. Hello požadavek tooassociate hello zdrojové úložiště s hello účtu Data Lake Analytics je pouze v případě, že zdroj hello je účet úložiště Azure.
>
>

Spusťte následující příkaz toocopy z účtu Data Lake Store tooa objektu blob úložiště Azure pomocí účtu Data Lake Analytics hello:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Například:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

Podobně spusťte následující příkaz toocopy z účtu Data Lake Store tooa objektu blob úložiště Azure pomocí účtu Data Lake Analytics hello:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>Otázky výkonu

Při kopírování dat v rozsahu hello terabajtů, AdlCopy pomocí účtu Azure Data Lake Analytics poskytuje lepší a více předvídatelný výkon. Hello parametr, který by měl být přizpůsobená je hello počet toouse Azure Data Lake Analytics jednotky pro úlohu kopírování hello. Zvýšením počtu hello jednotek zvýší hello výkon vaší úlohu kopírování. Každý soubor toobe zkopírovat, můžete použít maximální jednu jednotku. Určení další jednotky než hello počet kopírované soubory nebude zvýšit výkon.

## <a name="use-adlcopy-toocopy-data-using-pattern-matching"></a>Použít data toocopy AdlCopy použití porovnávání vzorů
V této části se dozvíte, jak toouse AdlCopy toocopy dat ze zdroje (v našem příkladu níže používáme Azure Storage Blob) účtu Data Lake Store cílové tooa použití porovnávání vzorů. Například můžete hello kroků toocopy všechny soubory s příponou CSV z hello zdroj blob toohello cíl.

1. Otevřete příkazový řádek a přejděte toohello directory AdlCopy nainstalovanou, obvykle `%HOMEPATH%\Documents\adlcopy`.
2. Spusťte následující příkaz toocopy hello všechny soubory s příponou *.csv, z konkrétní objekt blob z hello zdrojový kontejner tooa Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Například:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Fakturace
* Pokud použijete nástroj AdlCopy hello jako samostatná, které bude platit pro odchozí náklady na přesunutí hello data, pokud není zdroj hello účet úložiště Azure ve stejné oblasti jako hello Data Lake Store.
* Pokud použijete nástroj AdlCopy hello s vaše Data Lake Analytics účet, standardní [Data Lake Analytics fakturace sazby](https://azure.microsoft.com/pricing/details/data-lake-analytics/) budou platit.

## <a name="considerations-for-using-adlcopy"></a>Důležité informace týkající se použití AdlCopy
* AdlCopy (pro verzi 1.0.5), podporuje kopírování dat ze zdrojů, které se souhrnně mít víc než tisíce souborů a složek. Ale pokud dojde k potížím kopírování velké datové sady, můžete distribuovat hello soubory a složky do jiné podřízené složky a hello cesta toothose dílčí složky místo toho použít jako zdroj hello.

## <a name="performance-considerations-for-using-adlcopy"></a>Důležité informace o výkonu pro používání AdlCopy

AdlCopy podporuje kopírování dat obsahující tisíce souborů a složek. Ale pokud dojde k potížím kopírování velké datové sady, můžete distribuovat hello soubory a složky do menší podsložky. AdlCopy bylo vytvořeno pro ad hoc kopie. Pokud se pokoušíte toocopy dat pravidelně, měli byste zvážit použití [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) poskytuje úplné správy kolem operace kopírování hello.

## <a name="release-notes"></a>Poznámky k verzi
* 1.0.13 - li zkopírovat data toohello příkazy stejný účet Azure Data Lake Store napříč více adlcopy tooreenter svoje přihlašovací údaje pro každou spustit už nepotřebujete. Adlcopy nyní v mezipaměti tyto informace napříč více spustí.

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
