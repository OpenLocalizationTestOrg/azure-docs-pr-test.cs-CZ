---
title: "Ověřování a autorizace s Power BI Embedded"
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
ms.openlocfilehash: 93027f0f5467e0b21c47bbcbc84c67cdd50b26b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Ověřování a autorizace s Power BI Embedded

Power BI Embedded používá služba **klíče** a **tokeny aplikací** pro ověřování a autorizaci, namísto ověřování explicitní koncového uživatele. V tomto modelu aplikaci spravuje ověřování a autorizace pro koncové uživatele. Pokud je to nezbytné, vaše aplikace vytvoří a odešle tokeny aplikací, která říká službě naši službu k vykreslení požadovanou sestavu. Tento návrh nevyžaduje vaše aplikace použít Azure Active Directory pro uživatele ověřování a autorizaci, i když stále můžete.

## <a name="two-ways-to-authenticate"></a>Dva způsoby ověření

**Klíč** -klíče můžete použít pro všechny Power BI Embedded volání rozhraní REST API. Klíče lze nalézt v **portál Azure** kliknutím na **všechna nastavení** a potom **přístupové klíče**. Vždy považovat klíče, jako by šlo heslo. Tyto klíče mají oprávnění k provádění jakéhokoli rozhraní API REST volání na kolekci konkrétní pracovní prostor.

Při volání REST pomocí klíče, přidejte následující hlavičku autorizace:            

    Authorization: AppKey {your key}

**Tokenu aplikace** -tokeny aplikací se používají pro všechny požadavky vnoření. Jsou navržené ke spuštění straně klienta, takže jste omezena na jednu sestavu a je vhodné nastavit čas vypršení platnosti.

Jsou aplikace tokeny JWT (JSON Web Token), který je podepsaný pomocí jedné z vašich klíčů.

Váš token zabezpečení aplikace může obsahovat následující deklarace identity:

| Deklarovat | Popis |
| --- | --- |
| **ver** |Verze tokenu aplikace. 0.2.0 je aktuální verze. |
| **oblast** |Zamýšlený příjemce tokenu. Pro Power BI Embedded použití: "https://analysis.windows.net/powerbi/api". |
| **iss** |Řetězec označující aplikace, který vydal token. |
| **Typ** |Typ tokenu aplikace, který je právě vytvářena. Aktuální se podporuje jen typ **vložení**. |
| **WCN** |Název kolekce prostoru token se vydává. |
| **interní databáze Windows** |ID pracovního prostoru token se vydává. |
| **identifikátorů RID** |ID sestavy token se vydává. |
| **uživatelské jméno** (volitelné) |Používá se s RLS, to je řetězec, který může pomoct identifikovat uživatele při použití pravidla RLS. |
| **role** (volitelné) |Řetězec obsahující rolí vyberte při použití pravidla zabezpečení na úrovni řádků. Pokud předávání víc než jedné role, že mají být předány jako pole palčivost. |
| **spojovací bod služby** (volitelné) |Řetězec obsahující obory oprávnění. Pokud předávání víc než jedné role, že mají být předány jako pole palčivost. |
| **Exp** (volitelné) |Označuje čas, kdy vyprší platnost tokenu. Tyto mají být předány v jako systému Unix časová razítka. |
| **NBF** (volitelné) |Označuje čas, ve kterém začíná token je platný. Tyto mají být předány v jako systému Unix časová razítka. |

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

Nejsou k dispozici v rámci sady SDK, které usnadňují vytváření apptokens metody. Například pro rozhraní .NET můžete prohlédnout [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) třídy a [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metody.

Pro .NET SDK, můžete se podívat do [obory](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).

## <a name="scopes"></a>Obory

Pokud používáte vložení tokeny, můžete omezit využití prostředků, které vám umožní získat přístup k. Z tohoto důvodu můžete vygenerovat token s vymezená oprávnění.

Níže jsou k dispozici obory pro Power BI Embedded.

|Rozsah|Popis|
|---|---|
|Dataset.Read|Poskytuje oprávnění ke čtení zadané sady dat.|
|Dataset.Write|Poskytuje oprávnění k zápisu do zadané sady dat.|
|Dataset.ReadWrite|Poskytuje oprávnění ke čtení a zápisu do zadaný formát datové sady.|
|Report.Read|Poskytuje oprávnění k zobrazení zadané sestavy.|
|Report.ReadWrite|Poskytuje oprávnění k zobrazení a úpravě zadané sestavy.|
|Workspace.Report.Create|Poskytuje oprávnění k vytvoření nové sestavy v rámci zadaného pracovního prostoru.|
|Workspace.Report.Copy|Poskytuje oprávnění ke klonování existující sestavy v rámci zadaného pracovního prostoru.|

Můžete zadat více oborů pomocí mezeru mezi obory podobně jako tento.

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Požadované deklarace - oborů**

spojovací bod služby: scopesClaim {scopesClaim} může být buď řetězec nebo pole řetězců, poznamenat povolené oprávnění k prostředkům prostoru (sestavy, datové sady atd.)

Token dekódované s obory, které jsou definovány, by vypadat podobně jako následující.

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
|Vytvoření nové sestavy založené na datovou sadu (v paměti) a sestavu uložit.|Datová sada|* Dataset.Read<br>* Workspace.Report.Create|
|Zobrazení a prozkoumat či upravit (v paměti) existující sestavy. Report.Read znamená Dataset.Read. To nebude povolovat oprávnění k uložení úpravy.|Sestava|Report.Read|
|Upravit a uložit existující sestavy.|Sestava|Report.ReadWrite|
|Uložte kopii sestavy (Uložit jako).|Sestava|* Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-the-flow-works"></a>Tady je Princip toku
1. Zkopírujte tyto klíče rozhraní API do vaší aplikace. Můžete získat klíče **portálu Azure**.
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. Token vyhodnotí deklarace identity a má čas vypršení platnosti.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. Získá token podepsán s klíči přístup rozhraní API.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. Uživatelské požadavky na Zobrazit sestavu.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. Token je ověřit s klíči přístup rozhraní API.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. Power BI Embedded odešle zprávu pro uživatele.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

Po **Power BI Embedded** zasílá sestav pro uživatele, uživatel může zobrazit sestavy ve vaší vlastní aplikaci. Například, pokud jste importovali [ukázkový soubor PBIX analýza dat prodej](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), ukázkovou webovou aplikaci bude vypadat takto:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Viz také

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Začínáme s ukázkou Microsoft Power BI Embedded](power-bi-embedded-get-started-sample.md)  
[Běžné scénáře Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Začínáme s Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[Úložiště Git PowerBI CSharp](https://github.com/Microsoft/PowerBI-CSharp)  
Chcete se ještě na něco zeptat? [Vyzkoušejte komunitu Power BI](http://community.powerbi.com/)

