---
title: aaaOptimize algoritmy v Azure Machine Learning | Microsoft Docs
description: "Vysvětluje, jak nastavit toochoose hello optimální parametr algoritmu v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6717e30e-b8d8-4cc1-ad0b-1d4727928d32
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: fbf2f71abdbce19483fb048d67a39cbb368a928e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="choose-parameters-toooptimize-your-algorithms-in-azure-machine-learning"></a>Vyberte parametry toooptimize algoritmy v Azure Machine Learning
Toto téma popisuje, jak nastavit správné hyperparameter toochoose hello algoritmu v Azure Machine Learning. Většina algoritmy strojového učení mít tooset parametry. Pokud jste trénování modelu, je potřeba tooprovide hodnoty pro tyto parametry. Hello účinnosti hello trained model, závisí na hello modelu parametry, které zvolíte. Hello hledání hello optimální sadu parametrů, proces se označuje jako *modelu výběr*.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Existují různé způsoby toodo modelu výběr. Ve strojovém učení se křížového ověření je jednou z nejčastěji používá hello metody pro výběr modelu a je hello výchozí model výběr mechanismus v Azure Machine Learning. Protože Azure Machine Learning podporuje R a Python, můžete implementovat vlastní mechanismy Výběr modelu vždy pomocí R nebo Python.

V procesu hello nalezení hello nejlepší sada parametrů je čtyři kroky:

1. **Definování hello parametr místo**: pro algoritmus hello nejdřív rozhodněte hello přesný parametr hodnoty, které mají tooconsider.
2. **Zadejte nastavení křížové ověření hello**: Rozhodněte, jak křížové ověření toochoose složení pro datovou sadu hello.
3. **Zadejte metriku hello**: Rozhodněte, jaké metriky toouse pro určení nejlepší sadu parametrů, třeba přesnost hello, střední kořenové spolehlivosti chybu, přesnost, odvolání nebo f – score.
4. **Cvičení, hodnocení a porovnání**: pro každou jedinečnou kombinaci hodnot parametrů hello křížového ověřování prováděné a podle metrika chyba hello definujete. Po vyhodnocení a porovnání můžete vybrat nejlepší provádění modelu hello.

Hello následující obrázek znázorňuje ukazuje, jak toho lze dosáhnout v Azure Machine Learning.

![Najít hello nejlepší sadu parametrů](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-hello-parameter-space"></a>Zadejte parametr místo hello
Můžete definovat hello parametr nastavit na hello modelu inicializačnímu kroku. Hello parametr podokně všechny algoritmy strojového učení má dva režimy trainer: *jeden parametr* a *parametr rozsahu*. Zvolte režim parametr rozsahu. V režimu parametr rozsahu můžete zadat více hodnot pro jednotlivé parametry. Do textového pole hello můžete zadat hodnot oddělených čárkami.

![Two-class boosted rozhodovací strom, jeden parametr](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Alternativně můžete definovat maximální a minimální body hello hello mřížky a celkový počet bodů toobe vygeneroval s hello **Tvůrce rozsahu použití**. Ve výchozím nastavení se hodnoty parametrů hello generují u lineární stupnice. Avšak v tom případě **škálování protokolu** je zaškrtnuto, hello hodnoty jsou generovány v hello protokolu škálování (hello poměr sousedících bodů hello je konstantní místo jejich rozdíl). Pro parametry celé číslo můžete definovat rozsah použitím spojovníku. Například "1-10" znamená, že všechny celá čísla od 1 do 10 (včetně) formuláře hello sadu parametrů. Ve smíšeném režimu je také podporována. Například hello sadu parametrů "1 – 10, 20, 50" by mělo zahrnovat celá čísla 1 až 10, 20 až 50.

![Two-class boosted rozhodovací strom, parametr rozsahu](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Definování křížové ověření složení
Hello [rozdělení a vzorky] [ partition-and-sample] modulu je možné je použít toorandomly přiřazení složení toohello data. V následující ukázková konfigurace pro modul hello hello jsme definujte pět složení a náhodně přiřadit násobek číslo toohello ukázka instancí.

![Rozdělení a vzorky](./media/machine-learning-algorithm-parameters-optimize/fig4.png)

## <a name="define-hello-metric"></a>Zadejte metriku hello
Hello [Tune Model Hyperparameters] [ tune-model-hyperparameters] modulu poskytuje podporu pro empirically výběr hello nejlepší sadu parametrů pro danou algoritmus a datové sady. Kromě toho tooother informace o školení hello model hello **vlastnosti** podokně tohoto modulu zahrnuje hello Metrika pro určení nejlepší sadu parametrů hello. Má dva různé rozevírací seznamy pro klasifikaci a regrese algoritmy, v uvedeném pořadí. Pokud algoritmus hello v úvahu je klasifikační algoritmus, hello regrese metrika je ignorován a naopak. V tomto konkrétním příkladu hello metrika je **přesnost**.   

![Parametry oblouku](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Cvičení, hodnocení a porovnání
Hello stejné [Tune Model Hyperparameters] [ tune-model-hyperparameters] modulu soupravy všechny modely hello, které odpovídají toohello sadu parametrů, vyhodnotí různé metriky a potom vytvoří přizpůsobené trained model hello podle hello metriku, které zvolíte. Tento modul má dvě povinné zadání:

* Nezkušený student Hello
* Datová sada Hello

modul Hello má také volitelné datové sadě služby vstup. Připojte hello datovou sadu s násobek informace toohello povinná datová sada vstupem. Je-li datovou sadu hello nemá přiřazeny žádné informace o násobek, 10-fold křížové ověření je provedeno automaticky ve ve výchozím nastavení. Pokud není provádí hello násobek přiřazení a ověření datové sady je k dispozici na portu hello volitelné datovou sadu, je zvolen režim train-test a první datovou sadu hello je použité tootrain hello model pro každou kombinaci parametrů.

![Vylepšené rozhodovací strom třídění](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

Hello model je následně vyhodnocovaná u hello ověření datové sady. Hello vlevo výstupní port modulu ukazuje hello jiné metriky jako funkce hodnot parametrů. Hello správné výstupní port dává hello trained model, který odpovídá toohello přizpůsobené provádění modelu podle zvolené metrika toohello (**přesnost** v tomto případě).  

![Ověření datové sady](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Můžete zobrazit přesný parametry hello vybrali vizualizací hello správné výstupní port. Tento model lze použít v vyhodnocování testovací sada nebo v operationalized webové služby po uložení jako modulu trained model.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
