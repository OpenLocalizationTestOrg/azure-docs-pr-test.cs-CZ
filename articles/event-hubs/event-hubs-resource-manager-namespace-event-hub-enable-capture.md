---
title: "Vytvoření oboru názvů Azure Event Hubs a povolení funkce Capture pomocí šablony | Dokumentace Microsoftu"
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
ms.openlocfilehash: 19bbb51868e767aa1d15f4574628b7fd36607207
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-event-hubs-namespace-with-an-event-hub-and-enable-capture-using-an-azure-resource-manager-template"></a><span data-ttu-id="2ad9d-103">Vytvoření oboru názvů Event Hubs s centrem událostí a povolení funkce Capture pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="2ad9d-103">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>

<span data-ttu-id="2ad9d-104">Tento článek ukazuje, jak použít šablonu Azure Resource Manageru, která vytvoří obor názvů Event Hubs s jednou instancí centra událostí, ve kterém také povolí [funkci Capture](event-hubs-capture-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2ad9d-104">This article shows how to use an Azure Resource Manager template that creates an Event Hubs namespace, with one event hub instance, and also enables the [Capture feature](event-hubs-capture-overview.md) on the event hub.</span></span> <span data-ttu-id="2ad9d-105">Tento článek popisuje, jak definovat, které prostředky se nasadí, a jak definovat parametry zadávané při spuštění nasazení.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-105">The article describes how to define which resources are deployed, and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="2ad9d-106">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="2ad9d-107">Tento článek také ukazuje, jak určit, že se události mají zachytávat do objektů Azure Storage Blob nebo do služby Azure Data Lake Store v závislosti na zvoleném cíli.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-107">This article also shows how to specify that events are captured into either Azure Storage Blobs or an Azure Data Lake Store, based on the destination you choose.</span></span>

<span data-ttu-id="2ad9d-108">Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="2ad9d-108">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="2ad9d-109">Další informace o vzorech a postupech pro zásady vytváření názvů prostředků Azure najdete v tématu [Zásady vytváření názvů prostředků Azure][Azure Resources naming conventions].</span><span class="sxs-lookup"><span data-stu-id="2ad9d-109">For more information about patterns and practices for Azure Resources naming conventions, see [Azure Resources naming conventions][Azure Resources naming conventions].</span></span>

<span data-ttu-id="2ad9d-110">Hotové šablony můžete získat kliknutím na následující odkazy na web GitHub:</span><span class="sxs-lookup"><span data-stu-id="2ad9d-110">For the complete templates, click the following GitHub links:</span></span>

- <span data-ttu-id="2ad9d-111">[Šablona centra událostí a povolení zachytávání pomocí funkce Capture do služby Storage][Event Hub and enable Capture to Storage template]</span><span class="sxs-lookup"><span data-stu-id="2ad9d-111">[Event hub and enable Capture to Storage template][Event Hub and enable Capture to Storage template]</span></span> 
- <span data-ttu-id="2ad9d-112">[Šablona centra událostí a povolení zachytávání pomocí funkce Capture do služby Azure Data Lake Store][Event Hub and enable Capture to Azure Data Lake Store template]</span><span class="sxs-lookup"><span data-stu-id="2ad9d-112">[Event hub and enable Capture to Azure Data Lake Store template][Event Hub and enable Capture to Azure Data Lake Store template]</span></span>

> [!NOTE]
> <span data-ttu-id="2ad9d-113">Nejnovější šablony můžete zkontrolovat tak, že přejdete do galerie [Šablony Azure pro rychlý start][Azure Quickstart Templates] a vyhledáte Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-113">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="2ad9d-114">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="2ad9d-114">What will you deploy?</span></span>

<span data-ttu-id="2ad9d-115">Pomocí této šablony nasadíte obor názvů Event Hubs s centrem událostí a povolíte funkci [Event Hubs Capture](event-hubs-capture-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2ad9d-115">With this template, you deploy an Event Hubs namespace with an event hub, and also enable [Event Hubs Capture](event-hubs-capture-overview.md).</span></span>

<span data-ttu-id="2ad9d-116">Služba [Event Hubs](event-hubs-what-is-event-hubs.md) zpracovává události a zajišťuje příjem příchozích dat událostí a telemetrie do Azure v masivním měřítku, s nízkou latencí a vysokou spolehlivostí.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-116">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used to provide event and telemetry ingress to Azure at massive scale, with low latency and high reliability.</span></span> <span data-ttu-id="2ad9d-117">Funkce Event Hubs Capture umožňuje automatické doručování streamovaných dat ve službě Event Hubs do služby Azure Blob Storage nebo Azure Data Lake Store v rámci zvoleného časového nebo velikostního intervalu.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-117">Event Hubs Capture enables you to automatically deliver the streaming data in Event Hubs to Azure Blob storage or Azure Data Lake Store, within a specified time or size interval of your choosing.</span></span>

<span data-ttu-id="2ad9d-118">Kliknutím na následující tlačítko povolíte zachytávání pomocí funkce Event Hubs Capture do služby Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="2ad9d-118">Click the following button to enable Event Hubs Capture into Azure Storage:</span></span>

<span data-ttu-id="2ad9d-119">[![Nasazení do Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="2ad9d-119">[![Deploy to Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span></span>

<span data-ttu-id="2ad9d-120">Kliknutím na následující tlačítko povolíte zachytávání pomocí funkce Event Hubs Capture do služby Azure Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="2ad9d-120">Click the following button to enable Event Hubs Capture into Azure Data Lake Store:</span></span>

<span data-ttu-id="2ad9d-121">[![Nasazení do Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="2ad9d-121">[![Deploy to Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="2ad9d-122">Parametry</span><span class="sxs-lookup"><span data-stu-id="2ad9d-122">Parameters</span></span>

<span data-ttu-id="2ad9d-123">Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-123">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="2ad9d-124">Šablona obsahuje část `Parameters`, která obsahuje všechny hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-124">The template includes a section called `Parameters` that contains all the parameter values.</span></span> <span data-ttu-id="2ad9d-125">Parametr byste měli definovat pro hodnoty, které se mění v závislosti na nasazovaném projektu nebo prostředí, do kterého nasazujete.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-125">You should define a parameter for those values that vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="2ad9d-126">Nedefinujte parametry pro hodnoty, které jsou vždy stejné.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-126">Do not define parameters for values that always stay the same.</span></span> <span data-ttu-id="2ad9d-127">Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-127">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="2ad9d-128">Šablona definuje následující parametry.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-128">The template defines the following parameters.</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="2ad9d-129">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="2ad9d-129">eventHubNamespaceName</span></span>

<span data-ttu-id="2ad9d-130">Název [oboru názvů Event Hubs](event-hubs-create.md), který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-130">The name of the [Event Hubs namespace](event-hubs-create.md) to create.</span></span>

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a><span data-ttu-id="2ad9d-131">eventHubName</span><span class="sxs-lookup"><span data-stu-id="2ad9d-131">eventHubName</span></span>

<span data-ttu-id="2ad9d-132">Název centra událostí vytvořeného v [oboru názvů Event Hubs](event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="2ad9d-132">The name of the event hub created in the [Event Hubs namespace](event-hubs-create.md).</span></span>

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the event hub"
    }
}
```

### <a name="messageretentionindays"></a><span data-ttu-id="2ad9d-133">messageRetentionInDays</span><span class="sxs-lookup"><span data-stu-id="2ad9d-133">messageRetentionInDays</span></span>

<span data-ttu-id="2ad9d-134">Počet dní, po které se zprávy budou uchovávat v centru událostí.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-134">The number of days to retain the messages in the event hub.</span></span> 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in event hub"
     }
 }
```

### <a name="partitioncount"></a><span data-ttu-id="2ad9d-135">partitionCount</span><span class="sxs-lookup"><span data-stu-id="2ad9d-135">partitionCount</span></span>

<span data-ttu-id="2ad9d-136">Počet oddílů, které se mají vytvořit v centru událostí.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-136">The number of partitions to create in the event hub.</span></span>

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

### <a name="captureenabled"></a><span data-ttu-id="2ad9d-137">captureEnabled</span><span class="sxs-lookup"><span data-stu-id="2ad9d-137">captureEnabled</span></span>

<span data-ttu-id="2ad9d-138">Povolení funkce Capture v centru událostí.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-138">Enable Capture on the event hub.</span></span>

```json
"captureEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Capture for your event hub"
    }
 }
```
### <a name="captureencodingformat"></a><span data-ttu-id="2ad9d-139">captureEncodingFormat</span><span class="sxs-lookup"><span data-stu-id="2ad9d-139">captureEncodingFormat</span></span>

<span data-ttu-id="2ad9d-140">Formát kódování, který zadáte pro serializaci dat událostí.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-140">The encoding format you specify to serialize the event data.</span></span>

```json
"captureEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format in which Capture serializes the EventData"
    }
}
```

### <a name="capturetime"></a><span data-ttu-id="2ad9d-141">captureTime</span><span class="sxs-lookup"><span data-stu-id="2ad9d-141">captureTime</span></span>

<span data-ttu-id="2ad9d-142">Časový interval, ve kterém funkce Event Hubs Capture začne zachytávat data.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-142">The time interval in which Event Hubs Capture starts capturing the data.</span></span>

```json
"captureTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the capture"
    }
}
```

### <a name="capturesize"></a><span data-ttu-id="2ad9d-143">captureSize</span><span class="sxs-lookup"><span data-stu-id="2ad9d-143">captureSize</span></span>
<span data-ttu-id="2ad9d-144">Velikostní interval, ve kterém funkce Capture začne zachytávat data.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-144">The size interval at which Capture starts capturing the data.</span></span>

```json
"captureSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"The size window in bytes for capture"
    }
}
```

###<a name="capturenameformat"></a><span data-ttu-id="2ad9d-145">captureNameFormat</span><span class="sxs-lookup"><span data-stu-id="2ad9d-145">captureNameFormat</span></span>

<span data-ttu-id="2ad9d-146">Formát názvu, který má funkce Event Hubs Capture používat k zápisu souborů Avro.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-146">The name format used by Event Hubs Capture to write the Avro files.</span></span> <span data-ttu-id="2ad9d-147">Nezapomeňte, že formát názvu pro funkci Capture musí obsahovat pole `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}` a `{Second}`.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-147">Note that a Capture name format must contain `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}`, and `{Second}` fields.</span></span> <span data-ttu-id="2ad9d-148">Tato pole můžete uspořádat v libovolném pořadí s oddělovači nebo bez.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-148">These can be arranged in any order, with or without delimiters.</span></span>
 
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

### <a name="apiversion"></a><span data-ttu-id="2ad9d-149">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2ad9d-149">apiVersion</span></span>

<span data-ttu-id="2ad9d-150">Verze rozhraní API šablony.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-150">The API version of the template.</span></span>

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

<span data-ttu-id="2ad9d-151">Pokud jako cíl zvolíte službu Azure Storage, použijte následující parametry.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-151">Use the following parameters if you choose Azure Storage as your destination.</span></span>

### <a name="destinationstorageaccountresourceid"></a><span data-ttu-id="2ad9d-152">destinationStorageAccountResourceId</span><span class="sxs-lookup"><span data-stu-id="2ad9d-152">destinationStorageAccountResourceId</span></span>

<span data-ttu-id="2ad9d-153">Funkce Capture pro povolení zachytávání do požadovaného účtu Storage vyžaduje ID prostředku účtu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-153">Capture requires an Azure Storage account resource ID to enable capturing to your desired Storage account.</span></span>

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing Storage account resource ID where you want the blobs be captured"
    }
 }
```

### <a name="blobcontainername"></a><span data-ttu-id="2ad9d-154">blobContainerName</span><span class="sxs-lookup"><span data-stu-id="2ad9d-154">blobContainerName</span></span>

<span data-ttu-id="2ad9d-155">Kontejner objektů blob, do kterého se mají zachytávat data událostí.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-155">The blob container in which to capture your event data.</span></span>

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container in which you want the blobs captured"
    }
}
```

<span data-ttu-id="2ad9d-156">Pokud jako cíl zvolíte službu Azure Data Lake Store, použijte následující parametry.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-156">Use the following parameters if you choose Azure Data Lake Store as your destination.</span></span> <span data-ttu-id="2ad9d-157">Je potřeba nastavit oprávnění k cestě Data Lake Store, ve které chcete událost zachytávat.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-157">You must set permissions on your Data Lake Store path, in which you want to Capture the event.</span></span> <span data-ttu-id="2ad9d-158">Informace o nastavení oprávnění najdete v [tomto článku](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="2ad9d-158">To set permissions, see [this article](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span></span>

###<a name="subscriptionid"></a><span data-ttu-id="2ad9d-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="2ad9d-159">subscriptionId</span></span>

<span data-ttu-id="2ad9d-160">ID předplatného pro obor názvů Event Hubs a službu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-160">Subscription ID for the Event Hubs namespace and Azure Data Lake Store.</span></span> <span data-ttu-id="2ad9d-161">Oba tyto prostředky musí patřit pod stejné ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-161">Both these resources must be under the same subscription ID.</span></span>

```json
"subscriptionId": {
    "type": "string",
    "metadata": {
        "description": "Subscription Id of both Azure Data Lake Store and Event Hub namespace"
     }
 }
```

###<a name="datalakeaccountname"></a><span data-ttu-id="2ad9d-162">dataLakeAccountName</span><span class="sxs-lookup"><span data-stu-id="2ad9d-162">dataLakeAccountName</span></span>

<span data-ttu-id="2ad9d-163">Název služby Azure Data Lake Store pro zachycené události.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-163">The Azure Data Lake Store name for the captured events.</span></span>

```json
"dataLakeAccountName": {
    "type": "string",
    "metadata": {
        "description": "Azure Data Lake Store name"
    }
}
```

###<a name="datalakefolderpath"></a><span data-ttu-id="2ad9d-164">dataLakeFolderPath</span><span class="sxs-lookup"><span data-stu-id="2ad9d-164">dataLakeFolderPath</span></span>

<span data-ttu-id="2ad9d-165">Cesta k cílové složce pro zachycené události.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-165">The destination folder path for the captured events.</span></span>

```json
"dataLakeFolderPath": {
    "type": "string",
    "metadata": {
        "description": "Destination archive folder path"
    }
}
```

## <a name="resources-to-deploy-for-azure-storage-as-destination-to-captured-events"></a><span data-ttu-id="2ad9d-166">Prostředky k nasazení pro službu Azure Storage jako cíl zachycených událostí</span><span class="sxs-lookup"><span data-stu-id="2ad9d-166">Resources to deploy for Azure Storage as destination to captured events</span></span>

<span data-ttu-id="2ad9d-167">Vytvoří obor názvů typu **EventHubs** s jedním centrem událostí a povolí zachytávání pomocí funkce Capture do služby Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-167">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture to Azure Blob Storage.</span></span>

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

## <a name="resources-to-deploy-for-azure-data-lake-store-as-destination"></a><span data-ttu-id="2ad9d-168">Prostředky k nasazení pro službu Azure Data Lake Store jako cíl</span><span class="sxs-lookup"><span data-stu-id="2ad9d-168">Resources to deploy for Azure Data Lake Store as destination</span></span>

<span data-ttu-id="2ad9d-169">Vytvoří obor názvů typu **EventHubs** s jedním centrem událostí a povolí zachytávání pomocí funkce Capture do služby Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2ad9d-169">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture to Azure Data Lake Store.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="2ad9d-170">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="2ad9d-170">Commands to run deployment</span></span>

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="2ad9d-171">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ad9d-171">PowerShell</span></span>

<span data-ttu-id="2ad9d-172">Nasazení šablony pro povolení zachytávání pomocí funkce Event Hubs Capture do služby Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="2ad9d-172">Deploy your template to enable Event Hubs Capture into Azure Storage:</span></span>
 
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json
```

<span data-ttu-id="2ad9d-173">Nasazení šablony pro povolení zachytávání pomocí funkce Event Hubs Capture do služby Azure Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="2ad9d-173">Deploy your template to enable Event Hubs Capture into Azure Data Lake Store:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="2ad9d-174">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2ad9d-174">Azure CLI</span></span>

<span data-ttu-id="2ad9d-175">Výběr služby Azure Blob Storage jako cíle:</span><span class="sxs-lookup"><span data-stu-id="2ad9d-175">Choosing Azure Blob Storage as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json][]
```

<span data-ttu-id="2ad9d-176">Výběr služby Azure Data Lake Store jako cíle:</span><span class="sxs-lookup"><span data-stu-id="2ad9d-176">Choosing Azure Data Lake Store as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="2ad9d-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ad9d-177">Next steps</span></span>

<span data-ttu-id="2ad9d-178">Funkci Event Hubs Capture můžete konfigurovat také prostřednictvím webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2ad9d-178">You can also configure Event Hubs Capture via the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="2ad9d-179">Další informace najdete v tématu [Povolení funkce Event Hubs Capture pomocí webu Azure Portal](event-hubs-capture-enable-through-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2ad9d-179">For more information, see [Enable Event Hubs Capture using the Azure portal](event-hubs-capture-enable-through-portal.md).</span></span>

<span data-ttu-id="2ad9d-180">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="2ad9d-180">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="2ad9d-181">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="2ad9d-181">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="2ad9d-182">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="2ad9d-182">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="2ad9d-183">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="2ad9d-183">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Azure Resources naming conventions]: https://azure.microsoft.com/documentation/articles/guidance-naming-conventions/
[Event hub and enable Capture to Storage template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture
[Event hub and enable Capture to Azure Data Lake Store template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture-for-adls