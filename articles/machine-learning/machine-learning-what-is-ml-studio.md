---
title: aaaWhat je Azure Machine Learning Studio? | Dokumentace Microsoftu
description: "Přehled nástroje Azure ML Studio, který umožňuje přetahováním rychle vytvářet modely z předpřipravených knihoven algoritmů a modulů"
keywords: azure machine learning,azure ml,ml studio
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: a25f2d9e75d088a37930de98240c6e14c9567fa4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-machine-learning-studio"></a>Co je Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio je nástroj spolupráci, přetahování myší, můžete použít toobuild, testovat a nasazovat řešení prediktivní analýzy na vaše data. Machine Learning Studio publikuje modely jako webové služby, které je možné snadno využívat ve vlastních aplikacích nebo nástrojích BI, například v Excelu.

Machine Learning Studio je místo, kde se setkává datová věda, prediktivní analýza, cloudové prostředky a vaše data.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-machine-learning-studio-interactive-workspace"></a>interaktivní pracovní prostor Machine Learning Studio Hello
toodevelop model prediktivní analýzy zpravidla použijete data z jednoho nebo více zdrojů, transformace a analyzovat data prostřednictvím různých statistických funkcí a manipulaci s daty a vygenerujete sadu výsledků. Vývoj takového modelu je iterativní proces. Úpravou hello různých funkcí a jejich parametrů, výsledky zpřesňují, dokud nebudete přesvědčeni, že máte natrénován efektivní model.

**Azure Machine Learning Studio** poskytuje tooeasily interaktivní vizuální pracovní prostor vytvoříte, testovat a iterovat model prediktivní analýzy. Můžete přetahování myší ***datové sady*** a analýzy ***moduly*** na interaktivní plátno, jejich připojením společně tooform ***experimentovat***, které spustíte v Machine Learning Studio. tooiterate návrhu modelu hello experiment, uložení kopie v případě potřeby upravit a potom ho spusťte znovu. Až budete připraveni, můžete převést vaše ***výukový experiment*** tooa ***prediktivní experiment***a potom ho jako publikovat ***webovou službu*** tak, aby váš model přístupná pomocí ostatní.

Neexistuje žádné programování potřeby vizuálním propojováním datových sad a modulů tooconstruct model prediktivní analýzy.

> [!TIP]
> toodownload a tisk diagram, který bude stručně charakterizovat hello možností nástroje Machine Learning Studio, najdete v [diagram s přehledem funkcí Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).
> 
> 

![Diagram nástroje Azure ML Studio: Vytváření experimentů, čtení dat z mnoha zdrojů, zápis dat se stanoveným skóre, zápis modelů][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Začínáme s nástrojem Machine Learning Studio
Když poprvé vstoupíte [Machine Learning Studio](https://studio.azureml.net) zobrazí hello **Domů** stránky. Zde si můžete zobrazit dokumentaci, videa a webináře a najdete tu i další přínosné materiály.

Klikněte na levý horní nabídce hello ![Nabídka](media/machine-learning-what-is-ml-studio/menu.png) a zobrazí se několik možností.

### <a name="cortana-intelligence"></a>Cortana Intelligence
Klikněte na tlačítko **Cortana Intelligence** a přejdete toohello domovské stránce hello [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite). Hello Cortana Intelligence Suite je plně spravovaná velkých objemů dat a rozšířené analytics suite tootransform data do inteligentního akce. V tématu hello Suite domovskou stránku pro úplnou dokumentaci, včetně scénářů zákazníka.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Existují dvě možnosti zde **Domů**, kde jste spustili, stránku hello a **Studio**.

Klikněte na tlačítko **Studio** a přejdete toohello **Azure Machine Learning Studio**. Zobrazí se první dotaz toosign pomocí účtu Microsoft nebo pracovní nebo školní účet. Po přihlášení se zobrazí hello na levé straně hello následující karty:

* **PROJEKTY** – Kolekce experimentů, datových sad, poznámkových bloků a jiných prostředků představující jeden objekt
* **EXPERIMENTY** – Experimenty, které jste vytvořili a spustili nebo uložili jako koncepty
* **WEBOVÉ SLUŽBY** – Webové služby, které jste nasadili z experimentů
* **POZNÁMKOVÉ BLOKY** – Jupyter Notebooks, které jste vytvořili
* **DATOVÉ SADY** – Datové sady, které jste nahráli do nástroje Studio
* **TRÉNOVANÉ MODELY** – Modely, které jste v experimentech natrénovali a uložili je v nástroji Studio
* **NASTAVENÍ** – kolekce nastavení, které můžete tooconfigure účtu a prostředků.

### <a name="gallery"></a>Galerie
Klikněte na tlačítko **Galerie** a přejdete toohello  **[Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)**. Hello Galerie je místo, kde komunita datových vědců a vývojářů sdílet řešení vytvořená pomocí komponent sady Cortana Intelligence Suite hello.

Další informace o hello Galerie najdete v tématu [sdílené složky a hledání řešení hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Komponenty experimentu
Experiment sestává z datových sad, které poskytují data tooanalytical moduly, které lze vzájemně propojit tooconstruct model prediktivní analýzy. Platný experiment má konkrétně tyto charakteristiky:

* Hello experiment obsahuje alespoň jednu datovou sadu a jeden modul.
* Datové sady může být připojený jenom toomodules
* Moduly může být připojené tooeither datové sady nebo dalších modulů
* Musí mít všechny vstupní porty pro moduly některá data toohello připojení toku
* Musí být nastaveny všechny povinné parametry jednotlivých modulů.

Experiment můžete vytvořit zcela od začátku, ale také můžete využít existující ukázku jako šablonu. Další informace najdete v tématu [kopie příklad experimenty toocreate nové strojovým učením](machine-learning-sample-experiments.md).

Příklad vytvoření jednoduchého experimentu najdete v tématu [Vytvoření jednoduchého experimentu v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md).

Obsáhlejší návod, jak vytvořit řešení prediktivní analýzy, najdete v tématu o [vývoji prediktivního řešení s Azure Machine Learningem](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Datové sady
Datové sady je, že data, která jsou načtený tooMachine Learning Studio, takže je možné v procesu modelování hello. Několik ukázkových datových sad, které jsou součástí Machine Learning Studio pro tooexperiment s a můžete nahrávat další datové sady je podle potřeby. Zde jsou některé příklady dodávaných datových sad:

* **Data o spotřebě paliv u různých automobilů** – Hodnoty spotřeby paliva u automobilů na jednotku vzdálenosti (MPG) stanovené podle počtu válců, koňských sil apod.
* **Data o rakovině prsu** – Diagnostická data rakoviny prsu
* **Data o lesních požárech** – Rozsahy lesních požárů v severovýchodním Portugalsku

Během vytváření experimentu je možné z hello seznam datových sad, které jsou k dispozici, že toohello levé hello plátno.

Seznam ukázkových datových sad v nástroji Machine Learning Studio najdete v tématu [použít hello ukázkových datových sad v nástroji Azure Machine Learning Studio](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Moduly
Modul je algoritmus, který je možné provést na datech. Machine Learning Studio obsahuje několik modulů od data příchozího funkce tootraining, stanovení skóre a ověřování procesy. Zde jsou některé příklady dodávaných modulů:

* [Převést tooARFF] [ convert-to-arff] – formát souboru převede .NET serializovanou datovou sadu tooAttribute-Relation (ARFF).
* [Výpočet základních statistik][elementary-statistics] – Vypočítá základní statistiky, jako je střední hodnota, směrodatná odchylka atd.
* [Lineární regrese][linear-regression] – Vytvoří online model lineární regrese na základě klesání gradientu.
* [Určení skóre modelu][score-model] – Stanoví skóre pro trénovaný klasifikační nebo regresní model.

Během vytváření experimentu je možné hello seznamu modulů, které jsou k dispozici, že toohello levé hello plátno.  

Modul může obsahovat sadu parametrů, které můžete použít modul hello tooconfigure vnitřní algoritmy. Když vyberete na modul na plátno hello, parametry modulu hello se zobrazují v hello **vlastnosti** toohello podokně napravo od plátna hello. Můžete změnit parametry hello v tomto podokně tootune modelu.

Některé nápovědy procházení hello velké knihovny algoritmů strojového učení dostupné, najdete v části [jak toochoose algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Nasazení webové služby prediktivní analýzy
Jakmile je váš model prediktivní analýzy připraven, můžete jej přímo v nástroji Machine Learning Studio nasadit jako webovou službu. Další podrobnosti k tomuto procesu najdete v tématu [Nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
