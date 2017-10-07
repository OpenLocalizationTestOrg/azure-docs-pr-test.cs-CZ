---
title: "nahrávání souborů Azure portálu tooconfigure hello aaaUse | Microsoft Docs"
description: "Jak toouse hello Azure portálu tooconfigure souboru IoT hub tooenable ukládání z připojených zařízení. Obsahuje informace o konfiguraci hello, cílový účet úložiště Azure."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: b90c3fbed47b4eb144d3cb7480068b7cfc776ba6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-hello-azure-portal"></a>Konfigurace služby IoT Hub nahrávání souborů pomocí hello portálu Azure

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Nahrávání souborů

toouse hello [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet služby Azure Storage pomocí centra. Vyberte **nahrávání souborů** toodisplay seznam vlastností nahrávání souboru pro hello IoT hub, která je upravována.

![Nastavení portálu hello odesílání souboru IoT Hub zobrazení][13]

**Kontejner úložiště**: použití hello Azure portálu tooselect kontejner objektů blob v účtu Azure Storage ve vaší aktuální předplatné tooassociate službou IoT Hub. Pokud potřeby můžete vytvořit účet úložiště Azure na hello **účty úložiště** okno a objektů blob kontejneru na hello **kontejnery** okno. Identifikátory URI SAS služby IoT Hub automaticky generuje s zápisu oprávnění toothis kontejner objektů blob pro toouse zařízení, když odesílají soubory.

![Zobrazení portálu hello kontejnery úložiště pro nahrávání souborů][14]

**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru prostřednictvím přepnutí hello.

**Hodnota TTL SAS**: Toto nastavení je hello time-to-live z hello identifikátory URI SAS vrácený toohello zařízení IoT Hub. Ve výchozím nastavení tooone hodinu, ale může být vlastní tooother hodnoty pomocí posuvníku hello.

**Upozornění na nastavení výchozí hodnota TTL souboru**: hello time-to-live oznámení nahrávání souboru předtím, než vyprší jejich platnost. Ve výchozím nastavení tooone den, ale může být vlastní tooother hodnoty pomocí posuvníku hello.

**Soubor doručení maximální počet oznámení**: hello kolikrát hello toodeliver pokusy o IoT Hub oznámení nahrávání souboru. Ve výchozím nastavení too10, ale může být vlastní tooother hodnoty pomocí posuvníku hello.

![Konfigurace služby IoT Hub nahrávání souborů hello portálu][15]

## <a name="next-steps"></a>Další kroky

Další informace o možnostech nahrávání souboru hello Centrum IoT najdete v tématu [nahrání souborů ze zařízení] [ lnk-upload] v hello Příručka vývojáře pro službu IoT Hub.

Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:

* [Hromadné spravovat zařízení IoT][lnk-bulk]
* [Metriky služby IoT Hub][lnk-metrics]
* [Monitorování operací][lnk-monitor]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s hranou IoT][lnk-iotedge]
* [Zabezpečení řešení IoT z hello pozadí][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
