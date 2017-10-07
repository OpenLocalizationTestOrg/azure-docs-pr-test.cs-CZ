---
title: "zařízení IoT aaaAzure SDK pro jazyk C - IoTHubClient | Microsoft Docs"
description: "Jak toouse hello IoTHubClient knihovny zařízení Azure IoT hello SDK pro aplikace C toocreate zařízení, které komunikují pomocí služby IoT hub."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: 828cf2bf-999d-4b8a-8a28-c7c901629600
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: d1ece79e9ba6d1e5fd45cabb8fca393b24052e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Pro zařízení Azure IoT SDK pro jazyk C – informace o IoTHubClient
Hello [nejprve článek](iot-hub-device-sdk-c-intro.md) v této řady zavedená hello **zařízení Azure IoT SDK pro jazyk C**. Tento článek vysvětluje, SDK jsou dvě vrstvy architektury. Na základní hello je hello **IoTHubClient** knihovnu, která přímo spravuje komunikace se službou IoT Hub. Je také hello **serializátor** knihovny, který sestaví v horní části daného tooprovide služby serializace. V tomto článku jsme vám poskytují další podrobnosti hello **IoTHubClient** knihovny.

Hello předchozí článek popisuje, jak toouse hello **IoTHubClient** knihovny toosend události tooIoT rozbočovače a přijímat zprávy. Tento článek rozšiřuje této diskuzi o vysvětlením, jak přesně toomore spravovat *při* odesílat a přijímat data, Představujeme vám toohello **nižší úrovně rozhraní API**. Dále vysvětlíme jak tooattach vlastnosti tooevents (a je načtou ze zprávy) pomocí vlastnosti hello zpracování funkcí v hello **IoTHubClient** knihovny. Nakonec poskytujeme další vysvětlení různé způsoby toohandle zprávy přijaté ze služby IoT Hub.

Hello článku se ukončí pokrývajících několik ostatní témata, včetně více informací o zařízení a jak toochange hello chování hello **IoTHubClient** prostřednictvím možnosti konfigurace.

Použijeme hello **IoTHubClient** SDK ukázky tooexplain tato témata. Pokud chcete toofollow společně, najdete v části hello **iothub\_klienta\_ukázka\_http** a **iothub\_klienta\_ukázka\_amqp** aplikace, které jsou součástí zařízení Azure IoT hello SDK pro C. všechno popsané v následující části hello ukazují tyto ukázky.

Můžete najít hello [ **zařízení Azure IoT SDK pro jazyk C** ](https://github.com/Azure/azure-iot-sdk-c) Githubu úložiště a zobrazte podrobnosti o hello rozhraní API v hello [referenční dokumentace rozhraní API jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="hello-lower-level-apis"></a>Hello nižší úrovně rozhraní API
Hello předchozí článek popisuje základní operace hello hello **IotHubClient** v rámci kontextu hello hello **iothub\_klienta\_ukázka\_amqp** aplikace. Například vysvětlení, jak tooinitialize hello knihovny pomocí tohoto kódu.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Také popisuje, jak pomocí této události toosend volání funkce.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Hello článku také popisuje, jak tooreceive zpráv tak, že zaregistrujete funkce zpětného volání.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Hello článku také ukázal, jak toofree prostředků pomocí kódu, například následující hello.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Jsou ale doprovodné funkce tooeach těchto rozhraní API:

* IoTHubClient\_UDOU\_CreateFromConnectionString
* IoTHubClient\_UDOU\_SendEventAsync
* IoTHubClient\_UDOU\_SetMessageCallback
* IoTHubClient\_UDOU\_Destroy

Všechny tyto funkce zahrnout "Vše" v názvu hello rozhraní API. Než tu, která hello parametry každou z těchto funkcí se svými protějšky bez UDOU identické tootheir. Hello chování těchto funkcí je však jinou důležitá jedním způsobem.

Při volání **IoTHubClient\_CreateFromConnectionString**, základní knihovny hello vytvořit nové vlákno, který je spuštěn v pozadí hello. Tento přístup z více vláken odesílá události do a ze služby IoT Hub přijímá zprávy. Žádný takový vlákno se vytvoří při práci s hello "Vše" rozhraní API. Vytvoření Hello vlákna na pozadí hello je vývojář toohello pohodlí. Nemáte tooworry o explicitně události odesílání a přijímání zpráv ze služby IoT Hub – dojde automaticky hello pozadí. Naproti tomu hello "Vše" rozhraní API umožňují explicitní kontrolu nad komunikace se službou IoT Hub, pokud je to nutné.

toounderstand tento lepší Podíváme se na příklad:

Při volání **IoTHubClient\_SendEventAsync**, jaké ve skutečnosti úlohy je uvedení hello událostí do vyrovnávací paměti. Hello vlákna na pozadí vytvořen při volání **IoTHubClient\_CreateFromConnectionString** neustále monitoruje této vyrovnávací paměti a pošle nějaká data datovým obsahuje tooIoT rozbočovače. To se stane, hello pozadí na hello stejný čas této hello hlavního vlákna právě provádí jinou práci.

Podobně když se zaregistrujete funkce zpětného volání pro zprávy pomocí **IoTHubClient\_SetMessageCallback**, že instruující hello SDK toohave hello pozadí vlákno vyvolat hello funkce zpětného volání, když zprávu přijme, nezávisle na hello hlavního vlákna.

Hello "Vše" rozhraní API nevytvářejte vlákna na pozadí. Místo toho nové rozhraní API musí být voláno tooexplicitly odesílat a přijímat data ze služby IoT Hub. Ukazují to následující ukázka hello.

Hello **iothub\_klienta\_ukázka\_http** aplikace, která je součástí hello SDK ukazuje hello nižší úrovně rozhraní API. V této ukázce jsme odesílat události tooIoT rozbočovače s kódem například hello následující:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Hello první tři řádky vytvořte uvítací zprávu, a odešle poslední řádek hello hello událostí. Ale jak je uvedeno nahoře, "odesílání" hello událost znamená, že hello data jednoduše umístí do vyrovnávací paměti. Nic se přenášejí v síti hello při říkáme **IoTHubClient\_UDOU\_SendEventAsync**. V pořadí tooactually příjem příchozích dat hello data tooIoT rozbočovače, je třeba volat **IoTHubClient\_UDOU\_DoWork**, protože v tomto příkladu:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Tento kód (z hello **iothub\_klienta\_ukázka\_http** aplikace) opakovaného volá **IoTHubClient\_UDOU\_DoWork** . Pokaždé, když **IoTHubClient\_UDOU\_DoWork** je volána, odešle některé události z vyrovnávací paměti tooIoT hello rozbočovače a načte zprávu ve frontě odesílaných toohello zařízení. Hello druhém případě znamená, že pokud jsme zaregistrován funkce zpětného volání pro zprávy, pak hello zpětné volání je volána (za předpokladu, že všechny zprávy jsou zařazeny do fronty). By zaregistrovali funkci zpětného volání s kódem například hello následující:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Hello důvodu, že **IoTHubClient\_UDOU\_DoWork** se často říká smyčku je, že pokaždé, když je volána, odešle *některé* uložená do vyrovnávací paměti události tooIoT rozbočovače a načte *hello vedle* zprávy do fronty pro hello zařízení. Každé volání není zaručena toosend uložená do vyrovnávací paměti události nebo zařazených do fronty tooretrieve všechny zprávy. Pokud chcete, aby toosend všechny události v hello vyrovnávací paměti a pokračujte na další zpracování můžete nahradit kódu třeba následující hello Tato smyčka:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Tento kód zavolá **IoTHubClient\_UDOU\_DoWork** dokud všechny události ve vyrovnávací paměti hello byly odeslány tooIoT rozbočovače. Všimněte si, že to také neznamená, že všechny zprávy ve frontě byly přijaty. Součástí hello důvodem je, že kontrola "vše" zpráv není deterministický jako akce. Co se stane, když je načíst "všechny" hello zprávy, ale jinou se pak odešlou toohello zařízení hned po? Lepší způsob toodeal, pomocí které je s naprogramovaných vypršení časového limitu. Funkce zpětného volání zprávy hello mohli například resetovat časovač pokaždé, když je volána. Poté si můžete napsat logiku toocontinue zpracování, pokud například přijaty žádné zprávy byla v hello poslední *X* sekund.

Pokud jste dokončení ingressing události a přijímání zpráv, být jisti toocall hello odpovídající funkce tooclean systémové prostředky.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

V podstatě je jen jednu sadu rozhraní API toosend a přijímat data z vlákna na pozadí a další sadu rozhraní API, která hello samé bez hello pozadí přístup z více vláken. Mnoho vývojáři můžou upřednostňovat hello jiný - UDOU rozhraní API, ale hello nižší úrovně rozhraní API je užitečný v případě hello vývojáře chce explicitní kontrolu nad síťových přenosů. Například některá zařízení shromažďování dat přes čas a události příchozího pouze v určitých intervalech (například jednou za hodinu nebo jednou za den). Hello nižší úrovně rozhraní API udělení hello řízení tooexplicitly možnost při odesílání a příjem dat ze služby IoT Hub. Ostatní bude jednoduše přednost hello jednoduchost této hello nižší úrovně rozhraní API nabízejí. Všechno, co se stane na hello hlavního vlákna než některé pracovní situaci hello pozadí.

Podle toho, která modelu si zvolíte, být konzistentní se toobe které rozhraní API používáte. Pokud spustíte voláním **IoTHubClient\_UDOU\_CreateFromConnectionString**, ujistěte se, budete používat jenom hello odpovídající nižší úrovně rozhraní API pro všechny následné pracovní:

* IoTHubClient\_UDOU\_SendEventAsync
* IoTHubClient\_UDOU\_SetMessageCallback
* IoTHubClient\_UDOU\_Destroy
* IoTHubClient\_UDOU\_DoWork

Hello opačné je také hodnotu true. Pokud začnete se **IoTHubClient\_CreateFromConnectionString**, potom použijte hello jiný - UDOU rozhraní API pro žádné další zpracování.

V hello Azure IoT zařízení SDK pro jazyk C, naleznete v části hello **iothub\_klienta\_ukázka\_http** aplikace pro kompletní příklad, jak hello nižší úrovně rozhraní API. Hello **iothub\_klienta\_ukázka\_amqp** aplikace může být odkazováno kompletní příklad hello jiný - UDOU rozhraní API.

## <a name="property-handling"></a>Vlastnost zpracování
Pokud při Popsali jsme odesílání dat, jsme jste byla odkazující toohello těla zprávy hello. Představte si třeba tento kód:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Tento příklad odešle zprávu tooIoT rozbočovače s hello text "Hello, World". IoT Hub však také umožňuje vlastnosti toobe připojené tooeach zprávy. Vlastnosti jsou páry název/hodnota, které mohou být připojené toohello zprávy. Například můžeme můžete upravit hello předchozí kód tooattach zprávu toohello vlastnost:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Začneme voláním **IoTHubMessage\_vlastnosti** a předání hello popisovač našem zpráv. Se nám získat zpět **MAPY\_zpracování** odkaz, který umožňuje nám toostart přidání vlastností. Hello pozdější provádí volání **mapy\_AddOrUpdate**, což trvá odkaz tooa MAPY\_POPISOVAČ, hello název vlastnosti a hodnotu vlastnosti hello. S tímto rozhraním API přidáme jako mnoho vlastností, jako jsme jako.

Když je hello událostí pro čtení z **Event Hubs**, hello příjemce můžete vytvořit výčet vlastností hello a načíst jejich příslušné hodnoty. Například v rozhraní .NET to by se udělat přístup k hello [vlastnosti kolekce u objektu EventData hello](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

V předchozím příkladu hello jsme se připojuje vlastnosti tooan událostí, odešleme tooIoT rozbočovače. Vlastnosti může být také připojené toomessages přijatých ze služby IoT Hub. Pokud chceme tooretrieve vlastnosti zprávy, můžeme použít kód například následující hello v našem zpráv funkce zpětného volání:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from hello message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Hello volání příliš**IoTHubMessage\_vlastnosti** vrátí hello **MAPY\_zpracování** odkaz. Jsme pak předejte tento odkaz příliš**mapy\_GetInternals** tooobtain odkaz na pole tooan hello název hodnota páry (a také počet hello vlastnosti). V tomto okamžiku je jednoduché vytváření výčtu hello vlastnosti tooget toohello hodnoty, které má být.

Nemáte toouse vlastnosti ve vaší aplikaci. Ale pokud budete potřebovat tooset je na události nebo je načtou ze zprávy, hello **IoTHubClient** knihovny lze snadno.

## <a name="message-handling"></a>Zpracování zpráv
Jak jsme uvedli dříve, při doručování zpráv ze služby IoT Hub hello **IoTHubClient** knihovny reaguje vyvoláním registrovaná funkce zpětného volání. Návratový parametr této funkce, které si zaslouží další vysvětlení není k dispozici. Tady je výňatek hello funkce zpětného volání v hello **iothub\_klienta\_ukázka\_http** ukázkovou aplikaci:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Všimněte si, že je návratový typ hello **IOTHUBMESSAGE\_DISPOZICE\_výsledek** a v tomto případě vrátíme **IOTHUBMESSAGE\_platných**. Existují jiné hodnoty může vrátíme z této funkce jak hello změnu **IoTHubClient** knihovny reaguje toohello zpětné volání zprávy. Zde jsou možnosti hello.

* **IOTHUBMESSAGE\_platných** – hello zpráva byla úspěšně zpracována. Hello **IoTHubClient** knihovny nebude vyvolání funkce zpětného volání hello znovu s hello stejnou zprávu.
* **IOTHUBMESSAGE\_ZAMÍTNUTÝ** – uvítací zprávu nebyla zpracována a neexistuje žádný desire toodo hello takže v budoucí. Hello **IoTHubClient** knihovny nesmí vyvolání funkce zpětného volání hello znovu s hello stejnou zprávu.
* **IOTHUBMESSAGE\_ABANDONED** – uvítací zprávu nebyla úspěšně zpracována, ale hello **IoTHubClient** knihovny by měla vyvolat funkce zpětného volání hello znovu s hello stejnou zprávu.

Pro hello první dva návratové kódy, hello **IoTHubClient** knihovny odešle zprávu tooIoT rozbočovače, která určuje, že uvítací zprávu by se odstraní z fronty hello zařízení a nejsou doručeny znovu. čistý efekt Hello je hello stejné (hello odstraní zprávu z fronty zařízení hello), ale stále zaznamenat zda byla zpráva hello přijetí nebo odmítnutí.  Záznam tento rozdíl je užitečné toosenders hello zprávy, který může sledovat zpětnou vazbu a zjistit, pokud má zařízení přijímat nebo konkrétní zprávu odmítl.

V případě poslední hello je odeslána zpráva taky tooIoT rozbočovače, ale znamená, že by měl vysláním hello zprávy. Obvykle budete abandon zprávu, pokud dojde k nějaké chybě, ale chtít tootry tooprocess hello zprávu znovu. Spočívá v tom, odmítat zprávu odpovídající když dojde k neodstranitelné chybě (nebo jednoduše se rozhodnete, že nechcete, aby tooprocess uvítací zprávu).

V každém případě mějte na paměti hello různých návratových kódů tak, aby dokáže vyvolat hello chování, které chcete z hello **IoTHubClient** knihovny.

## <a name="alternate-device-credentials"></a>Přihlašovací údaje jiného zařízení
Jak je popsáno dříve, hello nejprve thing toodo při práci s hello **IoTHubClient** knihovna je tooobtain **IOTHUB\_klienta\_zpracování** pomocí volání například hello následující:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Hello argumenty příliš**IoTHubClient\_CreateFromConnectionString** jsou hello zařízení připojovací řetězec a parametr, který označuje hello protokol používáme toocommunicate službou IoT Hub. Hello zařízení připojovací řetězec má formát, který se zobrazí takto:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Existují čtyři údaje v tento řetězec: název služby IoT Hub, příponu IoT Hub, ID zařízení a sdílený přístupový klíč. Získat hello plně kvalifikovaný název domény (FQDN) služby IoT hub při vytváření IoT hub instance v hello portál Azure – to vám dává název centra IoT hello (hello první část hello plně kvalifikovaný název domény) a hello IoT hub přípony (hello zbytek hello plně kvalifikovaný název domény). Získat ID zařízení hello a hello sdílený přístupový klíč při registraci zařízení s centrem IoT (jak je popsáno v hello [předchozí článek](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** vám dá tooinitialize hello knihovně. Pokud dáváte přednost, můžete vytvořit nový **IOTHUB\_klienta\_zpracování** pomocí těchto jednotlivé parametry místo hello zařízení připojovací řetězec. Můžete toho dosáhnout s hello následující kód:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

To provede hello samé jako **IoTHubClient\_CreateFromConnectionString**.

To nemusí připadat zřejmé, že chcete by toouse **IoTHubClient\_CreateFromConnectionString** místo Tato podrobnější metoda inicializace. Mějte na paměti, ale, že při registraci zařízení do služby IoT Hub můžete získat je ID zařízení a klíč zařízení (ne připojovací řetězec). Hello *explorer zařízení* nástroj sady SDK byla zavedená v hello [předchozí článek](iot-hub-device-sdk-c-intro.md) používá knihovny v hello **sady SDK služby Azure IoT** toocreate hello zařízení připojovacího řetězce z ID zařízení Hello, klíč zařízení a název hostitele služby IoT Hub. Proto volání **IoTHubClient\_UDOU\_vytvořit** může být vhodnější, protože díky tomu můžete hello krok generování připojovací řetězec. Použijte kteroukoli metodou je vhodné.

## <a name="configuration-options"></a>Možnosti konfigurace
Pokud vše popsané o hello způsob hello **IoTHubClient** knihovny funguje odráží jeho výchozí chování. Existují však několik možností, které se dají nastavit toochange fungování hello knihovně. Toho dosahuje využitím hello **IoTHubClient\_UDOU\_SetOption** rozhraní API. Vezměte v úvahu v tomto příkladu:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Existuje několik možností, které se běžně používají:

* **SetBatching** (logická hodnota) – Pokud **true**, data odeslaná tooIoT centra je odeslaný v dávkách. Pokud **false**, pak jsou zprávy odesílány jednotlivě. Výchozí hodnota Hello je **false**. Všimněte si, že hello **SetBatching** možnost se vztahuje pouze protokol toohello HTTP a není toohello MQTT nebo AMQP protokoly.
* **Časový limit** (bez znaménka int) – Tato hodnota je reprezentována v milisekundách. Pokud odesílání požadavku protokolu HTTP nebo přijímání odpověď trvá déle, než tuto chvíli a pak časový limit připojení hello.

dávkování možnost Hello je důležité. Ve výchozím nastavení, hello knihovně ingresses události jednotlivě (je jedinou událost, ať předáte příliš**IoTHubClient\_UDOU\_SendEventAsync**). Pokud je hello dávkování možnost **true**, hello knihovně shromažďuje tolik události, jak je možné z vyrovnávací paměti hello (až toohello maximální velikost zprávy, bude přijímat IoT Hub).  Hello batch událost je odeslána tooIoT centra v jediném volání protokolu HTTP (hello jednotlivé události jsou seskupeny do pole JSON). Povolení dávkování obvykle za následek zvýšení výkonu velkých vzhledem k tomu, že se snižuje odezvy sítě. Také významně snižuje šířky pásma, protože při odesílání jednu sadu hlaviček protokolu HTTP s batch událostí a nikoli sadu hlavičky pro každé jednotlivé události. Pokud nemáte konkrétní důvod, proč toodo jinak, obvykle budete muset tooenable dávkování.

## <a name="next-steps"></a>Další kroky
Tento článek popisuje podrobnosti hello chování aplikace hello **IoTHubClient** knihovna nalezena v hello **zařízení Azure IoT SDK pro jazyk C**. Pomocí těchto informací byste měli mít dobrou znalost jazyka hello možnosti hello **IoTHubClient** knihovny. Hello [následující článek](iot-hub-device-sdk-c-serializer.md) poskytuje podobné podrobností na hello **serializátor** knihovny.

toolearn Další informace o vývoji pro IoT Hub, najdete v části hello [SDK služby Azure IoT][lnk-sdks].

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
