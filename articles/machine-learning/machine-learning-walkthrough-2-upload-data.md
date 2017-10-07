---
title: "Krok 2: Nahrání dat do experimentu Machine Learning | Microsoft Docs"
description: "Krok 2 hello vývoj prediktivního řešení návod: nahrávání veřejná data uložena do Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 0ea21dcca2d0934ed06508560cf85cf31b48ce6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Krok 2 průvodce: Nahrání stávajících dat do experimentu služby Azure Machine Learning
Toto je druhý krok hello hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. **Nahrání existujících dat**
3. [Vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů hello](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby hello](machine-learning-walkthrough-5-publish-web-service.md)
6. [Přístup k webové službě hello](machine-learning-walkthrough-6-access-web-service.md)

- - -
toodevelop prediktivního modelu pro úvěrové riziko, potřebujeme data, která jsme použít tootrain a pak hello model testů. V tomto návodu použijeme hello "UCI Statlog (němčině platební Data) Data Set" z úložiště hello UC Irvine Machine Learning. Najdete ho tady:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ICS.uci.edu/ml/datasets/Statlog+(German+Credit+data)</a>

Použijeme hello soubor s názvem **german.data**. Stáhněte si tento soubor tooyour místní pevný disk.  

Hello **german.data** datová sada obsahuje řádky 20 proměnných pro 1000 posledních žadatel o platební. Tyto proměnné 20 představují hello dataset sadu funkcí (hello *funkce vector*), který nabízí charakteristiky identifikace každý platební žadatel. Sloupec v jednotlivých řádcích představuje hello uchazeče počítané úvěrové riziko, s 700 uchazeči označený jako nízkou úvěrové riziko a 300 vysokým rizikem.

Hello UCI web obsahuje popis hello atributů hello vektor funkce pro tato data. To zahrnuje finanční informace, platební historii, stav zaměstnání a osobní údaje. Pro každý žadatel binární hodnocení byl daný, která určuje, jestli jsou na nízkou nebo vysokou úvěrového rizika. 

Použijeme tato data tootrain model prediktivní analýzy. Když jsme hotovi, našeho modelu by měl být schopný tooaccept funkce vector pro nové jednotlivé a předvídání, zda je nízkou nebo vysokou úvěrové riziko.  

Zde je zajímavé Točitost. Hello popis hello datovou sadu na hello UCI webu zmínkami nákladů Pokud jsme misclassify osoby úvěrové riziko.
Pokud hello model předpovídá vysoké úvěrové riziko pro uživatele, který je ve skutečnosti nízkou úvěrové riziko, udělal hello modelu chybnou klasifikaci.
Ale zpětné chybnou hello je pětkrát dražší toohello finanční instituce: Pokud hello model předpovídá nízkou úvěrové riziko pro uživatele, který je ve skutečnosti vysoké úvěrové riziko.

Ano chceme tootrain našeho modelu tak, aby hello náklady na tento druhý typ chybnou pětkrát vyšší než misclassifying hello jiným způsobem.
Jeden toodo jednoduchý způsob, jak to při tréninku modelu hello v našem experimentu je duplikováním (pětkrát) ty položky, které představují někdo s vysokou úvěrové riziko. Poté Pokud hello model misclassifies někdo jako nízkou úvěrové riziko, pokud jsou ve skutečnosti vysoce rizikové, hello model nemá tento stejný chybnou pětkrát, jednou pro každou duplicitní. Tím se zvýší náklady na hello této chyby ve výsledcích školení hello.


## <a name="convert-hello-dataset-format"></a>Převést formát hello datové sady
původní datové sady Hello používá formát oddělené prázdné. Machine Learning Studio funguje lépe se soubor s oddělovači (CSV), proto jsme budete převést datovou sadu hello nahrazením mezer čárkami.  

Existuje mnoho způsobů tooconvert tato data. Jedním ze způsobů je pomocí hello následující příkaz prostředí Windows PowerShell:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Dalším způsobem je pomocí příkazu menšit hello Unix:  

    sed 's/ /,/g' german.data > german.csv  

V obou případech jsme vytvořili textový soubor s oddělovači verzi hello data do souboru s názvem **german.csv** , jsme můžete použít v našem experimentu.

## <a name="upload-hello-dataset-toomachine-learning-studio"></a>Nahrát hello datovou sadu tooMachine Learning Studio
Jakmile hello data byla převedená tooCSV formátu, potřebujeme tooupload ji do nástroje Machine Learning Studio. 

1. Domovská stránka Machine Learning Studio otevřete hello ([https://studio.azureml.net](https://studio.azureml.net)). 

2. V nabídce hello ![nabídky][1] v levém horním rohu hello hello okna, klikněte na **Azure Machine Learning**, vyberte **Studio**a přihlaste se.

3. Klikněte na tlačítko **+ nový** v hello dolní části okna hello.

4. Vyberte **datovou sadu**.

5. Vyberte **z místního souboru**.

    ![Přidejte datovou sadu, z místního souboru][2]

6. V hello **nahrát nová datová sada** dialogové okno, klikněte na tlačítko **Procházet** a najde hello **german.csv** souborů, které jste vytvořili.

7. Zadejte název pro datovou sadu hello. V tomto návodu volání ji "UCI němčina platební karty Data".

8. Datový typ, vyberte **obecné soubor CSV neobsahuje záhlaví (. nh.csv)**.

9. Pokud chcete přidáte popis.

10. Klikněte na tlačítko hello **OK** zaškrtnutí.  

    ![Nahrát hello datové sady][3]

Tato hello data ukládání do datové sady modul, který používáme v experimentu.

Můžete spravovat datové sady, že jste odeslali tooStudio kliknutím hello **datové sady** levé kartě toohello okna Studio hello.

![Spravovat datové sady][4]

Další informace o importu dalších typů dat do experimentu najdete v tématu [importu trénovacích dat do Azure Machine Learning Studio](machine-learning-data-science-import-data.md).

**Další krok: [vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png
