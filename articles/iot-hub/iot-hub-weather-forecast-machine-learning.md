---
title: "aaaWeather prognózy pomocí Azure Machine Learning s daty ze služby IoT Hub | Microsoft Docs"
description: "Použití Azure Machine Learning toopredict hello riziko déšť založené na hello teploty a vlhkosti data, která shromažďuje služby IoT hub ze senzoru."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "předpověď počasí machine learning"
ms.assetid: 8ba7d9e7-699c-4448-b353-0f3e1429d198
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 04abe97558ccfc152bae2e0d435033433c0023dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="weather-forecast-using-hello-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Předpověď počasí pomocí hello senzor dat z centra IoT v Azure Machine Learning

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Machine learning je technika, kdy vědecké zpracování dat, která pomáhá počítače dozvědět se od existujícího data tooforecast budoucí chování, výsledky a trendy. Azure Machine Learning je Cloudová služba prediktivní analýzy, která je možné tooquickly vytvářet a nasazovat prediktivní modely jako analytická řešení.

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak toouse Azure Machine Learning toodo předpověď počasí (riziko déšť) pomocí hello teploty a vlhkosti data ze služby Azure IoT hub. Hello riziko déšť je výstup hello modelu předpovědi počasí připravené. Hello model je založena na historických datech tooforecast riziko déšť vychází z teploty a vlhkosti.

## <a name="what-you-do"></a>Co dělat

- Nasazení modelu předpovědi počasí hello jako webovou službu.
- Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.
- Vytvořit úlohu služby Stream Analytics a nakonfigurujte hello úlohy:
  - Čtení dat teploty a vlhkosti ze služby IoT hub.
  - Volání hello webové služby tooget hello déšť příležitosti.
  - Uložte hello výsledek tooan Azure blob storage.
- Použít Microsoft Azure Storage Explorer tooview hello počasí prognózu.

## <a name="what-you-need"></a>Co potřebujete

- Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:
  - Aktivní předplatné Azure.
  - V rámci svého předplatného služby Azure IoT hub.
  - Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.
- Účet Azure Machine Learning Studio. ([Zkuste Machine Learning Studio zdarma](https://studio.azureml.net/)).

## <a name="deploy-hello-weather-prediction-model-as-a-web-service"></a>Nasazení modelu předpovědi počasí hello jako webovou službu

1. Přejděte toohello [stránky modelu předpovědi počasí](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Klikněte na tlačítko **Open in Studio** v Microsoft Azure Machine Learning Studio.
   ![Otevřete hello počasí předpovědi modelu stránky v Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. Klikněte na tlačítko **spustit** toovalidate hello kroky v modelu hello. Tento krok může trvat toocomplete 2 minut.
   ![Model předpovědi počasí otevřete hello v Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Klikněte na tlačítko **nastavení webové služby** > **prediktivní webové služby**.
   ![Nasazení modelu předpovědi počasí hello v Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. V diagramu hello přetáhněte hello **webové služby vstup** modulu někde téměř hello **Score Model** modulu.
1. Připojit hello **webové služby vstup** modulu toohello **Score Model** modulu.
   ![Připojení dvě moduly v nástroji Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. Klikněte na tlačítko **spustit** toovalidate hello kroky v modelu hello.
1. Klikněte na tlačítko **nasazení webové služby** toodeploy hello model jako webovou službu.
1. Na řídicím panelu hello hello modelu, stáhněte si hello **Excel 2010 nebo starší sešitu** pro **požadavků a odpovědí**.

   > [!Note]
   > Ujistěte se, že si stáhnout hello **Excel 2010 nebo starší sešitu** i v případě, že používáte novější verzi aplikace Excel ve vašem počítači.

   ![Stáhnout hello aplikace Excel pro koncový bod REQUEST RESPONSE hello](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Otevřete sešit aplikace Excel hello, poznamenejte si hello **adresa URL webové služby** a **přístupový klíč**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Vytvoření, konfigurace a spuštění úlohy Stream Analytics

### <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics

1. V hello [portál Azure](https://ms.portal.azure.com/), klikněte na tlačítko **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.
1. Zadejte následující informace pro úlohu hello hello.

   **Název úlohy**: název hello hello úlohy. Název Hello musí být globálně jedinečný.

   **Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.

   **Umístění**: použití hello stejné umístění jako vaší skupiny prostředků.

   **PIN kód toodashboard**: zaškrtnete tuto možnost pro Centrum IoT tooyour snadný přístup z řídicího panelu hello.

   ![Vytvořit úlohu služby Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="add-an-input-toohello-stream-analytics-job"></a>Přidat úloha Stream Analytics vstupní toohello

1. Úloha Stream Analytics otevřete hello.
1. V části **úlohy topologie**, klikněte na tlačítko **vstupy**.
1. V hello **vstupy** podokně klikněte na tlačítko **přidat**a pak zadejte hello následující informace:

   **Vstupní alias**: hello jedinečný odkaz pro vstup hello.

   **Zdroj**: vyberte **služby IoT hub**.

   **Skupiny příjemců**: Vyberte hello příjemce skupinu, kterou jste vytvořili.

   ![Přidat úloha Stream Analytics vstupní toohello v Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="add-an-output-toohello-stream-analytics-job"></a>Přidat úloha Stream Analytics toohello výstup

1. V části **úlohy topologie**, klikněte na tlačítko **výstupy**.
1. V hello **výstupy** podokně klikněte na tlačítko **přidat**a pak zadejte hello následující informace:

   **Alias pro výstup**: hello jedinečný odkaz pro výstup hello.

   **Jímky**: vyberte **úložiště objektů Blob**.

   **Účet úložiště**: hello účet úložiště pro úložiště objektů blob. Můžete vytvořit účet úložiště nebo použijte existující.

   **Kontejner**: hello kontejneru uložení objektů blob hello. Můžete vytvořit kontejner, nebo použijte existující.

   **Formát serializace událostí**: vyberte **CSV**.

   ![Přidat úloha Stream Analytics toohello výstupu v Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="add-a-function-toohello-stream-analytics-job-toocall-hello-web-service-you-deployed"></a>Přidání funkce toohello Stream Analytics úlohy toocall hello webové služby, které jste nasadili

1. V části **úlohy topologie**, klikněte na tlačítko **funkce** > **přidat**.
1. Zadejte hello následující informace:

   **Funkce Alias**: Zadejte `machinelearning`.

   **Typ funkce**: vyberte **Azure ML**.

   **Import možnost**: vyberte **Import z jiného předplatného**.

   **Adresa URL**: Zadejte hello adresa URL webové služby, kterou jste si poznamenali dolů ze sešitu aplikace Excel hello.

   **Klíč**: Zadejte hello přístupový klíč, který jste si poznamenali dolů ze sešitu aplikace Excel hello.

   ![Přidat úloha Stream Analytics toohello funkce v Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a>Konfigurace hello dotazu úlohy Stream Analytics hello

1. V části **úlohy topologie**, klikněte na tlačítko **dotazu**.
1. Nahraďte stávající kód hello hello následující kód:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Nahraďte `[YourInputAlias]` s aliasem hello vstupní hello úlohy.

   Nahraďte `[YourOutputAlias]` s alias pro výstup hello hello úlohy.

1. Klikněte na **Uložit**.

### <a name="run-hello-stream-analytics-job"></a>Spustit úlohu služby Stream Analytics hello

V úloze Stream Analytics hello, klikněte na tlačítko **spustit** > **nyní** > **spustit**. Jakmile hello úloha úspěšně spustí, změní se stav úlohy hello ze **Zastaveno** příliš**systémem**.

![Spustit úlohu služby Stream Analytics hello](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-tooview-hello-weather-forecast"></a>Použití Microsoft Azure Storage Explorer tooview hello počasí prognózy

Spusťte hello klienta aplikace toostart shromažďování a odesílání teploty a vlhkosti data tooyour IoT hub. Pro každou zprávu, která přijímá služby IoT hub zavolá úlohy služby Stream Analytics hello hello předpověď počasí webové služby tooproduce hello riziko dešti. výsledek Hello pak je uložena tooyour úložiště objektů blob Azure. Azure Storage Explorer je nástroj, který můžete použít výsledek hello tooview.

1. [Stáhněte a nainstalujte Microsoft Azure Storage Explorer](http://storageexplorer.com/).
1. Otevřete Průzkumníka úložiště Azure.
1. Přihlaste se tooyour účet Azure.
1. Vyberte své předplatné.
1. Klikněte na předplatné > **účty úložiště** > váš účet úložiště > **kontejnery objektů Blob** > vaše kontejneru.
1. Otevřete výsledek hello toosee soubor .csv. poslední sloupec záznamů Hello hello riziko dešti.

   ![Získání výsledku předpověď počasí pomocí Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Souhrn

Úspěšně jste použili Azure Machine Learning tooproduce hello riziko déšť na základě dat hello teploty a vlhkosti, která přijímá služby IoT hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]