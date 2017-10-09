---
title: "aaaCreate pracovní prostor Machine Learning | Microsoft Docs"
description: "Jak toocreate pracovního prostoru pro Azure Machine Learning Studio"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
ms.openlocfilehash: 178293af222365993fade666124f34269d892325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Vytvoření a sdílení pracovního prostoru Azure Machine Learning
Tato nabídka propojí tootopics, které popisují, jak tooset až hello různé vědě prostředí datových používají v hello Cortana Analytics procesu (CAP).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

toouse Azure Machine Learning Studio, musíte toohave pracovní prostor Machine Learning. Tento pracovní prostor obsahuje hello nástroje, které potřebujete toocreate, spravovat a publikovat experimenty.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="toocreate-a-workspace"></a>toocreate pracovního prostoru
1. Přihlaste se toohello [portálu Azure](https://portal.azure.com/)

    > [!NOTE]
    > toosign v a vytvořit pracovní prostor, musíte toobe správce předplatného Azure. 
    >
    > 

2. Klikněte na tlačítko **+ nové**

3. Vyberte **Intelligence + analýzy**, klikněte na tlačítko **pracovní prostor Machine Learning**, pak klikněte na tlačítko **vytvořit**

4. Zadejte informace o pracovním prostoru

    - Hello *název pracovního prostoru* může být až too260 znaky, není končí v prostoru. Hello název nesmí obsahovat tyto znaky:`< > * % & : \ ? + /`
    - Hello *plán webové služby* vyberete (nebo vytvořte), spolu s hello přidružené *cenová úroveň* můžete vybrat, se používá při nasazování webových služeb z tohoto pracovního prostoru.

    ![Vytvořit nový pracovní prostor](media/machine-learning-create-workspace/create-new-workspace.png)

5. Klikněte na **Vytvořit**

Po nasazení hello prostoru můžete ho otevřít v nástroji Machine Learning Studio.

1. Procházet tooMachine Learning Studio na [https://studio.azureml.net/](https://studio.azureml.net/).

2. Vyberte pracovní prostor v pravém horním rohu hello.

    ![Vyberte pracovní prostor](media/machine-learning-create-workspace/open-workspace.png)

3. Klikněte na tlačítko **Moje experimenty**.

    ![Otevřete experimenty](media/machine-learning-create-workspace/my-experiments.png)

Informace o správě pracovního prostoru najdete v tématu [spravovat pracovní prostor služby Azure Machine Learning](machine-learning-manage-workspace.md).
Pokud narazíte na potíže při vytváření pracovního prostoru, přečtěte si téma [Průvodce odstraňováním potíží: Vytvořte a připojte pracovní prostor Machine Learning tooa](machine-learning-troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Sdílení pracovní prostor služby Azure Machine Learning
Po vytvoření pracovního prostoru Machine Learning můžete pozvat uživatele tooyour prostoru tooshare přístup tooyour prostoru a všechny jeho experimenty, datové sady, poznámkových bloků, atd. Můžete přidat uživatele v jednom ze dvou rolí:

* **Uživatel** -prostoru uživatel může vytvořit, otevřít, upravit a odstranit experimenty, datové sady atd., v pracovním prostoru hello.
* **Vlastník** – vlastníka můžete pozvat a odebírání uživatelů v prostoru hello v toowhat přidání uživatele můžete provést.

> [!NOTE]
> Hello účet správce, který vytvoří hello prostoru se automaticky přidá toohello prostoru jako pracovní prostor vlastníka. Ale jiní správci nebo uživatelé v tomto předplatném nejsou automaticky udělen přístup toohello prostoru – potřebovat tooinvite je explicitně.
> 
> 

### <a name="tooshare-a-workspace"></a>tooshare pracovního prostoru

1. Přihlaste se tooMachine Learning Studio zadává [https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. V levém panelu hello, klikněte na tlačítko **nastavení**

3. Klikněte na tlačítko hello **uživatelé** karta

4. Klikněte na tlačítko **POZVAT uživatele více** na hello dolní části stránky hello

    ![Nastavení Studio](media/machine-learning-create-workspace/settings.png)

5. Zadejte jeden nebo více e-mailové adresy. Uživatelé Hello, potřebujete účet Microsoft nebo účet organizace (z Azure Active Directory).

6. Vyberte, zda chcete, aby uživatelé hello tooadd jako vlastníka nebo uživatele.

7. Klikněte na tlačítko hello **OK** značku zaškrtnutí.

Každý uživatel, které přidáte obdrží e-mail s pokyny, jak sdílet toosign v toohello prostoru.

> [!NOTE]
> Pro možnost toodeploy toobe uživatelé nebo spravovat webových služeb v tomto pracovním prostoru, musí to být Přispěvatel nebo správce v hello předplatného Azure. 



