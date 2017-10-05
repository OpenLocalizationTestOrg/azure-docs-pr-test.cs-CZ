---
title: "Spuštění a zastavení virtuálního počítače - grafu | Microsoft Docs"
description: "Pracovní postup prostředí PowerShell verze scénář automatizace Azure, včetně sady runbook pro spuštění a zastavení klasické virtuální počítače."
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
redirect_document_id: FALSE
ms.openlocfilehash: 338d5712239356e13cbf480d9655ca3ca499701d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Automation scénář – spuštění a zastavení virtuálních počítačů
Tento scénář automatizace Azure zahrnuje sady runbook pro spuštění a zastavení klasické virtuální počítače.  V tomto scénáři můžete použít pro žádné z následujících akcí:  

* Pomocí sad runbook bez úprav ve svém vlastním prostředí.
* Úprava sady runbook k provedení vlastní funkce.  
* Volání sady runbook z jiné sady runbook v rámci celého řešení.
* Pomocí sady runbook jako kurzy se dozvíte vytváření koncepty sady runbook.

> [!div class="op_single_selector"]
> * [Grafický](automation-solution-startstopvm-graphical.md)
> * [Pracovní postup PowerShellu](automation-solution-startstopvm-psworkflow.md)
>
>

Toto je verze grafický runbook tohoto scénáře. Je také k dispozici pomocí [runbooky pracovních postupů Powershellu](automation-solution-startstopvm-psworkflow.md).

## <a name="getting-the-scenario"></a>Získání scénáře
Tento scénář se skládá ze dvou dvě grafické runbooky, které si můžete stáhnout z následujících odkazů.  Najdete v článku [pracovního postupu Powershellu verze](automation-solution-startstopvm-psworkflow.md) tohoto scénáře odkazy na runbooky pracovních postupů Powershellu.

| Runbook | Odkaz | Typ | Popis |
|:--- |:--- |:--- |:--- |
| StartAzureClassicVM |[Spuštění virtuálního počítače Azure Classic grafický Runbook](https://gallery.technet.microsoft.com/scriptcenter/Start-Azure-Classic-VM-c6067b3d) |Grafické |Všechny klasické virtuální počítače se spustí v předplatné Azure nebo všechny virtuální počítače s názvem konkrétní služby. |
| StopAzureClassicVM |[Zastavte virtuální počítač Azure Classic grafický Runbook](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-Classic-VM-397819bd) |Grafické |Zastaví všechny virtuální počítače na účtu automation nebo všechny virtuální počítače s názvem konkrétní služby. |

## <a name="installing-and-configuring-the-scenario"></a>Instalace a konfigurace scénář
### <a name="1-install-the-runbooks"></a>1. Instalace sady runbook
Po stažení sady runbook, můžete importovat pomocí postupu v [grafický runbook postupy](automation-graphical-authoring-intro.md#graphical-runbook-procedures).

### <a name="2-review-the-description-and-requirements"></a>2. Přečtěte si popis a požadavky
Sady runbook zahrnují aktivity volá **čtení mi** který obsahuje popis a požadované prostředky.  Tyto informace můžete zobrazit výběrem **čtení mi** aktivitu a potom **Workflow je skript** parametr.  Můžete také získat stejné informace z tohoto článku.

### <a name="3-configure-assets"></a>3. Konfigurace prostředků
Sady runbook vyžaduje následující prostředky, které musíte vytvořit a naplnit s příslušnými hodnotami.  Názvy jsou výchozí.  Pokud zadáte tyto názvy v, můžete použít prostředky s různými názvy [vstupní parametry](#using-the-runbooks) při spuštění sady runbook.

| Typ prostředku | Výchozí název | Popis |
|:--- |:--- |:--- |:--- |
| [Přihlašovací údaje](automation-credentials.md) |AzureCredential |Obsahuje přihlašovací údaje pro účet, který má oprávnění ke spuštění a zastavení virtuálních počítačů v rámci předplatného Azure. |
| [Proměnná](automation-variables.md) |AzureSubscriptionId |Obsahuje ID předplatného vaše předplatné Azure. |

## <a name="using-the-scenario"></a>Pomocí tohoto scénáře
### <a name="parameters"></a>Parametry
Sady runbook každé mají následující [vstupní parametry](automation-starting-a-runbook.md#runbook-parameters).  Zadejte hodnoty všech povinných parametrů a volitelně můžete zadat hodnoty pro ostatní parametry v závislosti na požadavcích.

| Parametr | Typ | Povinné | Popis |
|:--- |:--- |:--- |:--- |
| ServiceName |Řetězec |Ne |Pokud je zadána hodnota, jsou všechny virtuální počítače s tímto názvem služby spustit nebo zastavit.  Pokud není zadána žádná hodnota, jsou všechny klasické virtuální počítače v rámci předplatného Azure spustit nebo zastavit. |
| AzureSubscriptionIdAssetName |Řetězec |Ne |Obsahuje název [variabilní prostředek](#installing-and-configuring-the-scenario) obsahující ID předplatného vaše předplatné Azure.  Pokud nezadáte hodnotu, *AzureSubscriptionId* se používá. |
| AzureCredentialAssetName |Řetězec |Ne |Obsahuje název [asset přihlašovacích údajů](#installing-and-configuring-the-scenario) obsahující pověření pro sadu runbook použít.  Pokud nezadáte hodnotu, *AzureCredential* se používá. |

### <a name="starting-the-runbooks"></a>Spuštění sady runbook
Můžete použít některou z metod v [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) spustit buď sady runbook v tomto článku.

Následující vzorové příkazy prostředí Windows PowerShell používá ke spuštění **StartAzureClassicVM** k spuštění všech virtuálních počítačů s názvem služby *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "StartAzureClassicVM" –Parameters $params

### <a name="output"></a>Výstup
Sady runbook bude [na výstup zprávu](automation-runbook-output-and-messages.md) pro každý virtuální počítač, která určuje, zda byl pokyn spuštění nebo zastavení byla úspěšně odeslána.  Můžete vyhledat konkrétní řetězec ve výstupu určit výsledek pro každou sadu runbook.  V následující tabulce jsou uvedeny možné výstup řetězce.

| Runbook | Podmínka | Zpráva |
|:--- |:--- |:--- |
| StartAzureClassicVM |Virtuální počítač je již spuštěna. |Můjvp je již spuštěna. |
| StartAzureClassicVM |Žádost o spuštění pro virtuální počítač, byla úspěšně odeslána |Můjvp byla spuštěna. |
| StartAzureClassicVM |Žádost o spuštění pro virtuální počítač se nezdařilo |Nepodařilo se spustit Můjvp |
| StopAzureClassicVM |Virtuální počítač je již spuštěna. |Můjvp je již zastaveno. |
| StopAzureClassicVM |Žádost o spuštění pro virtuální počítač, byla úspěšně odeslána |Můjvp byla spuštěna. |
| StopAzureClassicVM |Žádost o spuštění pro virtuální počítač se nezdařilo |Nepodařilo se spustit Můjvp |

Následuje bitové kopie pomocí **StartAzureClassicVM** jako [podřízeného runbooku](automation-child-runbooks.md) v ukázkové grafický runbook.  Podmíněná propojení se používá v následující tabulce.

| Odkaz | Kritéria |
|:--- |:--- |
| Úspěch odkaz |$ActivityOutput ['StartAzureClassicVM']-jako "\* byla spuštěna" |
| Chyba propojení |$ActivityOutput ['StartAzureClassicVM']-notlike "\* byla spuštěna" |

![Příklad podřízené sady runbook](media/automation-solution-startstopvm/graphical-childrunbook-example.png)

## <a name="detailed-breakdown"></a>Podrobné rozdělení
Toto je podrobný rozpis sad runbook v tomto scénáři.  Tyto informace slouží k přizpůsobení sady runbook nebo právě dozvědět se od nich vlastní scénáře automatizace pro vytváření obsahu.

### <a name="authentication"></a>Authentication
![Authentication](media/automation-solution-startstopvm/graphical-authentication.png)

Sada runbook se spouští aktivity nastavit [pověření](automation-credentials.md) a předplatné Azure, který se použije pro zbytek sady runbook.

První dvě aktivity **získat Id předplatného** a **získat přihlašovací údaje Azure**, načíst [prostředky](#installing-the-runbook) jsou používány následující dvě aktivity.  Tyto aktivity přímo zadat prostředky, ale potřebují názvy datových zdrojů.  Vzhledem k tomu, že jsme se umožní uživateli zadat tyto názvy v [vstupní parametry](#using-the-runbooks), je třeba tyto aktivity k načtení prostředků s názvem určeného vstupní parametr.

**Přidat-AzureAccount** Nastaví pověření, které se použijí pro zbytek sady runbook.  Asset přihlašovacích údajů, který je načte z **získat přihlašovací údaje Azure** musí mít přístup ke spuštění a zastavení virtuálních počítačů v rámci předplatného Azure.  Odběr, který se používá je vybrána jako **Select-AzureSubscription** , který používá Id předplatného z **získat Id předplatného**.

### <a name="get-virtual-machines"></a>Získat virtuální počítače
![Získat virtuální počítače](media/automation-solution-startstopvm/graphical-getvms.png)

Sada runbook je potřeba určit, které virtuální počítače, bude práce a zda jsou již spustit nebo zastavit (v závislosti na sadě runbook).   Virtuální počítače mezi dvěma aktivitami bude načíst.  **Získat virtuální počítače ve službě** se spustí, pokud *ServiceName* vstupního parametru pro sadu runbook obsahuje hodnotu.  **Získání všech virtuálních počítačů** se spustí, pokud *ServiceName* vstupního parametru pro sadu runbook neobsahuje hodnotu.  Tato logika je realizována podmíněného propojení před každou aktivitu.

Obě aktivity používají **Get-AzureVM** rutiny.  **Získání všech virtuálních počítačů** používá **ListAllVMs** parametr nastaven na vrátit všechny virtuální počítače.  **Získat virtuální počítače ve službě** používá **GetVMByServiceAndVMName** parametr nastavit a poskytuje **ServiceName** vstupního parametru pro **ServiceName** parametr.  

### <a name="merge-vms"></a>Sloučení virtuální počítače
![Sloučení virtuální počítače](media/automation-solution-startstopvm/graphical-mergevms.png)

**Sloučení virtuálních počítačů** aktivity je potřeba k zadání **Start-AzureVM** kterou musí název a název služby virtuální počítače spustit.  Aby vstup může pocházet z buď **získat všechny virtuální počítače** nebo **získat virtuální počítače ve službě**, ale **Start-AzureVM** lze zadat pouze jednu aktivitu pro vstupní.   

Tento scénář je vytvoření **sloučení virtuálních počítačů** které běží **Write-Output** rutiny.  **InputObject** parametr pro tuto rutinu je Powershellový výraz, který kombinuje vstup předchozích dvou aktivit.  Pouze jeden z těchto aktivit se spustí, takže jenom jednu sadu výstup se očekává.  **Start-AzureVM** můžete použít tento výstup pro jeho vstupní parametry.

### <a name="startstop-virtual-machines"></a>Spuštění / zastavení virtuálních počítačů
![Spustit virtuální počítače](media/automation-solution-startstopvm/graphical-startvm.png) ![Zastavte virtuální počítače](media/automation-solution-startstopvm/graphical-stopvm.png)

V závislosti na sadě runbook, aktivity další pokus o spuštění nebo zastavení sady runbook pomocí **Start-AzureVM** nebo **Stop-AzureVM**.  Vzhledem k tomu, že aktivita předchází propojení kanálu, se aplikace spustí jednou pro každý objekt vrácený **sloučení virtuálních počítačů**.  Odkaz je podmíněného tak, aby aktivita se spustí jen tehdy, pokud *RunningState* virtuálního počítače je *Zastaveno* pro **Start-AzureVM** a *Začínáme*  pro **Stop-AzureVM**. Pokud tento není podmínka splněná, pak **oznámit spuštění již** nebo **upozornění již zastaveno** používá spustit odeslat zprávu **Write-Output**.

### <a name="send-output"></a>Odeslání výstupu
![Oznámit spuštění virtuálních počítačů](media/automation-solution-startstopvm/graphical-notifystart.png) ![Oznámit zastavení virtuálních počítačů](media/automation-solution-startstopvm/graphical-notifystop.png)

Poslední krok v sadě runbook je pro odeslání výstupu, zda požadavek na spuštění nebo zastavení u každého virtuálního počítače byla úspěšně odeslána. Je samostatný **Write-Output** aktivitu, a jsme určení, která se spouští s využitím podmíněných propojení.  **Oznámit spuštění virtuálního počítače** nebo **oznámit zastavit virtuální počítač** se spustí, pokud *OperationStatus* je *úspěšné*.  Pokud *OperationStatus* jakoukoli jinou hodnotu, pak je **oznámení se nezdařilo spusťte** nebo **oznámení se nepodařilo zastavit** běží.

## <a name="next-steps"></a>Další kroky
* [Grafické vytváření obsahu v Azure Automation.](automation-graphical-authoring-intro.md)
* [Podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md)
* [Výstup a zprávy ve službě Azure Automation Runbooku](automation-runbook-output-and-messages.md)
