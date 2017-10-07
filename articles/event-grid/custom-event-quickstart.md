---
title: "aaaCustom události pro události mřížky Azure | Microsoft Docs"
description: "Pomocí Azure událostí mřížky toopublish téma a přihlášení k odběru událostí toothat."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 5055df1c970b043cadf06978a076f7f5c83501cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-route-custom-events-with-azure-event-grid"></a><span data-ttu-id="baaf3-103">Vytvoření a směrování vlastních událostí pomocí služby Azure Event Grid</span><span class="sxs-lookup"><span data-stu-id="baaf3-103">Create and route custom events with Azure Event Grid</span></span>

<span data-ttu-id="baaf3-104">Azure mřížky událostí je služba eventing pro hello cloud.</span><span class="sxs-lookup"><span data-stu-id="baaf3-104">Azure Event Grid is an eventing service for hello cloud.</span></span> <span data-ttu-id="baaf3-105">V tomto článku použijte rozhraní příkazového řádku Azure toocreate hello vlastní téma, přihlášení k odběru tématu toohello a aktivuje hello událostí tooview hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="baaf3-105">In this article, you use hello Azure CLI toocreate a custom topic, subscribe toohello topic, and trigger hello event tooview hello result.</span></span> <span data-ttu-id="baaf3-106">Obvykle odesílají koncový bod tooan události, který odpovídá toohello události, jako je webhooku nebo funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="baaf3-106">Typically, you send events tooan endpoint that responds toohello event, such as, a webhook or Azure Function.</span></span> <span data-ttu-id="baaf3-107">Však toosimplify to článku, odeslání hello události tooa URL, která shromažďuje jenom hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="baaf3-107">However, toosimplify this article, you send hello events tooa URL that merely collects hello messages.</span></span> <span data-ttu-id="baaf3-108">Tuto adresu URL vytvoříte pomocí open source nástroje třetí strany [RequestBin](https://requestb.in/).</span><span class="sxs-lookup"><span data-stu-id="baaf3-108">You create this URL by using an open source, third-party tool called [RequestBin](https://requestb.in/).</span></span>

>[!NOTE]
><span data-ttu-id="baaf3-109">**RequestBin** je open source nástroj, který není určený pro použití vyžadující vysokou propustnost.</span><span class="sxs-lookup"><span data-stu-id="baaf3-109">**RequestBin** is an open source tool that is not intended for high throughput usage.</span></span> <span data-ttu-id="baaf3-110">použití Hello hello nástroje tady je čistě demonstrative.</span><span class="sxs-lookup"><span data-stu-id="baaf3-110">hello use of hello tool here is purely demonstrative.</span></span> <span data-ttu-id="baaf3-111">Pokud je více než jednu událost push najednou, nemusíte to vidět všechny události v nástroji hello.</span><span class="sxs-lookup"><span data-stu-id="baaf3-111">If you push more than one event at a time, you might not see all of your events in hello tool.</span></span>

<span data-ttu-id="baaf3-112">Po dokončení uvidíte, že data události hello byl odeslán tooan koncový bod.</span><span class="sxs-lookup"><span data-stu-id="baaf3-112">When you are finished, you see that hello event data has been sent tooan endpoint.</span></span>

![Data událostí](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="baaf3-114">Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto článku vyžaduje, že používáte nejnovější verzi rozhraní příkazového řádku Azure hello (2.0.14 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="baaf3-114">If you choose tooinstall and use hello CLI locally, this article requires that you are running hello latest version of Azure CLI (2.0.14 or later).</span></span> <span data-ttu-id="baaf3-115">verze hello toofind, spusťte `az --version`.</span><span class="sxs-lookup"><span data-stu-id="baaf3-115">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="baaf3-116">Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="baaf3-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="baaf3-117">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="baaf3-117">Create a resource group</span></span>

<span data-ttu-id="baaf3-118">Témata služby Event Grid jsou prostředky Azure a musí být umístěné ve skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="baaf3-118">Event Grid topics are Azure resources, and must be placed in an Azure resource group.</span></span> <span data-ttu-id="baaf3-119">Skupina prostředků Hello je logické kolekce, do které jsou nasazené a spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="baaf3-119">hello resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="baaf3-120">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="baaf3-120">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="baaf3-121">Hello následující příklad vytvoří skupinu prostředků s názvem *gridResourceGroup* v hello *westus2* umístění.</span><span class="sxs-lookup"><span data-stu-id="baaf3-121">hello following example creates a resource group named *gridResourceGroup* in hello *westus2* location.</span></span>

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a><span data-ttu-id="baaf3-122">Vytvoření vlastního tématu</span><span class="sxs-lookup"><span data-stu-id="baaf3-122">Create a custom topic</span></span>

<span data-ttu-id="baaf3-123">Téma poskytuje uživatelsky definovaný koncový bod, do kterého odesíláte události.</span><span class="sxs-lookup"><span data-stu-id="baaf3-123">A topic provides a user-defined endpoint that you post your events to.</span></span> <span data-ttu-id="baaf3-124">Hello následující příklad vytvoří téma hello ve vaší skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="baaf3-124">hello following example creates hello topic in your resource group.</span></span> <span data-ttu-id="baaf3-125">Nahraďte `<topic_name>` jedinečným názvem vašeho tématu.</span><span class="sxs-lookup"><span data-stu-id="baaf3-125">Replace `<topic_name>` with a unique name for your topic.</span></span> <span data-ttu-id="baaf3-126">název tématu Hello musí být jedinečný, protože je reprezentována položky DNS.</span><span class="sxs-lookup"><span data-stu-id="baaf3-126">hello topic name must be unique because it is represented by a DNS entry.</span></span> <span data-ttu-id="baaf3-127">Pro verzi preview hello událostí mřížky podporuje **westus2** a **westcentralus** umístění.</span><span class="sxs-lookup"><span data-stu-id="baaf3-127">For hello preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span>

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a><span data-ttu-id="baaf3-128">Vytvoření koncového bodu zpráv</span><span class="sxs-lookup"><span data-stu-id="baaf3-128">Create a message endpoint</span></span>

<span data-ttu-id="baaf3-129">Před odběr tématu toohello, vytvoříme hello koncový bod pro zprávy události hello.</span><span class="sxs-lookup"><span data-stu-id="baaf3-129">Before subscribing toohello topic, let's create hello endpoint for hello event message.</span></span> <span data-ttu-id="baaf3-130">Místo zápisu kódu toorespond toohello událostí, vytvoříme koncový bod, který shromažďuje hello zprávy, aby se dala zobrazit.</span><span class="sxs-lookup"><span data-stu-id="baaf3-130">Rather than write code toorespond toohello event, let's create an endpoint that collects hello messages so you can view them.</span></span> <span data-ttu-id="baaf3-131">RequestBin je typu open source, nástroj třetí strany, která vám umožní toocreate koncový bod a zobrazit žádosti, které se odesílají tooit.</span><span class="sxs-lookup"><span data-stu-id="baaf3-131">RequestBin is an open source, third-party tool that enables you toocreate an endpoint, and view requests that are sent tooit.</span></span> <span data-ttu-id="baaf3-132">Přejděte příliš[RequestBin](https://requestb.in/)a klikněte na tlačítko **vytvořit RequestBin**.</span><span class="sxs-lookup"><span data-stu-id="baaf3-132">Go too[RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span>  <span data-ttu-id="baaf3-133">Kopírování hello bin adresu URL, protože je třeba při přihlášení k odběru toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="baaf3-133">Copy hello bin URL, because you need it when subscribing toohello topic.</span></span>

## <a name="subscribe-tooa-topic"></a><span data-ttu-id="baaf3-134">Přihlášení k odběru tématu tooa</span><span class="sxs-lookup"><span data-stu-id="baaf3-134">Subscribe tooa topic</span></span>

<span data-ttu-id="baaf3-135">Přihlášení k odběru tématu tootell tooa událostí mřížky události, které chcete tootrack.</span><span class="sxs-lookup"><span data-stu-id="baaf3-135">You subscribe tooa topic tootell Event Grid which events you want tootrack.</span></span> <span data-ttu-id="baaf3-136">Hello následující příklad odběratel toohello tématu jste vytvořili a předává hello URL z RequestBin jako hello koncový bod pro oznámení o události.</span><span class="sxs-lookup"><span data-stu-id="baaf3-136">hello following example subscribes toohello topic you created, and passes hello URL from RequestBin as hello endpoint for event notification.</span></span> <span data-ttu-id="baaf3-137">Nahraďte `<event_subscription_name>` s jedinečným názvem pro vaše předplatné a `<URL_from_RequestBin>` s hodnotou hello z hello předcházející části.</span><span class="sxs-lookup"><span data-stu-id="baaf3-137">Replace `<event_subscription_name>` with a unique name for your subscription, and `<URL_from_RequestBin>` with hello value from hello preceding section.</span></span> <span data-ttu-id="baaf3-138">Zadáním koncový bod při přihlášení k odběru, zpracovává událost mřížky hello směrování události toothat koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="baaf3-138">By specifying an endpoint when subscribing, Event Grid handles hello routing of events toothat endpoint.</span></span> <span data-ttu-id="baaf3-139">Pro `<topic_name>`, použijte hodnotu hello jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="baaf3-139">For `<topic_name>`, use hello value you created earlier.</span></span> 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-tooyour-topic"></a><span data-ttu-id="baaf3-140">Odeslat tooyour téma události</span><span class="sxs-lookup"><span data-stu-id="baaf3-140">Send an event tooyour topic</span></span>

<span data-ttu-id="baaf3-141">Teď umožňuje aktivovat události toosee jak událostí mřížky distribuuje koncový bod tooyour zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="baaf3-141">Now, let's trigger an event toosee how Event Grid distributes hello message tooyour endpoint.</span></span> <span data-ttu-id="baaf3-142">Nejprve umožňuje získat adresu URL hello a klíče pro téma hello.</span><span class="sxs-lookup"><span data-stu-id="baaf3-142">First, let's get hello URL and key for hello topic.</span></span> <span data-ttu-id="baaf3-143">Znovu místo `<topic_name>` použijte název vašeho tématu.</span><span class="sxs-lookup"><span data-stu-id="baaf3-143">Again, use your topic name for `<topic_name>`.</span></span>

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

<span data-ttu-id="baaf3-144">toosimplify v tomto článku jsme vytvořili ukázkové události dat toosend toohello téma.</span><span class="sxs-lookup"><span data-stu-id="baaf3-144">toosimplify this article, we have set up sample event data toosend toohello topic.</span></span> <span data-ttu-id="baaf3-145">Aplikace nebo služba Azure by obvykle odesílají data události hello.</span><span class="sxs-lookup"><span data-stu-id="baaf3-145">Typically, an application or Azure service would send hello event data.</span></span> <span data-ttu-id="baaf3-146">Následující ukázka Hello získá data události hello:</span><span class="sxs-lookup"><span data-stu-id="baaf3-146">hello following example gets hello event data:</span></span>

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

<span data-ttu-id="baaf3-147">Pokud jste `echo "$body"` uvidíte hello úplné událostí.</span><span class="sxs-lookup"><span data-stu-id="baaf3-147">If you `echo "$body"` you can see hello full event.</span></span> <span data-ttu-id="baaf3-148">Hello `data` element hello JSON je hello datové části události.</span><span class="sxs-lookup"><span data-stu-id="baaf3-148">hello `data` element of hello JSON is hello payload of your event.</span></span> <span data-ttu-id="baaf3-149">V tomto poli může být libovolný JSON ve správném formátu.</span><span class="sxs-lookup"><span data-stu-id="baaf3-149">Any well-formed JSON can go in this field.</span></span> <span data-ttu-id="baaf3-150">Pole Předmět hello můžete použít také pro pokročilé směrování a filtrování.</span><span class="sxs-lookup"><span data-stu-id="baaf3-150">You can also use hello subject field for advanced routing and filtering.</span></span>

<span data-ttu-id="baaf3-151">CURL je nástroj, který provádí požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="baaf3-151">CURL is a utility that performs HTTP requests.</span></span> <span data-ttu-id="baaf3-152">V tomto článku používáme CURL toosend hello událostí tooour tématu.</span><span class="sxs-lookup"><span data-stu-id="baaf3-152">In this article, we use CURL toosend hello event tooour topic.</span></span> 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

<span data-ttu-id="baaf3-153">Mít aktivuje hello události a události mřížky odeslána hello toohello koncový bod zprávy, které jste nakonfigurovali během přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="baaf3-153">You have triggered hello event, and Event Grid sent hello message toohello endpoint you configured when subscribing.</span></span> <span data-ttu-id="baaf3-154">Procházejte toohello RequestBin adresu URL, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="baaf3-154">Browse toohello RequestBin URL that you created earlier.</span></span> <span data-ttu-id="baaf3-155">Nebo v prohlížeči klikněte na tlačítko pro obnovení otevřeného okna s webem RequestBin.</span><span class="sxs-lookup"><span data-stu-id="baaf3-155">Or, click refresh in your open RequestBin browser.</span></span> <span data-ttu-id="baaf3-156">Zobrazí hello událostí, které jste poslali.</span><span class="sxs-lookup"><span data-stu-id="baaf3-156">You see hello event you just sent.</span></span> 

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a><span data-ttu-id="baaf3-157">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="baaf3-157">Clean up resources</span></span>
<span data-ttu-id="baaf3-158">Pokud máte v plánu toocontinue práce se tato událost, to není vyčištění hello prostředky vytvořené v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="baaf3-158">If you plan toocontinue working with this event, do not clean up hello resources created in this article.</span></span> <span data-ttu-id="baaf3-159">Pokud neplánujete toocontinue, použijte následující příkaz toodelete hello prostředků, kterou jste vytvořili v tomto článku hello.</span><span class="sxs-lookup"><span data-stu-id="baaf3-159">If you do not plan toocontinue, use hello following command toodelete hello resources you created in this article.</span></span>

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="baaf3-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="baaf3-160">Next steps</span></span>

<span data-ttu-id="baaf3-161">Teď, když víte, jak toocreate témat a odběrů událostí, další informace o jaké události mřížky vám můžou pomoct:</span><span class="sxs-lookup"><span data-stu-id="baaf3-161">Now that you know how toocreate topics and event subscriptions, learn more about what Event Grid can help you do:</span></span>

- [<span data-ttu-id="baaf3-162">Informace o službě Event Grid</span><span class="sxs-lookup"><span data-stu-id="baaf3-162">About Event Grid</span></span>](overview.md)
- [<span data-ttu-id="baaf3-163">Monitorování změn virtuálního počítače pomocí služeb Azure Event Grid a Logic Apps</span><span class="sxs-lookup"><span data-stu-id="baaf3-163">Monitor virtual machine changes with Azure Event Grid and Logic Apps</span></span>](monitor-virtual-machine-changes-event-grid-logic-app.md)
