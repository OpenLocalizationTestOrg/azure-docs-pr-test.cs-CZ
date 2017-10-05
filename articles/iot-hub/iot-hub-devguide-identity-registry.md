---
title: "Pochopení registru identit Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – Popis registru identit služby IoT Hub a způsobu jeho použití ke správě svých zařízení. Obsahuje informace o import a export identit zařízení hromadně."
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
ms.openlocfilehash: b6e9c7b71fa6fc78f97c0144c735fc44778181d8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="understand-the-identity-registry-in-your-iot-hub"></a>Pochopení registru identit ve službě IoT hub.

Každé centrum IoT má registru identit, která uchovává informace o zařízení lze připojit ke službě IoT hub. Než se zařízení může připojit ke službě IoT hub, musí existovat položka pro toto zařízení v registru identit služby IoT hub. Zařízení musí ověřit také pomocí založené na přihlašovací údaje uložené v registru identit služby IoT hub.

ID zařízení, které jsou uložené v registru identit rozlišuje velká a malá písmena.

Na vysoké úrovni je registru identit podporující REST kolekce prostředků identity zařízení. Když přidáte položku v registru identit, IoT Hub vytvoří sadu prostředků na zařízení, jako jsou fronty, který obsahuje neukládají zprávy typu cloud zařízení.

### <a name="when-to-use"></a>Kdy je použít

Registr identit použijte, když potřebujete:

* Zřizování zařízení, které se připojují ke službě IoT hub.
* Řízení přístupu podle zařízení do koncových bodů rozbočovače na straně zařízení.

> [!NOTE]
> Registr identity neobsahuje žádné metadata specifická pro aplikaci.

## <a name="identity-registry-operations"></a>Operace registru identit

Registr identit služby IoT Hub zpřístupní následující operace:

* Vytvoření identity zařízení
* Aktualizovat identita zařízení.
* Načíst identitu zařízení podle ID
* Odstranit identita zařízení.
* Seznam až 1 000 identit
* Exportovat všechny identity do Azure blob storage
* Import identit z Azure blob storage

Všechny tyto operace můžete použít optimistickou metodu souběžného, jak je uvedeno v [RFC7232][lnk-rfc7232].

> [!IMPORTANT]
> Jediný způsob, jak načíst všechny identity v registru identit služby IoT hub je použití [exportovat] [ lnk-export] funkce.

Registr identit IoT Hub:

* Neobsahuje žádné metadata aplikace.
* Je přístupný jako slovník, pomocí **deviceId** jako klíč.
* Nepodporuje výrazovou dotazů.

Řešení IoT obvykle obsahuje samostatné úložiště specifické pro řešení, který obsahuje metadata specifické pro aplikaci. Konkrétní řešení úložiště v inteligentní sestavování řešení by například záznam místnosti, ve kterém je nasazená teplotní snímač.

> [!IMPORTANT]
> Registr identity lze použijte pouze pro správu zařízení a zajišťování operace. Vysoké propustnosti operace v době běhu nesmí závisí na provádění operací v registru identit. Například kontrola stavu připojení zařízení před odesláním příkazu není podporované vzor. Nezapomeňte zaškrtnout [míru omezení] [ lnk-quotas] pro registr identit a [prezenčního signálu zařízení] [ lnk-guidance-heartbeat] vzor.

## <a name="disable-devices"></a>Zakažte zařízení

Zařízení můžete zakázat aktualizací **stav** vlastnost identity v registru identit. Tato vlastnost se obvykle používá ve dvou scénářích:

* Během procesu zřizování orchestration. Další informace najdete v tématu [zřizování zařízení][lnk-guidance-provisioning].
* Pokud z nějakého důvodu byste zvážit, dojde k ohrožení bezpečnosti nebo se staly neoprávněné zařízení.

## <a name="import-and-export-device-identities"></a>Import a export identit zařízení

Identit zařízení hromadné z registru identit služby IoT hub, můžete exportovat pomocí asynchronních operací [koncový bod zprostředkovatele prostředků služby IoT Hub][lnk-endpoints]. Exportuje jsou dlouho běžící úlohy, které používají kontejner objektů blob zadané zákazníka k uložení dat identity zařízení přečíst z registru identit.

Identit zařízení hromadné můžete importovat do registru identit služby IoT hub, pomocí asynchronních operací [koncový bod zprostředkovatele prostředků služby IoT Hub][lnk-endpoints]. Importy jsou dlouho běžící úlohy, které používají data v kontejneru objektů blob zadané zákazníka k zápisu dat identity zařízení do registru identit.

* Podrobné informace o importu a exportu rozhraní API najdete v tématu [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis].
* Další informace o spouštění import a export úloh najdete v tématu [hromadné správu identit zařízení IoT Hub][lnk-bulk-identity].

## <a name="device-provisioning"></a>Zřizování zařízení

Data zařízení, která ukládá daného řešení IoT, závisí na konkrétní požadavky tohoto řešení. Ale minimálně, musí řešení úložiště identit zařízení a ověřovací klíče. Azure IoT Hub obsahuje registr identity, který může ukládat hodnoty pro každé zařízení, jako je například ID ověřovací klíče a stavové kódy. Řešení můžete použít jinými službami Azure, jako je například úložiště table, úložiště objektů blob nebo Cosmos DB k ukládání dat další zařízení.

*Zřizování zařízení* je proces přidávání počáteční zařízení data do úložiště v řešení. Pokud chcete povolit nové zařízení pro připojení do vašeho centra, musíte přidat ID zařízení a klíče do registru identit služby IoT Hub. Jako součást procesu zřizování může být potřeba inicializovat data specifická pro zařízení v jiné řešení úložiště.

## <a name="device-heartbeat"></a>Prezenční signál zařízení

Registr identit služby IoT Hub obsahuje pole s názvem **connectionState**. Použít pouze **connectionState** pole při vývoji a ladění. Řešení IoT by neměl dotaz na pole v době běhu. Například dotazování **connectionState** pole ke kontrole, pokud je zařízení připojené před odesláním zprávy typu cloud zařízení nebo serveru služby SMS.

Pokud je potřeba vědět, pokud je zařízení připojené, měli byste implementovat řešení IoT *prezenčního signálu vzor*.

Ve vzoru prezenčního signálu zařízení odesílá zprávy typu zařízení cloud alespoň jednou každých pevné množství času (například alespoň jednou za hodinu). Proto i v případě, že zařízení nemá žádná data k odeslání, stále odešle zprávu typu zařízení cloud prázdný (obvykle s vlastnost, která ji identifikuje jako prezenční signál). Na straně služby řešení udržuje mapu s poslední prezenční signál pro každé zařízení. Pokud řešení neobdrží zprávu prezenčního signálu v očekávaném čase ze zařízení, předpokládá, že došlo k potížím se zařízením.

Složitější implementace může obsahovat informace z [operations monitorování] [ lnk-devguide-opmon] k identifikaci zařízení, která se snaží připojit nebo komunikovat, ale selhání. Pokud implementujete vzoru prezenčního signálu, nezapomeňte zaškrtnout [IoT Hub kvóty a omezení][lnk-quotas].

> [!NOTE]
> Pokud řešení IoT používá výhradně k určení, zda se k odesílání zpráv typu cloud zařízení stav připojení a zprávy nejsou všesměrové vysílání pro velké sady zařízení, zvažte použití jednodušší *krátkodobých čas vypršení platnosti* vzor. Tento vzor dosáhne stejného výsledku jako zachování registru stav připojení zařízení pomocí vzoru prezenčního signálu, aniž by byly efektivnější. Pokud budete požadovat potvrzení zprávy, IoT Hub vás může upozornit, které jsou o zařízení, která může přijímat zprávy, které nejsou.

## <a name="device-lifecycle-notifications"></a>Oznámení životního cyklu zařízení

IoT Hub můžete řešení IoT upozornit, když je vytvořené nebo odstraněné zasláním oznámení životního cyklu zařízení identitu zařízení. Uděláte to tak, musí vaše řešení IoT vytvořit trasu a nastavte zdroj dat na hodnotu *DeviceLifecycleEvents*. Ve výchozím nastavení jsou odeslána žádná oznámení životního cyklu, tedy předem neexistuje žádný takový trasy. Oznámení obsahuje vlastnosti a text.

Vlastnosti: Vlastnosti zprávu systému mají předponu `'$'` symbol.

| Name (Název) | Hodnota |
| --- | --- |
$content – typ | application/json |
$iothub-enqueuedtime |  Čas odeslání oznámení. |
$iothub – zpráva – zdroj | deviceLifecycleEvents |
$content – kódování | znakové sady UTF-8 |
opType | **createDeviceIdentity** nebo **deleteDeviceIdentity** |
hubName | Název centra IoT |
deviceId | ID zařízení |
operationTimestamp | Časové razítko ISO8601 operace |
schéma zprávy iothub | deviceLifecycleNotification |

Text: V této části je ve formátu JSON a představuje twin identity vytvořený zařízení. Například:

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

Následující referenční témata poskytují další informace o registru identit.

## <a name="device-identity-properties"></a>Vlastnosti identity zařízení

Identit zařízení jsou reprezentovány jako dokumenty JSON s následujícími vlastnostmi:

| Vlastnost | Možnosti | Popis |
| --- | --- | --- |
| deviceId |aktualizace požadované, jen pro čtení |Řetězec malá a velká písmena (až 128 znaků.) z alfanumerických znaků ASCII 7bitového + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId |vyžaduje jen pro čtení |IoT hub generovaných, malá a velká písmena řetězec až 128 znaků. Tato hodnota se používá k rozlišení zařízení se stejným **deviceId**, pokud byla odstraněna a znovu vytvořena. |
| Značka Etag |vyžaduje jen pro čtení |Řetězec představující na slabou značku ETag pro identitu zařízení dle [RFC7232][lnk-rfc7232]. |
| ověřování |Volitelné |Složené objekt obsahující informace a zabezpečení materiály ověřování. |
| auth.symkey |Volitelné |Objekt složený obsahující primární a sekundární klíč uložený ve formátu base64. |
| status |Požadované |Slouží jako ukazatel přístup. Může být **povoleno** nebo **zakázané**. Pokud **povoleno**, zařízení se může připojit. Pokud **zakázané**, toto zařízení nemá přístup k žádný koncový bod směřujících zařízení. |
| statusReason |Volitelné |128 znaků dlouhý řetězec, který ukládá důvod stavu identity zařízení. Jsou povoleny všechny znaky UTF-8. |
| statusUpdateTime |jen pro čtení |Dočasné ukazatele zobrazuje datum a čas poslední aktualizace stavu. |
| Hodnota connectionState |jen pro čtení |Pole, která určuje stav připojení: buď **připojeno** nebo **odpojeno**. Toto pole představuje IoT Hub pohled na stav připojení zařízení. **Důležité**: Toto pole by měl použít pouze pro účely ladění nebo vývoj. Stav připojení je aktualizovat jenom pro zařízení pomocí MQTT nebo AMQP. Navíc je založena na úrovni protokolu příkazy ping (příkazy ping MQTT nebo AMQP příkazy ping) a může mít maximální zpoždění jenom 5 minut. Z těchto důvodů může být falešně pozitivních zjištění, například zařízení hlášené jako připojené, ale které jsou odpojené. |
| connectionStateUpdatedTime |jen pro čtení |Byl aktualizován na dočasné ukazatel zobrazuje datum a čas posledního stavu připojení. |
| lastActivityTime |jen pro čtení |Dočasné indikátor zobrazuje datum a čas poslední zařízení připojené, přijatých nebo odeslaných zprávu. |

> [!NOTE]
> Stav připojení může představovat pouze IoT Hub zobrazení stavu připojení. Aktualizace tohoto stavu může zpozdit, v závislosti na stavu sítě a konfigurace.

## <a name="additional-reference-material"></a>Odkaz na další materiály

Další témata referenční příručka vývojáře IoT Hub patří:

* [Koncové body centra IoT] [ lnk-endpoints] popisuje různé koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.
* [Omezování a kvóty] [ lnk-quotas] popisuje kvóty a omezení chování, které se vztahují ke službě IoT Hub.
* [Azure IoT zařízení a služby sady SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.
* [IoT Hub dotazovací jazyk] [ lnk-query] popisuje dotazovací jazyk, můžete použít k načtení informací ze služby IoT Hub o úlohách a dvojčata zařízení.
* [Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT.

## <a name="next-steps"></a>Další kroky

Nyní jste se naučili použití registru identit služby IoT Hub, může zajímat v následujících tématech Příručka vývojáře IoT Hub:

* [Řízení přístupu ke službě IoT Hub][lnk-devguide-security]
* [Pomocí dvojčata zařízení synchronizovat stavu a konfigurace][lnk-devguide-device-twins]
* [Volání metody přímé na zařízení][lnk-devguide-directmethods]
* [Plánování úloh na několika zařízeních][lnk-devguide-jobs]

Pokud chcete vyzkoušet některé konceptů popsaných v tomto článku, může zajímat v následujícím kurzu IoT Hub:

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
