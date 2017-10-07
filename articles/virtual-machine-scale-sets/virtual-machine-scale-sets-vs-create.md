---
title: "aaaDeploy sadu škálování virtuálního počítače pomocí sady Visual Studio | Microsoft Docs"
description: "Nasazení sady škálování virtuálního počítače pomocí sady Visual Studio a šablony Resource Manageru"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c89a9f2478ccc3d22989aea604a4273bcc46df82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-visual-studio"></a>Jak toocreate Škálovací sady virtuálních počítačů pomocí sady Visual Studio
Tento článek ukazuje, jak toodeploy Azure Škálovací sady virtuálních počítačů pomocí Visual Studio nasazení skupiny prostředků.

[Azure sady škálování virtuálního počítače](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) Azure Compute toodeploy prostředků a spravovat kolekce podobně jako virtuální počítače s automatickému škálování a vyrovnávání zatížení. Můžete zřídit a nasadit sady škálování virtuálního počítače pomocí [šablon Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates). Šablony Azure Resource Manageru můžete nasadit pomocí rozhraní příkazového řádku Azure, prostředí PowerShell, REST a také přímo ze sady Visual Studio. Visual Studio poskytuje sadu příklad šablony, které můžete nasadit jako součást projektu nasazení skupiny prostředků Azure.

Nasazení skupiny prostředků Azure jsou toogroup způsob a publikovat sadu souvisejících prostředků Azure v rámci operace jedno nasazení. Další informace o nich zde: [vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Požadavky
tooget začít s nasazením sady škálování virtuálního počítače v sadě Visual Studio, potřebujete následující hello:

* Visual Studio 2013 nebo novější
* Azure SDK 2.7, 2.8 nebo 2.9

>[!NOTE]
>Tyto pokyny předpokládají, že používáte Visual Studio s [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Vytvoření projektu
1. Vytvořte nový projekt v sadě Visual Studio výběrem **souboru | Nové | Projekt**.
   
    ![Nový soubor][file_new]

2. V části **Visual C# | Cloud**, zvolte **Azure Resource Manager** toocreate projekt pro nasazení šablonu Azure Resource Manager.
   
    ![Vytvoření projektu][create_project]

3. Hello seznam šablon vyberte hello Linux nebo Windows nastavte šablony škálování virtuálního počítače.
   
   ![Vyberte šablonu][select_Template]

4. Po vytvoření projektu zobrazí nasazení skriptů prostředí PowerShell, šablonu Azure Resource Manager a soubor parametrů pro hello sadu škálování virtuálního počítače.
   
    ![Průzkumník řešení][solution_explorer]

## <a name="customize-your-project"></a>Přizpůsobení projektu
Teď můžete upravit hello šablony toocustomize pro potřeby vaší aplikace, například přidávání vlastnosti rozšíření virtuálního počítače nebo úprava pravidla Vyrovnávání zatížení. Ve výchozím nastavení jsou šablony sad škálování hello virtuální počítač nakonfigurovaný toodeploy hello AzureDiagnostics rozšíření, které umožňuje snadno tooadd škálování pravidla. Kromě toho nasadí nástroj pro vyrovnávání zatížení s veřejnou IP adresu, nakonfigurované příchozích pravidel NAT. 

Nástroj pro vyrovnávání zatížení Hello vám umožní připojit se toohello instance virtuálních počítačů pomocí protokolu SSH (Linux) nebo protokolu RDP (Windows). rozsah portů front-endu Hello začíná na 50000. Pro linux to znamená, že pokud jste SSH tooport 50000, jsou směrované tooport 22 z hello první virtuální počítač v hello sady škálování virtuálního počítače. Připojení tooport 50001 je směrované tooport 22 Dobrý den sekundu virtuálních počítačů, a tak dále.

 Dobře tooedit vaše šablony v prostředí Visual Studio je toouse hello osnovy JSON tooorganize hello parametry, proměnné a prostředky. Se seznámíte s hello můžete před nasazením schématu sady Visual Studio upozorníme na chyby ve vaší šabloně.

![Průzkumník JSON][json_explorer]

## <a name="deploy-hello-project"></a>Nasazení projektu hello
1. Nasaďte hello šablony Azure Resource Manageru toocreate hello sadu škálování virtuálního počítače prostředků. Klikněte pravým tlačítkem na uzel projektu hello a zvolte **nasadit | Nové nasazení**.
   
    ![Nasazení šablony][5deploy_Template]
    
2. V dialogovém okně "Nasadit tooResource skupiny" hello, vyberte své předplatné.
   
    ![Nasazení šablony][6deploy_Template]

3. Tady můžete vytvořit šablonu, aby toodeploy skupiny prostředků Azure.
   
    ![Novou skupinu prostředků][new_resource]

4. Klikněte na tlačítko **upravit parametry** tooenter parametry, které se předávají tooyour šablony. Zadejte hello uživatelské jméno a heslo pro hello operačního systému, což je nasazení požadované toocreate hello. Pokud nemáte prostředí PowerShell nástroje pro sadu Visual Studio, je vhodné toocheck **ukládat hesla** tooavoid skrytá příkazového řádku prostředí PowerShell vyzve, nebo použijte [keyvault podporu](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Upravit parametry][edit_parameters]

5. Klikněte na tlačítko **nasadit**. Hello **výstup** v okně se zobrazí průběh nasazení hello. Všimněte si, že akce hello provádí hello **nasadit AzureResourceGroup.ps1** skriptu.
   
   ![Výstup – okno][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a>Prohlížení vaší Škálovací sadu virtuálních počítačů
Po dokončení nasazení hello můžete zobrazit hello novou sadu škálování virtuálního počítače v sadě Visual Studio hello **Průzkumník cloudu** (aktualizace seznamu hello). Průzkumník cloudu umožňuje spravovat prostředky Azure v sadě Visual Studio při vývoji aplikace. Můžete také zobrazit sadu škálování virtuálního počítače v hello [portál Azure](https://portal.azure.com) a [Průzkumníka prostředků Azure](https://resources.azure.com/).

![Průzkumník cloudu][cloud_explorer]

 portál Hello poskytuje nejlepší způsob hello toovisually spravovat infrastrukturu Azure s webovým prohlížečem, zatímco Průzkumníka prostředků Azure poskytuje tooexplore snadný způsob a ladění prostředky Azure, udělíte okno do hello "zobrazit instance" a také zobrazuje prostředí PowerShell příkazy pro hello prostředky, který hledáte v.

## <a name="next-steps"></a>Další kroky
Jakmile jste úspěšně nasazena sady škálování virtuálního počítače pomocí sady Visual Studio, můžete dále přizpůsobit vašeho projektu toosuit vaše požadavky aplikací. Můžete například nakonfigurovat automatické škálování přidáním **Insights** prostředků, přidávání infrastruktury tooyour šablony (jako samostatné virtuální počítače) nebo nasazování aplikací pomocí rozšíření vlastních skriptů hello. Dobrým příkladem šablony lze nalézt v hello [šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates) úložiště GitHub (vyhledejte "vmss").

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
