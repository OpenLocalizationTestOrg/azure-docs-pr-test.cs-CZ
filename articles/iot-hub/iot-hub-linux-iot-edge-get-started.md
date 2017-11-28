---
title: "Začínáme s Azure IoT okraj (Linux) | Microsoft Docs"
description: "Postup vytvoření brány na počítači s Linuxem a popis klíčových konceptů v Azure IoT Edge, jako jsou moduly a konfigurační soubory JSON."
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
ms.openlocfilehash: b02d79fcd9cd2a2ef0041aac4e85528263c8d58a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a><span data-ttu-id="a76be-103">Prozkoumání architektury Azure IoT Edge v Linuxu</span><span class="sxs-lookup"><span data-stu-id="a76be-103">Explore Azure IoT Edge architecture on Linux</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="a76be-104">Spuštění ukázky</span><span class="sxs-lookup"><span data-stu-id="a76be-104">How to run the sample</span></span>

<span data-ttu-id="a76be-105">Skript **build.sh** generuje výstup ve složce **build** v místní kopii úložiště **iot-edge**.</span><span class="sxs-lookup"><span data-stu-id="a76be-105">The **build.sh** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="a76be-106">Tento výstup zahrnuje dvě IoT Edge moduly používané v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="a76be-106">This output includes the two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="a76be-107">Skript sestavení umístí modul **liblogger.so** do složky **build/modules/logger/** a modul **libhello\_world.so** do složky **build/modules/hello_world/**.</span><span class="sxs-lookup"><span data-stu-id="a76be-107">The build script places **liblogger.so** in the **build/modules/logger/** folder and **libhello\_world.so** in the **build/modules/hello_world/** folder.</span></span> <span data-ttu-id="a76be-108">Použít tyto cesty pro **modulu cesta** hodnoty, jak je znázorněno v následujícím příkladu JSON nastavení souboru.</span><span class="sxs-lookup"><span data-stu-id="a76be-108">Use these paths for the **module path** values as shown in the following example JSON settings file.</span></span>

<span data-ttu-id="a76be-109">Proces hello\_world\_sample přijímá jako argument příkazového řádku cestu ke konfiguračnímu souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="a76be-109">The hello\_world\_sample process takes the path to a JSON configuration file a command-line argument.</span></span> <span data-ttu-id="a76be-110">Následující ukázkový soubor JSON je součástí úložiště sady SDK v umístění **samples/hello\_world/src/hello\_world\_lin.json**.</span><span class="sxs-lookup"><span data-stu-id="a76be-110">The following example JSON file is provided in the SDK repository at **samples/hello\_world/src/hello\_world\_lin.json**.</span></span> <span data-ttu-id="a76be-111">Tento konfigurační soubor funguje, jako je nezměníte skriptu buildu umístit IoT Edge moduly nebo ukázka spustitelné soubory v jiné než výchozí umístění.</span><span class="sxs-lookup"><span data-stu-id="a76be-111">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="a76be-112">Cesty k modulům jsou relativní vzhledem k aktuálnímu pracovnímu adresáři, ze kterého je spuštěn spustitelný soubor hello\_world\_sample, ne k adresáři, ve kterém je spustitelný soubor umístěn.</span><span class="sxs-lookup"><span data-stu-id="a76be-112">The module paths are relative to the current working directory from where the hello\_world\_sample executable is launched, not the directory where the executable is located.</span></span> <span data-ttu-id="a76be-113">Ukázkový konfigurační soubor JSON ve výchozím nastavení zapíše do aktuálního pracovního adresáře soubor log.txt.</span><span class="sxs-lookup"><span data-stu-id="a76be-113">The sample JSON configuration file defaults to writing 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="a76be-114">Přejděte na **sestavení** složku v kořenovém místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="a76be-114">Navigate to the **build** folder in the root of your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="a76be-115">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a76be-115">Run the following command:</span></span>

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

