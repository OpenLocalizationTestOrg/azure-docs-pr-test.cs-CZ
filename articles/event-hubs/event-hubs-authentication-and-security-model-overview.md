---
title: "aaaOverview modelu zabezpečení a ověřování Azure Event Hubs | Microsoft Docs"
description: "Ověřování a zabezpečení modelu přehled služby Event Hubs."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 93841e30-0c5c-4719-9dc1-57a4814342e7
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: sethm;clemensv
ms.openlocfilehash: e57ccda33e5ee20e635487cf91d9e8af594d3bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-authentication-and-security-model-overview"></a>Ověřování a zabezpečení modelu přehled služby Event Hubs
model zabezpečení služby Azure Event Hubs Hello splňuje hello následující požadavky:

* Centra událostí tooan dat může odesílat pouze klienty, která představují platné přihlašovací údaje.
* Klient nemůže zosobnit jiného klienta.
* Podvodného klienta můžete blokovat odesílání centra událostí tooan data.

## <a name="client-authentication"></a>Ověření klienta
model zabezpečení služby Event Hubs Hello je založen na kombinaci [sdíleného přístupového podpisu (SAS)](../service-bus-messaging/service-bus-sas.md) tokeny a *zdroje událostí*. Vydavatel události definuje virtuální koncový bod pro centra událostí. Vydavatel Hello může být pouze centra událostí tooan použité toosend zprávy. Není možné tooreceive zprávy od vydavatele.

Centra událostí obvykle aktivuje jeden vydavatele za klienta. Všechny zprávy, které se odesílají tooany hello vydavatele centra událostí jsou zařazených do fronty v rámci tohoto centra událostí. Vydavatelé povolte řízení podrobných přístupu a omezení.

Každý klient služby Event Hubs je přiřazen jedinečný token, který je nahraný toohello klienta. Hello tokenů vytváří tak, aby každý jedinečný token uděluje přístup tooa jiný jedinečný vydavatele. Klient, který má token lze odeslat pouze tooone vydavatele, ale žádný další vydavatel. Pokud sdílená složka více klientů hello stejný token a každý z nich sdílí vydavatel.

I když není doporučeno, je možné tooequip zařízení s tokeny, které udělit centra událostí tooan přímý přístup. Zprávy můžete přímo do tohoto centra událostí posílat jakékoli zařízení, která obsahuje tento token. Taková zařízení nebude toothrottling subjektu. Kromě toho hello zařízení nemůže být zakázáno odesílání centra událostí toothat z.

Všechny tokeny jsou podepsané klíče SAS. Obvykle jsou všechny tokeny hello podepsané stejným klíčem. Klienti nemají informace o hello klíč; To zabrání tomu, aby ostatní klienty výrobní tokeny.

### <a name="create-hello-sas-key"></a>Vytvořte klíč SAS hello

Při vytváření na obor názvů služby Event Hubs, hello služby vygeneruje SAS klíč 256 bitů s názvem **RootManageSharedAccessKey**. Tento klíč uděluje odeslat, naslouchat a spravovat obor názvů toohello práva. Můžete také vytvořit další klíče. Doporučuje se, že můžete vytvořit klíče, odeslat uděluje oprávnění toohello konkrétní události rozbočovače. Pro hello zbývající část tohoto tématu, předpokládá se, tento klíč s názvem **EventHubSendKey**.

Hello následující ukázka vytvoří jen odesílání klíč při vytváření centra událostí hello:

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create event hub with a SAS rule that enables sending toothat event hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Generování tokenů

Můžete vygenerovat tokeny pomocí klíče SAS hello. Musíte vytvořit jenom jeden token za klienta. Tokeny je pak možné vytvořit pomocí hello následující metodu. Všechny tokeny jsou generovány pomocí hello **EventHubSendKey** klíč. Každý token je přiřazen jedinečný identifikátor URI.

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Při volání této metody, hello identifikátor URI musí být zadány ve tvaru `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Pro všechny tokeny, je stejné, s výjimkou hello hello URI `PUBLISHER_NAME`, což by mělo být různé pro každý token. V ideálním případě `PUBLISHER_NAME` představuje hello ID hello klienta, která přijímá token.

Tato metoda generuje token s hello strukturu:

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

čas vypršení platnosti tokenu Hello je zadána v sekundách od 1. ledna 1970. Hello následuje příklad token:

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Obvykle hello tokeny mají životnost, která se podobá nebo překračuje hello životnost klienta hello. Pokud má klient hello hello schopností tooobtain nový token, lze použít tokeny s kratší životnost.

### <a name="sending-data"></a>Odeslání dat
Po vytvoření hello tokeny jednotlivých klientů je opatřen svůj vlastní jedinečný token.

Když klient hello odešle data do centra událostí, značky odeslání požadavku s tokenem hello. tooprevent útočník z odposlouchávání a krádež hello token, hello komunikaci mezi klientem hello a hello centra událostí, musí dojít přes šifrovaný kanál.

### <a name="blacklisting-clients"></a>Blokované klienty
V případě krádeže token uživatelem se zlými úmysly může útočník hello zosobnit klienta hello odcizen jejichž token. Seznamů zakázaných klienta vykreslí tohoto klienta nepoužitelný, dokud neobdrží nový token, který používá jiný vydavatel.

## <a name="authentication-of-back-end-applications"></a>Ověřování back-end aplikace

tooauthenticate back-end aplikace, které využívají data hello generované klientů služby Event Hubs, Event Hubs využívá model zabezpečení, který je podobný toohello model, který se používá pro témata Service Bus. Skupiny uživatelů služby Event Hubs je ekvivalentní tooa předplatné tooa Service Bus tématu. Klienta můžete vytvořit skupiny příjemců, pokud se skupina uživatelů hello požadavek toocreate hello spolu token spravovat uděluje oprávnění pro hello centra událostí, nebo pro obor názvů toowhich hello centra událostí hello patří. Klient je povolen, tooconsume data ze skupiny příjemců Pokud hello přijmout žádost o je doplněny tokenem, přijímat uděluje oprávnění v této skupiny příjemců, hello centra událostí, nebo Centrum událostí hello obor názvů toowhich hello patří.

Hello aktuální verzi Service Bus nepodporuje SAS pravidla pro jednotlivé odběry. Dobrý den, které to samé platí pro skupiny uživatelů služby Event Hubs. Podpora SAS přidá pro obě funkce v budoucnu hello.

Hello existovat ověřování SAS pro jednotlivé příjemce skupiny můžete toosecure klíče SAS všechny skupiny příjemců běžné klíčem. Tento přístup umožňuje tooconsume data aplikací ze všech skupin uživatelů hello centra událostí.

## <a name="next-steps"></a>Další kroky
toolearn víc o službě Event Hubs naleznete hello následující témata:

* [Přehled služby Event Hubs]
* [Přehled sdílených přístupových podpisů]
* [Ukázkové aplikace, které používají službu Event Hubs]

[Přehled služby Event Hubs]: event-hubs-what-is-event-hubs.md
[Ukázkové aplikace, které používají službu Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Přehled sdílených přístupových podpisů]: ../service-bus-messaging/service-bus-sas.md

