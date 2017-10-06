---
title: "Power BI Embedded - připojování zdroje dat tooa aaaMicrosoft"
description: "Power BI Embedded, připojit toodata zdroje"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: b1aad6e638104716d90f7e1d060eefcbc9daedbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-data-source"></a>Připojení zdroje dat tooa
S **Power BI Embedded**, můžete vložení sestavy do vlastní aplikace. Při vložení sestavy Power BI do vaší aplikace hello sestav připojí toohello základní data podle **import** kopii dat hello nebo pomocí **připojení přímo** toohello datový zdroj pomocí  **DirectQuery**.

Tady jsou hello rozdíly mezi použitím **Import** a **DirectQuery**.

| Import | DirectQuery |
| --- | --- |
| Tabulky, sloupce *a data* se naimportují, tedy zkopírují do datové sady sestavu hello. toosee změní základní dat došlo k toohello, musíte aktualizovat, nebo importovat, celou aktuální datovou sadu znovu. |Pouze *tabulky a sloupce* se naimportují, tedy zkopírují do datové sady sestavu hello. Vždy zobrazí hello nejnovější data. |

S Power BI Embedded jste můžete použít DirectQuery s cloudovými zdroji dat, ale není místní zdroje dat v tuto chvíli.

> [!NOTE]
> Hello místní brána dat není podporován s Power BI Embedded v tuto chvíli. To znamená, že DirectQuery nelze použít s místní datové zdroje.

## <a name="supported-data-sources"></a>Podporované zdroje dat

**DirectQuery**
* Databáze SQL Azure
* Azure SQL Data Warehouse

**Import**

Můžete importovat pomocí všechny hello dostupných zdrojů dat v rámci Power BI Desktop. Budete **není** být schopný toorefresh dat v rámci Power BI Embedded. Je nutné změny tooupload tooyour soubor PBIX souboru tooPower BI Embedded. Toto je z důvodu toono brána k dispozici. 

## <a name="benefits-of-using-directquery"></a>Výhody použití DirectQuery
Existují dvě hlavní výhody při použití **DirectQuery**:

* **DirectQuery** hello umožňuje vizualizace sestavení v velmi rozsáhlých datových sad, pokud v opačném případě by bylo nebude proveditelné toofirst importovat všechna data.
* Základní změny dat může vyžadovat aktualizaci dat a pro některé sestavy hello potřebovat toodisplay aktuální data můžete vyžadovat přenos velkých objemů dat, vytváření nebude proveditelné znovu je importovat data. Naopak **DirectQuery** sestavy vždy používat aktuální data.

## <a name="limitations-of-directquery"></a>Omezení DirectQuery
   Existuje několik omezení toousing **DirectQuery**:

* Všechny tabulky musí pocházet z jedné databáze.
* Pokud hello dotazu je příliš složité, dojde k chybě. Chyba hello tooremedy hello dotazu musí Refaktorovat, tak, aby byl menší komplexní. Pokud dotaz hello musí být komplexní, budete potřebovat data hello tooimport místo použití **DirectQuery**.
* Filtrování relace je omezená tooa jeden směr, nikoli obou směrech.
* Nelze změnit hello datový typ sloupce.
* Ve výchozím omezení jsou umístěny na výrazy jazyka DAX v míry povoleny. V tématu [DirectQuery a míry](#measures).

<a name="measures"/>

## <a name="directquery-and-measures"></a>DirectQuery a míry.
tooensure dotazů odesílaných toohello podkladové zdroje dat mají přijatelný výkon, jsou omezení vynucená pro míry. Při použití **Power BI Desktop**rozšířené uživatelé mohou toobypass toto omezení výběrem **soubor > Možnosti a nastavení > Možnosti**. V hello **možnosti** dialogovém okně, vyberte **DirectQuery**a vyberte možnost hello **Povolit neomezené míry v režimu DirectQuery**. Pokud je vybraná tato možnost, lze použít jakýkoli DAX výraz, který je platný pro míru. Uživatelé musí být vědomi; ale, že některé výrazy, provádět velmi dobře při importu dat hello může vést k velmi pomalé dotazy toohello back-end zdroj při v **DirectQuery** režimu. 

## <a name="see-also"></a>Viz také
* [Začínáme s Microsoft Power BI Embedded](power-bi-embedded-get-started.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)

