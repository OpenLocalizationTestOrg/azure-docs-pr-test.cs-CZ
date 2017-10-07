---
title: "Šifrování na straně aaaClient s Python pro úložiště Microsoft Azure | Microsoft Docs"
description: "Klientská knihovna pro úložiště Azure pro jazyk Python Hello podporuje šifrování na straně klienta pro maximální zabezpečení pro vaše aplikace Azure Storage."
services: storage
documentationcenter: python
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: f9bf7981-9948-4f83-8931-b15679a09b8a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.openlocfilehash: 3a52b64f93daf85a55308f8a4bee9c98b2315d4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Šifrování na straně klienta s Python pro Microsoft Azure Storage
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Přehled
Hello [Klientská knihovna pro úložiště Azure pro jazyk Python](https://pypi.python.org/pypi/azure-storage) podporuje šifrování dat v rámci klientské aplikace před nahráním tooAzure úložiště a dešifrování dat při stahování toohello klienta.

> [!NOTE]
> Knihovna Python úložiště Azure Hello je ve verzi preview.
> 
> 

## <a name="encryption-and-decryption-via-hello-envelope-technique"></a>Šifrování a dešifrování prostřednictvím hello obálky technika
Hello procesy šifrování a dešifrování podle technika obálky hello.

### <a name="encryption-via-hello-envelope-technique"></a>Šifrování prostřednictvím hello obálky technika
Šifrování pomocí technik obálky hello funguje v hello následujícím způsobem:

1. Klientská knihovna pro úložiště Azure Hello generuje obsahu šifrovací klíč (CEK), což je použití jednoho bránu symetrického klíče.
2. Data se šifrují pomocí této CEK.
3. Hello CEK je vnořena (šifrované) pomocí hello klíče šifrovacího klíče (KEK). Hello KEK je identifikovaná identifikátorem klíče a může být pár asymetrických klíčů nebo symetrický klíč, který je spravovaný místně.
   Hello Klientská knihovna pro úložiště samotné nikdy tooKEK přístup. Knihovna Hello vyvolá hello klíč zabalení algoritmus, který zajišťuje hello KEK. Uživatelé mohou toouse vlastního zprostředkovatele pro klíče zabalení/rozbalování v případě potřeby.
4. Hello šifrovaná data se pak nahrán toohello služby Azure Storage. Hello zabalené klíč společně se některé další šifrování metadat je uložena jako metadata (na binární rozsáhlý objekt) nebo interpolované s hello zašifrovaná data (zprávy fronty a entity tabulky).

### <a name="decryption-via-hello-envelope-technique"></a>Dešifrování pomocí technik hello obálky
Dešifrování pomocí technik obálky hello funguje v hello následujícím způsobem:

1. Klientská knihovna pro Hello předpokládá, že tento uživatel hello spravuje hello klíče šifrovací klíč (KEK) místně. Hello uživatele nemusí hello tooknow konkrétní se klíč, který slouží k šifrování. Překladač klíče, který se přeloží různé klíče identifikátory tookeys, místo toho můžete nastavit a používat.
2. Klientská knihovna pro Hello stáhne hello zašifrovaná data spolu se žádné šifrování materiál, který je uložený ve službě hello.
3. Hello zabalené obsahu šifrovací klíč (CEK) je pak rozbalenou (dešifrovaný) pomocí hello klíče šifrovací klíč (KEK). Zde znovu hello klientské knihovny nemá tooKEK přístup. Vyvolá jednoduše rozbalení algoritmem hello vlastního zprostředkovatele.
4. Hello obsahu šifrovací klíč (CEK) je pak používá toodecrypt hello šifrované uživatelská data.

## <a name="encryption-mechanism"></a>Mechanismus šifrování
Klientská knihovna pro úložiště Hello používá [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) v pořadí tooencrypt uživatelská data. Konkrétně [šifrovací bloku algoritmem CBC](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) režimu pomocí standardu AES. Každý trochu jinak, služba funguje, se budeme zabývat každý z nich zde.

### <a name="blobs"></a>Objekty blob
Klientská knihovna pro Hello aktuálně podporuje šifrování pouze celý objekty BLOB. Konkrétně je podporováno šifrování, pokud uživatelé používají hello **vytvořit*** metody. Pro stahování, obě dokončení a rozsah stahování jsou podporované a paralelizace nahrávání a stahování je k dispozici.

Během šifrování se hello klientské knihovny bude generovat náhodných inicializace vektoru (IV) 16 bajtů, společně s náhodných obsahu šifrovací klíč (CEK) 32 bajtů a provádí obálky šifrování dat objektů blob hello používá tyto informace. Hello zabalená CEK a některé další šifrování metadata jsou pak uloženy jako objekt blob metadat spolu s hello zašifrovaný objekt blob ve službě hello.

> [!WARNING]
> Pokud upravujete nebo nahrát vlastní metadata pro objekt blob hello, je nutné tooensure, že se zachová, i tato metadata. Pokud nahrajete novými metadaty bez těchto metadat, hello zabalené CEK, bude ztracena IV a další metadata a obsah objektu blob hello se nikdy bude nenávratně ztracený.
> 
> 

Stahování zašifrovaný objekt blob zahrnuje načítání hello obsahu objektu blob celý hello pomocí hello **získat*** usnadňující metody. Hello zabalené CEK je úkony, spočívající a použít společně s hello IV (uložené jako metadata objektu blob v tomto případě) tooreturn hello dešifrovat data toohello uživatele.

Stahování libovolný rozsah (**získat*** předané metody s parametry rozsahu) v hello zašifrovaný objekt blob zahrnuje úpravy rozsahu hello poskytnutých uživateli v pořadí tooget malé množství další data, která lze použít dešifrování hello toosuccessfully požaduje rozsah.

Objekty BLOB bloku a objekty BLOB stránky může být pouze šifrovat nebo dešifrovat použití tohoto schématu. Aktuálně nepodporuje se pro šifrování doplňovací objekty BLOB.

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
3. Hello zabalená CEK a některé další šifrování metadata jsou pak uloženy jako dva další rezervované vlastnosti. Hello nejprve rezervované vlastnosti (\_ClientEncryptionMetadata1) se ve vlastnosti string, který obsahuje hello informace o IV, verzi a zabalené klíč. druhý vyhrazené vlastnost Hello (\_ClientEncryptionMetadata2) je binární vlastnost, která obsahuje hello informace o hello vlastnosti, které jsou zašifrované. informace v této druhé vlastnosti Hello (\_ClientEncryptionMetadata2) je sám zašifrovaná.
4. Z důvodu toothese další rezervované vlastnosti vyžadované pro šifrování uživatelé nyní mohou mít pouze 250 vlastní vlastnosti místo 252. Celková velikost Hello hello entity musí být menší než 1MB.
   
   Všimněte si, že pouze vlastnosti řetězce mohou být šifrována. Pokud jsou i další typy vlastností toobe zašifrovaná, musí být převeden toostrings. Hello šifrované řetězce jsou uložené ve službě hello jako binární vlastnosti a převedení back toostrings (nezpracovaná řetězce, není EntityProperties s typem EdmType.STRING) po dešifrování.
   
   Pro tabulky kromě toohello zásady šifrování, musí uživatelé zadat toobe vlastnosti hello zašifrovaná. To lze provést buď ukládání těchto vlastností v TableEntity objekty s hello typ sadu tooEdmType.STRING a šifrování sadu nastavení nebo tootrue encryption_resolver_function hello hello tableservice objektu. Překladač šifrování je funkce, která přebírá klíč oddílu, klíč řádku a název vlastnosti a vrátí logickou hodnotu, která určuje, jestli by se šifrovat tuto vlastnost. Během šifrování se použije hello klientské knihovny tato informace toodecide zda vlastnosti by se měla šifrovat během zápisu toohello přenosu. Delegát Hello také poskytuje možnost hello logiky kolem jak jsou zašifrované vlastnosti. (Například pokud X, potom šifrování vlastnost A; v opačném případě šifrování vlastnosti A a B.) Všimněte si, že IT oddělení není nutné tooprovide tyto informace při čtení nebo dotazování entity.

### <a name="batch-operations"></a>Dávkové operace
Jedna zásada šifrování platí tooall řádků v dávce hello. Klientská knihovna pro Hello interně vygenerujte nový náhodný IV a náhodných CEK na řádek v dávce hello. Uživatele můžete také zvolit tooencrypt různé vlastnosti pro všechny operace v dávce hello definováním toto chování v hello šifrování překladač.
Pokud dávky je vytvořen jako správce kontextu prostřednictvím metody batch() tableservice hello, zásady šifrování hello tableservice bude automaticky použité toohello batch. Dávky explicitně vytvořena voláním hello konstruktor, musí být předán zásady šifrování hello jako parametr a doleva ponechat beze změny dobu jeho existence hello hello dávky.
Všimněte si, že entit je jím zašifrovaná, jako jsou vloženy do hello batch pomocí zásad šifrování hello batch (entit je jím zašifrovaná není v době hello potvrzení hello batch pomocí zásad šifrování hello tableservice).

### <a name="queries"></a>Dotazy
operace dotazů tooperform, je nutné zadat klíče překladače, který je možné tooresolve všechny hello klíče v sadě výsledků hello. Entity obsažené ve výsledku dotazu hello nelze přeložit tooa poskytovatele, vyvolá výjimku hello klientské knihovny k chybě. Po jakémkoli dotazu, který provádí projekce straně server hello klientské knihovny přidá vlastnosti metadat speciální šifrování hello (\_ClientEncryptionMetadata1 a \_ClientEncryptionMetadata2) ve výchozím nastavení toohello vybrané sloupce .

> [!IMPORTANT]
> Mějte na paměti tyto důležité bodů při použití šifrování na straně klienta:
> 
> * Při čtení z nebo psaní tooan objektů blob, použití celý objekt blob odesílání příkazů a příkazy stažení objektů blob rozsah nebo celé šifrovaný. Vyhněte se zápis tooan zašifrovaný objekt blob pomocí protokolu operací, jako je Put bloku uvést seznam blokovaných, zápis stránky stránkách nebo zrušte; jinak může dojít k poškození hello zašifrovaný objekt blob a nastavit jej jako nečitelná.
> * Pro tabulky podobně jako omezení existuje. Být opatrní toonot aktualizace zašifrované vlastnosti bez aktualizace metadata šifrování hello.
> * Pokud jste nastavili metadata na hello zašifrovaný objekt blob, může přepsat hello metadata šifrování požadované pro dešifrování, protože není sčítání nastavení metadat. To platí také pro snímky; Vyhněte se zadání metadat při vytváření snímku zašifrovaný objekt blob. Pokud musí být nastavená metadata, zda text hello toocall být **get_blob_metadata** tooget první metoda hello aktuální metadata šifrování a provádět souběžné zápisy při nastavování metadat.
> * Povolit hello **require_encryption** příznak na hello objekt služby pro uživatele, kteří by měla fungovat jenom s šifrovaná data. Další informace najdete níže.
> 
> 

Klientská knihovna pro úložiště Hello očekává, že hello zadaný KEK a klíče překladač tooimplement hello následující rozhraní. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) podporu pro správu Python KEK čeká na vyřízení a integrují do této knihovny po dokončení.

## <a name="client-api--interface"></a>Klientské rozhraní API / rozhraní
Po vytvoření objektu služby úložiště (tj. blockblobservice) hello uživatel může přiřadit hodnoty toohello pole, které tvoří zásady šifrování: key_encryption_key, key_resolver_function a require_encryption. Uživatelé zadat pouze KEK pouze překladač nebo obojí. key_encryption_key je hello základní typ klíče, je identifikován pomocí identifikátoru klíče a zabalení/rozbalování poskytuje logiku hello. key_resolver_function je použité tooresolve klíč během dešifrování hello. Vrátí platný KEK zadaný identifikátor klíče. To poskytuje uživatelům hello možnost toochoose mezi více klíčů, které jsou spravovány v několika umístěních.

Hello KEK musí implementovat následující hello toosuccessfully metody šifrování dat:

* wrap_key(cek): zabalí hello zadaný CEK (bajty) pomocí algoritmu podle volby uživatele hello. Vrátí hello zabalená klíč.
* get_key_wrap_algorithm(): vrátí hello algoritmus používaný toowrap klíče.
* get_kid(): vrátí hello řetězec id klíče pro tento KEK.
  Hello KEK musí implementovat hello následující metody toosuccessfully dešifrování dat:
* unwrap_key (cek, algoritmus): vrátí hello úkony, spočívající formu hello zadaný CEK pomocí algoritmu zadaný řetězec hello.
* get_kid(): vrací řetězec id klíče pro tento KEK.

překladač klíče Hello alespoň musí implementovat metodu, která zadané id klíče, vrátí hello odpovídající KEK implementující hello rozhraní výše. Jenom tato metoda je toobe přiřazené toohello key_resolver_function vlastnost v objektu služby hello.

* Pro šifrování vždy používá klíč hello a hello absenci klíč bude výsledkem chyba.
* K dešifrování:
  
  * překladač klíče Hello je volána, pokud zadaný klíč tooget hello. Pokud je zadaný překladač hello, ale nemá mapování pro identifikátor hello klíče, je vržena chyba.
  * Pokud není zadaný překladač, ale je určen klíč, hello klíč se používá, pokud jeho identifikátoru odpovídá identifikátoru klíče hello vyžaduje. Pokud hello identifikátor neodpovídá, je vyvolána k chybě.
    
    Hello ukázky šifrování v azure.storage.samples <fix URL>předvádí podrobnější scénář začátku do konce pro objekty BLOB, fronty a tabulky.
      Ukázka implementace hello KEK a klíče překladač jsou uvedeny v hello ukázkové soubory jako KeyWrapper a KeyResolver v uvedeném pořadí.

### <a name="requireencryption-mode"></a>Režim RequireEncryption
Uživatelé mohou volitelně povolit režim operace, kde musí být všechny nahrávání a stahování zašifrována. V tomto režimu se nezdaří pokusy o tooupload dat bez šifrování zásad nebo stažení data, která nejsou šifrována hello služby v klientovi hello. Hello **require_encryption** příznak v ovládacích prvcích objektu služby hello na toto chování.

### <a name="blob-service-encryption"></a>Šifrování služby objektů BLOB
Pole zásad šifrování hello nastavit u objektu blockblobservice hello. Všem ostatním bude zpracovávat hello klientské knihovny interně.

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set hello KEK and key resolver on hello service object.
my_block_blob_service.key_encryption_key = kek
my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

# Upload hello encrypted contents toohello blob.
my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

# Download and decrypt hello encrypted contents from hello blob.
blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)
```

### <a name="queue-service-encryption"></a>Šifrování služby fronty
Pole zásad šifrování hello nastavit u objektu queueservice hello. Všem ostatním bude zpracovávat hello klientské knihovny interně.

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set hello KEK and key resolver on hello service object.
my_queue_service.key_encryption_key = kek
my_queue_service.key_resolver_funcion = key_resolver.resolve_key

# Add message
my_queue_service.put_message(queue_name, content)

# Retrieve message
retrieved_message_list = my_queue_service.get_messages(queue_name)
```

### <a name="table-service-encryption"></a>Šifrování služby Table
Kromě toho toocreating zásady šifrování a jeho nastavení na žádost o možnostech, musíte buď zadat **encryption_resolver_function** na hello **tableservice**, nebo sadu hello šifrování atributu na Hello EntityProperty.

### <a name="using-hello-resolver"></a>Pomocí překladače hello

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Define hello encryption resolver_function.
def my_encryption_resolver(pk, rk, property_name):
        if property_name == 'foo':
                return True
        return False

# Set hello KEK and key resolver on hello service object.
my_table_service.key_encryption_key = kek
my_table_service.key_resolver_funcion = key_resolver.resolve_key
my_table_service.encryption_resolver_function = my_encryption_resolver

# Insert Entity
my_table_service.insert_entity(table_name, entity)

# Retrieve Entity
# Note: No need toospecify an encryption resolver for retrieve, but it is harmless tooleave hello property set.
my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])
```

### <a name="using-attributes"></a>Pomocí atributů
Jak je uvedeno nahoře, může být vlastnost označené pro šifrování ukládáním do objektu EntityProperty a nastavení hello šifrování pole.

```python
encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)
```

## <a name="encryption-and-performance"></a>Šifrování a výkonu
Všimněte si, že šifrování dat výsledky úložiště v dalších zatížení. Hello obsahu musí být generovány klíč a IV, musí být šifrovaný samotný obsah hello a další metadata musí být naformátovaná a nahrát. Tato dodatečná režie se bude lišit v závislosti na objemu dat šifrovaný hello. Doporučujeme vám, že zákazníci vždy testování aplikací pro výkon při vývoji.

## <a name="next-steps"></a>Další kroky
* Stáhnout hello [Klientská knihovna pro úložiště Azure pro úložiště PyPi Java balíček](https://pypi.python.org/pypi/azure-storage)
* Stáhnout hello [Klientská knihovna pro úložiště Azure pro jazyk Python zdrojového kódu z Githubu](https://github.com/Azure/azure-storage-python)
