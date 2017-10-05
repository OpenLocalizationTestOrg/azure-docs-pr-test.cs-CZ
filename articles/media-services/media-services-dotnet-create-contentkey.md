---
title: "Vytvoření ContentKeys s rozhraním .NET"
description: "Informace o vytváření obsahu klíče, které zajišťují zabezpečený přístup k prostředkům."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 3280a6fcde59bae360da7cb9fea4bb649f984e43
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-contentkeys-with-net"></a><span data-ttu-id="d1c8e-103">Vytvoření ContentKeys s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="d1c8e-103">Create ContentKeys with .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d1c8e-104">REST</span><span class="sxs-lookup"><span data-stu-id="d1c8e-104">REST</span></span>](media-services-rest-create-contentkey.md)
> * [<span data-ttu-id="d1c8e-105">.NET</span><span class="sxs-lookup"><span data-stu-id="d1c8e-105">.NET</span></span>](media-services-dotnet-create-contentkey.md)
> 
> 

<span data-ttu-id="d1c8e-106">Služba Media Services umožňuje vytvářet a doručovat šifrované prostředky.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-106">Media Services enables you to create and deliver encrypted assets.</span></span> <span data-ttu-id="d1c8e-107">A **ContentKey** zajišťuje zabezpečený přístup k vaší **Asset**s.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-107">A **ContentKey** provides secure access to your **Asset**s.</span></span> 

<span data-ttu-id="d1c8e-108">Při vytváření nového prostředku (například před [nahrání souborů](media-services-dotnet-upload-files.md)), můžete zadat následující možnosti šifrování: **StorageEncrypted**, **CommonEncryptionProtected**, nebo **EnvelopeEncryptionProtected**.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-108">When you create a new asset (for example, before you [upload files](media-services-dotnet-upload-files.md)), you can specify the following encryption options: **StorageEncrypted**, **CommonEncryptionProtected**, or **EnvelopeEncryptionProtected**.</span></span> 

<span data-ttu-id="d1c8e-109">Při předvádění prostředky pro klienty, můžete [konfigurace pro prostředky dynamicky šifrovat](media-services-dotnet-configure-asset-delivery-policy.md) s jedním z následujících dvou šifrování: **DynamicEnvelopeEncryption** nebo  **DynamicCommonEncryption**.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-109">When you deliver assets to your clients, you can [configure for assets to be dynamically encrypted](media-services-dotnet-configure-asset-delivery-policy.md) with one of the following two encryptions: **DynamicEnvelopeEncryption** or **DynamicCommonEncryption**.</span></span>

<span data-ttu-id="d1c8e-110">Šifrované prostředky musí být přidružený **ContentKey**s.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-110">Encrypted assets have to be associated with **ContentKey**s.</span></span> <span data-ttu-id="d1c8e-111">Tento článek popisuje postup vytvoření klíče k obsahu.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-111">This article describes how to create a content key.</span></span>

> [!NOTE]
> <span data-ttu-id="d1c8e-112">Při vytváření nového **StorageEncrypted** asset pomocí sady Media Services .NET SDK **ContentKey** je automaticky vytvořené a připojené k assetu.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-112">When creating a new **StorageEncrypted** asset using the Media Services .NET SDK , the **ContentKey** is automatically created and linked with the asset.</span></span>
> 
> 

## <a name="contentkeytype"></a><span data-ttu-id="d1c8e-113">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="d1c8e-113">ContentKeyType</span></span>
<span data-ttu-id="d1c8e-114">Jedna z hodnot musí nastavit při vytváření obsahu klíč je typ obsahu klíče.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-114">One of the values that you must set when create a content key is the content key type.</span></span> <span data-ttu-id="d1c8e-115">Vyberte jednu z následujících hodnot.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-115">Choose from one of the following values.</span></span> 

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

## <span data-ttu-id="d1c8e-116"><a id="envelope_contentkey"></a>Vytvoření typu obálky ContentKey</span><span class="sxs-lookup"><span data-stu-id="d1c8e-116"><a id="envelope_contentkey"></a>Create envelope type ContentKey</span></span>
<span data-ttu-id="d1c8e-117">Následující fragment kódu vytvoří klíč obsahu typu šifrování obálku.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-117">The following code snippet creates a content key of the envelope encryption type.</span></span> <span data-ttu-id="d1c8e-118">Potom přidruží klíč zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-118">It then associates the key with the specified asset.</span></span>

    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

<span data-ttu-id="d1c8e-119">Volání</span><span class="sxs-lookup"><span data-stu-id="d1c8e-119">call</span></span>

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <span data-ttu-id="d1c8e-120"><a id="common_contentkey"></a>Vytvoření stejného typu ContentKey</span><span class="sxs-lookup"><span data-stu-id="d1c8e-120"><a id="common_contentkey"></a>Create common type ContentKey</span></span>
<span data-ttu-id="d1c8e-121">Následující fragment kódu vytvoří klíč obsahu běžné typu šifrování.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-121">The following code snippet creates a content key of the common encryption type.</span></span> <span data-ttu-id="d1c8e-122">Potom přidruží klíč zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="d1c8e-122">It then associates the key with the specified asset.</span></span>

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
<span data-ttu-id="d1c8e-123">Volání</span><span class="sxs-lookup"><span data-stu-id="d1c8e-123">call</span></span>

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a><span data-ttu-id="d1c8e-124">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="d1c8e-124">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d1c8e-125">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="d1c8e-125">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

