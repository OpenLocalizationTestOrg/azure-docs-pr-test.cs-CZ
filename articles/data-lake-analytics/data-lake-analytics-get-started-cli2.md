---
title: "aaaGet začít s Azure Data Lake Analytics pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak toouse hello 2.0 rozhraní příkazového řádku Azure toocreate účet Data Lake Analytics, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy hello. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: jgao
ms.openlocfilehash: c4e91c0d3526e4932c2948c0a326d4cedc985791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a>Začínáme s Azure Data Lake Analytics s využitím rozhraní Azure CLI 2.0
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

V tomto kurzu vyvinete úlohu, která načte soubor hodnot oddělených tabulátory (TSV) a převede ho na soubor hodnot oddělených čárkami (CSV). toogo prostřednictvím hello stejný kurz pomocí jiné podporované nástroje použijte hello rozevíracího seznamu nahoře hello této části.

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít hello následující položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI 2.0**. Viz téma [Instalace a konfigurace rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

toolog v tooyour předplatné Azure:

```
azurecli
az login
```

Jsou tooa toobrowse požadovanou adresu URL a zadání ověřovacího kódu.  A pak postupujte podle pokynů tooenter hello přihlašovacích údajů.

Po přihlášení hello přihlášení příkaz vypíše vašich předplatných.

toouse konkrétní předplatné:

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics
Je nutné, abyste před spuštěním jakékoli úlohy měli účet Data Lake Analytics. toocreate účtu Data Lake Analytics, je nutné zadat hello následující položky:

* **Skupina prostředků Azure**. Účet Data Lake Analytics se musí vytvořit v rámci Skupiny prostředků Azure. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) vám umožní toowork s hello prostředky v aplikaci jako se skupinou. Můžete nasadit, aktualizovat nebo odstranit všechny prostředky hello pro vaši aplikaci v rámci jediné koordinované operace.  

toolist hello existující skupiny prostředků v rámci svého předplatného:

```
az group list
```

toocreate novou skupinu prostředků:

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Název účtu Data Lake Analytics**. Každý účtu Data Lake Analytics má název.
* **Umístění**. Použijte jeden z hello datových center Azure podporující Data Lake Analytics.
* **Výchozí účet Data Lake Store:** Každý účet Data Lake Analytics má výchozí účet Data Lake Store.

toolist hello existující účet Data Lake Store:

```
az dls account list
```

toocreate nový účet Data Lake Store:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Použijte následující syntaxi toocreate účtu Data Lake Analytics hello:

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Po vytvoření účtu, můžete použít následující příkazy toolist hello účty hello a zobrazit podrobnosti o účtu:

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-toodata-lake-store"></a>Nahrání dat tooData Lake Store
V tomto kurzu zpracujete několik protokolů hledání.  Protokol hledání Hello mohou být uloženy v Data Lake store nebo úložiště objektů Blob v Azure.

Hello portál Azure poskytuje uživatelské rozhraní pro kopírování některých ukázkových dat soubory toohello výchozí účtu Data Lake Store, včetně souboru protokolu hledání. V tématu [Příprava zdrojových dat](data-lake-analytics-get-started-portal.md) tooupload hello data toohello výchozí účet Data Lake Store.

tooupload soubory pomocí rozhraní příkazového řádku 2.0, použijte hello následující příkazy:

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics má také přístup k úložišti objektů Azure Blob.  Odesílání dat tooAzure Blob storage, najdete v části [hello pomocí rozhraní příkazového řádku Azure s Azure Storage](../storage/common/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Odesílání úloh Data Lake Analytics
Hello úloh Data Lake Analytics se píšou v hello jazykem U-SQL. toolearn Další informace o U-SQL, najdete v části [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md) a [eence jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**toocreate skriptu úlohy Data Lake Analytics**

Vytvořte textový soubor s následující skript U-SQL a uložte hello textového souboru tooyour pracovní stanice:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

Tento skript U-SQL přečte hello zdrojový datový soubor pomocí **Extractors.Tsv()**a poté vytvoří soubor csv pomocí **Outputters.Csv()**.

Neupravujte dvě cesty hello, pokud zkopírujete hello zdrojového souboru do jiného umístění.  Data Lake Analytics vytvoří výstupní složku hello, pokud neexistuje.

Je jednodušší toouse relativní cesty pro soubory uložené v výchozí účty Data Lake Store. Můžete také použít absolutní cesty.  Například:

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

Je nutné použít absolutní cesty tooaccess souborům v propojených účtech úložiště.  Tady je syntax Hello případě souborů uložených v propojeném účtu Azure Storage:

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Kontejner Azure Blob s veřejnými objekty blob není podporován.      
> Kontejner Azure Blob s veřejnými kontejnery není podporován.      
>

**toosubmit úlohy**

Pomocí následující syntaxe toosubmit úlohu hello.

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Například:

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**toolist úlohy a zobrazit podrobnosti úlohy**

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**toocancel úlohy**

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>Načtení výsledků úlohy

Po dokončení úlohy můžete použít následující příkazy toolist hello výstupní soubory hello a stahování souborů hello:

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

Například:

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="pipelines-and-recurrences"></a>Kanály a opakování

**Získání informací o kanálech a opakováních**

Použití hello `az dla job pipeline` příkazy toosee hello kanálu informace dříve odeslání úlohy.

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

Použití hello `az dla job recurrence` příkazy toosee hello opakování informace pro dříve odeslaný úlohy.

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a>Další kroky

* toosee hello Data Lake Analytics rozhraní příkazového řádku 2.0 referenčním dokumentu, najdete v části [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).
* toosee hello Data Lake Store rozhraní příkazového řádku 2.0 referenčním dokumentu, najdete v části [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).
* toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
