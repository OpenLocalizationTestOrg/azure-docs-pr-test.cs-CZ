---
title: "Nahrání souborů do účtu Media Services pomocí REST | Microsoft Docs"
description: "Další informace o získání mediálního obsahu ve službě Media Services pomocí vytvoření a odeslání prostředky."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 41df7cbe-b8e2-48c1-a86c-361ec4e5251f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 955356ffe6fc524c1528364add7e2c2a336137b7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a><span data-ttu-id="cecef-103">Nahrání souborů do účtu Media Services pomocí REST</span><span class="sxs-lookup"><span data-stu-id="cecef-103">Upload files into a Media Services account using REST</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cecef-104">.NET</span><span class="sxs-lookup"><span data-stu-id="cecef-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="cecef-105">REST</span><span class="sxs-lookup"><span data-stu-id="cecef-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="cecef-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cecef-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="cecef-107">Ve službě Media Services můžete digitální soubory nahrát do assetu.</span><span class="sxs-lookup"><span data-stu-id="cecef-107">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="cecef-108">[Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entita může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a metadata o těchto souborech.)  Jakmile soubory odešlete do assetu, váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="cecef-108">The [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.)  Once the files are uploaded into the asset, your content is stored securely in the cloud for further processing and streaming.</span></span> 

> [!NOTE]
> <span data-ttu-id="cecef-109">Platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="cecef-109">The following considerations apply:</span></span>
> 
> * <span data-ttu-id="cecef-110">Služba Media Services použije hodnotu vlastnosti IAssetFile.Name při sestavování adresy URL pro streamování obsah (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech.</span><span class="sxs-lookup"><span data-stu-id="cecef-110">Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="cecef-111">Hodnota **název** vlastnost nemůže mít žádné z následujících [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="cecef-111">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="cecef-112">Navíc může existovat pouze jedna '.' pro příponu názvu souboru.</span><span class="sxs-lookup"><span data-stu-id="cecef-112">Also, there can only be one '.' for the file name extension.</span></span>
> * <span data-ttu-id="cecef-113">Délka názvu nesmí být větší než 260 znaků.</span><span class="sxs-lookup"><span data-stu-id="cecef-113">The length of the name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="cecef-114">Maximální velikost souboru podporovaná při zpracování ve službě Media Services je omezená.</span><span class="sxs-lookup"><span data-stu-id="cecef-114">There is a limit to the maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="cecef-115">Podrobnosti o omezení velikosti souboru najdete [tady](media-services-quotas-and-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="cecef-115">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
> 

<span data-ttu-id="cecef-116">Základní pracovní postup pro odesílání prostředky je rozdělené do následujících částí:</span><span class="sxs-lookup"><span data-stu-id="cecef-116">The basic workflow for uploading Assets is divided into the following sections:</span></span>

* <span data-ttu-id="cecef-117">Vytvořit prostředek</span><span class="sxs-lookup"><span data-stu-id="cecef-117">Create an Asset</span></span>
* <span data-ttu-id="cecef-118">Šifrování prostředek (volitelné)</span><span class="sxs-lookup"><span data-stu-id="cecef-118">Encrypt an Asset (Optional)</span></span>
* <span data-ttu-id="cecef-119">Nahrát soubor do úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="cecef-119">Upload a file to blob storage</span></span>

<span data-ttu-id="cecef-120">AMS můžete také nahrát prostředky hromadně.</span><span class="sxs-lookup"><span data-stu-id="cecef-120">AMS also enables you to upload assets in bulk.</span></span> <span data-ttu-id="cecef-121">Další informace najdete v [tomto](media-services-rest-upload-files.md#upload_in_bulk) oddílu.</span><span class="sxs-lookup"><span data-stu-id="cecef-121">For more information, see [this](media-services-rest-upload-files.md#upload_in_bulk) section.</span></span>

> [!NOTE]
> <span data-ttu-id="cecef-122">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="cecef-122">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="cecef-123">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="cecef-123">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
> 

## <a name="connect-to-media-services"></a><span data-ttu-id="cecef-124">Připojení ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="cecef-124">Connect to Media Services</span></span>

<span data-ttu-id="cecef-125">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="cecef-125">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="cecef-126">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="cecef-126">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="cecef-127">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="cecef-127">You must make subsequent calls to the new URI.</span></span>

## <a name="upload-assets"></a><span data-ttu-id="cecef-128">Nahrát prostředky</span><span class="sxs-lookup"><span data-stu-id="cecef-128">Upload assets</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="cecef-129">Vytvořit prostředek</span><span class="sxs-lookup"><span data-stu-id="cecef-129">Create an asset</span></span>

<span data-ttu-id="cecef-130">Prostředek je kontejner pro více typů nebo sady objektů ve službě Media Services, včetně video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků.</span><span class="sxs-lookup"><span data-stu-id="cecef-130">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="cecef-131">Vytvoření prostředek v rozhraní REST API vyžaduje odesílání požadavku POST ke službě Media Services a umístění žádné vlastnosti informace o váš asset v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="cecef-131">In the REST API, creating an Asset requires sending POST request to Media Services and placing any property information about your asset in the request body.</span></span>

<span data-ttu-id="cecef-132">Jedna z vlastností, které můžete zadat při vytváření prostředek je **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="cecef-132">One of the properties that you can specify when creating an asset is **Options**.</span></span> <span data-ttu-id="cecef-133">**Možnosti** je hodnota výčtu, která popisuje možnosti šifrování, které prostředek může být vytvořen pomocí.</span><span class="sxs-lookup"><span data-stu-id="cecef-133">**Options** is an enumeration value that describes the encryption options that an Asset can be created with.</span></span> <span data-ttu-id="cecef-134">Platná hodnota je jedna z hodnot v seznamu níže není kombinace hodnot.</span><span class="sxs-lookup"><span data-stu-id="cecef-134">A valid value is one of the values from the list below, not a combination of values.</span></span> 

* <span data-ttu-id="cecef-135">**Žádný** = **0**: použije se žádné šifrování.</span><span class="sxs-lookup"><span data-stu-id="cecef-135">**None** = **0**: No encryption will be used.</span></span> <span data-ttu-id="cecef-136">Toto je výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="cecef-136">This is the default value.</span></span> <span data-ttu-id="cecef-137">Všimněte si, že při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.</span><span class="sxs-lookup"><span data-stu-id="cecef-137">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="cecef-138">Pokud chcete pomocí progresivního stahování dodávat obsah ve formátu MP4, použijte tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="cecef-138">If you plan to deliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="cecef-139">**StorageEncrypted** = **1**: Zadejte, pokud chcete použít pro vaše soubory k zašifrování s AES 256 bitové šifrování pro odesílání a úložiště.</span><span class="sxs-lookup"><span data-stu-id="cecef-139">**StorageEncrypted** = **1**: Specify if you want for your files to be encrypted with AES-256 bit encryption for upload and storage.</span></span>
  
    <span data-ttu-id="cecef-140">Pokud váš asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="cecef-140">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="cecef-141">Další informace najdete v části [konfigurace zásad doručení assetu](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="cecef-141">For more information see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>
* <span data-ttu-id="cecef-142">**CommonEncryptionProtected** = **2**: Zadejte, pokud odesíláte soubory chráněné pomocí běžnou metodu šifrování (např. PlayReady).</span><span class="sxs-lookup"><span data-stu-id="cecef-142">**CommonEncryptionProtected** = **2**: Specify if you are uploading files protected with a common encryption method (such as PlayReady).</span></span> 
* <span data-ttu-id="cecef-143">**EnvelopeEncryptionProtected** = **4**: Zadejte, pokud odesíláte HLS se šifrováním pomocí standardu AES soubory.</span><span class="sxs-lookup"><span data-stu-id="cecef-143">**EnvelopeEncryptionProtected** = **4**: Specify if you are uploading HLS encrypted with AES files.</span></span> <span data-ttu-id="cecef-144">Pamatujte, že soubory musí být zakódované a zašifrované pomocí správce transformací.</span><span class="sxs-lookup"><span data-stu-id="cecef-144">Note that the files must have been encoded and encrypted by Transform Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="cecef-145">Pokud váš asset používat šifrování, musíte vytvořit **ContentKey** a tu propojit na váš asset, jak je popsáno v následujícím tématu:[postup vytvoření ContentKey](media-services-rest-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="cecef-145">If your asset will use encryption, you must create a **ContentKey** and link it to your asset as described in the following topic:[How to create a ContentKey](media-services-rest-create-contentkey.md).</span></span> <span data-ttu-id="cecef-146">Poznámka: po odeslání souborů do assetu, budete muset aktualizovat vlastnosti šifrování na **AssetFile** entity s hodnotami, které jste získali při **Asset** šifrování.</span><span class="sxs-lookup"><span data-stu-id="cecef-146">Note that after you upload the files into the asset, you need to update the encryption properties on the **AssetFile** entity with the values you got during the **Asset** encryption.</span></span> <span data-ttu-id="cecef-147">To provést pomocí **SLOUČENÍ** požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="cecef-147">Do it by using the **MERGE** HTTP request.</span></span> 
> 
> 

<span data-ttu-id="cecef-148">Následující příklad ukazuje, jak vytvořit prostředek.</span><span class="sxs-lookup"><span data-stu-id="cecef-148">The following example shows how to create an asset.</span></span>

<span data-ttu-id="cecef-149">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-149">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"BigBuckBunny.mp4"}

<span data-ttu-id="cecef-150">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-150">**HTTP Response**</span></span>

<span data-ttu-id="cecef-151">V případě úspěchu se vrátí následující:</span><span class="sxs-lookup"><span data-stu-id="cecef-151">If successful, the following is returned:</span></span>

    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a><span data-ttu-id="cecef-152">Vytvoření AssetFile</span><span class="sxs-lookup"><span data-stu-id="cecef-152">Create an AssetFile</span></span>
<span data-ttu-id="cecef-153">[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity představuje soubor video nebo zvuk, který je uložený v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="cecef-153">The [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="cecef-154">Soubor asset je vždy přidružena k assetu a prostředek může obsahovat mnoho soubory asset.</span><span class="sxs-lookup"><span data-stu-id="cecef-154">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="cecef-155">Media Services Encoder úloh selže, pokud objekt souboru asset není spojen s digitálnímu souboru v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="cecef-155">The Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="cecef-156">Všimněte si, že **AssetFile** instance a samotný mediální soubor jsou dva odlišné objekty.</span><span class="sxs-lookup"><span data-stu-id="cecef-156">Note that the **AssetFile** instance and the actual media file are two distinct objects.</span></span> <span data-ttu-id="cecef-157">AssetFile instance obsahuje metadata o souboru média, zatímco souboru média obsahuje samotný mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="cecef-157">The AssetFile instance contains metadata about the media file, while the media file contains the actual media content.</span></span>

<span data-ttu-id="cecef-158">Po odeslání souboru digitálního média do kontejneru objektů blob, kterou použijete **SLOUČENÍ** HTTP žádost o aktualizaci AssetFile s informacemi o souboru média (Jak uvidíte později v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="cecef-158">After you upload your digital media file into a blob container, you will use the **MERGE** HTTP request to update the AssetFile with information about your media file (as shown later in the topic).</span></span> 

<span data-ttu-id="cecef-159">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-159">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

<span data-ttu-id="cecef-160">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-160">**HTTP Response**</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }

### <a name="creating-the-accesspolicy-with-write-permission"></a><span data-ttu-id="cecef-161">Vytváření AccessPolicy s oprávnění k zápisu.</span><span class="sxs-lookup"><span data-stu-id="cecef-161">Creating the AccessPolicy with write permission.</span></span>

>[!NOTE]
><span data-ttu-id="cecef-162">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="cecef-162">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="cecef-163">Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad.</span><span class="sxs-lookup"><span data-stu-id="cecef-163">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="cecef-164">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="cecef-164">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="cecef-165">Před nahráním do úložiště objektů blob všechny soubory, nastavení přístupu zásady oprávnění pro zápis do prostředek.</span><span class="sxs-lookup"><span data-stu-id="cecef-165">Before uploading any files into blob storage, set the access policy rights for writing to an asset.</span></span> <span data-ttu-id="cecef-166">Kvůli tomu odeslat požadavek HTTP do sady entit AccessPolicies.</span><span class="sxs-lookup"><span data-stu-id="cecef-166">To do that, POST an HTTP request to the AccessPolicies entity set.</span></span> <span data-ttu-id="cecef-167">Zadejte hodnotu doba trvání v minutách, po vytvoření nebo obdržíte zprávu 500 Chyba interní Server zpět v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="cecef-167">Define a DurationInMinutes value upon creation or you will receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="cecef-168">Další informace o AccessPolicies najdete v tématu [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="cecef-168">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="cecef-169">Následující příklad ukazuje, jak vytvořit AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="cecef-169">The following example shows how to create an AccessPolicy:</span></span>

<span data-ttu-id="cecef-170">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-170">**HTTP Request**</span></span>

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

<span data-ttu-id="cecef-171">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-171">**HTTP Request**</span></span>

    If successful, the following response is returned:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a><span data-ttu-id="cecef-172">Získat adresu URL pro odeslání</span><span class="sxs-lookup"><span data-stu-id="cecef-172">Get the Upload URL</span></span>
<span data-ttu-id="cecef-173">Pokud chcete získat adresu URL skutečného odeslání, vytvoření lokátoru SAS.</span><span class="sxs-lookup"><span data-stu-id="cecef-173">To receive the actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="cecef-174">Lokátory zadejte čas spuštění a typ koncového bodu připojení pro klienty, kteří mají přístup k souborům v prostředek.</span><span class="sxs-lookup"><span data-stu-id="cecef-174">Locators define the start time and type of connection endpoint for clients that want to access Files in an Asset.</span></span> <span data-ttu-id="cecef-175">Můžete vytvořit více Lokátor entit pro danou AccessPolicy a Asset dvojici pro zpracování různých klientských požadavků a potřebách.</span><span class="sxs-lookup"><span data-stu-id="cecef-175">You can create multiple Locator entities for a given AccessPolicy and Asset pair to handle different client requests and needs.</span></span> <span data-ttu-id="cecef-176">Každý z těchto lokátory pomocí hodnoty StartTime plus hodnota Doba trvání v minutách AccessPolicy můžete určit dobu, kterou lze použít adresu URL.</span><span class="sxs-lookup"><span data-stu-id="cecef-176">Each of these Locators use the StartTime value plus the DurationInMinutes value of the AccessPolicy to determine the length of time a URL can be used.</span></span> <span data-ttu-id="cecef-177">Další informace najdete v tématu [Lokátor](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="cecef-177">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="cecef-178">Adresa URL typu SAS má následující formát:</span><span class="sxs-lookup"><span data-stu-id="cecef-178">A SAS URL has the following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="cecef-179">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="cecef-179">Some considerations apply:</span></span>

* <span data-ttu-id="cecef-180">Nemůže mít více než pět jedinečný lokátory spojené s danou Asset najednou.</span><span class="sxs-lookup"><span data-stu-id="cecef-180">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="cecef-181">Další informace najdete v tématu lokátoru.</span><span class="sxs-lookup"><span data-stu-id="cecef-181">For more information, see Locator.</span></span>
* <span data-ttu-id="cecef-182">Pokud potřebujete k nahrání souborů okamžitě, byste měli nastavit vaše hodnoty StartTime 5 minut před aktuálním časem.</span><span class="sxs-lookup"><span data-stu-id="cecef-182">If you need to upload your files immediately, you should set your StartTime value to five minutes before the current time.</span></span> <span data-ttu-id="cecef-183">Je to proto, že je možné, hodiny zkosení mezi klientský počítač a služba Media Services.</span><span class="sxs-lookup"><span data-stu-id="cecef-183">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="cecef-184">V následujícím formátu data a času musí být také hodnota pro čas spuštění: rrrr-MM-ddTHH (například "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="cecef-184">Also, your StartTime value must be in the following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="cecef-185">Může být druhý 30-40 zpoždění po vytvoření lokátoru k případě, že je k dispozici pro použití.</span><span class="sxs-lookup"><span data-stu-id="cecef-185">There may be a 30-40 second delay after a Locator is created to when it is available for use.</span></span> <span data-ttu-id="cecef-186">Tento problém se vztahuje na SAS adresa URL a lokátory původu.</span><span class="sxs-lookup"><span data-stu-id="cecef-186">This issue applies to both SAS URL and Origin Locators.</span></span>

<span data-ttu-id="cecef-187">Následující příklad ukazuje, jak vytvořit lokátor SAS adresa URL, podle definice vlastnost typu v textu požadavku ("1" pro Lokátor SAS) a "2" pro Lokátor původ na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="cecef-187">The following example shows how to create a SAS URL Locator, as defined by the Type property in the request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="cecef-188">**Cesta** vlastnost vrátil obsahuje adresu URL, kterou musíte použít k odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="cecef-188">The **Path** property returned contains the URL that you must use to upload your file.</span></span>

<span data-ttu-id="cecef-189">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-189">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }

<span data-ttu-id="cecef-190">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-190">**HTTP Response**</span></span>

<span data-ttu-id="cecef-191">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="cecef-191">If successful, the following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="cecef-192">Nahrát soubor do kontejneru úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="cecef-192">Upload a file into a blob storage container</span></span>
<span data-ttu-id="cecef-193">Jakmile máte AccessPolicy a Lokátor nastavit, skutečný soubor nahraje do kontejneru Azure Blob Storage pomocí rozhraní API REST úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="cecef-193">Once you have the AccessPolicy and Locator set, the actual file is uploaded to an Azure Blob Storage container using the Azure Storage REST APIs.</span></span> <span data-ttu-id="cecef-194">Soubory musíte nahrát jako objekty BLOB bloku.</span><span class="sxs-lookup"><span data-stu-id="cecef-194">You must upload the files as block blobs.</span></span> <span data-ttu-id="cecef-195">Objekty BLOB stránky nejsou podporovány službou Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="cecef-195">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="cecef-196">Musíte přidat název souboru pro soubor, který chcete nahrát do Lokátor **cesta** přijaté v předchozí části hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cecef-196">You must add the file name for the file you want to upload to the Locator **Path** value received in the previous section.</span></span> <span data-ttu-id="cecef-197">Například https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="cecef-197">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="cecef-198">.</span><span class="sxs-lookup"><span data-stu-id="cecef-198">.</span></span> <span data-ttu-id="cecef-199">.</span><span class="sxs-lookup"><span data-stu-id="cecef-199">.</span></span> <span data-ttu-id="cecef-200">.</span><span class="sxs-lookup"><span data-stu-id="cecef-200">.</span></span> 
> 
> 

<span data-ttu-id="cecef-201">Další informace o práci s objekty BLOB úložiště Azure najdete v tématu [rozhraní API REST služby objektů Blob](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="cecef-201">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-the-assetfile"></a><span data-ttu-id="cecef-202">Aktualizace AssetFile</span><span class="sxs-lookup"><span data-stu-id="cecef-202">Update the AssetFile</span></span>
<span data-ttu-id="cecef-203">Teď, když jste odeslali souboru, aktualizujte informace FileAsset velikost (a další).</span><span class="sxs-lookup"><span data-stu-id="cecef-203">Now that you've uploaded your file, update the FileAsset size (and other) information.</span></span> <span data-ttu-id="cecef-204">Například:</span><span class="sxs-lookup"><span data-stu-id="cecef-204">For example:</span></span>

    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


<span data-ttu-id="cecef-205">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-205">**HTTP Response**</span></span>

<span data-ttu-id="cecef-206">Pokud úspěšné, následující, je vrácena: HTTP/1.1 204 ne obsahu</span><span class="sxs-lookup"><span data-stu-id="cecef-206">If successful, the following is returned: HTTP/1.1 204 No Content</span></span>

### <a name="delete-the-locator-and-accesspolicy"></a><span data-ttu-id="cecef-207">Odstraňte Lokátor a AccessPolicy</span><span class="sxs-lookup"><span data-stu-id="cecef-207">Delete the Locator and AccessPolicy</span></span>
<span data-ttu-id="cecef-208">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-208">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="cecef-209">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-209">**HTTP Response**</span></span>

<span data-ttu-id="cecef-210">V případě úspěchu se vrátí následující:</span><span class="sxs-lookup"><span data-stu-id="cecef-210">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

<span data-ttu-id="cecef-211">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-211">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="cecef-212">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-212">**HTTP Response**</span></span>

<span data-ttu-id="cecef-213">V případě úspěchu se vrátí následující:</span><span class="sxs-lookup"><span data-stu-id="cecef-213">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

## <span data-ttu-id="cecef-214"><a id="upload_in_bulk"></a>Nahrát prostředky hromadně</span><span class="sxs-lookup"><span data-stu-id="cecef-214"><a id="upload_in_bulk"></a>Upload assets in bulk</span></span>
### <a name="create-the-ingestmanifest"></a><span data-ttu-id="cecef-215">Vytvořte IngestManifest</span><span class="sxs-lookup"><span data-stu-id="cecef-215">Create the IngestManifest</span></span>
<span data-ttu-id="cecef-216">IngestManifest je kontejner pro sadu prostředky, soubory prostředků a statistiky informace, které můžete použít k určení průběh hromadné příjem pro sadu.</span><span class="sxs-lookup"><span data-stu-id="cecef-216">The IngestManifest is a container for a set of assets, asset files, and statistic information that can be used to determine the progress of bulk ingesting for the set.</span></span>

<span data-ttu-id="cecef-217">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-217">**HTTP Request**</span></span>

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue

    { "Name" : "ExampleManifestREST" }

### <a name="create-assets"></a><span data-ttu-id="cecef-218">Vytvořit prostředky</span><span class="sxs-lookup"><span data-stu-id="cecef-218">Create assets</span></span>
<span data-ttu-id="cecef-219">Před vytvořením IngestManifestAsset, musíte vytvořit Asset, který bude možné provést pomocí hromadné příjem.</span><span class="sxs-lookup"><span data-stu-id="cecef-219">Before creating the IngestManifestAsset, you need to create the Asset that will be completed using bulk ingesting.</span></span> <span data-ttu-id="cecef-220">Prostředek je kontejner pro více typů nebo sady objektů ve službě Media Services, včetně video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků.</span><span class="sxs-lookup"><span data-stu-id="cecef-220">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="cecef-221">Vytvoření prostředek v rozhraní REST API vyžaduje odesílání požadavku HTTP POST do Microsoft Azure Media Services a umístění žádné vlastnosti informace o váš asset v textu požadavku. V tomto příkladu se vytvoří Asset pomocí možnosti StorageEncrption(1) zahrnuté do textu žádosti.</span><span class="sxs-lookup"><span data-stu-id="cecef-221">In the REST API, creating an Asset requires sending a HTTP POST request to Microsoft Azure Media Services and placing any property information about your asset in the request body.In this example, the Asset is created using the StorageEncrption(1) option included with the request body.</span></span>

<span data-ttu-id="cecef-222">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-222">**HTTP Response**</span></span>

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue

    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

### <a name="create-the-ingestmanifestassets"></a><span data-ttu-id="cecef-223">Vytvořte IngestManifestAssets</span><span class="sxs-lookup"><span data-stu-id="cecef-223">Create the IngestManifestAssets</span></span>
<span data-ttu-id="cecef-224">IngestManifestAssets představují prostředky v rámci IngestManifest, které se používají s hromadné příjem.</span><span class="sxs-lookup"><span data-stu-id="cecef-224">IngestManifestAssets represent Assets within an IngestManifest that are used with bulk ingesting.</span></span> <span data-ttu-id="cecef-225">V podstatě propojit manifest prostředku.</span><span class="sxs-lookup"><span data-stu-id="cecef-225">The basically link the asset to the manifest.</span></span> <span data-ttu-id="cecef-226">Azure Media Services sleduje interně pro nahrávání souborů na základě IngestManifestFiles kolekce přidružené k IngestManifestAsset.</span><span class="sxs-lookup"><span data-stu-id="cecef-226">Azure Media Services watches internally for the file upload based on IngestManifestFiles collection associated to the IngestManifestAsset.</span></span> <span data-ttu-id="cecef-227">Jakmile tyto soubory jsou odeslány, asset byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="cecef-227">Once these files are uploaded, the asset is completed.</span></span> <span data-ttu-id="cecef-228">Můžete vytvořit nové IngestManifestAsset s požadavek HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="cecef-228">You can create a new IngestManifestAsset with a HTTP POST request.</span></span> <span data-ttu-id="cecef-229">V textu požadavku zahrnují IngestManifest Id a Asset Id, které IngestManifestAsset by měl společně odkaz pro příjem hromadně.</span><span class="sxs-lookup"><span data-stu-id="cecef-229">In the request body, include the IngestManifest Id and the Asset Id that the IngestManifestAsset should link together for bulk ingesting.</span></span>

<span data-ttu-id="cecef-230">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-230">**HTTP Response**</span></span>

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


### <a name="create-the-ingestmanifestfiles-for-each-asset"></a><span data-ttu-id="cecef-231">Vytvořte IngestManifestFiles pro každý prostředek</span><span class="sxs-lookup"><span data-stu-id="cecef-231">Create the IngestManifestFiles for each Asset</span></span>
<span data-ttu-id="cecef-232">IngestManifestFile představuje skutečný video nebo zvuk blob objekt, který v rámci hromadné příjem pro určitý prostředek, nebude možné odesílat.</span><span class="sxs-lookup"><span data-stu-id="cecef-232">An IngestManifestFile represents an actual video or audio blob object that will be uploaded as part of bulk ingesting for an asset.</span></span> <span data-ttu-id="cecef-233">Vlastnosti nejsou vyžadovány, pokud asset používá možnost šifrování související s šifrování.</span><span class="sxs-lookup"><span data-stu-id="cecef-233">Encryption related properties are not required unless the asset is using an encryption option.</span></span> <span data-ttu-id="cecef-234">Příkladu v této části ukazuje, že vytvoření IngestManifestFile, který používá StorageEncryption pro prostředek vytvořili.</span><span class="sxs-lookup"><span data-stu-id="cecef-234">The example used in this section demonstrates creating an IngestManifestFile that uses StorageEncryption for the Asset previously created.</span></span>

<span data-ttu-id="cecef-235">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-235">**HTTP Response**</span></span>

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue

    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }

### <a name="upload-the-files-to-blob-storage"></a><span data-ttu-id="cecef-236">Odeslat soubory do úložiště objektů Blob</span><span class="sxs-lookup"><span data-stu-id="cecef-236">Upload the Files to Blob Storage</span></span>
<span data-ttu-id="cecef-237">Můžete použít libovolná aplikace klienta vysokorychlostní schopná nahrávání souborů asset ke kontejneru úložiště objektů blob Uri poskytované vlastnost BlobStorageUriForUpload IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="cecef-237">You can use any high speed client application capable of uploading the asset files to the blob storage container Uri provided by the BlobStorageUriForUpload property of the IngestManifest.</span></span> <span data-ttu-id="cecef-238">Je jedna služba nahrávání významné vysokorychlostní [Aspera na vyžádání pro aplikaci Azure](http://go.microsoft.com/fwlink/?LinkId=272001).</span><span class="sxs-lookup"><span data-stu-id="cecef-238">One notable high speed upload service is [Aspera On Demand for Azure Application](http://go.microsoft.com/fwlink/?LinkId=272001).</span></span>

### <a name="monitor-bulk-ingest-progress"></a><span data-ttu-id="cecef-239">Monitorování hromadné Ingestování průběh</span><span class="sxs-lookup"><span data-stu-id="cecef-239">Monitor Bulk Ingest Progress</span></span>
<span data-ttu-id="cecef-240">Můžete sledovat průběh hromadné příjem operací pro IngestManifest pomocí cyklického dotazování vlastnost statistiky IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="cecef-240">You can monitor the progress of bulk ingesting operations for an IngestManifest by polling the Statistics property of the IngestManifest.</span></span> <span data-ttu-id="cecef-241">Zda je vlastnost komplexního typu, [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span><span class="sxs-lookup"><span data-stu-id="cecef-241">That property is a complex type, [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span></span> <span data-ttu-id="cecef-242">Dotazování vlastnost statistiky, odešlete požadavek HTTP GET předáním IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="cecef-242">To poll the Statistics property, submit a HTTP GET request passing the IngestManifest Id.</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="cecef-243">Vytvoření ContentKeys pro šifrování</span><span class="sxs-lookup"><span data-stu-id="cecef-243">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="cecef-244">Pokud váš asset používat šifrování, musíte vytvořit ContentKey, který se má použít pro šifrování před vytvořením soubory prostředků.</span><span class="sxs-lookup"><span data-stu-id="cecef-244">If your asset will use encryption, you must create the ContentKey to be used for encryption before creating the asset files.</span></span> <span data-ttu-id="cecef-245">Šifrování úložiště by měla zahrnovat následující vlastnosti v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="cecef-245">For storage encryption, the following properties should be included in the request body.</span></span>

| <span data-ttu-id="cecef-246">Vlastnost text žádosti</span><span class="sxs-lookup"><span data-stu-id="cecef-246">Request body property</span></span> | <span data-ttu-id="cecef-247">Popis</span><span class="sxs-lookup"><span data-stu-id="cecef-247">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cecef-248">ID</span><span class="sxs-lookup"><span data-stu-id="cecef-248">Id</span></span> |<span data-ttu-id="cecef-249">ContentKey Id, které jsme si generovat v následujícím formátu "nb:kid:UUID:<NEW GUID>".</span><span class="sxs-lookup"><span data-stu-id="cecef-249">The ContentKey Id which we generate ourselves using the following format, “nb:kid:UUID:<NEW GUID>”.</span></span> |
| <span data-ttu-id="cecef-250">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="cecef-250">ContentKeyType</span></span> |<span data-ttu-id="cecef-251">Toto je typ obsahu klíče jako celé číslo pro tento klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="cecef-251">This is the content key type as an integer for this content key.</span></span> <span data-ttu-id="cecef-252">Jsme předejte hodnotu 1 pro šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="cecef-252">We pass the value 1 for storage encryption.</span></span> |
| <span data-ttu-id="cecef-253">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="cecef-253">EncryptedContentKey</span></span> |<span data-ttu-id="cecef-254">Vytvoříme novou hodnotu obsahu klíče, což je hodnota 256 bitů (32 bajtů).</span><span class="sxs-lookup"><span data-stu-id="cecef-254">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="cecef-255">Že je klíč zašifrovaný pomocí certifikátu X.509 šifrování úložiště, který načteme ze služby Microsoft Azure Media Services spuštěním požadavek HTTP GET pro GetProtectionKeyId a GetProtectionKey metody.</span><span class="sxs-lookup"><span data-stu-id="cecef-255">The key is encrypted using the storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for the GetProtectionKeyId and GetProtectionKey Methods.</span></span> |
| <span data-ttu-id="cecef-256">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="cecef-256">ProtectionKeyId</span></span> |<span data-ttu-id="cecef-257">Jedná se o ochranu id klíče pro certifikát X.509 šifrování úložiště, který slouží k šifrování naše klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="cecef-257">This is the protection key id for the storage encryption X.509 certificate that was used to encrypt our content key.</span></span> |
| <span data-ttu-id="cecef-258">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="cecef-258">ProtectionKeyType</span></span> |<span data-ttu-id="cecef-259">Jedná se o typ šifrování pro ochranu klíč, který slouží k šifrování klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="cecef-259">This is the encryption type for the protection key that was used to encrypt the content key.</span></span> <span data-ttu-id="cecef-260">Tato hodnota je StorageEncryption(1) pro náš příklad.</span><span class="sxs-lookup"><span data-stu-id="cecef-260">This value is StorageEncryption(1) for our example.</span></span> |
| <span data-ttu-id="cecef-261">Kontrolní součet</span><span class="sxs-lookup"><span data-stu-id="cecef-261">Checksum</span></span> |<span data-ttu-id="cecef-262">Algoritmus MD5 počítané kontrolního součtu pro klíč k obsahu.</span><span class="sxs-lookup"><span data-stu-id="cecef-262">The MD5 calculated checksum for the content key.</span></span> <span data-ttu-id="cecef-263">Výpočet je šifrování obsahu Id obsahu klíčem.</span><span class="sxs-lookup"><span data-stu-id="cecef-263">It is computed by encrypting the content Id with the content key.</span></span> <span data-ttu-id="cecef-264">Příklad kódu ukazuje, jak k výpočtu kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="cecef-264">The example code demonstrates how to calculate the checksum.</span></span> |

<span data-ttu-id="cecef-265">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-265">**HTTP Response**</span></span>

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue

    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a><span data-ttu-id="cecef-266">Odkaz ContentKey pro daný prostředek</span><span class="sxs-lookup"><span data-stu-id="cecef-266">Link the ContentKey to the Asset</span></span>
<span data-ttu-id="cecef-267">ContentKey je přidružena k jedné nebo více prostředků odesláním požadavku HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="cecef-267">The ContentKey is associated to one or more assets by sending a HTTP POST request.</span></span> <span data-ttu-id="cecef-268">Následující požadavek je příklad propojení příklad ContentKey pro daný prostředek příklad podle Id.</span><span class="sxs-lookup"><span data-stu-id="cecef-268">The following request is an example to link the example ContentKey to the example asset by Id.</span></span>

<span data-ttu-id="cecef-269">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-269">**HTTP Response**</span></span>

    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue

    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

<span data-ttu-id="cecef-270">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="cecef-270">**HTTP Response**</span></span>

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net

## <a name="next-steps"></a><span data-ttu-id="cecef-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cecef-271">Next steps</span></span>

<span data-ttu-id="cecef-272">Nyní můžete kódovat nahrané assety.</span><span class="sxs-lookup"><span data-stu-id="cecef-272">You can now encode your uploaded assets.</span></span> <span data-ttu-id="cecef-273">Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="cecef-273">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="cecef-274">Můžete také použít službu Azure Functions k aktivaci úlohy kódování při příchodu souboru do nakonfigurovaného kontejneru.</span><span class="sxs-lookup"><span data-stu-id="cecef-274">You can also use Azure Functions to trigger an encoding job based on a file arriving in the configured container.</span></span> <span data-ttu-id="cecef-275">Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="cecef-275">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="cecef-276">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="cecef-276">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="cecef-277">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="cecef-277">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[How to Get a Media Processor]: media-services-get-media-processor.md

