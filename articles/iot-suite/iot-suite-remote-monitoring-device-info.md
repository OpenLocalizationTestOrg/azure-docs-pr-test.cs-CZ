---
title: "informace metadat aaaDevice v hello řešení vzdáleného sledování | Microsoft Docs"
description: "Popis hello Azure IoT předkonfigurovaného řešení vzdáleného monitorování a jeho architektura."
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
ms.openlocfilehash: 8387b98b8b2ae4934b0c900bc4df37dc17337c60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="device-information-metadata-in-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="a6fbb-103">Informace metadat zařízení v hello předkonfigurovanému řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="a6fbb-103">Device information metadata in hello remote monitoring preconfigured solution</span></span>

<span data-ttu-id="a6fbb-104">Hello Azure IoT Suite předkonfigurovanému řešení vzdáleného monitorování ukazuje přístup pro správu metadat zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-104">hello Azure IoT Suite remote monitoring preconfigured solution demonstrates an approach for managing device metadata.</span></span> <span data-ttu-id="a6fbb-105">Tento článek popisuje způsob hello toto řešení trvá tooenable toounderstand můžete:</span><span class="sxs-lookup"><span data-stu-id="a6fbb-105">This article outlines hello approach this solution takes tooenable you toounderstand:</span></span>

* <span data-ttu-id="a6fbb-106">Jaká zařízení metadata hello řešení ukládá.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-106">What device metadata hello solution stores.</span></span>
* <span data-ttu-id="a6fbb-107">Jak řešení hello spravuje hello metadat zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-107">How hello solution manages hello device metadata.</span></span>

## <a name="context"></a><span data-ttu-id="a6fbb-108">Kontext</span><span class="sxs-lookup"><span data-stu-id="a6fbb-108">Context</span></span>

<span data-ttu-id="a6fbb-109">Hello vzdálené monitorování předkonfigurované řešení používá [Azure IoT Hub] [ lnk-iot-hub] tooenable vaše zařízení toosend data toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-109">hello remote monitoring preconfigured solution uses [Azure IoT Hub][lnk-iot-hub] tooenable your devices toosend data toohello cloud.</span></span> <span data-ttu-id="a6fbb-110">Hello řešení ukládá informace o zařízeních ve třech různých umístěních:</span><span class="sxs-lookup"><span data-stu-id="a6fbb-110">hello solution stores information about devices in three different locations:</span></span>

| <span data-ttu-id="a6fbb-111">Umístění</span><span class="sxs-lookup"><span data-stu-id="a6fbb-111">Location</span></span> | <span data-ttu-id="a6fbb-112">Informace uložené</span><span class="sxs-lookup"><span data-stu-id="a6fbb-112">Information stored</span></span> | <span data-ttu-id="a6fbb-113">Implementace</span><span class="sxs-lookup"><span data-stu-id="a6fbb-113">Implementation</span></span> |
| -------- | ------------------ | -------------- |
| <span data-ttu-id="a6fbb-114">Registr identit</span><span class="sxs-lookup"><span data-stu-id="a6fbb-114">Identity registry</span></span> | <span data-ttu-id="a6fbb-115">Id zařízení, ověřovací klíče, povolené stavu</span><span class="sxs-lookup"><span data-stu-id="a6fbb-115">Device id, authentication keys, enabled state</span></span> | <span data-ttu-id="a6fbb-116">Součástí tooIoT rozbočovače</span><span class="sxs-lookup"><span data-stu-id="a6fbb-116">Built in tooIoT Hub</span></span> |
| <span data-ttu-id="a6fbb-117">Dvojčata zařízení</span><span class="sxs-lookup"><span data-stu-id="a6fbb-117">Device twins</span></span> | <span data-ttu-id="a6fbb-118">Metadata: hlášené vlastnosti, požadované vlastnosti, značky</span><span class="sxs-lookup"><span data-stu-id="a6fbb-118">Metadata: reported properties, desired properties, tags</span></span> | <span data-ttu-id="a6fbb-119">Součástí tooIoT rozbočovače</span><span class="sxs-lookup"><span data-stu-id="a6fbb-119">Built in tooIoT Hub</span></span> |
| <span data-ttu-id="a6fbb-120">Databáze Cosmos</span><span class="sxs-lookup"><span data-stu-id="a6fbb-120">Cosmos DB</span></span> | <span data-ttu-id="a6fbb-121">Příkaz a metoda historie</span><span class="sxs-lookup"><span data-stu-id="a6fbb-121">Command and method history</span></span> | <span data-ttu-id="a6fbb-122">Vlastní řešení</span><span class="sxs-lookup"><span data-stu-id="a6fbb-122">Custom for solution</span></span> |

<span data-ttu-id="a6fbb-123">Služba IoT Hub obsahuje [registr identit zařízení] [ lnk-identity-registry] toomanage přístup tooan IoT hub a používá [dvojčata zařízení] [ lnk-device-twin] toomanage metadat zařízení .</span><span class="sxs-lookup"><span data-stu-id="a6fbb-123">IoT Hub includes a [device identity registry][lnk-identity-registry] toomanage access tooan IoT hub and uses [device twins][lnk-device-twin] toomanage device metadata.</span></span> <span data-ttu-id="a6fbb-124">Je také vzdálené monitorování řešení – konkrétní *registru zařízení* , ukládá příkaz a metoda historie.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-124">There is also a remote monitoring solution-specific *device registry* that stores command and method history.</span></span> <span data-ttu-id="a6fbb-125">Hello vzdálené monitorování používá řešení [Cosmos DB] [ lnk-docdb] databáze tooimplement vlastní úložiště pro příkaz a metoda historie.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-125">hello remote monitoring solution uses a [Cosmos DB][lnk-docdb] database tooimplement a custom store for command and method history.</span></span>

> [!NOTE]
> <span data-ttu-id="a6fbb-126">Hello předkonfigurovanému řešení vzdáleného monitorování udržuje registr identit zařízení hello synchronizována s hello informace v databázi Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-126">hello remote monitoring preconfigured solution keeps hello device identity registry in sync with hello information in hello Cosmos DB database.</span></span> <span data-ttu-id="a6fbb-127">Používejte hello stejné id toouniquely zařízení identifikaci jednotlivých zařízení připojené tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-127">Both use hello same device id toouniquely identify each device connected tooyour IoT hub.</span></span>

## <a name="device-metadata"></a><span data-ttu-id="a6fbb-128">Metadat zařízení</span><span class="sxs-lookup"><span data-stu-id="a6fbb-128">Device metadata</span></span>

<span data-ttu-id="a6fbb-129">IoT Hub uchovává [dvojče zařízení] [ lnk-device-twin] pro každé simulované a fyzické zařízení připojené tooa řešení vzdáleného sledování.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-129">IoT Hub maintains a [device twin][lnk-device-twin] for each simulated and physical device connected tooa remote monitoring solution.</span></span> <span data-ttu-id="a6fbb-130">řešení Hello používá zařízení dvojčata toomanage hello metadata přidružená zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-130">hello solution uses device twins toomanage hello metadata associated with devices.</span></span> <span data-ttu-id="a6fbb-131">Dvojče zařízení je dokument JSON spravuje pomocí služby IoT Hub a řešení hello používá hello IoT Hub API toointeract s dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-131">A device twin is a JSON document maintained by IoT Hub, and hello solution uses hello IoT Hub API toointeract with device twins.</span></span>

<span data-ttu-id="a6fbb-132">Tři typy metadata se ukládají dvojče zařízení:</span><span class="sxs-lookup"><span data-stu-id="a6fbb-132">A device twin stores three types of metadata:</span></span>

- <span data-ttu-id="a6fbb-133">*Hlášené vlastnosti* odesílá tooan IoT hub zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-133">*Reported properties* are sent tooan IoT hub by a device.</span></span> <span data-ttu-id="a6fbb-134">V řešení vzdáleného sledování hello, Simulovaná zařízení odesílají hlášené vlastnosti při spuštění a odpovědi příliš**změnit stav zařízení** příkazy a metody.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-134">In hello remote monitoring solution, simulated devices send reported properties at start-up and in response too**Change device state** commands and methods.</span></span> <span data-ttu-id="a6fbb-135">Můžete zobrazit vlastnosti hlášené v hello **seznam zařízení** a **podrobnosti o zařízení** portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-135">You can view reported properties in hello **Device list** and **Device details** in hello solution portal.</span></span> <span data-ttu-id="a6fbb-136">Hlášené vlastnosti jsou jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-136">Reported properties are read only.</span></span>
- <span data-ttu-id="a6fbb-137">*Požadovaného vlastnosti* se načítají ze služby IoT hub hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-137">*Desired properties* are retrieved from hello IoT hub by devices.</span></span> <span data-ttu-id="a6fbb-138">Je zodpovědností hello hello zařízení toomake žádnou nutné konfiguraci změnit na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-138">It is hello responsibility of hello device toomake any necessary configuration change on hello device.</span></span> <span data-ttu-id="a6fbb-139">Je také hello odpovědnost hello zařízení tooreport hello změnu zpět toohello centra jako hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-139">It is also hello responsibility of hello device tooreport hello change back toohello hub as a reported property.</span></span> <span data-ttu-id="a6fbb-140">Můžete nastavit hodnotu požadované vlastnosti prostřednictvím portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-140">You can set a desired property value through hello solution portal.</span></span>
- <span data-ttu-id="a6fbb-141">*Značky* existují jenom v dvojče zařízení hello a nikdy se synchronizují s zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-141">*Tags* only exist in hello device twin and are never synchronized with a device.</span></span> <span data-ttu-id="a6fbb-142">Můžete nastavit hodnoty značky hello řešení portálu a použít je při filtrování hello seznam zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-142">You can set tag values in hello solution portal and use them when you filter hello list of devices.</span></span> <span data-ttu-id="a6fbb-143">značky tooidentify hello ikonu toodisplay Hello řešení používá také pro zařízení na portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-143">hello solution also uses a tag tooidentify hello icon toodisplay for a device in hello solution portal.</span></span>

<span data-ttu-id="a6fbb-144">Příklad ohlásil, že vlastnosti z hello simulované zařízení zahrnují výrobce, číslo modelu, zeměpisnou šířku a zeměpisnou délku.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-144">Example reported properties from hello simulated devices include manufacturer, model number, latitude, and longitude.</span></span> <span data-ttu-id="a6fbb-145">Simulovaná zařízení také vrátí hello seznam podporovaných metod jako hlášené vlastnost.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-145">Simulated devices also return hello list of supported methods as a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="a6fbb-146">Hello kód simulované zařízení používá jenom hello **Desired.Config.TemperatureMeanValue** a **Desired.Config.TelemetryInterval** požadované vlastnosti tooupdate hello hlášené vlastnosti odeslána zpět tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-146">hello simulated device code only uses hello **Desired.Config.TemperatureMeanValue** and **Desired.Config.TelemetryInterval** desired properties tooupdate hello reported properties sent back tooIoT Hub.</span></span> <span data-ttu-id="a6fbb-147">Všechny ostatní žádosti o změnu požadovanou vlastnost se ignorují.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-147">All other desired property change requests are ignored.</span></span>

<span data-ttu-id="a6fbb-148">Dokument JSON metadata informace zařízení uložená v databázi Cosmos DB registru zařízení hello má hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="a6fbb-148">A device information metadata JSON document stored in hello device registry Cosmos DB database has hello following structure:</span></span>

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
> <span data-ttu-id="a6fbb-149">Informace o zařízení mohou také obsahovat metadata toodescribe hello telemetrie hello zařízení odesílá tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-149">Device information can also include metadata toodescribe hello telemetry hello device sends tooIoT Hub.</span></span> <span data-ttu-id="a6fbb-150">řešení vzdáleného monitorování Hello používá tento toocustomize metadata telemetrie jak hello řídicí panel zobrazuje [dynamické telemetrie][lnk-dynamic-telemetry].</span><span class="sxs-lookup"><span data-stu-id="a6fbb-150">hello remote monitoring solution uses this telemetry metadata toocustomize how hello dashboard displays [dynamic telemetry][lnk-dynamic-telemetry].</span></span>

## <a name="lifecycle"></a><span data-ttu-id="a6fbb-151">Životní cyklus</span><span class="sxs-lookup"><span data-stu-id="a6fbb-151">Lifecycle</span></span>

<span data-ttu-id="a6fbb-152">Při prvním vytváření zařízení na portálu řešení hello, řešení hello vytvoří záznam v hello příkaz toostore database Cosmos DB a metoda historie.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-152">When you first create a device in hello solution portal, hello solution creates an entry in hello Cosmos DB database toostore command and method history.</span></span> <span data-ttu-id="a6fbb-153">V tomto okamžiku hello řešení také vytvoří záznam pro hello zařízení v registru identit zařízení hello, který generuje hello klíče hello zařízení používá tooauthenticate službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-153">At this point, hello solution also creates an entry for hello device in hello device identity registry, which generates hello keys hello device uses tooauthenticate with IoT Hub.</span></span> <span data-ttu-id="a6fbb-154">Vytvoří také dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-154">It also creates a device twin.</span></span>

<span data-ttu-id="a6fbb-155">Když se zařízení poprvé připojí toohello řešení, odešle hlášené vlastnosti a informační zprávu zařízení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-155">When a device first connects toohello solution, it sends reported properties and a device information message.</span></span> <span data-ttu-id="a6fbb-156">Hello ohlásil, že hodnoty vlastností se automaticky uloží v dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-156">hello reported property values are automatically saved in hello device twin.</span></span> <span data-ttu-id="a6fbb-157">Hello ohlásil, že vlastnosti zahrnují hello výrobce zařízení, číslo modelu, sériové číslo a seznam podporovaných metod.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-157">hello reported properties include hello device manufacturer, model number, serial number, and a list of supported methods.</span></span> <span data-ttu-id="a6fbb-158">Hello zařízení informační zprávu zahrnuje hello seznam hello příkazy, které podporuje hello zařízení včetně informací o všech parametrů příkazu.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-158">hello device information message includes hello list of hello commands hello device supports including information about any command parameters.</span></span> <span data-ttu-id="a6fbb-159">Při řešení hello obdrží tuto zprávu, aktualizuje informace o zařízení hello v databázi Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-159">When hello solution receives this message, it updates hello device information in hello Cosmos DB database.</span></span>

### <a name="view-and-edit-device-information-in-hello-solution-portal"></a><span data-ttu-id="a6fbb-160">Zobrazit a upravit informace o zařízení na portálu řešení hello</span><span class="sxs-lookup"><span data-stu-id="a6fbb-160">View and edit device information in hello solution portal</span></span>

<span data-ttu-id="a6fbb-161">Hello seznam zařízení na portálu řešení hello zobrazí následující vlastnosti zařízení jako sloupce ve výchozím nastavení hello: **stav**, **DeviceId**, **výrobce**, **Číslo modelu**, **sériové číslo**, **Firmware**, **platformy**, **procesoru**a  **Nainstalovaná paměť RAM**.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-161">hello device list in hello solution portal displays hello following device properties as columns by default: **Status**, **DeviceId**, **Manufacturer**, **Model Number**, **Serial Number**, **Firmware**, **Platform**, **Processor**, and **Installed RAM**.</span></span> <span data-ttu-id="a6fbb-162">Hello sloupce můžete přizpůsobit kliknutím **editor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-162">You can customize hello columns by clicking **Column editor**.</span></span> <span data-ttu-id="a6fbb-163">Hello vlastnosti zařízení **zeměpisnou šířku** a **zeměpisné délky** jednotka hello umístění v hello mapy Bing na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-163">hello device properties **Latitude** and **Longitude** drive hello location in hello Bing Map on hello dashboard.</span></span>

![Editor sloupců v seznamu zařízení][img-device-list]

<span data-ttu-id="a6fbb-165">V hello **podrobnosti o zařízení** podokně hello řešení portálu, můžete upravit požadované vlastnosti a značky (hlášené vlastnosti jsou jen pro čtení).</span><span class="sxs-lookup"><span data-stu-id="a6fbb-165">In hello **Device Details** pane in hello solution portal, you can edit desired properties and tags (reported properties are read only).</span></span>

![V podokně podrobností zařízení][img-device-edit]

<span data-ttu-id="a6fbb-167">Hello řešení portálu tooremove zařízení můžete použít z vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-167">You can use hello solution portal tooremove a device from your solution.</span></span> <span data-ttu-id="a6fbb-168">Při odebrání zařízení hello řešení odebere položku hello zařízení z registru identit a poté se odstraní dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-168">When you remove a device, hello solution removes hello device entry from identity registry and then deletes hello device twin.</span></span> <span data-ttu-id="a6fbb-169">řešení Hello také odebere zařízení toohello související informace z databáze Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-169">hello solution also removes information related toohello device from hello Cosmos DB database.</span></span> <span data-ttu-id="a6fbb-170">Před odebráním zařízení, je nutné ho vypnout.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-170">Before you can remove a device, you must disable it.</span></span>

![Odebrání zařízení][img-device-remove]

## <a name="device-information-message-processing"></a><span data-ttu-id="a6fbb-172">Zpracování zpráv informace o zařízení</span><span class="sxs-lookup"><span data-stu-id="a6fbb-172">Device information message processing</span></span>

<span data-ttu-id="a6fbb-173">Zprávy s informacemi o zařízení odeslaný zařízením se liší od telemetrické zprávy.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-173">Device information messages sent by a device are distinct from telemetry messages.</span></span> <span data-ttu-id="a6fbb-174">Zprávy s informacemi o zařízení zahrnovat hello příkazy, které může reagovat na zařízení a všechny historii příkazů.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-174">Device information messages include hello commands a device can respond to, and any command history.</span></span> <span data-ttu-id="a6fbb-175">IoT Hub, sám nemá žádné informace o hello metadat obsažených v informační zprávu zařízení a procesy hello zprávy v hello stejně jako zpracovává všechny zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-175">IoT Hub itself has no knowledge of hello metadata contained in a device information message and processes hello message in hello same way it processes any device-to-cloud message.</span></span> <span data-ttu-id="a6fbb-176">V řešení, vzdáleného sledování hello [Azure Stream Analytics] [ lnk-stream-analytics] úlohy (ASA) čte zprávy hello ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-176">In hello remote monitoring solution, an [Azure Stream Analytics][lnk-stream-analytics] (ASA) job reads hello messages from IoT Hub.</span></span> <span data-ttu-id="a6fbb-177">Hello **DeviceInfo** úlohy stream analytics se filtry pro zprávy, které obsahují **"ObjectType": "DeviceInfo"** a předává je toohello **EventProcessorHost** hostitele instance, která běží ve webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-177">hello **DeviceInfo** stream analytics job filters for messages that contain **"ObjectType": "DeviceInfo"** and forwards them toohello **EventProcessorHost** host instance that runs in a web job.</span></span> <span data-ttu-id="a6fbb-178">Logika v hello **EventProcessorHost** instance používá hello zařízení id toofind hello Cosmos DB záznam pro hello konkrétní zařízení a aktualizace záznam hello.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-178">Logic in hello **EventProcessorHost** instance uses hello device id toofind hello Cosmos DB record for hello specific device and update hello record.</span></span>

> [!NOTE]
> <span data-ttu-id="a6fbb-179">Informační zprávu zařízení je standardní zpráva zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-179">A device information message is a standard device-to-cloud message.</span></span> <span data-ttu-id="a6fbb-180">řešení Hello rozlišuje mezi zprávy s informacemi o zařízení a telemetrické zprávy pomocí ASA dotazů.</span><span class="sxs-lookup"><span data-stu-id="a6fbb-180">hello solution distinguishes between device information messages and telemetry messages by using ASA queries.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6fbb-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6fbb-181">Next steps</span></span>

<span data-ttu-id="a6fbb-182">Nyní jste dokončili učení, jak můžete přizpůsobit hello předkonfigurované řešení, můžete některé z hello prozkoumat další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="a6fbb-182">Now you've finished learning how you can customize hello preconfigured solutions, you can explore some of hello other features and capabilities of hello IoT Suite preconfigured solutions:</span></span>

* <span data-ttu-id="a6fbb-183">[Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]</span><span class="sxs-lookup"><span data-stu-id="a6fbb-183">[Predictive maintenance preconfigured solution overview][lnk-predictive-overview]</span></span>
* <span data-ttu-id="a6fbb-184">[Nejčastější dotazy k sadě IoT Suite][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="a6fbb-184">[Frequently asked questions for IoT Suite][lnk-faq]</span></span>
* <span data-ttu-id="a6fbb-185">[Zabezpečení IoT z hello pozadí][lnk-security-groundup]</span><span class="sxs-lookup"><span data-stu-id="a6fbb-185">[IoT security from hello ground up][lnk-security-groundup]</span></span>

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
