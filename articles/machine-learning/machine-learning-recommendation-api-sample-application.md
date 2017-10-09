---
title: "operace aaaCommon v hello Machine Learning doporučení API | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: da16767134a1386617e1184e4a4850f1f346e972
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="recommendations-api-sample-application-walkthrough"></a>Průvodce ukázkovou aplikací rozhraní Recommendations API
> [!NOTE]
> Měli byste začít používat hello kognitivní služby API doporučení místo tuto verzi. Hello kognitivní službu doporučení budou nahrazení této služby, a všechny nové funkce hello bude vyvinutý existuje. Obsahuje nové funkce, jako je dávkování podpory, lepší Explorer rozhraní API, čisticí prostředí plochy, konzistentnější registrace nebo fakturace rozhraní API, atd.
> Další informace o [toohello migrace nové kognitivní služby](http://aka.ms/recomigrate)
> 
> 

## <a name="purpose"></a>Účel
Tento dokument ukazuje použití hello hello Azure Learning doporučení počítače rozhraní API prostřednictvím [ukázkové aplikace](https://code.msdn.microsoft.com/Recommendations-144df403).

Tato aplikace není určený tooinclude plnou funkčnost, ani nebude použita všechny hello rozhraní API. Pokud chcete tooplay s hello doporučení služby Machine Learning ukazuje některé běžné operace tooperform. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="introduction-toomachine-learning-recommendation-service"></a>Úvod tooMachine Learning doporučení služby
Doporučení prostřednictvím hello doporučení služby Machine Learning je zapnuté, pokud vytvoříte doporučení model založený na hello následující data:

* Úložiště hello položky, které chcete toorecommend, také známé jako katalog
* Data představující hello využití položky podle uživatele nebo relace (to můžete získat v čase prostřednictvím získávání dat, nikoli jako součást hello ukázkové aplikace)

Jakmile je vytvořen doporučení modelu, můžete ji použít toopredict položky, které může být zajímá uživatele, podle tooa sadu položek (nebo jednu položku) hello uživatel vybere.

tooenable hello předchozí situaci, proveďte následující hello hello doporučení služby Machine Learning:

* Vytvoření modelu: Toto je logický kontejner, který obsahuje data hello (katalogu a využití) a modely předpovědi hello. Každý kontejner modelu je identifikován pomocí jedinečné ID, který je přidělen, když je vytvořeno. Toto ID je volána hello ID modelu a použije se většina hello rozhraní API. 
* Nahrát toocatalog: když je vytvořen kontejner modelu, můžete přidružit tooit katalog.

**Poznámka:**: vytvoření modelu a odesílání tooa katalogu se obvykle provádí jednou životního cyklu hello modelu.

* Nahrát využití: Tento postup přidá kontejner modelu toohello dat využití.
* Vytvoření modelu doporučení: až budete mít dostatek dat, můžete vytvořit model doporučení hello. Tato operace používá hello nejvyšší Machine Learning algoritmy toocreate model doporučení. Každé sestavení je přidružen jedinečný identifikátor. Je nutné tookeep záznam toto ID, protože je nezbytná pro fungování hello některé rozhraní API.
* Hello monitorování procesu vytváření: sestavení modelu doporučení je asynchronní operaci, a může trvat několik minut tooseveral hodin, v závislosti na hello množství dat (katalogu a využití) a hello sestavení parametry. Proto musíte toomonitor hello sestavení. Model doporučení je vytvořen jen v případě, že jeho přidružené sestavení se dokončí úspěšně.
* (Volitelné) Zvolte sestavení modelu active doporučení: Tento krok je nezbytný, pokud máte více než jeden model doporučení, které jsou součástí vašeho kontejneru modelu. Žádná doporučení tooget požadavek bez označující hello active doporučení modelu je automaticky přesměrován podle hello systému toohello výchozí active sestavení. 

**Poznámka:**: model active doporučení je připraven produkční a je sestavena pro produkční zatížení. To se liší od doporučení neaktivní modelu, který zůstává v prostředí testovací jako (někdy nazývané pracovní).

* Získejte doporučení: Jakmile model doporučení, můžete aktivovat doporučení pro jednu položku nebo seznam položek, které jste vybrali. 

Získejte doporučení se obvykle vyvolat po určitou dobu. Během tohoto časového období můžete přesměrovat využití dat toohello Machine Learning doporučení systém, který přidá tento kontejner dat toohello zadaného modelu. Pokud máte dostatek data o využití, můžete vytvořit nový model doporučení, která zahrnuje data o využití dalších hello. 

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2013 nebo novější
* Přístup k internetu 
* Předplatné toohello doporučení rozhraní API (https://datamarket.azure.com/dataset/amla/recommendations).

## <a name="azure-machine-learning-sample-app-solution"></a>Azure Machine Learning ukázkové aplikace řešení
Toto řešení obsahuje hello zdrojového kódu, využití vzorků, soubor katalogu a direktivy toodownload hello balíčky, které jsou požadovány pro kompilaci.

## <a name="hello-apis-used"></a>Hello používá rozhraní API
aplikace Hello používá funkce Machine Learning doporučení prostřednictvím podmnožinu dostupných rozhraní API. Dobrý den, který je v aplikaci hello ukázán následující rozhraní API:

* Vytvoření modelu: toohold logický kontejner vytvořit modely dat a doporučení. Model je identifikována názvem, a nelze vytvořit více než jeden model s hello stejný název.
* Nahrát soubor katalogu: použijte tooupload data katalogu.
* Nahrát soubor využití: použijte tooupload data o využití.
* Aktivovat sestavení: použijte toocreate model doporučení.
* Monitorovat spuštění sestavení: použijte toomonitor hello stav sestavení modelu doporučení.
* Vyberte model vytvořený pro doporučení: tooindicate které toouse modelu doporučení ve výchozím nastavení používá pro určité kontejner modelu. Tento krok je nutné pouze v případě, že máte více než jeden model doporučení a chcete, aby tooactivate neaktivní sestavení jako model active doporučení hello.
* Získejte doporučení: použijte tooretrieve doporučená položky podle tooa danou jednu položku nebo sadu položek. 

Úplný popis hello rozhraní API naleznete v dokumentaci hello Microsoft Azure Marketplace. 

**Poznámka:**: model může několik sestavení mít v čase (ne najednou). Každé sestavení je vytvořen s hello stejné nebo aktualizované katalogu a údaje o další využití.

## <a name="common-pitfalls"></a>Běžné nástrahy
* Potřebujete tooprovide svoje uživatelské jméno a ukázkové aplikace Microsoft Azure Marketplace primární účet klíče toorun hello.
* Běžící ukázkovou aplikaci hello intervalu následně se nezdaří. tok aplikace Hello zahrnuje vytváření, odeslání, vytvoření hello monitorování a získávání doporučení z předdefinovaných modelu; proto selže na provedení po sobě jdoucích Pokud neměnit název modelu hello mezi volání.
* Doporučení může vrátit bez data. Hello ukázková aplikace používá velmi malý soubor katalogu a využití. Některé položky z katalogu hello proto bude mít žádné doporučené položky.

## <a name="disclaimer"></a>Právní omezení
Ukázková aplikace Hello není určený toobe spuštění v produkčním prostředí. Hello data poskytnutá v katalogu hello je velmi malé a nepřinese model smysluplný doporučení. Hello dat je k dispozici jako ukázka. 

