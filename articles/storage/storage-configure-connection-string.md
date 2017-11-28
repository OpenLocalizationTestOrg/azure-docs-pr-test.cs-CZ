---
title: "aaaConfigure připojovací řetězec pro Azure Storage | Microsoft Docs"
description: "Nakonfigurujte připojovací řetězec pro účet úložiště Azure. Připojovací řetězec obsahuje informace o hello potřeby tooauthenticate přístup k účtu úložiště tooa z vaší aplikace za běhu."
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
ms.openlocfilehash: 80c38a6f8f0d4f06b99e7c487647b984e01d1772
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-storage-connection-strings"></a><span data-ttu-id="6b11d-104">Nakonfigurování připojovacích řetězců Azure Storage</span><span class="sxs-lookup"><span data-stu-id="6b11d-104">Configure Azure Storage connection strings</span></span>

<span data-ttu-id="6b11d-105">Připojovací řetězec obsahuje hello ověřovací informace požadované pro vaše aplikace tooaccess data v účtu Azure Storage za běhu.</span><span class="sxs-lookup"><span data-stu-id="6b11d-105">A connection string includes hello authentication information required for your application tooaccess data in an Azure Storage account at runtime.</span></span> <span data-ttu-id="6b11d-106">Můžete nakonfigurovat připojovací řetězce pro:</span><span class="sxs-lookup"><span data-stu-id="6b11d-106">You can configure connection strings to:</span></span>

* <span data-ttu-id="6b11d-107">Připojte toohello emulátoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6b11d-107">Connect toohello Azure storage emulator.</span></span>
* <span data-ttu-id="6b11d-108">Přístup k účtu úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="6b11d-108">Access a storage account in Azure.</span></span>
* <span data-ttu-id="6b11d-109">Přístup k zadané prostředky v Azure pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="6b11d-109">Access specified resources in Azure via a shared access signature (SAS).</span></span>

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a><span data-ttu-id="6b11d-110">Ukládání připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="6b11d-110">Storing your connection string</span></span>
<span data-ttu-id="6b11d-111">Aplikace musí tooaccess hello připojovací řetězec na modulu runtime tooauthenticate požadavkům tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="6b11d-111">Your application needs tooaccess hello connection string at runtime tooauthenticate requests made tooAzure Storage.</span></span> <span data-ttu-id="6b11d-112">Máte několik možností pro ukládání připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="6b11d-112">You have several options for storing your connection string:</span></span>

* <span data-ttu-id="6b11d-113">Je spuštěna aplikace na ploše hello nebo na zařízení můžete ukládat hello připojovací řetězec **app.config** nebo **web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="6b11d-113">An application running on hello desktop or on a device can store hello connection string in an **app.config** or **web.config** file.</span></span> <span data-ttu-id="6b11d-114">Přidat hello připojovací řetězec toohello **AppSettings** část v těchto souborech.</span><span class="sxs-lookup"><span data-stu-id="6b11d-114">Add hello connection string toohello **AppSettings** section in these files.</span></span>
* <span data-ttu-id="6b11d-115">Aplikace běžící v cloudové službě Azure můžete ukládat hello připojovací řetězec v hello [konfigurační soubor schématu (.cscfg) Azure služby](https://msdn.microsoft.com/library/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="6b11d-115">An application running in an Azure cloud service can store hello connection string in hello [Azure service configuration schema (.cscfg) file](https://msdn.microsoft.com/library/ee758710.aspx).</span></span> <span data-ttu-id="6b11d-116">Přidat hello připojovací řetězec toohello **ConfigurationSettings** části konfigurační soubor služby hello.</span><span class="sxs-lookup"><span data-stu-id="6b11d-116">Add hello connection string toohello **ConfigurationSettings** section of hello service configuration file.</span></span>
* <span data-ttu-id="6b11d-117">Můžete vytvořit připojovací řetězec přímo v kódu.</span><span class="sxs-lookup"><span data-stu-id="6b11d-117">You can use your connection string directly in your code.</span></span> <span data-ttu-id="6b11d-118">Doporučujeme však připojovací řetězec uložit v konfiguračním souboru ve většině scénářů.</span><span class="sxs-lookup"><span data-stu-id="6b11d-118">However, we recommend that you store your connection string in a configuration file in most scenarios.</span></span>

<span data-ttu-id="6b11d-119">Ukládání připojovací řetězec v konfiguračním souboru umožňuje snadno tooupdate hello připojovací řetězec tooswitch mezi hello emulátor úložiště a účet úložiště Azure v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6b11d-119">Storing your connection string in a configuration file makes it easy tooupdate hello connection string tooswitch between hello storage emulator and an Azure storage account in hello cloud.</span></span> <span data-ttu-id="6b11d-120">Potřebujete jenom tooedit hello připojovací řetězec toopoint tooyour cílovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="6b11d-120">You only need tooedit hello connection string toopoint tooyour target environment.</span></span>

<span data-ttu-id="6b11d-121">Můžete použít hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess váš připojovací řetězec za běhu bez ohledu na to, kde je aplikace spuštěna.</span><span class="sxs-lookup"><span data-stu-id="6b11d-121">You can use hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess your connection string at runtime regardless of where your application is running.</span></span>

## <a name="create-a-connection-string-for-hello-storage-emulator"></a><span data-ttu-id="6b11d-122">Vytvořit připojovací řetězec pro emulátor úložiště hello</span><span class="sxs-lookup"><span data-stu-id="6b11d-122">Create a connection string for hello storage emulator</span></span>
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

<span data-ttu-id="6b11d-123">Další informace o emulátoru úložiště hello najdete v tématu [používejte hello emulátoru úložiště Azure pro vývoj a testování](storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="6b11d-123">For more information about hello storage emulator, see [Use hello Azure storage emulator for development and testing](storage-use-emulator.md).</span></span>

## <a name="create-a-connection-string-for-an-azure-storage-account"></a><span data-ttu-id="6b11d-124">Vytvoření připojovacího řetězce pro účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6b11d-124">Create a connection string for an Azure storage account</span></span>
<span data-ttu-id="6b11d-125">toocreate připojovací řetězec pro váš účet úložiště Azure, použijte následující hello formátu.</span><span class="sxs-lookup"><span data-stu-id="6b11d-125">toocreate a connection string for your Azure storage account, use hello following format.</span></span> <span data-ttu-id="6b11d-126">Označuje, zda chcete účet úložiště toohello tooconnect prostřednictvím protokolu HTTPS (doporučeno) nebo protokolu HTTP, nahraďte `myAccountName` hello název účtu úložiště a nahraďte `myAccountKey` vaším přístupovým klíčem účet:</span><span class="sxs-lookup"><span data-stu-id="6b11d-126">Indicate whether you want tooconnect toohello storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with hello name of your storage account, and replace `myAccountKey` with your account access key:</span></span>

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

<span data-ttu-id="6b11d-127">Například připojovací řetězec může vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="6b11d-127">For example, your connection string might look similar to:</span></span>

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

<span data-ttu-id="6b11d-128">I když Azure Storage podporuje protokoly HTTP a HTTPS v připojovacím řetězci, *HTTPS se důrazně doporučuje*.</span><span class="sxs-lookup"><span data-stu-id="6b11d-128">Although Azure Storage supports both HTTP and HTTPS in a connection string, *HTTPS is highly recommended*.</span></span>

> [!TIP]
> <span data-ttu-id="6b11d-129">Připojovací řetězce účtu úložiště můžete najít v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6b11d-129">You can find your storage account's connection strings in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6b11d-130">Přejděte příliš**nastavení** > **přístupové klíče** v účtu úložiště nabídky okno toosee připojovací řetězce pro obě primární a sekundární přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="6b11d-130">Navigate too**SETTINGS** > **Access keys** in your storage account's menu blade toosee connection strings for both primary and secondary access keys.</span></span>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a><span data-ttu-id="6b11d-131">Vytvoření připojovacího řetězce pomocí sdíleného přístupového podpisu</span><span class="sxs-lookup"><span data-stu-id="6b11d-131">Create a connection string using a shared access signature</span></span>
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a><span data-ttu-id="6b11d-132">Vytvoření připojovacího řetězce pro koncový bod explicitní úložiště</span><span class="sxs-lookup"><span data-stu-id="6b11d-132">Create a connection string for an explicit storage endpoint</span></span>
<span data-ttu-id="6b11d-133">Koncové body explicitní služby můžete zadat v připojovací řetězec místo použití výchozí hello koncové body.</span><span class="sxs-lookup"><span data-stu-id="6b11d-133">You can specify explicit service endpoints in your connection string instead of using hello default endpoints.</span></span> <span data-ttu-id="6b11d-134">toocreate připojovací řetězec, který určuje explicitní koncový bod, zadejte hello koncový bod dokončení služby pro jednotlivé služby včetně hello specifikace protokolu (HTTPS (doporučeno) nebo HTTP), v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="6b11d-134">toocreate a connection string that specifies an explicit endpoint, specify hello complete service endpoint for each service, including hello protocol specification (HTTPS (recommended) or HTTP), in hello following format:</span></span>

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

<span data-ttu-id="6b11d-135">Jeden scénář, kde byste mohli toospecify explicitní koncový bod je, když jste změnili vaší tooa koncový bod úložiště objektů Blob [vlastní domény](storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="6b11d-135">One scenario where you might wish toospecify an explicit endpoint is when you've mapped your Blob storage endpoint tooa [custom domain](storage-custom-domain-name.md).</span></span> <span data-ttu-id="6b11d-136">V takovém případě můžete zadat svůj vlastní koncový bod pro úložiště objektů Blob v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="6b11d-136">In that case, you can specify your custom endpoint for Blob storage in your connection string.</span></span> <span data-ttu-id="6b11d-137">Volitelně můžete zadat hello výchozí koncové body pro hello jiných služeb Pokud vaše aplikace používá je.</span><span class="sxs-lookup"><span data-stu-id="6b11d-137">You can optionally specify hello default endpoints for hello other services if your application uses them.</span></span>

<span data-ttu-id="6b11d-138">Tady je příklad připojovacího řetězce, který určuje explicitní koncový bod pro hello služby objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="6b11d-138">Here is an example of a connection string that specifies an explicit endpoint for hello Blob service:</span></span>

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="6b11d-139">Tento příklad určuje explicitní koncových bodů pro všechny služby, včetně vlastní doménu pro hello služby objektů Blob:</span><span class="sxs-lookup"><span data-stu-id="6b11d-139">This example specifies explicit endpoints for all services, including a custom domain for hello Blob service:</span></span>

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

<span data-ttu-id="6b11d-140">jsou použité tooconstruct hello žádost služby úložiště toohello identifikátory URI Hello koncový bod hodnoty v připojovacím řetězci a stanovují hello formu všechny identifikátory URI, který se vrátil kód tooyour.</span><span class="sxs-lookup"><span data-stu-id="6b11d-140">hello endpoint values in a connection string are used tooconstruct hello request URIs toohello storage services, and dictate hello form of any URIs that are returned tooyour code.</span></span>

<span data-ttu-id="6b11d-141">Pokud jste namapovat vlastní doménu tooa koncový bod úložiště a pak už nebudete moct toouse vynechejte tohoto koncového bodu z připojovacího řetězce, že připojovací řetězec tooaccess data v této službě z vašeho kódu.</span><span class="sxs-lookup"><span data-stu-id="6b11d-141">If you've mapped a storage endpoint tooa custom domain and omit that endpoint from a connection string, then you will not be able toouse that connection string tooaccess data in that service from your code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6b11d-142">Hodnoty koncového bodu služby v připojovací řetězce musí být ve správném formátu identifikátory URI, včetně `https://` (doporučeno) nebo `http://`.</span><span class="sxs-lookup"><span data-stu-id="6b11d-142">Service endpoint values in your connection strings must be well-formed URIs, including `https://` (recommended) or `http://`.</span></span> <span data-ttu-id="6b11d-143">Protože Azure Storage ještě nepodporuje protokol HTTPS pro vlastní domény, můžete *musí* zadejte `http://` pro libovolný koncový bod identifikátor URI, který ukazuje tooa vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="6b11d-143">Because Azure Storage does not yet support HTTPS for custom domains, you *must* specify `http://` for any endpoint URI that points tooa custom domain.</span></span>
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a><span data-ttu-id="6b11d-144">Vytvoření připojovacího řetězce s příponou koncový bod</span><span class="sxs-lookup"><span data-stu-id="6b11d-144">Create a connection string with an endpoint suffix</span></span>
<span data-ttu-id="6b11d-145">toocreate připojovací řetězec pro službu úložiště v oblasti nebo instancí s přípon jinému koncovému bodu, například pro Azure China nebo Azure Government, hello použijte následující formát řetězce připojení.</span><span class="sxs-lookup"><span data-stu-id="6b11d-145">toocreate a connection string for a storage service in regions or instances with different endpoint suffixes, such as for Azure China or Azure Government, use hello following connection string format.</span></span> <span data-ttu-id="6b11d-146">Označuje, zda chcete účet úložiště toohello tooconnect prostřednictvím protokolu HTTPS (doporučeno) nebo protokolu HTTP, nahraďte `myAccountName` nahradil hello název svého účtu úložiště, `myAccountKey` se přístupový klíč účtu a nahraďte `mySuffix` s hello identifikátor URI Přípona:</span><span class="sxs-lookup"><span data-stu-id="6b11d-146">Indicate whether you want tooconnect toohello storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with hello name of your storage account, replace `myAccountKey` with your account access key, and replace `mySuffix` with hello URI suffix:</span></span>

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

<span data-ttu-id="6b11d-147">Tady je příklad řetězce připojení pro služby úložiště v Číně Azure:</span><span class="sxs-lookup"><span data-stu-id="6b11d-147">Here's an example connection string for storage services in Azure China:</span></span>

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a><span data-ttu-id="6b11d-148">Analýza připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="6b11d-148">Parsing a connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a><span data-ttu-id="6b11d-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b11d-149">Next steps</span></span>
* [<span data-ttu-id="6b11d-150">Používejte hello emulátoru úložiště Azure pro vývoj a testování</span><span class="sxs-lookup"><span data-stu-id="6b11d-150">Use hello Azure storage emulator for development and testing</span></span>](storage-use-emulator.md)
* [<span data-ttu-id="6b11d-151">Azure průzkumníci úložiště</span><span class="sxs-lookup"><span data-stu-id="6b11d-151">Azure Storage explorers</span></span>](storage-explorers.md)
* [<span data-ttu-id="6b11d-152">Použití sdílených přístupových podpisů (SAS)</span><span class="sxs-lookup"><span data-stu-id="6b11d-152">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)

