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
# <a name="create-contentkeys-with-net"></a>Vytvoření ContentKeys s rozhraním .NET
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Služba Media Services umožňuje toocreate a doručovat šifrované prostředky. A **ContentKey** poskytuje zabezpečený přístup tooyour **Asset**s. 

Při vytváření nového prostředku (například před [nahrání souborů](media-services-dotnet-upload-files.md)), můžete zadat následující možnosti šifrování hello: **StorageEncrypted**, **CommonEncryptionProtected**, nebo **EnvelopeEncryptionProtected**. 

Při předvádění prostředky tooyour klientů, můžete [konfigurace pro toobe prostředky dynamicky šifrovat](media-services-dotnet-configure-asset-delivery-policy.md) s jedním z následujících dvou šifrování hello: **DynamicEnvelopeEncryption** nebo  **DynamicCommonEncryption**.

Šifrované prostředky mají toobe přidružené **ContentKey**s. Tento článek popisuje, jak toocreate klíč obsahu.

> [!NOTE]
> Při vytváření nového **StorageEncrypted** pomocí asset hello sady Media Services .NET SDK hello **ContentKey** je automaticky vytvořen a je propojená s hello asset.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
Jedna z hodnot hello musí nastavit při vytváření obsahu klíč je typ obsahu klíče hello. Vyberte jednu z následujících hodnot hello. 

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

## <a id="envelope_contentkey"></a>Vytvoření typu obálky ContentKey
Hello následující fragment kódu vytvoří klíč obsahu hello obálky šifrování typu. Potom přidruží hello klíč hello daný prostředek.

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

Volání

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <a id="common_contentkey"></a>Vytvoření stejného typu ContentKey
Hello následující fragment kódu vytvoří klíč obsahu hello běžné šifrování typu. Potom přidruží hello klíč hello daný prostředek.

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
Volání

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

