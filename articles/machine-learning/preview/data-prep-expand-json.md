---
title: "Rozbalte položku pomocí Azure Machine Learning Workbench transformace JSON"
description: "V referenčním dokumentu pro transformaci, rozbalte položku JSON."
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 614f4422aa987fc32dcce62826bb2477473fdc32
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="expand-json-transformation"></a>Rozbalte JSON transformace
**Rozbalte JSON** transformace umožňuje uživatelům rozbalte jako existující sloupec, který obsahuje platný text JSON do více sloupců.

## <a name="how-to-perform-this-transformation"></a>Jak provádět Tato transformace

Postupujte podle těchto kroků k provedení této transformace:
1. Vyberte zdrojový sloupec, který obsahuje JSON text.
2. Vyberte **rozbalte JSON** z **transformuje** nabídky. Nebo klikněte pravým tlačítkem na záhlaví sloupce, zdroje a vyberte **rozbalte JSON** v místní nabídce. 
3. Klikněte na **OK**. 
 
Vedle zdrojového sloupce jsou přidány sloupce. Tyto sloupce obsahují vlastnosti z další úroveň hierarchie v textu JSON. Vlastnost klíče, pokud existuje, je použít k vytvoření názvu odpovídajícího sloupce. Vnořené vlastnosti se zachovají jako text JSON pro tohoto uživatele můžete interaktivně rozbalit nebo zahodit podle potřeby.

## <a name="examples"></a>Příklady

Zdroj požadován *zákazníka* je rozšířena do dvou sloupců *Customer.Name* a *Customer.Phone*.

| Zákazník                                                | Customer.Name   | Customer.Phone |
|---------------------------------------------------------|-----------------|----------------|
| {"Název": "Carrie Dodson", "Phone": "123-4567-890"}   | Carrie Dodson   | 123-4567-890   |
| {"Název": "Leonardem Robledo", "Phone": "456 7890 123"} | Leonardem Robledo | 456-7890-123   |

