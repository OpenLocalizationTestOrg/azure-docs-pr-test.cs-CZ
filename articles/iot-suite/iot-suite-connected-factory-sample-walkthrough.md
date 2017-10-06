---
title: "návod řešení Azure IoT Suite factory aaaConnected | Microsoft Docs"
description: "Popis hello řešení Azure IoT předkonfigurované připojen objekt pro vytváření a jeho architektura."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 7fd55c51351659401349cfde91a20fce1045b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Průvodce předkonfigurovaným řešením propojené továrny

Hello IoT Suite připojen objekt pro vytváření [předkonfigurované řešení] [ lnk-preconfigured-solutions] je implementací řešení průmyslových začátku do konce který:

* Připojí tooboth simulated průmyslových zařízení se systémem OPC UA servery v řádky výrobní simulované objekt pro vytváření a skutečné zařízení OPC UA serveru. Další informace o OPC UA najdete v tématu hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).
* Ukazuje klíčové ukazatele výkonu a celkovou efektivitu těchto zařízení a výrobních linek.
* Ukazuje, jak k aplikaci založené na cloudu může být použité toointeract systémech OPC UA server.
* Umožňuje vám tooconnect si vlastní zařízení OPC UA serveru.
* Umožňuje toobrowse a upravit hello OPC UA dat serveru.
* Vlastní zobrazení dat hello z vašich serverů OPC UA se integruje s hello tooprovide služby Azure časové řady přehledy (TSI).

Hello řešení můžete použít jako výchozí bod pro vlastní implementaci a [přizpůsobit] [ lnk-customize] ho toomeet specifických podnikových požadavků.

Tento článek vás provede procesem některé klíčové prvky hello hello připojen objekt pro vytváření řešení tooenable toounderstand jak to funguje. Díky tomu budete moct:

* Řešení problémů v řešení hello.
* Plánování jak toocustomize toohello řešení toomeet vaše konkrétní požadavky.
* Navrhněte vlastní řešení IoT, které používá služby Azure.

Další informace najdete v tématu hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).

## <a name="logical-architecture"></a>Logická architektura

Hello následující diagram popisuje logické součásti hello hello předkonfigurované řešení:

![Logická architektura propojené továrny][connected-factory-logical]

## <a name="communication-patterns"></a>Vzory komunikace

řešení Hello používá hello [OPC UA Pub nebo Sub specifikace](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC UA telemetrická data tooIoT rozbočovače ve formátu JSON. řešení Hello používá hello [OPC vydavatele](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge modul pro tento účel.

řešení Hello má také klientem OPC UA integrována do webové aplikace, které může vytvořit připojení s místními OPC UA servery. Klient Hello používá [reverznímu proxy serveru](https://wikipedia.org/wiki/Reverse_proxy) a přijímá nápovědy ze služby IoT Hub toomake hello připojení bez nutnosti otevřít porty v bráně firewall místní hello. Tento vzor komunikace se nazývá [komunikace s asistencí služby](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/). řešení Hello používá hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge modul pro tento účel.


## <a name="simulation"></a>Simulace

Hello simulované stanic a provádění hello simulated výroby, které systémy (MES) tvoří tovární. Hello simulované zařízení a hello OPC vydavatele modulu jsou založené na hello [OPC UA .NET Standard] [ lnk-OPC-UA-NET-Standard] publikováno hello OPC Foundation.

Hello OPC Proxy a OPC vydavatele jsou implementované jako moduly založené na [Azure IoT Edge][lnk-Azure-IoT-Gateway]. Každá simulovaná výrobní linka má připojenu vyhrazenou bránu.

Všechny simulované komponenty jsou spuštěné v kontejnerech Dockeru hostovaných na virtuálních počítačích Azure s Linuxem. simulace Hello je nakonfigurované toorun osm simulované produkční řádků ve výchozím nastavení.

## <a name="simulated-production-line"></a>Simulovaná výrobní linka

Výrobní linka vyrábí součásti. Skládá se z různých stanic: montážní stanice, testovací stanice a balicí stanice.

simulace Hello spustí a aktualizuje hello dat, který je zveřejněný prostřednictvím hello OPC UA uzlů. Všechny stanice produkční Simulovaná jsou řízená hello MES prostřednictvím OPC UA.

## <a name="simulated-manufacturing-execution-system"></a>Simulovaný systém řízení výroby

Hello MES monitoruje každé stanici v řádku produkční hello prostřednictvím změny stavu stanice toodetect OPC UA. Volá metody toocontrol hello stanice OPC UA a předává produktu z jedné stanici toohello vedle jeho dokončení.

## <a name="gateway-opc-publisher-module"></a>Modul vydavatele brány OPC

OPC vydavatele modul připojí toohello stanice OPC UA servery a přihlásí se k odběru toohello OPC uzly toobe publikovaná. modul Hello převede data uzlu hello do formátu JSON, zašifruje a odešle ji jako zprávy OPC UA Pub nebo Sub tooIoT rozbočovače.

modul OPC vydavatele Hello pouze vyžaduje k portu odchozí https (443) a mohou pracovat s stávající infrastrukturu.

## <a name="gateway-opc-proxy-module"></a>Modul proxy serveru brány OPC

Hello brány OPC UA Proxy modulu tunelových propojení binární OPC UA příkazy a ovládání zpráv a vyžaduje pouze k portu odchozí https (443). Může fungovat s existující podnikovou infrastrukturou, včetně webových proxy serverů.

Používá zařízení IoT Hub metody tootransfer packetized TCP/IP data na aplikační vrstvu hello a tím zajišťuje důvěryhodnosti koncový bod, šifrování dat a integritu pomocí protokolu SSL/TLS.

Hello OPC UA binární protokol přes předávací službu přes proxy server hello samotné používá uživatelský Agent ověřování a šifrování.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Hello brány OPC vydavatele modulu odběratel tooOPC uživatelský Agent serveru uzly toodetect změnu v hodnotě hello datových hodnot. Pokud je zjištěna změna dat v jednom z uzlů hello, tento modul pak odešle zprávy tooAzure IoT Hub.

Služba IoT Hub zajišťuje tooAzure zdroje událostí TSI. TSI ukládá data po dobu 30 dnů podle časová razítka připojených toohello zprávy. Mezi tato data patří:

* Identifikátor OPC UA ApplicationUri
* Identifikátor OPC UA NodeId
* Hodnota uzlu hello
* Časové razítko zdroje
* Název OPC UA DisplayName

V současné době TSI neumožňuje zákazníci toocustomize jak dlouho si přejí tookeep hello data.

Dotazy služby TSI na data uzlů se provádí pomocí SearchSpan (Time.From a Time.To) a agregace podle identifikátoru OPC UA ApplicationUri nebo OPC UA NodeId nebo názvu OPC UA DisplayName.

agregovaná data hello tooretrieve pro měřidla hello OEE a klíčových ukazatelů výkonu a grafy řady hello, data podle počtu událostí, Sum, Avg, Min a Max.

Hello časové řady jsou vytvořeny pomocí jiným procesem. OEE a klíčových ukazatelů výkonu se počítá z stanice základní data a probublala do topologie hello (řádků výroby, objekty Factory, enterprise) v aplikaci hello.

Kromě toho časové řady pro topologii s OEE a klíčového ukazatele výkonu se počítá v aplikaci hello vždy, když je připraven na zobrazené časové rozpětí. Hello den zobrazení je třeba aktualizovat každou hodinu úplné.

Hello časové řady zobrazení data uzlu pochází přímo z TSI pomocí agregace pro časový interval.

## <a name="iot-hub"></a>IoT Hub
Hello [služby IoT hub] [ lnk-IoT Hub] přijímá data odeslaná z hello OPC vydavatele modulu do cloudu hello a je k dispozici toohello Azure TSI služby. 

Hello IoT Hub v řešení hello také:
- Udržuje registr identity, který ukládá hello ID pro všechny OPC vydavatele modulu a všechny moduly OPC Proxy.
- Slouží jako kanál přenosu pro obousměrnou komunikaci hello OPC modul proxy serveru.

## <a name="azure-storage"></a>Azure Storage
řešení Hello používá úložiště objektů blob v Azure jako úložiště disku pro data nasazení virtuálních počítačů a toostore hello.

## <a name="web-app"></a>Webová aplikace
webové aplikace Hello nasadit jako součást hello předkonfigurované řešení se skládá ze klientem integrované OPC UA, výstrahy zpracování a vizualizace telemetrie.

## <a name="next-steps"></a>Další kroky

Budete-li pokračovat, Začínáme se službou IoT Suite načtením hello následující články:

* [Oprávnění na webu azureiotsuite.com hello][lnk-permissions]
* [Nasazení brány v systému Windows nebo Linux hello připojen objekt pro vytváření předkonfigurovaného řešení](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
