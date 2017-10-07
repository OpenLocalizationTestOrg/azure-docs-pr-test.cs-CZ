---
title: "aaaLimitations v databázi Azure pro PostgreSQL | Microsoft Docs"
description: "Popisuje omezení v Azure databázi PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 06/01/2017
ms.openlocfilehash: f53dd240e55e0633bc1dfb8ad25e1818fa8ae18c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Omezení v Azure databázi PostgreSQL
Hello Azure databáze PostgreSQL služby je ve verzi public preview. Hello následující části popisují kapacitu a funkční omezení v databázi služby hello.

## <a name="service-tier-maximums"></a>Maximální hodnoty úroveň služby
Azure databáze PostgreSQL má více úrovní služeb, které lze vybírat při vytváření serveru. Další informace najdete v tématu [co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md).  

Je maximální počet připojení, výpočetní jednotky a úložiště v jednotlivých úrovních služeb během verze preview služby hello, následujícím způsobem: 

|                            |                   |
| :------------------------- | :---------------- |
| **Maximální počet připojení**        |                   |
| Základní 50 výpočetní jednotky     | 50 připojení    |
| Základní 100 výpočetní jednotky    | připojení 100   |
| Standardní 100 výpočetní jednotky | 200 připojení   |
| Standardní 200 výpočetní jednotky | 300 připojení   |
| Standardní 400 výpočetní jednotky | 400 připojení   |
| Standardní 800 výpočetní jednotky | připojení 500   |
| **Maximální počet výpočetní jednotky**      |                   |
| Úroveň služeb Basic         | 100 výpočetní jednotky |
| Úroveň služeb Standard      | 800 výpočetní jednotky |
| **Maximální počet úložiště**            |                   |
| Úroveň služeb Basic         | 1 TB              |
| Úroveň služeb Standard      | 1 TB              |

Když se dosáhne příliš mnoha připojení, může se zobrazit hello následující chybě:
> Závažná chyba: bohužel již příliš mnoho klientů

## <a name="preview-functional-limitations"></a>Funkční omezení verze Preview
### <a name="scale-operations"></a>Operace škálování
1.  Dynamické škálování serverů mezi úrovně služeb není aktuálně podporováno. To znamená přepínání mezi úrovně služeb Basic a Standard.
2.  Dynamické zvětšování na vyžádání úložiště na předem vytvořené serveru není aktuálně podporováno.
3.  Zmenšuje velikost úložiště serveru není podporována.

### <a name="server-version-upgrades"></a>Upgrady verze serveru
- Automatické migrace mezi verzemi modul hlavní databáze není aktuálně podporována.

### <a name="subscription-management"></a>Správa předplatného
- Dynamicky přesunutí předem vytvořené serverů mezi předplatné a skupina prostředků není aktuálně podporováno.

### <a name="point-in-time-restore"></a>Obnovení do bodu v čase
1.  Obnovení vrstvy služby toodifferent nebo výpočetní jednotky a velikost úložiště není povoleno.
2.  Obnovení vynechaných server není podporováno.

## <a name="next-steps"></a>Další kroky
- Pochopení [co je k dispozici v jednotlivých cenových úrovní](concepts-service-tiers.md)
- Pochopení [podporované verze databáze PostgreSQL](concepts-supported-versions.md)
- Zkontrolujte [jak hello tooBack zálohu a obnovení na serveru v Azure databázi PostgreSQL pomocí portálu Azure](howto-restore-server-portal.md)
