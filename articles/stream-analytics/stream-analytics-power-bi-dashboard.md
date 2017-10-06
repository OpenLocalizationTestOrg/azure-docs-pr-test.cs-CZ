---
title: "řídicí panel BI aaaPower na Azure Stream Analytics | Microsoft Docs"
description: "Použijte v reálném čase streamování Power BI řídicí panel toogather business intelligence a analýze velkých objemů dat z úlohy Stream Analytics."
keywords: "Analýza řídicího panelu, řídicí panel v reálném čase"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: jeffstok
ms.openlocfilehash: cb9127576230e9d327b437b674f31cc23869bfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Stream Analytics a Power BI: řídicí panel analýzy v reálném čase pro datový proud
Azure Stream Analytics můžete využít jeden z hello úvodní nástroje business intelligence tootake [Microsoft Power BI](https://powerbi.com/). V tomto článku se dozvíte, jak vytvořit nástroje business intelligence pomocí Power BI jako výstup pro úlohy Azure Stream Analytics. Také zjistíte, jak toocreate a použití řídicího panelu v reálném čase.

Tento článek pokračuje z hello Stream Analytics [odhalování podvodů v reálném čase](stream-analytics-real-time-fraud-detection.md) kurzu. Staví na hello pracovní postup vytvořený v tomto kurzu, přičemž přidá Power BI výstup, takže můžete vizualizovat podvodné telefonních hovorů, které byly zjištěny nástrojem úlohu streamování Analytics. 

Můžete sledovat [video](https://www.youtube.com/watch?v=SGUpT-a99MA) který znázorňuje tento scénář.


## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte hello následující:

* Účet Azure.
* Účet pro Power BI. Můžete použít účet pracovní nebo školní účet.
* Dokončené verzi hello [odhalování podvodů v reálném čase](stream-analytics-real-time-fraud-detection.md) kurzu. kurz Hello zahrnuje aplikaci, která generuje metadata fiktivní telefonní hovor. V kurzu hello vytvoření centra událostí a odeslání hello streamování centra událostí toohello data telefonního hovoru. Můžete vytvořit dotaz, který zjistí podvodné volání (volání z hello stejné číslo na hello stejný čas v různých umístěních). 


## <a name="add-power-bi-output"></a>Přidat výstup Power BI
V kurzu zjišťování podvodů v reálném čase hello výstup hello je odeslán tooAzure úložiště objektů Blob. V této části přidáte výstupu, který odesílá informace tooPower BI.

1. V hello portálu Azure otevřete úlohy streamování Analytics hello, kterou jste vytvořili dříve. Pokud jste použili hello navrhovaný název, má název úlohy hello `sa_frauddetection_job_demo`.

2. Vyberte hello **výstupy** uprostřed hello hello úlohy řídicího panelu a potom vyberte položku **+ přidat**.

3. Pro **Alias pro výstup**, zadejte `CallStream-PowerBI`. Můžete použít jiný název. Pokud tak učiníte, poznamenejte si ho, protože potřebujete název hello později. 

4. V části **jímky**, vyberte **Power BI**.

   ![Vytvoření výstupu pro Power BI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. Klikněte na tlačítko **Autorizovat**.

    Otevře okno se kterém můžete zadat vaše přihlašovací údaje Azure pro pracovní nebo školní účet. 

    ![Zadejte přihlašovací údaje pro přístup k tooPower BI](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. Zadejte své přihlašovací údaje. Upozorňujeme, pak po zadání přihlašovacích údajů, můžete se také udělíte oprávnění toohello streamování Analytics úlohy tooaccess vaší oblasti Power BI.

7. Když se vrátil toohello **nový výstupní** okno, zadejte hello následující informace:

    * **Skupina pracovního prostoru**: Vyberte pracovní prostor v klientovi služby Power BI, kde chcete datovou sadu toocreate hello.
    * **Název datové sady**: Zadejte `sa-dataset`. Můžete použít jiný název. Pokud tak učiníte, poznamenejte si ho na později.
    * **Název tabulky**: Zadejte `fraudulent-calls`. Power BI výstup z úlohy Stream Analytics v současné době může mít pouze jednu tabulku v datové sadě.

    ![Pracovní prostor PBI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > Pokud má Power BI datové sady a tabulky, které mají hello stejné názvy jako hello ta, která zadáte v úloze Stream Analytics hello, se přepíší existující hello.
    > Doporučujeme vám, že nevytvoříte explicitně tuto datovou sadu a tabulky ve vašem účtu Power BI. Se automaticky vytvoří při spuštění úlohy Stream Analytics a hello úloha spustí čerpací výstup do Power BI. Pokud úloha dotaz nevrátí žádné výsledky, nejsou vytvořeny hello datovou sadu a tabulku.
    >

8. Klikněte na možnost **Vytvořit**.

Datová sada Hello je vytvořen s hello následující nastavení:

* **defaultRetentionPolicy: BasicFIFO**: Data jsou FIFO, s maximálně 200 000 řádků.
* **Vlastnost defaultMode: pushStreaming**: hello datovou sadu (také známa jako podporuje streamování dlaždice a tradiční vizuální prvky založené na sestavy nabídka).

V současné době nelze vytvořit datové sady s další příznaky.

Další informace o datových sadách Power BI najdete v tématu hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) odkaz.


## <a name="write-hello-query"></a>Zápis dotazů hello

1. Zavřít hello **výstupy** okno a okno návratový toohello úlohy.

2. Klikněte na tlačítko hello **dotazu** pole. 

3. Zadejte následující dotaz hello. Tento dotaz je podobné toohello spojení sama na sebe dotazu, kterou jste vytvořili v kurzu hello odhalování podvodů. Hello rozdílem je, že tento dotaz odešle výsledky toohello nový výstupní jste vytvořili (`CallStream-PowerBI`). 

    >[!NOTE]
    >Pokud není název vstupu hello `CallStream` v kurzu hello odhalování podvodů, nahraďte název pro `CallStream` v hello **FROM** a **připojení** klauzule v dotazu hello.

        /* Our criteria for fraud:
        Calls made from hello same caller tootwo phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where hello caller is hello same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where hello switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. Klikněte na **Uložit**.


## <a name="test-hello-query"></a>Test hello dotazu
Tato část je volitelný, ale doporučujeme. 

1. Pokud aplikace hello TelcoStreaming není aktuálně spuštěná, spusťte ji pomocí následujících kroků:

    * Otevřete okno příkazového řádku.
    * Přejděte toohello složku, kde jsou hello telcogenerator.exe a upravené telcodatagen.exe.config soubory.
    * Spusťte následující příkaz hello:

            telcodatagen.exe 1000 .2 2

2. V hello **dotazu** okně klikněte na tlačítko Další toohello hello tečky `CallStream` vstup a pak vyberte **vzorová data ze vstupu**.

3. Určete, zda má tři minut za dat a klikněte na tlačítko **OK**. Počkejte, dokud se oznámení, že hello data odebírají vzorky.

4. Klikněte na tlačítko **Test** a zajistěte, aby vám výsledky.


## <a name="run-hello-job"></a>Spustit úlohu hello

1. Ujistěte se, že tuto aplikaci TelcoStreaming hello běží.

2. Zavřít hello **dotazu** okno.

3. V okně úlohy hello, klikněte na tlačítko **spustit**.

    ![Spuštění úlohy Stream Analytics hello](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

Úlohu streamování Analytics spustí vyhledávání podvodné volání v příchozím datovém proudu hello. Úloha Hello také vytvoří hello datovou sadu a tabulkou v Power BI a spustí odesílání dat o toothem podvodné volání hello.


## <a name="create-hello-dashboard-in-power-bi"></a>Vytvořit hello řídicí panel v Power BI

1. Přejděte příliš[Powerbi.com](https://powerbi.com) a přihlaste se pomocí svého pracovního nebo školního účtu. Pokud dotaz úlohy Stream Analytics hello výstupy výsledků, zobrazí vaše datová sada je už vytvořený:

    ![Streamovanou datovou sadu v Power BI](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. V pracovním prostoru, klikněte na tlačítko  **+ &nbsp;vytvořit**.

    ![tlačítko pro vytvoření Hello v prostoru Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Vytvořit nový řídicí panel a pojmenujte ji `Fraudulent Calls`.

    ![Vytvořit řídicí panel a pojmenujte ho v prostoru Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Hello horní části okna hello, klikněte na tlačítko **přidat dlaždice**, vyberte **datových proudů vlastní**a potom klikněte na **Další**.

    ![Vlastní streamování datové sady](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. V části **si DATSETS**, vyberte datovou sadu a pak klikněte na tlačítko **Další**.

    ![Vaši streamovanou datovou sadu](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. V části **typ vizualizace**, vyberte **karty**a potom v hello **pole** seznamu, vyberte **fraudulentcalls**.

    ![Vizualizace podrobnosti nové dlaždice.](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. Klikněte na **Další**.

8. Zadejte podrobnosti o dlaždici jako nadpis a podnadpis.

    ![Nadpis a podnadpis nové dlaždice.](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. Klikněte na tlačítko **Použít**.

    Nyní máte čítač podvod!

    ![Čítač podvod](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. Postupujte podle hello kroky opakujte tooadd dlaždici (počínaje krok 4). Tentokrát hello následující:

    * Když získáte příliš**typ vizualizace**, vyberte **spojnicový graf**. 
    * Přidejte osy a vyberte **windowend**. 
    * Přidejte hodnotu a vyberte **fraudulentcalls**.
    * Pro **časové okno toodisplay**, vyberte hello posledních 10 minut.

    ![Vytvoření dlaždici spojnicový graf](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. Klikněte na tlačítko **Další**, přidat nadpis a podnadpis a klikněte na **použít**.

    řídicí panel Power BI Hello teď umožňuje dvě zobrazení dat o podvodné volání jako zjistil v hello streamovaných dat užitečné.

    ![Dokončení zobrazující dvě dlaždice pro podvodné volání řídicí panel Power BI](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>Další informace o Power BI

Tento kurz ukazuje, jak toocreate pouze několik druhů vizualizace pro datovou sadu. Power BI můžete vytvořit další nástroje business intelligence zákazníka pro vaši organizaci. Další nápady najdete hello následující prostředky:

* Další příklad řídicí panel Power BI, podívejte se na hello [Začínáme s Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) videa.
* Další informace o konfiguraci streamování Analytics úlohy výstup tooPower BI a používání skupin Power BI, zkontrolujte hello [Power BI](stream-analytics-define-outputs.md#power-bi) části hello [Stream Analytics výstupy](stream-analytics-define-outputs.md) článku. 
* Informace o používání Power BI obecně najdete v tématu [řídicí panely v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>Další informace o omezení a doporučené postupy
V současné době Power BI je možné volat přibližně jednou za sekundu. Streamování vizuály podporu paketů 15 kb. Kromě toho streamování vizuály nezdaří (ale stále nabízené toowork). Z důvodu tato omezení Power BI různě konfigurovat nejvíce přirozeně toocases, kde Azure Stream Analytics nepodporuje snížení zatížení významné data. Doporučujeme používat Přeskakující okno nebo Hopping okno tooensure, datová oznámení jsou maximálně jeden nabízené za sekundu a že váš dotaz pojmenováváme v rámci požadavky na propustnost hello.

Vaše okno můžete použít následující rovnice toocompute hello hodnota toogive hello v sekundách:

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Například:

* Máte 1 000 zařízení odesílání dat v intervalech jednu sekundu.
* Používáte hello Power BI Pro skladová položka podporující 1 000 000 řádků za hodinu.
* Chcete toopublish hello množství průměr dat na zařízení tooPower BI.

V důsledku toho hello rovnici:

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

V této konfiguraci můžete změnit hello původní dotaz toohello následující:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a>Obnovit ověřování
Pokud došlo ke změně hesla hello vzhledem k tomu, že vaše úlohy vytvoření nebo poslední ověření, musíte tooreauthenticate svůj účet Power BI. Pokud Azure Multi-Factor Authentication nakonfigurován v klientovi služby Azure Active Directory (Azure AD), musíte taky toorenew Power BI autorizace každé dva týdny. Pokud neobnovíte, se může zobrazit příznaky například chybějících výstup úlohy nebo `Authenticate user error` v protokoly operací hello.

Podobně pokud se úloha spustí po vypršení platnosti tokenu hello, dojde k chybě a hello úloha selže. tooresolve tento problém, zastavte hello úlohu, která je spuštěna a přejděte tooyour, které výstup Power BI. tooavoid ztrátě dat, vyberte hello **obnovit autorizace** propojit a pak restartujte úlohu z hello **naposledy Zastaveno**.

Po aktualizaci hello autorizaci s Power BI, Zelená výstraha se zobrazí v hello autorizace oblasti tooreflect, že hello problém byl vyřešen.

## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language – referenční informace](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční dokumentace Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
