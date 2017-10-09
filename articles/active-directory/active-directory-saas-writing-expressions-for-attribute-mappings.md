---
title: "aaaWriting výrazy pro mapování atributů v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak toouse výraz mapování tootransform atribut hodnoty do formátu přijatelné během automatického zřizování objektů aplikace SaaS ve službě Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: b13c51cd-1bea-4e5e-9791-5d951a518943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: markvi
ms.openlocfilehash: caa0dd8144f6e5279a869e015ed75bd24169d585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Zapisují se výrazy pro mapování atributů v Azure Active Directory
Při konfiguraci zřizování aplikace SaaS tooa je jeden z typů hello mapování atributů, které můžete zadat mapování u výrazu. Pro tyto musíte napsat skript jako výraz, který vám umožní tootransform dat uživatelů do formátů, které jsou více přijatelné pro aplikace SaaS hello.

## <a name="syntax-overview"></a>Přehled syntaxe
Hello syntaxe pro výrazy pro mapování atributů je připomínající jazyka Visual Basic pro aplikace (VBA) funkce.

* Hello celý výraz musí být definován z hlediska funkcí, které se skládají z název, za nímž následují argumenty v závorce: <br>
  *%{FunctionName/ (<< argument 1 >>, <<argument N>>)*
* Funkce v sobě navzájem může vnořit. Například: <br> *FunctionOne (FunctionTwo (<<argument1>>))*
* Tři různé typy argumentů můžete předat do funkce:
  
  1. Atributy, které musí být uzavřena do odmocnina hranaté závorky. Příklad: [attributeName]
  2. Řetězcové konstanty, které musí být uzavřena do uvozovek. Například: "USA"
  3. Další funkce. Příklad: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))
* Pro řetězcové konstanty Pokud potřebujete zpětné lomítko (\) nebo uvozovky (") v řetězci hello ho, je nutné uvést symbolem hello zpětné lomítko (\). Například: "název společnosti: \"Contoso\""

## <a name="list-of-functions"></a>Seznam funkcí
[Připojit](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [připojení](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; [není](#not) &nbsp; &nbsp; &nbsp; &nbsp; [Nahradit](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [Přepínače](#switch)

- - -
### <a name="append"></a>Připojit
**Funkce:**<br> Append(Source, suffix)

**Popis:**<br> Přebírá hodnotu řetězce zdroje a připojí hello příponu toohello jeho konec.

**Parametry:**<br> 

| Name (Název) | Požadované / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **zdroj** |Požaduje se |Řetězec |Obvykle název atributu hello z hello zdrojového objektu |
| **přípona** |Požaduje se |Řetězec |Hello řetězce, které chcete tooappend toohello konec hello zdroj hodnoty. |

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Funkce:**<br> FormatDateTime (zdroj, inputFormat outputFormat.)

**Popis:**<br> Přebírá řetězec data z jednoho formátu a převede jej na do jiného formátu.

**Parametry:**<br> 

| Name (Název) | Požadované / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **zdroj** |Požaduje se |Řetězec |Obvykle název atributu hello z hello zdrojový objekt. |
| **inputFormat** |Požaduje se |Řetězec |Očekávaný formát hodnoty zdroj hello. Podporovaných formátů naleznete v části [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Požaduje se |Řetězec |Formát hello výstupní data. |

- - -
### <a name="join"></a>Spojit
**Funkce:**<br> Připojení (oddělovač, zdroj1, zdroj2,...)

**Popis:**<br> Join() je podobné tooAppend(), s tím rozdílem, že ho můžete kombinovat více **zdroj** hodnoty řetězce do jednoho řetězce, a všechny hodnoty oddělené bránou **oddělovače** řetězec.

Pokud jedna z hodnot zdroj hello je atribut s více hodnotami, každá hodnota v tento atribut bude připojený k společně, oddělených hello oddělovače hodnotu.

**Parametry:**<br> 

| Name (Název) | Požadované / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **Oddělovač** |Požaduje se |Řetězec |Řetězec používá tooseparate zdroj hodnoty, když jsou zřetězeny do jednoho řetězce. Může být "" Pokud žádné oddělovače je vyžadován. |
| ** zdroj1... zdrojN ** |Požadované proměnné počet pokusů |Řetězec |Řetězec toobe hodnoty, které jsou propojeny. |

- - -
### <a name="mid"></a>Mid –
**Funkce:**<br> Mid (zdroj, spuštění, délka)

**Popis:**<br> Vrátí dílčí řetězec hello zdroj hodnoty. Dílčí řetězec je řetězec, který obsahuje jenom některé z hello znaků z řetězce zdroj hello.

**Parametry:**<br> 

| Name (Název) | Požadované / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **zdroj** |Požaduje se |Řetězec |Obvykle název atributu hello. |
| **start** |Požaduje se |celé číslo |Index v hello **zdroj** řetězec, kde by se měl spustit dílčí řetězec. První znak v řetězci hello bude mít index 1, druhý znak bude mít index 2 a tak dále. |
| **Délka** |Požaduje se |celé číslo |Délka hello dílčí řetězec. Pokud délka skončí mimo hello **zdroj** řetězec, funkce vrátí dílčí řetězec z **spustit** indexu do konce **zdroj** řetězec. |

- - -
### <a name="not"></a>není
**Funkce:**<br> Not(Source)

**Popis:**<br> Převrátí hello logickou hodnotu hello **zdroj**. Pokud **zdroj** hodnota je "*True*", vrátí "*False*". Jinak vrátí "*True*".

**Parametry:**<br> 

| Name (Název) | Požadované / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **zdroj** |Požaduje se |Logická hodnota řetězce |Očekávaný **zdroje** jsou hodnoty "True" nebo "Nepravda"... |

- - -
### <a name="replace"></a>Nahradit
**Funkce:**<br> ObsoleteReplace (zdroj, oldValue, regexPattern, regexGroupName, zastaralá, replacementAttributeName, šablony)

**Popis:**<br>
Nahradí hodnoty v řetězci. Funguje jinak v závislosti na parametry hello zadané:

* Když **oldValue** a **zastaralá** jsou k dispozici:
  
  * Nahradí všechny výskyty oldValue ve zdroji hello zastaralá
* Když **oldValue** a **šablony** jsou k dispozici:
  
  * Nahradí všechny výskyty hello **oldValue** v hello **šablony** s hello **zdroj** hodnota
* Když **oldValueRegexPattern**, **oldValueRegexGroupName**, **zastaralá** jsou k dispozici:
  
  * Nahradí všechny hodnoty odpovídající oldValueRegexPattern v hello zdrojový řetězec s zastaralá
* Když **oldValueRegexPattern**, **oldValueRegexGroupName**, **replacementPropertyName** jsou k dispozici:
  
  * Pokud **zdroj** má hodnotu, **zdroj** je vrácen
  * Pokud **zdroj** nemá žádnou hodnotu, používá **oldValueRegexPattern** a **oldValueRegexGroupName** tooextract nahrazující hodnotou z vlastnosti hello s  **replacementPropertyName**. Nahrazující hodnotou se vrátí jako výsledek hello

**Parametry:**<br> 

| Name (Název) | Požadované / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **zdroj** |Požaduje se |Řetězec |Obvykle název atributu hello z hello zdrojový objekt. |
| **oldValue** |Nepovinné |Řetězec |Hodnota toobe ve **zdroj** nebo **šablony**. |
| **regexPattern** |Nepovinné |Řetězec |Vzor regulárního výrazu pro hello hodnotu toobe ve **zdroj**. Nebo, pokud je použita replacementPropertyName, vzor tooextract hodnotu z vlastnosti nahrazení. |
| **regexGroupName** |Nepovinné |Řetězec |Název skupiny hello uvnitř **regexPattern**. Jenom v případě, že se používá replacementPropertyName, jsme jako zastaralá z vlastnosti nahrazení extrahuje hodnotu této skupiny. |
| **Zastaralá** |Nepovinné |Řetězec |Nová hodnota tooreplace starý s. |
| **replacementAttributeName** |Nepovinné |Řetězec |Název toobe atribut hello používá se pro nahrazující hodnotou při zdroj nemá žádnou hodnotu. |
| **šablony** |Nepovinné |Řetězec |Když **šablony** je zadána hodnota, podíváme se **oldValue** uvnitř hello šablony a nahraďte ji metodou hodnota zdroje. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Funkce:**<br> StripSpaces(source)

**Popis:**<br> Odebere všechny mezery ("") znaky z hello zdroje řetězec.

**Parametry:**<br> 

| Name (Název) | Požadované / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **zdroj** |Požaduje se |Řetězec |**Zdroj** tooupdate hodnotu. |

- - -
### <a name="switch"></a>Přepínač
**Funkce:**<br> Přepínače (zdroj, defaultValue, key1, hodnota1, key2, hodnota2,...)

**Popis:**<br> Když **zdroj** hodnota odpovídá **klíč**, vrátí **hodnotu** pro tento **klíč**. Pokud **zdroj** hodnota se neshoduje se všechny klíče, vrátí **defaultValue**.  **Klíč** a **hodnotu** parametry musí vždy pocházejí v párech. Funkce Hello vždy očekává sudý počet parametrů.

**Parametry:**<br> 

| Name (Název) | Požadované / s opakováním | Typ | Poznámky |
| --- | --- | --- | --- |
| **zdroj** |Požaduje se |Řetězec |**Zdroj** tooupdate hodnotu. |
| **Výchozí hodnota** |Nepovinné |Řetězec |Výchozí hodnota toobe používá při zdroj neodpovídá žádné klíče. Může být prázdný řetězec (""). |
| **klíč** |Požaduje se |Řetězec |**Klíč** toocompare **zdroj** hodnotu s. |
| **Hodnota** |Požaduje se |Řetězec |Nahrazující hodnotou pro hello **zdroj** odpovídající klíč hello. |

## <a name="examples"></a>Příklady
### <a name="strip-known-domain-name"></a>Název domény známý pruhu
Je nutné toostrip známé doménu z uživatele e-mailu tooobtain uživatelské jméno. <br>
Například pokud hello doména "contoso.com", pak můžete použít následující výraz hello:

**Výraz:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Ukázkové vstup / výstup:** <br>

* **VSTUP** (e-mailu): "john.doe@contoso.com"
* **VÝSTUP**: "john.doe"

### <a name="append-constant-suffix-toouser-name"></a>Připojit konstantní toouser příponu
Pokud používáte izolovaného prostoru služby Salesforce, bude pravděpodobně nutné tooappend další přípony tooall uživatelská jména před provedením jejich synchronizace.

**Výraz:** <br>
`Append([userPrincipalName], ".test"))`

**Ukázka vstupu a výstupu:** <br>

* **VSTUP**: (userPrincipalName): "John.Doe@contoso.com"
* **VÝSTUP**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Generovat alias uživatele spojováním části křestní jméno a příjmení
Je nutné toogenerate alias uživatele provedením nejprve 3 písmena křestní jméno uživatele a prvních 5 písmena příjmení uživatele.

**Výraz:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Ukázka vstupu a výstupu:** <br>

* **VSTUP** (givenName): "Jan"
* **VSTUP** (Přezdívka): "Doe"
* **VÝSTUP**: "JohDoe"

### <a name="output-date-as-a-string-in-a-certain-format"></a>Výstupní data jako řetězec v určitém formátu
Chcete aplikaci SaaS tooa toosend kalendářních dat v určitém formátu. <br>
Například chcete tooformat data pro ServiceNow.

**Výraz:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Ukázka vstupu a výstupu:**

* **VSTUP** (extensionAttribute1): "20150123105347.1Z"
* **VÝSTUP**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Nahraďte hodnotu podle předdefinovanou sadu voleb
Je nutné toodefine hello časové pásmo hello uživatele na základě kódu stavu hello uložené ve službě Azure AD. <br>
Pokud kód stavu hello se neshoduje se některé z možností hello předdefinované, použijte výchozí hodnotu "Austrálie/Sydney".

**Výraz:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Ukázka vstupu a výstupu:**

* **VSTUP** (stav): "QLD"
* **VÝSTUP**: "Austrálie/Brisbane"

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Automatizace zřizování uživatelů nebo jeho rušení tooSaaS aplikace](active-directory-saas-app-provisioning.md)
* [Přizpůsobení mapování atributů pro zřizování uživatelů](active-directory-saas-customizing-attribute-mappings.md)
* [Filtry pro zřizování uživatelů oborů](active-directory-saas-scoping-filters.md)
* [Pomocí SCIM tooenable automatické zřizování uživatelů a skupin ze služby Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
* [Seznam kurzů tooIntegrate aplikace SaaS](active-directory-saas-tutorial-list.md)

