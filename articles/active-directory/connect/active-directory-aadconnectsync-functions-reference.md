---
title: "Synchronizace Azure AD Connect: referenční dokumentace funkcí | Microsoft Docs"
description: "Odkaz výrazů deklarativního zřizování v synchronizaci Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: fbe0df856ca2efda965650fb85c7e831a0be32c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a>Synchronizace Azure AD Connect: odkaz na funkce
Ve službě Azure AD Connect jsou funkce použité toomanipulate hodnota atributu během synchronizace.  
Hello syntaxe funkce hello je vyjádřit pomocí hello následující formát:  
`<output type> FunctionName(<input type> <position name>, ..)`

Pokud funkci je přetížena a přijímá více syntaxe, jsou uvedeny všechny platné syntaxe.  
Hello funkce jsou silného typu a jejich ověřte, že hello typ předaný typ odpovídá hello popsané.  
Pokud hello typ neodpovídá, je vyvolána k chybě.

Hello typy jsou vyjádřeny s hello následující syntaxi:

* **Koš** – binární
* **BOOL** – Boolean
* **DT** – datum a čas UTC
* **výčet** – výčet známé konstanty
* **Exp** – výraz, který je očekáván tooevaluate tooa logická hodnota
* **mvbin** – vícehodnotových binární
* **mvstr** – vícehodnotových řetězců
* **mvref** – vícehodnotových odkaz
* **Poče** – číselné
* **REF** – odkaz
* **Str –** – řetězec
* **var** – hodnotu typu variant (téměř) žádného jiného typu
* **void** – nevrací hodnotu

Hello funkce s typy hello **mvbin**, **mvstr**, a **mvref** můžete používat jenom u více hodnot atributů. Funkce s **bin**, **str**, a **ref** pracovní jednohodnotové i více hodnot atributů.

## <a name="functions-reference"></a>Reference k funkcím
| Seznam funkcí |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| **Certifikát** | | | | |
| [CertExtensionOids](#certextensionoids) |[CertFormat](#certformat) |[CertFriendlyName](#certfriendlyname) |[CertHashString](#certhashstring) | |
| [CertIssuer](#certissuer) |[CertIssuerDN](#certissuerdn) |[CertIssuerOid](#certissueroid) |[CertKeyAlgorithm](#certkeyalgorithm) | |
| [CertKeyAlgorithmParams](#certkeyalgorithmparams) |[CertNameInfo](#certnameinfo) |[CertNotAfter](#certnotafter) |[CertNotBefore](#certnotbefore) | |
| [CertPublicKeyOid](#certpublickeyoid) |[CertPublicKeyParametersOid](#certpublickeyparametersoid) |[CertSerialNumber](#certserialnumber) |[CertSignatureAlgorithmOid](#certsignaturealgorithmoid) | |
| [CertSubject](#certsubject) |[CertSubjectNameDN](#certsubjectnamedn) |[CertSubjectNameOid](#certsubjectnameoid) |[CertThumbprint](#certthumbprint) | |
[CertVersion](#certversion) |[IsCert](#iscert) | | | |
| **Převod** | | | | |
| [CBool –](#cbool) |[CDate –](#cdate) |[CGuid](#cguid) |[ConvertFromBase64](#convertfrombase64) | |
| [ConvertToBase64](#converttobase64) |[ConvertFromUTF8Hex](#convertfromutf8hex) |[ConvertToUTF8Hex](#converttoutf8hex) |[CNum](#cnum) | |
| [CRef –](#cref) |[CStr](#cstr) |[StringFromGuid](#StringFromGuid) |[StringFromSid](#stringfromsid) | |
| **Datum a čas** | | | | |
| [Funkce DateAdd](#dateadd) |[DateFromNum](#datefromnum) |[FormatDateTime](#formatdatetime) |[Nyní](#now) | |
| [NumFromDate](#numfromdate) | | | | |
| **Adresář** | | | | |
| [DNComponent](#dncomponent) |[DNComponentRev](#dncomponentrev) |[EscapeDNComponent](#escapedncomponent) | | |
| **Vyhodnocení** | | | | |
| [IsBitSet](#isbitset) |[IsDate](#isdate) |[IsEmpty –](#isempty) |[IsGuid](#isguid) | |
| [IsNull](#isnull) |[IsNullOrEmpty](#isnullorempty) |[IsNumeric](#isnumeric) |[IsPresent](#ispresent) | |
| [IsString](#isstring) | | | | |
| **Matematické** | | | | |
| [BitAnd](#bitand) |[BitOr](#bitor) |[RandomNum](#randomnum) | | |
| **Více hodnot** | | | | |
| [Obsahuje](#contains) |[Počet](#count) |[Položka](#item) |[ItemOrNull](#itemornull) | |
| [Spojení](#join) |[Removeduplicates –](#removeduplicates) |[Rozdělení](#split) | | |
| **Tok programu** | | | | |
| [Chyba](#error) |[IIF](#iif) |[Výběr](#select) |[Přepínače](#switch) | |
| [Kde](#where) |[S](#with) | | | |
| **Text** | | | | |
| [IDENTIFIKÁTOR GUID](#guid) |[InStr](#instr) |[InStrRev](#instrrev) |[LCase](#lcase) | |
| [Vlevo](#left) |[Len](#len) |[LTrim](#ltrim) |[Mid –](#mid) | |
| [PadLeft](#padleft) |[PadRight –](#padright) |[PCase](#pcase) |[Nahradit](#replace) | |
| [ReplaceChars](#replacechars) |[Vpravo](#right) |[RTrim](#rtrim) |[Uvolnění dočasné paměti](#trim) | |
| [UCase](#ucase) |[Aplikace Word](#word) | | | |

- - -
### <a name="bitand"></a>BitAnd
**Popis:**  
Hello BitAnd – funkce nastaví na hodnotu určeného bits.

**Syntaxe:**  
`num BitAnd(num value1, num value2)`

* Hodnota1, hodnota2: číselných hodnot, které by měly být logickým společně

**Poznámky:**  
Tato funkce převede binární reprezentace oba parametry toohello a nastaví trochu na:

* 0 – Pokud jeden nebo oba hello odpovídající bitů *maska* a *příznak* mají hodnotu 0
* 1 – Pokud jsou obě odpovídající bitů hello 1.

Jinými slovy vrátí hodnotu 0 ve všech případech, s výjimkou případů, kdy hello odpovídající oba parametry bitů 1.

**Příklad:**  
`BitAnd(&HF, &HF7)`  
Vrátí 7, protože hexadecimální "F" a "F7" vyhodnotit toothis hodnotu.

- - -
### <a name="bitor"></a>BitOr
**Popis:**  
Hello BitOr – funkce nastaví na hodnotu určeného bits.

**Syntaxe:**  
`num BitOr(num value1, num value2)`

* Hodnota1, hodnota2: číselných hodnot, které by měly být společně sjednocením (OR)

**Poznámky:**  
Tato funkce převede binární reprezentace oba parametry toohello a nastaví bit too1, pokud jsou jedno nebo obě hello odpovídající bitů maska a příznak 1 a too0 Pokud jsou obě odpovídající bitů hello 0. Jinými slovy vrátí 1 ve všech případech, s výjimkou případů, kdy hello odpovídající bity oba parametry mají hodnotu 0.

- - -
### <a name="cbool"></a>CBool –
**Popis:**  
Hello CBool – funkce vrátí že logickou hodnotu na základě hello vyhodnocení výrazu

**Syntaxe:**  
`bool CBool(exp Expression)`

**Poznámky:**  
Pokud hello výraz vyhodnotí tooa nenulovou hodnotu, pak CBool – vrátí hodnotu True, jinak vrátí hodnotu False.

**Příklad:**  
`CBool([attrib1] = [attrib2])`  

Vrátí hodnotu True, pokud oba atributy mít hello stejnou hodnotu.

- - -
### <a name="cdate"></a>CDate –
**Popis:**  
Hello CDate – funkce vrátí z řetězce na datum a čas UTC. Data a času není typu nativní atribut synchronizované, ale je používána některé funkce.

**Syntaxe:**  
`dt CDate(str value)`

* Hodnota: Řetězec datum, čas a volitelně časové pásmo

**Poznámky:**  
Hello byl vrácen řetězec je vždy ve standardu UTC.

**Příklad:**  
`CDate([employeeStartTime])`  
Vrací hodnotu DateTime podle času zahájení hello zaměstnance

`CDate("2013-01-10 4:00 PM -8")`  
Vrátí data a času představující "2013-01-11 12:00:00"








- - -
### <a name="certextensionoids"></a>CertExtensionOids
**Popis:**  
Vrátí hello Oid hodnoty všech hello důležitých rozšíření certifikátů objektu.

**Syntaxe:**  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certformat"></a>CertFormat
**Popis:**  
Vrátí hello název formátu hello tohoto certifikátu x.509 v3.

**Syntaxe:**  
`str CertFormat(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certfriendlyname"></a>CertFriendlyName
**Popis:**  
Vrátí hello přidružené alias pro certifikát.

**Syntaxe:**  
`str CertFriendlyName(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certhashstring"></a>CertHashString
**Popis:**  
Vrátí hodnotu hash SHA1 certifikátu x.509 v3 hello hello jako řetězec hexadecimálních znaků.

**Syntaxe:**  
`str CertHashString(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certissuer"></a>CertIssuer
**Popis:**  
Vrátí hello název hello certifikační autority, která vystavila certifikát x.509 v3 hello.

**Syntaxe:**  
`str CertIssuer(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certissuerdn"></a>CertIssuerDN
**Popis:**  
Vrátí hello rozlišující název vystavitele certifikátu hello.

**Syntaxe:**  
`str CertIssuerDN(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certissueroid"></a>CertIssuerOid
**Popis:**  
Vrátí hello Oid hello vystavitele certifikátu.

**Syntaxe:**  
`str CertIssuerOid(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certkeyalgorithm"></a>CertKeyAlgorithm
**Popis:**  
Vrací informace hello algoritmus klíče pro tento certifikát x.509 v3 jako řetězec.

**Syntaxe:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certkeyalgorithmparams"></a>CertKeyAlgorithmParams
**Popis:**  
Vrátí parametry hello algoritmus klíče pro certifikát x.509 v3 hello jako řetězec hexadecimálních znaků.

**Syntaxe:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certnameinfo"></a>CertNameInfo
**Popis:**  
Vrátí hello předmět a vystavitele názvy z certifikátu.

**Syntaxe:**  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.
*   X509NameType: hello X509NameType hodnoty pro předmět hello.
*   includesIssuerName: název vystavitele hello true tooinclude; jinak hodnota false.

- - -
### <a name="certnotafter"></a>CertNotAfter
**Popis:**  
Vrátí hello datum v místní čase, po kterém certifikát je již neplatný.

**Syntaxe:**  
`dt CertNotAfter(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certnotbefore"></a>CertNotBefore
**Popis:**  
Vrátí hello datum v místní čase, při kterém certifikát začne platit.

**Syntaxe:**  
`dt CertNotBefore(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certpublickeyoid"></a>CertPublicKeyOid
**Popis:**  
Vrátí hello Oid hello veřejný klíč pro certifikát x.509 v3 hello.

**Syntaxe:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certpublickeyparametersoid"></a>CertPublicKeyParametersOid
**Popis:**  
Vrátí hello Oid parametrů hello veřejného klíče pro certifikát x.509 v3 hello.

**Syntaxe:**  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certserialnumber"></a>CertSerialNumber
**Popis:**  
Vrátí hello sériové číslo certifikátu x.509 v3 hello.

**Syntaxe:**  
`str CertSerialNumber(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certsignaturealgorithmoid"></a>CertSignatureAlgorithmOid
**Popis:**  
Vrátí hello Oid algoritmu hello používá toocreate hello podpisu certifikátu.

**Syntaxe:**  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certsubject"></a>CertSubject
**Popis:**  
Získá hello rozlišující název subjektu z certifikátu.

**Syntaxe:**  
`str CertSubject(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certsubjectnamedn"></a>CertSubjectNameDN
**Popis:**  
Vrátí hello rozlišující název subjektu z certifikátu.

**Syntaxe:**  
`str CertSubjectNameDN(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certsubjectnameoid"></a>CertSubjectNameOid
**Popis:**  
Vrátí hello Oid hello název subjektu z certifikátu.

**Syntaxe:**  
`str CertSubjectNameOid(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certthumbprint"></a>CertThumbprint
**Popis:**  
Vrátí hello kryptografického otisku certifikátu.

**Syntaxe:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="certversion"></a>CertVersion
**Popis:**  
Vrátí hello verze formátu X.509 certifikátu.

**Syntaxe:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.

- - -
### <a name="cguid"></a>CGuid
**Popis:**  
Hello CGuid funkce převede řetězcovou reprezentaci hello binární reprezentace tooits identifikátor GUID.

**Syntaxe:**  
`bin CGuid(str GUID)`

* Řetězec formátu v tomto vzoru: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx nebo {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

- - -
### <a name="contains"></a>Contains
**Popis:**  
Hello obsahuje funkce najde řetězec uvnitř vícehodnotového atributu

**Syntaxe:**  
`num Contains (mvstring attribute, str search)`-malá a velká písmena  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-malá a velká písmena

* Atribut: toosearch hello více hodnot atributů.
* hledání: řetězec toofind v atributu hello.
* Casetype: CaseInsensitive nebo CaseSensitive.

Vrátí index hello více hodnot atributů, kde hello řetězce byl nalezen. 0 je vrácena, pokud řetězec hello nebyl nalezen.

**Poznámky:**  
Pro více hodnot řetězec atributy hello hledáním dílčích řetězců v hello hodnoty.  
Pro atributy typu odkaz hello vyhledávaná řetězec musí přesně shodovat toobe hodnota hello považovány za shodné.

**Příklad:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Pokud atribut proxyAddresses hello má primární e-mailovou adresu (indikován velká písmena "SMTP:"), pak se vraťte hello proxyAddress atribut, jinak vrátí chybu.

- - -
### <a name="convertfrombase64"></a>ConvertFromBase64
**Popis:**  
Hello ConvertFromBase64 funkce převede hello zadat regulární řetězec s kódováním base64 hodnotu tooa.

**Syntaxe:**  
`str ConvertFromBase64(str source)`-předpokládá pro kódování Unicode  
`str ConvertFromBase64(str source, enum Encoding)`

* Zdroj: řetězec kódování Base64  
* Kódování: Znakové sady Unicode, ASCII, UTF8

**Příklad**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Vrátí oba příklady "*Hello, world!*"

- - -
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex
**Popis:**  
Hello ConvertFromUTF8Hex funkce převede hello zadaný řetězec tooa UTF8 šestnáctkově kódování.

**Syntaxe:**  
`str ConvertFromUTF8Hex(str source)`

* Zdroj: UTF8 2bajtová kódovaného palčivost

**Poznámky:**  
Hello rozdíl mezi tato funkce a ConvertFromBase64([],UTF8) ve výsledku této hello je popisný hello rozlišující název atributu.  
Tento formát se službou Azure Active Directory používá jako rozlišující název.

**Příklad:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Vrátí "*Hello, world!*"

- - -
### <a name="converttobase64"></a>ConvertToBase64
**Popis:**  
Hello ConvertToBase64 funkce převede tooa řetězec base64 řetězec znaků Unicode.  
Převede hello hodnotu v poli celých čísel tooits ekvivalentní řetězcovou reprezentaci, který je zakódován pomocí kódování base-64 číslic.

**Syntaxe:**  
`str ConvertToBase64(str source)`

**Příklad:**  
`ConvertToBase64("Hello world!")`  
Vrátí "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

- - -
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Popis:**  
Hello ConvertToUTF8Hex funkce převede řetězec tooa UTF8 šestnáctkově kódovaný hodnotu.

**Syntaxe:**  
`str ConvertToUTF8Hex(str source)`

**Poznámky:**  
výstupní formát Hello této funkce se službou Azure Active Directory používá jako atribut formátu DN.

**Příklad:**  
`ConvertToUTF8Hex("Hello world!")`  
Vrátí 48656C6C6F20776F726C6421

- - -
### <a name="count"></a>Počet
**Popis:**  
Hello funkce Count vrátí hello počet elementů ve více hodnotami atributu

**Syntaxe:**  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a>CNum
**Popis:**  
Hello CNum funkce přebírá řetězec a vrátí číselným datovým typem.

**Syntaxe:**  
`num CNum(str value)`

- - -
### <a name="cref"></a>CRef –
**Popis:**  
Převede atribut typu odkaz tooa řetězec

**Syntaxe:**  
`ref CRef(str value)`

**Příklad:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a>CStr
**Popis:**  
CStr – Funkce Hello převede tooa string – datový typ.

**Syntaxe:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* hodnota: může být číselnou hodnotu, atribut odkaz nebo logická hodnota.

**Příklad:**  
`CStr([dn])`  
Může vrátit "cn = Jan, dc = contoso, dc = com"

- - -
### <a name="dateadd"></a>Funkce DateAdd
**Popis:**  
Vrátí datum obsahující toowhich datum, byl přidán do zadaného časového intervalu.

**Syntaxe:**  
`dt DateAdd(str interval, num value, dt date)`

* interval: řetězec výraz, který je hello interval, když chcete tooadd. Hello řetězec musí mít jednu z hello následující hodnoty:
  * rrrr roku
  * q čtvrtletí.
  * m měsíc
  * y den roku
  * d den
  * w den v týdnu
  * TT týden
  * h hodinu
  * n minutu
  * s druhou
* hodnota: hello počet jednotek chcete tooadd. Může být kladná (tooget data v budoucnu hello) nebo záporné (tooget data v minulosti hello).
* datum: data a času představující datum toowhich hello interval je přidána.

**Příklad:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Přidá 3 měsíce a vrátí hodnotu DateTime představující "2001-04-01".

- - -
### <a name="datefromnum"></a>DateFromNum
**Popis:**  
Hello DateFromNum funkce převede hodnotu v AD na datum formátu tooa typu datum a čas.

**Syntaxe:**  
`dt DateFromNum(num value)`

**Příklad:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Vrací hodnotu DateTime představující 2012-01-01 23:00:00

- - -
### <a name="dncomponent"></a>DNComponent
**Popis:**  
Hello DNComponent funkce vrátí hodnotu hello zadanou součástí rozlišující název budete zleva.

**Syntaxe:**  
`str DNComponent(ref dn, num ComponentNumber)`

* rozlišující název: hello odkaz na atribut toointerpret
* ComponentNumber: hello součástí tooreturn hello rozlišující název

**Příklad:**  
`DNComponent([dn],1)`  
Pokud je rozlišující název "cn = Jan, ou =...," vrátí Jan

- - -
### <a name="dncomponentrev"></a>DNComponentRev
**Popis:**  
Hello DNComponentRev funkce vrátí hodnotu hello zadanou součástí rozlišující název budete zprava (hello end).

**Syntaxe:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* rozlišující název: hello odkaz na atribut toointerpret
* ComponentNumber - hello součástí tooreturn hello rozlišující název
* Možnosti: Řadič domény – ignorovat všechny součásti s "dc ="

**Příklad:**  
Pokud rozlišující název "cn Jan, ou = Atlantu, ou = GA, ou = = USA, řadič domény = contoso, dc = com" pak  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Obě vrací nás.

- - -
### <a name="error"></a>Chyba
**Popis:**  
Hello Chyba funkce je použité tooreturn o vlastní chybě.

**Syntaxe:**  
`void Error(str ErrorMessage)`

**Příklad:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Pokud hello atribut accountName není k dispozici, vyvolá chybu, hello objektu.

- - -
### <a name="escapedncomponent"></a>EscapeDNComponent
**Popis:**  
Hello EscapeDNComponent funkce využívá jednu součást názvu domény a řídicí sekvence, může být reprezentován v protokolu LDAP.

**Syntaxe:**  
`str EscapeDNComponent(str value)`

**Příklad:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Zajistí, že objekt hello lze vytvořit v adresáři LDAP, i v případě, že atribut hello displayName obsahuje znaky, které je nutné uvést v protokolu LDAP.

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Popis:**  
Funkce FormatDateTime Hello je použité tooformat řetězec tooa DateTime s zadaný formát

**Syntaxe:**  
`str FormatDateTime(dt value, str format)`

* hodnota: hodnotu ve formátu data a času hello
* formát: řetězec představující tooconvert formátu hello k.

**Poznámky:**  
Hello možné hodnoty pro formát hello naleznete zde: [uživatelem definované formátu data a času (formát funkce)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Příklad:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Výsledkem "2007-12-25".

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Může mít za následek "20140905081453.0Z"

- - -
### <a name="guid"></a>IDENTIFIKÁTOR GUID
**Popis:**  
Funkce Hello GUID generuje nový náhodný identifikátor GUID

**Syntaxe:**  
`str GUID()`

- - -
### <a name="iif"></a>IIF
**Popis:**  
Hello funkce IIF vrátí jednu z možných hodnot na základě podmínky pro zadanou sadu.

**Syntaxe:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* Podmínka: Libovolná hodnota nebo výraz, který lze vyhodnotit tootrue, nebo hodnotu NEPRAVDA.
* valueIfTrue: Pokud hello podmínka vyhodnocena jako tootrue, hello vrátil hodnotu.
* valueIfFalse: Pokud hello podmínka vyhodnocena jako toofalse, hello vrátil hodnotu.

**Příklad:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Pokud je uživatel hello učně, vrátí přidat hello alias uživatele s "t-" vrátí toohello začátku ho jiný alias hello uživatele, jako je.

- - -
### <a name="instr"></a>InStr
**Popis:**  
Hello InStr Funkce hello první výskyt je dílčí řetězec najde v řetězci

**Syntaxe:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* prohledávaný_řetězec: řetězec toobe vyhledávat
* hledaný_řetězec: řetězec toobe nalezen
* spustit: počáteční pozice toofind hello substring
* porovnání: vbTextCompare nebo vbBinaryCompare

**Poznámky:**  
Vrátí pozici hello kde nebyl nalezen dílčí řetězec hello nebo 0, pokud nebyl nalezen.

**Příklad:**  
`InStr("hello quick brown fox","quick")`  
Too5 hodnoty

`InStr("repEated","e",3,vbBinaryCompare)`  
Vyhodnotí too7

- - -
### <a name="instrrev"></a>InStrRev
**Popis:**  
Hello funkce InStrRev hello posledního výskytu dílčí řetězec najde v řetězci

**Syntaxe:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* prohledávaný_řetězec: řetězec toobe vyhledávat
* hledaný_řetězec: řetězec toobe nalezen
* spustit: počáteční pozice toofind hello substring
* porovnání: vbTextCompare nebo vbBinaryCompare

**Poznámky:**  
Vrátí pozici hello kde nebyl nalezen dílčí řetězec hello nebo 0, pokud nebyl nalezen.

**Příklad:**  
`InStrRev("abbcdbbbef","bb")`  
Vrátí hodnotu 7

- - -
### <a name="isbitset"></a>IsBitSet
**Popis:**  
Funkce Hello IsBitSet testů, pokud je trochu nastavená nebo ne

**Syntaxe:**  
`bool IsBitSet(num value, num flag)`

* hodnota: číselnou hodnotu, která je evaluated.flag: číselnou hodnotu, která má hello bit toobe vyhodnocení

**Příklad:**  
`IsBitSet(&HF,4)`  
Vrátí hodnotu True, protože je nastaven bit "4" v šestnáctkové hodnoty hello "F"

- - -
### <a name="isdate"></a>IsDate
**Popis:**  
Pokud hello výraz může být vyhodnotí jako typ DateTime, pak hello Funkce IsDate vyhodnotí tooTrue.

**Syntaxe:**  
`bool IsDate(var Expression)`

**Poznámky:**  
Toodetermine použít, pokud CDate() může být úspěšné.

- - -
### <a name="iscert"></a>IsCert
**Popis:**  
Vrátí hodnotu true Pokud serializovat lze hello nezpracovaná data do objektu certifikát .NET X509Certificate2.

**Syntaxe:**  
`bool CertThumbprint(binary certificateRawData)`  
*   certificateRawData: reprezentace bajtové pole certifikátu X.509. Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.
- - -
### <a name="isempty"></a>IsEmpty –
**Popis:**  
Pokud atribut hello je k dispozici v hello CS nebo více hodnot, ale vyhodnotí tooan prázdný řetězec, vyhodnotí hello IsEmpty – funkce tooTrue.

**Syntaxe:**  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a>IsGuid
**Popis:**  
Pokud řetězec hello může být převedená tooa identifikátor GUID, funkce IsGuid hello vyhodnotit tootrue.

**Syntaxe:**  
`bool IsGuid(str GUID)`

**Poznámky:**  
Identifikátor GUID je definován jako jeden z těchto vzorů následující řetězec: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx nebo {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Toodetermine použít, pokud CGuid() může být úspěšné.

**Příklad:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Pokud hello StrAttribute má formát identifikátoru GUID, vrátit binární reprezentace, jinak vrátí hodnotu Null.

- - -
### <a name="isnull"></a>IsNull
**Popis:**  
Pokud hello výraz vyhodnocen jako tooNull, hello IsNull funkce vrátí hodnotu true.

**Syntaxe:**  
`bool IsNull(var Expression)`

**Poznámky:**  
Pro atribut s hodnotou Null se vyjádří hello absenci hello atributu.

**Příklad:**  
`IsNull([displayName])`  
Vrátí hodnotu True, pokud není k dispozici v hello CS nebo MV hello atribut.

- - -
### <a name="isnullorempty"></a>IsNullOrEmpty
**Popis:**  
Pokud hello výraz má hodnotu null nebo prázdný řetězec, hello IsNullOrEmpty funkce vrátí hodnotu true.

**Syntaxe:**  
`bool IsNullOrEmpty(var Expression)`

**Poznámky:**  
Pro atribut to by vyhodnocovaly tooTrue Pokud atribut hello chybí nebo je k dispozici, ale prázdný řetězec.  
inverzní Hello této funkce je s názvem IsPresent.

**Příklad:**  
`IsNullOrEmpty([displayName])`  
Vrátí hodnotu True, pokud není přítomen nebo je prázdný řetězec v hello CS nebo MV hello atributu.

- - -
### <a name="isnumeric"></a>IsNumeric
**Popis:**  
Hello Funkce IsNumeric vrací logickou hodnotu udávající, zda výraz může být vyhodnocen jako typ number.

**Syntaxe:**  
`bool IsNumeric(var Expression)`

**Poznámky:**  
Toodetermine použít, pokud CNum() může být úspěšné tooparse hello výraz.

- - -
### <a name="isstring"></a>IsString
**Popis:**  
Pokud hello výraz může být vyhodnotí tooa řetězec typu, potom funkce IsString hello vyhodnotí tooTrue.

**Syntaxe:**  
`bool IsString(var expression)`

**Poznámky:**  
Toodetermine použít, pokud CStr() může být úspěšné tooparse hello výraz.

- - -
### <a name="ispresent"></a>IsPresent
**Popis:**  
Pokud je výsledkem výrazu hello tooa řetězec, který není Null a není prázdná, pak hello IsPresent funkce vrátí hodnotu true.

**Syntaxe:**  
`bool IsPresent(var expression)`

**Poznámky:**  
inverzní Hello této funkce je s názvem IsNullOrEmpty.

**Příklad:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a>Položka
**Popis:**  
Hello položky funkce vrátí jednu položku z více hodnot řetězec nebo atributu.

**Syntaxe:**  
`var Item(mvstr attribute, num index)`

* Atribut: vícehodnotového atributu
* index: položky tooan indexu v řetězci hello více hodnotami.

**Poznámky:**  
Hello funkce Item je užitečné společně s hello obsahuje funkce, protože hello pozdější funkce vrátí hello index tooan položku hello více hodnot atributů.

Vrátí chybu, pokud index je mimo rozsah.

**Příklad:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Vrátí hello primární e-mailovou adresu.

- - -
### <a name="itemornull"></a>ItemOrNull
**Popis:**  
Hello ItemOrNull funkce vrátí jednu položku z více hodnot řetězec nebo atributu.

**Syntaxe:**  
`var ItemOrNull(mvstr attribute, num index)`

* Atribut: vícehodnotového atributu
* index: položky tooan indexu v řetězci hello více hodnotami.

**Poznámky:**  
Hello ItemOrNull funkce je užitečné společně s hello obsahuje funkce, protože hello pozdější funkce vrátí hello index tooan položku hello více hodnot atributů.

Pokud index je mimo rozsah, vrátí hodnotu Null.

- - -
### <a name="join"></a>Spojit
**Popis:**  
Funkce spojení Hello přebírá řetězec s více hodnotami a vrátí řetězec s hodnotou jednoho zadaného oddělovače vložen mezi každou položku.

**Syntaxe:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* Atribut: obsahující více hodnot atributů řetězce toobe připojený.
* oddělovač: žádné řetězce, použité tooseparate hello dílčích řetězců v hello vrátil řetězec. Pokud tento parametr vynechán, hello znak mezery ("") se používá. Pokud oddělovač obsahuje řetězec nulové délky ("") nebo Nothing, jsou všechny položky v seznamu hello zřetězených žádný oddělovač.

**Poznámky**  
Rozdíly mezi hello spojení a rozdělení funkce není k dispozici. Hello funkce spojení trvá pole řetězců a připojí pomocí oddělovačem, tooreturn jeden řetězec. Hello funkce rozdělení přebírá řetězec a která ho odděluje v hello oddělovač, tooreturn pole řetězců. Klíčovým rozdílem je ale, že připojení lze zřetězení řetězců s libovolným řetězcem oddělovač, rozdělení můžete oddělit pouze řetězců pomocí jednoho znaku oddělovač.

**Příklad:**  
`Join([proxyAddresses],",")`  
Může vrátit: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

- - -
### <a name="lcase"></a>LCase
**Popis:**  
Hello funkce LCase převede všechny znaky v případě toolower řetězec.

**Syntaxe:**  
`str LCase(str value)`

**Příklad:**  
`LCase("TeSt")`  
Vrátí hodnotu "test".

- - -
### <a name="left"></a>Vlevo
**Popis:**  
Left – Hello funkce vrátí zadaný počet znaků zleva hello řetězce.

**Syntaxe:**  
`str Left(str string, num NumChars)`

* řetězec: hello tooreturn znaky řetězce z
* NumChars: číslo určující počet hello tooreturn znaky z hello začátku (vlevo) řetězce

**Poznámky:**  
Řetězec obsahující hello první numChars znaků v řetězci:

* Pokud numChars = 0, vrátí prázdný řetězec.
* Pokud numChars < 0, vrátí vstupní řetězec.
* Pokud má řetězec hodnotu null, vrátí prázdný řetězec.

Pokud řetězec obsahuje méně znaků, než je zadáno v numChars hello číslo, vrátí se identické toostring řetězec, (který je, obsahující všechny znaky v parametru 1).

**Příklad:**  
`Left("John Doe", 3)`  
Vrátí "Joh".

- - -
### <a name="len"></a>Len
**Popis:**  
Hello funkce Len vrátí počet znaků v řetězci.

**Syntaxe:**  
`num Len(str value)`

**Příklad:**  
`Len("John Doe")`  
Vrátí 8

- - -
### <a name="ltrim"></a>LTrim
**Popis:**  
Hello funkce LTrim odebere úvodní prázdné znaky v řetězci.

**Syntaxe:**  
`str LTrim(str value)`

**Příklad:**  
`LTrim(" Test ")`  
Vrátí "Test"

- - -
### <a name="mid"></a>Mid –
**Popis:**  
Hello Mid funkce vrací zadaný počet znaků od zadané pozici v řetězci.

**Syntaxe:**  
`str Mid(str string, num start, num NumChars)`

* řetězec: hello tooreturn znaky řetězce z
* spustit: číslo určující počáteční pozici v tooreturn znaky řetězce z hello
* NumChars: číslo určující počet hello tooreturn znaků z pozice v řetězci

**Poznámky:**  
Vrátí numChars znaků od počáteční pozice v řetězci.  
Řetězec obsahující numChars znaků od počáteční pozice v řetězci:

* Pokud numChars = 0, vrátí prázdný řetězec.
* Pokud numChars < 0, vrátí vstupní řetězec.
* Pokud spuštění > Dobrý den délky řetězce, vrátí vstupní řetězec.
* Pokud spuštění < = 0, vrátí vstupní řetězec.
* Pokud má řetězec hodnotu null, vrátí prázdný řetězec.

Pokud nejsou k dispozici numChar znaků zbývající v řetězci od počáteční pozice, jako jsou vráceny nejdříve mnoho znaků.

**Příklad:**  
`Mid("John Doe", 3, 5)`  
Vrátí "hn proveďte".

`Mid("John Doe", 6, 999)`  
Vrátí "Doe"

- - -
### <a name="now"></a>Nyní
**Popis:**  
Hello nyní funkce vrací hodnotu DateTime zadání hello aktuální datum a čas, podle tooyour počítače systémové datum a čas.

**Syntaxe:**  
`dt Now()`

- - -
### <a name="numfromdate"></a>NumFromDate
**Popis:**  
Hello NumFromDate funkce vrátí datum ve formátu data na AD.

**Syntaxe:**  
`num NumFromDate(dt value)`

**Příklad:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Vrátí 129699324000000000

- - -
### <a name="padleft"></a>padLeft
**Popis:**  
Hello PadLeft funkce doleva-dotyková zařízení řetězec tooa Zadaná délka pomocí zadané odsazení znaku.

**Syntaxe:**  
`str PadLeft(str string, num length, str padCharacter)`

* řetězec: hello toopad řetězec.
* Délka: celé číslo představující hello potřeby délku řetězce.
* padCharacter: řetězec, který se skládá z jednoho znaku toouse jako znak odsazení hello

**Poznámky:**

* Pokud hello délka řetězce je menší než délka, pak padCharacter je opakovaně připojením toohello začátku (vlevo) řetězce, dokud má stejné toolength délka.
* padCharacter může být znak mezery, ale jeho nesmí mít hodnotu null.
* Pokud hello délka řetězce je rovna tooor větší než délka, je vrácen řetězec s beze změny.
* Pokud řetězec má délku větší než nebo rovna toolength, je vrácena identické toostring řetězec.
* Pokud hello délka řetězce je menší než délka, nové řetězec hello potřeby délka je vrácena obsahující odsazenou s padCharacter řetězec.
* Pokud má řetězec hodnotu null, funkce hello vrací prázdný řetězec.

**Příklad:**  
`PadLeft("User", 10, "0")`  
Vrátí "000000User".

- - -
### <a name="padright"></a>PadRight –
**Popis:**  
Hello PadRight – funkce vpravo-dotyková zařízení řetězec tooa Zadaná délka pomocí zadané odsazení znaku.

**Syntaxe:**  
`str PadRight(str string, num length, str padCharacter)`

* řetězec: hello toopad řetězec.
* Délka: celé číslo představující hello potřeby délku řetězce.
* padCharacter: řetězec, který se skládá z jednoho znaku toouse jako znak odsazení hello

**Poznámky:**

* Pokud hello délka řetězce je menší než délka, pak padCharacter je opakovaně připojením toohello konci (vpravo) řetězec, dokud má stejné toolength délka.
* padCharacter může být znak mezery, ale jeho nesmí mít hodnotu null.
* Pokud hello délka řetězce je rovna tooor větší než délka, je vrácen řetězec s beze změny.
* Pokud řetězec má délku větší než nebo rovna toolength, je vrácena identické toostring řetězec.
* Pokud hello délka řetězce je menší než délka, nové řetězec hello potřeby délka je vrácena obsahující odsazenou s padCharacter řetězec.
* Pokud má řetězec hodnotu null, funkce hello vrací prázdný řetězec.

**Příklad:**  
`PadRight("User", 10, "0")`  
Vrátí "User000000".

- - -
### <a name="pcase"></a>PCase
**Popis:**  
Převede hello první znak každého slova mezerami v případě tooupper řetězec Hello PCase funkce a všechny ostatní znaky jsou převedeny toolower případu.

**Syntaxe:**  
`String PCase(string)`

**Poznámky:**

* Tato funkce nenabízí aktuálně tooconvert správné velká a malá písmena slovo, které je zcela velká, jako je například zkratka.

**Příklad:**  
`PCase("TEsT")`  
Vrátí hodnotu "Test".

`PCase(LCase("TEST"))`  
Vrátí "Test"

- - -
### <a name="randomnum"></a>RandomNum
**Popis:**  
Hello RandomNum funkce vrátí náhodné číslo o zadaný interval.

**Syntaxe:**  
`num RandomNum(num start, num end)`

* spustit: číslo identifikující hello nižší limit toogenerate náhodná hodnota hello
* end: číslo identifikující hello horní limit toogenerate náhodná hodnota hello

**Příklad:**  
`Random(100,999)`  
734 může vrátit.

- - -
### <a name="removeduplicates"></a>Removeduplicates –
**Popis:**  
Hello removeduplicates – funkce přebírá řetězec s více hodnotami a ujistěte se, že každá hodnota je jedinečný.

**Syntaxe:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Příklad:**  
`RemoveDuplicates([proxyAddresses])`  
Vrátí atribut upravený proxyAddress, kde byly odebrány všechny duplicitní hodnoty.

- - -
### <a name="replace"></a>Nahradit
**Popis:**  
Hello funkce Nahradit nahradí všechny výskyty řetězce tooanother řetězec.

**Syntaxe:**  
`str Replace(str string, str OldValue, str NewValue)`

* řetězec: řetězec tooreplace hodnoty v.
* OldValue: hello řetězec toosearch pro a tooreplace.
* NewValue: tooreplace řetězec hello k.

**Poznámky:**  
Funkce Hello rozpozná hello následující zvláštní monikery:

* \n – nový řádek
* \r – znaky CR
* \t – karta

**Příklad:**  
`Replace([address],"\r\n",", ")`  
Nahradí Line FEED čárkami a místo a to by mohlo vést příliš "Jeden Microsoft způsob, Redmond, WA, USA"

- - -
### <a name="replacechars"></a>ReplaceChars
**Popis:**  
Hello ReplaceChars funkce nahradí všechny výskyty znaků v hello ReplacePattern řetězec nalezen.

**Syntaxe:**  
`str ReplaceChars(str string, str ReplacePattern)`

* řetězec: tooreplace řetězec znaků.
* ReplacePattern: řetězec obsahující slovník s tooreplace znaků.

Formát Hello je {zdroj1}: {target1}, {zdroj2}: {target2}, {zdrojN}, {targetN} Pokud je zdrojem hello znak toofind a cíle hello řetězec tooreplace s.

**Poznámky:**

* Funkce Hello použije všechny výskyty definované zdroje a nahradí hello cíle.
* Zdroj Hello musí mít přesně jeden znak (unicode).
* Hello zdroj nesmí být prázdný nebo delší než jeden znak (Chyba analýzy).
* cíl Hello může mít více znaků, například ö:oe, β:ss.
* cíl Hello může být prázdný, která určuje, že má být odebrána hello znak.
* Zdroj Hello je malá a velká písmena a musí být přesně shodovat.
* Hello, (čárkou) a: (dvojtečka) jsou vyhrazené znaky a nelze jej nahradit pomocí této funkce.
* Mezery a další bílé znaky v řetězci ReplacePattern hello se ignorují.

**Příklad:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Vrátí Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Vrací "ONeil", jeden značek hello je definovaný toobe odebrat.

- - -
### <a name="right"></a>Vpravo
**Popis:**  
Dobrý den správné funkce vrátí zadaný počet znaků z hello vpravo (koncového) řetězce.

**Syntaxe:**  
`str Right(str string, num NumChars)`

* řetězec: hello tooreturn znaky řetězce z
* NumChars: číslo určující počet hello tooreturn znaky z hello konci (vpravo) řetězce

**Poznámky:**  
Od poslední pozice hello řetězce jsou vráceny NumChars znaky.

Řetězec obsahující hello poslední numChars znaků v řetězci:

* Pokud numChars = 0, vrátí prázdný řetězec.
* Pokud numChars < 0, vrátí vstupní řetězec.
* Pokud má řetězec hodnotu null, vrátí prázdný řetězec.

Pokud řetězec obsahuje menší počet znaků, než číslo zadané v NumChars text hello, vrátí se identické toostring řetězec.

**Příklad:**  
`Right("John Doe", 3)`  
Vrátí "Doe".

- - -
### <a name="rtrim"></a>RTrim
**Popis:**  
Hello funkce RTrim odebere koncové prázdné znaky v řetězci.

**Syntaxe:**  
`str RTrim(str value)`

**Příklad:**  
`RTrim(" Test ")`  
Vrátí hodnotu "Test".

- - -
### <a name="select"></a>Vyberte
**Popis:**  
Proces všech hodnot v více hodnot atributů (nebo výstupní výrazu) založený na zadanou funkci.

**Syntaxe:**  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* Položka: reprezentuje element v hello vícehodnotového atributu
* Atribut: hello vícehodnotového atributu
* výraz: výraz, který vrátí kolekce hodnot
* podmínky: žádné funkce, který dokáže zpracovat položku v atributu hello

**Příklady:**  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
Vrátí všechny hodnoty hello v hello více hodnot atributů otherPhone po odebrání pomlčky (-).

- - -
### <a name="split"></a>Rozdělení
**Popis:**  
Hello funkce rozdělení přebírá řetězec oddělené s oddělovačem a udělá z něj řetězec s více hodnotami.

**Syntaxe:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* hodnota: hello řetězec s tooseparate znak oddělovače.
* oddělovač: jeden znak toobe používá jako oddělovač hello.
* limit: maximální počet hodnot, které můžou vrátit.

**Příklad:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Vrátí řetězec s více hodnotami s 2 elementy užitečné pro atribut proxyAddress hello.

- - -
### <a name="stringfromguid"></a>StringFromGuid
**Popis:**  
Hello funkce StringFromGuid trvá binární GUID a převede jej tooa řetězec

**Syntaxe:**  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a>StringFromSid
**Popis:**  
Hello StringFromSid funkce převede bajtové pole obsahující řetězce tooa identifikátor zabezpečení.

**Syntaxe:**  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a>Přepínač
**Popis:**  
Přepínač Funkce Hello je použité tooreturn jednu hodnotu na základě vyhodnocených podmínek.

**Syntaxe:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* Expr: výraz typu Variant chcete tooevaluate.
* hodnota: hodnota toobe vrátit, pokud je hello odpovídající výraz hodnotu True.

**Poznámky:**  
Hello argument funkce přepínač seznam se skládá z dvojic výrazy a hodnoty. z levé tooright se vyhodnotí výrazy Hello a je vrácena hodnota hello přidružené hello první výraz tooevaluate tooTrue. Pokud hello částí nejsou spárovány správně, dojde k chybě spuštění.

Například pokud Výraz1 hodnotu True, vrátí přepínač value1. Pokud výraz-1 je False, ale výraz 2 má hodnotu True, vrátí přepínač hodnotu 2 a tak dále.

Přepínač vrátí žádnou pokud:

* Žádný z výrazů hello mají hodnotu True.
* První výraz True Hello má odpovídající hodnotu, která má hodnotu Null.

Přepínač vyhodnotí všechny výrazy, i když vrací pouze jeden z nich. Z tohoto důvodu je třeba dávat pozor na nežádoucí vedlejší účinky. Například pokud hello vyhodnocení jakýkoli výraz výsledkem dělení nulou, dojde k chybě.

Hodnota může být také hello Chyba funkce, která by vrátit vlastní řetězec.

**Příklad:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Vrátí hello jazyk používaný v některé hlavní města, jinak vrátí chybu.

- - -
### <a name="trim"></a>Uvolnění dočasné paměti
**Popis:**  
funkce Trim Hello odebere úvodní a koncové mezery z řetězce.

**Syntaxe:**  
`str Trim(str value)`  

**Příklad:**  
`Trim(" Test ")`  
Vrátí hodnotu "Test".

`Trim([proxyAddresses])`  
Odebere úvodní a koncové mezery pro každou hodnotu v atributu proxyAddress hello.

- - -
### <a name="ucase"></a>UCase
**Popis:**  
Hello funkce UCase převede všechny znaky v případě tooupper řetězec.

**Syntaxe:**  
`str UCase(str string)`

**Příklad:**  
`UCase("TeSt")`  
Vrátí hodnotu "TEST".

- - -
### <a name="where"></a>kde

**Popis:**  
Vrátí podmnožinu hodnoty z více hodnot atributů (nebo výstupní výrazu) na základě konkrétní podmínky.

**Syntaxe:**  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* Položka: reprezentuje element v hello vícehodnotového atributu
* Atribut: hello vícehodnotového atributu
* podmínky: žádné výraz, který lze vyhodnotit tootrue, nebo hodnotu NEPRAVDA
* výraz: výraz, který vrátí kolekce hodnot

**Příklad:**  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
Návratové hodnoty hello certifikátu v userCertificate hello více hodnot atributů, které nejsou vypršela platnost.

- - -
### <a name="with"></a>S
**Popis:**  
Hello pomocí funkce poskytuje způsob toosimplify pomocí proměnné toorepresent dílčím výrazu, která se zobrazí jeden nebo více časy ve výrazu komplexní hello složitý výraz.

**Syntaxe:**
`With(var variable, exp subExpression, exp complexExpression)`  
* Proměnná: představuje hello dílčím výrazu.
* dílčím výrazu: reprezentována proměnná dílčím výrazu.
* complexExpression: složitý výraz.

**Příklad:**  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
Je funkčně srovnatelný:  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
Která vrací pouze hodnoty neprošlé certifikátu v atributu userCertificate hello.


- - -
### <a name="word"></a>Word
**Popis:**  
Hello Word funkce vrátí slovo obsažené v řetězci, na základě parametrů popisující hello oddělovače toouse a číslo tooreturn hello aplikace word.

**Syntaxe:**  
`str Word(str string, num WordNumber, str delimiters)`

* řetězec: hello řetězec tooreturn slova.
* WordNumber: by měl vrátit číslo identifikující číslo, které aplikace word.
* oddělovače: řetězec představující hello delimiter(s), která má být použít tooidentify slova

**Poznámky:**  
Jednotlivé řetězce znaků v řetězci oddělených hello jeden hello znaků v oddělovače jsou označeny jako slova:

* Pokud počet < 1, vrátí prázdný řetězec.
* Pokud má řetězec hodnotu null, vrátí prázdný řetězec.

Pokud řetězec obsahuje méně než číslo slova nebo řetězec neobsahuje žádné slova se identifikovanou pomocí oddělovače, je vrácen prázdný řetězec.

**Příklad:**  
`Word("hello quick brown fox",3," ")`  
Vrátí "hnědá"

`Word("This,string!has&many separators",3,",!&#")`  
Vrátí "má"

## <a name="additional-resources"></a>Další zdroje
* [Principy výrazů deklarativního zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD Connect Sync: Možnosti přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
