---
title: aaaAdd hello dotazu akce v aplikace logiky | Microsoft Docs
description: "Přehled hello dotazu akce pro provádění akcí, jako je pole filtru."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: 3d4be901e7e6bf1b644057648930667ab34f2124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-query-action"></a>Začínáme s hello dotazu akce
Pomocí akce dotazu hello můžete pracovat s dávky a pole tooaccomplish pracovní postupy:

* Vytvořte úlohu pro všechny záznamy s vysokou prioritou z databáze.
* Uložte všechny přílohy PDF pro e-mailů do objektu blob Azure.

tooget spuštění pomocí akce hello dotazu v aplikaci logiky, aplikaci najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-query-action"></a>Pomocí akce dotazu hello
Akce je operace, která se provádí v pracovním postupu hello, která je definována v aplikaci logiky. [Další informace o akcích](connectors-overview.md).  

Akce dotazu Hello aktuálně používá jednu operaci, názvem hello pole filtru, který je zveřejněný v Návrháři hello. To vám umožní tooquery pole a vrátí sadu filtrované výsledky.

Zde je, jak můžete přidat logiku aplikace:

1. Vyberte hello **nový krok** tlačítko.
2. Zvolte **přidat akci**.
3. Hello akce vyhledávacího pole zadejte **filtru** toolist hello **pole filtru** akce.
   
    ![Vyberte akci dotazu hello](./media/connectors-native-query/using-action-1.png)
4. Vyberte toofilter pole. (hello následující snímek obrazovky ukazuje hello pole výsledky vyhledávání na Twitteru.)
5. Vytvořte podmínky tooevaluate pro každou položku. (hello následující snímek obrazovky filtruje tweetů od uživatelů, kteří mají více než 100 délky.)
   
    ![Dokončení hello dotazu akce](./media/connectors-native-query/using-action-2.png)
   
    Akce Hello výstup nové pole, která obsahuje jenom výsledky, které splnit požadavky filtru hello.
6. Klikněte na tlačítko hello levého horního rohu toosave hello panelu nástrojů a vaše logiku aplikace uloží obou a publikování (aktivovat).

## <a name="query-action"></a>Akce dotazu
Tady jsou hello podrobnosti hello akce, který podporuje tento konektor. konektor Hello má jednu možné akci.

| Akce | Popis |
| --- | --- |
| Pole filtru |Vyhodnocuje podmínku pro každou položku v pole a vrátí výsledky hello |

## <a name="action-details"></a>Podrobnosti akce
Akce dotazu Hello se dodává s možné jednu akci. Hello následující tabulky popisují hello požadované a volitelné vstupních polí pro akce hello a hello odpovídající výstup podrobností, které jsou spojené s použitím akce hello.

### <a name="filter-array"></a>Pole filtru
Hello následují vstupní pole pro hello akce, která umožňuje odchozí požadavku HTTP.
A * znamená, že je povinné pole.

| Zobrazované jméno | Název vlastnosti | Popis |
| --- | --- | --- |
| Z * |Z |pole toofilter Hello |
| Podmínka * |kde |Hello tooevaluate podmínku pro každou položku |

<br>

### <a name="output-details"></a>Podrobnosti o výstupu
Hello následují výstup podrobnosti hello odpověď HTTP.

| Název vlastnosti | Datový typ | Popis |
| --- | --- | --- |
| Filtrované pole |Pole |Pole, které obsahuje objekt pro každý filtrované výsledek |

## <a name="next-steps"></a>Další kroky
Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md). Můžete prozkoumat hello dalších dostupných konektorů v aplikacích logiky pohledem na našem [rozhraní API seznamu](apis-list.md).

