---
title: "aaaA prediktivní řešení pro úvěrové riziko v Machine Learning | Microsoft Docs"
description: "Podrobný průvodce jak toocreate řešení prediktivní analýzy pro platební hodnocení rizik v Azure Machine Learning Studio."
keywords: "úvěrové riziko,řešení prediktivní analýzy,posouzení rizika"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 00ed39081e6952b8d03b37a8285d8dc3ddff2cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Názorný průvodce: Vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning

V tomto návodu budeme prohlédněte rozšířené hello proces vývoj řešení prediktivní analýzy v nástroji Machine Learning Studio. Jsme vývoj jednoduchého modelu v nástroji Machine Learning Studio a nasaďte ho jako webové služby Azure Machine Learning, kde může hello modelu provádět předpovědi pomocí nová data. 

Tento názorný průvodce předpokládá, že jste nejméně jednou předtím použili Machine Learning Studio a že rozumíte konceptům strojového učení. Bere ale v úvahu, že nejste odborníkem ani na jedno.

Pokud jste použili nikdy **Azure Machine Learning Studio** před můžete chtít toostart s hello kurzu [vytvořit první vědecké zpracování dat experiment v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md). Tento kurz vás provede Machine Learning Studio pro hello poprvé. Zobrazuje hello základní informace o tom, jak toodrag myší modulů do experimentu, připojte je společně, spustit hello experiment a podívejte se na výsledky hello. Jiný nástroj, který může být užitečné při zahájení práce je diagram, který bude stručně charakterizovat hello možností nástroje Machine Learning Studio. Stáhnout a vytisknout si ho můžete odtud: [Diagram s přehledem možností nástroje Machine Learning Studio](machine-learning-studio-overview-diagram.md).
 
Pokud jste nové pole toohello strojového učení obecně, je série videí, který může být užitečné tooyou. Je volána [vědecké zpracování dat pro začátečníky](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) a ho můžete udělit pomocí jazyka pro každý den a koncepty výukový toomachine Skvělý úvod.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="hello-problem"></a>problém Hello

Předpokládejme, že potřebujete toopredict jednotlivého uživatele úvěrové riziko podle hello informace, které se přiřadil v o úvěr.  

Posouzení úvěrového rizika je komplexní problém, ale pro účely tohoto názorného průvodce jej můžeme zjednodušit. Použijeme jej jako příklad způsobu, jakým můžete vytvořit řešení prediktivní analýzy pomocí služby Microsoft Azure Machine Learning. toodo toho používáme Azure Machine Learning Studio a webové službě Machine Learning.  

## <a name="hello-solution"></a>řešení Hello

V tomto podrobném průvodci začneme s veřejně dostupnými daty o úvěrovém riziku, na jejichž základě vyvineme a natrénujeme prediktivní model. Pak můžeme hello model nasadit jako webovou službu, takže ho můžete použít třetími stranami pro posuzování úvěrového rizika.

toocreate toto řešení hodnocení rizik platební jsme postupujte podle těchto kroků:  

1. [Vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. [Vytvoření experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů hello](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby hello](machine-learning-walkthrough-5-publish-web-service.md)
6. [Přístup hello webové služby](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> Pracovní kopie hello experimentu, který jsme vyvíjet můžete najít v tomto návodu v hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com). Přejděte příliš**[návod - úvěrové riziko předpovědi](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**  a klikněte na tlačítko **Open in Studio** toodownload kopii hello experimentu do pracovního prostoru Machine Learning Studio.
> 
> Tento názorný postup je založen na zjednodušené verzi ukázkového experimentu hello, [binární klasifikace: predikce úvěrového rizika](http://go.microsoft.com/fwlink/?LinkID=525270), je dostupný také ve hello [Galerie](http://gallery.cortanaintelligence.com/).
