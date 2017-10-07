---
title: "projekty skupiny prostředků Studio Azure aaaVisual | Microsoft Docs"
description: "Pomocí sady Visual Studio toocreate projekt skupiny prostředků Azure a nasazení prostředků tooAzure hello."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 4bd084c8-0842-4a10-8460-080c6a085bec
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: tomfitz
ms.openlocfilehash: 672c1e71fb809b3b547f0fad30240d45de1ba923
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Vytvoření a nasazení skupiny prostředků Azure pomocí sady Visual Studio
Pomocí sady Visual Studio a hello [Azure SDK](https://azure.microsoft.com/downloads/), můžete vytvořit projekt, který nasadí vaši infrastrukturu a kód tooAzure. Můžete například definovat hello webového hostitele, webový server a databáze pro vaši aplikaci a nasadit tuto infrastrukturu spolu hello kódu. Nebo můžete definovat virtuální počítač, virtuální síť a účet úložiště a nasadit tuto infrastrukturu spolu se skriptem, který se spouští na virtuálním počítači. Hello **skupiny prostředků Azure** projekt nasazení umožňuje vám toodeploy všechny hello potřebné prostředky v jedné opakovatelné operace. Další informace o nasazení a správě prostředků najdete v tématu [Přehled Azure Resource Manageru](resource-group-overview.md).

Projekty skupiny prostředků Azure obsahují šablony JSON Azure Resource Manageru, které definují prostředky hello nasazení tooAzure. toolearn o hello elementy hello šablony Resource Manageru, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md). Visual Studio tooedit vám umožňuje tyto šablony a poskytuje nástroje, které zjednodušují práci se šablonami.

V tomto článku nasadíte webovou aplikaci a SQL Database. Ale hello kroky jsou téměř hello stejný pro libovolný typ prostředku. Stejně snadno můžete nasadit virtuální počítač a prostředky, které s ním souvisejí. Visual Studio poskytuje řadu různých předem připravených šablon pro běžné scénáře nasazení.

Tento článek ukazuje Visual Studio 2017. Pokud používáte Visual Studio 2015 Update 2 a Microsoft Azure SDK pro .NET 2.9 nebo Visual Studio 2013 a Azure SDK 2.9 prostředí je z velké části hello stejné. Můžete použít verzích hello Azure SDK z 2.6 nebo novější. prostředí hello uživatelské rozhraní však může být jiný než hello uživatelské rozhraní zobrazí v tomto článku. Důrazně doporučujeme nainstalovat nejnovější verzi hello hello [Azure SDK](https://azure.microsoft.com/downloads/) před spuštěním kroků hello. 

## <a name="create-azure-resource-group-project"></a>Vytvoření projektu skupiny prostředků Azure
V tomto postupu vytvoříte projekt skupiny prostředků Azure pomocí šablony **Web app + SQL** (Webová aplikace a SQL).

1. V sadě Visual Studio zvolte **Soubor**, **Nový projekt** a potom zvolte **C#** nebo **Visual Basic**. Potom vyberte **Cloud** a projekt **Azure Resource Group** (Skupina prostředků Azure).
   
    ![Projekt nasazení v cloudu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)
2. Výběr hello šablony, které chcete toodeploy tooAzure Resource Manager. Všimněte si, existuje mnoho různých možností v závislosti na hello typ projektu chcete toodeploy. V tomto článku zvolte hello **Web app + SQL** šablony.
   
    ![Volba šablony](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)
   
    Hello šablonu, kterou vyberete, je jenom výchozí bod; můžete přidávat a odebírat prostředky toofulfill váš scénář.
   
   > [!NOTE]
   > Visual Studio načte seznam dostupných šablon online. seznam Hello může změnit.
   > 
   > 
   
    Visual Studio vytvoří projekt nasazení skupiny prostředků pro hello webovou aplikaci a databázi SQL.
3. toosee co jste vytvořili, vyhledejte v uzlu hello v projektu nasazení hello.
   
    ![zobrazení uzlů](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)
   
    Vzhledem k tomu, že jsme zvolili hello Web app + SQL šablony v tomto příkladu, uvidíte hello následující soubory: 
   
   | Název souboru | Popis |
   | --- | --- |
   | Deploy-AzureResourceGroup.ps1 |Skript prostředí PowerShell, která volá tooAzure toodeploy příkazy prostředí PowerShell Resource Manager.<br />**Poznámka:** Visual Studio použije tento toodeploy skript prostředí PowerShell šablony. Provedené změny toothis skriptu ovlivnit nasazení v sadě Visual Studio, tak buďte opatrní. |
   | WebSiteSQLDatabase.json |šablonu Hello Resource Manager, která definuje hello infrastrukturu, kterou chcete nasadit tooAzure a hello parametrů, které můžete zadat během nasazování. Definuje také hello závislosti mezi prostředky hello, takže správce prostředků nasadí hello prostředky ve správném pořadí hello. |
   | WebSiteSQLDatabase.parameters.json |Soubor parametrů, který obsahuje hodnoty šablonou hello. Můžete předat toocustomize hodnoty parametru každého nasazení. |
   
    Tyto základní soubory obsahují všechny projekty nasazení skupiny prostředků. Ostatní projekty mohou obsahovat další soubory toosupport další funkce.

## <a name="customize-hello-resource-manager-template"></a>Přizpůsobení šablony Resource Manageru hello
Projekt nasazení lze přizpůsobit úpravou šablony JSON hello, které popisují hello zdroje, které má toodeploy. JSON je zkratka pro JavaScript Object Notation a je o serializovaný datový formát, který je snadno toowork s. Hello soubory JSON využívají schéma, na kterou odkazujete v horní části hello každého souboru. Pokud chcete toounderstand hello schématu, můžete stáhnout a analyzujte ji. Hello schéma definuje přizpůsobitelné prvky jsou platné, hello typy a formáty polí, hello možných hodnot výčtové hodnoty a tak dále. toolearn o hello elementy hello šablony Resource Manageru, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).

Otevřete toowork v šabloně, **WebSiteSQLDatabase.json**.

Hello Visual Studio nabízí editor nástroje tooassist k úpravě hello šablony Resource Manageru. Hello **osnovy JSON** okno umožňuje snadno toosee hello elementy v šabloně definované.

![zobrazení osnovy JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Výběrem libovolné hello elementů v přehledu hello přejdete toothat součástí hello šablony a zvýrazní hello odpovídající JSON.

![navigace JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Můžete přidat prostředek buď vyberete hello **přidat prostředek** tlačítka v horní části hello hello osnova JSON okno, nebo kliknutím pravým tlačítkem na **prostředky** a výběrem **přidat nový prostředek**.

![přidání prostředku](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

V tomto kurzu vyberte **účet úložiště** a pojmenujte ho. Zadejte název, který nebude delší než 11 znaků. Obsahovat smí jenom čísla a malá písmena.

![přidání úložiště](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Všimněte si, že byl přidán hello prostředků nejen, ale také parametr pro hello typ účtu úložiště a proměnnou pro hello název účtu úložiště hello.

![zobrazení osnovy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Hello **storageType** je předdefinovaný povolené typy a výchozí typ parametru. Tyto hodnoty můžete ponechat beze změny nebo je můžete podle potřeby upravit. Pokud nechcete, aby každý, kdo toodeploy **Premium_LRS** účet úložiště pomocí této šablony, odeberte ji z hello povolené typy. 

```json
"storageType": {
  "type": "string",
  "defaultValue": "Standard_LRS",
  "allowedValues": [
    "Standard_LRS",
    "Standard_ZRS",
    "Standard_GRS",
    "Standard_RAGRS"
  ]
}
```

Visual Studio také poskytuje technologii intellisense toohelp je pochopit, jaké vlastnosti jsou k dispozici při úpravě šablony hello. Například tooedit hello vlastnosti pro plán služby App Service přejděte toohello **HostingPlan** prostředků a přidejte hodnotu hello **vlastnosti**. Všimněte si, že intellisense zobrazí hello dostupné hodnoty a poskytuje popis této hodnoty.

![zobrazení technologie IntelliSense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Můžete nastavit **numberOfWorkers** too1.

```json
"properties": {
  "name": "[parameters('hostingPlanName')]",
  "numberOfWorkers": 1
}
```

## <a name="deploy-hello-resource-group-project-tooazure"></a>Nasazení tooAzure projektu skupiny prostředků hello
Můžete je nyní připraven toodeploy projektu. Při nasazení projektu skupiny prostředků Azure, můžete nasadit tooan skupina prostředků Azure. Skupina prostředků Hello je logické seskupení prostředků, které sdílejí společné životního cyklu.

1. Hello místní nabídce uzlu projektu nasazení hello, zvolte **nasadit** > **nový**.
   
    ![Položka nabídky Nasadit, Nové nasazení](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)
   
    Hello **nasazení tooResource skupiny** zobrazí se dialogové okno.
   
    ![Nasazení tooResource dialogové okno skupiny](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)
2. V hello **skupiny prostředků** rozevíracím, vyberte existující skupinu prostředků nebo vytvořte novou. toocreate skupinu prostředků, otevřete hello **skupiny prostředků** rozevírací pole a zvolte **vytvořit nový**.
   
    ![Nasazení tooResource dialogové okno skupiny](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)
   
    Hello **vytvořit skupinu prostředků** zobrazí se dialogové okno. Přidělte skupině název a umístění a vyberte hello **vytvořit** tlačítko.
   
    ![Dialogové okno vytvoření skupiny prostředků](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
3. Upravit hello parametry pro nasazení hello výběrem hello **upravit parametry** tlačítko.
   
    ![Tlačítko Upravit parametry](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)
4. Zadejte hodnoty pro parametry hello prázdné a vyberte hello **Uložit** tlačítko. Hello prázdné parametry jsou **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**, a **databaseName**.
   
    **hostingPlanName** Určuje název hello [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) toocreate. 
   
    **administratorLogin** určuje hello uživatelské jméno pro správce systému SQL Server hello. Nepoužívejte běžné názvy správců, například **sa** nebo **admin**. 
   
    Hello **administratorLoginPassword** Určuje heslo pro správce systému SQL Server. Hello **ukládání hesel jako prostý text v souboru parametrů hello** není možnost zabezpečeného; proto nevybírejte tuto možnost. Vzhledem k tomu, že heslo hello není uloženo jako prostý text, musíte tooprovide toto heslo znovu během nasazení. 
   
    **databaseName** Určuje název databáze toocreate hello. 
   
    ![Dialogové okno Upravit parametry](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
5. Zvolte hello **nasadit** tlačítko toodeploy hello projektu tooAzure. Otevře se konzole PowerShell mimo hello instanci aplikace Visual Studio. V konzole PowerShell hello po zobrazení výzvy zadejte heslo správce systému SQL Server hello. **Konzolu prostředí PowerShell může být skrytá za jiných položek nebo minimalizovaná hello hlavním panelu.** Vyhledejte této konzoly a vyberte jej tooprovide hello heslo.
   
   > [!NOTE]
   > Visual Studio může požádat tooinstall hello rutin prostředí Azure PowerShell. Třeba hello prostředí Azure PowerShell toosuccessfully rutin nasazení skupiny prostředků. Pokud k tomu budete vyzváni, nainstalujte je.
   > 
   > 
6. Hello nasazení může trvat několik minut. V hello **výstup** windows, najdete v části hello stav nasazení hello. Po dokončení nasazení hello hello poslední zpráva znamená úspěšné nasazení se něco podobného jako:
   
        ... 
        18:00:58 - Successfully deployed template 'websitesqldatabase.json' tooresource group 'DemoSiteGroup'.
7. V prohlížeči otevřete hello [portál Azure](https://portal.azure.com/) a přihlaste se tooyour účtu. toosee hello skupinu prostředků, vyberte **skupiny prostředků** a skupině prostředků hello jste nasadili.
   
    ![výběr skupiny](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)
8. Zobrazí všechny prostředky hello nasazení. Všimněte si, že hello název hello účet úložiště není právě jaké jste zadali při přidávání prostředku. účet úložiště Hello musí být jedinečný. Hello šablona automaticky přidá řetězec znaků názvu toohello jste zadali tooprovide jedinečný název. 
   
    ![zobrazení prostředků](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)
9. Pokud provést změny a chcete tooredeploy projektu, vyberte existující skupinu prostředků hello z hello místní nabídky projektu skupiny prostředků Azure. V místní nabídce hello, zvolte **nasadit**a potom vyberte skupinu prostředků hello jste nasadili.
   
    ![Nasazená skupina prostředků Azure](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Nasazení kódu do vaší infrastruktury
V tomto okamžiku jste nasadili hello infrastrukturu pro vaši aplikaci, ale není nasazený s projektem hello žádný kód. Tento článek ukazuje, jak toodeploy webové aplikace a SQL Database tabulky během nasazení. Pokud nasazujete virtuální počítač místo webové aplikace, budete chtít toorun nějaký kód na počítači hello jako součást nasazení. Hello proces nasazení kódu pro webovou aplikaci nebo pro nastavení virtuálního počítače je téměř hello stejné.

1. Přidejte projektu tooyour řešení sady Visual Studio. Klikněte pravým tlačítkem na řešení hello a vyberte **přidat** > **nový projekt**.
   
    ![přidání projektu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)
2. Přidejte **webovou aplikaci ASP.NET**. 
   
    ![přidání webové aplikace](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
3. Vyberte **MVC**.
   
    ![výběr MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
4. Jakmile sady Visual Studio vytvoří webovou aplikaci, zobrazí se oba projekty v řešení hello.
   
    ![zobrazení projektů](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)
5. Nyní musíte toomake se, že si je vědoma nový projekt hello projekt skupiny prostředků. Vraťte se zpátky projekt skupiny prostředků tooyour (AzureResourceGroup1). Klikněte pravým tlačítkem na **Odkazy** a vyberte **Přidat odkaz**.
   
    ![přidání odkazu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)
6. Vyberte projekt hello webové aplikace, kterou jste vytvořili.
   
    ![přidání odkazu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
   
    Přidáním odkazu propojení projektu skupiny prostředků projektu hello webové aplikace toohello a automaticky nastavíte tři klíčové vlastnosti. Zobrazí tyto vlastnosti v hello **vlastnosti** okna pro odkaz hello.
   
      ![zobrazení odkazu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
   
    Vlastnosti Hello jsou:
   
   * Hello **další vlastnosti** obsahuje pracovní umístění, které se posune toohello Azure Storage balíčku pro nasazení hello webu. Všimněte si hello složku (ExampleApp) a soubor (package.zip). Je nutné tooknow tyto hodnoty vzhledem k tomu, že jim poskytujete jako parametry při nasazování hello aplikace. 
   * Hello **patří cesta k souboru** obsahuje hello cestu, kde se má vytvořit balíček hello. Hello **zahrnout cíle** obsahuje hello příkaz, který provádí nasazení. 
   * Výchozí hodnota Hello **sestavení; Balíček** umožňuje hello toobuild nasazení a vytvoření balíčku pro nasazení webu (package.zip).  
     
     Není nutné profil publikování jako hello nasazení získá informace potřebné hello z hello vlastnosti toocreate hello balíčku.
7. Přejděte zpět tooWebSiteSQLDatabase.json a přidat šablonu toohello prostředků.
   
    ![přidání prostředku](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)
8. Tentokrát vyberte **Nasazení webu pro webové aplikace**. 
   
    ![přidání nasazení webu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
9. Znovu nasaďte skupiny prostředků toohello projektu skupiny prostředků. Tentokrát použijeme několik nových parametrů. Není nutné tooprovide hodnoty pro **_artifactsLocation** nebo **_artifactsLocationSasToken** protože Visual Studio automaticky generuje tyto hodnoty. Máte ale tooset hello složce a cestě toohello název souboru, která obsahuje balíček pro nasazení hello (zobrazené jako **ExampleAppPackageFolder** a **ExampleAppPackageFileName** v hello následující bitové kopie ). Zadejte hodnoty hello jste viděli dříve v hello vlastnosti odkazu (**ExampleApp** a **package.zip**).
   
    ![přidání nasazení webu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
   
    Pro hello **účet úložiště artefaktů**, vyberte hello jeden nasazený s touto skupinou prostředků.
10. Po dokončení nasazení hello výběr webové aplikace na portálu hello. Vyberte lokalitu toohello toobrowse URL hello.
    
     ![procházení webu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)
11. Všimněte si, že jste úspěšně nasadili hello výchozí aplikace ASP.NET.
    
     ![zobrazení nasazené aplikace](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Další kroky
* v tématu toolearn o správě prostředků prostřednictvím portálu hello [pomocí hello Azure portálu toomanage vašich prostředků Azure](resource-group-portal.md).
* toolearn Další informace o šablony, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).

