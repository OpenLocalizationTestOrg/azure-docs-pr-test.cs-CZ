---
title: Galerie aaaRunbook a modul Azure Automation | Microsoft Docs
description: "Runbooky a moduly z komunity služby společnosti Microsoft a hello jsou k dispozici pro jste tooinstall a použít v prostředí Azure Automation.  Tento článek popisuje, jak a jsou k dispozici tyto prostředky toocontribute Galerie toohello vaší sady runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d3fee7b4-630a-4c10-8425-9bf51d7c9e58
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 10b634460edf66dd7548017e3a2f7111b7125f30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Galerie Runbooků a modulů pro Azure Automation.
Místo vytvoření vlastní sady runbook a modulů ve službě Azure Automation, můžete zpřístupnit různé scénáře, které jste již vytvořili společnosti Microsoft a hello komunitou.  Můžete použít tyto scénáře bez jakýchkoli úprav nebo můžete používat jako výchozí bod a je upravit pro vaše konkrétní požadavky.

Sady runbook můžete získat z hello [Galerie Runbooků](#runbooks-in-runbook-gallery) a moduly z hello [Galerie prostředí PowerShell](#modules-in-powerShell-gallery).  Také můžete přispívat toohello komunity sdílením scénáře, které vyvíjíte.

## <a name="runbooks-in-runbook-gallery"></a>Sady Runbook v Galerie Runbooků
Hello [Galerie Runbooků](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) poskytuje celou řadu sady runbook ze společnosti Microsoft a hello komunity, který můžete importovat do Azure Automation. Buď můžete stáhnout sady runbook z Galerie hello, který je hostován v hello [centra skriptů TechNet](https://gallery.technet.microsoft.com/scriptcenter/site/upload), nebo můžete přímo naimportovat sady runbook z Galerie hello z hello portál Azure classic nebo portálu Azure.

Lze importovat pouze přímo z Galerie Runbooků hello pomocí hello portál Azure classic nebo portálu Azure. Nelze provést tuto funkci pomocí prostředí Windows PowerShell.

> [!NOTE]
> Je třeba ověřit obsah hello žádné sady runbook můžete získat z hello Galerie Runbooků a buďte velmi opatrní při instalaci a spustil je v produkčním prostředí. |
> 
> 

### <a name="tooimport-a-runbook-from-hello-runbook-gallery-with-hello-azure-classic-portal"></a>tooimport sady runbook z Galerie Runbooků hello s hello portál Azure classic
1. V hello portálu Azure klikněte na tlačítko, **nový**, **App Services**, **automatizace**, **Runbook**, **z Galerie**.
2. Vyberte kategorie tooview související sady runbook a vyberte runbook tooview její podrobnosti. Když vyberete hello runbook, které chcete, klikněte na tlačítko hello šipku vpravo.
   
    ![Galerie runbooků](media/automation-runbook-gallery/runbook-gallery.png)
3. Zkontrolujte obsah hello hello sady runbook a poznamenejte si všechny požadavky v popisu hello. Po dokončení, klikněte na tlačítko hello šipkou vpravo.
4. Zadejte podrobnosti hello sady runbook a potom klikněte na značku zaškrtnutí hello. Název sady runbook Hello se již být vyplněna.
5. Hello runbook se objeví na hello **Runbooky** karta hello účet Automation.

### <a name="tooimport-a-runbook-from-hello-runbook-gallery-with-hello-azure-portal"></a>tooimport sady runbook z Galerie Runbooků hello s hello portálu Azure
1. V hello portálu Azure otevřete účet Automation.
2. Klikněte na hello **Runbooky** dlaždice tooopen hello seznamu sad runbook.
3. Klikněte na tlačítko **procházet galerii** tlačítko.
   
    ![Galerie tlačítko Procházet](media/automation-runbook-gallery/browse-gallery-button.png)
4. Vyhledejte položku Galerie hello a vybrat tooview její podrobnosti.
   
    ![Procházet galerii](media/automation-runbook-gallery/browse-gallery.png)
5. Klikněte na **zobrazení zdroje projektu** tooview hello položky v hello [centra skriptů TechNet](http://gallery.technet.microsoft.com/).
6. tooimport některou položku, klikněte na jeho tooview podrobnosti a pak klikněte na tlačítko hello **Import** tlačítko.
   
    ![Tlačítko Import](media/automation-runbook-gallery/gallery-item-detail.png)
7. Volitelně můžete změnit název hello hello sady runbook a potom klikněte na **OK** tooimport hello runbook.
8. Hello runbook se objeví na hello **Runbooky** karta hello účet Automation.

### <a name="adding-a-runbook-toohello-runbook-gallery"></a>Přidávání Galerie runbooků toohello runbook
Společnost Microsoft doporučuje tooadd sady runbook toohello Galerie Runbooků, které by být užitečné tooother zákazníků.  Můžete přidat sadu runbook pomocí [pak ho nahrát toohello centra skriptů](http://gallery.technet.microsoft.com/site/upload) s ohledem na účet hello následujících podrobnostech.

* Je nutné zadat *systému Windows Azure* pro hello **kategorie** a *automatizace* pro hello **podkategorie** pro toobe runbook hello zobrazí v Průvodci hello.  
* nahrávání Hello musí být do jednoho souboru .ps1 nebo .graphrunbook.  Pokud hello runbook vyžaduje, aby všechny moduly, podřízené runbooky a prostředky, by měl těch v hello popis hello odeslání a v sekci komentáře hello hello runbook seznamu.  Pokud máte scénáři, které vyžadují více sad runbook, samostatně každý nahrát a seznamu názvů hello hello související sady runbook v každé z jejich popisy. Ujistěte se, že používáte stejné značky tak, aby se objeví v hello hello stejné kategorii. Uživatel bude mít tooread hello popis tooknow, jiné sady runbook jsou toowork scénář vyžaduje hello.
* Přidání značka hello "GraphicalPS", při publikování **grafický runbook** (ne grafický Workflow). 
* Vložit fragment kódu prostředí PowerShell nebo pracovního postupu Powershellu do hello popis pomocí **část kódu vložit** ikonu.
* Souhrn pro nahrávání hello Hello se zobrazí v hello Galerie Runbooků výsledků, takže byste měli poskytnout podrobné informace, které vám pomohou identifikovat hello funkce hello runbook uživatele.
* Byste měli přiřadit jeden toothree hello následující značky toohello nahrávání.  Hello runbook se objeví v Průvodci hello pod hello kategorií, které odpovídají jeho značky.  Všechny značky není v tomto seznamu se budou ignorovat průvodcem hello. Pokud nezadáte žádné odpovídající značky, hello runbook budou uvedené v části hello jiné kategorie.
  
  * Zálohování
  * Správa kapacit
  * Řízení změn
  * Dodržování předpisů
  * Dev / testovací prostředí
  * Zotavení po havárii
  * Monitorování
  * Opravy chyb
  * Zřizování
  * Nápravy
  * Správa životního cyklu virtuálních počítačů
* Automatizace aktualizuje hello Galerie tak vaše příspěvky se nezobrazí okamžitě jednou za hodinu.

## <a name="modules-in-powershell-gallery"></a>Moduly v galerii prostředí PowerShell
Moduly prostředí PowerShell obsahují rutiny, které můžete použít ve vašich sadách runbook a existující moduly, které můžete nainstalovat ve službě Azure Automation jsou k dispozici v hello [Galerie prostředí PowerShell](http://www.powershellgallery.com).  Můžete spustit tuto galerii z hello portál Azure a nainstalovat je přímo do Azure Automation, nebo můžete je stáhnout a nainstalovat ručně.  Hello moduly nemůžete instalovat přímo z hello portál Azure classic, ale můžete je stáhnout nainstalovat je stejně jako ostatní moduly.

### <a name="tooimport-a-module-from-hello-automation-module-gallery-with-hello-azure-portal"></a>tooimport modul z hello automatizace v galerii modulů s hello portálu Azure
1. V hello portálu Azure otevřete účet Automation.
2. Klikněte na hello **prostředky** dlaždice tooopen hello seznamu prostředků.
3. Klikněte na hello **moduly** dlaždice tooopen hello seznamu modulů.
4. Klikněte na hello **procházet galerii** spuštění tlačítko a hello okno Procházet galerii.
   
    ![V galerii modulů](media/automation-runbook-gallery/modules-blade.png) <br>
5. Po spuštění hello procházet galerii okno můžete vyhledávat podle hello následující pole:
   
   * Název modulu
   * Značky
   * Autor
   * Název prostředku rutiny nebo DSC
6. Vyhledejte modul zajímá a vyberte ho tooview její podrobnosti.  
   Pokud můžete přejít k podrobnostem konkrétního modulu, můžete zobrazit další informace o modulu hello, včetně toohello zpět na odkaz Galerie prostředí PowerShell potřebné závislosti, a všechny hello rutin a prostředků DSC, které hello modul obsahuje.
   
    ![Podrobnosti o modulu prostředí PowerShell](media/automation-runbook-gallery/gallery-item-details-blade.png) <br>
7. modul hello tooinstall přímo do Azure Automation, klikněte na tlačítko hello **Import** tlačítko.
   
    ![Tlačítko Import modulu](media/automation-runbook-gallery/module-import-button.png)
8. Když kliknete na tlačítko Importovat hello, zobrazí se název hello modulu, které jsou o tooimport. Pokud jsou nainstalovány všechny závislosti hello, hello **OK** tlačítko bude aktivní. Pokud jsou chybějících závislostí, je třeba tooimport ty před importováním tohoto modulu.
9. Klikněte na tlačítko **OK** tooimport hello modulu a okno modulu hello se spustí. Jakmile Azure Automation importuje účet tooyour modulu, extrahuje metadata o hello modulu a rutiny hello.
   
    ![Import modulu okno](media/automation-runbook-gallery/module-import-blade.png)
   
    Vzhledem k tomu, že každá aktivita musí toobe extrahovat, může to trvat několik minut.
10. Zobrazí se oznámení Tenhle modul hello se nasazuje a oznámení, když byla dokončena.
11. Po importování modulu hello, uvidíte hello dostupných aktivit a její prostředky v runboocích a konfigurace požadovaného stavu můžete použít.

## <a name="requesting-a-runbook-or-module"></a>Požaduje sady runbook nebo modulu
Můžete odesílat žádosti o příliš[User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  Pokud potřebujete pomoc, zápis sady runbook nebo máte dotazy týkající se prostředí PowerShell, post otázku tooour [fórum](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Další kroky
* tooget kroky s runbooky, najdete v části [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md)
* toounderstand hello rozdíly mezi prostředí PowerShell a pracovní postup prostředí PowerShell pomocí runbooků, najdete v části [pracovního postupu Powershellu učení](automation-powershell-workflow.md)

