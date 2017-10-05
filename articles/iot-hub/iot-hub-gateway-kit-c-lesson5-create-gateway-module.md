---
title: "Vytvoření první modul brány Azure IoT | Microsoft Docs"
description: "Modul vytvořit a přidat jej do ukázková aplikace k přizpůsobení chování modulu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cd7660f4-7b8b-4091-8d71-bb8723165b0b
ms.service: iot-hub
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5e28422158684c3aaf0ac3fdf5b19c80fbccfb02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a>Lekce 5: Vytvoření vaší první modul brány Azure IoT
Přestože Azure IoT Edge umožňuje vytvářet moduly, které jsou napsané v jazyce Java, .NET nebo Node.js, tento kurz vás provede kroky pro vytvoření modulu v C.

## <a name="what-you-will-do"></a>Co provedete

- Zkompilování a spuštění ukázkové aplikace hello_world na Intel NUC.
- Vytvořte modul a jeho kompilace Intel NUC.
- Přidání nového modulu do hello_world ukázkovou aplikaci a pak spusťte vzorku na Intel NUC. Nový modul vytiskne "hello_world" zprávy s časovým razítkem.

## <a name="what-you-will-learn"></a>Co se dozvíte

- Jak zkompilování a spuštění ukázkové aplikace na Intel NUC.
- Jak vytvořit modul.
- Postup přidání modulu pro ukázkovou aplikaci.

## <a name="what-you-need"></a>Co potřebujete

Azure IoT okraj, který se nainstaloval v hostitelském počítači.

## <a name="folder-structure"></a>Struktura složek

V podsložce Lekce 5 ukázkový kód, které jste naklonovali v lekci 1 je `module` složky a `sample` složky.

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- `module/my_module` Složka obsahuje zdrojový kód a skript k sestavení modulu.
- `sample` Složka obsahuje zdrojový kód a skript k vytvoření ukázkové aplikace.

## <a name="compile-and-run-the-helloworld-sample-app-on-intel-nuc"></a>Zkompilování a spuštění ukázkové aplikace hello_world na Intel NUC

`hello_world` Ukázka vytvoří brány na základě `hello_world.json` souboru, který určuje dvě předdefinované moduly přidružené aplikaci. Brána zaznamená zprávu "hello, world" do souboru každých 5 sekund. V této části zkompilování a spuštění `hello_world` aplikace pomocí jeho výchozí modul.

Pro zkompilování a spuštění `hello_world` aplikace, postupujte podle těchto kroků v hostitelském počítači:

1. Inicializace konfigurační soubory spuštěním následujících příkazů:

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. Aktualizujte konfigurační soubor brány s adresou MAC Intel NUC. Tento krok přeskočte, pokud jste prošli lekce k [konfiguraci a spuštění ukázkové aplikace zakázat][config_ble].

   1. Otevřete konfigurační soubor brány tak, že spustíte následující příkaz:

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. Aktualizace adres MAC brána, kdy jste [nastavit Intel NUC jako brána IoT][setup_nuc]a pak soubor uložte.

1. Kompilace zdrojového kódu ukázka spuštěním následujícího příkazu:

   ```bash
   gulp compile
   ```

   Příkaz přenese ukázkový kód zdroj Intel NUC a spustí `build.sh` jej zkompilovat.

1. Spustit `hello_world` aplikace na Intel NUC spuštěním následujícího příkazu:

   ```bash
   gulp run
   ```

   Příkaz spustí v `../Tools/run-hello-world.js` který je uveden v `config.json` ke spuštění ukázkové aplikace na Intel NUC.

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a>Vytvořte nový modul a jeho kompilace Intel NUC

Následující postup vás provede procesem vytvoření nový modul a jeho kompilace Intel NUC. Modul vytiskne zprávy s časovým razítkem po jejich přijetí. V této části vytvoříte první modul přizpůsobené brány.

Libovolný modul Azure IoT okraj musí implementovat rozhraní následující:

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

Volitelně můžete implementovat následující rozhraní:

   ```C
   pfModule_Start Module_Start
   ```

Následující diagram znázorňuje cesty důležité stav modulu. Odmocnina obdélníky představují metody, které můžete implementovat provádět operace, pokud modul přesune mezi stavy. Elips jsou hlavní stavy, které modul může být v.

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

Nyní vytvoříme modul založený na šabloně:

1. Otevřete složku šablonu spuštěním následujícího příkazu:

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - `src/my_module.c`slouží jako šablonu, která usnadňuje vytváření modulu. Šablona deklaruje rozhraní. Vše je potřeba udělat, je přidejte logiku pro `MyModule_Receive` funkce.
   - `build.sh`je skript buildu zkompilovat modul na Intel NUC.
1. Otevřete `src/my_module.c` souboru a obsahovat dva soubory hlaviček:

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. Přidejte následující kód, který `MyModule_Receive` funkce:

   ```C
   if (message == NULL)
   {
      LogError("invalid arg message");
   }
   else
   {
      // get the message content
      const CONSTBUFFER * content = Message_GetContent(message);
      // get the local time and format it
      time_t temp = time(NULL);
      if (temp == (time_t)-1)
      {
          LogError("time function failed");
      }
      else
      {
          struct tm* t = localtime(&temp);
          if (t == NULL)
          {
              LogError("localtime failed");
          }
          else
          {
              char timetemp[80] = { 0 };
              if (strftime(timetemp, sizeof(timetemp) / sizeof(timetemp[0]), "%c", t) == 0)
              {
                  LogError("unable to strftime");
              }
              else
              {
                  printf("[%s] %.*s\r\n", timetemp, (int)content->size, content->buffer);
              }
          }
      }
   }
   ```

1. Aktualizace `config.json` souboru k zadání `workspace` složky na hostitelském počítači a složka nasazení v Intel NUC. Při kompilování souborů `workspace` složky budou přeneseny do cesty nasazení.

   1. Otevřete `config.json` souboru tak, že spustíte následující příkaz:

      ```bash
      code config.json
      ```

   1. Aktualizace `config.json` s následující konfigurací:

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. Kompilace modul spuštěním následujícího příkazu:

   ```bash
   gulp compile
   ```

   Příkaz přenese Intel NUC zdrojový kód a spustí `build.sh` zkompilovat modul.

## <a name="add-the-module-to-the-helloworld-sample-app-and-run-the-app-on-intel-nuc"></a>Přidání modulu hello_world ukázkovou aplikaci a spusťte aplikaci v Intel NUC

K provedení této úlohy, postupujte takto:

1. Seznam všech k dispozici modul binárních souborů (.so) na Intel NUC spuštěním následujícího příkazu:

   ```bash
   gulp modules --list
   ```

   Binární cesta `my_module` kompilované by měl být uvedený jako níže:

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   Pokud změníte výchozí uživatelské jméno přihlášení v `config-gateway.json`, bude binární cesta začínat `home/<your username>` místo `root`.

1. Přidat `my_module` k `hello_world` ukázkovou aplikaci:

   1. Otevřete `hello_world.json` souboru tak, že spustíte následující příkaz:

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. Přidejte následující kód, který `modules` části:

      ```json
      {
       "name": "my_module",
       "loader": {
       "name": "native",
       "entrypoint": {
       "module.path": "/root/gateway_sample/module/my_module/build/libmy_module.so"
         }
        },
       "args": null
      }
      ```

      Hodnota `module.path` by měla být `/root/gateway_sample/module/my_module/build/libmy_module.so`. Kód deklaruje `my_module` přidruženou bránu, jakož i umístění binární zadaný modul v `module.path`.
   1. Přidejte následující kód, který `links` části:

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      Tento kód určuje, že zprávy se přenáší z `hello_world` modulu `my_module`.

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. Spustit `hello_world` ukázkovou aplikaci tak, že spustíte následující příkaz:

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   `--config` Parametr vynutí `run-hello-world.js` skript spustí pomocí `hello_world.json` souboru.

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

Blahopřejeme. Nyní můžete vidět chování tohoto nového modulu, jednoduše vytiskne se zprávy "hello, world" s časovým razítkem, jiné výsledky z modulu původní "hello_world".

## <a name="next-steps"></a>Další kroky

Jste vytvořili nový modul, přidáte do hello_world ukázka a získání ukázkovou aplikaci spustit s nového modulu na bránu. Pokud chcete další informace o modulech brány Azure IoT, můžete najít další ukázky modulu zde: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
