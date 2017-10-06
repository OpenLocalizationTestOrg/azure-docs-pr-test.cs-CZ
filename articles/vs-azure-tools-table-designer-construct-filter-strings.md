---
title: "řetězce aaaConstructing filtru pro návrháře tabulky hello | Microsoft Docs"
description: "Vytváření řetězců filtru pro návrháře tabulky hello"
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
ms.openlocfilehash: 48b38d27b97936064daa875e41881d51546bc11f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="constructing-filter-strings-for-hello-table-designer"></a>Vytváření řetězců filtru pro hello návrháře tabulky
## <a name="overview"></a>Přehled
hello toofilter data v Azure tabulky, který se zobrazí v sadě Visual Studio **návrháře tabulky**, můžete vytvořit řetězec filtru a zadejte do pole filtru hello. Hello se syntaxí filtru řetězce je definována hello služby WCF Data Services a je podobné tooa SQL klauzule WHERE, ale je odeslán toohello služby Table prostřednictvím požadavku HTTP. Hello **návrháře tabulky** popisovače hello správné kódování pro vás, tak toofilter na hodnotu požadovanou vlastnost, jenom musíte zadat název vlastnosti hello, operátor porovnání hodnotu pro kritéria, a můžete také filtrovat logický operátor ve hello pole. Jako při byly sestavování tabulky hello tooquery adresu URL prostřednictvím hello nemusí být povolena možnost dotazu hello $filter tooinclude [úložiště referenční dokumentace rozhraní API REST služby](http://go.microsoft.com/fwlink/p/?LinkId=400447).

Hello součásti WCF Data Services jsou založené na hello [protokol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Podrobnosti o možností dotazu systému filtru hello (**$filter**), najdete v části hello [konvence prostředí OData pro identifikátor URI specifikace](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Operátory porovnání
Hello následující logické operátory jsou podporovány pro všechny typy vlastností:

| Logický operátor | Popis | Příklad řetězec filtru |
| --- | --- | --- |
| EQ |Rovná |Město eq 'Redmond. |
| gt |Více než |Cena gt 20 |
| ge |Větší než nebo rovna příliš|Cena ge 10 |
| lt |Méně než |Cena lt 20 |
| Le |Menší než nebo rovno |Cena le 100 |
| Ne |Není rovno |Ne města, Londýn, |
| a |A |Cena le 200 a cena gt 3.5 |
| nebo |Nebo |Cena le 3.5 nebo cena gt 200 |
| není |není |není isAvailable |

Při vytváření řetězec filtru, hello následující pravidla jsou důležité:

* Použijte hello logické operátory toocompare tooa hodnotu vlastnosti. Všimněte si, že není možné toocompare tooa dynamické hodnotu vlastnosti; jedna strana hello výrazu musí být konstanta.
* Všechny části řetězec filtru hello rozlišují velká a malá písmena.
* Hello konstantní hodnota musí být hello stejný datový typ jako vlastnost hello v pořadí hello filtru tooreturn platný výsledků. Další informace o typech podporovaných vlastnost najdete v tématu [hello Principy datového modelu služby Table](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtrování na vlastnosti řetězce.
Při filtrování pro vlastnosti string, uzavřete hello řetězcová konstanta v jednoduchých uvozovkách.

Následující příklad filtry na hello Hello **PartitionKey** a **RowKey** vlastnosti; neklíčové další vlastnosti může také přidat řetězec filtru toohello:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

I když není potřeba, můžete v závorkách, uzavřete každý výraz filtru:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Upozorňujeme, že hello služby Table nepodporuje zástupné dotazů a nejsou podporovány v hello návrháře tabulky buď. Můžete však provést předponu odpovídající pomocí operátory porovnání na požadovanou předponu hello. Hello následující příklad vrací entity, LastName vlastnost, začíná písmenem hello "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filtrování na číselné vlastnosti
toofilter na celé číslo nebo číslo s plovoucí desetinnou čárkou, zadejte číslo hello bez uvozovek.

Tento příklad vrací všechny entity s vlastností stáří jehož hodnota je větší než 30:

    Age gt 30

Tento příklad vrací všechny entity s vlastností AmountDue jehož hodnota je menší než nebo roven hodnotě too100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filtrování na logická hodnota vlastnosti
Zadejte toofilter na logickou hodnotu, **true** nebo **false** bez uvozovek.

Hello následující příklad vrací všechny entity kde hello isActive – vlastnost je nastaven příliš**true**:

    IsActive eq true

Je také možné zapsat tento výraz filtru bez hello logický operátor. V následujícím příkladu hello, hello služby Table také vrátí všechny entity kde isActive – je **true**:

    IsActive

tooreturn všechny entity, pokud má hodnotu false, můžete použít IsActive hello není operátor:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filtrování na vlastnosti data a času
toofilter na hodnotu data a času, zadejte hello **data a času** – klíčové slovo, za nímž následuje hello konstanta datum a čas v jednoduchých uvozovkách. Konstanta Hello data a času musí být ve formátu UTC kombinované, jak je popsáno v [formátování data a času hodnoty vlastností](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Hello následující příklad vrací entity, kde vlastnost CustomerSince hello je rovna tooJuly 10, 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
