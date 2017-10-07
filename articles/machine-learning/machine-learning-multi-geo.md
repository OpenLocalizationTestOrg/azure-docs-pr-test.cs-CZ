---
title: "Geograficky aaaMulti nápovědě | Microsoft Docs"
description: "Zjistěte, jak toocreate pracovního prostoru a publikování webové služby v oblasti Azure, liší od hello Jižní střední USA (SCUS) oblasti Azure."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: 77b055950ebfe329131b40e5f0a2f6be1e33c51e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multi-geo-help-documentation"></a>Dokumentace nápovědy k více geografickým oblastem
Tento článek popisuje, jak můžete vytvořit pracovní prostor a publikovat webovou službu v různých oblastech Azure.  Hello [produkty Azure podle oblasti stránky](https://azure.microsoft.com/en-us/regions/services/) obsahuje seznam oblastí, kde je k dispozici Azure Machine Learning.

## <a name="create-a-workspace"></a>Vytvoření pracovního prostoru
1. Přihlaste se toohello portálu Azure Classic.
2. Klikněte na tlačítko **+ nový** > **datové služby** > **MACHINE LEARNING** > **rychle vytvořit**.  V části **umístění** vybrat jinou oblast, například **jihovýchodní Asie**.
   ![Více geograficky pomůže obrázek 1][1]
3. Vyberte pracovní prostor hello a pak klikněte na **přihlášení tooML Studio**.
   ![Více geograficky pomůže obrázek 2][2]
4. Nyní máte pracovního prostoru v jiné oblasti, které můžete používat stejně jako ostatní pracovního prostoru. tooswitch mezi pracovní prostory vzhled toohello pravé horní části obrazovky. Klikněte na rozevírací hello, vyberte hello oblast a pak vyberte hello prostoru. Všechno, co je místní toohello oblast pracovního prostoru.  Například všechny vaše webové služby vytvořené z jiného pracovního prostoru bude ve stejné oblasti hello prostoru se nachází v hello.
   ![Více geograficky pomůže image 3][3]

## <a name="open-an-experiment-from-gallery"></a>Mohli experiment otevřít z Galerie
Pokud jste mohli experiment otevřít z galerie, můžete také vybrat, které oblasti chcete toocopy hello experiment.

![Více geograficky pomůže obrázek 4][4a]

## <a name="web-service-management"></a>Správa webové služby
tooprogrammatically Správa webové služby, například pro retraining, použít hello oblast adresu:

* https://asiasoutheast.Management.azureml.NET
* https://europewest.Management.azureml.NET

### <a name="things-toonote"></a>Toonote věcí
1. Kopírovat můžete pouze experimenty mezi pracovní prostory, které patří toohello stejné oblasti tímto způsobem. Pokud potřebujete toocopy experimentu napříč pracovní prostory v různých oblastech, můžete použít hello [prostředí PowerShell](http://aka.ms/amlps) příkaz [ *kopie AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish který. Další alternativní řešení je toopublish hello experimentu do Galerie v seznamu neuvedené režimu a potom ho otevřete v pracovním prostoru hello z hello jiné oblasti.
2. selektor oblast Hello zobrazí pouze pracovní prostory pro jednu oblast v čase.  
3. Vytváření a hostované ve Jižní střední USA volného prostoru nebo prostoru přístup hosta (anonymní)  
4. Webové služby z pracovního prostoru v jihovýchodní Asie nasazena se hostují taky v jihovýchodní Asie.  

## <a name="more-information"></a>Další informace
Zeptejte se na hello [fórum Azure Machine Learning](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
