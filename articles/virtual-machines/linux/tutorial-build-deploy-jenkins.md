---
title: "aaaCI/CD z volaných tooAzure virtuálních počítačů s Team Services | Microsoft Docs"
description: "Nastavte průběžnou integraci (CI) a průběžné nasazování (CD) aplikace Node.js pomocí virtuálních počítačů tooAzure volaných z správy verzí v aplikaci Visual Studio Team Services (VSTS) nebo Microsoft Team Foundation Server (TFS)"
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
ms.openlocfilehash: 400ae34cbdf45da65351811c0ff6ff5d61ef862c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-toolinux-vms-using-jenkins-and-team-services"></a>Nasazení vaší aplikace tooLinux virtuálních počítačů pomocí volaných a Team Services

Průběžnou integraci (CI) a průběžné nasazování (CD) je kanál, pomocí kterého můžete sestavit, verzi a nasadíte tak svůj kód. Team Services poskytuje úplný, plně funkční sadu nástrojů automatizace CI nebo CD pro tooAzure nasazení. Volaných je Oblíbené 3. stran CI/CD na serveru nástroj, který poskytuje také CI/CD automatizace. Můžete použít obě společně toocustomize jak poskytovat cloudové aplikace nebo služby.

V tomto kurzu použijete volaných toobuild **webové aplikace Node.js**a Visual Studio Team Services toodeploy ho tooa [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) obsahující virtuální počítače s Linuxem.

Vaším úkolem je:

> [!div class="checklist"]
> * Sestavení aplikace ve volaných
> * Konfigurace volaných pro integraci produktů Team Services
> * Vytvořit skupinu nasazení pro hello virtuální počítače Azure
> * Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů hello a nasadí aplikaci hello

## <a name="before-you-begin"></a>Než začnete

* Budete potřebovat účet pro přístup k tooa volaných. Pokud jste ještě nevytvořili volaných server, přečtěte si téma [volaných dokumentaci](https://jenkins.io/doc/). 

* Přihlaste se tooyour účtu Team Services (`https://{youraccount}.visualstudio.com`). 
  Můžete získat [bezplatný účet Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Další informace najdete v tématu [připojení služby tooTeam](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

* Pokud jste ještě nemáte, vytvořte osobní přístupový token (Jan) ve vašem účtu Team Services. Volaných vyžaduje tento tooaccess informace vašeho účtu Team Services.
  Čtení [vytvoření osobního přístupového tokenu pro Team Services a TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn jak toogenerate jeden.

## <a name="get-hello-sample-app"></a>Získat hello ukázkové aplikace

Budete potřebovat aplikaci toodeploy uložené v úložišti Git.
V tomto kurzu doporučujeme, abyste použili [této ukázkové aplikace, které jsou k dispozici z Githubu](https://github.com/azooinmyluggage/fabrikam-node).

1. Vytvoření větve tuto aplikaci a poznamenejte si umístění hello (URL) pro použití v dalších krocích tohoto kurzu.

1. Ujistěte se, hello rozvětvení **veřejné** toosimplify tooGitHub připojení později.

> [!NOTE]
> Další informace najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/) a [nastavíte privátní úložiště jako veřejné](https://help.github.com/articles/making-a-private-repository-public/).

> [!NOTE]
> aplikace Hello bylo vytvořeno prostřednictvím [Yeoman](http://yeoman.io/learning/index.html); používá **Express**, **bower**, a **grunt**; a má některé **npm**balíčky jako závislosti.
> Ukázková aplikace Hello obsahuje sadu [šablon Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) , jsou použité toodynamically vytváření hello virtuálních počítačů pro nasazení v Azure. Tyto šablony jsou používány úlohy v hello [Team Services verze definice](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).
> Hello hlavní šablona vytvoří skupinu zabezpečení sítě, virtuální počítač a virtuální sítě. Ho přiřadí veřejnou IP adresu a otevře příchozí port 80. Také přidá značku, který je používán nasazení hello tooreceive příliš vyberte hello hello nasazení skupiny počítačů.
>
> Ukázka Hello také obsahuje skript, který nastaví Nginx a nasadí aplikaci hello. Spuštění na každý z hello virtuálních počítačů. Konkrétně hello skript nainstaluje uzlu, Nginx a PM2; Nakonfiguruje Nginx a PM2; pak spustí aplikaci hello uzlu.

## <a name="configure-jenkins-plugins"></a>Konfigurace volaných moduly plug-in

Nejdřív musíte nakonfigurovat dvě volaných moduly plug-in pro **NodeJS** a **integraci se službou Team Services**.

1. Otevřete váš účet volaných a zvolte **spravovat volaných**.

1. V hello **spravovat volaných** vyberte **Správa modulů plug-in**.

1. Filtr hello seznamu toolocate hello **NodeJS** modulů plug-in a nainstalovat bez restartování.

   ![Přidání tooJenkins modulu plug-in hello NodeJS](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. Filtr hello seznamu toofind hello **Team Foundation Server** modulů plug-in a nainstalujte ji. (Tento modul plug-in funguje pro Team Services a serveru Team Foundation Server.) Restartování volaných není nutné.

## <a name="configure-jenkins-build-for-nodejs"></a>Konfigurace sestavení volaných pro Node.js

Volaných vytvořte nový projekt sestavení a nakonfigurovat následujícím způsobem:

1. V hello **Obecné** zadejte název pro sestavení projektu.

1. V hello **správu zdrojového kódu** vyberte **Git** a zadejte podrobnosti o hello hello úložiště a větve hello obsahující kódu aplikace.

   ![Přidání úložiště tooyour sestavení](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > Pokud úložiště není veřejný, zvolte **přidat** a zadejte přihlašovací údaje tooconnect tooit.

1. V hello **sestavení aktivační události** vyberte **dotazování SCM** a zadejte plán hello `H/03 * * * *` toopoll hello úložiště Git pro změny každé tři minuty. 

1. V hello **sestavení prostředí** vyberte **poskytují uzlu &amp; npm bin / složky cesta** a zadejte `NodeJS` pro hello hodnota instalace JS uzlu. Nechte **npmrc souboru** nastavena na "použít výchozí systémové nastavení."

1. V hello **sestavení** kartě, zadejte příkaz hello `npm install` tooensure jsou aktualizovány všechny závislosti.

## <a name="configure-jenkins-for-team-services-integration"></a>Konfigurace volaných pro integraci produktů Team Services

1. V hello **akce po sestavení** kartě pro **soubory tooarchive**, zadejte `**/*` tooinclude všechny soubory.

1. Pro **aktivovat verze v TFS/Team Services**, zadejte úplnou adresu URL hello účtu (například `https://your-account-name.visualstudio.com`), hello název pro definici verze hello (vytvořit později), jako název projektu a hello účet tooyour tooconnect přihlašovací údaje.
   Je třeba, uživatelské jméno a hello Jan jste vytvořili dříve. 

   ![Konfigurace akce volaných po sestavení](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. Uložte hello sestavení projektu.

## <a name="create-a-jenkins-service-endpoint"></a>Vytvoření koncového bodu služby volaných

Koncový bod služby umožňuje tooJenkins tooconnect Team Services.

1. Otevřete hello **služby** stránky v Team Services, otevřete hello **nový koncový bod služby** seznam a vyberte **volaných**.

   ![Přidání koncového bodu volaných](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. Zadejte název budete používat toorefer toothis připojení.

1. Zadejte adresu URL hello volaných serveru a osové hello **přijetí nedůvěryhodných certifikátů SSL** možnost.

1. Zadejte hello uživatelské jméno a heslo účtu volaných.

1. Zvolte **ověření připojení** toocheck, který hello informace je správná.

1. Zvolte **OK** koncový bod služby toocreate hello.

## <a name="create-a-deployment-group"></a>Vytvořte skupinu nasazení

Je nutné [skupiny nasazení](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) příliš obsahovat hello virtuálních počítačů.

1. Otevřete hello **verze** kartě hello **sestavení &amp; verze** rozbočovače a pak otevřete hello **nasazení skupiny** a klikněte na příkaz **+ nový**.

1. Zadejte název pro skupinu nasazení hello a volitelný popis.
   Zvolte **vytvořit**.

Úloha nasazení skupiny prostředků Azure Hello vytvoří a zaregistruje hello virtuální počítače, když je spuštěna pomocí šablony Azure Resource Manager hello.
Nebudete potřebovat toocreate a hello virtuální počítače zaregistrovat sami.

## <a name="create-a-release-definition"></a>Vytvoření definice verze

Verze definice určuje, že proces hello Team Services, budou spuštěny toodeploy hello aplikace.
toocreate hello verze definice v Team Services:

1. Otevřete hello **verze** kartě hello **sestavení &amp; verze** rozbočovače, otevřete hello  **+**  rozevírací seznam v seznamu hello Definice vydání a vyberte Hello **vytvořit verze definice**. 

1. Vyberte hello **prázdný** šablony a zvolte **Další**.

1. V hello **artefakty** části, klikněte na **odkaz artefakt** a zvolte **volaných**. Vyberte připojení volaných koncový bod služby. Potom vyberte hello volaných zdrojového projektu a zvolte **vytvořit**. 

1. V hello nové verze definice, zvolte **+ přidat úlohy** a přidejte **nasazení skupiny prostředků Azure** prostředí výchozí toohello úloh.

1. Zvolte hello rozevírací šipku další toohello **+ přidat úlohy** propojit a přidejte definici toohello skupiny fáze nasazení.

   ![Přidání skupiny fáze nasazení](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. V katalogu hello úloh, otevřete hello **nástroj** a přidejte instance hello **skript prostředí** úloh.

1. Hello parametry šablony použité v úloze nasazení skupiny prostředků Azure hello nastaví hello správce heslo použité tooconnect toohello virtuálních počítačů.
   Zadejte toto heslo se proměnná hello **$(adminpassword)**:
   
   - Otevřete hello **proměnné** kartě a v hello **proměnné** části, zadejte název hello `adminpassword`.

   - Zadejte heslo správce hello.

   - Zvolte hello "visacího zámku" ikonu další toohello hodnota textbox tooprotect hello heslo. 

## <a name="configure-hello-azure-resource-group-deployment-task"></a>Konfigurace úloh hello nasazení skupiny prostředků Azure

Hello **nasazení skupiny prostředků Azure** úloha je použité toocreate hello nasazení skupiny. Nakonfigurujte následujícím způsobem:

* **Předplatné:** vyberte připojení hello seznamu v části **dostupné připojení služby Azure**. 
  Pokud žádné připojení k dispozici, zvolte **spravovat**, vyberte **nový koncový bod služby** pak **Azure Resource Manager**a budete postupovat podle pokynů hello.
  Vrátí tooyour definice vydání, aktualizace hello **AzureRM předplatné** seznam a vyberte hello připojení, které jste vytvořili.

* **Skupina prostředků**: Zadejte název skupiny prostředků hello jste vytvořili dříve.

* **Umístění**: Vyberte oblast pro nasazení hello.

  ![Vytvoření nové skupiny prostředků](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* **Umístění šablon**:`URL of hello file`

* **Šablona odkaz**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`

* **Odkaz parametry šablony**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`

* **Přepsání parametry šablony**: seznam hello přepsat hodnoty, například: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.<br />Vložení vlastní konkrétní hodnoty pro hello {zástupné symboly}. 

* **Povolit požadavky**:`Configure with Deployment Group agent`

* **Koncový bod služby sady TFS nebo VSTS**: Zvolte **přidat** a v dialogu "Přidat nové Team Foundation Server/Team Services připojení" hello, vyberte **tokenu ověřování na základě**. Zadejte název připojení hello a adresu URL hello týmového projektu. Potom generovat a zadejte [Personal Access Token (Jan)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello připojení tooyour týmového projektu.

  ![Vytvořit osobní přístupový Token](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* **Týmový projekt**: Vyberte aktuálního projektu.

* **Nasazení skupiny**: Zadejte hello stejný název skupiny pro nasazení, jako jste použili pro hello **skupiny prostředků** parametr.

Hello výchozí nastavení pro úlohu hello nasazení skupiny prostředků Azure jsou toocreate nebo aktualizovat prostředek a toodo tak postupně. Hello úloh vytvoří virtuální počítače hello hello poprvé spustí a následně právě aktualizovat.

## <a name="configure-hello-shell-script-task"></a>Konfigurace úloh skript prostředí hello

Hello **skript prostředí** úloh je použité tooprovide hello konfigurace pro skript toorun na každý server tooinstall Node.js a spusťte hello aplikace. Nakonfigurujte následujícím způsobem:

* **Skript cesta**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`

* **Zadat pracovní adresář**:`Checked`

* **Pracovní adresář**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
   
## <a name="rename-and-save-hello-release-definition"></a>Přejmenovat a Uložit definici verze hello

1. Upravit hello název hello verze definice toohello jste zadali v **akce po sestavení** kartě hello sestavení v volaných. Volaných vyžaduje tento název toobe možné tootrigger novou verzi, když jsou aktualizovány hello zdroj artefakty.

1. Volitelně můžete změníte název hello hello prostředí kliknutím na název hello. 

1. Zvolte **Uložit**a zvolte **OK**.

## <a name="start-a-manual-deployment"></a>Spustit ruční nasazení

1. Zvolte **+ verze** a vyberte **vytvořit verzi**.

1. Vyberte hello sestavení jste dokončili v hello zvýrazněná rozevíracího seznamu a zvolte **vytvořit**.

1. Vyberte odkaz verze hello v hello místní zpráva. Například: "verze **verze 1** byla vytvořena."

1. Otevřete hello **protokoly** kartě výstup toowatch hello verze konzoly.

1. V prohlížeči otevřete adresu URL hello jednoho ze serverů hello jste přidali skupinu tooyour nasazení. Zadejte například`http://{your-server-ip-address}`

## <a name="start-a-cicd-deployment"></a>Spuštění nasazení CI/CD

1. V hello vydání definice a zrušte zaškrtnutí políčka hello **povoleno** zaškrtnout políčko hello **možnosti řízení** části hello nastavení pro úlohu hello nasazení skupiny prostředků Azure.
   Pro budoucí nasazení toohello existující nasazení skupiny, není nutné toore-provedení tohoto úkolu.

1. Přejděte úložiště Git toohello zdroje a upravovat obsah hello hello **h1** záhlaví v souboru hello [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).

1. Potvrďte změny.

1. Po několika minutách, zobrazí se nová verze vytvořené v hello **verze** Team Services nebo TFS. Otevřete hello verze toosee hello nasazení probíhající. Blahopřejeme!

## <a name="next-steps"></a>Další kroky

V tomto kurzu automatizované hello nasazení tooAzure aplikaci pomocí volaných sestavení a Team Services pro verzi. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Sestavení aplikace ve volaných
> * Konfigurace volaných pro integraci produktů Team Services
> * Vytvořit skupinu nasazení pro hello virtuální počítače Azure
> * Vytvořit definici verze, která slouží ke konfiguraci virtuálních počítačů hello a nasadí aplikaci hello

Posunutí toohello další kurz toolearn informace o tom, jak zásobníku toodeploy svítilna (Linux, Apache, MySQL a PHP,).

> [!div class="nextstepaction"]
> [Nasazení zásobníku LAMP](tutorial-lamp-stack.md)