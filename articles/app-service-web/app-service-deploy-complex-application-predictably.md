---
title: "aaaProvision a nasazení mikroslužeb předvídatelné v Azure"
description: "Zjistěte, jak toodeploy aplikace skládá z mikroslužeb v Azure App Service jako na jednu jednotku a předvídatelný způsobem pomocí šablony pro skupiny prostředků JSON a skriptů prostředí PowerShell."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: bb51e565-e462-4c60-929a-2ff90121f41d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin
ms.openlocfilehash: d32c2fc82a8b09e89224ec437e5819b65b2e9e78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Zřídit a nasadit mikroslužeb předvídatelné v Azure
Tento kurz ukazuje, jak tooprovision a nasazení aplikace skládá z [mikroslužeb](https://en.wikipedia.org/wiki/Microservices) v [Azure App Service](/services/app-service/) jako na jednu jednotku a předvídatelný způsobem pomocí šablony pro skupiny prostředků JSON a Skriptů prostředí PowerShell. 

Při zřizování a nasazování aplikací špičkové, které se skládají z vysoce odpojeného mikroslužeb, opakovatelnost a předvídatelnost jsou zásadní toosuccess. [Aplikační služba Azure](/services/app-service/) vám umožní mikroslužeb toocreate, které zahrnují webové aplikace, mobilní aplikace, aplikace API a logic apps. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) umožňuje všechny hello mikroslužeb toomanage můžete jako jednotku, společně s závislosti prostředků, jako je například databáze a zdroj nastavení ovládacího prvku. Teď můžete nasadit taky takové aplikace pomocí šablony JSON a jednoduchý skriptů prostředí PowerShell. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Co provedete
V kurzu hello nasadíte aplikaci, která zahrnuje:

* Dva webové aplikace (tj. dva mikroslužeb)
* Back-end databáze SQL
* Nastavení aplikace, připojovací řetězce a Správa zdrojového kódu
* Application insights, výstrahy, nastavení automatického škálování

## <a name="tools-you-will-use"></a>Nástroje, které budete používat
V tomto kurzu použijete hello následující nástroje. Vzhledem k tomu, že není komplexní zabývat nástroje, I bude toostick toohello začátku do konce scénář a právě získáte tooeach stručný úvod a kde můžete najít další informace o jeho. 

### <a name="azure-resource-manager-templates-json"></a>Šablony Azure Resource Manageru (JSON)
Pokaždé, když vytvoříte webovou aplikaci v Azure App Service, například Azure Resource Manager používá JSON šablony toocreate hello celé skupiny prostředků s prostředky součást hello. Komplexní šablony z hello [Azure Marketplace](/marketplace) jako hello [škálovatelné WordPress](/marketplace/partners/wordpress/scalablewordpress/) aplikace mohou obsahovat hello databáze MySQL, účty úložiště, hello plán služby App Service, hello webové aplikace, pravidla výstrah, aplikace nastavení, nastavení automatického škálování a další a všechny tyto šablony jsou k dispozici tooyou pomocí prostředí PowerShell. Informace o tom, jak toodownload a použijte tyto šablony, najdete v části [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).

Další informace o šablonách Azure Resource Manager hello najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>2.6 Azure SDK pro Visual Studio
Hello nejnovější SDK obsahuje vylepšení toohello podpora šablony správce prostředků v editoru JSON hello. Můžete použít tento tooquickly vytvořit šablonu skupiny prostředků od začátku nebo otevřete stávající šablonu JSON (například šablonu stažené galerie) pro úpravy, naplnění souboru parametrů hello a i nasazení skupiny prostředků hello přímo z Azure Skupina prostředků řešení.

Další informace najdete v tématu [2.6 Azure SDK pro Visual Studio](https://azure.microsoft.com/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Prostředí Azure PowerShell 0.8.0 nebo novější
Počínaje verzí 0.8.0, zahrnuje hello prostředí Azure PowerShell instalace modulu Azure Resource Manager hello v toohello přidání modulu Azure. Tento nový modul umožňuje tooscript hello nasazení skupiny prostředků.

Další informace najdete v tématu [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Průzkumník prostředků Azure
To [nástroje preview](https://resources.azure.com) vám umožní tooexplore hello JSON definice všech hello skupin prostředků v předplatném a hello jednotlivé prostředky. V nástroji hello můžete upravit hello JSON definice prostředku, odstranit celou hierarchii prostředků a vytvořit nové prostředky.  informace o Hello snadno dostupné v tento nástroj je velmi užitečné při vytváření šablony, protože ho se dozvíte, co potřebujete tooset pro konkrétní typ prostředku, hello vlastnosti opravte hodnoty atd. Můžete například vytvořit vaší skupiny prostředků v hello [portálu Azure](https://portal.azure.com/), zkontrolujte jeho definice JSON v toohelp nástroj Průzkumník hello templatize hello skupinu prostředků.

### <a name="deploy-tooazure-button"></a>TooAzure tlačítko nasadit
Pokud používáte GitHub pro řízení zdrojů, které můžete vložit [nasadit tooAzure tlačítko](https://azure.microsoft.com/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) do vašeho souboru README. MD, která umožňuje tooAzure uživatelského rozhraní klíč nasazení. Zatímco můžete provést pro všechny jednoduché webové aplikace, můžete rozšířit tento tooenable nasazení umístěním souboru azuredeploy.json v úložišti kořenové hello celé skupiny prostředků. Tento soubor JSON, který obsahuje šablony skupiny prostředků hello, použije hello nasadit tooAzure tlačítko toocreate hello skupinu prostředků. Příklad najdete v tématu hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) vzorku, který budete používat v tomto kurzu.

## <a name="get-hello-sample-resource-group-template"></a>Získat šablony skupiny prostředků ukázka hello
Proto nyní Pojďme správné tooit.

1. Přejděte toohello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) Ukázka aplikace služby.
2. V readme.md, klikněte na **nasazení tooAzure**.
3. Jste prováděné toohello [nasadit do azure](https://deploy.azure.com) parametry kladené tooinput nasazení a lokality. Všimněte si, že většina polí hello se naplní hello úložiště název a některé náhodného řetězce pro vás. Všechna pole hello můžete změnit, pokud chcete, ale pouze věcí hello máte tooenter přihlášení správce systému SQL Server hello a hello heslo a pak klikněte na **Další**.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)
4. Klikněte na tlačítko **nasadit** procesu nasazení toostart hello. Jakmile hello proces běží toocompletion, klikněte na možnost hello http://todoapp*XXXX*. azurewebsites.net odkaz toobrowse hello nasazené aplikace. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)
   
   Hello uživatelského rozhraní může být trochu pomalé, kdy tooit nejprve procházet, protože se právě spuštění hello aplikace, ale přimět sami, že je plně funkční aplikaci.
5. Zpět na stránku hello nasadit, klikněte na hello **spravovat** odkaz toosee hello nové aplikace v hello portálu Azure.
6. V hello **Essentials** rozevíracího seznamu, klikněte na odkaz skupinu prostředků hello. Všimněte si také že hello webové aplikace je už připojené úložiště GitHub toohello pod **externí projektu**. 
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
7. V okně skupiny prostředků hello Všimněte si, že již existují dva webové aplikace a jedna databáze SQL ve skupině prostředků hello.
   
   ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)

Vše, co jste viděli pouze za pár minut krátké plně nasazené dva mikroslužbu aplikace, se všemi hello součásti, závislosti, nastavení, databáze a průběžné publikování nastavil automatizované orchestration ve službě Správce prostředků Azure. Všechny tomu bylo potřeba dvě věci:

* tlačítko tooAzure nasadit Hello
* azuredeploy.JSON v úložišti kořenové hello

Můžete nasadit stejná aplikace desítek, stovek nebo tisíců časy a přesně stejnou konfiguraci hello pokaždé, když máte. Hello opakovatelnost a hello předvídatelnost tento přístup umožňuje toodeploy špičkové aplikace s jednoduchosti a spolehlivosti.

## <a name="examine-or-edit-azuredeployjson"></a>Zkontrolujte (nebo upravit) AZUREDEPLOY. JSON
Nyní podíváme, jak bylo nastaveno úložiště GitHub hello. Budete používat hello JSON editor v hello .NET SDK služby Azure, takže pokud jste ještě nenainstalovali [Azure .NET SDK 2.6](/downloads/), udělejte teď.

1. Klon hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště pomocí vaše oblíbené git nástroje. Na snímku obrazovky hello níže to bude to v hello Průzkumník týmových projektů v sadě Visual Studio 2013.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)
2. Z kořenového úložiště hello otevřete v sadě Visual Studio azuredeploy.json. Pokud nevidíte podokně hello osnovou JSON, je třeba tooinstall Azure .NET SDK.
   
   ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

I mě není toodescribe má každý detail hello formátu JSON, ale hello [více prostředků](#resources) část obsahuje odkazy pro jazyk šablony skupiny prostředků hello učení. Zde právě kliknu tooshow hello zajímavé funkce, které vám pomůžou začít při vytvoření vlastní šablony pro nasazení aplikace.

### <a name="parameters"></a>Parametry
Podívejte se na hello parametry části toosee, většinu těchto parametrů jsou jaké hello **nasazení tooAzure** tlačítko vás vyzve tooinput. Hello lokality za hello **nasazení tooAzure** tlačítko naplní hello vstup uživatelského rozhraní pomocí hello parametry definované v azuredeploy.json. Tyto parametry se používají v definicích prostředků hello, například názvy prostředků, hodnoty vlastností atd.

### <a name="resources"></a>Zdroje
V uzlu hello prostředky uvidíte, že jsou definovány 4 nejvyšší úrovně prostředky, včetně instance systému SQL Server, plán služby App Service a dva webové aplikace. 

#### <a name="app-service-plan"></a>Plán služby App Service
Začněme jednoduché kořenové úrovni prostředku v hello JSON. V hello osnovou JSON, klikněte na plán služby App Service hello s názvem **[hostingPlanName]** toohighlight hello odpovídající kód JSON. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Všimněte si, že hello `type` element určuje hello řetězec pro plán služby App Service (byla volána v serverové farmě dlouhý, dlouhou dobou) a další elementy a vlastnosti jsou vyplněny pomocí hello parametry definované v souboru JSON hello a nemá tento prostředek všech vnořených prostředků.

> [!NOTE]
> Všimněte si také, že hodnota hello `apiVersion` informuje Azure, kterou verzi hello REST API toouse hello prostředků definici JSON s ale může mít vliv na způsob formátování hello prostředků uvnitř hello `{}`. 
> 
> 

#### <a name="sql-server"></a>SQL Server
Potom klikněte na prostředek hello systému SQL Server s názvem **SQLServer** v hello osnovy JSON.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)

Poznámka: hello následující o hello zvýrazněná kódu JSON:

* Hello použití parametrů zajišťuje, že jsou hello vytvořit prostředky s názvem a nakonfigurovat tak, že jsou konzistentní s navzájem.
* Hello SQLServer prostředků má dva vnořených prostředků, každá z nich má jinou hodnotu pro `type`.
* Hello vnořených prostředků v `“resources”: […]`, kde jsou definovány hello databáze a pravidel brány firewall hello, mají `dependsOn` element, který určuje ID prostředku hello hello kořenové úrovni SQLServer prostředku. Tato hodnota informuje Azure Resource Manager, "před vytvořením tohoto prostředku, který už musí existovat jiný prostředek; a pokud tento jiný prostředek je definován v šabloně hello, vytvořte než nejprve".
  
  > [!NOTE]
  > Podrobné informace o tom, toouse hello `resourceId()` funkce najdete v tématu [funkce šablon Azure Resource Manager](../azure-resource-manager/resource-group-template-functions-resource.md#resourceid).
  > 
  > 
* Hello účinku hello `dependsOn` element je, že Azure Resource Manager mohli zjistit prostředky, ke kterým může být vytvořen paralelně a prostředky, ke kterým musí být vytvořený postupně. 

#### <a name="web-app"></a>Webová aplikace
Teď umožňuje přesunout na toohello skutečné webové aplikace sami, které jsou složitější. Klikněte na tlačítko hello [variables('apiSiteName')] webové aplikace ve hello osnovy JSON toohighlight jeho kód JSON. Můžete si všimnout, že jsou získávání věcí mnohem víc zajímavé. Pro tento účel I mluvit o funkcích hello jeden po druhém:

##### <a name="root-resource"></a>Kořenové prostředků
webové aplikace Hello závisí na dvou různých prostředků. To znamená, že Azure Resource Manager bude vytvořit webovou aplikaci hello až po obou hello, které se vytvoří plán služby App Service a hello instance systému SQL Server.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Nastavení aplikace
nastavení aplikace Hello jsou také definovat jako vnořeného prostředku.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

V hello `properties` element pro `config/appsettings`, máte dvě nastavení aplikace ve formátu hello `“<name>” : “<value>”`.

* `PROJECT`je [KUDU nastavení](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) , která oznamuje nasazení Azure které toouse projektu v sadě Visual Studio řešení vícenásobného projektu. I vám ukáže, později zdrojového kódu konfiguraci, ale protože hello ToDoApp kódu je v sadě Visual Studio řešení vícenásobného projektu, je třeba, aby toto nastavení.
* `clientUrl`je jednoduše aplikace nastavení tohoto kódu aplikace hello používá.

##### <a name="connection-strings"></a>Připojovací řetězce
Hello připojovací řetězce, je definována jako vnořeného prostředku.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

V hello `properties` element pro `config/connectionstrings`, každý připojovací řetězec je také definován jako dvojice názvu a hodnoty hello formátu konkrétní `“<name>” : {“value”: “…”, “type”: “…”}`. Pro hello `type` elementu možné hodnoty jsou `MySql`, `SQLServer`, `SQLAzure`, a `Custom`.

> [!TIP]
> Pro konečné seznam typů řetězec připojení hello, spusťte následující příkaz v prostředí Azure PowerShell hello: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
> 
> 

##### <a name="source-control"></a>Správa zdrojového kódu
nastavení správy zdrojů Hello jsou také definovat jako vnořeného prostředku. Azure Resource Manager používá toto průběžné publikování prostředků tooconfigure (viz přímý přístup paměti na `IsManualIntegration` později) a také tookick vypnout hello nasazování kódu aplikace automaticky při zpracování souboru JSON hello hello.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`a `branch` by měl být poměrně intuitivní a by měla odkazovat Git toohello úložiště a hello název hello toopublish větev z. Opakujte tyto jsou definované vstupní parametry. 

Poznámka: v hello `dependsOn` element, který v toohello přidání webové aplikace prostředků, samostatně, `sourcecontrols/web` závisí také na `config/appsettings` a `config/connectionstrings`. Důvodem je, že po `sourcecontrols/web` je nakonfigurován, hello proces nasazení Azure se automaticky pokusí toodeploy, sestavit a spustit kód aplikace hello. Proto vkládání tuto závislost pomáhá byste se ujistit aplikace hello má nastavení přístupu toohello požadované aplikace a připojovacích řetězců před spuštěním kódu aplikace hello. 

> [!NOTE]
> Všimněte si také, že `IsManualIntegration` je nastaven příliš`true`. Tato vlastnost je nutné v tomto kurzu, protože ve skutečnosti nevlastníte hello úložiště GitHub a proto nelze udělit ve skutečnosti oprávnění tooAzure tooconfigure průběžné publikování z [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (tj. push automatické úložiště aktualizací tooAzure). Můžete použít výchozí hodnotu hello `false` pro zadané úložiště hello pouze v případě, že jste nakonfigurovali přihlašovací údaje hello vlastníka Githubu v hello [portál Azure](https://portal.azure.com/) před. Jinými slovy Pokud jste nastavili zdroj ovládacího prvku tooGitHub nebo BitBucket pro libovolnou aplikaci v hello [portálu Azure](https://portal.azure.com/) dříve, pomocí vaší uživatelské přihlašovací údaje, pak bude pamatovat přihlašovací údaje hello a je použít při každém nasazení žádné aplikace z Azure GitHub nebo BitBucket v budoucnu hello. Ale pokud jste to ještě neudělali, nasazení šablony JSON hello se nezdaří při Azure Resource Manager pokusí nastavení správy zdrojů tooconfigure hello webové aplikace, protože se nemůžete přihlásit do GitHub nebo BitBucket s přihlašovacími údaji úložiště vlastníka hello.
> 
> 

## <a name="compare-hello-json-template-with-deployed-resource-group"></a>Porovnání šablony JSON hello se skupinou nasazené prostředků
Zde můžete přejít pomocí okna všechny hello webové aplikace v hello [portálu Azure](https://portal.azure.com/), ale existuje jiný nástroj, který není stejně jako užitečné, pokud informace. Přejděte toohello [Průzkumníka prostředků Azure](https://resources.azure.com) preview nástroj, který poskytuje reprezentaci JSON všechny skupiny zdrojů hello v rámci vašich předplatných, které jsou ve skutečnosti v hello Azure back-end. Můžete také zjistit, jak hierarchie skupiny prostředků hello JSON v Azure odpovídá hello hierarchie v souboru hello šablony, který byl použit toocreate ho.

Například když přejít toohello [Průzkumníka prostředků Azure](https://resources.azure.com) nástroje a rozbalte uzly hello v Průzkumníku hello, zobrazují se skupina prostředků hello a hello kořenové úrovni prostředky, které byly shromážděny v jejich typy příslušných prostředků.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Pokud přejdete k podrobnostem tooa webové aplikace, měli byste mít možnost toosee webové aplikace konfigurace podrobnosti podobné toohello následující snímek obrazovky:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Znovu hello vnořených prostředků by měl mít velmi podobné toothose hierarchie v souboru šablony JSON a měli byste vidět nastavení aplikace hello připojovací řetězce, atd., správně odrážela v podokně hello JSON. nastavení v tomto poli chybí Hello může značit problém s souboru JSON a může pomoci při odstraňování souboru šablony JSON.

## <a name="deploy-hello-resource-group-template-yourself"></a>Nasazení šablony skupiny prostředků hello sami
Hello **nasazení tooAzure** tlačítko je skvělé, ale umožňuje vám šablony skupiny prostředků hello toodeploy v azuredeploy.json pouze v případě, že jste již nabídnutých azuredeploy.json tooGitHub. Hello Azure .NET SDK také poskytuje hello nástroje pro toodeploy jste žádné soubor šablony JSON přímo z vašeho místního počítače. toodo tento, postupujte podle kroků hello níže:

1. V sadě Visual Studio, klikněte na tlačítko **soubor** > **nový** > **projektu**.
2. Klikněte na tlačítko **Visual C#** > **cloudu** > **skupiny prostředků Azure**, pak klikněte na tlačítko **OK**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)
3. V **vybrat šablonu Azure**, vyberte **prázdné šablonu** a klikněte na tlačítko **OK**.
4. Přetáhněte azuredeploy.json do hello **šablony** složky nový projekt.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)
5. V Průzkumníku řešení otevřete azuredeploy.json hello zkopírovali.
6. Jenom pro hello zájmu ukázkový text hello, přidejme některé standardní informace o aplikaci prostředky tooour soubor JSON, kliknutím na **přidat prostředek**. Pokud vás zajímá jenom nasazení hello soubor JSON, přeskočte toohello kroky nasazení.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)
7. Vyberte **Application Insights pro webové aplikace**, pak se ujistěte, že je vybrána existující aplikaci plán a webové služby App Service a pak klikněte na tlačítko **přidat**.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)
   
   Nyní budete mít možnost toosee několik nových prostředků, že v závislosti na hello prostředků a jakým způsobem se mají závislosti na buď hello webové aplikace App Service plán nebo hello. Tyto prostředky nejsou povolené podle jejich stávající definice a budete toochange který.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
8. V hello osnovou JSON, klikněte na **appInsights škálování** toohighlight jeho kód JSON. Toto je hello škálování nastavení pro plán služby App Service.
9. V hello zvýrazněný kód JSON, vyhledejte hello `location` a `enabled` vlastnosti a nastavte je, jak je uvedeno níže.
   
   ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)
10. V hello osnovou JSON, klikněte na **CPUHigh appInsights** toohighlight jeho kód JSON. Toto je upozornění.
11. Vyhledejte hello `location` a `isEnabled` vlastnosti a nastavte je, jak je uvedeno níže. Hello stejné pro hello další tři výstrahy (fialové žárovky).
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)
12. Nyní jste toodeploy připraven. Klikněte pravým tlačítkem na projekt hello a vyberte **nasadit** > **nové nasazení**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)
13. Přihlaste se k účtu Azure, pokud jste tak již neučinili.
14. Vyberte existující skupinu prostředků ve vašem předplatném nebo vytvořte novou jeden, vyberte **azuredeploy.json**a potom klikněte na **upravit parametry**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)
    
    Nyní budete moct tooedit všechny parametry hello definované v souboru šablony hello dobrý tabulky. Parametry, které definují výchozí hodnoty budou již máte výchozí hodnoty a parametry, které definují seznam povolených hodnot se zobrazí jako rozevírací seznamy.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)
15. Zadejte všechny parametry prázdný hello a použít hello [adresu úložišti GitHub pro ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) v **repoUrl**. Potom klikněte na **Uložit**.
    
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)
    
    > [!NOTE]
    > Automatické škálování je funkce, které nabízí v **standardní** vrstvě nebo vyšší a plán úroveň výstrahy jsou funkce nenabízí **základní** úroveň, nebo vyšší, budete potřebovat tooset hello **sku** parametr příliš**standardní** nebo **Premium** v pořadí toosee všechny nové aplikace Insights prostředky světla nahoru.
    > 
    > 
16. Klikněte na tlačítko **nasazení**. Pokud jste vybrali **ukládat hesla**, hello heslo bude uloženo v souboru parametrů hello **ve formátu prostého textu**. Jinak zobrazí se výzva heslo k databázi hello tooinput během procesu nasazení hello.

A to je vše! Teď stačí toogo toohello [portálu Azure](https://portal.azure.com/) a hello [Průzkumníka prostředků Azure](https://resources.azure.com) nástroj toosee hello nové výstrahy a nastavení automatického škálování přidat tooyour JSON nasazené aplikace.

Vaše kroky v této části především udělat hello následující:

1. Soubor šablony připravené hello
2. Vytvoření parametr souboru toogo pomocí souboru šablony hello
3. Soubor šablony nasazené hello souborem parametr hello

poslední krok Hello je snadno provést pomocí rutiny prostředí PowerShell. toosee co Visual Studio se po jeho nasazení vaší Scripts\Deploy-AzureResourceGroup.ps1 aplikace, otevřete. Existuje mnoho kódu existuje, ale právě kliknu toohighlight všechny příslušné kód hello budete potřebovat soubor šablony hello toodeploy souborem parametr hello.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Hello poslední rutiny `New-AzureResourceGroup`, je hello ten, který ve skutečnosti provádí akce hello. To vše prokázat, že hello pomoci nástrojů, je relativně jednoduché toodeploy tooyou vaší cloudové aplikace předvídatelné. Pokaždé, když hello rutina se spouští na hello stejné šablony s hello stejný soubor parametrů, budete tooget hello stejného výsledku.

## <a name="summary"></a>Souhrn
V DevOps opakovatelnost a předvídatelnost jsou klíče tooany úspěšné nasazení velkého rozsahu aplikace skládá z mikroslužeb. V tomto kurzu jste nasadili dva mikroslužbu aplikace tooAzure jako jedna skupina prostředků pomocí šablony Azure Resource Manager hello. Doufáme, vám poskytla hello znalostní báze v pořadí toostart převodu šablonu aplikace v Azure a můžete zřídit a nasadit ji předvídatelné. 

## <a name="next-steps"></a>Další kroky
Zjistěte, jak příliš[použít agilní metod a průběžně snadno publikovat svoji aplikaci mikroslužeb](app-service-agile-software-development.md) a pokročilé techniky nasazení jako [flighting nasazení](app-service-web-test-in-production-controlled-test-flight.md) snadno.

<a name="resources"></a>

## <a name="more-resources"></a>Další zdroje informací
* [Jazyk šablony Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
* [Vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
* [Funkce šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-functions.md)
* [Nasazení aplikace pomocí šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md)
* [Použití Azure PowerShellu s Azure Resource Managerem](../azure-resource-manager/powershell-azure-resource-manager.md)
* [Řešení potíží s nasazením skupin prostředků v Azure](../azure-resource-manager/resource-manager-common-deployment-errors.md)

