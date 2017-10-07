---
title: "Odstranění skupiny prostředků aaaAutomate | Microsoft Docs"
description: "Pracovní postup prostředí PowerShell verzi scénářem automatizace Azure, včetně sady runbook tooremove prostředků všech skupin v rámci vašeho předplatného."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: 
ms.assetid: b848e345-fd5d-4b9d-bc57-3fe41d2ddb5c
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/26/2016
ms.author: magoedte
ms.openlocfilehash: d7ff8064842385d57b0eebdf7b263150c958255f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Scénář Azure Automation – automatizace odebrání skupin prostředků
Mnoho zákazníků vytváří více než jednu skupinu prostředků. Některé mohou sloužit ke správě produkčních aplikací, jiné mohou sloužit jako vývojové, testovací nebo přípravné prostředí. Automatizace hello nasazení těchto prostředků je jednou z věcí, ale je možné toodecommission skupinu prostředků s klikněte na tlačítko hello je jiné. Tuto běžnou úlohu správy můžete zjednodušit pomocí Azure Automation. To je užitečné, pokud pracujete s předplatné Azure, který má limitu útraty a automaticky prostřednictvím člen nabídka jako MSDN nebo hello programu Microsoft Partner sítě Cloud Essentials.

Tento scénář je založený na Powershellovém runbooku a navrženou tooremove je jeden nebo více skupin prostředků, které zadáte ze svého předplatného. Výchozí nastavení Hello hello sady runbook je tootest než budete pokračovat. Tím se zajistí, že neodstraníte omylem skupiny prostředků hello předtím, než jste připravené toocomplete tento postup.   

## <a name="getting-hello-scenario"></a>Získávání scénář hello
Tento scénář se skládá z runbook Powershellu, které si můžete stáhnout z hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). Můžete ho importovat také přímo z hello [Galerie Runbooků](automation-runbook-gallery.md) v hello portálu Azure.<br><br>

| Runbook | Popis |
| --- | --- |
| Remove-ResourceGroup |Odebere jedno nebo více skupin prostředků Azure a související prostředky z předplatného hello. |

<br>
Hello následující vstupní parametry jsou definovány pro tuto sadu runbook:

| Parametr | Popis |
| --- | --- |
| NameFilter (povinný) |Určuje název filtru toolimit hello skupiny prostředků, které chcete o odstranění. Můžete předat několik hodnot ve formě seznamu odděleného čárkami.<br>Hello filtru není velká a malá písmena a bude odpovídat libovolné skupině prostředků, který obsahuje řetězec hello. |
| PreviewMode (volitelný) |Provede toosee hello sady runbook, které skupiny prostředků by odstraněn, ale neprovede žádnou akci.<br>Výchozí hodnota Hello je **true** toohelp vyhnout náhodného odstranění jednoho nebo více skupin prostředků předán toohello runbook. |

## <a name="install-and-configure-this-scenario"></a>Instalace a konfigurace tohoto scénáře
### <a name="prerequisites"></a>Požadavky
Tato sada runbook se ověří pomocí hello [účet spustit v Azure jako](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-hello-runbooks"></a>Instalace a publikování sad runbook hello
Po stažení hello runbook, můžete ho importovat pomocí postupu hello v [import postupy runbook](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation). Publikujte hello runbook po byl úspěšně importován do vašeho účtu Automation.

## <a name="using-hello-runbook"></a>Pomocí sady runbook hello
Hello následující postup vás provede hello provádění této sady runbook a nápovědy, které jste se seznámili s jak to funguje. Jenom testujete hello runbook v tomto příkladu, ve skutečnosti není odstranění skupiny prostředků hello.  

1. Z hello portálu Azure otevřete účet Automation a klikněte na tlačítko **Runbooky**.
2. Vyberte hello **Remove-ResourceGroup** runbook a klikněte na tlačítko **spustit**.
3. Při spuštění sady runbook hello hello **spuštění Runbooku** otevře se okno a můžete nastavit parametry hello. Zadejte hello názvy skupin prostředků v rámci vašeho předplatného, můžete použít pro testování a způsobí není škodu, pokud náhodou smazala.<br> ![Parametry Remove-ResouceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)

   > [!NOTE]
   > Zajistěte, aby **Previewmode** je nastaven příliš**true** tooavoid odstraňování hello vybrané skupiny prostředků.  **Poznámka:** , nebude tato sada runbook odebrat hello skupinu prostředků, která obsahuje účet Automation hello, kterým je spuštěn tento runbook.  
   >
   >
4. Po nakonfigurování všech hodnot parametru hello, klikněte na tlačítko **OK**, a hello runbook se zařadí do fronty pro provedení.  

Podrobnosti hello tooview hello **Remove-ResourceGroup** úlohy runbooku v hello portál Azure, vyberte **úlohy** v hello runbook. Hello úlohy Souhrn zobrazí hello vstupní parametry a výstup hello stream kromě toogeneral informace o úloze hello a všechny výjimky, které došlo k chybě.<br> ![Stav úlohy runbooku Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png)

Hello **Souhrn úlohy** zahrnuje zprávy z datových proudů hello výstup, upozornění a chyby. Vyberte **výstup** tooview podrobné výsledky z hello spuštění sady runbook.<br> ![Výsledky výstupu runbooku Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Další kroky
* tooget zahájeno vytváření vlastní sady runbook, najdete v části [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md).
* tooget kroky s runbooky pracovních postupů Powershellu najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md).
