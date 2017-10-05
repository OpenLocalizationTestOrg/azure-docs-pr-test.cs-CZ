---
title: "Nakonfigurovat připojovací řetězec pro Azure Storage | Microsoft Docs"
description: "Nakonfigurujte připojovací řetězec pro účet úložiště Azure. Připojovací řetězec obsahuje informace potřebné k ověření přístupu k účtu úložiště z vaší aplikace za běhu."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: marsma
ms.openlocfilehash: 01aa506e2b47fc29a70592e670a206a2b74248a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-azure-storage-connection-strings"></a><span data-ttu-id="3d01a-104">Nakonfigurování připojovacích řetězců Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3d01a-104">Configure Azure Storage connection strings</span></span>

<span data-ttu-id="3d01a-105">Připojovací řetězec obsahuje informace o ověřování, které jsou potřebné pro vaši aplikaci pro přístup k datům v účtu Azure Storage za běhu.</span><span class="sxs-lookup"><span data-stu-id="3d01a-105">A connection string includes the authentication information required for your application to access data in an Azure Storage account at runtime.</span></span> <span data-ttu-id="3d01a-106">Můžete nakonfigurovat připojovací řetězce pro:</span><span class="sxs-lookup"><span data-stu-id="3d01a-106">You can configure connection strings to:</span></span>

* <span data-ttu-id="3d01a-107">Připojte k emulátoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3d01a-107">Connect to the Azure storage emulator.</span></span>
* <span data-ttu-id="3d01a-108">Přístup k účtu úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="3d01a-108">Access a storage account in Azure.</span></span>
* <span data-ttu-id="3d01a-109">Přístup k zadané prostředky v Azure pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="3d01a-109">Access specified resources in Azure via a shared access signature (SAS).</span></span>

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a><span data-ttu-id="3d01a-110">Ukládání připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3d01a-110">Storing your connection string</span></span>
<span data-ttu-id="3d01a-111">Aplikace musí pro přístup k připojovací řetězec za běhu k ověřování žádostí odeslaných do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3d01a-111">Your application needs to access the connection string at runtime to authenticate requests made to Azure Storage.</span></span> <span data-ttu-id="3d01a-112">Máte několik možností pro ukládání připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="3d01a-112">You have several options for storing your connection string:</span></span>

* <span data-ttu-id="3d01a-113">Je spuštěna aplikace na ploše nebo na zařízení můžete uložit připojovací řetězec **app.config** nebo **web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="3d01a-113">An application running on the desktop or on a device can store the connection string in an **app.config** or **web.config** file.</span></span> <span data-ttu-id="3d01a-114">Přidat připojovací řetězec k **AppSettings** část v těchto souborech.</span><span class="sxs-lookup"><span data-stu-id="3d01a-114">Add the connection string to the **AppSettings** section in these files.</span></span>
* <span data-ttu-id="3d01a-115">Aplikace běžící v cloudové službě Azure můžete uložit připojovací řetězec [konfigurační soubor schématu (.cscfg) Azure služby](https://msdn.microsoft.com/library/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d01a-115">An application running in an Azure cloud service can store the connection string in the [Azure service configuration schema (.cscfg) file](https://msdn.microsoft.com/library/ee758710.aspx).</span></span> <span data-ttu-id="3d01a-116">Přidat připojovací řetězec k **ConfigurationSettings** oddíl konfiguračního souboru služby.</span><span class="sxs-lookup"><span data-stu-id="3d01a-116">Add the connection string to the **ConfigurationSettings** section of the service configuration file.</span></span>
* <span data-ttu-id="3d01a-117">Můžete vytvořit připojovací řetězec přímo v kódu.</span><span class="sxs-lookup"><span data-stu-id="3d01a-117">You can use your connection string directly in your code.</span></span> <span data-ttu-id="3d01a-118">Doporučujeme však připojovací řetězec uložit v konfiguračním souboru ve většině scénářů.</span><span class="sxs-lookup"><span data-stu-id="3d01a-118">However, we recommend that you store your connection string in a configuration file in most scenarios.</span></span>

<span data-ttu-id="3d01a-119">Ukládání připojovací řetězec v konfiguračním souboru usnadňuje aktualizovat připojovací řetězec pro přepínání emulátor úložiště a účet úložiště Azure v cloudu.</span><span class="sxs-lookup"><span data-stu-id="3d01a-119">Storing your connection string in a configuration file makes it easy to update the connection string to switch between the storage emulator and an Azure storage account in the cloud.</span></span> <span data-ttu-id="3d01a-120">Potřebujete upravit připojovací řetězec tak, aby odkazoval na cílovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="3d01a-120">You only need to edit the connection string to point to your target environment.</span></span>

<span data-ttu-id="3d01a-121">Můžete použít [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) pro přístup k připojovací řetězec za běhu bez ohledu na to, kde je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="3d01a-121">You can use the [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) to access your connection string at runtime regardless of where your application is running.</span></span>

## <a name="create-a-connection-string-for-the-storage-emulator"></a><span data-ttu-id="3d01a-122">Vytvořit připojovací řetězec pro emulátor úložiště</span><span class="sxs-lookup"><span data-stu-id="3d01a-122">Create a connection string for the storage emulator</span></span>
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

<span data-ttu-id="3d01a-123">Další informace o emulátoru úložiště najdete v tématu [pomocí emulátoru úložiště Azure pro vývoj a testování](storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="3d01a-123">For more information about the storage emulator, see [Use the Azure storage emulator for development and testing](storage-use-emulator.md).</span></span>

## <a name="create-a-connection-string-for-an-azure-storage-account"></a><span data-ttu-id="3d01a-124">Vytvoření připojovacího řetězce pro účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="3d01a-124">Create a connection string for an Azure storage account</span></span>
<span data-ttu-id="3d01a-125">Vytvoření připojovacího řetězce pro váš účet úložiště Azure, použijte následující formát.</span><span class="sxs-lookup"><span data-stu-id="3d01a-125">To create a connection string for your Azure storage account, use the following format.</span></span> <span data-ttu-id="3d01a-126">Označuje, zda se chcete připojit k účtu úložiště prostřednictvím protokolu HTTPS (doporučeno) nebo protokol HTTP, nahraďte `myAccountName` s názvem účtu úložiště a nahraďte `myAccountKey` vaším přístupovým klíčem účet:</span><span class="sxs-lookup"><span data-stu-id="3d01a-126">Indicate whether you want to connect to the storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with the name of your storage account, and replace `myAccountKey` with your account access key:</span></span>

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

<span data-ttu-id="3d01a-127">Například připojovací řetězec může vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="3d01a-127">For example, your connection string might look similar to:</span></span>

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

<span data-ttu-id="3d01a-128">I když Azure Storage podporuje protokoly HTTP a HTTPS v připojovacím řetězci, *HTTPS se důrazně doporučuje*.</span><span class="sxs-lookup"><span data-stu-id="3d01a-128">Although Azure Storage supports both HTTP and HTTPS in a connection string, *HTTPS is highly recommended*.</span></span>

> [!TIP]
> <span data-ttu-id="3d01a-129">Můžete najít váš účet úložiště připojovací řetězce v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3d01a-129">You can find your storage account's connection strings in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3d01a-130">Přejděte na **nastavení** > **přístupové klíče** v okně účtu úložiště nabídky Zobrazit připojovací řetězce pro obě primární a sekundární přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="3d01a-130">Navigate to **SETTINGS** > **Access keys** in your storage account's menu blade to see connection strings for both primary and secondary access keys.</span></span>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a><span data-ttu-id="3d01a-131">Vytvoření připojovacího řetězce pomocí sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="3d01a-131">Create a connection string using a shared access signature</span></span>
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a><span data-ttu-id="3d01a-132">Vytvoření připojovacího řetězce pro koncový bod explicitní úložiště</span><span class="sxs-lookup"><span data-stu-id="3d01a-132">Create a connection string for an explicit storage endpoint</span></span>
<span data-ttu-id="3d01a-133">Koncové body explicitní služby můžete zadat v připojovací řetězec místo použití výchozí koncové body.</span><span class="sxs-lookup"><span data-stu-id="3d01a-133">You can specify explicit service endpoints in your connection string instead of using the default endpoints.</span></span> <span data-ttu-id="3d01a-134">Pokud chcete vytvořit připojovací řetězec, který určuje explicitní koncový bod, zadejte koncový bod dokončení služby pro jednotlivé služby včetně specifikace protokolu (HTTPS (doporučeno) nebo HTTP), v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="3d01a-134">To create a connection string that specifies an explicit endpoint, specify the complete service endpoint for each service, including the protocol specification (HTTPS (recommended) or HTTP), in the following format:</span></span>

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

<span data-ttu-id="3d01a-135">Jeden scénář, kde můžete chtít zadat explicitní koncový bod je, když jste změnili koncový bod úložiště objektů Blob [vlastní domény](storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="3d01a-135">One scenario where you might wish to specify an explicit endpoint is when you've mapped your Blob storage endpoint to a [custom domain](storage-custom-domain-name.md).</span></span> <span data-ttu-id="3d01a-136">V takovém případě můžete zadat svůj vlastní koncový bod pro úložiště objektů Blob v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="3d01a-136">In that case, you can specify your custom endpoint for Blob storage in your connection string.</span></span> <span data-ttu-id="3d01a-137">Volitelně můžete zadat výchozí koncové body pro jiné služby, pokud je vaše aplikace používá.</span><span class="sxs-lookup"><span data-stu-id="3d01a-137">You can optionally specify the default endpoints for the other services if your application uses them.</span></span>

<span data-ttu-id="3d01a-138">Tady je příklad připojovacího řetězce, který určuje explicitní koncový bod služby objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="3d01a-138">Here is an example of a connection string that specifies an explicit endpoint for the Blob service:</span></span>

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="3d01a-139">Tento příklad určuje explicitní koncových bodů pro všechny služby, včetně vlastní doménu pro služby objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="3d01a-139">This example specifies explicit endpoints for all services, including a custom domain for the Blob service:</span></span>

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="3d01a-140">Hodnoty koncového bodu v připojovacím řetězci slouží k vytvoření požadavku identifikátory URI služby storage a stanovují formu všechny identifikátory URI, které jsou vráceny do vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="3d01a-140">The endpoint values in a connection string are used to construct the request URIs to the storage services, and dictate the form of any URIs that are returned to your code.</span></span>

<span data-ttu-id="3d01a-141">Pokud jste změnili koncový bod úložiště vlastní domény a vynechejte tohoto koncového bodu z připojovacího řetězce, pak nebude možné použít tento připojovací řetězec pro přístup k datům v této službě z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="3d01a-141">If you've mapped a storage endpoint to a custom domain and omit that endpoint from a connection string, then you will not be able to use that connection string to access data in that service from your code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3d01a-142">Hodnoty koncového bodu služby v připojovací řetězce musí být ve správném formátu identifikátory URI, včetně `https://` (doporučeno) nebo `http://`.</span><span class="sxs-lookup"><span data-stu-id="3d01a-142">Service endpoint values in your connection strings must be well-formed URIs, including `https://` (recommended) or `http://`.</span></span> <span data-ttu-id="3d01a-143">Protože Azure Storage ještě nepodporuje protokol HTTPS pro vlastní domény, můžete *musí* zadejte `http://` pro libovolný koncový bod identifikátor URI, který odkazuje na vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="3d01a-143">Because Azure Storage does not yet support HTTPS for custom domains, you *must* specify `http://` for any endpoint URI that points to a custom domain.</span></span>
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a><span data-ttu-id="3d01a-144">Vytvoření připojovacího řetězce s příponou koncový bod</span><span class="sxs-lookup"><span data-stu-id="3d01a-144">Create a connection string with an endpoint suffix</span></span>
<span data-ttu-id="3d01a-145">Vytvoření připojovacího řetězce pro službu úložiště v oblasti nebo instancí s přípon jinému koncovému bodu, například pro Azure China nebo Azure Government, použijte následující formát řetězec připojení.</span><span class="sxs-lookup"><span data-stu-id="3d01a-145">To create a connection string for a storage service in regions or instances with different endpoint suffixes, such as for Azure China or Azure Government, use the following connection string format.</span></span> <span data-ttu-id="3d01a-146">Označuje, zda se chcete připojit k účtu úložiště prostřednictvím protokolu HTTPS (doporučeno) nebo protokol HTTP, nahraďte `myAccountName` s názvem účtu úložiště, nahraďte `myAccountKey` se přístupový klíč účtu a nahraďte `mySuffix` s příponou URI:</span><span class="sxs-lookup"><span data-stu-id="3d01a-146">Indicate whether you want to connect to the storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with the name of your storage account, replace `myAccountKey` with your account access key, and replace `mySuffix` with the URI suffix:</span></span>

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

<span data-ttu-id="3d01a-147">Tady je příklad řetězce připojení pro služby úložiště v Číně Azure:</span><span class="sxs-lookup"><span data-stu-id="3d01a-147">Here's an example connection string for storage services in Azure China:</span></span>

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a><span data-ttu-id="3d01a-148">Analýza připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="3d01a-148">Parsing a connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a><span data-ttu-id="3d01a-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d01a-149">Next steps</span></span>
* [<span data-ttu-id="3d01a-150">Použití emulátoru úložiště Azure pro vývoj a testování</span><span class="sxs-lookup"><span data-stu-id="3d01a-150">Use the Azure storage emulator for development and testing</span></span>](storage-use-emulator.md)
* [<span data-ttu-id="3d01a-151">Azure průzkumníci úložiště</span><span class="sxs-lookup"><span data-stu-id="3d01a-151">Azure Storage explorers</span></span>](storage-explorers.md)
* [<span data-ttu-id="3d01a-152">Použití sdílených přístupových podpisů (SAS)</span><span class="sxs-lookup"><span data-stu-id="3d01a-152">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)

