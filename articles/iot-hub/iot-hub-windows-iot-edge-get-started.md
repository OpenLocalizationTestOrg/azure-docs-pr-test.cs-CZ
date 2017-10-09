---
title: "aaaGet začít s Azure IoT okraj (Windows) | Microsoft Docs"
description: "Jak toobuild Azure IoT vstupní brána v systému Windows počítače a další informace o klíčových konceptů služby Azure IoT okraj jako jsou moduly a konfigurační soubory JSON."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5dd13cbfc02eeb55d9f2dbffca5021f2624acf14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Prozkoumejte Azure IoT Edge architektura v systému Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a>Jak toorun hello ukázka

Hello **build.cmd** skript generuje jeho výstup v hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště. Tento výstup zahrnuje hello dva okraje IoT moduly používané v této ukázce.

Hello sestavení skriptu místech **logger.dll** v hello **sestavení\\moduly\\protokolovač\\ladění** složky a **hello\_world.dll**  v hello **sestavení\\moduly\\hello_world\\ladění** složky. Použít tyto cesty pro hello **modulu cesta** hodnoty, jak je znázorněno v následující soubor JSON nastavení hello.

Hello hello\_world\_ukázka proces trvá hello konfigurační soubor JSON tooa cestu jako argument příkazového řádku. Hello následující příklad souboru JSON je součástí sady SDK úložiště hello na **ukázky\\hello\_world\\src\\hello\_world\_win.json**. Tato konfigurace soubor funguje jako je nezměníte hello vytvořit skript tooplace hello IoT Edge moduly nebo ukázkové spustitelné soubory v jiné než výchozí umístění.

> [!NOTE]
> cesty modulu Hello jsou relativní toohello adresáře, kde hello hello\_world\_sample.exe nachází. Ukázka Hello JSON konfigurační soubor výchozí hodnoty toowriting 'log.txt' v aktuální pracovní adresář.

```json
{
  "modules": [
    {
      "name": "logger",
      "loader": {
        "name": "native",
        "entrypoint": {
          "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
        }
      },
      "args": { "filename": "log.txt" }
    },
    {
      "name": "hello_world",
      "loader": {
        "name": "native",
        "entrypoint": {
          "module.path": "..\\..\\..\\modules\\hello_world\\Debug\\hello_world.dll"
        }
      },
      "args": null
      }
  ],
  "links": [
    {
      "source": "hello_world",
      "sink": "logger"
    }
  ]
}
```

1. Přejděte toohello **sestavení** složku v kořenovém adresáři hello místní kopii hello **iot hranou** úložiště.

1. Spusťte následující příkaz hello:

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
