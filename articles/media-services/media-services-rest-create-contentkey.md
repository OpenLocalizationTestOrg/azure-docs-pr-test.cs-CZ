---
title: "Vytváření obsahu klíčů se zbytkem | Microsoft Docs"
description: "Informace o vytváření obsahu klíče, které zajišťují zabezpečený přístup k prostředkům."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 95e9322b-168e-4a9d-8d5d-d7c946103745
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: ece09277d26fafb7c0eebf62730031c4dc01bfe0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-content-keys-with-rest"></a><span data-ttu-id="5ee9e-103">Vytváření obsahu klíčů se zbytkem</span><span class="sxs-lookup"><span data-stu-id="5ee9e-103">Create content keys with REST</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5ee9e-104">REST</span><span class="sxs-lookup"><span data-stu-id="5ee9e-104">REST</span></span>](media-services-rest-create-contentkey.md)
> * [<span data-ttu-id="5ee9e-105">.NET</span><span class="sxs-lookup"><span data-stu-id="5ee9e-105">.NET</span></span>](media-services-dotnet-create-contentkey.md)
> 
> 

<span data-ttu-id="5ee9e-106">Služba Media Services umožňuje vytvořit nový a doručovat šifrované prostředky.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-106">Media Services enables you to create new and deliver encrypted assets.</span></span> <span data-ttu-id="5ee9e-107">A **ContentKey** zajišťuje zabezpečený přístup k vaší **Asset**s.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-107">A **ContentKey** provides secure access to your **Asset**s.</span></span> 

<span data-ttu-id="5ee9e-108">Při vytváření nového prostředku (například před [nahrání souborů](media-services-rest-upload-files.md)), můžete zadat následující možnosti šifrování: **StorageEncrypted**, **CommonEncryptionProtected**, nebo **EnvelopeEncryptionProtected**.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-108">When you create a new asset (for example, before you [upload files](media-services-rest-upload-files.md)), you can specify the following encryption options: **StorageEncrypted**, **CommonEncryptionProtected**, or **EnvelopeEncryptionProtected**.</span></span> 

<span data-ttu-id="5ee9e-109">Při předvádění prostředky pro klienty, můžete [konfigurace pro prostředky dynamicky šifrovat](media-services-rest-configure-asset-delivery-policy.md) s jedním z následujících dvou šifrování: **DynamicEnvelopeEncryption** nebo  **DynamicCommonEncryption**.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-109">When you deliver assets to your clients, you can [configure for assets to be dynamically encrypted](media-services-rest-configure-asset-delivery-policy.md) with one of the following two encryptions: **DynamicEnvelopeEncryption** or **DynamicCommonEncryption**.</span></span>

<span data-ttu-id="5ee9e-110">Šifrované prostředky musí být přidružený **ContentKey**s.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-110">Encrypted assets have to be associated with **ContentKey**s.</span></span> <span data-ttu-id="5ee9e-111">Tento článek popisuje postup vytvoření klíče k obsahu.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-111">This article describes how to create a content key.</span></span>

<span data-ttu-id="5ee9e-112">Následují obecné kroky pro generování obsahu klíčů, které se spojují s prostředky, které chcete šifrovat.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-112">The following are general steps for generating content keys that you will associate with assets that you want to be encrypted.</span></span> 

1. <span data-ttu-id="5ee9e-113">Náhodně generovat klíče AES 16 bajtů (pro běžné a obálky šifrování) nebo klíče AES 32 bajtů (pro šifrování úložiště).</span><span class="sxs-lookup"><span data-stu-id="5ee9e-113">Randomly generate a 16-byte AES key (for common and envelope encryption) or a 32-byte AES key (for storage encryption).</span></span> 
   
    <span data-ttu-id="5ee9e-114">Bude jím klíč k obsahu pro váš asset, což znamená, že všechny soubory přidružené k této asset bude nutné použít stejný klíč k obsahu během dešifrování.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-114">This will be the content key for your asset, which means all files associated with that asset will need to use the same content key during decryption.</span></span> 
2. <span data-ttu-id="5ee9e-115">Volání [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) a [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) metod k získání správného certifikátu X.509, který použije k zašifrování obsahu klíče.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-115">Call the [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) and [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methods to get the correct X.509 Certificate that must be used to encrypt your content key.</span></span>
3. <span data-ttu-id="5ee9e-116">Zašifrování obsahu klíče pomocí veřejného klíče certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-116">Encrypt your content key with the public key of the X.509 Certificate.</span></span> 
   
   <span data-ttu-id="5ee9e-117">Media Services .NET SDK používá RSA s OAEP při provádění šifrování.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-117">Media Services .NET SDK uses RSA with OAEP when doing the encryption.</span></span>  <span data-ttu-id="5ee9e-118">Můžete zobrazit příklad v [EncryptSymmetricKeyData funkce](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="5ee9e-118">You can see an example in the [EncryptSymmetricKeyData function](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
4. <span data-ttu-id="5ee9e-119">Vytvoří hodnotu kontrolního součtu (založený na algoritmu PlayReady AES klíče kontrolního součtu) pomocí identifikátoru klíče a klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-119">Create a checksum value (based on the PlayReady AES key checksum algorithm) calculated using the key identifier and content key.</span></span> <span data-ttu-id="5ee9e-120">Další informace najdete v části "PlayReady AES klíč kontrolního součtu algoritmus" dokumentu PlayReady záhlaví objekt nachází [zde](http://www.microsoft.com/playready/documents/).</span><span class="sxs-lookup"><span data-stu-id="5ee9e-120">For more information, see the “PlayReady AES Key Checksum Algorithm” section of the PlayReady Header Object document located [here](http://www.microsoft.com/playready/documents/).</span></span>
   
   <span data-ttu-id="5ee9e-121">Následuje příklad rozhraní .NET, která vypočítá kontrolního součtu pomocí identifikátoru GUID části identifikátoru klíče a vymazat obsah klíče.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-121">The following is a .NET example that calculates the checksum using the GUID part of the key identifier and the clear content key.</span></span>

         public static string CalculateChecksum(byte[] contentKey, Guid keyId)
         {

            byte[] array = null;
            using (AesCryptoServiceProvider aesCryptoServiceProvider = new AesCryptoServiceProvider())
            {
                aesCryptoServiceProvider.Mode = CipherMode.ECB;
                aesCryptoServiceProvider.Key = contentKey;
                aesCryptoServiceProvider.Padding = PaddingMode.None;
                ICryptoTransform cryptoTransform = aesCryptoServiceProvider.CreateEncryptor();
                array = new byte[16];
                cryptoTransform.TransformBlock(keyId.ToByteArray(), 0, 16, array, 0);
            }
            byte[] array2 = new byte[8];
            Array.Copy(array, array2, 8);
            return Convert.ToBase64String(array2);
         }
5. <span data-ttu-id="5ee9e-122">Vytvořte klíč obsahu se **EncryptedContentKey** (převést na řetězec s kódováním base64), **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, a **kontrolního součtu** hodnoty, které jste dostali v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-122">Create the Content key with the **EncryptedContentKey** (converted to base64-encoded string), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**, and **Checksum** values you have received in previous steps.</span></span>
6. <span data-ttu-id="5ee9e-123">Přidružení **ContentKey** entita s vaší **Asset** entity prostřednictvím operace $links.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-123">Associate the **ContentKey** entity with your **Asset** entity through the $links operation.</span></span>

<span data-ttu-id="5ee9e-124">Všimněte si, že v tomto tématu není ukazují, jak generovat klíč standardu AES, šifrování klíče a vypočítat kontrolní součet.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-124">Note that this topic does not show how to generate an AES key, encrypt the key, and calculate the checksum.</span></span> 

>[!NOTE]

><span data-ttu-id="5ee9e-125">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-125">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="5ee9e-126">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="5ee9e-126">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="5ee9e-127">Připojení ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="5ee9e-127">Connect to Media Services</span></span>

<span data-ttu-id="5ee9e-128">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="5ee9e-128">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="5ee9e-129">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-129">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="5ee9e-130">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-130">You must make subsequent calls to the new URI.</span></span>

## <a name="retrieve-the-protectionkeyid"></a><span data-ttu-id="5ee9e-131">Načtení ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="5ee9e-131">Retrieve the ProtectionKeyId</span></span>
<span data-ttu-id="5ee9e-132">Následující příklad ukazuje, jak načíst ProtectionKeyId, kryptografický otisk certifikátu pro certifikát, který je nutné použít při šifrování klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-132">The following example shows how to retrieve the ProtectionKeyId, a certificate thumbprint, for the certificate you must use when encrypting your content key.</span></span> <span data-ttu-id="5ee9e-133">Proveďte tento krok, abyste měli jistotu, že už máte příslušný certifikát na počítači.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-133">Do this step to make sure that you already have the appropriate certificate on your machine.</span></span>

<span data-ttu-id="5ee9e-134">Žádost:</span><span class="sxs-lookup"><span data-stu-id="5ee9e-134">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net


<span data-ttu-id="5ee9e-135">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="5ee9e-135">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

## <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a><span data-ttu-id="5ee9e-136">Získání ProtectionKey ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="5ee9e-136">Retrieve the ProtectionKey for the ProtectionKeyId</span></span>
<span data-ttu-id="5ee9e-137">Následující příklad ukazuje, jak načíst pomocí ProtectionKeyId certifikátu X.509, který že jste dostali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-137">The following example shows how to retrieve the X.509 certificate using the ProtectionKeyId you received in the previous step.</span></span>

<span data-ttu-id="5ee9e-138">Žádost:</span><span class="sxs-lookup"><span data-stu-id="5ee9e-138">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net



<span data-ttu-id="5ee9e-139">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="5ee9e-139">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

## <a name="create-the-contentkey"></a><span data-ttu-id="5ee9e-140">Vytvořte ContentKey</span><span class="sxs-lookup"><span data-stu-id="5ee9e-140">Create the ContentKey</span></span>
<span data-ttu-id="5ee9e-141">Po načíst certifikát X.509 a používá svůj veřejný klíč k šifrování vašeho obsahu klíče, vytvoření **ContentKey** entity a sady jeho vlastnost hodnoty odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-141">After you have retrieved the X.509 certificate and used its public key to encrypt your content key, create a **ContentKey** entity and set its property values accordingly.</span></span>

<span data-ttu-id="5ee9e-142">Jedna z hodnot musí nastavit při vytváření obsahu je typ klíče.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-142">One of the values that you must set when create the content key is the type.</span></span> <span data-ttu-id="5ee9e-143">Vyberte jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-143">Choose from one of the following values.</span></span>

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }


<span data-ttu-id="5ee9e-144">Následující příklad ukazuje, jak vytvořit **ContentKey** s **ContentKeyType** nastavit šifrování úložiště ("1") a **ProtectionKeyType** nastaven na hodnotu "0" k označení, že klíč ochrany Id je kryptografický otisk certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="5ee9e-144">The following example shows how to create a **ContentKey** with a **ContentKeyType** set for storage encryption ("1") and the **ProtectionKeyType** set to "0" to indicate that the protection key Id is the X.509 certificate thumbprint.</span></span>  

<span data-ttu-id="5ee9e-145">Žádost</span><span class="sxs-lookup"><span data-stu-id="5ee9e-145">Request</span></span>

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


<span data-ttu-id="5ee9e-146">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="5ee9e-146">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="associate-the-contentkey-with-an-asset"></a><span data-ttu-id="5ee9e-147">ContentKey přidružit prostředek</span><span class="sxs-lookup"><span data-stu-id="5ee9e-147">Associate the ContentKey with an Asset</span></span>
<span data-ttu-id="5ee9e-148">Po vytvoření ContentKey, přidružte ho Asset pomocí operace $links, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="5ee9e-148">After creating the ContentKey, associate it with your Asset using the $links operation, as shown in the following example:</span></span>

<span data-ttu-id="5ee9e-149">Žádost:</span><span class="sxs-lookup"><span data-stu-id="5ee9e-149">Request:</span></span>

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net


    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

<span data-ttu-id="5ee9e-150">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="5ee9e-150">Response:</span></span>

    HTTP/1.1 204 No Content 


## <a name="media-services-learning-paths"></a><span data-ttu-id="5ee9e-151">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="5ee9e-151">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="5ee9e-152">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="5ee9e-152">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

