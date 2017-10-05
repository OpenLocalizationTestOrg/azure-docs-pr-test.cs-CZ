---
title: "Začínáme s Azure IoT okraj (Windows) | Microsoft Docs"
description: "Postup vytvoření Azure IoT vstupní brána na počítači s Windows a další informace o klíčových konceptů služby Azure IoT okraj jako jsou moduly a konfigurační soubory JSON."
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
ms.openlocfilehash: 5db39bab8e31a8e7026b34e72b4614b0f6f57772
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a><span data-ttu-id="812d1-103">Prozkoumejte Azure IoT Edge architektura v systému Windows</span><span class="sxs-lookup"><span data-stu-id="812d1-103">Explore Azure IoT Edge architecture on Windows</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="812d1-104">Spuštění ukázky</span><span class="sxs-lookup"><span data-stu-id="812d1-104">How to run the sample</span></span>

<span data-ttu-id="812d1-105">**Build.cmd** skript generuje jeho výstup v **sestavení** složky ve vaší místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="812d1-105">The **build.cmd** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="812d1-106">Tento výstup zahrnuje dvě IoT Edge moduly používané v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="812d1-106">This output includes the two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="812d1-107">Místech skriptu sestavení **logger.dll** v **sestavení\\moduly\\protokolovacího nástroje\\ladění** složky a **hello\_world.dll** v **sestavení\\moduly\\hello_world\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="812d1-107">The build script places **logger.dll** in the **build\\modules\\logger\\Debug** folder and **hello\_world.dll** in the **build\\modules\\hello_world\\Debug** folder.</span></span> <span data-ttu-id="812d1-108">Použít tyto cesty pro **modulu cesta** hodnoty, jak je znázorněno v následujícím nastavení souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="812d1-108">Use these paths for the **module path** values as shown in the following JSON settings file.</span></span>

<span data-ttu-id="812d1-109">Hello\_world\_ukázka proces má cestu k konfigurační soubor JSON jako argument příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="812d1-109">The hello\_world\_sample process takes the path to a JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="812d1-110">Následující příklad souboru JSON je součástí sady SDK úložiště na **ukázky\\hello\_world\\src\\hello\_world\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="812d1-110">The following example JSON file is provided in the SDK repository at **samples\\hello\_world\\src\\hello\_world\_win.json**.</span></span> <span data-ttu-id="812d1-111">Tento konfigurační soubor funguje, jako je nezměníte skriptu buildu umístit IoT Edge moduly nebo ukázka spustitelné soubory v jiné než výchozí umístění.</span><span class="sxs-lookup"><span data-stu-id="812d1-111">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="812d1-112">Jsou modulu cesty relativní k adresáři kde hello\_world\_sample.exe nachází.</span><span class="sxs-lookup"><span data-stu-id="812d1-112">The module paths are relative to the directory where the hello\_world\_sample.exe is located.</span></span> <span data-ttu-id="812d1-113">Ukázkový konfigurační soubor JSON ve výchozím nastavení zapíše do aktuálního pracovního adresáře soubor log.txt.</span><span class="sxs-lookup"><span data-stu-id="812d1-113">The sample JSON configuration file defaults to writing 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="812d1-114">Přejděte na **sestavení** složku v kořenovém místní kopii **iot hranou** úložiště.</span><span class="sxs-lookup"><span data-stu-id="812d1-114">Navigate to the **build** folder in the root of your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="812d1-115">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="812d1-115">Run the following command:</span></span>

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]