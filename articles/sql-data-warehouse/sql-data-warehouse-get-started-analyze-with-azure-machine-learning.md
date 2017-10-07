---
title: "aaaAnalyze dat pomocí Azure Machine Learning | Microsoft Docs"
description: "Pomocí Azure Machine Learning toobuild prediktivní strojového učení modelu na základě dat uložené v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 03/02/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 337a2cd77aaad4467683827c56e5015b262b2554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-with-azure-machine-learning"></a>Analýza dat pomocí Azure Machine Learning
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Tento kurz používá Azure Machine Learning toobuild prediktivní strojového učení modelu na základě dat uložené v Azure SQL Data Warehouse. Konkrétně to sestavení cílenou marketingovou kampaň společnosti Adventure Works, hello kolo obchod, Odhadnutím toho, jaká Pokud zákazník je pravděpodobně toobuy kolo nebo ne.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím tohoto kurzu potřebujete:

* SQL Data Warehouse s předem načtenými vzorovými daty AdventureWorksDW. tooprovision, najdete v tématu [vytvořit SQL Data Warehouse] [ Create a SQL Data Warehouse] a zvolte tooload hello ukázková data. Pokud už Data Warehouse máte, ale nemáte ukázková data, můžete [ukázková data načíst ručně][load sample data manually].

## <a name="1-get-hello-data"></a>1. Získat hello data
Hello data jsou v hello zobrazení dbo.vTargetMail v databázi AdventureWorksDW hello. tooread tato data:

1. Přihlaste se k [Azure Machine Learning Studio][Azure Machine Learning studio] a klikněte na Moje experimenty.
2. Klikněte na **+NOVÝ** a vyberte **Prázdný experiment**.
3. Zadejte název svého experimentu: Cílený marketing.
4. Přetáhněte hello **čtečky** modul z podokna modulů hello hello plátno.
5. Zadejte podrobnosti hello vaší databáze SQL datového skladu v podokně Vlastnosti hello.
6. Zadejte databázi hello **dotazu** tooread hello dat, které vás zajímají.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Spusťte hello experiment kliknutím **spustit** pod plátnem experimentu hello.
![Spusťte hello experiment][1]

Po dokončení experimentu hello spuštěna úspěšně, klikněte na výstupní port hello v hello dolní části modulu Reader hello a vyberte možnost **vizualizovat** toosee hello naimportovalo data.
![Zobrazení naimportovaných dat][3]

## <a name="2-clean-hello-data"></a>2. Vyčištění hello dat
tooclean hello data, vyřadit některé sloupce, které nejsou důležité pro hello model. toodo toto:

1. Přetáhněte hello **sloupce projektu** hello plátno modul.
2. Klikněte na tlačítko **spustit selektor sloupců** v hello vlastnosti podokně toospecify sloupce, které chcete toodrop.
   ![Project Columns][4]
3. Vylučte dva sloupce: CustomerAlternateKey a GeographyKey.
   ![Odebrání nepotřebných sloupců][5]

## <a name="3-build-hello-model"></a>3. Vytvoření modelu hello
Rozdělíme hello data 80: 20: 80 % tootrain model machine learning a 20 % tootest hello modelu. Budeme používat algoritmy hello "Two-Class" pro tento problém binární klasifikace.

1. Přetáhněte hello **rozdělení** hello plátno modul.
2. Zadejte 0,8 podíl řádků v první výstupní sadě hello dat v podokně Vlastnosti hello.
   ![Rozdělení dat na sadu učení a testovací sadu][6]
3. Přetáhněte hello **Two-Class Boosted Decision Tree** hello plátno modul.
4. Přetáhněte hello **Train Model** modul hello plátno a určete vstupy hello. Potom klikněte na **spustit selektor sloupců** v podokně Vlastnosti hello.
   * První vstup: Algoritmus strojového učení
   * Druhý vstup: algoritmus hello tootrain dat na.
     ![Připojení modulu Train Model hello][7]
5. Vyberte hello **BikeBuyer** sloupec jako sloupec toopredict hello.
   ![Vyberte sloupec toopredict][8]

## <a name="4-score-hello-model"></a>4. Určení skóre modelu hello
Teď otestujeme, jak hello model provádí testovacích datech. Porovnáme námi zvolený s toosee jiný algoritmus, který provádí lépe algoritmus hello.

1. Přetáhněte **Score Model** hello plátno modul.
    První vstup: Train model druhý vstup: Test data ![hello skóre][9]
2. Přetáhněte hello **Two-Class Bayes Point Machine** hello plátno experimentu. Porovnáme, jak tento algoritmus provádí v porovnání toohello Two-Class Boosted Decision Tree.
3. Kopírování a vložení hello moduly Train Model a Score Model v hello plátno.
4. Přetáhněte hello **Evaluate Model** modulu do hello plátno toocompare hello dva algoritmů.
5. **Spustit** hello experimentu.
   ![Spusťte hello experiment][10]
6. Klikněte na výstupní port hello v hello dolní části modulu Evaluate Model hello a klikněte na tlačítko vizualizovat.
   ![Vizualizace výsledků vyhodnocení][11]

poskytuje metriky Hello jsou hello: křivka ROC, diagram přesnosti a křivka navýšení. Vyhledávání na tyto metriky podíváme, vidíme této hello první model má lepší výsledky než druhý hello. toolook v hello co hello předpověděl první model, klikněte na výstupní port modulu Score Model hello a klikněte na vizualizovat.
![Vizualizace výsledků skóre][12]

Zobrazí se další dva sloupce přidat tooyour testovací datové sady.

* Scored Pravděpodobnostech: hello pravděpodobnost, že si zákazník koupí kolo.
* Scored popisky: hello klasifikace prováděná modelem hello – kolo (1) nebo ne (0). Tato prahová hodnota pravděpodobnosti pro popisky je nastavena too50 % a lze upravit.

Porovnání hello sloupce BikeBuyer (skutečnost) s hello popisky vyhodnocení (předpověď), můžete zobrazit, jak dobře hello model provedl. V dalších krocích můžete použít tento model toomake předpovědi nových zákazníků a publikovat tento model jako webovou službu nebo můžete zapsat výsledky zpět tooSQL datového skladu.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o vytváření prediktivních modelů strojového učení, najdete v příliš[tooMachine Úvod učení na platformě Azure][Introduction tooMachine Learning on Azure].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction tooMachine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
