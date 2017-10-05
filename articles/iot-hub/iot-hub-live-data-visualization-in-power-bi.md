---
title: "Vizualizace dat v reálném čase s daty ze snímačů z Azure IoT Hub – Power BI | Microsoft Docs"
description: "Pomocí Power BI vizualizovat data teploty a vlhkosti, který se shromažďují ze senzoru a odesílá do služby Azure IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vizualizace dat reálném čase, vizualizace dat za provozu, vizualizace dat snímačů"
ms.assetid: e67c9c09-6219-4f0f-ad42-58edaaa74f61
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: b190fea06ffc2406d781c7edad091f097cca9c2d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Vizualizovat data snímačů v reálném čase ze služby Azure IoT Hub pomocí Power BI

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak k vizualizaci dat snímačů v reálném čase, který Azure IoT hub přijímá pomocí Power BI. Pokud budete chtít zkusit znázorňovat data ve službě IoT hub s webovými aplikacemi, naleznete v tématu [použití Azure Web Apps k vizualizaci dat snímačů v reálném čase ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Co dělat

- Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.
- Vytvoření, konfiguraci a spusťte úlohu služby Stream Analytics pro přenos dat ze služby IoT hub ke svému účtu Power BI.
- Vytvoření a publikování sestavy Power BI můžete vizualizovat data.

## <a name="what-you-need"></a>Co potřebujete

- Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje následující požadavky:
  - Aktivní předplatné Azure.
  - V rámci svého předplatného služby Azure IoT hub.
  - Klientská aplikace, která odesílá zprávy do služby Azure IoT hub.
- Účet Power BI. ([Vyzkoušejte zdarma Power BI](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Vytvoření, konfigurace a spuštění úlohy Stream Analytics

### <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics

1. Na portálu Azure klikněte na nový > Internet věcí > úlohy služby Stream Analytics.
1. Zadejte následující informace pro úlohu.

   **Název úlohy**: název úlohy. Název musí být globálně jedinečný.

   **Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.

   **Umístění**: používalo stejné umístění jako vaší skupiny prostředků.

   **Připnout na řídicí panel**: zaškrtnete tuto možnost pro snadný přístup do služby IoT hub z řídicího panelu.

   ![Vytvořit úlohu služby Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Přidat vstup do úlohy Stream Analytics

1. Spusťte úlohu služby Stream Analytics.
1. V části **úlohy topologie**, klikněte na tlačítko **vstupy**.
1. V **vstupy** podokně klikněte na tlačítko **přidat**a potom zadejte následující informace:

   **Vstupní alias**: jedinečný odkaz pro vstup.

   **Zdroj**: vyberte **služby IoT hub**.

   **Skupiny příjemců**: Vyberte skupinu příjemců, kterou jste právě vytvořili.
1. Klikněte na možnost **Vytvořit**.

   ![Přidat vstup do úlohy Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Přidat výstup do úlohy Stream Analytics

1. V části **úlohy topologie**, klikněte na tlačítko **výstupy**.
1. V **výstupy** podokně klikněte na tlačítko **přidat**a potom zadejte následující informace:

   **Alias pro výstup**: jedinečný alias pro výstup.

   **Jímky**: vyberte **Power BI**.
1. Klikněte na tlačítko **Authorize**a pak se přihlaste ke svému účtu Power BI.
1. Po ověření, zadejte následující informace:

   **Skupina pracovního prostoru**: Vyberte pracovní prostor cílové skupiny.

   **Název datové sady**: Zadejte název datové sady.

   **Název tabulky**: Zadejte název tabulky.
1. Klikněte na možnost **Vytvořit**.

   ![Přidat výstup do úlohy Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Konfigurace dotazu úlohy Stream Analytics

1. V části **úlohy topologie**, klikněte na tlačítko **dotazu**.
1. Nahraďte `[YourInputAlias]` s alias vstupu úlohy.
1. Nahraďte `[YourOutputAlias]` s aliasem výstupu úlohy.
1. Klikněte na **Uložit**.

   ![Přidat dotaz do úlohy Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Spustit úlohu služby Stream Analytics

V úloze Stream Analytics, klikněte na tlačítko **spustit** > **nyní** > **spustit**. Jakmile se úloha úspěšně spustí, stav úlohy změní z **Zastaveno** k **systémem**.

![Spustit úlohu služby Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Vytvoření a publikování sestavy Power BI k vizualizaci dat

1. Zkontrolujte, jestli že ukázkové aplikace běží na vašem zařízení. Pokud není, můžete se podívat do kurzy pod [nastavit vaše zařízení](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).
1. Přihlaste se k vaší [Power BI](https://powerbi.microsoft.com/en-us/) účtu.
1. Přejděte do pracovního prostoru skupiny, které jste nastavili při vytváření výstupu pro úlohu služby Stream Analytics.
1. Klikněte na tlačítko **streamování datové sady**.

   Měli byste vidět uvedené datovou sadu, která jste zadali při vytváření výstupu pro úlohu služby Stream Analytics.
1. V části **akce**, klikněte na ikonu první pro vytvoření sestavy.

   ![Vytvoření sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Vytvořte na spojnicový graf a zobrazit v reálném čase teploty v čase.
   1. Na stránce pro vytvoření sestavy přidáte spojnicový graf.
   1. Na **pole** podokně rozbalte položku v tabulce, kterou jste zadali při vytváření výstupu pro úlohu služby Stream Analytics.
   1. Přetáhněte **EventEnqueuedUtcTime** k **osy** na **vizualizace** podokně.
   1. Přetáhněte **teploty** k **hodnoty**.

      Teď se vytvoří spojnicový graf. Osy x zobrazí datum a čas v časovém pásmu UTC. Osy y zobrazí z senzoru teploty.

      ![Přidání spojnicový graf pro teploty do sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Vytvořte jiný spojnicový graf zobrazíte v reálném čase vlhkosti v čase. To pokud chcete udělat, postupujte podle stejných kroků výše a umístěte **EventEnqueuedUtcTime** na ose x a **vlhkosti** na ose y.

   ![Přidání spojnicový graf pro vlhkosti do sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Klikněte na tlačítko **Uložit** pro uložení sestavy.
1. Klikněte na tlačítko **soubor** > **publikovat na webu**.
1. Klikněte na tlačítko **kód pro vložení vytvořit**a potom klikněte na **publikovat**.

Odkaz na sestavu jste zadali, že můžete sdílet s kýmkoli pro přístup k sestavě a fragmentu kódu pro integraci sestavy na blog nebo Web.

![Publikovat sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Společnost Microsoft nabízí [mobilních aplikací Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) pro zobrazení a interakci s řídicí panely Power BI a sestavy na vašem mobilním zařízení.

## <a name="next-steps"></a>Další kroky

Power BI jste úspěšně použili k vizualizaci dat snímačů v reálném čase ze služby Azure IoT hub.
Neexistuje jiný způsob, jak k vizualizaci dat ze služby Azure IoT Hub. V tématu [použití Azure Web Apps k vizualizaci dat snímačů v reálném čase ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
