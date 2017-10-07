---
title: aaaAuthenticating a autorizaci s Power BI Embedded
description: "Ověřování a autorizace s Power BI Embedded"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 483ca0499e8d03584e1151d3d278c0179d4b8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Ověřování a autorizace s Power BI Embedded

Hello služby Power BI Embedded používá **klíče** a **tokeny aplikací** pro ověřování a autorizaci, namísto ověřování explicitní koncového uživatele. V tomto modelu aplikaci spravuje ověřování a autorizace pro koncové uživatele. Pokud je to nezbytné, vaše aplikace vytvoří a odešle tokeny hello aplikace, které informuje naše služby toorender hello požadovanou sestavu. Tento návrh nevyžaduje toouse vaše aplikace Azure Active Directory pro uživatele ověřování a autorizaci, i když stále můžete.

## <a name="two-ways-tooauthenticate"></a>Dva způsoby, jak tooauthenticate

**Klíč** -klíče můžete použít pro všechny Power BI Embedded volání rozhraní REST API. Hello klíčů lze nalézt v hello **portál Azure** kliknutím na **všechna nastavení** a potom **přístupové klíče**. Vždy považovat klíče, jako by šlo heslo. Tyto klíče mají oprávnění toomake jakéhokoli rozhraní API REST volání na kolekci konkrétní pracovní prostor.

toouse klíč na volání REST, přidejte následující autorizační hlavičky hello:            

    Authorization: AppKey {your key}

**Tokenu aplikace** -tokeny aplikací se používají pro všechny požadavky vnoření. Jsou to navrženou toobe spustit straně klienta, tak, aby s omezeným přístupem tooa jednotné sestavy a jeho osvědčených postupů tooset čas vypršení platnosti.

Jsou aplikace tokeny JWT (JSON Web Token), který je podepsaný pomocí jedné z vašich klíčů.

Váš token zabezpečení aplikace může obsahovat hello následující deklarace identity:

| Deklarovat | Popis |
| --- | --- |
| **ver** |verze Hello tokenu aplikace hello. 0.2.0 je aktuální verze hello. |
| **oblast** |Hello určené příjemce hello tokenu. Pro Power BI Embedded použití: "https://analysis.windows.net/powerbi/api". |
| **iss** |Řetězec označující hello aplikace, který vydal hello token. |
| **Typ** |Hello typ tokenu aplikace, který je právě vytvářena. Je aktuální hello podporován pouze typ **vložení**. |
| **WCN** |Pracovní prostor kolekce název hello token se vydává. |
| **interní databáze Windows** |Token hello ID pracovního prostoru se vydává. |
| **identifikátorů RID** |Sestava ID hello token se vydává. |
| **uživatelské jméno** (volitelné) |Používá se s RLS, to je řetězec, který vám pomůže zjistit hello uživatele při použití pravidla RLS. |
| **role** (volitelné) |Řetězec obsahující hello role tooselect při použití pravidla zabezpečení na úrovni řádků. Pokud předávání víc než jedné role, že mají být předány jako pole palčivost. |
| **spojovací bod služby** (volitelné) |Řetězec obsahující obory oprávnění hello. Pokud předávání víc než jedné role, že mají být předány jako pole palčivost. |
| **Exp** (volitelné) |Označuje hello dobu, ve které hello vypršení platnosti tokenu. Tyto mají být předány v jako systému Unix časová razítka. |
| **NBF** (volitelné) |Označuje hello dobu, ve které hello spustí token je platný. Tyto mají být předány v jako systému Unix časová razítka. |

Ukázkový token pro aplikace bude vypadat takto:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

Když dekódovat, bude vypadat přibližně takto:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

Nejsou k dispozici v rámci hello sady SDK, které usnadňují vytváření apptokens metody. Například pro rozhraní .NET můžete se podívat na hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) třídy a hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metody.

Pro hello .NET SDK, naleznete v části příliš[obory](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).

## <a name="scopes"></a>Obory

Pokud používáte vložení tokeny, může být vhodné toorestrict využití hello prostředků, které vám umožní získat přístup k. Z tohoto důvodu můžete vygenerovat token s vymezená oprávnění.

Hello následují hello k dispozici obory pro Power BI Embedded.

|Rozsah|Popis|
|---|---|
|Dataset.Read|Poskytuje oprávnění tooread hello zadaný datovou sadu.|
|Dataset.Write|Poskytuje oprávnění toowrite toohello zadaný datovou sadu.|
|Dataset.ReadWrite|Poskytuje oprávnění tooread a zápis toohello zadaný formát datové sady.|
|Report.Read|Poskytuje oprávnění tooview hello zadané sestavy.|
|Report.ReadWrite|Poskytuje oprávnění tooview a úpravy hello zadané sestavy.|
|Workspace.Report.Create|Poskytuje nové sestavy v rámci hello zadaný oprávnění toocreate pracovního prostoru.|
|Workspace.Report.Copy|Poskytuje oprávnění tooclone existující sestavy v rámci hello zadaný pracovního prostoru.|

Můžete zadat více oborů pomocí mezeru mezi obory hello jako následující hello.

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Požadované deklarace - oborů**

spojovací bod služby: scopesClaim {scopesClaim} může být buď řetězec nebo pole řetězců, poznamenat hello povolené oprávnění tooworkspace prostředky (sestavy, datové sady atd.)

Token dekódované s obory, které jsou definovány, vypadat podobně jako následující toohello.

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a>Operace a obory

|Operace|Cílový prostředek|Token oprávnění|
|---|---|---|
|Vytvoření nové sestavy založené na datovou sadu (v paměti).|Datová sada|Dataset.Read|
|Vytvoření nové sestavy založené na datovou sadu (v paměti) a hello sestavu uložit.|Datová sada|* Dataset.Read<br>* Workspace.Report.Create|
|Zobrazení a prozkoumat či upravit (v paměti) existující sestavy. Report.Read znamená Dataset.Read. To nebude povolovat toosave úpravy oprávnění.|Sestava|Report.Read|
|Upravit a uložit existující sestavy.|Sestava|Report.ReadWrite|
|Uložte kopii sestavy (Uložit jako).|Sestava|* Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-hello-flow-works"></a>Tady je Princip toku hello
1. Zkopírujte aplikace tooyour klíče rozhraní API hello. Můžete získat hello klíče **portálu Azure**.
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. Token vyhodnotí deklarace identity a má čas vypršení platnosti.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. Získá token podepsán s klíči přístup rozhraní API.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. Uživatel požádá o tooview sestavy.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. Token je ověřit s klíči přístup rozhraní API.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. Power BI Embedded odešle toouser sestavy.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

Po **Power BI Embedded** odešle uživatele toohello sestavy, hello uživatele můžete zobrazit sestavu hello ve vaší vlastní aplikaci. Například, pokud jste importovali hello [ukázkový soubor PBIX analýza dat prodej](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello ukázkovou webovou aplikaci bude vypadat takto:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Viz také

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Začínáme s ukázkou Microsoft Power BI Embedded](power-bi-embedded-get-started-sample.md)  
[Běžné scénáře Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Začínáme s Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[Úložiště Git PowerBI CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)

