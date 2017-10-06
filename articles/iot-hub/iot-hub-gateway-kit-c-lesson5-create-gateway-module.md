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
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a><span data-ttu-id="4e183-103">Lekce 5: Vytvoření vaší první modul brány Azure IoT</span><span class="sxs-lookup"><span data-stu-id="4e183-103">Lesson 5: Create your first Azure IoT Gateway module</span></span>
<span data-ttu-id="4e183-104">Přestože Azure IoT Edge umožňuje toobuild moduly napsané v jazyce Java, .NET nebo Node.js, v tomto kurzu vás provede procesem hello kroky pro vytvoření modulu v C.</span><span class="sxs-lookup"><span data-stu-id="4e183-104">While Azure IoT Edge allows you toobuild modules written in Java, .NET, or Node.js, this tutorial walks you through hello steps for building a module in C.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="4e183-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="4e183-105">What you will do</span></span>

- <span data-ttu-id="4e183-106">Zkompilování a spuštění ukázkové aplikace hello hello_world na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-106">Compile and run hello hello_world sample app on Intel NUC.</span></span>
- <span data-ttu-id="4e183-107">Vytvořte modul a jeho kompilace Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-107">Create a module and compile it on Intel NUC.</span></span>
- <span data-ttu-id="4e183-108">Přidejte hello nového modulu toohello hello_world ukázkovou aplikaci a spusťte ukázkové hello na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-108">Add hello new module toohello hello_world sample app and then run hello sample on Intel NUC.</span></span> <span data-ttu-id="4e183-109">Nový modul Hello vytiskne "hello_world" zprávy s časovým razítkem.</span><span class="sxs-lookup"><span data-stu-id="4e183-109">hello new module prints out "hello_world" messages with a timestamp.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4e183-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="4e183-110">What you will learn</span></span>

- <span data-ttu-id="4e183-111">Jak toocompile a spusťte ukázkové aplikace na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-111">How toocompile and run a sample app on Intel NUC.</span></span>
- <span data-ttu-id="4e183-112">Jak toocreate modul.</span><span class="sxs-lookup"><span data-stu-id="4e183-112">How toocreate a module.</span></span>
- <span data-ttu-id="4e183-113">Jak tooadd modulu tooa ukázková aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e183-113">How tooadd module tooa sample app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4e183-114">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="4e183-114">What you need</span></span>

<span data-ttu-id="4e183-115">Azure IoT okraj, který se nainstaloval v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="4e183-115">Azure IoT Edge that has been installed on your host computer.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="4e183-116">Struktura složek</span><span class="sxs-lookup"><span data-stu-id="4e183-116">Folder structure</span></span>

<span data-ttu-id="4e183-117">V podsložce hello Lekce 5 hello ukázkový kód, které jste naklonovali v lekci 1, je `module` složky a `sample` složky.</span><span class="sxs-lookup"><span data-stu-id="4e183-117">In hello Lesson 5 subfolder of hello sample code which you cloned in lesson 1, there is a `module` folder and a `sample` folder.</span></span>

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- <span data-ttu-id="4e183-119">Hello `module/my_module` složka obsahuje hello zdrojový kód a skript toobuild hello modulu.</span><span class="sxs-lookup"><span data-stu-id="4e183-119">hello `module/my_module` folder contains hello source code and script toobuild hello module.</span></span>
- <span data-ttu-id="4e183-120">Hello `sample` složka obsahuje hello zdrojový kód a skript toobuild hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4e183-120">hello `sample` folder contains hello source code and script toobuild hello sample app.</span></span>

## <a name="compile-and-run-hello-helloworld-sample-app-on-intel-nuc"></a><span data-ttu-id="4e183-121">Zkompilování a spuštění ukázkové aplikace hello hello_world na Intel NUC</span><span class="sxs-lookup"><span data-stu-id="4e183-121">Compile and run hello hello_world sample app on Intel NUC</span></span>

<span data-ttu-id="4e183-122">Hello `hello_world` ukázka vytvoří brány podle hello `hello_world.json` souboru, který určuje hello dvě předdefinované modulů přidružená k aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="4e183-122">hello `hello_world` sample creates a gateway based on hello `hello_world.json` file which specifies hello two predefined modules associated with hello app.</span></span> <span data-ttu-id="4e183-123">Brána Hello zaznamená soubor tooa zprávu "hello, world" každých 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="4e183-123">hello gateway logs a "hello world" message tooa file every 5 seconds.</span></span> <span data-ttu-id="4e183-124">V této části zkompilování a spuštění hello `hello_world` aplikace pomocí jeho výchozí modul.</span><span class="sxs-lookup"><span data-stu-id="4e183-124">In this section, you compile and run hello `hello_world` app with its default module.</span></span>

<span data-ttu-id="4e183-125">toocompile a spusťte hello `hello_world` aplikace, postupujte podle těchto kroků v hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="4e183-125">toocompile and run hello `hello_world` app, follow these steps on your host computer:</span></span>

1. <span data-ttu-id="4e183-126">Inicializace hello konfigurační soubory spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="4e183-126">Initialize hello configuration files by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. <span data-ttu-id="4e183-127">Aktualizujte konfigurační soubor hello brány hello adresu MAC Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-127">Update hello gateway configuration file with hello MAC address of Intel NUC.</span></span> <span data-ttu-id="4e183-128">Tento krok přeskočte, pokud jste prošli hello lekce příliš[konfiguraci a spuštění ukázkové aplikace zakázat][config_ble].</span><span class="sxs-lookup"><span data-stu-id="4e183-128">Skip this step if you have gone through hello lesson too[configure and run a BLE sample application][config_ble].</span></span>

   1. <span data-ttu-id="4e183-129">Otevřete konfigurační soubor brány hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-129">Open hello gateway configuration file by running hello following command:</span></span>

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. <span data-ttu-id="4e183-130">Adresa MAC brány hello aktualizace, když jste [nastavit Intel NUC jako brána IoT][setup_nuc]a pak hello soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="4e183-130">Update hello gateway's MAC address when you [set up Intel NUC as a IoT gateway][setup_nuc], and then save hello file.</span></span>

1. <span data-ttu-id="4e183-131">Kompilace hello ukázka zdrojového kódu spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-131">Compile hello sample source code by running hello following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="4e183-132">Hello příkaz přenáší hello ukázka zdrojového kódu tooIntel NUC a spustí `build.sh` toocompile ho.</span><span class="sxs-lookup"><span data-stu-id="4e183-132">hello command transfers hello sample source code tooIntel NUC and runs `build.sh` toocompile it.</span></span>

1. <span data-ttu-id="4e183-133">Spustit hello `hello_world` aplikace na Intel NUC spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-133">Run hello `hello_world` app on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp run
   ```

   <span data-ttu-id="4e183-134">Spustí příkaz Hello `../Tools/run-hello-world.js` který je uveden v `config.json` toostart hello ukázkovou aplikaci na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-134">hello command runs `../Tools/run-hello-world.js` that is specified in `config.json` toostart hello sample app on Intel NUC.</span></span>

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a><span data-ttu-id="4e183-136">Vytvořte nový modul a jeho kompilace Intel NUC</span><span class="sxs-lookup"><span data-stu-id="4e183-136">Create a new module and compile it on Intel NUC</span></span>

<span data-ttu-id="4e183-137">Hello postup vás provede procesem vytvoření nový modul a jeho kompilace Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-137">hello steps below walk you through creating a new module and compile it on Intel NUC.</span></span> <span data-ttu-id="4e183-138">modul Hello vytiskne zprávy s časovým razítkem po jejich přijetí.</span><span class="sxs-lookup"><span data-stu-id="4e183-138">hello module prints out messages with a timestamp upon receiving them.</span></span> <span data-ttu-id="4e183-139">V této části vytvoříte první modul přizpůsobené brány.</span><span class="sxs-lookup"><span data-stu-id="4e183-139">You will create your first customized gateway module in this section.</span></span>

<span data-ttu-id="4e183-140">Libovolný modul Azure IoT okraj musí implementovat hello následující rozhraní:</span><span class="sxs-lookup"><span data-stu-id="4e183-140">Any Azure IoT Edge module must implement hello following interfaces:</span></span>

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

<span data-ttu-id="4e183-141">Volitelně můžete implementovat hello následující rozhraní:</span><span class="sxs-lookup"><span data-stu-id="4e183-141">You can optionally implement hello following interface:</span></span>

   ```C
   pfModule_Start Module_Start
   ```

<span data-ttu-id="4e183-142">Hello následující diagram znázorňuje hello cesty důležité stav modulu.</span><span class="sxs-lookup"><span data-stu-id="4e183-142">hello following diagram shows hello important state paths of a module.</span></span> <span data-ttu-id="4e183-143">Odmocnina obdélníků Hello představují metody hello modul přesune mezi stavy implementaci tooperform operace.</span><span class="sxs-lookup"><span data-stu-id="4e183-143">hello square rectangles represent methods you implement tooperform operations when hello module moves between states.</span></span> <span data-ttu-id="4e183-144">Hello elips jsou hlavní stavy, které modul hello může být v.</span><span class="sxs-lookup"><span data-stu-id="4e183-144">hello ovals are major states hello module can be in.</span></span>

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

<span data-ttu-id="4e183-146">Nyní vytvoříme modul založený na šabloně hello:</span><span class="sxs-lookup"><span data-stu-id="4e183-146">Now let’s create a module based on hello template:</span></span>

1. <span data-ttu-id="4e183-147">Otevřete složku šablony hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-147">Open hello template folder by running hello following command:</span></span>

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - <span data-ttu-id="4e183-149">`src/my_module.c`slouží jako šablonu, která usnadňuje vytváření hello modulu.</span><span class="sxs-lookup"><span data-stu-id="4e183-149">`src/my_module.c` serves as a template that facilitates hello creation of a module.</span></span> <span data-ttu-id="4e183-150">Šablona Hello deklaruje hello rozhraní.</span><span class="sxs-lookup"><span data-stu-id="4e183-150">hello template declares hello interfaces.</span></span> <span data-ttu-id="4e183-151">Stačí toodo je tooadd logiku toohello `MyModule_Receive` funkce.</span><span class="sxs-lookup"><span data-stu-id="4e183-151">All you need toodo is tooadd logic toohello `MyModule_Receive` function.</span></span>
   - <span data-ttu-id="4e183-152">`build.sh`je modulu hello hello sestavení skriptu toocompile na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-152">`build.sh` is hello build script toocompile hello module on Intel NUC.</span></span>
1. <span data-ttu-id="4e183-153">Otevřete hello `src/my_module.c` souboru a obsahovat dva soubory hlaviček:</span><span class="sxs-lookup"><span data-stu-id="4e183-153">Open hello `src/my_module.c` file and include two header files:</span></span>

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. <span data-ttu-id="4e183-154">Přidejte následující kód toohello hello `MyModule_Receive` funkce:</span><span class="sxs-lookup"><span data-stu-id="4e183-154">Add hello following code toohello `MyModule_Receive` function:</span></span>

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

1. <span data-ttu-id="4e183-155">Aktualizace hello `config.json` souboru toospecify hello `workspace` složky na trase hostitelský počítač a hello nasazení na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="4e183-155">Update hello `config.json` file toospecify hello `workspace` folder on your host computer and hello deployment path on Intel NUC.</span></span> <span data-ttu-id="4e183-156">Při kompilování, hello soubory v hello `workspace` , bude složka přenášená toohello cesta pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="4e183-156">During compiling, hello files in hello `workspace` folder will be transferred toohello deployment path.</span></span>

   1. <span data-ttu-id="4e183-157">Otevřete hello `config.json` souboru spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-157">Open hello `config.json` file by running hello following command:</span></span>

      ```bash
      code config.json
      ```

   1. <span data-ttu-id="4e183-158">Aktualizace `config.json` s hello následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="4e183-158">Update `config.json` with hello following configuration:</span></span>

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. <span data-ttu-id="4e183-160">Kompilace hello modulu spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-160">Compile hello module by running hello following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="4e183-161">Hello příkaz přenáší hello zdrojového kódu tooIntel NUC a spustí `build.sh` toocompile hello modulu.</span><span class="sxs-lookup"><span data-stu-id="4e183-161">hello command transfers hello source code tooIntel NUC and runs `build.sh` toocompile hello module.</span></span>

## <a name="add-hello-module-toohello-helloworld-sample-app-and-run-hello-app-on-intel-nuc"></a><span data-ttu-id="4e183-162">Přidat hello modulu toohello hello_world ukázkovou aplikaci a spuštění aplikace hello v Intel NUC</span><span class="sxs-lookup"><span data-stu-id="4e183-162">Add hello module toohello hello_world sample app and run hello app on Intel NUC</span></span>

<span data-ttu-id="4e183-163">tooperform to úloh, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4e183-163">tooperform this task, follow these steps:</span></span>

1. <span data-ttu-id="4e183-164">Seznam všech hello k dispozici modul binárních souborů (.so) na Intel NUC spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-164">List all hello available module binaries (.so files) on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp modules --list
   ```

   <span data-ttu-id="4e183-165">binární cesta Hello `my_module` kompilované by měl být uvedený jako níže:</span><span class="sxs-lookup"><span data-stu-id="4e183-165">hello binary path of `my_module` that you compiled should be listed as below:</span></span>

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   <span data-ttu-id="4e183-166">Pokud změníte hello výchozí přihlašovací uživatelské jméno v `config-gateway.json`, bude začínat hello binární cesta `home/<your username>` místo `root`.</span><span class="sxs-lookup"><span data-stu-id="4e183-166">If you change hello default login username in `config-gateway.json`, hello binary path will start with `home/<your username>` instead of `root`.</span></span>

1. <span data-ttu-id="4e183-167">Přidat `my_module` toohello `hello_world` ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="4e183-167">Add `my_module` toohello `hello_world` sample app:</span></span>

   1. <span data-ttu-id="4e183-168">Otevřete hello `hello_world.json` souboru spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-168">Open hello `hello_world.json` file by running hello following command:</span></span>

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. <span data-ttu-id="4e183-169">Přidejte následující kód toohello hello `modules` části:</span><span class="sxs-lookup"><span data-stu-id="4e183-169">Add hello following code toohello `modules` section:</span></span>

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

      <span data-ttu-id="4e183-170">Hello hodnotu `module.path` by měla být `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span><span class="sxs-lookup"><span data-stu-id="4e183-170">hello value of `module.path` should be `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span></span> <span data-ttu-id="4e183-171">Hello kód deklaruje `my_module` toobe přidružené hello brány, jakož i hello umístění hello modulu binární soubor zadaný v `module.path`.</span><span class="sxs-lookup"><span data-stu-id="4e183-171">hello code declares `my_module` toobe associated with hello gateway as well as hello location of hello module binary specified in `module.path`.</span></span>
   1. <span data-ttu-id="4e183-172">Přidejte následující kód toohello hello `links` části:</span><span class="sxs-lookup"><span data-stu-id="4e183-172">Add hello following code toohello `links` section:</span></span>

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      <span data-ttu-id="4e183-173">Tento kód určuje, zda jsou zprávy přenést z hello `hello_world` modulu příliš`my_module`.</span><span class="sxs-lookup"><span data-stu-id="4e183-173">This code specifies that messages are transferred from hello `hello_world` module too`my_module`.</span></span>

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. <span data-ttu-id="4e183-175">Spustit hello `hello_world` ukázkovou aplikaci spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4e183-175">Run hello `hello_world` sample app by running hello following command:</span></span>

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   <span data-ttu-id="4e183-176">Hello `--config` parametr vynutí hello `run-hello-world.js` skriptu toorun pomocí hello `hello_world.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="4e183-176">hello `--config` parameter forces hello `run-hello-world.js` script toorun by using hello `hello_world.json` file.</span></span>

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

<span data-ttu-id="4e183-178">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="4e183-178">Congratulations.</span></span> <span data-ttu-id="4e183-179">Nyní můžete vidět hello chování tohoto nového modulu, jednoduše vytiskne se zprávy "hello, world" s časovým razítkem, jiné výsledky z modulu původní "hello_world" hello.</span><span class="sxs-lookup"><span data-stu-id="4e183-179">You can now see hello behavior of this new module, it simply prints out "hello world" messages with a timestamp, a different result from hello original "hello_world" module.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e183-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4e183-180">Next Steps</span></span>

<span data-ttu-id="4e183-181">Jste vytvořili nový modul, přidat ukázkové hello_world toohello i get hello ukázkové aplikace toorun s nového modulu hello na bránu.</span><span class="sxs-lookup"><span data-stu-id="4e183-181">You’ve created a new module, added it toohello hello_world sample, and get hello sample app toorun with hello new module on your gateway.</span></span> <span data-ttu-id="4e183-182">Pokud chcete toolearn více o modulech brány Azure IoT, můžete najít další ukázky modulu zde: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span><span class="sxs-lookup"><span data-stu-id="4e183-182">If you want toolearn more about Azure IoT gateway modules, you can find more module samples here: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span></span>

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
