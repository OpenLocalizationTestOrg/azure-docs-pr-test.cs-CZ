---
title: "Krok 1: Vytvoření pracovního prostoru Machine Learning | Microsoft Docs"
description: "Krok 1 hello vývoj prediktivního řešení návod: Zjistěte, jak tooset si nový pracovní prostor Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 93d2e240826db9768e85b00cab0eb62510b4efb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>Krok 1 průvodce: Vytvoření pracovního prostoru Machine Learning
Toto je první krok hello hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

1. **Vytvoření pracovního prostoru Machine Learning**
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. [Vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů hello](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby hello](machine-learning-walkthrough-5-publish-web-service.md)
6. [Přístup k webové službě hello](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs toobe updated toorefer toohello new way of creating workspaces in hello Ibiza portal -->

toouse Machine Learning Studio, musíte toohave prostoru Microsoft Azure Machine Learning. Tento pracovní prostor obsahuje hello nástroje, které potřebujete toocreate, spravovat a publikovat experimenty.  

<!--
## toocreate a workspace
1. Sign in toohello [Azure classic portal](https://manage.windowsazure.com).
2. In hello  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On hello **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

Hello správce pro vašeho předplatného Azure potřebuje toocreate hello prostoru a potom je přidat jako roli vlastníka nebo přispěvatele. Podrobnosti najdete v tématu [vytvoření a sdílení pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).

Po vytvoření pracovního prostoru Machine Learning Studio otevřete ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). Pokud máte více než jednoho pracovního prostoru, můžete vybrat hello prostoru v panelu nástrojů hello v pravém horním rohu hello okna hello.

![Vyberte pracovní prostor v Studio][2]

> [!TIP]
> Pokud byly provedeny vlastníka hello pracovního prostoru, můžete sdílet hello experimenty pracujete pomocí vyzvání jiných uživatelů toohello prostoru. To provedete v nástroji Machine Learning Studio na hello **nastavení** stránky. Stačí hello účtu Microsoft nebo účtu organizace pro každého uživatele.
> 
> Na hello **nastavení** klikněte na tlačítko **uživatelé**, pak klikněte na tlačítko **POZVAT uživatele více** v hello dolní části okna hello.
> 
> 

- - -
**Další krok: [nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)**

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
