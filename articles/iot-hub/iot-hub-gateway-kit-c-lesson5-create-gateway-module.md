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
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a><span data-ttu-id="41bb2-103">Lekce 5: Vytvoření vaší první modul brány Azure IoT</span><span class="sxs-lookup"><span data-stu-id="41bb2-103">Lesson 5: Create your first Azure IoT Gateway module</span></span>
<span data-ttu-id="41bb2-104">Přestože Azure IoT Edge umožňuje vytvářet moduly, které jsou napsané v jazyce Java, .NET nebo Node.js, tento kurz vás provede kroky pro vytvoření modulu v C.</span><span class="sxs-lookup"><span data-stu-id="41bb2-104">While Azure IoT Edge allows you to build modules written in Java, .NET, or Node.js, this tutorial walks you through the steps for building a module in C.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="41bb2-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="41bb2-105">What you will do</span></span>

- <span data-ttu-id="41bb2-106">Zkompilování a spuštění ukázkové aplikace hello_world na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-106">Compile and run the hello_world sample app on Intel NUC.</span></span>
- <span data-ttu-id="41bb2-107">Vytvořte modul a jeho kompilace Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-107">Create a module and compile it on Intel NUC.</span></span>
- <span data-ttu-id="41bb2-108">Přidání nového modulu do hello_world ukázkovou aplikaci a pak spusťte vzorku na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-108">Add the new module to the hello_world sample app and then run the sample on Intel NUC.</span></span> <span data-ttu-id="41bb2-109">Nový modul vytiskne "hello_world" zprávy s časovým razítkem.</span><span class="sxs-lookup"><span data-stu-id="41bb2-109">The new module prints out "hello_world" messages with a timestamp.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="41bb2-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="41bb2-110">What you will learn</span></span>

- <span data-ttu-id="41bb2-111">Jak zkompilování a spuštění ukázkové aplikace na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-111">How to compile and run a sample app on Intel NUC.</span></span>
- <span data-ttu-id="41bb2-112">Jak vytvořit modul.</span><span class="sxs-lookup"><span data-stu-id="41bb2-112">How to create a module.</span></span>
- <span data-ttu-id="41bb2-113">Postup přidání modulu pro ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41bb2-113">How to add module to a sample app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="41bb2-114">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="41bb2-114">What you need</span></span>

<span data-ttu-id="41bb2-115">Azure IoT okraj, který se nainstaloval v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="41bb2-115">Azure IoT Edge that has been installed on your host computer.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="41bb2-116">Struktura složek</span><span class="sxs-lookup"><span data-stu-id="41bb2-116">Folder structure</span></span>

<span data-ttu-id="41bb2-117">V podsložce Lekce 5 ukázkový kód, které jste naklonovali v lekci 1 je `module` složky a `sample` složky.</span><span class="sxs-lookup"><span data-stu-id="41bb2-117">In the Lesson 5 subfolder of the sample code which you cloned in lesson 1, there is a `module` folder and a `sample` folder.</span></span>

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- <span data-ttu-id="41bb2-119">`module/my_module` Složka obsahuje zdrojový kód a skript k sestavení modulu.</span><span class="sxs-lookup"><span data-stu-id="41bb2-119">The `module/my_module` folder contains the source code and script to build the module.</span></span>
- <span data-ttu-id="41bb2-120">`sample` Složka obsahuje zdrojový kód a skript k vytvoření ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="41bb2-120">The `sample` folder contains the source code and script to build the sample app.</span></span>

## <a name="compile-and-run-the-helloworld-sample-app-on-intel-nuc"></a><span data-ttu-id="41bb2-121">Zkompilování a spuštění ukázkové aplikace hello_world na Intel NUC</span><span class="sxs-lookup"><span data-stu-id="41bb2-121">Compile and run the hello_world sample app on Intel NUC</span></span>

<span data-ttu-id="41bb2-122">`hello_world` Ukázka vytvoří brány na základě `hello_world.json` souboru, který určuje dvě předdefinované moduly přidružené aplikaci.</span><span class="sxs-lookup"><span data-stu-id="41bb2-122">The `hello_world` sample creates a gateway based on the `hello_world.json` file which specifies the two predefined modules associated with the app.</span></span> <span data-ttu-id="41bb2-123">Brána zaznamená zprávu "hello, world" do souboru každých 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="41bb2-123">The gateway logs a "hello world" message to a file every 5 seconds.</span></span> <span data-ttu-id="41bb2-124">V této části zkompilování a spuštění `hello_world` aplikace pomocí jeho výchozí modul.</span><span class="sxs-lookup"><span data-stu-id="41bb2-124">In this section, you compile and run the `hello_world` app with its default module.</span></span>

<span data-ttu-id="41bb2-125">Pro zkompilování a spuštění `hello_world` aplikace, postupujte podle těchto kroků v hostitelském počítači:</span><span class="sxs-lookup"><span data-stu-id="41bb2-125">To compile and run the `hello_world` app, follow these steps on your host computer:</span></span>

1. <span data-ttu-id="41bb2-126">Inicializace konfigurační soubory spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="41bb2-126">Initialize the configuration files by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. <span data-ttu-id="41bb2-127">Aktualizujte konfigurační soubor brány s adresou MAC Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-127">Update the gateway configuration file with the MAC address of Intel NUC.</span></span> <span data-ttu-id="41bb2-128">Tento krok přeskočte, pokud jste prošli lekce k [konfiguraci a spuštění ukázkové aplikace zakázat][config_ble].</span><span class="sxs-lookup"><span data-stu-id="41bb2-128">Skip this step if you have gone through the lesson to [configure and run a BLE sample application][config_ble].</span></span>

   1. <span data-ttu-id="41bb2-129">Otevřete konfigurační soubor brány tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="41bb2-129">Open the gateway configuration file by running the following command:</span></span>

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. <span data-ttu-id="41bb2-130">Aktualizace adres MAC brána, kdy jste [nastavit Intel NUC jako brána IoT][setup_nuc]a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="41bb2-130">Update the gateway's MAC address when you [set up Intel NUC as a IoT gateway][setup_nuc], and then save the file.</span></span>

1. <span data-ttu-id="41bb2-131">Kompilace zdrojového kódu ukázka spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="41bb2-131">Compile the sample source code by running the following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="41bb2-132">Příkaz přenese ukázkový kód zdroj Intel NUC a spustí `build.sh` jej zkompilovat.</span><span class="sxs-lookup"><span data-stu-id="41bb2-132">The command transfers the sample source code to Intel NUC and runs `build.sh` to compile it.</span></span>

1. <span data-ttu-id="41bb2-133">Spustit `hello_world` aplikace na Intel NUC spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="41bb2-133">Run the `hello_world` app on Intel NUC by running the following command:</span></span>

   ```bash
   gulp run
   ```

   <span data-ttu-id="41bb2-134">Příkaz spustí v `../Tools/run-hello-world.js` který je uveden v `config.json` ke spuštění ukázkové aplikace na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-134">The command runs `../Tools/run-hello-world.js` that is specified in `config.json` to start the sample app on Intel NUC.</span></span>

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a><span data-ttu-id="41bb2-136">Vytvořte nový modul a jeho kompilace Intel NUC</span><span class="sxs-lookup"><span data-stu-id="41bb2-136">Create a new module and compile it on Intel NUC</span></span>

<span data-ttu-id="41bb2-137">Následující postup vás provede procesem vytvoření nový modul a jeho kompilace Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-137">The steps below walk you through creating a new module and compile it on Intel NUC.</span></span> <span data-ttu-id="41bb2-138">Modul vytiskne zprávy s časovým razítkem po jejich přijetí.</span><span class="sxs-lookup"><span data-stu-id="41bb2-138">The module prints out messages with a timestamp upon receiving them.</span></span> <span data-ttu-id="41bb2-139">V této části vytvoříte první modul přizpůsobené brány.</span><span class="sxs-lookup"><span data-stu-id="41bb2-139">You will create your first customized gateway module in this section.</span></span>

<span data-ttu-id="41bb2-140">Libovolný modul Azure IoT okraj musí implementovat rozhraní následující:</span><span class="sxs-lookup"><span data-stu-id="41bb2-140">Any Azure IoT Edge module must implement the following interfaces:</span></span>

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

<span data-ttu-id="41bb2-141">Volitelně můžete implementovat následující rozhraní:</span><span class="sxs-lookup"><span data-stu-id="41bb2-141">You can optionally implement the following interface:</span></span>

   ```C
   pfModule_Start Module_Start
   ```

<span data-ttu-id="41bb2-142">Následující diagram znázorňuje cesty důležité stav modulu.</span><span class="sxs-lookup"><span data-stu-id="41bb2-142">The following diagram shows the important state paths of a module.</span></span> <span data-ttu-id="41bb2-143">Odmocnina obdélníky představují metody, které můžete implementovat provádět operace, pokud modul přesune mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="41bb2-143">The square rectangles represent methods you implement to perform operations when the module moves between states.</span></span> <span data-ttu-id="41bb2-144">Elips jsou hlavní stavy, které modul může být v.</span><span class="sxs-lookup"><span data-stu-id="41bb2-144">The ovals are major states the module can be in.</span></span>

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

<span data-ttu-id="41bb2-146">Nyní vytvoříme modul založený na šabloně:</span><span class="sxs-lookup"><span data-stu-id="41bb2-146">Now let’s create a module based on the template:</span></span>

1. <span data-ttu-id="41bb2-147">Otevřete složku šablonu spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="41bb2-147">Open the template folder by running the following command:</span></span>

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - <span data-ttu-id="41bb2-149">`src/my_module.c`slouží jako šablonu, která usnadňuje vytváření modulu.</span><span class="sxs-lookup"><span data-stu-id="41bb2-149">`src/my_module.c` serves as a template that facilitates the creation of a module.</span></span> <span data-ttu-id="41bb2-150">Šablona deklaruje rozhraní.</span><span class="sxs-lookup"><span data-stu-id="41bb2-150">The template declares the interfaces.</span></span> <span data-ttu-id="41bb2-151">Vše je potřeba udělat, je přidejte logiku pro `MyModule_Receive` funkce.</span><span class="sxs-lookup"><span data-stu-id="41bb2-151">All you need to do is to add logic to the `MyModule_Receive` function.</span></span>
   - <span data-ttu-id="41bb2-152">`build.sh`je skript buildu zkompilovat modul na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-152">`build.sh` is the build script to compile the module on Intel NUC.</span></span>
1. <span data-ttu-id="41bb2-153">Otevřete `src/my_module.c` souboru a obsahovat dva soubory hlaviček:</span><span class="sxs-lookup"><span data-stu-id="41bb2-153">Open the `src/my_module.c` file and include two header files:</span></span>

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. <span data-ttu-id="41bb2-154">Přidejte následující kód, který `MyModule_Receive` funkce:</span><span class="sxs-lookup"><span data-stu-id="41bb2-154">Add the following code to the `MyModule_Receive` function:</span></span>

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

1. <span data-ttu-id="41bb2-155">Aktualizace `config.json` souboru k zadání `workspace` složky na hostitelském počítači a složka nasazení v Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="41bb2-155">Update the `config.json` file to specify the `workspace` folder on your host computer and the deployment path on Intel NUC.</span></span> <span data-ttu-id="41bb2-156">Při kompilování souborů `workspace` složky budou přeneseny do cesty nasazení.</span><span class="sxs-lookup"><span data-stu-id="41bb2-156">During compiling, the files in the `workspace` folder will be transferred to the deployment path.</span></span>

   1. <span data-ttu-id="41bb2-157">Otevřete `config.json` souboru tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="41bb2-157">Open the `config.json` file by running the following command:</span></span>

      ```bash
      code config.json
      ```

   1. <span data-ttu-id="41bb2-158">Aktualizace `config.json` s následující konfigurací:</span><span class="sxs-lookup"><span data-stu-id="41bb2-158">Update `config.json` with the following configuration:</span></span>

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. <span data-ttu-id="41bb2-160">Kompilace modul spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="41bb2-160">Compile the module by running the following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="41bb2-161">Příkaz přenese Intel NUC zdrojový kód a spustí `build.sh` zkompilovat modul.</span><span class="sxs-lookup"><span data-stu-id="41bb2-161">The command transfers the source code to Intel NUC and runs `build.sh` to compile the module.</span></span>

## <a name="add-the-module-to-the-helloworld-sample-app-and-run-the-app-on-intel-nuc"></a><span data-ttu-id="41bb2-162">Přidání modulu hello_world ukázkovou aplikaci a spusťte aplikaci v Intel NUC</span><span class="sxs-lookup"><span data-stu-id="41bb2-162">Add the module to the hello_world sample app and run the app on Intel NUC</span></span>

<span data-ttu-id="41bb2-163">K provedení této úlohy, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="41bb2-163">To perform this task, follow these steps:</span></span>

1. <span data-ttu-id="41bb2-164">Seznam všech k dispozici modul binárních souborů (.so) na Intel NUC spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="41bb2-164">List all the available module binaries (.so files) on Intel NUC by running the following command:</span></span>

   ```bash
   gulp modules --list
   ```

   <span data-ttu-id="41bb2-165">Binární cesta `my_module` kompilované by měl být uvedený jako níže:</span><span class="sxs-lookup"><span data-stu-id="41bb2-165">The binary path of `my_module` that you compiled should be listed as below:</span></span>

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   <span data-ttu-id="41bb2-166">Pokud změníte výchozí uživatelské jméno přihlášení v `config-gateway.json`, bude binární cesta začínat `home/<your username>` místo `root`.</span><span class="sxs-lookup"><span data-stu-id="41bb2-166">If you change the default login username in `config-gateway.json`, the binary path will start with `home/<your username>` instead of `root`.</span></span>

1. <span data-ttu-id="41bb2-167">Přidat `my_module` k `hello_world` ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="41bb2-167">Add `my_module` to the `hello_world` sample app:</span></span>

   1. <span data-ttu-id="41bb2-168">Otevřete `hello_world.json` souboru tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="41bb2-168">Open the `hello_world.json` file by running the following command:</span></span>

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. <span data-ttu-id="41bb2-169">Přidejte následující kód, který `modules` části:</span><span class="sxs-lookup"><span data-stu-id="41bb2-169">Add the following code to the `modules` section:</span></span>

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

      <span data-ttu-id="41bb2-170">Hodnota `module.path` by měla být `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span><span class="sxs-lookup"><span data-stu-id="41bb2-170">The value of `module.path` should be `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span></span> <span data-ttu-id="41bb2-171">Kód deklaruje `my_module` přidruženou bránu, jakož i umístění binární zadaný modul v `module.path`.</span><span class="sxs-lookup"><span data-stu-id="41bb2-171">The code declares `my_module` to be associated with the gateway as well as the location of the module binary specified in `module.path`.</span></span>
   1. <span data-ttu-id="41bb2-172">Přidejte následující kód, který `links` části:</span><span class="sxs-lookup"><span data-stu-id="41bb2-172">Add the following code to the `links` section:</span></span>

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      <span data-ttu-id="41bb2-173">Tento kód určuje, že zprávy se přenáší z `hello_world` modulu `my_module`.</span><span class="sxs-lookup"><span data-stu-id="41bb2-173">This code specifies that messages are transferred from the `hello_world` module to `my_module`.</span></span>

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. <span data-ttu-id="41bb2-175">Spustit `hello_world` ukázkovou aplikaci tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="41bb2-175">Run the `hello_world` sample app by running the following command:</span></span>

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   <span data-ttu-id="41bb2-176">`--config` Parametr vynutí `run-hello-world.js` skript spustí pomocí `hello_world.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="41bb2-176">The `--config` parameter forces the `run-hello-world.js` script to run by using the `hello_world.json` file.</span></span>

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

<span data-ttu-id="41bb2-178">Blahopřejeme.</span><span class="sxs-lookup"><span data-stu-id="41bb2-178">Congratulations.</span></span> <span data-ttu-id="41bb2-179">Nyní můžete vidět chování tohoto nového modulu, jednoduše vytiskne se zprávy "hello, world" s časovým razítkem, jiné výsledky z modulu původní "hello_world".</span><span class="sxs-lookup"><span data-stu-id="41bb2-179">You can now see the behavior of this new module, it simply prints out "hello world" messages with a timestamp, a different result from the original "hello_world" module.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41bb2-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41bb2-180">Next Steps</span></span>

<span data-ttu-id="41bb2-181">Jste vytvořili nový modul, přidáte do hello_world ukázka a získání ukázkovou aplikaci spustit s nového modulu na bránu.</span><span class="sxs-lookup"><span data-stu-id="41bb2-181">You’ve created a new module, added it to the hello_world sample, and get the sample app to run with the new module on your gateway.</span></span> <span data-ttu-id="41bb2-182">Pokud chcete další informace o modulech brány Azure IoT, můžete najít další ukázky modulu zde: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span><span class="sxs-lookup"><span data-stu-id="41bb2-182">If you want to learn more about Azure IoT gateway modules, you can find more module samples here: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span></span>

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
