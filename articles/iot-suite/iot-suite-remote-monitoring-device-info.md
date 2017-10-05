---
title: "Informace metadat zařízení v řešení vzdáleného monitorování | Microsoft Docs"
description: "Popis předkonfigurovaného řešení Azure IoT pro vzdálené monitorování a jeho architektura."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: f8fd452806a0a0b98cf8e434c9bd55700083a6c5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="0328e-103">Informace metadat zařízení v předkonfigurovaného řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="0328e-103">Device information metadata in the remote monitoring preconfigured solution</span></span>

<span data-ttu-id="0328e-104">Azure IoT Suite předkonfigurovanému řešení vzdáleného monitorování ukazuje přístup pro správu metadat zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-104">The Azure IoT Suite remote monitoring preconfigured solution demonstrates an approach for managing device metadata.</span></span> <span data-ttu-id="0328e-105">Tento článek popisuje přístupů, které trvá toto řešení vám umožní pochopit:</span><span class="sxs-lookup"><span data-stu-id="0328e-105">This article outlines the approach this solution takes to enable you to understand:</span></span>

* <span data-ttu-id="0328e-106">Jaká zařízení metadata ukládá řešení.</span><span class="sxs-lookup"><span data-stu-id="0328e-106">What device metadata the solution stores.</span></span>
* <span data-ttu-id="0328e-107">Jak řešení spravuje metadat zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-107">How the solution manages the device metadata.</span></span>

## <a name="context"></a><span data-ttu-id="0328e-108">Kontext</span><span class="sxs-lookup"><span data-stu-id="0328e-108">Context</span></span>

<span data-ttu-id="0328e-109">Vzdálené monitorování předkonfigurované řešení používá [Azure IoT Hub] [ lnk-iot-hub] povolit zařízení k odesílání dat do cloudu.</span><span class="sxs-lookup"><span data-stu-id="0328e-109">The remote monitoring preconfigured solution uses [Azure IoT Hub][lnk-iot-hub] to enable your devices to send data to the cloud.</span></span> <span data-ttu-id="0328e-110">Řešení ukládá informace o zařízeních ve třech různých umístěních:</span><span class="sxs-lookup"><span data-stu-id="0328e-110">The solution stores information about devices in three different locations:</span></span>

| <span data-ttu-id="0328e-111">Umístění</span><span class="sxs-lookup"><span data-stu-id="0328e-111">Location</span></span> | <span data-ttu-id="0328e-112">Informace uložené</span><span class="sxs-lookup"><span data-stu-id="0328e-112">Information stored</span></span> | <span data-ttu-id="0328e-113">Implementace</span><span class="sxs-lookup"><span data-stu-id="0328e-113">Implementation</span></span> |
| -------- | ------------------ | -------------- |
| <span data-ttu-id="0328e-114">Registr identit</span><span class="sxs-lookup"><span data-stu-id="0328e-114">Identity registry</span></span> | <span data-ttu-id="0328e-115">Id zařízení, ověřovací klíče, povolené stavu</span><span class="sxs-lookup"><span data-stu-id="0328e-115">Device id, authentication keys, enabled state</span></span> | <span data-ttu-id="0328e-116">Integrovaný do služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="0328e-116">Built in to IoT Hub</span></span> |
| <span data-ttu-id="0328e-117">Dvojčata zařízení</span><span class="sxs-lookup"><span data-stu-id="0328e-117">Device twins</span></span> | <span data-ttu-id="0328e-118">Metadata: hlášené vlastnosti, požadované vlastnosti, značky</span><span class="sxs-lookup"><span data-stu-id="0328e-118">Metadata: reported properties, desired properties, tags</span></span> | <span data-ttu-id="0328e-119">Integrovaný do služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="0328e-119">Built in to IoT Hub</span></span> |
| <span data-ttu-id="0328e-120">Databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="0328e-120">Cosmos DB</span></span> | <span data-ttu-id="0328e-121">Příkaz a metoda historie</span><span class="sxs-lookup"><span data-stu-id="0328e-121">Command and method history</span></span> | <span data-ttu-id="0328e-122">Vlastní řešení</span><span class="sxs-lookup"><span data-stu-id="0328e-122">Custom for solution</span></span> |

<span data-ttu-id="0328e-123">Služba IoT Hub obsahuje [registr identit zařízení] [ lnk-identity-registry] můžete spravovat přístup k služby IoT hub a používá [dvojčata zařízení] [ lnk-device-twin] ke správě zařízení metadat.</span><span class="sxs-lookup"><span data-stu-id="0328e-123">IoT Hub includes a [device identity registry][lnk-identity-registry] to manage access to an IoT hub and uses [device twins][lnk-device-twin] to manage device metadata.</span></span> <span data-ttu-id="0328e-124">Je také vzdálené monitorování řešení – konkrétní *registru zařízení* , ukládá příkaz a metoda historie.</span><span class="sxs-lookup"><span data-stu-id="0328e-124">There is also a remote monitoring solution-specific *device registry* that stores command and method history.</span></span> <span data-ttu-id="0328e-125">Používá řešení vzdáleného monitorování [Cosmos DB] [ lnk-docdb] databáze implementovat vlastní úložiště pro příkaz a metoda historie.</span><span class="sxs-lookup"><span data-stu-id="0328e-125">The remote monitoring solution uses a [Cosmos DB][lnk-docdb] database to implement a custom store for command and method history.</span></span>

> [!NOTE]
> <span data-ttu-id="0328e-126">Předkonfigurované řešení vzdáleného monitorování udržuje registr identit zařízení synchronizované s informacemi v databázi Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0328e-126">The remote monitoring preconfigured solution keeps the device identity registry in sync with the information in the Cosmos DB database.</span></span> <span data-ttu-id="0328e-127">Oba stejné id zařízení použít k jednoznačné identifikaci jednotlivých zařízení připojené ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0328e-127">Both use the same device id to uniquely identify each device connected to your IoT hub.</span></span>

## <a name="device-metadata"></a><span data-ttu-id="0328e-128">Metadat zařízení</span><span class="sxs-lookup"><span data-stu-id="0328e-128">Device metadata</span></span>

<span data-ttu-id="0328e-129">IoT Hub uchovává [dvojče zařízení] [ lnk-device-twin] každé simulované a fyzické zařízení připojeného k řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="0328e-129">IoT Hub maintains a [device twin][lnk-device-twin] for each simulated and physical device connected to a remote monitoring solution.</span></span> <span data-ttu-id="0328e-130">Toto řešení využívá dvojčata zařízení ke správě budou metadata spojená s zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-130">The solution uses device twins to manage the metadata associated with devices.</span></span> <span data-ttu-id="0328e-131">Dvojče zařízení je dokument JSON spravuje pomocí služby IoT Hub a řešení používá rozhraní API služby IoT Hub pro interakci s dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-131">A device twin is a JSON document maintained by IoT Hub, and the solution uses the IoT Hub API to interact with device twins.</span></span>

<span data-ttu-id="0328e-132">Tři typy metadata se ukládají dvojče zařízení:</span><span class="sxs-lookup"><span data-stu-id="0328e-132">A device twin stores three types of metadata:</span></span>

- <span data-ttu-id="0328e-133">*Hlášené vlastnosti* zařízení odesílá do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0328e-133">*Reported properties* are sent to an IoT hub by a device.</span></span> <span data-ttu-id="0328e-134">V řešení vzdáleného monitorování, Simulovaná zařízení odesílají hlášené vlastnosti při spuštění a v reakci na **změnit stav zařízení** příkazy a metody.</span><span class="sxs-lookup"><span data-stu-id="0328e-134">In the remote monitoring solution, simulated devices send reported properties at start-up and in response to **Change device state** commands and methods.</span></span> <span data-ttu-id="0328e-135">Můžete zobrazit vlastnosti hlášené v **seznam zařízení** a **podrobnosti o zařízení** na portálu řešení.</span><span class="sxs-lookup"><span data-stu-id="0328e-135">You can view reported properties in the **Device list** and **Device details** in the solution portal.</span></span> <span data-ttu-id="0328e-136">Hlášené vlastnosti jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="0328e-136">Reported properties are read only.</span></span>
- <span data-ttu-id="0328e-137">*Požadovaného vlastnosti* se načítají ze služby IoT hub zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-137">*Desired properties* are retrieved from the IoT hub by devices.</span></span> <span data-ttu-id="0328e-138">Je zodpovědností zařízení, aby všechny nezbytné konfigurace změnit na zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-138">It is the responsibility of the device to make any necessary configuration change on the device.</span></span> <span data-ttu-id="0328e-139">Je také odpovědnost zařízení nahlásit změnu zpět k centru jako hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0328e-139">It is also the responsibility of the device to report the change back to the hub as a reported property.</span></span> <span data-ttu-id="0328e-140">Můžete nastavit hodnotu požadované vlastnosti prostřednictvím portálu řešení.</span><span class="sxs-lookup"><span data-stu-id="0328e-140">You can set a desired property value through the solution portal.</span></span>
- <span data-ttu-id="0328e-141">*Značky* existují jenom v dvojče zařízení a nikdy se synchronizují s zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-141">*Tags* only exist in the device twin and are never synchronized with a device.</span></span> <span data-ttu-id="0328e-142">Můžete nastavit hodnoty značky na portálu řešení a použít je při filtrování seznamu zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-142">You can set tag values in the solution portal and use them when you filter the list of devices.</span></span> <span data-ttu-id="0328e-143">Značku řešení také používá k identifikaci ikonu zobrazíte pro zařízení na portálu řešení.</span><span class="sxs-lookup"><span data-stu-id="0328e-143">The solution also uses a tag to identify the icon to display for a device in the solution portal.</span></span>

<span data-ttu-id="0328e-144">Příklad ohlásil, že vlastnosti z Simulovaná zařízení zahrnují výrobce, číslo modelu, zeměpisnou šířku a zeměpisnou délku.</span><span class="sxs-lookup"><span data-stu-id="0328e-144">Example reported properties from the simulated devices include manufacturer, model number, latitude, and longitude.</span></span> <span data-ttu-id="0328e-145">Simulovaná zařízení také vrátí seznam podporovaných metod jako hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0328e-145">Simulated devices also return the list of supported methods as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="0328e-146">Kód simulovaného zařízení k aktualizaci ohlášených vlastností odeslaných zpět do služby IoT Hub využívá pouze požadované vlastnosti **Desired.Config.TemperatureMeanValue** a **Desired.Config.TelemetryInterval**.</span><span class="sxs-lookup"><span data-stu-id="0328e-146">The simulated device code only uses the **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties to update the reported properties sent back to IoT Hub.</span></span> <span data-ttu-id="0328e-147">Všechny ostatní žádosti o změnu požadovanou vlastnost se ignorují.</span><span class="sxs-lookup"><span data-stu-id="0328e-147">All other desired property change requests are ignored.</span></span>

<span data-ttu-id="0328e-148">Dokument JSON metadata informace zařízení uložena v databázi Cosmos DB registru zařízení má následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="0328e-148">A device information metadata JSON document stored in the device registry Cosmos DB database has the following structure:</span></span>

```json
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

> [!NOTE]
> <span data-ttu-id="0328e-149">Informace o zařízení může také obsahovat metadata k popisu telemetrie, které zařízení odesílá do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="0328e-149">Device information can also include metadata to describe the telemetry the device sends to IoT Hub.</span></span> <span data-ttu-id="0328e-150">Řešení vzdáleného monitorování používá tato metadata telemetrie přizpůsobit jak řídicí panel zobrazuje [dynamické telemetrie][lnk-dynamic-telemetry].</span><span class="sxs-lookup"><span data-stu-id="0328e-150">The remote monitoring solution uses this telemetry metadata to customize how the dashboard displays [dynamic telemetry][lnk-dynamic-telemetry].</span></span>

## <a name="lifecycle"></a><span data-ttu-id="0328e-151">Životní cyklus</span><span class="sxs-lookup"><span data-stu-id="0328e-151">Lifecycle</span></span>

<span data-ttu-id="0328e-152">Při prvním vytváření zařízení na portálu řešení, řešení vytvoří položku v databázi Cosmos DB pro ukládání historie příkaz a metoda.</span><span class="sxs-lookup"><span data-stu-id="0328e-152">When you first create a device in the solution portal, the solution creates an entry in the Cosmos DB database to store command and method history.</span></span> <span data-ttu-id="0328e-153">V tomto okamžiku řešení také vytvoří záznam zařízení v registru identit zařízení, která generuje klíče, který zařízení používá k ověření službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="0328e-153">At this point, the solution also creates an entry for the device in the device identity registry, which generates the keys the device uses to authenticate with IoT Hub.</span></span> <span data-ttu-id="0328e-154">Vytvoří také dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-154">It also creates a device twin.</span></span>

<span data-ttu-id="0328e-155">Když se zařízení poprvé připojí k řešení, odešle hlášené vlastnosti a informační zprávu zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-155">When a device first connects to the solution, it sends reported properties and a device information message.</span></span> <span data-ttu-id="0328e-156">Hodnoty hlášené vlastností automaticky uloží do dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-156">The reported property values are automatically saved in the device twin.</span></span> <span data-ttu-id="0328e-157">Hlášené vlastnosti zahrnují výrobce zařízení, číslo modelu, sériové číslo a seznam podporovaných metod.</span><span class="sxs-lookup"><span data-stu-id="0328e-157">The reported properties include the device manufacturer, model number, serial number, and a list of supported methods.</span></span> <span data-ttu-id="0328e-158">Informační zpráva zařízení obsahuje seznam příkazů, které podporuje zařízení, včetně informací o všech parametrů příkazu.</span><span class="sxs-lookup"><span data-stu-id="0328e-158">The device information message includes the list of the commands the device supports including information about any command parameters.</span></span> <span data-ttu-id="0328e-159">Při řešení obdrží tuto zprávu, aktualizuje informace o zařízení v databázi Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0328e-159">When the solution receives this message, it updates the device information in the Cosmos DB database.</span></span>

### <a name="view-and-edit-device-information-in-the-solution-portal"></a><span data-ttu-id="0328e-160">Zobrazit a upravit informace o zařízení na portálu řešení</span><span class="sxs-lookup"><span data-stu-id="0328e-160">View and edit device information in the solution portal</span></span>

<span data-ttu-id="0328e-161">V seznamu zařízení na portálu řešení zobrazuje následující vlastnosti zařízení jako sloupce ve výchozím nastavení: **stav**, **DeviceId**, **výrobce**, **modelu Číslo**, **sériové číslo**, **Firmware**, **platformy**, **procesoru**, a  **Nainstalovaná paměť RAM**.</span><span class="sxs-lookup"><span data-stu-id="0328e-161">The device list in the solution portal displays the following device properties as columns by default: **Status**, **DeviceId**, **Manufacturer**, **Model Number**, **Serial Number**, **Firmware**, **Platform**, **Processor**, and **Installed RAM**.</span></span> <span data-ttu-id="0328e-162">Sloupce můžete přizpůsobit kliknutím **editor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="0328e-162">You can customize the columns by clicking **Column editor**.</span></span> <span data-ttu-id="0328e-163">Vlastnosti zařízení **zeměpisnou šířku** a **zeměpisné délky** jednotka umístění mapy Bing na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="0328e-163">The device properties **Latitude** and **Longitude** drive the location in the Bing Map on the dashboard.</span></span>

![Editor sloupců v seznamu zařízení][img-device-list]

<span data-ttu-id="0328e-165">V **podrobnosti o zařízení** podokně na portálu řešení můžete upravit požadované vlastnosti a značky (hlášené vlastnosti jsou jen pro čtení).</span><span class="sxs-lookup"><span data-stu-id="0328e-165">In the **Device Details** pane in the solution portal, you can edit desired properties and tags (reported properties are read only).</span></span>

![V podokně podrobností zařízení][img-device-edit]

<span data-ttu-id="0328e-167">Na portálu řešení můžete odebrat zařízení z vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="0328e-167">You can use the solution portal to remove a device from your solution.</span></span> <span data-ttu-id="0328e-168">Když odeberete zařízení, řešení odebere položku zařízení z registru identit a poté se odstraní dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="0328e-168">When you remove a device, the solution removes the device entry from identity registry and then deletes the device twin.</span></span> <span data-ttu-id="0328e-169">Řešení také odebere informace související s zařízení z databáze Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0328e-169">The solution also removes information related to the device from the Cosmos DB database.</span></span> <span data-ttu-id="0328e-170">Před odebráním zařízení, je nutné ho vypnout.</span><span class="sxs-lookup"><span data-stu-id="0328e-170">Before you can remove a device, you must disable it.</span></span>

![Odebrání zařízení][img-device-remove]

## <a name="device-information-message-processing"></a><span data-ttu-id="0328e-172">Zpracování zpráv informace o zařízení</span><span class="sxs-lookup"><span data-stu-id="0328e-172">Device information message processing</span></span>

<span data-ttu-id="0328e-173">Zprávy s informacemi o zařízení odeslaný zařízením se liší od telemetrické zprávy.</span><span class="sxs-lookup"><span data-stu-id="0328e-173">Device information messages sent by a device are distinct from telemetry messages.</span></span> <span data-ttu-id="0328e-174">Zprávy s informacemi o zařízení zahrnovat příkazy, které může reagovat na zařízení a všechny historii příkazů.</span><span class="sxs-lookup"><span data-stu-id="0328e-174">Device information messages include the commands a device can respond to, and any command history.</span></span> <span data-ttu-id="0328e-175">IoT Hub, sám nemá žádné informace o metadat obsažených v informační zprávu zařízení a zprávu zpracuje stejným způsobem zpracovává všechny zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="0328e-175">IoT Hub itself has no knowledge of the metadata contained in a device information message and processes the message in the same way it processes any device-to-cloud message.</span></span> <span data-ttu-id="0328e-176">V řešení vzdáleného monitorování [Azure Stream Analytics] [ lnk-stream-analytics] úlohy (ASA) čte zprávy ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="0328e-176">In the remote monitoring solution, an [Azure Stream Analytics][lnk-stream-analytics] (ASA) job reads the messages from IoT Hub.</span></span> <span data-ttu-id="0328e-177">**DeviceInfo** úlohy stream analytics se filtry pro zprávy, které obsahují **"ObjectType": "DeviceInfo"** a předává je na **EventProcessorHost** instance hostitele která se spouští v rámci webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="0328e-177">The **DeviceInfo** stream analytics job filters for messages that contain **"ObjectType": "DeviceInfo"** and forwards them to the **EventProcessorHost** host instance that runs in a web job.</span></span> <span data-ttu-id="0328e-178">Logiky **EventProcessorHost** instance používá id zařízení a najít záznam Cosmos DB pro určité zařízení aktualizovat záznam.</span><span class="sxs-lookup"><span data-stu-id="0328e-178">Logic in the **EventProcessorHost** instance uses the device id to find the Cosmos DB record for the specific device and update the record.</span></span>

> [!NOTE]
> <span data-ttu-id="0328e-179">Informační zprávu zařízení je standardní zpráva zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="0328e-179">A device information message is a standard device-to-cloud message.</span></span> <span data-ttu-id="0328e-180">Řešení rozlišuje mezi zprávy s informacemi o zařízení a telemetrické zprávy pomocí ASA dotazů.</span><span class="sxs-lookup"><span data-stu-id="0328e-180">The solution distinguishes between device information messages and telemetry messages by using ASA queries.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0328e-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0328e-181">Next steps</span></span>

<span data-ttu-id="0328e-182">Nyní jste dokončili učení, jak můžete přizpůsobit předkonfigurovaná řešení, můžete prozkoumat některé z dalších funkcí a možností předkonfigurovaná řešení IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="0328e-182">Now you've finished learning how you can customize the preconfigured solutions, you can explore some of the other features and capabilities of the IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="0328e-183">[Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="0328e-183">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* <span data-ttu-id="0328e-184">[Nejčastější dotazy k sadě IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="0328e-184">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="0328e-185">[Zabezpečení IoT od počátku][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="0328e-185">[IoT security from the ground up][lnk-security-groundup]</span></span>

<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
