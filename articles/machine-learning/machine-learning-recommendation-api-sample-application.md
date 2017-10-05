---
title: "Běžné operace v Machine Learning API doporučení | Microsoft Docs"
description: "Azure ML doporučení ukázkové aplikace"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 0220bc17-3315-47d7-84a3-ef490263a343
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 8d8efa93e820f4a745ed93c0f4d13b2438dfa1eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="recommendations-api-sample-application-walkthrough"></a>Průvodce ukázkovou aplikací rozhraní Recommendations API
> [!NOTE]
> Měli byste začít používat službu doporučení rozhraní API kognitivní místo tuto verzi. Službu kognitivní doporučení budou nahrazení této služby, a všechny nové funkce bude vyvinutý existuje. Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.
> Další informace o [migraci na novou službu kognitivní](http://aka.ms/recomigrate)
> 
> 

## <a name="purpose"></a>Účel
Tento dokument ukazuje použití doporučení Azure Machine Learning API prostřednictvím [ukázkové aplikace](https://code.msdn.microsoft.com/Recommendations-144df403).

Tato aplikace není určena pro zahrnují plnou funkčnost, ani nebude použita všechna rozhraní API. Ukazuje některých běžných operací provést v případě, že chcete nejprve přehrávat doporučení službou Machine Learning. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="introduction-to-machine-learning-recommendation-service"></a>Úvod do služby Machine Learning doporučení
Doporučení přes službu doporučení Machine Learning je zapnuté, pokud vytvoříte model doporučení na základě následujících dat:

* Úložiště položek, které chcete doporučujeme také označované jako katalog
* Data představující použití položky podle uživatele nebo relace (to můžete získat v čase prostřednictvím získávání dat, nikoli jako součást ukázková aplikace)

Jakmile je vytvořen model doporučení, je možné použít k předpovědi položky, které uživatel může být zajímá podle sadu položek (nebo jednu položku) uživatel vybere.

Pokud chcete povolit předchozím scénáři, proveďte následující doporučení služby Machine Learning:

* Vytvoření modelu: Toto je logický kontejner, který obsahuje data (katalogu a využití) a modely předpovědi. Každý kontejner modelu je identifikován pomocí jedinečné ID, který je přidělen, když je vytvořeno. Toto ID je volána ID modelu a použije se většina rozhraní API. 
* Nahrajte do katalogu: když je vytvořen kontejner modelu, můžete přidružit k němu katalog.

**Poznámka:**: vytvoření modelu a odesílání na katalog se obvykle provádí jednou životního cyklu modelu.

* Nahrát využití: Tento postup přidá data o využití do kontejneru modelu.
* Vytvoření modelu doporučení: až budete mít dostatek dat, můžete vytvořit model doporučení. Tato operace se používá nejvyšší algoritmů strojového učení pro vytvoření modelu doporučení. Každé sestavení je přidružen jedinečný identifikátor. Je třeba zachovat záznam toto ID, protože je nezbytná pro fungování některé rozhraní API.
* Monitorování procesu sestavení: sestavení modelu doporučení je asynchronní operace a několik minut, může trvat několik hodin v závislosti na množství dat (katalogu a využití) a parametry sestavení. Proto je nutné sledovat sestavení. Model doporučení je vytvořen jen v případě, že jeho přidružené sestavení se dokončí úspěšně.
* (Volitelné) Zvolte sestavení modelu active doporučení: Tento krok je nezbytný, pokud máte více než jeden model doporučení, které jsou součástí vašeho kontejneru modelu. Každá žádost o získání doporučení bez označující modelu active doporučení v systému aktivní Build výchozí přesměrována automaticky. 

**Poznámka:**: model active doporučení je připraven produkční a je sestavena pro produkční zatížení. To se liší od doporučení neaktivní modelu, který zůstává v prostředí testovací jako (někdy nazývané pracovní).

* Získejte doporučení: Jakmile model doporučení, můžete aktivovat doporučení pro jednu položku nebo seznam položek, které jste vybrali. 

Získejte doporučení se obvykle vyvolat po určitou dobu. Během daného časového intervalu můžete přesměrovat data o využití do systému doporučení Machine Learning, která přidává tato data ke kontejneru zadaného modelu. Pokud máte dostatek data o využití, můžete vytvořit nový model doporučení, která zahrnuje data pro další použití. 

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2013 nebo novější
* Přístup k internetu 
* Předplatné doporučení rozhraní API (https://datamarket.azure.com/dataset/amla/recommendations).

## <a name="azure-machine-learning-sample-app-solution"></a>Azure Machine Learning ukázkové aplikace řešení
Toto řešení obsahuje zdrojový kód, využití vzorků, soubor katalogu a direktivy stáhnout balíčky, které jsou požadovány pro kompilaci.

## <a name="the-apis-used"></a>Rozhraní API používaná
Aplikace používá funkce Machine Learning doporučení prostřednictvím podmnožinu dostupných rozhraní API. Následující rozhraní API je ukázán v aplikaci:

* Vytvoření modelu: vytvořte logický kontejner pro uložení dat a doporučení modely. Model je identifikována názvem a nelze vytvořit více než jeden model se stejným názvem.
* Nahrát soubor katalogu: použít k nahrání data katalogu.
* Nahrát soubor využití: použít k nahrání dat o využití.
* Aktivovat sestavení: použijte pro vytvoření modelu doporučení.
* Monitorovat spuštění sestavení: použijte k monitorování stavu sestavení modelu doporučení.
* Vyberte model vytvořený pro doporučení: slouží k označení, které doporučení model ve výchozím nastavení pro určité kontejner modelu. Tento krok je nutné pouze v případě, že máte více než jeden model doporučení a chcete aktivovat neaktivní sestavení jako model active doporučení.
* Získejte doporučení: použijte k načtení Doporučené položky podle dané jednu položku nebo sadu položek. 

Úplný popis rozhraní API najdete v dokumentaci k Microsoft Azure Marketplace. 

**Poznámka:**: model může několik sestavení mít v čase (ne najednou). Každé sestavení je vytvořen se stejné nebo aktualizované katalogu a data o další využití.

## <a name="common-pitfalls"></a>Běžné nástrahy
* Budete muset zadat svoje uživatelské jméno a klíč primární účtu Microsoft Azure Marketplace ke spuštění ukázkové aplikace.
* Ukázková aplikace běžící postupně se nezdaří. Tok aplikace zahrnuje vytvoření, odeslání, vytvoření monitorování a získávání doporučení z předdefinovaných modelu; proto selže na provedení po sobě jdoucích Pokud neměnit název modelu mezi volání.
* Doporučení může vrátit bez data. Ukázková aplikace používá velmi malý soubor katalogu a využití. Některé položky z katalogu proto bude mít žádné doporučené položky.

## <a name="disclaimer"></a>Právní omezení
Ukázková aplikace není určen ke spuštění v produkčním prostředí. Data zadaná v katalogu je velmi malé a nepřinese model smysluplný doporučení. Data k dispozici jako ukázka. 

