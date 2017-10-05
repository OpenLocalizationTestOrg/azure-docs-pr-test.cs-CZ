---
title: "Krok 1: Vytvoření pracovního prostoru Machine Learning | Microsoft Docs"
description: "Krok 1 vývoj prediktivního řešení návod: Zjistěte, jak nastavit nový pracovní prostor Azure Machine Learning Studio."
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
ms.openlocfilehash: 8ca42ef8f5314866301f5c9e93caa90dc837a66e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>Krok 1 průvodce: Vytvoření pracovního prostoru Machine Learning
Toto je první krok tohoto průvodce, [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

1. **Vytvoření pracovního prostoru Machine Learning**
2. [Nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)
3. [Vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Natrénování a vyhodnocení modelů](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)
6. [Přístup k webové službě](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs to be updated to refer to the new way of creating workspaces in the Ibiza portal -->

Pokud chcete používat Machine Learning Studio, potřebujete pracovní prostor Microsoft Azure Machine Learning. Tento pracovní prostor obsahuje nástroje, které potřebujete k vytváření, správu a publikování experimenty.  

<!--
## To create a workspace
1. Sign in to the [Azure classic portal](https://manage.windowsazure.com).
2. In the  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On the **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

Správce pro vaše předplatné Azure je potřeba vytvořit pracovní prostor a pak je přidejte jako roli vlastníka nebo přispěvatele. Podrobnosti najdete v tématu [vytvoření a sdílení pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).

Po vytvoření pracovního prostoru Machine Learning Studio otevřete ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). Pokud máte více než jednoho pracovního prostoru, můžete vybrat pracovním prostoru na panelu nástrojů v pravém horním rohu okna.

![Vyberte pracovní prostor v Studio][2]

> [!TIP]
> Pokud byly provedeny vlastníka pracovního prostoru, můžete sdílet experimenty, kterou právě pracujete na podle vyzvání jiných uživatelů do pracovního prostoru. To provedete v nástroji Machine Learning Studio na **nastavení** stránky. Stačí účtu Microsoft nebo účtu organizace pro každého uživatele.
> 
> Na **nastavení** klikněte na tlačítko **uživatelé**, pak klikněte na tlačítko **POZVAT uživatele více** v dolní části okna.
> 
> 

- - -
**Další krok: [nahrání existujících dat](machine-learning-walkthrough-2-upload-data.md)**

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
