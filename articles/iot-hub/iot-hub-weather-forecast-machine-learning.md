---
title: "Počasí prognózy pomocí Azure Machine Learning s daty ze služby IoT Hub | Microsoft Docs"
description: "Použití Azure Machine Learning k předvídání riziko déšť podle služby IoT hub shromažďuje ze senzoru teploty a vlhkosti data."
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
ms.openlocfilehash: 50ae54b9476c49b80236e295c0bf244df8236cff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Počasí prognózy používající senzor data ze služby IoT hub v Azure Machine Learning

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Machine learning je technika, kdy vědecké zpracování dat, která pomáhá počítače informace z existující data předpovídat budoucí chování, výsledky a trendy. Azure Machine Learning je cloudová služba pro prediktivní analýzu, která umožňuje rychle vytvářet a nasazovat prediktivní modely jako analytická řešení.

## <a name="what-you-learn"></a>Co se naučíte

Naučte se používat Azure Machine Learning na informace o počasí prognózy (riziko déšť) používající teploty a vlhkosti data ze služby Azure IoT hub. Riziko déšť je výstup z modelu předpovědi počasí připravené. Model je založena na historických dat prognózy riziko déšť vychází z teploty a vlhkosti.

## <a name="what-you-do"></a>Co dělat

- Model předpovědi počasí nasaďte jako webovou službu.
- Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.
- Vytvořit úlohu služby Stream Analytics a nakonfigurujte úlohy:
  - Čtení dat teploty a vlhkosti ze služby IoT hub.
  - Volání webové služby, bude mít déšť možnost.
  - Uložte výsledek do Azure blob storage.
- Chcete-li zobrazit předpovědi počasí pomocí Microsoft Azure Storage Explorer.

## <a name="what-you-need"></a>Co potřebujete

- Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje následující požadavky:
  - Aktivní předplatné Azure.
  - V rámci svého předplatného služby Azure IoT hub.
  - Klientská aplikace, která odesílá zprávy do služby Azure IoT hub.
- Účet Azure Machine Learning Studio. ([Zkuste Machine Learning Studio zdarma](https://studio.azureml.net/)).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Model předpovědi počasí nasadit jako webovou službu

1. Přejděte na [stránky modelu předpovědi počasí](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Klikněte na tlačítko **Open in Studio** v Microsoft Azure Machine Learning Studio.
   ![Otevřete stránku modelu předpovědi počasí v Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. Klikněte na tlačítko **spustit** ověření kroky v modelu. Tento krok může trvat 2 minut.
   ![Otevřete modelu předpovědi počasí v Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Klikněte na tlačítko **nastavení webové služby** > **prediktivní webové služby**.
   ![Nasazení modelu předpovědi počasí v Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. V diagramu, přetáhněte **webové služby vstup** modulu někde téměř **Score Model** modulu.
1. Připojení **webové služby vstup** modulu **Score Model** modulu.
   ![Připojení dvě moduly v nástroji Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. Klikněte na tlačítko **spustit** ověření kroky v modelu.
1. Klikněte na tlačítko **nasazení webové služby** nasadit model jako webovou službu.
1. Na řídicím panelu modelu Stáhnout **Excel 2010 nebo starší sešitu** pro **požadavků a odpovědí**.

   > [!Note]
   > Ujistěte se, abyste si stáhli **Excel 2010 nebo starší sešitu** i v případě, že používáte novější verzi aplikace Excel ve vašem počítači.

   ![Stažení aplikace Excel pro koncový bod REQUEST RESPONSE](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Otevřete sešit aplikace Excel, poznamenejte si **adresa URL webové služby** a **přístupový klíč**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Vytvoření, konfigurace a spuštění úlohy Stream Analytics

### <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics

1. V [portál Azure](https://ms.portal.azure.com/), klikněte na tlačítko **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.
1. Zadejte následující informace pro úlohu.

   **Název úlohy**: název úlohy. Název musí být globálně jedinečný.

   **Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.

   **Umístění**: používalo stejné umístění jako vaší skupiny prostředků.

   **Připnout na řídicí panel**: zaškrtnete tuto možnost pro snadný přístup do služby IoT hub z řídicího panelu.

   ![Vytvořit úlohu služby Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Přidat vstup do úlohy Stream Analytics

1. Spusťte úlohu služby Stream Analytics.
1. V části **úlohy topologie**, klikněte na tlačítko **vstupy**.
1. V **vstupy** podokně klikněte na tlačítko **přidat**a potom zadejte následující informace:

   **Vstupní alias**: jedinečný odkaz pro vstup.

   **Zdroj**: vyberte **služby IoT hub**.

   **Skupiny příjemců**: Vyberte skupinu uživatelů, kterou jste vytvořili.

   ![Přidat vstup do úlohy Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Přidat výstup do úlohy Stream Analytics

1. V části **úlohy topologie**, klikněte na tlačítko **výstupy**.
1. V **výstupy** podokně klikněte na tlačítko **přidat**a potom zadejte následující informace:

   **Alias pro výstup**: jedinečný alias pro výstup.

   **Jímky**: vyberte **úložiště objektů Blob**.

   **Účet úložiště**: účet úložiště pro úložiště objektů blob. Můžete vytvořit účet úložiště nebo použijte existující.

   **Kontejner**: uložení objektu blob kontejneru. Můžete vytvořit kontejner, nebo použijte existující.

   **Formát serializace událostí**: vyberte **CSV**.

   ![Přidat výstup do úlohy Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Přidání funkce do úlohy Stream Analytics k volání webové služby, které jste nasadili

1. V části **úlohy topologie**, klikněte na tlačítko **funkce** > **přidat**.
1. Zadejte následující informace:

   **Funkce Alias**: Zadejte `machinelearning`.

   **Typ funkce**: vyberte **Azure ML**.

   **Import možnost**: vyberte **Import z jiného předplatného**.

   **Adresa URL**: Zadejte adresu URL webové služby, kterou jste si poznamenali dolů ze sešitu aplikace Excel.

   **Klíč**: Zadejte přístupový klíč, který jste si poznamenali dolů ze sešitu aplikace Excel.

   ![Přidání funkce do úlohy Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Konfigurace dotazu úlohy Stream Analytics

1. V části **úlohy topologie**, klikněte na tlačítko **dotazu**.
1. Existujícího kódu nahraďte následujícím kódem:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Nahraďte `[YourInputAlias]` s alias vstupu úlohy.

   Nahraďte `[YourOutputAlias]` s aliasem výstupu úlohy.

1. Klikněte na **Uložit**.

### <a name="run-the-stream-analytics-job"></a>Spustit úlohu služby Stream Analytics

V úloze Stream Analytics, klikněte na tlačítko **spustit** > **nyní** > **spustit**. Jakmile se úloha úspěšně spustí, stav úlohy změní z **Zastaveno** k **systémem**.

![Spustit úlohu služby Stream Analytics](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Použít Microsoft Azure Storage Explorer zobrazíte předpovědi počasí

Spusťte klientskou aplikaci spustit shromažďování a odesílání teploty a vlhkosti dat do služby IoT hub. Pro každou zprávu, která přijímá služby IoT hub zavolá úlohu služby Stream Analytics webovou službu předpověď počasí k vytvoření riziko dešti. Výsledkem pak je uložena do Azure blob storage. Azure Storage Explorer je nástroj, který můžete použít k zobrazení výsledek.

1. [Stáhněte a nainstalujte Microsoft Azure Storage Explorer](http://storageexplorer.com/).
1. Otevřete Průzkumníka úložiště Azure.
1. Přihlaste se k účtu Azure.
1. Vyberte své předplatné.
1. Klikněte na předplatné > **účty úložiště** > váš účet úložiště > **kontejnery objektů Blob** > vaše kontejneru.
1. Otevřete soubor .csv zobrazíte výsledek. Poslední sloupec zaznamenává riziko dešti.

   ![Získání výsledku předpověď počasí pomocí Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Souhrn

Azure Machine Learning jste úspěšně použili k vytvoření riziko déšť na základě dat teploty a vlhkosti, která přijímá služby IoT hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]