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
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-windows"></a><span data-ttu-id="7a4e6-103">Připojit vaše zařízení toohello pro vzdálené monitorování předkonfigurované řešení (Windows)</span><span class="sxs-lookup"><span data-stu-id="7a4e6-103">Connect your device toohello remote monitoring preconfigured solution (Windows)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a><span data-ttu-id="7a4e6-104">Vytvoření ukázkové řešení C v systému Windows</span><span class="sxs-lookup"><span data-stu-id="7a4e6-104">Create a C sample solution on Windows</span></span>
<span data-ttu-id="7a4e6-105">Hello následující kroky vám ukážou, jak toocreate klientskou aplikaci, která komunikuje s hello vzdálené monitorování předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-105">hello following steps show you how toocreate a client application that communicates with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="7a4e6-106">Tato aplikace napsané v jazyce C a vytvořené a spusťte v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-106">This application is written in C and built and run on Windows.</span></span>

<span data-ttu-id="7a4e6-107">Vytvoření projektu starter v sadě Visual Studio 2015 nebo Visual Studio 2017 a přidání balíčků NuGet klienta zařízení IoT Hub hello:</span><span class="sxs-lookup"><span data-stu-id="7a4e6-107">Create a starter project in Visual Studio 2015 or Visual Studio 2017 and add hello IoT Hub device client NuGet packages:</span></span>

1. <span data-ttu-id="7a4e6-108">Ve Visual Studiu Vytvořte konzolovou aplikaci C hello Visual C++ pomocí **Konzolová aplikace Win32** šablony.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-108">In Visual Studio, create a C console application using hello Visual C++ **Win32 Console Application** template.</span></span> <span data-ttu-id="7a4e6-109">Název projektu hello **RMDevice**.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-109">Name hello project **RMDevice**.</span></span>
2. <span data-ttu-id="7a4e6-110">Na hello **nastavení aplikace** stránku hello **Win32 – Průvodce aplikací**, ujistěte se, že **Konzolová aplikace** je vybrána a zrušte zaškrtnutí políčka **předkompilované záhlaví** a **kontroluje životního cyklu SDL (Security Development)**.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-110">On hello **Application Settings** page in hello **Win32 Application Wizard**, ensure that **Console application** is selected, and uncheck **Precompiled header** and **Security Development Lifecycle (SDL) checks**.</span></span>
3. <span data-ttu-id="7a4e6-111">V **Průzkumníku**, odstraňte soubory stdafx.h hello targetver.h a stdafx.cpp.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-111">In **Solution Explorer**, delete hello files stdafx.h, targetver.h, and stdafx.cpp.</span></span>
4. <span data-ttu-id="7a4e6-112">V **Průzkumníku**, přejmenujte tooRMDevice.c RMDevice.cpp souboru hello.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-112">In **Solution Explorer**, rename hello file RMDevice.cpp tooRMDevice.c.</span></span>
5. <span data-ttu-id="7a4e6-113">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **RMDevice** projektu a pak klikněte na **spravovat balíčky NuGet balíčky**.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-113">In **Solution Explorer**, right-click on hello **RMDevice** project and then click **Manage NuGet packages**.</span></span> <span data-ttu-id="7a4e6-114">Klikněte na tlačítko **Procházet**, vyhledejte a nainstalujte následující balíčky NuGet hello:</span><span class="sxs-lookup"><span data-stu-id="7a4e6-114">Click **Browse**, then search for and install hello following NuGet packages:</span></span>
   
   * <span data-ttu-id="7a4e6-115">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="7a4e6-115">Microsoft.Azure.IoTHub.Serializer</span></span>
   * <span data-ttu-id="7a4e6-116">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="7a4e6-116">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
   * <span data-ttu-id="7a4e6-117">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="7a4e6-117">Microsoft.Azure.IoTHub.MqttTransport</span></span>
6. <span data-ttu-id="7a4e6-118">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **RMDevice** projektu a pak klikněte na **vlastnosti** projektu hello tooopen **stránky vlastností**dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-118">In **Solution Explorer**, right-click on hello **RMDevice** project and then click **Properties** tooopen hello project's **Property Pages** dialog box.</span></span> <span data-ttu-id="7a4e6-119">Podrobnosti najdete v tématu [nastavení vlastností projektu Visual C++][lnk-c-project-properties].</span><span class="sxs-lookup"><span data-stu-id="7a4e6-119">For details, see [Setting Visual C++ Project Properties][lnk-c-project-properties].</span></span> 
7. <span data-ttu-id="7a4e6-120">Klikněte na tlačítko hello **Linkeru** složku, pak klikněte na tlačítko hello **vstup** stránku vlastností.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-120">Click hello **Linker** folder, then click hello **Input** property page.</span></span>
8. <span data-ttu-id="7a4e6-121">Přidat **crypt32.lib** toohello **Další závislosti** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-121">Add **crypt32.lib** toohello **Additional Dependencies** property.</span></span> <span data-ttu-id="7a4e6-122">Klikněte na tlačítko **OK** a potom **OK** znovu toosave hello projektu hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-122">Click **OK** and then **OK** again toosave hello project property values.</span></span>

<span data-ttu-id="7a4e6-123">Přidat hello Parson JSON knihovny toohello **RMDevice** projekt a přidejte požadované hello `#include` příkazy:</span><span class="sxs-lookup"><span data-stu-id="7a4e6-123">Add hello Parson JSON library toohello **RMDevice** project and add hello required `#include` statements:</span></span>

1. <span data-ttu-id="7a4e6-124">Ve složce vhodný ve vašem počítači klonovat úložiště Parson GitHub hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7a4e6-124">In a suitable folder on your computer, clone hello Parson GitHub repository using hello following command:</span></span>

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. <span data-ttu-id="7a4e6-125">Zkopírujte soubory parson.h a parson.c hello hello místní kopii hello Parson úložiště tooyour **RMDevice** složce projektu.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-125">Copy hello parson.h and parson.c files from hello local copy of hello Parson repository tooyour **RMDevice** project folder.</span></span>

1. <span data-ttu-id="7a4e6-126">V sadě Visual Studio, klikněte pravým tlačítkem na hello **RMDevice** projektu, klikněte na tlačítko **přidat**a potom klikněte na **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-126">In Visual Studio, right-click hello **RMDevice** project, click **Add**, and then click **Existing Item**.</span></span>

1. <span data-ttu-id="7a4e6-127">V hello **přidat existující položku** dialogové okno, vyberte hello parson.h a parson.c soubory v hello **RMDevice** složce projektu.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-127">In hello **Add Existing Item** dialog, select hello parson.h and parson.c files in hello **RMDevice** project folder.</span></span> <span data-ttu-id="7a4e6-128">Pak klikněte na tlačítko **přidat** tooadd tyto dva soubory tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-128">Then click **Add** tooadd these two files tooyour project.</span></span>

1. <span data-ttu-id="7a4e6-129">V sadě Visual Studio otevřete soubor RMDevice.c hello.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-129">In Visual Studio, open hello RMDevice.c file.</span></span> <span data-ttu-id="7a4e6-130">Nahradit stávající hello `#include` příkazy s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="7a4e6-130">Replace hello existing `#include` statements with hello following code:</span></span>
   
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
    > <span data-ttu-id="7a4e6-131">Nyní můžete ověřit, že váš projekt má hello nastavit tak, že vytváření se správnými závislostmi.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-131">Now you can verify that your project has hello correct dependencies set up by building it.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a><span data-ttu-id="7a4e6-132">Sestavení a spuštění ukázkových hello</span><span class="sxs-lookup"><span data-stu-id="7a4e6-132">Build and run hello sample</span></span>

<span data-ttu-id="7a4e6-133">Přidat kód tooinvoke hello **vzdáleného\_monitorování\_spustit** funkce a potom sestavení a spuštění aplikace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-133">Add code tooinvoke hello **remote\_monitoring\_run** function and then build and run hello device application.</span></span>

1. <span data-ttu-id="7a4e6-134">Nahraďte hello **hlavní** funkce s následující kód tooinvoke hello **vzdáleného\_monitorování\_spustit** funkce:</span><span class="sxs-lookup"><span data-stu-id="7a4e6-134">Replace hello **main** function with following code tooinvoke hello **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="7a4e6-135">Klikněte na tlačítko **sestavení** a potom **sestavit řešení** toobuild hello zařízení aplikací.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-135">Click **Build** and then **Build Solution** toobuild hello device application.</span></span>

1. <span data-ttu-id="7a4e6-136">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **RMDevice** projektu, klikněte na tlačítko **ladění**a potom klikněte na **spustit novou instanci** toorun hello Ukázka.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-136">In **Solution Explorer**, right-click hello **RMDevice** project, click **Debug**, and then click **Start new instance** toorun hello sample.</span></span> <span data-ttu-id="7a4e6-137">Hello konzoly zobrazí zprávy jako hello aplikace odešle ukázková telemetrická toohello předkonfigurované řešení, obdrží požadovanou vlastnost hodnotami nastavenými v řídicí panel řešení hello a odpoví toomethods volat z řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="7a4e6-137">hello console displays messages as hello application sends sample telemetry toohello preconfigured solution, receives desired property values set in hello solution dashboard, and responds toomethods invoked from hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
