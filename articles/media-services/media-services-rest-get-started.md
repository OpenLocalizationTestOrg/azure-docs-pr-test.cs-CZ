---
title: "aaaGet začít s doručováním obsahu na vyžádání pomocí REST | Microsoft Docs"
description: "Tento kurz vás provede procesem hello kroky implementace aplikace pro doručování obsahu na vyžádání pomocí Azure Media Services pomocí rozhraní REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 88194b59-e479-43ac-b179-af4f295e3780
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: f270ed59e9ae9745e8403ec6e19d5c3533fc82b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a><span data-ttu-id="9d170-103">Začínáme s doručováním obsahu na vyžádání pomocí REST</span><span class="sxs-lookup"><span data-stu-id="9d170-103">Get started with delivering content on demand using REST</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="9d170-104">Tento rychlý start vás provede procesem hello kroky implementace aplikace pro doručování obsahu na vyžádání pomocí rozhraní API REST Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="9d170-104">This quickstart walks you through hello steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) REST APIs.</span></span>

<span data-ttu-id="9d170-105">Hello kurz představuje hello základní pracovní postup služby Media Services a nejběžnější programovací objekty hello a úkoly vyžadované pro vývoj pro Media Services.</span><span class="sxs-lookup"><span data-stu-id="9d170-105">hello tutorial introduces hello basic Media Services workflow and hello most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="9d170-106">Na hello dokončení kurzu hello bude se mít toostream nebo progresivně stáhnout ukázkový mediální soubor, který nahráli, kódování a stáhnout.</span><span class="sxs-lookup"><span data-stu-id="9d170-106">At hello completion of hello tutorial, you will be able toostream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

<span data-ttu-id="9d170-107">Hello následující obrázek ukazuje některé objekty hello nejčastěji používaná při vývoji aplikace VoD vůči modelu hello Media Services OData.</span><span class="sxs-lookup"><span data-stu-id="9d170-107">hello following image shows some of hello most commonly used objects when developing VoD applications against hello Media Services OData model.</span></span>

<span data-ttu-id="9d170-108">Klikněte na tlačítko tooview hello bitové kopie je plná velikost.</span><span class="sxs-lookup"><span data-stu-id="9d170-108">Click hello image tooview it full size.</span></span>  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a><span data-ttu-id="9d170-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9d170-109">Prerequisites</span></span>
<span data-ttu-id="9d170-110">Hello následující požadované součásti jsou požadované toostart vývoj pomocí služby Media Services pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="9d170-110">hello following prerequisites are required toostart developing with Media Services with REST APIs.</span></span>

* <span data-ttu-id="9d170-111">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="9d170-111">An Azure account.</span></span> <span data-ttu-id="9d170-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9d170-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9d170-113">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="9d170-113">A Media Services account.</span></span> <span data-ttu-id="9d170-114">toocreate účet Media Services najdete v části [jak tooCreate účtu Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="9d170-114">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="9d170-115">Pochopení toho, jak toodevelop s Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="9d170-115">Understanding of how toodevelop with Media Services REST API.</span></span> <span data-ttu-id="9d170-116">Další informace najdete v tématu [Media Services REST API přehled](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="9d170-116">For more information, see [Media Services REST API overview](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="9d170-117">Aplikace podle vašeho výběru, který může odesílat požadavky a odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d170-117">An application of your choice that can send HTTP requests and responses.</span></span> <span data-ttu-id="9d170-118">Tento kurz používá [Fiddler](http://www.telerik.com/download/fiddler).</span><span class="sxs-lookup"><span data-stu-id="9d170-118">This tutorial uses [Fiddler](http://www.telerik.com/download/fiddler).</span></span>

<span data-ttu-id="9d170-119">v tento rychlý Start jsou uvedeny Hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="9d170-119">hello following tasks are shown in this quickstart.</span></span>

1. <span data-ttu-id="9d170-120">Spuštění (pomocí portálu Azure hello) koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="9d170-120">Start streaming endpoint (using hello Azure portal).</span></span>
2. <span data-ttu-id="9d170-121">Účet Media Services toohello připojte pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="9d170-121">Connect toohello Media Services account with REST API.</span></span>
3. <span data-ttu-id="9d170-122">Vytvoření nového prostředku a odeslání videosouboru pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="9d170-122">Create a new asset and upload a video file with REST API.</span></span>
4. <span data-ttu-id="9d170-123">Zakódujte hello zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="9d170-123">Encode hello source file into a set of adaptive bitrate MP4 files with REST API.</span></span>
5. <span data-ttu-id="9d170-124">Publikujte hello asset a get streamování a progresivní stahování adresy URL pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="9d170-124">Publish hello asset and get streaming and progressive download URLs with REST API.</span></span>
6. <span data-ttu-id="9d170-125">Přehrání obsahu</span><span class="sxs-lookup"><span data-stu-id="9d170-125">Play your content.</span></span>

>[!NOTE]
><span data-ttu-id="9d170-126">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="9d170-126">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="9d170-127">Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="9d170-127">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="9d170-128">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="9d170-128">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="9d170-129">Podrobnosti o entitách AMS REST použitým v tomto tématu najdete v tématu [Azure Media Services REST API – referenční informace](/rest/api/media/services/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="9d170-129">For details about AMS REST entities used in this topic, see [Azure Media Services REST API Reference](/rest/api/media/services/azure-media-services-rest-api-reference).</span></span> <span data-ttu-id="9d170-130">Další informace naleznete v [konceptech Azure Media Services](media-services-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="9d170-130">Also, see [Azure Media Services concepts](media-services-concepts.md).</span></span>

>[!NOTE]
><span data-ttu-id="9d170-131">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d170-131">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="9d170-132">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="9d170-132">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a><span data-ttu-id="9d170-133">Spustit pomocí portálu Azure hello koncové body streamování</span><span class="sxs-lookup"><span data-stu-id="9d170-133">Start streaming endpoints using hello Azure portal</span></span>

<span data-ttu-id="9d170-134">Při práci se službou Azure Media Services je jedním hello nejběžnějších scénářů doručování videa přes streamování s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="9d170-134">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="9d170-135">Služba Media Services poskytuje dynamické balení, což vám umožní toodeliver kódováním MP4 obsah ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming) v běhu, aniž byste museli toostore předem zabalené vaší s adaptivní přenosovou rychlostí verze pro každý z těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="9d170-135">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="9d170-136">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="9d170-136">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="9d170-137">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="9d170-137">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="9d170-138">toostart hello koncový bod streamování, hello následující:</span><span class="sxs-lookup"><span data-stu-id="9d170-138">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="9d170-139">Přihlaste se na hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9d170-139">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="9d170-140">V okně Nastavení hello klikněte Streaming koncové body.</span><span class="sxs-lookup"><span data-stu-id="9d170-140">In hello Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="9d170-141">Klikněte na tlačítko hello výchozího koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="9d170-141">Click hello default streaming endpoint.</span></span>

    <span data-ttu-id="9d170-142">Zobrazí se okno Hello výchozí podrobnosti koncový bod STREAMOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="9d170-142">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="9d170-143">Klikněte na ikonu Start hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-143">Click hello Start icon.</span></span>
5. <span data-ttu-id="9d170-144">Klikněte na tlačítko toosave tlačítko hello uložit provedené změny.</span><span class="sxs-lookup"><span data-stu-id="9d170-144">Click hello Save button toosave your changes.</span></span>

## <span data-ttu-id="9d170-145"><a id="connect"></a>Připojit toohello účtu Media Services pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="9d170-145"><a id="connect"></a>Connect toohello Media Services account with REST API</span></span>

<span data-ttu-id="9d170-146">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="9d170-146">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="9d170-147">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="9d170-147">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="9d170-148">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="9d170-148">You must make subsequent calls toohello new URI.</span></span>

<span data-ttu-id="9d170-149">Například pokud po pokusu o tooconnect, jste získali hello následující:</span><span class="sxs-lookup"><span data-stu-id="9d170-149">For example, if after trying tooconnect, you got hello following:</span></span>

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

<span data-ttu-id="9d170-150">Publikujte vaší následné toohttps://wamsbayclus001rest-hs.cloudapp.net/api/ volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9d170-150">You should post your subsequent API calls toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

## <span data-ttu-id="9d170-151"><a id="upload"></a>Vytvoření nového prostředku a odeslání videosouboru pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="9d170-151"><a id="upload"></a>Create a new asset and upload a video file with REST API</span></span>

<span data-ttu-id="9d170-152">Ve službě Media Services můžete digitální soubory nahrát do assetu.</span><span class="sxs-lookup"><span data-stu-id="9d170-152">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="9d170-153">Hello **Asset** entita může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a hello metadata o těchto souborech.)  Jakmile hello soubory jsou odeslány do hello asset, váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="9d170-153">hello **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.)  Once hello files are uploaded into hello asset, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="9d170-154">Jedna z hodnot hello mít tooprovide, při vytváření prostředek je možností vytvoření prostředku.</span><span class="sxs-lookup"><span data-stu-id="9d170-154">One of hello values that you have tooprovide when creating an asset is asset creation options.</span></span> <span data-ttu-id="9d170-155">Hello **možnosti** vlastnost je hodnota výčtu, která popisuje možnosti hello šifrování, které prostředek může být vytvořen pomocí.</span><span class="sxs-lookup"><span data-stu-id="9d170-155">hello **Options** property is an enumeration value that describes hello encryption options that an Asset can be created with.</span></span> <span data-ttu-id="9d170-156">Platná hodnota je jedna z hodnot hello hello seznamu níže není kombinace hodnot z tohoto seznamu:</span><span class="sxs-lookup"><span data-stu-id="9d170-156">A valid value is one of hello values from hello list below, not a combination of values from this list:</span></span>

* <span data-ttu-id="9d170-157">**Žádný** = **0** – nepoužívá se žádné šifrování.</span><span class="sxs-lookup"><span data-stu-id="9d170-157">**None** = **0** - No encryption is used.</span></span> <span data-ttu-id="9d170-158">Při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.</span><span class="sxs-lookup"><span data-stu-id="9d170-158">When using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="9d170-159">Tuto možnost použijte, pokud máte v plánu toodeliver MP4 pomocí progresivního stahování.</span><span class="sxs-lookup"><span data-stu-id="9d170-159">If you plan toodeliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="9d170-160">**StorageEncrypted** = **1** – šifruje vaše nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a odešle ho tooAzure úložiště, kde je uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="9d170-160">**StorageEncrypted** = **1** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="9d170-161">Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="9d170-161">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="9d170-162">Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure souborů vysoce kvalitními vstupními médii pomocí silného šifrování v klidovém stavu na disku.</span><span class="sxs-lookup"><span data-stu-id="9d170-162">hello primary use case for Storage Encryption is when you want toosecure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="9d170-163">**CommonEncryptionProtected** = **2** – tuto možnost použijte, pokud nahráváte obsah, který byl zašifrován a chráněný běžným šifrováním nebo DRM s technologií PlayReady (například technologie Smooth Streaming chránit pomocí DRM s technologií PlayReady).</span><span class="sxs-lookup"><span data-stu-id="9d170-163">**CommonEncryptionProtected** = **2** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="9d170-164">**EnvelopeEncryptionProtected** = **4** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES.</span><span class="sxs-lookup"><span data-stu-id="9d170-164">**EnvelopeEncryptionProtected** = **4** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="9d170-165">Hello soubory musí být kódovaný a zašifrované pomocí Správce transformací.</span><span class="sxs-lookup"><span data-stu-id="9d170-165">hello files must have been encoded and encrypted by Transform Manager.</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="9d170-166">Vytvořit prostředek</span><span class="sxs-lookup"><span data-stu-id="9d170-166">Create an asset</span></span>
<span data-ttu-id="9d170-167">Prostředek je kontejner pro více typů nebo sady objektů ve službě Media Services, včetně video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků.</span><span class="sxs-lookup"><span data-stu-id="9d170-167">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="9d170-168">V rozhraní REST API, vytváření prostředek vyžaduje odesílání POST hello požadovat tooMedia služby a umístění žádné vlastnosti informace o váš asset v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-168">In hello REST API, creating an Asset requires sending POST request tooMedia Services and placing any property information about your asset in hello request body.</span></span>

<span data-ttu-id="9d170-169">Následující příklad ukazuje, jak Hello toocreate prostředek.</span><span class="sxs-lookup"><span data-stu-id="9d170-169">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="9d170-170">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-170">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45

    {"Name":"BigBuckBunny.mp4", "Options":"0"}


<span data-ttu-id="9d170-171">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-171">**HTTP Response**</span></span>

<span data-ttu-id="9d170-172">V případě úspěchu se vrátí hello následující:</span><span class="sxs-lookup"><span data-stu-id="9d170-172">If successful, hello following is returned:</span></span>

    HTTP/1.1 201 Created
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
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a><span data-ttu-id="9d170-173">Vytvoření AssetFile</span><span class="sxs-lookup"><span data-stu-id="9d170-173">Create an AssetFile</span></span>
<span data-ttu-id="9d170-174">Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity představuje soubor video nebo zvuk, který je uložený v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9d170-174">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="9d170-175">Soubor asset je vždy přidružena k assetu a prostředek může obsahovat jednu nebo více AssetFiles.</span><span class="sxs-lookup"><span data-stu-id="9d170-175">An asset file is always associated with an asset, and an asset may contain one or many AssetFiles.</span></span> <span data-ttu-id="9d170-176">Hello Media Services Encoder úloh selže, pokud objekt souboru asset není spojen s digitálnímu souboru v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="9d170-176">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="9d170-177">Po odeslání souboru digitálního média do kontejneru objektů blob, použijete hello **SLOUČENÍ** HTTP žádost tooupdate hello AssetFile s informacemi o souboru média (jak je uvedeno dále v tématu hello).</span><span class="sxs-lookup"><span data-stu-id="9d170-177">After you upload your digital media file into a blob container, you use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (as shown later in hello topic).</span></span>

<span data-ttu-id="9d170-178">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-178">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


<span data-ttu-id="9d170-179">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-179">**HTTP Response**</span></span>

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


### <a name="creating-hello-accesspolicy-with-write-permission"></a><span data-ttu-id="9d170-180">Vytvoření hello AccessPolicy pomocí oprávnění k zápisu</span><span class="sxs-lookup"><span data-stu-id="9d170-180">Creating hello AccessPolicy with write permission</span></span>
<span data-ttu-id="9d170-181">Před nahráním do úložiště objektů blob všechny soubory, nastavení přístupu hello zásady oprávnění pro zápis tooan asset.</span><span class="sxs-lookup"><span data-stu-id="9d170-181">Before uploading any files into blob storage, set hello access policy rights for writing tooan asset.</span></span> <span data-ttu-id="9d170-182">nastavit toodo, který POST toohello požadavku HTTP AccessPolicies entity.</span><span class="sxs-lookup"><span data-stu-id="9d170-182">toodo that, POST an HTTP request toohello AccessPolicies entity set.</span></span> <span data-ttu-id="9d170-183">Zadejte hodnotu doba trvání v minutách, po vytvoření nebo zobrazí 500 chybová zpráva Interní Server zpět v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9d170-183">Define a DurationInMinutes value upon creation or you receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="9d170-184">Další informace o AccessPolicies najdete v tématu [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="9d170-184">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="9d170-185">Následující příklad ukazuje, jak Hello toocreate AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="9d170-185">hello following example shows how toocreate an AccessPolicy:</span></span>

<span data-ttu-id="9d170-186">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-186">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"}

<span data-ttu-id="9d170-187">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-187">**HTTP Response**</span></span>

<span data-ttu-id="9d170-188">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="9d170-188">If successful, hello following response is returned:</span></span>

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
    X-Powered-By: ASP.NET
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

### <a name="get-hello-upload-url"></a><span data-ttu-id="9d170-189">Získat hello adresa URL pro odeslání</span><span class="sxs-lookup"><span data-stu-id="9d170-189">Get hello Upload URL</span></span>

<span data-ttu-id="9d170-190">tooreceive hello URL skutečného odeslání, vytvoření lokátoru SAS.</span><span class="sxs-lookup"><span data-stu-id="9d170-190">tooreceive hello actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="9d170-191">Lokátory definovat hello počáteční čas a typ koncového bodu připojení pro klienty, kteří mají tooaccess soubory v prostředek.</span><span class="sxs-lookup"><span data-stu-id="9d170-191">Locators define hello start time and type of connection endpoint for clients that want tooaccess Files in an Asset.</span></span> <span data-ttu-id="9d170-192">Požadavky a požadavky, můžete vytvořit více entit Lokátor pro danou AccessPolicy a Asset pár toohandle jiným klientem.</span><span class="sxs-lookup"><span data-stu-id="9d170-192">You can create multiple Locator entities for a given AccessPolicy and Asset pair toohandle different client requests and needs.</span></span> <span data-ttu-id="9d170-193">Každá z těchto lokátory používá hodnoty StartTime hello plus hodnota Doba trvání v minutách hello hello AccessPolicy toodetermine hello délky doba, kterou lze použít adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9d170-193">Each of these Locators uses hello StartTime value plus hello DurationInMinutes value of hello AccessPolicy toodetermine hello length of time a URL can be used.</span></span> <span data-ttu-id="9d170-194">Další informace najdete v tématu [Lokátor](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="9d170-194">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="9d170-195">Adresa URL typu SAS má následující formát hello:</span><span class="sxs-lookup"><span data-stu-id="9d170-195">A SAS URL has hello following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="9d170-196">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="9d170-196">Some considerations apply:</span></span>

* <span data-ttu-id="9d170-197">Nemůže mít více než pět jedinečný lokátory spojené s danou Asset najednou.</span><span class="sxs-lookup"><span data-stu-id="9d170-197">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="9d170-198">Další informace najdete v tématu lokátoru.</span><span class="sxs-lookup"><span data-stu-id="9d170-198">For more information, see Locator.</span></span>
* <span data-ttu-id="9d170-199">Pokud potřebujete tooupload vaše soubory okamžitě, byste měli nastavit vaše StartTime hodnota toofive minut před aktuálním časem hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-199">If you need tooupload your files immediately, you should set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="9d170-200">Je to proto, že je možné, hodiny zkosení mezi klientský počítač a služba Media Services.</span><span class="sxs-lookup"><span data-stu-id="9d170-200">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="9d170-201">V hello formátu data a času musí být také hodnota pro čas spuštění: rrrr-MM-ddTHH (například "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="9d170-201">Also, your StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="9d170-202">Může být druhý 30-40 zpoždění po vytvoření lokátoru toowhen je k dispozici pro použití.</span><span class="sxs-lookup"><span data-stu-id="9d170-202">There may be a 30-40 second delay after a Locator is created toowhen it is available for use.</span></span> <span data-ttu-id="9d170-203">Tento problém se vztahuje tooboth SAS adresa URL a lokátory původu.</span><span class="sxs-lookup"><span data-stu-id="9d170-203">This issue applies tooboth SAS URL and Origin Locators.</span></span>

<span data-ttu-id="9d170-204">Další informace o tokenu SAS najdete v části lokátory [to](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogu.</span><span class="sxs-lookup"><span data-stu-id="9d170-204">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="9d170-205">Hello následující příklad ukazuje, jak toocreate lokátoru SAS adresa URL, podle definice hello vlastnost typu v textu žádosti hello ("1" pro Lokátor SAS) a "2" pro Lokátor původ na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="9d170-205">hello following example shows how toocreate a SAS URL Locator, as defined by hello Type property in hello request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="9d170-206">Hello **cesta** vlastnost vrátil obsahuje adresu URL hello, musíte použít tooupload souboru.</span><span class="sxs-lookup"><span data-stu-id="9d170-206">hello **Path** property returned contains hello URL that you must use tooupload your file.</span></span>

<span data-ttu-id="9d170-207">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-207">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178

    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


<span data-ttu-id="9d170-208">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-208">**HTTP Response**</span></span>

<span data-ttu-id="9d170-209">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="9d170-209">If successful, hello following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="9d170-210">Nahrát soubor do kontejneru úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="9d170-210">Upload a file into a blob storage container</span></span>
<span data-ttu-id="9d170-211">Jakmile máte hello AccessPolicy a Lokátor sady, skutečný soubor hello je kontejner úložiště objektů blob v Azure nahrané tooan pomocí hello rozhraní API REST úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9d170-211">Once you have hello AccessPolicy and Locator set, hello actual file is uploaded tooan Azure blob storage container using hello Azure Storage REST APIs.</span></span> <span data-ttu-id="9d170-212">Soubory hello musíte nahrát jako objekty BLOB bloku.</span><span class="sxs-lookup"><span data-stu-id="9d170-212">You must upload hello files as block blobs.</span></span> <span data-ttu-id="9d170-213">Objekty BLOB stránky nejsou podporovány službou Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="9d170-213">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="9d170-214">Musíte přidat název souboru hello hello souboru chcete tooupload toohello Lokátor **cesta** přijaté v předchozí části hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9d170-214">You must add hello file name for hello file you want tooupload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="9d170-215">Například https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="9d170-215">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="9d170-216">.</span><span class="sxs-lookup"><span data-stu-id="9d170-216">.</span></span> <span data-ttu-id="9d170-217">.</span><span class="sxs-lookup"><span data-stu-id="9d170-217">.</span></span> <span data-ttu-id="9d170-218">.</span><span class="sxs-lookup"><span data-stu-id="9d170-218">.</span></span>
>
>

<span data-ttu-id="9d170-219">Další informace o práci s objekty BLOB úložiště Azure najdete v tématu [rozhraní API REST služby objektů Blob](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="9d170-219">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-hello-assetfile"></a><span data-ttu-id="9d170-220">Aktualizace hello AssetFile</span><span class="sxs-lookup"><span data-stu-id="9d170-220">Update hello AssetFile</span></span>
<span data-ttu-id="9d170-221">Teď, když jste odeslali souboru, aktualizujte informace hello FileAsset velikost (a další).</span><span class="sxs-lookup"><span data-stu-id="9d170-221">Now that you've uploaded your file, update hello FileAsset size (and other) information.</span></span> <span data-ttu-id="9d170-222">Například:</span><span class="sxs-lookup"><span data-stu-id="9d170-222">For example:</span></span>

    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


<span data-ttu-id="9d170-223">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-223">**HTTP Response**</span></span>

<span data-ttu-id="9d170-224">V případě úspěchu se vrátí hello následující:</span><span class="sxs-lookup"><span data-stu-id="9d170-224">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <a name="delete-hello-locator-and-accesspolicy"></a><span data-ttu-id="9d170-225">Odstranit hello Lokátor a AccessPolicy</span><span class="sxs-lookup"><span data-stu-id="9d170-225">Delete hello Locator and AccessPolicy</span></span>
<span data-ttu-id="9d170-226">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-226">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="9d170-227">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-227">**HTTP Response**</span></span>

<span data-ttu-id="9d170-228">V případě úspěchu se vrátí hello následující:</span><span class="sxs-lookup"><span data-stu-id="9d170-228">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

<span data-ttu-id="9d170-229">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-229">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

<span data-ttu-id="9d170-230">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-230">**HTTP Response**</span></span>

<span data-ttu-id="9d170-231">V případě úspěchu se vrátí hello následující:</span><span class="sxs-lookup"><span data-stu-id="9d170-231">If successful, hello following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <span data-ttu-id="9d170-232"><a id="encode"></a>Kódování hello zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="9d170-232"><a id="encode"></a>Encode hello source file into a set of adaptive bitrate MP4 files</span></span>

<span data-ttu-id="9d170-233">Po zpracování, které může být zakódován prostředky ve službě Media Services, média, transmuxovat, označit vodoznakem a tak dále před doručení tooclients.</span><span class="sxs-lookup"><span data-stu-id="9d170-233">After ingesting Assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered tooclients.</span></span> <span data-ttu-id="9d170-234">Tyto aktivity jsou naplánované a spouštění více pozadí role instance tooensure vysoký výkon a dostupnost.</span><span class="sxs-lookup"><span data-stu-id="9d170-234">These activities are scheduled and run against multiple background role instances tooensure high performance and availability.</span></span> <span data-ttu-id="9d170-235">Tyto aktivity se nazývají úlohy a každá úloha se skládá z atomických úloh, které hello samotnou práci na souboru Assetu hello (Další informace najdete v tématu [úlohy](/rest/api/media/services/job), [úloh](/rest/api/media/services/task) popisy).</span><span class="sxs-lookup"><span data-stu-id="9d170-235">These activities are called Jobs and each Job is composed of atomic Tasks that do hello actual work on hello Asset file (for more information, see [Job](/rest/api/media/services/job), [Task](/rest/api/media/services/task) descriptions).</span></span>

<span data-ttu-id="9d170-236">Jak již bylo zmíněno dříve, při práci s Azure Media Services jeden hello nejběžnějších scénářů doručování adaptivní přenosové rychlosti streamování tooyour klientů.</span><span class="sxs-lookup"><span data-stu-id="9d170-236">As was mentioned earlier, when working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="9d170-237">Služba Media Services umí dynamicky balit sady souborů MP4 s adaptivní přenosovou rychlostí do jednoho z následujících formátů hello: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="9d170-237">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of hello following formats: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span>

<span data-ttu-id="9d170-238">Následující části Hello ukazuje, jak toocreate úlohu, která obsahuje jeden kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="9d170-238">hello following section shows how toocreate a job that contains one encoding task.</span></span> <span data-ttu-id="9d170-239">Hello úloh určuje tootranscode hello souboru mezzanine do sady s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi pomocí **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="9d170-239">hello task specifies tootranscode hello mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="9d170-240">část Hello také ukazuje, jak toomonitor hello průběh zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="9d170-240">hello section also shows how toomonitor hello job processing progress.</span></span> <span data-ttu-id="9d170-241">Po dokončení úlohy hello, bude možné toocreate lokátory, které jsou potřebné tooget přístup tooyour prostředky.</span><span class="sxs-lookup"><span data-stu-id="9d170-241">When hello job is complete, you would be able toocreate locators that are needed tooget access tooyour assets.</span></span>

### <a name="get-a-media-processor"></a><span data-ttu-id="9d170-242">Získat procesor médií</span><span class="sxs-lookup"><span data-stu-id="9d170-242">Get a media processor</span></span>
<span data-ttu-id="9d170-243">Ve službě Media Services je procesor médií komponenty, která zpracovává zpracování specifické pro úlohy, jako je například kódování, převod formátu, šifrování nebo dešifrování objektu mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="9d170-243">In Media Services, a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="9d170-244">Pro hello kódování úloh uvedené v tomto kurzu přidáme toouse hello Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="9d170-244">For hello encoding task shown in this tutorial, we are going toouse hello Media Encoder Standard.</span></span>

<span data-ttu-id="9d170-245">Hello následující kód id kodér hello požadavky.</span><span class="sxs-lookup"><span data-stu-id="9d170-245">hello following code requests hello encoder's id.</span></span>

<span data-ttu-id="9d170-246">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-246">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="9d170-247">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-247">**HTTP Response**</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a><span data-ttu-id="9d170-248">Vytvoření úlohy</span><span class="sxs-lookup"><span data-stu-id="9d170-248">Create a job</span></span>
<span data-ttu-id="9d170-249">Každá úloha může mít jeden nebo více úloh v závislosti na typu hello zpracování, které chcete tooaccomplish.</span><span class="sxs-lookup"><span data-stu-id="9d170-249">Each Job can have one or more Tasks depending on hello type of processing that you want tooaccomplish.</span></span> <span data-ttu-id="9d170-250">Prostřednictvím hello REST API, můžete vytvořit úlohy a jejich související úlohy v jedné ze dvou způsobů: úloh může být definována vložením prostřednictvím hello úlohy navigační vlastnost u entity úlohy nebo zpracování dávky OData.</span><span class="sxs-lookup"><span data-stu-id="9d170-250">Through hello REST API, you can create Jobs and their related Tasks in one of two ways: Tasks can be defined inline through hello Tasks navigation property on Job entities, or through OData batch processing.</span></span> <span data-ttu-id="9d170-251">Hello sady Media Services SDK používá dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="9d170-251">hello Media Services SDK uses batch processing.</span></span> <span data-ttu-id="9d170-252">Hello čitelnější hello příklady kódu v tomto tématu jsou úlohy však definována vložením.</span><span class="sxs-lookup"><span data-stu-id="9d170-252">However, for hello readability of hello code examples in this topic, tasks are defined inline.</span></span> <span data-ttu-id="9d170-253">Informace o zpracování dávky, najdete v části [Open Data Protocol (OData) dávkové zpracování](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="9d170-253">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

<span data-ttu-id="9d170-254">Hello následující ukázka ukazuje, jak toocreate a post úlohu s jeden úkol nastavit tooencode video na konkrétní řešení a kvality.</span><span class="sxs-lookup"><span data-stu-id="9d170-254">hello following example shows you how toocreate and post a Job with one Task set tooencode a video at a specific resolution and quality.</span></span> <span data-ttu-id="9d170-255">Hello následující dokumentace obsahuje hello seznam všech hello [úkolů přednastavení](http://msdn.microsoft.com/library/mt269960) nepodporuje procesoru Media Encoder Standard hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-255">hello following documentation section contains hello list of all hello [task presets](http://msdn.microsoft.com/library/mt269960) supported by hello Media Encoder Standard processor.</span></span>  

<span data-ttu-id="9d170-256">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-256">**HTTP Request**</span></span>

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"Adaptive Streaming",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

<span data-ttu-id="9d170-257">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-257">**HTTP Response**</span></span>

<span data-ttu-id="9d170-258">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="9d170-258">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  

             ]
          }
       }
    }


<span data-ttu-id="9d170-259">V žádné žádosti o úlohu jsou toonote několik důležitých věcí:</span><span class="sxs-lookup"><span data-stu-id="9d170-259">There are a few important things toonote in any Job request:</span></span>

* <span data-ttu-id="9d170-260">Taskbody – vlastnosti musí používat literál XML toodefine hello počet vstup nebo výstup prostředky, které jsou používány hello úloh.</span><span class="sxs-lookup"><span data-stu-id="9d170-260">TaskBody properties MUST use literal XML toodefine hello number of input, or output assets that are used by hello Task.</span></span> <span data-ttu-id="9d170-261">Hello úloh téma obsahuje hello definice schématu XML pro hello XML.</span><span class="sxs-lookup"><span data-stu-id="9d170-261">hello Task topic contains hello XML Schema Definition for hello XML.</span></span>
* <span data-ttu-id="9d170-262">V hello taskbody – definice, každý vnitřní hodnota <inputAsset> a <outputAsset> musí být nastavena jako JobInputAsset(value) nebo JobOutputAsset(value).</span><span class="sxs-lookup"><span data-stu-id="9d170-262">In hello TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="9d170-263">Úloha může mít více prostředků výstup.</span><span class="sxs-lookup"><span data-stu-id="9d170-263">A task can have multiple output assets.</span></span> <span data-ttu-id="9d170-264">Jeden JobOutputAsset(x) lze použít jako výstup úlohy pro úlohu pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="9d170-264">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="9d170-265">Můžete zadat JobInputAsset nebo JobOutputAsset jako vstupní datový zdroj úlohy.</span><span class="sxs-lookup"><span data-stu-id="9d170-265">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="9d170-266">Úlohy nesmí tvoří cyklus.</span><span class="sxs-lookup"><span data-stu-id="9d170-266">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="9d170-267">parametr hodnoty Hello předáte tooJobInputAsset nebo JobOutputAsset představuje hodnotu indexu hello pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="9d170-267">hello value parameter that you pass tooJobInputAsset or JobOutputAsset represents hello index value for an Asset.</span></span> <span data-ttu-id="9d170-268">Hello skutečné prostředky jsou definovány v hello InputMediaAssets a OutputMediaAssets navigační vlastnosti na hello entity definice úlohy.</span><span class="sxs-lookup"><span data-stu-id="9d170-268">hello actual Assets are defined in hello InputMediaAssets and OutputMediaAssets navigation properties on hello Job entity definition.</span></span>

> [!NOTE]
> <span data-ttu-id="9d170-269">Služba Media Services je integrovaná v OData v3, hello jednotlivé prostředky v InputMediaAssets a OutputMediaAssets navigační vlastnost kolekce se odkazuje prostřednictvím "__metadata: identifikátor uri" dvojice název hodnota.</span><span class="sxs-lookup"><span data-stu-id="9d170-269">Because Media Services is built on OData v3, hello individual assets in InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
>
>

* <span data-ttu-id="9d170-270">InputMediaAssets mapuje tooone nebo další prostředky, které jste vytvořili ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="9d170-270">InputMediaAssets maps tooone or more Assets you have created in Media Services.</span></span> <span data-ttu-id="9d170-271">OutputMediaAssets jsou vytvořené pomocí systému hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-271">OutputMediaAssets are created by hello system.</span></span> <span data-ttu-id="9d170-272">Neodkazujte existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="9d170-272">They do not reference an existing asset.</span></span>
* <span data-ttu-id="9d170-273">OutputMediaAssets může mít název pomocí atributu assetName hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-273">OutputMediaAssets can be named using hello assetName attribute.</span></span> <span data-ttu-id="9d170-274">Pokud tento atribut není k dispozici, je název hello hello OutputMediaAsset libovolnou hodnotu vnitřní text hello hello <outputAsset> element je s příponou hello název úlohy hodnotu nebo hodnotu Id úlohy hello (v případě hello, kde není definován název vlastnosti hello).</span><span class="sxs-lookup"><span data-stu-id="9d170-274">If this attribute is not present, then hello name of hello OutputMediaAsset is whatever hello inner text value of hello <outputAsset> element is with a suffix of either hello Job Name value, or hello Job Id value (in hello case where hello Name property isn't defined).</span></span> <span data-ttu-id="9d170-275">Například pokud nastavíte hodnotu assetName příliš "Ukázkový", pak hello se nastavuje vlastnost název OutputMediaAsset příliš "ukázkové".</span><span class="sxs-lookup"><span data-stu-id="9d170-275">For example, if you set a value for assetName too"Sample", then hello OutputMediaAsset Name property would be set too"Sample".</span></span> <span data-ttu-id="9d170-276">Pokud jste nenastavili hodnotu assetName, ale nastavit název úlohy hello příliš "NewJob", pak hello název OutputMediaAsset by však bylo "_NewJob JobOutputAsset (hodnota)".</span><span class="sxs-lookup"><span data-stu-id="9d170-276">However, if you did not set a value for assetName, but did set hello job name too"NewJob", then hello OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob".</span></span>

    <span data-ttu-id="9d170-277">Hello následující příklad ukazuje, jak tooset hello assetName atribut:</span><span class="sxs-lookup"><span data-stu-id="9d170-277">hello following example shows how tooset hello assetName attribute:</span></span>

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* <span data-ttu-id="9d170-278">řetězení úloh tooenable:</span><span class="sxs-lookup"><span data-stu-id="9d170-278">tooenable task chaining:</span></span>

  * <span data-ttu-id="9d170-279">Úloha musí mít alespoň dvě úlohy</span><span class="sxs-lookup"><span data-stu-id="9d170-279">A job must have at least two tasks</span></span>
  * <span data-ttu-id="9d170-280">Musí existovat alespoň jeden úkol, jejichž vstup je výstup jiná úloha v úloze hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-280">There must be at least one task whose input is output of another task in hello job.</span></span>

<span data-ttu-id="9d170-281">Další informace najdete v tématu [vytváření úlohu kódování s hello Media Services REST API](media-services-rest-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="9d170-281">For more information see, [Creating an Encoding Job with hello Media Services REST API](media-services-rest-encode-asset.md).</span></span>

### <a name="monitor-processing-progress"></a><span data-ttu-id="9d170-282">Monitorování zpracování průběh</span><span class="sxs-lookup"><span data-stu-id="9d170-282">Monitor Processing Progress</span></span>
<span data-ttu-id="9d170-283">Stav úlohy hello můžete načíst pomocí vlastnosti hello stavu, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-283">You can retrieve hello Job status by using hello State property, as shown in hello following example.</span></span>

<span data-ttu-id="9d170-284">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-284">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


<span data-ttu-id="9d170-285">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-285">**HTTP Response**</span></span>

<span data-ttu-id="9d170-286">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="9d170-286">If successful, hello following response is returned:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT

    {"d":{"State":2}}


### <a name="cancel-a-job"></a><span data-ttu-id="9d170-287">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="9d170-287">Cancel a job</span></span>
<span data-ttu-id="9d170-288">Služba Media Services umožňuje toocancel spuštěné úlohy prostřednictvím hello CancelJob funkce.</span><span class="sxs-lookup"><span data-stu-id="9d170-288">Media Services allows you toocancel running jobs through hello CancelJob function.</span></span> <span data-ttu-id="9d170-289">Toto volání vrátí kód, Chyba 400, pokud se pokusíte toocancel úlohu, když se zruší její stav, zrušení, chyby nebo skončila.</span><span class="sxs-lookup"><span data-stu-id="9d170-289">This call returns a 400 error code if you try toocancel a Job when its state is canceled, canceling, error, or finished.</span></span>

<span data-ttu-id="9d170-290">Následující příklad ukazuje, jak Hello toocall CancelJob.</span><span class="sxs-lookup"><span data-stu-id="9d170-290">hello following example shows how toocall CancelJob.</span></span>

<span data-ttu-id="9d170-291">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-291">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


<span data-ttu-id="9d170-292">V případě úspěchu se vrátí kód odpovědi 204 s žádný text zprávy.</span><span class="sxs-lookup"><span data-stu-id="9d170-292">If successful, a 204 response code is returned with no message body.</span></span>

> [!NOTE]
> <span data-ttu-id="9d170-293">Je nutné kódování URL hello úlohu s id (obvykle nb:jid:UUID: somevalue) při předávání v jako parametr tooCancelJob.</span><span class="sxs-lookup"><span data-stu-id="9d170-293">You must URL-encode hello job id (normally nb:jid:UUID: somevalue) when passing it in as a parameter tooCancelJob.</span></span>
>
>

### <a name="get-hello-output-asset"></a><span data-ttu-id="9d170-294">Získat prostředek výstup hello</span><span class="sxs-lookup"><span data-stu-id="9d170-294">Get hello output asset</span></span>
<span data-ttu-id="9d170-295">Hello následující kód ukazuje, jak toorequest hello výstupní asset ID.</span><span class="sxs-lookup"><span data-stu-id="9d170-295">hello following code shows how toorequest hello output asset Id.</span></span>

<span data-ttu-id="9d170-296">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-296">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="9d170-297">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="9d170-297">**HTTP Response**</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <span data-ttu-id="9d170-298"><a id="publish_get_urls"></a>Publikujte hello asset a get streamování a progresivní stahování adresy URL pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="9d170-298"><a id="publish_get_urls"></a>Publish hello asset and get streaming and progressive download URLs with REST API</span></span>

<span data-ttu-id="9d170-299">toostream nebo stažení prostředek, je nejprve nutné příliš "publikovat" vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="9d170-299">toostream or download an asset, you first need too"publish" it by creating a locator.</span></span> <span data-ttu-id="9d170-300">Lokátory zajišťují přístup toofiles obsažené v hello asset.</span><span class="sxs-lookup"><span data-stu-id="9d170-300">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="9d170-301">Služba Media Services podporuje dva typy lokátorů: OnDemandOrigin lokátory použité toostream médií (například MPEG DASH, HLS nebo technologie Smooth Streaming) a přístupového podpisu (SAS), používá toodownload mediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="9d170-301">Media Services supports two types of locators: OnDemandOrigin locators, used toostream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used toodownload media files.</span></span> <span data-ttu-id="9d170-302">Další informace o tokenu SAS najdete v části lokátory [to](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogu.</span><span class="sxs-lookup"><span data-stu-id="9d170-302">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="9d170-303">Po vytvoření lokátorů hello, můžete vytvořit hello adresy URL, které jsou používané toostream a stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="9d170-303">Once you create hello locators, you can build hello URLs that are used toostream or download your files.</span></span>

>[!NOTE]
><span data-ttu-id="9d170-304">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="9d170-304">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="9d170-305">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="9d170-305">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

<span data-ttu-id="9d170-306">Streamovací adresa URL pro technologii Smooth Streaming má následující formát hello:</span><span class="sxs-lookup"><span data-stu-id="9d170-306">A streaming URL for Smooth Streaming has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="9d170-307">Streamovací adresa URL pro HLS má následující formát hello:</span><span class="sxs-lookup"><span data-stu-id="9d170-307">A streaming URL for HLS has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="9d170-308">Streamovací adresa URL pro MPEG DASH má následující formát hello:</span><span class="sxs-lookup"><span data-stu-id="9d170-308">A streaming URL for MPEG DASH has hello following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="9d170-309">SAS adresa URL používaná toodownload souborů má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="9d170-309">A SAS URL used toodownload files has hello following format:</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="9d170-310">Tato část uvádí, jak tooperform hello následující úkoly potřebné příliš "publikovat" vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="9d170-310">This section shows how tooperform hello following tasks necessary too"publish" your assets.</span></span>  

* <span data-ttu-id="9d170-311">Vytváření hello AccessPolicy s oprávněními ke čtení</span><span class="sxs-lookup"><span data-stu-id="9d170-311">Creating hello AccessPolicy with read permission</span></span>
* <span data-ttu-id="9d170-312">Vytváření SAS adresa URL pro stahování obsahu</span><span class="sxs-lookup"><span data-stu-id="9d170-312">Creating a SAS URL for downloading content</span></span>
* <span data-ttu-id="9d170-313">Vytváření původní adresu URL pro streamování obsahu</span><span class="sxs-lookup"><span data-stu-id="9d170-313">Creating an origin URL for streaming content</span></span>

### <a name="creating-hello-accesspolicy-with-read-permission"></a><span data-ttu-id="9d170-314">Vytváření hello AccessPolicy s oprávněními ke čtení</span><span class="sxs-lookup"><span data-stu-id="9d170-314">Creating hello AccessPolicy with read permission</span></span>
<span data-ttu-id="9d170-315">Před stažením nebo streamování žádný obsah média, nejprve definovat AccessPolicy s oprávněními ke čtení a vytvořit hello odpovídající Lokátor entita, která určuje typ hello doručení mechanismu chcete tooenable pro klienty.</span><span class="sxs-lookup"><span data-stu-id="9d170-315">Before downloading or streaming any media content, first define an AccessPolicy with read permissions and create hello appropriate Locator entity that specifies hello type of delivery mechanism you want tooenable for your clients.</span></span> <span data-ttu-id="9d170-316">Další informace o vlastnostech hello k dispozici, najdete v části [vlastností Entity AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span><span class="sxs-lookup"><span data-stu-id="9d170-316">For more information on hello properties available, see [AccessPolicy Entity Properties](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span></span>

<span data-ttu-id="9d170-317">Hello následující příklad ukazuje, jak toospecify hello AccessPolicy pro oprávnění ke čtení pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="9d170-317">hello following example shows how toospecify hello AccessPolicy for read permissions for a given Asset.</span></span>

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

<span data-ttu-id="9d170-318">V případě úspěchu se vrátí kód 201 úspěšnosti popisující hello AccessPolicy entity, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9d170-318">If successful, a 201 success code is returned describing hello AccessPolicy entity that you created.</span></span> <span data-ttu-id="9d170-319">Pak použijete hello AccessPolicy Id společně s hello Asset Id hello asset, který obsahuje soubor hello chcete entity Lokátor hello toocreate toodeliver (například výstupní asset).</span><span class="sxs-lookup"><span data-stu-id="9d170-319">You then use hello AccessPolicy Id along with hello Asset Id of hello asset that contains hello file you want toodeliver(such as an output asset) toocreate hello Locator entity.</span></span>

> [!NOTE]
> <span data-ttu-id="9d170-320">Tento základní pracovní postup je hello stejné jako nahrání souboru, když příjem prostředek (jak je popsané v tomto tématu výše).</span><span class="sxs-lookup"><span data-stu-id="9d170-320">This basic workflow is hello same as uploading a file when ingesting an Asset (as was discussed earlier in this topic).</span></span> <span data-ttu-id="9d170-321">Také jako nahrávání souborů, pokud jste (nebo klientů) potřebovat tooaccess vaše soubory okamžitě, nastavte vaše StartTime hodnota toofive minut před aktuálním časem hello.</span><span class="sxs-lookup"><span data-stu-id="9d170-321">Also, like uploading files, if you (or your clients) need tooaccess your files immediately, set your StartTime value toofive minutes before hello current time.</span></span> <span data-ttu-id="9d170-322">Tato akce je nutné, protože je možné, hodiny zkosení mezi hello klient a služba Media Services.</span><span class="sxs-lookup"><span data-stu-id="9d170-322">This action is necessary because there may be clock skew between hello client and Media Services.</span></span> <span data-ttu-id="9d170-323">Hello hodnoty StartTime musí být ve formátu data a času hello: rrrr-MM-ddTHH (například "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="9d170-323">hello StartTime value must be in hello following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a><span data-ttu-id="9d170-324">Vytváření SAS adresa URL pro stahování obsahu</span><span class="sxs-lookup"><span data-stu-id="9d170-324">Creating a SAS URL for downloading content</span></span>
<span data-ttu-id="9d170-325">Hello následující kód ukazuje, jak tooget adresu URL, která lze použít toodownload soubor média vytvořit a nahrát dříve.</span><span class="sxs-lookup"><span data-stu-id="9d170-325">hello following code shows how tooget a URL that can be used toodownload a media file created and uploaded previously.</span></span> <span data-ttu-id="9d170-326">Hello AccessPolicy má číst sadu oprávnění, a Lokátor cesty hello odkazuje adresa URL pro stahování tooa SAS.</span><span class="sxs-lookup"><span data-stu-id="9d170-326">hello AccessPolicy has read permissions set and hello Locator path refers tooa SAS download URL.</span></span>

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

<span data-ttu-id="9d170-327">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="9d170-327">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


<span data-ttu-id="9d170-328">Hello vrátil **cesta** vlastnost obsahuje hello SAS adresa URL.</span><span class="sxs-lookup"><span data-stu-id="9d170-328">hello returned **Path** property contains hello SAS URL.</span></span>

> [!NOTE]
> <span data-ttu-id="9d170-329">Pokud si stáhnout obsah šifrování úložiště, musíte ručně dešifrovat před vykreslením, nebo použijte hello MediaProcessor dešifrování úložiště v toooutput zpracování úloh zpracování souborů v hello zrušte tooan OutputAsset a následné stažení z tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="9d170-329">If you download storage encrypted content, you must manually decrypt it before rendering it, or use hello Storage Decryption MediaProcessor in a processing task toooutput processed files in hello clear tooan OutputAsset and then download from that Asset.</span></span> <span data-ttu-id="9d170-330">Další informace o zpracování najdete v části vytváření úlohu kódování s hello Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="9d170-330">For more information on processing, see Creating an Encoding Job with hello Media Services REST API.</span></span> <span data-ttu-id="9d170-331">SAS adresa URL lokátory navíc nelze aktualizovat, po jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="9d170-331">Also, SAS URL Locators cannot be updated after they have been created.</span></span> <span data-ttu-id="9d170-332">Například nemůžete použít opakovaně hello stejné Lokátor s hodnotou aktualizované čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="9d170-332">For example, you cannot reuse hello same Locator with an updated StartTime value.</span></span> <span data-ttu-id="9d170-333">To je kvůli hello způsob, jakým se vytvářejí SAS adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9d170-333">This is because of hello way SAS URLs are created.</span></span> <span data-ttu-id="9d170-334">Pokud chcete tooaccess prostředek ke stažení po vypršení platnosti lokátoru, musíte vytvořit novou na nový čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="9d170-334">If you want tooaccess an asset for download after a Locator has expired, then you must create a new one with a new StartTime.</span></span>
>
>

### <a name="download-files"></a><span data-ttu-id="9d170-335">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="9d170-335">Download files</span></span>
<span data-ttu-id="9d170-336">Jakmile máte hello AccessPolicy a Lokátor sady, můžete si stáhnout soubory pomocí hello rozhraní API REST úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9d170-336">Once you have hello AccessPolicy and Locator set, you can download files using hello Azure Storage REST APIs.</span></span>  

> [!NOTE]
> <span data-ttu-id="9d170-337">Musíte přidat název souboru hello hello souboru chcete toodownload toohello Lokátor **cesta** přijaté v předchozí části hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9d170-337">You must add hello file name for hello file you want toodownload toohello Locator **Path** value received in hello previous section.</span></span> <span data-ttu-id="9d170-338">Například https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="9d170-338">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="9d170-339">.</span><span class="sxs-lookup"><span data-stu-id="9d170-339">.</span></span> <span data-ttu-id="9d170-340">.</span><span class="sxs-lookup"><span data-stu-id="9d170-340">.</span></span> <span data-ttu-id="9d170-341">.</span><span class="sxs-lookup"><span data-stu-id="9d170-341">.</span></span>
>
>

<span data-ttu-id="9d170-342">Další informace o práci s objekty BLOB úložiště Azure najdete v tématu [rozhraní API REST služby objektů Blob](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="9d170-342">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

<span data-ttu-id="9d170-343">V důsledku hello kódování úlohy, které jste provedli dříve (kódování do sady souborů MP4 s adaptivní) máte více souborů MP4, které jste ho progresivně stahovat.</span><span class="sxs-lookup"><span data-stu-id="9d170-343">As a result of hello encoding job that you performed earlier (encoding into Adaptive MP4 set), you have multiple MP4 files that you can progressively download.</span></span> <span data-ttu-id="9d170-344">Například:</span><span class="sxs-lookup"><span data-stu-id="9d170-344">For example:</span></span>    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a><span data-ttu-id="9d170-345">Vytváření streamovací adresa URL pro streamování obsahu</span><span class="sxs-lookup"><span data-stu-id="9d170-345">Creating a streaming URL for streaming content</span></span>
<span data-ttu-id="9d170-346">Následující kód ukazuje, jak Hello toocreate Lokátor streamování adresy URL:</span><span class="sxs-lookup"><span data-stu-id="9d170-346">hello following code shows how toocreate a streaming URL Locator:</span></span>

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

<span data-ttu-id="9d170-347">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="9d170-347">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

<span data-ttu-id="9d170-348">toostream technologie Smooth Streaming adresu URL původ v streamování přehrávač médií, musí připojit hello cesta vlastnost s názvem hello hello technologie Smooth Streaming manifest souboru, za nímž následuje "/ manifest".</span><span class="sxs-lookup"><span data-stu-id="9d170-348">toostream a Smooth Streaming origin URL in a streaming media player, you must append hello Path property with hello name of hello Smooth Streaming manifest file, followed by "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="9d170-349">toostream HLS, připojte (format = m3u8-aapl) po hello "/ manifest".</span><span class="sxs-lookup"><span data-stu-id="9d170-349">toostream HLS, append (format=m3u8-aapl) after hello "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="9d170-350">připojit toostream MPEG DASH (formát = mpd. čas csf) po hello "/ manifest".</span><span class="sxs-lookup"><span data-stu-id="9d170-350">toostream MPEG DASH, append (format=mpd-time-csf) after hello "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <span data-ttu-id="9d170-351"><a id="play"></a>Přehrání obsahu</span><span class="sxs-lookup"><span data-stu-id="9d170-351"><a id="play"></a>Play your content</span></span>
<span data-ttu-id="9d170-352">toostream video, použijete [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="9d170-352">toostream you video, use [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="9d170-353">tootest progresivní stahování, vložte adresu URL do prohlížeče (například aplikace Internet Explorer, Chrome, Safari).</span><span class="sxs-lookup"><span data-stu-id="9d170-353">tootest progressive download, paste a URL into a browser (for example, IE, Chrome, Safari).</span></span>

## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="9d170-354">Další kroky: Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="9d170-354">Next Steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="9d170-355">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="9d170-355">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
