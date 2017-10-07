---
title: modul aaaPowerShell pro Machine Learning | Microsoft Docs
description: "Hello modul prostředí PowerShell pro Azure Machine Learning je k dispozici v režimu verzi public preview. Pomocí prostředí PowerShell toocreate a spravovat pracovní prostory, experimenty, webové služby a další."
keywords: "experiment,lineární regrese,algoritmy Machine Learningu,kurz Machine Learningu,techniky prediktivního modelování,experiment z oblasti datové vědy"
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: a9001cc2-3aa0-47e1-b175-1f76408ba1d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: garye;haining
ms.openlocfilehash: 59362027356b86bf286b7c07380db677ae1d71c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Modul PowerShell pro Microsoft Azure Machine Learning
Hello modul prostředí PowerShell pro Azure Machine Learning je výkonný nástroj, který vám umožní toouse prostředí Windows PowerShell toomanage pracovních prostorů, experimenty, datové sady, Classic webové služby a další.

Můžete zobrazit dokumentaci hello a stáhnout modul hello, společně s hello úplný zdrojový kód, na adrese [https://aka.ms/amlps](https://aka.ms/amlps). 

> [!NOTE]
> modul Azure Machine Learning PowerShell Hello je aktuálně v režimu preview. modul Hello bude toobe vylepšené a rozšířit během tohoto období preview. Dohlížet na hello [Cortana Intelligence a Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/) pro příspěvky a informace.

## <a name="what-is-hello-machine-learning-powershell-module"></a>Co je modul Machine Learning PowerShell hello?
modul Machine Learning PowerShell Hello. Na základě NET modulu DLL, který vám umožní toofully spravovat pracovních prostorů Azure Machine Learning, experimenty, datové sady, Classic webové služby a koncových bodů webové služby Classic z prostředí Windows PowerShell. 

Společně s hello modul, si můžete stáhnout hello úplný zdrojový kód, který zahrnuje řádně oddělených [vrstvu rozhraní API jazyka C#](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Můžete odkazovat na tuto knihovnu DLL z projektu rozhraní .NET a spravovat Azure Machine Learning prostřednictvím rozhraní .NET kódu. Kromě toho hello DLL závisí na základní rozhraní REST API, který můžete použít přímo z vašeho oblíbeného klienta.

## <a name="what-can-i-do-with-hello-powershell-module"></a>Co můžete dělat s modulu PowerShell hello?
Zde jsou některé hello úlohy, které můžete provádět s Tento modul prostředí PowerShell. Podívejte se na hello [úplné dokumentace](https://aka.ms/amlps) pro tyto a další mnoho funkcí.

* Zřiďte nový pracovní prostor s pomocí certifikátu pro správu ([New-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace)).
* Exportujte a importujte soubor JSON s grafem experimentu ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) a [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph)).
* Spusťte experiment ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment)).
* Z prediktivního experimentu vytvořte webovou službu ([New-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice)).
* Vytvoření koncového bodu na publikované webové službě ([přidat AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Zavolejte koncový bod webové služby RRS či BES ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) a [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint)).

Tady je zběžný příklad pomocí prostředí PowerShell toorun existující experimentu:

        #Find hello first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run hello Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Podrobnější případu použití, najdete v článku o používání tooautomate modulu prostředí PowerShell hello úlohu běžně požadovaný: [vytvořit mnoho modely Machine Learning a webové koncové body služby z jednoho experimentu pomocí prostředí PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Jak mám začít?
tooget spuštění pomocí prostředí PowerShell Machine Learning, stáhněte si hello [balíček verze](https://github.com/hning86/azuremlps/releases) z Githubu a postupujte podle hello [pokyny pro instalaci](https://github.com/hning86/azuremlps/blob/master/README.md). Hello pokyny popisují, jak toounblock hello stáhnout nebo rozbalené DLL a importujte ho do prostředí PowerShell. Většina hello rutiny vyžadují zadat ID pracovního prostoru hello, hello prostoru autorizační token a hello oblast Azure, který hello pracovního prostoru se. Hello nejjednodušší způsob, jak tooprovide hello hodnoty je prostřednictvím výchozí config.json soubor. pokyny Hello také vysvětlují, jak tooconfigure tento soubor. 

A pokud chcete, můžete klonovat hello git stromu, upravit kód hello a zkompilovat ho místně pomocí sady Visual Studio.

## <a name="next-steps"></a>Další kroky
Úplnou dokumentaci hello pro modul prostředí PowerShell hello na můžete najít [https://aka.ms/amlps](https://aka.ms/amlps). 

Příklad rozšířené jak toouse hello modulu ve scénáři reálného, podívejte se na hello podrobné případu, použití [vytvořit mnoho modely Machine Learning a webové koncové body služby z jednoho experimentu pomocí prostředí PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).
