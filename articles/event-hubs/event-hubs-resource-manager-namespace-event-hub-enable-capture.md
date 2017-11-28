---
title: "aaaCreate oboru názvů Azure Event Hubs a povolit zachytit pomocí šablony | Microsoft Docs"
description: "Vytvoření oboru názvů Azure Event Hubs s jedním centrem událostí a povolení funkce Capture pomocí šablony Azure Resource Manageru"
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 8bdda6a2-5ff1-45e3-b696-c553768f1090
ms.service: event-hubs
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: a43b4e8d690ae825047e8a9d609bfda89cf2a06f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-with-an-event-hub-and-enable-capture-using-an-azure-resource-manager-template"></a><span data-ttu-id="e8fcc-103">Vytvoření oboru názvů Event Hubs s centrem událostí a povolení funkce Capture pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e8fcc-103">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>

<span data-ttu-id="e8fcc-104">Tento článek ukazuje, jak toouse šablonu Azure Resource Manager, který vytvořil na obor názvů služby Event Hubs s instanci rozbočovače jedné události a také umožňuje hello [funkci zachycení](event-hubs-capture-overview.md) na hello centra událostí.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-104">This article shows how toouse an Azure Resource Manager template that creates an Event Hubs namespace, with one event hub instance, and also enables hello [Capture feature](event-hubs-capture-overview.md) on hello event hub.</span></span> <span data-ttu-id="e8fcc-105">Hello článek popisuje, jak toodefine prostředky, ke kterým jsou nasazeny, a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-105">hello article describes how toodefine which resources are deployed, and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="e8fcc-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="e8fcc-107">Tento článek také ukazuje, jak toospecify, jsou do objektů BLOB služby Azure Storage nebo Azure Data Lake Store, zachycení událostí podle hello cílové zvolíte.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-107">This article also shows how toospecify that events are captured into either Azure Storage Blobs or an Azure Data Lake Store, based on hello destination you choose.</span></span>

<span data-ttu-id="e8fcc-108">Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="e8fcc-108">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="e8fcc-109">Další informace o vzorech a postupech pro zásady vytváření názvů prostředků Azure najdete v tématu [Zásady vytváření názvů prostředků Azure][Azure Resources naming conventions].</span><span class="sxs-lookup"><span data-stu-id="e8fcc-109">For more information about patterns and practices for Azure Resources naming conventions, see [Azure Resources naming conventions][Azure Resources naming conventions].</span></span>

<span data-ttu-id="e8fcc-110">Pro dokončení šablony hello klikněte na tlačítko hello následující odkazy Githubu:</span><span class="sxs-lookup"><span data-stu-id="e8fcc-110">For hello complete templates, click hello following GitHub links:</span></span>

- <span data-ttu-id="e8fcc-111">[Události rozbočovače a povolit zachycení tooStorage šablony][Event Hub and enable Capture tooStorage template]</span><span class="sxs-lookup"><span data-stu-id="e8fcc-111">[Event hub and enable Capture tooStorage template][Event Hub and enable Capture tooStorage template]</span></span> 
- <span data-ttu-id="e8fcc-112">[Rozbočovače a povolit zachycení tooAzure Data Lake Store šablony události][Event Hub and enable Capture tooAzure Data Lake Store template]</span><span class="sxs-lookup"><span data-stu-id="e8fcc-112">[Event hub and enable Capture tooAzure Data Lake Store template][Event Hub and enable Capture tooAzure Data Lake Store template]</span></span>

> [!NOTE]
> <span data-ttu-id="e8fcc-113">toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-113">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="e8fcc-114">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="e8fcc-114">What will you deploy?</span></span>

<span data-ttu-id="e8fcc-115">Pomocí této šablony nasadíte obor názvů Event Hubs s centrem událostí a povolíte funkci [Event Hubs Capture](event-hubs-capture-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e8fcc-115">With this template, you deploy an Event Hubs namespace with an event hub, and also enable [Event Hubs Capture](event-hubs-capture-overview.md).</span></span>

<span data-ttu-id="e8fcc-116">[Služba Event Hubs](event-hubs-what-is-event-hubs.md) zpracovává používanou tooprovide událostí a telemetrie příjem příchozích dat tooAzure v masivním měřítku, s nízkou latencí a vysokou spolehlivostí události.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-116">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used tooprovide event and telemetry ingress tooAzure at massive scale, with low latency and high reliability.</span></span> <span data-ttu-id="e8fcc-117">Umožňuje zaznamenat centra událostí můžete tooautomatically poskytovat hello streamování dat ve službě Event Hubs tooAzure Blob storage nebo Azure Data Lake Store v rámci určeného časového nebo velikost intervalu dle vlastního výběru.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-117">Event Hubs Capture enables you tooautomatically deliver hello streaming data in Event Hubs tooAzure Blob storage or Azure Data Lake Store, within a specified time or size interval of your choosing.</span></span>

<span data-ttu-id="e8fcc-118">Kliknutím na následující tlačítko tooenable zaznamenat centra událostí do Azure Storage hello:</span><span class="sxs-lookup"><span data-stu-id="e8fcc-118">Click hello following button tooenable Event Hubs Capture into Azure Storage:</span></span>

<span data-ttu-id="e8fcc-119">[![Nasazení tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e8fcc-119">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span></span>

<span data-ttu-id="e8fcc-120">Kliknutím na následující tlačítko tooenable zaznamenat centra událostí do Azure Data Lake Store hello:</span><span class="sxs-lookup"><span data-stu-id="e8fcc-120">Click hello following button tooenable Event Hubs Capture into Azure Data Lake Store:</span></span>

<span data-ttu-id="e8fcc-121">[![Nasazení tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e8fcc-121">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="e8fcc-122">Parametry</span><span class="sxs-lookup"><span data-stu-id="e8fcc-122">Parameters</span></span>

<span data-ttu-id="e8fcc-123">S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-123">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="e8fcc-124">Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-124">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="e8fcc-125">Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-125">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="e8fcc-126">Parametry nedefinuje pro hello hodnoty, které vždy zůstávají stejné.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-126">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="e8fcc-127">Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-127">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="e8fcc-128">Šablona Hello definuje hello následující parametry.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-128">hello template defines hello following parameters.</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="e8fcc-129">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="e8fcc-129">eventHubNamespaceName</span></span>

<span data-ttu-id="e8fcc-130">Název Hello hello [oboru názvů služby Event Hubs](event-hubs-create.md) toocreate.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-130">hello name of hello [Event Hubs namespace](event-hubs-create.md) toocreate.</span></span>

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of hello EventHub namespace"
      }
}
```

### <a name="eventhubname"></a><span data-ttu-id="e8fcc-131">eventHubName</span><span class="sxs-lookup"><span data-stu-id="e8fcc-131">eventHubName</span></span>

<span data-ttu-id="e8fcc-132">název centra událostí hello vytvořené v hello Hello [oboru názvů služby Event Hubs](event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="e8fcc-132">hello name of hello event hub created in hello [Event Hubs namespace](event-hubs-create.md).</span></span>

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of hello event hub"
    }
}
```

### <a name="messageretentionindays"></a><span data-ttu-id="e8fcc-133">messageRetentionInDays</span><span class="sxs-lookup"><span data-stu-id="e8fcc-133">messageRetentionInDays</span></span>

<span data-ttu-id="e8fcc-134">Hello počet dní tooretain hello zpráv v Centru událostí hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-134">hello number of days tooretain hello messages in hello event hub.</span></span> 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long tooretain hello data in event hub"
     }
 }
```

### <a name="partitioncount"></a><span data-ttu-id="e8fcc-135">partitionCount</span><span class="sxs-lookup"><span data-stu-id="e8fcc-135">partitionCount</span></span>

<span data-ttu-id="e8fcc-136">Hello počet toocreate oddíly v Centru událostí hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-136">hello number of partitions toocreate in hello event hub.</span></span>

```json
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="captureenabled"></a><span data-ttu-id="e8fcc-137">captureEnabled</span><span class="sxs-lookup"><span data-stu-id="e8fcc-137">captureEnabled</span></span>

<span data-ttu-id="e8fcc-138">Povolte zachycení v Centru událostí hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-138">Enable Capture on hello event hub.</span></span>

```json
"captureEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable hello Capture for your event hub"
    }
 }
```
### <a name="captureencodingformat"></a><span data-ttu-id="e8fcc-139">captureEncodingFormat</span><span class="sxs-lookup"><span data-stu-id="e8fcc-139">captureEncodingFormat</span></span>

<span data-ttu-id="e8fcc-140">Formát kódování Hello zadejte data události tooserialize hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-140">hello encoding format you specify tooserialize hello event data.</span></span>

```json
"captureEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"hello encoding format in which Capture serializes hello EventData"
    }
}
```

### <a name="capturetime"></a><span data-ttu-id="e8fcc-141">captureTime</span><span class="sxs-lookup"><span data-stu-id="e8fcc-141">captureTime</span></span>

<span data-ttu-id="e8fcc-142">Hello časový interval, ve kterém zaznamenat centra událostí spustí zaznamenání dat hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-142">hello time interval in which Event Hubs Capture starts capturing hello data.</span></span>

```json
"captureTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"hello time window in seconds for hello capture"
    }
}
```

### <a name="capturesize"></a><span data-ttu-id="e8fcc-143">captureSize</span><span class="sxs-lookup"><span data-stu-id="e8fcc-143">captureSize</span></span>
<span data-ttu-id="e8fcc-144">interval velikost Hello, od kterého začne zachycení zaznamenání dat hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-144">hello size interval at which Capture starts capturing hello data.</span></span>

```json
"captureSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"hello size window in bytes for capture"
    }
}
```

###<a name="capturenameformat"></a><span data-ttu-id="e8fcc-145">captureNameFormat</span><span class="sxs-lookup"><span data-stu-id="e8fcc-145">captureNameFormat</span></span>

<span data-ttu-id="e8fcc-146">Formát názvu Hello používá zaznamenat centra událostí hello toowrite Avro soubory.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-146">hello name format used by Event Hubs Capture toowrite hello Avro files.</span></span> <span data-ttu-id="e8fcc-147">Nezapomeňte, že formát názvu pro funkci Capture musí obsahovat pole `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}` a `{Second}`.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-147">Note that a Capture name format must contain `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}`, and `{Second}` fields.</span></span> <span data-ttu-id="e8fcc-148">Tato pole můžete uspořádat v libovolném pořadí s oddělovači nebo bez.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-148">These can be arranged in any order, with or without delimiters.</span></span>
 
```json
"captureNameFormat": {
      "type": "string",
      "defaultValue": "{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}",
      "metadata": {
        "description": "A Capture Name Format must contain {Namespace}, {EventHub}, {PartitionId}, {Year}, {Month}, {Day}, {Hour}, {Minute} and {Second} fields. These can be arranged in any order with or without delimeters. E.g.  Prod_{EventHub}/{Namespace}\\{PartitionId}_{Year}_{Month}/{Day}/{Hour}/{Minute}/{Second}"
      }
    }
  }
```

### <a name="apiversion"></a><span data-ttu-id="e8fcc-149">apiVersion</span><span class="sxs-lookup"><span data-stu-id="e8fcc-149">apiVersion</span></span>

<span data-ttu-id="e8fcc-150">Hello rozhraní API verze hello šablony.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-150">hello API version of hello template.</span></span>

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by hello template"
    }
 }
```

<span data-ttu-id="e8fcc-151">Pomocí následujících parametrů, pokud se rozhodnete Azure Storage jako cíl hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-151">Use hello following parameters if you choose Azure Storage as your destination.</span></span>

### <a name="destinationstorageaccountresourceid"></a><span data-ttu-id="e8fcc-152">destinationStorageAccountResourceId</span><span class="sxs-lookup"><span data-stu-id="e8fcc-152">destinationStorageAccountResourceId</span></span>

<span data-ttu-id="e8fcc-153">Zachycení vyžaduje Azure Storage účet prostředků ID tooenable zaznamenávání tooyour požadovaného účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-153">Capture requires an Azure Storage account resource ID tooenable capturing tooyour desired Storage account.</span></span>

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing Storage account resource ID where you want hello blobs be captured"
    }
 }
```

### <a name="blobcontainername"></a><span data-ttu-id="e8fcc-154">blobContainerName</span><span class="sxs-lookup"><span data-stu-id="e8fcc-154">blobContainerName</span></span>

<span data-ttu-id="e8fcc-155">Hello kontejneru objektů blob ve které toocapture vaše data události.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-155">hello blob container in which toocapture your event data.</span></span>

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container in which you want hello blobs captured"
    }
}
```

<span data-ttu-id="e8fcc-156">Pomocí následujících parametrů, pokud se rozhodnete Azure Data Lake Store jako cíl hello.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-156">Use hello following parameters if you choose Azure Data Lake Store as your destination.</span></span> <span data-ttu-id="e8fcc-157">Musíte nastavit oprávnění na cestu k Data Lake Store, ve kterém chcete tooCapture hello událost.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-157">You must set permissions on your Data Lake Store path, in which you want tooCapture hello event.</span></span> <span data-ttu-id="e8fcc-158">najdete v části oprávnění tooset [v tomto článku](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="e8fcc-158">tooset permissions, see [this article](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span></span>

###<a name="subscriptionid"></a><span data-ttu-id="e8fcc-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="e8fcc-159">subscriptionId</span></span>

<span data-ttu-id="e8fcc-160">ID předplatného pro obor názvů hello Event Hubs a Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-160">Subscription ID for hello Event Hubs namespace and Azure Data Lake Store.</span></span> <span data-ttu-id="e8fcc-161">Obě tyto prostředky musí být v rámci hello stejné ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-161">Both these resources must be under hello same subscription ID.</span></span>

```json
"subscriptionId": {
    "type": "string",
    "metadata": {
        "description": "Subscription Id of both Azure Data Lake Store and Event Hub namespace"
     }
 }
```

###<a name="datalakeaccountname"></a><span data-ttu-id="e8fcc-162">dataLakeAccountName</span><span class="sxs-lookup"><span data-stu-id="e8fcc-162">dataLakeAccountName</span></span>

<span data-ttu-id="e8fcc-163">Název Azure Data Lake Store Hello hello zaznamenat události.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-163">hello Azure Data Lake Store name for hello captured events.</span></span>

```json
"dataLakeAccountName": {
    "type": "string",
    "metadata": {
        "description": "Azure Data Lake Store name"
    }
}
```

###<a name="datalakefolderpath"></a><span data-ttu-id="e8fcc-164">dataLakeFolderPath</span><span class="sxs-lookup"><span data-stu-id="e8fcc-164">dataLakeFolderPath</span></span>

<span data-ttu-id="e8fcc-165">Cesta k cílové složce Hello pro hello zaznamenat události.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-165">hello destination folder path for hello captured events.</span></span>

```json
"dataLakeFolderPath": {
    "type": "string",
    "metadata": {
        "description": "Destination archive folder path"
    }
}
```

## <a name="resources-toodeploy-for-azure-storage-as-destination-toocaptured-events"></a><span data-ttu-id="e8fcc-166">Toodeploy prostředky pro Azure Storage jako cílový toocaptured události</span><span class="sxs-lookup"><span data-stu-id="e8fcc-166">Resources toodeploy for Azure Storage as destination toocaptured events</span></span>

<span data-ttu-id="e8fcc-167">Vytvoří obor názvů typu **EventHubs**, s rozbočovači jedna událost a také umožňuje zaznamenat tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-167">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture tooAzure Blob Storage.</span></span>

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "CaptureDescription":{
                        "enabled":"[parameters('captureEnabled')]",
                        "encoding":"[parameters('captureEncodingFormat')]",
                        "intervalInSeconds":"[parameters('captureTime')]",
                        "sizeLimitInBytes":"[parameters('captureSize')]",
                        "destination":{
                            "name":"EventHubCapture.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }

               }

            }
         ]
      }
   ]
```

## <a name="resources-toodeploy-for-azure-data-lake-store-as-destination"></a><span data-ttu-id="e8fcc-168">Toodeploy prostředky pro Azure Data Lake Store jako cíl</span><span class="sxs-lookup"><span data-stu-id="e8fcc-168">Resources toodeploy for Azure Data Lake Store as destination</span></span>

<span data-ttu-id="e8fcc-169">Vytvoří obor názvů typu **EventHubs**, s rozbočovači jedna událost a také umožňuje zaznamenat tooAzure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e8fcc-169">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture tooAzure Data Lake Store.</span></span>

```json
 "resources": [
        {
            "apiVersion": "2015-08-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "2015-08-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('eventHubName')]",
                        "ArchiveDescription": {
                            "enabled": "true",
                            "encoding": "[parameters('archiveEncodingFormat')]",
                            "intervalInSeconds": "[parameters('archiveTime')]",
                            "sizeLimitInBytes": "[parameters('archiveSize')]",
                            "destination": {
                                "name": "EventHubArchive.AzureDataLake",
                                "properties": {
                                    "DataLakeSubscriptionId": "[parameters('subscriptionId')]",
                                    "DataLakeAccountName": "[parameters('dataLakeAccountName')]",
                                    "DataLakeFolderPath": "[parameters('dataLakeFolderPath')]",
                                    "ArchiveNameFormat": "[parameters('archiveNameFormat')]"
                                }
                            }
                        }
                    }
                }
            ]
        }
    ]
```

## <a name="commands-toorun-deployment"></a><span data-ttu-id="e8fcc-170">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="e8fcc-170">Commands toorun deployment</span></span>

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="e8fcc-171">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e8fcc-171">PowerShell</span></span>

<span data-ttu-id="e8fcc-172">Nasazení vaší šablony tooenable zaznamenat centra událostí do úložiště Azure:</span><span class="sxs-lookup"><span data-stu-id="e8fcc-172">Deploy your template tooenable Event Hubs Capture into Azure Storage:</span></span>
 
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json
```

<span data-ttu-id="e8fcc-173">Nasazení vaší šablony tooenable zaznamenat centra událostí do Azure Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="e8fcc-173">Deploy your template tooenable Event Hubs Capture into Azure Data Lake Store:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="e8fcc-174">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e8fcc-174">Azure CLI</span></span>

<span data-ttu-id="e8fcc-175">Výběr služby Azure Blob Storage jako cíle:</span><span class="sxs-lookup"><span data-stu-id="e8fcc-175">Choosing Azure Blob Storage as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json][]
```

<span data-ttu-id="e8fcc-176">Výběr služby Azure Data Lake Store jako cíle:</span><span class="sxs-lookup"><span data-stu-id="e8fcc-176">Choosing Azure Data Lake Store as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="e8fcc-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8fcc-177">Next steps</span></span>

<span data-ttu-id="e8fcc-178">Můžete také nakonfigurovat zaznamenat centra událostí prostřednictvím hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e8fcc-178">You can also configure Event Hubs Capture via hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e8fcc-179">Další informace najdete v tématu [povolit zaznamenat centra událostí pomocí portálu Azure hello](event-hubs-capture-enable-through-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e8fcc-179">For more information, see [Enable Event Hubs Capture using hello Azure portal](event-hubs-capture-enable-through-portal.md).</span></span>

<span data-ttu-id="e8fcc-180">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="e8fcc-180">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="e8fcc-181">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="e8fcc-181">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="e8fcc-182">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="e8fcc-182">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="e8fcc-183">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="e8fcc-183">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Azure Resources naming conventions]: https://azure.microsoft.com/documentation/articles/guidance-naming-conventions/
[Event hub and enable Capture tooStorage template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture
[Event hub and enable Capture tooAzure Data Lake Store template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture-for-adls
