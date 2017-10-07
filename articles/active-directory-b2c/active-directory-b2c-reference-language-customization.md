---
title: "Azure Active Directory B2C: Jazyk přizpůsobení pomocí | Microsoft Docs"
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: sama
ms.openlocfilehash: a8e4037014f5adf929dac7b5dc4db72ba0f5b292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Přizpůsobení jazyka pomocí

>[!NOTE] 
>Tato funkce je ve verzi public preview.  Doporučuje se použít testovacím klientem, při použití této funkce.  Plánujeme není na všechny nejnovějších změn z verze obecné dostupnosti toohello hello preview, ale jsme rezervovat správné toomake hello takové funkce hello tooimprove změn.  Až využijete funkce tootry příležitosti, zadejte zpětnou vazbu pro vaše prostředí a jak bychom mohli je lepší.  Nástroj řez emotikona hello v pravé horní hello můžete poskytnout zpětnou vazbu prostřednictvím hello portálu Azure.   Pokud je za provozu pomocí této funkce preview fázi hello obchodním požadavkem pro jste toogo, dejte nám vědět, vaše scénáře a jsme vám může poskytnout hello správné informace a pomoc.  Obraťte se na nás na adrese [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).

Přizpůsobení jazyka umožňuje toochange uživatelů přepravě toosuit tooa jiným jazykem, musí zákazník.  Poskytujeme překladů pro 36 jazyků (viz [Další informace o](#additional-information)).  I když prostředí je k dispozici pouze pro jeden jazyk, můžete přizpůsobit jakýkoli text na hello stránky toosuit vašim potřebám.  

## <a name="how-does-language-customization-work"></a>Jak funguje jazyk přizpůsobení?
Přizpůsobení jazyka umožňuje tooselect jazyky, které vám dobře slouží uživatele je k dispozici v.  Jakmile je povolená funkce hello, můžete zadat parametr řetězce dotazu hello, ui_locales, z vaší aplikace.  Při volání do Azure AD B2C, jsme převede toohello stránky národního prostředí, která jste.  Pomocí typu konfigurace vám poskytuje úplnou kontrolu nad hello jazyky vám dobře slouží uživatele a ignoruje hello jazykové nastavení prohlížeče hello zákazníka. Můžete taky nemusí být nutné tuto úroveň kontroly nad jaké jazyky, najdete v části vašich zákazníků.  Pokud nezadáte parametr ui_locales, zkušeností zákazníků hello je závisí na nastavení svého prohlížeče.  Můžete řídit který jazyků, se vám dobře slouží uživatele přeložit tooby ji přidáte jako podporovaný jazyk.  Pokud prohlížeč zákazníka je sada tooshow jazyk nechcete, aby toosupport pak hello jazyk, který jste vybrali jako výchozí v podporovaných jazykových verzí se zobrazí místo.

1. **Zadaný jazyk uživatelského rozhraní – národní prostředí** -Jakmile povolíte jazyk přizpůsobení, vám dobře slouží uživatele je jazyk přeložený toohello zde zadané
2. **Požadovaný jazyk prohlížeče** – Pokud nebyly zadány žádné uživatelské rozhraní národní prostředí, převádí toohello prohlížeče požadovaný jazyk, **Pokud byl součástí podporované jazyky**
3. **Výchozí jazyk zásad** – Pokud prohlížeč hello neurčuje jazyk nebo Určuje jednu, která není podporována, převádí toohello zásad výchozí jazyk

>[!NOTE]
>Pokud používáte vlastní uživatelské atributy, je třeba tooprovide vlastní překladů. Najdete v části "[přizpůsobit vaší řetězce](#customize-your-strings)' Podrobnosti.
>

## <a name="support-uilocales-requested-languages"></a>Podpora ui_locales požadované jazyky 
Když zapnete jazyk přizpůsobení na zásadu, nyní je možné určit hello jazyk uživatele cesty hello přidáním parametru ui_locales hello.
1. [Postupujte podle těchto kroků toonavigate toohello okno s funkcemi B2C na hello portálu Azure.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Přejděte tooa zásady, že chcete tooenable pro překlad.
3. Klikněte na tlačítko **jazyk přizpůsobení**.
4. Přečtěte si upozornění pečlivě hello.  Jakmile bude povoleno, nelze vypnout, přizpůsobení jazyk'.
5. Zapněte funkci hello a klikněte na tlačítko **OK**.
6. Uložit vaše zásady na hello levého horního rohu vaše **upravit zásady** okno.

## <a name="select-which-languages-your-user-journey-supports"></a>Vyberte jazyky, které podporuje vaše uživatelské cesty 
Vytvoří seznam povolených jazyků pro vaše uživatele cesty toobe přeložený do, pokud není zadán parametr ui_locales hello.
1. Zajistěte, aby byl jazyk přizpůsobení z předchozích pokynů zapnout vaše zásady.
2. Z vaší **upravit zásady** vyberte **jazyk přizpůsobení**.
3. Budete přesměrováni tooyour **podporované jazyky** okno.  Tady můžete vybrat **přidat jazyk**.
4. Vyberte všechny jazyky hello, které byste chtěli toobe podporována.  

>[!NOTE]
>Pokud není zadaný parametr ui_locales, pak stránku hello je jazyk prohlížeče přeložený toohello zákazníka pouze v případě, že je v tomto seznamu
>

5. Klikněte na tlačítko **Ok** dolnímu hello
6. Zavřít hello **jazyk přizpůsobení** okno a **Uložit** vaše zásady.

## <a name="customize-your-strings"></a>Přizpůsobení vaší řetězce
'Jazyk přizpůsobení, vám umožní toocustomize žádné řetězce v vám dobře slouží uživatele.
1. Zajistěte, aby byl jazyk přizpůsobení z předchozích pokynů hello zapnout vaše zásady.
2. Z vaší **upravit zásady** vyberte **jazyk přizpůsobení**.
3. Hello levé navigační nabídce vyberte **stahování obsahu**.
4. Vyberte stránku hello chcete tooedit.
5. V rozevírací nabídce hello vyberte požadovaný tooedit pro jazyk hello.
6. Klikněte na **Stáhnout**. 

Tyto kroky poskytnout soubor JSON, které můžete použít toostart úpravy vaší řetězce.

### <a name="changing-any-string-on-hello-page"></a>Změna libovolného řetězce na stránce hello
1. Otevřete hello souboru JSON stáhnout z předchozích pokynů v editoru JSON.
2. Element hello najít chcete toochange.  Můžete najít hello `StringId` řetězce hello hledáte, nebo vyhledejte hello `Value` chcete toochange.
3. Aktualizace hello `Value` atributem, co chcete zobrazit.
4. Uložte soubor hello a odešlete své změny.

### <a name="changing-extension-attributes"></a>Změna atributů rozšíření
Pokud řetězec hello toochange hledáte vlastních uživatelský atribut, nebo má jeden toohello tooadd JSON, je ve formátu hello:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": false,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```
>[!IMPORTANT]
>Pokud potřebujete toooverride řetězec, ujistěte se, zda text hello tooset `Override` hodnota příliš`true`.  Pokud nedojde ke změně hello hodnotu, hello položka bude ignorována. 
>

Nahraďte `<ExtensionAttribute>` s názvem hello vaše vlastní uživatelské atributu.  
Nahraďte `<ExtensionAttributeValue>` s hello nové řetězec toobe zobrazí.

### <a name="using-localizedcollections"></a>Pomocí LocalizedCollections
Pokud chcete tooprovide sadu seznam hodnot pro odpovědi, je nutné toocreate `LocalizedCollections`.  A `LocalizedCollections` je pole `Name` a `Value` páry.  Hello `Name` je co se zobrazí a hello `Value` je co je vrácený v hello deklarace identity.  tooadd `LocalizedCollections`, má hello následující formát:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": false,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```
>[!IMPORTANT]
>Pokud potřebujete toooverride řetězec, ujistěte se, zda text hello tooset `Override` hodnota příliš`true`.  Pokud nedojde ke změně hello hodnotu, hello položka bude ignorována. 
>

* `ElementId`je hello uživatele atribut které tento `LocalizedCollections` je odpovědí na
* `Name`je uživatel uvedené toohello hodnota hello
* `Value`Co je vrácený v hello deklarace identity, pokud je vybraná tato možnost je

### <a name="upload-your-changes"></a>Odeslat změny
1. Po dokončení soubor JSON tooyour hello změny, přejděte zpět tooyour klienta B2C.
2. Z vaší **upravit zásady** vyberte **jazyk přizpůsobení**.
3. Hello levé navigační nabídce vyberte **nahrát obsah**.
4. Vyberte stránku hello, kterou chcete tooupload změny pro.
5. Pokud chcete, aby tooupload jazyk, který jste zadali dříve JSON pro, je třeba toodelete hello předchozí položce.  Chcete-li odstranit kliknutím hello `...` hello napravo od tento jazyk a vyberte **odstranit**.
6. Klikněte na tlačítko **přidat** na hello levého horního rohu.
7. Vyberte jazyk hello souboru JSON.
8. Klikněte na tlačítko složky hello na hello správné a vyhledejte souboru JSON.
9. Klikněte na tlačítko hello **nahrát** tlačítko na hello dolní části okna hello.
10. Přejděte zpět tooyour **upravit zásady** a klikněte na **Uložit**.

## <a name="additional-information"></a>Další informace
### <a name="recommendations-for-language-customization"></a>Doporučení pro 'jazyk přizpůsobení.
Doporučujeme, abyste jenom uvedení v položky tooyour jazyk prostředky pro řetězce je explicitně tooreplace chcete.  Jsme vynutit soubor toohello limit velikosti, který kompiluje mimo všechny JSON překladů.  Pokud vaše soubory příliš velký, ovlivní výkon hello vám dobře slouží uživatele.
### <a name="page-ui-customization-labels-are-removed-once-language-customization-is-enabled"></a>Popisky přizpůsobení uživatelského rozhraní stránky jsou odebrány, jakmile je povoleno, přizpůsobení jazyk.
Když povolíte 'Jazyk přizpůsobení', se odeberou všechny předchozí úpravy štítků, které používají přizpůsobení uživatelského rozhraní stránky s výjimkou vlastní uživatelské atributy.  Tato změna se provádí tooavoid konflikty, ve kterém můžete upravit vaše řetězce.  Tím, že nahrajete prostředků jazyka v 'jazyk přizpůsobení, můžou pokračovat toochange štítky a jiných řetězců.
### <a name="microsoft-is-committed-tooprovide-hello-most-up-to-date-translations-for-your-use"></a>Společnost Microsoft se potvrdí tooprovide hello nejaktuálnější překladů pro vaše použití
Jsme bude nepřetržitě zlepšit překlady a zůstanou v dodržování předpisů pro vás.  Jsme se identifikace chyb a změny v globálních terminologie a bezproblémově proveďte hello aktualizace, které budou fungovat v vám dobře slouží uživatele.
### <a name="support-for-right-to-left-languages"></a>Podpora jazyků zprava doleva
Neexistuje žádná podpora pro zprava doleva jazyky, pokud budete potřebovat, tato funkce hlasovat prosím pro tuto funkci na [zpětnou vazbu Azure](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Sociální překlady zprostředkovatele Identity
Poskytujeme hello ui_locales OIDC parametr toosocial přihlášení, ale není dodrženo některé poskytovateli sociální identity, včetně Facebook a Google. 
### <a name="browser-behavior"></a>Chování prohlížeče
Chrome a Firefox obě žádosti pro své jazykové sady a pokud je podporovaném jazyce, se zobrazí před výchozí hello.  V současné době Edge neuvede v požadavku jazyk a přejde přímých toohello výchozí jazyk.
### <a name="known-issues"></a>Známé problémy
* Odesílání prostředky jazyk pro stránku hello vícefaktorového ověřování v zásadách upravit profil není aktuálně dostupná.
* `LocalizedCollections`nejsou generované hodnoty, pokud to vyžaduje typ odpovědi hello
### <a name="what-if-i-want-a-language-that-isnt-supported"></a>Co když chci, aby jazyk, který není podporován?
Plánování jsme tooprovide rozšíření této funkce, která umožňuje tooupload prostředek JSON směrem "vlastní jazycích".  Hello funkce vám umožní toospecify hello název a jazyk kódu pro žádný jazyk a zadejte *všechny* hello překladů pro požadovaný jazyk.  Pokud potřebujete tuto funkci, pošlete nám na váš scénář [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).  

### <a name="what-languages-are-supported"></a>Jaké jazyky jsou podporovány?

| Jazyk              | Kód jazyka |
|-----------------------|---------------|
| Bengálském                | Bn            |
| čeština                 | cs            |
| dánština                | da            |
| němčina                | de            |
| řečtina                 | el            |
| Angličtina               | en            |
| španělština               | es            |
| finština               | fi            |
| francouzština                | fr            |
| Gudžarátština              | gu            |
| Hindština                 | Ahoj            |
| Chorvatština              | personální oddělení            |
| maďarština             | hu            |
| italština               | it            |
| japonština              | ja            |
| Kannadština               | KN            |
| korejština                | ko            |
| Malajalámština             | ml            |
| Maráthština               | MR            |
| Malajština                 | MS            |
| Norská Bokmål      | nb            |
| holandština                 | nl            |
| Paňdžábština               | Pa            |
| polština                | pl            |
| Portugalština – Brazílie   | pt-br         |
| Portugalština – Portugalsko | pt-pt         |
| rumunština              | ro            |
| ruština               | ru            |
| Slovenština                | Sk            |
| švédština               | sv            |
| Tamilština                 | tových            |
| Telugština                | te            |
| Thajština                  | tý            |
| turečtina               | tr            |
| -Čínština, zjednodušená čínština  | zh-hans       |
| Tradiční čínština – | zh-hant       |
