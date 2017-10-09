---
title: "aaaGet začít s Azure IoT okraj (Linux) | Microsoft Docs"
description: "Jak toobuild brány na systémem Linux počítače a další informace o klíčových konceptů služby Azure IoT okraj jako jsou moduly a konfigurační soubory JSON."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40aa9c8ddca6a974c361cbb0b453c7d0ddc71b8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Prozkoumání architektury Azure IoT Edge v Linuxu

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a>Jak toorun hello ukázka

Hello **build.sh** skript generuje jeho výstup v hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště. Tento výstup zahrnuje hello dva okraje IoT moduly používané v této ukázce.

Hello sestavení skriptu místech **liblogger.so** v hello **sestavení nebo moduly nebo protokolovacího nástroje/** složky a **libhello\_world.so** v hello **sestavení nebo moduly nebohello_world/** složky. Použít tyto cesty pro hello **modulu cesta** hodnoty, jak ukazuje následující příklad JSON nastavení souboru hello.

Hello hello\_world\_ukázka proces trvá hello cesta tooa JSON konfigurační soubor argument příkazového řádku. Hello následující příklad souboru JSON je součástí sady SDK úložiště hello na **ukázky/hello\_world/src/hello\_world\_lin.json**. Tato konfigurace soubor funguje jako je nezměníte hello vytvořit skript tooplace hello IoT Edge moduly nebo ukázkové spustitelné soubory v jiné než výchozí umístění.

> [!NOTE]
> Hello modulu cest relativní toohello aktuální pracovní adresář odkud hello hello\_world\_spuštění ukázkové spustitelný soubor, není hello adresáře, kde je umístěný hello spustitelný soubor. Ukázka Hello JSON konfigurační soubor výchozí hodnoty toowriting 'log.txt' v aktuální pracovní adresář.

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. Přejděte toohello **sestavení** složku v kořenovém adresáři hello místní kopii hello **iot hranou** úložiště.

1. Spusťte následující příkaz hello:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

