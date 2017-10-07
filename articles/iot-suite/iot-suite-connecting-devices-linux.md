---
title: "aaaConnect zařízení pomocí jazyka C v systému Linux | Microsoft Docs"
description: "Popisuje, jak tooconnect toohello zařízení Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C systémem Linux."
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
ms.openlocfilehash: 57393817d40d3555177956a01fa71058bc256988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-linux"></a><span data-ttu-id="47fda-103">Připojit vaše zařízení toohello pro vzdálené monitorování předkonfigurované řešení (Linux)</span><span class="sxs-lookup"><span data-stu-id="47fda-103">Connect your device toohello remote monitoring preconfigured solution (Linux)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a><span data-ttu-id="47fda-104">Sestavení a spuštění ukázkových C klienta Linux</span><span class="sxs-lookup"><span data-stu-id="47fda-104">Build and run a sample C client Linux</span></span>
<span data-ttu-id="47fda-105">Hello následující kroky vám ukážou, jak toocreate klientskou aplikaci, která komunikuje s hello vzdálené monitorování předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="47fda-105">hello following steps show you how toocreate a client application that communicates with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="47fda-106">Tato aplikace napsané v jazyce C a vytvořené a spusťte na Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="47fda-106">This application is written in C and built and run on Ubuntu Linux.</span></span>

<span data-ttu-id="47fda-107">toocomplete tyto kroky, je třeba zařízení se systémem Ubuntu verze 15.04 nebo 15.10.</span><span class="sxs-lookup"><span data-stu-id="47fda-107">toocomplete these steps, you need a device running Ubuntu version 15.04 or 15.10.</span></span> <span data-ttu-id="47fda-108">Než budete pokračovat, nainstalujte požadované balíčky hello zařízení Ubuntu pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="47fda-108">Before proceeding, install hello prerequisite packages on your Ubuntu device using hello following command:</span></span>

```
sudo apt-get install cmake gcc g++
```

## <a name="install-hello-client-libraries-on-your-device"></a><span data-ttu-id="47fda-109">Nainstalujte klientské knihovny hello zařízení</span><span class="sxs-lookup"><span data-stu-id="47fda-109">Install hello client libraries on your device</span></span>
<span data-ttu-id="47fda-110">Hello knihovny klienta Azure IoT Hub jsou k dispozici jako balíček můžete nainstalovat na zařízení Ubuntu pomocí hello **výstižný get** příkaz.</span><span class="sxs-lookup"><span data-stu-id="47fda-110">hello Azure IoT Hub client libraries are available as a package you can install on your Ubuntu device using hello **apt-get** command.</span></span> <span data-ttu-id="47fda-111">Proveďte následující kroky tooinstall hello balíček, který obsahuje hello Klientská knihovna pro IoT Hub a hlavičkových souborů v počítači Ubuntu hello:</span><span class="sxs-lookup"><span data-stu-id="47fda-111">Complete hello following steps tooinstall hello package that contains hello IoT Hub client library and header files on your Ubuntu computer:</span></span>

1. <span data-ttu-id="47fda-112">V prostředí přidejte hello AzureIoT úložiště tooyour počítače:</span><span class="sxs-lookup"><span data-stu-id="47fda-112">In a shell, add hello AzureIoT repository tooyour computer:</span></span>
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. <span data-ttu-id="47fda-113">Nainstalovat balíček azure-iot-sdk-c-dev hello</span><span class="sxs-lookup"><span data-stu-id="47fda-113">Install hello azure-iot-sdk-c-dev package</span></span>
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-hello-parson-json-parser"></a><span data-ttu-id="47fda-114">Nainstalujte hello analyzátor Parson JSON</span><span class="sxs-lookup"><span data-stu-id="47fda-114">Install hello Parson JSON parser</span></span>
<span data-ttu-id="47fda-115">Hello IoT Hub klientské knihovny používají hello datové části Parson JSON analyzátor tooparse zprávy.</span><span class="sxs-lookup"><span data-stu-id="47fda-115">hello IoT Hub client libraries use hello Parson JSON parser tooparse message payloads.</span></span> <span data-ttu-id="47fda-116">Ve složce vhodný ve vašem počítači klonovat úložiště Parson GitHub hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="47fda-116">In a suitable folder on your computer, clone hello Parson GitHub repository using hello following command:</span></span>

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a><span data-ttu-id="47fda-117">Příprava projektu</span><span class="sxs-lookup"><span data-stu-id="47fda-117">Prepare your project</span></span>
<span data-ttu-id="47fda-118">Na počítači Ubuntu, vytvořte složku s názvem **vzdáleného\_monitorování**.</span><span class="sxs-lookup"><span data-stu-id="47fda-118">On your Ubuntu machine, create a folder called **remote\_monitoring**.</span></span> <span data-ttu-id="47fda-119">V hello **vzdáleného\_monitorování** složky:</span><span class="sxs-lookup"><span data-stu-id="47fda-119">In hello **remote\_monitoring** folder:</span></span>

- <span data-ttu-id="47fda-120">Vytvoření hello čtyři soubory **main.c**, **vzdáleného\_monitoring.c**, **vzdáleného\_monitoring.h**, a **CMakeLists.txt**.</span><span class="sxs-lookup"><span data-stu-id="47fda-120">Create hello four files **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, and **CMakeLists.txt**.</span></span>
- <span data-ttu-id="47fda-121">Vytvořte složku s názvem **parson**.</span><span class="sxs-lookup"><span data-stu-id="47fda-121">Create folder called **parson**.</span></span>

<span data-ttu-id="47fda-122">Zkopírujte soubory hello **parson.c** a **parson.h** z místní kopie hello Parson úložiště do hello **vzdáleného\_monitorování nebo parson** složky.</span><span class="sxs-lookup"><span data-stu-id="47fda-122">Copy hello files **parson.c** and **parson.h** from your local copy of hello Parson repository into hello **remote\_monitoring/parson** folder.</span></span>

<span data-ttu-id="47fda-123">V textovém editoru otevřete hello **vzdáleného\_monitoring.c** souboru.</span><span class="sxs-lookup"><span data-stu-id="47fda-123">In a text editor, open hello **remote\_monitoring.c** file.</span></span> <span data-ttu-id="47fda-124">Přidejte následující hello `#include` příkazy:</span><span class="sxs-lookup"><span data-stu-id="47fda-124">Add hello following `#include` statements:</span></span>
   
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

## <a name="call-hello-remotemonitoringrun-function"></a><span data-ttu-id="47fda-125">Volání hello vzdálené\_monitorování\_run – funkce</span><span class="sxs-lookup"><span data-stu-id="47fda-125">Call hello remote\_monitoring\_run function</span></span>
<span data-ttu-id="47fda-126">V textovém editoru otevřete hello **remote_monitoring.h** souboru.</span><span class="sxs-lookup"><span data-stu-id="47fda-126">In a text editor, open hello **remote_monitoring.h** file.</span></span> <span data-ttu-id="47fda-127">Přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="47fda-127">Add hello following code:</span></span>

```
void remote_monitoring_run(void);
```

<span data-ttu-id="47fda-128">V textovém editoru otevřete hello **main.c** souboru.</span><span class="sxs-lookup"><span data-stu-id="47fda-128">In a text editor, open hello **main.c** file.</span></span> <span data-ttu-id="47fda-129">Přidejte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="47fda-129">Add hello following code:</span></span>

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-hello-application"></a><span data-ttu-id="47fda-130">Sestavení a spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="47fda-130">Build and run hello application</span></span>
<span data-ttu-id="47fda-131">Hello následující kroky popisují, jak toouse *CMake* toobuild klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="47fda-131">hello following steps describe how toouse *CMake* toobuild your client application.</span></span>

1. <span data-ttu-id="47fda-132">V textovém editoru otevřete hello **CMakeLists.txt** souboru v hello **remote_monitoring** složky.</span><span class="sxs-lookup"><span data-stu-id="47fda-132">In a text editor, open hello **CMakeLists.txt** file in hello **remote_monitoring** folder.</span></span>

1. <span data-ttu-id="47fda-133">Přidejte následující pokyny toodefine jak hello toobuild klientské aplikace:</span><span class="sxs-lookup"><span data-stu-id="47fda-133">Add hello following instructions toodefine how toobuild your client application:</span></span>
   
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
1. <span data-ttu-id="47fda-134">V hello **remote_monitoring** složky, vytvořit složku toostore hello *zkontrolujte* soubory této CMake generuje a pak spusťte hello **cmake** a **zkontrolujte** příkazy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="47fda-134">In hello **remote_monitoring** folder, create a folder toostore hello *make* files that CMake generates and then run hello **cmake** and **make** commands as follows:</span></span>
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. <span data-ttu-id="47fda-135">Spuštění klienta aplikace hello a odesílat telemetrii tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="47fda-135">Run hello client application and send telemetry tooIoT Hub:</span></span>
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

