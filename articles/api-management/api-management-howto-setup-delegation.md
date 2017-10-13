---
title: "Pro delegování uživatele registrace a produktu předplatného"
description: "Zjistěte, jak delegovat uživatele registrace a produktu předplatné třetí straně ve službě Azure API Management."
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
ms.openlocfilehash: 2637ab6405f2d4ea1da84981295a144874dfa4f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Pro delegování uživatele registrace a produktu předplatného
Delegování umožňuje použít existující web pro zpracování vývojáře sign v nebo registrace-množství a předplatné produkty oproti pomocí integrované funkce v portálu pro vývojáře. To umožňuje webu do vlastní data uživatele a provést ověření z těchto kroků vlastní způsobem.

## <a name="delegate-signin-up"></a>Delegování vývojáře přihlášení a odhlášení
Delegovat vývojáře přihlášení a odhlášení na existující web budete muset vytvořit koncový bod speciální delegování na váš web, který funguje jako vstupní bod pro takové žádosti inicializována z portálu pro vývojáře API Management.

Poslední pracovní postup bude takto:

1. Vývojáře klikne na odkaz přihlášení nebo registraci na portálu pro vývojáře API Management
2. Prohlížeč je přesměrován na koncový bod delegování
3. Koncový bod delegování na oplátku přesměruje na nebo uvede požadavek uživatele pro přihlášení nebo registraci uživatelského rozhraní
4. V případě úspěchu je uživatel přesměrován zpět na stránky portálu API Management vývojáře, které se spouští z

Pokud chcete začít, můžeme první nastavení API Management pro směrování požadavků prostřednictvím delegování koncový bod. Na portál vydavatele služby API Management, klikněte na **zabezpečení** a klikněte **delegování** kartě. Klikněte na zaškrtávací políčko Povolit delegáta přihlášení a registrace.

![Stránku delegování][api-management-delegation-signin-up]

* Rozhodněte, jaké adresu URL vašeho koncového bodu speciální delegování bude a zadejte ho v **adresu URL koncového bodu delegování** pole. 
* V rámci **delegování ověřovací klíč** pole zadejte tajný klíč, který se použije k výpočtu podpisu, které jste pro kontroluje, zda požadavek pochází skutečně z Azure API Management získali. Můžete kliknout na **generovat** tlačítko tak, aby měl stav rozhraní API náhodně Generovat klíč pro vás.

Teď je potřeba vytvořit **koncový bod delegování**. Je třeba provést několik akcí:

1. Zobrazit žádost o následující tvar:
   
   > *http://www.yourwebsite.com/apimdelegation?Operation=SignIn&returnUrl= {URL zdrojové stránky} & salt = {řetězec} & sig = {řetězec}*
   > 
   > 
   
    Parametry dotazu pro případ přihlášení / registrace:
   
   * **operace**: Určuje, jaký typ žádosti o delegování je – lze pouze **přihlášení** v tomto případě
   * **returnUrl**: adresa URL stránky, kde uživatel klepl na přihlášení nebo registraci propojení
   * **řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení
   * **SIG**: hodnotu hash počítaný zabezpečení, který se má použít pro porovnání se vlastní Vypočítaný algoritmus hash
2. Ověřte, zda požadavek pochází z Azure API Management, (volitelný, ale důrazně doporučujeme pro zabezpečení)
   
   * Výpočetní HMAC SHA512 hash řetězce na základě **returnUrl** a **řetězce salt** parametrů dotazu ([příklad kódu níže uvedenou]):
     
     > Metoda HMAC (**řetězce salt** + '\n' + **returnUrl**)
     > 
     > 
   * Porovnat hodnotu výše vypočítanou na hodnotu **sig** parametr dotazu. Pokud dva klíče hash shodují, přejít k dalšímu kroku, v opačném případě daný požadavek je odepřen.
3. Ověřte, zda jsou přijímají požadavek na přihlášení v nebo přihlašovací up: **operace** parametr dotazu bude nastavena na "**přihlášení**".
4. Uživateli zprostředkovali uživatelského rozhraní pro přihlášení nebo registraci
5. Pokud je uživatel zaregistrovali budete muset vytvořit účet odpovídající pro ně ve službě API Management. [Vytvoření uživatele] pomocí rozhraní API REST API pro správu. Když to uděláte, zkontrolujte, že je nastavená ID uživatele stejná, který je v úložišti uživatele nebo ID, které můžete sledovat určité.
6. Když je uživatel úspěšně ověřen:
   
   * [žádosti o token jednotného přihlašování (SSO)] prostřednictvím rozhraní API REST API pro správu
   * parametr dotazu returnUrl připojte k adrese URL jednotné přihlašování jste dostali od volání rozhraní API výše:
     
     > například https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * přesměruje uživatele na výše uvedenou adresu URL vytvořené

Kromě **přihlášení** operace, můžete také provést správy účtů podle předchozího postupu a pomocí některého z následujících operací.

* **Změna hesla byla**
* **ChangeProfile**
* **CloseAccount**

Musíte zadat následující parametry dotazu pro operace správy účtů.

* **operace**: Určuje, jaký typ žádosti o delegování je (Změna hesla byla, ChangeProfile nebo CloseAccount)
* **ID uživatele**: id uživatele účtu pro správu
* **řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení
* **SIG**: hodnotu hash počítaný zabezpečení, který se má použít pro porovnání se vlastní Vypočítaný algoritmus hash

## <a name="delegate-product-subscription"></a>Delegování odběru produktu
Delegování odběru produktů funguje podobně delegování uživatele přihlášení /-up. Poslední pracovní postup bude následující:

1. Vývojář vybere produktu v portálu pro vývojáře API Management a klikne na tlačítko přihlásit k odběru
2. Prohlížeč je přesměrován na koncový bod delegování
3. Koncový bod delegování provede kroky požadované produktu předplatné – to je na vás a může mít za následek přesměrování na jinou stránku k požadavku na fakturační informace klást další otázky, nebo jednoduše ukládání informací a které nevyžadují žádné akce uživatele

K povolení funkcí, na **delegování** klikněte na stránce **delegovat odběru produktů**.

Potom zkontrolujte, že koncový bod delegování provede následující akce:

1. Zobrazit žádost o následující tvar:
   
   > *{operaci} http://www.yourwebsite.com/apimdelegation?Operation= & productId = {produktu pro přihlášení k odběru} & userId = {uživatele, který vytvořil požadavek} & řetězce salt = {řetězec} & sig = {řetězec}*
   > 
   > 
   
    Parametry dotazu pro případ odběru produktu:
   
   * **operace**: Určuje, jaký typ žádosti o delegování je. Pro odběr produktů požadavky platné možnosti jsou:
     * "Přihlásit": požadavek na přihlášení k odběru uživateli daný produkt zadané ID (viz níže)
     * "Zrušit": požadavek na odhlášení uživatele z produktu
     * "Obnovit": žádost o obnovení předplatného, (například to může být vypršení platnosti)
   * **productId**: ID uživatel si vyžádal, přihlaste se k odběru produktu
   * **ID uživatele**: ID uživatele, pro kterého je žádosti
   * **řetězce Salt**: speciální řetězec salt používat pro výpočty hash zabezpečení
   * **SIG**: hodnotu hash počítaný zabezpečení, který se má použít pro porovnání se vlastní Vypočítaný algoritmus hash
2. Ověřte, zda požadavek pochází z Azure API Management, (volitelný, ale důrazně doporučujeme pro zabezpečení)
   
   * Výpočetní SHA512 HMAC řetězce na základě **productId**, **userId** a **řetězce salt** parametrů dotazu:
     
     > Metoda HMAC (**řetězce salt** + '\n' + **productId** + '\n' + **userId**)
     > 
     > 
   * Porovnat hodnotu výše vypočítanou na hodnotu **sig** parametr dotazu. Pokud dva klíče hash shodují, přejít k dalšímu kroku, v opačném případě daný požadavek je odepřen.
3. Proveďte jakékoli produktu zpracování odběru na základě typu požadovanou v operaci **operace** – například fakturace, další otázky, atd.
4. Na přihlášení uživatele k produktu na vaší straně úspěšně odběru, přihlášení k odběru uživateli produktů pro správu rozhraní API pomocí [volání rozhraní REST API pro odběr produktů].

## <a name="delegate-example-code"></a> Příklad kódu
Tyto ukázky kódu ukazují, jak provést *delegování ověřovací klíč*, který je nastaven na obrazovce delegování portálu vydavatele, chcete-li vytvořit HMAC, které se pak použije k ověření podpisu, prokázání platnost předaný returnUrl . Stejný kód funguje pro productId a ID uživatele se mírně upraven.

**Kód jazyka C# pro generování hash returnUrl**

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
}
```

**Kód NodeJS ke generování hash returnUrl**

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature to sig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a>Další kroky
Další informace o delegování najdete v následujícím videu.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[žádosti o token jednotného přihlašování (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409
[vytvoření uživatele]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[volání rozhraní REST API pro odběr produktů]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[příklad kódu níže uvedenou]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
