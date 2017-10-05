---
title: "Připojení zařízení pomocí jazyka C v systému Linux | Microsoft Docs"
description: "Popisuje, jak se připojit zařízení k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C systémem Linux."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0c7c8039-0bbf-4bb5-9e79-ed8cff433629
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 9adbc9cc13f0b4cafa3a3a7703c46f8085b15232
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a><span data-ttu-id="b3f77-103">Připojte zařízení k monitorování předkonfigurované řešení vzdáleného (Linux)</span><span class="sxs-lookup"><span data-stu-id="b3f77-103">Connect your device to the remote monitoring preconfigured solution (Linux)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a><span data-ttu-id="b3f77-104">Sestavení a spuštění ukázkových C klienta Linux</span><span class="sxs-lookup"><span data-stu-id="b3f77-104">Build and run a sample C client Linux</span></span>
<span data-ttu-id="b3f77-105">Následující kroky ukazují, jak vytvořit klientskou aplikaci, která komunikuje s předkonfigurovaného řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="b3f77-105">The following steps show you how to create a client application that communicates with the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="b3f77-106">Tato aplikace napsané v jazyce C a vytvořené a spusťte na Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="b3f77-106">This application is written in C and built and run on Ubuntu Linux.</span></span>

<span data-ttu-id="b3f77-107">K provedení těchto kroků, je třeba zařízení se systémem Ubuntu verze 15.04 nebo 15.10.</span><span class="sxs-lookup"><span data-stu-id="b3f77-107">To complete these steps, you need a device running Ubuntu version 15.04 or 15.10.</span></span> <span data-ttu-id="b3f77-108">Než budete pokračovat, nainstalujte požadované balíčky zařízení Ubuntu pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b3f77-108">Before proceeding, install the prerequisite packages on your Ubuntu device using the following command:</span></span>

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a><span data-ttu-id="b3f77-109">Nainstalujte klientské knihovny zařízení</span><span class="sxs-lookup"><span data-stu-id="b3f77-109">Install the client libraries on your device</span></span>
<span data-ttu-id="b3f77-110">Knihovny klienta Azure IoT Hub jsou k dispozici jako balíček můžete nainstalovat pomocí zařízení Ubuntu **výstižný get** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b3f77-110">The Azure IoT Hub client libraries are available as a package you can install on your Ubuntu device using the **apt-get** command.</span></span> <span data-ttu-id="b3f77-111">Proveďte následující kroky k instalaci balíčku, který obsahuje IoT Hub knihovny a hlavičky souborů klienta v počítači Ubuntu:</span><span class="sxs-lookup"><span data-stu-id="b3f77-111">Complete the following steps to install the package that contains the IoT Hub client library and header files on your Ubuntu computer:</span></span>

1. <span data-ttu-id="b3f77-112">V prostředí přidejte do počítače AzureIoT úložiště:</span><span class="sxs-lookup"><span data-stu-id="b3f77-112">In a shell, add the AzureIoT repository to your computer:</span></span>
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. <span data-ttu-id="b3f77-113">Nainstalovat balíček azure-iot-sdk-c vývojářů</span><span class="sxs-lookup"><span data-stu-id="b3f77-113">Install the azure-iot-sdk-c-dev package</span></span>
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-the-parson-json-parser"></a><span data-ttu-id="b3f77-114">Nainstalujte analyzátor Parson JSON</span><span class="sxs-lookup"><span data-stu-id="b3f77-114">Install the Parson JSON parser</span></span>
<span data-ttu-id="b3f77-115">Knihovny klienta služby IoT Hub pomocí analyzátoru Parson JSON analyzovat datové části zprávy.</span><span class="sxs-lookup"><span data-stu-id="b3f77-115">The IoT Hub client libraries use the Parson JSON parser to parse message payloads.</span></span> <span data-ttu-id="b3f77-116">Ve složce vhodný ve vašem počítači naklonujte úložiště Parson GitHub pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b3f77-116">In a suitable folder on your computer, clone the Parson GitHub repository using the following command:</span></span>

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a><span data-ttu-id="b3f77-117">Příprava projektu</span><span class="sxs-lookup"><span data-stu-id="b3f77-117">Prepare your project</span></span>
<span data-ttu-id="b3f77-118">Na počítači Ubuntu, vytvořte složku s názvem **vzdáleného\_monitorování**.</span><span class="sxs-lookup"><span data-stu-id="b3f77-118">On your Ubuntu machine, create a folder called **remote\_monitoring**.</span></span> <span data-ttu-id="b3f77-119">V **vzdáleného\_monitorování** složky:</span><span class="sxs-lookup"><span data-stu-id="b3f77-119">In the **remote\_monitoring** folder:</span></span>

- <span data-ttu-id="b3f77-120">Vytvořit čtyři soubory **main.c**, **vzdáleného\_monitoring.c**, **vzdáleného\_monitoring.h**, a **CMakeLists.txt**.</span><span class="sxs-lookup"><span data-stu-id="b3f77-120">Create the four files **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, and **CMakeLists.txt**.</span></span>
- <span data-ttu-id="b3f77-121">Vytvořte složku s názvem **parson**.</span><span class="sxs-lookup"><span data-stu-id="b3f77-121">Create folder called **parson**.</span></span>

<span data-ttu-id="b3f77-122">Zkopírujte soubory **parson.c** a **parson.h** z místní kopie Parson úložiště do **vzdáleného\_monitorování nebo parson** složky.</span><span class="sxs-lookup"><span data-stu-id="b3f77-122">Copy the files **parson.c** and **parson.h** from your local copy of the Parson repository into the **remote\_monitoring/parson** folder.</span></span>

<span data-ttu-id="b3f77-123">V textovém editoru otevřete **vzdáleného\_monitoring.c** souboru.</span><span class="sxs-lookup"><span data-stu-id="b3f77-123">In a text editor, open the **remote\_monitoring.c** file.</span></span> <span data-ttu-id="b3f77-124">Přidejte následující příkazy `#include`:</span><span class="sxs-lookup"><span data-stu-id="b3f77-124">Add the following `#include` statements:</span></span>
   
```
#include "iothubtransportmqtt.h"
#include "schemalib.h"
#include "iothub_client.h"
#include "serializer_devicetwin.h"
#include "schemaserializer.h"
#include "azure_c_shared_utility/threadapi.h"
#include "azure_c_shared_utility/platform.h"
#include "parson.h"
```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="call-the-remotemonitoringrun-function"></a><span data-ttu-id="b3f77-125">Volání vzdálených\_monitorování\_run – funkce</span><span class="sxs-lookup"><span data-stu-id="b3f77-125">Call the remote\_monitoring\_run function</span></span>
<span data-ttu-id="b3f77-126">V textovém editoru otevřete **remote_monitoring.h** souboru.</span><span class="sxs-lookup"><span data-stu-id="b3f77-126">In a text editor, open the **remote_monitoring.h** file.</span></span> <span data-ttu-id="b3f77-127">Přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="b3f77-127">Add the following code:</span></span>

```
void remote_monitoring_run(void);
```

<span data-ttu-id="b3f77-128">V textovém editoru otevřete **main.c** souboru.</span><span class="sxs-lookup"><span data-stu-id="b3f77-128">In a text editor, open the **main.c** file.</span></span> <span data-ttu-id="b3f77-129">Přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="b3f77-129">Add the following code:</span></span>

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-the-application"></a><span data-ttu-id="b3f77-130">Sestavení a spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b3f77-130">Build and run the application</span></span>
<span data-ttu-id="b3f77-131">Následující kroky popisují způsob použití *CMake* k vytvoření klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="b3f77-131">The following steps describe how to use *CMake* to build your client application.</span></span>

1. <span data-ttu-id="b3f77-132">V textovém editoru otevřete **CMakeLists.txt** v soubor **remote_monitoring** složky.</span><span class="sxs-lookup"><span data-stu-id="b3f77-132">In a text editor, open the **CMakeLists.txt** file in the **remote_monitoring** folder.</span></span>

1. <span data-ttu-id="b3f77-133">Přidejte podle následujících pokynů můžete definovat, jak vytvořit klientskou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="b3f77-133">Add the following instructions to define how to build your client application:</span></span>
   
    ```
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```
1. <span data-ttu-id="b3f77-134">V **remote_monitoring** složky, vytvořte složku pro uložení *Ujistěte se,* soubory, které generuje CMake a znovu spusťte **cmake** a **zkontrolujte** příkazy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b3f77-134">In the **remote_monitoring** folder, create a folder to store the *make* files that CMake generates and then run the **cmake** and **make** commands as follows:</span></span>
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. <span data-ttu-id="b3f77-135">Spusťte aplikaci klienta a odesílat telemetrická data do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="b3f77-135">Run the client application and send telemetry to IoT Hub:</span></span>
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

