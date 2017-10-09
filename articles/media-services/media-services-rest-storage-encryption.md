---
title: "aaaEncrypting svůj obsah pomocí šifrování úložiště pomocí rozhraní REST API pro AMS"
description: "Zjistěte, jak tooencrypt svůj obsah pomocí šifrování úložiště pomocí rozhraní REST API pro AMS."
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
ms.openlocfilehash: d5f8cb8dd1dcded76c9fededccc772d8102ccbad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encrypting-your-content-with-storage-encryption"></a><span data-ttu-id="5c8b6-103">Šifrování svůj obsah pomocí šifrování úložiště</span><span class="sxs-lookup"><span data-stu-id="5c8b6-103">Encrypting your content with storage encryption</span></span>

<span data-ttu-id="5c8b6-104">Důrazně doporučujeme tooencrypt vašeho obsahu místně pomocí standardu AES 256 bitů šifrování a nahrajte ho tooAzure úložiště, kde bude uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-104">It is highly recommended tooencrypt your content locally using AES-256 bit encryption and then upload it tooAzure Storage where it will be stored encrypted at rest.</span></span>

<span data-ttu-id="5c8b6-105">Tento článek poskytuje přehled o šifrování úložiště AMS a ukazuje, jak úložiště hello tooupload šifrovat obsah:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-105">This article gives an overview of AMS storage encryption and shows you how tooupload hello storage encrypted content:</span></span>

* <span data-ttu-id="5c8b6-106">Vytvořte klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-106">Create a content key.</span></span>
* <span data-ttu-id="5c8b6-107">Vytvořte Asset.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-107">Create an Asset.</span></span> <span data-ttu-id="5c8b6-108">Při vytváření hello Asset nastavte hello AssetCreationOption tooStorageEncryption.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-108">Set hello AssetCreationOption tooStorageEncryption when creating hello Asset.</span></span>
  
     <span data-ttu-id="5c8b6-109">Šifrované prostředky mít toobe spojená s obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-109">Encrypted assets have toobe associated with content keys.</span></span>
* <span data-ttu-id="5c8b6-110">Odkaz hello obsahu klíče toohello asset.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-110">Link hello content key toohello asset.</span></span>  
* <span data-ttu-id="5c8b6-111">Nastavení šifrování hello souvisejících parametrů na hello AssetFile entity.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-111">Set hello encryption related parameters on hello AssetFile entities.</span></span>

## <a name="considerations"></a><span data-ttu-id="5c8b6-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5c8b6-112">Considerations</span></span> 

<span data-ttu-id="5c8b6-113">Pokud chcete toodeliver asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu hello.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-113">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="5c8b6-114">Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-114">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="5c8b6-115">Další informace najdete v tématu [konfigurace zásad doručení Assetu](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-115">For more information, see [Configuring Asset Delivery Policies](media-services-rest-configure-asset-delivery-policy.md).</span></span>

<span data-ttu-id="5c8b6-116">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-116">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="5c8b6-117">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-117">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span> 

## <a name="connect-toomedia-services"></a><span data-ttu-id="5c8b6-118">Připojení služby tooMedia</span><span class="sxs-lookup"><span data-stu-id="5c8b6-118">Connect tooMedia Services</span></span>

<span data-ttu-id="5c8b6-119">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-119">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="5c8b6-120">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-120">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="5c8b6-121">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-121">You must make subsequent calls toohello new URI.</span></span>

## <a name="storage-encryption-overview"></a><span data-ttu-id="5c8b6-122">Šifrování úložiště – přehled</span><span class="sxs-lookup"><span data-stu-id="5c8b6-122">Storage encryption overview</span></span>
<span data-ttu-id="5c8b6-123">šifrování úložiště Hello AMS platí **AES PEV.cenu** režim šifrování toohello celý soubor.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-123">hello AMS storage encryption applies **AES-CTR** mode encryption toohello entire file.</span></span>  <span data-ttu-id="5c8b6-124">Režim PEV.cenu AES je blok šifer, které můžete šifrovat libovolné délce dat bez nutnosti odsazení.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-124">AES-CTR mode is a block cipher that can encrypt arbitrary length data without need for padding.</span></span> <span data-ttu-id="5c8b6-125">Ho funguje tak, že čítač blok s hello AES – algoritmus a potom XOR-ing hello výstup AES s hello data tooencrypt šifrování nebo dešifrování.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-125">It operates by encrypting a counter block with hello AES algorithm and then XOR-ing hello output of AES with hello data tooencrypt or decrypt.</span></span>  <span data-ttu-id="5c8b6-126">blok čítač Hello používá je vytvořený tak, že zkopírujete hello hodnotu hello InitializationVector toobytes 0 too7 hodnoty čítače hello a too15 bajtů 8 hello čítač hodnoty jsou nastaveny toozero.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-126">hello counter block used is constructed by copying hello value of hello InitializationVector toobytes 0 too7 of hello counter value and bytes 8 too15 of hello counter value are set toozero.</span></span> <span data-ttu-id="5c8b6-127">Hello 16 bajtů čítač bloku too15 bajtů 8 (tj. bajty hello nejméně významný) se používají jako celé číslo bez znaménka jednoduché 64bitová verze, která se zvýší, jedna pro každou další blok zpracování dat a je udržována v síťovém pořadí bajtů.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-127">Of hello 16 byte counter block, bytes 8 too15 (i.e. hello least significant bytes) are used as a simple 64 bit unsigned integer that is incremented by one for each subsequent block of data processed and is kept in network byte order.</span></span> <span data-ttu-id="5c8b6-128">Všimněte si, že pokud toto celé číslo dosáhne hello maximální hodnotu (0xFFFFFFFFFFFFFFFF) pak zvyšování ho obnoví hello bloku čítač toozero (too15 bajtů 8) bez ovlivnění hello jiných 64bitová verze čítače hello (tj. too7 0 bajtů).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-128">Note that if this integer reaches hello maximum value (0xFFFFFFFFFFFFFFFF) then incrementing it resets hello block counter toozero (bytes 8 too15) without affecting hello other 64 bits of hello counter (i.e. bytes 0 too7).</span></span>   <span data-ttu-id="5c8b6-129">V rámci pořadí toomaintain hello zabezpečení šifrování AES-PEV.cenu hello režimu hello InitializationVector hodnotu pro daný identifikátor klíče pro každý klíč k obsahu musí být jedinečný pro každý soubor a soubory musí být menší než 2 ^ 64 bloky délku.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-129">In order toomaintain hello security of hello AES-CTR mode encryption, hello InitializationVector value for a given Key Identifier for each content key shall be unique for each file and files shall be less than 2^64 blocks in length.</span></span>  <span data-ttu-id="5c8b6-130">Toto je tooensure, která hodnota čítače se nikdy opětovně použít s daným klíčem.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-130">This is tooensure that a counter value is never reused with a given key.</span></span> <span data-ttu-id="5c8b6-131">Další informace o režimu PEV.cenu hello najdete v tématu [této stránce wikiwebu](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (článek na wikiwebu hello používá hello termín "Nonce" místo "InitializationVector").</span><span class="sxs-lookup"><span data-stu-id="5c8b6-131">For more information about hello CTR mode, see [this wiki page](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (hello wiki article uses hello term "Nonce" instead of "InitializationVector").</span></span>

<span data-ttu-id="5c8b6-132">Použití **šifrování úložiště** tooencrypt obsahu místně pomocí standardu AES 256 bitů šifrování a nahrajte ho tooAzure úložiště, kde je uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-132">Use **Storage Encryption** tooencrypt your clear content locally using AES-256 bit encryption and then upload it tooAzure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="5c8b6-133">Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-133">Assets protected with storage encryption are automatically unencrypted and placed in an encrypted file system prior tooencoding, and optionally re-encrypted prior toouploading back as a new output asset.</span></span> <span data-ttu-id="5c8b6-134">Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure rest souborů vysoké kvality vstupními médii pomocí silného šifrování na disku.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-134">hello primary use case for storage encryption is when you want toosecure your high quality input media files with strong encryption at rest on disk.</span></span>

<span data-ttu-id="5c8b6-135">Toodeliver order asset šifrované úložiště musíte nakonfigurovat zásady doručení assetu hello tak Media Services vědět, jak chcete toodeliver, obsah.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-135">In order toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy so Media Services knows how you want toodeliver your content.</span></span> <span data-ttu-id="5c8b6-136">Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení (například AES, běžným šifrováním nebo žádné šifrování).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-136">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy (for example, AES, common encryption, or no encryption).</span></span>

## <a name="create-contentkeys-used-for-encryption"></a><span data-ttu-id="5c8b6-137">Vytvoření ContentKeys pro šifrování</span><span class="sxs-lookup"><span data-stu-id="5c8b6-137">Create ContentKeys used for encryption</span></span>
<span data-ttu-id="5c8b6-138">Šifrované prostředky mít toobe přidružené úložiště šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-138">Encrypted assets have toobe associated with Storage Encryption key.</span></span> <span data-ttu-id="5c8b6-139">Je nutné vytvořit toobe hello obsahu klíče pro šifrování před vytvořením hello soubory prostředků.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-139">You must create hello content key toobe used for encryption before creating hello asset files.</span></span> <span data-ttu-id="5c8b6-140">Tato část popisuje, jak toocreate klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-140">This section describes how toocreate a content key.</span></span>

<span data-ttu-id="5c8b6-141">Hello následují obecné kroky pro generování obsahu klíčů, které se spojují s prostředky, které chcete toobe zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-141">hello following are general steps for generating content keys that you will associate with assets that you want toobe encrypted.</span></span> 

1. <span data-ttu-id="5c8b6-142">Šifrování úložiště náhodně Generovat klíč standardu AES 32 bajtů.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-142">For storage encryption, randomly generate a 32-byte AES key.</span></span> 
   
    <span data-ttu-id="5c8b6-143">Bude jím hello klíč obsahu pro váš asset, což znamená, všechny soubory, které jsou spojené s tohoto prostředku bude potřebovat toouse hello stejný klíč obsahu během dešifrování.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-143">This will be hello content key for your asset, which means all files associated with that asset will need toouse hello same content key during decryption.</span></span> 
2. <span data-ttu-id="5c8b6-144">Volání hello [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) a [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) metody tooget hello správné certifikátu X.509, který musí být použité tooencrypt klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-144">Call hello [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) and [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methods tooget hello correct X.509 Certificate that must be used tooencrypt your content key.</span></span>
3. <span data-ttu-id="5c8b6-145">Šifrování klíče obsahu hello veřejným klíčem hello certifikát X.509.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-145">Encrypt your content key with hello public key of hello X.509 Certificate.</span></span> 
   
   <span data-ttu-id="5c8b6-146">Media Services .NET SDK používá RSA s OAEP při provádění šifrování hello.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-146">Media Services .NET SDK uses RSA with OAEP when doing hello encryption.</span></span>  <span data-ttu-id="5c8b6-147">Vidíte příklad rozhraní .NET v hello [EncryptSymmetricKeyData funkce](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-147">You can see a .NET example in hello [EncryptSymmetricKeyData function](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
4. <span data-ttu-id="5c8b6-148">Vytvoří hodnotu kontrolního součtu vypočítává pomocí identifikátoru klíče hello a klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-148">Create a checksum value calculated using hello key identifier and content key.</span></span> <span data-ttu-id="5c8b6-149">Hello následující ukázka .NET vypočítá kontrolního součtu hello pomocí hello GUID součástí identifikátoru klíče hello a hello zrušte klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-149">hello following .NET example calculates hello checksum using hello GUID part of hello key identifier and hello clear content key.</span></span>

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting hello KID
            // with hello content key.
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

1. <span data-ttu-id="5c8b6-150">Vytvořte klíč obsahu hello s hello **EncryptedContentKey** (převést řetězec s kódováním toobase64), **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, a **kontrolního součtu** hodnoty, které jste dostali v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-150">Create hello Content key with hello **EncryptedContentKey** (converted toobase64-encoded string), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**, and **Checksum** values you have received in previous steps.</span></span>

    <span data-ttu-id="5c8b6-151">Šifrování úložiště hello zahrnovat následující vlastnosti by v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-151">For storage encryption, hello following properties should be included in hello request body.</span></span>

    <span data-ttu-id="5c8b6-152">Vlastnost text žádosti</span><span class="sxs-lookup"><span data-stu-id="5c8b6-152">Request body property</span></span>    | <span data-ttu-id="5c8b6-153">Popis</span><span class="sxs-lookup"><span data-stu-id="5c8b6-153">Description</span></span>
    ---|---
    <span data-ttu-id="5c8b6-154">ID</span><span class="sxs-lookup"><span data-stu-id="5c8b6-154">Id</span></span> | <span data-ttu-id="5c8b6-155">Hello ContentKey Id, které se vygeneruje označována pomocí hello následující formátu, "nb:kid:UUID:<NEW GUID>".</span><span class="sxs-lookup"><span data-stu-id="5c8b6-155">hello ContentKey Id which we generate ourselves using hello following format, “nb:kid:UUID:<NEW GUID>”.</span></span>
    <span data-ttu-id="5c8b6-156">ContentKeyType</span><span class="sxs-lookup"><span data-stu-id="5c8b6-156">ContentKeyType</span></span> | <span data-ttu-id="5c8b6-157">Toto je typ obsahu klíče hello jako celé číslo pro tento klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-157">This is hello content key type as an integer for this content key.</span></span> <span data-ttu-id="5c8b6-158">Jsme předejte hello hodnotu 1 pro šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-158">We pass hello value 1 for storage encryption.</span></span>
    <span data-ttu-id="5c8b6-159">EncryptedContentKey</span><span class="sxs-lookup"><span data-stu-id="5c8b6-159">EncryptedContentKey</span></span> | <span data-ttu-id="5c8b6-160">Vytvoříme novou hodnotu obsahu klíče, což je hodnota 256 bitů (32 bajtů).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-160">We create a new content key value which is a 256-bit (32 byte) value.</span></span> <span data-ttu-id="5c8b6-161">Hello je klíč zašifrovaný pomocí hello úložiště šifrování X.509 certifikátu, který načteme ze služby Microsoft Azure Media Services spuštěním požadavek HTTP GET pro hello GetProtectionKeyId a GetProtectionKey metody.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-161">hello key is encrypted using hello storage encryption X.509 certificate which we retrieve from Microsoft Azure Media Services by executing a HTTP GET request for hello GetProtectionKeyId and GetProtectionKey Methods.</span></span> <span data-ttu-id="5c8b6-162">Jako příklad najdete v části hello následující kód .NET: hello **EncryptSymmetricKeyData** metoda definované [zde](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-162">As an example, see hello following .NET code: hello  **EncryptSymmetricKeyData** method defined [here](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).</span></span>
    <span data-ttu-id="5c8b6-163">ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="5c8b6-163">ProtectionKeyId</span></span> | <span data-ttu-id="5c8b6-164">To je hello ochrany id klíče pro hello úložiště šifrovací X.509 certifikát, který byl použité tooencrypt naše klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-164">This is hello protection key id for hello storage encryption X.509 certificate that was used tooencrypt our content key.</span></span>
    <span data-ttu-id="5c8b6-165">ProtectionKeyType</span><span class="sxs-lookup"><span data-stu-id="5c8b6-165">ProtectionKeyType</span></span> | <span data-ttu-id="5c8b6-166">Toto je typ šifrování hello hello ochrany klíče, který byl klíč obsahu použité tooencrypt hello.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-166">This is hello encryption type for hello protection key that was used tooencrypt hello content key.</span></span> <span data-ttu-id="5c8b6-167">Tato hodnota je StorageEncryption(1) pro náš příklad.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-167">This value is StorageEncryption(1) for our example.</span></span>
    <span data-ttu-id="5c8b6-168">Kontrolní součet</span><span class="sxs-lookup"><span data-stu-id="5c8b6-168">Checksum</span></span> |<span data-ttu-id="5c8b6-169">Hello MD5 počítané kontrolního součtu pro klíč obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-169">hello MD5 calculated checksum for hello content key.</span></span> <span data-ttu-id="5c8b6-170">Výpočet je šifrování obsahu hello Id obsahu klíčem hello.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-170">It is computed by encrypting hello content Id with hello content key.</span></span> <span data-ttu-id="5c8b6-171">Hello příklad kódu ukazuje, jak toocalculate hello kontrolního součtu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-171">hello example code demonstrates how toocalculate hello checksum.</span></span>


### <a name="retrieve-hello-protectionkeyid"></a><span data-ttu-id="5c8b6-172">Načtení hello ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="5c8b6-172">Retrieve hello ProtectionKeyId</span></span>
<span data-ttu-id="5c8b6-173">Hello následující příklad ukazuje, jak tooretrieve hello ProtectionKeyId, kryptografický otisk certifikátu pro certifikát hello, které musí použít při šifrování klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-173">hello following example shows how tooretrieve hello ProtectionKeyId, a certificate thumbprint, for hello certificate you must use when encrypting your content key.</span></span> <span data-ttu-id="5c8b6-174">Proveďte tento krok toomake jistotu, že už máte příslušný certifikát hello na počítači.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-174">Do this step toomake sure that you already have hello appropriate certificate on your machine.</span></span>

<span data-ttu-id="5c8b6-175">Žádost:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-175">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="5c8b6-176">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-176">Response:</span></span>

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

### <a name="retrieve-hello-protectionkey-for-hello-protectionkeyid"></a><span data-ttu-id="5c8b6-177">Načtení hello ProtectionKey pro hello ProtectionKeyId</span><span class="sxs-lookup"><span data-stu-id="5c8b6-177">Retrieve hello ProtectionKey for hello ProtectionKeyId</span></span>
<span data-ttu-id="5c8b6-178">Hello následující příklad ukazuje, jak certifikát X.509 hello tooretrieve pomocí hello ProtectionKeyId jste obdrželi v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-178">hello following example shows how tooretrieve hello X.509 certificate using hello ProtectionKeyId you received in hello previous step.</span></span>

<span data-ttu-id="5c8b6-179">Žádost:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-179">Request:</span></span>

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

<span data-ttu-id="5c8b6-180">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-180">Response:</span></span>

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

### <a name="create-hello-content-key"></a><span data-ttu-id="5c8b6-181">Vytvořte klíč obsahu hello</span><span class="sxs-lookup"><span data-stu-id="5c8b6-181">Create hello content key</span></span>
<span data-ttu-id="5c8b6-182">Po načíst certifikát X.509 hello a používá svůj veřejný klíč tooencrypt vašeho obsahu klíče, vytvoření **ContentKey** entity a sady jeho vlastnost hodnoty odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-182">After you have retrieved hello X.509 certificate and used its public key tooencrypt your content key, create a **ContentKey** entity and set its property values accordingly.</span></span>

<span data-ttu-id="5c8b6-183">Jedna z hodnot hello musí nastavit při vytváření hello obsahu, že je klíč hello typu.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-183">One of hello values that you must set when create hello content key is hello type.</span></span> <span data-ttu-id="5c8b6-184">V případě hello šifrování úložiště hello hodnota je '1'.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-184">In case of hello storage encryption, hello value is '1'.</span></span> 

<span data-ttu-id="5c8b6-185">Následující příklad ukazuje, jak Hello toocreate **ContentKey** s **ContentKeyType** nastavení pro šifrování úložiště ("1") a hello **ProtectionKeyType** nastavit příliš "0" tooindicate, který hello ochrany klíče Id je kryptografický otisk certifikátu X.509 hello.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-185">hello following example shows how toocreate a **ContentKey** with a **ContentKeyType** set for storage encryption ("1") and hello **ProtectionKeyType** set too"0" tooindicate that hello protection key Id is hello X.509 certificate thumbprint.</span></span>  

<span data-ttu-id="5c8b6-186">Žádost</span><span class="sxs-lookup"><span data-stu-id="5c8b6-186">Request</span></span>

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

<span data-ttu-id="5c8b6-187">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-187">Response:</span></span>

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

## <a name="create-an-asset"></a><span data-ttu-id="5c8b6-188">Vytvořit prostředek</span><span class="sxs-lookup"><span data-stu-id="5c8b6-188">Create an asset</span></span>
<span data-ttu-id="5c8b6-189">Následující příklad ukazuje, jak Hello toocreate prostředek.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-189">hello following example shows how toocreate an asset.</span></span>

<span data-ttu-id="5c8b6-190">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="5c8b6-190">**HTTP Request**</span></span>

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

<span data-ttu-id="5c8b6-191">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="5c8b6-191">**HTTP Response**</span></span>

<span data-ttu-id="5c8b6-192">V případě úspěchu se vrátí hello následující:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-192">If successful, hello following is returned:</span></span>

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

## <a name="associate-hello-contentkey-with-an-asset"></a><span data-ttu-id="5c8b6-193">Přidružit hello ContentKey prostředek</span><span class="sxs-lookup"><span data-stu-id="5c8b6-193">Associate hello ContentKey with an Asset</span></span>
<span data-ttu-id="5c8b6-194">Po vytvoření hello ContentKey, přidružte ho Asset pomocí operace hello $links, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-194">After creating hello ContentKey, associate it with your Asset using hello $links operation, as shown in hello following example:</span></span>

<span data-ttu-id="5c8b6-195">Žádost:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-195">Request:</span></span>

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

<span data-ttu-id="5c8b6-196">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="5c8b6-196">Response:</span></span>

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a><span data-ttu-id="5c8b6-197">Vytvoření AssetFile</span><span class="sxs-lookup"><span data-stu-id="5c8b6-197">Create an AssetFile</span></span>
<span data-ttu-id="5c8b6-198">Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity představuje soubor video nebo zvuk, který je uložený v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-198">hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity represents a video or audio file that is stored in a blob container.</span></span> <span data-ttu-id="5c8b6-199">Soubor asset je vždy přidružena k assetu a prostředek může obsahovat mnoho soubory asset.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-199">An asset file is always associated with an asset, and an asset may contain one or many asset files.</span></span> <span data-ttu-id="5c8b6-200">Hello Media Services Encoder úloh selže, pokud objekt souboru asset není spojen s digitálnímu souboru v kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-200">hello Media Services Encoder task fails if an asset file object is not associated with a digital file in a blob container.</span></span>

<span data-ttu-id="5c8b6-201">Všimněte si, že hello **AssetFile** instance a hello samotný mediální soubor jsou dva odlišné objekty.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-201">Note that hello **AssetFile** instance and hello actual media file are two distinct objects.</span></span> <span data-ttu-id="5c8b6-202">Hello AssetFile instance obsahuje metadata o hello soubor média, zatímco soubor média hello obsahuje hello samotný mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="5c8b6-202">hello AssetFile instance contains metadata about hello media file, while hello media file contains hello actual media content.</span></span>

<span data-ttu-id="5c8b6-203">Po odeslání souboru digitálního média do kontejneru objektů blob, budete používat hello **SLOUČENÍ** HTTP žádost tooupdate hello AssetFile s informacemi o souboru média (v tomto tématu není znázorněné).</span><span class="sxs-lookup"><span data-stu-id="5c8b6-203">After you upload your digital media file into a blob container, you will use hello **MERGE** HTTP request tooupdate hello AssetFile with information about your media file (not shown in this topic).</span></span> 

<span data-ttu-id="5c8b6-204">**Požadavek HTTP**</span><span class="sxs-lookup"><span data-stu-id="5c8b6-204">**HTTP Request**</span></span>

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

<span data-ttu-id="5c8b6-205">**Odpověď HTTP**</span><span class="sxs-lookup"><span data-stu-id="5c8b6-205">**HTTP Response**</span></span>

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
