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
# <a name="explore-azure-iot-edge-architecture-on-linux"></a><span data-ttu-id="f34cb-103">Prozkoumání architektury Azure IoT Edge v Linuxu</span><span class="sxs-lookup"><span data-stu-id="f34cb-103">Explore Azure IoT Edge architecture on Linux</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="f34cb-104">Jak toorun hello ukázka</span><span class="sxs-lookup"><span data-stu-id="f34cb-104">How toorun hello sample</span></span>

<span data-ttu-id="f34cb-105">Hello **build.sh** skript generuje jeho výstup v hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="f34cb-105">hello **build.sh** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="f34cb-106">Tento výstup zahrnuje hello dva okraje IoT moduly používané v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="f34cb-106">This output includes hello two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="f34cb-107">Hello sestavení skriptu místech **liblogger.so** v hello **sestavení nebo moduly nebo protokolovacího nástroje/** složky a **libhello\_world.so** v hello **sestavení nebo moduly nebohello_world/** složky.</span><span class="sxs-lookup"><span data-stu-id="f34cb-107">hello build script places **liblogger.so** in hello **build/modules/logger/** folder and **libhello\_world.so** in hello **build/modules/hello_world/** folder.</span></span> <span data-ttu-id="f34cb-108">Použít tyto cesty pro hello **modulu cesta** hodnoty, jak ukazuje následující příklad JSON nastavení souboru hello.</span><span class="sxs-lookup"><span data-stu-id="f34cb-108">Use these paths for hello **module path** values as shown in hello following example JSON settings file.</span></span>

<span data-ttu-id="f34cb-109">Hello hello\_world\_ukázka proces trvá hello cesta tooa JSON konfigurační soubor argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="f34cb-109">hello hello\_world\_sample process takes hello path tooa JSON configuration file a command-line argument.</span></span> <span data-ttu-id="f34cb-110">Hello následující příklad souboru JSON je součástí sady SDK úložiště hello na **ukázky/hello\_world/src/hello\_world\_lin.json**.</span><span class="sxs-lookup"><span data-stu-id="f34cb-110">hello following example JSON file is provided in hello SDK repository at **samples/hello\_world/src/hello\_world\_lin.json**.</span></span> <span data-ttu-id="f34cb-111">Tato konfigurace soubor funguje jako je nezměníte hello vytvořit skript tooplace hello IoT Edge moduly nebo ukázkové spustitelné soubory v jiné než výchozí umístění.</span><span class="sxs-lookup"><span data-stu-id="f34cb-111">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="f34cb-112">Hello modulu cest relativní toohello aktuální pracovní adresář odkud hello hello\_world\_spuštění ukázkové spustitelný soubor, není hello adresáře, kde je umístěný hello spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="f34cb-112">hello module paths are relative toohello current working directory from where hello hello\_world\_sample executable is launched, not hello directory where hello executable is located.</span></span> <span data-ttu-id="f34cb-113">Ukázka Hello JSON konfigurační soubor výchozí hodnoty toowriting 'log.txt' v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="f34cb-113">hello sample JSON configuration file defaults toowriting 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="f34cb-114">Přejděte toohello **sestavení** složku v kořenovém adresáři hello místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="f34cb-114">Navigate toohello **build** folder in hello root of your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="f34cb-115">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f34cb-115">Run hello following command:</span></span>

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

