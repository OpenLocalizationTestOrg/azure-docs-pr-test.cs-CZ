---
title: "vizualizace dat aaaReal čas dat snímačů z Azure IoT Hub – Power BI | Microsoft Docs"
description: "Pomocí Power BI toovisualize teploty a vlhkosti data, která se shromažďují ze snímačů hello a odesílá tooyour Azure IoT hub."
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
ms.openlocfilehash: d79ce757a9f2ab7a4744e8a0c523106e0f72cecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Vizualizovat data snímačů v reálném čase ze služby Azure IoT Hub pomocí Power BI

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak toovisualize snímačů v reálném čase data, která Azure IoT hub přijímá pomocí Power BI. Pokud chcete vizualizovat tootry hello data ve službě IoT hub s webovými aplikacemi, najdete v tématu [dat snímačů v reálném čase pomocí Azure Web Apps toovisualize ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Co dělat

- Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.
- Vytvoření, konfiguraci a spusťte úlohu služby Stream Analytics pro přenos dat z vaší IoT hub tooyour účet Power BI.
- Vytvoření a publikování dat hello toovisualize sestavy Power BI.

## <a name="what-you-need"></a>Co potřebujete

- Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:
  - Aktivní předplatné Azure.
  - V rámci svého předplatného služby Azure IoT hub.
  - Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.
- Účet Power BI. ([Vyzkoušejte zdarma Power BI](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Vytvoření, konfigurace a spuštění úlohy Stream Analytics

### <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy Stream Analytics

1. V hello portálu Azure, klikněte na nový > Internet věcí > úlohy služby Stream Analytics.
1. Zadejte následující informace pro úlohu hello hello.

   **Název úlohy**: název hello hello úlohy. Název Hello musí být globálně jedinečný.

   **Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.

   **Umístění**: použití hello stejné umístění jako vaší skupiny prostředků.

   **PIN kód toodashboard**: zaškrtnete tuto možnost pro Centrum IoT tooyour snadný přístup z řídicího panelu hello.

   ![Vytvořit úlohu služby Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. Klikněte na možnost **Vytvořit**.

### <a name="add-an-input-toohello-stream-analytics-job"></a>Přidat úloha Stream Analytics vstupní toohello

1. Úloha Stream Analytics otevřete hello.
1. V části **úlohy topologie**, klikněte na tlačítko **vstupy**.
1. V hello **vstupy** podokně klikněte na tlačítko **přidat**a pak zadejte hello následující informace:

   **Vstupní alias**: hello jedinečný odkaz pro vstup hello.

   **Zdroj**: vyberte **služby IoT hub**.

   **Skupiny příjemců**: Skupina uživatelů vyberte hello jste právě vytvořili.
1. Klikněte na možnost **Vytvořit**.

   ![Přidat úloha Stream Analytics vstupní tooa v Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-toohello-stream-analytics-job"></a>Přidat úloha Stream Analytics toohello výstup

1. V části **úlohy topologie**, klikněte na tlačítko **výstupy**.
1. V hello **výstupy** podokně klikněte na tlačítko **přidat**a pak zadejte hello následující informace:

   **Alias pro výstup**: hello jedinečný odkaz pro výstup hello.

   **Jímky**: vyberte **Power BI**.
1. Klikněte na tlačítko **Authorize**a pak se přihlaste ke svému účtu Power BI.
1. Po ověření, zadejte hello následující informace:

   **Skupina pracovního prostoru**: Vyberte pracovní prostor cílové skupiny.

   **Název datové sady**: Zadejte název datové sady.

   **Název tabulky**: Zadejte název tabulky.
1. Klikněte na možnost **Vytvořit**.

   ![Přidat úloha Stream Analytics tooa výstupu v Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a>Konfigurace hello dotazu úlohy Stream Analytics hello

1. V části **úlohy topologie**, klikněte na tlačítko **dotazu**.
1. Nahraďte `[YourInputAlias]` s aliasem hello vstupní hello úlohy.
1. Nahraďte `[YourOutputAlias]` s alias pro výstup hello hello úlohy.
1. Klikněte na **Uložit**.

   ![Přidat úloha Stream Analytics tooa dotazu v Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-hello-stream-analytics-job"></a>Spustit úlohu služby Stream Analytics hello

V úloze Stream Analytics hello, klikněte na tlačítko **spustit** > **nyní** > **spustit**. Jakmile hello úloha úspěšně spustí, změní se stav úlohy hello ze **Zastaveno** příliš**systémem**.

![Spustit úlohu služby Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-toovisualize-hello-data"></a>Vytvoření a publikování dat hello toovisualize sestavy Power BI

1. Zkontrolujte, jestli hello ukázkové aplikace běží na vašem zařízení. Pokud ne, může odkazovat toohello kurzy pod [nastavit vaše zařízení](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).
1. Přihlaste se tooyour [Power BI](https://powerbi.microsoft.com/en-us/) účtu.
1. Přejděte toohello pracovní prostor skupiny, který jste nastavili při vytváření hello výstup úlohy služby Stream Analytics hello.
1. Klikněte na tlačítko **streamování datové sady**.

   Měli byste vidět hello uvedené datovou sadu, která jste zadali při vytvoření hello výstup úlohy Stream Analytics hello.
1. V části **akce**, klikněte na tlačítko hello první ikona toocreate sestavy.

   ![Vytvoření sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Vytvořte řádku grafu tooshow v reálném čase teplotu v průběhu času.
   1. Na stránce vytváření sestav hello přidáte spojnicový graf.
   1. Na hello **pole** podokně rozbalte hello tabulku, kterou jste zadali při vytvoření hello výstup úlohy služby Stream Analytics hello.
   1. Přetáhněte **EventEnqueuedUtcTime** příliš**osy** na hello **vizualizace** podokně.
   1. Přetáhněte **teploty** příliš**hodnoty**.

      Teď se vytvoří spojnicový graf. osy x Hello zobrazí datum a čas v časovém pásmu UTC hello. osy y Hello zobrazí teploty ze snímačů hello.

      ![Přidat spojnicový graf pro teploty tooa sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Vytvořte další řádek grafu tooshow v reálném čase vlhkosti v čase. toodo, postupujte podle hello stejné kroky výše a umístěte **EventEnqueuedUtcTime** na ose x hello a **vlhkosti** na ose y hello.

   ![Přidat spojnicový graf pro vlhkosti tooa sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Klikněte na tlačítko **Uložit** toosave hello sestavy.
1. Klikněte na tlačítko **soubor** > **publikování tooweb**.
1. Klikněte na tlačítko **kód pro vložení vytvořit**a potom klikněte na **publikovat**.

Jste zadat odkaz na sestavu hello, které můžete sdílet s kýmkoli pro přístup k sestavě a sestavy hello toointegrate fragment kódu do blogu nebo Web.

![Publikovat sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Společnost Microsoft nabízí hello [mobilních aplikací Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) pro zobrazení a interakci s řídicí panely Power BI a sestavy na vašem mobilním zařízení.

## <a name="next-steps"></a>Další kroky

Úspěšně jste použili Power BI toovisualize snímačů v reálném čase data ze služby Azure IoT hub.
Nejsou jiný způsob, jak toovisualize data ze služby Azure IoT Hub. V tématu [dat snímačů v reálném čase pomocí Azure Web Apps toovisualize ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
