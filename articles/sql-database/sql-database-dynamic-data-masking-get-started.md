---
title: "maskování dynamická data aaaAzure databáze SQL | Microsoft docs"
description: "Databáze SQL dynamická data maskování omezuje ohrožení citlivých dat pomocí maskování ho toonon privilegovaných uživatelů"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: 4b36d78e-7749-4f26-9774-eed1120a9182
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/09/2017
ms.author: ronitr; ronmat
ms.openlocfilehash: 68b55128dc096f7e3dd0e5ed1427b39da5d64736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-dynamic-data-masking"></a>Maskování dynamická data databáze SQL

Databáze SQL dynamická data maskování omezuje ohrožení citlivých dat pomocí maskování ho toonon privilegovaných uživatelů. 

Dynamická data maskování pomáhá zabránit neoprávněnému přístupu toosensitive data tím, že umožňuje zákazníkům toodesignate kolik hello citlivá data tooreveal s minimálním dopadem na aplikační vrstvu hello. Je funkce založená na zásadách zabezpečení, která skryje hello citlivá data v hello sadu výsledků dotazu přes pole určené databáze, zatímco hello data v databázi hello se nezmění.

Například na zástupce služby v telefonickém centru může identifikovat volající podle několik číslic jejich číslo platební karty, ale tato data, která položky by neměl být plně zveřejněné toohello zástupce služby. Že masky všechny ale hello poslední čtyři číslice jakékoli číslo platební karty v sadě výsledků hello jakýkoli dotaz, může být definováno pravidlo maskování. Například maska příslušná data může být dat definované tooprotect identifikovatelné osobní údaje (PII), tak, aby vývojář může dotazovat provozní prostředí pro účely odstraňování potíží, aniž bychom při tom porušili předpisy.

## <a name="sql-database-dynamic-data-masking-basics"></a>Dynamické maskování základy dat databáze SQL
Nastavíte dynamické maskování zásad v hello dat portálu Azure tak, že vyberete dynamické maskování operace v okně Konfigurace databázi SQL nebo okno nastavení dat hello.

### <a name="dynamic-data-masking-permissions"></a>Dynamická data maskování oprávnění
Dynamická data maskování se dá nakonfigurovat Dobrý den, správce databáze Azure, správce serveru nebo úředník role zabezpečení.

### <a name="dynamic-data-masking-policy"></a>Dynamická data maskování zásad
* **Uživatelé SQL vyloučení z maskování** – A sada uživatelů SQL nebo AAD identity, které získat odmaskovaná data v hello SQL výsledky dotazu. Uživatelé s oprávněními správce jsou vždycky vyloučení z maskování a zobrazit hello původní data bez jakékoli maska.
* **Maskování pravidla** -sadu pravidel, které definují hello určené pole toobe maskovat a hello maskování funkce, která se používá. Hello určené pole lze definovat pomocí název schématu databáze, název tabulky a název sloupce.
* **Funkce maskování** -sadu metod, které řídí hello úniku dat pro různé scénáře.

| Funkci maskování | Maskování logiky |
| --- | --- |
| **Výchozí** |**Úplné maskování dat podle toohello typy hello uvedených polí**<br/><br/>• Použijte XXXX nebo méně Xs Pokud hello velikost pole hello je menší než 4 znaky pro datové typy řetězec (ntext nchar, nvarchar).<br/>• Použijte hodnotu 0 pro číselné datové typy (bigint, bit, decimal, int, peníze, číselné, smallint, smallmoney, tinyint, float, reálné).<br/>• Použijte 01-01-1900 pro datum a čas datové typy (datum, datetime2, datetime, datetimeoffset, smalldatetime, čas).<br/>• SQL variant, hello výchozí hodnota typu aktuální hello se používá.<br/>• Pro dokument XML hello <masked/> se používá.<br/>• Použijte prázdnou hodnotu pro speciální typy dat. (časové razítko tabulky, hierarchyid, identifikátor GUID, binární, image, varbinary prostorové typy). |
| **Platební karty** |**Maskování metoda, která zpřístupňuje hello poslední čtyři číslice hello uvedených polí** a přidá konstantní řetězec jako předpony v podobě hello platební karty.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **E-mailu** |**Maskování metoda, která zveřejňuje hello první písmeno a nahradí hello domény XXX.com** pomocí konstantní řetězec předpony hello tvar e-mailovou adresu.<br/><br/>aXX@XXXX.com |
| **Náhodné číslo** |**Maskování metodu, která generuje náhodné číslo** podle toohello vybrané hranice a skutečný datové typy. Pokud hello určené hranice jsou stejné, hello funkci maskování je konstantní číslo.<br/><br/>![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Vlastní text** |**Maskování metoda, která zpřístupňuje hello první a poslední znaky** a přidá řetězec vlastní odsazení uprostřed hello. Pokud původní řetězec hello je kratší než hello zveřejněné předponu a příponu, použije se jenom hello odsazení řetězec. <br/>přípona předponu [odsazení]<br/><br/>![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-toomask"></a>Doporučená pole toomask
Hello DDM modul doporučení, příznaky určitá pole z databáze jako potenciálně citlivých pole, které můžou být vhodnými kandidáty pro maskování. Hello dynamického maskování dat v okně v portálu hello uvidíte hello doporučená sloupce pro vaši databázi. Stačí toodo je, klikněte na tlačítko **přidat masku** pro jeden nebo více sloupců a potom **Uložit** tooapply masku pro tato pole.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Nastavit dynamické maskování dat pro vaši databázi pomocí rutin prostředí Powershell
V tématu [rutiny databáze Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Nastavit maskování dynamická data pro vaši databázi pomocí rozhraní REST API
V tématu [operací pro databáze Azure SQL](https://msdn.microsoft.com/library/dn505719.aspx).

