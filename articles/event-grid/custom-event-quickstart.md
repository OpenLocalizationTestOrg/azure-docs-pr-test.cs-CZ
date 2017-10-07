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
# <a name="create-and-route-custom-events-with-azure-event-grid"></a>Vytvoření a směrování vlastních událostí pomocí služby Azure Event Grid

Azure mřížky událostí je služba eventing pro hello cloud. V tomto článku použijte rozhraní příkazového řádku Azure toocreate hello vlastní téma, přihlášení k odběru tématu toohello a aktivuje hello událostí tooview hello výsledek. Obvykle odesílají koncový bod tooan události, který odpovídá toohello události, jako je webhooku nebo funkce Azure. Však toosimplify to článku, odeslání hello události tooa URL, která shromažďuje jenom hello zprávy. Tuto adresu URL vytvoříte pomocí open source nástroje třetí strany [RequestBin](https://requestb.in/).

>[!NOTE]
>**RequestBin** je open source nástroj, který není určený pro použití vyžadující vysokou propustnost. použití Hello hello nástroje tady je čistě demonstrative. Pokud je více než jednu událost push najednou, nemusíte to vidět všechny události v nástroji hello.

Po dokončení uvidíte, že data události hello byl odeslán tooan koncový bod.

![Data událostí](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto článku vyžaduje, že používáte nejnovější verzi rozhraní příkazového řádku Azure hello (2.0.14 nebo novější). verze hello toofind, spusťte `az --version`. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Témata služby Event Grid jsou prostředky Azure a musí být umístěné ve skupině prostředků Azure. Skupina prostředků Hello je logické kolekce, do které jsou nasazené a spravovat prostředky.

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. 

Hello následující příklad vytvoří skupinu prostředků s názvem *gridResourceGroup* v hello *westus2* umístění.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Vytvoření vlastního tématu

Téma poskytuje uživatelsky definovaný koncový bod, do kterého odesíláte události. Hello následující příklad vytvoří téma hello ve vaší skupině prostředků. Nahraďte `<topic_name>` jedinečným názvem vašeho tématu. název tématu Hello musí být jedinečný, protože je reprezentována položky DNS. Pro verzi preview hello událostí mřížky podporuje **westus2** a **westcentralus** umístění.

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a>Vytvoření koncového bodu zpráv

Před odběr tématu toohello, vytvoříme hello koncový bod pro zprávy události hello. Místo zápisu kódu toorespond toohello událostí, vytvoříme koncový bod, který shromažďuje hello zprávy, aby se dala zobrazit. RequestBin je typu open source, nástroj třetí strany, která vám umožní toocreate koncový bod a zobrazit žádosti, které se odesílají tooit. Přejděte příliš[RequestBin](https://requestb.in/)a klikněte na tlačítko **vytvořit RequestBin**.  Kopírování hello bin adresu URL, protože je třeba při přihlášení k odběru toohello tématu.

## <a name="subscribe-tooa-topic"></a>Přihlášení k odběru tématu tooa

Přihlášení k odběru tématu tootell tooa událostí mřížky události, které chcete tootrack. Hello následující příklad odběratel toohello tématu jste vytvořili a předává hello URL z RequestBin jako hello koncový bod pro oznámení o události. Nahraďte `<event_subscription_name>` s jedinečným názvem pro vaše předplatné a `<URL_from_RequestBin>` s hodnotou hello z hello předcházející části. Zadáním koncový bod při přihlášení k odběru, zpracovává událost mřížky hello směrování události toothat koncového bodu. Pro `<topic_name>`, použijte hodnotu hello jste vytvořili dříve. 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-tooyour-topic"></a>Odeslat tooyour téma události

Teď umožňuje aktivovat události toosee jak událostí mřížky distribuuje koncový bod tooyour zprávy hello. Nejprve umožňuje získat adresu URL hello a klíče pro téma hello. Znovu místo `<topic_name>` použijte název vašeho tématu.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

toosimplify v tomto článku jsme vytvořili ukázkové události dat toosend toohello téma. Aplikace nebo služba Azure by obvykle odesílají data události hello. Následující ukázka Hello získá data události hello:

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

Pokud jste `echo "$body"` uvidíte hello úplné událostí. Hello `data` element hello JSON je hello datové části události. V tomto poli může být libovolný JSON ve správném formátu. Pole Předmět hello můžete použít také pro pokročilé směrování a filtrování.

CURL je nástroj, který provádí požadavky HTTP. V tomto článku používáme CURL toosend hello událostí tooour tématu. 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Mít aktivuje hello události a události mřížky odeslána hello toohello koncový bod zprávy, které jste nakonfigurovali během přihlášení k odběru. Procházejte toohello RequestBin adresu URL, kterou jste vytvořili dříve. Nebo v prohlížeči klikněte na tlačítko pro obnovení otevřeného okna s webem RequestBin. Zobrazí hello událostí, které jste poslali. 

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

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud máte v plánu toocontinue práce se tato událost, to není vyčištění hello prostředky vytvořené v tomto článku. Pokud neplánujete toocontinue, použijte následující příkaz toodelete hello prostředků, kterou jste vytvořili v tomto článku hello.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Další kroky

Teď, když víte, jak toocreate témat a odběrů událostí, další informace o jaké události mřížky vám můžou pomoct:

- [Informace o službě Event Grid](overview.md)
- [Monitorování změn virtuálního počítače pomocí služeb Azure Event Grid a Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
