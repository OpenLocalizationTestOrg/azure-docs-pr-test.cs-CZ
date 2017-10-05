---
title: "Připojení zařízení pomocí jazyka C v systému Windows | Microsoft Docs"
description: "Popisuje, jak se připojit zařízení k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v jazyce C v systému Windows."
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
ms.openlocfilehash: d222bcbd64f288d4091acb0ecd2922b9ceee57e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a><span data-ttu-id="0d6d0-103">Připojte zařízení k monitorování předkonfigurované řešení vzdáleného (Windows)</span><span class="sxs-lookup"><span data-stu-id="0d6d0-103">Connect your device to the remote monitoring preconfigured solution (Windows)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a><span data-ttu-id="0d6d0-104">Vytvoření ukázkové řešení C v systému Windows</span><span class="sxs-lookup"><span data-stu-id="0d6d0-104">Create a C sample solution on Windows</span></span>
<span data-ttu-id="0d6d0-105">Následující kroky ukazují, jak vytvořit klientskou aplikaci, která komunikuje s předkonfigurovaného řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-105">The following steps show you how to create a client application that communicates with the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="0d6d0-106">Tato aplikace napsané v jazyce C a vytvořené a spusťte v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-106">This application is written in C and built and run on Windows.</span></span>

<span data-ttu-id="0d6d0-107">Vytvoření projektu starter v sadě Visual Studio 2015 nebo Visual Studio 2017 a přidání balíčků NuGet IoT Hub zařízení klienta:</span><span class="sxs-lookup"><span data-stu-id="0d6d0-107">Create a starter project in Visual Studio 2015 or Visual Studio 2017 and add the IoT Hub device client NuGet packages:</span></span>

1. <span data-ttu-id="0d6d0-108">Ve Visual Studiu Vytvořte konzolovou aplikaci C pomocí Visual C++ **Konzolová aplikace Win32** šablony.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-108">In Visual Studio, create a C console application using the Visual C++ **Win32 Console Application** template.</span></span> <span data-ttu-id="0d6d0-109">Název projektu **RMDevice**.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-109">Name the project **RMDevice**.</span></span>
2. <span data-ttu-id="0d6d0-110">Na **nastavení aplikace** stránku **Win32 – Průvodce aplikací**, ujistěte se, že **Konzolová aplikace** je vybrána a zrušte zaškrtnutí políčka **předkompilované záhlaví** a **kontroluje životního cyklu SDL (Security Development)**.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-110">On the **Application Settings** page in the **Win32 Application Wizard**, ensure that **Console application** is selected, and uncheck **Precompiled header** and **Security Development Lifecycle (SDL) checks**.</span></span>
3. <span data-ttu-id="0d6d0-111">V **Průzkumníku**, odstraňte soubory stdafx.h, targetver.h a stdafx.cpp.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-111">In **Solution Explorer**, delete the files stdafx.h, targetver.h, and stdafx.cpp.</span></span>
4. <span data-ttu-id="0d6d0-112">V **Průzkumníku**, přejmenujte soubor RMDevice.cpp RMDevice.c.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-112">In **Solution Explorer**, rename the file RMDevice.cpp to RMDevice.c.</span></span>
5. <span data-ttu-id="0d6d0-113">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **RMDevice** projektu a pak klikněte na **spravovat balíčky NuGet balíčky**.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-113">In **Solution Explorer**, right-click on the **RMDevice** project and then click **Manage NuGet packages**.</span></span> <span data-ttu-id="0d6d0-114">Klikněte na tlačítko **Procházet**, vyhledejte a nainstalujte následující balíčky NuGet:</span><span class="sxs-lookup"><span data-stu-id="0d6d0-114">Click **Browse**, then search for and install the following NuGet packages:</span></span>
   
   * <span data-ttu-id="0d6d0-115">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="0d6d0-115">Microsoft.Azure.IoTHub.Serializer</span></span>
   * <span data-ttu-id="0d6d0-116">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="0d6d0-116">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
   * <span data-ttu-id="0d6d0-117">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="0d6d0-117">Microsoft.Azure.IoTHub.MqttTransport</span></span>
6. <span data-ttu-id="0d6d0-118">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **RMDevice** projektu a pak klikněte na **vlastnosti** otevření projektu **stránky vlastností** Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-118">In **Solution Explorer**, right-click on the **RMDevice** project and then click **Properties** to open the project's **Property Pages** dialog box.</span></span> <span data-ttu-id="0d6d0-119">Podrobnosti najdete v tématu [nastavení vlastností projektu Visual C++][lnk-c-project-properties].</span><span class="sxs-lookup"><span data-stu-id="0d6d0-119">For details, see [Setting Visual C++ Project Properties][lnk-c-project-properties].</span></span> 
7. <span data-ttu-id="0d6d0-120">Klikněte na tlačítko **Linkeru** složku, klikněte **vstup** stránku vlastností.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-120">Click the **Linker** folder, then click the **Input** property page.</span></span>
8. <span data-ttu-id="0d6d0-121">Přidat **crypt32.lib** k **Další závislosti** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-121">Add **crypt32.lib** to the **Additional Dependencies** property.</span></span> <span data-ttu-id="0d6d0-122">Klikněte na tlačítko **OK** a potom **OK** znovu k uložení projektu hodnot vlastností.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-122">Click **OK** and then **OK** again to save the project property values.</span></span>

<span data-ttu-id="0d6d0-123">Přidat Parson JSON knihovnu, která má **RMDevice** projekt a přidejte požadované `#include` příkazy:</span><span class="sxs-lookup"><span data-stu-id="0d6d0-123">Add the Parson JSON library to the **RMDevice** project and add the required `#include` statements:</span></span>

1. <span data-ttu-id="0d6d0-124">Ve složce vhodný ve vašem počítači naklonujte úložiště Parson GitHub pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="0d6d0-124">In a suitable folder on your computer, clone the Parson GitHub repository using the following command:</span></span>

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. <span data-ttu-id="0d6d0-125">Zkopírujte soubory parson.h a parson.c z místní kopie Parson úložiště k vaší **RMDevice** složce projektu.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-125">Copy the parson.h and parson.c files from the local copy of the Parson repository to your **RMDevice** project folder.</span></span>

1. <span data-ttu-id="0d6d0-126">V sadě Visual Studio, klikněte pravým tlačítkem myši **RMDevice** projektu, klikněte na tlačítko **přidat**a potom klikněte na **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-126">In Visual Studio, right-click the **RMDevice** project, click **Add**, and then click **Existing Item**.</span></span>

1. <span data-ttu-id="0d6d0-127">V **přidat existující položku** dialogovém okně, vyberte parson.h a parson.c soubory **RMDevice** složce projektu.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-127">In the **Add Existing Item** dialog, select the parson.h and parson.c files in the **RMDevice** project folder.</span></span> <span data-ttu-id="0d6d0-128">Pak klikněte na tlačítko **přidat** přidat tyto dva soubory do projektu.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-128">Then click **Add** to add these two files to your project.</span></span>

1. <span data-ttu-id="0d6d0-129">V sadě Visual Studio otevřete soubor RMDevice.c.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-129">In Visual Studio, open the RMDevice.c file.</span></span> <span data-ttu-id="0d6d0-130">Nahradit existující `#include` příkazy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0d6d0-130">Replace the existing `#include` statements with the following code:</span></span>
   
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
    > <span data-ttu-id="0d6d0-131">Nyní můžete ověřit, že váš projekt má nastavit tak, že vytváření se správnými závislostmi.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-131">Now you can verify that your project has the correct dependencies set up by building it.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a><span data-ttu-id="0d6d0-132">Sestavit a spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="0d6d0-132">Build and run the sample</span></span>

<span data-ttu-id="0d6d0-133">Přidejte kód, který má být vyvolán **vzdáleného\_monitorování\_spustit** funkce a potom sestavit a spustit aplikaci zařízení.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-133">Add code to invoke the **remote\_monitoring\_run** function and then build and run the device application.</span></span>

1. <span data-ttu-id="0d6d0-134">Nahraďte **hlavní** funkce s následující kód k vyvolání **vzdáleného\_monitorování\_spustit** funkce:</span><span class="sxs-lookup"><span data-stu-id="0d6d0-134">Replace the **main** function with following code to invoke the **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="0d6d0-135">Klikněte na tlačítko **sestavení** a potom **sestavit řešení** sestavit aplikaci pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-135">Click **Build** and then **Build Solution** to build the device application.</span></span>

1. <span data-ttu-id="0d6d0-136">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **RMDevice** projektu, klikněte na tlačítko **ladění**a potom klikněte na **spustit novou instanci** ke spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-136">In **Solution Explorer**, right-click the **RMDevice** project, click **Debug**, and then click **Start new instance** to run the sample.</span></span> <span data-ttu-id="0d6d0-137">Konzole zobrazí zprávy jako aplikace odesílá telemetrii vzorek pro předkonfigurované řešení, obdrží požadovanou vlastnost hodnotami nastavenými na řídicím panelu řešení a reaguje na metody vyvolané z řídicího panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="0d6d0-137">The console displays messages as the application sends sample telemetry to the preconfigured solution, receives desired property values set in the solution dashboard, and responds to methods invoked from the solution dashboard.</span></span>

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
