---
title: "Šifrování svůj obsah pomocí šifrování úložiště pomocí rozhraní REST API pro AMS"
description: "Zjistěte, jak k zašifrování obsahu pomocí šifrování úložiště pomocí rozhraní REST API pro AMS."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 1979f5bf5e8cab88dab5fba49018afacf24504b3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="encrypting-your-content-with-storage-encryption"></a><span data-ttu-id="60bc4-103">Šifrování svůj obsah pomocí šifrování úložiště</span><span class="sxs-lookup"><span data-stu-id="60bc4-103">Encrypting your content with storage encryption</span></span>

<span data-ttu-id="60bc4-104">Důrazně doporučujeme k šifrování vašeho obsahu místně pomocí 256bitového šifrování AES 256 a nahrajte ho do Azure Storage kde bude uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="60bc4-104">It is highly recommended to encrypt your content locally using AES-256 bit encryption and then upload it to Azure Storage where it will be stored encrypted at rest.</span></span>

<span data-ttu-id="60bc4-105">Tento článek přináší přehled o šifrování úložiště AMS a ukazuje, jak nahrát obsah šifrování úložiště:</span><span class="sxs-lookup"><span data-stu-id="60bc4-105">This article gives an overview of AMS storage encryption and shows you how to upload the storage encrypted content:</span></span>

* <span data-ttu-id="60bc4-106">Vytvořte klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-106">Create a content key.</span></span>
* <span data-ttu-id="60bc4-107">Vytvořte Asset.</span><span class="sxs-lookup"><span data-stu-id="60bc4-107">Create an Asset.</span></span> <span data-ttu-id="60bc4-108">Nastavte AssetCreationOption StorageEncryption při vytváření prostředku.</span><span class="sxs-lookup"><span data-stu-id="60bc4-108">Set the AssetCreationOption to StorageEncryption when creating the Asset.</span></span>
  
     <span data-ttu-id="60bc4-109">Šifrované prostředky musí být přidružen klíčů k obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-109">Encrypted assets have to be associated with content keys.</span></span>
* <span data-ttu-id="60bc4-110">Klíč k obsahu na odkaz pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="60bc4-110">Link the content key to the asset.</span></span>  
* <span data-ttu-id="60bc4-111">Nastavení šifrování souvisejících parametrů na AssetFile entity.</span><span class="sxs-lookup"><span data-stu-id="60bc4-111">Set the encryption related parameters on the AssetFile entities.</span></span>

## <a name="considerations"></a><span data-ttu-id="60bc4-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="60bc4-112">Considerations</span></span> 

<span data-ttu-id="60bc4-113">Pokud chcete doručovat šifrované asset úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-113">If you want to deliver a storage encrypted asset, you must configure the asset’s delivery policy.</span></span> <span data-ttu-id="60bc4-114">Před asset Streamovat, server datových proudů odebere šifrování úložiště a datové proudy svůj obsah pomocí zadaného doručování zásad.</span><span class="sxs-lookup"><span data-stu-id="60bc4-114">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy.</span></span> <span data-ttu-id="60bc4-115">Další informace najdete v tématu [konfigurace zásad doručení Assetu](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="60bc4-115">For more information, see [Configuring Asset Delivery Policies](media-services-rest-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="60bc4-116">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="60bc4-116">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="60bc4-117">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="60bc4-117">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span> 

## <a name="connect-to-media-services"></a><span data-ttu-id="60bc4-118">Připojení ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="60bc4-118">Connect to Media Services</span></span>

<span data-ttu-id="60bc4-119">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="60bc4-119">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="60bc4-120">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="60bc4-120">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="60bc4-121">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="60bc4-121">You must make subsequent calls to the new URI.</span></span>

## <a name="storage-encryption-overview"></a><span data-ttu-id="60bc4-122">Šifrování úložiště – přehled</span><span class="sxs-lookup"><span data-stu-id="60bc4-122">Storage encryption overview</span></span>
<span data-ttu-id="60bc4-123">Šifrování úložiště AMS platí **AES PEV.cenu** režim šifrování pro celý soubor.</span><span class="sxs-lookup"><span data-stu-id="60bc4-123">The AMS storage encryption applies **AES-CTR** mode encryption to the entire file.</span></span>  <span data-ttu-id="60bc4-124">Režim PEV.cenu AES je blok šifer, které můžete šifrovat libovolné délce dat bez nutnosti odsazení.</span><span class="sxs-lookup"><span data-stu-id="60bc4-124">AES-CTR mode is a block cipher that can encrypt arbitrary length data without need for padding.</span></span> <span data-ttu-id="60bc4-125">Funguje šifrování čítač blok s AES – algoritmus a XOR končící na-ing výstup AES s daty se zašifrovat nebo dešifrovat.</span><span class="sxs-lookup"><span data-stu-id="60bc4-125">It operates by encrypting a counter block with the AES algorithm and then XOR-ing the output of AES with the data to encrypt or decrypt.</span></span>  <span data-ttu-id="60bc4-126">Čítač bloku používá je vytvořený tak, že zkopírujete hodnotu InitializationVector bajtů 0 až 7 hodnota čítače a bajtů 8 až 15 hodnota čítače je nastaven na hodnotu nula.</span><span class="sxs-lookup"><span data-stu-id="60bc4-126">The counter block used is constructed by copying the value of the InitializationVector to bytes 0 to 7 of the counter value and bytes 8 to 15 of the counter value are set to zero.</span></span> <span data-ttu-id="60bc4-127">Čítač bloku 16 bajtů bajtů (tj. nejméně významný bajtů) 8 až 15 slouží jako celé číslo bez znaménka jednoduché 64bitová verze, která se zvýší, jedna pro každou další blok dat zpracovat a je uložen v síťovém pořadí bajtů.</span><span class="sxs-lookup"><span data-stu-id="60bc4-127">Of the 16 byte counter block, bytes 8 to 15 (i.e. the least significant bytes) are used as a simple 64 bit unsigned integer that is incremented by one for each subsequent block of data processed and is kept in network byte order.</span></span> <span data-ttu-id="60bc4-128">Pamatujte, že pokud toto celé číslo nedosáhne maximální hodnoty (0xFFFFFFFFFFFFFFFF) zvyšování ho resetovat čítač bloku na nula (v bajtech 8 až 15) bez vlivu 64 bitů čítače (tj. bajty 0 až 7).</span><span class="sxs-lookup"><span data-stu-id="60bc4-128">Note that if this integer reaches the maximum value (0xFFFFFFFFFFFFFFFF) then incrementing it resets the block counter to zero (bytes 8 to 15) without affecting the other 64 bits of the counter (i.e. bytes 0 to 7).</span></span>   <span data-ttu-id="60bc4-129">Chcete-li zajistit bezpečnost šifrování AES-PEV.cenu režimu, musí být jedinečný pro každý soubor InitializationVector hodnotu pro daný identifikátor klíče pro každý klíč k obsahu a soubory musí být menší než 2 ^ 64 bloky délku.</span><span class="sxs-lookup"><span data-stu-id="60bc4-129">In order to maintain the security of the AES-CTR mode encryption, the InitializationVector value for a given Key Identifier for each content key shall be unique for each file and files shall be less than 2^64 blocks in length.</span></span>  <span data-ttu-id="60bc4-130">To je potřeba zajistit, že hodnota čítače se nikdy znovu použije k danému klíči.</span><span class="sxs-lookup"><span data-stu-id="60bc4-130">This is to ensure that a counter value is never reused with a given key.</span></span> <span data-ttu-id="60bc4-131">Další informace o režimu PEV.cenu najdete v tématu [této stránce wikiwebu](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (článku na wiki používá termín "Nonce" místo "InitializationVector").</span><span class="sxs-lookup"><span data-stu-id="60bc4-131">For more information about the CTR mode, see [this wiki page](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (the wiki article uses the term "Nonce" instead of "InitializationVector").</span></span>

<span data-ttu-id="60bc4-132">Použití **šifrování úložiště** k zašifrování obsahu místně pomocí standardu AES 256 bitů šifrování a nahrajte ho do Azure Storage kde bude uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="60bc4-132">Use **Storage Encryption** to encrypt your clear content locally using AES-256 bit encryption and then upload it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="60bc4-133">Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístěny do systému souborů EFS před kódování a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="60bc4-133">Assets protected with storage encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="60bc4-134">Případem primárního použití šifrování úložiště je, když chcete zabezpečit vysoké kvality souborů vstupními médii pomocí silného šifrování v klidovém stavu na disku.</span><span class="sxs-lookup"><span data-stu-id="60bc4-134">The primary use case for storage encryption is when you want to secure your high quality input media files with strong encryption at rest on disk.</span></span>

<span data-ttu-id="60bc4-135">Aby bylo možné poskytovat asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu, aby věděl Media Services může způsob doručení obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-135">In order to deliver a storage encrypted asset, you must configure the asset’s delivery policy so Media Services knows how you want to deliver your content.</span></span> <span data-ttu-id="60bc4-136">Před asset Streamovat, server datových proudů odebere šifrování úložiště a datové proudy svůj obsah pomocí zadaného doručování zásad (například AES, běžným šifrováním nebo žádné šifrování).</span><span class="sxs-lookup"><span data-stu-id="60bc4-136">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy (for example, AES, common encryption, or no encryption).</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="60bc4-137">Vytvoření ContentKeys pro šifrování</span><span class="sxs-lookup"><span data-stu-id="60bc4-137">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="60bc4-138">Šifrované prostředky musí být přidružen úložiště šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="60bc4-138">Encrypted assets have to be associated with Storage Encryption key.</span></span> <span data-ttu-id="60bc4-139">Je nutné vytvořit klíč obsahu, který se má použít pro šifrování před vytvořením soubory prostředků.</span><span class="sxs-lookup"><span data-stu-id="60bc4-139">You must create the content key to be used for encryption before creating the asset files.</span></span> <span data-ttu-id="60bc4-140">Tato část popisuje postup vytvoření klíče k obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-140">This section describes how to create a content key.</span></span>

<span data-ttu-id="60bc4-141">Následují obecné kroky pro generování obsahu klíčů, které se spojují s prostředky, které chcete šifrovat.</span><span class="sxs-lookup"><span data-stu-id="60bc4-141">The following are general steps for generating content keys that you will associate with assets that you want to be encrypted.</span></span> 

1. <span data-ttu-id="60bc4-142">Šifrování úložiště náhodně Generovat klíč standardu AES 32 bajtů.</span><span class="sxs-lookup"><span data-stu-id="60bc4-142">For storage encryption, randomly generate a 32-byte AES key.</span></span> 
   
    <span data-ttu-id="60bc4-143">Bude jím klíč k obsahu pro váš asset, což znamená, že všechny soubory přidružené k této asset bude nutné použít stejný klíč k obsahu během dešifrování.</span><span class="sxs-lookup"><span data-stu-id="60bc4-143">This will be the content key for your asset, which means all files associated with that asset will need to use the same content key during decryption.</span></span> 
2. <span data-ttu-id="60bc4-144">Volání [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) a [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) metod k získání správného certifikátu X.509, který použije k zašifrování obsahu klíče.</span><span class="sxs-lookup"><span data-stu-id="60bc4-144">Call the [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) and [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methods to get the correct X.509 Certificate that must be used to encrypt your content key.</span></span>
3. <span data-ttu-id="60bc4-145">Zašifrování obsahu klíče pomocí veřejného klíče certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="60bc4-145">Encrypt your content key with the public key of the X.509 Certificate.</span></span> 
   
   <span data-ttu-id="60bc4-146">Media Services .NET SDK používá RSA s OAEP při provádění šifrování.</span><span class="sxs-lookup"><span data-stu-id="60bc4-146">Media Services .NET SDK uses RSA with OAEP when doing the encryption.</span></span>  <span data-ttu-id="60bc4-147">Vidíte příklad rozhraní .NET v [EncryptSymmetricKeyData funkce](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="60bc4-147">You can see a .NET example in the [EncryptSymmetricKeyData function](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
4. <span data-ttu-id="60bc4-148">Vytvoří hodnotu kontrolního součtu vypočítává pomocí identifikátoru klíče a klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-148">Create a checksum value calculated using the key identifier and content key.</span></span> <span data-ttu-id="60bc4-149">Následující příklad .NET vypočítá kontrolního součtu pomocí identifikátoru GUID části identifikátoru klíče a vymazat obsah klíče.</span><span class="sxs-lookup"><span data-stu-id="60bc4-149">The following .NET example calculates the checksum using the GUID part of the key identifier and the clear content key.</span></span>

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }

1. <span data-ttu-id="60bc4-150">Vytvořte klíč obsahu se **EncryptedContentKey** (převést na řetězec s kódováním base64), **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, a **kontrolního součtu** hodnoty, které jste dostali v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="60bc4-150">Create the Content key with the **EncryptedContentKey** (converted to base64-encoded string), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**, and **Checksum** values you have received in previous steps.</span></span>

    <span data-ttu-id="60bc4-151">Šifrování úložiště by měla zahrnovat následující vlastnosti v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="60bc4-151">For storage encryption, the following properties should be included in the request body.</span></span>

    <span data-ttu-id="60bc4-152">Vlastnost text žádosti</span><span class="sxs-lookup"><span data-stu-id="60bc4-152">Request body property</span></span>    | <span data-ttu-id="60bc4-153">Popis</span><span class="sxs-lookup"><span data-stu-id="60bc4-153">Description</span></span>
    ---|---
    <span data-ttu-id="60bc4-154">ID</span><span class="sxs-lookup"><span data-stu-id="60bc4-154">Id</span></span> | <span data-ttu-id="60bc4-155">ContentKey Id, které jsme si generovat v následujícím formátu "nb:kid:UUID:<NEW GUID>".</span><span class="sxs-lookup"><span data-stu-id="60bc4-155">The ContentKey Id which we generate ourselves using the following format, “nb:kid:UUID:<NEW GUID>”.</span></span>
    <span data-ttu-id="60bc4-156">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="60bc4-156">ContentKeyType</span></span> | <span data-ttu-id="60bc4-157">Toto je typ obsahu klíče jako celé číslo pro tento klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-157">This is the content key type as an integer for this content key.</span></span> <span data-ttu-id="60bc4-158">Jsme předejte hodnotu 1 pro šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="60bc4-158">We pass the value 1 for storage encryption.</span></span>
    <span data-ttu-id="60bc4-159">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="60bc4-159">EncryptedContentKey</span></span> | <span data-ttu-id="60bc4-160">Vytvoříme novou hodnotu obsahu klíče, což je hodnota 256 bitů (32 bajtů).</span><span class="sxs-lookup"><span data-stu-id="60bc4-160">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="60bc4-161">Že je klíč zašifrovaný pomocí certifikátu X.509 šifrování úložiště, který načteme ze služby Microsoft Azure Media Services spuštěním požadavek HTTP GET pro GetProtectionKeyId a GetProtectionKey metody.</span><span class="sxs-lookup"><span data-stu-id="60bc4-161">The key is encrypted using the storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for the GetProtectionKeyId and GetProtectionKey Methods.</span></span> <span data-ttu-id="60bc4-162">Jako příklad, viz následující kód .NET: **EncryptSymmetricKeyData** metoda definované [zde](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="60bc4-162">As an example, see the following .NET code: the  **EncryptSymmetricKeyData** method defined [here](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
    <span data-ttu-id="60bc4-163">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="60bc4-163">ProtectionKeyId</span></span> | <span data-ttu-id="60bc4-164">Jedná se o ochranu id klíče pro certifikát X.509 šifrování úložiště, který slouží k šifrování naše klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-164">This is the protection key id for the storage encryption X.509 certificate that was used to encrypt our content key.</span></span>
    <span data-ttu-id="60bc4-165">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="60bc4-165">ProtectionKeyType</span></span> | <span data-ttu-id="60bc4-166">Jedná se o typ šifrování pro ochranu klíč, který slouží k šifrování klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-166">This is the encryption type for the protection key that was used to encrypt the content key.</span></span> <span data-ttu-id="60bc4-167">Tato hodnota je StorageEncryption(1) pro náš příklad.</span><span class="sxs-lookup"><span data-stu-id="60bc4-167">This value is StorageEncryption(1) for our example.</span></span>
    <span data-ttu-id="60bc4-168">Kontrolní součet</span><span class="sxs-lookup"><span data-stu-id="60bc4-168">Checksum</span></span> |<span data-ttu-id="60bc4-169">Algoritmus MD5 počítané kontrolního součtu pro klíč k obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-169">The MD5 calculated checksum for the content key.</span></span> <span data-ttu-id="60bc4-170">Výpočet je šifrování obsahu Id obsahu klíčem.</span><span class="sxs-lookup"><span data-stu-id="60bc4-170">It is computed by encrypting the content Id with the content key.</span></span> <span data-ttu-id="60bc4-171">Příklad kódu ukazuje, jak k výpočtu kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-171">The example code demonstrates how to calculate the checksum.</span></span>


### <a name="retrieve-the-protectionkeyid"></a><span data-ttu-id="60bc4-172">Načtení ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="60bc4-172">Retrieve the ProtectionKeyId</span></span>
<span data-ttu-id="60bc4-173">Následující příklad ukazuje, jak načíst ProtectionKeyId, kryptografický otisk certifikátu pro certifikát, který je nutné použít při šifrování klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="60bc4-173">The following example shows how to retrieve the ProtectionKeyId, a certificate thumbprint, for the certificate you must use when encrypting your content key.</span></span> <span data-ttu-id="60bc4-174">Proveďte tento krok, abyste měli jistotu, že už máte příslušný certifikát na počítači.</span><span class="sxs-lookup"><span data-stu-id="60bc4-174">Do this step to make sure that you already have the appropriate certificate on your machine.</span></span>

<span data-ttu-id="60bc4-175">Žádost:</span><span class="sxs-lookup"><span data-stu-id="60bc4-175">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="60bc4-176">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="60bc4-176">Response:</span></span>

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

### <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a><span data-ttu-id="60bc4-177">Získání ProtectionKey ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="60bc4-177">Retrieve the ProtectionKey for the ProtectionKeyId</span></span>
<span data-ttu-id="60bc4-178">Následující příklad ukazuje, jak načíst pomocí ProtectionKeyId certifikátu X.509, který že jste dostali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="60bc4-178">The following example shows how to retrieve the X.509 certificate using the ProtectionKeyId you received in the previous step.</span></span>

<span data-ttu-id="60bc4-179">Žádost:</span><span class="sxs-lookup"><span data-stu-id="60bc4-179">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

<span data-ttu-id="60bc4-180">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="60bc4-180">Response:</span></span>

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

### <a name="create-the-content-key"></a><span data-ttu-id="60bc4-181">Vytvořte klíč obsahu</span><span class="sxs-lookup"><span data-stu-id="60bc4-181">Create the content key</span></span>
<span data-ttu-id="60bc4-182">Po načíst certifikát X.509 a používá svůj veřejný klíč k šifrování vašeho obsahu klíče, vytvoření **ContentKey** entity a sady jeho vlastnost hodnoty odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="60bc4-182">After you have retrieved the X.509 certificate and used its public key to encrypt your content key, create a **ContentKey** entity and set its property values accordingly.</span></span>

<span data-ttu-id="60bc4-183">Jedna z hodnot musí nastavit při vytváření obsahu je typ klíče.</span><span class="sxs-lookup"><span data-stu-id="60bc4-183">One of the values that you must set when create the content key is the type.</span></span> <span data-ttu-id="60bc4-184">V případě šifrování úložiště hodnota je '1'.</span><span class="sxs-lookup"><span data-stu-id="60bc4-184">In case of the storage encryption, the value is '1'.</span></span> 

<span data-ttu-id="60bc4-185">Následující příklad ukazuje, jak vytvořit **ContentKey** s **ContentKeyType** nastavit šifrování úložiště ("1") a **ProtectionKeyType** nastaven na hodnotu "0" k označení, že klíč ochrany Id je kryptografický otisk certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="60bc4-185">The following example shows how to create a **ContentKey** with a **ContentKeyType** set for storage encryption ("1") and the **ProtectionKeyType** set to "0" to indicate that the protection key Id is the X.509 certificate thumbprint.</span></span>  

<span data-ttu-id="60bc4-186">Žádost</span><span class="sxs-lookup"><span data-stu-id="60bc4-186">Request</span></span>

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

<span data-ttu-id="60bc4-187">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="60bc4-187">Response:</span></span>

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

## <a name="create-an-asset"></a><span data-ttu-id="60bc4-188">Vytvořit prostředek</span><span class="sxs-lookup"><span data-stu-id="60bc4-188">Create an asset</span></span>
<span data-ttu-id="60bc4-189">Následující příklad ukazuje, jak vytvořit prostředek.</span><span class="sxs-lookup"><span data-stu-id="60bc4-189">The following example shows how to create an asset.</span></span>

<span data-ttu-id="60bc4-190">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="60bc4-190">**HTTP Request**</span></span>

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

<span data-ttu-id="60bc4-191">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="60bc4-191">**HTTP Response**</span></span>

<span data-ttu-id="60bc4-192">V případě úspěchu se vrátí následující:</span><span class="sxs-lookup"><span data-stu-id="60bc4-192">If successful, the following is returned:</span></span>

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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

## <a name="associate-the-contentkey-with-an-asset"></a><span data-ttu-id="60bc4-193">ContentKey přidružit prostředek</span><span class="sxs-lookup"><span data-stu-id="60bc4-193">Associate the ContentKey with an Asset</span></span>
<span data-ttu-id="60bc4-194">Po vytvoření ContentKey, přidružte ho Asset pomocí operace $links, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="60bc4-194">After creating the ContentKey, associate it with your Asset using the $links operation, as shown in the following example:</span></span>

<span data-ttu-id="60bc4-195">Žádost:</span><span class="sxs-lookup"><span data-stu-id="60bc4-195">Request:</span></span>

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

<span data-ttu-id="60bc4-196">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="60bc4-196">Response:</span></span>

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a><span data-ttu-id="60bc4-197">Vytvoření AssetFile</span><span class="sxs-lookup"><span data-stu-id="60bc4-197">Create an AssetFile</span></span>
<span data-ttu-id="60bc4-198">[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity představuje soubor video nebo zvuk, který je uložený v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="60bc4-198">The [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="60bc4-199">Soubor asset je vždy přidružena k assetu a prostředek může obsahovat mnoho soubory asset.</span><span class="sxs-lookup"><span data-stu-id="60bc4-199">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="60bc4-200">Media Services Encoder úloh selže, pokud objekt souboru asset není spojen s digitálnímu souboru v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="60bc4-200">The Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="60bc4-201">Všimněte si, že **AssetFile** instance a samotný mediální soubor jsou dva odlišné objekty.</span><span class="sxs-lookup"><span data-stu-id="60bc4-201">Note that the **AssetFile** instance and the actual media file are two distinct objects.</span></span> <span data-ttu-id="60bc4-202">AssetFile instance obsahuje metadata o souboru média, zatímco souboru média obsahuje samotný mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="60bc4-202">The AssetFile instance contains metadata about the media file, while the media file contains the actual media content.</span></span>

<span data-ttu-id="60bc4-203">Po odeslání souboru digitálního média do kontejneru objektů blob, kterou použijete **SLOUČENÍ** HTTP žádost o aktualizaci AssetFile s informacemi o souboru média (v tomto tématu není znázorněné).</span><span class="sxs-lookup"><span data-stu-id="60bc4-203">After you upload your digital media file into a blob container, you will use the **MERGE** HTTP request to update the AssetFile with information about your media file (not shown in this topic).</span></span> 

<span data-ttu-id="60bc4-204">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="60bc4-204">**HTTP Request**</span></span>

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

<span data-ttu-id="60bc4-205">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="60bc4-205">**HTTP Response**</span></span>

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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
