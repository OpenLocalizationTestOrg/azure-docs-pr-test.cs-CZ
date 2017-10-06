---
title: "Krok 4: Natrénování a vyhodnocení hello prediktivní analýzy modely | Microsoft Docs"
description: "Krok 4 hello vývoj prediktivního řešení návod: Train, stanovení skóre a vyhodnotit více modely v Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: d86d7c5ae7524f71fe44d985db67c4618b7965a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-hello-predictive-analytic-models"></a>Návod krok 4: Natrénování a vyhodnocení hello prediktivní analýzy modely
Toto téma obsahuje hello čtvrtý krok hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. [Vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. **Natrénování a vyhodnocení modelů hello**
5. [Nasazení webové služby hello](machine-learning-walkthrough-5-publish-web-service.md)
6. [Přístup k webové službě hello](machine-learning-walkthrough-6-access-web-service.md)

- - -
Jednou z výhod hello pomocí Azure Machine Learning Studio pro vytváření modelů machine learning je více než jeden typ modelu hello možnost tootry v daný okamžik v jednom experimentu a porovnejte výsledky hello. Tento typ experimentování vám pomůže najít hello nejlepší řešení pro váš problém.

V experimentu hello jsme se vývojem v tomto návodu, jsme vytvoříte dva různé typy modely a potom porovnejte jejich vyhodnocování toodecide výsledky který algoritmus chceme toouse v našem konečný experiment.  

Existují různé modely, které jsme může vybírat. toosee hello modely k dispozici, rozbalte položku hello **Machine Learning** uzel v hello palety modulů a potom rozbalte **inicializovat Model** a uzly hello pod ní. Pro účely hello tohoto experimentu, jsme vyberete hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] (SVM) a hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] moduly.    

> [!TIP]
> Nápověda tooget rozhodování, který algoritmus Machine Learning nejlépe vyhovuje hello konkrétního problému, které se snažíte toosolve najdete v tématu [jak toochoose algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).
> 
> 

## <a name="train-hello-models"></a>Cvičení hello modely

Přidáme obou hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu a [Two-Class Support Vector Machine] [ two-class-support-vector-machine] v tomto modulu experimentu.

### <a name="two-class-boosted-decision-tree"></a>Two-Class Boosted Decision Tree

Nejprve nastavíme hello boosted rozhodovacího stromu modelu.

1. Najde hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu v palety modulů hello a přetáhněte ji na plátno hello.

2. Najde hello [Train Model] [ train-model] modulu, přetáhněte na plátno hello a připojte hello výstup hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree]toohello modulu zbývajících vstupní port hello [Train Model] [ train-model] modulu.
   
   Hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu inicializuje hello obecné modelu a [Train Model] [ train-model] používá Cvičná data tootrain hello model. 

3. Připojit hello levém výstup hello vlevo [spustit skript jazyka R] [ execute-r-script] modulu toohello právo vstupní port hello [Train Model] [ train-model] modulu (jsme Rozhodli v [krok 3](machine-learning-walkthrough-3-create-new-experiment.md) tento návod toouse hello dat pocházejících z levé straně hello rozdělení dat modul pro školení hello).
   
   > [!TIP]
   > Společnost Microsoft nepotřebuje dva vstupy hello a jeden z hello výstupy hello [spustit skript jazyka R] [ execute-r-script] modul pro tento experiment, takže jsme můžete je nechat odpojit. 
   > 
   > 

Tato část hello experimentu nyní vypadat třeba takto:  

![Cvičení modelu][1]

Nyní potřebujeme tootell hello [Train Model] [ train-model] modulu, že má být hodnota úvěrové riziko hello modelu toopredict hello.

1. Vyberte hello [Train Model] [ train-model] modulu. V hello **vlastnosti** podokně klikněte na tlačítko **spustit selektor sloupců**.

2. V hello **vyberte jeden sloupec** dialogové okno, zadejte "úvěrového rizika" v poli vyhledávání hello pod **dostupné sloupce**, vyberte "Úvěrového rizika" níže a tlačítko s šipkou vpravo hello ( **>** ) toomove "Úvěrového rizika" příliš**vybrané sloupce**. 

    ![Vyberte sloupec hello úvěrové riziko pro modulu Train Model hello][0]

3. Klikněte na tlačítko hello **OK** zaškrtnutí.

### <a name="two-class-support-vector-machine"></a>Support Vector Machine (SVM) se dvěma třídami

V dalším kroku nastavíme hello SVM modelu.  

První, trochu vysvětlení SVM. Vylepšené rozhodovací stromy fungují dobře u funkce libovolného typu. Vzhledem k tomu, že modul SVM hello generuje lineární třídění, hello model, který generuje má však nejlepší Chyba testu hello Pokud všechny číselné funkce hello stejné měřítko. tooconvert numerické funkce toohello stejné škálování, používáme transformace "Tanh" (s hello [normalizaci dat] [ normalize-data] modulu). To transformuje naše čísla na rozsah hello [0,1]. Hello SVM modulu převede řetězec funkce toocategorical funkce a pak funkce toobinary 0 nebo 1, takže nepodporujeme potřebovat toomanually transformace funkce řetězec. Také jsme nechcete, aby tootransform hello úvěrové riziko sloupci (21) – je číselná, ale je hodnota hello jsme se cvičení hello toopredict modelu, takže potřebujeme tooleave ho samostatně.  

tooset až hello SVM modelu hello následující:

1. Najde hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu v palety modulů hello a přetáhněte ji na plátno hello.

2. Klikněte pravým tlačítkem na hello [Train Model] [ train-model] modulu, vyberte **kopie**a potom klikněte pravým tlačítkem na plátno hello a vyberte **vložení**. Hello kopii hello [Train Model] [ train-model] modul má hello stejné výběr sloupce jako původní hello.

3. Připojit hello výstup hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] toohello modulu zbývajících vstupní port hello druhý [Train Model] [ train-model] modul.

4. Najde hello [normalizaci dat] [ normalize-data] modulu a přetáhněte ji na plátno hello.

5. Připojit hello levém výstup hello vlevo [spustit skript jazyka R] [ execute-r-script] vstup toohello modulu tohoto modulu (Všimněte si, že hello výstupní port modulu může být připojené toomore než jeden další modul).

6. Připojit hello zbývajících výstupní port modulu hello [normalizaci dat] [ normalize-data] modulu toohello právo vstupní port hello druhý [Train Model] [ train-model] modulu.

Tato část naší experiment by teď měl vypadat přibližně takto:  

![Školení hello druhý modelu][2]  

Teď nakonfigurovat hello [normalizaci dat] [ normalize-data] modul:

1. Klikněte na tlačítko tooselect hello [normalizaci dat] [ normalize-data] modulu. V hello **vlastnosti** podokně, vyberte **Tanh** pro hello **transformace metoda** parametr.

2. Klikněte na tlačítko **spustit selektor sloupců**, vyberte možnost "Žádné sloupce" pro **začít s**, vyberte **zahrnout** v prvním rozevíracím hello vyberte **typ sloupce**v hello druhý rozevírací seznam a vyberte **číselné** v rozevírací nabídce třetí hello. Určuje transformaci všech hello číselné sloupce (a pouze číslice).

3. Klikněte na tlačítko hello toohello znaménko plus (+) napravo od tento řádek – tím se vytvoří řádek rozevíracích seznamů. Vyberte **vyloučit** v prvním rozevíracím hello vyberte **názvy sloupců** v hello druhý rozevíracího seznamu a zadejte "Úvěrového rizika" v hello textové pole. Určuje sloupce úvěrové riziko hello třeba ji ignorovat (potřebujeme toodo to protože tento sloupec je číselné a Ano by transformován Pokud jsme ho nebylo vyloučit).

4. Klikněte na tlačítko hello **OK** zaškrtnutí.  

    ![Vyberte sloupce pro modul normalizaci dat hello][5]

Hello [normalizaci dat] [ normalize-data] modul je nyní sada tooperform Tanh transformace na všechny číselné sloupce s výjimkou hello úvěrové riziko sloupce.  

## <a name="score-and-evaluate-hello-models"></a>Stanovení skóre a vyhodnocení modelů hello

Používáme hello testování data, která byla oddělených hello [rozdělení dat] [ split] modulu tooscore naše trénované modely. Jsme pak můžete porovnat výsledky hello hello dva modely toosee který vygenerovat lepší výsledky.  

### <a name="add-hello-score-model-modules"></a>Přidání modulů Score Model hello

1. Najde hello [Score Model] [ score-model] modulu a přetáhněte ji na plátno hello.

2. Připojit hello [Train Model] [ train-model] modul, který byl připojen toohello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] levé vstup toohello modulu port hello [Score Model] [ score-model] modulu.

3. Připojit hello vpravo [spustit skript jazyka R] [ execute-r-script] modulu (naše testování data) toohello právo vstupní port hello [Score Model] [ score-model] modulu.

    ![Modul určení skóre modelu připojení][6]
   
   Hello [Score Model] [ score-model] modulu může trvat teď hello platební informace z hello testování dat, spusťte ho pomocí modelu hello a porovnání hello předpovědi modelu hello generuje s hello skutečné úvěrového rizika sloupec v hello testování data.

4. Zkopírujte a vložte hello [Score Model] [ score-model] modulu toocreate druhé kopie.

5. Připojit výstup hello hello SVM modelu (tedy hello výstupní port hello [Train Model] [ train-model] modul, který byl připojen toohello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu) toohello vstupní port hello druhý [Score Model] [ score-model] modulu.

6. Pro hello SVM model máme toodo hello stejnou transformaci toohello testovací data, jako jsme to udělali toohello Cvičná data. Proto zkopírujte a vložte hello [normalizaci dat] [ normalize-data] modulu toocreate druhé kopie a připojte ho toohello vpravo [spustit skript jazyka R] [ execute-r-script] modulu.

7. Druhý připojit hello levém výstup hello [normalizaci dat] [ normalize-data] modulu toohello právo vstupní port hello druhý [Score Model] [ score-model] modul.

    ![Obě Score Model moduly připojení][7]

### <a name="add-hello-evaluate-model-module"></a>Přidání modulu Evaluate Model hello

tooevaluate hello oba vyhodnocování výsledky a porovnat je, používáme [Evaluate Model] [ evaluate-model] modulu.  

1. Najde hello [Evaluate Model] [ evaluate-model] modulu a přetáhněte ji na plátno hello.

2. Připojit hello výstupní port modulu hello [Score Model] [ score-model] modulu přidružené hello boosted rozhodovacího stromu modelu vlevo toohello vstupní port hello [Evaluate Model] [ evaluate-model] modulu.

3. Připojit hello jiných [Score Model] [ score-model] modulu toohello právo vstupní port.  

    ![Vyhodnocení modelu modulu připojení][8]

### <a name="run-hello-experiment-and-check-hello-results"></a>Spusťte hello experiment a hello výsledky kontroly

toorun hello experiment, klikněte na tlačítko hello **spustit** tlačítko pod plátnem hello. To může trvat několik minut. Na roztočený ukazatel na každý modul ukazuje, zda je spuštěná a poté zobrazí zelená značka zaškrtnutí po dokončení modulu hello. Pokud všechny moduly hello zaškrtnutí, hello experimentu bylo dokončeno.

Hello experiment by teď měl vypadat přibližně takto:  

![Vyhodnocení obou modelů][3]

toocheck hello výsledky, klikněte na výstupní port hello hello [Evaluate Model] [ evaluate-model] modul a vyberte **vizualizovat**.  

Hello [Evaluate Model] [ evaluate-model] modulu vytvoří dvojici křivek a metriky, které umožňují toocompare hello výsledky dva modely scored hello. Můžete zobrazit výsledky hello jako křivek příjemce operátor vlastnosti (ROC), křivek přesnost nebo odvolání nebo křivek navýšení. Další data zobrazí zahrnuje matice nejasnostem, kumulativní hodnoty pro oblast hello pod křivkou hello (AUC) a další metriky. Můžete změnit tak, že přesunutí hello posuvník doleva nebo doprava hello prahovou hodnotu a zjistit, jak ovlivňuje hello sadu metriky.  

Klikněte na tlačítko toohello napravo od grafu hello **skóre pro Magnitudu datovou sadu** nebo **skóre pro Magnitudu toocompare datovou sadu** hello hello toohighlight přidružené křivky a toodisplay přidružené metriky níže. V legendě hello pro hello křivek, "Skóre pro Magnitudu datovou sadu" odpovídá toohello zbývajících vstupní port hello [Evaluate Model] [ evaluate-model] modul – v našem případě je to hello boosted rozhodovacího stromu modelu. "Skóre pro magnitudu toocompare datovou sadu" odpovídá toohello pravý vstupní port - hello SVM modelu v našem případě. Když kliknete na jednu z těchto popisky, hello křivky pro tento model je označený a odpovídající metriky hello se zobrazí, jak ukazuje následující obrázek hello.  

![Křivek ROC pro modely][4]

Prověřením tyto hodnoty můžete rozhodnout, které model nejbližší toogiving hello výsledků, které hledáte. Můžete se vrátit zpět a iterovat experimentu změnou hodnoty parametrů v různých modelech hello. 

Hello vědy a obrázky interpretace těchto výsledků a ladění výkonu modelu hello je mimo rámec tohoto návodu hello. Potřebujete další pomoc můžou číst hello následující články:
- [Jak tooevaluate modelu výkon v Azure Machine Learning](machine-learning-evaluate-model-performance.md)
- [Vyberte parametry toooptimize algoritmy v Azure Machine Learning](machine-learning-algorithm-parameters-optimize.md)
- [Interpretovat výsledky modelu v Azure Machine Learning](machine-learning-interpret-model-results.md)

> [!TIP]
> Pokaždé, když spustíte hello experimentu záznam této iterace je uložen v hello spustit historie. Můžete zobrazit tyto iterací a vrátí tooany z nich, kliknutím na **zobrazit HISTORII BĚHŮ** níže hello plátno. Můžete také kliknout na **předchozí spustit** v hello **vlastnosti** podokně tooreturn toohello iterace bezprostředně předcházející hello jeden máte otevřený.
> 
> Kliknutím můžete vytvořit kopii kteroukoli iteraci experimentu **uložit jako** níže hello plátno. 
> Použít hello experimentu **Souhrn** a **popis** vlastnosti tookeep záznam jste zkusili ve vašem iterací experimentu.
> 
> Další podrobnosti najdete v tématu [Správa iterací experimentu v nástroji Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).  
> 
> 

- - -
**Další krok: [nasazení hello webové služby](machine-learning-walkthrough-5-publish-web-service.md)**

[0]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
