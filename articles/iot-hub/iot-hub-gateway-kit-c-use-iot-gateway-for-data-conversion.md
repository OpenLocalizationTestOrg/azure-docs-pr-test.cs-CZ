---
title: "Převod dat na IoT brány s hranou Azure IoT | Microsoft Docs"
description: "Pomocí brány IoT převést formát data snímačů pomocí vlastní modul od Azure IoT okraje."
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
ms.openlocfilehash: d5c735a4adbc59e9526ec4fd40720c5ec136d63d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a><span data-ttu-id="376e7-104">Použít bránu IoT pro transformaci dat snímačů s hranou Azure IoT</span><span class="sxs-lookup"><span data-stu-id="376e7-104">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>

> [!NOTE]
> <span data-ttu-id="376e7-105">Než začnete tento kurz, ujistěte se, že jste dokončili následující lekce v pořadí:</span><span class="sxs-lookup"><span data-stu-id="376e7-105">Before you start this tutorial, make sure you’ve completed the following lessons in sequence:</span></span>
> * [<span data-ttu-id="376e7-106">Nastavení Intel NUC jako brány IoT</span><span class="sxs-lookup"><span data-stu-id="376e7-106">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [<span data-ttu-id="376e7-107">Použití IoT bránu pro připojení akcí k cloudu - SensorTag do služby Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="376e7-107">Use IoT gateway to connect things to the cloud - SensorTag to Azure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

<span data-ttu-id="376e7-108">Jediný účel bránu Iot se má zpracovat shromážděná data před odesláním do cloudu.</span><span class="sxs-lookup"><span data-stu-id="376e7-108">One purpose of an Iot gateway is to process collected data before sending it to the cloud.</span></span> <span data-ttu-id="376e7-109">Azure IoT Edge zavádí moduly, které lze vytvořit a sestaví formuláři zpracování dat pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="376e7-109">Azure IoT Edge introduces modules which can be created and assembled to form the data processing workflow.</span></span> <span data-ttu-id="376e7-110">Modul přijme nějakou zprávu, provádí některé akce a potom posuňte pro jiné modulů pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="376e7-110">A module receives a message, performs some action on it, and then move it on for other modules to process.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="376e7-111">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="376e7-111">What you learn</span></span>

<span data-ttu-id="376e7-112">Zjistíte, jak vytvořit modul převést zprávy z SensorTag do jiného formátu.</span><span class="sxs-lookup"><span data-stu-id="376e7-112">You learn how to create a module to convert messages from the SensorTag into a different format.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="376e7-113">Co dělat</span><span class="sxs-lookup"><span data-stu-id="376e7-113">What you do</span></span>

* <span data-ttu-id="376e7-114">Modul pro převod do formátu .json přijatou zprávu vytvořte.</span><span class="sxs-lookup"><span data-stu-id="376e7-114">Create a module to convert a received message into the .json format.</span></span>
* <span data-ttu-id="376e7-115">Zkompilujte modul.</span><span class="sxs-lookup"><span data-stu-id="376e7-115">Compile the module.</span></span>
* <span data-ttu-id="376e7-116">Přidání modulu k ukázkové aplikaci zakázat od Azure IoT okraje.</span><span class="sxs-lookup"><span data-stu-id="376e7-116">Add the module to the BLE sample application from Azure IoT Edge.</span></span>
* <span data-ttu-id="376e7-117">Spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="376e7-117">Run the sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="376e7-118">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="376e7-118">What you need</span></span>

* <span data-ttu-id="376e7-119">Následující kurzy dokončit v pořadí:</span><span class="sxs-lookup"><span data-stu-id="376e7-119">The following tutorials completed in sequence:</span></span>
  * [<span data-ttu-id="376e7-120">Nastavení Intel NUC jako brány IoT</span><span class="sxs-lookup"><span data-stu-id="376e7-120">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [<span data-ttu-id="376e7-121">Použití IoT bránu pro připojení akcí k cloudu - SensorTag do služby Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="376e7-121">Use IoT gateway to connect things to the cloud - SensorTag to Azure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* <span data-ttu-id="376e7-122">Klient SSH, který běží v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="376e7-122">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="376e7-123">V systému Windows se doporučuje puTTY.</span><span class="sxs-lookup"><span data-stu-id="376e7-123">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="376e7-124">Linux a systému macOS již obsahují klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="376e7-124">Linux and macOS already come with an SSH client.</span></span>
* <span data-ttu-id="376e7-125">IP adresa a uživatelské jméno a heslo pro přístup k bráně z klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="376e7-125">The IP address and the username and password to access the gateway from the SSH client.</span></span>
* <span data-ttu-id="376e7-126">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="376e7-126">An Internet connection.</span></span>

## <a name="create-a-module"></a><span data-ttu-id="376e7-127">Vytvoření modulu</span><span class="sxs-lookup"><span data-stu-id="376e7-127">Create a module</span></span>

1. <span data-ttu-id="376e7-128">Na hostitelském počítači spusťte použije klient SSH a připojení k bráně IoT.</span><span class="sxs-lookup"><span data-stu-id="376e7-128">On the host computer, run the SSH client and connect to the IoT gateway.</span></span>
1. <span data-ttu-id="376e7-129">Klonování zdrojové soubory modulu převod z webu GitHub na domovský adresář brány IoT tak, že spustíte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="376e7-129">Clone the source files of the conversion module from GitHub to the home directory of the IoT gateway by running the following commands:</span></span>

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   <span data-ttu-id="376e7-130">Toto je nativní modul Azure Edge napsané v C programovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="376e7-130">This is a native Azure Edge module written in the C programming language.</span></span> <span data-ttu-id="376e7-131">Modul převede formát přijaté zprávy tu následující:</span><span class="sxs-lookup"><span data-stu-id="376e7-131">The module converts the format of received messages into the following one:</span></span>

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-the-module"></a><span data-ttu-id="376e7-132">Kompilace modulu</span><span class="sxs-lookup"><span data-stu-id="376e7-132">Compile the module</span></span>

<span data-ttu-id="376e7-133">Kompilace modul, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="376e7-133">To compile the module, run the following commands:</span></span>

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change the build script runnable
chmod 777 build.sh
# remove the invalid windows character
sed -i -e "s/\r$//" build.sh
# run the build shell script
./build.sh
```

<span data-ttu-id="376e7-134">Můžete získat `libmy_module.so` souboru po dokončení kompilaci.</span><span class="sxs-lookup"><span data-stu-id="376e7-134">You get a `libmy_module.so` file after the compile is completed.</span></span> <span data-ttu-id="376e7-135">Poznamenejte si tento soubor absolutní cesta.</span><span class="sxs-lookup"><span data-stu-id="376e7-135">Make a note of the absolute path of this file.</span></span>

## <a name="add-the-module-to-the-ble-sample-application"></a><span data-ttu-id="376e7-136">Přidání modulu k ukázkové aplikaci zakázat</span><span class="sxs-lookup"><span data-stu-id="376e7-136">Add the module to the BLE sample application</span></span>

1. <span data-ttu-id="376e7-137">Přejděte do složky Ukázky spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="376e7-137">Go to the samples folder by running the following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. <span data-ttu-id="376e7-138">Otevřete konfigurační soubor tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="376e7-138">Open the configuration file by running the following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="376e7-139">Přidat modul vložením následující kód, který `modules` části.</span><span class="sxs-lookup"><span data-stu-id="376e7-139">Add a module by inserting the following code to the `modules` section.</span></span>

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

1. <span data-ttu-id="376e7-140">Nahraďte `[Your libmy_module.so path]` v kódu s absolutní cesta libmy_module.so' souboru.</span><span class="sxs-lookup"><span data-stu-id="376e7-140">Replace `[Your libmy_module.so path]` in the code with the absolute path of the libmy_module.so\` file.</span></span>
1. <span data-ttu-id="376e7-141">Nahraďte kód v `links` oddíl s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="376e7-141">Replace the code in the `links` section with the following one:</span></span>

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

1. <span data-ttu-id="376e7-142">Stiskněte klávesu `ESC`a pak zadejte `:wq` k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="376e7-142">Press `ESC`, and then type `:wq` to save the file.</span></span>

## <a name="run-the-sample-application"></a><span data-ttu-id="376e7-143">Spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="376e7-143">Run the sample application</span></span>

1. <span data-ttu-id="376e7-144">Zapnutí SensorTag.</span><span class="sxs-lookup"><span data-stu-id="376e7-144">Power on the SensorTag.</span></span>
1. <span data-ttu-id="376e7-145">Nastavte proměnnou prostředí SSL_CERT_FILE spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="376e7-145">Set the SSL_CERT_FILE environment variable by running the following command:</span></span>

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. <span data-ttu-id="376e7-146">Spusťte ukázkové aplikace s přidaným modulem spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="376e7-146">Run the sample application with the added module by running the following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="376e7-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="376e7-147">Next steps</span></span>

<span data-ttu-id="376e7-148">Bránu IoT jste úspěšně použijte k převedení zprávy z SensorTag do formátu .json.</span><span class="sxs-lookup"><span data-stu-id="376e7-148">You’ve successfully use the IoT gateway to convert the message from SensorTag into the .json format.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
