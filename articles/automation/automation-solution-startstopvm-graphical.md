---
title: "Graf aaaStarting a zastavení virtuálních počítačů - | Microsoft Docs"
description: "Pracovní postup prostředí PowerShell verze scénář automatizace Azure, včetně sady runbook toostart a ukončení klasické virtuální počítače."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 62d93170-ca3d-42ef-ac1d-6d50b61ac87d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: False
ms.openlocfilehash: 5add8d8cf35ea2e89a570744755ac7db0a6feb07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Automation scénář – spuštění a zastavení virtuálních počítačů
Tento scénář automatizace Azure zahrnuje runbooky toostart a ukončení klasické virtuální počítače.  V tomto scénáři můžete použít pro žádné z následujících hello:  

* Ve svém vlastním prostředí pomocí sady runbook hello bez úprav.
* Upravte funkce tooperform přizpůsobit hello sady runbook.  
* Volání sady runbook hello z jiné sady runbook v rámci celého řešení.
* Pomocí sad runbook hello jako runbook toolearn kurzy vytváření koncepty.

> [!div class="op_single_selector"]
> * [Grafický](automation-solution-startstopvm-graphical.md)
> * [Pracovní postup PowerShellu](automation-solution-startstopvm-psworkflow.md)
>
>

Toto je verze grafický runbook hello tohoto scénáře. Je také k dispozici pomocí [runbooky pracovních postupů Powershellu](automation-solution-startstopvm-psworkflow.md).

## <a name="getting-hello-scenario"></a>Získávání scénář hello
Tento scénář se skládá ze dvou dvě grafické runbooky, které si můžete stáhnout z hello následující odkazy.  V tématu hello [pracovního postupu Powershellu verze](automation-solution-startstopvm-psworkflow.md) tohoto scénáře pro odkazy toohello runbooky pracovních postupů Powershellu.

| Runbook | Odkaz | Typ | Popis |
|:--- |:--- |:--- |:--- |
| StartAzureClassicVM |[Spuštění virtuálního počítače Azure Classic grafický Runbook](https://gallery.technet.microsoft.com/scriptcenter/Start-Azure-Classic-VM-c6067b3d) |Grafické |Všechny klasické virtuální počítače se spustí v předplatné Azure nebo všechny virtuální počítače s názvem konkrétní služby. |
| StopAzureClassicVM |[Zastavte virtuální počítač Azure Classic grafický Runbook](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-Classic-VM-397819bd) |Grafické |Zastaví všechny virtuální počítače na účtu automation nebo všechny virtuální počítače s názvem konkrétní služby. |

## <a name="installing-and-configuring-hello-scenario"></a>Instalace a konfigurace hello scénář
### <a name="1-install-hello-runbooks"></a>1. Instalace sady runbook hello
Po stažení hello sady runbook, můžete je importovat pomocí postupu hello v [grafický runbook postupy](automation-graphical-authoring-intro.md#graphical-runbook-procedures).

### <a name="2-review-hello-description-and-requirements"></a>2. Zkontrolujte hello popis a požadavky
sady runbook Hello zahrnují aktivity volá **čtení mi** který obsahuje popis a požadované prostředky.  Tyto informace můžete zobrazit výběrem hello **čtení mi** aktivitu a potom hello **Workflow je skript** parametr.  Můžete také získat hello stejné informace z tohoto článku.

### <a name="3-configure-assets"></a>3. Konfigurace prostředků
sady runbook Hello vyžadují hello následující prostředky, které musíte vytvořit a naplnit s příslušnými hodnotami.  Hello je výchozí.  Pokud zadáte tyto názvy v hello, můžete použít prostředky s různými názvy [vstupní parametry](#using-the-runbooks) při spuštění sady runbook hello.

| Typ prostředku | Výchozí název | Popis |
|:--- |:--- |:--- |:--- |
| [Přihlašovací údaje](automation-credentials.md) |AzureCredential |Obsahuje přihlašovací údaje pro účet, který má autority toostart a ukončete virtuální počítače v hello předplatného Azure. |
| [Proměnná](automation-variables.md) |AzureSubscriptionId |Obsahuje ID předplatného hello vašeho předplatného Azure. |

## <a name="using-hello-scenario"></a>Pomocí hello scénáře
### <a name="parameters"></a>Parametry
Hello každý musí mít sady runbook hello následující [vstupní parametry](automation-starting-a-runbook.md#runbook-parameters).  Zadejte hodnoty všech povinných parametrů a volitelně můžete zadat hodnoty pro ostatní parametry v závislosti na požadavcích.

| Parametr | Typ | Povinné | Popis |
|:--- |:--- |:--- |:--- |
| ServiceName |Řetězec |Ne |Pokud je zadána hodnota, jsou všechny virtuální počítače s tímto názvem služby spustit nebo zastavit.  Pokud není zadána žádná hodnota, jsou všechny klasické virtuální počítače v hello předplatného Azure spustit nebo zastavit. |
| AzureSubscriptionIdAssetName |Řetězec |Ne |Obsahuje název hello hello [variabilní prostředek](#installing-and-configuring-the-scenario) obsahující ID předplatného hello vašeho předplatného Azure.  Pokud nezadáte hodnotu, *AzureSubscriptionId* se používá. |
| AzureCredentialAssetName |Řetězec |Ne |Obsahuje název hello hello [asset přihlašovacích údajů](#installing-and-configuring-the-scenario) obsahující hello přihlašovací údaje pro hello runbook toouse.  Pokud nezadáte hodnotu, *AzureCredential* se používá. |

### <a name="starting-hello-runbooks"></a>Spouštění sad runbook hello
Můžete použít některou z metod hello v [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) toostart buď hello sady runbook v tomto článku.

Následující vzorové příkazy Hello používá prostředí Windows PowerShell toorun **StartAzureClassicVM** toostart všechny virtuální počítače s názvem služby hello *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "StartAzureClassicVM" –Parameters $params

### <a name="output"></a>Výstup
Hello sady runbook bude [na výstup zprávu](automation-runbook-output-and-messages.md) pro každý virtuální počítač označující, zda text hello spuštění nebo zastavení instrukce byla úspěšně odeslána.  Můžete vyhledat konkrétní řetězec v hello výstup toodetermine hello výsledek pro každou sadu runbook.  řetězce výstup Hello jsou uvedeny v následující tabulce hello.

| Runbook | Podmínka | Zpráva |
|:--- |:--- |:--- |
| StartAzureClassicVM |Virtuální počítač je již spuštěna. |Můjvp je již spuštěna. |
| StartAzureClassicVM |Žádost o spuštění pro virtuální počítač, byla úspěšně odeslána |Můjvp byla spuštěna. |
| StartAzureClassicVM |Žádost o spuštění pro virtuální počítač se nezdařilo |Toostart Můjvp se nezdařilo |
| StopAzureClassicVM |Virtuální počítač je již spuštěna. |Můjvp je již zastaveno. |
| StopAzureClassicVM |Žádost o spuštění pro virtuální počítač, byla úspěšně odeslána |Můjvp byla spuštěna. |
| StopAzureClassicVM |Žádost o spuštění pro virtuální počítač se nezdařilo |Toostart Můjvp se nezdařilo |

Toto je bitové kopie pomocí hello **StartAzureClassicVM** jako [podřízené sady runbook](automation-child-runbooks.md) v ukázkové grafický runbook.  Podmíněná propojení hello se používá v hello následující tabulka.

| Odkaz | Kritéria |
|:--- |:--- |
| Úspěch odkaz |$ActivityOutput ['StartAzureClassicVM']-jako "\* byla spuštěna" |
| Chyba propojení |$ActivityOutput ['StartAzureClassicVM']-notlike "\* byla spuštěna" |

![Příklad podřízené sady runbook](media/automation-solution-startstopvm/graphical-childrunbook-example.png)

## <a name="detailed-breakdown"></a>Podrobné rozdělení
Toto je podrobný rozpis hello sady runbook v tomto scénáři.  Tyto informace můžete použít tooeither přizpůsobení sady runbook hello nebo jenom toolearn z nich vlastní scénáře automatizace pro vytváření obsahu.

### <a name="authentication"></a>Authentication
![Authentication](media/automation-solution-startstopvm/graphical-authentication.png)

Hello runbook začíná textem hello tooset aktivity [pověření](automation-credentials.md) a předplatné Azure, který se použije pro hello zbytek hello runbook.

Hello první dvě aktivity **získat Id předplatného** a **získat přihlašovací údaje Azure**, načtení hello [prostředky](#installing-the-runbook) jsou používány následující dvě aktivity hello.  Tyto aktivity přímo zadat hello prostředky, ale potřebují hello názvy datových zdrojů.  Vzhledem k tomu, že jsme se povolení uživatele toospecify hello tyto názvy v hello [vstupní parametry](#using-the-runbooks), potřebujeme tyto aktivity tooretrieve hello prostředky s názvem určeného vstupní parametr.

**Přidat-AzureAccount** sady hello přihlašovací údaje, které se použije pro hello zbytek hello runbook.  Hello asset přihlašovacích údajů, který je načte z **získat přihlašovací údaje Azure** musí mít přístup k virtuálním počítačům toostart a ukončení v hello předplatného Azure.  Hello odběr, který se používá je vybraná **Select-AzureSubscription** , který používá Id předplatného hello z **získat Id předplatného**.

### <a name="get-virtual-machines"></a>Získat virtuální počítače
![Získat virtuální počítače](media/automation-solution-startstopvm/graphical-getvms.png)

Hello runbook potřebuje toodetermine, které virtuální počítače se budou pracovat se a zda jsou již spustit nebo zastavit (v závislosti na hello runbook).   Mezi dvěma aktivitami bude načíst hello virtuálních počítačů.  **Získat virtuální počítače ve službě** se spustí, pokud hello *ServiceName* vstupní parametr runbooku hello obsahuje hodnotu.  **Získání všech virtuálních počítačů** se spustí, pokud hello *ServiceName* vstupní parametr runbooku hello neobsahuje hodnotu.  Tato logika je realizována vytvořením hello podmíněných propojení před každou aktivitu.

Obě aktivity používají hello **Get-AzureVM** rutiny.  **Získání všech virtuálních počítačů** hello používá **ListAllVMs** nastavený parametr tooreturn všechny virtuální počítače.  **Získat virtuální počítače ve službě** hello používá **GetVMByServiceAndVMName** parametr nastavit a poskytuje hello **ServiceName** vstupního parametru pro hello **ServiceName**parametr.  

### <a name="merge-vms"></a>Sloučení virtuální počítače
![Sloučení virtuální počítače](media/automation-solution-startstopvm/graphical-mergevms.png)

Hello **sloučení virtuálních počítačů** aktivity je požadovaná tooprovide vstupní příliš**Start-AzureVM** které je potřeba hello název a název služby toostart hello virtuální počítače.  Aby vstup může pocházet z buď **získat všechny virtuální počítače** nebo **získat virtuální počítače ve službě**, ale **Start-AzureVM** lze zadat pouze jednu aktivitu pro vstupní.   

scénář Hello je toocreate **sloučení virtuálních počítačů** která se spouští hello **Write-Output** rutiny.  Hello **InputObject** parametr pro tuto rutinu je Powershellový výraz, který kombinuje hello vstup hello předchozích dvou aktivit.  Pouze jeden z těchto aktivit se spustí, takže jenom jednu sadu výstup se očekává.  **Start-AzureVM** můžete použít tento výstup pro jeho vstupní parametry.

### <a name="startstop-virtual-machines"></a>Spuštění / zastavení virtuálních počítačů
![Spustit virtuální počítače](media/automation-solution-startstopvm/graphical-startvm.png) ![Zastavte virtuální počítače](media/automation-solution-startstopvm/graphical-stopvm.png)

V závislosti na hello runbook další aktivity hello pokus toostart nebo přestat používat runbook hello **Start-AzureVM** nebo **Stop-AzureVM**.  Vzhledem k tomu, že aktivita hello předchází propojení kanálu, se aplikace spustí jednou pro každý objekt vrácený **sloučení virtuálních počítačů**.  Hello odkaz je podmíněného tak, aby hello aktivita se spustí jen tehdy, pokud hello *RunningState* hello virtuálního počítače je *Zastaveno* pro **Start-AzureVM** a  *Spuštění* pro **Stop-AzureVM**. Pokud tento není podmínka splněná, pak **oznámit spuštění již** nebo **upozornění již zastaveno** se spustit toosend zprávu pomocí **Write-Output**.

### <a name="send-output"></a>Odeslání výstupu
![Oznámit spuštění virtuálních počítačů](media/automation-solution-startstopvm/graphical-notifystart.png) ![Oznámit zastavení virtuálních počítačů](media/automation-solution-startstopvm/graphical-notifystop.png)

Hello poslední krok v sadě runbook hello je výstup toosend tom, zda text hello počáteční nebo byla úspěšně odeslána žádost o zastavení pro každý virtuální počítač. Je samostatný **Write-Output** aktivitu, a jsme určit, které jeden toorun s využitím podmíněných propojení.  **Oznámit spuštění virtuálního počítače** nebo **oznámit zastavit virtuální počítač** se spustí, pokud *OperationStatus* je *úspěšné*.  Pokud *OperationStatus* jakoukoli jinou hodnotu, pak je **oznámit neúspěšné tooStart** nebo **oznámit neúspěšné tooStop** běží.

## <a name="next-steps"></a>Další kroky
* [Grafické vytváření obsahu v Azure Automation.](automation-graphical-authoring-intro.md)
* [Podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md)
* [Výstup a zprávy ve službě Azure Automation Runbooku](automation-runbook-output-and-messages.md)
