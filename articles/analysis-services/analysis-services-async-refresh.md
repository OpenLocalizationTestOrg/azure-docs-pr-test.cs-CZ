---
title: "Asynchronní aktualizace pro modely Azure Analysis Services | Microsoft Docs"
description: "Naučte se pomocí rozhraní REST API kódu asynchronní aktualizace."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 12/18/2017
ms.author: owend
ms.openlocfilehash: 06d807b83f700c675c6979998dd8f74372a4845f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="asynchronous-refresh-with-the-rest-api"></a>Asynchronní aktualizace pomocí rozhraní REST API
Pomocí žádný programovací jazyk, který podporuje volání REST, můžete provádět operace asynchronní aktualizace dat na vaše tabulkové modely služby Azure Analysis Services. To zahrnuje synchronizace repliky jen pro čtení pro dotaz Škálováním na více systémů. 

Operace aktualizace dat může trvat nějakou dobu v závislosti na počtu faktorů, včetně datový svazek, úroveň optimalizace pomocí oddíly atd. Tyto operace mít byla vyvolána tradičně s existující metodami, například pomocí [TNÍ](https://docs.microsoft.com/sql/analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo) (tabulkový objektový Model), [prostředí PowerShell](https://docs.microsoft.com/sql/analysis-services/powershell/analysis-services-powershell-reference) rutin, nebo [TMSL](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) (tabulkový Model. Skriptovací jazyk). Tyto metody však může vyžadovat často nespolehlivé, dlouhotrvajících připojení prostřednictvím protokolu HTTP.

Rozhraní REST API pro Azure Analysis Services umožňuje operace aktualizace dat mají být prováděny asynchronně. Pomocí rozhraní REST API, nejsou nutné dlouho běžící připojení protokolu HTTP z klientské aplikace. Existují také další integrované funkce pro spolehlivost, jako je automatické opakování a dávkové potvrzení.

## <a name="base-url"></a>Základní adresa URL

Základní adresu URL následující formát:

```
https://<rollout>.asazure.windows.net/servers/<serverName>/models/<resource>/
```

Představte si třeba model s názvem AdventureWorks, na serveru s názvem myserver, nachází v oblasti Azure USA – Západ, je název serveru:

```
asazure://westus.asazure.windows.net/myserver 
```

Základní adresa URL pro tento název serveru je:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/ 
```

S použitím základní adresu URL, prostředky a operace by se daly připojit podle těchto kritérií: 

![Asynchronní aktualizace](./media/analysis-services-async-refresh/aas-async-refresh-flow.png)

- Všechny položky, které končí v **s** je kolekce.
- Všechny položky, které končí **()** je funkce.
- Cokoliv jiného je prostředek nebo objekt.

Například můžete použít příkaz POST na aktualizace kolekce k provedení operace aktualizace, jako je tento:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/refreshes
```

## <a name="authentication"></a>Authentication

Všechna volání musí být ověřeny pomocí platný token v hlavičce autorizace Azure Active Directory (OAuth 2) a musí splňovat následující požadavky:

- Token musí být token uživatele nebo aplikaci instanční objekt.
- Uživatel nebo aplikace musí mít dostatečná oprávnění na serveru nebo model pro požadovaný volání. Úroveň oprávnění je určen podle rolí v rámci modelu nebo skupiny správce na serveru.
- Token musí mít na správnou cílovou skupinu nastavena na `https://*.asazure.windows.net`.

## <a name="post-refreshes"></a>POST /refreshes

K provedení operace aktualizace, použijte příkaz POST na kolekci /refreshes přidat novou položku obnovení do kolekce. Hlavička umístění v odpovědi obsahuje ID aktualizace. Klientská aplikace můžete odpojit a zkontrolujte stav později, pokud je to nutné, protože je asynchronní.

Operace pouze jeden aktualizace byla přijata v čase pro model. Pokud je aktuální běžící operace aktualizace a jiné odeslání, je vrácena 409 stavový kód HTTP konflikt.

Text může vypadat takto:

```
{
    "Type": "Full",
    "CommitMode": "transactional",
    "MaxParallelism": 2,
    "RetryCount": 2,
    "Objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer"
        },
        {
            "table": "DimDate"
        }
    ]
}
```

### <a name="parameters"></a>Parametry
Zadání parametrů se nevyžaduje. Je použita výchozí hodnota.

|Název  |Typ  |Popis  |Výchozí  |
|---------|---------|---------|---------|
|Typ     |  výčet       |  Typ zpracování k provedení. Typy jsou v souladu s TMSL [aktualizovat příkaz](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl) typy: full, clearValues, vypočítat, dataOnly, automatické a přidejte defragmentaci.       |   Automatické      |
|CommitMode     |  výčet       |  Určuje, pokud objekty budou potvrzeny v dávkách nebo jenom v případě, že je kompletní. Režimy: výchozí, transakcí, partialBatch.  |  transakcí       |
|MaxParallelism     |   Int      |  Tato hodnota určuje maximální počet vláken, na který se má spustit zpracování příkazů paralelně. To v souladu s MaxParallelism vlastnost, která může být nastavena v TMSL [pořadí příkaz](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/sequence-command-tmsl) nebo použití jiných metod.       | 10        |
|retryCount    |    Int     |   Označuje číslo, kolikrát bude operaci opakovat než selže.      |     0    |
|Objekty     |   Pole      |   Pole objektů, které mají být zpracovány. Každý objekt zahrnuje: "tabulky" při zpracování celou tabulku nebo "tabulky" a "oddíl" při zpracování oddílu. Pokud nejsou zadány žádné objekty, se aktualizují celý model. |   Proces celý model      |

CommitMode se rovná partialBatch. Používá se při provádění modulu počáteční zatížení rozsáhlých datových sad, které může trvat hodiny. Pokud se operace aktualizace se nezdaří po úspěšně potvrzení jeden nebo více dávek, úspěšně potvrzeny dávky zůstanou potvrdit (ho nebude vrácení úspěšně potvrzeny dávky).

> [!NOTE]
> V době psaní velikost dávky je hodnota MaxParallelism, ale tato hodnota může změnit.

## <a name="get-refreshesrefreshid"></a>GET /refreshes/\<refreshId >

Chcete-li zkontrolovat stav operace aktualizace, použijte příkaz GET na aktualizace ID. Tady je příklad textu odpovědi. Pokud je v průběhu operace **inProgress** je vrácen stav.

```
{
    "startTime": "2017-12-07T02:06:57.1838734Z",
    "endTime": "2017-12-07T02:07:00.4929675Z",
    "type": "full",
    "status": "succeeded",
    "currentRefreshType": "full",
    "objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer",
            "status": "succeeded"
        },
        {
            "table": "DimDate",
            "partition": "DimDate",
            "status": "succeeded"
        }
    ]
}
```

## <a name="get-refreshes"></a>ZÍSKAT /refreshes

Seznam operací historických aktualizace pro model, použijte příkaz GET na kolekci /refreshes. Tady je příklad textu odpovědi. 

> [!NOTE]
> V době psaní jsou uloženy a vráceny za posledních 30 dní operace aktualizace, ale toto číslo může změnit.

```
[
    {
        "refreshId": "1344a272-7893-4afa-a4b3-3fb87222fdac",
        "startTime": "2017-12-09T01:58:04.76",
        "endTime": "2017-12-09T01:58:12.607",
        "status": "succeeded"
    },
    {
        "refreshId": "474fc5a0-3d69-4c5d-adb4-8a846fa5580b",
        "startTime": "2017-12-07T02:05:48.32",
        "endTime": "2017-12-07T02:05:54.913",
        "status": "succeeded"
    }
]
```

## <a name="delete-refreshesrefreshid"></a>ODSTRANĚNÍ /refreshes/\<refreshId >

Zrušení operace probíhající aktualizace, použijte příkaz DELETE u aktualizace ID.

## <a name="post-sync"></a>/ POST Sync

Nutnosti provádět operace aktualizace, může být nutné synchronizovat nová data s replikami pro dotaz Škálováním na více systémů. K provedení operace synchronizace pro model, použijte příkaz POST na funkci/Sync. Hlavička umístění v odpovědi obsahuje ID synchronizační operace.

## <a name="get-sync-status"></a>ZÍSKAT stav/Sync

Pokud chcete zkontrolovat stav synchronizace operace, použijte příkaz GET předávání ID operace jako parametr. Tady je příklad textu odpovědi na:

```
{
    "operationId": "cd5e16c6-6d4e-4347-86a0-762bdf5b4875",
    "database": "AdventureWorks2",
    "UpdatedAt": "2017-12-09T02:44:26.18",
    "StartedAt": "2017-12-09T02:44:20.743",
    "syncstate": 2,
    "details": null
}
```

Hodnoty pro stav synchronizace:

- 0: replikace. Soubory databáze jsou replikovány do cílové složky.
- 1: rehydratace. Databáze je právě rehydrated na instance serveru jen pro čtení.
- 2: dokončení. Synchronizační operace byla úspěšně dokončena.
- 3: se nezdařilo. Operace synchronizace se nezdařila.
- 4: dokončení. Synchronizační operace byla dokončena, ale provádí kroky čištění.

## <a name="code-sample"></a>Ukázka kódu

Zde je ukázka kódu C# pro začátek, [RestApiSample na Githubu](https://github.com/Microsoft/Analysis-Services/tree/master/RestApiSample).

### <a name="to-use-the-code-sample"></a>Chcete-li použít ukázka kódu

1.  Klonovat nebo stáhnout úložišti. Otevřete RestApiSample řešení.
2.  Vyhledejte řádek **klienta. BaseAddress =...** a poskytnout vaší [základní adresa URL](#base-url).

Ukázka kódu můžete použít interaktivní přihlášení, uživatelské jméno a heslo, nebo [instanční objekt](#service-principle).

#### <a name="interactive-login-or-usernamepassword"></a>Interaktivní přihlášení nebo uživatelské jméno a heslo

Tato forma ověřování vyžaduje aplikaci Azure vytvořit s potřebnými oprávněními rozhraní API, které jsou přiřazeny. 

1.  Na portálu Azure, klikněte na tlačítko **nový** > **Azure Active Directory** > **registrace aplikace** > **nový registrace aplikace**.

    ![Registrace nové aplikace](./media/analysis-services-async-refresh/aas-async-app-reg.png)


2.  V **vytvořit**, zadejte název, vyberte **nativní** typ aplikace. Pro **identifikátor URI pro přesměrování**, zadejte **urn: ietf:wg:oauth:2.0:oob**a potom klikněte na **vytvořit**.

    ![Nastavení](./media/analysis-services-async-refresh/aas-async-app-reg-name.png)

3.  Vyberte svou aplikaci a pak zkopírujte a uložte **ID aplikace**.

    ![Zkopírujte ID aplikace](./media/analysis-services-async-refresh/aas-async-app-id.png)

4.  V **nastavení**, klikněte na tlačítko **požadovaná oprávnění** > **přidat**.

    ![Přidat přístup přes rozhraní API](./media/analysis-services-async-refresh/aas-async-add.png)

5.  V **vybrat rozhraní API**, typ **SQL Server Analysis Services** do vyhledávacího pole a pak vyberte **Azure Analysis Services (SQL Server Analysis Services Azure)**.

    ![Vybrat rozhraní API](./media/analysis-services-async-refresh/aas-async-select-api.png)

6.  Vyberte **číst a zapisovat všechny modely**a potom klikněte na **vyberte**. Pokud jsou obě vybraná, klikněte na tlačítko **provádí** přidat oprávnění. To může trvat několik minut, aby se rozšířily změny.

    ![Vyberte pro čtení a zápis všech modelů](./media/analysis-services-async-refresh/aas-async-select-read.png)

7.  V ukázce kódu, najdete **UpdateToken()** metoda. Sledujte obsah této metody.
8.  Najít **řetězec clientID =...** a pak zadejte **ID aplikace** jste zkopírovali v kroku 3.
9.  Spusťte ukázku.

#### <a name="service-principal"></a>Instanční objekt

Najdete v článku [Automation z Azure Analysis Services s objekty služby a prostředí PowerShell](https://azure.microsoft.com/blog/automation-of-azure-analysis-services-with-service-principals-and-powershell/) příspěvku na blogu k nastavení hlavní název služby a přiřaďte potřebná oprávnění ve službě Azure Analysis Services. Po dokončení kroky popsané v příspěvku na blogu, proveďte následující kroky:

1.  V ukázce kódu, Najít **řetězec autority =...** , nahraďte **běžné** ve vaší organizaci klienta ID.
2.  Komentář nebo zrušte komentář u tak ClientCredential třída slouží k vytvoření instance objektu pověření. Ujistěte se, \<ID aplikace > a \<klíč aplikace > hodnot, ke kterým se přistupuje bezpečným způsobem nebo použít ověřování pomocí certifikátů pro objekty služby.
3.  Spusťte ukázku.


## <a name="see-also"></a>Další informace najdete v tématech

[Ukázky](analysis-services-samples.md)   
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)   


