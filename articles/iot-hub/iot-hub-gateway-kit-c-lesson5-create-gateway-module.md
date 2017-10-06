---
title: "aaaCreate první modul brány Azure IoT | Microsoft Docs"
description: "Vytvořte modul a přidejte ho tooa ukázkové aplikace toocustomize modulu chování."
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
ms.openlocfilehash: 48996fc026c8b708e328b5629801465810e5b6a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a>Lekce 5: Vytvoření vaší první modul brány Azure IoT
Přestože Azure IoT Edge umožňuje toobuild moduly napsané v jazyce Java, .NET nebo Node.js, v tomto kurzu vás provede procesem hello kroky pro vytvoření modulu v C.

## <a name="what-you-will-do"></a>Co provedete

- Zkompilování a spuštění ukázkové aplikace hello hello_world na Intel NUC.
- Vytvořte modul a jeho kompilace Intel NUC.
- Přidejte hello nového modulu toohello hello_world ukázkovou aplikaci a spusťte ukázkové hello na Intel NUC. Nový modul Hello vytiskne "hello_world" zprávy s časovým razítkem.

## <a name="what-you-will-learn"></a>Co se dozvíte

- Jak toocompile a spusťte ukázkové aplikace na Intel NUC.
- Jak toocreate modul.
- Jak tooadd modulu tooa ukázková aplikace.

## <a name="what-you-need"></a>Co potřebujete

Azure IoT okraj, který se nainstaloval v hostitelském počítači.

## <a name="folder-structure"></a>Struktura složek

V podsložce hello Lekce 5 hello ukázkový kód, které jste naklonovali v lekci 1, je `module` složky a `sample` složky.

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- Hello `module/my_module` složka obsahuje hello zdrojový kód a skript toobuild hello modulu.
- Hello `sample` složka obsahuje hello zdrojový kód a skript toobuild hello ukázkovou aplikaci.

## <a name="compile-and-run-hello-helloworld-sample-app-on-intel-nuc"></a>Zkompilování a spuštění ukázkové aplikace hello hello_world na Intel NUC

Hello `hello_world` ukázka vytvoří brány podle hello `hello_world.json` souboru, který určuje hello dvě předdefinované modulů přidružená k aplikaci hello. Brána Hello zaznamená soubor tooa zprávu "hello, world" každých 5 sekund. V této části zkompilování a spuštění hello `hello_world` aplikace pomocí jeho výchozí modul.

toocompile a spusťte hello `hello_world` aplikace, postupujte podle těchto kroků v hostitelském počítači:

1. Inicializace hello konfigurační soubory spuštěním hello následující příkazy:

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. Aktualizujte konfigurační soubor hello brány hello adresu MAC Intel NUC. Tento krok přeskočte, pokud jste prošli hello lekce příliš[konfiguraci a spuštění ukázkové aplikace zakázat][config_ble].

   1. Otevřete konfigurační soubor brány hello spuštěním hello následující příkaz:

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. Adresa MAC brány hello aktualizace, když jste [nastavit Intel NUC jako brána IoT][setup_nuc]a pak hello soubor uložte.

1. Kompilace hello ukázka zdrojového kódu spuštěním hello následující příkaz:

   ```bash
   gulp compile
   ```

   Hello příkaz přenáší hello ukázka zdrojového kódu tooIntel NUC a spustí `build.sh` toocompile ho.

1. Spustit hello `hello_world` aplikace na Intel NUC spuštěním hello následující příkaz:

   ```bash
   gulp run
   ```

   Spustí příkaz Hello `../Tools/run-hello-world.js` který je uveden v `config.json` toostart hello ukázkovou aplikaci na Intel NUC.

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a>Vytvořte nový modul a jeho kompilace Intel NUC

Hello postup vás provede procesem vytvoření nový modul a jeho kompilace Intel NUC. modul Hello vytiskne zprávy s časovým razítkem po jejich přijetí. V této části vytvoříte první modul přizpůsobené brány.

Libovolný modul Azure IoT okraj musí implementovat hello následující rozhraní:

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

Volitelně můžete implementovat hello následující rozhraní:

   ```C
   pfModule_Start Module_Start
   ```

Hello následující diagram znázorňuje hello cesty důležité stav modulu. Odmocnina obdélníků Hello představují metody hello modul přesune mezi stavy implementaci tooperform operace. Hello elips jsou hlavní stavy, které modul hello může být v.

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

Nyní vytvoříme modul založený na šabloně hello:

1. Otevřete složku šablony hello spuštěním hello následující příkaz:

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - `src/my_module.c`slouží jako šablonu, která usnadňuje vytváření hello modulu. Šablona Hello deklaruje hello rozhraní. Stačí toodo je tooadd logiku toohello `MyModule_Receive` funkce.
   - `build.sh`je modulu hello hello sestavení skriptu toocompile na Intel NUC.
1. Otevřete hello `src/my_module.c` souboru a obsahovat dva soubory hlaviček:

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. Přidejte následující kód toohello hello `MyModule_Receive` funkce:

   ```C
   if (message == NULL)
   {
      LogError("invalid arg message");
   }
   else
   {
      // get hello message content
      const CONSTBUFFER * content = Message_GetContent(message);
      // get hello local time and format it
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
                  LogError("unable toostrftime");
              }
              else
              {
                  printf("[%s] %.*s\r\n", timetemp, (int)content->size, content->buffer);
              }
          }
      }
   }
   ```

1. Aktualizace hello `config.json` souboru toospecify hello `workspace` složky na trase hostitelský počítač a hello nasazení na Intel NUC. Při kompilování, hello soubory v hello `workspace` , bude složka přenášená toohello cesta pro nasazení.

   1. Otevřete hello `config.json` souboru spuštěním hello následující příkaz:

      ```bash
      code config.json
      ```

   1. Aktualizace `config.json` s hello následující konfigurace:

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. Kompilace hello modulu spuštěním hello následující příkaz:

   ```bash
   gulp compile
   ```

   Hello příkaz přenáší hello zdrojového kódu tooIntel NUC a spustí `build.sh` toocompile hello modulu.

## <a name="add-hello-module-toohello-helloworld-sample-app-and-run-hello-app-on-intel-nuc"></a>Přidat hello modulu toohello hello_world ukázkovou aplikaci a spuštění aplikace hello v Intel NUC

tooperform to úloh, postupujte takto:

1. Seznam všech hello k dispozici modul binárních souborů (.so) na Intel NUC spuštěním hello následující příkaz:

   ```bash
   gulp modules --list
   ```

   binární cesta Hello `my_module` kompilované by měl být uvedený jako níže:

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   Pokud změníte hello výchozí přihlašovací uživatelské jméno v `config-gateway.json`, bude začínat hello binární cesta `home/<your username>` místo `root`.

1. Přidat `my_module` toohello `hello_world` ukázkovou aplikaci:

   1. Otevřete hello `hello_world.json` souboru spuštěním hello následující příkaz:

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. Přidejte následující kód toohello hello `modules` části:

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

      Hello hodnotu `module.path` by měla být `/root/gateway_sample/module/my_module/build/libmy_module.so`. Hello kód deklaruje `my_module` toobe přidružené hello brány, jakož i hello umístění hello modulu binární soubor zadaný v `module.path`.
   1. Přidejte následující kód toohello hello `links` části:

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      Tento kód určuje, zda jsou zprávy přenést z hello `hello_world` modulu příliš`my_module`.

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. Spustit hello `hello_world` ukázkovou aplikaci spuštěním hello následující příkaz:

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   Hello `--config` parametr vynutí hello `run-hello-world.js` skriptu toorun pomocí hello `hello_world.json` souboru.

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

Blahopřejeme. Nyní můžete vidět hello chování tohoto nového modulu, jednoduše vytiskne se zprávy "hello, world" s časovým razítkem, jiné výsledky z modulu původní "hello_world" hello.

## <a name="next-steps"></a>Další kroky

Jste vytvořili nový modul, přidat ukázkové hello_world toohello i get hello ukázkové aplikace toorun s nového modulu hello na bránu. Pokud chcete toolearn více o modulech brány Azure IoT, můžete najít další ukázky modulu zde: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
