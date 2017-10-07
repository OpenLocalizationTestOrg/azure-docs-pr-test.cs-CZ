---
title: "zasílání zpráv s platformou iothub-explorer cloud zařízení Azure IoT Hub pro aaaManage | Microsoft Docs"
description: "Zjistěte, jak toouse hello zprávy iothub-explorer rozhraní příkazového řádku nástroje toomonitor zařízení toocloud (D2C) a odesílat zprávy toodevice (C2D) cloudu v Azure IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "iothub Průzkumníku cloudu zařízení zasílání zpráv a toodevice cloudové Centrum iot, cloudu toodevice zasílání zpráv"
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 741383b5631714cc2fef3309541fd2117aa7824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-toosend-and-receive-messages-between-your-device-and-iot-hub"></a>Pomocí Průzkumníka iothub toosend a příjem zpráv mezi zařízením a IoT Hub

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[iothub-explorer](https://github.com/azure/iothub-explorer) má několik příkazů, který usnadňuje správu IoT Hub. Tento kurz se zaměřuje na toouse iothub-explorer toosend a příjem zpráv mezi zařízením a služby IoT hub.

## <a name="what-you-will-learn"></a>Co se dozvíte

Zjistíte, jak toouse iothub-explorer toomonitor zařízení cloud zprávy a zprávy toosend cloud zařízení. Zprávy typu zařízení cloud může být data snímačů, aby vaše zařízení shromažďuje a poté odešle tooyour IoT hub. Zprávy typu cloud zařízení může být příkazy odešle službě IoT hub tooyour zařízení tooblink DIODU, který je připojený tooyour zařízení.

## <a name="what-you-will-do"></a>Co provedete

- Pomocí zpráv typu zařízení cloud toomonitor iothub-explorer.
- Pomocí zpráv typu cloud zařízení toosend iothub-explorer.

## <a name="what-you-need"></a>Co potřebujete

- Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:
  - Aktivní předplatné Azure.
  - V rámci svého předplatného služby Azure IoT hub.
  - Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.
- iothub-explorer. ([Nainstalovat iothub-explorer](https://github.com/azure/iothub-explorer))

## <a name="monitor-device-to-cloud-messages"></a>Sledování zpráv typu zařízení cloud

toomonitor zprávy, které jsou odesílány z tooyour zařízení IoT hub, postupujte takto:

1. Otevřete okno konzoly.
1. Spusťte následující příkaz hello:

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > Získat `<device-id>` a `<IoTHubConnectionString>` ze služby IoT hub. Ujistěte se, že jste dokončili kurz předchozí hello. Nebo můžete zkusit toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` Pokud máte `HostName`, `SharedAccessKeyName` a `SharedAccessKey`.

## <a name="send-cloud-to-device-messages"></a>Odesílání zpráv z cloudu do zařízení

toosend zprávy ze zařízení IoT hub tooyour postupujte takto:

1. Otevřete okno konzoly.
1. Spusťte relaci ve službě IoT hub spuštěním hello následující příkaz:

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. Odeslat zprávu tooyour zařízení tak, že spustíte následující příkaz hello:

   ```bash
   iothub-explorer send <device-id> <message>
   ```

příkaz Hello bliká hello DIODU tooyour připojených zařízení a odešle hello zpráva tooyour zařízení.

> [!Note]
> Není nutné pro hello zařízení toosend samostatné potvrzení příkazu back tooyour IoT hub po přijetí zprávy hello.

## <a name="next-steps"></a>Další kroky

Když jste se naučili jak toomonitor zařízení cloud zprávy a odesílání zpráv typu cloud zařízení mezi zařízení IoT a Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
