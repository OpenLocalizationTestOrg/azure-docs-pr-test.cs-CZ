---
title: "Agile software development službou Azure App Service"
description: "Informace o vytváření komplexních aplikací špičkové službou Azure App Service způsobem, který podporuje agile software development."
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
ms.openlocfilehash: 5ed888cbb422766cf2094f5980dfd1c599bd431c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="agile-software-development-with-azure-app-service"></a>Agile software development službou Azure App Service
V tomto kurzu se dozvíte, jak k vytvoření složitých aplikací špičkové s [Azure App Service](/azure/app-service/) způsobem, který podporuje [agile software development](https://en.wikipedia.org/wiki/Agile_software_development). Předpokládá, že již víte, jak [nasazení složitých aplikací předvídatelné v Azure](app-service-deploy-complex-application-predictably.md).

Omezení v technických procesů můžete často bránit úspěšné dokončení implementace agilní metod. Aplikační služba Azure s funkcemi, jako [průběžné publikování](app-service-continuous-deployment.md), [přípravná prostředí](web-sites-staged-publishing.md) (sloty), a [monitorování](web-sites-monitor.md), když dobře spojených s orchestration a správu nasazení v [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md), můžou být součástí vynikající řešení pro vývojáře, kteří si osvojí agile software development.

V následující tabulce je seznam krátké požadavky související s agilní vývoje, a jak povolit službám Azure, každý z nich.

| Požadavek | Jak Azure umožňuje |
| --- | --- |
| -Sestavení s každou potvrzenou změnou<br>-Vytvořit automaticky a rychlými |Pokud konfigurován s průběžné nasazování, Azure App Service můžou fungovat jako live spuštění sestavení založené na dev větev. Pokaždé, když kód vložena do větev, je automaticky vytvořený a spuštění za provozu v Azure. |
| – Zkontrolujte sestavení samoobslužné testování |Spouštění testů, webové testy, atd., můžete nasadit pomocí šablony Azure Resource Manager. |
| -Provedení testů v klonu produkčního prostředí |Šablony Azure Resource Manager umožňuje vytvářet klony Azure produkčního prostředí (včetně nastavení aplikace, připojovací řetězec šablony, škálování, atd.) pro testování rychle a předvídatelné. |
| -Zobrazení výsledků nejnovější sestavení snadno |Průběžné nasazování do Azure z úložiště znamená, že můžete otestovat nový kód v za provozu aplikaci, ihned po potvrzení změn. |
| -Potvrdit do hlavní větve každý den<br>-Automatizovat nasazení |Každý potvrzení sloučení průběžnou integraci produkční aplikace s hlavní větve v úložišti automaticky nasadí do hlavní větve do produkčního prostředí. |

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Co provedete
Vás provede typický pracovní postup dev zkušební fáze produkční Chcete-li publikovat nové změny [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) ukázkovou aplikaci, která se skládá ze dvou [webové aplikace](/services/app-service/web/), jeden se front-endu (FE) a druhá je webové rozhraní API back-end (BE) a [databáze SQL](/services/sql-database/). Budete pracovat s architekturou následující nasazení:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Chcete-li obrázek umístit do slova:

* Architektura nasazení je rozdělené do tří různých prostředích (nebo [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) v Azure), každou s vlastním [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [škálování](web-sites-scale.md) nastavení a databáze SQL. 
* Každé prostředí lze spravovat samostatně. Mohou existovat i v různých předplatných.
* Pracovní a provozní jsou implementované jako dvě sloty stejná aplikace služby App Service. Hlavní větve je nastavený pro nepřetržitou integraci s přípravný slot.
* Při zápisu do hlavní větve je ověřen na přípravný slot (s použitím provozních dat), je vzájemně zaměněny ověřené pracovní aplikace na produkční slot [bez výpadků](web-sites-staged-publishing.md).

Produkční a pracovní prostředí je definována v šabloně [  *&lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

Prostředí vývoje a testování jsou definovány šablonou v [  *&lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

Typické větvení strategie, budou taky používat s kódem přesouvání z větve dev až testovací větve, pak do hlavní větve (přesunutí v kvality, takže na speak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-need"></a>Co potřebujete
* Účet Azure
* A [Githubu](https://github.com/) účtu
* Git prostředí (nainstalované s [GitHub pro Windows](https://windows.github.com/)) – umožňuje spouštět příkazy Git i prostředí PowerShell ve stejné relaci 
* Nejnovější [prostředí Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps) bits
* Základní znalosti o následující nástroje:
  * [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nasazení šablony (viz také [nasazení komplexní aplikace předvídatelné v Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít účet Azure:
> 
> * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití můžete účet ponechat a používat bezplatné služby Azure, jako jsou webové aplikace.
> * Můžete [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -si Visual Studio předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.
> 
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="set-up-your-production-environment"></a>Nastavit produkční prostředí
> [!NOTE]
> Skript používá v tomto kurzu se automaticky nakonfiguruje průběžné publikování z úložiště Githubu. To vyžaduje, aby vaše přihlašovací údaje Githubu jsou již uloženy v Azure, jinak hodnota skriptované nasazení se nezdaří, při pokusu o konfiguraci nastavení správy zdrojů pro webové aplikace. 
> 
> K ukládání přihlašovacích údajů Githubu v Azure, vytvořit webovou aplikaci v [portál Azure](https://portal.azure.com/) a [konfigurace nasazení Githubu](app-service-continuous-deployment.md). Stačí k tomu jednou. 
> 
> 

V případě typické DevOps máte aplikaci, která běží v Azure za provozu a chcete provést změny prostřednictvím průběžné publikování. V tomto scénáři máte šablonu, která vyvinuté, testovat a použít k nasazení produkčním prostředí. Nastavíte ho v této části.

1. Vytvoření vlastního pokračovatelem [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště. Informace o vytváření vašeho rozvětvení najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/). Po vytvoření vaší rozvětvení, zobrazí se v prohlížeči.
   
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Otevřete relaci prostředí Git. Pokud ještě nemáte Git prostředí, nainstalujte [GitHub pro Windows](https://windows.github.com/) nyní.
3. Vytvořte místní vaší rozvětvení spuštěním následujícího příkazu:

        git clone https://github.com/<your_fork>/ToDoApp.git 
4. Až budete mít vaše místní klonování, přejděte na  *&lt;repository_root >*\ARMTemplates a spustit deploy.ps1 skript následujícím způsobem:
   
        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git
5. Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi.
   
   Měli byste vidět zřizování průběh různých prostředků Azure. Po dokončení nasazení skript spustí aplikaci v prohlížeči a získáte popisný zvukový signál.
   
    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
   
   > [!TIP]
   > Podívejte se na  *&lt;repository_root >*\ARMTemplates\Deploy.ps1 zobrazíte jak vygeneruje prostředky s jedinečné identifikátory. Stejný postup slouží k vytvoření klony stejného nasazení bez obav o konfliktní názvy prostředků.
   > 
   > 
6. Zpět v relaci prostředí Git, spusťte tento příkaz:
   
        .\swap –Name ToDoApp<unique_string>master
   
    ![](./media/app-service-agile-software-development/production-4-swap.png)
7. Po dokončení skriptu, přejděte zpět a přejděte na adresu front-endu (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) zobrazíte aplikace běžící v produkčním prostředí.
8. Přihlaste se k [portál Azure](https://portal.azure.com/) a prohlédněte si co je vytvořen.
   
   Byste měli vidět dvě webové aplikace ve stejné skupině prostředků, jeden s `Api` přípony v názvu. Pokud se podíváte na zobrazení skupiny prostředků, uvidíte také SQL Database a serveru, plán služby App Service a přípravné sloty pro webové aplikace. Procházet různých prostředků a porovnejte je s  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json zobrazíte, jak jsou nakonfigurované v šabloně.
   
    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Nyní jste nastavili v provozním prostředí. V dalším kroku se ji nové aktualizace aplikace.

## <a name="create-dev-and-test-branches"></a>Vytvoření vývoj a testování větví
Teď, když máte komplexní aplikace spuštěna v produkčním prostředí v Azure, budou do vaší aplikace v souladu s agilní metodika aktualizace. V této části vytvoříte vývoj a testování větve, které budete muset provést požadované aktualizace.

1. Nejprve vytvořte testovací prostředí. V relaci prostředí Git, spusťte následující příkazy k vytvoření prostředí pro nové větve názvem **NewUpdate**. 
   
        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate
2. Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi. 
   
   Po dokončení nasazení skript spustí aplikaci v prohlížeči a získáte popisný zvukový signál. Nyní máte nové větve s vlastním testovacím prostředí. Za chvíli zkontrolovat pár věcí o toto testovací prostředí:
   
   * Můžete ho vytvořit v jakéhokoli předplatného Azure. To znamená, že v provozním prostředí lze spravovat nezávisle z testovacího prostředí.
   * Testovací prostředí je spuštěna za provozu v Azure.
   * Testovací prostředí je stejný jako do produkčního prostředí, s výjimkou přípravné sloty a nastavení škálování. Vy víte, že vzhledem k tomu, že jsou pouze rozdíly mezi ProdandStage.json a Dev.json.
   * Testovacího prostředí můžete spravovat svůj vlastní plán služby App Service, s vrstvou, jinou cenu (například **volné**).
   * Odstranění tohoto testovacího prostředí je jednoduché, odstraníte skupinu prostředků. Zjistíte, jak to udělat [později](#delete).
3. Přejděte k vytvoření větve dev spuštěním následujících příkazů:
   
        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev
4. Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi. 
   
   Pozorně přečtěte si o tomto prostředí dev pár věcí: 
   
   * Vaše vývojová prostředí má konfiguraci identické do testovacího prostředí, protože je nasazena pomocí stejné šablony.
   * Vaše vývojová prostředí lze vytvořit v rámci předplatného Azure pro vývojáře vlastní ponechat testovacím prostředí, abyste samostatně spravovat.
   * Vaše vývojová prostředí je spuštěna za provozu v Azure.
   * Odstranění vývojová prostředí je jednoduché, odstraníte skupinu prostředků. Zjistíte, jak to udělat [později](#delete).

> [!NOTE]
> Pokud máte více vývojáře, kteří pracují na novou aktualizaci, každý z nich můžete snadno vytvořit větve a vyhrazené vývojová prostředí pomocí následujících kroků:
> 
> 1. Vytvořit vlastní rozvětvení úložiště na webu GitHub (najdete v části [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/)).
> 2. Klonování rozvětvení v jejich místním počítači
> 3. Spusťte stejné příkazy, které vytvářet své vlastní firemní pobočky vývojářů a prostředí.
> 
> 

Když jste hotovi, vaše Githubu rozvětvení musí mít tři větve:

![](./media/app-service-agile-software-development/test-1-github-view.png)

A v tři samostatné skupiny prostředků byste měli mít šest webové aplikace (tři sady dva):

![](./media/app-service-agile-software-development/test-2-all-webapps.png)

> [!NOTE]
> ProdandStage.json určuje provozním prostředí použít **standardní** cenové úrovně, který je vhodný pro škálovatelnost produkční aplikace.
> 
> 

## <a name="build-and-test-every-commit"></a>Sestavení a testů každou potvrzenou změnou
Soubory šablon ProdAndStage.json a Dev.json již zadejte parametry Zdroj ovládacího prvku, který ve výchozím nastavení nastavuje průběžné publikování webové aplikace. Proto každou potvrzenou změnou do větve Githubu se aktivuje automatické nasazení do Azure z tohoto větve. Podívejme se nyní fungování vašeho nastavení.

1. Ujistěte se, že jste ve větvi Dev místní úložiště. Chcete-li to provést, spusťte následující příkaz v prostředí Git:
   
        git checkout Dev
2. Změňte vrstva uživatelského rozhraní aplikace tak, že změníte kód, který použije [Bootstrap](http://getbootstrap.com/components/) uvádí. Otevřete  *&lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml a zkontrolujte následující zvýrazněnou změnit:
   
    ![](./media/app-service-agile-software-development/commit-1-changes.png)
   
    > [!NOTE]
    > Pokud předchozí obrázek nelze pro čtení: 
    > 
    > * V řádku 18 změnit `check-list` k `list-group`.
    > * V řádku 19, změňte `class="check-list-item"` k `class="list-group-item"`.
    > 
    > 
3. Uložte změnu. Zpět v prostředí Git, spusťte následující příkazy:
   
        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
   
   Tyto příkazy gitu jsou podobná "kontrola ve vašem kódu" v jiném systému správy zdrojů jako sady TFS. Při spuštění `git push`, nové potvrzení aktivuje nabízené automatické kód do Azure, který pak znovu sestaví aplikace, aby odrážely změny ve vývojovém prostředí.
4. Pokud chcete ověřit, že tento kód nabízené prostředí dev došlo, přejděte na stránku vývojová prostředí webové aplikace a podívejte se na **nasazení** část. Nyní byste měli mít zobrazíte nejnovější potvrzení zprávu existuje.
   
    ![](./media/app-service-agile-software-development/commit-2-deployed.png)
5. Zde, klikněte na tlačítko **Procházet** zobrazíte nové změny v živé aplikace v Azure.
   
    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)
   
   Je menší změny do aplikace. Ale mnoho časy nové změny komplexních webových aplikací mít nezamýšleným a nežádoucí vedlejší účinky. Schopnost snadno testovat každou potvrzenou změnou v za provozu sestavení umožňuje catch tyto problémy před zobrazovat vašich zákazníků.

Nyní by by měla být celý uskutečňování, jako vývojář **NewUpdate** projektu, můžete vytvořit prostředí dev sami, pak sestavení každou potvrzenou změnou a otestovat každé sestavení.

## <a name="merge-code-into-test-environment"></a>Sloučení kódu do testovacího prostředí
Až budete připraveni k nabízení kódu z větve Dev až NewUpdate větev, je proces standardní git:

1. Sloučit všechny nové potvrzení k NewUpdate do větve vývojářů v Githubu, jako je například potvrzení vytvořené jinými vývojáři. Všechny nové potvrdit na Githubu aktivační události push kódu a sestavení v prostředí vývojářů. Pak zajistit, že kód do větve Dev pořád funguje s nejnovější bity z NewUpdate větve.
2. Sloučit všechny nové potvrzení z vývojářů větve do NewUpdate větve na Githubu. Tato akce aktivuje kódu a sestavení v testovacím prostředí. 

Všimněte si znovu, protože se tyto větví git je již nastavena průběžné nasazování, není potřeba provádět další akce jako s integrací sestavení. Jednoduše je potřeba provést postupy řízení standardní zdroj pomocí gitu a Azure provádí všechny procesy sestavení pro vás.

Nyní Pojďme push svůj kód **NewUpdate** firemní pobočky. V prostředí Git spusťte následující příkazy:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

A to je vše! 

Přejděte na stránku webové aplikace pro testovací prostředí zobrazíte nové potvrzení (sloučí NewUpdate větev), teď nabídnutých do testovacího prostředí. Potom klikněte na **Procházet** zobrazíte, že změna stylu je nyní spuštěna za provozu v Azure.

## <a name="deploy-update-to-production"></a>Nasadit aktualizace do produkčního prostředí
Vkládání kódu pro pracovní a provozní prostředí by měl mít nejsou jiné než co již bylo provedeno při nabídnutých kódu do testovacího prostředí. Je skutečně tak jednoduché. 

V prostředí Git spusťte následující příkazy:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Nezapomeňte, že na základě způsobu pracovní a provozní prostředí se nastaví ve ProdandStage.json, váš nový kód vložena do **pracovní** pozice a běží existuje. Takže pokud přejdete na adresu URL přípravný slot, uvidíte nový kód spuštěný existuje. Provedete to spuštěním následující rutiny v prostředí Git.

    Start-Process -FilePath "http://ToDoApp<unique_string>master-Staging.azurewebsites.net"

A teď po aktualizaci v přípravný slot ověříte, jediné, co zleva provést Prohodit do produkčního prostředí. V prostředí Git spusťte následující příkazy:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Blahopřejeme! Nové aktualizace jste úspěšně publikovala na produkční webové aplikace. Je týmovými, který se snadno vytvořením prostředí vývoje a testování a vytváření a testování každou potvrzenou změnou. Jedná se o velmi důležitý stavebních bloků agile software development.

<a name="delete"></a>

## <a name="delete-dev-and-test-environments"></a>Odstraňte vývojářů a testovací prostředí
Vzhledem k tomu, že jste vytvořili účelově vaše prostředí vývoje a testování být nezávislý prostředek skupiny po, je snadné je odstranit. Chcete-li odstranit ty, které jste vytvořili v tomto kurzu Githubu větví i Azure artefaktů, stačí spustit následující příkazy v prostředí Git:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Souhrn
Agile software development je musí obsahovat pro mnoho společností, kteří chtějí přijmout službu Azure jako jejich platformy pro aplikace. V tomto kurzu jste se naučili, jak vytvořit a snadno i pro komplexní aplikace oddělení dolů přesný repliky nebo téměř repliky produkčního prostředí. Naučili jste jak využít tuto schopnost vytvářet vývoj proces, který můžete sestavit a otestovat každých jediné potvrzení v Azure. V tomto kurzu zpravidla uvádí, jak nejlépe můžete Azure App Service a Azure Resource Manager společně k vytvoření řešení DevOps, který je určen pro agilní metod. Potom můžete vytvořit v tomto scénáři provedením pokročilé techniky DevOps, jako [testování v produkčním prostředí](app-service-web-test-in-production-get-start.md). Běžné scénáře testování v produkčním, najdete v části [Flighting nasazení (testování verze beta) v Azure App Service](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Další zdroje informací
* [Nasazení aplikace komplexní předvídatelné v Azure](app-service-deploy-complex-application-predictably.md)
* [Agilní vývoj v praxi: tipy a triky pro Modernized vývoj cyklu](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
* [Pokročilé nasazení strategie pro webové aplikace Azure pomocí šablony Resource Manageru](http://channel9.msdn.com/Events/Build/2015/2-620)
* [Vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - validátor JSON](http://jsonlint.com/)
* [ARMClient – nastavit Githubu publikování na webu](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
* [Vytvoření větve Git – základní větvení a slučování](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Blog David Ebbo](http://blog.davidebbo.com/)
* [Azure PowerShell](/powershell/azure/overview)
* [Nástroje příkazového řádku Azure a platformy](../cli-install-nodejs.md)
* [Vytvoření nebo úprava uživatelů ve službě Azure AD](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
* [Wiki Kudu projektu](https://github.com/projectkudu/kudu/wiki)

