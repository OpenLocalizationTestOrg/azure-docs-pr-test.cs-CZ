---
title: "předplatné aaaHow toodelegate uživatele registrace a produktu"
description: "Zjistěte, jak toodelegate uživatele registrace a produktu předplatné tooa třetí strany ve službě Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 406648db2d2f37c4dcda466294726d331cc0551b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodelegate-user-registration-and-product-subscription"></a>Jak toodelegate uživatele registrace a produktu předplatného
Delegování vám umožní toouse vaší existující web pro vývojáře sign v nebo registrace-množství a předplatné tooproducts jako zpracování byl proti toousing hello integrovanou funkci v portálu pro vývojáře hello. To umožňuje webu tooown hello uživatelská data a provést ověření hello z těchto kroků vlastní způsobem.

## <a name="delegate-signin-up"></a>Delegování vývojáře přihlášení a odhlášení
toodelegate přihlášení a odhlášení tooyour existujícího webu pro vývojáře budete potřebovat toocreate koncový bod speciální delegování na váš web, který slouží jako hello vstupní bod pro takové žádosti inicializována z portálu pro vývojáře hello API Management.

Hello konečné pracovní postup bude takto:

1. Vývojář klikne na odkaz přihlášení nebo registraci hello v hello portál pro vývojáře API Management
2. Prohlížeč je koncový bod přesměrovaného toohello delegování
3. Koncový bod delegování na oplátku přesměruje tooor uvede uživatelského rozhraní dotazem uživatel toosign-in nebo registrace
4. V případě úspěchu je uživatel hello přesměrovaného back toohello API Management vývojáře stránky portálu, který se spouští z

toobegin, můžeme první tooroute API Management nastavení požadavků prostřednictvím delegování koncový bod. Na portálu vydavatele hello API Management, klikněte na **zabezpečení** a pak klikněte na tlačítko hello **delegování** kartě. Klikněte na zaškrtávací políčko tooenable hello delegáta přihlášení a registrace.

![Stránku delegování][api-management-delegation-signin-up]

* Rozhodněte, co bude hello URL speciální delegování koncový bod se a zadejte ho v hello **adresu URL koncového bodu delegování** pole. 
* V rámci hello **delegování ověřovací klíč** pole zadejte tajný klíč, který bude použité toocompute tooyou podpis zadaný pro ověření tooensure, který hello žádost skutečně pochází z Azure API Management. Můžete kliknout na hello **generovat** tlačítko toohave stav rozhraní API náhodně Generovat klíč pro vás.

Nyní je třeba toocreate hello **koncový bod delegování**. Tooperform má několik akcí:

1. V následující formulář hello zobrazit žádost:
   
   > *http://www.yourwebsite.com/apimdelegation?Operation=SignIn&returnUrl= {URL zdrojové stránky} & salt = {řetězec} & sig = {řetězec}*
   > 
   > 
   
    Parametry dotazu pro případ hello přihlášení / registrace:
   
   * **operace**: Určuje, jaký typ žádosti o delegování je – lze pouze **přihlášení** v tomto případě
   * **returnUrl**: hello URL hello stránky, kde uživatel hello použil odkaz přihlášení nebo registraci
   * **řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení
   * **SIG**: toobe hash počítaný zabezpečení použít pro porovnání tooyour vlastní Vypočítaný algoritmus hash
2. Ověřte, že tento požadavek hello přichází z Azure API Management (volitelný, ale důrazně doporučujeme pro zabezpečení)
   
   * Výpočetní HMAC SHA512 hash řetězec v závislosti na hello **returnUrl** a **řetězce salt** parametrů dotazu ([příklad kódu níže uvedenou]):
     
     > Metoda HMAC (**řetězce salt** + '\n' + **returnUrl**)
     > 
     > 
   * Porovnat hodnotu toohello výše počítaný hash hello hello **sig** parametr dotazu. Pokud hello dva klíče hash shodují, v dalším kroku toohello přesune, jinak hello požadavek je odepřen.
3. Ověřte, zda jsou přijímají požadavek na přihlášení v nebo přihlašovací up: hello **operace** parametr dotazu se nastaví příliš "**přihlášení**".
4. K dispozici hello uživatele pomocí uživatelského rozhraní v toosign nebo registrace
5. Pokud uživatel hello zaregistrovali máte toocreate odpovídající účet je ve službě API Management. [Vytvoření uživatele] s hello rozhraní API REST API pro správu. Když to uděláte, ujistěte se, abyste nastavili toohello ID uživatele hello stejné, který je v úložišti uživatele nebo tooan ID, které můžete sledovat určité.
6. Když uživatel hello úspěšně ověřen:
   
   * [žádosti o token jednotného přihlašování (SSO)] prostřednictvím hello rozhraní API REST API pro správu
   * Připojte toohello parametr dotazu returnUrl jednotné přihlašování adresu URL jste dostali od volání rozhraní API hello výše:
     
     > například https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * přesměrování hello uživatele toohello výše vytvořeného adresy URL

V přidání toohello **přihlášení** operace, můžete také provést správy účtů podle předchozích kroků hello a pomocí některého z hello následující operace.

* **Změna hesla byla**
* **ChangeProfile**
* **CloseAccount**

Je nutné předat hello následující parametry dotazu pro operace správy účtů.

* **operace**: Určuje, jaký typ žádosti o delegování je (Změna hesla byla, ChangeProfile nebo CloseAccount)
* **ID uživatele**: hello id uživatele účtu toomanage hello
* **řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení
* **SIG**: toobe hash počítaný zabezpečení použít pro porovnání tooyour vlastní Vypočítaný algoritmus hash

## <a name="delegate-product-subscription"></a>Delegování odběru produktu
Delegování odběru produktů funguje podobně toodelegating uživatele přihlášení /-up. poslední pracovního postupu Hello vypadat takto:

1. Vývojář vybere produktu v portálu pro vývojáře hello API Management a klikne na tlačítko přihlásit k odběru hello
2. Prohlížeč je koncový bod přesměrovaného toohello delegování
3. Koncový bod delegování provede kroky požadované produktu předplatné – to je tooyou a může mít za následek přesměrování stránky toorequest tooanother fakturační informace, klást další otázky, nebo jednoduše ukládání informací o hello a které nevyžadují žádné akce uživatele

Funkce hello tooenable, na hello **delegování** klikněte na stránce **delegovat odběru produktů**.

Potom zkontrolujte, že koncový bod delegování hello provede následující akce hello:

1. V následující formulář hello zobrazit žádost:
   
   > *http://www.yourwebsite.com/apimdelegation?Operation= {operaci} & productId = {produktu toosubscribe k} & userId = {uživatele, který vytvořil požadavek} & řetězce salt = {řetězec} & sig = {řetězec}*
   > 
   > 
   
    Parametry dotazu pro případ odběru produktu hello:
   
   * **operace**: Určuje, jaký typ žádosti o delegování je. Pro odběr produktů požadavky hello platné možnosti jsou:
     * "Přihlásit": žádost toosubscribe hello uživatele tooa zadaný produkt zadané ID (viz níže)
     * "Zrušit": žádost toounsubscribe uživatele z produktu
     * "Obnovit": žádost toorenew předplatné (například to může být vypršení platnosti)
   * **productId**: toosubscribe na požadovaný hello ID uživatele hello hello produktu
   * **ID uživatele**: hello ID hello uživatele, pro kterého hello požadavku
   * **řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení
   * **SIG**: toobe hash počítaný zabezpečení použít pro porovnání tooyour vlastní Vypočítaný algoritmus hash
2. Ověřte, že tento požadavek hello přichází z Azure API Management (volitelný, ale důrazně doporučujeme pro zabezpečení)
   
   * Výpočetní SHA512 HMAC řetězce podle hello **productId**, **userId** a **řetězce salt** parametrů dotazu:
     
     > Metoda HMAC (**řetězce salt** + '\n' + **productId** + '\n' + **userId**)
     > 
     > 
   * Porovnat hodnotu toohello výše počítaný hash hello hello **sig** parametr dotazu. Pokud hello dva klíče hash shodují, v dalším kroku toohello přesune, jinak hello požadavek je odepřen.
3. Proveďte jakékoli produktu zpracování odběru na základě typu hello požadovanou v operaci **operace** – například fakturace, další otázky, atd.
4. Na úspěšně přihlášení k odběru produktu toohello hello uživatele na vaší straně, přihlášení k odběru hello uživatele toohello API Management produktu podle [hello volání rozhraní REST API pro odběr produktů].

## <a name="delegate-example-code"></a> Příklad kódu
Tyto ukázky zobrazit code jak tootake hello *delegování ověřovací klíč*, který je nastaven na obrazovce delegování hello hello portálu vydavatele, toocreate klíčem HMAC, který je pak použit toovalidate hello podpis, prokázání hello platnost hello Předaný returnUrl. Dobrý den, kterou stejný kód pracuje hello productId a ID uživatele se mírně upraven.

**C# kódu toogenerate hash returnUrl**

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature toosig query parameter
}
```

**Kód toogenerate hash returnUrl NodeJS**

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature toosig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a>Další kroky
Další informace o delegování najdete v tématu hello následující videa.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[žádosti o token jednotného přihlašování (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409
[vytvoření uživatele]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[hello volání rozhraní REST API pro odběr produktů]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[příklad kódu níže uvedenou]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
