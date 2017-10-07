---
title: "Webová služba bude aaaHow model Azure Machine Learning | Microsoft Docs"
description: "Přehled hello mechanismů jak vaše postupuje model Azure Machine Learning z vývojovém experimentovat tooan operationalized webové služby."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 25e0c025-f8b0-44ab-beaf-d0f2d485eb91
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 2bbe09fa8fa22662cf8ec4a8b6249d23c87c8ddf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-a-machine-learning-model-progresses-from-an-experiment-tooan-operationalized-web-service"></a>Jak operationalized postupuje model Machine Learning z tooan experimentu webové služby
Azure Machine Learning Studio poskytuje interaktivní plátno, který vám umožní toodevelop, spuštění, testovat a iterovat ***experimentovat*** představující model prediktivní analýzy. Existují širokou škálu moduly, které můžou:

* Vstupní data do experimentu
* Pracovat s daty hello
* Cvičení modelu pomocí algoritmy strojového učení
* Určení skóre modelu hello
* Vyhodnoťte výsledky hello
* Hodnoty finální výstup

Jakmile budete spokojeni s experimentu, můžete nasadit jako ***Classic Azure Machine Learning webové služby*** nebo ***nové Azure Machine Learning webové služby*** , aby uživatelé mohli odeslat nová data a zobrazí zpět výsledky.

V tomto článku jsme poskytnutí přehledu o hello mechanismů jak operationalized vaše postupuje model Machine Learning z tooan vývoj experimentu webové služby.

> [!NOTE]
> Existují jiné způsoby toodevelop a nasadit modely machine learning, ale tento článek se zaměřuje na tom, jak používáte Machine Learning Studio. Například tooread popis jak toocreate classic prediktivní webové služby pomocí R, najdete v příspěvku blogu hello [sestavení & nasadit prediktivní webové aplikace pomocí Rstudia a Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).
> 
> 

Zatímco Azure Machine Learning Studio je navrženou toohelp vývoji a nasazení *model prediktivní analýzy*, je možné toouse Studio toodevelop experimentu, který neobsahuje model prediktivní analýzy. Například experimentu může jenom vstupní data, zpracování ho a pak výsledky výstup hello. Stejně jako experiment prediktivní analýzy můžete nasadit tento není prediktivní experiment jako webovou službu, ale je jednodušší proces, protože hello experimentu není cvičení nebo vyhodnocování model machine learning. I když není hello typické toouse Studio tímto způsobem, jsme budete ho zahrňte v diskusi hello tak, aby bylo možné přidělit podrobné informace o tom, jak funguje Studio.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Vývoj a nasazení prediktivní webové služby
Zde jsou hello fází, které následuje typický řešení při vývoji a nasadit pomocí Machine Learning Studio:

![Tok nasazení](media/machine-learning-model-progression-experiment-to-web-service/model-stages-from-experiment-to-web-service.png)

*Obrázek 1 – fáze model typické prediktivní analýzy*

### <a name="hello-training-experiment"></a>Výukový experiment Hello
Hello ***výukový experiment*** je hello počáteční fáze vývoje webové služby v nástroji Machine Learning Studio. účelem Hello hello výukový experiment je toogive jste toodevelop místní testování, iterace a nakonec cvičení strojového učení modelu. Vám může i cvičení více modelů současně jako vyhledejte hello nejlepším řešením, avšak po dokončení můžete experimentovat vyberte jeden Trénink modelu a eliminovat hello rest z hello experimentu. Příklad vývoj experiment prediktivní analýzy, naleznete v části [vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="hello-predictive-experiment"></a>Prediktivní experiment Hello
Jakmile máte modulu trained model v experimentu školení, klikněte na tlačítko **nastavit webové služby** a vyberte **webové služby prediktivní** v Machine Learning Studio tooinitiate hello proces převodu vaší školení experiment tooa ***prediktivní experiment***. Hello účel prediktivní experiment hello je toouse trained model tooscore nových dat, s cílem hello nakonec stane operationalized jako Azure webové služby.

Tento převod je pro vás provést prostřednictvím hello následující kroky:

* Převést hello sadu moduly používané pro školení do jediného modulu a uložte ho jako modulu trained model.
* Odstranit všechny nadbytečné moduly nesouvisejí tooscoring
* Přidat vstup a výstup porty, které hello případné webové služby bude používat.

Mohou existovat další změny, které chcete toomake tooget vaše prediktivní experiment připravené toodeploy jako webovou službu. Například pokud chcete hello webové služby toooutput pouze podmnožinu výsledků, můžete přidat filtrování modul před hello výstupní port.

V tomto procesu převodu není hello výukový experiment zahodí. Po dokončení procesu hello máte dvě karty Studio: jeden pro hello výukový experiment a jeden pro prediktivní experiment hello. Tímto způsobem můžete provádět změny toohello výukový experiment před nasazením webové služby a znovu sestavte prediktivní experiment hello. Nebo můžete uložit kopii hello školení experimentu toostart další řádek experimenty.

> [!NOTE]
> Když kliknete na tlačítko **webové služby prediktivní** spuštění tooconvert automatický proces prediktivní experimentu školení experimentu tooa, a to funguje dobře ve většině případů. Pokud je komplexní výukový experiment (například máte více cest k trénování, které se můžete připojit společně), pokud dáváte přednost toodo tento převod ručně. Další informace najdete v tématu [jak tooprepare vašeho modelu pro nasazení v nástroji Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).
> 
> 

### <a name="hello-web-service"></a>Hello webové služby
Jakmile budete spokojeni, prediktivní experiment je připraven, můžete nasadit služby jako buď Classic webovou službu nebo nové webové služby založené na Azure Resource Manager. toooperationalize modelu po nasazení jako *Classic Machine Learning webové služby*, klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]**. toodeploy jako *nové Machine Learning webové služby*, klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení [nové] webové služby**. Uživatelé teď můžete odesílat datový model tooyour pomocí hello webové služby REST API a zobrazí zpět hello výsledky. Další informace najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

## <a name="hello-non-typical-case-creating-a-non-predictive-web-service"></a>Hello-obvyklý případ: vytváření-prediktivní webové služby
Pokud není cvičení experimentu model prediktivní analýzy a pak nepotřebujete toocreate výukový experiment a vyhodnocování experimentu – je jedním experiment a můžete nasadit jako webovou službu. Machine Learning Studio zjistí, zda experimentu obsahuje model prediktivní analýzou hello moduly, které byly použity.

Poté, co jste vstupní v experimentu a spokojeni s ním:

1. Klikněte na tlačítko **nastavit webové služby** a vyberte **Retraining webové služby** – vstup a výstup uzly se přidávají automaticky
2. Klikněte na tlačítko **spustit**
3. Klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby** v závislosti na prostředí toowhich hello chcete toodeploy.

Webová služba je nyní nasadit, a můžete používat a spravovat stejně jako prediktivní webové služby.

## <a name="updating-your-web-service"></a>Aktualizace webové služby
Teď, když máte ukázku nasazenou experimentu jako webovou službu, jak postupovat, pokud je třeba tooupdate ho?

To závisí na tom, co potřebujete tooupdate:

**Chcete-li toochange hello vstup nebo výstup, nebo chcete toomodify jak hello webová služba zpracovává data**

Pokud nejsou změna hello modelu, ale pouze upravit jak hello webová služba zpracovává data, můžete upravit hello prediktivní experiment a pak klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby** znovu. Hello webová služba je zastavena, hello aktualizované prediktivní experiment nasazuje a restartování hello webové služby.

Tady je příklad: Předpokládejme, že prediktivní experiment vrátí hello celý řádek vstupních dat s hello předpovědět výsledek. Můžete rozhodnout, že chcete hello webové služby toojust návratový hello výsledek. Proto můžete přidat **sloupce projektu** tooexclude sloupce mimo hello způsobit modulu v experimentu prediktivní hello přímo před hello výstupní port,. Když kliknete na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby** znovu aktualizovat hello webové služby.

**Chcete-li model hello tooretrain se nová data**

Pokud chcete, aby tookeep vaše strojového učení modelu, ale vy byste chtěli tooretrain ho s nová data, máte dvě možnosti:

1. **Přeučování hello modelu, když hello webové služby běží** – Pokud chcete tooretrain modelu spuštěného hello prediktivní webové služby, můžete k tomu tak, že několik úpravy toohello školení experimentu toomake ho *** retraining experimentu***, pak je můžete nasadit jako  ***retraining webové* služby**. Návod, jak toodo, najdete v tématu [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).
2. **Vrátit toohello původní výukový experiment a použít jiný školení data toodevelop modelu** – prediktivní experiment je propojené toohello webové služby, ale hello výukový experiment nejsou přímo spojeny tímto způsobem. Upravíte-li hello původní výukový experiment a klikněte na tlačítko **nastavit webové služby**, vytvoří *nové* prediktivní experiment, který se vytvoří při nasazení, *nové* webové Služba. Právě neaktualizuje hello původní webové služby.
   
   Pokud potřebujete toomodify hello výukový experiment, otevřete ho a klikněte na **uložit jako** toomake kopii. To bude nechte beze změn hello původní výukový experiment prediktivní experiment a webové služby. Nyní můžete vytvořit novou webovou službu s vašimi změnami. Jakmile nasadíte hello novou webovou službu, kterou pak můžete rozhodnout, jestli toostop hello předchozí webové služby, nebo ponechat běh spolu s hello nový.

**Chcete-li tootrain jiného modelu**

Pokud chcete toomake změny tooyour původní prediktivní experiment, jako je výběr jiný algoritmus strojového učení, při různých školení metoda, atd., bude nutné toofollow hello druhý postup popisuje výše pro retraining modelu: Otevřete hello výukový experiment, klikněte na **uložit jako** toomake kopii a pak spusťte dolů hello novou cestu při vývoji modelu, vytváření hello prediktivní experiment a nasazení hello webové služby. Tím se vytvoří nové webové služby, které nejsou toohello původní – můžete rozhodnout, které jednoho nebo obou, tookeep systémem.

## <a name="next-steps"></a>Další kroky
Další podrobnosti o procesu hello vývoj a experimentu najdete v tématu hello následující články:

* převádění hello experimentu - [jak tooprepare vašeho modelu pro nasazení v nástroji Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md)
* nasazení webové služby hello - [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md)
* model retraining hello – [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md)

Příklady hello celého procesu naleznete v tématu:

* [Kurz strojového učení: vytvoření prvního experimentu v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md)
* [Návod: Vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

