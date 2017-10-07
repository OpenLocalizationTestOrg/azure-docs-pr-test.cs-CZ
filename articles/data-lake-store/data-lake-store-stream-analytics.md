---
title: "aaaStream data ze služby Stream Analytics do Data Lake Store | Microsoft Docs"
description: "Použití Azure Stream Analytics toostream data do Azure Data Lake Store"
services: data-lake-store,stream-analytics
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 68c727d4807db0abe6efa90145d68c78902eb789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Streamování dat z Azure Storage Blob do služby Data Lake Store pomocí Azure Stream Analytics
V tomto článku se dozvíte, jak toouse Azure Data Lake uložit jako výstup pro úlohu služby Azure Stream Analytics. Tento článek ukazuje jednoduchého scénáře, který čte data z objektu blob Azure Storage (vstup) a zápisy hello data tooData Lake Store (výstup).

> [!NOTE]
> V tomto okamžiku vytvoření a konfigurace Data Lake Store výstupy Stream Analytics je podporováno pouze v hello [portálu Azure Classic](https://manage.windowsazure.com). Proto některé části tohoto kurzu použije hello portálu Azure Classic.
>
>

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Účet služby Azure Storage**. Kontejner objektů blob z těchto dat tooinput účet budete používat pro úlohu služby Stream Analytics. Pro tento kurz předpokládá, že máte účet úložiště s názvem **storageforasa** a nazývá kontejner v rámci účtu hello **storageforasacontainer**. Po vytvoření kontejneru hello nahrajte ukázková data souboru tooit. 
  
* **Účet Azure Data Lake Store**. Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md). Předpokládejme, máte účet Data Lake Store s názvem **asadatalakestore**. 

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics
Začněte vytvořením úlohy služby Stream Analytics, který obsahuje vstupní zdroj a cíl výstupu. V tomto kurzu hello zdroj je kontejner objektů blob v Azure a cíl hello je Data Lake Store.

1. Přihlaste se toohello [portálu Azure](https://portal.azure.com).

2. V levém podokně hello, klikněte na **úlohy Stream Analytics**a potom klikněte na **přidat**.

    ![Vytvoření úlohy Stream Analytics](./media/data-lake-store-stream-analytics/create.job.png "vytvořit úlohu služby Stream Analytics")

    > [!NOTE]
    > Ujistěte se, vytvoření úlohy v hello stejné oblasti jako účet úložiště hello nebo je pro vás znamená další náklady na přesun dat mezi oblastmi.
    >

## <a name="create-a-blob-input-for-hello-job"></a>Vytvoření vstupní objekt Blob pro úlohu hello

1. Otevřete hello stránka pro úlohy Stream Analytics hello hello levém podokně klikněte na tlačítko hello **vstupy** a pak klikněte **přidat**.

    ![Přidat úlohu vstupní tooyour](./media/data-lake-store-stream-analytics/create.input.1.png "přidat úloha vstupní tooyour")

2. Na hello **nové vstup** okno, zadejte následující hodnoty hello.

    ![Přidat úlohu vstupní tooyour](./media/data-lake-store-stream-analytics/create.input.2.png "přidat úloha vstupní tooyour")

    * Pro **vstupní alias**, zadejte jedinečný název pro vstupní úlohy hello.
    * Pro **typ zdroje**, vyberte **datový proud**.
    * Pro **zdroj**, vyberte **úložiště objektů Blob**.
    * Pro **předplatné**, vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.
    * Pro **účet úložiště**, vyberte účet úložiště hello, kterou jste vytvořili v rámci požadavků hello. 
    * Pro **kontejneru**vyberte hello kontejneru, který jste vytvořili v hello vybraný účet úložiště.
    * Pro **formát serializace událostí**, vyberte **CSV**.
    * Pro **oddělovač**, vyberte **kartě**.
    * Pro **kódování**, vyberte **UTF-8**.

    Klikněte na možnost **Vytvořit**. portál Hello teď přidá hello vstup a testy tooit hello připojení.


## <a name="create-a-data-lake-store-output-for-hello-job"></a>Vytvoření výstupní Data Lake Store pro úlohu hello

1. Otevřete stránku hello hello úlohy Stream Analytics, klikněte na tlačítko hello **výstupy** a pak klikněte **přidat**.

    ![Přidat úlohu tooyour výstup](./media/data-lake-store-stream-analytics/create.output.1.png "přidat úloha tooyour výstup")

2. Na hello **nový výstupní** okno, zadejte následující hodnoty hello.

    ![Přidat úlohu tooyour výstup](./media/data-lake-store-stream-analytics/create.output.2.png "přidat úloha tooyour výstup")

    * Pro **alias pro výstup**, zadejte jedinečný název pro výstup úlohy hello. Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis Data Lake Store.
    * Pro **jímky**, vyberte **Data Lake Store**.
    * Bude výzvami tooauthorize přístup k účtu tooData Lake Store. Klikněte na tlačítko **Autorizovat**.

3. Na hello **nový výstupní** okně pokračovat tooprovide hello následující hodnoty.

    ![Přidat úlohu tooyour výstup](./media/data-lake-store-stream-analytics/create.output.3.png "přidat úloha tooyour výstup")

    * Pro **název účtu**, vyberte účet Data Lake Store hello jste již vytvořili, kam chcete odeslat toobe pro výstup hello úlohy.
    * Pro **vzorek cesty předponu**, zadejte toowrite cesty použité souboru zadány vaše soubory v rámci hello účtu Data Lake Store.
    * Pro **formát data**, pokud jste použili token kalendářního data v cestě hello předponu, můžete vybrat formát data hello, ve kterém jsou uspořádány soubory.
    * Pro **formát času**, pokud jste použili token čas v cestě hello předponu, zadejte hello čas formát, ve kterém jsou uspořádány soubory.
    * Pro **formát serializace událostí**, vyberte **CSV**.
    * Pro **oddělovač**, vyberte **kartě**.
    * Pro **kódování**, vyberte **UTF-8**.
    
    Klikněte na možnost **Vytvořit**. portál Hello teď přidá výstup hello a otestuje připojení tooit hello.
    
## <a name="run-hello-stream-analytics-job"></a>Spustit úlohu služby Stream Analytics hello

1. toorun úloha Stream Analytics, je nutné spustit dotaz z hello **dotazu** kartě. V tomto kurzu můžete spustit ukázkový dotaz hello nahraďte zástupné symboly hello hello úlohy vstupní a výstupní aliasy, jak je znázorněno v následujícím snímku obrazovky hello.

    ![Spuštění dotazu](./media/data-lake-store-stream-analytics/run.query.png "spuštění dotazu")

2. Klikněte na tlačítko **Uložit** shora hello hello obrazovky a pak z hello **přehled** , klikněte na **spustit**. Dialogové okno hello, vyberte **vlastní čas**a poté nastavte hello aktuální datum a čas.

    ![Nastavit čas úlohy](./media/data-lake-store-stream-analytics/run.query.2.png "nastavit čas úlohy")

    Klikněte na tlačítko **spustit** toostart hello úlohy. To může trvat až tooa několik minut toostart hello úlohy.

3. data hello tootrigger hello úlohy toopick z objektu blob hello, zkopírujte kontejneru objektů blob souboru toohello ukázková data. Ukázkový datový soubor můžete získat z hello [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). V tomto kurzu budeme zkopírujte soubor hello **vehicle1_09142014.csv**. Můžete používat různé klienty, jako třeba [Azure Storage Explorer](http://storageexplorer.com/), kontejner objektů blob tooa tooupload data.

4. Z hello **přehled** v části **monitorování**, najdete v části jak zpracování dat hello.

    ![Úloha monitoru](./media/data-lake-store-stream-analytics/run.query.3.png "úlohy monitorování")

5. Nakonec můžete ověřit, zda je k dispozici v účtu Data Lake Store hello hello úlohy výstupní data. 

    ![Zkontrolujte výstup](./media/data-lake-store-stream-analytics/run.query.4.png "ověřte výstup")

    V podokně Průzkumník dat hello, Všimněte si, že hello výstup jako cestu ke složce napsané tooa zadaný v hello, nastavení výstupu Data Lake Store (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Viz také
* [Vytvoření toouse clusteru HDInsight Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
