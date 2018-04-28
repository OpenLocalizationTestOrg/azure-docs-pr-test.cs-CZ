---
title: "Omezení v Azure databázi PostgreSQL | Microsoft Docs"
description: "Popisuje omezení v Azure databázi PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 12/04/2017
ms.openlocfilehash: 6dbed1a834d74047178a9f996683d65520047e66
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Omezení v Azure databázi PostgreSQL
Databáze Azure pro PostgreSQL služby je ve verzi public preview. Následující části popisují kapacitu a funkční omezení ve službě databáze.

## <a name="service-tier-maximums"></a>Maximální hodnoty úroveň služby
Azure databáze PostgreSQL má více úrovní služeb, které lze vybírat při vytváření serveru. Další informace najdete v tématu [co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md).  

Je maximální počet připojení, výpočetní jednotky a úložiště v jednotlivých úrovních služeb ve verzi Preview služby následujícím způsobem: 

| | |
| :------------------------- | :---------------- |
| **Maximální počet připojení**        |                   |
| Základní 50 výpočetní jednotky     | 55 připojení    |
| Základní 100 výpočetní jednotky    | 105 připojení   |
| Standardní 100 výpočetní jednotky | 150 připojení   |
| Standardní 200 výpočetní jednotky | 250 připojení   |
| Standardní 400 výpočetní jednotky | 480 připojení   |
| Standardní 800 výpočetní jednotky | 950 připojení   |
| **Maximální počet výpočetní jednotky**      |                   |
| Úroveň služeb Basic         | 100 výpočetní jednotky |
| Úroveň služeb Standard      | 800 výpočetní jednotky |
| **Maximální počet úložiště**            |                   |
| Úroveň služeb Basic         | 1 TB              |
| Úroveň služeb Standard      | 1 TB              |

Azure systém vyžaduje pět připojení k Azure databáze PostgreSQL serveru. Když se dosáhne příliš mnoha připojení, může se zobrazit chybová zpráva:
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
1.  Obnovení na jinou službu vrstvy nebo výpočetní jednotky a velikost úložiště není povoleno.
2.  Obnovení vynechaných server není podporováno.

## <a name="next-steps"></a>Další postup
- Pochopení [co je k dispozici v jednotlivých cenových úrovní](concepts-service-tiers.md)
- Pochopení [podporované verze databáze PostgreSQL](concepts-supported-versions.md)
- Zkontrolujte [postup zálohování a obnovení serveru v databázi Azure pro PostgreSQL pomocí portálu Azure](howto-restore-server-portal.md)