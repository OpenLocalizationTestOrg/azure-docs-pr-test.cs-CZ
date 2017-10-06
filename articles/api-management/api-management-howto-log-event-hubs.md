---
title: "aaaHow toolog události tooAzure Event Hubs ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toolog události tooAzure Event Hubs ve službě Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 09ca65fc48a874467c6662858f7594e9b19fcdb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toolog-events-tooazure-event-hubs-in-azure-api-management"></a>Jak toolog události tooAzure Event Hubs ve službě Azure API Management
Azure Event Hubs je vysoce škálovatelná data příjem příchozích dat služba, která dokáže přijímat miliony událostí za sekundu, takže umožňuje zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello. Služba Event Hubs slouží jako hello "přední dveře" pro kanál událostí, a jakmile jsou data shromážděna do centra událostí, lze je transformovat a uložené pomocí kteréhokoli poskytovatele služeb, analýzu v reálném čase nebo adaptérů pro dávkování či ukládání. Služba Event Hubs oddělí hello produkce datového proudu událostí od spotřeby těchto události, hello, aby příjemci událostí přístup hello události svého vlastního plánu.

Tento článek je doprovodný toohello [integrovat Azure API Management službou Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video a popisuje, jak toolog API Management událostí pomocí služby Azure Event Hubs.

## <a name="create-an-azure-event-hub"></a>Vytvoření centra událostí Azure
toocreate nového centra událostí, přihlášení toohello [portál Azure classic](https://manage.windowsazure.com) a klikněte na tlačítko **nový**->**App Services**->**Service Bus**  -> **Centra událostí**->**rychle vytvořit**. Zadejte název centra událostí, oblast, vyberte předplatné a vyberte obor názvů. Pokud jste dosud nevytvořili oboru názvů můžete vytvořit jeden zadáním názvu do hello **Namespace** textové pole. Po nakonfigurování všech vlastností klikněte na tlačítko **vytvoření nového centra událostí** toocreate hello centra událostí.

![Vytvoření centra událostí][create-event-hub]

Dále přejděte toohello **konfigurace** kartě nového centra událostí a vytvořte dvě **sdílené zásady přístupu**. Název první hello **odesílající** a pojmenujte ho **odeslat** oprávnění.

![Odesílání zásad][sending-policy]

Název hello druhá **přijetí**, poskytněte **naslouchání** oprávnění a klikněte na tlačítko **Uložit**.

![Přijetí zásad][receiving-policy]

Každá zásada sdíleného přístupu umožňuje toosend aplikace a přijímat události tooand z hello centra událostí. tooaccess hello připojovací řetězce pro tyto zásady, přejděte toohello **řídicí panel** kartě hello centra událostí a klikněte na tlačítko **informace o připojení**.

![Připojovací řetězec][event-hub-dashboard]

Hello **odesílající** připojovací řetězec se používá při protokolování událostí a hello **přijetí** připojovací řetězec se používá při stahování události z hello centra událostí.

![Připojovací řetězec][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a>Vytvoření protokolovač API Management
Teď, když máte centra událostí, je dalším krokem hello tooconfigure [Protokolovač](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) ve službě API Management služby tak, aby mohl zaprotokolovat události toohello centra událostí.

Protokolovací nástroje API Management jsou konfigurováni pomocí hello [rozhraní API REST API správy](http://aka.ms/smapi). Před použitím hello REST API pro hello poprvé, přečtěte si hello [požadavky](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) a ujistěte se, že máte [povolen přístup toohello REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

toocreate protokolovacího nástroje, ujistěte se, požadavek HTTP PUT pomocí hello následující adresu URL šablony.

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* Nahraďte `{your service}` s názvem hello instanci služby API Management.
* Nahraďte `{new logger name}` s hello požadovaný název pro vaše nové protokolovacího nástroje. Tento název bude odkazovat, když konfigurujete hello [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) zásad

Přidejte následující hlavičky požadavku toohello hello.

* Content-Type: application/json
* Autorizace: SharedAccessSignature 58...
  * Pokyny pro generování hello `SharedAccessSignature` najdete v části [Azure API Management REST API ověřování](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Zadejte text žádosti hello pomocí následující šablony hello.

```json
{
  "type" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of hello Event Hub from hello Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `type`musí být nastaven příliš`AzureEventHub`.
* `description`poskytuje volitelný popis hello protokoly a v případě potřeby může být řetězec nulové délky.
* `credentials`obsahuje hello `name` a `connectionString` centra událostí Azure.

Pokud vytvoříte požadavek hello, pokud hello protokoly se vytvoří stavový kód `201 Created` je vrácen.

> [!NOTE]
> Ostatní možné návratové kódy a jejich důvodů najdete v tématu [vytvořit protokoly](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). toosee jak provádět další operace, například seznam, aktualizace a odstranění, najdete v části hello [Protokolovač](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) dokumentace entity.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Konfigurace zásad protokolu eventhubs
Jakmile vaše protokoly nakonfigurovaný ve službě API Management, můžete nakonfigurovat zásady protokolu eventhubs toolog hello potřeby události. Hello protokolu eventhubs zásadu lze použít buď hello oddílu zásad příchozí nebo odchozí zásady hello.

tooconfigure zásady, přihlášení toohello [portál Azure](https://portal.azure.com), přejděte tooyour služba API Management a klikněte na tlačítko **portál vydavatele** tooaccess hello portál vydavatele.

![Portál vydavatele][publisher-portal]

Klikněte na tlačítko **zásady** v nabídce hello API Management na levé straně hello, vyberte požadovaný produkt hello a rozhraní API a klikněte na **přidat zásadu**. V tomto příkladu je právě přidávána zásad toohello **Echo API** v hello **neomezený** produktu.

![Přidání zásad][add-policy]

Umístěte kurzor v hello `inbound` zásad a klikněte na hello **protokolu tooEventHub** zásad tooinsert hello `log-to-eventhub` příkaz šablony zásad.

![Editor zásad][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

Nahraďte `logger-id` hello název protokolovacího nástroje API Management hello, které jste nakonfigurovali v předchozím kroku hello.

Můžete použít libovolný výraz, který vrací řetězec jako hodnotu hello hello `log-to-eventhub` elementu. V tomto příkladu se zaznamená řetězec obsahující hello datum a čas, název služby, id požadavku, požadavek ip adresu a název operace.

Klikněte na tlačítko **Uložit** toosave hello aktualizovat konfiguraci zásad. Jakmile je uložen hello zásad je aktivní a události jsou zaznamenané toohello určené centra událostí.

## <a name="next-steps"></a>Další kroky
* Další informace o Azure Event Hubs
  * [Začínáme s Azure Event Hubs](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Přijímat zprávy pomocí třídy EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Průvodce programováním pro službu Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Další informace o integraci API Management a služby Event Hubs
  * [Odkaz na entitu protokolovacího nástroje](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [referenční informace o protokolu eventhub zásad](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Sledovat vaše rozhraní API s Azure API Management, Event Hubs a Runscope](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a>Podívejte se na video s návodem
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Integrate-Azure-API-Management-with-Event-Hubs/player]
>
>

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
