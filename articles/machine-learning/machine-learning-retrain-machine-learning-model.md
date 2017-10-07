---
title: "aaaRetrain Model strojového učení | Microsoft Docs"
description: "Zjistěte, jak tooretrain modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 342bb9954105339b4b634ff20968a64f4f9f750e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-machine-learning-model"></a>Přeučování strojového učení modelu
Jako součást procesu hello operationalization modelů machine learning v Azure Machine Learning je váš model vycvičena a uložit. Pak použijete ho toocreate predicative webové služby. Hello webové služby, pak mohou být využívány v weby, řídicí panely a mobilní aplikace. 

Obvykle nejsou statické modely, které vytvoříte pomocí Machine Learning. Jakmile nová data k dispozici nebo pokud má příjemce hello hello rozhraní API modelu hello svá vlastní data musí toobe retrained. 

Retraining může dojít, často. S funkcí rozhraní API programové Přeučení hello můžete programově přeučit hello model pomocí hello Retraining API a aktualizace hello webové služby s nově trained model hello. 

Tento dokument popisuje hello retraining procesu a ukazuje, jak toouse hello Retraining API.

## <a name="why-retrain-defining-hello-problem"></a>Proč přeučování: definování problému hello
Jako součást hello strojové učení cvičení proces cvičení modelu pomocí datové sadě. Obvykle nejsou statické modely, které vytvoříte pomocí Machine Learning. Jakmile nová data k dispozici nebo pokud má příjemce hello hello rozhraní API modelu hello svá vlastní data musí toobe retrained.

V těchto scénářích poskytuje programovací rozhraní API tooallow pohodlný způsob jste nebo hello příjemce toocreate vaše rozhraní API klienta, který může na základě jednorázové nebo regulární přeučování hello model pomocí svá vlastní data. Potom se můžete vyhodnotit hello výsledky retraining a aktualizovat hello webové rozhraní API toouse hello nově trained model služby.

> [!NOTE]
> Pokud máte existující výukový Experiment a nové webové služby, můžete toocheck out obsloužených existující prediktivní webové služby místo následující návod hello uvedených v následující části hello.
> 
> 

## <a name="end-to-end-workflow"></a>Ucelený pracovní postup
Hello proces zahrnuje hello následující součásti: A výukový Experiment a prediktivní Experiment publikované jako webovou službu. tooenable retraining z modulu trained model, hello výukový Experiment musí být publikován jako webovou službu s hello výstup modulu trained model. To umožňuje model toohello přístupu rozhraní API pro přeučení. 

Hello postupem použít tooboth nové a Classic webové služby:

Vytvoření hello počáteční prediktivní webové služby:

* Vytvoření experimentu školení
* Vytvoření experimentu prediktivní web
* Nasadit prediktivní webové služby

Přeučování hello webové služby:

* Aktualizovat tooallow experimentu školení pro retraining
* Nasazení hello retraining webové služby
* Použít hello služba Batch provádění kódu tooretrain hello model

Návod hello předchozích kroků najdete v tématu [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).

> [!NOTE] 
> toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby. Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md). 

Pokud jste nasadili Classic webové služby:

* Vytvořte nový koncový bod na hello prediktivní webové služby
* Získat adresu URL hello opravy a kódu
* Použití hello oprava URL toopoint hello nový koncový bod v hello retrained modelu 

Návod hello předchozích kroků najdete v tématu [Přeučování Classic webové služby](machine-learning-retrain-a-classic-web-service.md).

Pokud narazíte na problémy retraining Classic webové služby, přečtěte si téma [řešení potíží s hello retraining Azure Machine Learning Classic webové služby](machine-learning-troubleshooting-retraining-models.md).

Pokud jste nasadili do nové webové služby:

* Přihlaste se tooyour účet Azure Resource Manager
* Získat definice hello webové služby
* Exportovat hello definice webové služby jako JSON
* Aktualizovat hello odkaz toohello `ilearner` objektů blob v hello JSON
* Importovat hello JSON do definice webové služby
* Aktualizovat hello webové služby s novou definice webové služby

Návod hello předchozích kroků najdete v tématu [Přeučování nové webové službě pomocí rutiny prostředí PowerShell správu Machine Learning hello](machine-learning-retrain-new-web-service-using-powershell.md).

proces Hello nastavení retraining Classic webové služby zahrnuje hello následující kroky:

![Retraining přehled procesu][1]

proces Hello nastavení pro novou webovou službu retraining zahrnuje hello následující kroky:

![Retraining přehled procesu][7]

## <a name="other-resources"></a>Další prostředky
* [Modely retraining a aktualizaci Azure Machine Learning s Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Vytvořit mnoho modely Machine Learning a webové koncové body služby z jednoho experimentu pomocí prostředí PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
* Hello [AML Přeučení modelů pomocí rozhraní API](https://www.youtube.com/watch?v=wwjglA8xllg) video ukazuje, jak vytvořit tooretrain modely Machine Learning v Azure Machine Learning pomocí hello Retraining API a prostředí PowerShell.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

