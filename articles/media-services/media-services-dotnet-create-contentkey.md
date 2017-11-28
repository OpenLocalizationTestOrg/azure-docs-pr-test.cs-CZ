---
title: "aaaCreate ContentKeys s rozhraním .NET"
description: "Zjistěte, jak toocreate obsahu klíče, které zajišťují zabezpečený přístup k tooAssets."
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
ms.openlocfilehash: 35909c64e8393e228be75c464a034ffc40122952
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-contentkeys-with-net"></a><span data-ttu-id="7bede-103">Vytvoření ContentKeys s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="7bede-103">Create ContentKeys with .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7bede-104">REST</span><span class="sxs-lookup"><span data-stu-id="7bede-104">REST</span></span>](media-services-rest-create-contentkey.md)
> * [<span data-ttu-id="7bede-105">.NET</span><span class="sxs-lookup"><span data-stu-id="7bede-105">.NET</span></span>](media-services-dotnet-create-contentkey.md)
> 
> 

<span data-ttu-id="7bede-106">Služba Media Services umožňuje toocreate a doručovat šifrované prostředky.</span><span class="sxs-lookup"><span data-stu-id="7bede-106">Media Services enables you toocreate and deliver encrypted assets.</span></span> <span data-ttu-id="7bede-107">A **ContentKey** poskytuje zabezpečený přístup tooyour **Asset**s.</span><span class="sxs-lookup"><span data-stu-id="7bede-107">A **ContentKey** provides secure access tooyour **Asset**s.</span></span> 

<span data-ttu-id="7bede-108">Při vytváření nového prostředku (například před [nahrání souborů](media-services-dotnet-upload-files.md)), můžete zadat následující možnosti šifrování hello: **StorageEncrypted**, **CommonEncryptionProtected**, nebo **EnvelopeEncryptionProtected**.</span><span class="sxs-lookup"><span data-stu-id="7bede-108">When you create a new asset (for example, before you [upload files](media-services-dotnet-upload-files.md)), you can specify hello following encryption options: **StorageEncrypted**, **CommonEncryptionProtected**, or **EnvelopeEncryptionProtected**.</span></span> 

<span data-ttu-id="7bede-109">Při předvádění prostředky tooyour klientů, můžete [konfigurace pro toobe prostředky dynamicky šifrovat](media-services-dotnet-configure-asset-delivery-policy.md) s jedním z následujících dvou šifrování hello: **DynamicEnvelopeEncryption** nebo  **DynamicCommonEncryption**.</span><span class="sxs-lookup"><span data-stu-id="7bede-109">When you deliver assets tooyour clients, you can [configure for assets toobe dynamically encrypted](media-services-dotnet-configure-asset-delivery-policy.md) with one of hello following two encryptions: **DynamicEnvelopeEncryption** or **DynamicCommonEncryption**.</span></span>

<span data-ttu-id="7bede-110">Šifrované prostředky mají toobe přidružené **ContentKey**s.</span><span class="sxs-lookup"><span data-stu-id="7bede-110">Encrypted assets have toobe associated with **ContentKey**s.</span></span> <span data-ttu-id="7bede-111">Tento článek popisuje, jak toocreate klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="7bede-111">This article describes how toocreate a content key.</span></span>

> [!NOTE]
> <span data-ttu-id="7bede-112">Při vytváření nového **StorageEncrypted** pomocí asset hello sady Media Services .NET SDK hello **ContentKey** je automaticky vytvořen a je propojená s hello asset.</span><span class="sxs-lookup"><span data-stu-id="7bede-112">When creating a new **StorageEncrypted** asset using hello Media Services .NET SDK , hello **ContentKey** is automatically created and linked with hello asset.</span></span>
> 
> 

## <a name="contentkeytype"></a><span data-ttu-id="7bede-113">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="7bede-113">ContentKeyType</span></span>
<span data-ttu-id="7bede-114">Jedna z hodnot hello musí nastavit při vytváření obsahu klíč je typ obsahu klíče hello.</span><span class="sxs-lookup"><span data-stu-id="7bede-114">One of hello values that you must set when create a content key is hello content key type.</span></span> <span data-ttu-id="7bede-115">Vyberte jednu z následujících hodnot hello.</span><span class="sxs-lookup"><span data-stu-id="7bede-115">Choose from one of hello following values.</span></span> 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is hello default value.</remarks>
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

## <span data-ttu-id="7bede-116"><a id="envelope_contentkey"></a>Vytvoření typu obálky ContentKey</span><span class="sxs-lookup"><span data-stu-id="7bede-116"><a id="envelope_contentkey"></a>Create envelope type ContentKey</span></span>
<span data-ttu-id="7bede-117">Hello následující fragment kódu vytvoří klíč obsahu hello obálky šifrování typu.</span><span class="sxs-lookup"><span data-stu-id="7bede-117">hello following code snippet creates a content key of hello envelope encryption type.</span></span> <span data-ttu-id="7bede-118">Potom přidruží hello klíč hello daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="7bede-118">It then associates hello key with hello specified asset.</span></span>

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

<span data-ttu-id="7bede-119">Volání</span><span class="sxs-lookup"><span data-stu-id="7bede-119">call</span></span>

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <span data-ttu-id="7bede-120"><a id="common_contentkey"></a>Vytvoření stejného typu ContentKey</span><span class="sxs-lookup"><span data-stu-id="7bede-120"><a id="common_contentkey"></a>Create common type ContentKey</span></span>
<span data-ttu-id="7bede-121">Hello následující fragment kódu vytvoří klíč obsahu hello běžné šifrování typu.</span><span class="sxs-lookup"><span data-stu-id="7bede-121">hello following code snippet creates a content key of hello common encryption type.</span></span> <span data-ttu-id="7bede-122">Potom přidruží hello klíč hello daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="7bede-122">It then associates hello key with hello specified asset.</span></span>

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

        // Associate hello key with hello asset.
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
<span data-ttu-id="7bede-123">Volání</span><span class="sxs-lookup"><span data-stu-id="7bede-123">call</span></span>

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a><span data-ttu-id="7bede-124">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="7bede-124">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7bede-125">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="7bede-125">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

