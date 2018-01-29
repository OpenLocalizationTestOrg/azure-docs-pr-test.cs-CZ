---
title: "Vytváření řetězců filtru pro návrháře tabulky | Microsoft Docs"
description: "Vytváření řetězců filtru pro návrháře tabulky"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 069224d84462b4955912ce1462a65298a5acc04a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="constructing-filter-strings-for-the-table-designer"></a>Vytváření řetězců filtru pro návrháře tabulky
## <a name="overview"></a>Přehled
Filtrování dat v Azure tabulky, který se zobrazí v sadě Visual Studio **návrháře tabulky**, můžete vytvořit řetězec filtru a zadejte do pole filtru. Řetězec syntaxe filtru je definována služby WCF Data Services a je podobná SQL klauzule WHERE, ale je odeslána do služby Table prostřednictvím požadavku HTTP. **Návrháře tabulky** zpracovává správné kódování pro vás, tak k filtrování na hodnotu požadovanou vlastnost, můžete potřebovat pouze zadejte název vlastnosti, operátor porovnání, hodnotu pro kritéria a volitelně logická hodnota operátor do pole filtru. Nemusíte zahrnovat možnost dotazu $filter, jako byste, pokud byly generuje adresu URL a dotazovat tabulky pomocí [úložiště referenční dokumentace rozhraní API REST služby](http://go.microsoft.com/fwlink/p/?LinkId=400447).

Služby WCF Data Services jsou založené na [protokol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Podrobnosti o možnost dotazu filter systému (**$filter**), najdete v článku [konvence prostředí OData pro identifikátor URI specifikace](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Operátory porovnání
Pro všechny typy vlastností jsou podporovány následující logické operátory:

| Logický operátor | Popis | Příklad řetězec filtru |
| --- | --- | --- |
| EQ |Rovná |Město eq 'Redmond. |
| gt |Více než |Cena gt 20 |
| ge |Větší než nebo rovno |Cena ge 10 |
| lt |Méně než |Cena lt 20 |
| Le |Menší než nebo rovno |Cena le 100 |
| Ne |Není rovno |Ne města, Londýn, |
| a |A |Cena le 200 a cena gt 3.5 |
| nebo |Nebo |Cena le 3.5 nebo cena gt 200 |
| není |není |není isAvailable |

Při vytváření řetězec filtru, jsou důležité následující pravidla:

* Logické operátory slouží k porovnání vlastnost na hodnotu. Všimněte si, že není možné k porovnání vlastnost na hodnotu dynamické; jedna strana výrazu musí být konstanta.
* Všechny části řetězec filtru rozlišují velká a malá písmena.
* Hodnota konstanty musí být stejného typu dat jako vlastnost v pořadí pro filtr vracet výsledky platný. Další informace o typech podporovaných vlastnost najdete v tématu [Principy datového modelu služby Table](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtrování na vlastnosti řetězce.
Při filtrování pro vlastnosti string, uzavřete řetězcová konstanta v jednoduchých uvozovkách.

Následující příklad filtry na **PartitionKey** a **RowKey** vlastnosti; neklíčové další vlastnosti nebylo možné přidat také do řetězec filtru:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

I když není potřeba, můžete v závorkách, uzavřete každý výraz filtru:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Upozorňujeme, že služby Table nepodporuje zástupné dotazů a nejsou podporovány v Návrháři tabulky buď. Můžete však provést předponu odpovídající pomocí relačních operátorů na požadovanou předponu. Následující příklad vrací entity, LastName vlastnost, začíná písmenem "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filtrování na číselné vlastnosti
Chcete-li filtrovat na celé číslo nebo číslo s plovoucí desetinnou čárkou, zadejte počet bez uvozovek.

Tento příklad vrací všechny entity s vlastností stáří jehož hodnota je větší než 30:

    Age gt 30

Tento příklad vrací všechny entity s vlastností AmountDue jehož hodnota je menší než nebo rovna 100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filtrování na logická hodnota vlastnosti
Chcete-li filtrovat na logickou hodnotu, zadejte **true** nebo **false** bez uvozovek.

Následující příklad vrací všechny entity, kde je vlastnost IsActive nastavena na **true**:

    IsActive eq true

Je také možné zapsat tento výraz filtru bez logický operátor. V následujícím příkladu služby Table také vrátí všechny entity kde isActive – je **true**:

    IsActive

Pokud chcete vrátit všechny entity, pokud má hodnotu false IsActive, můžete použít not operátor:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filtrování na vlastnosti data a času
Chcete-li filtrovat na hodnotu data a času, zadejte **data a času** – klíčové slovo, za nímž následuje konstanta datum a čas v jednoduchých uvozovkách. Konstanta data a času musí být ve formátu UTC kombinované, jak je popsáno v [formátování data a času hodnoty vlastností](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Následující příklad vrací entity, kde je vlastnost CustomerSince rovno 10. července 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
