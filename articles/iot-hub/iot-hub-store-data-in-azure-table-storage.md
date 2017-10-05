---
title: "Uložení zpráv IoT hub do úložiště dat Azure | Microsoft Docs"
description: "Pomocí aplikace Azure funkce IoT hub zprávy uložit Azure table Storage. IoT hub zprávy obsahují informace, například data snímačů, která je odeslána ze zařízení IoT."
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
ms.openlocfilehash: 06503f9564e00ef62587d02f2da4778974e246c5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-table-storage"></a>Uložit IoT hub zprávy, které obsahují data snímačů Azure table Storage

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Co se naučíte

Zjistíte, jak vytvořit účet úložiště Azure a Azure funkce aplikace na ukládání IoT Centrum zpráv ve službě table storage.

## <a name="what-you-do"></a>Co dělat

- Vytvoření účtu úložiště Azure
- Příprava připojení centra IoT umožní číst zprávy.
- Vytvořte a nasaďte aplikaci Azure funkce.

## <a name="what-you-need"></a>Co potřebujete

- [Nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) tak, aby pokrývalo následující požadavky:
  - Aktivní předplatné Azure
  - Služby IoT hub v rámci svého předplatného 
  - Spuštěné aplikace, která odesílá zprávy do služby IoT hub

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure

1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **úložiště** > **účet úložiště**  >   **Vytvoření**.

2. Zadejte informace potřebné pro účet úložiště:

   ![Vytvoření účtu úložiště na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Název**: název účtu úložiště. Název musí být globálně jedinečný.

   * **Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.

   * **Připnout na řídicí panel:** Vyberte tuto možnost pro snadný přístup k centru IoT z řídicího panelu.

3. Klikněte na možnost **Vytvořit**.

## <a name="prepare-your-iot-hub-connection-to-read-messages"></a>Příprava připojení centra IoT umožní číst zprávy

IoT hub zpřístupní koncový bod předdefinované událostí kompatibilní s centrem umožnit aplikacím umožní číst zprávy typu IoT hub. Mezitím aplikace používají skupiny uživatelů pro čtení dat ze služby IoT hub. Než vytvoříte aplikaci Azure funkce načíst data ze služby IoT hub, postupujte takto:

- Získáte připojovací řetězec koncový bod centra IoT.
- Vytvořte skupinu uživatelů pro službu IoT hub.

### <a name="get-the-connection-string-of-your-iot-hub-endpoint"></a>Získat připojovací řetězec koncový bod centra IoT

1. Otevřete službu IoT hub.

2. Na **IoT Hub** podokně v části **zasílání zpráv**, klikněte na tlačítko **koncové body**.

3. V pravém podokně v části **předdefinované koncové body**, klikněte na tlačítko **události**.

4. V **vlastnosti** podokně, pamatujte na následující hodnoty:
   - Koncový bod kompatibilní s centrem událostí
   - Název kompatibilní s centrem událostí

   ![Získat připojovací řetězec koncový bod centra IoT na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. V **IoT Hub** podokně v části **nastavení**, klikněte na tlačítko **zásady sdíleného přístupu**.

6. Klikněte na tlačítko **iothubowner**.

7. Poznámka: **primární klíč** hodnotu.

8. Připojovací řetězec koncový bod centra IoT vytvořte takto:

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > Nahraďte `<Event Hub-compatible endpoint>` a `<Primary key>` hodnotami, které jste si předtím poznamenali.

### <a name="create-a-consumer-group-for-your-iot-hub"></a>Vytvořte skupinu uživatelů pro službu IoT hub

1. Otevřete službu IoT hub.

2. V **IoT Hub** podokně v části **zasílání zpráv**, klikněte na tlačítko **koncové body**.

3. V pravém podokně v části **předdefinované koncové body**, klikněte na tlačítko **události**.

4. V **vlastnosti** podokně v části **skupiny příjemců**, zadejte název a poznamenejte si ho.

5. Klikněte na **Uložit**.

## <a name="create-and-deploy-an-azure-function-app"></a>Vytvoření a nasazení aplikace Azure – funkce

1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **výpočetní** > **aplikaci funkce**  >   **Vytvoření**.

2. Zadejte informace potřebné pro aplikaci funkce.

   ![Vytvoření aplikace pro funkce na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * **Název aplikace**: název aplikace funkce. Název musí být globálně jedinečný.

   * **Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.

   * **Účet úložiště**: účet úložiště, který jste vytvořili.

   * **Připnout na řídicí panel**: zaškrtnete tuto možnost pro snadný přístup k aplikaci funkce z řídicího panelu.

3. Klikněte na možnost **Vytvořit**.

4. Po vytvoření funkce aplikace, otevřete ho.

5. V aplikaci funkce a vytvořte novou funkci následujícím způsobem:

   a. Klikněte na tlačítko **novou funkci**.

   b. Vyberte **JavaScript** pro **jazyk**, a **zpracování dat** pro **scénář**.

   c. Klikněte na tlačítko **vytvořit tuto funkci**a potom klikněte na **novou funkci**.

   d. Vyberte **JavaScript** pro daný jazyk, a **zpracování dat** pro scénář.

   e. Klikněte **EventHubTrigger JavaScript** šablony.

   f. Zadejte informace potřebné pro šablonu.

      * **Název funkce**: název funkce.

      * **Název centra událostí**: název kompatibilní s centrem událostí, který jste si předtím poznamenali.

      * **Připojení rozbočovače událostí**: Chcete-li přidat připojovací řetězec koncový bod centra IoT, kterou jste vytvořili, klikněte na tlačítko **nový**.

   g. Klikněte na možnost **Vytvořit**.

6. Výstup funkce nakonfigurujte následujícím způsobem:

   a. Klikněte na tlačítko **integrovat** > **nový výstupní** > **Azure Table Storage** > **vyberte**.

      ![Přidání úložiště tabulek do funkce aplikace na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   b. Zadejte potřebné informace.

      * **Parametr název tabulky**: použití `outputTable`, který se používá v kódu funkci Azure.
      
      * **Název tabulky**: použití `deviceData`.

      * **Připojení účtu úložiště**: klikněte na tlačítko **nový**a potom vyberte nebo zadejte svůj účet úložiště. Pokud se nezobrazí, účet úložiště, najdete v části [požadavky na účet úložiště](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).
      
   c. Klikněte na **Uložit**.

7. V části **aktivační události**, klikněte na tlačítko **centra událostí Azure (eventHubMessages)**.

8. V části **skupiny příjemců centra událostí**, zadejte název skupiny uživatelů, který jste vytvořili a pak klikněte na tlačítko **Uložit**.

9. Klikněte na funkce, které jste vytvořili na levé straně a pak klikněte na **zobrazit soubory** na pravé straně.

10. Nahraďte kód v `index.js` následujícím kódem:

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in the IoT hub.
   // The message payload is persisted in an Azure storage table
 
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

Nyní jste vytvořili aplikaci funkce. Ukládá zprávy, které IoT hub přijímá ve službě table storage.

> [!NOTE]
> Můžete použít **spustit** tlačítko Testovat aplikaci funkce. Když kliknete na tlačítko **spustit**, je testovací zpráva odeslána do služby IoT hub. Přijetí zprávy by měly aktivovat aplikaci funkce spuštění a potom uložte zprávu table Storage. **Protokoly** podokně zaznamenává informace o procesu.

## <a name="verify-your-message-in-your-table-storage"></a>Ověřit zprávu ve službě table storage

1. Spuštění ukázkové aplikace na zařízení k odesílání zpráv do služby IoT hub.

2. [Stáhněte a nainstalujte Azure Storage Explorer](http://storageexplorer.com/).

3. Otevřete Storage Explorer, klikněte na **přidat účet Azure** > **přihlášení**a potom se přihlaste k účtu Azure.

4. Klikněte na předplatné Azure > **účty úložiště** > váš účet úložiště > **tabulky** > **deviceData**.

   Měli byste vidět zprávy odeslané ze zařízení do služby IoT hub přihlášení `deviceData` tabulky.

## <a name="next-steps"></a>Další kroky

Úspěšně jste vytvořili účet úložiště Azure a Azure funkce aplikace, která ukládá zprávy, které IoT hub přijímá ve službě table storage.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
