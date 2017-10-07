---
title: "aaaConnect zařízení pomocí jazyka C v systému Windows | Microsoft Docs"
description: "Popisuje, jak tooconnect toohello zařízení Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C v systému Windows."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 51041e0cec113a5cfa006ab2276096baf928eef5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-windows"></a>Připojit vaše zařízení toohello pro vzdálené monitorování předkonfigurované řešení (Windows)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Vytvoření ukázkové řešení C v systému Windows
Hello následující kroky vám ukážou, jak toocreate klientskou aplikaci, která komunikuje s hello vzdálené monitorování předkonfigurované řešení. Tato aplikace napsané v jazyce C a vytvořené a spusťte v systému Windows.

Vytvoření projektu starter v sadě Visual Studio 2015 nebo Visual Studio 2017 a přidání balíčků NuGet klienta zařízení IoT Hub hello:

1. Ve Visual Studiu Vytvořte konzolovou aplikaci C hello Visual C++ pomocí **Konzolová aplikace Win32** šablony. Název projektu hello **RMDevice**.
2. Na hello **nastavení aplikace** stránku hello **Win32 – Průvodce aplikací**, ujistěte se, že **Konzolová aplikace** je vybrána a zrušte zaškrtnutí políčka **předkompilované záhlaví** a **kontroluje životního cyklu SDL (Security Development)**.
3. V **Průzkumníku**, odstraňte soubory stdafx.h hello targetver.h a stdafx.cpp.
4. V **Průzkumníku**, přejmenujte tooRMDevice.c RMDevice.cpp souboru hello.
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **RMDevice** projektu a pak klikněte na **spravovat balíčky NuGet balíčky**. Klikněte na tlačítko **Procházet**, vyhledejte a nainstalujte následující balíčky NuGet hello:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **RMDevice** projektu a pak klikněte na **vlastnosti** projektu hello tooopen **stránky vlastností**dialogové okno. Podrobnosti najdete v tématu [nastavení vlastností projektu Visual C++][lnk-c-project-properties]. 
7. Klikněte na tlačítko hello **Linkeru** složku, pak klikněte na tlačítko hello **vstup** stránku vlastností.
8. Přidat **crypt32.lib** toohello **Další závislosti** vlastnost. Klikněte na tlačítko **OK** a potom **OK** znovu toosave hello projektu hodnot vlastností.

Přidat hello Parson JSON knihovny toohello **RMDevice** projekt a přidejte požadované hello `#include` příkazy:

1. Ve složce vhodný ve vašem počítači klonovat úložiště Parson GitHub hello pomocí hello následující příkaz:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Zkopírujte soubory parson.h a parson.c hello hello místní kopii hello Parson úložiště tooyour **RMDevice** složce projektu.

1. V sadě Visual Studio, klikněte pravým tlačítkem na hello **RMDevice** projektu, klikněte na tlačítko **přidat**a potom klikněte na **existující položka**.

1. V hello **přidat existující položku** dialogové okno, vyberte hello parson.h a parson.c soubory v hello **RMDevice** složce projektu. Pak klikněte na tlačítko **přidat** tooadd tyto dva soubory tooyour projektu.

1. V sadě Visual Studio otevřete soubor RMDevice.c hello. Nahradit stávající hello `#include` příkazy s hello následující kód:
   
    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

    > [!NOTE]
    > Nyní můžete ověřit, že váš projekt má hello nastavit tak, že vytváření se správnými závislostmi.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a>Sestavení a spuštění ukázkových hello

Přidat kód tooinvoke hello **vzdáleného\_monitorování\_spustit** funkce a potom sestavení a spuštění aplikace hello zařízení.

1. Nahraďte hello **hlavní** funkce s následující kód tooinvoke hello **vzdáleného\_monitorování\_spustit** funkce:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klikněte na tlačítko **sestavení** a potom **sestavit řešení** toobuild hello zařízení aplikací.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **RMDevice** projektu, klikněte na tlačítko **ladění**a potom klikněte na **spustit novou instanci** toorun hello Ukázka. Hello konzoly zobrazí zprávy jako hello aplikace odešle ukázková telemetrická toohello předkonfigurované řešení, obdrží požadovanou vlastnost hodnotami nastavenými v řídicí panel řešení hello a odpoví toomethods volat z řídicí panel řešení hello.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
