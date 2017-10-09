---
title: "aaaCryptography - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: cab981bf116a0e76bbf44fe0f0a1a3650e4ab0f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-cryptography--mitigations"></a>Rámce zabezpečení: Kryptografie | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Webové aplikace** | <ul><li>[Používejte pouze šifry schválené symetrický bloku a délky klíčů](#cipher-length)</li><li>[Použití schváleno režimy šifrování bloku a inicializační vektory pro symetrické šifry](#vector-ciphers)</li><li>[Použít schválené asymetrických algoritmů, délek klíčů a odsazení](#padding)</li><li>[Použití schváleno generátory náhodných čísel](#numgen)</li><li>[Nepoužívejte šifry symetrický datového proudu](#stream-ciphers)</li><li>[Použití schváleno MAC, HMAC/keyed algoritmů hash](#mac-hash)</li><li>[Použít pouze funkce schválené kryptografické hodnoty hash](#hash-functions)</li></ul> |
| **Database** | <ul><li>[Používat silné šifrování algoritmy tooencrypt data v databázi hello](#strong-db)</li><li>[Balíčky SSIS by měl být zašifrované a digitálně podepsané](#ssis-signed)</li><li>[Přidat zabezpečitelné prostředky databáze toocritical digitální podpis](#securables-db)</li><li>[Použití SQL serveru EKM tooprotect šifrovací klíče](#ekm-keys)</li><li>[Pokud šifrovacích klíčů by neměl být modul odhalené tooDatabase funkci AlwaysEncrypted používat](#keys-engine)</li></ul> |
| **Zařízení IoT** | <ul><li>[Bezpečně uložit šifrovací klíče v zařízení IoT](#keys-iot)</li></ul> | 
| **Brána IoT cloudu** | <ul><li>[Generoval náhodný symetrický klíč dostatečně dlouhé pro ověřování tooIoT rozbočovače](#random-hub)</li></ul> | 
| **Dynamics CRM mobilního klienta** | <ul><li>[Ujistěte se, že k zásadě správy zařízení je na místě, který vyžaduje použití kódu PIN a umožňuje vzdálené vymazání](#pin-remote)</li></ul> | 
| **Klient aplikace Outlook Dynamics CRM** | <ul><li>[Zkontrolujte, zda že je k zásadě správy zařízení v místě, ke kterému se vyžaduje PIN kód nebo heslo nebo automatické uzamčení a šifruje všechna data (např. Bitlocker)](#bitlocker)</li></ul> | 
| **Serveru identit** | <ul><li>[Ujistěte se, že podpisové klíče jsou převracet při použití serveru identit](#rolled-server)</li><li>[Zkontrolujte, zda ID kryptograficky silnou klienta, sdílený tajný klíč klienta jsou používány serveru identit](#client-server)</li></ul> | 

## <a id="cipher-length"></a>Používejte pouze šifry schválené symetrický bloku a délky klíčů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Produkty musí používat jenom tyto šifry symetrický bloku a přidružené délky klíčů, které byly explicitně schváleny hello kryptografických Advisor ve vaší organizaci. Schválené symetrické algoritmy ve společnosti Microsoft zahrnují následující bloku šifry hello:</p><ul><li>Pro nový kód jsou přijatelné AES-128, AES 192 a AES 256</li><li>Pro zpětnou kompatibilitu s existující kód je přijatelné tři klíč 3DES.</li><li>Pro produkty pomocí šifry symetrický bloku:<ul><li>Advanced Encryption (Standard AES) je vyžadována pro nový kód</li><li>Tři klíč triple Data Encryption Standard (3DES) je povolený v existujícím kódu z důvodu zpětné kompatibility</li><li>Všechny ostatní bloku šiframi, včetně RC2, DES, 2 klíč 3DES, DESX a pruhovaného, lze použít pouze pro dešifrování stará data a pokud se používá pro šifrování, je nutné nahradit</li></ul></li><li>U bloku symetrický šifrovací algoritmy se vyžaduje minimální délku klíče 128 bitů. Hello pouze algoritmus šifrování bloku doporučené pro nový kód je AES (AES-128, AES 192 a AES 256 jsou všechny přijatelná)</li><li>V existujícím kódu; je aktuálně přijatelné, pokud je v 3DES tři klíč doporučuje se tooAES přechod. DES, DESX, RC2 a pruhovaného jsou už považované za bezpečné. Tyto algoritmy lze použít pouze pro dešifrování stávající data pro hello zájmu zpětné kompatibility a data by měl být znovu zašifrována pomocí šifru doporučené bloku</li></ul><p>Prosím poznamenat, že všechny šifry symetrický bloku musí použít s režimem schválené šifer, které vyžaduje použití odpovídající inicializační vektor (IV). Odpovídající IV, je obvykle náhodné číslo a nikdy konstantní hodnota</p><p>Po zkontrolujte panel kryptografických vaší organizace, může být povoleno použití Hello starší verze nebo jinak neschválených kryptografických algoritmů a délek menší klíčů pro čtení stávající data (jako názvem na rozdíl od toowriting nová data). Však nutné soubor pro výjimku pro tento požadavek. Kromě toho v podnikových nasazeních produkty měli zvážit upozornění správci po slabé kryptografické algoritmy použité tooread data. Takové upozornění by měl být vysvětlující a možné použít. V některých případech může být vhodné toohave zásad skupiny řízení hello použití slabé kryptografické algoritmy</p><p>Povolené .NET algoritmy pro spravované kryptografických flexibility (v upřednostňovaném pořadí)</p><ul><li>AesCng (kompatibilní se standardem FIPS)</li><li>AuthenticatedAesCng (kompatibilní se standardem FIPS)</li><li>AESCryptoServiceProvider (kompatibilní se standardem FIPS)</li><li>AESManaged (bez – kompatibilní se standardem FIPS)</li></ul><p>Upozorňujeme, že žádná z tyto algoritmy lze zadat prostřednictvím hello `SymmetricAlgorithm.Create` nebo `CryptoConfig.CreateFromName` metody bez provedení změny souboru machine.config toohello. Všimněte si také, že je s názvem AES ve verzích předchozí too.NET rozhraní .NET 3.5 `RijndaelManaged`, a `AesCng` a `AuthenticatedAesCng` jsou > k dispozici prostřednictvím webu CodePlex a vyžadovat CNG v hello základní operačního systému</p>

## <a id="vector-ciphers"></a>Použití schváleno režimy šifrování bloku a inicializační vektory pro symetrické šifry

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Všechny šifry symetrický bloku musí použít s režim schválené symetrického šifrování. režimy Hello pouze schválené jsou CBC a CTS. Konkrétně hello elektronické kód adresáře (ECB) režim provozu je nutno; použití ECB vyžaduje kontrolu kryptografických Tabule vaší organizace. Všechny využití OFB, CFB, PEV.cenu, CCM, a GCM nebo jiných režim šifrování musí být zkontrolovány uživatelem Tabule kryptografických vaší organizace. Opětovné použití hello stejné inicializační vektor (IV) s šifry bloku v "streamování šifer režimech" například PEV.cenu, může způsobit, že šifrovaná data toobe zjištěny. Všechny šifry symetrický bloku musí být rovněž použit s příslušnou inicializační vektor (IV). Odpovídající IV je kryptograficky silnou, náhodné číslo a nikdy konstantní hodnotu. |

## <a id="padding"></a>Použít schválené asymetrických algoritmů, délek klíčů a odsazení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>použití Hello zakázaného kryptografických algoritmů představuje významné riziko tooproduct zabezpečení a je třeba se vyhnout. Produkty musí používat jenom tyto kryptografické algoritmy a přidružené délky klíčů a odsazení explicitně schválené Radou kryptografických vaší organizace.</p><ul><li>**RSA -** mohou být použity pro šifrování, podpisu a výměny klíčů. Šifrování RSA musí používat pouze hello OAEP nebo RSA KEM odsazení režimy. Existující kód mohou používat PKCS č. 1 v1.5 odsazení režimu pouze z důvodu kompatibility. Použijte hodnotu null odsazení je explicitně zakázané. Klíče > = 2 048 bitů pro nový kód je vyžadován. Existující kód může podporovat klíče < 2048 bitů pouze pro zpětné kompatibility po kontrolní Radou kryptografických vaší organizace. Klíče < 1 024 bitů lze použít pouze pro dešifrování ověření stará data, a pokud nahrazené použije pro šifrování nebo operace podepisování</li><li>**ECDSA -** mohou být použity pro pouze podpis. ECDSA s > = 256 bitů se vyžaduje pro nový kód. Na základě ECDSA podpisy musí používat jednu z hello tři NIST schválení křivek (p-256, 384 nebo P521). Křivek, které nebyly analyzovány důkladně může použít až po kontrolu s Tabule kryptografických vaší organizace.</li><li>**ECDH -** mohou být použity pro výměnu klíčů pouze. ECDH s > = 256 bitů se vyžaduje pro nový kód. Na základě ECDH výměny klíčů musí používat jednu z hello tři NIST schválení křivek (p-256, 384 nebo P521). Křivek, které nebyly analyzovány důkladně může použít až po kontrolu s Tabule kryptografických vaší organizace.</li><li>**DSA -** může být po kontrole a schválení od vaší organizace kryptografických Tabule přijatelný. Obraťte se na vaše tooschedule advisor zabezpečení zkontrolujte panel kryptografických vaší organizace. Pokud je používání agenta DSA, Všimněte si, že budete potřebovat tooprohibit použití klíčů menší než 2 048 bitů. CNG podporuje 2048 bitů a větší délky klíčů od verze Windows 8.</li><li>**Diffie-Hellman -** mohou být použity pro relace správy klíčů pouze. Délka klíče > = 2 048 bitů pro nový kód je vyžadován. Existující kód může podporovat délky klíčů < 2048 bitů pouze pro zpětné kompatibility po kontrolní Radou kryptografických vaší organizace. Klíče nesmí být použity < 1 024 bitů.</li><ul>

## <a id="numgen"></a>Použití schváleno generátory náhodných čísel

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Produkty musí používat schválené generátory náhodných čísel. Pseudonáhodná funkce jako je například rand – funkce hello C runtime, třídy rozhraní .NET Framework hello System.Random nebo funkce systému, jako je například GetTickCount nesmí, proto být nikdy použit v takový kód. Zakazuje použití algoritmu hello duální eliptické křivky náhodné číslo generátor (DUAL_EC_DRBG)</p><ul><li>**CNG -** BCryptGenRandom (použijte příznak BCRYPT_USE_SYSTEM_PREFERRED_RNG hello nedoporučuje, pokud hello volající by se mohly spustit na jakékoli OVĚŘILO větší než 0 [PASSIVE_LEVEL])</li><li>**CAPI -** cryptGenRandom</li><li>**Win32/64-** RtlGenRandom (nové implementace by měl použít BCryptGenRandom nebo CryptGenRandom) * rand_s – * SystemPrng (pro režimu jádra)</li><li>**. NET -** RNGCryptoServiceProvider nebo RNGCng</li><li>**Aplikace Windows Store -** Windows.Security.Cryptography.CryptographicBuffer.GenerateRandom nebo. GenerateRandomNumber</li><li>**Apple OS X (10.7+)/iOS(2.0+) -** int SecRandomCopyBytes (SecRandomRef náhodné, size_t – počet, uint8_t *bajtů)</li><li>**Apple OS X (< 10.7)-** použití nebo dev/náhodných tooretrieve náhodná čísla</li><li>**Java(Including Google Android Java Code) -** java.security.SecureRandom třídy. Poznámka: pro Android 4.3 (želé položku Bean), vývojáři musí podle hello Android doporučená řešení a aktualizovat své aplikace tooexplicitly inicializovat hello PRNG s entropie z /dev/urandom nebo /dev/random</li></ul>|

## <a id="stream-ciphers"></a>Nepoužívejte šifry symetrický datového proudu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Šifry symetrický datového proudu, jako je například RC4, nesmí se používat. Místo šifry symetrický datového proudu měli používat produkty cipher block, konkrétně AES s délkou klíče nejméně 128 bitů. |

## <a id="mac-hash"></a>Použití schváleno MAC, HMAC/keyed algoritmů hash

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Produkty musí používat jen schválení ověřovací kód zprávy (MAC) nebo algoritmy kódu (HMAC) ověřování na základě hodnoty hash zpráv.</p><p>Ověřovací kód zprávy (MAC) je informace připojené tooa zprávy, která umožňuje jeho příjemce tooverify hello pravost hello odesílatele i hello integritu uvítací zprávu pomocí tajný klíč. Hello použijte buď MAC na základě hodnoty hash ([HMAC](http://csrc.nist.gov/publications/nistpubs/800-107-rev1/sp800-107-rev1.pdf)) nebo [na základě bloku šifrovací MAC](http://csrc.nist.gov/publications/nistpubs/800-38B/SP_800-38B.pdf) je přípustné, dokud všechny základní hodnotu hash nebo symetrický šifrovací algoritmy jsou také schválené pro použití; aktuálně to zahrnuje hello funkce SHA2 HMAC s klíčem (HMAC SHA256, HMAC SHA384 a HMAC SHA512) a hello CMAC/OMAC1 a OMAC2 blokovat na základě šifrovací MAC (jsou založeny na standardu AES).</p><p>Může být použití HMAC SHA1 povolenou pro platformu kompatibility, ale bude se vyžaduje toofile proceduru toothis výjimky a projít kontrolní kryptografický vaší organizace. Zkrácení HMAC tooless než 128 bitů není povoleno. Pomocí metody toohash zákazník klíč a data není schválený a musí projít předchozí toouse zkontrolujte vaší organizace kryptografických panelu.</p>|

## <a id="hash-functions"></a>Použít pouze funkce schválené kryptografické hodnoty hash

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Produkty musí používat algoritmy hash (SHA256, SHA384 a SHA512) řady hello SHA-2. V případě potřeby kratší hodnotu hash, jako je délka výstupní 128-bit v pořadí toofit datová struktura navrhovat s hello kratší hodnotu hash MD5 na paměti, může zkrátit produktovými týmy jednu z hodnot hash SHA2 hello (obvykle SHA256). Všimněte si, SHA384 je oříznuta verzi SHA512. Zkrácení kryptografické hodnoty hash pro účely tooless zabezpečení než 128 bitů není povoleno. Nový kód nesmí používat hello MD2, MD4, MD5, SHA-0, SHA-1 nebo RIPEMD algoritmů hash. Kolize hodnot hash jsou u vhodná pro tyto algoritmy, které efektivně dělí.</p><p>Povolená .NET algoritmů hash pro spravované kryptografických flexibility (v upřednostňovaném pořadí):</p><ul><li>SHA512Cng (kompatibilní se standardem FIPS)</li><li>SHA384Cng (kompatibilní se standardem FIPS)</li><li>SHA256Cng (kompatibilní se standardem FIPS)</li><li>SHA512Managed (bez – kompatibilní se standardem FIPS) (použití SHA512 jako název algoritmu ve volání tooHashAlgorithm.Create nebo CryptoConfig.CreateFromName)</li><li>SHA384Managed (bez – kompatibilní se standardem FIPS) (použití SHA384 jako algoritmu název v tooHashAlgorithm.Create volání nebo CryptoConfig.CreateFromName)</li><li>SHA256Managed (bez – kompatibilní se standardem FIPS) (použití SHA256 jako název algoritmu ve volání tooHashAlgorithm.Create nebo CryptoConfig.CreateFromName)</li><li>SHA512CryptoServiceProvider (kompatibilní se standardem FIPS)</li><li>SHA256CryptoServiceProvider (kompatibilní se standardem FIPS)</li><li>SHA384CryptoServiceProvider (kompatibilní se standardem FIPS)</li></ul>| 

## <a id="strong-db"></a>Používat silné šifrování algoritmy tooencrypt data v databázi hello

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Výběr šifrovacího algoritmu](https://technet.microsoft.com/library/ms345262(v=sql.130).aspx) |
| **Kroky** | Algoritmy šifrování definovat transformace dat, které nelze snadno vrátit neoprávnění uživatelé. SQL Server umožňuje správci a vývojáři toochoose některé z těchto několik algoritmy, včetně DES, Triple DES, TRIPLE_DES_3KEY, RC2, RC4, RC4 128-bit, DESX, AES 128-bit, AES 192 bitů a 256 bitů AES |

## <a id="ssis-signed"></a>Balíčky SSIS by měl být zašifrované a digitálně podepsané

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Identifikovat hello zdroj balíčků s digitálními podpisy](https://msdn.microsoft.com/library/ms141174.aspx), [hrozby a ohrožení zabezpečení zmírnění (Integration Services)](https://msdn.microsoft.com/library/bb522559.aspx) |
| **Kroky** | Hello zdroj balíčku je hello jednotlivé nebo organizace, který vytvořili balíček hello. Spuštění balíčku z neznámý nebo nedůvěryhodný zdroje může být riskantní. tooprevent Neautorizováno manipulaci s balíčky SSIS slouží digitální podpisy. Také tooensure hello důvěrnost hello balíčků během úložiště nebo přenosu, balíčky SSIS mít šifrované toobe |

## <a id="securables-db"></a>Přidat zabezpečitelné prostředky databáze toocritical digitální podpis

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Přidat podpis (Transact-SQL)](https://msdn.microsoft.com/library/ms181700) |
| **Kroky** | V případech, kdy hello integrity zabezpečitelné kritické databáze obsahuje toobe ověřit je třeba použít digitální podpisy. Databáze zabezpečitelné prostředky, jako jsou uložené procedury, funkce, sestavení nebo aktivační událost může být digitálně podepsané. Dole je příklad, pokud to může být užitečné: dejte nám vyslovení nezávislý dodavatel softwaru (nezávislé dodavatele softwaru) poskytl podporu tooa softwaru doručit tooone zákazníků. Před zajištění podpory, vhodnější hello ISV tooensure že zabezpečitelné hello software v databázi nebyla záměrně buď omylem nebo škodlivý pokus. Pokud hello zabezpečitelné digitálně podepsané, hello ISV můžete ověřit digitální podpis a ověřit jeho integritu.| 

## <a id="ekm-keys"></a>Použití SQL serveru EKM tooprotect šifrovací klíče

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [SQL Server Ekm (EKM)](https://msdn.microsoft.com/library/bb895340), [Extensible Key Management pomocí Azure Key Vault (SQL Server)](https://msdn.microsoft.com/library/dn198405) |
| **Kroky** | SQL Server Extensible Key Management umožňuje hello šifrovací klíče, které chrání toobe soubory databáze hello uložené v zařízení s vypnout pole například čipová karta, zařízení USB nebo modulu EKM/HSM. To také umožňuje ochranu dat před správce databáze (s výjimkou členové skupiny sysadmin hello). Data mohou být šifrována pomocí šifrovacích klíčů, že pouze hello databáze má uživatel přístup tooon hello externí EKM/modulu HSM. |

## <a id="keys-engine"></a>Pokud šifrovacích klíčů by neměl být modul odhalené tooDatabase funkci AlwaysEncrypted používat

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | SQL Azure a místní |
| **Atributy**              | MsSQL2016 verzi - 12, SQL |
| **Odkazy**              | [Funkce Always Encrypted (databázový stroj)](https://msdn.microsoft.com/library/mt163865) |
| **Kroky** | Vždy je šifrovaný funkce určená tooprotect citlivých dat, třeba čísla platebních karet nebo national identifikačními čísly (například USA čísel sociálního pojištění), uloženy v databázi Azure SQL Database nebo SQL Server. Šifrovaný vždy umožňuje klientům tooencrypt citlivá data uvnitř klientské aplikace a nikdy odhalit hello šifrovací klíče toohello databázový stroj (SQL Database nebo SQL Server). V důsledku toho vždy šifrována zajišťuje oddělení mezi uživatelů, kteří vlastní hello data (a můžete ji zobrazit) a ty, kteří spravují hello data (ale musí mít žádný přístup) |

## <a id="keys-iot"></a>Bezpečně uložit šifrovací klíče v zařízení IoT

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Zařízení IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Zařízení OS - jádro IoT Windows, připojení zařízení - SDK pro zařízení Azure IoT |
| **Odkazy**              | [Čip TPM na jádro IoT Windows](https://developer.microsoft.com/windows/iot/docs/tpm), [nastavení TPM na jádro IoT Windows](https://developer.microsoft.com/windows/iot/win10/setuptpm), [TPM SDK zařízení IoT Azure](https://github.com/Azure/azure-iot-hub-vs-cs/wiki/Device-Provisioning-with-TPM) |
| **Kroky** | Chráněný Symmetric nebo certifikátu privátního klíče, bezpečně v hardwaru úložiště, jako je čipy TPM a čipové karty. Jádro IoT Windows 10 podporuje hello uživatele čipu TPM a existuje několik kompatibilní čipy TPM, které lze použít: https://developer.microsoft.com/windows/iot/win10/tpm. Je doporučeno toouse firmwaru nebo diskrétní TPM. Čip TPM softwaru lze používat pouze pro účely vývoje a testování. Jakmile čip TPM je k dispozici a hello klíče jsou zřízené v něm, by měly být zapsány hello kód, který generuje hello token bez pevné kódování žádné citlivé informace, které se v něm. | 

### <a name="example"></a>Příklad
```
TpmDevice myDevice = new TpmDevice(0);
// Use logical device 0 on hello TPM 
string hubUri = myDevice.GetHostName(); 
string deviceId = myDevice.GetDeviceId(); 
string sasToken = myDevice.GetSASToken(); 

var deviceClient = DeviceClient.Create( hubUri, AuthenticationMethodFactory. CreateAuthenticationWithToken(deviceId, sasToken), TransportType.Amqp); 
```
Jak je vidět, primární klíč zařízení hello se nenachází v kódu hello. Místo toho je uložený v hello TPM na pozici 0. Čip TPM zařízení generuje krátkodobou SAS token, který je pak použit tooconnect toohello IoT Hub. 

## <a id="random-hub"></a>Generoval náhodný symetrický klíč dostatečně dlouhé pro ověřování tooIoT rozbočovače

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána IoT cloudu | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Volba brány - Azure IoT Hub |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | IoT Hub obsahuje registr identit zařízení a při zřizování zařízení, automaticky vygeneruje náhodné symetrického klíče. Doporučujeme toouse použít tuto funkci hello Azure IoT Hub Identity registru toogenerate hello klíče pro ověřování. Centrum IoT také umožňuje klíče toobe zadané při vytváření hello zařízení. Pokud klíč je generován mimo IoT Hub během zřizování zařízení, je doporučeno toocreate náhodných symetrického klíče nebo minimálně 256 bitů. |

## <a id="pin-remote"></a>Ujistěte se, že k zásadě správy zařízení je na místě, který vyžaduje použití kódu PIN a umožňuje vzdálené vymazání

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM mobilního klienta | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Ujistěte se, že k zásadě správy zařízení je na místě, který vyžaduje použití kódu PIN a umožňuje vzdálené vymazání |

## <a id="bitlocker"></a>Zkontrolujte, zda že je k zásadě správy zařízení v místě, ke kterému se vyžaduje PIN kód nebo heslo nebo automatické uzamčení a šifruje všechna data (např. Bitlocker)

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Klient aplikace Outlook Dynamics CRM | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zkontrolujte, zda že je k zásadě správy zařízení v místě, ke kterému se vyžaduje PIN kód nebo heslo nebo automatické uzamčení a šifruje všechna data (např. Bitlocker) |

## <a id="rolled-server"></a>Ujistěte se, že podpisové klíče jsou převracet při použití serveru identit

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Serveru identit | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Identita serveru – klíčů, podpisů a šifrování](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html) |
| **Kroky** | Ujistěte se, že podpisové klíče jsou převracet při použití Identity serveru. Hello odkaz v části odkazy hello vysvětluje by měl být plánované jak aniž by to způsobilo výpadků tooapplications spoléhat na serveru identit. |

## <a id="client-server"></a>Zkontrolujte, zda ID kryptograficky silnou klienta, sdílený tajný klíč klienta jsou používány serveru identit

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Serveru identit | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Zkontrolujte, zda ID kryptograficky silnou klienta, sdílený tajný klíč klienta jsou používány serveru identit. Následující pokyny Hello by měl být použit při generování ID klienta a tajný klíč:</p><ul><li>Generuje náhodné GUID jako ID klienta hello</li><li>Generování generovaných kryptograficky náhodných klíče 256 bitů jako hello tajný klíč.</li></ul>|
