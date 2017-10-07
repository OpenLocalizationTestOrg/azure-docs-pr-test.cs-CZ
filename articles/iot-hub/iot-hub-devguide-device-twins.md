---
title: "aaaUnderstand dvojčata zařízení Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - použití zařízení dvojčata toosynchronize stavu a konfiguraci dat mezi IoT Hub a zařízení"
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7dade18665108ed352ff3d18e864dc34f451bbf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>Rady pro pochopení a použít dvojčata zařízení IoT hub
## <a name="overview"></a>Přehled
*Dvojčata zařízení* jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky). IoT Hub trvá dvojče zařízení pro každé zařízení připojení tooIoT rozbočovače. Tento článek popisuje:

* Hello struktura dvojče zařízení hello: *značky*, *požadované* a *hlášené vlastnosti*, a
* Hello operace, které aplikace pro zařízení a back-EndY můžete provádět na dvojčata zařízení.

> [!NOTE]
> V současné době jsou přístupné pouze ze zařízení, která se připojují tooIoT rozbočovače dvojčata zařízení pomocí protokolu MQTT hello. Odkazovat toohello [MQTT podporu] [ lnk-devguide-mqtt] článku pokyny, jak tooconvert existující zařízení aplikaci toouse MQTT.
> 
> 

### <a name="when-toouse"></a>Když toouse
Použijte dvojčata zařízení na:

* Metadata specifická pro zařízení ukládat v cloudu hello. Například hello počítače prodejních umístění nasazení.
* Sestavy aktuální informace o stavu, jako jsou k dispozici funkce a podmínky z vaší aplikace zařízení. Například zařízení je připojených tooyour IoT hub přes mobilní nebo Wi-Fi.
* Synchronizujte hello stavu pracovních postupů dlouho běžící mezi aplikace zařízení a back-end aplikace. Například při řešení hello back end určuje hello nové tooinstall verze firmwaru a sestavy aplikace hello zařízení hello různé fáze procesu aktualizace hello.
* Dotaz na vaše zařízení metadata, konfigurace nebo stavu.

Odkazovat příliš[pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] pokyny k používání hlášen vlastnostech, zpráv typu zařízení cloud nebo nahrávání souborů.
Odkazovat příliš[Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] pokyny k použití požadované vlastnosti, přímé metody nebo zprávy typu cloud zařízení.

## <a name="device-twins"></a>Dvojčata zařízení
Dvojčata zařízení ukládat informace týkající se zařízení který:

* Zařízení a zpět končí pomocí toosynchronize zařízení podmínky a konfigurace.
* back-end Hello řešení můžete použít tooquery a cíle dlouhotrvající operace.

životní cyklus Hello dvojče zařízení je propojený odpovídající toohello [identitu zařízení][lnk-identity]. Dvojčata zařízení jsou implicitně vytvořen a odstraněn, když je vytvořené nebo odstraněné v IoT Hub novou identitu zařízení.

Dvojče zařízení je dokument JSON, který zahrnuje:

* **Značky**. Oddíl hello dokument JSON, který hello back-end řešení může číst z a zapisovat. Značky nejsou viditelné toodevice aplikace.
* **Požadovaného vlastnosti**. Použít konfiguraci zařízení hlášené vlastnosti toosynchronize nebo podmínky. Požadované vlastnosti lze nastavit pouze zpět hello řešení end a mohli číst hello zařízení aplikací. aplikace Hello zařízení můžete také upozorněni v reálném čase změn v hello požadovaných vlastností.
* **Hlášené vlastnosti**. Použít konfiguraci zařízení toosynchronize požadované vlastnosti nebo podmínky. Hlášené vlastnosti lze nastavit pouze hello zařízení aplikace a můžete číst a je dotazován pomocí back-end hello řešení.

Kromě toho obsahuje hello kořen dokumentu JSON twin zařízení hello hello jen pro čtení vlastnosti z odpovídající identitu zařízení hello uložené v hello [registru identit][lnk-identity].

![][img-twin]

Hello následující příklad ukazuje dvojče zařízení dokumentu JSON:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

V hello kořenový objekt, jsou hello vlastnosti systému a kontejner objektů pro `tags` a obě `reported` a `desired` vlastnosti. Hello `properties` kontejner obsahuje některé prvky jen pro čtení (`$metadata`, `$etag`, a `$version`) popsané v hello [metadat zařízení twin] [ lnk-twin-metadata] a [ Optimistickou metodu souběžného] [ lnk-concurrency] oddíly.

### <a name="reported-property-example"></a>Příklad hlášené vlastnost
V předchozím příkladu hello dvojče zařízení hello obsahuje `batteryLevel` vlastnosti, který je hlášen aplikace hello zařízení. Tato vlastnost je možné tooquery a provozovat na zařízení podle hello poslední hlášené stav baterie. Další příklady zahrnují hello zařízení aplikace zařízení možnosti vytváření sestav nebo možnosti připojení.

> [!NOTE]
> Hlášené vlastnosti zjednodušit scénáře, kde je back-end hello řešení zajímá hello poslední známá hodnota vlastnosti. Použití [zpráv typu zařízení cloud] [ lnk-d2c] Pokud back-end hello řešení musí tooprocess telemetrie zařízení v podobě hello pořadí označen časovým razítkem události, jako je například časové řady.

### <a name="desired-property-example"></a>Příklad požadovanou vlastnost
V předchozím příkladu hello hello `telemetryConfig` potřeby dvojče zařízení a hlášené vlastnosti jsou používány back-end hello řešení a hello zařízení aplikaci toosynchronize hello telemetrická data konfigurace pro toto zařízení. Například:

1. back-end Hello řešení nastaví vlastnost hello požadovaného s hodnotou hello požadovaných konfigurací. Zde je část hello hello dokumentu s hello požadovaných vlastností nastavenou:
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. Hello zařízení aplikaci je upozornění na změnu hello okamžitě, pokud je připojený, nebo v hello nejprve znovu připojit. Hello aplikaci zařízení hlásí hello aktualizovat konfiguraci (nebo chybový stav pomocí hello `status` vlastnost). Tady je hello část hello hlášené vlastnosti:
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. back-end Hello řešení můžete hello výsledky operace konfigurace hello sledovat prostřednictvím zařízení, pomocí [dotazování] [ lnk-query] dvojčata zařízení.

> [!NOTE]
> Hello předchozí fragmenty kódu jsou příklady, optimalizované pro čitelnost tooencode jedním ze způsobů konfigurace zařízení a jeho stav. IoT Hub nepředstavuje určité schéma pro dvojče zařízení hello potřeby a hlásí vlastnosti ve dvojčata zařízení hello.
> 
> 

Můžete použít dvojčata toosynchronize dlouhotrvající operace jako jsou aktualizace firmwaru. Další informace o způsobu toouse vlastnosti toosynchronize a sledovat dlouhotrvající operace v zařízeních, najdete v části [použití požadovaného vlastnosti tooconfigure zařízení][lnk-twin-properties].

## <a name="back-end-operations"></a>Operace back-end
back-end Hello řešení funguje na dvojče zařízení hello pomocí hello následující atomické operací, k dispozici prostřednictvím protokolu HTTP:

1. **Načíst dvojče zařízení tak, že id**. Tato operace vrátí hello zařízení twin dokumentu, včetně značky a potřeby nahlásí a vlastnosti systému.
2. **Částečné aktualizace dvojče zařízení**. Tato operace umožňuje hello řešení back-end toopartially aktualizace hello značky nebo požadované vlastnosti v dvojče zařízení. částečné aktualizace Hello je vyjádřen jako dokument JSON, který přidá nebo aktualizuje libovolné vlastnosti. Příliš nastaveny vlastnosti`null` se odeberou. Hello následující příklad vytvoří novou požadovanou vlastnost s hodnotou `{"newProperty": "newValue"}`, přepíše existující hodnotu hello `existingProperty` s `"otherNewValue"`a také odebere `otherOldProperty`. Tooexisting potřeby vlastnosti a značky jsou provedeny žádné další změny:
   
        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }
3. **Nahraďte požadované vlastnosti**. Tato operace povoluje hello řešení back-end toocompletely přepsat všechny existující požadované vlastnosti a nahraďte nový dokument JSON pro `properties/desired`.
4. **Nahraďte značky**. Tato operace povoluje hello řešení back-end toocompletely přepsat všechny existující značky a nahraďte nový dokument JSON pro `tags`.
5. **Přijímat oznámení twin**. Tato operace povoluje toobe back-end řešení hello oznámení o změně hello twin. toodo tedy řešení IoT potřebuje toocreate trasu a tooset hello zdroj dat rovná příliš*twinChangeEvents*. Ve výchozím nastavení žádné twin oznámení se odesílají, tedy předem neexistuje žádný takový trasy. Pokud je příliš vysoká rychlost hello změny, nebo z jiných důvodů, jako je například interní chyby, hello IoT Hub může odeslat pouze jedno oznámení, která obsahuje všechny změny. Proto, pokud aplikace potřebuje spolehlivé auditování a protokolování všechny zprostředkující stavy, pak je stále doporučujeme použít D2C zprávy. zpráva oznámení twin Hello zahrnuje vlastnosti a text.

    - Vlastnosti

    | Name (Název) | Hodnota |
    | --- | --- |
    $content – typ | application/json |
    $iothub-enqueuedtime |  Čas odeslání oznámení hello |
    $iothub – zpráva – zdroj | twinChangeEvents |
    $content – kódování | znakové sady UTF-8 |
    deviceId | ID zařízení hello |
    hubName | Název centra IoT |
    operationTimestamp | [ISO8601] časové razítko operace |
    schéma zprávy iothub | deviceLifecycleNotification |
    opType | "replaceTwin" nebo "updateTwin" |

    Vlastnosti zprávu systému mají předponu hello `'$'` symbol.

    - Tělo
        
    Tato část obsahuje všechny změny twin hello ve formátu JSON. Používá stejný formát hello jako opravu, s hello rozdíl to může obsahovat všechny části twin: značky, properties.reported, properties.desired a že obsahuje prvky hello "$metadata". Například:
    ```
    {
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

Všechny hello předchozí operace podporu [optimistickou metodu souběžného] [ lnk-concurrency] a vyžadovat hello **ServiceConnect** oprávnění, jak jsou definovány v hello [zabezpečení ] [ lnk-security] článku.

Kromě toho toothese operace, řešení hello back end může:

* Dotaz hello dvojčata zařízení pomocí hello SQL jako [IoT Hub dotazovací jazyk][lnk-query].
* Provádění operací na velkých sad dvojčata zařízení pomocí [úlohy][lnk-jobs].

## <a name="device-operations"></a>Operace zařízení
aplikace Hello zařízení funguje na dvojče zařízení hello pomocí hello následující atomické operací:

1. **Načtení dvojče zařízení**. Tato operace vrátí hello zařízení twin dokument (včetně značky a potřeby, hlášené a vlastnosti systému) pro hello aktuálně připojené zařízení.
2. **Částečné aktualizace hlášené vlastnosti**. Tato operace povoluje hello částečné aktualizace hello hlášené vlastnosti hello aktuálně připojené zařízení. Tato operace hello používá stejné JSON aktualizovat formátu této hello řešení back end používá pro částečné aktualizace požadované vlastnosti.
3. **Sledovat požadované vlastnosti**. Hello aktuálně připojené zařízení můžete zvolit toobe oznámeno dějí aktualizace toohello požadovaných vlastností. Hello zařízení obdrží hello stejného formuláře aktualizace (náhrada částečně nebo zcela) provedený back-end hello řešení.

Všechny hello předchozí operace vyžadují hello **DeviceConnect** oprávnění, jak jsou definovány v hello [zabezpečení] [ lnk-security] článku.

Hello [sady SDK pro zařízení Azure IoT] [ lnk-sdks] bylo snadné toouse hello předcházející operace z mnoha jazyky a platformy. Další informace o hello podrobnosti primitiv IoT Hub pro požadované vlastnosti synchronizace naleznete v [toku opětovné připojení zařízení][lnk-reconnection].

> [!NOTE]
> V současné době jsou přístupné pouze ze zařízení, která se připojují tooIoT rozbočovače dvojčata zařízení pomocí protokolu MQTT hello.
> 
> 

## <a name="reference-topics"></a>Témata odkazů:
Hello následující odkazy na témata poskytují další informace o řízení přístupu tooyour IoT hub.

## <a name="tags-and-properties-format"></a>Formát značky a vlastnosti
Značky, požadovanou a oznámená vlastnosti jsou objekty JSON s hello následující omezení:

* Všechny klíče v objekty JSON jsou malá a velká písmena 64 bajtů řetězců v kódu UNICODE UTF-8. Povolené znaky vyloučit řídicí znaky UNICODE (segmenty C0 a C1), a `'.'`, `' '`, a `'$'`.
* Všechny hodnoty v objektů JSON může mít následující typy JSON hello: logická hodnota, číslo, řetězec, objekt. Pole nejsou povoleny.
* Všechny objekty JSON ve značkách, požadovanou a oznámená vlastnosti může mít maximální hloubka začlenění na 5. Například hello následující objektu je platná:

        {
            ...
            "tags": {
                "one": {
                    "two": {
                        "three": {
                            "four": {
                                "five": {
                                    "property": "value"
                                }
                            }
                        }
                    }
                }
            },
            ...
        }

* Všechny hodnoty řetězce může být o maximální délce 512 bajtů.

## <a name="device-twin-size"></a>Velikost twin zařízení
IoT Hub vynucuje omezení velikosti 8KB na hodnotách hello `tags`, `properties/desired`, a `properties/reported`, s výjimkou elementy jen pro čtení.
Hello velikost se počítá podle počítání řídit všechny znaky kódování UNICODE s výjimkou znaků (segmenty C0 a C1) a místo `' '` při zdá mimo řetězcová konstanta.
IoT Hub s chybou odmítne všechny operace, které by zvětšete velikost hello těchto dokumentů přesahuje omezení hello.

## <a name="device-twin-metadata"></a>Metadata twin zařízení
IoT Hub uchovává hello časové razítko hello poslední aktualizace pro každý objekt JSON v dvojče zařízení potřeby a který ohlásil vlastnosti. časová razítka Hello se ve standardu UTC a v hello kódování [ISO8601] formátu `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Například:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Tyto informace jsou uchovávány v každé úrovni (ne jenom hello nechá hello struktuře JSON) toopreserve aktualizace, které odebrat objekt klíče.

## <a name="optimistic-concurrency"></a>Optimistickou metodu souběžného zpracování
Značky, potřeby a jsou uvedeny vlastnosti všech optimistickou metodu souběžného podpory.
Značky mají značku ETag dle [RFC7232], představující reprezentace JSON hello značky. Značky etag binárním rozsáhlým můžete použít v operacích podmíněného aktualizace z konzistence tooensure back-end řešení hello.

Dvojče zařízení potřeby a který ohlásil vlastnosti nemají značky etag binárním rozsáhlým, ale mají `$version` hodnotu, která je zaručeno toobe přírůstkové. Stejně tooan značka ETag, hello verzi můžete používat hello aktualizace strany tooenforce konzistence aktualizací. Například zařízení aplikaci pro hlášené vlastnost nebo hello řešení back-end pro požadovanou vlastnost.

Verze jsou užitečné také při observing agenta (například aplikace zařízení hello sledování hello požadovaných vlastností) musí sjednotit RAS mezi hello výsledek operace načtení a oznámení o aktualizaci. Hello části [toku opětovné připojení zařízení] [ lnk-reconnection] poskytuje další informace.

## <a name="device-reconnection-flow"></a>Postup opětovné připojení zařízení
IoT Hub nezachovává oznámení o aktualizacích požadované vlastnosti pro odpojené zařízení. Z toho vyplývá, že zařízení, která se připojuje musí načíst hello úplné požadované vlastnosti dokumentu v přidání toosubscribing pro oznámení o aktualizaci. Zadána možnost hello RAS mezi oznámení o aktualizaci a úplné načtení vhodné zajistit hello následující postup:

1. Aplikace zařízení připojí tooan IoT hub.
2. Aplikace zařízení pro požadované vlastnosti Odběratel oznámení o aktualizaci.
3. Aplikace zařízení načte hello celého dokumentu pro požadované vlastnosti.

aplikace Hello zařízení můžete ignorovat všechna oznámení s `$version` menší nebo roven hello verzi celého dokumentu načtené hello. Tento přístup je možné, protože IoT Hub zaručuje, že verze vždy zvýšit.

> [!NOTE]
> Tato logika je už implementované v hello [sady SDK pro zařízení Azure IoT][lnk-sdks]. Tento popis je užitečný jenom v případě, že aplikace hello zařízení nelze použít sady SDK pro zařízení Azure IoT a musí programu hello MQTT rozhraní přímo.
> 
> 

## <a name="additional-reference-material"></a>Odkaz na další materiály
Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:

* Hello [koncové body centra IoT] [ lnk-endpoints] článek popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.
* Hello [omezování a kvóty] [ lnk-quotas] článek popisuje hello kvóty, které se vztahují toohello služby IoT Hub a hello omezení tooexpect chování při použití služby hello.
* Hello [sady SDK zařízení a služby Azure IoT] [ lnk-sdks] článku seznamy hello různých sadách SDK jazyka, které můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.
* Hello [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] článek popisuje hello můžete tooretrieve informací ze služby IoT Hub o úlohách a dvojčata zařízení IoT Hub dotazovací jazyk .
* Hello [IoT Hub MQTT podporu] [ lnk-devguide-mqtt] článek obsahuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.

## <a name="next-steps"></a>Další kroky
Nyní jste se naučili o dvojčata zařízení, může být zájem o hello následující témata Průvodce vývojáře IoT Hub:

* [Volání metody přímé na zařízení][lnk-methods]
* [Plánování úloh na několika zařízeních][lnk-jobs]

Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzy IoT Hub:

* [Jak toouse hello dvojče zařízení][lnk-twin-tutorial]
* [Jak dvojče zařízení toouse vlastnosti][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png
