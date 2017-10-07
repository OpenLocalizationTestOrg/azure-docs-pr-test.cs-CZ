---
title: "aaaAzure pojmů IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – Glosář běžných termínů souvisejících s tooAzure IoT Hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 16ef29ea-a185-48c3-ba13-329325dc6716
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 217eb082c13e06df5c07543c65d498ad3e395939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="glossary-of-iot-hub-terms"></a>Glosář termínů služby IoT Hub
V tomto článku jsou uvedeny některé hello běžných termínů používaných v hello články IoT Hub.

## <a name="advanced-message-queueing-protocol"></a>Pokročilé Message Queueing Protocol
[Rozšířené zpráv služby Řízení front Protocol (AMQP)](https://www.amqp.org/) je jeden zasílání zpráv hello protokoly, které [IoT Hub](#iot-hub) podporuje pro komunikaci se zařízeními. Další informace o hello protokoly, které podporuje IoT Hub zasílání zpráv najdete v tématu [odesílat a přijímat zprávy službou IoT Hub](iot-hub-devguide-messaging.md).

## <a name="azure-cli"></a>Azure CLI
Hello [rozhraní příkazového řádku Azure](../cli-install-nodejs.md) je nástroj příkaz napříč platformami, open source, založený na prostředí pro vytváření a správu prostředků v Microsoft Azure. Tato verze hello rozhraní příkazového řádku je implementovaná pomocí Node.js.

## <a name="azure-cli-20"></a>Azure CLI 2.0
Hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) je nástroj příkaz napříč platformami, open source, založený na prostředí pro vytváření a správu prostředků v Microsoft Azure. Tato verze preview hello rozhraní příkazového řádku je implementovaná pomocí Python.


## <a name="azure-iot-device-sdks"></a>Azure SDK zařízení IoT
Existují _sady SDK pro zařízení_ k dispozici více jazyků, které umožňují toocreate [aplikací pro zařízení](#device-app) které komunikují s služby IoT hub. Hello IoT Hub kurzy vám ukážou, jak toouse tyto sady SDK zařízení. Hello zdrojového kódu a další informace o zařízení hello sady SDK můžete najít v této Githubu [úložiště](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-edge"></a>Azure IoT Edge
Okraj IoT vám umožní toowrite aplikace, které umožňují toocommunicate brána připojená zařízení s [IoT Hub](#iot-hub). Hello IoT Edge kurzy vám ukážou, jak toouse této služby. Hello zdrojového kódu a další informace o Azure IoT Edge můžete najít v této Githubu [úložiště](https://github.com/Azure/iot-edge).

## <a name="azure-iot-service-sdks"></a>Služby sady SDK služby Azure IoT
Existují _služby sady SDK_ k dispozici více jazyků, které umožňují toocreate [back-end aplikace](#back-end-app) které komunikují s služby IoT hub. Hello IoT Hub kurzy vám ukážou, jak se toouse tyto služby sady SDK. Hello zdrojového kódu a další informace o službě hello sady SDK můžete najít v této Githubu [úložiště](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-portal"></a>portál Azure
Hello [portálu Microsoft Azure](https://portal.azure.com) je centrálním místem, kde můžete zřizovat a správě prostředků Azure. Slouží k uspořádání obsahu pomocí _okna_. V některých kurzy hello Centrum IoT, můžete být vyzváni toouse hello [portál Azure classic](https://manage.windowsazure.com).

## <a name="azure-powershell"></a>Azure PowerShell
[Prostředí Azure PowerShell](/powershell/azure/overview) je kolekce rutiny můžete použít toomanage Azure pomocí prostředí Windows PowerShell. Můžete použít rutiny toocreate hello, testování, nasadit a spravovat řešení a služby se doručují prostřednictvím hello platformy Azure.

## <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) vám umožní toowork s hello prostředky ve vašem řešení jako se skupinou. Můžete nasadit, aktualizovat nebo odstranit hello prostředky pro vaše řešení v rámci jediné koordinované operace.

## <a name="azure-service-bus"></a>Azure Service Bus
[Service Bus](../service-bus/index.md) poskytuje povolenou podporu cloudu komunikace s podnikových způsobů zasílání zpráv a přenosu komunikace, které vám pomůžou připojit místní řešení s cloudem hello. Některé služby IoT Hub kurzy využívat Service Bus [fronty](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="azure-storage"></a>Azure Storage
[Úložiště Azure](../storage/common/storage-introduction.md) je řešení cloudového úložiště. Obsahuje hello úložiště objektů Blob služby, které můžete použít toostore Nestrukturovaná data objektu. Některé služby IoT Hub kurzy používat úložiště objektů blob.

## <a name="back-end-app"></a>Back-end aplikace
V kontextu hello [IoT Hub](#iot-hub), back-end aplikace je aplikace, která se připojuje tooone hello koncových bodů služby orientovaný na služby IoT hub. Například může načíst back-end aplikačním [zařízení cloud](#device-to-cloud)zprávy nebo spravovat hello [registru identit](#identity-registry). Obvykle back-end aplikace běží v cloudu hello, ale v mnoha hello kurzy hello back-end aplikace jsou aplikace konzoly, spuštěna na místním vývojovém počítači.

## <a name="built-in-endpoints"></a>Předdefinované koncové body
Každé centrum IoT obsahuje integrované [koncový bod](iot-hub-devguide-endpoints.md) který je kompatibilní s centrem událostí. Můžete použít jakéhokoli mechanismu, který funguje s Event Hubs tooread zařízení cloud zprávy z tohoto koncového bodu.

## <a name="cloud-gateway"></a>Cloudová brána
Cloudové brány umožňuje připojení pro zařízení, která se nemůže připojit přímo příliš[IoT Hub](#iot-hub). Cloudové brány je hostovaná v cloudu hello v kontrastu tooa [brána pole](#field-gateway) používající tooyour místní zařízení. Typické použití případu pro cloudové brány je tooimplement překlad protokolu pro vaše zařízení.

## <a name="cloud-to-device"></a>Cloud zařízení
Odkazuje toomessages odeslaných z připojených zařízení IoT hub tooa. Tyto zprávy jsou často, příkazy, které dáte pokyn hello zařízení tootake akce. Další informace najdete v tématu [odesílat a přijímat zprávy službou IoT Hub](iot-hub-devguide-messaging.md).

## <a name="connection-string"></a>Připojovací řetězec
V aplikaci kód tooencapsulate hello informace požadované tooconnect tooan koncový bod služby používáte připojovací řetězce. Připojovací řetězec obvykle zahrnuje adresu hello hello koncový bod a informace o zabezpečení, ale připojovací řetězec, který formáty lišit ve službách. Existují dva typy připojovacího řetězce, které jsou přidružené k hello služby IoT Hub:
- *Připojovací řetězce zařízení* povolit zařízení tooconnect toohello zařízení směřujících koncové body služby IoT hub.
- *Připojovací řetězce centra IoT* povolit back-end aplikace tooconnect toohello koncových bodů služby orientovaný na služby IoT hub.

## <a name="custom-endpoints"></a>Vlastní koncové body
Můžete vytvořit vlastní [koncové body](iot-hub-devguide-endpoints.md) na IoT hub toodeliver zpráv odeslaných pomocí [pravidlo směrování](#routing-rules). Vlastní koncové body připojit přímo tooan centra událostí, fronty Service Bus nebo téma sběrnice.

## <a name="custom-gateway"></a>Vlastní brány
Brána umožňuje připojení pro zařízení, která se nemůže připojit přímo příliš[IoT Hub](#iot-hub). Můžete použít [Azure IoT Edge](#azure-iot-edge) toobuild vlastní brány, které implementují vlastní logiky toohandle zprávy, vlastní protokol převody a další zpracování na hello okraj.

## <a name="data-point-message"></a>Zpráva datového bodu
Zpráva datového bodu [zařízení cloud](#device-to-cloud) zprávu, která obsahuje [telemetrie](#telemetry) data, jako jsou rychlosti větru nebo teploty.

## <a name="desired-configuration"></a>Požadované konfigurace
V kontextu hello [dvojče zařízení](iot-hub-devguide-device-twins.md), desired configuration odkazuje toohello úplnou sadu vlastností a metadata v hello dvojče zařízení, která má být synchronizována s hello zařízení.

## <a name="desired-properties"></a>Požadované vlastnosti
V kontextu hello [dvojče zařízení](iot-hub-devguide-device-twins.md), požadovaného vlastnosti je dílčí části hello dvojče zařízení, která se používá s [hlášené vlastnosti](#reported-properties) toosynchronize konfigurace zařízení nebo podmínku. Požadované vlastnosti lze nastavit pouze [back-end aplikačním](#back-end-app) a jsou dodržovat hello [aplikaci zařízení](#device-app).

## <a name="device-to-cloud"></a>Zařízení cloud
Odkazuje toomessages odeslaných z připojeného zařízení příliš[IoT Hub](#iot-hub). Tyto zprávy mohou být [datového bodu](#data-point-message) nebo [interaktivní](#interactive-message) zprávy. Další informace najdete v tématu [odesílat a přijímat zprávy službou IoT Hub](iot-hub-devguide-messaging.md).

## <a name="device"></a>Zařízení
V kontextu hello IOT zařízení je obvykle malého rozsahu, samostatné výpočetní zařízení, které může shromažďovat data nebo jiné zařízení. Zařízení může být například monitorování zařízení s prostředí nebo řadičem takové hello napájení a ventilace systémy v skleníkových. Hello [zařízení katalogu](https://catalog.azureiotsuite.com/) obsahuje seznam hardwaru certifikovaného toowork zařízení s [IoT Hub](#iot-hub).

## <a name="device-app"></a>Aplikace zařízení
Spuštění aplikace zařízení na vaše [zařízení](#device) a zpracovává hello komunikaci s vaší [služby IoT hub](#iot-hub). Obvykle, použijte jednu z hello [sady SDK pro zařízení Azure IoT](#azure-iot-device-sdks) při implementaci aplikace na zařízení. V mnoha hello IoT kurzy používáte [simulované zařízení](#simulated-device) ke zvýšení pohodlí.

## <a name="device-condition"></a>Stav zařízení
Označuje informace o stavu toodevice, jako je například metoda připojení hello právě používá, a to podle [aplikaci zařízení](#device-app). [Aplikace pro zařízení](#device-app) může také nahlásit jejich možnosti. Pro informace o stavu a schopností pomocí dvojčata zařízení se můžete dotazovat.

## <a name="device-data"></a>Data v zařízení
Zařízení data představují data na zařízení toohello uložená v hello IoT Hub [registru identit](#identity-registry). Je možné tooimport a tato data exportovat.

## <a name="device-explorer"></a>Průzkumník zařízení
Hello [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) je nástroj, který běží v systému Windows a umožňuje vám toomanage zařízení v hello [registru identit](#identity-registry).hello nástroj může také odesílat a přijímat zprávy tooyour zařízení.

## <a name="device-identities-rest-api"></a>Rozhraní REST API identit zařízení
Hello [zařízení identity REST API](https://docs.microsoft.com/rest/api/iothub/iothubresource) vám umožní zaregistrovat zařízení v hello toomanage [registru identit](#identity-registry) pomocí rozhraní REST API. Obvykle, měli byste použít jeden z hello vyšší úrovně [služby sady SDK](#azure-iot-service-sdks) jak je znázorněno v hello kurzy IoT Hub.

## <a name="device-identity"></a>Identita zařízení
Hello identitu zařízení je hello jedinečný identifikátor přiřazený tooevery zařízení registrovaná v hello [registru identit](#identity-registry).

## <a name="device-management"></a>Správa zařízení
Správa zařízení zahrnuje celý životní hello spojené se správou zařízení hello ve vašem řešení IoT, včetně plánování, zřizování, konfiguraci, monitorování a vyřazení.

## <a name="device-management-patterns"></a>Schémata správy zařízení
[Centrum IoT](#iot-hub) umožňuje běžné způsoby správy zařízení včetně restartování, provádění obnovení továrního nastavení a provádění aktualizace firmwaru v zařízení.

## <a name="device-messaging-rest-api"></a>Rozhraní REST API zasílání zpráv zařízení
Můžete použít hello [rozhraní API REST pro zasílání zpráv zařízení](https://docs.microsoft.com/rest/api/iothub/httpruntime) ze zařízení toosend zařízení cloud zprávy tooan IoT hub a příjem [cloud zařízení](#cloud-to-device) zpráv ze služby IoT hub. Obvykle, měli byste použít jeden z hello vyšší úrovně [sady SDK pro zařízení](#azure-iot-device-sdks) jak je znázorněno v hello kurzy IoT Hub.

## <a name="device-provisioning"></a>Zřizování zařízení
Zřizování zařízení je hello proces přidávání hello počáteční [data zařízení](#device-data) toohello ukládá v řešení. tooenable nového centra tooyour tooconnect zařízení, je nutné přidat zařízení ID a klíče toohello IoT Hub [registru identit](#identity-registry). Jako součást procesu zřizování hello bude pravděpodobně nutné tooinitialize data specifická pro zařízení v jiné řešení úložiště.

## <a name="device-twin"></a>Dvojče zařízení
A [dvojče zařízení](iot-hub-devguide-device-twins.md) je dokument JSON, který ukládá informace o stavu zařízení například metadata, konfigurace a podmínky. [IoT Hub](#iot-hub) potrvají dvojče zařízení pro každé zařízení, která zřídit ve službě IoT hub. Dvojčata zařízení umožňují toosynchronize [zařízení podmínky](#device-condition) a konfigurací mezi hello zařízení a řešení hello back-end. Zařízení dvojčata toolocate konkrétní zařízení a stav hello dotazu dlouhotrvající operace se můžete dotazovat.

## <a name="device-twin-queries"></a>Dotazy twin zařízení
[Dotazy twin zařízení](iot-hub-devguide-query-language.md) hello SQL jako IoT Hub dotazu jazyka tooretrieve informace z vašeho dvojčata zařízení. Můžete použít stejné služby IoT Hub dotazu jazyka tooretrieve informace o hello [úlohy](#job) spuštěná ve službě IoT hub.

## <a name="device-twin-rest-api"></a>Rozhraní API REST Twin zařízení
Můžete použít hello [zařízení Twin REST API](https://docs.microsoft.com/rest/api/iothub/devicetwinapi) z řešení hello back-end toomanage vaše dvojčata zařízení. Hello rozhraní API umožňuje tooretrieve a aktualizace [dvojče zařízení](#device-twin) vlastnosti a vyvolání [přímé metody](#direct-method). Obvykle, měli byste použít jeden z hello vyšší úrovně [služby sady SDK](#azure-iot-service-sdks) jak je znázorněno v hello kurzy IoT Hub.

## <a name="device-twin-synchronization"></a>Synchronizace zařízení twin
Synchronizace zařízení twin používá hello [potřeby vlastnosti](#desired-properties) ve vaší tooconfigure dvojčata zařízení zařízení a načtení [hlášené vlastnosti](#reported-properties) z vašeho zařízení toostore v dvojče zařízení hello.

## <a name="direct-method"></a>Přímá metoda
A [přímá metoda](iot-hub-devguide-direct-methods.md) je způsob, jak můžete tootrigger metoda tooexecute na zařízení vyvoláním rozhraní API ve službě IoT hub.

## <a name="endpoint"></a>Koncový bod
IoT hub zpřístupní více [koncové body](iot-hub-devguide-endpoints.md) které umožňují aplikace tooconnect toohello IoT hub. Koncové body zařízení přístupem, umožňující zařízení tooperform operace, například odeslání [zařízení cloud](#device-to-cloud) zpráv a příjem [cloud zařízení](#cloud-to-device) zprávy. Koncové body správy orientované na služby, které umožňují [back-end aplikace](#back-end-app) tooperform operací, jako [identitu zařízení](#device-identity) správu a twin správy zařízení. Existují služby směřujících [předdefinované koncové body](#built-in-endpoints) pro čtení zpráv typu zařízení cloud. Můžete vytvořit [vlastní koncové body](#custom-endpoints) zprávy typu zařízení cloud tooreceive odeslaných pomocí [pravidlo směrování](#routing-rules).

## <a name="event-hubs-service"></a>Služba centra událostí
[Služba Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) je služba příchozího vysoce škálovatelné data, která dokáže přijímat miliony událostí za sekundu. Hello služba vám umožní tooprocess a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello. Porovnání s hello služby IoT Hub najdete v tématu [porovnání služeb Azure IoT Hub a Azure Event Hubs](iot-hub-compare-event-hubs.md).

## <a name="event-hub-compatible-endpoint"></a>Koncový bod kompatibilní s centrem událostí
tooread [zařízení cloud](#device-to-cloud) zprávy odeslané tooyour IoT hub, můžete se připojit tooan koncový bod na rozbočovače a používat žádné kompatibilní s centrem událostí metoda tooread tyto zprávy. Metody kompatibilní s centrem událostí využívají hello [SDK centra událostí](../event-hubs/event-hubs-programming-guide.md) a [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md).

## <a name="field-gateway"></a>Brána pole
Brána pole umožňuje připojení pro zařízení, která se nemůže připojit přímo příliš[IoT Hub](#iot-hub) a je obvykle nasazena místně ve vašich zařízeních. Další informace najdete v tématu [co je Azure IoT Hub?](iot-hub-what-is-iot-hub.md)

## <a name="free-account"></a>Bezplatný účet
Můžete vytvořit [bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/) toocomplete hello kurzy IoT Hub a Experimentujte s hello služby IoT Hub (a jinými službami Azure).

## <a name="gateway"></a>brána
Brána umožňuje připojení pro zařízení, která se nemůže připojit přímo příliš[IoT Hub](#iot-hub). Viz také [pole brány](#field-gateway), [cloudové brány](#cloud-gateway), a [vlastní brány](#custom-gateway).

## <a name="identity-registry"></a>Registr identit
Hello [registru identit](iot-hub-devguide-identity-registry.md) je hello integrovanou součástí služby IoT hub, který obsahuje informace o jednotlivých zařízeních hello povolené tooconnect tooan IoT hub.

## <a name="interactive-message"></a>Interaktivní zprávy
Interaktivní zpráva [cloud zařízení](#cloud-to-device) zprávu, která spustí okamžitou akci v back-end hello řešení. Zařízení může například odeslat alarm o selhání, který by měl být automaticky přihlášeni tooa systému CRM.

## <a name="iot-hub"></a>IoT Hub
Back-end řešení IoT Hub je plně spravovaná služba Azure, která umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení. Další informace najdete v tématu [co je Azure IoT Hub?](iot-hub-what-is-iot-hub.md) Pomocí vašeho [předplatného Azure](#subscription), můžete vytvořit IoT hubs toohandle vaše IoT úlohy pro zasílání zpráv.

## <a name="iot-hub-metrics"></a>Metriky služby IoT Hub
[IoT Hub metriky](iot-hub-metrics.md) poskytují údaje o stavu hello centra IoT hello ve vaší [předplatného Azure](#subscription). Povolení služby IoT Hub metriky můžete tooassess hello celkový stav služby hello a hello zařízení připojená tooit. IoT Hub metriky můžete zobrazit, co se děje službou IoT hub a zkoumání příčin problémů bez nutnosti toocontact podporu Azure.

## <a name="iot-hub-query-language"></a>IoT Hub dotazovací jazyk
Hello [IoT Hub dotazovací jazyk](iot-hub-devguide-query-language.md) je podobné jazyku SQL jazyk, který vám umožní tooquery vaše [úlohy](#job) a dvojčata zařízení.

## <a name="iot-hub-resource-provider-rest-api"></a>Zprostředkovatel prostředků služby IoT Hub rozhraní REST API
Můžete použít hello [IoT Hub prostředků zprostředkovatele REST API](https://docs.microsoft.com/rest/api/iothub/resourceprovider/iot-hub-resource-provider-rest) centra IoT hello toomanage ve vaší [předplatného Azure](#subscription) provádění operací, jako je například vytváření, aktualizaci a odstraňování centra.

## <a name="iot-suite"></a>IoT Suite
Azure IoT Suite balíčky současně více služeb Azure s předkonfigurovaných řešení. Tato předkonfigurovaná řešení povolit tooget rychle začít s začátku do konce implementace běžné scénáře IoT. Další informace najdete v tématu [co je Azure IoT Suite?](../iot-suite/iot-suite-overview.md)

## <a name="iothub-explorer"></a>iothub-explorer
Hello [iothub-explorer](https://github.com/azure/iothub-explorer) je nástroj pro různé platformy, příkazového řádku. Hello nástroj umožňuje toomanage je zařízení v hello [registru identit](#identity-registry), odesílání a příjem zpráv a soubory z vašich zařízení a sledovat vaše operace centra IoT.

## <a name="job"></a>Úloha
Můžete použít váš back-end řešení [úlohy](iot-hub-devguide-jobs.md) tooschedule a sledovat aktivity na sadu zařízení zaregistrovali služby IoT hub. Aktivity zahrnují aktualizace dvojče zařízení [potřeby vlastnosti](#desired-properties), aktualizuje dvojče zařízení [značky](#tags)a vyvolání [přímé metody](#direct-method). [IoT Hub](#iot-hub) také používá úlohy příliš[import a tooand export](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) z hello [registru identit](#identity-registry).

## <a name="jobs-rest-api"></a>Úlohy rozhraní REST API
Hello [úloh REST API](https://docs.microsoft.com/rest/api/iothub/jobapi) vám umožní toomanage [úlohy](#job) spuštěná ve službě IoT hub.

## <a name="module"></a>Modul
V [Azure IoT Edge](iot-hub-linux-iot-edge-get-started.md), [modulu](iot-hub-linux-iot-edge-get-started.md) je komponenta, která provádí konkrétní úlohu. Úlohy mohou zahrnovat příjem zpráv ze zařízení, transformace zprávy nebo odeslání zpráva tooan IoT hub. Zprostředkovatel je zodpovědná za předávání zpráv mezi moduly. Azure IoT Edge zahrnuje sadu ukázka modulů. Můžete také vytvořit vlastní moduly.

## <a name="mqtt"></a>MQTT
[MQTT](http://mqtt.org/) je jeden zasílání zpráv hello protokoly, které [IoT Hub](#iot-hub) podporuje pro komunikaci se zařízeními. Další informace o hello protokoly, které podporuje IoT Hub zasílání zpráv najdete v tématu [odesílat a přijímat zprávy službou IoT Hub](iot-hub-devguide-messaging.md).

## <a name="operations-monitoring"></a>Monitorování operací
IoT Hub [operations monitorování](iot-hub-operations-monitoring.md) vám umožní toomonitor hello stav operace ve službě IoT hub v reálném čase. [IoT Hub](#iot-hub) sleduje události napříč několika kategorií operací. Můžete se taky rozhodnout do odesílání událostí z jednoho nebo více kategorií tooan koncový bod centra IoT pro zpracování. Můžete sledovat data hello chyby nebo nastavit složitější zpracování vámi data.

## <a name="physical-device"></a>Fyzické zařízení
Fyzické zařízení je skutečné zařízení, jako je například malin platformy, která se připojuje tooan IoT hub. Pro usnadnění práce řadu hello IoT Hub kurzy použít [simulované zařízení](#simulated-device) tooenable jste toorun ukázky na místním počítači.

## <a name="primary-and-secondary-keys"></a>Primární a sekundární klíče
Když připojíte zařízení přístupem tooa nebo koncový bod služby orientovaný na služby IoT hub, vaše [připojovací řetězec](#connection-string) zahrnuje klíče toogrant přístup. Když přidáte zařízení toohello [registru identit](#identity-registry) nebo přidejte [sdílené zásady přístupu](#shared-access-policy) tooyour rozbočovače, hello service generuje primární a sekundární klíč. Dva klíče můžete tooroll přes z jednoho klíče tooanother při aktualizaci klíče bez ztráty přístupu toohello IoT hub.

## <a name="protocol-gateway"></a>Brána protokolu
Brána protokolu je obvykle nasazený v cloudu hello a poskytuje protokol služby překladu pro zařízení připojující se příliš[IoT Hub](#iot-hub). Další informace najdete v tématu [co je Azure IoT Hub?](iot-hub-what-is-iot-hub.md)

## <a name="quotas-and-throttling"></a>Kvóty a omezování
Existují různé [kvóty](iot-hub-devguide-quotas-throttling.md) které se týkají použití tooyour [IoT Hub](#iot-hub), řadu hello kvóty se liší podle hello úroveň hello IoT hub. [IoT Hub](#iot-hub) platí také [omezí generovaný](iot-hub-devguide-quotas-throttling.md) tooyour použití hello služby za běhu.

## <a name="reported-configuration"></a>Hlášené konfigurace
V kontextu hello [dvojče zařízení](iot-hub-devguide-device-twins.md), hlášené konfigurace odkazuje toohello úplnou sadu vlastností a metadata v hello dvojče zařízení, která má být hlášené back-end toohello řešení.

## <a name="reported-properties"></a>Hlášené vlastnosti
V kontextu hello [dvojče zařízení](iot-hub-devguide-device-twins.md), oznámila vlastnosti je dílčí části hello dvojče zařízení použít s [potřeby vlastnosti](#desired-properties) toosynchronize konfigurace zařízení nebo podmínku. Hlášené vlastnosti lze nastavit pouze hello [aplikaci zařízení](#device-app) a můžete číst a zadají dotaz [back-end aplikačním](#back-end-app).

## <a name="resource-group"></a>Skupina prostředků
[Azure Resource Manager](#azure-resource-manager) toogroup skupiny prostředků používá související prostředky společně. Na hello skupiny můžete použít prostředek skupiny tooperform operací na všechny prostředky hello současně.

## <a name="retry-policy"></a>Zásady opakování
Použít toohandle zásady opakování [přechodné chyby](https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx) když připojíte tooa cloudové služby.

## <a name="routing-rules"></a>Pravidla směrování
Nakonfigurujete [pravidla směrování](iot-hub-devguide-messages-read-custom.md) ve vaší IoT hub tooroute zpráv typu zařízení cloud tooa [vestavěným koncovým bodem](#built-in-endpoints) nebo příliš[vlastní koncové body](#custom-endpoints) pro zpracování v vaše řešení zpět end.

## <a name="sasl-plain"></a>PROSTÝ SASL
PROSTÝ SASL je protokol, který hello [AMQP](#advanced-message-queue-protocol) protokol používá tokeny zabezpečení tootransfer.

## <a name="shared-access-signature"></a>Sdílený přístupový podpis
Podpisy sdíleného přístupu (SAS) se mechanismus ověřování na základě zabezpečeného hodnoty hash SHA-256 nebo identifikátory URI. Ověřování SAS má dvě součásti: _zásada sdíleného přístupu_ a _sdíleného přístupového podpisu_ (často říká token). Zařízení používá SAS tooauthenticate službou IoT hub. [Back-end aplikace](#back-end-app) také použít SAS tooauthenticate s koncovými body služby směřujících hello na služby IoT hub. Obvykle můžete zahrnout tokenu SAS hello hello [připojovací řetězec](#connection-string) , některá aplikace používá tooestablish připojení tooan IoT hub.

## <a name="shared-access-policy"></a>Zásada sdíleného přístupu
Zásada sdíleného přístupu definuje hello oprávnění udělená tooanyone, který má platnou [primární nebo sekundární klíč](#primary-and-secondary-keys) přidružené k této zásadě. Můžete spravovat hello sdílené zásady přístupu a klíče pro vaše centrum v hello [portál](#azure-portal).

## <a name="simulated-device"></a>Simulované zařízení
Pro usnadnění práce řadu hello, které pomocí služby IoT Hub kurzy simulované zařízení tooenable jste toorun ukázky na místním počítači. Naproti tomu [fyzického zařízení](#physical-device) je skutečné zařízení, jako je například malin platformy, která se připojuje tooan IoT hub.

## <a name="solution"></a>Řešení
A _řešení_ najdete tooa řešení sady Visual Studio, která obsahuje jeden nebo více projektů. A _řešení_ může označovat také tooan řešení IoT, který obsahuje prvky, jako jsou například zařízení, [aplikací pro zařízení](#device-app), služby IoT hub, jinými službami Azure a [back-end aplikace](#back-end-app).

## <a name="subscription"></a>Předplatné
Předplatné Azure je, kde probíhá fakturace. Každý prostředek Azure vytvoříte nebo je přidružen k jednomu předplatnému služby Azure, které používáte. Mnoho kvóty vztahovat na úrovni hello předplatného.

## <a name="system-properties"></a>Systémové vlastnosti
V kontextu hello [dvojče zařízení](iot-hub-devguide-device-twins.md), vlastnosti systému jsou jen pro čtení a zahrnují informace o využití zařízení hello například poslední stav aktivity čas a připojení.

## <a name="tags"></a>Značky
V kontextu hello [dvojče zařízení](iot-hub-devguide-device-twins.md), značky jsou zařízení metadata uložená a načíst hello back-end řešení v podobě hello dokumentu JSON. Značky nejsou viditelné tooapps na zařízení.

## <a name="telemetry"></a>Telemetrická data
Shromažďování telemetrických dat, jako je například rychlosti větru nebo teploty, zařízení a používat [data bodu zprávy](#data-point-messages) toosend hello telemetrie tooan IoT hub.

## <a name="token-service"></a>Služba tokenu
Služba tokenu tooimplement mechanismus ověřování můžete použít pro vaše zařízení. Používá služby IoT Hub [sdílené zásady přístupu](#shared-access-policy) s **DeviceConnect** oprávnění toocreate *obor zařízení* tokeny. Tyto tokeny umožňují zařízení tooconnect tooyour IoT hub. Zařízení používá mechanismus tooauthenticate vlastní ověřování pomocí služby tokenů hello. Pokud hello zařízení úspěšně ověří, služba tokenu hello vydá token SAS pro tooaccess toouse hello zařízení služby IoT hub.

## <a name="x509-client-certificate"></a>Klientský certifikát X.509
Zařízení můžete použít tooauthenticate certifikát X.509 s [IoT Hub](#iot-hub). Pomocí certifikátu X.509. certifikát je alternativní toousing [tokenu SAS](#shared-access-signature).
