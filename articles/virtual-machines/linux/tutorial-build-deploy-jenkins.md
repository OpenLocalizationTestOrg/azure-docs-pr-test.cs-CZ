---
title: "CI/CD z volaných pro virtuální počítače Azure se Team Services | Microsoft Docs"
description: "Nastavte průběžnou integraci (CI) a průběžné nasazování (CD) aplikace Node.js pomocí volaných k virtuálním počítačům Azure ze správy verzí v aplikaci Visual Studio Team Services (VSTS) nebo Microsoft Team Foundation Server (TFS)"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/15/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: a40e26a8681df31fad664e4d1df4c1513311900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-team-services"></a>Nasazení aplikace do virtuální počítače s Linuxem pomocí volaných a Team Services

Průběžnou integraci (CI) a průběžné nasazování (CD) je kanál, pomocí kterého můžete sestavit, verzi a nasadíte tak svůj kód. Team Services poskytuje úplný, plně funkční sadu nástrojů automatizace CI nebo CD pro nasazení do Azure. Volaných je Oblíbené 3. stran CI/CD na serveru nástroj, který poskytuje také CI/CD automatizace. Můžete i společně k přizpůsobení jak poskytovat cloudové aplikace nebo služby.

V tomto kurzu použijete k sestavení volaných **webové aplikace Node.js**a Visual Studio Team Services k jeho nasazení [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) obsahující virtuální počítače s Linuxem.

Vaším úkolem je:

> [!div class="checklist"]
> * Sestavení aplikace ve volaných
> * Konfigurace volaných pro integraci produktů Team Services
> * Vytvořit skupinu nasazení pro virtuální počítače Azure
> * Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů a nasadí aplikaci

## <a name="before-you-begin"></a>Než začnete

* Potřebujete přístup k účtu volaných. Pokud jste ještě nevytvořili volaných server, přečtěte si téma [volaných dokumentaci](https://jenkins.io/doc/). 

* Přihlaste se ke svému účtu Team Services (`https://{youraccount}.visualstudio.com`). 
  Můžete získat [bezplatný účet Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Další informace najdete v tématu [připojit k Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

* Pokud jste ještě nemáte, vytvořte osobní přístupový token (Jan) ve vašem účtu Team Services. Volaných vyžaduje zadání těchto informací pro přístup k účtu Team Services.
  Čtení [vytvoření osobního přístupového tokenu pro Team Services a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) se dozvíte, jak jej vygenerovat.

## <a name="get-the-sample-app"></a>Načíst ukázková aplikace

Je nutné aplikaci k nasazení uložené v úložišti Git.
V tomto kurzu doporučujeme, abyste použili [této ukázkové aplikace, které jsou k dispozici z Githubu](https://github.com/azooinmyluggage/fabrikam-node).

1. Vytvoření větve tuto aplikaci a poznamenejte si umístění (URL) pro použití v dalších krocích tohoto kurzu.

1. Ujistěte se, rozvětvení **veřejné** ke zjednodušení připojení ke Githubu později.

> [!NOTE]
> Další informace najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/) a [nastavíte privátní úložiště jako veřejné](https://help.github.com/articles/making-a-private-repository-public/).

> [!NOTE]
> Aplikace bylo vytvořeno prostřednictvím [Yeoman](http://yeoman.io/learning/index.html); používá **Express**, **bower**, a **grunt**; a má některé **npm** balíčky jako závislosti.
> Ukázková aplikace obsahuje sadu [šablon Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) používané dynamicky vytváření virtuálních počítačů pro nasazení v Azure. Tyto šablony jsou používány úlohy v [Team Services verze definice](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).
> Hlavní šablona vytvoří skupinu zabezpečení sítě, virtuální počítač a virtuální sítě. Ho přiřadí veřejnou IP adresu a otevře příchozí port 80. Také přidá značku, který je používán skupiny nasazení vybrat počítače pro nasazení obdrží.
>
> Ukázka také obsahuje skript, který nastaví Nginx a nasadí aplikaci. Spuštění na každý z virtuálních počítačů. Konkrétně skript nainstaluje uzlu, Nginx a PM2; Nakonfiguruje Nginx a PM2; pak spustí aplikaci uzlu.

## <a name="configure-jenkins-plugins"></a>Konfigurace volaných moduly plug-in

Nejdřív musíte nakonfigurovat dvě volaných moduly plug-in pro **NodeJS** a **integraci se službou Team Services**.

1. Otevřete váš účet volaných a zvolte **spravovat volaných**.

1. V **spravovat volaných** vyberte **Správa modulů plug-in**.

1. Filtrování seznamu a vyhledejte **NodeJS** modulů plug-in a nainstalovat bez restartování.

   ![Přidání modulu plug-in NodeJS do volaných](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. Filtrovat seznam a vyhledat **Team Foundation Server** modulů plug-in a nainstalujte ji. (Tento modul plug-in funguje pro Team Services a serveru Team Foundation Server.) Restartování volaných není nutné.

## <a name="configure-jenkins-build-for-nodejs"></a>Konfigurace sestavení volaných pro Node.js

Volaných vytvořte nový projekt sestavení a nakonfigurovat následujícím způsobem:

1. V **Obecné** zadejte název pro sestavení projektu.

1. V **správu zdrojového kódu** vyberte **Git** a zadejte podrobnosti o úložišti a větev obsahující kódu aplikace.

   ![Přidat úložišti do vaší sestavení](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > Pokud úložiště není veřejný, zvolte **přidat** a zadejte pověření pro připojení k němu.

1. V **sestavení aktivační události** vyberte **dotazování SCM** a zadejte plán `H/03 * * * *` k dotazování úložiště Git pro změny každé tři minuty. 

1. V **sestavení prostředí** vyberte **poskytují uzlu &amp; npm bin / složky cesta** a zadejte `NodeJS` pro hodnotu instalace JS uzlu. Nechte **npmrc souboru** nastavena na "použít výchozí systémové nastavení."

1. V **sestavení** kartě, zadejte příkaz `npm install` zajistit jsou aktualizovány všechny závislosti.

## <a name="configure-jenkins-for-team-services-integration"></a>Konfigurace volaných pro integraci produktů Team Services

1. V **akce po sestavení** kartě pro **soubory k archivaci**, zadejte `**/*` zahrnout všechny soubory.

1. Pro **aktivovat verze v TFS/Team Services**, zadejte úplnou adresu URL vašeho účtu (například `https://your-account-name.visualstudio.com`), název projektu, název pro definici verze (vytvořit později) a pověření pro připojení k vašemu účtu.
   Budete potřebovat vaše uživatelské jméno a Jana jste vytvořili dříve. 

   ![Konfigurace akce volaných po sestavení](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. Uložte sestavení projektu.

## <a name="create-a-jenkins-service-endpoint"></a>Vytvoření koncového bodu služby volaných

Koncový bod služby umožňuje Team Services pro připojení k volaných.

1. Otevřete **služby** stránky v Team Services, otevřete **nový koncový bod služby** seznam a vyberte **volaných**.

   ![Přidání koncového bodu volaných](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. Zadejte název, který budete používat k odkazování na toto připojení.

1. Zadejte adresu URL vašeho serveru volaných a značek **přijetí nedůvěryhodných certifikátů SSL** možnost.

1. Zadejte uživatelské jméno a heslo účtu volaných.

1. Zvolte **ověření připojení** zkontrolujte správnost informací.

1. Zvolte **OK** vytvořit koncový bod služby.

## <a name="create-a-deployment-group"></a>Vytvořte skupinu nasazení

Je nutné [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) tak, aby obsahovala virtuálních počítačů.

1. Otevřete **verze** kartě **sestavení &amp; verze** rozbočovače, otevřete **nasazení skupiny** a klikněte na příkaz **+ nový**.

1. Zadejte název skupiny nasazení a volitelný popis.
   Zvolte **vytvořit**.

Úloha nasazení skupiny prostředků Azure vytvoří a zaregistruje virtuální počítače, když je spuštěna pomocí šablony Azure Resource Manager.
Nemusíte vytvářet a registraci virtuálních počítačů.

## <a name="create-a-release-definition"></a>Vytvoření definice verze

Verze definice určuje, že proces Team Services, budou spuštěny při nasazení aplikace.
K vytvoření definice verze v Team Services:

1. Otevřete **verze** kartě **sestavení &amp; verze** rozbočovače, otevřete  **+**  rozevírací seznam v seznamu definice vydání a vyberte  **Vytvoření verze definice**. 

1. Vyberte **prázdný** šablony a zvolte **Další**.

1. V **artefakty** části, klikněte na **odkaz artefakt** a zvolte **volaných**. Vyberte připojení volaných koncový bod služby. Pak vyberte úlohu volaných zdroje a zvolte **vytvořit**. 

1. V nové verzi definice, zvolte **+ přidat úlohy** a přidejte **nasazení skupiny prostředků Azure** úloh do výchozí prostředí.

1. Vyberte šipku rozevíracího seznamu vedle položky **+ přidat úlohy** propojení a přidejte do definice skupiny fáze nasazení.

   ![Přidání skupiny fáze nasazení](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. V katalogu úloh, otevřete **nástroj** a přidejte instance **skript prostředí** úloh.

1. K šabloně parametry použité při nasazení skupiny prostředků Azure úloh nastavuje heslo správce používá k připojení k virtuálním počítačům.
   Zadejte toto heslo s proměnnou **$(adminpassword)**:
   
   - Otevřete **proměnné** kartě a v **proměnné** části, zadejte název `adminpassword`.

   - Zadejte heslo správce.

   - Zvolte ikonu "visacího zámku" vedle textové pole hodnoty k ochraně heslo. 

## <a name="configure-the-azure-resource-group-deployment-task"></a>Konfigurace úloh nasazení skupiny prostředků Azure

**Nasazení skupiny prostředků Azure** úloha se používá k vytvoření skupiny nasazení. Nakonfigurujte následujícím způsobem:

* **Předplatné:** vyberte připojení ze seznamu v části **dostupné připojení služby Azure**. 
  Pokud žádné připojení k dispozici, zvolte **spravovat**, vyberte **nový koncový bod služby** pak **Azure Resource Manager**a postupujte podle pokynů.
  Vrátit do vaší definice vydání, aktualizujte **AzureRM předplatné** seznam a vyberte připojení, které jste vytvořili.

* **Skupina prostředků**: Zadejte název skupiny prostředků, kterou jste vytvořili dříve.

* **Umístění**: Vyberte oblast pro nasazení.

  ![Vytvořit novou skupinu prostředků](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* **Umístění šablon**:`URL of the file`

* **Šablona odkaz**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`

* **Odkaz parametry šablony**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`

* **Přepsání parametry šablony**: seznam přepsání hodnoty, například: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.<br />Vložení vlastní konkrétní hodnoty pro {zástupné symboly}. 

* **Povolit požadavky**:`Configure with Deployment Group agent`

* **Koncový bod služby sady TFS nebo VSTS**: Zvolte **přidat** a v dialogovém okně "Přidat nové Team Foundation Server/Team Services připojení" vyberte **tokenu ověřování na základě**. Zadejte název připojení a adresu URL týmového projektu. Potom generovat a zadejte [Personal Access Token (Jan)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) k ověření připojení k vašemu týmovému projektu.

  ![Vytvořit osobní přístupový Token](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* **Týmový projekt**: Vyberte aktuálního projektu.

* **Nasazení skupiny**: Zadejte stejný název skupiny pro nasazení, jako jste použili při **skupiny prostředků** parametr.

Výchozí nastavení pro úlohu nasazení skupiny prostředků Azure jsou vytvořit nebo aktualizovat prostředek a to udělat postupně. Úloha vytvoří virtuální počítače poprvé se spustí a následně právě provede jejich aktualizaci.

## <a name="configure-the-shell-script-task"></a>Nakonfigurujte úlohu skript prostředí

**Skript prostředí** úloha slouží k poskytování konfigurace pro skript běžet na každém serveru nainstalovat Node.js a spusťte aplikaci. Nakonfigurujte následujícím způsobem:

* **Skript cesta**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`

* **Zadat pracovní adresář**:`Checked`

* **Pracovní adresář**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
   
## <a name="rename-and-save-the-release-definition"></a>Přejmenujte a uložte definici verze

1. Upravit název definice verze na název, který jste zadali v **akce po sestavení** kartě sestavení v volaných. Volaných vyžaduje tento název moct aktivovat novou verzi, když jsou aktualizovány zdrojové artefakty.

1. Volitelně můžete změníte název prostředí kliknutím na název. 

1. Zvolte **Uložit**a zvolte **OK**.

## <a name="start-a-manual-deployment"></a>Spustit ruční nasazení

1. Zvolte **+ verze** a vyberte **vytvořit verzi**.

1. Vyberte sestavení, můžete dokončit v zvýrazněná rozevíracího seznamu a zvolte **vytvořit**.

1. Vyberte odkaz verze v místní zpráva. Například: "verze **verze 1** byla vytvořena."

1. Otevřete **protokoly** a podívejte se na výstup konzoly verze.

1. V prohlížeči otevřete adresu URL jednoho ze serverů, které jste přidali do vaší skupiny nasazení. Zadejte například`http://{your-server-ip-address}`

## <a name="start-a-cicd-deployment"></a>Spuštění nasazení CI/CD

1. V definici verze, zrušte zaškrtnutí políčka **povoleno** zaškrtnout políčko **možnosti řízení** část nastavení pro úlohu nasazení skupiny prostředků Azure.
   Pro budoucí nasazení do existující skupiny nasazení není potřeba znovu spustit tuto úlohu.

1. Přejít na zdrojové úložiště Git a upravovat obsah **h1** záhlaví v souboru [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).

1. Potvrďte změny.

1. Po několika minutách, zobrazí se nová verze vytvořené v **verze** Team Services nebo TFS. Otevřete verzi zobrazíte probíhající nasazení. Blahopřejeme!

## <a name="next-steps"></a>Další kroky

V tomto kurzu automatické nasazení aplikace do Azure pomocí volaných sestavení a Team Services pro verzi. Jste se dozvěděli, jak na:

> [!div class="checklist"]
> * Sestavení aplikace ve volaných
> * Konfigurace volaných pro integraci produktů Team Services
> * Vytvořit skupinu nasazení pro virtuální počítače Azure
> * Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů a nasadí aplikaci

Přechod na další kurzu se dozvíte další informace o tom, jak nasadit SVÍTILNU zásobníku (Linux, Apache, MySQL a PHP,).

> [!div class="nextstepaction"]
> [Nasazení zásobníku LAMP](tutorial-lamp-stack.md)