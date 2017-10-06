---
title: "aaaIoT vzdálené monitorování a oznámení službou Azure Logic Apps | Microsoft Docs"
description: "Použití Azure Logic Apps pro IoT teploty monitorování ve službě IoT hub a automaticky odesílat e-mailové oznámení tooyour schránky pro případných zjištěných."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT iot oznámení monitorování monitorování teploty iot"
ms.assetid: 43043067-2e1f-42c9-953d-e2dce8fd86df
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 89396528ed63c37258e1b49f342f0723e686ecb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>IoT pro vzdálené monitorování a oznámení službou Azure Logic Apps připojení služby IoT hub a poštovní schránky

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Azure Logic Apps poskytuje způsob tooautomate procesy jako řadu kroků. Aplikace logiky můžete připojit přes různé služby a protokoly. Začíná aktivační událost, jako "Při přidání účtu" a a kombinace akcí, jako "odesílat nabízená oznámení". Díky této funkci Logic Apps ideální řešení IoT pro IoT monitorování, jako je například zachování výstrahy anomálií mezi scénáře použití.

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak toocreate aplikace logiky, která se připojuje služby IoT hub a poštovní schránky pro monitorování teploty a oznámení. Když hello je nad 30 C, hello klienta aplikace značky `temperatureAlert = "true"` ve zprávě hello odešle tooyour IoT hub. Hello zprávy aktivačních událostí hello toosend aplikace logiky můžete e-mailové oznámení.

## <a name="what-you-do"></a>Co dělat

* Vytvořit obor názvů sběrnice služby a přidejte tooit fronty.
* Přidáte koncový bod a směrování pravidlo tooyour IoT hub.
* Vytvoření, konfiguraci a testování aplikace logiky.

## <a name="what-you-need"></a>Co potřebujete

* Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:
  * Aktivní předplatné Azure.
  * V rámci svého předplatného služby Azure IoT hub.
  * Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.

## <a name="create-service-bus-namespace-and-add-a-queue-tooit"></a>Vytvoření oboru názvů service bus a přidejte tooit fronty

### <a name="create-a-service-bus-namespace"></a>Vytvořit obor názvů sběrnice služby

1. Na hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **Enterprise integrace** > **Service Bus**.
1. Zadejte hello následující informace:

   **Název**: název hello hello service Bus.

   **Cenová úroveň**: klikněte na tlačítko **základní** > **vyberte**. úroveň Basic Hello je dostačující pro účely tohoto kurzu.

   **Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.

   **Umístění**: použití hello stejné umístění, které používá služby IoT hub.
1. Klikněte na možnost **Vytvořit**.

   ![Vytvořit obor názvů sběrnice služby v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Přidat frontou service bus

1. Otevřete obor názvů sběrnice hello a pak klikněte na tlačítko **+ fronty**.
1. Zadejte název pro frontu hello a pak klikněte na tlačítko **vytvořit**.
1. Otevřete hello frontou service bus a pak klikněte na tlačítko **zásady sdíleného přístupu** > **+ přidat**.
1. Zadejte název zásady hello, zkontrolujte **spravovat**a potom klikněte na **vytvořit**.

   ![Přidání frontou service bus v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-tooyour-iot-hub"></a>Přidat koncový bod a směrování pravidlo tooyour IoT hub

### <a name="add-an-endpoint"></a>Přidání koncového bodu

1. Otevřete službu IoT hub, klikněte na koncové body > + Přidat.
1. Zadejte hello následující informace:

   **Název**: hello název koncového bodu hello.

   **Typ koncového bodu**: vyberte **frontou Service Bus**.

   **Obor názvů sběrnice**: Vyberte hello obor názvů, které jste vytvořili.

   **Fronty Service Bus**: Vyberte hello fronty, které jste vytvořili.
1. Klikněte na **OK**.

   ![Přidání koncového bodu tooyour IoT hub v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Přidat pravidlo směrování

1. Ve službě IoT hub, klikněte na tlačítko **trasy** > **+ přidat**.
1. Zadejte hello následující informace:

   **Název**: hello název pravidla směrování hello.

   **Zdroj dat**: vyberte **DeviceMessages**.

   **Koncový bod**: Vyberte koncový bod hello jste vytvořili.

   **Řetězec dotazu**: Zadejte `temperatureAlert = "true"`.
1. Klikněte na **Uložit**.

   ![Přidat pravidlo směrování v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Vytvoření a konfigurace aplikace logiky

### <a name="create-a-logic-app"></a>Vytvoření aplikace logiky

1. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **Enterprise integrace** > **aplikace logiky**.
1. Zadejte hello následující informace:

   **Název**: název hello hello logiku aplikace.

   **Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.

   **Umístění**: použití hello stejné umístění, které používá služby IoT hub.
1. Klikněte na možnost **Vytvořit**.

### <a name="configure-hello-logic-app"></a>Konfigurace aplikace logiky hello

1. Otevřete aplikaci logiky hello, které se otevře do hello návrhář aplikace logiky.
1. V hello návrhář aplikace logiky, klikněte na **prázdné aplikace logiky**.

   ![Začít s prázdnou logiku aplikace v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Klikněte na tlačítko **Service Bus**.

   ![Vyberte služby Service Bus toostart vytvoření aplikace logiky v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Klikněte na tlačítko **Service Bus – při obdržení jeden nebo více zpráv ve frontě (automatické dokončování)**.
1. Umožňuje vytvořte připojení service bus.
   1. Zadejte název připojení.
   1. Klikněte na obor názvů sběrnice hello > hello service bus zásady > **vytvořit**.

      ![Vytvořte připojení sběrnice služby pro svou aplikaci logiky v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Klikněte na tlačítko **pokračovat** po vytvoření připojení sběrnice služby hello.
   1. Vyberte hello fronty, kterou jste vytvořili a zadejte `175` pro **maximální počet zpráv**

      ![Zadejte počet hello maximální zpráv pro připojení sběrnice služby hello v aplikaci logiky](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Klikněte na "tlačítko toosave hello změny uložit".

1. Vytvoření připojení služby SMTP.
   1. Klikněte na tlačítko **nový krok** > **přidat akci**.
   1. Typ `SMTP`, klikněte na tlačítko hello **SMTP** služby ve výsledku hledání hello a pak klikněte na **SMTP - odeslat E-mail**.

      ![Vytvoření připojení k SMTP ve vaší aplikaci logiky v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Zadejte informace hello SMTP poštovní schránky a pak klikněte na tlačítko **vytvořit**.

      ![Zadejte informace o připojení protokolu SMTP ve vaší aplikaci logiky v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Získání informací o hello SMTP pro [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [z Gmailu](https://support.google.com/a/answer/176600?hl=en), a [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).
   1. Zadejte e-mailovou adresu pro **z** a **k**, a `High temperature detected` pro **subjektu** a **textu**.
   1. Klikněte na **Uložit**.

aplikace logiky Hello je ve funkčním stavu, když jste jej uložili.

## <a name="test-hello-logic-app"></a>Testování aplikace logiky hello

1. Spustit aplikaci hello klienta, který nasazujete tooyour zařízení v [připojit ESP8266 tooAzure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).
1. Zvýšení teploty prostředí hello kolem hello SensorTag toobe nad 30 C. Například light svíčkového grafu okolo vaší SensorTag.
1. Měli byste obdržet e-mail s oznámením poslal aplikace logiky hello.

   > [!NOTE]
   > Poskytovatel služeb e-mailu může být nutné tooverify hello odesílatele identity toomake se, že se že jedná o vás, který odešle e-mail hello.

## <a name="next-steps"></a>Další kroky

Úspěšně jste vytvořili aplikaci logiky, která se připojuje služby IoT hub a poštovní schránky pro monitorování teploty a oznámení.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
