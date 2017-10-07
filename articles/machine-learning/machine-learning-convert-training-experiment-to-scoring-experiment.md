---
title: "aaaHow tooprepare vašeho modelu pro nasazení v nástroji Azure Machine Learning Studio | Microsoft Docs"
description: "Jak tooprepare trained model pro nasazení jako webové služby Převod vaší Machine Learning Studio školení experiment prediktivní experiment tooa."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: eb943c45-541a-401d-844a-c3337de82da6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: garye
ms.openlocfilehash: d25bc68be63679a803bfc24a9e29e009a9263f5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprepare-your-model-for-deployment-in-azure-machine-learning-studio"></a>Jak tooprepare vašeho modelu pro nasazení v nástroji Azure Machine Learning Studio

Azure Machine Learning Studio poskytuje hello nástroje potřebujete toodevelop model prediktivní analýzy a zprovoznit ho pomocí jeho nasazení jako Azure webové služby.

toodo, použijte toocreate Studio experimentu - názvem *výukový experiment* – kde jste trénování, stanovení skóre a upravovat modelu. Jakmile budete spokojeni, dostanete připravené toodeploy váš model tak, že převod vaší školení experimentu tooa *prediktivní experiment* který nakonfigurovaný tooscore uživatelská data.

Můžete zobrazit příklad tohoto procesu v [návod: vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

Tento článek přebírá podrobné informace do hello podrobnosti o tom, jak získá výukový experiment převeden do prediktivní experiment a nasazení této prediktivní experiment. Pochopením tyto podrobnosti můžete získat informace jak tooconfigure vaše nasazené modelu toomake je efektivnější.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Přehled 

Hello proces převodu prediktivní experiment školení experimentu tooa zahrnuje tři kroky:

1. Nahraďte hello strojového učení algoritmu modulů s trained model.
2. Trim hello experimentu tooonly tyto moduly, které jsou potřebné pro vyhodnocování. Výukový experiment obsahuje několik modulů, které jsou nezbytné, aby školení ale nejsou potřebné, jakmile cvičení modelu hello.
3. Definujte, jak váš model bude přijímat data z uživatel hello webové služby, a jaká data budou vráceny.

> [!TIP]
> V experimentu školení jste byla nevadí, školení a vyhodnocování model pomocí svoje vlastní data. Ale po nasazení, uživatelé budou posílat nový model tooyour data a vrátí výsledky předpovědi. Tak jak je převést vaše školení experimentu tooa prediktivní experiment tooget je připravené pro nasazení, mějte na paměti použití modelu hello třetími stranami.
> 
> 

## <a name="set-up-web-service-button"></a>Tlačítko nastavit webové služby
Po spuštění experimentu (klikněte na tlačítko **spustit** dole hello plátno experimentu hello), klikněte na tlačítko hello **nastavit webové služby** tlačítko (vyberte hello **prediktivní webové služby** možnost). **Nastavení webové služby** provádí pro hello převodu prediktivní experimentu školení experimentu tooa tři kroky:

1. Ukládá trained model v hello **Trénované modely** části palety modulů hello (toohello nalevo od plátna experimentu hello). Nahradí hello algoritmu strojového učení a [Train Model] [ train-model] modulů s hello uložit cvičení modelu.
2. Ho analyzuje experimentu a odebere moduly, které byly jasně používají pouze pro školení a už nejsou potřeba.
3. Vloží _webové služby vstup_ a _výstup_ moduly do výchozího umístění v experimentu (tyto moduly přijmout a vrátit data uživatele).

Například následující hello experimentovat vlaky two-class boosted decision stromu modelu pomocí ukázkových dat úplné zjišťování:

![Výukový experiment][figure1]

Hello modulů do tohoto experimentu proveďte v podstatě čtyři různé funkce:

![Funkce modulu][figure2]

Při převodu prediktivní experiment toto školení experimentu tooa některé z těchto modulů už nejsou potřeba nebo se teď mají různé účely:

* **Data** -hello dat v této datové sadě ukázka nepoužívá během vyhodnocování – uživatel hello hello webové služby se zadat toobe data hello vypočítat skóre. Hello metadata z této datové sady, jako je například datové typy, ale používá hello trained model. Proto musíte tookeep hello datovou sadu v prediktivní experiment hello tak, že může poskytovat tato metadata.

* **Příprava** – v závislosti na hello uživatelská data, která bude odeslána pro vyhodnocování, tyto moduly může nebo nemusí být nutné tooprocess příchozích dat hello. Hello **nastavit webové služby** tlačítko nemá touch tyto – potřebovat toodecide způsob toohandle je.
  
    Například v tomto příkladu hello ukázkovou datovou sadu může mít chybějící hodnoty, takže [vyčištění chybějících dat] [ clean-missing-data] modul byl součástí toodeal s nimi. Také zahrnuje hello ukázkovou datovou sadu sloupců, které nejsou potřebné tootrain hello modelu. Proto [výběr sloupců v datové sadě] [ select-columns] modul byl součástí tooexclude tyto další sloupce z hello datový tok. Pokud znáte hello data, která bude odeslána pro vyhodnocování prostřednictvím hello webová služba nebude mít chybějící hodnoty a pak můžete odebrat hello [vyčištění chybějících dat] [ clean-missing-data] modulu. Ale protože hello [výběr sloupců v datové sadě] [ select-columns] modulu pomáhá definovat hello sloupce dat se trained model této hello očekává, že modul musí tooremain.

* **Train** -tyto moduly jsou použité tootrain hello modelu. Když kliknete na tlačítko **nastavit webové služby**, nahradí tyto moduly se jeden modul, který obsahuje model hello je cvičení. Tento nový modul se uloží do hello **Trénované modely** části palety modulů hello.

* **Skóre** – v tomto příkladu hello [rozdělení dat] [ split] modul je použité toodivide hello datový proud do testovací data a Cvičná data. V prediktivní experiment hello, jsme nejsou cvičení už nepotřebujeme, takže [rozdělení dat] [ split] lze odebrat. Podobně hello druhý [Score Model] [ score-model] modul a hello [Evaluate Model] [ evaluate-model] modulu jsou použité toocompare výsledky testu hello data, takže tyto moduly nejsou potřebné v hello prediktivní experiment. Zbývající Hello [Score Model] [ score-model] modulu, je však potřebný tooreturn výsledku skóre prostřednictvím hello webové služby.

Zde je, jak našem příkladu vypadá po kliknutí na **nastavit webové služby**:

![Převést prediktivní experiment][figure3]

Hello pracovní provádí **nastavit webové služby** pravděpodobně dostatečná tooprepare vaše experimentu toobe nasadit jako webovou službu. Ale můžete chtít toodo experimentu konkrétní tooyour některé další kroky.

### <a name="adjust-input-and-output-modules"></a>Upravte vstupní a výstupní moduly
V experimentu školení používá sadu Cvičná data a pak se některé tooget zpracování dat ve formuláři, který hello algoritmu strojového učení potřeby hello. Pokud očekáváte, že tooreceive prostřednictvím webové služby hello data hello nebudete potřebovat zpracování, můžete ho obejít: připojení hello výstup hello **vstupní modul webové služby** tooa jiného modulu v experimentu. data uživatele Hello teď dorazí v hello modelu v tomto umístění.

Například ve výchozím nastavení **nastavit webové služby** PUT hello **webové služby vstup** modulu v horní části hello vaší toku dat, jak ukazuje předchozí obrázek hello. Ale jsme můžete ručně umístit hello **webové služby vstup** po zpracování dat moduly hello:

![Přesunutí hello vstup webové služby][figure4]

Hello vstupní data poskytované prostřednictvím hello předá webová služba nyní přímo do modul určení skóre modelu hello bez jakékoli předběžného zpracování.

Podobně, ve výchozím nastavení **nastavit webové služby** PUT hello modul výstupní webové služby v hello dolní části datový tok. V tomto příkladu hello webová služba vrátí toohello uživatele hello výstup hello [Score Model] [ score-model] modul, který zahrnuje hello dokončení vstupní data vektoru plus hello vyhodnocování výsledky.
Ale pokud si přejete tooreturn něco jiný, potom můžete přidat další moduly před hello **webové služby výstup** modulu. 

Například přidejte tooreturn pouze hello vyhodnocování výsledky a není hello celý vektoru vstupních dat, [výběr sloupců v datové sadě] [ select-columns] modulu tooexclude všechny sloupce kromě hello vyhodnocování výsledky. Přesuňte hello **webové služby výstup** modulu toohello výstup hello [výběr sloupců v datové sadě] [ select-columns] modulu. Hello experiment vypadá takto:

![Přesunutí hello webové služby výstup][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Přidat nebo odebrat moduly další zpracování dat
Pokud existují další moduly v experimentu víte, že nebude potřeba při vyhodnocování, tyto lze odebrat. Například protože jsme přesunout hello **webové služby vstup** modulu tooa bodu po hello zpracování dat, moduly, jsme můžete odebrat hello [vyčištění chybějících dat] [ clean-missing-data] modulu z Prediktivní experiment Hello.

Naše prediktivní experiment nyní vypadat třeba takto:

![Odebrání dalších modulu][figure6]


### <a name="add-optional-web-service-parameters"></a>Přidejte volitelné parametry webové služby
V některých případech můžete tooallow hello uživatel chování webové služby toochange hello modulů při přístupu služby hello. *Parametry webové služby* umožňují toodo to.

Běžným příkladem je nastavení [importovat Data] [ import-data] modul, aby uživatel hello hello nasadit webové služby můžete zadat jiný zdroj dat, při přístupu k hello webové služby. Nebo konfigurace [Export dat] [ export-data] modulu tak, aby lze zadat jiný cíl.

Můžete definovat parametry webové služby a přidružit jeden nebo více parametrů modulu a můžete určit, jestli jsou požadované nebo volitelné. Při hello služby přistupuje a hello modul akce jsou upraveny Hello uživatele hello webové služby obsahuje hodnoty pro tyto parametry.

Jsou další informace o jaké parametry webové služby a jak toouse, najdete v části [pomocí parametry webové služby Azure Machine Learning][webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-hello-predictive-experiment-as-a-web-service"></a>Nasadit prediktivní experiment hello jako webovou službu
Teď, když dostatečně připravený hello prediktivní experiment, můžete ho nasadit jako Azure webové služby. Pomocí hello webové služby, mohou uživatelé odesílat data tooyour modelu a modelu hello vrátí jeho předpovědi.

Další informace o procesu hello dokončení nasazení najdete v tématu [nasazení webové služby Azure Machine Learning][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
