---
title: registr identit Azure IoT Hub hello aaaUnderstand | Microsoft Docs
description: "Příručka vývojáře – popis hello registru identit služby IoT Hub a jak toouse ho toomanage zařízení. Obsahuje informace o hello import a export identit zařízení hromadně."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0706eccd-e84c-4ae7-bbd4-2b1a22241147
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c9fe3730a4608e28c47807ecb42e13e73f6a2e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-identity-registry-in-your-iot-hub"></a>Pochopení hello registru identit ve službě IoT hub.

Každé centrum IoT má registru identit, která uchovává informace o zařízení hello povolené tooconnect toohello IoT hub. Než se zařízení může připojit tooan IoT hub, musí existovat položka pro toto zařízení v registru identit služby IoT hub hello. Zařízení musí také ověřovat službou IoT hub hello založené na přihlašovací údaje uložené v registru identit hello.

ID zařízení Hello uložené v registru identit hello rozlišuje velká a malá písmena.

Na vysoké úrovni je registr identit hello podporující REST kolekce prostředků identity zařízení. Když přidáte položku v registru identit hello, IoT Hub vytvoří sadu prostředků na zařízení, jako jsou hello fronty, který obsahuje neukládají zprávy typu cloud zařízení.

### <a name="when-toouse"></a>Když toouse

Registr identit hello použijte, když potřebujete:

* Zřizování zařízení, která se připojují tooyour IoT hub.
* Řízení na zařízení přístup tooyour rozbočovače na zařízení přístupem koncových bodů.

> [!NOTE]
> registr identit Hello neobsahuje žádné metadata specifická pro aplikaci.

## <a name="identity-registry-operations"></a>Operace registru identit

Hello registru identit služby IoT Hub zpřístupní hello následující operace:

* Vytvoření identity zařízení
* Aktualizovat identita zařízení.
* Načíst identitu zařízení podle ID
* Odstranit identita zařízení.
* Seznam si too1000 identity
* Exportovat všechny úložiště objektů blob tooAzure identity
* Import identit z Azure blob storage

Všechny tyto operace můžete použít optimistickou metodu souběžného, jak je uvedeno v [RFC7232][lnk-rfc7232].

> [!IMPORTANT]
> Hello pouze způsob tooretrieve všechny identity v registru identit služby IoT hub je toouse hello [exportovat] [ lnk-export] funkce.

Registr identit IoT Hub:

* Neobsahuje žádné metadata aplikace.
* Je přístupný pomocí hello jako slovník, **deviceId** jako klíč hello.
* Nepodporuje výrazovou dotazů.

Řešení IoT obvykle obsahuje samostatné úložiště specifické pro řešení, který obsahuje metadata specifické pro aplikaci. Například hello konkrétní řešení úložiště v inteligentní sestavování řešení by záznam hello místnosti, ve kterém je nasazená teplotní snímač.

> [!IMPORTANT]
> Registr identit hello lze použijte pouze pro správu zařízení a zajišťování operace. Vysoké propustnosti operace v době běhu nesmí závisí na provádění operací v registru identit hello. Například kontrola stavu připojení hello zařízení před odesláním příkazu není podporované vzor. Ujistěte se, zda text hello toocheck [míru omezení] [ lnk-quotas] pro registr identit hello a hello [prezenčního signálu zařízení] [ lnk-guidance-heartbeat] vzor.

## <a name="disable-devices"></a>Zakažte zařízení

Zařízení můžete zakázat aktualizací hello **stav** vlastnost identity v registru identit hello. Tato vlastnost se obvykle používá ve dvou scénářích:

* Během procesu zřizování orchestration. Další informace najdete v tématu [zřizování zařízení][lnk-guidance-provisioning].
* Pokud z nějakého důvodu byste zvážit, dojde k ohrožení bezpečnosti nebo se staly neoprávněné zařízení.

## <a name="import-and-export-device-identities"></a>Import a export identit zařízení

Identit zařízení hromadné z registru identit služby IoT hub, můžete exportovat pomocí asynchronních operací na hello [koncový bod zprostředkovatele prostředků služby IoT Hub][lnk-endpoints]. Exportuje běží dlouho, že úlohy, které používají identitu dat zákazníka zadaný objekt blob kontejneru toosave zařízení přečíst z registru identit hello.

Identit zařízení v registru identit hromadné tooan IoT hub společnosti, můžete importovat pomocí asynchronních operací na hello [koncový bod zprostředkovatele prostředků služby IoT Hub][lnk-endpoints]. Importy jsou dlouho běžící úlohy, které používají data v zákazníka zadaný objekt blob kontejneru toowrite zařízení identity dat do registru identit hello.

* Podrobné informace o hello import a export rozhraní API najdete v tématu [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis].
* toolearn informace o spuštění import a export úloh naleznete v tématu [hromadné správu identit zařízení IoT Hub][lnk-bulk-identity].

## <a name="device-provisioning"></a>Zřizování zařízení

data Hello zařízení, která ukládá daného řešení IoT, závisí na hello specifické požadavky tohoto řešení. Ale minimálně, musí řešení úložiště identit zařízení a ověřovací klíče. Azure IoT Hub obsahuje registr identity, který může ukládat hodnoty pro každé zařízení, jako je například ID ověřovací klíče a stavové kódy. Řešení můžete použít jinými službami Azure, jako je například úložiště table, úložiště objektů blob nebo Cosmos DB toostore žádná další zařízení data.

*Zřizování zařízení* je hello proces přidávání úložiště toohello dat hello počáteční zařízení ve vašem řešení. tooenable nového centra tooyour tooconnect zařízení, je nutné přidat zařízení ID a klíče toohello registru identit služby IoT Hub. Jako součást procesu zřizování hello bude pravděpodobně nutné tooinitialize data specifická pro zařízení v jiné řešení úložiště.

## <a name="device-heartbeat"></a>Prezenční signál zařízení

Hello registru identit služby IoT Hub obsahuje pole s názvem **connectionState**. Používat pouze hello **connectionState** pole při vývoji a ladění. Řešení IoT by neměl dotaz hello pole za běhu. Například není dotaz hello **connectionState** toocheck pole, pokud je zařízení připojené před odesláním zprávy typu cloud zařízení nebo serveru služby SMS.

Pokud vaše řešení IoT potřebuje tooknow, pokud je zařízení připojené, měli byste implementovat hello *prezenčního signálu vzor*.

Ve vzoru prezenčního signálu hello hello zařízení odesílá zprávy typu zařízení cloud alespoň jednou každých pevné množství času (například alespoň jednou za hodinu). Proto i v případě, že zařízení nemá žádné toosend dat, ještě odešle zprávu typu zařízení cloud prázdný (obvykle s vlastnost, která ji identifikuje jako prezenční signál). Na straně služby hello hello řešení udržuje mapu s hello poslední prezenční signál pro každé zařízení. Pokud řešení hello neobdrží zprávu prezenčního signálu v rámci hello očekávaný čas z hello zařízení, předpokládá, že došlo k potížím s hello zařízení.

Složitější implementace může obsahovat informace o hello z [operations monitorování] [ lnk-devguide-opmon] tooidentify zařízení, které se pokoušíte tooconnect nebo komunikovat, ale selhání. Pokud implementujete hello prezenčního signálu vzor, ujistěte se, že toocheck [IoT Hub kvóty a omezení][lnk-quotas].

> [!NOTE]
> Pokud připojení k hello používá řešení IoT stavu výhradně toodetermine jestli toosend zprávy typu cloud zařízení, a jsou zprávy není vysílání toolarge sady zařízení, zvažte použití hello jednodušší *krátkodobých čas vypršení platnosti* vzor. Tento vzor dosahuje hello stejný výsledek jako zachování registru stav připojení zařízení pomocí vzoru hello prezenčního signálu, aniž by byly efektivnější. Pokud budete požadovat potvrzení zprávy, IoT Hub vás můžou informovat o zařízení, která jsou možné tooreceive zprávy a které nejsou.

## <a name="device-lifecycle-notifications"></a>Oznámení životního cyklu zařízení

IoT Hub můžete řešení IoT upozornit, když je vytvořené nebo odstraněné zasláním oznámení životního cyklu zařízení identitu zařízení. toodo tedy řešení IoT potřebuje toocreate trasu a tooset hello zdroj dat rovná příliš*DeviceLifecycleEvents*. Ve výchozím nastavení jsou odeslána žádná oznámení životního cyklu, tedy předem neexistuje žádný takový trasy. Hello oznámení obsahuje vlastnosti a text.

Vlastnosti: Vlastnosti zprávu systému mají předponu hello `'$'` symbol.

| Name (Název) | Hodnota |
| --- | --- |
$content – typ | application/json |
$iothub-enqueuedtime |  Čas odeslání oznámení hello |
$iothub – zpráva – zdroj | deviceLifecycleEvents |
$content – kódování | znakové sady UTF-8 |
opType | **createDeviceIdentity** nebo **deleteDeviceIdentity** |
hubName | Název centra IoT |
deviceId | ID zařízení hello |
operationTimestamp | Časové razítko ISO8601 operace |
schéma zprávy iothub | deviceLifecycleNotification |

Text: V této části je ve formátu JSON a představuje hello twin Dobrý den, vytvořili identitu zařízení. Například:

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "etag":"AAAAAAAAAAE=",
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2016-02-30T16:24:48.789Z"
            },
            "$version": 1
        }
    }
}
```

## <a name="reference-topics"></a>Témata odkazů:

Hello následující odkazy na témata poskytují další informace o registru identit hello.

## <a name="device-identity-properties"></a>Vlastnosti identity zařízení

Identit zařízení jsou reprezentovány jako dokumenty JSON s hello následující vlastnosti:

| Vlastnost | Možnosti | Popis |
| --- | --- | --- |
| deviceId |aktualizace požadované, jen pro čtení |Řetězec malá a velká písmena (až too128 znaků) z alfanumerických znaků ASCII 7bitového + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId |vyžaduje jen pro čtení |IoT hub generovaných, malá a velká písmena řetězec až too128 znaků. Tato hodnota je použité toodistinguish zařízení s hello stejné **deviceId**, pokud byla odstraněna a znovu vytvořena. |
| Značka Etag |vyžaduje jen pro čtení |Řetězec představující na slabou značku ETag pro hello identitu zařízení, jako za [RFC7232][lnk-rfc7232]. |
| ověřování |Volitelné |Složené objekt obsahující informace a zabezpečení materiály ověřování. |
| auth.symkey |Volitelné |Objekt složený obsahující primární a sekundární klíč uložený ve formátu base64. |
| status |Požadované |Slouží jako ukazatel přístup. Může být **povoleno** nebo **zakázané**. Pokud **povoleno**, hello zařízení je povolený tooconnect. Pokud **zakázané**, toto zařízení nemá přístup k žádný koncový bod směřujících zařízení. |
| statusReason |Volitelné |128 znaků dlouhý řetězec z tohoto důvodu hello úložiště pro stav identity zařízení hello. Jsou povoleny všechny znaky UTF-8. |
| statusUpdateTime |jen pro čtení |Dočasné ukazatel zobrazující hello datum a čas poslední aktualizace stavu hello. |
| Hodnota connectionState |jen pro čtení |Pole, která určuje stav připojení: buď **připojeno** nebo **odpojeno**. Toto pole představuje hello IoT Hub zobrazení stavu připojení zařízení hello. **Důležité**: Toto pole by měl použít pouze pro účely ladění nebo vývoj. Stav připojení Hello je aktualizovat jenom pro zařízení pomocí MQTT nebo AMQP. Navíc je založena na úrovni protokolu příkazy ping (příkazy ping MQTT nebo AMQP příkazy ping) a může mít maximální zpoždění jenom 5 minut. Z těchto důvodů může být falešně pozitivních zjištění, například zařízení hlášené jako připojené, ale které jsou odpojené. |
| connectionStateUpdatedTime |jen pro čtení |Dočasné indikátor zobrazující poslední stav připojení hello čas a datum hello byl aktualizován. |
| lastActivityTime |jen pro čtení |Dočasné indikátor zobrazující zařízení hello poslední čas a datum hello připojené, přijatých nebo odeslaných zprávu. |

> [!NOTE]
> Stav připojení může představovat pouze hello IoT Hub zobrazení stavu hello hello připojení. Stav aktualizace toothis mohou se proto objevit v závislosti na stavu sítě a konfigurace.

## <a name="additional-reference-material"></a>Odkaz na další materiály

Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:

* [Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.
* [Omezování a kvóty] [ lnk-quotas] popisuje hello kvót a omezování chování, které se vztahují toohello služby IoT Hub.
* [Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.
* [IoT Hub dotazovací jazyk] [ lnk-query] popisuje hello dotazovací jazyk tooretrieve informace ze služby IoT Hub o úlohách a dvojčata zařízení můžete použít.
* [Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.

## <a name="next-steps"></a>Další kroky

Nyní jste se naučili, jak registru identit služby IoT Hub toouse hello, vás může zajímat hello následující témata Průvodce vývojáře IoT Hub:

* [Řízení přístupu tooIoT rozbočovače][lnk-devguide-security]
* [Používání stavu toosynchronize dvojčata zařízení a konfigurací][lnk-devguide-device-twins]
* [Volání metody přímé na zařízení][lnk-devguide-directmethods]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzu IoT Hub:

* [Začínáme s Azure IoT Hub][lnk-getstarted-tutorial]

<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
