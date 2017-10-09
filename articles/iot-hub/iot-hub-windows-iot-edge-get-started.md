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
# <a name="explore-azure-iot-edge-architecture-on-windows"></a><span data-ttu-id="3ef1d-103">Prozkoumejte Azure IoT Edge architektura v systému Windows</span><span class="sxs-lookup"><span data-stu-id="3ef1d-103">Explore Azure IoT Edge architecture on Windows</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="3ef1d-104">Jak toorun hello ukázka</span><span class="sxs-lookup"><span data-stu-id="3ef1d-104">How toorun hello sample</span></span>

<span data-ttu-id="3ef1d-105">Hello **build.cmd** skript generuje jeho výstup v hello **sestavení** složky ve vaší místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-105">hello **build.cmd** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="3ef1d-106">Tento výstup zahrnuje hello dva okraje IoT moduly používané v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-106">This output includes hello two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="3ef1d-107">Hello sestavení skriptu místech **logger.dll** v hello **sestavení\\moduly\\protokolovač\\ladění** složky a **hello\_world.dll**  v hello **sestavení\\moduly\\hello_world\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-107">hello build script places **logger.dll** in hello **build\\modules\\logger\\Debug** folder and **hello\_world.dll** in hello **build\\modules\\hello_world\\Debug** folder.</span></span> <span data-ttu-id="3ef1d-108">Použít tyto cesty pro hello **modulu cesta** hodnoty, jak je znázorněno v následující soubor JSON nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-108">Use these paths for hello **module path** values as shown in hello following JSON settings file.</span></span>

<span data-ttu-id="3ef1d-109">Hello hello\_world\_ukázka proces trvá hello konfigurační soubor JSON tooa cestu jako argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-109">hello hello\_world\_sample process takes hello path tooa JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="3ef1d-110">Hello následující příklad souboru JSON je součástí sady SDK úložiště hello na **ukázky\\hello\_world\\src\\hello\_world\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-110">hello following example JSON file is provided in hello SDK repository at **samples\\hello\_world\\src\\hello\_world\_win.json**.</span></span> <span data-ttu-id="3ef1d-111">Tato konfigurace soubor funguje jako je nezměníte hello vytvořit skript tooplace hello IoT Edge moduly nebo ukázkové spustitelné soubory v jiné než výchozí umístění.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-111">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="3ef1d-112">cesty modulu Hello jsou relativní toohello adresáře, kde hello hello\_world\_sample.exe nachází.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-112">hello module paths are relative toohello directory where hello hello\_world\_sample.exe is located.</span></span> <span data-ttu-id="3ef1d-113">Ukázka Hello JSON konfigurační soubor výchozí hodnoty toowriting 'log.txt' v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-113">hello sample JSON configuration file defaults toowriting 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="3ef1d-114">Přejděte toohello **sestavení** složku v kořenovém adresáři hello místní kopii hello **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="3ef1d-114">Navigate toohello **build** folder in hello root of your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="3ef1d-115">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="3ef1d-115">Run hello following command:</span></span>

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
