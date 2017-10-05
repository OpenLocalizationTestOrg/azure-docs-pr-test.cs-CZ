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
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Prozkoumejte Azure IoT Edge architektura v systému Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a>Spuštění ukázky

**Build.cmd** skript generuje jeho výstup v **sestavení** složky ve vaší místní kopii **iot hranou** úložiště. Tento výstup zahrnuje dvě IoT Edge moduly používané v této ukázce.

Místech skriptu sestavení **logger.dll** v **sestavení\\moduly\\protokolovacího nástroje\\ladění** složky a **hello\_world.dll** v **sestavení\\moduly\\hello_world\\ladění** složky. Použít tyto cesty pro **modulu cesta** hodnoty, jak je znázorněno v následujícím nastavení souboru JSON.

Hello\_world\_ukázka proces má cestu k konfigurační soubor JSON jako argument příkazového řádku. Následující příklad souboru JSON je součástí sady SDK úložiště na **ukázky\\hello\_world\\src\\hello\_world\_win.json**. Tento konfigurační soubor funguje, jako je nezměníte skriptu buildu umístit IoT Edge moduly nebo ukázka spustitelné soubory v jiné než výchozí umístění.

> [!NOTE]
> Jsou modulu cesty relativní k adresáři kde hello\_world\_sample.exe nachází. Ukázkový konfigurační soubor JSON ve výchozím nastavení zapíše do aktuálního pracovního adresáře soubor log.txt.

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

1. Přejděte na **sestavení** složku v kořenovém místní kopii **iot hranou** úložiště.

1. Spusťte následující příkaz:

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
