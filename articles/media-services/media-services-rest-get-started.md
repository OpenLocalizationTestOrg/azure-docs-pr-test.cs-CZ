---
title: "Začínáme s doručováním obsahu na vyžádání pomocí REST | Microsoft Docs"
description: "Tento kurz vás provede jednotlivými kroky implementace aplikace pro doručování obsahu na vyžádání pomocí Azure Media Services pomocí rozhraní REST API."
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
ms.openlocfilehash: f304f7671465862123f64c8b0f9af95a7c828cc2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a><span data-ttu-id="e28db-103">Začínáme s doručováním obsahu na vyžádání pomocí REST</span><span class="sxs-lookup"><span data-stu-id="e28db-103">Get started with delivering content on demand using REST</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="e28db-104">Tento rychlý start vás provede jednotlivými kroky implementace aplikace pro doručování obsahu na vyžádání pomocí rozhraní API REST Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="e28db-104">This quickstart walks you through the steps of implementing a Video-on-Demand (VoD) content delivery application using Azure Media Services (AMS) REST APIs.</span></span>

<span data-ttu-id="e28db-105">Kurz představuje základní pracovní postup služby Media Services a nejběžnější programovací objekty a úkoly, které Media Services vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="e28db-105">The tutorial introduces the basic Media Services workflow and the most common programming objects and tasks required for Media Services development.</span></span> <span data-ttu-id="e28db-106">Po dokončení kurzu bude umět streamovat nebo progresivně stáhnout ukázkový multimediální soubor, který jste odeslali, zakódovali a stáhli.</span><span class="sxs-lookup"><span data-stu-id="e28db-106">At the completion of the tutorial, you will be able to stream or progressively download a sample media file that you uploaded, encoded, and downloaded.</span></span>

<span data-ttu-id="e28db-107">Následující obrázek ukazuje některé z nejčastěji používaných objektů při vývoji aplikace VoD na základě modelu Media Services OData.</span><span class="sxs-lookup"><span data-stu-id="e28db-107">The following image shows some of the most commonly used objects when developing VoD applications against the Media Services OData model.</span></span>

<span data-ttu-id="e28db-108">Kliknutím na obrázek zobrazíte jeho plnou velikost.</span><span class="sxs-lookup"><span data-stu-id="e28db-108">Click the image to view it full size.</span></span>  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a><span data-ttu-id="e28db-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e28db-109">Prerequisites</span></span>
<span data-ttu-id="e28db-110">Chcete-li začít vyvíjet pomocí služby Media Services pomocí rozhraní REST API jsou požadovány následující součásti.</span><span class="sxs-lookup"><span data-stu-id="e28db-110">The following prerequisites are required to start developing with Media Services with REST APIs.</span></span>

* <span data-ttu-id="e28db-111">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="e28db-111">An Azure account.</span></span> <span data-ttu-id="e28db-112">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e28db-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e28db-113">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="e28db-113">A Media Services account.</span></span> <span data-ttu-id="e28db-114">Pokud chcete vytvořit účet Media Services, přečtěte si článek [Jak vytvořit účet Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="e28db-114">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="e28db-115">Pochopení toho, jak pro vývoj pomocí Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="e28db-115">Understanding of how to develop with Media Services REST API.</span></span> <span data-ttu-id="e28db-116">Další informace najdete v tématu [Media Services REST API přehled](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="e28db-116">For more information, see [Media Services REST API overview](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="e28db-117">Aplikace podle vašeho výběru, který může odesílat požadavky a odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e28db-117">An application of your choice that can send HTTP requests and responses.</span></span> <span data-ttu-id="e28db-118">Tento kurz používá [Fiddler](http://www.telerik.com/download/fiddler).</span><span class="sxs-lookup"><span data-stu-id="e28db-118">This tutorial uses [Fiddler](http://www.telerik.com/download/fiddler).</span></span>

<span data-ttu-id="e28db-119">V tento rychlý Start jsou uvedeny následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="e28db-119">The following tasks are shown in this quickstart.</span></span>

1. <span data-ttu-id="e28db-120">Spuštění koncového bodu streamování (pomocí webu Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="e28db-120">Start streaming endpoint (using the Azure portal).</span></span>
2. <span data-ttu-id="e28db-121">Připojte k účtu Media Services pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="e28db-121">Connect to the Media Services account with REST API.</span></span>
3. <span data-ttu-id="e28db-122">Vytvoření nového prostředku a odeslání videosouboru pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="e28db-122">Create a new asset and upload a video file with REST API.</span></span>
4. <span data-ttu-id="e28db-123">Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="e28db-123">Encode the source file into a set of adaptive bitrate MP4 files with REST API.</span></span>
5. <span data-ttu-id="e28db-124">Publikování prostředku a get streamování a progresivního stahování adresy URL pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="e28db-124">Publish the asset and get streaming and progressive download URLs with REST API.</span></span>
6. <span data-ttu-id="e28db-125">Přehrání obsahu</span><span class="sxs-lookup"><span data-stu-id="e28db-125">Play your content.</span></span>

>[!NOTE]
><span data-ttu-id="e28db-126">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="e28db-126">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="e28db-127">Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad.</span><span class="sxs-lookup"><span data-stu-id="e28db-127">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="e28db-128">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="e28db-128">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="e28db-129">Podrobnosti o entitách AMS REST použitým v tomto tématu najdete v tématu [Azure Media Services REST API – referenční informace](/rest/api/media/services/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="e28db-129">For details about AMS REST entities used in this topic, see [Azure Media Services REST API Reference](/rest/api/media/services/azure-media-services-rest-api-reference).</span></span> <span data-ttu-id="e28db-130">Další informace naleznete v [konceptech Azure Media Services](media-services-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="e28db-130">Also, see [Azure Media Services concepts](media-services-concepts.md).</span></span>

>[!NOTE]
><span data-ttu-id="e28db-131">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="e28db-131">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="e28db-132">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="e28db-132">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="start-streaming-endpoints-using-the-azure-portal"></a><span data-ttu-id="e28db-133">Spuštění koncového bodu streamování pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e28db-133">Start streaming endpoints using the Azure portal</span></span>

<span data-ttu-id="e28db-134">Při práci se službou Azure Media Services je jedním z nejběžnější scénářů doručování videa prostřednictvím streamování s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="e28db-134">When working with Azure Media Services one of the most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="e28db-135">Služba Media Services poskytuje dynamické balení, které umožňuje doručovat obsah s adaptivní přenosovou rychlostí s kódováním MP4 ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming). není přitom potřeba ukládat předem zabalené verze pro každý z těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="e28db-135">Media Services provides dynamic packaging, which allows you to deliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having to store pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="e28db-136">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="e28db-136">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="e28db-137">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="e28db-137">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span>

<span data-ttu-id="e28db-138">Pokud chcete spustit koncový bod streamování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e28db-138">To start the streaming endpoint, do the following:</span></span>

1. <span data-ttu-id="e28db-139">Přihlaste se na [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e28db-139">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e28db-140">V okně Nastavení klikněte na Koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="e28db-140">In the Settings window, click Streaming endpoints.</span></span>
3. <span data-ttu-id="e28db-141">Klikněte na výchozí koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="e28db-141">Click the default streaming endpoint.</span></span>

    <span data-ttu-id="e28db-142">Zobrazí se okno VÝCHOZÍ KONCOVÝ BOD STREAMOVÁNÍ – PODROBNOSTI.</span><span class="sxs-lookup"><span data-stu-id="e28db-142">The DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="e28db-143">Klikněte na ikonu Spustit.</span><span class="sxs-lookup"><span data-stu-id="e28db-143">Click the Start icon.</span></span>
5. <span data-ttu-id="e28db-144">Kliknutím na tlačítko Uložit uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="e28db-144">Click the Save button to save your changes.</span></span>

## <span data-ttu-id="e28db-145"><a id="connect"></a>Připojení k účtu Media Services pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="e28db-145"><a id="connect"></a>Connect to the Media Services account with REST API</span></span>

<span data-ttu-id="e28db-146">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="e28db-146">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="e28db-147">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="e28db-147">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="e28db-148">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="e28db-148">You must make subsequent calls to the new URI.</span></span>

<span data-ttu-id="e28db-149">Například pokud po pokusu o připojení, vy máte následující:</span><span class="sxs-lookup"><span data-stu-id="e28db-149">For example, if after trying to connect, you got the following:</span></span>

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

<span data-ttu-id="e28db-150">Publikujte následující volání rozhraní API https://wamsbayclus001rest-hs.cloudapp.net/api/.</span><span class="sxs-lookup"><span data-stu-id="e28db-150">You should post your subsequent API calls to https://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

## <span data-ttu-id="e28db-151"><a id="upload"></a>Vytvoření nového prostředku a odeslání videosouboru pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="e28db-151"><a id="upload"></a>Create a new asset and upload a video file with REST API</span></span>

<span data-ttu-id="e28db-152">Ve službě Media Services můžete digitální soubory nahrát do assetu.</span><span class="sxs-lookup"><span data-stu-id="e28db-152">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="e28db-153">**Asset** entita může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a metadata o těchto souborech.)  Jakmile soubory odešlete do assetu, váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="e28db-153">The **Asset** entity can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and the metadata about these files.)  Once the files are uploaded into the asset, your content is stored securely in the cloud for further processing and streaming.</span></span>

<span data-ttu-id="e28db-154">Jedna z hodnot, které je nutné zadat při vytváření prostředek je možností vytvoření prostředku.</span><span class="sxs-lookup"><span data-stu-id="e28db-154">One of the values that you have to provide when creating an asset is asset creation options.</span></span> <span data-ttu-id="e28db-155">**Možnosti** vlastnost je hodnota výčtu, která popisuje možnosti šifrování, které prostředek může být vytvořen pomocí.</span><span class="sxs-lookup"><span data-stu-id="e28db-155">The **Options** property is an enumeration value that describes the encryption options that an Asset can be created with.</span></span> <span data-ttu-id="e28db-156">Platná hodnota je jedna z hodnot v seznamu níže není kombinace hodnot z tohoto seznamu:</span><span class="sxs-lookup"><span data-stu-id="e28db-156">A valid value is one of the values from the list below, not a combination of values from this list:</span></span>

* <span data-ttu-id="e28db-157">**Žádný** = **0** – nepoužívá se žádné šifrování.</span><span class="sxs-lookup"><span data-stu-id="e28db-157">**None** = **0** - No encryption is used.</span></span> <span data-ttu-id="e28db-158">Při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.</span><span class="sxs-lookup"><span data-stu-id="e28db-158">When using this option your content is not protected in transit or at rest in storage.</span></span>
    <span data-ttu-id="e28db-159">Pokud chcete pomocí progresivního stahování dodávat obsah ve formátu MP4, použijte tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="e28db-159">If you plan to deliver an MP4 using progressive download, use this option.</span></span>
* <span data-ttu-id="e28db-160">**StorageEncrypted** = **1** – šifruje vaše nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a odešle ji do Azure Storage kde bude uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="e28db-160">**StorageEncrypted** = **1** - Encrypts your clear content locally using AES-256 bit encryption and then uploads it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="e28db-161">Prostředky chráněné pomocí šifrování úložiště jsou před kódováním automaticky bez šifrování umístěny do systému souborů EFS a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="e28db-161">Assets protected with Storage Encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="e28db-162">Případem primárního použití šifrování úložiště je situace, kdy chcete zabezpečit soubory s vysoce kvalitními vstupními multimediálními soubory pomocí silného šifrování na disku.</span><span class="sxs-lookup"><span data-stu-id="e28db-162">The primary use case for Storage Encryption is when you want to secure your high-quality input media files with strong encryption at rest on disk.</span></span>
* <span data-ttu-id="e28db-163">**CommonEncryptionProtected** = **2** – tuto možnost použijte, pokud nahráváte obsah, který byl zašifrován a chráněný běžným šifrováním nebo DRM s technologií PlayReady (například technologie Smooth Streaming chránit pomocí DRM s technologií PlayReady).</span><span class="sxs-lookup"><span data-stu-id="e28db-163">**CommonEncryptionProtected** = **2** - Use this option if you are uploading content that has already been encrypted and protected with Common Encryption or PlayReady DRM (for example, Smooth Streaming protected with PlayReady DRM).</span></span>
* <span data-ttu-id="e28db-164">**EnvelopeEncryptionProtected** = **4** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES.</span><span class="sxs-lookup"><span data-stu-id="e28db-164">**EnvelopeEncryptionProtected** = **4** – Use this option if you are uploading HLS encrypted with AES.</span></span> <span data-ttu-id="e28db-165">Soubory musí mít kódování a zašifrované pomocí Správce transformací.</span><span class="sxs-lookup"><span data-stu-id="e28db-165">The files must have been encoded and encrypted by Transform Manager.</span></span>

### <a name="create-an-asset"></a><span data-ttu-id="e28db-166">Vytvořit prostředek</span><span class="sxs-lookup"><span data-stu-id="e28db-166">Create an asset</span></span>
<span data-ttu-id="e28db-167">Prostředek je kontejner pro více typů nebo sady objektů ve službě Media Services, včetně video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků.</span><span class="sxs-lookup"><span data-stu-id="e28db-167">An asset is a container for multiple types or sets of objects in Media Services, including video, audio, images, thumbnail collections, text tracks, and closed caption files.</span></span> <span data-ttu-id="e28db-168">Vytvoření prostředek v rozhraní REST API vyžaduje odesílání požadavku POST ke službě Media Services a umístění žádné vlastnosti informace o váš asset v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="e28db-168">In the REST API, creating an Asset requires sending POST request to Media Services and placing any property information about your asset in the request body.</span></span>

<span data-ttu-id="e28db-169">Následující příklad ukazuje, jak vytvořit prostředek.</span><span class="sxs-lookup"><span data-stu-id="e28db-169">The following example shows how to create an asset.</span></span>

<span data-ttu-id="e28db-170">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-170">**HTTP Request**</span></span>

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


<span data-ttu-id="e28db-171">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-171">**HTTP Response**</span></span>

<span data-ttu-id="e28db-172">V případě úspěchu se vrátí následující:</span><span class="sxs-lookup"><span data-stu-id="e28db-172">If successful, the following is returned:</span></span>

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

### <a name="create-an-assetfile"></a><span data-ttu-id="e28db-173">Vytvoření AssetFile</span><span class="sxs-lookup"><span data-stu-id="e28db-173">Create an AssetFile</span></span>
<span data-ttu-id="e28db-174">[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity představuje soubor video nebo zvuk, který je uložený v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e28db-174">The [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="e28db-175">Soubor asset je vždy přidružena k assetu a prostředek může obsahovat jednu nebo více AssetFiles.</span><span class="sxs-lookup"><span data-stu-id="e28db-175">An asset file is always associated with an asset, and an asset may contain one or many AssetFiles.</span></span> <span data-ttu-id="e28db-176">Media Services Encoder úloh selže, pokud objekt souboru asset není spojen s digitálnímu souboru v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="e28db-176">The Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="e28db-177">Po odeslání souboru digitálního média do kontejneru objektů blob, můžete použít **SLOUČENÍ** HTTP žádost o aktualizaci AssetFile s informacemi o souboru média (Jak uvidíte později v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="e28db-177">After you upload your digital media file into a blob container, you use the **MERGE** HTTP request to update the AssetFile with information about your media file (as shown later in the topic).</span></span>

<span data-ttu-id="e28db-178">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-178">**HTTP Request**</span></span>

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


<span data-ttu-id="e28db-179">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-179">**HTTP Response**</span></span>

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


### <a name="creating-the-accesspolicy-with-write-permission"></a><span data-ttu-id="e28db-180">Vytváření AccessPolicy s oprávnění k zápisu</span><span class="sxs-lookup"><span data-stu-id="e28db-180">Creating the AccessPolicy with write permission</span></span>
<span data-ttu-id="e28db-181">Před nahráním do úložiště objektů blob všechny soubory, nastavení přístupu zásady oprávnění pro zápis do prostředek.</span><span class="sxs-lookup"><span data-stu-id="e28db-181">Before uploading any files into blob storage, set the access policy rights for writing to an asset.</span></span> <span data-ttu-id="e28db-182">Kvůli tomu odeslat požadavek HTTP do sady entit AccessPolicies.</span><span class="sxs-lookup"><span data-stu-id="e28db-182">To do that, POST an HTTP request to the AccessPolicies entity set.</span></span> <span data-ttu-id="e28db-183">Zadejte hodnotu doba trvání v minutách, po vytvoření nebo zobrazí 500 chybová zpráva Interní Server zpět v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e28db-183">Define a DurationInMinutes value upon creation or you receive a 500 Internal Server error message back in response.</span></span> <span data-ttu-id="e28db-184">Další informace o AccessPolicies najdete v tématu [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span><span class="sxs-lookup"><span data-stu-id="e28db-184">For more information on AccessPolicies, see [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).</span></span>

<span data-ttu-id="e28db-185">Následující příklad ukazuje, jak vytvořit AccessPolicy:</span><span class="sxs-lookup"><span data-stu-id="e28db-185">The following example shows how to create an AccessPolicy:</span></span>

<span data-ttu-id="e28db-186">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-186">**HTTP Request**</span></span>

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

<span data-ttu-id="e28db-187">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-187">**HTTP Response**</span></span>

<span data-ttu-id="e28db-188">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="e28db-188">If successful, the following response is returned:</span></span>

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

### <a name="get-the-upload-url"></a><span data-ttu-id="e28db-189">Získat adresu URL pro odeslání</span><span class="sxs-lookup"><span data-stu-id="e28db-189">Get the Upload URL</span></span>

<span data-ttu-id="e28db-190">Pokud chcete získat adresu URL skutečného odeslání, vytvoření lokátoru SAS.</span><span class="sxs-lookup"><span data-stu-id="e28db-190">To receive the actual upload URL, create a SAS Locator.</span></span> <span data-ttu-id="e28db-191">Lokátory zadejte čas spuštění a typ koncového bodu připojení pro klienty, kteří mají přístup k souborům v prostředek.</span><span class="sxs-lookup"><span data-stu-id="e28db-191">Locators define the start time and type of connection endpoint for clients that want to access Files in an Asset.</span></span> <span data-ttu-id="e28db-192">Můžete vytvořit více Lokátor entit pro danou AccessPolicy a Asset dvojici pro zpracování různých klientských požadavků a potřebách.</span><span class="sxs-lookup"><span data-stu-id="e28db-192">You can create multiple Locator entities for a given AccessPolicy and Asset pair to handle different client requests and needs.</span></span> <span data-ttu-id="e28db-193">Každá z těchto lokátory používá hodnoty StartTime plus hodnota Doba trvání v minutách AccessPolicy určit dobu, kterou lze použít adresu URL.</span><span class="sxs-lookup"><span data-stu-id="e28db-193">Each of these Locators uses the StartTime value plus the DurationInMinutes value of the AccessPolicy to determine the length of time a URL can be used.</span></span> <span data-ttu-id="e28db-194">Další informace najdete v tématu [Lokátor](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="e28db-194">For more information, see [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>

<span data-ttu-id="e28db-195">Adresa URL typu SAS má následující formát:</span><span class="sxs-lookup"><span data-stu-id="e28db-195">A SAS URL has the following format:</span></span>

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

<span data-ttu-id="e28db-196">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="e28db-196">Some considerations apply:</span></span>

* <span data-ttu-id="e28db-197">Nemůže mít více než pět jedinečný lokátory spojené s danou Asset najednou.</span><span class="sxs-lookup"><span data-stu-id="e28db-197">You cannot have more than five unique Locators associated with a given Asset at one time.</span></span> <span data-ttu-id="e28db-198">Další informace najdete v tématu lokátoru.</span><span class="sxs-lookup"><span data-stu-id="e28db-198">For more information, see Locator.</span></span>
* <span data-ttu-id="e28db-199">Pokud potřebujete k nahrání souborů okamžitě, byste měli nastavit vaše hodnoty StartTime 5 minut před aktuálním časem.</span><span class="sxs-lookup"><span data-stu-id="e28db-199">If you need to upload your files immediately, you should set your StartTime value to five minutes before the current time.</span></span> <span data-ttu-id="e28db-200">Je to proto, že je možné, hodiny zkosení mezi klientský počítač a služba Media Services.</span><span class="sxs-lookup"><span data-stu-id="e28db-200">This is because there may be clock skew between your client machine and Media Services.</span></span> <span data-ttu-id="e28db-201">V následujícím formátu data a času musí být také hodnota pro čas spuštění: rrrr-MM-ddTHH (například "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="e28db-201">Also, your StartTime value must be in the following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>    
* <span data-ttu-id="e28db-202">Může být druhý 30-40 zpoždění po vytvoření lokátoru k případě, že je k dispozici pro použití.</span><span class="sxs-lookup"><span data-stu-id="e28db-202">There may be a 30-40 second delay after a Locator is created to when it is available for use.</span></span> <span data-ttu-id="e28db-203">Tento problém se vztahuje na SAS adresa URL a lokátory původu.</span><span class="sxs-lookup"><span data-stu-id="e28db-203">This issue applies to both SAS URL and Origin Locators.</span></span>

<span data-ttu-id="e28db-204">Další informace o tokenu SAS najdete v části lokátory [to](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogu.</span><span class="sxs-lookup"><span data-stu-id="e28db-204">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="e28db-205">Následující příklad ukazuje, jak vytvořit lokátor SAS adresa URL, podle definice vlastnost typu v textu požadavku ("1" pro Lokátor SAS) a "2" pro Lokátor původ na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="e28db-205">The following example shows how to create a SAS URL Locator, as defined by the Type property in the request body ("1" for a SAS locator and "2" for an On-Demand origin locator).</span></span> <span data-ttu-id="e28db-206">**Cesta** vlastnost vrátil obsahuje adresu URL, kterou musíte použít k odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="e28db-206">The **Path** property returned contains the URL that you must use to upload your file.</span></span>

<span data-ttu-id="e28db-207">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-207">**HTTP Request**</span></span>

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


<span data-ttu-id="e28db-208">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-208">**HTTP Response**</span></span>

<span data-ttu-id="e28db-209">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="e28db-209">If successful, the following response is returned:</span></span>

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

### <a name="upload-a-file-into-a-blob-storage-container"></a><span data-ttu-id="e28db-210">Nahrát soubor do kontejneru úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="e28db-210">Upload a file into a blob storage container</span></span>
<span data-ttu-id="e28db-211">Jakmile máte AccessPolicy a Lokátor nastavit, skutečný soubor nahraje do kontejner úložiště objektů blob v Azure pomocí rozhraní API REST úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e28db-211">Once you have the AccessPolicy and Locator set, the actual file is uploaded to an Azure blob storage container using the Azure Storage REST APIs.</span></span> <span data-ttu-id="e28db-212">Soubory musíte nahrát jako objekty BLOB bloku.</span><span class="sxs-lookup"><span data-stu-id="e28db-212">You must upload the files as block blobs.</span></span> <span data-ttu-id="e28db-213">Objekty BLOB stránky nejsou podporovány službou Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="e28db-213">Page blobs are not supported by Azure Media Services.</span></span>  

> [!NOTE]
> <span data-ttu-id="e28db-214">Musíte přidat název souboru pro soubor, který chcete nahrát do Lokátor **cesta** přijaté v předchozí části hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e28db-214">You must add the file name for the file you want to upload to the Locator **Path** value received in the previous section.</span></span> <span data-ttu-id="e28db-215">Například https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="e28db-215">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="e28db-216">.</span><span class="sxs-lookup"><span data-stu-id="e28db-216">.</span></span> <span data-ttu-id="e28db-217">.</span><span class="sxs-lookup"><span data-stu-id="e28db-217">.</span></span> <span data-ttu-id="e28db-218">.</span><span class="sxs-lookup"><span data-stu-id="e28db-218">.</span></span>
>
>

<span data-ttu-id="e28db-219">Další informace o práci s objekty BLOB úložiště Azure najdete v tématu [rozhraní API REST služby objektů Blob](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="e28db-219">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

### <a name="update-the-assetfile"></a><span data-ttu-id="e28db-220">Aktualizace AssetFile</span><span class="sxs-lookup"><span data-stu-id="e28db-220">Update the AssetFile</span></span>
<span data-ttu-id="e28db-221">Teď, když jste odeslali souboru, aktualizujte informace FileAsset velikost (a další).</span><span class="sxs-lookup"><span data-stu-id="e28db-221">Now that you've uploaded your file, update the FileAsset size (and other) information.</span></span> <span data-ttu-id="e28db-222">Například:</span><span class="sxs-lookup"><span data-stu-id="e28db-222">For example:</span></span>

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


<span data-ttu-id="e28db-223">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-223">**HTTP Response**</span></span>

<span data-ttu-id="e28db-224">V případě úspěchu se vrátí následující:</span><span class="sxs-lookup"><span data-stu-id="e28db-224">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <a name="delete-the-locator-and-accesspolicy"></a><span data-ttu-id="e28db-225">Odstraňte Lokátor a AccessPolicy</span><span class="sxs-lookup"><span data-stu-id="e28db-225">Delete the Locator and AccessPolicy</span></span>
<span data-ttu-id="e28db-226">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-226">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="e28db-227">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-227">**HTTP Response**</span></span>

<span data-ttu-id="e28db-228">V případě úspěchu se vrátí následující:</span><span class="sxs-lookup"><span data-stu-id="e28db-228">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

<span data-ttu-id="e28db-229">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-229">**HTTP Request**</span></span>

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

<span data-ttu-id="e28db-230">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-230">**HTTP Response**</span></span>

<span data-ttu-id="e28db-231">V případě úspěchu se vrátí následující:</span><span class="sxs-lookup"><span data-stu-id="e28db-231">If successful, the following is returned:</span></span>

    HTTP/1.1 204 No Content
    ...

## <span data-ttu-id="e28db-232"><a id="encode"></a>Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="e28db-232"><a id="encode"></a>Encode the source file into a set of adaptive bitrate MP4 files</span></span>

<span data-ttu-id="e28db-233">Po zpracování, které může být zakódován prostředky ve službě Media Services, média, transmuxovat, označit vodoznakem a tak dále před doručení klientům.</span><span class="sxs-lookup"><span data-stu-id="e28db-233">After ingesting Assets into Media Services, media can be encoded, transmuxed, watermarked, and so on, before it is delivered to clients.</span></span> <span data-ttu-id="e28db-234">Tyto aktivity se plánují a spouštějí s několika instancemi role na pozadí, abyste měli zajištěný vysoký výkon a dostupnost.</span><span class="sxs-lookup"><span data-stu-id="e28db-234">These activities are scheduled and run against multiple background role instances to ensure high performance and availability.</span></span> <span data-ttu-id="e28db-235">Tyto aktivity se nazývají úlohy a každá úloha se skládá z atomických úloh, které vykonávají samotnou práci na souboru prostředku (Další informace najdete v tématu [úlohy](/rest/api/media/services/job), [úloh](/rest/api/media/services/task) popisy).</span><span class="sxs-lookup"><span data-stu-id="e28db-235">These activities are called Jobs and each Job is composed of atomic Tasks that do the actual work on the Asset file (for more information, see [Job](/rest/api/media/services/job), [Task](/rest/api/media/services/task) descriptions).</span></span>

<span data-ttu-id="e28db-236">Jak již bylo zmíněno dříve, při práci se službou Azure Media Services jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům.</span><span class="sxs-lookup"><span data-stu-id="e28db-236">As was mentioned earlier, when working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="e28db-237">Služba Media Services umí dynamicky balit sady souborů MP4 s adaptivní přenosovou do jedné z následujících formátů: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="e28db-237">Media Services can dynamically package a set of adaptive bitrate MP4 files into one of the following formats: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span>

<span data-ttu-id="e28db-238">V následující části ukazuje, jak vytvořit úlohu, která obsahuje jednu úlohu, kódování.</span><span class="sxs-lookup"><span data-stu-id="e28db-238">The following section shows how to create a job that contains one encoding task.</span></span> <span data-ttu-id="e28db-239">Úloha Určuje převod souboru mezzanine do sady s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi pomocí **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="e28db-239">The task specifies to transcode the mezzanine file into a set of adaptive bitrate MP4s using **Media Encoder Standard**.</span></span> <span data-ttu-id="e28db-240">V části také ukazuje, jak monitorovat zpracování průběh úlohy.</span><span class="sxs-lookup"><span data-stu-id="e28db-240">The section also shows how to monitor the job processing progress.</span></span> <span data-ttu-id="e28db-241">Po dokončení úlohy by mohli k vytvoření lokátorů, které jsou potřebné k získání přístupu k vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="e28db-241">When the job is complete, you would be able to create locators that are needed to get access to your assets.</span></span>

### <a name="get-a-media-processor"></a><span data-ttu-id="e28db-242">Získat procesor médií</span><span class="sxs-lookup"><span data-stu-id="e28db-242">Get a media processor</span></span>
<span data-ttu-id="e28db-243">Ve službě Media Services je procesor médií komponenty, která zpracovává zpracování specifické pro úlohy, jako je například kódování, převod formátu, šifrování nebo dešifrování objektu mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="e28db-243">In Media Services, a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="e28db-244">Pro kódování úkol uvedené v tomto kurzu budeme používat Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="e28db-244">For the encoding task shown in this tutorial, we are going to use the Media Encoder Standard.</span></span>

<span data-ttu-id="e28db-245">Následující kód požadavků kodér id.</span><span class="sxs-lookup"><span data-stu-id="e28db-245">The following code requests the encoder's id.</span></span>

<span data-ttu-id="e28db-246">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-246">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="e28db-247">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-247">**HTTP Response**</span></span>

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

### <a name="create-a-job"></a><span data-ttu-id="e28db-248">Vytvoření úlohy</span><span class="sxs-lookup"><span data-stu-id="e28db-248">Create a job</span></span>
<span data-ttu-id="e28db-249">Každá úloha může mít jeden nebo více úloh, v závislosti na typu zpracování, který chcete provést.</span><span class="sxs-lookup"><span data-stu-id="e28db-249">Each Job can have one or more Tasks depending on the type of processing that you want to accomplish.</span></span> <span data-ttu-id="e28db-250">Přes rozhraní REST API, můžete vytvořit úlohy a jejich související úlohy v jedné ze dvou způsobů: úloh může být definována vložením prostřednictvím úlohy navigační vlastnost u entity úlohy nebo zpracování dávky OData.</span><span class="sxs-lookup"><span data-stu-id="e28db-250">Through the REST API, you can create Jobs and their related Tasks in one of two ways: Tasks can be defined inline through the Tasks navigation property on Job entities, or through OData batch processing.</span></span> <span data-ttu-id="e28db-251">Sada SDK služby Media používá dávkové zpracování.</span><span class="sxs-lookup"><span data-stu-id="e28db-251">The Media Services SDK uses batch processing.</span></span> <span data-ttu-id="e28db-252">Čitelnější příklady kódu v tomto tématu jsou úlohy však definována vložením.</span><span class="sxs-lookup"><span data-stu-id="e28db-252">However, for the readability of the code examples in this topic, tasks are defined inline.</span></span> <span data-ttu-id="e28db-253">Informace o zpracování dávky, najdete v části [Open Data Protocol (OData) dávkové zpracování](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="e28db-253">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

<span data-ttu-id="e28db-254">Následující příklad ukazuje, jak vytvořit a odeslat úlohu s jedním úloh nastavit ke kódování videa na konkrétní řešení a kvality.</span><span class="sxs-lookup"><span data-stu-id="e28db-254">The following example shows you how to create and post a Job with one Task set to encode a video at a specific resolution and quality.</span></span> <span data-ttu-id="e28db-255">V následující části dokumentace obsahuje seznam všech [úkolů přednastavení](http://msdn.microsoft.com/library/mt269960) nepodporuje procesoru Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="e28db-255">The following documentation section contains the list of all the [task presets](http://msdn.microsoft.com/library/mt269960) supported by the Media Encoder Standard processor.</span></span>  

<span data-ttu-id="e28db-256">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-256">**HTTP Request**</span></span>

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

<span data-ttu-id="e28db-257">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-257">**HTTP Response**</span></span>

<span data-ttu-id="e28db-258">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="e28db-258">If successful, the following response is returned:</span></span>

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


<span data-ttu-id="e28db-259">Poznámka: v žádné žádosti o úlohu několik důležité věci:</span><span class="sxs-lookup"><span data-stu-id="e28db-259">There are a few important things to note in any Job request:</span></span>

* <span data-ttu-id="e28db-260">Taskbody – vlastnosti používaly literál XML Definujte počet vstup nebo výstup prostředky, které jsou používány úlohu.</span><span class="sxs-lookup"><span data-stu-id="e28db-260">TaskBody properties MUST use literal XML to define the number of input, or output assets that are used by the Task.</span></span> <span data-ttu-id="e28db-261">Úloha téma obsahuje definici schématu XML pro soubor XML.</span><span class="sxs-lookup"><span data-stu-id="e28db-261">The Task topic contains the XML Schema Definition for the XML.</span></span>
* <span data-ttu-id="e28db-262">V definici taskbody – každý vnitřní hodnota <inputAsset> a <outputAsset> musí být nastavena jako JobInputAsset(value) nebo JobOutputAsset(value).</span><span class="sxs-lookup"><span data-stu-id="e28db-262">In the TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="e28db-263">Úloha může mít více prostředků výstup.</span><span class="sxs-lookup"><span data-stu-id="e28db-263">A task can have multiple output assets.</span></span> <span data-ttu-id="e28db-264">Jeden JobOutputAsset(x) lze použít jako výstup úlohy pro úlohu pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="e28db-264">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="e28db-265">Můžete zadat JobInputAsset nebo JobOutputAsset jako vstupní datový zdroj úlohy.</span><span class="sxs-lookup"><span data-stu-id="e28db-265">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="e28db-266">Úlohy nesmí tvoří cyklus.</span><span class="sxs-lookup"><span data-stu-id="e28db-266">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="e28db-267">Hodnota parametru, který můžete předat JobInputAsset nebo JobOutputAsset představuje hodnotu indexu pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="e28db-267">The value parameter that you pass to JobInputAsset or JobOutputAsset represents the index value for an Asset.</span></span> <span data-ttu-id="e28db-268">Skutečné prostředky jsou definovány v navigační vlastnosti InputMediaAssets a OutputMediaAssets v definici úlohy entity.</span><span class="sxs-lookup"><span data-stu-id="e28db-268">The actual Assets are defined in the InputMediaAssets and OutputMediaAssets navigation properties on the Job entity definition.</span></span>

> [!NOTE]
> <span data-ttu-id="e28db-269">Služba Media Services je integrovaná v OData v3, jsou jednotlivé prostředky v InputMediaAssets a OutputMediaAssets navigační vlastnost kolekce odkazuje prostřednictvím "__metadata: identifikátor uri" dvojice název hodnota.</span><span class="sxs-lookup"><span data-stu-id="e28db-269">Because Media Services is built on OData v3, the individual assets in InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
>
>

* <span data-ttu-id="e28db-270">InputMediaAssets se mapuje na jeden nebo více prostředků, které jste vytvořili ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="e28db-270">InputMediaAssets maps to one or more Assets you have created in Media Services.</span></span> <span data-ttu-id="e28db-271">OutputMediaAssets jsou vytvořené v systému.</span><span class="sxs-lookup"><span data-stu-id="e28db-271">OutputMediaAssets are created by the system.</span></span> <span data-ttu-id="e28db-272">Neodkazujte existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="e28db-272">They do not reference an existing asset.</span></span>
* <span data-ttu-id="e28db-273">Používání atributu assetName, může mít název OutputMediaAssets.</span><span class="sxs-lookup"><span data-stu-id="e28db-273">OutputMediaAssets can be named using the assetName attribute.</span></span> <span data-ttu-id="e28db-274">Pokud tento atribut není k dispozici, je název OutputMediaAsset bez ohledu na hodnotu vnitřní text z <outputAsset> element je s příponou název úlohy hodnota nebo hodnota Id úlohy (v případě, kde není definována vlastnost Name).</span><span class="sxs-lookup"><span data-stu-id="e28db-274">If this attribute is not present, then the name of the OutputMediaAsset is whatever the inner text value of the <outputAsset> element is with a suffix of either the Job Name value, or the Job Id value (in the case where the Name property isn't defined).</span></span> <span data-ttu-id="e28db-275">Například pokud nastavíte hodnotu assetName "Ukázce", pak vlastnost názvu OutputMediaAsset se nastavuje "Ukázce".</span><span class="sxs-lookup"><span data-stu-id="e28db-275">For example, if you set a value for assetName to "Sample", then the OutputMediaAsset Name property would be set to "Sample".</span></span> <span data-ttu-id="e28db-276">Ale pokud jste nenastavili hodnotu assetName, ale nastavena název úlohy na "NewJob", pak OutputMediaAsset název bude "_NewJob JobOutputAsset (hodnota)".</span><span class="sxs-lookup"><span data-stu-id="e28db-276">However, if you did not set a value for assetName, but did set the job name to "NewJob", then the OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob".</span></span>

    <span data-ttu-id="e28db-277">Následující příklad ukazuje, jak nastavit atribut assetName:</span><span class="sxs-lookup"><span data-stu-id="e28db-277">The following example shows how to set the assetName attribute:</span></span>

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* <span data-ttu-id="e28db-278">Chcete-li povolit řetězení úloh:</span><span class="sxs-lookup"><span data-stu-id="e28db-278">To enable task chaining:</span></span>

  * <span data-ttu-id="e28db-279">Úloha musí mít alespoň dvě úlohy</span><span class="sxs-lookup"><span data-stu-id="e28db-279">A job must have at least two tasks</span></span>
  * <span data-ttu-id="e28db-280">Musí existovat alespoň jeden úkol, jejichž vstup je výstup jiná úloha v úloze.</span><span class="sxs-lookup"><span data-stu-id="e28db-280">There must be at least one task whose input is output of another task in the job.</span></span>

<span data-ttu-id="e28db-281">Další informace najdete v tématu [vytváření úlohu kódování s Media Services REST API](media-services-rest-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="e28db-281">For more information see, [Creating an Encoding Job with the Media Services REST API](media-services-rest-encode-asset.md).</span></span>

### <a name="monitor-processing-progress"></a><span data-ttu-id="e28db-282">Monitorování zpracování průběh</span><span class="sxs-lookup"><span data-stu-id="e28db-282">Monitor Processing Progress</span></span>
<span data-ttu-id="e28db-283">Stav úlohy můžete načíst pomocí vlastnosti stavu, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="e28db-283">You can retrieve the Job status by using the State property, as shown in the following example.</span></span>

<span data-ttu-id="e28db-284">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-284">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


<span data-ttu-id="e28db-285">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-285">**HTTP Response**</span></span>

<span data-ttu-id="e28db-286">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="e28db-286">If successful, the following response is returned:</span></span>

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


### <a name="cancel-a-job"></a><span data-ttu-id="e28db-287">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="e28db-287">Cancel a job</span></span>
<span data-ttu-id="e28db-288">Služba Media Services umožňuje zrušit probíhající úlohy prostřednictvím funkce CancelJob.</span><span class="sxs-lookup"><span data-stu-id="e28db-288">Media Services allows you to cancel running jobs through the CancelJob function.</span></span> <span data-ttu-id="e28db-289">Toto volání vrátí, a kód 400 chyby, pokud se pokusíte zrušení úlohy, když se zruší její stav, zrušení, chyby nebo skončila.</span><span class="sxs-lookup"><span data-stu-id="e28db-289">This call returns a 400 error code if you try to cancel a Job when its state is canceled, canceling, error, or finished.</span></span>

<span data-ttu-id="e28db-290">Následující příklad ukazuje způsob volání CancelJob.</span><span class="sxs-lookup"><span data-stu-id="e28db-290">The following example shows how to call CancelJob.</span></span>

<span data-ttu-id="e28db-291">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-291">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


<span data-ttu-id="e28db-292">V případě úspěchu se vrátí kód odpovědi 204 s žádný text zprávy.</span><span class="sxs-lookup"><span data-stu-id="e28db-292">If successful, a 204 response code is returned with no message body.</span></span>

> [!NOTE]
> <span data-ttu-id="e28db-293">Je nutné kódování URL úlohu s id (obvykle nb:jid:UUID: somevalue) při předávání v CancelJob jako parametr.</span><span class="sxs-lookup"><span data-stu-id="e28db-293">You must URL-encode the job id (normally nb:jid:UUID: somevalue) when passing it in as a parameter to CancelJob.</span></span>
>
>

### <a name="get-the-output-asset"></a><span data-ttu-id="e28db-294">Získat výstupní asset</span><span class="sxs-lookup"><span data-stu-id="e28db-294">Get the output asset</span></span>
<span data-ttu-id="e28db-295">Následující kód ukazuje, jak požádat o výstupní asset ID.</span><span class="sxs-lookup"><span data-stu-id="e28db-295">The following code shows how to request the output asset Id.</span></span>

<span data-ttu-id="e28db-296">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-296">**HTTP Request**</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="e28db-297">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="e28db-297">**HTTP Response**</span></span>

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



## <span data-ttu-id="e28db-298"><a id="publish_get_urls"></a>Publikování prostředku a get streamování a progresivního stahování adresy URL pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="e28db-298"><a id="publish_get_urls"></a>Publish the asset and get streaming and progressive download URLs with REST API</span></span>

<span data-ttu-id="e28db-299">Pokud chcete prostředek streamovat nebo stáhnout, musíte ho nejdřív „publikovat“ vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="e28db-299">To stream or download an asset, you first need to "publish" it by creating a locator.</span></span> <span data-ttu-id="e28db-300">Lokátory zajišťují přístup k souborům, které jsou obsaženy v assetu.</span><span class="sxs-lookup"><span data-stu-id="e28db-300">Locators provide access to files contained in the asset.</span></span> <span data-ttu-id="e28db-301">Služba Media Services podporuje dva typy lokátorů: lokátor OnDemandOrigin, používaný ke streamování médií (například MPEG DASH, HLS nebo technologie Smooth Streaming), a lokátor s přístupovým podpisem (SAS), používaný ke stahování mediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="e28db-301">Media Services supports two types of locators: OnDemandOrigin locators, used to stream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used to download media files.</span></span> <span data-ttu-id="e28db-302">Další informace o tokenu SAS najdete v části lokátory [to](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogu.</span><span class="sxs-lookup"><span data-stu-id="e28db-302">For more information about SAS locators see [this](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.</span></span>

<span data-ttu-id="e28db-303">Po vytvoření lokátorů můžete sestavit adresy URL, které se používají ke streamování a stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="e28db-303">Once you create the locators, you can build the URLs that are used to stream or download your files.</span></span>

>[!NOTE]
><span data-ttu-id="e28db-304">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="e28db-304">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="e28db-305">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="e28db-305">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span>

<span data-ttu-id="e28db-306">Streamovací adresa URL pro technologii Smooth Streaming má následující formát:</span><span class="sxs-lookup"><span data-stu-id="e28db-306">A streaming URL for Smooth Streaming has the following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="e28db-307">Streamovací adresa URL pro HLS má následující formát:</span><span class="sxs-lookup"><span data-stu-id="e28db-307">A streaming URL for HLS has the following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="e28db-308">Streamovací adresa URL pro MPEG DASH má následující formát:</span><span class="sxs-lookup"><span data-stu-id="e28db-308">A streaming URL for MPEG DASH has the following format:</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="e28db-309">SAS adresa URL používaná ke stahování souborů má následující formát:</span><span class="sxs-lookup"><span data-stu-id="e28db-309">A SAS URL used to download files has the following format:</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="e28db-310">V této části ukazuje, jak provádět následující úlohy nezbytné "publikovat" vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="e28db-310">This section shows how to perform the following tasks necessary to "publish" your assets.</span></span>  

* <span data-ttu-id="e28db-311">Vytváření AccessPolicy s oprávněními ke čtení</span><span class="sxs-lookup"><span data-stu-id="e28db-311">Creating the AccessPolicy with read permission</span></span>
* <span data-ttu-id="e28db-312">Vytváření SAS adresa URL pro stahování obsahu</span><span class="sxs-lookup"><span data-stu-id="e28db-312">Creating a SAS URL for downloading content</span></span>
* <span data-ttu-id="e28db-313">Vytváření původní adresu URL pro streamování obsahu</span><span class="sxs-lookup"><span data-stu-id="e28db-313">Creating an origin URL for streaming content</span></span>

### <a name="creating-the-accesspolicy-with-read-permission"></a><span data-ttu-id="e28db-314">Vytváření AccessPolicy s oprávněními ke čtení</span><span class="sxs-lookup"><span data-stu-id="e28db-314">Creating the AccessPolicy with read permission</span></span>
<span data-ttu-id="e28db-315">Před stažením nebo streamování žádný obsah média, nejprve definovat AccessPolicy s oprávněními ke čtení a vytvořte odpovídající Lokátor entita, která určuje typ mechanismus doručování, který chcete povolit pro klienty.</span><span class="sxs-lookup"><span data-stu-id="e28db-315">Before downloading or streaming any media content, first define an AccessPolicy with read permissions and create the appropriate Locator entity that specifies the type of delivery mechanism you want to enable for your clients.</span></span> <span data-ttu-id="e28db-316">Další informace o dostupných vlastnostech najdete v tématu [vlastností Entity AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span><span class="sxs-lookup"><span data-stu-id="e28db-316">For more information on the properties available, see [AccessPolicy Entity Properties](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).</span></span>

<span data-ttu-id="e28db-317">Následující příklad ukazuje, jak zadat AccessPolicy pro oprávnění ke čtení pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="e28db-317">The following example shows how to specify the AccessPolicy for read permissions for a given Asset.</span></span>

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

<span data-ttu-id="e28db-318">V případě úspěchu se vrátí kód 201 úspěšnosti popisující AccessPolicy entity, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="e28db-318">If successful, a 201 success code is returned describing the AccessPolicy entity that you created.</span></span> <span data-ttu-id="e28db-319">Pak použijete AccessPolicy Id společně s Id prostředku asset, který obsahuje soubor, který chcete vytvořit lokátor entity doručit (například výstupní asset).</span><span class="sxs-lookup"><span data-stu-id="e28db-319">You then use the AccessPolicy Id along with the Asset Id of the asset that contains the file you want to deliver(such as an output asset) to create the Locator entity.</span></span>

> [!NOTE]
> <span data-ttu-id="e28db-320">Tento základní pracovní postup je stejný jako nahrání souboru, když příjem prostředek (jak je popsané v tomto tématu výše).</span><span class="sxs-lookup"><span data-stu-id="e28db-320">This basic workflow is the same as uploading a file when ingesting an Asset (as was discussed earlier in this topic).</span></span> <span data-ttu-id="e28db-321">Také jako nahrávání souborů, pokud vy (nebo klientů) potřebovat přístup k souborům okamžitě, můžete nastavte vaše hodnoty StartTime 5 minut před aktuálním časem.</span><span class="sxs-lookup"><span data-stu-id="e28db-321">Also, like uploading files, if you (or your clients) need to access your files immediately, set your StartTime value to five minutes before the current time.</span></span> <span data-ttu-id="e28db-322">Tato akce je nutné, protože je možné, hodiny zkosení mezi klientem a služba Media Services.</span><span class="sxs-lookup"><span data-stu-id="e28db-322">This action is necessary because there may be clock skew between the client and Media Services.</span></span> <span data-ttu-id="e28db-323">Hodnota čas spuštění musí být ve formátu data a času: rrrr-MM-ddTHH (například "2014-05-23T17:53:50Z").</span><span class="sxs-lookup"><span data-stu-id="e28db-323">The StartTime value must be in the following DateTime format: YYYY-MM-DDTHH:mm:ssZ (for example, "2014-05-23T17:53:50Z").</span></span>
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a><span data-ttu-id="e28db-324">Vytváření SAS adresa URL pro stahování obsahu</span><span class="sxs-lookup"><span data-stu-id="e28db-324">Creating a SAS URL for downloading content</span></span>
<span data-ttu-id="e28db-325">Následující kód ukazuje, jak získat adresu URL, která slouží k stažení souboru média vytvořen a odesláno dříve.</span><span class="sxs-lookup"><span data-stu-id="e28db-325">The following code shows how to get a URL that can be used to download a media file created and uploaded previously.</span></span> <span data-ttu-id="e28db-326">AccessPolicy má číst sadu oprávnění, a Lokátor cesta odkazuje na adresu URL typu SAS stahování.</span><span class="sxs-lookup"><span data-stu-id="e28db-326">The AccessPolicy has read permissions set and the Locator path refers to a SAS download URL.</span></span>

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

<span data-ttu-id="e28db-327">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="e28db-327">If successful, the following response is returned:</span></span>

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


<span data-ttu-id="e28db-328">Vrácený **cesta** vlastnost obsahuje adresu URL SAS.</span><span class="sxs-lookup"><span data-stu-id="e28db-328">The returned **Path** property contains the SAS URL.</span></span>

> [!NOTE]
> <span data-ttu-id="e28db-329">Pokud si stáhnout obsah šifrování úložiště, musí vám ručně dešifrovat před vykreslením nebo použijte MediaProcessor dešifrování úložiště úlohy zpracování výstupních zpracovaných souborů v nešifrované podobě k OutputAsset a pak stáhnout z tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="e28db-329">If you download storage encrypted content, you must manually decrypt it before rendering it, or use the Storage Decryption MediaProcessor in a processing task to output processed files in the clear to an OutputAsset and then download from that Asset.</span></span> <span data-ttu-id="e28db-330">Další informace o zpracování najdete v části Vytvoření úlohu kódování pomocí Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="e28db-330">For more information on processing, see Creating an Encoding Job with the Media Services REST API.</span></span> <span data-ttu-id="e28db-331">SAS adresa URL lokátory navíc nelze aktualizovat, po jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="e28db-331">Also, SAS URL Locators cannot be updated after they have been created.</span></span> <span data-ttu-id="e28db-332">Například nelze znovu použít stejné Lokátor s hodnotou aktualizované čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="e28db-332">For example, you cannot reuse the same Locator with an updated StartTime value.</span></span> <span data-ttu-id="e28db-333">To je kvůli způsob, jakým se vytvářejí SAS adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e28db-333">This is because of the way SAS URLs are created.</span></span> <span data-ttu-id="e28db-334">Pokud chcete získat přístup ke stažení prostředek po vypršení platnosti lokátoru, musíte vytvořit novou na nový čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="e28db-334">If you want to access an asset for download after a Locator has expired, then you must create a new one with a new StartTime.</span></span>
>
>

### <a name="download-files"></a><span data-ttu-id="e28db-335">Stažení souborů</span><span class="sxs-lookup"><span data-stu-id="e28db-335">Download files</span></span>
<span data-ttu-id="e28db-336">Jakmile máte AccessPolicy a Lokátor nastavení, můžete si stáhnout soubory pomocí rozhraní API REST úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="e28db-336">Once you have the AccessPolicy and Locator set, you can download files using the Azure Storage REST APIs.</span></span>  

> [!NOTE]
> <span data-ttu-id="e28db-337">Musíte přidat název souboru pro soubor, který chcete stáhnout do Lokátor **cesta** přijaté v předchozí části hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e28db-337">You must add the file name for the file you want to download to the Locator **Path** value received in the previous section.</span></span> <span data-ttu-id="e28db-338">Například https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span><span class="sxs-lookup"><span data-stu-id="e28db-338">For example, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?</span></span> <span data-ttu-id="e28db-339">.</span><span class="sxs-lookup"><span data-stu-id="e28db-339">.</span></span> <span data-ttu-id="e28db-340">.</span><span class="sxs-lookup"><span data-stu-id="e28db-340">.</span></span> <span data-ttu-id="e28db-341">.</span><span class="sxs-lookup"><span data-stu-id="e28db-341">.</span></span>
>
>

<span data-ttu-id="e28db-342">Další informace o práci s objekty BLOB úložiště Azure najdete v tématu [rozhraní API REST služby objektů Blob](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span><span class="sxs-lookup"><span data-stu-id="e28db-342">For more information on working with Azure storage blobs, see [Blob Service REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).</span></span>

<span data-ttu-id="e28db-343">V důsledku úlohy kódování, které jste provedli starší (kódování do sady souborů MP4 s adaptivní) máte více souborů MP4, které jste ho progresivně stahovat.</span><span class="sxs-lookup"><span data-stu-id="e28db-343">As a result of the encoding job that you performed earlier (encoding into Adaptive MP4 set), you have multiple MP4 files that you can progressively download.</span></span> <span data-ttu-id="e28db-344">Například:</span><span class="sxs-lookup"><span data-stu-id="e28db-344">For example:</span></span>    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a><span data-ttu-id="e28db-345">Vytváření streamovací adresa URL pro streamování obsahu</span><span class="sxs-lookup"><span data-stu-id="e28db-345">Creating a streaming URL for streaming content</span></span>
<span data-ttu-id="e28db-346">Následující kód ukazuje, jak vytvořit lokátor streamování adresy URL:</span><span class="sxs-lookup"><span data-stu-id="e28db-346">The following code shows how to create a streaming URL Locator:</span></span>

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

<span data-ttu-id="e28db-347">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="e28db-347">If successful, the following response is returned:</span></span>

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

<span data-ttu-id="e28db-348">K vysílání datového proudu technologie Smooth Streaming původní adresu URL v streamování přehrávač médií, musíte připojit cesta vlastnost s názvem technologie Smooth Streaming manifest souboru, za nímž následuje "/ manifest".</span><span class="sxs-lookup"><span data-stu-id="e28db-348">To stream a Smooth Streaming origin URL in a streaming media player, you must append the Path property with the name of the Smooth Streaming manifest file, followed by "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

<span data-ttu-id="e28db-349">K vysílání datového proudu HLS, připojte (format = m3u8-aapl) po "/ manifest".</span><span class="sxs-lookup"><span data-stu-id="e28db-349">To stream HLS, append (format=m3u8-aapl) after the "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

<span data-ttu-id="e28db-350">Chcete-li stream MPEG DASH, připojte (formát = mpd. čas csf) po "/ manifest".</span><span class="sxs-lookup"><span data-stu-id="e28db-350">To stream MPEG DASH, append (format=mpd-time-csf) after the "/manifest".</span></span>

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <span data-ttu-id="e28db-351"><a id="play"></a>Přehrání obsahu</span><span class="sxs-lookup"><span data-stu-id="e28db-351"><a id="play"></a>Play your content</span></span>
<span data-ttu-id="e28db-352">Pokud chcete video streamovat, použijte [přehrávač služby Azure Media Services](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="e28db-352">To stream you video, use [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

<span data-ttu-id="e28db-353">Chcete-li otestovat progresivní stahování, vložte adresu URL do prohlížeče (například aplikace Internet Explorer, Chrome, Safari).</span><span class="sxs-lookup"><span data-stu-id="e28db-353">To test progressive download, paste a URL into a browser (for example, IE, Chrome, Safari).</span></span>

## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="e28db-354">Další kroky: Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="e28db-354">Next Steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e28db-355">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="e28db-355">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
