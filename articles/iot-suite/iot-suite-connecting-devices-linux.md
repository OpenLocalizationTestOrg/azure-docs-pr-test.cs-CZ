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
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-linux"></a>Připojit vaše zařízení toohello pro vzdálené monitorování předkonfigurované řešení (Linux)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Sestavení a spuštění ukázkových C klienta Linux
Hello následující kroky vám ukážou, jak toocreate klientskou aplikaci, která komunikuje s hello vzdálené monitorování předkonfigurované řešení. Tato aplikace napsané v jazyce C a vytvořené a spusťte na Ubuntu Linux.

toocomplete tyto kroky, je třeba zařízení se systémem Ubuntu verze 15.04 nebo 15.10. Než budete pokračovat, nainstalujte požadované balíčky hello zařízení Ubuntu pomocí hello následující příkaz:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-hello-client-libraries-on-your-device"></a>Nainstalujte klientské knihovny hello zařízení
Hello knihovny klienta Azure IoT Hub jsou k dispozici jako balíček můžete nainstalovat na zařízení Ubuntu pomocí hello **výstižný get** příkaz. Proveďte následující kroky tooinstall hello balíček, který obsahuje hello Klientská knihovna pro IoT Hub a hlavičkových souborů v počítači Ubuntu hello:

1. V prostředí přidejte hello AzureIoT úložiště tooyour počítače:
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. Nainstalovat balíček azure-iot-sdk-c-dev hello
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-hello-parson-json-parser"></a>Nainstalujte hello analyzátor Parson JSON
Hello IoT Hub klientské knihovny používají hello datové části Parson JSON analyzátor tooparse zprávy. Ve složce vhodný ve vašem počítači klonovat úložiště Parson GitHub hello pomocí hello následující příkaz:

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a>Příprava projektu
Na počítači Ubuntu, vytvořte složku s názvem **vzdáleného\_monitorování**. V hello **vzdáleného\_monitorování** složky:

- Vytvoření hello čtyři soubory **main.c**, **vzdáleného\_monitoring.c**, **vzdáleného\_monitoring.h**, a **CMakeLists.txt**.
- Vytvořte složku s názvem **parson**.

Zkopírujte soubory hello **parson.c** a **parson.h** z místní kopie hello Parson úložiště do hello **vzdáleného\_monitorování nebo parson** složky.

V textovém editoru otevřete hello **vzdáleného\_monitoring.c** souboru. Přidejte následující hello `#include` příkazy:
   
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

## <a name="call-hello-remotemonitoringrun-function"></a>Volání hello vzdálené\_monitorování\_run – funkce
V textovém editoru otevřete hello **remote_monitoring.h** souboru. Přidejte následující kód hello:

```
void remote_monitoring_run(void);
```

V textovém editoru otevřete hello **main.c** souboru. Přidejte následující kód hello:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-hello-application"></a>Sestavení a spuštění aplikace hello
Hello následující kroky popisují, jak toouse *CMake* toobuild klientské aplikace.

1. V textovém editoru otevřete hello **CMakeLists.txt** souboru v hello **remote_monitoring** složky.

1. Přidejte následující pokyny toodefine jak hello toobuild klientské aplikace:
   
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
1. V hello **remote_monitoring** složky, vytvořit složku toostore hello *zkontrolujte* soubory této CMake generuje a pak spusťte hello **cmake** a **zkontrolujte** příkazy následujícím způsobem:
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Spuštění klienta aplikace hello a odesílat telemetrii tooIoT rozbočovače:
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

