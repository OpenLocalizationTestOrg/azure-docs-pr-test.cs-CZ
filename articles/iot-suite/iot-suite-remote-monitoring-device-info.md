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
# <a name="device-information-metadata-in-hello-remote-monitoring-preconfigured-solution"></a>Informace metadat zařízení v hello předkonfigurovanému řešení vzdáleného monitorování

Hello Azure IoT Suite předkonfigurovanému řešení vzdáleného monitorování ukazuje přístup pro správu metadat zařízení. Tento článek popisuje způsob hello toto řešení trvá tooenable toounderstand můžete:

* Jaká zařízení metadata hello řešení ukládá.
* Jak řešení hello spravuje hello metadat zařízení.

## <a name="context"></a>Kontext

Hello vzdálené monitorování předkonfigurované řešení používá [Azure IoT Hub] [ lnk-iot-hub] tooenable vaše zařízení toosend data toohello cloudu. Hello řešení ukládá informace o zařízeních ve třech různých umístěních:

| Umístění | Informace uložené | Implementace |
| -------- | ------------------ | -------------- |
| Registr identit | Id zařízení, ověřovací klíče, povolené stavu | Součástí tooIoT rozbočovače |
| Dvojčata zařízení | Metadata: hlášené vlastnosti, požadované vlastnosti, značky | Součástí tooIoT rozbočovače |
| Databáze Cosmos | Příkaz a metoda historie | Vlastní řešení |

Služba IoT Hub obsahuje [registr identit zařízení] [ lnk-identity-registry] toomanage přístup tooan IoT hub a používá [dvojčata zařízení] [ lnk-device-twin] toomanage metadat zařízení . Je také vzdálené monitorování řešení – konkrétní *registru zařízení* , ukládá příkaz a metoda historie. Hello vzdálené monitorování používá řešení [Cosmos DB] [ lnk-docdb] databáze tooimplement vlastní úložiště pro příkaz a metoda historie.

> [!NOTE]
> Hello předkonfigurovanému řešení vzdáleného monitorování udržuje registr identit zařízení hello synchronizována s hello informace v databázi Cosmos DB hello. Používejte hello stejné id toouniquely zařízení identifikaci jednotlivých zařízení připojené tooyour IoT hub.

## <a name="device-metadata"></a>Metadat zařízení

IoT Hub uchovává [dvojče zařízení] [ lnk-device-twin] pro každé simulované a fyzické zařízení připojené tooa řešení vzdáleného sledování. řešení Hello používá zařízení dvojčata toomanage hello metadata přidružená zařízení. Dvojče zařízení je dokument JSON spravuje pomocí služby IoT Hub a řešení hello používá hello IoT Hub API toointeract s dvojčata zařízení.

Tři typy metadata se ukládají dvojče zařízení:

- *Hlášené vlastnosti* odesílá tooan IoT hub zařízení. V řešení vzdáleného sledování hello, Simulovaná zařízení odesílají hlášené vlastnosti při spuštění a odpovědi příliš**změnit stav zařízení** příkazy a metody. Můžete zobrazit vlastnosti hlášené v hello **seznam zařízení** a **podrobnosti o zařízení** portálu řešení hello. Hlášené vlastnosti jsou jen pro čtení.
- *Požadovaného vlastnosti* se načítají ze služby IoT hub hello zařízení. Je zodpovědností hello hello zařízení toomake žádnou nutné konfiguraci změnit na hello zařízení. Je také hello odpovědnost hello zařízení tooreport hello změnu zpět toohello centra jako hlášené vlastnost. Můžete nastavit hodnotu požadované vlastnosti prostřednictvím portálu řešení hello.
- *Značky* existují jenom v dvojče zařízení hello a nikdy se synchronizují s zařízení. Můžete nastavit hodnoty značky hello řešení portálu a použít je při filtrování hello seznam zařízení. značky tooidentify hello ikonu toodisplay Hello řešení používá také pro zařízení na portálu řešení hello.

Příklad ohlásil, že vlastnosti z hello simulované zařízení zahrnují výrobce, číslo modelu, zeměpisnou šířku a zeměpisnou délku. Simulovaná zařízení také vrátí hello seznam podporovaných metod jako hlášené vlastnost.

> [!NOTE]
> Hello kód simulované zařízení používá jenom hello **Desired.Config.TemperatureMeanValue** a **Desired.Config.TelemetryInterval** požadované vlastnosti tooupdate hello hlášené vlastnosti odeslána zpět tooIoT rozbočovače. Všechny ostatní žádosti o změnu požadovanou vlastnost se ignorují.

Dokument JSON metadata informace zařízení uložená v databázi Cosmos DB registru zařízení hello má hello strukturu:

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
> Informace o zařízení mohou také obsahovat metadata toodescribe hello telemetrie hello zařízení odesílá tooIoT rozbočovače. řešení vzdáleného monitorování Hello používá tento toocustomize metadata telemetrie jak hello řídicí panel zobrazuje [dynamické telemetrie][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Životní cyklus

Při prvním vytváření zařízení na portálu řešení hello, řešení hello vytvoří záznam v hello příkaz toostore database Cosmos DB a metoda historie. V tomto okamžiku hello řešení také vytvoří záznam pro hello zařízení v registru identit zařízení hello, který generuje hello klíče hello zařízení používá tooauthenticate službou IoT Hub. Vytvoří také dvojče zařízení.

Když se zařízení poprvé připojí toohello řešení, odešle hlášené vlastnosti a informační zprávu zařízení. Hello ohlásil, že hodnoty vlastností se automaticky uloží v dvojče zařízení hello. Hello ohlásil, že vlastnosti zahrnují hello výrobce zařízení, číslo modelu, sériové číslo a seznam podporovaných metod. Hello zařízení informační zprávu zahrnuje hello seznam hello příkazy, které podporuje hello zařízení včetně informací o všech parametrů příkazu. Při řešení hello obdrží tuto zprávu, aktualizuje informace o zařízení hello v databázi Cosmos DB hello.

### <a name="view-and-edit-device-information-in-hello-solution-portal"></a>Zobrazit a upravit informace o zařízení na portálu řešení hello

Hello seznam zařízení na portálu řešení hello zobrazí následující vlastnosti zařízení jako sloupce ve výchozím nastavení hello: **stav**, **DeviceId**, **výrobce**, **Číslo modelu**, **sériové číslo**, **Firmware**, **platformy**, **procesoru**a  **Nainstalovaná paměť RAM**. Hello sloupce můžete přizpůsobit kliknutím **editor sloupců**. Hello vlastnosti zařízení **zeměpisnou šířku** a **zeměpisné délky** jednotka hello umístění v hello mapy Bing na řídicím panelu hello.

![Editor sloupců v seznamu zařízení][img-device-list]

V hello **podrobnosti o zařízení** podokně hello řešení portálu, můžete upravit požadované vlastnosti a značky (hlášené vlastnosti jsou jen pro čtení).

![V podokně podrobností zařízení][img-device-edit]

Hello řešení portálu tooremove zařízení můžete použít z vašeho řešení. Při odebrání zařízení hello řešení odebere položku hello zařízení z registru identit a poté se odstraní dvojče zařízení hello. řešení Hello také odebere zařízení toohello související informace z databáze Cosmos DB hello. Před odebráním zařízení, je nutné ho vypnout.

![Odebrání zařízení][img-device-remove]

## <a name="device-information-message-processing"></a>Zpracování zpráv informace o zařízení

Zprávy s informacemi o zařízení odeslaný zařízením se liší od telemetrické zprávy. Zprávy s informacemi o zařízení zahrnovat hello příkazy, které může reagovat na zařízení a všechny historii příkazů. IoT Hub, sám nemá žádné informace o hello metadat obsažených v informační zprávu zařízení a procesy hello zprávy v hello stejně jako zpracovává všechny zprávy typu zařízení cloud. V řešení, vzdáleného sledování hello [Azure Stream Analytics] [ lnk-stream-analytics] úlohy (ASA) čte zprávy hello ze služby IoT Hub. Hello **DeviceInfo** úlohy stream analytics se filtry pro zprávy, které obsahují **"ObjectType": "DeviceInfo"** a předává je toohello **EventProcessorHost** hostitele instance, která běží ve webové úlohy. Logika v hello **EventProcessorHost** instance používá hello zařízení id toofind hello Cosmos DB záznam pro hello konkrétní zařízení a aktualizace záznam hello.

> [!NOTE]
> Informační zprávu zařízení je standardní zpráva zařízení cloud. řešení Hello rozlišuje mezi zprávy s informacemi o zařízení a telemetrické zprávy pomocí ASA dotazů.

## <a name="next-steps"></a>Další kroky

Nyní jste dokončili učení, jak můžete přizpůsobit hello předkonfigurované řešení, můžete některé z hello prozkoumat další funkce a možnosti hello předkonfigurovaná řešení IoT Suite:

* [Přehled řešení předkonfigurované prediktivní údržby][lnk-predictive-overview]
* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]
* [Zabezpečení IoT z hello pozadí][lnk-security-groundup]

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
