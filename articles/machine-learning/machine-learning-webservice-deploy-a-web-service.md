---
title: "aaaDeploying novou webovou službu v Azure Machine Learning | Microsoft Docs"
description: "Hello pracovní postup nasazení ARM na základě webové služby"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: a358b04f-0d08-4d50-820e-eeac971854cf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: v-donglo
ROBOTS: NOINDEX
redirect_url: machine-learning-publish-a-machine-learning-web-service
redirect_document_id: True
ms.openlocfilehash: 2cbfda44b97a6b992fbdfdfb0c761e6c9e169035
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-new-web-service"></a>Nasazení nové webové služby
Microsoft Azure Machine learning teď poskytuje webové služby, které jsou založeny na [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) povolení pro nové fakturace možnosti plánování a nasazení vaší webové služby toomultiple oblasti.

Obecný postup toodeploy Hello webové služby pomocí nástroje Microsoft Azure Machine Learning webové služby je:

* Vytvořit prediktivní experiment
* nasazení
* Konfigurace názvu
* plán fakturace
* otestovat
* ho využívají.

Hello následující obrázek ukazuje pracovní postup nasazení hello.

![Pracovní postup nasazení webové služby][1]

## <a name="deploy-web-service-from-studio"></a>Nasazení webové služby ze sady Studio
toodeploy experimentu jako novou webovou službu. Přihlaste se k hello Machine Learning Studio a vytvořte novou prediktivní webovou službu. 

**Poznámka:**: Pokud už mají nasazenou experimentu jako classic webovou službu, nelze ho nasadit jako novou webovou službu.

Klikněte na tlačítko **spustit** dole hello hello experimentovat plátno a potom klikněte na **nasazení webové služby** a **nasazení [nové] webové služby**. Otevře se stránka nasazení Hello hello webová služba Machine Learning správce.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Machine Learning webové služby Manager nasadit stránce experimentu
Na stránce experimentu nasazení hello zadejte název pro hello webovou službu.
Vyberte cenový plán. Pokud máte existující cenový plán, že můžete ji vybrat, v opačném případě musíte vytvořit nový plán ceny služby hello. 

1. V hello **cena plán** rozevírací nabídky vyberte existující plán nebo hello **vyberte nový plán** možnost.
2. V **název plánu**, zadejte název, který bude identifikovat hello plán ve vašem vyúčtování.
3. Vyberte jednu z hello **měsíční plánování vrstev**. Upozorňujeme, že úrovně plánu hello výchozí toohello plány pro vaši oblast výchozí a webové služby je nasazené toothat oblast.

Klikněte na tlačítko **nasadit** a otevře se stránka hello rychlý start pro webovou službu.

## <a name="quickstart-page"></a>Stránku rychlý start
Rychlý start stránku Hello webové služby obsahuje přístup a pokyny o hello nejčastějších úloh, které provedete po vytvoření nové webové služby. Tady jsou snadno přístupné oba hello **Test** stránky a **spotřebě** stránky.

## <a name="testing-your-web-service"></a>Testování webové služby
Na stránce Rychlý start hello klikněte na tlačítko Test webové služby v rámci běžné úlohy.   

Webová služba hello tootest jako požadavků a odpovědí služby (RR):

* Klikněte na tlačítko **Test** v řádku nabídek hello.
* Klikněte na tlačítko **požadavků a odpovědí**.
* Zadejte odpovídající hodnoty pro vstupní sloupce hello experimentu.
* Klikněte na tlačítko Test **požadavků a odpovědí**.

Výsledky se zobrazí na hello pravé straně stránky hello.

tootest webové služby Batch spuštění služby (BES), budete používat soubor CSV:

* Klikněte na tlačítko **Test** v řádku nabídek hello.
* Klikněte na tlačítko **Batch**.
* V části váš vstup klikněte na tlačítko Procházet a přejděte tooyour ukázkový datový soubor.
* Klikněte na tlačítko **Test**.

Stav Hello svůj test se zobrazí v části **testovací úlohy Batch**.

## <a name="consuming-your-web-service"></a>Využívají webové služby
Při nasazení jako webové služby, zadejte experimenty Azure Machine Learning REST API, které mohou být spotřebovávána širokou škálu zařízení a platforem. To je proto zpráv ve formátu hello jednoduché rozhraní API REST přijme a odpoví JSON. Hello portál Azure Machine Learning poskytuje kód, který lze použít toocall hello webové služby v R, C# a Python.

Na stránce spotřeba hello můžete najít:

* klíč Hello rozhraní API a identifikátorů URI pro použití webové služby v aplikacích.
* Tookick šablony aplikace Excel a webové spuštění procesu vaší spotřeby.
* Ukázkový kód v jazyce C#, python a R tooget jste spustili.

Další informace o využívání webových služeb najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Další kroky
Další informace o využívání webových služeb najdete v tématu:

[Jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md)

[Azure Machine Learning webové služby: Nasazení a používání](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
