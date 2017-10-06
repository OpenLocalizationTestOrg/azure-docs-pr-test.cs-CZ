---
title: "aaaMicrosoft šifrování na Rest Azure dat | Microsoft Docs"
description: "Tento článek obsahuje přehled Microsoft Azure šifrování dat na rest, hello celkové možností a aspektů Obecné."
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: yurid
ms.openlocfilehash: d74d103e4fd7585543b4f039877af7d742a6b2e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-encryption-at-rest"></a>Šifrování na Rest Azure dat
Existuje několik nástrojů v rámci Microsoft Azure toosafeguard data podle potřeby tooyour společnosti, zabezpečení a dodržování předpisů. Tento dokument se zaměřuje na jak dat je chráněn v klidovém stavu uložených v Microsoft Azure, popisuje různé součásti, které se účastní implementace ochrany dat hello hello a zkontroluje výhody a nevýhody hello jinou správu klíčů ochrany přístupů. 

Šifrování v klidovém stavu je běžné požadavek na zabezpečení. Výhodou Microsoft Azure je, že organizace můžete dosáhnout šifrování v klidovém stavu bez nutnosti hello náklady na implementaci a správu a hello riziko řešení pro správu vlastní klíče. Organizace mají možnost hello umožníte Azure, úplně spravovat šifrování v klidovém stavu. Kromě toho organizace mají různé možnosti tooclosely spravovat šifrování nebo šifrovací klíče.

## <a name="what-is-encryption-at-rest"></a>Co je šifrování v klidovém stavu?
Šifrování v klidovém stavu odkazuje toohello kryptografických kódování (šifrování) dat pokud je nastavené jako trvalé. Hello šifrování v návrzích Rest v Azure použijte tooencrypt symetrického šifrování a dešifrování velké objemy dat rychle podle tooa jednoduché konceptuálního modelu:

- Symetrický šifrovací klíč je použité tooencrypt data, jako je trvalé 
- Hello stejný šifrovací klíč je použité toodecrypt dat, jako je readied pro použití v paměti
- Data mohou být rozdělena na oddíly a může být používají různé klíče pro každý oddíl
- Klíče musí být uložen v zabezpečeném umístění, s omezením přístupu toocertain identity a použití klíče protokolování zásady řízení přístupu. Šifrovací klíče dat jsou často šifrován asymetrické šifrování toofurther omezení přístupu (popsané v hello *klíč hierarchie*dál v tomto článku)

Hello výše popisuje hello běžné základní prvky šifrování v klidovém stavu. V praxi klíčové scénáře pro správu a řízení a také škálování a dostupnosti záruky, vyžadují další konstrukce. Microsoft Azure šifrování v Rest konceptech a součástech jsou popsané níže.

## <a name="hello-purpose-of-encryption-at-rest"></a>účelem Hello šifrování v klidovém stavu
Šifrování v klidovém stavu je určený tooprovide ochrana dat pro data v rest (jak je popsáno výše.) Útoky na data v rest zahrnout pokusy o tooobtain fyzický přístup toohello hardware, na které hello jsou uložena data, a pak ohrožení hello obsahovala data. Takový útok pevný disk server může mít byla manipulace během údržby umožní útočníkovi tooremove hello pevný disk. Novější útočník hello přepnutím hello pevný disk do počítače v rámci své řídicí tooattempt tooaccess hello data. 

Šifrování v klidovém stavu je navrženou tooprevent hello útočník z přístup k datům hello bez šifrování zajištěním hello data se šifrují, pokud na disku. Pokud útočník byly tooobtain pevný disk s takovým šifrování dat a žádný přístup toohello šifrovacích klíčů, hello útočník by ohrožení dat hello bez velké problémy. V takovém scénáři útočník by mohl tooattempt útoky na šifrovaná data, která jsou mnohem složitější a využívání prostředků než přístup k bez šifrování dat na pevném disku. Z tohoto důvodu šifrování v klidovém stavu se důrazně doporučuje a je vyžadováno pro mnoho společností s vysokou prioritou. 

V některých případech šifrování v klidovém stavu vyžaduje i organizace potřebu úsilí data vedení a dodržování předpisů. Předpisy odvětví a government, jako je HIPAA, PCI a FedRAMP a mezinárodní zákonné požadavky, rozvrhněte konkrétní bezpečnostní opatření prostřednictvím zásady týkající se požadavků na ochranu a šifrování dat a procesy. Pro mnoho těchto nařízení šifrování v klidovém stavu je povinné míra vyžadované pro správu dat odpovídající a ochrany. 

Kromě toho toocompliance a zákonné požadavky, šifrování v klidovém stavu by měl považován za schopností obrany do hloubky platformy. I když společnost Microsoft poskytuje kompatibilní platformu pro služby, aplikace a data, komplexní zařízením a fyzického zabezpečení dat řízení přístupu a auditování, je důležité tooprovide další "překrývající se" bezpečnostní opatření v případě mezi hello Další bezpečnostní opatření selže. Šifrování v klidovém stavu poskytuje tyto další mechanismus obrany.

Společnost Microsoft se potvrdit tooproviding šifrování v možnosti rest napříč cloudové služby a šifrovacích klíčů a toologs přístup k zobrazení, pokud se používá šifrovací klíče tooprovide možnosti správy vhodný zákazníků. Kromě toho společnost Microsoft pracuje vůči cíli hello převedení všechny zákaznická data zašifrovaná přinejmenším ve výchozím nastavení.

## <a name="azure-encryption-at-rest-components"></a>Azure šifrování v součásti Rest

Jak je popsáno dříve, hello cílem šifrování v klidovém stavu je, že data, která je trvalé na disku je šifrován tajný šifrovací klíč. tooachieve této cílem zabezpečit vytvoření klíče, úložiště, řízení přístupu a správu hello šifrování, které je třeba zadat klíče. I když podrobnosti se může lišit, Azure services šifrování na Rest implementace lze popsat z hlediska hello níže koncepty, které jsou pak zobrazené hello následující diagram.

![Komponenty](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

umístění úložiště Hello hello šifrovacích klíčů a klíče toothose řízení přístupu je centrální tooan šifrování v modelu rest. Hello klíče potřebovat toobe vysoce zabezpečená ale spravovat určenými uživateli a k dispozici toospecific služby. Pro služby Azure Azure Key Vault je hello doporučené řešení úložiště klíčů a poskytuje společné rozhraní pro správu v rámci služby. Klíče jsou uložené a spravovat v trezorů klíčů a klíče trezoru tooa přístup je možné přidělit toousers nebo služby. Azure Key Vault podporuje vytvoření odběratele klíčů nebo import klíče zákazníků v situacích spravované zákazníkem šifrovací klíče.

### <a name="azure-active-directory"></a>Azure Active Directory

Oprávnění toouse hello klíče uložené v Azure Key Vault, toomanage nebo tooaccess je pro šifrování na Rest šifrování a dešifrování, je možné přidělit tooAzure účtů služby Active Directory. 

### <a name="key-hierarchy"></a>Klíče hierarchie

Obvykle více než jeden šifrovací klíč se používá v šifrování v implementaci rest. Asymetrické šifrování je užitečné pro navázání vztahu důvěryhodnosti hello a ověřování pro přístup ke klíčům a správu potřeba. Symetrické šifrování je efektivnější pro hromadné šifrování a dešifrování, povolení pro silnější šifrování a lepší výkon. Kromě toho omezení použití hello jediný šifrovací klíče snížení hello riziko, že hello klíč bude dojde k ohrožení a hello náklady znova šifrovat, když klíč se musí nahradit. výhody hello tooleverage asymetrické symetrického šifrování a použití hello limit a ohrožení jeden klíč, použijte Azure šifrování v rest modely klíče hierarchie skládá z hello následující typy klíčů:

- **Datový šifrovací klíč (DEK)** – symetrického klíče AES256 používá tooencrypt oddílu nebo datového bloku.  Jediný zdroj, může mít mnoho oddíly a mnoho šifrovací klíče dat. Šifrování každého bloku dat pomocí jiného klíče umožňuje analysis kryptografických útoků obtížnější. TooDEKs přístupu je potřeba hello prostředků zprostředkovatele nebo aplikace instanci, která je šifrování a dešifrování konkrétní blok. Při klíč DEK se nahradí nový klíč pouze hello data v jeho přidružené bloku musí být znovu zašifrovat s hello nový klíč.
- **Šifrovací klíč klíčů (KEK)** – tooencrypt hello šifrovací klíče dat používá asymetrické šifrovací klíč. Použití klíče šifrovací klíč umožňuje hello šifrovací klíče dat, sami toobe šifrované a řídí. Hello entity, který má přístup toohello KEK může být jiný než hello entita, která vyžaduje hello klíč DEK. To umožňuje entity toobroker přístup toohello klíč DEK hello za účelem zajištění omezený přístup každý oddíl toospecific klíč DEK. Vzhledem k tomu, že hello KEK je požadovaná toodecrypt hello DEKs, je jediný bod, podle kterého DEKs může efektivně odstranit odstranění hello KEK efektivně hello KEK.

Šifrovací klíče dat Hello, šifrován hello klíč šifrovací klíče jsou uloženy odděleně a pouze entity s toohello přístupu, které klíč šifrovacího klíče můžete získat žádné šifrovací klíče dat šifrována s tímto klíčem. Podporovány jsou různé modely úložiště klíčů. Každý model později v další části hello podrobněji se budeme zabývat.

## <a name="data-encryption-models"></a>Modely šifrování dat

Principy hello jednotlivých šifrování modelů a jejich výhody a nevýhody je nezbytné, pro pochopení jak hello různých poskytovatelů prostředků v Azure implementace šifrování v klidovém stavu. Tyto definice jsou sdílené mezi všech poskytovatelů prostředků v Azure tooensure běžné jazyk a taxonomii. 

Existují tři scénáře pro šifrování na straně serveru:

- Šifrování na straně serveru pomocí služby spravovat klíče
    - Azure zprostředkovatelé prostředků provádět operace šifrování a dešifrování hello
    - Microsoft spravuje hello klíče
    - Funkce úplné cloudu

- Šifrování na straně serveru pomocí spravovaného zákazníkem klíčů v Azure Key Vault
    - Azure zprostředkovatelé prostředků provádět operace šifrování a dešifrování hello
    - Zákazník ovládací prvky klíče přes Azure Key Vault
    - Funkce úplné cloudu

- Šifrování na straně serveru pomocí klíče spravovaného zákazníkem na řídí hardwaru
    - Azure zprostředkovatelé prostředků provádět operace šifrování a dešifrování hello
    - Ovládací prvky klíče zákazníků na zákazníka řídí hardwaru
    - Funkce úplné cloudu

Šifrování na straně klienta zvažte následující hello:

- Služby Azure nemůžete zobrazit dešifrovaná data
- Zákazníci správě a ukládání klíčů v místě (nebo v jiné zabezpečené úložiště). Klíče nejsou k dispozici tooAzure služby
- Funkce snížené cloudu

šifrování Hello podporované modely v Azure rozdělit do dvou hlavních skupin: "Klienta šifrování" a "serverové šifrování", jako jsou již bylo zmíněno dříve. Všimněte si, že nezávislé hello šifrování ve rest model použité, Azure služby vždy doporučujeme hello použití zabezpečeného přenosu například TLS nebo HTTPS. Šifrování v přenosu proto, mělo by se řešit pomocí hello přenosový protokol a by neměla být hlavní faktorem při určování, které šifrování v rest model toouse.

### <a name="client-encryption-model"></a>Model klientského šifrování

Modelu šifrování klienta odkazuje tooencryption, které se provádí mimo hello poskytovatele prostředků nebo Azure pomocí služby hello nebo volající aplikace. šifrování Hello lze provést hello služby aplikací v Azure, nebo aplikace běžící v datovém centru hello zákazníka. V obou případech, při využití tohoto modelu šifrování hello poskytovatel prostředků Azure obdrží zašifrovaný objekt blob dat bez hello možnost toodecrypt hello data žádným způsobem nebo mít přístup toohello šifrovací klíče. V tomto modelu správy klíčů hello se provádí hello volání aplikace nebo služby a je zcela neprůhledný toohello služby Azure.

![Klient](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>Model šifrování na straně serveru

Modely šifrování na straně serveru naleznete v nápovědě tooencryption, které se provádí pomocí hello služby Azure. V tomto modelu jsou hello poskytovatele prostředků provede hello šifrování a dešifrování operace. Například Azure Storage data může zobrazit v prostém textu operacích a provede hello šifrování a dešifrování interně. Hello poskytovatele prostředků může použít šifrovací klíče, které jsou spravované společností Microsoft nebo zákazníkem hello v závislosti na hello zadané konfigurace. 

![Server](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Modely šifrování na straně serveru správy klíčů

Každý z šifrování na straně serveru hello na rest modely znamená odlišné charakteristiky správy klíčů. To zahrnuje kde a jak šifrovací klíče jsou vytvořeny a uloženy jako i jako hello přístup modely a hello postupy střídání klíče. 

#### <a name="server-side-encryption-using-service-managed-keys"></a>Šifrování na straně serveru pomocí služby spravovat klíče

Pro mnoho zákazníků základní požadavek hello je tooensure, který hello data se šifrují vždy, když je v klidovém stavu. Šifrování na straně serveru pomocí služby spravovat klíče umožňuje tento model umožňující zákazníkům toomark hello konkrétní prostředek (účet úložiště, databáze SQL atd.) pro šifrování a ponechat všechny aspekty správy klíčů, jako je například klíče vystavování, otáčení a zálohování tooMicrosoft. Většina služeb Azure, která podporují šifrování v klidovém stavu obvykle podporují tento model zpracování úloh hello správu hello šifrovací klíče tooAzure. Poskytovatel prostředků Azure Hello vytvoří hello klíče, umístí je do zabezpečeného úložiště a načte je podle potřeby. To znamená, že služba hello má úplný přístup toohello klíče a hello služby má plnou kontrolu nad Správa životního cyklu hello přihlašovacích údajů.

![Spravované](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig4.png)

Šifrování na straně serveru pomocí služby spravovat klíče proto rychle řeší hello šifrování nutné toohave v klidovém stavu s nízkou režijní toohello zákazníka. Pokud je k dispozici zákazník obvykle otevře hello portál Azure pro hello cílového předplatného a zprostředkovatele prostředků a zkontroluje pole, která určuje, že mu toobe hello data zašifrovaná. V některých šifrování na straně serveru správci prostředků službou je ve výchozím nastavení spravovaných klíčů. 

Šifrování na straně serveru s klíči spravovaný Microsoftem implikují hello služby má úplný přístup toostore a spravuje hello klíče. Když někteří zákazníci chtít toomanage hello klíče, protože jejich pocit, že se můžete zajistit vyšší úroveň zabezpečení, hello náklady a riziko spojené s řešením vlastní úložiště klíčů měli zvážit při vyhodnocování tohoto modelu. V mnoha případech může organizace určit omezení prostředků nebo rizika v případě místních řešení může větší než riziko hello správy cloudu hello šifrování na rest klíče.  Však tento model nemusí být dostatečné pro organizace, které mají požadavky toocontrol hello vytvoření nebo životního cyklu hello šifrovací klíče nebo jiné pracovníky toohave Spravovat šifrovací klíče služby než ty, které Správa služby hello (tj.), oddělení správy klíčů z hello celkové model správy pro službu hello).

##### <a name="key-access"></a>Přístup ke klíčům

Pokud se používá šifrování na straně serveru s klíči službu spravovat, hello vytvoření klíče, úložiště a služby přístupu jsou spravovány službou hello. Zprostředkovatelé prostředků základní Azure hello obvykle uloží šifrovací klíče dat hello v úložišti, které je zavřete toohello dat a rychle k dispozici a dostupné při hello klíč šifrovací klíče jsou uloženy v zabezpečené interní úložiště.

**Výhody**

- Jednoduché nastavení
- Spravuje Microsoft střídání klíče, zálohování a redundance
- Zákazník nemá hello náklady na implementaci nebo hello riziko schéma vlastní správy klíčů.

**Nevýhody**

- Žádné zákazníka kontrolu nad hello šifrovací klíče (specifikace klíče, životního cyklu, odvolání, atd.)
- Žádná možnost toosegregate správy klíčů z celkové model správy pro službu hello

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Šifrování na straně serveru pomocí klíčů zákazníků spravované v Azure Key Vault 

Pro scénáře, kde hello požadavek je tooencrypt hello dat v klidovém stavu a řízení hello šifrovací klíče zákazníci mohou používat šifrování na straně serveru pomocí klíčů zákazníků spravované v Key Vault. Některé služby uložit jenom hello kořenový klíč šifrovacího klíče v Azure Key Vault a úložiště hello zašifrován datový šifrovací klíč v datech toohello blíže k interní umístění. V, aby zákazníci scénáři můžete zahrnout vlastní klíče tooKey trezoru (BYOK – přineste si vlastní klíč), nebo generovat nové a používat je tooencrypt hello potřeby prostředky. Během operace šifrování a dešifrování hello provádí hello poskytovatele prostředků používá klíč hello nakonfigurované jako hello kořenový klíč pro všechny operace šifrování. 

##### <a name="key-access"></a>Přístup ke klíčům

Hello modelu šifrování na straně serveru pomocí klíčů zákazníků spravované v Azure Key Vault zahrnuje hello služby přístupu k hello klíče tooencrypt a dešifrování podle potřeby. Šifrování klíče rest jsou vytvářeny přístupné tooa služby pomocí zásad řízení přístupu udělení služba identity přístup tooreceive hello klíči. Služba Azure systémem jménem přidružené předplatné se dá nakonfigurovat s identitou pro služby v rámci tohoto předplatného. Hello služby můžete provádět ověřování Azure Active Directory a přijímat ověřovací token identifikace sám sebe jako služby funguje jménem hello předplatného. Pak se vidění tooKey trezoru tooobtain klíč ho nebyla zadána přístup k může tento token.

Pro operace pomocí šifrovacích klíčů, lze udělit identita služby access tooany Dobrý den následující operace: dešifrovat, šifrování, unwrapKey, wrapKey, ověřte, přihlášení, získání, seznam, aktualizace, vytvořit, importovat, odstranit, zálohování a obnovení.

tooobtain klíč pro použití v šifrování nebo dešifrování dat identitu služby rest hello této hello Resource Manager instance služby se spustí jako musí mít UnwrapKey (tooget hello klíč pro dešifrování) a WrapKey (tooinsert klíč do trezoru klíčů při vytváření nového klíče).


>[!NOTE] 
>Další podrobnosti o Key Vault autorizace naleznete v části hello zabezpečení stránku trezoru klíčů v hello [dokumentace Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault). 

**Výhody**

- Plnou kontrolu nad klíče hello používá – šifrovací klíče se spravují v Key Vault hello zákazníka pod kontrolou hello zákazníka.
- Možnost tooencrypt více tooone hlavní služby
- Můžete oddělit správu klíčů z celkové model správy pro službu hello
- Můžete definovat služby a umístění klíče v oblastech

**Nevýhody**

- Zákazník má plnou odpovědnost za správu klíčů přístupu
- Zákazník má plnou odpovědnost za správu životního cyklu u klíče
- Další režii instalace a konfigurace

#### <a name="server-side-encryption-using-service-managed-keys-in-customer-controlled-hardware"></a>Šifrování na straně serveru pomocí služby spravovat klíče v řídí hardwaru

Některé služby Azure pro scénáře, kde hello požadavek tooencrypt hello dat v klidovém stavu a správu klíčů hello proprietární úložiště mimo kontrolu společnosti Microsoft, povolit model správy klíčů hello hostitele si vlastní klíč (HYOK). V tomto modelu hello služba musí načíst klíč hello z externího webu a proto dopad na výkon a dostupnost záruky a konfigurace je složitější. Navíc vzhledem k tomu, že služba hello mít přístup toohello klíč DEK během hello šifrování a dešifrování operations hello celkové záruky zabezpečení tohoto modelu jsou podobné hello toowhen, které jsou klíče zákazníků spravované v Azure Key Vault.  Tento model v důsledku toho není vhodné pro většinu organizací, pokud mají očekávání se požadavky na konkrétní správy klíčů. Většina služeb Azure z důvodu omezení toothese nepodporují šifrování na straně serveru pomocí serveru spravované klíčů v řídí hardwaru.

##### <a name="key-access"></a>Přístup ke klíčům

Pokud se používá šifrování na straně serveru pomocí služby spravovat klíčů v řídí hardwaru hello klíče udržované systému nakonfiguroval hello zákazníka. Služby Azure, které podporují tento model poskytují úložiště klíčů zadaný prostředek pro zákazníka tooa zabezpečené připojení.

**Výhody**

- Plnou kontrolu nad hello kořenový klíč používá – šifrovací klíče se spravují přes zákazníka poskytuje úložiště
- Možnost tooencrypt více tooone hlavní služby
- Můžete oddělit správu klíčů z celkové model správy pro službu hello
- Můžete definovat služby a umístění klíče v oblastech

**Nevýhody**

- Plnou odpovědnost za úložiště klíčů, zabezpečení, výkonu a dostupnosti
- Plnou odpovědnost za správu klíčů přístupu
- Plnou odpovědnost za správu životního cyklu u klíče
- Významné instalaci, konfiguraci a náklady na průběžnou údržbu
- Zvýšená závislost na dostupnost sítě mezi datovým centrem zákazníka hello a datových centrech Azure.

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>Šifrování v klidovém stavu uložených v cloudové služby společnosti Microsoft

Cloudové služby společnosti Microsoft se používají v všechny tři cloudu modely: IaaS, PaaS, SaaS. Zde máte příklady, jak se vešly na každý model:

- Software služby, označují tooas softwaru jako Server nebo SaaS, které aplikace poskytované hello cloudu, např. Office 365.
- Služby platformy uvedeni zákazníci, kteří využívají cloud hello ve svých aplikacích pomocí hello cloudu pro věci jako funkce úložiště, analýzy a service bus.
- Služby infrastruktury, nebo infrastruktura jako služba (IaaS) v kterého odběratele nasazení operačních systémů a aplikací, které jsou hostované v hello cloudu a může být využívat jiné cloudové služby.

### <a name="encryption-at-rest-for-saas-customers"></a>Šifrování v klidovém stavu pro zákazníky SaaS

Software jako služba (SaaS) Zákazníci obvykle třeba šifrování v klidovém stavu povolený nebo k dispozici v jednotlivých služeb. Služby Office 365 obsahuje celou řadu možností pro zákazníky tooverify nebo povolit šifrování v klidovém stavu. Informace o službách Office 365 najdete v části technologií pro šifrování dat pro Office 365.

### <a name="encryption-at-rest-for-paas-customers"></a>Šifrování v klidovém stavu pro zákazníky PaaS

Platforma jako služba (PaaS) zákazníka data se obvykle nachází v prostředí pro spuštění aplikace a všech poskytovatelů prostředků Azure používat toostore zákaznická data. šifrování hello toosee rest možnosti k dispozici tooyou zkontrolujte hello tabulce pro platformy hello úložiště a aplikace, které používáte. Pokud je podporováno, tooinstructions odkazy na povolení šifrování v klidovém stavu jsou uvedené pro každý poskytovatel prostředků. 

### <a name="encryption-at-rest-for-iaas-customers"></a>Šifrování v klidovém stavu pro zákazníky IaaS

Infrastruktura jako služba (IaaS) zákazníků může mít celou řadu služeb a aplikací používá. Můžete povolit služby IaaS šifrování v klidovém stavu uložených v jejich Azure hostovaných virtuálních počítačů a virtuálních pevných disků pomocí Azure Disk Encryption. 

#### <a name="encrypted-storage"></a>Šifrované úložiště

Jako PaaS můžete využít řešení IaaS jinými službami Azure, které ukládají data zašifrovaná přinejmenším. V těchto případech můžete povolit hello šifrování prostřednictvím podpory Rest podle jednotlivých spotřebované Azure službou. Vytvoří výčet Hello níže uvedená tabulka, hello hlavní úložiště, služby a aplikace platformy a hello model šifrování v klidovém stavu podporovány. Pokud je podporováno, jsou k dispozici odkazy tooinstructions k povolení šifrování v klidovém stavu. 

#### <a name="encrypted-compute"></a>Šifrované výpočetní

Dokončení šifrování v Rest řešení vyžaduje tento hello data nikdy přetrvají v nezašifrované podobě. Protože se používá, na server hello načítání, které data v paměti, data lze zachovat místně různými způsoby, včetně stránkovací soubor Windows hello, stav systému a všechny hello protokolování aplikace mohou provádět. Tato data se šifrují tooensure v klidovém stavu IaaS aplikace můžete použít Azure Disk Encryption na virtuálním počítači Azure IaaS (Windows nebo Linux) a virtuální disk. 

#### <a name="custom-encryption-at-rest"></a>Vlastní šifrování v klidovém stavu

Doporučuje se, že pokud je to možné, IaaS aplikace využívat Azure Disk Encryption a šifrování na Rest možnosti poskytované spotřebované služby Azure. V některých případech například požadavky na šifrování nestandardní nebo úložiště na bázi mimo Azure, vývojář aplikace IaaS může být nutné tooimplement šifrování v rest sami. Vývojáři mohou lepší řešení IaaS Azure správy a k zákaznickým očekávání integrovat s využitím určité Azure komponenty. Konkrétně vývojáři využít hello Azure Key Vault tooprovide služby Zabezpečené úložiště klíčů a také svým zákazníkům poskytovali konzistentní správu klíčů možnosti u nejvíce Azure služby platformy. Kromě toho by měl vlastních řešení pomocí identita spravované služby Azure tooenable služby účty tooaccess šifrovací klíče. Informace pro vývojáře Azure Key Vault a identita spravované služby najdete v jejich příslušné sady SDK.

## <a name="azure-resource-providers-encryption-model-support"></a>Podpora modelu šifrování zprostředkovatelé prostředků Azure.

Služby Microsoft Azure každý podporují jeden nebo více hello šifrování v modelech rest. U některých služeb ale jeden nebo více modelů hello šifrování nemusí být použitelná. Kromě toho mohou služby uvolnit podporu pro tyto scénáře na různé plány. Tato část popisuje hello šifrování prostřednictvím podpory rest v době psaní tohoto textu pro jednotlivé služby úložiště dat hlavní Azure hello hello.

### <a name="azure-disk-encryption"></a>Azure disk encryption

Každý zákazník pomocí Azure infrastruktury jako služby (IaaS) funkce můžete dosáhnout šifrování v klidovém stavu pro své virtuální počítače IaaS a disky prostřednictvím Azure Disk Encryption. Další informace o Azure Disk encryption najdete v části hello [dokumentace Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

#### <a name="azure-storage"></a>Úložiště Azure

Azure Blob a soubor podporuje šifrování v klidovém stavu pro scénáře šifrované na straně serveru, jakož i data zákazníků šifrovat (šifrování na straně klienta).

- Serverové: zákazníky používající úložiště objektů blob v Azure můžete povolit šifrování v klidovém stavu u každého účtu prostředků úložiště Azure. Jednou povolené šifrování na straně serveru se transparentně provádí toohello aplikace. V tématu [šifrování služby úložiště Azure pro Data v klidovém stavu](https://docs.microsoft.com/azure/storage/storage-service-encryption) Další informace.
- Klienta: je podporováno šifrování na straně klienta objektů BLOB Azure. Když pomocí šifrování na straně klienta zákazníci šifrovat hello data a nahrajte hello data jako zašifrovaný objekt blob. Správu klíčů je potřeba hello zákazníka. V tématu [šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) Další informace.


#### <a name="sql-azure"></a>SQL Azure

SQL Azure aktuálně podporuje šifrování v klidovém stavu straně Microsoft spravované služby a scénáře šifrování na straně klienta.

Sever podporu pro šifrování je nyní k dispozici prostřednictvím hello funkci SQL transparentní šifrování dat. Jakmile zákazník SQL Azure umožňuje šifrování TDE klíč se automaticky vytváří a spravují pro ně. Na úrovních hello databáze a serveru lze povolit šifrování v klidovém stavu. Od června 2017 [transparentní šifrování šifrování dat (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) bude povolená ve výchozím nastavení pro nově vytvořené databáze.

Šifrování na straně klienta dat SQL Azure je podporováno prostřednictvím hello [vždy šifrována](https://msdn.microsoft.com/library/mt163865.aspx) funkce. Vždy používá šifrovaný klíč, který vytvoří a uloží klientem hello. Zákazníci může ukládat hello hlavní klíč do úložiště certifikátů systému Windows, Azure Key Vault nebo místního modulu hardwarového zabezpečení. Pomocí SQL Server Management Studio, SQL uživatelé zvolit, jaké klíče se mu toouse tooencrypt sloupec.

|                                  |                |                     | **Šifrování modelu**             |                              |        |
|----------------------------------|----------------|---------------------|------------------------------|------------------------------|--------|
|                                  |                |                     |                              |                              | **Klienta** |
|                                  | **Správa klíčů** | **Službu spravovat klíče** | **Zákazník spravované v trezoru klíčů** | **Zákazník spravované na místě** |        |
| **Úložiště a databáze**            |                |                     |                              |                              |        |
| Disk (IaaS)                      |                | -                   | Ano                          | Ano*                         | -      |
| Systému SQL Server (IaaS)                |                | Ano                 | Ano                          | Ano                          | Ano    |
| Azure SQL (PaaS)                 |                | Ano                 | Preview                      | -                            | Ano    |
| Úložiště Azure (objekty BLOB bloku nebo stránky) |                | Ano                 | Preview                      | -                            | Ano    |
| Úložiště Azure (soubory)            |                | Ano                 | -                            | -                            | -      |
| Úložiště Azure (tabulky, fronty)   |                | -                   | -                            | -                            | Ano    |
| Cosmos DB (dokument DB)          |                | Ano                 | -                            | -                            | -      |
| StorSimple                       |                | Ano                 | -                            | -                            | Ano    |
| Zálohování                           |                | -                   | -                            | -                            | Ano    |
| **Intelligence a analýzy**       |                |                     |                              |                              |        |
| Azure Data Factory               |                | Ano                 | -                            | -                            | -      |
| Azure Machine Learning           |                | -                   | Preview                      | -                            | -      |
| Azure Stream Analytics           |                | Ano                 | -                            | -                            | -      |
| HDInsights (úložiště objektů Blob v Azure)  |                | Ano                 | -                            | -                            | -      |
| HDInsights (Data Lake Storage)   |                | Ano                 | -                            | -                            | -      |
| Azure Data Lake Store            |                | Ano                 | Ano                          | -                            | -      |
| Katalog dat Azure               |                | Ano                 | -                            | -                            | -      |
| Power BI                         |                | Ano                 | -                            | -                            | -      |
| **Služby IoT**                     |                |                     |                              |                              |        |
| IoT Hub                          |                | -                   | -                            | -                            | Ano    |
| Service Bus                      |                | -              | -                            | -                            | Ano    |
| Event Hubs                       |                | -             | -                            | -                            | -      |


## <a name="conclusion"></a>Závěr

Ochrana dat zákazníků ukládaných v rámci služby Azure se z tooMicrosoft prvořadý význam. Všechny Azure hostované služby jsou potvrzené tooproviding šifrování v možnosti Rest. Základní služby, jako je například Azure Storage, SQL Azure a klíče analýzy a intelligence služby již zadat šifrování v možnosti Rest. Některé z těchto služeb podporují zákazníka řídí klíčů a šifrování na straně klienta a také služby spravovaných klíčů a šifrování. Nové možnosti plánováno pro verzi preview a obecné dostupnosti v nadcházejících měsících hello a služby Microsoft Azure jsou široce rozšíření šifrování v dostupnosti Rest.

