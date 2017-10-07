---
title: "aaaStream analýza Data Lake Store výstup | Microsoft Docs"
description: "Konfigurace ověřování a autorizace Azure Data Lake Store v úloze Stream Analytics"
keywords: 
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ea5baafa-0054-4c70-973a-6a3a8c6eaffc
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 183cf51edb2e49ac3e42257e67a8077b95777258
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-data-lake-store-output"></a>Výstupní datový proud analýza Data Lake Store
Úlohy Stream Analytics podporovat několik metod pro výstup, jeden se [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store je celopodnikové, flexibilně škálovatelné úložiště pro analytické úlohy s velkými objemy dat. Data Lake Store umožňuje toostore data jakékoli velikosti, typu a rychlosti příjmu pro provozní a zjišťovací analýzy.

## <a name="authorize-a-data-lake-store-account"></a>Autorizace účtu Data Lake Store
1. Pokud Data Lake Store je vybraná jako výstupu v hello portálu Azure, zobrazí se výzva tooauthorize použití existující Data Lake Store nebo toorequest přístup toohello Data Lake Store prostřednictvím hello portálu Classic.
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. Pokud již máte přístup k tooData Lake Store, klikněte na tlačítko Autorizovat a po krátkou dobu a stránky objeví se označující "Přesměrováním tooauthorization". stránku Hello se automaticky zavře a zobrazí se stránka hello, která by umožnilo tooconfigure hello Data Lake Store výstup.

Pokud nemáte registraci pro Data Lake Store, můžete podle hello "Přihlásit" odkaz tooinitiate hello požadavku nebo postupujte podle hello [získají pokyny k Začínáme](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-hello-data-lake-store-output-properties"></a>Konfigurovat vlastnosti výstup hello Data Lake Store
Jakmile máte účet Data Lake Store hello ověřený, můžete nakonfigurovat vlastnosti hello pro výstup do Data Lake Store. Následující tabulka Hello je hello seznam názvů vlastností a jejich popis tooconfigure, které výstupní Data Lake Store.

<table>
<tbody>
<tr>
<td><B>NÁZEV VLASTNOSTI</B></td>
<td><B>POPIS</B></td>
</tr>
<tr>
<td>Alias pro výstup</td>
<td>Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis Data Lake Store.</td>
</tr>
<tr>
<td>Účet data Lake Store</td>
<td>Hello název účtu úložiště hello kde odesílání výstupu. Zobrazí seznam účtů Data Lake Store, který má přístup k hello přihlášeného uživatele.</td>
</tr>
<tr>
<td>Předpony vzorek cesty [<I>volitelné</I>]</td>
<td>Hello souboru cesta používaná toowrite vaše soubory v rámci hello zadaný účet Data Lake Store. <BR>{date}, {time}<BR>Příklad 1: složku1/logs / {date} / {time}<BR>Příklad 2: složku1/logs / {date}</td>
</tr>
<tr>
<td>Datum formátu [<I>volitelné</I>]</td>
<td>Pokud token kalendářního data hello se používá v cestě hello předponu, můžete vybrat formát data hello, ve kterém jsou uspořádány soubory. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [<I>volitelné</I>]</td>
<td>Pokud token čas hello se používá v cestě hello předponu, zadejte formát času hello, ve kterém jsou uspořádány soubory. Aktuálně hello podporována pouze hodnota je HH.</td>
</tr>
<tr>
<td>Formát serializace událostí</td>
<td>Formát serializace pro výstupní data. Jsou podporovány JSON, CSV a Avro.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Pokud formátu CSV nebo formátu JSON, kódování musí být zadán. Znakové sady UTF-8 je hello pouze v současné době podporovaný formát kódování.</td>
</tr>
<tr>
<td>Oddělovač</td>
<td>Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje řadu běžných oddělovačů pro serializaci dat sdíleného svazku clusteru. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára.</td>
</tr>
<tr>
<td>Formát</td>
<td>Platí jenom pro serializaci JSON. Řádcích: Určuje, že se bude formátovat výstup hello tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že hello výstup naformátovaný jako pole objektů JSON.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Obnovit autorizační Data Lake Store
V současné době není omezení kde hello ověřovací token musí toobe ručně aktualizovat každých 90 dní pro všechny úlohy s výstupem Data Lake Store. Budete také potřebovat toore-ověření svého účtu Data Lake Store, pokud jste změnili heslo, protože úlohu vytvoření nebo poslední ověření. Příznakem tohoto problému je žádný výstup úlohy a chybu v protokolech operaci hello označující potřebu opětovná autorizace.

tooresolve tento problém, zastavte spuštěná úloha a přejděte výstup tooyour Data Lake Store. Kliknutím na odkaz "Autorizace obnovení" hello a po krátkou dobu a stránky objeví se označující "Přesměrováním tooauthorization..". stránku Hello se automaticky zavře a v případě úspěšného označí "Autorizace byl úspěšně obnoven". Potom potřebovat tooclick "Uložit" v dolní části hello hello stránky a můžete pokračovat restartováním úlohu z hello ztráty dat tooavoid naposledy zastaveno.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

