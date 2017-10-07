---
title: "aaaImport data do nástroje Machine Learning Studio | Microsoft Docs"
description: "Jak tooimport data do Azure Machine Learning Studio z různých zdrojů dat.. Zjistěte, jaké datové typy a formáty dat jsou podporovány."
keywords: "Importujte dat, formát dat, datové typy, zdroje dat, Cvičná data"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: garye;bradsev
ms.openlocfilehash: 830dcdde9d43809900c520a41d6d94a65731ca3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Import cvičných dat do nástroje Azure Machine Learning Studio z různých zdrojů dat
toouse svoje vlastní data v Machine Learning Studio toodevelop a train řešení prediktivní analýzy, můžete: 

* nahrání dat z **místního souboru** dříve času z vašeho pevného disku toocreate datovou sadu modulu v pracovním prostoru
* přístup k datům z jednoho z několika **zdroje dat online** experimentu je spuštěn pomocí hello [importovat Data] [ import-data] modulu 
* použít data z jiného Azure Machine learning **experimentovat** uložit jako datové sady
* použít data z místního **databáze systému SQL Server**

Každá z těchto možností je popsán v tématech hello v nabídce hello níže. Tato témata ukazují, jak tooimport dat od těchto různé zdroje toouse v nástroji Machine Learning Studio. 

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> Nejsou k dispozici v Machine Learning Studio, který můžete použít pro Cvičná data několik ukázkových datových sad. Informace naleznete v tématu [použít hello ukázkových datových sad v nástroji Azure Machine Learning Studio](machine-learning-use-sample-datasets.md)).
> 
> 

Toto úvodní téma také popisuje, jak tooget dat připravené pro použití v nástroji Machine Learning Studio a popisuje, jaké formáty dat a datové typy jsou podporovány. 

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Příprava dat pro použití v nástroji Azure Machine Learning Studio
Machine Learning Studio je navrženou toowork obdélníková nebo tabulkovém daty, jako je například textová data, která má oddělený nebo strukturovaná data z databáze, i když se v některých případech může použít obdélníkový data.

Je nejvhodnější Pokud vaše data jsou relativně vyčistit. To znamená budete muset tootake péče o problémy, jako jsou třeba nekotovaných řetězce před nahráním dat hello do experimentu.

Však nejsou k dispozici v Machine Learning Studio, které umožňují některé manipulaci s daty v rámci experimentu moduly. V závislosti na tom, které budete používat algoritmů strojového učení hello, může být nutné toodecide jak budete zpracovávají data strukturální problémy, například chybějící hodnoty a zhuštěných dat a jsou moduly, které mohou pomoci s které. Hledat v hello **transformaci dat** části palety modulů hello pro moduly, které provádějí tyto funkce.

V libovolném bodě v experimentu můžete zobrazit nebo stáhnout hello data, která je produkovaný modul kliknutím na výstupní port hello. V závislosti na modulu hello je možné možnosti různých stahování k dispozici, nebo může být schopný toovisualize hello dat webového prohlížeče v nástroji Machine Learning Studio.

## <a name="data-formats-and-data-types-supported"></a>Data formátů a datové typy podporované
Můžete importovat do experimentu několik typů dat, v závislosti na tom, jaký mechanismus používáte tooimport dat a odkud pocházejí z:

* Prostý text (TXT)
* Hodnot oddělených čárkami (CSV) s hlavičkou (CSV) nebo bez (. nh.csv)
* Karta s oddělovači (TSV) s hlavičkou (TSV) nebo bez (. nh.tsv)
* Soubor aplikace Excel
* Tabulky Azure
* Tabulku Hive
* Tabulka databáze SQL
* Hodnoty OData
* Data SVMLight (.svmlight) (viz hello [SVMLight definice](http://svmlight.joachims.org/) formátu informace)
* Atribut dat vztah soubor formátu (ARFF) (.arff) (viz hello [ARFF definice](http://weka.wikispaces.com/ARFF) formátu informace)
* Soubor ZIP (.zip)
* R objekt nebo prostoru souboru (. RData)

Pokud importujete data ve formátu, například ARFF, který obsahuje metadata, Machine Learning Studio používá tento nadpis hello toodefine metadata a datový typ jednotlivých sloupců.

Pokud importujete data, jako jsou TSV nebo CSV formátu, který neobsahuje tato metadata, Machine Learning Studio odvodí hello datový typ pro každý sloupec vzorkováním hello data. Pokud hello data také nemá záhlaví sloupců, Machine Learning Studio obsahuje výchozí názvy.

Můžete explicitně zadat nebo změnit hello záhlaví a datové typy pro sloupce pomocí hello [upravit Metadata][edit-metadata].

Následující Hello **datové typy** rozpoznává Machine Learning Studio:

* Řetězec
* Integer
* Double
* Logická hodnota
* Data a času
* Časový interval

Machine Learning Studio používá typ interních datových názvem ***tabulky dat*** toopass dat mezi moduly. Data můžete explicitně převést do formátu dat tabulky pomocí hello [převést tooDataset] [ convert-to-dataset] modulu.

Libovolný modul, který přijímá formáty než tabulky datového převede hello data tooData tabulky bez upozornění před jeho odesláním toohello další modul.

V případě potřeby můžete převést formát Data tabulky zpět do sdíleného svazku clusteru, TSV, ARFF nebo SVMLight formátu pomocí převodu z ostatních modulů.
Hledat v hello **převody formát dat** části palety modulů hello pro moduly, které provádějí tyto funkce.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
