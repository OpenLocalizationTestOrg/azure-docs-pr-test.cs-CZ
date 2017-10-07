---
title: "aaaCreate nasazení šablon pro Azure Logic Apps | Microsoft Docs"
description: "Vytváření šablon Azure Resource Manageru pro logic apps nasazení a release management"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 2f09445f10a376a745d6acbba94ca29d5f79fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-templates-for-logic-apps-deployment-and-release-management"></a>Vytvoření šablon pro logic apps nasazení a release management

Po vytvoření aplikace logiky můžete chtít toocreate jej jako šablonu Azure Resource Manager.
Tímto způsobem můžete snadno nasadit hello logiku aplikace tooany prostředí nebo skupinu prostředků, kde je může být nutné.
Další informace o šablonách Resource Manager najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) a [nasazení prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Šablona nasazení aplikace logiky

Aplikace logiky má tři základní součásti:

* **Prostředek aplikace logiky**: obsahuje informace o třeba ceny plán, umístění a hello Definice pracovního postupu.
* **Definice pracovního postupu**: popisuje kroky pracovního postupu a jak modul Logic Apps hello by měla spustit pracovní postup hello svou aplikaci logiky.
V aplikaci logiky můžete zobrazit tuto definici **zobrazení kódu** okno.
V hello prostředků aplikace logiky, můžete tuto definici najít v hello `definition` vlastnost.
* **Připojení**: odkazuje tooseparate prostředky, které bezpečně uložit metadata o všechna připojení konektoru, jako je například připojovací řetězec a přístupový token.
V hello prostředků aplikace logiky, aplikaci logiky odkazuje na tyto prostředky v hello `parameters` části.

Všechny tyto údaje existující aplikace logiky můžete zobrazit pomocí některého nástroje, například [Průzkumníka prostředků Azure](http://resources.azure.com).

toomake šablonu pro toouse aplikaci logiky s nasazením skupin prostředků, je nutné definovat hello prostředky a Parametrizace podle potřeby.
Například Pokud nasazujete tooa vývoj, testování a provozním prostředí, pravděpodobně chcete toouse jiným připojením řetězce tooa SQL database v každé prostředí.
Nebo můžete chtít toodeploy v rámci různých předplatných nebo skupiny prostředků.  

## <a name="create-a-logic-app-deployment-template"></a>Vytvoření šablony nasazení aplikace logiky

Nejjednodušší způsob, jak toohave Hello šablony nasazení platný logiku aplikace je toouse [Visual Studio Tools for Logic Apps](logic-apps-deploy-from-vs.md).
Nástroje sady Visual Studio Hello vytvořit šablonu platný nasazení, který lze použít v rámci žádné předplatné nebo umístění.

Několik dalších nástrojů vám může pomoci při vytváření šablony nasazení aplikace logiky.
Můžete vytvářet ručně, který je pomocí hello prostředky již tady popisovaných toocreate parametry podle potřeby.
Další možností je toouse [creator šablona aplikace logiky](https://github.com/jeffhollan/LogicAppTemplateCreator) modul prostředí PowerShell. Tento modul open-source nejprve vyhodnotí aplikace logiky hello a všechna připojení, používá a poté generuje prostředků šablony s hello potřebné parametry pro nasazení.
Například pokud máte aplikace logiky, která přijímá zprávy z fronty služby Azure Service Bus a přidá data tooan Azure SQL database, nástroj hello zachovává veškerou logiku hello orchestration a parameterizes hello SQL a sběrnice připojovací řetězce, takže můžete nastavit v nasazení.

> [!NOTE]
> Připojení musí být v rámci hello stejné skupině prostředků jako aplikace logiky hello.
>
>

### <a name="install-hello-logic-app-template-powershell-module"></a>Nainstalujte modul prostředí PowerShell šablona aplikace logiky hello
Hello nejjednodušší způsob, jak tooinstall hello modul je prostřednictvím hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), pomocí příkazu hello `Install-Module -Name LogicAppTemplate`.  

Také můžete nainstalovat modul prostředí PowerShell hello ručně:

1. Stáhněte si nejnovější verzi hello hello [creator šablona aplikace logiky](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. Extrakci hello složku ve složce modulu PowerShell (obvykle `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Pro modul toowork hello žádné klienta a předplatné přístup tokenu, doporučujeme použít s hello [ARMClient](https://github.com/projectkudu/ARMClient) nástroj příkazového řádku.  To [příspěvku na blogu](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) popisuje ARMClient podrobněji.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generovat šablonu aplikace logiky pomocí prostředí PowerShell
Po instalaci prostředí PowerShell můžete vygenerovat šablonu pomocí hello následující příkaz:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`je ID hello předplatného Azure. Tento řádek nejprve získá přístupový token prostřednictvím ARMClient, potom prostřednictvím kanálu předá ji prostřednictvím toohello skript prostředí PowerShell a potom vytvoří šablonu hello v souboru JSON.

## <a name="add-parameters-tooa-logic-app-template"></a>Přidání parametrů tooa logiku aplikace šablony
Po vytvoření šablony aplikace logiky, můžete pokračovat tooadd nebo upravte parametry, které může být nutné. Například pokud vaše definice obsahuje tooan ID prostředku Azure funkci nebo vnořený pracovní postup, abyste naplánovali toodeploy v jednom nasazení, můžete přidat další prostředky tooyour šablonu a Parametrizace ID podle potřeby. Hello totéž platí i tooany odkazy toocustom rozhraní API nebo Swagger koncových bodů očekávat toodeploy s každé skupině prostředků.

## <a name="deploy-a-logic-app-template"></a>Nasazení šablony aplikace logiky

Šablony můžete nasadit pomocí všechny nástroje, například prostředí PowerShell, REST API, [Visual Studio Team Services Release Management](#team-services)a nasazení šablony prostřednictvím hello portálu Azure.
Navíc toostore hello hodnoty parametrů, doporučujeme, abyste vytvořili [soubor parametrů](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Zjistěte, jak příliš[nasazení prostředků pomocí šablony Azure Resource Manageru a prostředí PowerShell](../azure-resource-manager/resource-group-template-deploy.md) nebo [nasazení prostředků pomocí šablony Azure Resource Manager a hello portál Azure](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>Autorizaci OAuth připojení

Po nasazení aplikace logiky hello funguje začátku do konce s platné parametry.
Však musí povolit stále OAuth připojení toogenerate token platný přístup.
připojení tooauthorize OAuth, otevřete aplikaci logiky hello v hello návrhář aplikace logiky a autorizaci těchto připojení. Nebo pro automatické nasazení, můžete použít skriptu tooconsent tooeach OAuth připojení.
Na Githubu v části je ukázkový skript [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projektu.

<a name="team-services"></a>
## <a name="visual-studio-team-services-release-management"></a>Visual Studio Team Services Release Management

Běžný scénář pro nasazení a správu prostředí je toouse nástroje, jako je správa verzí ve Visual Studio Team Services, pomocí šablony nasazení aplikace logiky. Visual Studio Team Services zahrnuje [nasazení skupiny prostředků Azure](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) úloh můžete přidat tooany sestavení nebo verzi kanálu. Je třeba toohave [instanční objekt](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) pro autorizaci toodeploy a pak může generovat hello verze definice.

1. Správa verzí, vyberte **prázdný** tak, aby vytvořit prázdnou definici.

    ![Vytvořte prázdnou definici][1]

2. Vyberte všechny prostředky, které potřebujete k této, pravděpodobně včetně hello logiku aplikace šablony, která je generována ručně nebo jako součást hello proces sestavení.
3. Přidat **nasazení skupiny prostředků Azure** úloh.
4. Nakonfigurovat [instanční objekt](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)a odkazovat hello šablony a parametry šablony soubory.
5. Toobuild kroky pokračujte v procesu hello verze pro všechny prostředí, automatizovaných testů nebo schvalovatelů podle potřeby.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
