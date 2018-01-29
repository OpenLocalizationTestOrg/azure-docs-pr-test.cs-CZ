---
title: "Šifrování na straně klienta s Javou pro Microsoft Azure Storage | Microsoft Docs"
description: "Klientská knihovna pro úložiště Azure pro jazyk Java podporuje šifrování na straně klienta a integraci s Azure Key Vault pro maximální zabezpečení pro vaše aplikace Azure Storage."
services: storage
documentationcenter: java
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 3df49907-554c-404a-9b0c-b3e3269ad04f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.openlocfilehash: 9f9ed8043d3671beacb9fabeb9e96604a8f065ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="client-side-encryption-and-azure-key-vault-with-java-for-microsoft-azure-storage"></a>Šifrování na straně klienta a Azure Key Vault s Javou pro Microsoft Azure Storage
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Přehled
[Klientská knihovna pro úložiště Azure pro jazyk Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) podporuje šifrování dat v rámci klientské aplikace před nahráním do úložiště Azure a dešifrování dat při stahování do klienta. Knihovna také podporuje integraci s [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) pro správu klíčů účtu úložiště.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Šifrování a dešifrování pomocí technik obálky
Procesy šifrování a dešifrování podle technika obálku.  

### <a name="encryption-via-the-envelope-technique"></a>Šifrování pomocí technik obálky
Šifrování pomocí technik obálky funguje následujícím způsobem:  

1. Klientská knihovna pro úložiště Azure generuje obsahu šifrovací klíč (CEK), což je použití jednoho bránu symetrického klíče.  
2. Data se šifrují pomocí této CEK.   
3. CEK je vnořena (šifrované) pomocí klíčů šifrovacího klíče (KEK). Klíče KEK je identifikovaná identifikátorem klíče a může být pár asymetrických klíčů nebo symetrického klíče a mohou být spravovaný místně nebo uloženy v Azure klíč trezorů.  
   Klientská knihovna pro úložiště, samotné nikdy má přístup k KEK. Knihovny vyvolá algoritmus zabalení klíče, který zajišťuje Key Vault. Uživatelé mohou používat vlastní zprostředkovatele pro klíče zabalení/rozbalování v případě potřeby.  
4. Šifrovaná data se pak odešlou do služby Azure Storage. Zabalená klíč společně se některé další šifrování metadat je uložena jako metadata (na binární rozsáhlý objekt) nebo interpolované s šifrovaná data (zprávy fronty a entity tabulky).

### <a name="decryption-via-the-envelope-technique"></a>Dešifrování pomocí technik obálky
Dešifrování pomocí technik obálky funguje následujícím způsobem:  

1. Klientské knihovny se předpokládá, že uživatel místně nebo v Azure klíč trezory spravuje klíče šifrovací klíč (KEK). Uživatel nemusí vědět konkrétní klíč, který slouží k šifrování. Místo toho klíče překladače, který se přeloží různé klíče identifikátorů klíče můžete nastavit a používat.  
2. Klientská knihovna stáhne šifrovaná data spolu se žádné šifrování materiál, který je uložený ve službě.  
3. Zabalená obsahu šifrovací klíč (CEK) je pak rozbalenou (dešifrovaný) používání klíčů šifrovací klíč (KEK). Zde znovu, knihovny klienta nemá přístup k KEK. Jednoduše volá, vlastní nebo rozbalení algoritmem Key Vault zprostředkovatele.  
4. Obsahu šifrovací klíč (CEK) se pak používá k dešifrování dat šifrovaných uživatele.

## <a name="encryption-mechanism"></a>Mechanismus šifrování
Klientská knihovna pro úložiště používá [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) k šifrování dat uživatele. Konkrétně [šifrovací bloku algoritmem CBC](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) režimu pomocí standardu AES. Každý trochu jinak, služba funguje, se budeme zabývat každý z nich zde.

### <a name="blobs"></a>Objekty blob
Klientská knihovna aktuálně podporuje šifrování pouze celý objekty BLOB. Konkrétně je podporováno šifrování, pokud uživatelé používají **nahrát*** metody nebo **openOutputStream** metoda. Pro stahování, obě dokončení a rozsah stahování jsou podporovány.  

Během šifrování se klientské knihovny bude generovat náhodných inicializace vektoru (IV) 16 bajtů, společně s náhodných obsahu šifrovací klíč (CEK) 32 bajtů a provádí obálky šifrování dat objektů blob na základě těchto informací. Zabalená CEK a některé další šifrování metadata jsou pak uloženy jako objekt blob metadat spolu s zašifrovaný objekt blob ve službě.

> [!WARNING]
> Pokud jsou úpravy nebo odesílání vlastní metadata pro objekt blob, je třeba zajistit, že se zachová, i tato metadata. Pokud nahrajete novými metadaty bez těchto metadat, bude ztracena zabalené CEK, IV a další metadata a obsah objektu blob se nikdy bude nenávratně ztracený.
> 
> 

Stahování zašifrovaný objekt blob zahrnuje načítání obsah pomocí celý objekt blob  **Stáhnout*/openInputStream** usnadňující metody. Zabalená CEK je úkony, spočívající a společně s IV (uložené v tomto případě jako metadata objektu blob) používá k vrácení dešifrovaná data pro uživatele.

Stahování libovolný rozsah (**downloadRange*** metody) v zašifrovaný objekt blob zahrnuje úpravy rozsahu poskytované uživatelé mohli malé množství další data, která slouží k dešifrování úspěšně žádost rozsah.  

Všechny typy blob (objekty BLOB bloků, objekty BLOB stránky a doplňovacích objektů BLOB) můžete šifrovat nebo dešifrovat použití tohoto schématu.

### <a name="queues"></a>Fronty
Vzhledem k tomu, že fronta zprávy můžou být libovolném formátu, klientské knihovny definuje vlastní formát, který zahrnuje inicializační vektor (IV) a šifrované obsahu šifrovací klíč (CEK) v textu zprávy.  

Během šifrování se klientské knihovny generuje náhodné IV 16 bajtů společně s náhodných CEK 32 bajtů a provádí šifrování obálky text zprávy fronty pomocí těchto informací. Zabalená CEK a některé další šifrování metadat se pak přidají do zprávy ve frontě šifrované. Této upravené zprávy (zobrazené dole) je uložený ve službě.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

Během dešifrování je zabalené klíč extrahovaným ze zprávy ve frontě a úkony, spočívající. IV je také extrahovaným ze zprávy ve frontě a používat společně s rozbalenou klíčem k dešifrování dat zprávu fronty. Všimněte si, že metadata šifrování je malá (v bajtech) 500, takže když ho započítávat limit 64KB pro zprávu fronty, by mělo být dopad spravovat.

### <a name="tables"></a>Tabulky
Klientská knihovna podporuje šifrování vlastností entity pro vložení a nahrazovat operace.

> [!NOTE]
> Sloučení se aktuálně nepodporuje. Vzhledem k tomu, že podmnožinu vlastností může být šifrována dříve pomocí jiného klíče, jednoduše slučování nové vlastnosti a aktualizace metadat dojde ke ztrátě dat. Slučování buď vyžaduje volání další služby ke čtení existující entity ze služby, nebo pomocí nového klíče na vlastnosti, které nejsou vhodné z důvodů výkonu.
> 
> 

Šifrování dat tabulky funguje takto:  

1. Uživatelé zadat vlastnosti k zašifrování.  
2. Klientská knihovna generuje náhodných inicializace vektoru (IV) 16 bajtů společně s klíčem náhodných šifrování obsahu (CEK) 32 bajtů pro každou entitu a provádí šifrování obálky na jednotlivé vlastnosti k zašifrování odvozením nová IV za vlastnost. Zašifrované vlastnosti se ukládají jako binární data.  
3. Zabalená CEK a některé další šifrování metadata jsou pak uloženy jako dva další rezervované vlastnosti. Vlastnost první vyhrazené (_ClientEncryptionMetadata1) je ve vlastnosti string, který obsahuje informace o IV, verzi a zabalené klíč. Vlastnost druhý vyhrazené (_ClientEncryptionMetadata2) je binární vlastnost, která obsahuje informace o vlastnosti, které jsou zašifrované. Informace v této druhé vlastnosti (_ClientEncryptionMetadata2) je zašifrovaná.  
4. Z důvodu tyto další rezervované vlastnosti vyžadované pro šifrování uživatelé nyní mohou mít pouze 250 vlastní vlastnosti místo 252. Celková velikost entity, musí být menší než 1MB.  
   
   Všimněte si, že pouze vlastnosti řetězce mohou být šifrována. Pokud jsou i další typy vlastností k šifrování, musí být převeden na řetězce. Šifrované řetězce jsou uložené ve službě jako binární vlastnosti a převedené zpět do řetězce po dešifrování.
   
   Pro tabulky, kromě zásady šifrování musí uživatelé zadat vlastnosti k zašifrování. To lze provést zadáním buď atribut [šifrovat] \(pro entity objektů POCO, které jsou odvozeny od TableEntity) nebo šifrování překladač v žádosti o možnostech. Překladač šifrování je delegáta, který přebírá klíč oddílu, klíč řádku a název vlastnosti a vrátí logickou hodnotu, která určuje, jestli by se šifrovat tuto vlastnost. Během šifrování se klientské knihovny použije tyto informace se rozhodnout, jestli by se vlastnost šifrovat při zápisu do sítě. Delegát taky poskytuje možnost logiku kolem jak jsou zašifrované vlastnosti. (Například pokud X, potom šifrování vlastnost A; v opačném případě šifrování vlastnosti A a B.) Všimněte si, že není nutné poskytnout tyto informace při čtení nebo dotazování entity.

### <a name="batch-operations"></a>Dávkové operace
V dávkových operací stejné KEK se použije mezi všechny řádky v této dávkové operace protože klientské knihovny umožňuje pouze jeden objekt možnosti (a proto jednu zásadu nebo KEK) za dávkovou operaci. Však klientské knihovny bude interně vygenerujte nový náhodný IV a náhodných CEK na řádek v dávce. Uživatelé mohou také k šifrování různé vlastnosti pro všechny operace v dávce definováním toto chování v překladač šifrování.

### <a name="queries"></a>Dotazy
K provedení operace dotazů, je nutné zadat klíče překladače, který se bude moct vyřešit všechny klíče v sadě výsledků dotazu. Pokud entity obsažené ve výsledku dotazu nelze přeložit na poskytovatele, knihovny klienta vyvolá chybu. Po jakémkoli dotazu, který provádí projekce straně serveru přidá klientské knihovny vlastnosti metadat speciální šifrování (_ClientEncryptionMetadata1 a _ClientEncryptionMetadata2) ve výchozím nastavení vybrané sloupce.

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Key Vault pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami. Pomocí Azure Key Vault, uživatelé mohou šifrovat klíče a tajné klíče (např. ověřovací klíče, klíče účtu úložiště, šifrovací klíče dat. Soubory PFX a hesla) pomocí klíčů chráněných moduly hardwarového zabezpečení (HSM). Další informace najdete v tématu [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md).

Klientská knihovna pro úložiště používá k zajištění společná architektura pro správu klíčů v Azure Key Vault základní knihovny. Uživatelé také získat další výhody použití knihovny rozšíření Key Vault. Knihovna rozšíření poskytuje užitečné funkce kolem snadný Symmetric/RSA místní a klíče poskytovatelů cloudu, a také s agregaci a ukládání do mezipaměti.

### <a name="interface-and-dependencies"></a>Rozhraní a závislosti
Existují tři balíčky Key Vault:  

* Azure-keyvault-core obsahuje IKey a IKeyResolver. Jedná se o malé balíček nemá žádné závislosti. Klientská knihovna pro úložiště pro jazyk Java se definuje jako závislost.
* Azure keyvault obsahuje klienta klíč trezoru REST.  
* rozšíření Azure keyvault obsahuje rozšíření kód, který zahrnuje implementace kryptografické algoritmy a RSAKey SymmetricKey. To závisí na základní a KeyVault obory názvů a poskytuje funkce, které definují agregační překladač (když uživatelé chtějí používat více poskytovatelů klíče) a ukládání do mezipaměti klíče překladač. I když klientská knihovna pro úložiště nezávisí přímo na tomto balíčku, pokud chcete používat Azure Key Vault k ukládání svých klíčů nebo použít rozšíření Key Vault využívat místní a Cloudová zprostředkovatelů kryptografických služeb uživatele, potřebují tento balíček.  
  
  Key Vault je navržený pro vysoké hodnoty hlavního klíče a omezení omezení za Key Vault jsou navrženy s tímto v paměti. Při provádění šifrování na straně klienta s Key Vault, je preferovaným modelem použití symetrické klíče hlavní uložené jako tajných klíčů v Key Vault a uložili do mezipaměti místně. Uživatelé, postupujte takto:  

1. Vytvoření tajného klíče do režimu offline a nahrajte ho do Key Vault.  
2. Základní identifikátor tajného klíče můžete použijte jako parametr vyřešit aktuální verzi tajného klíče pro šifrování a tyto informace místně do mezipaměti. Použít CachingKeyResolver pro ukládání do mezipaměti; Uživatelé se nepředpokládá implementovat vlastní ukládání do mezipaměti logiku.  
3. Použijte překladač ukládání do mezipaměti jako vstup při vytváření zásady šifrování.
   Další informace o využití Key Vault naleznete v ukázky kódu šifrování. <fix URL>  

## <a name="best-practices"></a>Osvědčené postupy
Podpora šifrování je dostupné pouze v Klientská knihovna pro úložiště pro jazyk Java.

> [!IMPORTANT]
> Mějte na paměti tyto důležité bodů při použití šifrování na straně klienta:
> 
> * Při čtení nebo zápisu do zašifrovaný objekt blob, pomocí příkazů nahrávání celý objekt blob a rozsah nebo celé blob stažení příkazy. Vyhněte se zápis do zašifrovaný objekt blob pomocí operace protokolu například Put bloku, uveďte seznam blokovaných, zápis stránky, zrušte stránky nebo připojit blok; jinak může dojít k poškození zašifrovaný objekt blob a nastavit jej jako nečitelná.
> * Pro tabulky podobně jako omezení existuje. Pečlivě nelze aktualizovat zašifrované vlastnosti bez aktualizace metadata šifrování.  
> * Pokud jste nastavili metadata na zašifrovaný objekt blob, může přepsat metadata šifrování požadované pro dešifrování, protože není sčítání nastavení metadat. To platí také pro snímky; Vyhněte se zadání metadat při vytváření snímku zašifrovaný objekt blob. Pokud musí být nastavená metadata, je nutné volat **downloadAttributes** metodu nejdřív získat aktuální metadata šifrování a zabránit souběžných zápisy při nastavování metadat.  
> * Povolit **requireEncryption** příznak ve výchozí možnosti požadavku pro uživatele, kteří by měla fungovat jenom s šifrovaná data. Další informace najdete níže.  
> 
> 

## <a name="client-api--interface"></a>Klientské rozhraní API / rozhraní
Při vytváření objektu EncryptionPolicy, uživatelé klíč můžete zadat jenom (implementace IKey), pouze překladač (implementace IKeyResolver), nebo obojí. IKey je základní typ klíče, který je identifikován pomocí identifikátoru klíče, který poskytuje logiku pro zabalení/rozbalování. IKeyResolver se používá k překladu během procesu dešifrovací klíč. Definuje metodu ResolveKey, která vrátí IKey zadaný identifikátor klíče. To poskytuje uživatelům možnost zvolit mezi více klíčů, které jsou spravovány v několika umístěních.

* Pro šifrování používá se vždy a chybí klíč bude mít za následek chybu.  
* K dešifrování:  
  
  * Překladač klíče je volána, pokud zadaný k získání klíče. Pokud je zadaný překladač, ale nemá mapování pro identifikátor klíče, je vyvolána k chybě.  
  * Pokud není zadaný překladač, ale je určen klíč, klíč se používá, pokud jeho identifikátoru odpovídá identifikátoru požadované klíče. Pokud identifikátor neodpovídá, je vyvolána k chybě.  
    
    [Šifrování ukázky](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>ukazují podrobnější případě začátku do konce pro objekty BLOB, fronty a tabulky, spolu s integrací služby Key Vault.

### <a name="requireencryption-mode"></a>Režim RequireEncryption
Uživatelé mohou volitelně povolit režim operace, kde musí být všechny nahrávání a stahování zašifrována. V tomto režimu se nezdaří pokusy o odeslání dat bez zásady šifrování nebo data, která nejsou šifrována ve službě Stažení na straně klienta. **RequireEncryption** příznak možnosti objektu žádosti řídí toto chování. Pokud vaše aplikace bude šifrování všech objektech uložených ve službě Azure Storage, pak můžete nastavit **requireEncryption** vlastnost na výchozí možnosti požadavek pro objekt služby klienta.   

Například použít **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** k vyžadování šifrování pro všechny operace provedené prostřednictvím klienta objektu blob.

### <a name="blob-service-encryption"></a>Šifrování služby objektů BLOB
Vytvoření **BlobEncryptionPolicy** objektu a nastavte ji v možnosti požadavku (na rozhraní API nebo na úrovni klienta pomocí **DefaultRequestOptions**). Všem ostatním bude zpracovávat klientské knihovny interně.

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

// Set the encryption policy on the request options.
BlobRequestOptions options = new BlobRequestOptions();
options.setEncryptionPolicy(policy);

// Upload the encrypted contents to the blob.
blob.upload(stream, size, null, options, null);

// Download and decrypt the encrypted contents from the blob.
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
blob.download(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Šifrování služby fronty
Vytvoření **QueueEncryptionPolicy** objektu a nastavte ji v možnosti požadavku (na rozhraní API nebo na úrovni klienta pomocí **DefaultRequestOptions**). Všem ostatním bude zpracovávat klientské knihovny interně.

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

// Add message
QueueRequestOptions options = new QueueRequestOptions();
options.setEncryptionPolicy(policy);

queue.addMessage(message, 0, 0, options, null);

// Retrieve message
CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);
```

### <a name="table-service-encryption"></a>Šifrování služby Table
Kromě vytváření zásad šifrování a jeho nastavení na žádost o možnostech, musíte buď určit **EncryptionResolver** v **TableRequestOptions**, nebo nastavte atribut [šifrovat] entity Metoda getter a setter.

### <a name="using-the-resolver"></a>Pomocí překladače

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

TableRequestOptions options = new TableRequestOptions()
options.setEncryptionPolicy(policy);
options.setEncryptionResolver(new EncryptionResolver() {
    public boolean encryptionResolver(String pk, String rk, String key) {
        if (key == "foo")
        {
            return true;
        }
        return false;
    }
});

// Insert Entity
currentTable.execute(TableOperation.insert(ent), options, null);

// Retrieve Entity
// No need to specify an encryption resolver for retrieve
TableRequestOptions retrieveOptions = new TableRequestOptions()
retrieveOptions.setEncryptionPolicy(policy);

TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
TableResult result = currentTable.execute(operation, retrieveOptions, null);
```

### <a name="using-attributes"></a>Pomocí atributů
Jak je uvedeno výše, pokud entity implementuje TableEntity, pak vlastnosti getter a setter může být doplněny pomocí atributu [šifrovat] místo zadávání **EncryptionResolver**.

```java
private string encryptedProperty1;

@Encrypt
public String getEncryptedProperty1 () {
    return this.encryptedProperty1;
}

@Encrypt
public void setEncryptedProperty1(final String encryptedProperty1) {
    this.encryptedProperty1 = encryptedProperty1;
}
```

## <a name="encryption-and-performance"></a>Šifrování a výkonu
Všimněte si, že šifrování dat výsledky úložiště v dalších zatížení. Musí být generovány klíč obsahu a IV, musí být šifrovaný samotný obsah a další metadata musí být naformátovaná a nahrát. Tato dodatečná režie se bude lišit v závislosti na objemu dat šifrovaný. Doporučujeme vám, že zákazníci vždy testování aplikací pro výkon při vývoji.

## <a name="next-steps"></a>Další kroky
* Stažení [Klientská knihovna pro úložiště Azure pro balíček Java Maven](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)  
* Stažení [Klientská knihovna pro úložiště Azure pro Javu zdrojového kódu z Githubu](https://github.com/Azure/azure-storage-java)   
* Stáhněte si Maven knihovny Azure klíč trezoru pro balíčky Java Maven:
  * [Základní](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core) balíčku
  * [Klient](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault) balíčku
* Přejděte [dokumentace Azure Key Vault](../../key-vault/key-vault-whatis.md)