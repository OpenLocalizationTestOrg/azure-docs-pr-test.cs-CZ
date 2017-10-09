---
title: "Šifrování na straně aaaClient s Javou pro úložiště Microsoft Azure | Microsoft Docs"
description: "Hello Klientská knihovna pro úložiště Azure pro jazyk Java podporuje šifrování na straně klienta a integraci s Azure Key Vault pro maximální zabezpečení pro vaše aplikace Azure Storage."
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
ms.openlocfilehash: cb40c737b9065005f0f31f654e7e43fd27b923f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="client-side-encryption-and-azure-key-vault-with-java-for-microsoft-azure-storage"></a>Šifrování na straně klienta a Azure Key Vault s Javou pro Microsoft Azure Storage
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Přehled
Hello [Klientská knihovna pro úložiště Azure pro jazyk Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) podporuje šifrování dat v rámci klientské aplikace před nahráním tooAzure úložiště a dešifrování dat při stahování toohello klienta. Hello knihovna také podporuje integraci s [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) pro správu klíčů účtu úložiště.

## <a name="encryption-and-decryption-via-hello-envelope-technique"></a>Šifrování a dešifrování prostřednictvím hello obálky technika
Hello procesy šifrování a dešifrování podle technika obálky hello.  

### <a name="encryption-via-hello-envelope-technique"></a>Šifrování prostřednictvím hello obálky technika
Šifrování pomocí technik obálky hello funguje v hello následujícím způsobem:  

1. Klientská knihovna pro úložiště Azure Hello generuje obsahu šifrovací klíč (CEK), což je použití jednoho bránu symetrického klíče.  
2. Data se šifrují pomocí této CEK.   
3. Hello CEK je vnořena (šifrované) pomocí hello klíče šifrovacího klíče (KEK). Hello KEK je identifikovaná identifikátorem klíče a může být pár asymetrických klíčů nebo symetrického klíče a mohou být spravovaný místně nebo uloženy v Azure klíč trezorů.  
   Hello Klientská knihovna pro úložiště samotné nikdy tooKEK přístup. Knihovna Hello vyvolá hello klíče zabalení algoritmus, který zajišťuje Key Vault. Uživatelé mohou toouse vlastního zprostředkovatele pro klíče zabalení/rozbalování v případě potřeby.  
4. Hello šifrovaná data se pak nahrán toohello služby Azure Storage. Hello zabalené klíč společně se některé další šifrování metadat je uložena jako metadata (na binární rozsáhlý objekt) nebo interpolované s hello zašifrovaná data (zprávy fronty a entity tabulky).

### <a name="decryption-via-hello-envelope-technique"></a>Dešifrování pomocí technik hello obálky
Dešifrování pomocí technik obálky hello funguje v hello následujícím způsobem:  

1. Klientská knihovna pro Hello předpokládá, že tento uživatel hello místně nebo v Azure klíč trezory spravuje hello klíče šifrovací klíč (KEK). Hello uživatele nemusí hello tooknow konkrétní se klíč, který slouží k šifrování. Překladač klíče, který se přeloží tookeys různé klíče identifikátory místo toho můžete nastavit a používat.  
2. Klientská knihovna pro Hello stáhne hello zašifrovaná data spolu se žádné šifrování materiál, který je uložený ve službě hello.  
3. Hello zabalené obsahu šifrovací klíč (CEK) je pak rozbalenou (dešifrovaný) pomocí hello klíče šifrovací klíč (KEK). Zde znovu hello klientské knihovny nemá tooKEK přístup. Jednoduše volá, hello vlastní nebo rozbalení algoritmem Key Vault zprostředkovatele.  
4. Hello obsahu šifrovací klíč (CEK) je pak používá toodecrypt hello šifrované uživatelská data.

## <a name="encryption-mechanism"></a>Mechanismus šifrování
Klientská knihovna pro úložiště Hello používá [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) v pořadí tooencrypt uživatelská data. Konkrétně [šifrovací bloku algoritmem CBC](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) režimu pomocí standardu AES. Každý trochu jinak, služba funguje, se budeme zabývat každý z nich zde.

### <a name="blobs"></a>Objekty blob
Klientská knihovna pro Hello aktuálně podporuje šifrování pouze celý objekty BLOB. Konkrétně je podporováno šifrování, pokud uživatelé používají hello **nahrát*** metody nebo hello **openOutputStream** metoda. Pro stahování, obě dokončení a rozsah stahování jsou podporovány.  

Během šifrování se hello klientské knihovny bude generovat náhodných inicializace vektoru (IV) 16 bajtů, společně s náhodných obsahu šifrovací klíč (CEK) 32 bajtů a provádí obálky šifrování dat objektů blob hello používá tyto informace. Hello zabalená CEK a některé další šifrování metadata jsou pak uloženy jako objekt blob metadat spolu s hello zašifrovaný objekt blob ve službě hello.

> [!WARNING]
> Pokud upravujete nebo nahrát vlastní metadata pro objekt blob hello, je nutné tooensure, že se zachová, i tato metadata. Pokud nahrajete novými metadaty bez těchto metadat, hello zabalené CEK, bude ztracena IV a další metadata a obsah objektu blob hello se nikdy bude nenávratně ztracený.
> 
> 

Stahování zašifrovaný objekt blob zahrnuje načítání hello obsahu objektu blob celý hello pomocí hello  **Stáhnout*/openInputStream** usnadňující metody. Hello zabalené CEK je úkony, spočívající a použít společně s hello IV (uložené jako metadata objektu blob v tomto případě) tooreturn hello dešifrovat data toohello uživatele.

Stahování libovolný rozsah (**downloadRange*** metody) v hello zašifrovaný objekt blob zahrnuje úpravy rozsahu hello zadaný uživateli v pořadí tooget malé množství další data, která lze použít toosuccessfully dešifrovat hello požadovaný rozsah.  

Všechny typy blob (objekty BLOB bloků, objekty BLOB stránky a doplňovacích objektů BLOB) můžete šifrovat nebo dešifrovat použití tohoto schématu.

### <a name="queues"></a>Fronty
Vzhledem k tomu, že fronta zprávy můžou být libovolném formátu, definuje hello Klientská knihovna pro vlastní formát, který obsahuje text zprávy hello hello inicializační vektor (IV) a hello šifrované obsahu šifrovací klíč (CEK).  

Během šifrování se hello klientské knihovny generuje náhodné IV 16 bajtů společně s náhodných CEK 32 bajtů a provádí šifrování obálky text zprávy fronty hello na základě těchto informací. Hello zabalená CEK a některé další šifrování metadat se pak přidají toohello šifrované fronty zpráv. Této upravené zprávy (zobrazené dole) jsou uloženy na hello služby.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

Během dešifrování je klíč zabalené hello extrahovaným ze zprávy fronty hello a úkony, spočívající. Hello IV je také extrahovaným ze zprávy fronty hello a používat společně s daty zprávy hello úkony, spočívající klíče toodecrypt hello fronty. Všimněte si, že metadata šifrování hello jsou malé (v bajtech) 500, takže když ho započítávat hello limit 64 KB pro zprávu fronty, by mělo být hello dopad spravovat.

### <a name="tables"></a>Tabulky
Hello klienta knihovny podporuje šifrování vlastností entity pro vložení a nahrazovat operace.

> [!NOTE]
> Sloučení se aktuálně nepodporuje. Vzhledem k tomu, že podmnožinu vlastností může být šifrována dříve pomocí jiného klíče, jednoduše slučování hello nové vlastnosti a aktualizace hello metadat dojde ke ztrátě dat. Slučování buď vyžaduje provedení další služby volání tooread hello existující entity ze služby hello nebo pomocí nového klíče na vlastnosti, které nejsou vhodné z důvodů výkonu.
> 
> 

Šifrování dat tabulky funguje takto:  

1. Uživatelé zadat toobe vlastnosti hello zašifrovaná.  
2. Klientská knihovna pro Hello generuje náhodných inicializace vektoru (IV) 16 bajtů společně s klíčem náhodných šifrování obsahu (CEK) 32 bajtů pro každou entitu a provádí šifrování obálky toobe jednotlivé vlastnosti hello šifrované odvozením nová IV za Vlastnost. Hello zašifrované vlastnosti se ukládají jako binární data.  
3. Hello zabalená CEK a některé další šifrování metadata jsou pak uloženy jako dva další rezervované vlastnosti. první vyhrazené vlastnost Hello (_ClientEncryptionMetadata1) je řetězec vlastnost, která obsahuje hello informace o IV, verzi a zabalené klíč. druhý vyhrazené vlastnost Hello (_ClientEncryptionMetadata2) je binární vlastnost, která obsahuje hello informace o hello vlastnosti, které jsou zašifrované. Hello informace v této druhé vlastnosti (_ClientEncryptionMetadata2) je zašifrovaná.  
4. Z důvodu toothese další rezervované vlastnosti vyžadované pro šifrování uživatelé nyní mohou mít pouze 250 vlastní vlastnosti místo 252. Celková velikost Hello hello entity musí být menší než 1MB.  
   
   Všimněte si, že pouze vlastnosti řetězce mohou být šifrována. Pokud jsou i další typy vlastností toobe zašifrovaná, musí být převeden toostrings. Hello šifrované řetězce jsou uložené ve službě hello jako binární vlastnosti a převedení back toostrings po dešifrování.
   
   Pro tabulky kromě toohello zásady šifrování, musí uživatelé zadat toobe vlastnosti hello zašifrovaná. To lze provést zadáním buď atribut [šifrovat] (pro entity objektů POCO, které jsou odvozeny od TableEntity) nebo šifrování překladač v žádosti o možnostech. Překladač šifrování je delegáta, který přebírá klíč oddílu, klíč řádku a název vlastnosti a vrátí logickou hodnotu, která určuje, jestli by se šifrovat tuto vlastnost. Během šifrování se použije hello klientské knihovny tato informace toodecide zda vlastnosti by se měla šifrovat během zápisu toohello přenosu. Delegát Hello také poskytuje možnost hello logiky kolem jak jsou zašifrované vlastnosti. (Například pokud X, potom šifrování vlastnost A; v opačném případě šifrování vlastnosti A a B.) Všimněte si, že IT oddělení není nutné tooprovide tyto informace při čtení nebo dotazování entity.

### <a name="batch-operations"></a>Dávkové operace
V dávkových operací hello stejné KEK se použije mezi všechny řádky hello v této dávkové operace protože hello klientské knihovny umožňuje pouze jeden objekt možnosti (a proto jednu zásadu nebo KEK) za dávkovou operaci. Ale hello klientské knihovny bude interně vygenerujte nový náhodný IV a náhodných CEK na řádek v dávce hello. Uživatele můžete také zvolit tooencrypt různé vlastnosti pro všechny operace v dávce hello definováním toto chování v hello šifrování překladač.

### <a name="queries"></a>Dotazy
operace dotazů tooperform, je nutné zadat klíče překladače, který je možné tooresolve všechny hello klíče v sadě výsledků hello. Entity obsažené ve výsledku dotazu hello nelze přeložit tooa poskytovatele, vyvolá výjimku hello klientské knihovny k chybě. Po jakémkoli dotazu, který provádí projekce straně serveru bude hello klientské knihovny přidat hello speciální šifrování metadat vlastností (_ClientEncryptionMetadata1 a _ClientEncryptionMetadata2) výchozí toohello vybrané sloupce.

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Key Vault pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami. Pomocí Azure Key Vault, uživatelé mohou šifrovat klíče a tajné klíče (např. ověřovací klíče, klíče účtu úložiště, šifrovací klíče dat. Soubory PFX a hesla) pomocí klíčů chráněných moduly hardwarového zabezpečení (HSM). Další informace najdete v tématu [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md).

Klientská knihovna pro úložiště Hello využívá hello Key Vault základní knihovna v pořadí tooprovide společné architektury v Azure pro správu klíčů. Uživatelé získat také pomocí hello Key Vault rozšíření knihovny hello další výhody. Knihovna rozšíření Hello poskytuje užitečné funkce kolem snadný Symmetric/RSA místní a klíče poskytovatelů cloudu, a také s agregaci a ukládání do mezipaměti.

### <a name="interface-and-dependencies"></a>Rozhraní a závislosti
Existují tři balíčky Key Vault:  

* Azure-keyvault-core obsahuje hello IKey a IKeyResolver. Jedná se o malé balíček nemá žádné závislosti. Klientská knihovna pro Hello úložiště pro jazyk Java se definuje jako závislost.
* Azure keyvault obsahuje hello REST trezoru klíč klienta.  
* rozšíření Azure keyvault obsahuje rozšíření kód, který zahrnuje implementace kryptografické algoritmy a RSAKey SymmetricKey. To závisí na základní a KeyVault hello obory názvů a poskytuje funkce toodefine agregační překladač (když uživatelé mají toouse několik poskytovatelů klíče) a ukládání do mezipaměti klíče překladač. I když klientská knihovna pro úložiště hello nezávisí přímo na tomto balíčku, pokud chcete toouse Azure Key Vault toostore jejich klíče nebo toouse hello Key Vault rozšíření tooconsume hello místní uživatele a cloud zprostředkovatelů kryptografických služeb, potřebují tento balíček.  
  
  Key Vault je navržený pro vysoké hodnoty hlavního klíče a omezení omezení za Key Vault jsou navrženy s tímto v paměti. Při provádění šifrování na straně klienta s Key Vault, je preferovaným modelem hello toouse symetrické hlavního klíče uložené jako tajných klíčů v Key Vault a v místní mezipaměti. Uživatelé musí provést hello následující:  

1. Vytvoření tajného klíče do režimu offline a nahrajte ho tooKey trezoru.  
2. Jako parametr tooresolve hello aktuální verzi hello tajný klíč pro šifrování a tyto informace místně do mezipaměti, použijte základní identifikátor hello tajný klíč. Použít CachingKeyResolver pro ukládání do mezipaměti; Uživatelé není očekávaný tooimplement vlastní ukládání do mezipaměti logiku.  
3. Pomocí ukládání do mezipaměti překladač hello jako vstup při vytváření zásady šifrování hello.
   Další informace o využití Key Vault naleznete v ukázky kódu hello šifrování. <fix URL>  

## <a name="best-practices"></a>Osvědčené postupy
Podpora šifrování je dostupné pouze v klientské knihovny pro hello úložiště pro jazyk Java.

> [!IMPORTANT]
> Mějte na paměti tyto důležité bodů při použití šifrování na straně klienta:
> 
> * Při čtení z nebo psaní tooan objektů blob, použití celý objekt blob odesílání příkazů a příkazy stažení objektů blob rozsah nebo celé šifrovaný. Vyhněte se zápis tooan zašifrovaný objekt blob pomocí operace protokolu například Put bloku, uveďte seznam blokovaných, zápis stránky, zrušte stránky nebo připojit blok; jinak může dojít k poškození hello zašifrovaný objekt blob a nastavit jej jako nečitelná.
> * Pro tabulky podobně jako omezení existuje. Být opatrní toonot aktualizace zašifrované vlastnosti bez aktualizace metadata šifrování hello.  
> * Pokud jste nastavili metadata na hello zašifrovaný objekt blob, může přepsat hello metadata šifrování požadované pro dešifrování, protože není sčítání nastavení metadat. To platí také pro snímky; Vyhněte se zadání metadat při vytváření snímku zašifrovaný objekt blob. Pokud musí být nastavená metadata, zda text hello toocall být **downloadAttributes** tooget první metoda hello aktuální metadata šifrování a provádět souběžné zápisy při nastavování metadat.  
> * Povolit hello **requireEncryption** příznak hello výchozí žádost o možnosti pro uživatele, kteří by měla fungovat jenom s šifrovaná data. Další informace najdete níže.  
> 
> 

## <a name="client-api--interface"></a>Klientské rozhraní API / rozhraní
Při vytváření objektu EncryptionPolicy, uživatelé klíč můžete zadat jenom (implementace IKey), pouze překladač (implementace IKeyResolver), nebo obojí. IKey je hello základní typ klíče, je identifikován pomocí identifikátoru klíče a zabalení/rozbalování poskytuje logiku hello. IKeyResolver je použité tooresolve klíč během dešifrování hello. Definuje metodu ResolveKey, která vrátí IKey zadaný identifikátor klíče. To poskytuje uživatelům hello možnost toochoose mezi více klíčů, které jsou spravovány v několika umístěních.

* Pro šifrování vždy používá klíč hello a hello absenci klíč bude výsledkem chyba.  
* K dešifrování:  
  
  * překladač klíče Hello je volána, pokud zadaný klíč tooget hello. Pokud je zadaný překladač hello, ale nemá mapování pro identifikátor hello klíče, je vržena chyba.  
  * Pokud není zadaný překladač, ale je určen klíč, hello klíč se používá, pokud jeho identifikátoru odpovídá identifikátoru klíče hello vyžaduje. Pokud hello identifikátor neodpovídá, je vyvolána k chybě.  
    
    Hello [šifrování ukázky](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>ukazují podrobnější případě začátku do konce pro objekty BLOB, fronty a tabulky, spolu s integrací služby Key Vault.

### <a name="requireencryption-mode"></a>Režim RequireEncryption
Uživatelé mohou volitelně povolit režim operace, kde musí být všechny nahrávání a stahování zašifrována. V tomto režimu se nezdaří pokusy o tooupload dat bez šifrování zásad nebo stažení data, která nejsou šifrována hello služby v klientovi hello. Hello **requireEncryption** příznak hello požadavek možnosti objektu určuje toto chování. Pokud vaše aplikace bude šifrování všech objektech uložených ve službě Azure Storage, pak můžete nastavit hello **requireEncryption** vlastnost hello výchozí možnosti požadavek pro objekt klienta služby hello.   

Například použít **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** toorequire šifrování pro všechny operace objektů blob se provádí prostřednictvím objektu klienta.

### <a name="blob-service-encryption"></a>Šifrování služby objektů BLOB
Vytvoření **BlobEncryptionPolicy** objektu a nastavte ji v možnosti požadavek hello (na rozhraní API nebo na úrovni klienta pomocí **DefaultRequestOptions**). Všem ostatním bude zpracovávat hello klientské knihovny interně.

```java
// Create hello IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create hello encryption policy toobe used for upload and download.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

// Set hello encryption policy on hello request options.
BlobRequestOptions options = new BlobRequestOptions();
options.setEncryptionPolicy(policy);

// Upload hello encrypted contents toohello blob.
blob.upload(stream, size, null, options, null);

// Download and decrypt hello encrypted contents from hello blob.
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
blob.download(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Šifrování služby fronty
Vytvoření **QueueEncryptionPolicy** objektu a nastavte ji v možnosti požadavek hello (na rozhraní API nebo na úrovni klienta pomocí **DefaultRequestOptions**). Všem ostatním bude zpracovávat hello klientské knihovny interně.

```java
// Create hello IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create hello encryption policy toobe used for upload and download.
QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

// Add message
QueueRequestOptions options = new QueueRequestOptions();
options.setEncryptionPolicy(policy);

queue.addMessage(message, 0, 0, options, null);

// Retrieve message
CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);
```

### <a name="table-service-encryption"></a>Šifrování služby Table
Kromě toho toocreating zásady šifrování a jeho nastavení na žádost o možnostech, musíte buď zadat **EncryptionResolver** v **TableRequestOptions**, nebo atributu [šifrovat] hello sady hello entity metody getter a setter.

### <a name="using-hello-resolver"></a>Pomocí překladače hello

```java
// Create hello IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create hello encryption policy toobe used for upload and download.
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
// No need toospecify an encryption resolver for retrieve
TableRequestOptions retrieveOptions = new TableRequestOptions()
retrieveOptions.setEncryptionPolicy(policy);

TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
TableResult result = currentTable.execute(operation, retrieveOptions, null);
```

### <a name="using-attributes"></a>Pomocí atributů
Jak je uvedeno výše, pokud hello entity implementuje TableEntity, pak hello vlastnosti getter a setter může být doplněny pomocí atributu hello [šifrovat] místo zadání hello **EncryptionResolver**.

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
Všimněte si, že šifrování dat výsledky úložiště v dalších zatížení. Hello obsahu musí být generovány klíč a IV, musí být šifrovaný samotný obsah hello a další metadata musí být naformátovaná a nahrát. Tato dodatečná režie se bude lišit v závislosti na objemu dat šifrovaný hello. Doporučujeme vám, že zákazníci vždy testování aplikací pro výkon při vývoji.

## <a name="next-steps"></a>Další kroky
* Stáhnout hello [Klientská knihovna pro úložiště Azure pro balíček Java Maven](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)  
* Stáhnout hello [Klientská knihovna pro úložiště Azure pro Javu zdrojového kódu z Githubu](https://github.com/Azure/azure-storage-java)   
* Stáhněte si hello Azure Key Vault Maven knihovna Java Maven balíčky:
  * [Základní](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core) balíčku
  * [Klient](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault) balíčku
* Navštivte hello [dokumentaci k Azure Key Vault](../../key-vault/key-vault-whatis.md)
