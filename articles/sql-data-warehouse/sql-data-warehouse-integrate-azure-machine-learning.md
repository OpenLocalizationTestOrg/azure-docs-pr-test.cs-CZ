---
title: aaaUse Azure Machine Learning s SQL Data Warehouse | Microsoft Docs
description: "Kurz pro používání Azure Machine Learning s Azure SQL Data Warehousem pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: ac6bc731-6add-47a9-b3fe-68996e656f4d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: fdfe8c936d2bb7a02163a0bbf6435e1ebd518d4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>SQL Data Warehouse pomocí Azure Machine Learning
Azure Machine Learning je plně spravovaná prediktivní analýzy služby, můžete použít toocreate prediktivní modely pro vaše data v SQL Data Warehouse a pak publikovat jako připravené využívají webové služby. Můžete další hello základy prediktivní analýzy a strojového učení načtením [tooMachine Úvod učení na platformě Azure][Introduction tooMachine Learning on Azure].  Potom dozvíte, jak toocreate, trénování, stanovení skóre a otestování modelu strojového učení pomocí hello [vytvořit experimentu kurzu][Create experiment tutorial].

V tomto článku se dozvíte, jak toodo hello následující pomocí hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:

* Čtení dat z vaší databáze toocreate, školení a stanovení skóre prediktivního modelu
* Zápis dat tooyour databáze

## <a name="read-data-from-sql-data-warehouse"></a>Čtení dat z SQL Data Warehouse
Jsme z produktu tabulky v databázi AdventureWorksDW hello číst data.

### <a name="step-1"></a>Krok 1
Začněte nový experiment kliknutím na + nové dole hello hello okno Machine Learning Studio, vyberte EXPERIMENT a pak vyberte prázdný Experiment. Vyberte hello výchozí experimentovat název hello horní části plátna hello a přejmenujte ji toosomething smysluplného, například předpověď ceny jízdních kol.

### <a name="step-2"></a>Krok 2
Vyhledejte modul čtečky hello v hello paleta datových sad a modulů na hello nalevo od plátna experimentu hello. Přetáhněte plátno experimentu toohello modulu hello.
![][drag_reader]

### <a name="step-3"></a>Krok 3
Vyberte modul čtečky hello a vyplňte hello podokně Vlastnosti.

1. Vyberte Azure SQL Database jako zdroj dat hello.
2. Název databázového serveru: název serveru hello typu. Můžete použít hello [portál Azure] [ Azure portal] toofind to.

![][server_name]

1. Název databáze: název typu hello databáze na serveru hello jste právě určili.
2. Název uživatelského účtu serveru: typ hello uživatelské jméno účtu, který má přístupová oprávnění pro databázi hello.
3. Heslo uživatelského účtu serveru: Zadejte heslo hello hello zadaný uživatelský účet.
4. Přijměte všechny certifikát serveru: pomocí této možnosti (méně bezpečné), pokud chcete, aby tooskip kontrola certifikát webu hello před číst vaše data.
5. Databázový dotaz: Zadejte příkaz SQL, který popisuje hello data, která chcete tooread. V takovém případě jsme bude číst data z tabulky produktu pomocí hello následující dotaz.

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Krok 4
1. Spusťte hello experiment kliknutím na tlačítko spustit pod plátnem experimentu hello.
2. Po dokončení hello experimentu modul čtečky hello bude mít tooindicate zelená značka zaškrtnutí, která byla úspěšně dokončena. Všimněte si také hello dokončeno stav spuštění v pravém horním rohu hello.

![][run]

1. toosee hello importovaných dat, klikněte na výstupní port hello v hello dolní části datové sady automobilů hello a vyberte vizualizovat.

## <a name="create-train-and-score-a-model"></a>Vytváření, trénování a stanovení skóre modelu
Teď můžete použít tuto datovou sadu, která:

* Vytvoření modelu: zpracování dat a definice funkcí
* Train hello model: volba a použití algoritmu učení
* Skóre a testování hello modelu: předpovědi nová jízdních kol cena

![][model]

Další informace o tom, jak toocreate, trénování, stanovení skóre a otestování machine learning hello použití modelu toolearn [vytvořit experimentu kurzu][Create experiment tutorial].

## <a name="write-data-tooazure-sql-data-warehouse"></a>Zápis dat tooAzure SQL Data Warehouse
Zapíše jsme hello výsledek nastavit tooProductPriceForecast tabulku v databázi AdventureWorksDW hello.

### <a name="step-1"></a>Krok 1
Vyhledejte modul zapisovače hello v hello paleta datových sad a modulů na hello nalevo od plátna experimentu hello. Přetáhněte plátno experimentu toohello modulu hello.

![][drag_writer]

### <a name="step-2"></a>Krok 2
Vyberte modul hello zapisovače a vyplňte podokno properties hello.

1. Vyberte Azure SQL Database jako hello cílové Data.
2. Název databázového serveru: název serveru hello typu. Můžete použít hello [portál Azure] [ Azure portal] toofind to.
3. Název databáze: název typu hello databáze na serveru hello jste právě určili.
4. Název uživatelského účtu serveru: typ hello uživatelské jméno účtu, který má oprávnění k zápisu pro databázi hello.
5. Heslo uživatelského účtu serveru: Zadejte heslo hello hello zadaný uživatelský účet.
6. Přijměte všechny certifikát serveru (nezabezpečené): tuto možnost vyberte, pokud nechcete, aby tooview hello certifikátu.
7. Textový soubor s oddělovači seznam sloupců toobe uložit:, které chcete toooutput, poskytovat seznam hello datové sady nebo výsledek sloupců.
8. Název tabulky dat: Zadejte název hello hello dat tabulky.
9. Seznam oddělený čárkami datatable sloupců: Zadejte toouse názvy sloupců hello v nové tabulce hello. Hello názvy sloupců může lišit od hello ty, které jsou v sadě hello zdroje dat, ale musí seznam hello stejný počet sloupců sem, který definujete pro výstupní tabulku hello.
10. Počet řádků zapsaných za operace SQL Azure: můžete nakonfigurovat hello počet řádků, které jsou zapsány tooa SQL database v rámci jedné operace.

![][writer_properties]

### <a name="step-3"></a>Krok 3
1. Spusťte hello experiment kliknutím na tlačítko spustit pod plátnem experimentu hello.
2. Po dokončení experimentu hello budou mít všechny moduly tooindicate zelená značka zaškrtnutí, která budou úspěšně dokončena.

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Create experiment tutorial]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Introduction toomachine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
