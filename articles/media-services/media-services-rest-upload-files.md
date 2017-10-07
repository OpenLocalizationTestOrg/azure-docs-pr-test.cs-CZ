---
title: "aaaUpload souborů do účtu Media Services pomocí REST | Microsoft Docs"
description: "Zjistěte, jak tooget média obsahu ve službě Media Services pomocí vytvoření a odeslání prostředky."
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
ms.openlocfilehash: 2a92cecdc32d747d7a478946f069c15931eb32b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a><span data-ttu-id="6e26a-103">Nahrání souborů do účtu Media Services pomocí REST</span><span class="sxs-lookup"><span data-stu-id="6e26a-103">Upload files into a Media Services account using REST</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e26a-104">.NET</span><span class="sxs-lookup"><span data-stu-id="6e26a-104">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="6e26a-105">REST</span><span class="sxs-lookup"><span data-stu-id="6e26a-105">REST</span></span>](media-services-rest-upload-files.md)
> * [<span data-ttu-id="6e26a-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6e26a-106">Portal</span></span>](media-services-portal-upload-files.md)
> 
> 

<span data-ttu-id="6e26a-107">Ve službě Media Services můžete digitální soubory nahrát do assetu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-107">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="6e26a-108">Hello [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entita může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a hello metadata o těchto souborech.)  Jakmile hello soubory jsou odeslány do hello asset, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="6e26a-108">hello [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded into hello asset, your content is stored securely in hello cloud for further processing and streaming.</span></span> 

> [!NOTE]
> <span data-ttu-id="6e26a-109">použít Hello následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="6e26a-109">hello following considerations apply:</span></span>
> 
> * <span data-ttu-id="6e26a-110">Služba Media Services použije hello hodnotu hello IAssetFile.Name vlastnost při sestavování adresy URL pro hello streamování obsahu (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech.</span><span class="sxs-lookup"><span data-stu-id="6e26a-110">Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="6e26a-111">Hello hodnotu hello **název** vlastnost nemůže mít žádné z následujících hello [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="6e26a-111">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="6e26a-112">Navíc může existovat pouze jedna '.' pro příponu názvu souboru hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-112">Also, there can only be one '.' for hello file name extension.</span></span>
> * <span data-ttu-id="6e26a-113">Délka Hello hello názvu by neměla být větší než 260 znaků.</span><span class="sxs-lookup"><span data-stu-id="6e26a-113">hello length of hello name should not be greater than 260 characters.</span></span>
> * <span data-ttu-id="6e26a-114">Existuje limit toohello maximální velikost souboru pro zpracování ve službě Media Services podporována.</span><span class="sxs-lookup"><span data-stu-id="6e26a-114">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="6e26a-115">Najdete v tématu [to](media-services-quotas-and-limitations.md) téma podrobné informace o omezení velikosti souborů hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-115">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
> 

<span data-ttu-id="6e26a-116">Hello základní pracovní postup pro odesílání prostředky je rozdělené do následujících částí hello:</span><span class="sxs-lookup"><span data-stu-id="6e26a-116">hello basic workflow for uploading Assets is divided into hello following sections:</span></span>

* <span data-ttu-id="6e26a-117">Vytvořit prostředek</span><span class="sxs-lookup"><span data-stu-id="6e26a-117">Create an Asset</span></span>
* <span data-ttu-id="6e26a-118">Šifrování prostředek (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6e26a-118">Encrypt an Asset (Optional)</span></span>
* <span data-ttu-id="6e26a-119">Nahrát soubor tooblob úložiště</span><span class="sxs-lookup"><span data-stu-id="6e26a-119">Upload a file tooblob storage</span></span>

<span data-ttu-id="6e26a-120">AMS taky umožňuje tooupload prostředky hromadně.</span><span class="sxs-lookup"><span data-stu-id="6e26a-120">AMS also enables you tooupload assets in bulk.</span></span> <span data-ttu-id="6e26a-121">Další informace najdete v [tomto](media-services-rest-upload-files.md#upload_in_bulk) oddílu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-121">For more information, see [this](media-services-rest-upload-files.md#upload_in_bulk) section.</span></span>

> [!NOTE]
> <span data-ttu-id="6e26a-122">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e26a-122">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="6e26a-123">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="6e26a-123">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
> 

## <a name="connect-toomedia-services"></a><span data-ttu-id="6e26a-124">Připojení služby tooMedia</span><span class="sxs-lookup"><span data-stu-id="6e26a-124">Connect tooMedia Services</span></span>

<span data-ttu-id="6e26a-125">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="6e26a-125">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="6e26a-126">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="6e26a-126">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="6e26a-127">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="6e26a-127">You must make subsequent calls toohello new URI.</span></span>

## <a name="upload-assets"></a><span data-ttu-id="6e26a-128">Nahrát prostředky</span><span class="sxs-lookup"><span data-stu-id="6e26a-128">Upload assets</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="6e26a-129">Vytvořit prostředek</span><span class="sxs-lookup"><span data-stu-id="6e26a-129">Create an asset</span></span>

<span data-ttu-id="6e26a-130">Prostředek je kontejner pro více typů nebo sady objektů ve službě Media Services, včetně video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků.</span><span class="sxs-lookup"><span data-stu-id="6e26a-130">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="6e26a-131">V rozhraní REST API, vytváření prostředek vyžaduje odesílání POST hello požadovat tooMedia služby a umístění žádné vlastnosti informace o váš asset v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-131">In hello REST API, creating an Asset requires sending POST request tooMedia Services and placing any property information about your asset in hello request body.</span></span>

<span data-ttu-id="6e26a-132">Jedna z vlastností hello, které můžete zadat při vytváření prostředek je **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="6e26a-132">One of hello properties that you can specify when creating an asset is **Options**.</span></span> <span data-ttu-id="6e26a-133">**Možnosti** je hodnota výčtu, která popisuje možnosti hello šifrování, které prostředek může být vytvořen pomocí.</span><span class="sxs-lookup"><span data-stu-id="6e26a-133">**Options** is an enumeration value that describes hello encryption options that an Asset can be created with.</span></span> <span data-ttu-id="6e26a-134">Platná hodnota je jedna z hodnot hello hello seznamu níže, není kombinace hodnot.</span><span class="sxs-lookup"><span data-stu-id="6e26a-134">A valid value is one of hello values from hello list below, not a combination of values.</span></span> 

* <span data-ttu-id="6e26a-135">**Žádný** = **0**: použije se žádné šifrování.</span><span class="sxs-lookup"><span data-stu-id="6e26a-135">**None** = **0**: No encryption will be used.</span></span> <span data-ttu-id="6e26a-136">Toto je výchozí hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-136">This is hello default value.</span></span> <span data-ttu-id="6e26a-137">Všimněte si, že při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.</span><span class="sxs-lookup"><span data-stu-id="6e26a-137">Note that when using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="6e26a-138">Tuto možnost použijte, pokud máte v plánu toodeliver MP4 pomocí progresivního stahování.</span><span class="sxs-lookup"><span data-stu-id="6e26a-138">If you plan toodeliver an MP4 using progressive download, use this option.</span></span> 
* <span data-ttu-id="6e26a-139">**StorageEncrypted** = **1**: Zadejte, pokud chcete použít pro vaše soubory toobe zašifrované pomocí 256bitového šifrování AES 256 pro odesílání a úložiště.</span><span class="sxs-lookup"><span data-stu-id="6e26a-139">**StorageEncrypted** = **1**: Specify if you want for your files toobe encrypted with AES-256 bit encryption for upload and storage.</span></span>
  
    <span data-ttu-id="6e26a-140">Pokud váš asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-140">If your asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="6e26a-141">Další informace najdete v části [konfigurace zásad doručení assetu](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6e26a-141">For more information see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>
* <span data-ttu-id="6e26a-142">**CommonEncryptionProtected** = **2**: Zadejte, pokud odesíláte soubory chráněné pomocí běžnou metodu šifrování (např. PlayReady).</span><span class="sxs-lookup"><span data-stu-id="6e26a-142">**CommonEncryptionProtected** = **2**: Specify if you are uploading files protected with a common encryption method (such as PlayReady).</span></span> 
* <span data-ttu-id="6e26a-143">**EnvelopeEncryptionProtected** = **4**: Zadejte, pokud odesíláte HLS se šifrováním pomocí standardu AES soubory.</span><span class="sxs-lookup"><span data-stu-id="6e26a-143">**EnvelopeEncryptionProtected** = **4**: Specify if you are uploading HLS encrypted with AES files.</span></span> <span data-ttu-id="6e26a-144">Všimněte si, že hello soubory musí být kódovaný a zašifrované pomocí Správce transformací.</span><span class="sxs-lookup"><span data-stu-id="6e26a-144">Note that hello files must have been encoded and encrypted by Transform Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="6e26a-145">Pokud váš asset používat šifrování, musíte vytvořit **ContentKey** a propojit jej tooyour asset, jak je popsáno v následujícím tématu hello:[jak toocreate ContentKey](media-services-rest-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="6e26a-145">If your asset will use encryption, you must create a **ContentKey** and link it tooyour asset as described in hello following topic:[How toocreate a ContentKey](media-services-rest-create-contentkey.md).</span></span> <span data-ttu-id="6e26a-146">Všimněte si, že po odeslání souborů hello do hello asset, je nutné tooupdate hello šifrování vlastnosti hello **AssetFile** entity hello hodnotami, které jste získali při hello **Asset** šifrování.</span><span class="sxs-lookup"><span data-stu-id="6e26a-146">Note that after you upload hello files into hello asset, you need tooupdate hello encryption properties on hello **AssetFile** entity with hello values you got during hello **Asset** encryption.</span></span> <span data-ttu-id="6e26a-147">To provést pomocí hello **SLOUČENÍ** požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e26a-147">Do it by using hello **MERGE** HTTP request.</span></span> 
> 
> 

<span data-ttu-id="6e26a-148">Následující příklad ukazuje, jak Hello toocreate prostředek.</span><span class="sxs-lookup"><span data-stu-id="6e26a-148">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="6e26a-149">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-149">**HTTP Request**</span></span>

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

<span data-ttu-id="6e26a-150">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-150">**HTTP Response**</span></span>

<span data-ttu-id="6e26a-151">V případě úspěchu se vrátí hello následující:</span><span class="sxs-lookup"><span data-stu-id="6e26a-151">If successful, hello following is returned:</span></span>

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

### <a name="create-an-assetfile"></a><span data-ttu-id="6e26a-152">Vytvoření AssetFile</span><span class="sxs-lookup"><span data-stu-id="6e26a-152">Create an AssetFile</span></span>
<span data-ttu-id="6e26a-153">Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity představuje soubor video nebo zvuk, který je uložený v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="6e26a-153">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="6e26a-154">Soubor asset je vždy přidružena k assetu a prostředek může obsahovat mnoho soubory asset.</span><span class="sxs-lookup"><span data-stu-id="6e26a-154">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="6e26a-155">Hello Media Services Encoder úloh selže, pokud objekt souboru asset není spojen s digitálnímu souboru v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="6e26a-155">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="6e26a-156">Všimněte si, že hello **AssetFile** instance a hello samotný mediální soubor jsou dva odlišné objekty.</span><span class="sxs-lookup"><span data-stu-id="6e26a-156">Note that hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="6e26a-157">Hello AssetFile instance obsahuje metadata o hello soubor média, zatímco soubor média hello obsahuje hello samotný mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="6e26a-157">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

<span data-ttu-id="6e26a-158">Po odeslání souboru digitálního média do kontejneru objektů blob, budete používat hello **SLOUČENÍ** HTTP žádost tooupdate hello AssetFile s informacemi o souboru média (jak je uvedeno dále v tématu hello).</span><span class="sxs-lookup"><span data-stu-id="6e26a-158">After you upload your digital media file into a blob container, you will use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (as shown later in hello topic).</span></span> 

<span data-ttu-id="6e26a-159">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-159">**HTTP Request**</span></span>

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

<span data-ttu-id="6e26a-160">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-160">**HTTP Response**</span></span>

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

### <a name="creating-hello-accesspolicy-with-write-permission"></a><span data-ttu-id="6e26a-161">Vytvoření hello AccessPolicy pomocí oprávnění k zápisu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-161">Creating hello AccessPolicy with write permission.</span></span>

>[!NOTE]
><span data-ttu-id="6e26a-162">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="6e26a-162">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="6e26a-163">Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="6e26a-163">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="6e26a-164">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-164">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="6e26a-165">Před nahráním do úložiště objektů blob všechny soubory, nastavení přístupu hello zásady oprávnění pro zápis tooan asset.</span><span class="sxs-lookup"><span data-stu-id="6e26a-165">Before uploading any files into blob storage, set hello access policy rights for writing tooan asset.</span></span> <span data-ttu-id="6e26a-166">nastavit toodo, který POST toohello požadavku HTTP AccessPolicies entity.</span><span class="sxs-lookup"><span data-stu-id="6e26a-166">toodo that, POST an HTTP request toohello AccessPolicies entity set.</span></span> <span data-ttu-id="6e26a-167">Zadejte hodnotu doba trvání v minutách, po vytvoření nebo obdržíte zprávu 500 Chyba interní Server zpět v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6e26a-167">Define a DurationInMinutes value upon creation or you will receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="6e26a-168">Další informace o AccessPolicies najdete v tématu [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="6e26a-168">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="6e26a-169">Následující příklad ukazuje, jak Hello toocreate AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="6e26a-169">hello following example shows how toocreate an AccessPolicy:</span></span>

<span data-ttu-id="6e26a-170">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-170">**HTTP Request**</span></span>

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

<span data-ttu-id="6e26a-171">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-171">**HTTP Request**</span></span>

    If successful, hello following response is returned:

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

### <a name="get-hello-upload-url"></a><span data-ttu-id="6e26a-172">Získat hello adresa URL pro odeslání</span><span class="sxs-lookup"><span data-stu-id="6e26a-172">Get hello Upload URL</span></span>
<span data-ttu-id="6e26a-173">tooreceive hello URL skutečného odeslání, vytvoření lokátoru SAS.</span><span class="sxs-lookup"><span data-stu-id="6e26a-173">tooreceive hello actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="6e26a-174">Lokátory definovat hello počáteční čas a typ koncového bodu připojení pro klienty, kteří mají tooaccess soubory v prostředek.</span><span class="sxs-lookup"><span data-stu-id="6e26a-174">Locators define hello start time and type of connection endpoint for clients that want tooaccess Files in an Asset.</span></span> <span data-ttu-id="6e26a-175">Požadavky a požadavky, můžete vytvořit více entit Lokátor pro danou AccessPolicy a Asset pár toohandle jiným klientem.</span><span class="sxs-lookup"><span data-stu-id="6e26a-175">You can create multiple Locator entities for a given AccessPolicy and Asset pair toohandle different client requests and needs.</span></span> <span data-ttu-id="6e26a-176">Každý z těchto lokátory pomocí hodnoty StartTime hello plus hodnota Doba trvání v minutách hello hello AccessPolicy toodetermine hello délky doba, kterou lze použít adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6e26a-176">Each of these Locators use hello StartTime value plus hello DurationInMinutes value of hello AccessPolicy toodetermine hello length of time a URL can be used.</span></span> <span data-ttu-id="6e26a-177">Další informace najdete v tématu [Lokátor](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="6e26a-177">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="6e26a-178">Adresa URL typu SAS má následující formát hello:</span><span class="sxs-lookup"><span data-stu-id="6e26a-178">A SAS URL has hello following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="6e26a-179">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="6e26a-179">Some considerations apply:</span></span>

* <span data-ttu-id="6e26a-180">Nemůže mít více než pět jedinečný lokátory spojené s danou Asset najednou.</span><span class="sxs-lookup"><span data-stu-id="6e26a-180">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="6e26a-181">Další informace najdete v tématu lokátoru.</span><span class="sxs-lookup"><span data-stu-id="6e26a-181">For more information, see Locator.</span></span>
* <span data-ttu-id="6e26a-182">Pokud potřebujete tooupload vaše soubory okamžitě, byste měli nastavit vaše StartTime hodnota toofive minut před aktuálním časem hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-182">If you need tooupload your files immediately, you should set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="6e26a-183">Je to proto, že je možné, hodiny zkosení mezi klientský počítač a služba Media Services.</span><span class="sxs-lookup"><span data-stu-id="6e26a-183">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="6e26a-184">V hello formátu data a času musí být také hodnota pro čas spuštění: rrrr-MM-ddTHH (například "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="6e26a-184">Also, your StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="6e26a-185">Může být druhý 30-40 zpoždění po vytvoření lokátoru toowhen je k dispozici pro použití.</span><span class="sxs-lookup"><span data-stu-id="6e26a-185">There may be a 30-40 second delay after a Locator is created toowhen it is available for use.</span></span> <span data-ttu-id="6e26a-186">Tento problém se vztahuje tooboth SAS adresa URL a lokátory původu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-186">This issue applies tooboth SAS URL and Origin Locators.</span></span>

<span data-ttu-id="6e26a-187">Hello následující příklad ukazuje, jak toocreate lokátoru SAS adresa URL, podle definice hello vlastnost typu v textu žádosti hello ("1" pro Lokátor SAS) a "2" pro Lokátor původ na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="6e26a-187">hello following example shows how toocreate a SAS URL Locator, as defined by hello Type property in hello request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="6e26a-188">Hello **cesta** vlastnost vrátil obsahuje adresu URL hello, musíte použít tooupload souboru.</span><span class="sxs-lookup"><span data-stu-id="6e26a-188">hello **Path** property returned contains hello URL that you must use tooupload your file.</span></span>

<span data-ttu-id="6e26a-189">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-189">**HTTP Request**</span></span>

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

<span data-ttu-id="6e26a-190">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-190">**HTTP Response**</span></span>

<span data-ttu-id="6e26a-191">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="6e26a-191">If successful, hello following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="6e26a-192">Nahrát soubor do kontejneru úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="6e26a-192">Upload a file into a blob storage container</span></span>
<span data-ttu-id="6e26a-193">Jakmile máte hello AccessPolicy a Lokátor sady, skutečný soubor hello je nahrané tooan kontejneru Azure Blob Storage pomocí hello rozhraní API REST úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6e26a-193">Once you have hello AccessPolicy and Locator set, hello actual file is uploaded tooan Azure Blob Storage container using hello Azure Storage REST APIs.</span></span> <span data-ttu-id="6e26a-194">Soubory hello musíte nahrát jako objekty BLOB bloku.</span><span class="sxs-lookup"><span data-stu-id="6e26a-194">You must upload hello files as block blobs.</span></span> <span data-ttu-id="6e26a-195">Objekty BLOB stránky nejsou podporovány službou Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="6e26a-195">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="6e26a-196">Musíte přidat název souboru hello hello souboru chcete tooupload toohello Lokátor **cesta** přijaté v předchozí části hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6e26a-196">You must add hello file name for hello file you want tooupload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="6e26a-197">Například https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="6e26a-197">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="6e26a-198">.</span><span class="sxs-lookup"><span data-stu-id="6e26a-198">.</span></span> <span data-ttu-id="6e26a-199">.</span><span class="sxs-lookup"><span data-stu-id="6e26a-199">.</span></span> <span data-ttu-id="6e26a-200">.</span><span class="sxs-lookup"><span data-stu-id="6e26a-200">.</span></span> 
> 
> 

<span data-ttu-id="6e26a-201">Další informace o práci s objekty BLOB úložiště Azure najdete v tématu [rozhraní API REST služby objektů Blob](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="6e26a-201">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-hello-assetfile"></a><span data-ttu-id="6e26a-202">Aktualizace hello AssetFile</span><span class="sxs-lookup"><span data-stu-id="6e26a-202">Update hello AssetFile</span></span>
<span data-ttu-id="6e26a-203">Teď, když jste odeslali souboru, aktualizujte informace hello FileAsset velikost (a další).</span><span class="sxs-lookup"><span data-stu-id="6e26a-203">Now that you've uploaded your file, update hello FileAsset size (and other) information.</span></span> <span data-ttu-id="6e26a-204">Například:</span><span class="sxs-lookup"><span data-stu-id="6e26a-204">For example:</span></span>

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


<span data-ttu-id="6e26a-205">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-205">**HTTP Response**</span></span>

<span data-ttu-id="6e26a-206">Pokud úspěšné, následující hello je vrácena: HTTP/1.1 204 ne obsahu</span><span class="sxs-lookup"><span data-stu-id="6e26a-206">If successful, hello following is returned: HTTP/1.1 204 No Content</span></span>

### <a name="delete-hello-locator-and-accesspolicy"></a><span data-ttu-id="6e26a-207">Odstranit hello Lokátor a AccessPolicy</span><span class="sxs-lookup"><span data-stu-id="6e26a-207">Delete hello Locator and AccessPolicy</span></span>
<span data-ttu-id="6e26a-208">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-208">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="6e26a-209">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-209">**HTTP Response**</span></span>

<span data-ttu-id="6e26a-210">V případě úspěchu se vrátí hello následující:</span><span class="sxs-lookup"><span data-stu-id="6e26a-210">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

<span data-ttu-id="6e26a-211">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-211">**HTTP Request**</span></span>

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="6e26a-212">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-212">**HTTP Response**</span></span>

<span data-ttu-id="6e26a-213">V případě úspěchu se vrátí hello následující:</span><span class="sxs-lookup"><span data-stu-id="6e26a-213">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content 
    ...

## <span data-ttu-id="6e26a-214"><a id="upload_in_bulk"></a>Nahrát prostředky hromadně</span><span class="sxs-lookup"><span data-stu-id="6e26a-214"><a id="upload_in_bulk"></a>Upload assets in bulk</span></span>
### <a name="create-hello-ingestmanifest"></a><span data-ttu-id="6e26a-215">Vytvoření hello IngestManifest</span><span class="sxs-lookup"><span data-stu-id="6e26a-215">Create hello IngestManifest</span></span>
<span data-ttu-id="6e26a-216">Hello IngestManifest je kontejner pro sadu prostředky, soubory prostředků a statistiky informace, které lze použít toodetermine hello průběh hromadné příjem pro sadu hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-216">hello IngestManifest is a container for a set of assets, asset files, and statistic information that can be used toodetermine hello progress of bulk ingesting for hello set.</span></span>

<span data-ttu-id="6e26a-217">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-217">**HTTP Request**</span></span>

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

### <a name="create-assets"></a><span data-ttu-id="6e26a-218">Vytvořit prostředky</span><span class="sxs-lookup"><span data-stu-id="6e26a-218">Create assets</span></span>
<span data-ttu-id="6e26a-219">Před vytvořením hello IngestManifestAsset, je nutné toocreate hello Asset, který bude možné provést pomocí hromadné příjem.</span><span class="sxs-lookup"><span data-stu-id="6e26a-219">Before creating hello IngestManifestAsset, you need toocreate hello Asset that will be completed using bulk ingesting.</span></span> <span data-ttu-id="6e26a-220">Prostředek je kontejner pro více typů nebo sady objektů ve službě Media Services, včetně video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků.</span><span class="sxs-lookup"><span data-stu-id="6e26a-220">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="6e26a-221">Vytvoření prostředek v hello REST API, vyžaduje odesílání tooMicrosoft požadavek HTTP POST Azure Media Services a umístění žádné vlastnosti informace o váš asset v textu žádosti hello. V tomto příkladu se vytvoří hello Asset pomocí možnosti StorageEncrption(1) hello zahrnuté do textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-221">In hello REST API, creating an Asset requires sending a HTTP POST request tooMicrosoft Azure Media Services and placing any property information about your asset in hello request body.In this example, hello Asset is created using hello StorageEncrption(1) option included with hello request body.</span></span>

<span data-ttu-id="6e26a-222">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-222">**HTTP Response**</span></span>

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

### <a name="create-hello-ingestmanifestassets"></a><span data-ttu-id="6e26a-223">Vytvoření hello IngestManifestAssets</span><span class="sxs-lookup"><span data-stu-id="6e26a-223">Create hello IngestManifestAssets</span></span>
<span data-ttu-id="6e26a-224">IngestManifestAssets představují prostředky v rámci IngestManifest, které se používají s hromadné příjem.</span><span class="sxs-lookup"><span data-stu-id="6e26a-224">IngestManifestAssets represent Assets within an IngestManifest that are used with bulk ingesting.</span></span> <span data-ttu-id="6e26a-225">Hello v podstatě propojit hello asset toohello manifestu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-225">hello basically link hello asset toohello manifest.</span></span> <span data-ttu-id="6e26a-226">Azure Media Services sleduje interně pro nahrávání souborů hello založené na kolekci spojenou toohello IngestManifestFiles IngestManifestAsset.</span><span class="sxs-lookup"><span data-stu-id="6e26a-226">Azure Media Services watches internally for hello file upload based on IngestManifestFiles collection associated toohello IngestManifestAsset.</span></span> <span data-ttu-id="6e26a-227">Jakmile tyto soubory jsou odeslány, hello asset byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="6e26a-227">Once these files are uploaded, hello asset is completed.</span></span> <span data-ttu-id="6e26a-228">Můžete vytvořit nové IngestManifestAsset s požadavek HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="6e26a-228">You can create a new IngestManifestAsset with a HTTP POST request.</span></span> <span data-ttu-id="6e26a-229">V textu žádosti hello zahrnují hello IngestManifest Id a hello Asset Id této hello IngestManifestAsset měli propojit pro příjem hromadně.</span><span class="sxs-lookup"><span data-stu-id="6e26a-229">In hello request body, include hello IngestManifest Id and hello Asset Id that hello IngestManifestAsset should link together for bulk ingesting.</span></span>

<span data-ttu-id="6e26a-230">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-230">**HTTP Response**</span></span>

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


### <a name="create-hello-ingestmanifestfiles-for-each-asset"></a><span data-ttu-id="6e26a-231">Vytvoření hello IngestManifestFiles pro každý prostředek</span><span class="sxs-lookup"><span data-stu-id="6e26a-231">Create hello IngestManifestFiles for each Asset</span></span>
<span data-ttu-id="6e26a-232">IngestManifestFile představuje skutečný video nebo zvuk blob objekt, který v rámci hromadné příjem pro určitý prostředek, nebude možné odesílat.</span><span class="sxs-lookup"><span data-stu-id="6e26a-232">An IngestManifestFile represents an actual video or audio blob object that will be uploaded as part of bulk ingesting for an asset.</span></span> <span data-ttu-id="6e26a-233">Vlastnosti nejsou vyžadovány, pokud hello asset používá možnost šifrování související s šifrování.</span><span class="sxs-lookup"><span data-stu-id="6e26a-233">Encryption related properties are not required unless hello asset is using an encryption option.</span></span> <span data-ttu-id="6e26a-234">Příklad Hello použitý v této části ukazuje, že vytvoření IngestManifestFile, který používá StorageEncryption pro hello, které vytvořili Asset.</span><span class="sxs-lookup"><span data-stu-id="6e26a-234">hello example used in this section demonstrates creating an IngestManifestFile that uses StorageEncryption for hello Asset previously created.</span></span>

<span data-ttu-id="6e26a-235">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-235">**HTTP Response**</span></span>

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

### <a name="upload-hello-files-tooblob-storage"></a><span data-ttu-id="6e26a-236">Nahrát hello soubory tooBlob úložiště</span><span class="sxs-lookup"><span data-stu-id="6e26a-236">Upload hello Files tooBlob Storage</span></span>
<span data-ttu-id="6e26a-237">Můžete použít libovolná aplikace klienta vysokorychlostní schopná odesílání hello asset soubory toohello kontejner úložiště objektů blob poskytované hello BlobStorageUriForUpload vlastnost hello IngestManifest identifikátor Uri.</span><span class="sxs-lookup"><span data-stu-id="6e26a-237">You can use any high speed client application capable of uploading hello asset files toohello blob storage container Uri provided by hello BlobStorageUriForUpload property of hello IngestManifest.</span></span> <span data-ttu-id="6e26a-238">Je jedna služba nahrávání významné vysokorychlostní [Aspera na vyžádání pro aplikaci Azure](http://go.microsoft.com/fwlink/?LinkId=272001).</span><span class="sxs-lookup"><span data-stu-id="6e26a-238">One notable high speed upload service is [Aspera On Demand for Azure Application](http://go.microsoft.com/fwlink/?LinkId=272001).</span></span>

### <a name="monitor-bulk-ingest-progress"></a><span data-ttu-id="6e26a-239">Monitorování hromadné Ingestování průběh</span><span class="sxs-lookup"><span data-stu-id="6e26a-239">Monitor Bulk Ingest Progress</span></span>
<span data-ttu-id="6e26a-240">Můžete sledovat průběh hello hromadné příjem operací pro IngestManifest pomocí cyklického dotazování hello statistiky vlastnost hello IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="6e26a-240">You can monitor hello progress of bulk ingesting operations for an IngestManifest by polling hello Statistics property of hello IngestManifest.</span></span> <span data-ttu-id="6e26a-241">Zda je vlastnost komplexního typu, [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span><span class="sxs-lookup"><span data-stu-id="6e26a-241">That property is a complex type, [IngestManifestStatistics](https://docs.microsoft.com/rest/api/media/operations/ingestmanifeststatistics).</span></span> <span data-ttu-id="6e26a-242">hello toopoll vlastnost statistiky, odešlete požadavek HTTP GET předávání hello IngestManifest Id.</span><span class="sxs-lookup"><span data-stu-id="6e26a-242">toopoll hello Statistics property, submit a HTTP GET request passing hello IngestManifest Id.</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="6e26a-243">Vytvoření ContentKeys pro šifrování</span><span class="sxs-lookup"><span data-stu-id="6e26a-243">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="6e26a-244">Pokud váš asset používat šifrování, musíte vytvořit toobe ContentKey hello použitý k šifrování před vytvořením hello soubory prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e26a-244">If your asset will use encryption, you must create hello ContentKey toobe used for encryption before creating hello asset files.</span></span> <span data-ttu-id="6e26a-245">Šifrování úložiště hello zahrnovat následující vlastnosti by v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-245">For storage encryption, hello following properties should be included in hello request body.</span></span>

| <span data-ttu-id="6e26a-246">Vlastnost text žádosti</span><span class="sxs-lookup"><span data-stu-id="6e26a-246">Request body property</span></span> | <span data-ttu-id="6e26a-247">Popis</span><span class="sxs-lookup"><span data-stu-id="6e26a-247">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6e26a-248">ID</span><span class="sxs-lookup"><span data-stu-id="6e26a-248">Id</span></span> |<span data-ttu-id="6e26a-249">Hello ContentKey Id, které se vygeneruje označována pomocí hello následující formátu, "nb:kid:UUID:<NEW GUID>".</span><span class="sxs-lookup"><span data-stu-id="6e26a-249">hello ContentKey Id which we generate ourselves using hello following format, “nb:kid:UUID:<NEW GUID>”.</span></span> |
| <span data-ttu-id="6e26a-250">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="6e26a-250">ContentKeyType</span></span> |<span data-ttu-id="6e26a-251">Toto je typ obsahu klíče hello jako celé číslo pro tento klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-251">This is hello content key type as an integer for this content key.</span></span> <span data-ttu-id="6e26a-252">Jsme předejte hello hodnotu 1 pro šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="6e26a-252">We pass hello value 1 for storage encryption.</span></span> |
| <span data-ttu-id="6e26a-253">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="6e26a-253">EncryptedContentKey</span></span> |<span data-ttu-id="6e26a-254">Vytvoříme novou hodnotu obsahu klíče, což je hodnota 256 bitů (32 bajtů).</span><span class="sxs-lookup"><span data-stu-id="6e26a-254">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="6e26a-255">Hello je klíč zašifrovaný pomocí hello úložiště šifrování X.509 certifikátu, který načteme ze služby Microsoft Azure Media Services spuštěním požadavek HTTP GET pro hello GetProtectionKeyId a GetProtectionKey metody.</span><span class="sxs-lookup"><span data-stu-id="6e26a-255">hello key is encrypted using hello storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for hello GetProtectionKeyId and GetProtectionKey Methods.</span></span> |
| <span data-ttu-id="6e26a-256">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="6e26a-256">ProtectionKeyId</span></span> |<span data-ttu-id="6e26a-257">To je hello ochrany id klíče pro hello úložiště šifrovací X.509 certifikát, který byl použité tooencrypt naše klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-257">This is hello protection key id for hello storage encryption X.509 certificate that was used tooencrypt our content key.</span></span> |
| <span data-ttu-id="6e26a-258">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="6e26a-258">ProtectionKeyType</span></span> |<span data-ttu-id="6e26a-259">Toto je typ šifrování hello hello ochrany klíče, který byl klíč obsahu použité tooencrypt hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-259">This is hello encryption type for hello protection key that was used tooencrypt hello content key.</span></span> <span data-ttu-id="6e26a-260">Tato hodnota je StorageEncryption(1) pro náš příklad.</span><span class="sxs-lookup"><span data-stu-id="6e26a-260">This value is StorageEncryption(1) for our example.</span></span> |
| <span data-ttu-id="6e26a-261">Kontrolní součet</span><span class="sxs-lookup"><span data-stu-id="6e26a-261">Checksum</span></span> |<span data-ttu-id="6e26a-262">Hello MD5 počítané kontrolního součtu pro klíč obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-262">hello MD5 calculated checksum for hello content key.</span></span> <span data-ttu-id="6e26a-263">Výpočet je šifrování obsahu hello Id obsahu klíčem hello.</span><span class="sxs-lookup"><span data-stu-id="6e26a-263">It is computed by encrypting hello content Id with hello content key.</span></span> <span data-ttu-id="6e26a-264">Hello příklad kódu ukazuje, jak toocalculate hello kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="6e26a-264">hello example code demonstrates how toocalculate hello checksum.</span></span> |

<span data-ttu-id="6e26a-265">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-265">**HTTP Response**</span></span>

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

### <a name="link-hello-contentkey-toohello-asset"></a><span data-ttu-id="6e26a-266">Odkaz hello ContentKey toohello Asset</span><span class="sxs-lookup"><span data-stu-id="6e26a-266">Link hello ContentKey toohello Asset</span></span>
<span data-ttu-id="6e26a-267">Hello ContentKey je přidružené tooone nebo další prostředky odesláním požadavku HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="6e26a-267">hello ContentKey is associated tooone or more assets by sending a HTTP POST request.</span></span> <span data-ttu-id="6e26a-268">Hello následující požadavek je příklad toolink hello příklad ContentKey toohello příklad prostředek podle Id.</span><span class="sxs-lookup"><span data-stu-id="6e26a-268">hello following request is an example toolink hello example ContentKey toohello example asset by Id.</span></span>

<span data-ttu-id="6e26a-269">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-269">**HTTP Response**</span></span>

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

<span data-ttu-id="6e26a-270">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="6e26a-270">**HTTP Response**</span></span>

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net

## <a name="next-steps"></a><span data-ttu-id="6e26a-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e26a-271">Next steps</span></span>

<span data-ttu-id="6e26a-272">Nyní můžete kódovat nahrané assety.</span><span class="sxs-lookup"><span data-stu-id="6e26a-272">You can now encode your uploaded assets.</span></span> <span data-ttu-id="6e26a-273">Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).</span><span class="sxs-lookup"><span data-stu-id="6e26a-273">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="6e26a-274">Můžete také použít Azure Functions tootrigger úlohu kódování na základě souboru přicházejících do kontejneru hello nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="6e26a-274">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="6e26a-275">Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="6e26a-275">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="6e26a-276">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="6e26a-276">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6e26a-277">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="6e26a-277">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[How tooGet a Media Processor]: media-services-get-media-processor.md

