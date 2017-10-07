---
title: "Data Factory kurz: první kanál dat | Microsoft Docs"
description: "Tento objekt pro vytváření dat Azure kurz ukazuje, jak toocreate a plán objekt pro vytváření dat, která zpracovává data pomocí Hive skript na clusteru Hadoop."
services: data-factory
keywords: Azure data factory kurz, cluster hadoop, hadoop hive
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.assetid: 81f36c76-6e78-4d93-a3f2-0317b413f1d0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: ed9c0ade4500d4ac1f7c2c2312c1fa675e0b1f02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-pipeline-tootransform-data-using-hadoop-cluster"></a>Kurz: Vytvoření vaší první dat tootransform kanálu pomocí clusteru Hadoop
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Šablona Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

V tomto kurzu vytvoříte vaše první pro vytváření dat Azure s datovém kanálu. kanál Hello transformuje vstupní data pomocí skriptu Hive v Azure HDInsight (Hadoop) clusteru tooproduce výstupní data.  

Tento článek obsahuje přehled a předpoklady pro kurz hello. Po dokončení hello požadavky, můžete provést pomocí jedné z následujících nástrojů nebo sady SDK hello kurzu hello: portál Azure, Visual Studio, prostředí PowerShell, šablony Resource Manageru, REST API. Vyberte jednu z možností hello v rozevíracím seznamu hello v hello začátku (nebo) odkazy na konci hello tohoto článku toodo hello kurzu pomocí jedné z těchto možností.    

## <a name="tutorial-overview"></a>Přehled kurzu
V tomto kurzu provedete hello následující kroky:

1. Vytvoření **objekt pro vytváření dat**. Objekt pro vytváření dat může obsahovat jeden nebo více datových kanálů, které přesunout a transformovat data. 

    V tomto kurzu vytvoříte jeden kanál v objektu pro vytváření dat hello. 
2. Vytvoření **kanálu**. Kanál může mít jednu nebo více aktivit (příklady: aktivita kopírování, aktivita HDInsight Hive). Tato ukázka používá hello aktivitu HDInsight Hive, která spouští skript Hive v clusteru HDInsight Hadoop. skript Hello nejprve vytvoří tabulku, která odkazuje na hello nezpracovaná webové protokolu data uložená v Azure blob storage a potom oddíly hello nezpracovaná data podle roku a měsíce.

    V tomto kurzu kanálu hello používá data tootransform Hive aktivit hello spuštěním dotazu Hive v clusteru Azure HDInsight Hadoop. 
3. Vytvoření **propojené služby**. Úložiště dat nebo výpočetní služby toohello objekt pro vytváření dat vytvoříte toolink propojené služby. Úložiště dat, jako je například Azure Storage obsahuje vstupní a výstupní data aktivity v kanálu hello. Výpočetní služba například cluster HDInsight Hadoop procesy nebo transformace dat.

    V tomto kurzu vytvoříte dvě propojené služby: **Azure Storage** a **Azure HDInsight**. Hello Azure Storage propojená služba propojuje účet úložiště Azure, který obsahuje vstupní a výstupní data hello toohello služby data factory. Azure HDInsight propojené služby odkazy, je použít tootransform toohello data objektu pro vytváření dat clusteru Azure HDInsight. 
3. Vytvoření vstupní a výstupní **datové sady**. Vstupní datové sady reprezentuje hello vstup pro aktivitu v kanálu hello a výstupní datové reprezentuje hello výstup aktivity hello.

    V tomto kurzu, hello vstupní a výstupní datové sady, zadejte umístění vstupní a výstupní data v hello Azure Blob Storage. Hello propojené služby Azure Storage Určuje účet úložiště Azure používají. Vstupní datové sady Určuje, kde hello vstupní soubory jsou umístěny a výstupní datové Určuje, kde jsou umístěny hello výstupní soubory. 


V tématu [Úvod tooAzure Data Factory](data-factory-introduction.md) článku podrobnější přehled Azure Data Factory.
  
Tady je hello **zobrazení diagramu** objektu pro vytváření dat ukázkové hello v tomto kurzu vytvoříte. **MyFirstPipeline** má jednu aktivitu typu Hive, který využívá **AzureBlobInput** datovou sadu jako vstup a vytváří **AzureBlobOutput** datovou sadu jako výstup. 

![Zobrazení diagramu v kurzu služby Data Factory](media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)


V tomto kurzu **inputdata** složky hello **adfgetstarted** kontejner objektů blob v Azure obsahuje jeden soubor s názvem input.log. Tento soubor protokolu obsahuje záznamy z tři měsíce: leden, února a března 2016. Tady jsou hello ukázka řádků pro každý měsíc ve vstupním souboru hello. 

```
2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

Při zpracování souboru hello hello kanálu s aktivitou HDInsight Hive hello aktivity spouští skript Hive v clusteru HDInsight hello že oddíly vstupní data tak, že rok a měsíc. skript Hello vytvoří tři výstupní složky, které obsahují soubor s položky z každý měsíc.  

```
adfgetstarted/partitioneddata/year=2016/month=1/000000_0
adfgetstarted/partitioneddata/year=2016/month=2/000000_0
adfgetstarted/partitioneddata/year=2016/month=3/000000_0
```

Z výše uvedeném řádky ukázkové hello, hello nejprve (s 2016-01-01) je napsán toohello 000000_0 souboru v měsíci hello = 1 složky. Podobně hello druhá je napsán toohello souboru v měsíci hello = 2 složky a hello třetí jeden je napsán toohello souboru v měsíci hello = 3 složky.  

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít hello následující požadavky:

1. **Předplatné Azure** – Pokud nemáte předplatné Azure, můžete si během několika minut vytvořit bezplatný zkušební účet. V tématu hello [bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/) článek věnovaný tomu, jak můžete získat bezplatný zkušební účet.
2. **Úložiště Azure** – použít účet pro obecné účely úložiště Azure úrovně standard pro ukládání dat hello v tomto kurzu. Pokud nemáte účet úložiště pro obecné účely standardní Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) článku. Po vytvoření účtu úložiště hello, poznamenejte si hello **název účtu** a **přístupový klíč**. Přečtěte si článek [Zobrazení, kopírování a opětovné vygenerování přístupových klíčů k úložišti](../storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys).
3. Stáhnout a revidovat soubor dotazů Hive hello (**HQL**) nachází v: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql](https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql). Tento dotaz transformuje vstupní data tooproduce výstupní data. 
4. Stáhnout a revidovat hello ukázkový vstupní soubor (**input.log**) nachází v: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log](https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log)
5. Vytvořte kontejner objektů blob s názvem **adfgetstarted** ve službě Azure Blob Storage. 
6. Nahrát **partitionweblogs.hql** souboru toohello **skriptu** složky v hello **adfgetstarted** kontejneru. Pomocí nástrojů, jako [Microsoft Azure Storage Explorer](http://storageexplorer.com/). 
7. Nahrát **input.log** souboru toohello **inputdata** složky v hello **adfgetstarted** kontejneru. 

Po dokončení hello požadavky, vyberte jednu z následujících nástrojů nebo sady SDK toodo hello kurzu hello: 

- [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Šablona Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Portál Azure a Visual Studio poskytují grafickým uživatelským rozhraním způsob vytváření datové továrny. Vzhledem k tomu, prostředí PowerShell, šablony Resource Manageru a REST API možnosti skriptování a programování způsob vytváření datové továrny.

> [!NOTE]
> Hello datovém kanálu v tomto kurzu transformuje vstupní data tooproduce výstupní data. Nekopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md). 





  
