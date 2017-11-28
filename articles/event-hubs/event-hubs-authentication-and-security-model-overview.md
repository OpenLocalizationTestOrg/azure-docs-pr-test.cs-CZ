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
# <a name="event-hubs-authentication-and-security-model-overview"></a><span data-ttu-id="77b56-103">Ověřování a zabezpečení modelu přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="77b56-103">Event Hubs authentication and security model overview</span></span>
<span data-ttu-id="77b56-104">model zabezpečení služby Azure Event Hubs Hello splňuje hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="77b56-104">hello Azure Event Hubs security model meets hello following requirements:</span></span>

* <span data-ttu-id="77b56-105">Centra událostí tooan dat může odesílat pouze klienty, která představují platné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="77b56-105">Only clients that present valid credentials can send data tooan event hub.</span></span>
* <span data-ttu-id="77b56-106">Klient nemůže zosobnit jiného klienta.</span><span class="sxs-lookup"><span data-stu-id="77b56-106">A client cannot impersonate another client.</span></span>
* <span data-ttu-id="77b56-107">Podvodného klienta můžete blokovat odesílání centra událostí tooan data.</span><span class="sxs-lookup"><span data-stu-id="77b56-107">A rogue client can be blocked from sending data tooan event hub.</span></span>

## <a name="client-authentication"></a><span data-ttu-id="77b56-108">Ověření klienta</span><span class="sxs-lookup"><span data-stu-id="77b56-108">Client authentication</span></span>
<span data-ttu-id="77b56-109">model zabezpečení služby Event Hubs Hello je založen na kombinaci [sdíleného přístupového podpisu (SAS)](../service-bus-messaging/service-bus-sas.md) tokeny a *zdroje událostí*.</span><span class="sxs-lookup"><span data-stu-id="77b56-109">hello Event Hubs security model is based on a combination of [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) tokens and *event publishers*.</span></span> <span data-ttu-id="77b56-110">Vydavatel události definuje virtuální koncový bod pro centra událostí.</span><span class="sxs-lookup"><span data-stu-id="77b56-110">An event publisher defines a virtual endpoint for an event hub.</span></span> <span data-ttu-id="77b56-111">Vydavatel Hello může být pouze centra událostí tooan použité toosend zprávy.</span><span class="sxs-lookup"><span data-stu-id="77b56-111">hello publisher can only be used toosend messages tooan event hub.</span></span> <span data-ttu-id="77b56-112">Není možné tooreceive zprávy od vydavatele.</span><span class="sxs-lookup"><span data-stu-id="77b56-112">It is not possible tooreceive messages from a publisher.</span></span>

<span data-ttu-id="77b56-113">Centra událostí obvykle aktivuje jeden vydavatele za klienta.</span><span class="sxs-lookup"><span data-stu-id="77b56-113">Typically, an event hub employs one publisher per client.</span></span> <span data-ttu-id="77b56-114">Všechny zprávy, které se odesílají tooany hello vydavatele centra událostí jsou zařazených do fronty v rámci tohoto centra událostí.</span><span class="sxs-lookup"><span data-stu-id="77b56-114">All messages that are sent tooany of hello publishers of an event hub are enqueued within that event hub.</span></span> <span data-ttu-id="77b56-115">Vydavatelé povolte řízení podrobných přístupu a omezení.</span><span class="sxs-lookup"><span data-stu-id="77b56-115">Publishers enable fine-grained access control and throttling.</span></span>

<span data-ttu-id="77b56-116">Každý klient služby Event Hubs je přiřazen jedinečný token, který je nahraný toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="77b56-116">Each Event Hubs client is assigned a unique token, which is uploaded toohello client.</span></span> <span data-ttu-id="77b56-117">Hello tokenů vytváří tak, aby každý jedinečný token uděluje přístup tooa jiný jedinečný vydavatele.</span><span class="sxs-lookup"><span data-stu-id="77b56-117">hello tokens are produced such that each unique token grants access tooa different unique publisher.</span></span> <span data-ttu-id="77b56-118">Klient, který má token lze odeslat pouze tooone vydavatele, ale žádný další vydavatel.</span><span class="sxs-lookup"><span data-stu-id="77b56-118">A client that possesses a token can only send tooone publisher, but no other publisher.</span></span> <span data-ttu-id="77b56-119">Pokud sdílená složka více klientů hello stejný token a každý z nich sdílí vydavatel.</span><span class="sxs-lookup"><span data-stu-id="77b56-119">If multiple clients share hello same token, then each of them shares a publisher.</span></span>

<span data-ttu-id="77b56-120">I když není doporučeno, je možné tooequip zařízení s tokeny, které udělit centra událostí tooan přímý přístup.</span><span class="sxs-lookup"><span data-stu-id="77b56-120">Although not recommended, it is possible tooequip devices with tokens that grant direct access tooan event hub.</span></span> <span data-ttu-id="77b56-121">Zprávy můžete přímo do tohoto centra událostí posílat jakékoli zařízení, která obsahuje tento token.</span><span class="sxs-lookup"><span data-stu-id="77b56-121">Any device that holds this token can send messages directly into that event hub.</span></span> <span data-ttu-id="77b56-122">Taková zařízení nebude toothrottling subjektu.</span><span class="sxs-lookup"><span data-stu-id="77b56-122">Such a device will not be subject toothrottling.</span></span> <span data-ttu-id="77b56-123">Kromě toho hello zařízení nemůže být zakázáno odesílání centra událostí toothat z.</span><span class="sxs-lookup"><span data-stu-id="77b56-123">Furthermore, hello device cannot be blacklisted from sending toothat event hub.</span></span>

<span data-ttu-id="77b56-124">Všechny tokeny jsou podepsané klíče SAS.</span><span class="sxs-lookup"><span data-stu-id="77b56-124">All tokens are signed with a SAS key.</span></span> <span data-ttu-id="77b56-125">Obvykle jsou všechny tokeny hello podepsané stejným klíčem.</span><span class="sxs-lookup"><span data-stu-id="77b56-125">Typically, all tokens are signed with hello same key.</span></span> <span data-ttu-id="77b56-126">Klienti nemají informace o hello klíč; To zabrání tomu, aby ostatní klienty výrobní tokeny.</span><span class="sxs-lookup"><span data-stu-id="77b56-126">Clients are not aware of hello key; this prevents other clients from manufacturing tokens.</span></span>

### <a name="create-hello-sas-key"></a><span data-ttu-id="77b56-127">Vytvořte klíč SAS hello</span><span class="sxs-lookup"><span data-stu-id="77b56-127">Create hello SAS key</span></span>

<span data-ttu-id="77b56-128">Při vytváření na obor názvů služby Event Hubs, hello služby vygeneruje SAS klíč 256 bitů s názvem **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="77b56-128">When creating an Event Hubs namespace, hello service generates a 256-bit SAS key named **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="77b56-129">Tento klíč uděluje odeslat, naslouchat a spravovat obor názvů toohello práva.</span><span class="sxs-lookup"><span data-stu-id="77b56-129">This key grants send, listen, and manage rights toohello namespace.</span></span> <span data-ttu-id="77b56-130">Můžete také vytvořit další klíče.</span><span class="sxs-lookup"><span data-stu-id="77b56-130">You can also create additional keys.</span></span> <span data-ttu-id="77b56-131">Doporučuje se, že můžete vytvořit klíče, odeslat uděluje oprávnění toohello konkrétní události rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="77b56-131">It is recommended that you produce a key that grants send permissions toohello specific event hub.</span></span> <span data-ttu-id="77b56-132">Pro hello zbývající část tohoto tématu, předpokládá se, tento klíč s názvem **EventHubSendKey**.</span><span class="sxs-lookup"><span data-stu-id="77b56-132">For hello remainder of this topic, it is assumed that you named this key **EventHubSendKey**.</span></span>

<span data-ttu-id="77b56-133">Hello následující ukázka vytvoří jen odesílání klíč při vytváření centra událostí hello:</span><span class="sxs-lookup"><span data-stu-id="77b56-133">hello following example creates a send-only key when creating hello event hub:</span></span>

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

### <a name="generate-tokens"></a><span data-ttu-id="77b56-134">Generování tokenů</span><span class="sxs-lookup"><span data-stu-id="77b56-134">Generate tokens</span></span>

<span data-ttu-id="77b56-135">Můžete vygenerovat tokeny pomocí klíče SAS hello.</span><span class="sxs-lookup"><span data-stu-id="77b56-135">You can generate tokens using hello SAS key.</span></span> <span data-ttu-id="77b56-136">Musíte vytvořit jenom jeden token za klienta.</span><span class="sxs-lookup"><span data-stu-id="77b56-136">You must produce only one token per client.</span></span> <span data-ttu-id="77b56-137">Tokeny je pak možné vytvořit pomocí hello následující metodu.</span><span class="sxs-lookup"><span data-stu-id="77b56-137">Tokens can then be produced using hello following method.</span></span> <span data-ttu-id="77b56-138">Všechny tokeny jsou generovány pomocí hello **EventHubSendKey** klíč.</span><span class="sxs-lookup"><span data-stu-id="77b56-138">All tokens are generated using hello **EventHubSendKey** key.</span></span> <span data-ttu-id="77b56-139">Každý token je přiřazen jedinečný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="77b56-139">Each token is assigned a unique URI.</span></span>

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

<span data-ttu-id="77b56-140">Při volání této metody, hello identifikátor URI musí být zadány ve tvaru `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="77b56-140">When calling this method, hello URI should be specified as `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span></span> <span data-ttu-id="77b56-141">Pro všechny tokeny, je stejné, s výjimkou hello hello URI `PUBLISHER_NAME`, což by mělo být různé pro každý token.</span><span class="sxs-lookup"><span data-stu-id="77b56-141">For all tokens, hello URI is identical, with hello exception of `PUBLISHER_NAME`, which should be different for each token.</span></span> <span data-ttu-id="77b56-142">V ideálním případě `PUBLISHER_NAME` představuje hello ID hello klienta, která přijímá token.</span><span class="sxs-lookup"><span data-stu-id="77b56-142">Ideally, `PUBLISHER_NAME` represents hello ID of hello client that receives that token.</span></span>

<span data-ttu-id="77b56-143">Tato metoda generuje token s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="77b56-143">This method generates a token with hello following structure:</span></span>

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

<span data-ttu-id="77b56-144">čas vypršení platnosti tokenu Hello je zadána v sekundách od 1. ledna 1970.</span><span class="sxs-lookup"><span data-stu-id="77b56-144">hello token expiration time is specified in seconds from Jan 1, 1970.</span></span> <span data-ttu-id="77b56-145">Hello následuje příklad token:</span><span class="sxs-lookup"><span data-stu-id="77b56-145">hello following is an example of a token:</span></span>

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

<span data-ttu-id="77b56-146">Obvykle hello tokeny mají životnost, která se podobá nebo překračuje hello životnost klienta hello.</span><span class="sxs-lookup"><span data-stu-id="77b56-146">Typically, hello tokens have a lifespan that resembles or exceeds hello lifespan of hello client.</span></span> <span data-ttu-id="77b56-147">Pokud má klient hello hello schopností tooobtain nový token, lze použít tokeny s kratší životnost.</span><span class="sxs-lookup"><span data-stu-id="77b56-147">If hello client has hello capability tooobtain a new token, tokens with a shorter lifespan can be used.</span></span>

### <a name="sending-data"></a><span data-ttu-id="77b56-148">Odeslání dat</span><span class="sxs-lookup"><span data-stu-id="77b56-148">Sending data</span></span>
<span data-ttu-id="77b56-149">Po vytvoření hello tokeny jednotlivých klientů je opatřen svůj vlastní jedinečný token.</span><span class="sxs-lookup"><span data-stu-id="77b56-149">Once hello tokens have been created, each client is provisioned with its own unique token.</span></span>

<span data-ttu-id="77b56-150">Když klient hello odešle data do centra událostí, značky odeslání požadavku s tokenem hello.</span><span class="sxs-lookup"><span data-stu-id="77b56-150">When hello client sends data into an event hub, it tags its send request with hello token.</span></span> <span data-ttu-id="77b56-151">tooprevent útočník z odposlouchávání a krádež hello token, hello komunikaci mezi klientem hello a hello centra událostí, musí dojít přes šifrovaný kanál.</span><span class="sxs-lookup"><span data-stu-id="77b56-151">tooprevent an attacker from eavesdropping and stealing hello token, hello communication between hello client and hello event hub must occur over an encrypted channel.</span></span>

### <a name="blacklisting-clients"></a><span data-ttu-id="77b56-152">Blokované klienty</span><span class="sxs-lookup"><span data-stu-id="77b56-152">Blacklisting clients</span></span>
<span data-ttu-id="77b56-153">V případě krádeže token uživatelem se zlými úmysly může útočník hello zosobnit klienta hello odcizen jejichž token.</span><span class="sxs-lookup"><span data-stu-id="77b56-153">If a token is stolen by an attacker, hello attacker can impersonate hello client whose token has been stolen.</span></span> <span data-ttu-id="77b56-154">Seznamů zakázaných klienta vykreslí tohoto klienta nepoužitelný, dokud neobdrží nový token, který používá jiný vydavatel.</span><span class="sxs-lookup"><span data-stu-id="77b56-154">Blacklisting a client renders that client unusable until it receives a new token that uses a different publisher.</span></span>

## <a name="authentication-of-back-end-applications"></a><span data-ttu-id="77b56-155">Ověřování back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="77b56-155">Authentication of back-end applications</span></span>

<span data-ttu-id="77b56-156">tooauthenticate back-end aplikace, které využívají data hello generované klientů služby Event Hubs, Event Hubs využívá model zabezpečení, který je podobný toohello model, který se používá pro témata Service Bus.</span><span class="sxs-lookup"><span data-stu-id="77b56-156">tooauthenticate back-end applications that consume hello data generated by Event Hubs clients, Event Hubs employs a security model that is similar toohello model that is used for Service Bus topics.</span></span> <span data-ttu-id="77b56-157">Skupiny uživatelů služby Event Hubs je ekvivalentní tooa předplatné tooa Service Bus tématu.</span><span class="sxs-lookup"><span data-stu-id="77b56-157">An Event Hubs consumer group is equivalent tooa subscription tooa Service Bus topic.</span></span> <span data-ttu-id="77b56-158">Klienta můžete vytvořit skupiny příjemců, pokud se skupina uživatelů hello požadavek toocreate hello spolu token spravovat uděluje oprávnění pro hello centra událostí, nebo pro obor názvů toowhich hello centra událostí hello patří.</span><span class="sxs-lookup"><span data-stu-id="77b56-158">A client can create a consumer group if hello request toocreate hello consumer group is accompanied by a token that grants manage privileges for hello event hub, or for hello namespace toowhich hello event hub belongs.</span></span> <span data-ttu-id="77b56-159">Klient je povolen, tooconsume data ze skupiny příjemců Pokud hello přijmout žádost o je doplněny tokenem, přijímat uděluje oprávnění v této skupiny příjemců, hello centra událostí, nebo Centrum událostí hello obor názvů toowhich hello patří.</span><span class="sxs-lookup"><span data-stu-id="77b56-159">A client is allowed tooconsume data from a consumer group if hello receive request is accompanied by a token that grants receive rights on that consumer group, hello event hub, or hello namespace toowhich hello event hub belongs.</span></span>

<span data-ttu-id="77b56-160">Hello aktuální verzi Service Bus nepodporuje SAS pravidla pro jednotlivé odběry.</span><span class="sxs-lookup"><span data-stu-id="77b56-160">hello current version of Service Bus does not support SAS rules for individual subscriptions.</span></span> <span data-ttu-id="77b56-161">Dobrý den, které to samé platí pro skupiny uživatelů služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="77b56-161">hello same holds true for Event Hubs consumer groups.</span></span> <span data-ttu-id="77b56-162">Podpora SAS přidá pro obě funkce v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="77b56-162">SAS support will be added for both features in hello future.</span></span>

<span data-ttu-id="77b56-163">Hello existovat ověřování SAS pro jednotlivé příjemce skupiny můžete toosecure klíče SAS všechny skupiny příjemců běžné klíčem.</span><span class="sxs-lookup"><span data-stu-id="77b56-163">In hello absence of SAS authentication for individual consumer groups, you can use SAS keys toosecure all consumer groups with a common key.</span></span> <span data-ttu-id="77b56-164">Tento přístup umožňuje tooconsume data aplikací ze všech skupin uživatelů hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="77b56-164">This approach enables an application tooconsume data from any of hello consumer groups of an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="77b56-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77b56-165">Next steps</span></span>
<span data-ttu-id="77b56-166">toolearn víc o službě Event Hubs naleznete hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="77b56-166">toolearn more about Event Hubs, visit hello following topics:</span></span>

* <span data-ttu-id="77b56-167">[Přehled služby Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="77b56-167">[Event Hubs overview]</span></span>
* <span data-ttu-id="77b56-168">[Přehled sdílených přístupových podpisů]</span><span class="sxs-lookup"><span data-stu-id="77b56-168">[Overview of Shared Access Signatures]</span></span>
* <span data-ttu-id="77b56-169">[Ukázkové aplikace, které používají službu Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="77b56-169">[Sample applications that use Event Hubs]</span></span>

[Přehled služby Event Hubs]: event-hubs-what-is-event-hubs.md
[Ukázkové aplikace, které používají službu Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Přehled sdílených přístupových podpisů]: ../service-bus-messaging/service-bus-sas.md

