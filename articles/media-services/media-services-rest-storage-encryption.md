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
# <a name="encrypting-your-content-with-storage-encryption"></a>Šifrování svůj obsah pomocí šifrování úložiště

Důrazně doporučujeme tooencrypt vašeho obsahu místně pomocí standardu AES 256 bitů šifrování a nahrajte ho tooAzure úložiště, kde bude uložený v zašifrované podobě.

Tento článek poskytuje přehled o šifrování úložiště AMS a ukazuje, jak úložiště hello tooupload šifrovat obsah:

* Vytvořte klíč obsahu.
* Vytvořte Asset. Při vytváření hello Asset nastavte hello AssetCreationOption tooStorageEncryption.
  
     Šifrované prostředky mít toobe spojená s obsahu.
* Odkaz hello obsahu klíče toohello asset.  
* Nastavení šifrování hello souvisejících parametrů na hello AssetFile entity.

## <a name="considerations"></a>Požadavky 

Pokud chcete toodeliver asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu hello. Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení. Další informace najdete v tématu [konfigurace zásad doručení Assetu](media-services-rest-configure-asset-delivery-policy.md).

Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP. Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md). 

## <a name="connect-toomedia-services"></a>Připojení služby tooMedia

Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání toohello nový identifikátor URI.

## <a name="storage-encryption-overview"></a>Šifrování úložiště – přehled
šifrování úložiště Hello AMS platí **AES PEV.cenu** režim šifrování toohello celý soubor.  Režim PEV.cenu AES je blok šifer, které můžete šifrovat libovolné délce dat bez nutnosti odsazení. Ho funguje tak, že čítač blok s hello AES – algoritmus a potom XOR-ing hello výstup AES s hello data tooencrypt šifrování nebo dešifrování.  blok čítač Hello používá je vytvořený tak, že zkopírujete hello hodnotu hello InitializationVector toobytes 0 too7 hodnoty čítače hello a too15 bajtů 8 hello čítač hodnoty jsou nastaveny toozero. Hello 16 bajtů čítač bloku too15 bajtů 8 (tj. bajty hello nejméně významný) se používají jako celé číslo bez znaménka jednoduché 64bitová verze, která se zvýší, jedna pro každou další blok zpracování dat a je udržována v síťovém pořadí bajtů. Všimněte si, že pokud toto celé číslo dosáhne hello maximální hodnotu (0xFFFFFFFFFFFFFFFF) pak zvyšování ho obnoví hello bloku čítač toozero (too15 bajtů 8) bez ovlivnění hello jiných 64bitová verze čítače hello (tj. too7 0 bajtů).   V rámci pořadí toomaintain hello zabezpečení šifrování AES-PEV.cenu hello režimu hello InitializationVector hodnotu pro daný identifikátor klíče pro každý klíč k obsahu musí být jedinečný pro každý soubor a soubory musí být menší než 2 ^ 64 bloky délku.  Toto je tooensure, která hodnota čítače se nikdy opětovně použít s daným klíčem. Další informace o režimu PEV.cenu hello najdete v tématu [této stránce wikiwebu](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (článek na wikiwebu hello používá hello termín "Nonce" místo "InitializationVector").

Použití **šifrování úložiště** tooencrypt obsahu místně pomocí standardu AES 256 bitů šifrování a nahrajte ho tooAzure úložiště, kde je uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování a umístit do předchozí tooencoding systému souborů EFS a volitelně znovu zašifrovat předchozí toouploading zpět v podobě nového výstupního prostředku. Hello případem primárního použití šifrování úložiště je, pokud chcete toosecure rest souborů vysoké kvality vstupními médii pomocí silného šifrování na disku.

Toodeliver order asset šifrované úložiště musíte nakonfigurovat zásady doručení assetu hello tak Media Services vědět, jak chcete toodeliver, obsah. Před Streamovat asset hello streamování šifrování úložiště hello odebere server a datových proudů svůj obsah pomocí hello zadat zásady pro doručení (například AES, běžným šifrováním nebo žádné šifrování).

## <a name="create-contentkeys-used-for-encryption"></a>Vytvoření ContentKeys pro šifrování
Šifrované prostředky mít toobe přidružené úložiště šifrovací klíč. Je nutné vytvořit toobe hello obsahu klíče pro šifrování před vytvořením hello soubory prostředků. Tato část popisuje, jak toocreate klíč obsahu.

Hello následují obecné kroky pro generování obsahu klíčů, které se spojují s prostředky, které chcete toobe zašifrovaná. 

1. Šifrování úložiště náhodně Generovat klíč standardu AES 32 bajtů. 
   
    Bude jím hello klíč obsahu pro váš asset, což znamená, všechny soubory, které jsou spojené s tohoto prostředku bude potřebovat toouse hello stejný klíč obsahu během dešifrování. 
2. Volání hello [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) a [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) metody tooget hello správné certifikátu X.509, který musí být použité tooencrypt klíč obsahu.
3. Šifrování klíče obsahu hello veřejným klíčem hello certifikát X.509. 
   
   Media Services .NET SDK používá RSA s OAEP při provádění šifrování hello.  Vidíte příklad rozhraní .NET v hello [EncryptSymmetricKeyData funkce](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4. Vytvoří hodnotu kontrolního součtu vypočítává pomocí identifikátoru klíče hello a klíč obsahu. Hello následující ukázka .NET vypočítá kontrolního součtu hello pomocí hello GUID součástí identifikátoru klíče hello a hello zrušte klíč obsahu.

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

1. Vytvořte klíč obsahu hello s hello **EncryptedContentKey** (převést řetězec s kódováním toobase64), **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, a **kontrolního součtu** hodnoty, které jste dostali v předchozích krocích.

    Šifrování úložiště hello zahrnovat následující vlastnosti by v textu žádosti hello.

    Vlastnost text žádosti    | Popis
    ---|---
    ID | Hello ContentKey Id, které se vygeneruje označována pomocí hello následující formátu, "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Toto je typ obsahu klíče hello jako celé číslo pro tento klíč obsahu. Jsme předejte hello hodnotu 1 pro šifrování úložiště.
    EncryptedContentKey | Vytvoříme novou hodnotu obsahu klíče, což je hodnota 256 bitů (32 bajtů). Hello je klíč zašifrovaný pomocí hello úložiště šifrování X.509 certifikátu, který načteme ze služby Microsoft Azure Media Services spuštěním požadavek HTTP GET pro hello GetProtectionKeyId a GetProtectionKey metody. Jako příklad najdete v části hello následující kód .NET: hello **EncryptSymmetricKeyData** metoda definované [zde](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | To je hello ochrany id klíče pro hello úložiště šifrovací X.509 certifikát, který byl použité tooencrypt naše klíč obsahu.
    ProtectionKeyType | Toto je typ šifrování hello hello ochrany klíče, který byl klíč obsahu použité tooencrypt hello. Tato hodnota je StorageEncryption(1) pro náš příklad.
    Kontrolní součet |Hello MD5 počítané kontrolního součtu pro klíč obsahu hello. Výpočet je šifrování obsahu hello Id obsahu klíčem hello. Hello příklad kódu ukazuje, jak toocalculate hello kontrolního součtu.


### <a name="retrieve-hello-protectionkeyid"></a>Načtení hello ProtectionKeyId
Hello následující příklad ukazuje, jak tooretrieve hello ProtectionKeyId, kryptografický otisk certifikátu pro certifikát hello, které musí použít při šifrování klíče obsahu. Proveďte tento krok toomake jistotu, že už máte příslušný certifikát hello na počítači.

Žádost:

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

Odpověď:

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

### <a name="retrieve-hello-protectionkey-for-hello-protectionkeyid"></a>Načtení hello ProtectionKey pro hello ProtectionKeyId
Hello následující příklad ukazuje, jak certifikát X.509 hello tooretrieve pomocí hello ProtectionKeyId jste obdrželi v předchozím kroku hello.

Žádost:

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

Odpověď:

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

### <a name="create-hello-content-key"></a>Vytvořte klíč obsahu hello
Po načíst certifikát X.509 hello a používá svůj veřejný klíč tooencrypt vašeho obsahu klíče, vytvoření **ContentKey** entity a sady jeho vlastnost hodnoty odpovídajícím způsobem.

Jedna z hodnot hello musí nastavit při vytváření hello obsahu, že je klíč hello typu. V případě hello šifrování úložiště hello hodnota je '1'. 

Následující příklad ukazuje, jak Hello toocreate **ContentKey** s **ContentKeyType** nastavení pro šifrování úložiště ("1") a hello **ProtectionKeyType** nastavit příliš "0" tooindicate, který hello ochrany klíče Id je kryptografický otisk certifikátu X.509 hello.  

Žádost

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

Odpověď:

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

## <a name="create-an-asset"></a>Vytvořit prostředek
Následující příklad ukazuje, jak Hello toocreate prostředek.

**Požadavek HTTP**

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

**Odpověď HTTP**

V případě úspěchu se vrátí hello následující:

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

## <a name="associate-hello-contentkey-with-an-asset"></a>Přidružit hello ContentKey prostředek
Po vytvoření hello ContentKey, přidružte ho Asset pomocí operace hello $links, jak ukazuje následující příklad hello:

Žádost:

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

Odpověď:

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a>Vytvoření AssetFile
Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entity představuje soubor video nebo zvuk, který je uložený v kontejneru objektů blob. Soubor asset je vždy přidružena k assetu a prostředek může obsahovat mnoho soubory asset. Hello Media Services Encoder úloh selže, pokud objekt souboru asset není spojen s digitálnímu souboru v kontejneru objektů blob.

Všimněte si, že hello **AssetFile** instance a hello samotný mediální soubor jsou dva odlišné objekty. Hello AssetFile instance obsahuje metadata o hello soubor média, zatímco soubor média hello obsahuje hello samotný mediální obsah.

Po odeslání souboru digitálního média do kontejneru objektů blob, budete používat hello **SLOUČENÍ** HTTP žádost tooupdate hello AssetFile s informacemi o souboru média (v tomto tématu není znázorněné). 

**Požadavek HTTP**

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

**Odpověď HTTP**

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
