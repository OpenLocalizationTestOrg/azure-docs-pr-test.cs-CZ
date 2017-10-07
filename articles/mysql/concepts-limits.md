---
title: "aaaLimitations ve službě Azure Database pro databázi MySQL | Microsoft Docs"
description: "Popisuje omezení verze preview ve službě Azure Database pro databázi MySQL."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 9c877c592bf640f62182d8761c9c51363882d706
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a>Omezení v Azure databáze pro databázi MySQL (Preview)
Hello databáze Azure pro službu MySQL je ve verzi public preview. Hello následující části popisují kapacitu a funkční omezení v databázi služby hello.

## <a name="service-tier-maximums"></a>Maximální hodnoty úroveň služby
Azure databáze MySQL, má více úrovní služeb, které lze vybírat při vytváření serveru. Další informace najdete v tématu [co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md).  

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
> Chyba 1040 (08004): Příliš mnoho připojení

## <a name="preview-functional-limitations"></a>Funkční omezení verze Preview:
### <a name="scale-operations"></a>Operace škálování:
1.  Dynamické škálování serverů mezi úrovně služeb není aktuálně podporováno. To znamená přepínání mezi úrovně služeb Basic a Standard.
2.  Dynamické zvětšování na vyžádání úložiště na předem vytvořené serveru není aktuálně podporováno.
3.  Zmenšuje velikost úložiště serveru není podporována.

### <a name="server-version-upgrades"></a>Upgrady verze serveru:
- Automatické migrace mezi verzemi modul hlavní databáze není aktuálně podporována.

### <a name="subscription-management"></a>Správa předplatného:
- Dynamicky přesunutí předem vytvořené serverů mezi předplatné a skupina prostředků není aktuálně podporováno.

### <a name="point-in-time-restore"></a>Bod v čas – obnovení:
1.  Obnovení vrstvy služby toodifferent nebo výpočetní jednotky a velikost úložiště není povoleno.
2.  Obnovení vynechaných server není podporováno.

## <a name="next-steps"></a>Další kroky:
[Co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md)
[podporované verze databáze MySQL](concepts-supported-versions.md)
