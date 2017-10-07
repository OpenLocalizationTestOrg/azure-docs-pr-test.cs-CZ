---
title: "aaaMove tooor dat z Azure Blob Storage pomocí služby SSIS konektory | Microsoft Docs"
description: "Přesun dat tooor z Azure Blob Storage pomocí služby SSIS konektory."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 15068af1c69f11e74e109ee5ae2b9f1a674ea388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooor-from-azure-blob-storage-using-ssis-connectors"></a>Přesun dat tooor z Azure Blob Storage pomocí služby SSIS konektory
Hello [SQL Server Integration Services Feature Pack pro Azure](https://msdn.microsoft.com/library/mt146770.aspx) poskytuje součásti tooconnect tooAzure, přenos dat mezi Azure a místní zdroje dat a zpracování dat uložených v Azure.

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

Jakmile zákazníci byl přesunut místní data do cloudu hello, můžete přístup z jakékoli služby Azure tooleverage hello power úplné sady hello Azure technologií. Se může použít, například v Azure Machine Learning, nebo v clusteru služby HDInsight.

To je obvykle být hello prvním krokem pro hello [SQL](machine-learning-data-science-process-sql-walkthrough.md) a [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) návody.

Diskuzi o kanonický scénáře, které používají služby SSIS tooaccomplish obchodní potřebuje běžné v hybridní scénáře integrace data, najdete v části [to více s SQL Server Integration Services Feature Pack pro Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blogu.

> [!NOTE]
> Úložiště objektů blob tooAzure dokončení úvod, najdete v části příliš[základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a příliš[služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Požadavky
tooperform hello úlohy popsané v tomto článku, musíte mít předplatné Azure a účet úložiště Azure nastavit. Musíte vědět, vaše úložiště Azure účet účet a název klíče tooupload nebo stahování dat.

* tooset až **předplatného Azure**, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
* Pokyny k vytváření **účet úložiště** a získání účtu a klíč, najdete v části [účty Azure storage](../storage/common/storage-create-storage-account.md).

toouse hello **SSIS konektory**, je nutné stáhnout:

* **SQL Server 2014 nebo 2016 Standard (nebo novější)**: instalace zahrnuje SQL Server Integration Services.
* **Microsoft SQL Server 2014 nebo 2016 Integration Services Feature Pack pro Azure**: tyto soubory můžete stáhnout, respektive z hello [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) a [SQL serveru 2016 integrace Služby](https://www.microsoft.com/download/details.aspx?id=49492) stránky.

> [!NOTE]
> Služby SSIS je nainstalovaná se systémem SQL Server, ale není zahrnut ve verzi Express hello. Informace, na které aplikace jsou zahrnuty v různých edicích systému SQL Server najdete v tématu [edice serveru SQL](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)
> 
> 

Výukové materiály v SSIS, najdete v části [rukou na školení pro SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Informace o tom tooget až a spuštěná pomocí SISS toobuild jednoduchá extrakce, transformace a načítání (ETL) balíčky, najdete v tématu [SSIS kurz: vytvoření jednoduchého balíčku ETL](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Stáhnout NYC taxíkem datové sady
Hello příklad popsané tady použijte veřejně dostupné datové sady – hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu. Datová sada Hello se skládá z o 173 milionů taxíkem lyžovačku v NYC hello roku 2013. Existují dva typy dat: cestě podrobnosti dat a tarif data. Protože soubor pro každý měsíc, máme 24 soubory ve všech, z nichž každý je přibližně 2GB nekomprimovaným.

## <a name="upload-data-tooazure-blob-storage"></a>Nahrání dat tooAzure objektu blob úložiště
balíček z úložiště objektů blob tooAzure místní funkcí toomove dat pomocí hello SSIS, můžeme použít instanci hello [ **úloh nahrát objekt Blob Azure**](https://msdn.microsoft.com/library/mt146776.aspx), znázorněno zde:

![Konfigurace data-vědecké účely vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

Hello parametry, které hello používá úloh jsou popsány zde:

| Pole | Popis |
| --- | --- |
| **AzureStorageConnection** |Určuje existující Správce připojení úložiště Azure nebo vytvoří nový ten, který odkazuje tooan účtu úložiště Azure, který odkazuje toowhere hello blob soubory jsou hostovány. |
| **BlobContainer** |Určuje název hello hello kontejner objektů blob, který obsahovat hello nahrát soubory jako objekty BLOB. |
| **BlobDirectory** |Určuje adresář hello objekt blob se uloží soubor hello odeslán jako objekt blob bloku. Hello blob adresář je hierarchická struktura virtuální. Pokud objekt blob hello již existuje, it ia nahradit. |
| **LocalDirectory** |Určuje hello místní adresář, který obsahuje toobe soubory hello nahrát. |
| **Název souboru** |Určuje název filtru tooselect souborů pomocí vzoru hello zadaný název. Například MySheet\*.xls\* obsahuje soubory, jako jsou MySheet001.xls a MySheetABC.xlsx |
| **TimeRangeFrom/TimeRangeTo** |Určuje časový rozsah filtr. Soubory upravené po *TimeRangeFrom* a před *TimeRangeTo* jsou zahrnuty. |

> [!NOTE]
> Hello **AzureStorageConnection** přihlašovací údaje potřebovat toobe správné a hello **BlobContainer** musí existovat před pokusem o přenos hello.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Stahování dat z Azure blob storage
toodownload dat z Azure blob storage tooon místní úložiště s SSIS, použít instanci hello [úloh nahrát objekt Blob Azure](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Pokročilejší scénáře SSIS Azure
Sada funkcí služby SSIS Hello umožňuje složitější toky toobe zpracovávaných úloh balení společně. Například může hello blob datového kanálu přímo do clusteru HDInsight, jejíž výstup může stáhnout back tooa blob a potom tooon místní úložiště. SSIS můžete spustit úlohy Hive a Pig v clusteru HDInsight pomocí dalších SSIS konektory:

* skript Hive v Azure HDInsight, cluster s SSIS, použijte toorun [Azure HDInsight Hive úloh](https://msdn.microsoft.com/library/mt146771.aspx).
* skript Pig v Azure HDInsight, cluster s SSIS, použijte toorun [Azure HDInsight Pig úloh](https://msdn.microsoft.com/library/mt146781.aspx).

