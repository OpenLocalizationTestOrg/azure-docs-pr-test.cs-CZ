---
title: aaaUse Azure Stream Analytics s SQL Data Warehouse | Microsoft Docs
description: "Tipy pro používání Azure Stream Analytics s datovým skladem SQL Azure pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1278197a6764864124fd92fc672de00b83ec343f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Použití služby Azure Stream Analytics s SQL Data Warehouse
Azure Stream Analytics je plně spravovaná služba poskytuje nízkou latencí, vysoce dostupných, škálovatelných komplexní zpracování událostí přes streamování dat v cloudu hello. Dozvíte se základy hello načtením [tooAzure Úvod Stream Analytics][Introduction tooAzure Stream Analytics]. Potom dozvíte, jak toocreate řešení začátku do konce pomocí služby Stream Analytics podle hello [začít používat Azure Stream Analytics] [ Get started using Azure Stream Analytics] kurzu.

V tomto článku se dozvíte, jak toouse Azure SQL Data Warehouse databáze jako výstupní jímku pro datový proud Analytics úlohy.

## <a name="prerequisites"></a>Požadavky
Nejprve spustit prostřednictvím hello proveďte kroky v hello [začít používat Azure Stream Analytics] [ Get started using Azure Stream Analytics] kurzu.  

1. Vytvoření centra událostí vstup
2. Nakonfigurujte a spusťte aplikaci generátor událostí
3. Zřídit úloha Stream Analytics
4. Zadejte vstup úlohy a dotaz

Pak vytvořte databázi Azure SQL Data Warehouse

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Zadejte výstup úlohy: databáze Azure SQL Data Warehouse
### <a name="step-1"></a>Krok 1
V úloze Stream Analytics klikněte na tlačítko **výstup** shora hello hello stránky a pak klikněte na tlačítko **přidat výstup**.

### <a name="step-2"></a>Krok 2
Vyberte databázi SQL a klikněte na tlačítko Další.

![][add-output]

### <a name="step-3"></a>Krok 3
Zadejte následující hodnoty na další stránku hello hello:

* *Výstup Alias*: Zadejte popisný název pro tento výstup úlohy.
* *Předplatné*:
  * Pokud je databáze SQL Data Warehouse v hello stejnému předplatnému jako hello úlohy služby Stream Analytics, vyberte použít databázi SQL z aktuálního předplatného.
  * Pokud vaše databáze je v jiném předplatném, vyberte použít SQL Database z jiné předplatné.
* *Databáze*: Zadejte název hello cílové databáze.
* *Název serveru*: Zadejte název serveru hello hello databáze, které jste zadali. Tímto způsobem můžete portál Azure Classic toofind hello.

![][server-name]

* *Uživatelské jméno*: Zadejte hello uživatelské jméno účtu, který má oprávnění k zápisu pro databázi hello.
* *Heslo*: Zadejte heslo hello hello zadaný uživatelský účet.
* *Tabulka*: Zadejte název hello hello cílové tabulky v databázi hello.

![][add-database]

### <a name="step-4"></a>Krok 4
Tento výstup úlohy a tooverify Stream Analytics můžete úspěšně připojit toohello databáze, klikněte na tlačítko tooadd tlačítko Kontrola hello.

![][test-connection]

Pokud je databáze toohello připojení hello úspěšná, zobrazí se oznámení v dolním hello hello portálu. Test připojení můžete kliknutím na hello dolní tootest hello připojení toohello databáze.

## <a name="next-steps"></a>Další kroky
Přehled integrace najdete v tématu [Přehled integrace SQL Data Warehouse][SQL Data Warehouse integration overview].

Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction tooAzure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
