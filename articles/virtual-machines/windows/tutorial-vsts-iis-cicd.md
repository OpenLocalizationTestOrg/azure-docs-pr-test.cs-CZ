---
title: "aaaCreate CI/CD kanál v Azure s Team Services | Microsoft Docs"
description: "Zjistěte, jak toocreate Visual Studio Team Services kanálu pro průběžnou integraci a doručení, která nasazuje tooIIS webové aplikace na virtuální počítač s Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b758a124c4742854dd3b543f747fd8700f954414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a>Vytvoření kanálu průběžnou integraci s Visual Studio Team Services a služby IIS
tooautomate hello sestavení, testování a fáze nasazení pro vývoj aplikací, můžete použít průběžnou integraci a nasazení (CI/CD) kanálu. V tomto kurzu vytvoříte kanál CI/CD pomocí Visual Studio Team Services a virtuálního počítače (VM) s Windows v Azure, který spouští IIS. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Publikování projektu Team Services tooa webové aplikace ASP.NET
> * Vytvořit definici sestavení, která se spustí při potvrzení kódu
> * Instalace a konfigurace služby IIS na virtuální počítač v Azure
> * Přidání skupiny nasazení tooa instance služby IIS hello v Team Services
> * Vytvoření nové webové verzi definice toopublish nasadit balíčky tooIIS
> * Test hello CI/CD kanálu

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit `Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="create-project-in-team-services"></a>Vytvoření projektu v Team Services
Visual Studio Team Services umožňuje snadno spolupráce a vývoj bez zachování do řešení pro správu kódu na místě. Team Services poskytuje testování kódu cloudu, sestavení a službu application insights. Můžete IDE, které nejlépe vyhovuje vaší vývoj kódu a verzi ovládací prvek úložiště. V tomto kurzu můžete použít bezplatný účet toocreate základní webové aplikace ASP.NET a CI/CD kanálu. Pokud jste již nemají účet Team Services [vytvořit](http://go.microsoft.com/fwlink/?LinkId=307137).

toomanage hello kód potvrzení procesu sestavení definice a verze definice, vytvoření projektu v Team Services následujícím způsobem:

1. Otevřete řídicí panel Team Services ve webovém prohlížeči a zvolte **nový projekt**.
2. Zadejte *myWebApp* pro hello **název projektu**. Nechte všechny ostatní výchozí hodnoty toouse *Git* verzí a *Agile* zpracování pracovní položky.
3. Vyberte možnost hello příliš**sdílet s** *členové týmu*, pak vyberte **vytvořit**.
5. Po vytvoření projektu, zvolte možnost hello příliš**inicializovat README nebo gitignore**, pak **inicializovat**.
6. Uvnitř nový projekt, vyberte **řídicí panely** napříč hello nahoře, pak vyberte **otevřete v sadě Visual Studio**.


## <a name="create-aspnet-web-application"></a>Vytvoření webové aplikace ASP.NET
V předchozím kroku hello jste vytvořili projektu v Team Services. posledním krokem Hello otevře nový projekt v sadě Visual Studio. Spravovat vaše potvrzení kódu v hello **Team Explorer** okno. Vytvořit místní kopii nový projekt, a poté vytvořit webovou aplikaci ASP.NET ze šablony takto:

1. Vyberte **klon** toocreate úložiště místní git projektu Team Services.
    
    ![Klonování úložiště z projektu Team Services](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. V části **řešení**, vyberte **nový**.

    ![Vytvoření řešení webové aplikace](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Vyberte **webové** šablony a potom zvolte hello **webové aplikace ASP.NET** šablony.
    1. Zadejte název vaší aplikace, jako například *myWebApp*a zrušte zaškrtnutí políčka hello **vytvořit adresář pro řešení**.
    2. Pokud je k dispozici možnost hello, zrušte zaškrtnutí políčka hello příliš**přidat službu Application Insights tooproject**. Application Insights vyžaduje jste tooauthorize webové aplikace pomocí služby Azure Application Insights. tookeep to jednoduché v tomto kurzu, tento proces přeskočit.
    3. Vyberte **OK**.
4. Zvolte **MVC** hello šablony seznamu.
    1. Vyberte **změna ověřování**, zvolte **bez ověřování**, pak vyberte **OK**.
    2. Vyberte **OK** toocreate řešení.
5. V hello **Team Explorer** okně zvolte **změny**.

    ![Potvrdit úložiště git služby tooTeam místní změny](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. Do textového pole hello potvrzení, zadejte zprávu *počáteční potvrzení*. Zvolte **potvrdit všechny a synchronizace** z rozevírací nabídky hello.


## <a name="create-build-definition"></a>Vytvořit definici sestavení
V Team Services použijte toooutline definice sestavení, jak by měly být vytvořeny vaší aplikace. V tomto kurzu vytvoříme základní definice, trvá sestavení řešení hello naše zdrojový kód, a potom vytvoří web nasazení balíčku používáme toorun hello webové aplikace na serveru IIS.

1. V projektu Team Services, zvolte **sestavení a verze** napříč hello nahoře, pak vyberte **sestavení**.
3. Vyberte **+ novou definici**.
4. Zvolte hello **ASP.NET (PREVIEW)** šablony a vyberte **použít**.
5. Ponechte výchozí všechny hello hodnoty úkolů. V části **získat zdroje**, ujistěte se, že hello *myWebApp* úložiště a *hlavní* jsou vybrat větev.

    ![Vytvořit definici sestavení v projektu Team Services](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. Na hello **aktivační události** kartě, posuvník hello pro **povolení této aktivační události** příliš*povoleno*.
7. Uložit definici sestavení hello a fronty nového sestavení tak, že vyberete **Uložit & fronty** , pak **Uložit & fronty** znovu. Ponechte výchozí hodnoty hello a vyberte možnost **fronty**.

Kukátko jako hello sestavení je naplánováno na hostovaného agenta, pak začne toobuild. Hello výstup je podobné toohello následující ukázka:

![Úspěšné sestavení projektu Team Services](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
tooprovide toorun platformy vaší webové aplikace ASP.NET, je nutné virtuálního počítače s Windows, který spouští IIS. Team Services používá toointeract agenta s instancí služby IIS hello potvrdíte nějaký kód a aktivaci sestavení.

Vytvoření virtuálního počítače Windows serveru 2016 pomocí [tento ukázkový skript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json). Ho trvá několik minut, než hello skriptu toorun a vytvořte hello virtuálních počítačů. Po vytvoření hello virtuálních počítačů, otevřete port 80 pro webový provoz s [přidat AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) následujícím způsobem:

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

tooconnect tooyour virtuálních počítačů, získat hello veřejnou IP adresu s [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) následujícím způsobem:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

Vytvoření relace vzdálené plochy tooyour virtuálních počítačů:

```cmd
mstsc /v:<publicIpAddress>
```

Na hello virtuálních počítačů, otevřete **Powershellu správce** příkazového řádku. Instalace IIS a požadované funkce .NET následujícím způsobem:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Vytvoření skupiny nasazení
toopush out hello webové nasadit server služby IIS toohello balíček, můžete definovat skupiny nasazení v Team Services. Tato skupina umožňuje toospecify serverů, které jsou cílem hello nových sestavení automaticky jako potvrzení tooTeam kódu služby a sestavení jsou dokončeny.

1. V Team Services, zvolte **sestavení a verze** a pak vyberte **nasazení skupiny**.
2. Zvolte **skupiny přidat nasazení**.
3. Například zadejte název pro skupinu hello *mojeiis*, pak vyberte **vytvořit**.
4. V hello **registraci počítačů** se ujistěte, *Windows* je vybrána, a zaškrtněte políčko hello příliš**používat osobní přístupový token v hello skriptu pro ověřování**.
5. Vyberte **zkopírujte skript tooclipboard**.


### <a name="add-iis-vm-toohello-deployment-group"></a>Přidat skupinu toohello nasazení virtuálních počítačů služby IIS
S hello nasazení skupina vytvořená přidejte každou skupinu toohello instance služby IIS. Team Services vytváří skript, který se stáhne a nakonfiguruje agenta na hello nasadit virtuální počítač, který obdrží nové webové balíčky pak použije ji tooIIS.

1. Zpět v hello **Powershellu správce** relaci na vašem virtuálním počítači, vložte a spusťte skript hello zkopírovaných z Team Services.
2. Když výzvami tooconfigure značky pro hello agenta, vyberte *Y* a zadejte *webové*.
3. Po zobrazení výzvy k hello uživatelský účet, stiskněte klávesu *vrátit* tooaccept hello výchozí hodnoty.
4. Počkejte hello skriptu toofinish zprávou *vstsagent.account.computername služby byla úspěšně spuštěna*.
5. V hello **nasazení skupiny** stránku hello **sestavení a verze** nabídky, otevřete hello *mojeiis* skupiny nasazení. Na hello **počítače** kartě, zkontrolujte, zda virtuální počítač.

    ![Virtuální počítač úspěšně přidal skupinu nasazení služby tooTeam](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a>Vytvoření definice verze
toopublish sestavení, můžete vytvořit definici verze v Team Services. Tuto definici se automaticky spustí podle úspěšném sestavení vaší aplikace. Zvolte hello nasazení skupiny toopush webového nasazení balíčku a zadejte odpovídající nastavení služby IIS hello.

1. Zvolte **sestavení a verze**, pak vyberte **sestavení**. Zvolte definici sestavení hello vytvořili v předchozím kroku.
2. V části **nedávném dokončení**, zvolte poslední sestavení hello a pak vyberte **verze**.
3. Zvolte **Ano** toocreate definici verze.
4. Zvolte hello **prázdný** šablony, vyberte **Další**.
5. Zkontrolujte definici sestavení projektu a zdroj hello se naplní projektu.
6. Vyberte hello **průběžné nasazování** zaškrtněte políčko a potom vyberte **vytvořit**.
7. Vyberte hello rozevíracího seznamu vedle příliš**+ přidat úlohy** a zvolte *přidat skupiny fáze nasazení*.
    
    ![Přidat úloha toorelease definice v Team Services](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Zvolte **přidat** další příliš**Deploy(Preview) aplikace webové služby IIS**, pak vyberte **Zavřít**.
9. Vyberte hello **spustit na skupiny nasazení** nadřazené úlohy.
    1. Pro **skupiny nasazení**, vyberte nasazení hello skupiny, kterou jste vytvořený dříve, jako třeba *mojeiis*.
    2. V hello **počítač značky** vyberte **přidat** a zvolte hello *webové* značky.
    
    ![Verze definice nasazení skupiny úloh pro službu IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Vyberte hello **nasadit: nasazení aplikace služby IIS webu** tooconfigure úloh vaší služby IIS instance nastavení následujícím způsobem:
    1. Pro **název webu**, zadejte *Default Web Site*.
    2. Ponechejte hello všechny ostatní výchozí nastavení.
12. Zvolte **Uložit**, pak vyberte **OK** dvakrát.


## <a name="create-a-release-and-publish"></a>Vytvoření vydání a publikování
Nyní můžete posouvat webového nasazení balíčku jako novou verzi. Tento krok komunikuje s agentem hello na každou instanci, která je součástí skupiny nasazení hello, nabízených oznámení hello webové nasazení balíčku a nakonfiguruje službu IIS toorun hello aktualizovat webovou aplikaci.

1. Ve vaší verzi definice, vyberte **+ verze**, zvolte **vytvořit verzi**.
2. Ověřte, zda je sestavení nejnovější hello vybrali v rozevíracím seznamu hello, spolu s **automatizované nasazení: Po vytvoření verze**. Vyberte **Vytvořit**.
3. Nápis informující o malé se zobrazí napříč hello horní části vaší definice vydání, jako například *vytvořil vydání verze 1*. Vyberte hello verze odkaz.
4. Otevřete hello **protokoly** kartě toowatch hello verze průběh.
    
    ![Úspěšné verze Team Services a webové nasadit balíček push](media/tutorial-vsts-iis-cicd/successful_release.png)

5. Po dokončení hello verze otevřete webový prohlížeč a zadejte hello veřejnou adresu RP vašeho virtuálního počítače. Je spuštěna webová aplikace ASP.NET.

    ![Webová aplikace ASP.NET spuštěná na virtuálním počítači služby IIS](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-hello-whole-cicd-pipeline"></a>Test hello celou CI/CD kanálu
S vaší webové aplikace běžící v IIS zkuste teď hello celou CI/CD kanálu. Po provedení změny v sadě Visual Studio a potvrzení spuštění kódu, sestavení, který potom aktivuje verzi aktualizované webového nasaďte balíček tooIIS:

1. V sadě Visual Studio otevřete hello **Průzkumníku řešení** okno.
2. Přejděte otevřete tooand *myWebApp | Zobrazení | Domů | Index.cshtml*
3. Upravte tooread řádek 6:

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. Uložte soubor hello.
5. Otevřete hello **Team Explorer** okno, vyberte hello *myWebApp* projektu, a potom vyberte **změny**.
6. Zadejte zprávu o potvrzení, jako například *testování CI/CD kanálu*, zvolte **potvrzení všechny a synchronizace** z rozevírací nabídky hello.
7. V pracovním prostoru Team Services aktivuje se z hello kód potvrzení nového sestavení. 
    - Zvolte **sestavení a verze**, pak vyberte **sestavení**. 
    - Vyberte svou definici sestavení a pak vyberte hello **zařazeno ve frontě & spuštěná** toowatch sestavení jako hello sestavení postupuje.
9. Po úspěšném sestavení hello se aktivuje novou verzi.
    - Zvolte **sestavení a verze**, pak **verze** nasazení webu hello toosee balíček nabídnutých tooyour virtuálních počítačů služby IIS. 
    - Vyberte hello **aktualizovat** ikonu tooupdate hello stavu. Když hello *prostředí* sloupci se zobrazuje zelená značka zaškrtnutí, verze hello úspěšně nasadil tooIIS.
11. toosee vaše změny použít, aktualizujte váš web služby IIS v prohlížeči.

    ![Spuštění virtuálního počítače služby IIS z CI/CD kanálu webové aplikace ASP.NET](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili webovou aplikaci ASP.NET v Team Services a nakonfigurované sestavení a verze definice toodeploy nové webové nasadit balíčky tooIIS na každém potvrzení kódu. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Publikování projektu Team Services tooa webové aplikace ASP.NET
> * Vytvořit definici sestavení, která se spustí při potvrzení kódu
> * Instalace a konfigurace služby IIS na virtuální počítač v Azure
> * Přidání skupiny nasazení tooa instance služby IIS hello v Team Services
> * Vytvoření nové webové verzi definice toopublish nasadit balíčky tooIIS
> * Test hello CI/CD kanálu

Jak zálohy další kurz toolearn toohello toosecure webového serveru pomocí certifikátů SSL.

> [!div class="nextstepaction"]
> [Zabezpečení webového serveru pomocí protokolu SSL](tutorial-secure-web-server.md)