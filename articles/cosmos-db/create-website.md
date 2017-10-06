---
title: "aaaDeploy webové aplikace pomocí šablony - Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak toodeploy účet Azure Cosmos DB, Azure App Service Web Apps a ukázku webové aplikace pomocí šablony Azure Resource Manager."
services: cosmos-db, app-service\web
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 087d8786-1155-42c7-924b-0eaba5a8b3e0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: b2bdde9279aad570606d7bf06dfc710f564b4d0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Nasazení databáze Cosmos Azure a Azure App Service Web Apps pomocí šablony Azure Resource Manager
Tento kurz ukazuje, jak toouse toodeploy šablony Azure Resource Manager a integrovat [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webové aplikace a ukázkové webové aplikaci.

Pomocí šablony Azure Resource Manager, můžete snadno automatizovat hello nasazení a konfigurace prostředků Azure.  Tento kurz ukazuje, jak toodeploy webové aplikace a automaticky konfigurovat informace o připojení účtu Azure Cosmos DB.

Po dokončení tohoto kurzu, budete moct tooanswer hello následující otázky:  

* Jak můžete používat toodeploy šablony Azure Resource Manager a integrovat Azure Cosmos DB účet a webové aplikace ve službě Azure App Service?
* Jak můžete používat toodeploy šablony Azure Resource Manager a integrovat Azure Cosmos DB účet a webové aplikace v App Service Web Apps a aplikaci Webdeploy?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Požadavky
> [!TIP]
> Při tomto kurzu nepředpokládá předchozí zkušenosti s šablon Azure Resource Manageru nebo JSON, by měl chcete toomodify hello odkazuje šablony nebo možnosti nasazení a pak se bude vyžadovat znalost každé z těchto oblastí.
> 
> 

Než budete postupovat hello pokyny v tomto kurzu, zajistěte, abyste měli hello následující:

* Předplatné Azure. Azure je platforma, na základě předplatného.  Další informace o získání předplatného najdete v tématu [možnostech nákupu](https://azure.microsoft.com/pricing/purchase-options/), [nabízí člen](https://azure.microsoft.com/pricing/member-offers/), nebo [bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>Krok 1: Stáhněte soubory šablony hello
Začněme tím stahování souborů hello šablony, které budeme používat v tomto kurzu.

1. Stáhnout hello [vytvoření účtu Azure Cosmos DB webové aplikace a nasazení aplikace demo-ukázka](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) šablony tooa místní složky (například C:\Azure Cosmos DBTemplates). Tato šablona nasadí účet Azure Cosmos DB, webové aplikace služby App Service a webové aplikace.  Kromě toho automaticky nakonfiguruje Azure Cosmos DB účet tooconnect toohello hello webových aplikací.
2. Stáhnout hello [vytvoření účtu Azure Cosmos DB a ukázkové webové aplikace](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) šablony tooa místní složky (například C:\Azure Cosmos DBTemplates). Tato šablona nasadí účtu Azure Cosmos DB, webové aplikace služby App Service a slouží k úpravě hello lokality aplikace nastavení tooeasily prostor Azure Cosmos DB informace o připojení, ale nezahrnuje webové aplikace.  

<a id="Build"></a>

## <a name="step-2-deploy-hello-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>Krok 2: Nasazení účtu Azure Cosmos DB hello, aplikace webové aplikace a ukázkové aplikace ukázka služby
Teď umožňuje nasadit naše první šablonu.

> [!TIP]
> Hello šablony není ověřte, zda jsou název webové aplikace hello a název účtu Azure Cosmos DB zadali níže a) platné a b) k dispozici.  Důrazně doporučujeme, abyste ověřili hello dostupnost hello názvy můžete naplánovat toosupply předchozí toosubmitting hello nasazení.
> 
> 

1. Přihlášení toohello [portálu Azure](https://portal.azure.com), klikněte na nový a vyhledejte řetězec "Nasazení šablony".
    ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment1.png)
2. Vyberte položku hello šablony nasazení a klikněte na **vytvořit** ![snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment2.png)
3. Klikněte na tlačítko **úpravy šablony**, vložte obsah hello hello DocDBWebsiteTodo.json šablony souboru a klikněte na tlačítko **Uložit**.
   ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment3.png)
4. Klikněte na tlačítko **upravit parametry**, zadejte hodnoty pro každé hello povinné parametry a klikněte na tlačítko **OK**.  Hello parametry jsou následující:
   
   1. Název webu: Určuje hello název webové aplikace služby App Service a použít tooconstruct hello adresa URL, kterou použijete tooaccess hello webové aplikace (například pokud zadáte "mydemodocdbwebapp", pak bude mít adresu URL hello, podle kterého budou přistupovat k hello webové aplikace mydemodocdbwebapp.azurewebsites.NET).
   2. HOSTINGPLANNAME: Určuje název hello hostování toocreate plán služby App Service.
   3. UMÍSTĚNÍ: Určuje hello umístění Azure, ve které toocreate hello Azure Cosmos DB a webové aplikace zdroje.
   4. DATABASEACCOUNTNAME: Určuje název hello toocreate účet Azure Cosmos DB hello.   
      
      ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment4.png)
5. Vyberte existující skupinu prostředků nebo zadejte název toomake novou skupinu prostředků a vyberte umístění pro skupinu prostředků hello.

    ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment5.png)
6. Klikněte na tlačítko **přečíst si právní podmínky**, **nákupu**a potom klikněte na **vytvořit** toobegin hello nasazení.  Vyberte **Pin toodashboard** hello výsledné nasazení je snadno zobrazovat na portálu Azure domovské stránky.
   ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment6.png)
7. Po dokončení nasazení hello, otevře se okno skupiny prostředků hello.
   ![Snímek obrazovky okna skupina prostředků hello](./media/create-website/TemplateDeployment7.png)  
8. toouse hello aplikace, jednoduše přejděte adresa URL webové aplikace toohello (v předchozím příkladu hello, hello by měla adresa URL http://mydemodocdbwebapp.azurewebsites.net).  Zobrazí se následující webové aplikace hello:
   
   ![Ukázková aplikace Todo](./media/create-website/image2.png)
9. Pokračujte a vytvořit několik úloh v hello webové aplikace a pak se vraťte toohello okně skupiny prostředků v hello portálu Azure. Klikněte na tlačítko hello Azure Cosmos DB účet prostředků v seznamu prostředků hello a pak klikněte na **Průzkumníka dotazů**.
    ![Snímek obrazovky hello souhrn objektivu se zvýrazněnou hello webové aplikace](./media/create-website/TemplateDeployment8.png)  
10. Spuštění dotazu výchozí hello, "vybrat * z c" a zkontrolovat výsledky hello.  Všimněte si, že takový dotaz hello má načíst hello reprezentace JSON, který jste vytvořili v kroku 7 výše uvedených položek todo hello.  Myslíte, že volné tooexperiment s dotazy; Například spusťte vybrat * z c WHERE c.isComplete = true tooreturn všechny položky todo, které byly označeny jako dokončené.
    
    ![Snímek obrazovky okna Průzkumníka dotazů a výsledky hello zobrazující hello výsledky dotazu](./media/create-website/image5.png)
11. Myslíte, že volné tooexplore hello práci s portálem Azure Cosmos DB nebo změnit aplikaci úkolů ukázkové hello.  Až budete připraveni, nyní nasadíme jinou šablonu.

<a id="Build"></a> 

## <a name="step-3-deploy-hello-document-account-and-web-app-sample"></a>Krok 3: Nasaďte hello dokumentu účet a ukázkové webové aplikace
Teď umožňuje nasadit naše druhou šablonu.  Tato šablona je užitečné tooshow jak je můžete vložit informace o připojení Azure Cosmos DB jako je koncový bod účtu a hlavní klíč do webové aplikace jako nastavení aplikace nebo jako vlastní připojovací řetězec. Například možná máte vlastní webové aplikace, že chcete vytvořit toodeploy s účtem Azure Cosmos DB a mít informace o připojení hello vyplní automaticky během nasazení.

> [!TIP]
> Hello šablony není ověřte, zda jsou název webové aplikace hello a název účtu Azure Cosmos DB zadali níže a) platné a b) k dispozici.  Důrazně doporučujeme, abyste ověřili hello dostupnost hello názvy můžete naplánovat toosupply předchozí toosubmitting hello nasazení.
> 
> 

1. V hello [portálu Azure](https://portal.azure.com), klikněte na nový a vyhledejte řetězec "Nasazení šablony".
    ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment1.png)
2. Vyberte položku hello šablony nasazení a klikněte na **vytvořit** ![snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment2.png)
3. Klikněte na tlačítko **úpravy šablony**, vložte obsah hello hello DocDBWebSite.json šablony souboru a klikněte na tlačítko **Uložit**.
   ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment3.png)
4. Klikněte na tlačítko **upravit parametry**, zadejte hodnoty pro každé hello povinné parametry a klikněte na tlačítko **OK**.  Hello parametry jsou následující:
   
   1. Název webu: Určuje hello název webové aplikace služby App Service a použít tooconstruct hello adresa URL, kterou použijete tooaccess hello webové aplikace (například pokud zadáte "mydemodocdbwebapp", pak bude mít adresu URL hello, podle kterého budou přistupovat k hello webové aplikace mydemodocdbwebapp.azurewebsites.NET).
   2. HOSTINGPLANNAME: Určuje název hello hostování toocreate plán služby App Service.
   3. UMÍSTĚNÍ: Určuje hello umístění Azure, ve které toocreate hello Azure Cosmos DB a webové aplikace zdroje.
   4. DATABASEACCOUNTNAME: Určuje název hello toocreate účet Azure Cosmos DB hello.   
      
      ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment4.png)
5. Vyberte existující skupinu prostředků nebo zadejte název toomake novou skupinu prostředků a vyberte umístění pro skupinu prostředků hello.

    ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment5.png)
6. Klikněte na tlačítko **přečíst si právní podmínky**, **nákupu**a potom klikněte na **vytvořit** toobegin hello nasazení.  Vyberte **Pin toodashboard** hello výsledné nasazení je snadno zobrazovat na portálu Azure domovské stránky.
   ![Snímek obrazovky nasazení šablony hello uživatelského rozhraní](./media/create-website/TemplateDeployment6.png)
7. Po dokončení nasazení hello, otevře se okno skupiny prostředků hello.
   ![Snímek obrazovky okna skupina prostředků hello](./media/create-website/TemplateDeployment7.png)  
8. Klikněte na prostředek hello webové aplikace v seznamu prostředků hello a pak klikněte na tlačítko **nastavení aplikace** ![– snímek obrazovky hello skupiny prostředků](./media/create-website/TemplateDeployment9.png)  
9. Všimněte si, jak jsou k dispozici pro koncový bod Azure Cosmos DB hello a každý z hello Azure Cosmos DB hlavní klíč nastavení aplikace.

    ![Snímek obrazovky nastavení aplikace](./media/create-website/TemplateDeployment10.png)  
10. Myslíte, že volné toocontinue zkoumat hello portálu Azure nebo použijte jednu z našich Azure DB Cosmos [ukázky](http://go.microsoft.com/fwlink/?LinkID=402386) toocreate aplikaci Azure Cosmos DB.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Další kroky
Blahopřejeme! Jste nasadili Azure Cosmos databáze, webové aplikace App Service a ukázkové webové aplikaci pomocí šablony Azure Resource Manager.

* Klikněte na tlačítko Další informace o Azure Cosmos DB, toolearn [zde](http://azure.com/docdb).
* Klikněte na tlačítko Další informace o Azure App Service Web apps, toolearn [zde](http://go.microsoft.com/fwlink/?LinkId=325362).
* Klikněte na tlačítko toolearn Další informace o šablonách Azure Resource Manageru, [zde](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)
* Průvodce toohello změnu hello starý portál toohello nového portálu najdete v tématu: [odkaz pro navigaci hello portálu Azure Classic](http://go.microsoft.com/fwlink/?LinkId=529715)

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](http://go.microsoft.com/fwlink/?LinkId=523751), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

