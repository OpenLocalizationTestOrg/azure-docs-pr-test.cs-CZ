---
title: "aaaSet až přípravná prostředí pro webové aplikace v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak toouse připravený publikování pro webové aplikace v Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: 338424100a20bf823323313fb6699e439f367421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Nastavení přípravných prostředí v Azure App Service
<a name="Overview"></a>

Při nasazení vaší webové aplikace, webové aplikace na Linuxu, mobilní back-end a aplikace API příliš[služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714), můžete nasadit tooa samostatné nasazovací slot místo produkčního slotu. hello výchozí při spuštění v hello **Standard**nebo **Premium** režimu plán služby App Service. Nasazovací sloty jsou ve skutečnosti za provozu aplikace s vlastní názvy hostitelů. Můžete si místo aplikace obsahu a konfigurace – elementy mezi dvěma sloty nasazení, včetně hello produkční slot. Nasazení vaší aplikace tooa nasazovací slot má hello následující výhody:

* Můžete ověřit změny aplikace v přípravný slot nasazení před odkládací s hello produkční slot.
* První nasazení slot tooa aplikace a odkládací do produkčního prostředí zajistí, jsou všechny instance hello slotu jestli před prohazují do produkčního prostředí. Tím se eliminuje výpadek při nasazení aplikace. přesměrování provozu Hello je bezproblémové a jsou v důsledku operace prohození vyřadit žádné požadavky. Tento celý pracovní postup je možné automatizovat tak, že nakonfigurujete [Prohodit automaticky](#Auto-Swap) při swap předběžné ověření není potřeba.
* Po prohození hello slot s dříve dvoufázové instalace aplikace teď má hello předchozí produkční aplikaci. Pokud jsou tyto změny hello vzájemně zaměněny do produkčního slotu. hello není podle očekávání, můžete provést stejný Prohodit okamžitě zpět tooget "poslední známé funkční web" hello.

Každý režim plán služby App Service podporuje různé počty nasazovací sloty. toofind hello snížit počet sloty režim vaše aplikace podporuje, najdete v části [App Service – ceny](https://azure.microsoft.com/pricing/details/app-service/).

* Pokud vaše aplikace obsahuje více sloty, nelze změnit režim hello.
* Škálování není k dispozici pro nevýrobní prostředí sloty.
* Správa propojeného prostředku není podporován pro nevýrobní sloty. V hello [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715) pouze se vyhnout této potenciální dopad na produkční slot dočasně přesunutím hello mimo produkční slot tooa různé služby App Service plán režimu. Všimněte si, že hello mimo produkční slot musejí sdílet znovu hello stejný režim s hello produkčního slotu. předtím, než můžete Prohodit sloty hello dva.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Přidat slot nasazení
Hello aplikace musí být spuštěna v hello **standardní** nebo **Premium** režimu v pořadí pro tooenable můžete více nasazovací sloty.

1. V hello [portálu Azure](https://portal.azure.com/), otevřete v této aplikaci [okna prostředků](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Zvolte hello **nasazovací sloty** možnost a potom klikněte na **přidat Slot**.
   
    ![Přidat nový slot nasazení][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Pokud aplikace hello ještě není v hello **standardní** nebo **Premium** režimu, obdržíte zprávu s upozorněním hello podporované režimy pro povolení publikování dvoufázové instalace. Nyní máte možnost tooselect hello **Upgrade** a přejděte toohello **škálování** kartě vaší aplikace, než budete pokračovat.
   > 
   > 
3. V hello **přidat slot** okně pojmenujte hello slot a vyberte zda tooclone konfigurace aplikací z jiného existujícího slotu nasazení. Klikněte na tlačítko zaškrtnutí toocontinue hello.
   
    ![Zdroj konfigurace][ConfigurationSource1]
   
    Hello poprvé přidat slot, budete mít pouze dvě možnosti: klonování konfiguraci z hello výchozí slotu v produkčním prostředí nebo vůbec.
    Po vytvoření několik sloty, budete moct tooclone konfigurace z slotu než hello, jeden v produkčním prostředí:
   
    ![Konfigurace zdroje][MultipleConfigurationSources]
4. V okně prostředků aplikace, klikněte na tlačítko **nasazovací sloty**, klikněte tooopen slotu nasazení tohoto slotu okna prostředků, sadu metriky a konfigurace stejně jako kterákoli jiná aplikace. Hello název slotu hello zobrazuje hello horní části okna tooremind hello je, že kliknete hello nasazovací slot.
   
    ![Název slotu nasazení][StagingTitle]
5. Klikněte na adresu URL aplikace hello v okně hello slot. Všimněte si hello nasazovací slot má svůj vlastní název hostitele a také za provozu aplikace. slot nasazení toohello toolimit veřejný přístup, najdete v části [webové aplikace App Service – blok webového přístupu toonon produkční nasazovací sloty](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Po vytvoření slotu nasazení není k dispozici žádný obsah. Můžete nasadit toohello slotu z různých úložiště větev nebo úplně jiné úložiště. Můžete také změnit konfiguraci slotu hello. Použití hello publikovat profilu nebo nasazení přihlašovací údaje spojené s hello slot nasazení pro aktualizace obsahu.  Například můžete [publikování slot toothis s gitem](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a>Konfigurace pro nasazovací sloty
Když naklonovat konfiguraci z jiného slotu nasazení klonovaného konfigurace hello je upravovat. Kromě toho některé konfigurace – elementy bude postupovat podle obsahu hello napříč prohození (ne slotu konkrétní) při další konfigurace – elementy zůstanou v hello stejné pozici po prohození (slotu konkrétní). Hello následující seznamy shrnují hello konfigurace, která se změní při Prohodit sloty.

**Nastavení, které jsou vzájemně zaměněny**:

* Obecné nastavení – například framework verze 32/64-bit, webové sokety
* Nastavení aplikace (může být nakonfigurované toostick tooa slot)
* Připojovací řetězce (může být nakonfigurované toostick tooa slot)
* Mapování obslužných rutin
* Nastavení monitorování a diagnostiky
* Obsah webové úlohy

**Nastavení, které nejsou vzájemně zaměněny**:

* Publikování koncové body
* Názvy vlastních domén
* Certifikáty SSL a vazeb
* Nastavení škálování
* Webové úlohy plánovače

tooconfigure nastavení nebo připojení řetězec toostick tooa slot aplikace (není prohodily), přístup hello **nastavení aplikace** okno pro konkrétní pozici a potom vyberte hello **nastavení slotu** pole pro hello konfigurační prvky, které by měl přilepit hello slot. Všimněte si, že označíte konfigurační prvek jako slotu konkrétní má vliv hello vytvoření tohoto prvku jako není swappable napříč všechny hello nasazovací sloty spojená s aplikací hello.

![Nastavení slotu][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Prohodit sloty nasazení 
Můžete Prohodit sloty nasazení v hello **přehled** nebo **nasazovací sloty** zobrazení okna prostředků vaší aplikace.

> [!IMPORTANT]
> Předtím, než jste odkládacího souboru aplikace z slotu nasazení do produkčního prostředí, zajistit, aby všechny bez slotu konkrétní nastavení jsou nakonfigurovaná přesně tak, jak chcete, aby toohave v cílové swap hello.
> 
> 

1. tooswap nasazovací sloty, klikněte na tlačítko hello **Prohodit** tlačítko panelu příkazů hello hello aplikace nebo v řádku nabídek hello slot nasazení.
   
    ![Swap – tlačítko][SwapButtonBar]

2. Ujistěte se, že jsou správně nastaveny swap hello cílených zdroje a velikost odkládacího souboru. Cíl swap hello je obvykle hello produkční slot. Klikněte na tlačítko **OK** toocomplete hello operaci. Po dokončení operace hello mít byla vzájemně zaměněny hello nasazovací sloty.

    ![Dokončit](./media/web-sites-staged-publishing/SwapImmediately.png)

    Pro hello **prohození s náhledem** Prohodit typu, najdete v části [prohození s náhledem (více fáze prohození)](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>Prohození s náhledem (více fáze swap)

Prohození s náhledem nebo více fáze prohození zjednodušit ověření konfigurace slotu specifické prvky, jako jsou například připojovací řetězce.
Pro klíčové úlohy chcete toovalidate, který hello aplikace chová podle očekávání při použití konfigurace hello produkčním slotu a je třeba provést takové ověření *před* aplikace hello je prohodily do produkčního prostředí. Prohození s náhledem je, co potřebujete.

> [!NOTE]
> Prohození s náhledem nepodporuje webové aplikace v systému Linux.

Při použití hello **prohození s náhledem** možnost (viz [Prohodit sloty nasazení](#Swap)), služby App Service hello následující:

- Není ovlivněná udržuje hello cílový slot beze změny tak existující zatížení na této pozici (například výroba).
- Aplikuje hello konfigurační prvky hello cílový slot toohello zdrojový slot, včetně hello specifické pro slot připojovací řetězce a nastavení aplikace.
- Restartuje hello pracovních procesů na zdrojový slot hello pomocí těchto zmíněnými konfigurace – elementy.
- Po dokončení hello swap: Přesune hello warmed vedlejší-up zdrojový slot do hello cílový slot. hello zdrojový slot jako ruční prohození přesunutí Hello cílový slot.
- Když zrušíte hello swap: znovu použije hello konfigurace – elementy hello zdrojový slot toohello zdrojový slot.

Můžete zobrazit náhled, přesně jak hello aplikace se bude chovat s konfigurací hello cílový slot. Po dokončení ověření dokončíte hello odkládacího souboru v samostatném kroku. Tento krok má hello přidané výhodu hello zdrojový slot již jestli s hello požadované konfigurace, a klienti nebudou mít žádné výpadky.  

Ukázky pro hello rutin prostředí Azure PowerShell, které jsou k dispozici pro více fáze prohození jsou součástí hello rutin Azure Powershellu pro část sloty nasazení.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Konfigurace automatického prohození
Automatické prohození zjednodušuje DevOps scénáře, kde chcete toocontinuously nasazení vaší aplikace pomocí nulové studený start a brání výpadkům pro koncové zákazníky aplikace hello. Když slot nasazení je nakonfigurována pro automatické prohození do produkčního prostředí, pokaždé, když push vaší slotu toothat aktualizace kódu, služby App Service automaticky odkládacího hello aplikace do produkčního prostředí poté, co se má již provozní teplotu ve slotu hello.

> [!IMPORTANT]
> Když povolíte automaticky Prohodit slot, ujistěte se, že konfigurace slotu hello přesně hello konfigurace určené pro cílový slot hello (obvykle hello produkční slot).
> 
> 

> [!NOTE]
> Nepodporuje automatické prohození webové aplikace v systému Linux.

Konfigurace automatického Prohodit pro slot je snadné. Postupujte podle následujících kroků hello:

1. V **nasazovací sloty**, vyberte mimo produkční slot a zvolit **nastavení aplikace** v okně prostředků tento slot.  
   
    ![][Autoswap1]
2. Vyberte **na** pro **Prohodit automaticky**, vyberte hello požadované cílový slot v **automaticky Prohodit Slot**a klikněte na tlačítko **Uložit** hello panelu příkazů. Zajistěte, aby konfigurace pro hello slot je přesně hello konfigurace určené pro cílový slot hello.
   
    Hello **oznámení** karta bude flash zelená **úspěch** po dokončení operace hello.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > tootest Prohodit automaticky pro vaši aplikaci, vyberte nejprve mimo produkční cílový slot v **automaticky Prohodit Slot** toobecome obeznámeni s funkcí hello.  
   > 
   > 
3. Spusťte slotu nasazení toothat nabízené kódu. Automatického prohození se stane po krátkou dobu a hello aktualizace se projeví na adrese URL vaší cílový slot.

<a name="Rollback"></a>

## <a name="toorollback-a-production-app-after-swap"></a>toorollback produkční aplikace po swap
Pokud žádné chyby se zjišťují v produkčním prostředí po prohození slotů, vrátit hello sloty back tootheir před swap stavy odkládací hello stejné dva sloty okamžitě.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Vlastní warm-up před swap
Některé aplikace může vyžadovat vlastní warm-up akce. Hello `applicationInitialization` konfigurace element v souboru web.config umožňuje vám toospecify vlastní inicializaci akce toobe provést dřív, než je přijat požadavek. operace přepnutí Hello vyčká pro tuto vlastní warm-up toocomplete. Zde je fragment ukázkový soubor web.config.

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="toodelete-a-deployment-slot"></a>toodelete slotu nasazení
V okně hello pro slot nasazení, okno otevřít hello nasazovací slot, klikněte na tlačítko **přehled** (hello výchozí stránka) a klikněte na tlačítko **odstranit** hello panelu příkazů.  

![Odstranit slotu nasazení][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a>Rutiny Azure PowerShell pro nasazovací sloty
Prostředí Azure PowerShell je modul, který poskytuje toomanage rutiny Azure pomocí prostředí Windows PowerShell, včetně podpory pro správu nasazovací sloty ve službě Azure App Service.

* Informace o instalaci a konfiguraci prostředí Azure PowerShell a týkající se ověřování Azure PowerShell s předplatným Azure najdete v tématu [jak tooinstall a nakonfigurovat Microsoft Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Vytvoření webové aplikace
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Vytvořte slot nasazení
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-toosource-slot"></a>Zahájení prohození s zkontrolujte (více fáze prohození) a použít cílový slot konfigurace toosource slot
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Zrušit čeká na prohození (prohození s zkontrolujte) a obnovit konfiguraci slotu zdroje
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Prohodit sloty nasazení
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a>Odstranit nasazovací slot.
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a>Azure příkazy rozhraní příkazového řádku (Azure CLI) pro nasazovací sloty
Hello rozhraní příkazového řádku Azure poskytuje příkazy a platformy pro práci s Azure, včetně podpory pro správu služby App Service nasazovací sloty.

* Pro pokyny k instalaci a konfiguraci hello rozhraní příkazového řádku Azure, včetně informací o tom, rozhraní příkazového řádku Azure tooyour tooconnect předplatné Azure, najdete v části [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md).
* volání toolist hello příkazy dostupné pro službu Azure App Service v hello rozhraní příkazového řádku Azure, `azure site -h`.

> [!NOTE] 
> Pro [Azure CLI 2.0](https://github.com/Azure/azure-cli) příkazy pro nasazovací sloty, najdete v části [slot nasazení webové služby App Service az](/cli/azure/appservice/web/deployment/slot).

- - -
### <a name="azure-site-list"></a>seznam webů Azure
Informace o aplikacích hello v aktuálním předplatném hello volání **seznam webů azure**, jako v hello následující ukázka.

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a>Vytvoření Azure lokality
volání toocreate nasazovací slot **azure lokality vytvořit** a zadejte název stávající aplikace hello a hello název slotu toocreate hello, stejně jako hello následující ukázka.

`azure site create webappslotstest --slot staging`

tooenable zdrojového kódu pro nový slot hello, použijte hello **– git** možnost, stejně jako hello následující ukázka.

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a>swap Azure lokality
toomake hello aktualizované nasazení slotu hello produkční aplikace, použijte hello **azure lokality swap** příkaz tooperform operaci přepnutí, stejně jako hello následující ukázka. Hello produkční aplikaci nebude mít žádný výpadek ani ho určitým studený start.

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a>odstranění webu Azure
toodelete slot nasazení, která už není potřeba, použijte hello **odstranění azure webu** příkaz jako hello následující ukázka.

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> Sledujte webovou aplikaci v akci. [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/) okamžitě a vytvořit krátkodobou úvodní aplikaci – nepožaduje se žádná platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="next-steps"></a>Další kroky
[Službě Azure App Service Web App – blokovat webový přístup k produkční toonon nasazovací sloty](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Úvod tooApp služby v systému Linux](./app-service-linux-intro.md)
[bezplatná zkušební verze Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

