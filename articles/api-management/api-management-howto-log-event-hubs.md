---
title: "Jak zapisovat do protokolu událostí Azure Event Hubs ve službě Azure API Management | Microsoft Docs"
description: "Zjistěte, jak do protokolu událostí Azure Event Hubs ve službě Azure API Management."
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
ms.openlocfilehash: a310236179677046ec49930b07cfdffdadc37974
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a><span data-ttu-id="3c74a-103">Jak zapisovat do protokolu událostí Azure Event Hubs ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="3c74a-103">How to log events to Azure Event Hubs in Azure API Management</span></span>
<span data-ttu-id="3c74a-104">Vysoce škálovatelná služba Azure Event Hubs slouží ke zpracování příchozích dat. Dokáže přijímat miliony událostí za sekundu a umožňuje zpracovávat a analyzovat masivní objemy dat vytvářených zařízeními a aplikacemi připojenými k vaší síti.</span><span class="sxs-lookup"><span data-stu-id="3c74a-104">Azure Event Hubs is a highly scalable data ingress service that can ingest millions of events per second so that you can process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="3c74a-105">Služba Event Hubs slouží jako "přední dveře" pro kanál událostí, a jakmile jsou data shromážděna do centra událostí, lze je transformovat a uložené pomocí kteréhokoli poskytovatele služeb, analýzu v reálném čase nebo adaptérů pro dávkování či ukládání.</span><span class="sxs-lookup"><span data-stu-id="3c74a-105">Event Hubs acts as the "front door" for an event pipeline, and once data is collected into an event hub, it can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="3c74a-106">Event Hubs oddělí vytvoření proudu událostí od spotřeby těchto události, aby spotřebitelé událostí mohli k událostem přistupovat podle svého vlastního plánu.</span><span class="sxs-lookup"><span data-stu-id="3c74a-106">Event Hubs decouples the production of a stream of events from the consumption of those events, so that event consumers can access the events on their own schedule.</span></span>

<span data-ttu-id="3c74a-107">Tento článek je Pomocníka pro [integrovat Azure API Management službou Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video a popisuje, jak do protokolu událostí správy rozhraní API pomocí Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="3c74a-107">This article is a companion to the [Integrate Azure API Management with Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video and describes how to log API Management events using Azure Event Hubs.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="3c74a-108">Vytvoření centra událostí Azure</span><span class="sxs-lookup"><span data-stu-id="3c74a-108">Create an Azure Event Hub</span></span>
<span data-ttu-id="3c74a-109">K vytvoření nového centra událostí, přihlaste se do [portál Azure classic](https://manage.windowsazure.com) a klikněte na tlačítko **nový**->**App Services**->**Service Bus**  -> **Centra událostí**->**rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3c74a-109">To create a new Event Hub, sign-in to the [Azure classic portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Service Bus**->**Event Hub**->**Quick Create**.</span></span> <span data-ttu-id="3c74a-110">Zadejte název centra událostí, oblast, vyberte předplatné a vyberte obor názvů.</span><span class="sxs-lookup"><span data-stu-id="3c74a-110">Enter an Event Hub name, region, select a subscription, and select a namespace.</span></span> <span data-ttu-id="3c74a-111">Pokud jste dosud nevytvořili oboru názvů můžete vytvořit jeden zadáním názvu do **Namespace** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3c74a-111">If you haven't previously created a namespace you can create one by typing a name in the **Namespace** textbox.</span></span> <span data-ttu-id="3c74a-112">Po nakonfigurování všech vlastností klikněte na tlačítko **vytvoření nového centra událostí** k vytvoření centra událostí.</span><span class="sxs-lookup"><span data-stu-id="3c74a-112">Once all properties are configured, click **Create a new Event Hub** to create the Event Hub.</span></span>

![Vytvoření centra událostí][create-event-hub]

<span data-ttu-id="3c74a-114">Potom přejděte na **konfigurace** kartě nového centra událostí a vytvořte dvě **sdílené zásady přístupu**.</span><span class="sxs-lookup"><span data-stu-id="3c74a-114">Next, navigate to the **Configure** tab for your new Event Hub and create two **shared access policies**.</span></span> <span data-ttu-id="3c74a-115">Název první **odesílající** a pojmenujte ho **odeslat** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="3c74a-115">Name the first one **Sending** and give it **Send** permissions.</span></span>

![Odesílání zásad][sending-policy]

<span data-ttu-id="3c74a-117">Název na druhou **přijetí**, poskytněte **naslouchání** oprávnění a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3c74a-117">Name the second one **Receiving**, give it **Listen** permissions, and click **Save**.</span></span>

![Přijetí zásad][receiving-policy]

<span data-ttu-id="3c74a-119">Každá zásada sdíleného přístupu umožňuje aplikacím odesílání a příjmu událostí do a z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="3c74a-119">Each shared access policy allows applications to send and receive events to and from the Event Hub.</span></span> <span data-ttu-id="3c74a-120">Chcete-li získat přístup k připojovací řetězce pro tyto zásady, přejděte na **řídicí panel** centra událostí a klikněte na kartu **informace o připojení**.</span><span class="sxs-lookup"><span data-stu-id="3c74a-120">To access the connection strings for these policies, navigate to the **Dashboard** tab of the Event Hub and click **Connection information**.</span></span>

![Připojovací řetězec][event-hub-dashboard]

<span data-ttu-id="3c74a-122">**Odesílající** připojovací řetězec se používá při protokolování událostí a **přijetí** připojovací řetězec se používá při stahování události z centra událostí.</span><span class="sxs-lookup"><span data-stu-id="3c74a-122">The **Sending** connection string is used when logging events, and the **Receiving** connection string is used when downloading events from the Event Hub.</span></span>

![Připojovací řetězec][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a><span data-ttu-id="3c74a-124">Vytvoření protokolovač API Management</span><span class="sxs-lookup"><span data-stu-id="3c74a-124">Create an API Management logger</span></span>
<span data-ttu-id="3c74a-125">Teď, když máte centra událostí, dalším krokem je konfigurace [Protokolovač](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) ve službě API Management služby tak, aby mohl zaprotokolovat události do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="3c74a-125">Now that you have an Event Hub, the next step is to configure a [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) in your API Management service so that it can log events to the Event Hub.</span></span>

<span data-ttu-id="3c74a-126">Protokolovací nástroje API Management jsou konfigurováni pomocí [rozhraní API REST API správy](http://aka.ms/smapi).</span><span class="sxs-lookup"><span data-stu-id="3c74a-126">API Management loggers are configured using the [API Management REST API](http://aka.ms/smapi).</span></span> <span data-ttu-id="3c74a-127">Před použitím rozhraní REST API poprvé, přečtěte si [požadavky](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) a ujistěte se, že máte [povolen přístup k rozhraní REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span><span class="sxs-lookup"><span data-stu-id="3c74a-127">Before using the REST API for the first time, review the [prerequisites](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) and ensure that you have [enabled access to the REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span></span>

<span data-ttu-id="3c74a-128">Pokud chcete vytvořit protokoly, ujistěte se, požadavek HTTP PUT pomocí následující šablony adresy URL.</span><span class="sxs-lookup"><span data-stu-id="3c74a-128">To create a logger, make an HTTP PUT request using the following URL template.</span></span>

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* <span data-ttu-id="3c74a-129">Nahraďte `{your service}` s názvem vaší instance služby API Management.</span><span class="sxs-lookup"><span data-stu-id="3c74a-129">Replace `{your service}` with the name of your API Management service instance.</span></span>
* <span data-ttu-id="3c74a-130">Nahraďte `{new logger name}` s požadovaným názvem pro vaše nové protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="3c74a-130">Replace `{new logger name}` with the desired name for your new logger.</span></span> <span data-ttu-id="3c74a-131">Tento název bude odkazovat, když konfigurujete [protokolu eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) zásad</span><span class="sxs-lookup"><span data-stu-id="3c74a-131">You will reference this name when you configure the [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) policy</span></span>

<span data-ttu-id="3c74a-132">Přidáte následující hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="3c74a-132">Add the following headers to the request.</span></span>

* <span data-ttu-id="3c74a-133">Content-Type: application/json</span><span class="sxs-lookup"><span data-stu-id="3c74a-133">Content-Type : application/json</span></span>
* <span data-ttu-id="3c74a-134">Autorizace: SharedAccessSignature 58...</span><span class="sxs-lookup"><span data-stu-id="3c74a-134">Authorization : SharedAccessSignature 58...</span></span>
  * <span data-ttu-id="3c74a-135">Pokyny pro generování `SharedAccessSignature` najdete v části [Azure API Management REST API ověřování](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span><span class="sxs-lookup"><span data-stu-id="3c74a-135">For instructions on generating the `SharedAccessSignature` see [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span></span>

<span data-ttu-id="3c74a-136">Zadejte text žádosti pomocí následující šablony.</span><span class="sxs-lookup"><span data-stu-id="3c74a-136">Specify the request body using the following template.</span></span>

```json
{
  "type" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* <span data-ttu-id="3c74a-137">`type`musí být nastavena na `AzureEventHub`.</span><span class="sxs-lookup"><span data-stu-id="3c74a-137">`type` must be set to `AzureEventHub`.</span></span>
* <span data-ttu-id="3c74a-138">`description`poskytuje volitelný popis protokolovacího nástroje a v případě potřeby může být řetězec nulové délky.</span><span class="sxs-lookup"><span data-stu-id="3c74a-138">`description` provides an optional description of the logger and can be a zero length string if desired.</span></span>
* <span data-ttu-id="3c74a-139">`credentials`obsahuje `name` a `connectionString` centra událostí Azure.</span><span class="sxs-lookup"><span data-stu-id="3c74a-139">`credentials` contains the `name` and `connectionString` of your Azure Event Hub.</span></span>

<span data-ttu-id="3c74a-140">Pokud vytvoříte požadavek, je-li protokolovač vytvořena stavový kód `201 Created` je vrácen.</span><span class="sxs-lookup"><span data-stu-id="3c74a-140">When you make the request, if the logger is created a status code of `201 Created` is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="3c74a-141">Ostatní možné návratové kódy a jejich důvodů najdete v tématu [vytvořit protokoly](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span><span class="sxs-lookup"><span data-stu-id="3c74a-141">For other possible return codes and their reasons, see [Create a Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span></span> <span data-ttu-id="3c74a-142">Zobrazíte jak provádět další operace, jako je například seznam, aktualizace a odstranění, najdete [protokolovacího nástroje](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) dokumentace entity.</span><span class="sxs-lookup"><span data-stu-id="3c74a-142">To see how perform other operations such as list, update, and delete, see the [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entity documentation.</span></span>
>
>

## <a name="configure-log-to-eventhubs-policies"></a><span data-ttu-id="3c74a-143">Konfigurace zásad protokolu eventhubs</span><span class="sxs-lookup"><span data-stu-id="3c74a-143">Configure log-to-eventhubs policies</span></span>
<span data-ttu-id="3c74a-144">Jakmile vaše protokoly nakonfigurovaný ve službě API Management, můžete nakonfigurovat zásad protokolu eventhubs k požadované událostem protokolu.</span><span class="sxs-lookup"><span data-stu-id="3c74a-144">Once your logger is configured in API Management, you can configure your log-to-eventhubs policies to log the desired events.</span></span> <span data-ttu-id="3c74a-145">Zásady protokolu eventhubs lze použít v části zásady příchozí nebo odchozí zásad.</span><span class="sxs-lookup"><span data-stu-id="3c74a-145">The log-to-eventhubs policy can be used in either the inbound policy section or the outbound policy section.</span></span>

<span data-ttu-id="3c74a-146">Ke konfiguraci zásad, přihlaste se do [portál Azure](https://portal.azure.com), přejděte do služby API Management a klikněte na tlačítko **portál vydavatele** pro přístup k portálu vydavatele.</span><span class="sxs-lookup"><span data-stu-id="3c74a-146">To configure policies, sign-in to the [Azure portal](https://portal.azure.com), navigate to your API Management service, and click **Publisher portal** to access the publisher portal.</span></span>

![Portál vydavatele][publisher-portal]

<span data-ttu-id="3c74a-148">Klikněte na tlačítko **zásady** v nabídce API Management na levé straně vyberte požadovaný produkt a rozhraní API a klikněte na tlačítko **přidat zásadu**.</span><span class="sxs-lookup"><span data-stu-id="3c74a-148">Click **Policies** in the API Management menu on the left, select the desired product and API, and click **Add policy**.</span></span> <span data-ttu-id="3c74a-149">V tomto příkladu je právě přidávána zásadu, která **Echo API** v **neomezený** produktu.</span><span class="sxs-lookup"><span data-stu-id="3c74a-149">In this example we're adding a policy to the **Echo API** in the **Unlimited** product.</span></span>

![Přidání zásad][add-policy]

<span data-ttu-id="3c74a-151">Umístěte kurzor v `inbound` zásad a klikněte na tlačítko **protokolu k centru EventHub** zásady a vložit `log-to-eventhub` příkaz šablony zásad.</span><span class="sxs-lookup"><span data-stu-id="3c74a-151">Position your cursor in the `inbound` policy section and click the **Log to EventHub** policy to insert the `log-to-eventhub` policy statement template.</span></span>

![Editor zásad][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

<span data-ttu-id="3c74a-153">Nahraďte `logger-id` s názvem protokolovacího nástroje správy rozhraní API, které jste nakonfigurovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3c74a-153">Replace `logger-id` with the name of the API Management logger you configured in the previous step.</span></span>

<span data-ttu-id="3c74a-154">Můžete použít libovolný výraz, který vrací řetězec jako hodnotu `log-to-eventhub` elementu.</span><span class="sxs-lookup"><span data-stu-id="3c74a-154">You can use any expression that returns a string as the value for the `log-to-eventhub` element.</span></span> <span data-ttu-id="3c74a-155">V tomto příkladu se zaznamená řetězec obsahující datum a čas, název služby, id požadavku, požadavek ip adresu a název operace.</span><span class="sxs-lookup"><span data-stu-id="3c74a-155">In this example a string containing the date and time, service name, request id, request ip address, and operation name is logged.</span></span>

<span data-ttu-id="3c74a-156">Klikněte na tlačítko **Uložit** aktualizované zásady konfiguraci uložíte.</span><span class="sxs-lookup"><span data-stu-id="3c74a-156">Click **Save** to save the updated policy configuration.</span></span> <span data-ttu-id="3c74a-157">Jakmile je uložen zásady nejsou aktivní a do určené centra událostí jsou zaznamenány události.</span><span class="sxs-lookup"><span data-stu-id="3c74a-157">As soon as it is saved the policy is active and events are logged to the designated Event Hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c74a-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c74a-158">Next steps</span></span>
* <span data-ttu-id="3c74a-159">Další informace o Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3c74a-159">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="3c74a-160">Začínáme s Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3c74a-160">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="3c74a-161">Přijímat zprávy pomocí třídy EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="3c74a-161">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="3c74a-162">Průvodce programováním pro službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3c74a-162">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="3c74a-163">Další informace o integraci API Management a služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3c74a-163">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="3c74a-164">Odkaz na entitu protokolovacího nástroje</span><span class="sxs-lookup"><span data-stu-id="3c74a-164">Logger entity reference</span></span>](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [<span data-ttu-id="3c74a-165">referenční informace o protokolu eventhub zásad</span><span class="sxs-lookup"><span data-stu-id="3c74a-165">log-to-eventhub policy reference</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [<span data-ttu-id="3c74a-166">Sledovat vaše rozhraní API s Azure API Management, Event Hubs a Runscope</span><span class="sxs-lookup"><span data-stu-id="3c74a-166">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a><span data-ttu-id="3c74a-167">Podívejte se na video s návodem</span><span class="sxs-lookup"><span data-stu-id="3c74a-167">Watch a video walkthrough</span></span>
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
