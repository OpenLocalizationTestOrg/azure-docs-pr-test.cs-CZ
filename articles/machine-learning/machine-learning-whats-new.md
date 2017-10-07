---
title: "aaaWhat je nového v Azure Machine Learning | Microsoft Docs"
description: "Nové funkce, které jsou k dispozici v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 6532aabcff023d6c81f79f21d501335c234c55fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-azure-machine-learning"></a>Novinky ve službě Azure Machine Learning

### <a name="hello-march-2017-release-of-microsoft-azure-machine-learning-updates-provides-hello-following-feature"></a>Hello března 2017 verzi aktualizace Microsoft Azure Machine Learning poskytuje hello následující funkce:



* Vyhrazené kapacity pro Azure Machine Learning BES úlohy

    Zpracování Machine Learning fondu Batch používá hello [Azure Batch](../batch/batch-technical-overview.md) služby tooprovide spravované zákazníkem škálování pro hello Azure Machine Learning dávky spuštění služby. Zpracování fondu batch vám umožní toocreate fondech Azure Batch, na kterých můžete odesílat dávkové úlohy a jejich spuštění předvídatelný způsobem.

    Další informace najdete v tématu [služby Azure Batch pro Machine Learning úlohy](machine-learning-dedicated-capacity-for-bes-jobs.md).


### <a name="hello-august-2016-release-of-microsoft-azure-machine-learning-updates-provide-hello-following-features"></a>Hello srpna 2016 verze služby Microsoft Azure Machine Learning aktualizace poskytují hello následující funkce:
* Classic webové služby se teď dají spravovat hello nové [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/) portál, který poskytuje jeden místní toomanage všechny aspekty webové služby.    
  * Které poskytuje webovou službu [Statistika využití](machine-learning-manage-new-webservice.md).
  * Zjednodušuje testování pomocí ukázkových dat volání Azure Machine Learning vzdáleného požadavku.
  * Poskytuje nové zkušební stránku služby Batch spuštění ukázkových dat a úlohy odeslání historie.
  * Nabízí jednodušší správu koncový bod.

### <a name="hello-july-2016-release-of-microsoft-azure-machine-learning-updates-provide-hello-following-features"></a>Hello července 2016 verze služby Microsoft Azure Machine Learning aktualizace poskytují hello následující funkce:
* Webové služby se nyní spravují jako prostředky Azure, které jsou spravované prostřednictvím [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) rozhraní, aby vám umožnil hello následující vylepšení:
  * Existují nové [rozhraní REST API](https://msdn.microsoft.com/library/azure/Dn950030.aspx) toodeploy a spravovat váš správce prostředků na základě webové služby.
  * Nová [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/) portál, který poskytuje jeden místní toomanage všechny aspekty webové služby.
* Zahrnuje nový model nasazení založené na předplatném, oblasti služby webové služby pomocí Správce prostředků na základě využití hello poskytovatele prostředků Resource Manageru pro webové služby, rozhraní API.
* Zavádí nové [cenových plánů](https://azure.microsoft.com/pricing/details/machine-learning/) a plán pomocí možnosti správy hello nové RP správce prostředků pro fakturaci.
  * Teď můžete [nasazení vaší webové služby toomultiple oblasti](machine-learning-how-to-deploy-to-multiple-regions.md) bez nutnosti toocreate předplatné v každé oblasti.
* Poskytuje webovou službu [Statistika využití](machine-learning-manage-new-webservice.md).
* Zjednodušuje testování pomocí ukázkových dat volání Azure Machine Learning vzdáleného požadavku.
* Poskytuje nové zkušební stránku služby Batch spuštění ukázkových dat a úlohy odeslání historie.

Kromě toho hello Machine Learning Studio byl aktualizovaný tooallow jste toodeploy toohello novou webovou službu modelu nebo pokračovat toodeploy toohello klasického webové služby modelu. 

