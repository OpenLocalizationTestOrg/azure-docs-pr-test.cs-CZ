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
# <a name="how-toolog-events-tooazure-event-hubs-in-azure-api-management"></a><span data-ttu-id="b9c41-103">Jak toolog události tooAzure Event Hubs ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="b9c41-103">How toolog events tooAzure Event Hubs in Azure API Management</span></span>
<span data-ttu-id="b9c41-104">Azure Event Hubs je vysoce škálovatelná data příjem příchozích dat služba, která dokáže přijímat miliony událostí za sekundu, takže umožňuje zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b9c41-104">Azure Event Hubs is a highly scalable data ingress service that can ingest millions of events per second so that you can process and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="b9c41-105">Služba Event Hubs slouží jako hello "přední dveře" pro kanál událostí, a jakmile jsou data shromážděna do centra událostí, lze je transformovat a uložené pomocí kteréhokoli poskytovatele služeb, analýzu v reálném čase nebo adaptérů pro dávkování či ukládání.</span><span class="sxs-lookup"><span data-stu-id="b9c41-105">Event Hubs acts as hello "front door" for an event pipeline, and once data is collected into an event hub, it can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="b9c41-106">Služba Event Hubs oddělí hello produkce datového proudu událostí od spotřeby těchto události, hello, aby příjemci událostí přístup hello události svého vlastního plánu.</span><span class="sxs-lookup"><span data-stu-id="b9c41-106">Event Hubs decouples hello production of a stream of events from hello consumption of those events, so that event consumers can access hello events on their own schedule.</span></span>

<span data-ttu-id="b9c41-107">Tento článek je doprovodný toohello [integrovat Azure API Management službou Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video a popisuje, jak toolog API Management událostí pomocí služby Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="b9c41-107">This article is a companion toohello [Integrate Azure API Management with Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video and describes how toolog API Management events using Azure Event Hubs.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="b9c41-108">Vytvoření centra událostí Azure</span><span class="sxs-lookup"><span data-stu-id="b9c41-108">Create an Azure Event Hub</span></span>
<span data-ttu-id="b9c41-109">toocreate nového centra událostí, přihlášení toohello [portál Azure classic](https://manage.windowsazure.com) a klikněte na tlačítko **nový**->**App Services**->**Service Bus**  -> **Centra událostí**->**rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b9c41-109">toocreate a new Event Hub, sign-in toohello [Azure classic portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Service Bus**->**Event Hub**->**Quick Create**.</span></span> <span data-ttu-id="b9c41-110">Zadejte název centra událostí, oblast, vyberte předplatné a vyberte obor názvů.</span><span class="sxs-lookup"><span data-stu-id="b9c41-110">Enter an Event Hub name, region, select a subscription, and select a namespace.</span></span> <span data-ttu-id="b9c41-111">Pokud jste dosud nevytvořili oboru názvů můžete vytvořit jeden zadáním názvu do hello **Namespace** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b9c41-111">If you haven't previously created a namespace you can create one by typing a name in hello **Namespace** textbox.</span></span> <span data-ttu-id="b9c41-112">Po nakonfigurování všech vlastností klikněte na tlačítko **vytvoření nového centra událostí** toocreate hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="b9c41-112">Once all properties are configured, click **Create a new Event Hub** toocreate hello Event Hub.</span></span>

![Vytvoření centra událostí][create-event-hub]

<span data-ttu-id="b9c41-114">Dále přejděte toohello **konfigurace** kartě nového centra událostí a vytvořte dvě **sdílené zásady přístupu**.</span><span class="sxs-lookup"><span data-stu-id="b9c41-114">Next, navigate toohello **Configure** tab for your new Event Hub and create two **shared access policies**.</span></span> <span data-ttu-id="b9c41-115">Název první hello **odesílající** a pojmenujte ho **odeslat** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="b9c41-115">Name hello first one **Sending** and give it **Send** permissions.</span></span>

![Odesílání zásad][sending-policy]

<span data-ttu-id="b9c41-117">Název hello druhá **přijetí**, poskytněte **naslouchání** oprávnění a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b9c41-117">Name hello second one **Receiving**, give it **Listen** permissions, and click **Save**.</span></span>

![Přijetí zásad][receiving-policy]

<span data-ttu-id="b9c41-119">Každá zásada sdíleného přístupu umožňuje toosend aplikace a přijímat události tooand z hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="b9c41-119">Each shared access policy allows applications toosend and receive events tooand from hello Event Hub.</span></span> <span data-ttu-id="b9c41-120">tooaccess hello připojovací řetězce pro tyto zásady, přejděte toohello **řídicí panel** kartě hello centra událostí a klikněte na tlačítko **informace o připojení**.</span><span class="sxs-lookup"><span data-stu-id="b9c41-120">tooaccess hello connection strings for these policies, navigate toohello **Dashboard** tab of hello Event Hub and click **Connection information**.</span></span>

![Připojovací řetězec][event-hub-dashboard]

<span data-ttu-id="b9c41-122">Hello **odesílající** připojovací řetězec se používá při protokolování událostí a hello **přijetí** připojovací řetězec se používá při stahování události z hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="b9c41-122">hello **Sending** connection string is used when logging events, and hello **Receiving** connection string is used when downloading events from hello Event Hub.</span></span>

![Připojovací řetězec][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a><span data-ttu-id="b9c41-124">Vytvoření protokolovač API Management</span><span class="sxs-lookup"><span data-stu-id="b9c41-124">Create an API Management logger</span></span>
<span data-ttu-id="b9c41-125">Teď, když máte centra událostí, je dalším krokem hello tooconfigure [Protokolovač](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) ve službě API Management služby tak, aby mohl zaprotokolovat události toohello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="b9c41-125">Now that you have an Event Hub, hello next step is tooconfigure a [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) in your API Management service so that it can log events toohello Event Hub.</span></span>

<span data-ttu-id="b9c41-126">Protokolovací nástroje API Management jsou konfigurováni pomocí hello [rozhraní API REST API správy](http://aka.ms/smapi).</span><span class="sxs-lookup"><span data-stu-id="b9c41-126">API Management loggers are configured using hello [API Management REST API](http://aka.ms/smapi).</span></span> <span data-ttu-id="b9c41-127">Před použitím hello REST API pro hello poprvé, přečtěte si hello [požadavky](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) a ujistěte se, že máte [povolen přístup toohello REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span><span class="sxs-lookup"><span data-stu-id="b9c41-127">Before using hello REST API for hello first time, review hello [prerequisites](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) and ensure that you have [enabled access toohello REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span></span>

<span data-ttu-id="b9c41-128">toocreate protokolovacího nástroje, ujistěte se, požadavek HTTP PUT pomocí hello následující adresu URL šablony.</span><span class="sxs-lookup"><span data-stu-id="b9c41-128">toocreate a logger, make an HTTP PUT request using hello following URL template.</span></span>

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* <span data-ttu-id="b9c41-129">Nahraďte `{your service}` s názvem hello instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="b9c41-129">Replace `{your service}` with hello name of your API Management service instance.</span></span>
* <span data-ttu-id="b9c41-130">Nahraďte `{new logger name}` s hello požadovaný název pro vaše nové protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="b9c41-130">Replace `{new logger name}` with hello desired name for your new logger.</span></span> <span data-ttu-id="b9c41-131">Tento název bude odkazovat, když konfigurujete hello [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) zásad</span><span class="sxs-lookup"><span data-stu-id="b9c41-131">You will reference this name when you configure hello [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) policy</span></span>

<span data-ttu-id="b9c41-132">Přidejte následující hlavičky požadavku toohello hello.</span><span class="sxs-lookup"><span data-stu-id="b9c41-132">Add hello following headers toohello request.</span></span>

* <span data-ttu-id="b9c41-133">Content-Type: application/json</span><span class="sxs-lookup"><span data-stu-id="b9c41-133">Content-Type : application/json</span></span>
* <span data-ttu-id="b9c41-134">Autorizace: SharedAccessSignature 58...</span><span class="sxs-lookup"><span data-stu-id="b9c41-134">Authorization : SharedAccessSignature 58...</span></span>
  * <span data-ttu-id="b9c41-135">Pokyny pro generování hello `SharedAccessSignature` najdete v části [Azure API Management REST API ověřování](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span><span class="sxs-lookup"><span data-stu-id="b9c41-135">For instructions on generating hello `SharedAccessSignature` see [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span></span>

<span data-ttu-id="b9c41-136">Zadejte text žádosti hello pomocí následující šablony hello.</span><span class="sxs-lookup"><span data-stu-id="b9c41-136">Specify hello request body using hello following template.</span></span>

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

* <span data-ttu-id="b9c41-137">`type`musí být nastaven příliš`AzureEventHub`.</span><span class="sxs-lookup"><span data-stu-id="b9c41-137">`type` must be set too`AzureEventHub`.</span></span>
* <span data-ttu-id="b9c41-138">`description`poskytuje volitelný popis hello protokoly a v případě potřeby může být řetězec nulové délky.</span><span class="sxs-lookup"><span data-stu-id="b9c41-138">`description` provides an optional description of hello logger and can be a zero length string if desired.</span></span>
* <span data-ttu-id="b9c41-139">`credentials`obsahuje hello `name` a `connectionString` centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="b9c41-139">`credentials` contains hello `name` and `connectionString` of your Azure Event Hub.</span></span>

<span data-ttu-id="b9c41-140">Pokud vytvoříte požadavek hello, pokud hello protokoly se vytvoří stavový kód `201 Created` je vrácen.</span><span class="sxs-lookup"><span data-stu-id="b9c41-140">When you make hello request, if hello logger is created a status code of `201 Created` is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="b9c41-141">Ostatní možné návratové kódy a jejich důvodů najdete v tématu [vytvořit protokoly](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span><span class="sxs-lookup"><span data-stu-id="b9c41-141">For other possible return codes and their reasons, see [Create a Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span></span> <span data-ttu-id="b9c41-142">toosee jak provádět další operace, například seznam, aktualizace a odstranění, najdete v části hello [Protokolovač](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) dokumentace entity.</span><span class="sxs-lookup"><span data-stu-id="b9c41-142">toosee how perform other operations such as list, update, and delete, see hello [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entity documentation.</span></span>
>
>

## <a name="configure-log-to-eventhubs-policies"></a><span data-ttu-id="b9c41-143">Konfigurace zásad protokolu eventhubs</span><span class="sxs-lookup"><span data-stu-id="b9c41-143">Configure log-to-eventhubs policies</span></span>
<span data-ttu-id="b9c41-144">Jakmile vaše protokoly nakonfigurovaný ve službě API Management, můžete nakonfigurovat zásady protokolu eventhubs toolog hello potřeby události.</span><span class="sxs-lookup"><span data-stu-id="b9c41-144">Once your logger is configured in API Management, you can configure your log-to-eventhubs policies toolog hello desired events.</span></span> <span data-ttu-id="b9c41-145">Hello protokolu eventhubs zásadu lze použít buď hello oddílu zásad příchozí nebo odchozí zásady hello.</span><span class="sxs-lookup"><span data-stu-id="b9c41-145">hello log-to-eventhubs policy can be used in either hello inbound policy section or hello outbound policy section.</span></span>

<span data-ttu-id="b9c41-146">tooconfigure zásady, přihlášení toohello [portál Azure](https://portal.azure.com), přejděte tooyour služba API Management a klikněte na tlačítko **portál vydavatele** tooaccess hello portál vydavatele.</span><span class="sxs-lookup"><span data-stu-id="b9c41-146">tooconfigure policies, sign-in toohello [Azure portal](https://portal.azure.com), navigate tooyour API Management service, and click **Publisher portal** tooaccess hello publisher portal.</span></span>

![Portál vydavatele][publisher-portal]

<span data-ttu-id="b9c41-148">Klikněte na tlačítko **zásady** v nabídce hello API Management na levé straně hello, vyberte požadovaný produkt hello a rozhraní API a klikněte na **přidat zásadu**.</span><span class="sxs-lookup"><span data-stu-id="b9c41-148">Click **Policies** in hello API Management menu on hello left, select hello desired product and API, and click **Add policy**.</span></span> <span data-ttu-id="b9c41-149">V tomto příkladu je právě přidávána zásad toohello **Echo API** v hello **neomezený** produktu.</span><span class="sxs-lookup"><span data-stu-id="b9c41-149">In this example we're adding a policy toohello **Echo API** in hello **Unlimited** product.</span></span>

![Přidání zásad][add-policy]

<span data-ttu-id="b9c41-151">Umístěte kurzor v hello `inbound` zásad a klikněte na hello **protokolu tooEventHub** zásad tooinsert hello `log-to-eventhub` příkaz šablony zásad.</span><span class="sxs-lookup"><span data-stu-id="b9c41-151">Position your cursor in hello `inbound` policy section and click hello **Log tooEventHub** policy tooinsert hello `log-to-eventhub` policy statement template.</span></span>

![Editor zásad][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

<span data-ttu-id="b9c41-153">Nahraďte `logger-id` hello název protokolovacího nástroje API Management hello, které jste nakonfigurovali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="b9c41-153">Replace `logger-id` with hello name of hello API Management logger you configured in hello previous step.</span></span>

<span data-ttu-id="b9c41-154">Můžete použít libovolný výraz, který vrací řetězec jako hodnotu hello hello `log-to-eventhub` elementu.</span><span class="sxs-lookup"><span data-stu-id="b9c41-154">You can use any expression that returns a string as hello value for hello `log-to-eventhub` element.</span></span> <span data-ttu-id="b9c41-155">V tomto příkladu se zaznamená řetězec obsahující hello datum a čas, název služby, id požadavku, požadavek ip adresu a název operace.</span><span class="sxs-lookup"><span data-stu-id="b9c41-155">In this example a string containing hello date and time, service name, request id, request ip address, and operation name is logged.</span></span>

<span data-ttu-id="b9c41-156">Klikněte na tlačítko **Uložit** toosave hello aktualizovat konfiguraci zásad.</span><span class="sxs-lookup"><span data-stu-id="b9c41-156">Click **Save** toosave hello updated policy configuration.</span></span> <span data-ttu-id="b9c41-157">Jakmile je uložen hello zásad je aktivní a události jsou zaznamenané toohello určené centra událostí.</span><span class="sxs-lookup"><span data-stu-id="b9c41-157">As soon as it is saved hello policy is active and events are logged toohello designated Event Hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9c41-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b9c41-158">Next steps</span></span>
* <span data-ttu-id="b9c41-159">Další informace o Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b9c41-159">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="b9c41-160">Začínáme s Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b9c41-160">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="b9c41-161">Přijímat zprávy pomocí třídy EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="b9c41-161">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="b9c41-162">Průvodce programováním pro službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b9c41-162">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="b9c41-163">Další informace o integraci API Management a služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b9c41-163">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="b9c41-164">Odkaz na entitu protokolovacího nástroje</span><span class="sxs-lookup"><span data-stu-id="b9c41-164">Logger entity reference</span></span>](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [<span data-ttu-id="b9c41-165">referenční informace o protokolu eventhub zásad</span><span class="sxs-lookup"><span data-stu-id="b9c41-165">log-to-eventhub policy reference</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [<span data-ttu-id="b9c41-166">Sledovat vaše rozhraní API s Azure API Management, Event Hubs a Runscope</span><span class="sxs-lookup"><span data-stu-id="b9c41-166">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a><span data-ttu-id="b9c41-167">Podívejte se na video s návodem</span><span class="sxs-lookup"><span data-stu-id="b9c41-167">Watch a video walkthrough</span></span>
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
