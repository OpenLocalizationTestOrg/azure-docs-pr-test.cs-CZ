---
title: aaaAzure integrace Stream Analytics a Machine Learning | Microsoft Docs
description: "Jak toouse uživatelsky definované funkce a Machine Learning v úloze Stream Analytics"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/06/2017
ms.author: jeffstok
ms.openlocfilehash: e1ba7ab51ece80719839793e1320a7666cfc4181
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Provedení analýzy postojích pomocí Azure Stream Analytics nebo Azure Machine Learning
Tento článek popisuje, jak tooquickly nastavit jednoduchou úlohu Azure Stream Analytics, která integruje Azure Machine Learning. Použijte model Machine Learning postojích analytics z hello Cortana Intelligence Gallery tooanalyze streamování textová data a určení skóre postojích hello v reálném čase. Pomocí hello Cortana Intelligence Suite umožňuje provedení této úlohy bez obav o rozbor všech hello vytváření model postojích analytics.

Můžete použít informace z tohoto článku tooscenarios například tyto:

* Analýza v reálném čase postojích na streamování dat Twitteru.
* Analýza záznamy zákazníka konverzace s pracovníky technické podpory.
* Vyhodnocení komentáře na fóra, blogy a videa. 
* Mnoho dalších v reálném čase, prediktivní vyhodnocování scénářů.

Ve scénáři reálného by získat hello data přímo z datového proudu Twitter. toosimplify hello kurzu jsme jste jej tak, aby hello úlohy streamování Analytics získá tweetů ze souboru CSV v úložišti objektů Blob Azure. Můžete vytvořit svůj vlastní soubor CSV, nebo můžete použít ukázkový soubor CSV, jak ukazuje následující obrázek hello:

![Ukázka tweetů v souboru CSV](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

úlohy streamování Analytics Hello, který vytvoříte platí model analýzy postojích hello jako uživatelem definované funkce (UDF) na hello ukázka textová data z úložiště objektů blob hello. výstup Hello (hello výsledek analýzy postojích hello) je zapsán toohello stejné úložiště objektů blob v jiném souboru CSV. 

Hello následující obrázek znázorňuje tuto konfiguraci. Jak jsme uvedli, realističtější scénáři můžete nahradit úložiště objektů blob streamování Twitter dat z Azure Event Hubs vstup. Kromě toho je sestavení [Microsoft Power BI](https://powerbi.microsoft.com/) v reálném čase vizualizace agregační postojích hello.    

![Přehled integrace Stream Analytics Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Požadavky
Než začnete, ujistěte se, že máte hello následující:

* Aktivní předplatné Azure.
* Soubor CSV s některá data v ní. Si můžete stáhnout soubor hello uvedena výše z [Githubu](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), nebo můžete vytvořit svůj vlastní soubor. V tomto článku předpokládáme, že používáte hello souboru z Githubu.

Na vysoké úrovni toocomplete hello úlohy ukázáno v tomto článku jste hello následující:

1. Vytvořit účet úložiště Azure a kontejner úložiště objektů blob a nahrajte kontejner toohello vstupní soubor ve formátu CSV.
3. Přidání modelu analytics postojích z pracovního prostoru hello Cortana Intelligence Gallery tooyour Azure Machine Learning a nasadit tento model jako webovou službu v pracovního prostoru Machine Learning hello.
5. Vytvořte úlohu služby Stream Analytics, který volání této webové služby jako funkci v pořadí toodetermine postojích pro zadávání textu hello.
6. Spuštění úlohy Stream Analytics hello a zkontrolujte výstup hello.

## <a name="create-a-storage-container-and-upload-hello-csv-input-file"></a>Vytvořit kontejner úložiště a odeslat vstupní soubor CSV hello
Pro tento krok můžete použít všechny souboru CSV, jako je například hello nějaký k dispozici z Githubu.

1. V hello portálu Azure, klikněte na **nový** &gt; **úložiště** &gt; **účet úložiště**.

   ![Vytvořit nový účet úložiště](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. Zadejte název (`samldemo` v příkladu hello). v názvu Hello lze použít pouze malá písmena a čísla a musí být jedinečný v Azure. 

3. Zadejte existující skupinu prostředků a zadejte umístění. Pro umístění, doporučujeme, aby všechny prostředky hello vytvořené v tomto kurzu hello stejné umístění.

    ![Zadejte podrobnosti o účtu úložiště](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. V hello portálu Azure vyberte účet úložiště hello. V okně účtu úložiště hello, klikněte na tlačítko **kontejnery** a pak klikněte na  **+ &nbsp;kontejneru** toocreate úložiště objektů blob.

    ![Vytvoření kontejneru objektů blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. Zadejte název pro kontejner hello (`azuresamldemoblob` v příkladu hello) a ověřte, že **přistupovat typu** je nastaven příliš**Blob**. Až to budete mít, klikněte na **OK**.

    ![Zadejte podrobnosti kontejner objektů blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. V hello **kontejnery** okně, vyberte hello nový kontejner, což otevře okno hello pro tento kontejner.

7. Klikněte na **Odeslat**.

    ![Nahrát tlačítko pro kontejner](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. V hello **nahrávání blob** okno, zadejte soubor CSV hello chcete toouse pro účely tohoto kurzu. Pro **Blob typ**, vyberte **objekt blob bloku** a zároveň se zablokují hello sadu velikost too4 MB, který je dostatečný pro účely tohoto kurzu.

    ![Nahrát soubor blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. Klikněte na tlačítko hello **nahrát** tlačítko v hello dolní části okna hello.

## <a name="add-hello-sentiment-analytics-model-from-hello-cortana-intelligence-gallery"></a>Přidat model analýzy postojích hello z hello Cortana Intelligence Gallery

Teď, když je hello ukázková data do objektu BLOB, můžete povolit model analýzy postojích hello v Cortana Intelligence Gallery.

1. Přejděte toohello [model postojích prediktivní analýzy](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) stránku hello Cortana Intelligence Gallery.  

2. Klikněte na tlačítko **Open in Studio**.  
   
   ![Stream Analytics Machine Learning, otevřete Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Přihlaste se toogo toohello prostoru. Vyberte umístění.

4. Klikněte na tlačítko **spustit** v hello dolní části stránky hello. Hello proces spouští, což trvá několik minut.

   ![Spusťte experiment v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. Jakmile proces hello proběhl úspěšně, vyberte **nasazení webové služby** na hello dolní části stránky hello.

   ![experiment v nástroji Machine Learning Studio nasaďte jako webovou službu](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. toovalidate, který hello postojích model analýzy je připraven toouse, klikněte na tlačítko hello **Test** tlačítko. Zadejte textový vstup, jako je "I rádi Microsoft". 

   ![Test experimentu v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    Pokud testovací hello funguje, zobrazí výsledek podobné toohello následující ukázka:

   ![výsledky testu v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. V hello **aplikace** sloupce, klikněte na tlačítko hello **Excel 2010 nebo starší sešitu** odkaz toodownload sešitu aplikace Excel. Hello sešit obsahuje klíč hello rozhraní API a adresy URL hello, je nutné, aby novější tooset do úlohy Stream Analytics hello.

    ![Stream Analytics Machine Learning, rychlého přehledu.](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-hello-machine-learning-model"></a>Vytvořit úlohu služby Stream Analytics, který používá model Machine Learning hello

Nyní můžete vytvořit úlohu Stream Analytics, která načte hello ukázka tweetů ze souboru CSV hello v úložišti objektů blob. 

### <a name="create-hello-job"></a>Vytvořit úlohu hello

1. Přejděte toohello [portál Azure](https://portal.azure.com).  

2. Klikněte na tlačítko **nový** > **Internet věcí** > **úlohy služby Stream Analytics**. 

   ![Cesty Azure portálu pro získání tooa nová úloha Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. Název úlohy hello `azure-sa-ml-demo`, zadejte předplatné, zadejte existující skupinu prostředků nebo vytvořte novou a vyberte umístění hello hello úlohy.

   ![Zadejte nastavení pro nové úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-hello-job-input"></a>Konfigurace vstupu úlohy hello
Úloha Hello získá vstupní ze souboru CSV hello, který jste nahráli starší tooblob úložiště.

1. Po hello byla vytvořena úloha, v části **úlohy topologie** v okně úlohy hello, klikněte na tlačítko hello **vstupy** pole.  
   
   !["Vstupy" pole v okně úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. V hello **vstupy** okně klikněte na tlačítko **+ přidat**.

   ![Přidání tlačítka pro přidání úlohu služby Stream Analytics vstupní toohello](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. Vyplňte hello **nové vstup** okno s těmito hodnotami:

    * **Vstupní alias**: použití hello název `datainput`.
    * **Typ zdroje**: vyberte **datový proud**.
    * **Zdroj**: vyberte **úložiště objektů Blob**.
    * **Import možnost**: vyberte **pomocí úložiště objektů blob z aktuálního předplatného**. 
    * **Účet úložiště**. Vyberte účet úložiště hello, které jste vytvořili dříve.
    * **Kontejner**. Vyberte hello kontejneru, které jste vytvořili dříve (`azuresamldemoblob`).
    * **Formát serializace událostí**. Vyberte **CSV**.

    ![Nastavení pro nové úlohy vstup](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. Klikněte na možnost **Vytvořit**.

### <a name="configure-hello-job-output"></a>Konfigurace hello výstup úlohy
toohello výsledky odešle úlohu Hello stejný objekt blob úložiště, kde získá vstup. 

1. V části **úlohy topologie** v okně úlohy hello, klikněte na tlačítko hello **výstupy** pole.  
  
   ![Vytvořit nový výstupní úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. V hello **výstupy** okně klikněte na tlačítko **+ přidat**a poté přidejte výstup s aliasem hello `datamloutput`. 

3. Pro **jímky**, vyberte **úložiště objektů Blob**. Potom vyplňte hello zbytek hello výstup hello nastavení pomocí stejné hodnoty, které jste použili pro hello úložiště objektů blob pro vstup:

    * **Účet úložiště**. Vyberte účet úložiště hello, které jste vytvořili dříve.
    * **Kontejner**. Vyberte hello kontejneru, které jste vytvořili dříve (`azuresamldemoblob`).
    * **Formát serializace událostí**. Vyberte **CSV**.

   ![Nastavení pro nové výstup úlohy](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. Klikněte na možnost **Vytvořit**.   


### <a name="add-hello-machine-learning-function"></a>Přidání funkce Machine Learning hello 
Dříve jste publikovali webovou službu tooa model Machine Learning. V tomto scénáři při spuštění úlohy analýzy datového proudu hello odešle každý ukázka tweet z hello vstupní toohello webové služby pro analýzu postojích. Vrátí Hello webové službě Machine Learning postojích (`positive`, `neutral`, nebo `negative`) a pravděpodobnost vzniku tweet hello se kladné. 

V této části kurzu hello definujete funkce v hello Úloha analýzy datového proudu. Funkce Hello můžete být vyvolaná toosend tweet toohello webové služby a získat odpovědi hello zpět. 

1. Ujistěte se, že máte hello webové služby adresy URL a rozhraní API klíč, který jste si stáhli dříve v sešitu aplikace Excel hello.

2. Vrátí okno Přehled toohello úlohy.

3. V části **nastavení**, vyberte **funkce** a pak klikněte na **+ přidat**.

   ![Přidat úloha Stream Analytics toohello – funkce](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. Zadejte `sentiment` jako hello funkce alias a vyplňte hello zbytek hello okno pomocí těchto hodnot:

    * **Typ funkce**: vyberte **Azure ML**.
    * **Import možnost**: vyberte **Import z jiného předplatného**. To vám dává možnost tooenter hello URL a klíč.
    * **Adresa URL**: vložte adresu URL webové služby hello.
    * **Klíč**: vložte klíč rozhraní API hello.
  
    ![Nastavení pro přidání úloha Stream Analytics toohello funkce Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. Klikněte na možnost **Vytvořit**.

### <a name="create-a-query-tootransform-hello-data"></a>Vytvoření dotazu tootransform hello dat

Stream Analytics používá deklarativní, na základě SQL dotaz tooexamine hello vstup a zpracovat. V této části vytvoříte dotaz, který čte každý tweet ze vstupu a potom se vyvolá hello Machine Learning funkce tooperform postojích analýzy. dotaz Hello pak odešle hello výsledek toohello výstup, že jste definovali (úložiště objektů blob).

1. Vrátí okno Přehled toohello úlohy.

2.  V části **úlohy topologie**, klikněte na tlačítko hello **dotazu** pole.

    ![Vytvoření dotazu úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. Zadejte hello následující dotaz:

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    dotaz Hello vyvolá hello funkce, které jste vytvořili dříve (`sentiment`) v pořadí tooperform postojích analýzy na každý tweet ve vstupu hello. 

4. Klikněte na tlačítko **Uložit** toosave hello dotazu.


## <a name="start-hello-stream-analytics-job-and-check-hello-output"></a>Spuštění úlohy Stream Analytics hello a zkontrolujte výstup hello

Nyní můžete spustit úlohy služby Stream Analytics hello.

### <a name="start-hello-job"></a>Spuštění úlohy hello
1. Vrátí okno Přehled toohello úlohy.

2. Klikněte na tlačítko **spustit** hello horní části okna hello.

    ![Vytvoření dotazu úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. V hello **spuštění úlohy**, vyberte **vlastní**a pak vyberte předchozí toowhen jeden den, který jste nahráli úložiště tooblob soubor CSV hello. Když jste hotovi, klikněte na tlačítko **spustit**.  


### <a name="check-hello-output"></a>Zkontrolujte výstup hello
1. Úlohy umožňují hello spustit několik minut, dokud neuvidíte aktivity v hello **monitorování** pole. 

2. Pokud máte nástroj normálně používat obsah hello tooexamine úložiště objektů blob, použijte tento nástroj tooexamine hello `azuresamldemoblob` kontejneru. Alternativně hello, proveďte kroky v hello portálu Azure:

    1. Hello portálu, najde hello `samldemo` úložiště účtu a v rámci účtu hello najde hello `azuresamldemoblob` kontejneru. Zobrazí dva soubory v kontejneru hello: hello soubor, který obsahuje hello ukázka tweetů a soubor CSV generované úlohy služby Stream Analytics hello.
    2. Klikněte pravým tlačítkem hello vygeneruje soubor a pak vyberte **Stáhnout**. 

   ![Stáhněte si výstup úlohy CSV z úložiště objektů Blob](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. Otevřete hello vygeneruje soubor CSV. Vidět něco podobného jako hello následující ukázka:  
   
   ![Stream Analytics Machine Learning, CSV zobrazení](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a>Zobrazit metriky
Také můžete zobrazit související funkce metrik Azure Machine Learning. Hello následující metriky funkce související se zobrazují v hello **monitorování** pole v okně úlohy hello:

* **Funkce požadavky** označuje hello počet odeslaných požadavků tooa webové službě Machine Learning.  
* **Funkce události** označuje hello počet událostí v žádosti o hello. Ve výchozím nastavení každý tooa žádost webové službě Machine Learning obsahuje too1 000 událostí.  


## <a name="next-steps"></a>Další kroky

* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Integrace rozhraní REST API a strojového učení](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)



