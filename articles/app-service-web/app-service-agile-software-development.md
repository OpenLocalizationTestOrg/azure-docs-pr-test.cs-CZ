---
title: "vývoj softwaru aaaAgile službou Azure App Service"
description: "Zjistěte, jak toocreate špičkové komplexních aplikací s Azure App Service způsobem, který podporuje agile software development."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: c0fdb676-36a6-4738-925f-65b4835d187f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: a1c1c78cfff711774943b0235ed762f03f48fc6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="agile-software-development-with-azure-app-service"></a>Agile software development službou Azure App Service
V tomto kurzu se dozvíte, jak toocreate špičkové komplexních aplikací s [Azure App Service](/azure/app-service/) způsobem, který podporuje [agile software development](https://en.wikipedia.org/wiki/Agile_software_development). Předpokládá, že již víte, jak příliš[nasazení složitých aplikací předvídatelné v Azure](app-service-deploy-complex-application-predictably.md).

Omezení v technických procesů může být mít často způsobem hello úspěšné implementace agilní metod. Aplikační služba Azure s funkcemi, jako [průběžné publikování](app-service-continuous-deployment.md), [přípravná prostředí](web-sites-staged-publishing.md) (sloty), a [monitorování](web-sites-monitor.md), když dobře spojených s hello orchestration a Správa nasazení v [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), můžou být součástí vynikající řešení pro vývojáře, kteří si osvojí agile software development.

Hello následující tabulce je seznam krátké požadavky související s agilní vývoje, a jak povolit službám Azure, každý z nich.

| Požadavek | Jak Azure umožňuje |
| --- | --- |
| -Sestavení s každou potvrzenou změnou<br>-Vytvořit automaticky a rychlými |Pokud konfigurován s průběžné nasazování, Azure App Service můžou fungovat jako live spuštění sestavení založené na dev větev. Pokaždé, když kód je nabídnutých toohello větev, je automaticky vytvořený a spuštěné za provozu v Azure. |
| – Zkontrolujte sestavení samoobslužné testování |Spouštění testů, webové testy, atd., můžete nasadit pomocí šablony Azure Resource Manager hello. |
| -Provedení testů v klonu produkčního prostředí |Šablony Azure Resource Manageru se dá použít toocreate klony hello Azure produkčního prostředí (včetně nastavení aplikace, připojovací řetězec šablony, škálování, atd.) pro testování rychle a předvídatelné. |
| -Zobrazení výsledků nejnovější sestavení snadno |Průběžné nasazování tooAzure z úložiště znamená, že můžete otestovat nový kód v za provozu aplikaci, ihned po potvrzení změn. |
| -Potvrdit hlavní větve toohello každý den<br>-Automatizovat nasazení |Průběžnou integraci produkční aplikace s hlavní větve v úložišti automaticky nasadí každých tooproduction potvrzení či sloučení toohello hlavní větve. |

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Co provedete
Vás provede typický pracovní postup dev zkušební fáze produkční v pořadí toopublish nové změny toohello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) ukázkovou aplikaci, která se skládá ze dvou [webové aplikace](/services/app-service/web/), jeden je front-end (FE) a Druhá je webové rozhraní API back-end (BE), Hello a [databáze SQL](/services/sql-database/). Budete pracovat s hello následující architektura nasazení:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Obrázek hello tooput na slova:

* Architektura Hello nasazení je rozdělené do tří různých prostředích (nebo [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) v Azure), každou s vlastním [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [škálování](web-sites-scale.md) nastavení a SQL database. 
* Každé prostředí lze spravovat samostatně. Mohou existovat i v různých předplatných.
* Pracovní a provozní jsou implementované jako dvě sloty hello stejná aplikace služby App Service. hlavní větve Hello je nastavený pro nepřetržitou integraci s hello pracovní pozici.
* Pokud větev toomaster potvrzení je ověřen na hello pracovní pozici (s použitím provozních dat), hello neověří pracovní aplikace je prohodily do produkčního slotu. hello [bez výpadků](web-sites-staged-publishing.md).

Hello produkční a pracovní prostředí je definován šablonou hello v [  *&lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Hello vývojářů a testovací prostředí jsou definovány šablonou hello v [  *&lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Také použijete hello typické větvení strategie, kódem přesouvání z větve dev hello až toohello testovací větve a pak hlavní větve toohello (přesunutí v kvality, takže toospeak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-need"></a>Co potřebujete
* Účet Azure
* A [Githubu](https://github.com/) účtu
* Git prostředí (nainstalované s [GitHub pro Windows](https://windows.github.com/)) – umožňuje vám toorun obě hello Git a prostředí PowerShell příkazů v hello stejné relace 
* Nejnovější [prostředí Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) bits
* Základní znalosti o hello následující nástroje:
  * [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nasazení šablony (viz také [nasazení komplexní aplikace předvídatelné v Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Je třeba účtu Azure toocomplete v tomto kurzu:
> 
> * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít tootry na placené služby Azure a i po jejich použití můžete ponechat hello účet a používat bezplatné služby Azure, jako jsou webové aplikace.
> * Můžete [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -si Visual Studio předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.
> 
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="set-up-your-production-environment"></a>Nastavit produkční prostředí
> [!NOTE]
> skript Hello použili v tomto kurzu se automaticky nakonfiguruje průběžné publikování z úložiště Githubu. To vyžaduje, aby vaše přihlašovací údaje Githubu jsou již uloženy v Azure, v opačném případě hello skripty nasazení nezdaří, při pokusu o nastavení ovládacího prvku tooconfigure zdroje pro hello webové aplikace. 
> 
> toostore vaše Githubu přihlašovací údaje v Azure, vytvořit webovou aplikaci v hello [portál Azure](https://portal.azure.com/) a [konfigurace nasazení Githubu](app-service-continuous-deployment.md). Potřebujete jenom toodo tomto jednou. 
> 
> 

V případě typické DevOps máte aplikaci, která běží v Azure za provozu a chcete toomake tooit změny prostřednictvím průběžné publikování. V tomto scénáři máte šablonu můžete vyvinuté, otestovat a použít toodeploy hello produkční prostředí. Nastavíte ho v této části.

1. Vytvoření vlastního rozvětvení hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště. Informace o vytváření vašeho rozvětvení najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/). Po vytvoření vaší rozvětvení, zobrazí se v prohlížeči.
   
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Otevřete relaci prostředí Git. Pokud ještě nemáte Git prostředí, nainstalujte [GitHub pro Windows](https://windows.github.com/) nyní.
3. Vytvořte místní vaší rozvětvení spuštěním hello následující příkaz:

        git clone https://github.com/<your_fork>/ToDoApp.git 
4. Až budete mít vaše místní klonování, přejděte příliš*&lt;repository_root >*\ARMTemplates a deploy.ps1 hello spusťte skript následujícím způsobem:
   
        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git
5. Po zobrazení výzvy zadejte hello požadované uživatelské jméno a heslo pro přístup k databázi.
   
   Měli byste vidět hello zřizování průběh různých prostředků Azure. Po dokončení nasazení hello skript spustí hello aplikace v prohlížeči hello a získáte popisný zvukový signál.
   
    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
   
   > [!TIP]
   > Podívejte se na  *&lt;repository_root >*\ARMTemplates\Deploy.ps1, toosee jak vygeneruje prostředky s jedinečné identifikátory. Můžete použít stejný postup toocreate provede klonování z hello hello stejného nasazení bez obav o konfliktní názvy prostředků.
   > 
   > 
6. Zpět v relaci prostředí Git, spusťte tento příkaz:
   
        .\swap –Name ToDoApp<unique_string>master
   
    ![](./media/app-service-agile-software-development/production-4-swap.png)
7. Po dokončení skriptu hello vraťte toobrowse toohello front-end adresa (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) toosee hello aplikace běžící v produkčním prostředí.
8. Přihlaste se toohello [portál Azure](https://portal.azure.com/) a prohlédněte si co je vytvořen.
   
   Musí být schopný toosee dva webové aplikace v hello stejnou skupinu prostředků, s hello `Api` přípony v názvu hello. Pokud se podíváte na zobrazení skupiny prostředků hello, uvidíte také hello SQL Database a serveru, hello plán služby App Service a hello přípravné sloty pro hello webové aplikace. Procházet hello různých prostředků a porovnejte je s  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee, jak jsou nakonfigurované v šabloně hello.
   
    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Nyní jste nastavili hello produkčního prostředí. V dalším kroku se ji novou aplikaci toohello aktualizace.

## <a name="create-dev-and-test-branches"></a>Vytvoření vývoj a testování větví
Teď, když máte komplexní aplikace spuštěna v produkčním prostředí v Azure, bude aplikace tooyour aktualizace v souladu s agile metody. V této části vytvoříte hello vývoje a testování větví, budete potřebovat toomake hello požadované aktualizace.

1. Nejprve vytvořte hello testovací prostředí. V relaci prostředí Git hello spusťte následující příkazy prostředí hello toocreate pro nové větve názvem **NewUpdate**. 
   
        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate
2. Po zobrazení výzvy zadejte hello požadované uživatelské jméno a heslo pro přístup k databázi. 
   
   Po dokončení nasazení hello skript spustí hello aplikace v prohlížeči hello a získáte popisný zvukový signál. Nyní máte nové větve s vlastním testovacím prostředí. Tooreview chvíli trvat pár věcí o toto testovací prostředí:
   
   * Můžete ho vytvořit v jakéhokoli předplatného Azure. To znamená, že hello provozním prostředí lze spravovat nezávisle z testovacího prostředí.
   * Testovací prostředí je spuštěna za provozu v Azure.
   * Testovací prostředí je identické toohello provozním prostředí, s výjimkou hello přípravné sloty a hello škálování nastavení. Vy víte, že vzhledem k tomu, že jsou hello pouze rozdíly mezi ProdandStage.json a Dev.json.
   * Testovacího prostředí můžete spravovat svůj vlastní plán služby App Service, s vrstvou, jinou cenu (například **volné**).
   * Odstranění tohoto testovacího prostředí je stejně jednoduché jako odstranění skupiny prostředků hello. Zjistíte, jak toodo to [později](#delete).
3. Přejdete na toocreate hello větev dev spuštěním následujících příkazů:
   
        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev
4. Po zobrazení výzvy zadejte hello požadované uživatelské jméno a heslo pro přístup k databázi. 
   
   Tooreview chvíli trvat pár věcí o tomto prostředí dev: 
   
   * Vývojová prostředí má konfigurace identické toohello testovacím prostředí, protože je nasazena pomocí hello stejné šablony.
   * Vaše vývojová prostředí lze vytvořit v hello vývojáře vlastní předplatné Azure, ponechat hello testovací prostředí toobe samostatně spravovat.
   * Vaše vývojová prostředí je spuštěna za provozu v Azure.
   * Odstranění hello vývojářů je stejně jednoduché jako odstranění skupiny prostředků hello prostředí. Zjistíte, jak toodo to [později](#delete).

> [!NOTE]
> Pokud máte více vývojáře, kteří pracují na novou aktualizaci hello, každý z nich můžete snadno vytvořit větve a vyhrazené vývojová prostředí s hello následující kroky:
> 
> 1. Vytvořit vlastní rozvětvení hello úložiště na webu GitHub (najdete v části [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/)).
> 2. Klonování hello rozvětvení v jejich místním počítači
> 3. Spustit hello stejné příkazy toocreate své vlastní firemní pobočky vývojářů a prostředí.
> 
> 

Když jste hotovi, vaše Githubu rozvětvení musí mít tři větve:

![](./media/app-service-agile-software-development/test-1-github-view.png)

A v tři samostatné skupiny prostředků byste měli mít šest webové aplikace (tři sady dva):

![](./media/app-service-agile-software-development/test-2-all-webapps.png)

> [!NOTE]
> ProdandStage.json určuje hello produkční prostředí toouse hello **standardní** cenové úrovně, který je vhodný pro škálovatelnost hello produkční aplikace.
> 
> 

## <a name="build-and-test-every-commit"></a>Sestavení a testů každou potvrzenou změnou
Hello soubory šablony ProdAndStage.json a Dev.json již zadat parametry řízení hello zdroj, který ve výchozím nastavení nastavuje průběžné publikování pro hello webovou aplikaci. Proto každé větve Githubu toohello potvrzení aktivuje tooAzure automatického nasazení z uvedené pobočky. Podívejme se nyní fungování vašeho nastavení.

1. Ujistěte se, že jste v hello Dev větev hello místní úložiště. toodo se spuštění hello následující příkaz v prostředí Git:
   
        git checkout Dev
2. Změňte vrstvu uživatelského rozhraní aplikace toohello změnit změnou hello kódu toouse [Bootstrap](http://getbootstrap.com/components/) uvádí. Otevřete  *&lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml a zkontrolujte hello následující zvýrazněný změn:
   
    ![](./media/app-service-agile-software-development/commit-1-changes.png)
   
    > [!NOTE]
    > Pokud nelze přečíst hello předchozí obrázek: 
    > 
    > * V řádku 18 změnit `check-list` příliš`list-group`.
    > * V řádku 19, změňte `class="check-list-item"` příliš`class="list-group-item"`.
    > 
    > 
3. Uložte změnu hello. Zpět v prostředí Git, spusťte následující příkazy hello:
   
        cd <repository_root>
        git add .
        git commit -m "changed toobootstrap style"
        git push origin Dev
   
   Tyto příkazy gitu jsou podobné příliš "kontrola ve vašem kódu" v jiném systému správy zdrojů jako sady TFS. Při spuštění `git push`, nové potvrzení hello aktivuje tooAzure nabízené automatické kód, který pak znovu sestaví hello změna hello tooreflect aplikace v prostředí dev hello.
4. tooverify, který tento kód nabízené tooyour vývojová prostředí došlo k chybě, přejděte tooyour vývojová prostředí webové aplikace stránky a podívejte se na hello **nasazení** část. Musí být schopný toosee vaše nejnovější potvrzení zprávy existuje.
   
    ![](./media/app-service-agile-software-development/commit-2-deployed.png)
5. Zde, klikněte na tlačítko **Procházet** toosee hello nové změny v hello živé aplikace v Azure.
   
    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)
   
   Jde o méně zásadní změna toohello aplikaci. Ale mnoho časy nové změny tooa komplexních webových aplikací mít nezamýšleným a nežádoucí vedlejší účinky. Je možné tooeasily testovací každou potvrzenou změnou v za provozu sestavení umožňuje toocatch můžete tyto problémy před zobrazovat vašich zákazníků.

Nyní by by měla být celý hello realizace, která jako vývojář hello **NewUpdate** projektu, můžete vytvořit prostředí dev sami, pak sestavení každou potvrzenou změnou a otestovat každé sestavení.

## <a name="merge-code-into-test-environment"></a>Sloučení kódu do testovacího prostředí
Když jste připravené toopush kódu z vývojářů větev až tooNewUpdate větve, je hello standardní git proces:

1. Sloučit všechny nové tooNewUpdate potvrzení do hello Dev větve na Githubu, jako je například potvrzení vytvořené jinými vývojáři. Všechny nové potvrzení na Githubu aktivuje kódu a sestavení v hello vývojová prostředí. Pak zajistit, že kód do větve Dev pořád funguje s nejnovější bity hello z NewUpdate větve.
2. Sloučit všechny nové potvrzení z vývojářů větve do NewUpdate větve na Githubu. Tato akce aktivuje kódu a sestavení v testovacím prostředí hello. 

Mějte na paměti znovu, protože se tyto větví git je již nastavena průběžné nasazování, nepotřebujete tootake, které jiných akcí, jako je spuštění integrace sestavení. Potřebujete tooperform standardní zdroj ovládacího prvku postupy pomocí git a Azure provádí všechny procesy sestavení hello za vás.

Nyní Pojďme příliš push kódu**NewUpdate** firemní pobočky. Spusťte v prostředí Git hello následující příkazy:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

A to je vše! 

Přejděte toohello webové aplikace stránky pro vaše testovací prostředí toosee vaší nové potvrzení (sloučí NewUpdate větve) teď nabídnutých toohello testovací prostředí. Potom klikněte na **Procházet** toosee, který hello Změna stylu je nyní spuštěna za provozu v Azure.

## <a name="deploy-update-tooproduction"></a>Nasazení aktualizace tooproduction
Vkládání toohello kód by měl pracovní a provozní prostředí zaregistrované nejsou jiné než co již bylo provedeno při poslat kód toohello testovací prostředí. Je skutečně tak jednoduché. 

Spusťte v prostředí Git hello následující příkazy:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Nezapomeňte, že podle hello způsob hello pracovní a provozní prostředí se nastaví ve ProdandStage.json, váš nový kód je nabídnutých toohello **pracovní** pozice a běží existuje. Pokud přejdete toohello přípravný slot adresu URL, takže zobrazí hello nový kód pro spuštění existuje. toodo se spuštění hello následující rutiny v prostředí Git.

    Start-Process -FilePath "http://ToDoApp<unique_string>master-Staging.azurewebsites.net"

A teď po ověříte hello aktualizace v hello pracovní pozici, hello pouze zopakuji toodo je tooswap ho do produkčního prostředí. V prostředí Git Shell, stačí spustit hello následující příkazy:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Blahopřejeme! Úspěšně jste publikovali nové aktualizace tooyour produkční webové aplikace. Je týmovými, který se snadno vytvořením prostředí vývoje a testování a vytváření a testování každou potvrzenou změnou. Jedná se o velmi důležitý stavebních bloků agile software development.

<a name="delete"></a>

## <a name="delete-dev-and-test-environments"></a>Odstraňte vývojářů a testovací prostředí
Vzhledem k tomu účelově vytvořili po vaší vývojářů a testovacích prostředí toobe nezávislý prostředek skupin, je snadné toodelete je. hello toodelete ty, které jste vytvořili v tomto kurzu hello Githubu větve a Azure artefaktů, stačí spustit následující příkazy v prostředí Git hello:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Souhrn
Agile software development je musí obsahovat pro mnoho společností, kteří chtějí tooadopt jako jejich platformu aplikací Azure. V tomto kurzu jste se naučili jak toocreate a úplné dolů přesný repliky nebo téměř repliky hello snadno i pro komplexní aplikace produkčního prostředí. Naučili jste se jak tooleverage tuto možnost toocreate vývojovém zpracovat, můžete sestavit a otestovat každých jediné potvrzení v Azure. V tomto kurzu zpravidla uvádí, jak nejlépe můžete Azure App Service a Azure Resource Manager společně toocreate řešení DevOps, který je určen tooagile metod. Potom můžete vytvořit v tomto scénáři provedením pokročilé techniky DevOps, jako [testování v produkčním prostředí](app-service-web-test-in-production-get-start.md). Běžné scénáře testování v produkčním, najdete v části [Flighting nasazení (testování verze beta) v Azure App Service](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Další zdroje informací
* [Nasazení aplikace komplexní předvídatelné v Azure](app-service-deploy-complex-application-predictably.md)
* [Agilní vývoj v praxi: tipy a triky pro Modernized vývoj cyklu](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
* [Pokročilé nasazení strategie pro webové aplikace Azure pomocí šablony Resource Manageru](http://channel9.msdn.com/Events/Build/2015/2-620)
* [Vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - hello validátoru JSON](http://jsonlint.com/)
* [ARMClient – nastavení publikování toosite Githubu](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
* [Vytvoření větve Git – základní větvení a slučování](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Blog David Ebbo](http://blog.davidebbo.com/)
* [Azure PowerShell](/powershell/azure/overview)
* [Nástroje příkazového řádku Azure a platformy](../cli-install-nodejs.md)
* [Vytvoření nebo úprava uživatelů ve službě Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
* [Wiki Kudu projektu](https://github.com/projectkudu/kudu/wiki)

