---
title: "aaaSave tooAzure datové úložiště zprávy služby IoT hub | Microsoft Docs"
description: "Pomocí služby Azure funkce aplikace toosave vaše IoT hub zprávy tooyour úložiště tabulek Azure. Hello IoT hub zprávy obsahují informace, například data snímačů, která je odeslána ze zařízení IoT."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "úložiště dat IOT, úložiště dat snímačů iot"
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: be72d9ba9a781822926364351b50f58f5d96e94a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-tooyour-azure-table-storage"></a>Uložit IoT hub zprávy, které obsahují úložiště tabulek Azure tooyour data snímačů

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak toocreate účet úložiště Azure a Azure funkce aplikace toostore IoT Centrum zpráv ve službě table storage.

## <a name="what-you-do"></a>Co dělat

- Vytvoření účtu úložiště Azure
- Příprava služby IoT hub zprávy tooread připojení.
- Vytvořte a nasaďte aplikaci Azure funkce.

## <a name="what-you-need"></a>Co potřebujete

- [Nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello následující požadavky:
  - Aktivní předplatné Azure
  - Služby IoT hub v rámci svého předplatného 
  - Spuštěné aplikace, která odesílá zprávy tooyour IoT hub

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure

1. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **úložiště** > **účet úložiště**  >   **Vytvoření**.

2. Zadejte hello potřebné informace pro účet úložiště hello:

   ![Vytvořit účet úložiště v hello portálu Azure](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Název**: hello název účtu úložiště hello. Název Hello musí být globálně jedinečný.

   * **Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.

   * **PIN kód toodashboard**: tuto možnost vyberte pro Centrum IoT tooyour snadný přístup z řídicího panelu hello.

3. Klikněte na možnost **Vytvořit**.

## <a name="prepare-your-iot-hub-connection-tooread-messages"></a>Příprava tooread zprávy připojení ke službě IoT hub

IoT hub zpřístupní předdefinovaný koncový bod kompatibilní s centrem tooenable aplikace tooread IoT hub zprávy o událostech. Mezitím aplikace využívat uživatelských skupin tooread dat ze služby IoT hub. Než vytvoříte model Azure funkce aplikace tooread data ze služby IoT hub, hello následující:

- Získáte připojovací řetězec hello koncový bod centra IoT.
- Vytvořte skupinu uživatelů pro službu IoT hub.

### <a name="get-hello-connection-string-of-your-iot-hub-endpoint"></a>Získat připojovací řetězec hello koncový bod centra IoT

1. Otevřete službu IoT hub.

2. Na hello **IoT Hub** podokně v části **zasílání zpráv**, klikněte na tlačítko **koncové body**.

3. V hello pravým podokně v části **předdefinované koncové body**, klikněte na tlačítko **události**.

4. V hello **vlastnosti** podokně, Poznámka hello následující hodnoty:
   - Koncový bod kompatibilní s centrem událostí
   - Název kompatibilní s centrem událostí

   ![Získat připojovací řetězec hello koncový bod centra IoT v hello portálu Azure](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. V hello **IoT Hub** podokně v části **nastavení**, klikněte na tlačítko **zásady sdíleného přístupu**.

6. Klikněte na tlačítko **iothubowner**.

7. Poznámka: hello **primární klíč** hodnotu.

8. Připojovací řetězec hello koncový bod centra IoT vytvořte takto:

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > Nahraďte `<Event Hub-compatible endpoint>` a `<Primary key>` hello hodnotami, které jste si předtím poznamenali.

### <a name="create-a-consumer-group-for-your-iot-hub"></a>Vytvořte skupinu uživatelů pro službu IoT hub

1. Otevřete službu IoT hub.

2. V hello **IoT Hub** podokně v části **zasílání zpráv**, klikněte na tlačítko **koncové body**.

3. V hello pravým podokně v části **předdefinované koncové body**, klikněte na tlačítko **události**.

4. V hello **vlastnosti** podokně v části **skupiny příjemců**, zadejte název a poznamenejte si ho.

5. Klikněte na **Uložit**.

## <a name="create-and-deploy-an-azure-function-app"></a>Vytvoření a nasazení aplikace Azure – funkce

1. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **výpočetní** > **aplikaci funkce**  >   **Vytvoření**.

2. Zadejte nezbytné informace hello hello funkce aplikace.

   ![Vytvoření aplikace pro funkce v hello portálu Azure](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * **Název aplikace**: název hello hello funkce aplikace. Název Hello musí být globálně jedinečný.

   * **Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.

   * **Účet úložiště**: hello účet úložiště, který jste vytvořili.

   * **PIN kód toodashboard**: zaškrtnete tuto možnost pro aplikaci funkce toohello snadný přístup z řídicího panelu hello.

3. Klikněte na možnost **Vytvořit**.

4. Po vytvoření hello funkce aplikace, otevřete ho.

5. V aplikaci funkce hello vytvořte novou funkci pomocí tohoto postupu hello následující:

   a. Klikněte na tlačítko **novou funkci**.

   b. Vyberte **JavaScript** pro **jazyk**, a **zpracování dat** pro **scénář**.

   c. Klikněte na tlačítko **vytvořit tuto funkci**a potom klikněte na **novou funkci**.

   d. Vyberte **JavaScript** pro jazyk hello a **zpracování dat** pro scénář hello.

   e. Klikněte na tlačítko hello **EventHubTrigger JavaScript** šablony.

   f. Zadejte nezbytné informace hello hello šablony.

      * **Název funkce**: hello název funkce hello.

      * **Název centra událostí**: hello název kompatibilní s centrem událostí, který jste si předtím poznamenali.

      * **Připojení rozbočovače událostí**: tooadd hello připojovací řetězec hello koncový bod centra IoT kterou jste vytvořili, klikněte na tlačítko **nový**.

   g. Klikněte na možnost **Vytvořit**.

6. Nakonfigurujte výstup hello funkce pomocí hello následující:

   a. Klikněte na tlačítko **integrovat** > **nový výstupní** > **Azure Table Storage** > **vyberte**.

      ![Přidání tabulky úložiště tooyour funkce aplikace v hello portálu Azure](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   b. Zadejte nezbytné informace hello.

      * **Parametr název tabulky**: použití `outputTable`, který se používá v hello Azure kód funkce.
      
      * **Název tabulky**: použití `deviceData`.

      * **Připojení účtu úložiště**: klikněte na tlačítko **nový**a potom vyberte nebo zadejte svůj účet úložiště. Pokud se nezobrazí hello účet úložiště, najdete v části [požadavky na účet úložiště](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).
      
   c. Klikněte na **Uložit**.

7. V části **aktivační události**, klikněte na tlačítko **centra událostí Azure (eventHubMessages)**.

8. V části **skupiny příjemců centra událostí**, zadejte název hello skupiny hello příjemce, který jste vytvořili a pak klikněte na tlačítko **Uložit**.

9. Klikněte na tlačítko hello funkce, které jste vytvořili na hello vlevo a pak klikněte na **zobrazit soubory** na hello správné.

10. Nahraďte kód hello v `index.js` s hello následující:

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in hello IoT hub.
   // hello message payload is persisted in an Azure storage table
 
   module.exports = function (context, iotHubMessage) {
    context.log('Message received: ' + JSON.stringify(iotHubMessage));
    var date = Date.now();
    var partitionKey = Math.floor(date / (24 * 60 * 60 * 1000)) + '';
    var rowKey = date + '';
    context.bindings.outputTable = {
     "partitionKey": partitionKey,
     "rowKey": rowKey,
     "message": JSON.stringify(iotHubMessage)
    };
    context.done();
   };
   ```

11. Klikněte na **Uložit**.

Nyní jste vytvořili aplikaci funkce hello. Ukládá zprávy, které IoT hub přijímá ve službě table storage.

> [!NOTE]
> Můžete použít hello **spustit** tlačítko tootest hello funkce aplikace. Když kliknete na tlačítko **spustit**, hello zkušební zprávu se odeslat tooyour IoT hub. Hello doručení zprávy hello by měly aktivovat hello funkce aplikace toostart a potom uložte hello zpráva tooyour tabulka úložiště. Hello **protokoly** podokně zaznamenává hello podrobnosti o procesu hello.

## <a name="verify-your-message-in-your-table-storage"></a>Ověřit zprávu ve službě table storage

1. Spusťte hello ukázkovou aplikaci na zařízení toosend zprávy tooyour IoT hub.

2. [Stáhněte a nainstalujte Azure Storage Explorer](http://storageexplorer.com/).

3. Otevřete Storage Explorer, klikněte na **přidat účet Azure** > **přihlášení**a potom se přihlaste tooyour účet Azure.

4. Klikněte na předplatné Azure > **účty úložiště** > váš účet úložiště > **tabulky** > **deviceData**.

   Měli byste vidět zprávy odeslané ze zařízení IoT hub tooyour přihlášení hello `deviceData` tabulky.

## <a name="next-steps"></a>Další kroky

Úspěšně jste vytvořili účet úložiště Azure a Azure funkce aplikace, která ukládá zprávy, které IoT hub přijímá ve službě table storage.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
