---
title: "aaaManage Azure Data Lake Analytics pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toomanage účtů Data Lake Analytics, datových zdrojů, úloh a uživatele pomocí rozhraní příkazového řádku Azure"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 0af1f89080739b39f6980989b7694734cc135715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Správa Azure Data Lake Analytics pomocí rozhraní příkazového řádku Azure (CLI)
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Zjistěte, jak hello toomanage účtů Azure Data Lake Analytics, zdroje dat, uživatelů a úloh pomocí rozhraní příkazového řádku Azure. témata týkající se řízení toosee pomocí jiných nástrojů, klikněte na výběr karty hello výše.


**Požadavky**

Než začnete tento kurz, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Rozhraní příkazového řádku Azure**. Viz téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).
  * Stáhněte a nainstalujte hello **předběžné verze** [nástrojů příkazového řádku Azure](https://github.com/MicrosoftBigData/AzureDataLake/releases) v pořadí toocomplete tuto ukázku.
* **Ověřování**pomocí hello následující příkaz:
  
        azure login
    Další informace týkající se ověřování pomocí pracovního nebo školního účtu najdete v tématu [připojit tooan předplatného Azure z rozhraní příkazového řádku Azure hello](../xplat-cli-connect.md).
* **Přepínač režimu Azure Resource Manager toohello**pomocí hello následující příkaz:
  
        azure config mode arm

**Data Lake Store a Data Lake Analytics příkazy serveru toolist hello:**

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a>Správa účtů
Před spuštěním jakékoli úlohy Data Lake Analytics, musí mít účet Data Lake Analytics. Na rozdíl od Azure HDInsight nemáte platí pro účet Analytics když úlohu není spuštěn.  Platíte jenom hello dobu, kdy je spuštěná úloha.  Další informace najdete v tématu [přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Vytvoření účtů
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a>Aktualizace účtů
Hello následující příkaz aktualizuje vlastnosti hello stávajícího účtu Data Lake Analytics

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a>Výpis účtů
Účty seznamu Data Lake Analytics 

    azure datalake analytics account list

Seznam Data Lake Analytics účtů v rámci určité skupiny zdrojů

    azure datalake analytics account list -g "<Azure Resource Group Name>"

Podrobnosti o konkrétní účet Data Lake Analytics

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a>Odstranění účtů Data Lake Analytics
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Správa zdrojů dat účtu
Data Lake Analytics teď podporuje hello následující zdroje dat:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Když vytvoříte účet Analytics, je třeba určit účet Azure Data Lake Storage účet toobe hello výchozí úložiště. Hello výchozí účet úložiště ADL slouží metadata toostore úlohy a úlohy protokoly auditu. Po vytvoření účtu Analytics můžete přidat další účty Data Lake Storage a účet úložiště Azure. 

### <a name="find-hello-default-adl-storage-account"></a>Najít hello výchozí účet úložiště ADL
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

Hodnota Hello je uveden v části vlastnosti: datalakeStoreAccount:name.

### <a name="add-additional-azure-blob-storage-accounts"></a>Přidat další účty úložiště objektů Blob v Azure
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> Jsou podporovány pouze objekt Blob úložiště krátké názvy.  Nepoužívejte plně kvalifikovaný název domény, například "myblob.blob.core.windows.net".
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a>Přidat další účty Data Lake Store
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

[-d] se tooindicate volitelný přepínač, jestli je hello Data Lake přidávané hello výchozí účet Data Lake. 

### <a name="update-existing-data-source"></a>Aktualizovat stávající zdroj dat
tooset existující Data Lake Store účtu toobe hello výchozím nastavení:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

tooupdate existující klíč účtu úložiště objektů Blob:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a>Seznam zdrojů dat:
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![Data Lake Analytics seznamu zdroj dat](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Odstraňte zdroje dat:
toodelete účtu Data Lake Store:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

toodelete účtu úložiště Blob:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a>Správa úloh
Abyste mohli vytvořit úlohu, musí mít účet Data Lake Analytics.  Další informace najdete v tématu [Správa Data Lake Analytics účty](#manage-accounts).

### <a name="list-jobs"></a>Seznam úloh
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![Data Lake Analytics seznamu zdroj dat](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Získání podrobností o úlohách
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a>Odesílání úloh
> [!NOTE]
> Hello výchozí prioritu úlohy je 1000 a hello výchozí stupně paralelního zpracování v rámci úlohy je 1.
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a>Zrušení úlohy
Použijte hello seznamu příkaz toofind hello id úlohy a potom použijte úlohu hello toocancel Storno.

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a>Správa katalogu
katalog Hello U-SQL je použité toostructure dat a kódu, takže může být sdílen skriptů U-SQL. katalog Hello umožňuje hello nejvyšší možný výkon s daty v Azure Data Lake. Další informace najdete v tématu [Použití katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-catalog-items"></a>Seznam položek katalogu
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

typy Hello obsahují databáze, schéma, sestavení, externí zdroj dat, tabulka, funkci vracející tabulku nebo statistiky tabulky.

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a>Použití skupin ARM
Aplikace obvykle obsahují celou řadu součástí, například webovou aplikaci, databázi, databázový server, úložiště a služby třetích stran. Azure Resource Manager (ARM) umožňuje toowork s hello prostředky v aplikaci jako skupina, která označují tooas skupiny prostředků Azure. Můžete nasadit, aktualizovat, sledovat nebo odstranit všechny prostředky hello pro vaši aplikaci v rámci jediné koordinované operace. Pro nasazení použijete šablonu a tato šablona může fungovat v různých prostředích, jako je testovací, přípravné nebo produkční prostředí. Můžete zpřehlednit fakturaci pro vaši organizaci zobrazením hello zahrnuté náklady pro celou skupinu hello. Další informace najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md). 

Služba Data Lake Analytics může zahrnovat hello následující součásti:

* Účet Azure Data Lake Analytics
* Vyžaduje výchozí účet Azure Data Lake Storage
* Účty úložiště další Azure Data Lake
* Další účty Azure Storage

Můžete vytvořit všechny tyto součásti v rámci jedné skupiny toomake ARM je snazší toomanage.

![Účet Azure Data Lake Analytics a úložiště](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Účet Data Lake Analytics a hello závislého úložiště účty musí být umístěny v hello stejné datové centrum Azure.
skupiny ARM Hello ale může být umístěno v různých datových center.  

## <a name="see-also"></a>Viz také
* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Začínáme se službou Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md)
* [Správa Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md)
* [Sledování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

