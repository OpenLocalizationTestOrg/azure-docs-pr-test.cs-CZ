---
title: aaaALM v Azure Machine Learning | Microsoft Docs
description: "Použít osvědčených postupů pro správu životního cyklu aplikací v nástroji Azure Machine Learning Studio"
keywords: "Správa verzí Azure ML, Správa životního cyklu aplikací, ALM, AML,"
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1be6577d-f2c7-425b-b6b9-d5038e52b395
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: haining
ms.openlocfilehash: 99470ff72fea7ab59d9d44f3fded7b9dd49a38c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Správa životního cyklu aplikací v nástroji Azure Machine Learning Studio
Azure Machine Learning Studio je nástroj pro vývoj experimenty strojové učení, které jsou operationalized v platformě Azure cloud hello. Je jako hello Visual Studio IDE a škálovatelné cloudové služby sloučeny do jednoho platformy. Standardní Application Lifecycle Management (ALM) postupy, Správa verzí můžete začlenit různé prostředky tooautomated provádění a nasazení do Azure Machine Learning Studio. Tento článek popisuje některé z možností hello a přístupy.

## <a name="versioning-experiment"></a>Správa verzí experimentu
Existují dva způsoby doporučené tooversion experimentů. Můžete buď závisí na předdefinované historie spouštění, nebo exportovat hello experimentu ve formátu JavaScript Object Notation (JSON) a externě k její správě. Každý přístup se dodává s jeho výhody a nevýhody.

### <a name="experiment-snapshots-using-run-history"></a>Snímky experimentu pomocí spustit historie
Ve model provádění hello hello Azure Machine Learning Studio learning experiment, pokaždé, když kliknete na tlačítko hello **spustit** tlačítko v hello experimentu editoru neměnné kopii hello experimentu je odeslaná toohello plánovače úloh. Tento seznam snímky můžete zobrazit kliknutím hello **historii běhů** tlačítka na panelu příkazů hello v zobrazení editor hello experimentu.

![Historie tlačítku spuštění](media/machine-learning-version-control/runhistory.png)

Můžete potom otevřete hello snímku v režimu uzamčen kliknutím hello název hello experimentu v experimentu hello čas hello byla odeslaná toorun a pořízení snímku hello. Všimněte si, že pouze hello první položky v seznamu hello, který představuje aktuální experimentu hello, je ve stavu upravovat. Všimněte si, že každý snímek může být v různých stavu také stavy i, včetně dokončení se nezdařilo (Partial spustit), se nezdařilo (Partial spustit), nebo koncept.

![Seznam pro spuštění historie](media/machine-learning-version-control/runhistorylist.png)

Po otevřel, můžete uložit hello snímku experimentu jako nový experiment a upravit ho. Pokud snímku experiment obsahuje prostředky například trained model, transformaci a datové sady, které byly aktualizovány verzí, zachová hello snímku při pořízení snímku hello hello odkazy toohello původní verze. Pokud uložíte hello uzamčení snímku jako nový experiment, Azure Machine Learning Studio zjistí hello existenci na novější verzi tyto prostředky a automaticky aktualizuje je v nový experiment hello.

Pokud odstraníte hello experiment, se odstraní všechny snímky tohoto testu.

### <a name="exportimport-experiment-in-json-format"></a>Export a import experimentu ve formátu JSON
pokaždé, když je odeslaná toorun snímky historie Hello spustit zachovat neměnné verzi aplikace hello experimentu v nástroji Azure Machine Learning Studio. Můžete také uložit místní kopii hello experiment a změnami tooyour Oblíbené správy zdrojového kódu, jako je Team Foundation Server a později znovu vytvořit nový experiment z tohoto místního souboru. Můžete použít hello [Azure Machine Learning PowerShell](http://aka.ms/amlps) rutiny PowerShell [ *Export AmlExperimentGraph* ](https://github.com/hning86/azuremlps#export-amlexperimentgraph) a [  *Import AmlExperimentGraph* ](https://github.com/hning86/azuremlps#import-amlexperimentgraph) tooaccomplish který.

soubor JSON Hello je textovou reprezentaci hello experimentovat grafu, která by mohla obsahovat odkaz tooassets v prostoru hello například datové sady nebo modulu trained model. Neobsahuje serializovaných verzi hello asset. Když zkusíte dokumentu JSON hello tooimport zpět do pracovního prostoru hello, hello odkazuje prostředky již musí existovat s hello stejné asset ID, které se odkazuje v experimentu hello. Jinak nebudete moct tooaccess hello importovat experimentu.

## <a name="versioning-trained-model"></a>Správa verzí trained model
Modulu trained model v Azure Machine Learning serializován do formátu známé jako soubor .iLearner a je uložená v účtu úložiště objektů Blob v Azure hello přidruženého k hello prostoru. Jedním ze způsobů tooget kopii souboru .iLearner hello je prostřednictvím hello retraining API. [Tento článek](machine-learning-retrain-models-programmatically.md) vysvětluje, jak funguje hello retraining API. Hello hlavní kroky:

1. Nastavte experimentu školení.
2. Přidání modulu toohello Train Model výstupní port webové služby, nebo hello modul, který vytváří hello vyškolení modelu, například ladit Hyperparameter modelu nebo vytvořit R modelu.
3. Spusťte výukový experiment a poté ji nasadit jako webovou službu školení modelu.
4. Volání hello koncový bod BES hello školení webové služby a zadejte název souboru požadovaného .iLearner hello a umístění účtu úložiště objektů Blob, kde bude uložena.
5. Sklizně hello vytvořeného .iLearner soubor po volání hello BES dokončí.

Jiný způsob tooretrieve hello .iLearner soubor je pomocí prostředí PowerShell hello [ *stažení AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput). To může být snazší, pokud chcete tooget kopii hello .iLearner souboru bez hello nutné tooretrain hello modelu prostřednictvím kódu programu.

Až budete mít hello .iLearner obsahující hello trained model, pak můžete použít strategie správy verzí. Hello strategie může být stejně jednoduché jako použití pre/operátory jako zásady vytváření názvů a právě ponechat hello .iLearner souboru v úložišti objektů Blob nebo kopírování nebo jeho import do systému správy verzí.

soubor uložený .iLearner Hello lze vyhodnocování prostřednictvím nasazených webových služeb.

## <a name="versioning-web-service"></a>Správa verzí webové služby
Můžete nasadit dva typy webových služeb z Azure Machine Learning experimentu. Hello classic webové služby je úzce spojeny s hello experiment, jakož i hello prostoru. Hello novou webovou službu používá framework hello Azure Resource Manager a je už doplněná s původní experimentu hello nebo hello prostoru.

### <a name="classic-web-service"></a>Classic webové služby
tooversion classic webové služby, můžete využít výhod hello webové služby koncového bodu konstrukce. Tady je obvyklý průběh:

1. Z experimentu prediktivní nasadíte novou classic webovou službu, která obsahuje výchozí koncový bod.
2. Můžete vytvořit nový koncový bod s názvem ep2, který zveřejňuje hello aktuální verzi hello experimentu/cvičení modelu.
3. Přejděte zpět a aktualizovat prediktivní experiment a trained model.
4. Můžete znovu nasadit prediktivní experiment hello, které aktualizují hello výchozí koncový bod. Ale to nijak nezmění ep2.
5. Můžete vytvořit další koncový bod s názvem ep3, který zveřejňuje hello novou verzi hello experiment a trained model.
6. V případě potřeby přejděte zpátky toostep 3.

V čase, může mít mnoho koncové body vytvořené v hello stejné webové služby. Každý koncový bod představuje kopii hello experimentu obsahující verzi v okamžiku hello hello trained model v daném okamžiku. Pak můžete použít externí logiku toodetermine které toocall koncový bod, což znamená efektivně výběr verzi hello Trénink modelu pro hello vyhodnocování spustit.

Můžete také vytvořit mnoho koncových bodů identické webové služby a potom oprava různých verzích hello .iLearner souboru toohello koncový bod tooachieve podobný vliv. [Tento článek](machine-learning-create-models-and-endpoints-with-powershell.md) další podrobně vysvětluje, jak tooaccomplish který.

### <a name="new-web-service"></a>Novou webovou službu
Pokud vytvoříte nový na základě Azure Resource Manager webové služby, koncový bod konstrukce hello již není k dispozici. Místo toho můžete vygenerovat definice (WSD) souborů webové služby, ve formátu JSON, z vaší prediktivní experiment pomocí hello [Export AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) prostředí PowerShell., nebo pomocí hello [ *Export AzureRmMlWebservice* ](https://msdn.microsoft.com/library/azure/mt767935.aspx) prostředí PowerShell ze služby nasazené využívající Resource Manager.

Až budete mít hello exportovali a verze souboru WSD řídit, hello WSD můžete taky nasadit jako novou webovou službu v plánu jiné webové služby v jiné oblasti Azure. Ujistěte se, že zadáte účet hello správné úložiště, konfiguraci a také hello nové webové ID plánu služby toopatch v různých .iLearner soubory, můžete upravit soubor WSD hello a aktualizace hello umístění odkaz hello Trénink modelu a nasaďte ho jako novou webovou službu.

## <a name="automate-experiment-execution-and-deployment"></a>Automatizovat nasazení a spuštění experimentu
Důležitým aspektem ALM je toobe možné tooautomate hello provádění a nasazení aplikace hello. V Azure Machine Learning toho lze dosáhnout pomocí hello [modulu PowerShell](http://aka.ms/amlps). Tady je příklad začátku do konce kroky, které jsou relevantní tooa standardní proces ALM automatizované provádění a nasazení pomocí hello [modul Azure Machine Learning Studio PowerShell](http://aka.ms/amlps). Každý krok je propojené tooone nebo další rutiny prostředí PowerShell, kterou můžete použít tooaccomplish, krok.

1. [Nahrání datové sady](https://github.com/hning86/azuremlps#upload-amldataset).
2. Zkopírujte výukový experiment do pracovního prostoru hello z [prostoru](https://github.com/hning86/azuremlps#copy-amlexperiment) nebo z [Galerie](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery), nebo [importovat](https://github.com/hning86/azuremlps#import-amlexperimentgraph) [exportovat](https://github.com/hning86/azuremlps#export-amlexperimentgraph) experimentovat z místní disk.
3. [Aktualizovat datovou sadu hello](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) v experimentu školení hello.
4. [Spusťte experiment školení hello](https://github.com/hning86/azuremlps#start-amlexperiment).
5. [Zvýšení úrovně hello trained model](https://github.com/hning86/azuremlps#promote-amltrainedmodel).
6. [Zkopírujte prediktivní experiment](https://github.com/hning86/azuremlps#copy-amlexperiment) do pracovního prostoru hello.
7. [Aktualizace hello trained model](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) v prediktivní experiment hello.
8. [Spusťte experiment prediktivní hello](https://github.com/hning86/azuremlps#start-amlexperiment).
9. [Nasazení webové služby](https://github.com/hning86/azuremlps#new-amlwebservice) z prediktivní experiment hello.
10. Testování hello webové služby [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) nebo [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) koncový bod.

## <a name="next-steps"></a>Další kroky
* Stáhnout hello [Azure Machine Learning Studio PowerShell](http://aka.ms/amlps) modulu a spusťte tooautomate ALM úkoly.
* Zjistěte, jak příliš[vytvořit a spravovat velký počet ML modelů pomocí právě jeden experimentu](machine-learning-create-models-and-endpoints-with-powershell.md) pomocí prostředí PowerShell a retraining API.
* Další informace o [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).
