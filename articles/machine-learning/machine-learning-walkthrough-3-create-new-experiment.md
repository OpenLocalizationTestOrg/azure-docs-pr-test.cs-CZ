---
title: "Krok 3: Vytvoření nového experimentu Machine Learning | Microsoft Docs"
description: "Krok 3 hello vývoj prediktivního řešení návod: vytvoření nového experimentu školení v Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 4697d461a205c50c8d2aa6a3bd56697840cb30f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Krok 3 průvodce: Vytvoření nového experimentu služby Azure Machine Learning
Toto je třetí krok hello hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. **Vytvoření nového experimentu**
4. [Natrénování a vyhodnocení modelů hello](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby hello](machine-learning-walkthrough-5-publish-web-service.md)
6. [Přístup k webové službě hello](machine-learning-walkthrough-6-access-web-service.md)

- - -
dalším krokem Hello v tomto návodu je toocreate experimentu v Machine Learning Studio používající hello datovou sadu, které jsme nahrát.  

1. V nástroji Studio klikněte na tlačítko **+ nový** v hello dolní části okna hello.
2. Vyberte **EXPERIMENTU**a potom vyberte "Prázdný Experiment". 

    ![Vytvoření nového experimentu][0]

2. Vyberte hello výchozí experimentovat název hello horní části plátna hello a přejmenujte ji toosomething smysluplný.

    ![Přejmenujte experimentu][5]
   
   > [!TIP]
   > Je dobrým zvykem toofill v **Souhrn** a **popis** hello experimentu v hello **vlastnosti** podokně. Tyto vlastnosti udělení hello prvního toodocument hello experimentu tak, aby každý, kdo ho později zabývá pochopili záměry a metody.
   > 
   > ![Vlastnosti experimentu][6]
   > 
3. V hello modulu palety toohello nalevo od plátna experimentu hello, rozbalte položku **uložit datové sady**.
4. Datová sada hello najít, které jste vytvořili v části **Moje datové sady** a přetáhněte ji na plátno hello. Můžete také najít datovou sadu hello tak, že zadáte název hello hello **vyhledávání** pole výše palety hello.  

    ![Přidat hello datovou sadu toohello experimentu][7]

## <a name="prepare-hello-data"></a>Příprava dat hello
Můžete zobrazit hello prvních 100 řádků hello dat a některé statistické informace pro celou datovou sadu hello: klikněte na výstupní port hello hello sady dat (přeškrtnutým kroužkem pro hello dolnímu hello) a vyberte **vizualizovat**.  

Protože hello datový soubor nebyl dodán záhlaví sloupců, Studio poskytl obecné záhlaví (Sloupec1, Sloupec2, *atd*). Základní toocreating model nejsou funkční záhlaví, ale jeho udělají jednodušší toowork daty hello v experimentu hello. Navíc pokud jsme nakonec publikovat tento model ve webové službě, záhlaví hello pomoci při identifikaci hello sloupce toohello uživatel hello služby.  

Nyní můžete přidat pomocí hello záhlaví sloupců [upravit Metadata] [ edit-metadata] modulu.
Použít hello [upravit Metadata] [ edit-metadata] modulu toochange metadata spojená s datovou sadu. V tomto případě používáme ho tooprovide další popisný názvy pro záhlaví sloupců. 

toouse [upravit Metadata][edit-metadata], nejprve zadat které toomodify sloupce (v tomto případě všechny z nich.) Dále určit toobe hello akce provést na tyto sloupce (v tomto případě změna záhlaví sloupců.)

1. V modulu palety hello, zadejte "metadata" v hello **vyhledávání** pole. Hello [upravit Metadata] [ edit-metadata] se zobrazí v seznamu modul hello.

2. Klikněte a přetáhněte hello [upravit Metadata] [ edit-metadata] modulu do hello plátno a umístěte jej pod datovou sadu hello jsme přidali dříve.

3. Připojit hello datovou sadu toohello [upravit Metadata][edit-metadata]: klikněte na výstupní port hello hello sady dat (přeškrtnutým kroužkem pro hello dole hello datovou sadu hello), přetáhněte toohello vstupní port [upravit Metadata ] [ edit-metadata] (hello přeškrtnutým kroužkem hello horní části modulu hello), pak uvolnění tlačítka myši hello. datovou sadu Hello a modul zůstanou připojené, i když přesouváte buď na plátně hello.
   
   Hello experiment by teď měl vypadat přibližně takto:  
   
   ![Přidání metadat úpravy][1]
   
   Hello červený vykřičník označuje, že jsme hello vlastnosti pro tento modul nenastavili ještě. Provedeme tento další.
   
   > [!TIP]
   > Můžete přidat komentář modul tooa tak, že dvakrát kliknete na panel hello modulu a zadání textu. Díky tomu můžete ihned uvidíte, jaké hello modul provádí v experimentu. V takovém případě klikněte dvakrát na hello [upravit Metadata] [ edit-metadata] modulu a typu hello komentář "přidat záhlaví sloupců". Kdekoliv jinde, klikněte na hello plátno tooclose hello textové pole. toodisplay hello komentář, klikněte na šipku dolů hello v modulu hello.
   > 
   > ![Upravit komentář přidat Metadata modulu][8]
   > 
4. Vyberte [upravit Metadata][edit-metadata]a v hello **vlastnosti** toohello podokně napravo od plátna hello, klikněte na tlačítko **spustit selektor sloupců**.

5. V hello **vyberte sloupce** dialogové okno, vyberte všechny řádky v hello **dostupné sloupce** a klikněte na tlačítko > toomove je příliš**vybrané sloupce**.
   Dialogové okno Hello by měl vypadat takto:

   ![Selektor sloupců s vybrané všechny sloupce][2]

6. Klikněte na tlačítko hello **OK** zaškrtnutí.

7. Zpět v hello **vlastnosti** podokně, podívejte se na hello **nové názvy sloupců** parametr. V tomto poli zadejte seznam názvů pro hello 21 sloupců v datové sadě hello, oddělených čárkami a pořadí sloupců. Názvy sloupců hello můžete získat z dokumentace hello datové sady na webu UCI hello nebo ke zvýšení pohodlí můžete zkopírovat a vložit hello následující seznamu:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   Podokno Properties Hello vypadá takto:
   
   ![Vlastnosti pro úpravy metadat][3]

> [!TIP]
> Pokud chcete záhlaví sloupců hello tooverify, spustit hello experiment (klikněte na tlačítko **spustit** pod plátnem experimentu hello). Po jeho dokončení spuštění (zelená značka zaškrtnutí se zobrazí na [upravit Metadata][edit-metadata]), klikněte na výstupní port hello hello [upravit Metadata] [ edit-metadata] modul a vyberte **vizualizovat**. Zobrazí se výstup hello libovolný modul v hello tooview způsob stejné hello průběh hello data prostřednictvím hello experimentu.
> 
> 

## <a name="create-training-and-test-datasets"></a>Vytvoření školení a testovací datové sady
Potřebujeme některé datový model hello tootrain a některé tootest ho.
Proto v dalším kroku hello pokusu se hello jsme datovou sadu hello rozdělit na dvě samostatné datové sady: jeden pro cvičení našeho modelu a jeden pro testování ho.

toodo toho používáme hello [rozdělení dat] [ split] modulu.  

1. Najde hello [rozdělení dat] [ split] modulu, přetáhněte na plátno hello a připojte ho toohello [upravit Metadata] [ edit-metadata] modulu.

2. Ve výchozím nastavení, je poměr rozdělení hello 0,5 a hello **Randomized rozdělení** parametr je nastaven. Znamená, že půl náhodných hello dat je výstup prostřednictvím jednoho portu hello [rozdělení dat] [ split] modulu a půl až hello jiné. Vám může upravit tyto parametry, stejně jako hello **náhodná počáteční hodnota** parametr toochange hello rozdělit mezi trénování a testování data. V tomto příkladu jsme je jako ponechte-je.
   
   > [!TIP]
   > Hello vlastnost **podíl řádků v hello nejprve výstupní datovou sadu** Určuje, kolik dat hello je výstup prostřednictvím hello *levém* výstupní port. Například pokud nastavíte too0.7 poměr hello, 70 % hello dat je výstup prostřednictvím hello zbývajících port a z 30 % prostřednictvím hello pravý port.  
   > 
   > 

3. Klikněte dvakrát na hello [rozdělení dat] [ split] modul a zadejte komentář hello, "školení nebo testování dat rozdělení 50 %". 

Můžeme použít hello výstupy hello [rozdělení dat] [ split] modulu jsme jako, ale umožňuje vybrat toouse hello levý výstupní jako Cvičná data a hello právo výstupní data jako testování.  

Jak je uvedeno v hello [předchozího kroku](machine-learning-walkthrough-2-upload-data.md), misclassifying vysokou úvěrové riziko jako nízké náklady hello je pětkrát vyšší než náklady hello misclassifying nízkou úvěrové riziko jako Vysoká. tooaccount pro to, se vygeneruje nová datová sada, která odráží tuto funkci náklady. V hello nová datová sada každý příklad vysoce rizikové replikují pětkrát, zatímco se replikují každý příklad nízké riziko.   

Provedeme tato replikace pomocí kódu jazyka R:  

1. Najděte a přetáhněte hello [spustit skript jazyka R] [ execute-r-script] modulu na plátno experimentu hello. 

2. Připojte zbývající výstupní port modulu hello hello [rozdělení dat] [ split] modulu toohello první vstup port ("Dataset1") z hello [spustit skript jazyka R] [ execute-r-script] modul.

3. Klikněte dvakrát na hello [spustit skript jazyka R] [ execute-r-script] modul a zadejte komentář hello "Sada úprava nákladů".

4. V hello **vlastnosti** podokně odstranění hello výchozí text v hello **skript jazyka R** parametr a zadejte tento skript:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R skript v modulu hello spustit skript jazyka R][9]

Potřebujeme toodo stejná operace replikace pro každé výstup z hello [rozdělení dat] [ split] modulu tak, že hello trénování a testování dat mají hello stejné úprava nákladů. Nejjednodušší způsob, jak toodo jedná se o duplikování hello Hello [spustit skript jazyka R] [ execute-r-script] modulu jsme právě provedli a jejím propojením toohello jiné výstupní port hello [rozdělení dat] [ split] modulu.

1. Klikněte pravým tlačítkem na hello [spustit skript jazyka R] [ execute-r-script] modul a vyberte **kopie**.

2. Klikněte pravým tlačítkem na plátno experimentu hello a vyberte **vložení**.

3. Přetáhněte hello nového modulu do pozice a potom se připojte hello správné výstupní port modulu hello [rozdělení dat] [ split] modulu toohello nové první vstup portu tohoto [spustit skript jazyka R] [ execute-r-script] modulu. 

4. Hello dolní části plátna hello, klikněte na **spustit**. 

> [!TIP]
> Hello kopii modulu spustit skript jazyka R hello obsahuje hello stejné jako původní modul hello skriptu. Při kopírování a vkládání modul na plátno hello hello kopie uchovává všechny vlastnosti hello hello původní.  
> 
> 

Naše experimentu nyní vypadat třeba takto:

![Přidání modulu rozdělení a skripty R][4]

Další informace o použití skripty R v experimentů najdete v tématu [rozšíření experimentů pomocí R](machine-learning-extend-your-experiment-with-r.md).

**Další krok: [Train a vyhodnocení modelů hello](machine-learning-walkthrough-4-train-and-evaluate-models.md)**

[0]: ./media/machine-learning-walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/machine-learning-walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/machine-learning-walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/machine-learning-walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
