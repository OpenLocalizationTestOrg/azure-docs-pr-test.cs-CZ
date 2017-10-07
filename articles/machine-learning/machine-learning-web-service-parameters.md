---
title: "aaaUse parametry webové služby Azure Machine Learning | Microsoft Docs"
description: "Jak toouse parametry webové služby Azure Machine Learning toomodify hello chování modelu při přístupu k hello webové služby."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: raymondl;garye
ms.openlocfilehash: 214711eb819a6cea34db905abdf015da11e846d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a>Použití parametrů webové služby Azure Machine Learning
Webové služby Azure Machine Learning je vytvořen publikování experimentu, který obsahuje moduly, které se dají konfigurovat parametry. V některých případech můžete toochange hello modulu chování při spuštění hello webové služby. *Parametry webové služby* umožňují toodo této úlohy. 

Běžným příkladem je nastavení hello [importovat Data] [ reader] modulu tak daného uživatele a hello hello publikované webové služby můžete zadat jiný zdroj dat, při přístupu k hello webové služby. Nebo konfigurace hello [Export dat] [ writer] modulu tak, aby lze zadat jiný cíl. Mezi další příklady patří změna hello počet bitů pro hello [Hashování] [ feature-hashing] modulu nebo hello počet požadovaných funkcí pro hello [na základě filtru výběr funkce] [ filter-based-feature-selection] modulu. 

Můžete nastavit parametry webové služby a je přidružit jeden nebo více parametrů modulu v experimentu a můžete určit, jestli jsou požadované nebo volitelné. uživatel Hello hello webové služby můžete poté zadejte hodnoty pro tyto parametry při volání webové služby hello. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-tooset-and-use-web-service-parameters"></a>Jak tooset a použijte parametry webové služby
Parametr webové služby můžete definovat na hello ikonu další toohello parametru pro modul a výběrem "Nastavit jako parametr webové služby". Tím se vytvoří nový parametr webové služby a připojí ho toothat modulu parametr. Potom při přístupu k webové službě hello hello uživatele můžete zadat hodnotu pro hello parametr webové služby a je použité toohello modulu parametr.

Jakmile definujete parametr webové služby, je k dispozici tooany druhý parametr modulu v experimentu hello. Pokud definujete webové služby parametr spojený s parametrem pro jeden modul, můžete použít tento stejný parametr webové služby pro ostatní moduly, tak dlouho, dokud hello parametr očekává hello stejný typ hodnoty. Například pokud hello parametr webové služby je číselná hodnota, pak lze být použít pouze pro parametry modulu, které očekávají číselná hodnota. Když uživatel hello nastaví hodnotu pro hello parametr webové služby, bude použité tooall přidružené parametry modulu.

Můžete rozhodnout, zda tooprovide výchozí hodnoty pro hello parametr webové služby. Pokud provedete, pak hello je parametr volitelný pro uživatele hello hello webové služby. Pokud nechcete zadat výchozí hodnotu, pak uživatel hello je požadované tooenter hodnotu, při přístupu k hello webové služby.

Hello dokumentaci k rozhraní API pro webovou službu hello obsahuje informace pro uživatele hello webové služby na tom, jak toospecify hello parametr webové služby prostřednictvím kódu programu při přístupu k hello webové služby.

> [!NOTE]
> Hello dokumentaci k rozhraní API pro classic webové služby je poskytována prostřednictvím hello **stránku nápovědy rozhraní API** odkaz v hello webové služby **řídicí panel** v nástroji Machine Learning Studio. Hello dokumentaci k rozhraní API pro novou webovou službu je poskytována prostřednictvím hello [webové služby Azure Machine Learning](https://services.azureml.net/Quickstart) portál hello **spotřebě** a **rozhraní API Swaggeru** stránky pro vaše webové služby.
> 
> 

## <a name="example"></a>Příklad
Jako příklad předpokládejme máme experimentu s [Export dat] [ writer] modul, který odešle úložiště objektů blob tooAzure informace. Budeme definovat parametr webové služby s názvem "Blob cestu", který umožňuje hello webové služby uživatele toochange hello cesta toohello úložiště objektů blob při přístupu služby hello.

1. V nástroji Machine Learning Studio, klikněte na tlačítko hello [Export dat] [ writer] modulu tooselect ho. Jeho vlastnosti jsou uvedené v hello vlastnosti podokně toohello napravo od plátna experimentu hello.
2. Zadejte typ úložiště hello:
   
   * V části **zadejte cíl dat**, vyberte "Azure Blob Storage".
   * V části **zadejte typ ověřování**, vyberte "Účet".
   * Zadejte informace o účtu hello pro hello úložiště objektů blob Azure. 
     <p />
3.Klikněte na ikonu toohello hello napravo od hello **cesta tooblob počínaje kontejneru parametr**. Vypadá takto:
   
   ![Ikona webové služby parametr][icon]
   
   Vyberte "Nastavit jako parametr webové služby".
   
   Položka se přidají pod **parametry webové služby** dole hello v podokně Vlastnosti hello s hello název "cesta tooblob začíná kontejneru". Toto je hello parametr webové služby, který je teď přidružený k tomuto [Export dat] [ writer] parametr modulu.
4. toorename hello parametr webové služby, klikněte na název hello, zadejte "Blob cestu" a stiskněte klávesu hello **Enter** klíč. 
5. tooprovide výchozí hodnota pro hello parametr webové služby, klikněte na ikonu toohello hello napravo od hello název, vyberte "Zadejte výchozí hodnotu", zadejte hodnotu (například "container1/output1.csv") a stiskněte klávesu hello **Enter** klíč.
   
   ![Parametr webové služby][parameter]
6. Klikněte na **Run** (Spustit). 
7. Klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby** toodeploy hello webové služby.

> [!NOTE] 
> toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby. Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md). 

uživatel Hello hello webové služby teď můžete zadat do nového cíle pro hello [Export dat] [ writer] modulu při přístupu k hello webové služby.

## <a name="more-information"></a>Další informace
Více podrobný příklad najdete v tématu hello [parametry webové služby](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) položku v hello [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Další informace o přístupu k webové službě Machine Learning najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

