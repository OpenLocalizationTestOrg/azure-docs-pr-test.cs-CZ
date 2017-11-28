---
title: "Převod aaaData na IoT brány s hranou Azure IoT | Microsoft Docs"
description: "Použijte IoT brány tooconvert hello formát data snímačů pomocí vlastní modul z Azure IoT okraj."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Převod dat IOT brány, transformaci dat brány iot"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: ae94b1f96f36dfcb4f77fadc0ece3cff3d0bba91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a><span data-ttu-id="36d5c-104">Použít bránu IoT pro transformaci dat snímačů s hranou Azure IoT</span><span class="sxs-lookup"><span data-stu-id="36d5c-104">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>

> [!NOTE]
> <span data-ttu-id="36d5c-105">Než začnete tento kurz, ujistěte se, že jste dokončené hello následující lekce v pořadí:</span><span class="sxs-lookup"><span data-stu-id="36d5c-105">Before you start this tutorial, make sure you’ve completed hello following lessons in sequence:</span></span>
> * [<span data-ttu-id="36d5c-106">Nastavení Intel NUC jako brány IoT</span><span class="sxs-lookup"><span data-stu-id="36d5c-106">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [<span data-ttu-id="36d5c-107">Použít IoT brány tooconnect věcí toohello cloudu - SensorTag tooAzure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="36d5c-107">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

<span data-ttu-id="36d5c-108">Jediný účel bránu Iot tooprocess shromažďovaných dat je před odesláním toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="36d5c-108">One purpose of an Iot gateway is tooprocess collected data before sending it toohello cloud.</span></span> <span data-ttu-id="36d5c-109">Azure IoT Edge zavádí moduly, které můžou být vytvořené a sestavený tooform hello zpracování dat pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="36d5c-109">Azure IoT Edge introduces modules which can be created and assembled tooform hello data processing workflow.</span></span> <span data-ttu-id="36d5c-110">Modul přijme nějakou zprávu, provádí některé akce a potom posuňte pro jiné moduly tooprocess.</span><span class="sxs-lookup"><span data-stu-id="36d5c-110">A module receives a message, performs some action on it, and then move it on for other modules tooprocess.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="36d5c-111">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="36d5c-111">What you learn</span></span>

<span data-ttu-id="36d5c-112">Zjistíte, jak toocreate modulu tooconvert zpráv ze hello SensorTag do jiného formátu.</span><span class="sxs-lookup"><span data-stu-id="36d5c-112">You learn how toocreate a module tooconvert messages from hello SensorTag into a different format.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="36d5c-113">Co dělat</span><span class="sxs-lookup"><span data-stu-id="36d5c-113">What you do</span></span>

* <span data-ttu-id="36d5c-114">Vytvoření modulu tooconvert přijaté zprávy do formátu .json hello.</span><span class="sxs-lookup"><span data-stu-id="36d5c-114">Create a module tooconvert a received message into hello .json format.</span></span>
* <span data-ttu-id="36d5c-115">Zkompilujte hello modulu.</span><span class="sxs-lookup"><span data-stu-id="36d5c-115">Compile hello module.</span></span>
* <span data-ttu-id="36d5c-116">Přidáte hello modulu toohello zakázat ukázkovou aplikaci z Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="36d5c-116">Add hello module toohello BLE sample application from Azure IoT Edge.</span></span>
* <span data-ttu-id="36d5c-117">Spuštění ukázkové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="36d5c-117">Run hello sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="36d5c-118">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="36d5c-118">What you need</span></span>

* <span data-ttu-id="36d5c-119">Hello následující kurzy dokončit v pořadí:</span><span class="sxs-lookup"><span data-stu-id="36d5c-119">hello following tutorials completed in sequence:</span></span>
  * [<span data-ttu-id="36d5c-120">Nastavení Intel NUC jako brány IoT</span><span class="sxs-lookup"><span data-stu-id="36d5c-120">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [<span data-ttu-id="36d5c-121">Použít IoT brány tooconnect věcí toohello cloudu - SensorTag tooAzure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="36d5c-121">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* <span data-ttu-id="36d5c-122">Klient SSH, který běží v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="36d5c-122">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="36d5c-123">V systému Windows se doporučuje puTTY.</span><span class="sxs-lookup"><span data-stu-id="36d5c-123">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="36d5c-124">Linux a systému macOS již obsahují klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="36d5c-124">Linux and macOS already come with an SSH client.</span></span>
* <span data-ttu-id="36d5c-125">Hello IP adresa a hello uživatelské jméno a heslo tooaccess hello brány z klienta SSH hello.</span><span class="sxs-lookup"><span data-stu-id="36d5c-125">hello IP address and hello username and password tooaccess hello gateway from hello SSH client.</span></span>
* <span data-ttu-id="36d5c-126">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="36d5c-126">An Internet connection.</span></span>

## <a name="create-a-module"></a><span data-ttu-id="36d5c-127">Vytvoření modulu</span><span class="sxs-lookup"><span data-stu-id="36d5c-127">Create a module</span></span>

1. <span data-ttu-id="36d5c-128">Na hostitelském počítači hello spusťte hello klient SSH a připojit toohello IoT bránu.</span><span class="sxs-lookup"><span data-stu-id="36d5c-128">On hello host computer, run hello SSH client and connect toohello IoT gateway.</span></span>
1. <span data-ttu-id="36d5c-129">Klonování hello zdrojové soubory modulu převod hello z Githubu toohello domovský adresář hello IoT brány spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="36d5c-129">Clone hello source files of hello conversion module from GitHub toohello home directory of hello IoT gateway by running hello following commands:</span></span>

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   <span data-ttu-id="36d5c-130">Toto je nativní modul Azure Edge napsané v hello programovacího jazyka C.</span><span class="sxs-lookup"><span data-stu-id="36d5c-130">This is a native Azure Edge module written in hello C programming language.</span></span> <span data-ttu-id="36d5c-131">modul Hello převede hello formát přijaté zprávy hello jeden následující:</span><span class="sxs-lookup"><span data-stu-id="36d5c-131">hello module converts hello format of received messages into hello following one:</span></span>

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-hello-module"></a><span data-ttu-id="36d5c-132">Kompilace hello modulu</span><span class="sxs-lookup"><span data-stu-id="36d5c-132">Compile hello module</span></span>

<span data-ttu-id="36d5c-133">modul hello toocompile spustit hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="36d5c-133">toocompile hello module, run hello following commands:</span></span>

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change hello build script runnable
chmod 777 build.sh
# remove hello invalid windows character
sed -i -e "s/\r$//" build.sh
# run hello build shell script
./build.sh
```

<span data-ttu-id="36d5c-134">Můžete získat `libmy_module.so` souboru po dokončení hello kompilace.</span><span class="sxs-lookup"><span data-stu-id="36d5c-134">You get a `libmy_module.so` file after hello compile is completed.</span></span> <span data-ttu-id="36d5c-135">Poznamenejte si hello absolutní cesta tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="36d5c-135">Make a note of hello absolute path of this file.</span></span>

## <a name="add-hello-module-toohello-ble-sample-application"></a><span data-ttu-id="36d5c-136">Přidání hello modulu toohello zakázat ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="36d5c-136">Add hello module toohello BLE sample application</span></span>

1. <span data-ttu-id="36d5c-137">Ukázky složky přejděte toohello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36d5c-137">Go toohello samples folder by running hello following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. <span data-ttu-id="36d5c-138">Otevřete konfigurační soubor hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36d5c-138">Open hello configuration file by running hello following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="36d5c-139">Přidat modul vložením hello následující kód toohello `modules` části.</span><span class="sxs-lookup"><span data-stu-id="36d5c-139">Add a module by inserting hello following code toohello `modules` section.</span></span>

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. <span data-ttu-id="36d5c-140">Nahraďte `[Your libmy_module.so path]` v kódu hello s hello absolutní cesta hello libmy_module.so' souboru.</span><span class="sxs-lookup"><span data-stu-id="36d5c-140">Replace `[Your libmy_module.so path]` in hello code with hello absolute path of hello libmy_module.so\` file.</span></span>
1. <span data-ttu-id="36d5c-141">Nahraďte kód hello v hello `links` oddíl s hello jeden následující:</span><span class="sxs-lookup"><span data-stu-id="36d5c-141">Replace hello code in hello `links` section with hello following one:</span></span>

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. <span data-ttu-id="36d5c-142">Stiskněte klávesu `ESC`a pak zadejte `:wq` toosave hello souboru.</span><span class="sxs-lookup"><span data-stu-id="36d5c-142">Press `ESC`, and then type `:wq` toosave hello file.</span></span>

## <a name="run-hello-sample-application"></a><span data-ttu-id="36d5c-143">Spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="36d5c-143">Run hello sample application</span></span>

1. <span data-ttu-id="36d5c-144">Zapnutí hello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="36d5c-144">Power on hello SensorTag.</span></span>
1. <span data-ttu-id="36d5c-145">Nastavit proměnnou prostředí SSL_CERT_FILE hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36d5c-145">Set hello SSL_CERT_FILE environment variable by running hello following command:</span></span>

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. <span data-ttu-id="36d5c-146">Spusťte hello ukázkovou aplikaci s přidaným modulem hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="36d5c-146">Run hello sample application with hello added module by running hello following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="36d5c-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36d5c-147">Next steps</span></span>

<span data-ttu-id="36d5c-148">Úspěšně jste použití hello IoT brány tooconvert hello zprávu od SensorTag do formátu .json hello.</span><span class="sxs-lookup"><span data-stu-id="36d5c-148">You’ve successfully use hello IoT gateway tooconvert hello message from SensorTag into hello .json format.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
