---
title: "Přehled modelu zabezpečení a ověřování Azure Event Hubs | Microsoft Docs"
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
ms.openlocfilehash: 5abdbf70d4fdb2c7feb0f3537ecc0f2abf0775a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="event-hubs-authentication-and-security-model-overview"></a><span data-ttu-id="c9a90-103">Ověřování a zabezpečení modelu přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c9a90-103">Event Hubs authentication and security model overview</span></span>
<span data-ttu-id="c9a90-104">Model zabezpečení služby Azure Event Hubs splňuje následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="c9a90-104">The Azure Event Hubs security model meets the following requirements:</span></span>

* <span data-ttu-id="c9a90-105">Pouze klienti, kteří platné přihlašovací údaje k dispozici může odesílat data do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-105">Only clients that present valid credentials can send data to an event hub.</span></span>
* <span data-ttu-id="c9a90-106">Klient nemůže zosobnit jiného klienta.</span><span class="sxs-lookup"><span data-stu-id="c9a90-106">A client cannot impersonate another client.</span></span>
* <span data-ttu-id="c9a90-107">Podvodného klienta můžete blokovat odesílání dat do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-107">A rogue client can be blocked from sending data to an event hub.</span></span>

## <a name="client-authentication"></a><span data-ttu-id="c9a90-108">Ověření klienta</span><span class="sxs-lookup"><span data-stu-id="c9a90-108">Client authentication</span></span>
<span data-ttu-id="c9a90-109">Model zabezpečení služby Event Hubs je založen na kombinaci [sdíleného přístupového podpisu (SAS)](../service-bus-messaging/service-bus-sas.md) tokeny a *zdroje událostí*.</span><span class="sxs-lookup"><span data-stu-id="c9a90-109">The Event Hubs security model is based on a combination of [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) tokens and *event publishers*.</span></span> <span data-ttu-id="c9a90-110">Vydavatel události definuje virtuální koncový bod pro centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-110">An event publisher defines a virtual endpoint for an event hub.</span></span> <span data-ttu-id="c9a90-111">Vydavatel slouží jenom k odesílání zpráv do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-111">The publisher can only be used to send messages to an event hub.</span></span> <span data-ttu-id="c9a90-112">Není možné přijímat zprávy od vydavatele.</span><span class="sxs-lookup"><span data-stu-id="c9a90-112">It is not possible to receive messages from a publisher.</span></span>

<span data-ttu-id="c9a90-113">Centra událostí obvykle aktivuje jeden vydavatele za klienta.</span><span class="sxs-lookup"><span data-stu-id="c9a90-113">Typically, an event hub employs one publisher per client.</span></span> <span data-ttu-id="c9a90-114">Všechny zprávy, které se odesílají do jakéhokoli z daných vydavatelů centra událostí jsou zařazených do fronty v rámci tohoto centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-114">All messages that are sent to any of the publishers of an event hub are enqueued within that event hub.</span></span> <span data-ttu-id="c9a90-115">Vydavatelé povolte řízení podrobných přístupu a omezení.</span><span class="sxs-lookup"><span data-stu-id="c9a90-115">Publishers enable fine-grained access control and throttling.</span></span>

<span data-ttu-id="c9a90-116">Každý klient služby Event Hubs je přiřazen jedinečný token, který je nahrán do klienta.</span><span class="sxs-lookup"><span data-stu-id="c9a90-116">Each Event Hubs client is assigned a unique token, which is uploaded to the client.</span></span> <span data-ttu-id="c9a90-117">Tokeny vznikají tak, aby každý jedinečný token uděluje přístup na jiný jedinečný vydavatele.</span><span class="sxs-lookup"><span data-stu-id="c9a90-117">The tokens are produced such that each unique token grants access to a different unique publisher.</span></span> <span data-ttu-id="c9a90-118">Klient, který má token lze odeslat pouze jeden vydavatele, ale žádný další vydavatel.</span><span class="sxs-lookup"><span data-stu-id="c9a90-118">A client that possesses a token can only send to one publisher, but no other publisher.</span></span> <span data-ttu-id="c9a90-119">Pokud více klientů sdílí stejný token, každý z nich sdílí vydavatel.</span><span class="sxs-lookup"><span data-stu-id="c9a90-119">If multiple clients share the same token, then each of them shares a publisher.</span></span>

<span data-ttu-id="c9a90-120">I když není doporučeno, je možné vybavit zařízení s tokeny, které udělit přímý přístup do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-120">Although not recommended, it is possible to equip devices with tokens that grant direct access to an event hub.</span></span> <span data-ttu-id="c9a90-121">Zprávy můžete přímo do tohoto centra událostí posílat jakékoli zařízení, která obsahuje tento token.</span><span class="sxs-lookup"><span data-stu-id="c9a90-121">Any device that holds this token can send messages directly into that event hub.</span></span> <span data-ttu-id="c9a90-122">Tato zařízení nejsou předmětem omezení.</span><span class="sxs-lookup"><span data-stu-id="c9a90-122">Such a device will not be subject to throttling.</span></span> <span data-ttu-id="c9a90-123">Kromě toho zařízení nemůže být zakázáno v odesílání do tohoto centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-123">Furthermore, the device cannot be blacklisted from sending to that event hub.</span></span>

<span data-ttu-id="c9a90-124">Všechny tokeny jsou podepsané klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="c9a90-124">All tokens are signed with a SAS key.</span></span> <span data-ttu-id="c9a90-125">Obvykle jsou všechny tokeny podepsané stejným klíčem.</span><span class="sxs-lookup"><span data-stu-id="c9a90-125">Typically, all tokens are signed with the same key.</span></span> <span data-ttu-id="c9a90-126">Klienti nemají informace o klíči; To zabrání tomu, aby ostatní klienty výrobní tokeny.</span><span class="sxs-lookup"><span data-stu-id="c9a90-126">Clients are not aware of the key; this prevents other clients from manufacturing tokens.</span></span>

### <a name="create-the-sas-key"></a><span data-ttu-id="c9a90-127">Vytvoření klíče SAS</span><span class="sxs-lookup"><span data-stu-id="c9a90-127">Create the SAS key</span></span>

<span data-ttu-id="c9a90-128">Při vytváření na obor názvů služby Event Hubs, službu vygeneruje SAS klíč 256 bitů s názvem **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="c9a90-128">When creating an Event Hubs namespace, the service generates a 256-bit SAS key named **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="c9a90-129">Tento klíč uděluje odeslat, naslouchat a spravovat práva k oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c9a90-129">This key grants send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="c9a90-130">Můžete také vytvořit další klíče.</span><span class="sxs-lookup"><span data-stu-id="c9a90-130">You can also create additional keys.</span></span> <span data-ttu-id="c9a90-131">Doporučuje se, že můžete vytvořit klíče, odeslat uděluje oprávnění k centru konkrétní události.</span><span class="sxs-lookup"><span data-stu-id="c9a90-131">It is recommended that you produce a key that grants send permissions to the specific event hub.</span></span> <span data-ttu-id="c9a90-132">Pro zbývající část tohoto tématu, předpokládá se, tento klíč s názvem **EventHubSendKey**.</span><span class="sxs-lookup"><span data-stu-id="c9a90-132">For the remainder of this topic, it is assumed that you named this key **EventHubSendKey**.</span></span>

<span data-ttu-id="c9a90-133">Následující příklad vytvoří jen odesílání klíč při vytváření centra událostí:</span><span class="sxs-lookup"><span data-stu-id="c9a90-133">The following example creates a send-only key when creating the event hub:</span></span>

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create event hub with a SAS rule that enables sending to that event hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a><span data-ttu-id="c9a90-134">Generování tokenů</span><span class="sxs-lookup"><span data-stu-id="c9a90-134">Generate tokens</span></span>

<span data-ttu-id="c9a90-135">Můžete vygenerovat tokeny pomocí klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="c9a90-135">You can generate tokens using the SAS key.</span></span> <span data-ttu-id="c9a90-136">Musíte vytvořit jenom jeden token za klienta.</span><span class="sxs-lookup"><span data-stu-id="c9a90-136">You must produce only one token per client.</span></span> <span data-ttu-id="c9a90-137">Tokeny je pak možné vytvořit pomocí následující metodu.</span><span class="sxs-lookup"><span data-stu-id="c9a90-137">Tokens can then be produced using the following method.</span></span> <span data-ttu-id="c9a90-138">Všechny tokeny jsou generovány pomocí **EventHubSendKey** klíč.</span><span class="sxs-lookup"><span data-stu-id="c9a90-138">All tokens are generated using the **EventHubSendKey** key.</span></span> <span data-ttu-id="c9a90-139">Každý token je přiřazen jedinečný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="c9a90-139">Each token is assigned a unique URI.</span></span>

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

<span data-ttu-id="c9a90-140">Při volání této metody, identifikátor URI musí být zadány ve tvaru `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="c9a90-140">When calling this method, the URI should be specified as `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span></span> <span data-ttu-id="c9a90-141">Pro všechny tokeny, identifikátor URI je identických, s výjimkou produktů `PUBLISHER_NAME`, což by mělo být různé pro každý token.</span><span class="sxs-lookup"><span data-stu-id="c9a90-141">For all tokens, the URI is identical, with the exception of `PUBLISHER_NAME`, which should be different for each token.</span></span> <span data-ttu-id="c9a90-142">V ideálním případě `PUBLISHER_NAME` reprezentuje ID klienta, který přijme tento token.</span><span class="sxs-lookup"><span data-stu-id="c9a90-142">Ideally, `PUBLISHER_NAME` represents the ID of the client that receives that token.</span></span>

<span data-ttu-id="c9a90-143">Tato metoda generuje token s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="c9a90-143">This method generates a token with the following structure:</span></span>

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

<span data-ttu-id="c9a90-144">V sekundách od 1. ledna 1970 je zadaný čas vypršení platnosti tokenu.</span><span class="sxs-lookup"><span data-stu-id="c9a90-144">The token expiration time is specified in seconds from Jan 1, 1970.</span></span> <span data-ttu-id="c9a90-145">Následuje příklad token:</span><span class="sxs-lookup"><span data-stu-id="c9a90-145">The following is an example of a token:</span></span>

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

<span data-ttu-id="c9a90-146">Obvykle tokeny mají životnost, která se podobá nebo překračuje životnost klienta.</span><span class="sxs-lookup"><span data-stu-id="c9a90-146">Typically, the tokens have a lifespan that resembles or exceeds the lifespan of the client.</span></span> <span data-ttu-id="c9a90-147">Pokud má klient schopnost získat nový token, můžete použít tokeny s kratší životnost.</span><span class="sxs-lookup"><span data-stu-id="c9a90-147">If the client has the capability to obtain a new token, tokens with a shorter lifespan can be used.</span></span>

### <a name="sending-data"></a><span data-ttu-id="c9a90-148">Odeslání dat</span><span class="sxs-lookup"><span data-stu-id="c9a90-148">Sending data</span></span>
<span data-ttu-id="c9a90-149">Po vytvoření tokenů jednotlivých klientů je opatřen svůj vlastní jedinečný token.</span><span class="sxs-lookup"><span data-stu-id="c9a90-149">Once the tokens have been created, each client is provisioned with its own unique token.</span></span>

<span data-ttu-id="c9a90-150">Když klient odešle data do centra událostí, značky jeho odeslán požadavek pomocí tokenu.</span><span class="sxs-lookup"><span data-stu-id="c9a90-150">When the client sends data into an event hub, it tags its send request with the token.</span></span> <span data-ttu-id="c9a90-151">Aby zabránil útočníkovi z odposlouchávání a krádež token, musí dojít k komunikace mezi klientem a centra událostí přes šifrovaný kanál.</span><span class="sxs-lookup"><span data-stu-id="c9a90-151">To prevent an attacker from eavesdropping and stealing the token, the communication between the client and the event hub must occur over an encrypted channel.</span></span>

### <a name="blacklisting-clients"></a><span data-ttu-id="c9a90-152">Blokované klienty</span><span class="sxs-lookup"><span data-stu-id="c9a90-152">Blacklisting clients</span></span>
<span data-ttu-id="c9a90-153">V případě krádeže token uživatelem se zlými úmysly může útočník zosobnit klienta odcizen jejichž token.</span><span class="sxs-lookup"><span data-stu-id="c9a90-153">If a token is stolen by an attacker, the attacker can impersonate the client whose token has been stolen.</span></span> <span data-ttu-id="c9a90-154">Seznamů zakázaných klienta vykreslí tohoto klienta nepoužitelný, dokud neobdrží nový token, který používá jiný vydavatel.</span><span class="sxs-lookup"><span data-stu-id="c9a90-154">Blacklisting a client renders that client unusable until it receives a new token that uses a different publisher.</span></span>

## <a name="authentication-of-back-end-applications"></a><span data-ttu-id="c9a90-155">Ověřování back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="c9a90-155">Authentication of back-end applications</span></span>

<span data-ttu-id="c9a90-156">K ověření back-end aplikace, které využívají data generována klientů služby Event Hubs, Event Hubs využívá model zabezpečení, který je podobný modelu, který se používá pro témata Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c9a90-156">To authenticate back-end applications that consume the data generated by Event Hubs clients, Event Hubs employs a security model that is similar to the model that is used for Service Bus topics.</span></span> <span data-ttu-id="c9a90-157">Skupiny uživatelů služby Event Hubs je ekvivalentní k odběru do tématu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c9a90-157">An Event Hubs consumer group is equivalent to a subscription to a Service Bus topic.</span></span> <span data-ttu-id="c9a90-158">Klienta můžete vytvořit skupiny příjemců, pokud požadavek na vytvoření skupiny uživatelů je doplněny tokenem, spravujete uděluje oprávnění pro centra událostí, nebo pro obor názvů, do které patří centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-158">A client can create a consumer group if the request to create the consumer group is accompanied by a token that grants manage privileges for the event hub, or for the namespace to which the event hub belongs.</span></span> <span data-ttu-id="c9a90-159">Klient může využívat data ze skupiny příjemců, pokud je požadavek na přijetí spolu s token, který uděluje práva receive na této skupiny příjemců, centra událostí nebo oboru názvů, do které patří centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-159">A client is allowed to consume data from a consumer group if the receive request is accompanied by a token that grants receive rights on that consumer group, the event hub, or the namespace to which the event hub belongs.</span></span>

<span data-ttu-id="c9a90-160">Aktuální verze služby Service Bus nepodporuje SAS pravidla pro jednotlivé odběry.</span><span class="sxs-lookup"><span data-stu-id="c9a90-160">The current version of Service Bus does not support SAS rules for individual subscriptions.</span></span> <span data-ttu-id="c9a90-161">To samé platí pro skupiny uživatelů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="c9a90-161">The same holds true for Event Hubs consumer groups.</span></span> <span data-ttu-id="c9a90-162">Podpora SAS se přidá pro obě funkce v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="c9a90-162">SAS support will be added for both features in the future.</span></span>

<span data-ttu-id="c9a90-163">Chybí ověřování SAS pro jednotlivé příjemce skupiny můžete zabezpečit všechny skupiny příjemců běžné klíčem klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="c9a90-163">In the absence of SAS authentication for individual consumer groups, you can use SAS keys to secure all consumer groups with a common key.</span></span> <span data-ttu-id="c9a90-164">Tento přístup umožňuje aplikaci využívat data ze všech skupin uživatelů centra událostí.</span><span class="sxs-lookup"><span data-stu-id="c9a90-164">This approach enables an application to consume data from any of the consumer groups of an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9a90-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9a90-165">Next steps</span></span>
<span data-ttu-id="c9a90-166">Další informace o službě Event Hubs naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="c9a90-166">To learn more about Event Hubs, visit the following topics:</span></span>

* <span data-ttu-id="c9a90-167">[Přehled služby Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="c9a90-167">[Event Hubs overview]</span></span>
* <span data-ttu-id="c9a90-168">[Přehled sdílených přístupových podpisů]</span><span class="sxs-lookup"><span data-stu-id="c9a90-168">[Overview of Shared Access Signatures]</span></span>
* <span data-ttu-id="c9a90-169">[Ukázkové aplikace, které používají službu Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="c9a90-169">[Sample applications that use Event Hubs]</span></span>

<span data-ttu-id="c9a90-170">[Přehled služby Event Hubs]: event-hubs-what-is-event-hubs.md</span><span class="sxs-lookup"><span data-stu-id="c9a90-170">[Event Hubs overview]: event-hubs-what-is-event-hubs.md</span></span>
<span data-ttu-id="c9a90-171">[Ukázkové aplikace, které používají službu Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples</span><span class="sxs-lookup"><span data-stu-id="c9a90-171">[Sample applications that use Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples</span></span>
<span data-ttu-id="c9a90-172">[Přehled sdílených přístupových podpisů]: ../service-bus-messaging/service-bus-sas.md</span><span class="sxs-lookup"><span data-stu-id="c9a90-172">[Overview of Shared Access Signatures]: ../service-bus-messaging/service-bus-sas.md</span></span>

