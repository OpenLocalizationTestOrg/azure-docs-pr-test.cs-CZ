---
title: "vizualizace dat aaaReal čas dat snímačů z Azure IoT hub – Web Apps | Microsoft Docs"
description: "Pomocí funkce hello webové aplikace Microsoft Azure App Service toovisualize teploty a vlhkosti dat, která se shromažďují ze snímačů hello a odesílá tooyour Iot hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vizualizace dat reálném čase, vizualizace dat za provozu, vizualizace dat snímačů"
ms.assetid: e42b07a8-ddd4-476e-9bfb-903d6b033e91
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 72f2dffee1c2f975948820eee9f2e287c3f77255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-hello-web-apps-feature-of-azure-app-service"></a>Vizualizovat data snímačů v reálném čase ze služby Azure IoT hub pomocí hello funkce Web Apps služby Azure App Service

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/5.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Co se naučíte

V tomto kurzu zjistíte, jak je toovisualize data snímačů v reálném čase, která IoT hub přijímá spuštěním webové aplikace hostované na webovou aplikaci. Pokud chcete tootry toovisualize hello data ve službě IoT hub pomocí Power BI, najdete v části [data snímačů v reálném čase pomocí Power BI toovisualize ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Co dělat

- Vytvoření webové aplikace v hello portálu Azure.
- Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.
- Nakonfigurujte data snímačů hello webové aplikace tooread ze služby IoT hub.
- Nahrajte toobe webové aplikace hostované hello webové aplikace.
- Otevřete hello webové aplikace toosee dat v reálném čase teploty a vlhkosti ze služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- [Nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md), která zahrnuje hello následující požadavky:
  - Aktivní předplatné Azure
  - Služby Iot hub v rámci svého předplatného
  - Klientská aplikace, která odesílá zprávy tooyour Iot hub
- [Stáhněte si Git](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Vytvoření webové aplikace

1. V hello [portál Azure](https://ms.portal.azure.com/), klikněte na tlačítko **nový** > **Web + mobilní** > **webové aplikace**.
2. Zadejte název jedinečné úlohy ověřte hello předplatné, zadejte skupinu prostředků a umístění, vyberte **Pin toodashboard**a potom klikněte na **vytvořit**.

   Doporučujeme vám, že jste vybrali hello stejné umístění jako u vaší skupiny prostředků. To pomáhá zajistit s rychlost zpracování a snižuje náklady na hello přenosu dat.

   ![Vytvoření webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-hello-web-app-tooread-data-from-your-iot-hub"></a>Nakonfigurovat hello webové aplikace tooread data ze služby IoT hub

1. Otevřete hello webovou aplikaci, kterou jste právě zřídili.
2. Klikněte na tlačítko **nastavení aplikace**a pak v části **nastavení aplikace**, přidejte následující páry klíč – hodnota hello:

   | Klíč                                   | Hodnota                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Získat z Průzkumníka iothub                                |
   | Azure.IoT.IoTHub.ConsumerGroup        | Hello název skupiny příjemců hello přidat tooyour IoT hub  |

   ![Přidat nastavení tooyour webové aplikace s páry klíč – hodnota](media/iot-hub-live-data-visualization-in-web-apps/4_web-app-settings-key-value-azure.png)

3. Klikněte na tlačítko **nastavení aplikace**v části **obecné nastavení**, přepnutí hello **webové sokety** a potom klikněte na **Uložit**.

   ![Přepněte možnost sockets webové hello](media/iot-hub-live-data-visualization-in-web-apps/10_toggle_web_sockets.png)

## <a name="upload-a-web-application-toobe-hosted-by-hello-web-app"></a>Nahrát toobe webové aplikace hostované hello webové aplikace

Na Githubu jsme provedli jsme k dispozici webové aplikace, které se zobrazují data snímačů v reálném čase ze služby IoT hub. Stačí toodo je hello webové aplikace toowork nakonfigurovat úložiště Git, stáhněte hello webové aplikace z webu GitHub a nahrajte ho tooAzure pro hello webové aplikace toohost.

1. V hello webové aplikace, klikněte na **možnosti nasazení** > **zvolit zdroj** > **místní úložiště Git**a pak klikněte na tlačítko **OK**.

   ![Konfigurace webové aplikace nasazení toouse hello místní úložiště Git](media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Klikněte na tlačítko **přihlašovací údaje nasazení**, vytvořte uživatelské jméno a heslo toouse tooconnect toohello úložiště Git v Azure a pak klikněte na tlačítko **Uložit**.

3. Klikněte na tlačítko **přehled**a poznamenejte si hodnotu hello **adresu url pro Git clone**.

   ![Získat adresu URL klonu Git hello vaší webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/7_web-app-git-clone-url-azure.png)

4. Otevřete okno terminálu v místním počítači nebo příkaz.

5. Stažení hello webové aplikace z webu GitHub a nahrajte ho tooAzure pro hello webové aplikace toohost. toodo tedy spusťte hello následující příkazy:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<Adresa URL klonu Git\> je hello URL úložiště Git hello najít na hello **přehled** stránku hello webové aplikace.

## <a name="open-hello-web-app-toosee-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Otevřete hello webové aplikace toosee dat v reálném čase teploty a vlhkosti ze služby IoT hub

Na hello **přehled** stránku vaší webové aplikace, klikněte na tlačítko hello URL tooopen hello webové aplikace.

![Získat adresu URL hello vaší webové aplikace](media/iot-hub-live-data-visualization-in-web-apps/8_web-app-url-azure.png)

Měli byste vidět hello v reálném čase teploty a vlhkosti data ze služby IoT hub.

![Webové stránky aplikace zobrazuje v reálném čase teploty a vlhkosti](media/iot-hub-live-data-visualization-in-web-apps/9_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Zkontrolujte, jestli hello ukázkové aplikace běží na vašem zařízení. Pokud není, zobrazí se prázdný graf, můžete se podívat toohello kurzy pod [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Další kroky
Úspěšně jste použili vaší webové aplikace toovisualize snímačů v reálném čase data ze služby IoT hub.

Toovisualize alternativní způsob data ze služby Azure IoT Hub, najdete v části [data snímačů v reálném čase pomocí Power BI toovisualize ze služby IoT hub](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
