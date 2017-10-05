---
title: "Spuštění a zastavení virtuálních počítačů s Azure Automation – pracovní postup prostředí PowerShell | Microsoft Docs"
description: "Grafické verze scénář automatizace Azure, včetně sady runbook pro spuštění a zastavení klasické virtuální počítače."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d380bd43-d45d-45af-a5b2-78e7f66263c3
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: FALSE
ms.openlocfilehash: 95a7b02b0d11bf18c398daea48d16e0ead30b543
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

Toto je verze sady runbook PowerShell Workflow tohoto scénáře. Je také k dispozici pomocí [grafické runbooky](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Získání scénáře
Tento scénář se skládá ze dvou runbooky pracovních postupů Powershellu, které si můžete stáhnout z následujících odkazů.  Najdete v článku [grafické verze](automation-solution-startstopvm-graphical.md) tohoto scénáře odkazy na grafické runbooky.

| Runbook | Odkaz | Typ | Popis |
|:--- |:--- |:--- |:--- |
| Počáteční AzureVMs |[Spustit virtuální počítače Azure Classic](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |Pracovní postup prostředí PowerShell |Spustí všechny klasické virtuální počítače v Azure subscriptionor všechny virtuální počítače s názvem konkrétní služby. |
| Stop-AzureVMs |[Zastavte virtuální počítače Azure Classic](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |Pracovní postup prostředí PowerShell |Zastaví všechny virtuální počítače na účtu automation nebo všechny virtuální počítače s názvem konkrétní služby. |

## <a name="installing-and-configuring-the-scenario"></a>Instalace a konfigurace scénář
### <a name="1-install-the-runbooks"></a>1. Instalace sady runbook
Po stažení sady runbook, můžete importovat pomocí postupu v [import Runbooku](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-the-description-and-requirements"></a>2. Přečtěte si popis a požadavky
Sady runbook zahrnují komentáři nápovědy text, který obsahuje popis a požadované prostředky.  Můžete také získat stejné informace z tohoto článku.

### <a name="3-configure-assets"></a>3. Konfigurace prostředků
Sady runbook vyžaduje následující prostředky, které musíte vytvořit a naplnit s příslušnými hodnotami.

| Typ prostředku | Název prostředku | Popis |
|:--- |:--- |:--- |:--- |
| Přihlašovací údaj |AzureCredential |Obsahuje přihlašovací údaje pro účet, který má oprávnění ke spuštění a zastavení virtuálních počítačů v rámci předplatného Azure.  Alternativně můžete zadat jiný prostředek přihlašovacích údajů v **pověření** parametr **Add-AzureAccount** aktivity. |
| Proměnná |AzureSubscriptionId |Obsahuje ID předplatného vaše předplatné Azure. |

## <a name="using-the-scenario"></a>Pomocí tohoto scénáře
### <a name="parameters"></a>Parametry
Sady runbook každý musí mít následující parametry.  Zadejte hodnoty všech povinných parametrů a volitelně můžete zadat hodnoty pro ostatní parametry v závislosti na požadavcích.

| Parametr | Typ | Povinné | Popis |
|:--- |:--- |:--- |:--- |
| ServiceName |Řetězec |Ne |Pokud je zadána hodnota, jsou všechny virtuální počítače s tímto názvem služby spustit nebo zastavit.  Pokud není zadána žádná hodnota, jsou všechny klasické virtuální počítače v rámci předplatného Azure spustit nebo zastavit. |
| AzureSubscriptionIdAssetName |Řetězec |Ne |Obsahuje název [variabilní prostředek](#installing-and-configuring-the-scenario) obsahující ID předplatného vaše předplatné Azure.  Pokud nezadáte hodnotu, *AzureSubscriptionId* se používá. |
| AzureCredentialAssetName |Řetězec |Ne |Obsahuje název [asset přihlašovacích údajů](#installing-and-configuring-the-scenario) obsahující pověření pro sadu runbook použít.  Pokud nezadáte hodnotu, *AzureCredential* se používá. |

### <a name="starting-the-runbooks"></a>Spuštění sady runbook
Můžete použít některou z metod v [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) spustit buď sady runbook v tomto scénáři.

Následující vzorové příkazy prostředí Windows PowerShell používá ke spuštění **StartAzureVMs** k spuštění všech virtuálních počítačů s názvem služby *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Výstup
Sady runbook bude [na výstup zprávu](automation-runbook-output-and-messages.md) pro každý virtuální počítač, která určuje, zda byl pokyn spuštění nebo zastavení byla úspěšně odeslána.  Můžete vyhledat konkrétní řetězec ve výstupu určit výsledek pro každou sadu runbook.  V následující tabulce jsou uvedeny možné výstup řetězce.

| Runbook | Podmínka | Zpráva |
|:--- |:--- |:--- |
| Počáteční AzureVMs |Virtuální počítač je již spuštěna. |Můjvp je již spuštěna. |
| Počáteční AzureVMs |Žádost o spuštění pro virtuální počítač, byla úspěšně odeslána |Můjvp byla spuštěna. |
| Počáteční AzureVMs |Žádost o spuštění pro virtuální počítač se nezdařilo |Nepodařilo se spustit Můjvp |
| Stop-AzureVMs |Virtuální počítač je již zastaveno. |Můjvp je již zastaveno. |
| Stop-AzureVMs |Zastavení virtuálního počítače byla úspěšně odeslána žádost |Můjvp byla zastavena. |
| Stop-AzureVMs |Žádost o zastavení pro virtuální počítač se nezdařilo. |Můjvp se nepovedlo zastavit |

Například následující fragment kódu ze sady runbook pokusí spustit všechny virtuální počítače s názvem služby *MyServiceName*.  Pokud některý z selhání žádosti o spuštění, můžete provedeny chyby akce.

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Podrobné rozdělení
Toto je podrobný rozpis sad runbook v tomto scénáři.  Tyto informace slouží k přizpůsobení sady runbook nebo právě dozvědět se od nich vlastní scénáře automatizace pro vytváření obsahu.

### <a name="parameters"></a>Parametry
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

Pracovní postup spustí získáním hodnoty [vstupní parametry](#using-the-scenario).  Pokud nejsou zadány názvy datových zdrojů se používají výchozí názvy.

### <a name="output"></a>Výstup
    # Returns strings with status messages
    [OutputType([String])]

Tento řádek deklaruje, že bude výstup runbooku řetězec.  Tento není povinný, ale je vhodné, když sada runbook slouží jako [podřízeného runbooku](automation-child-runbooks.md) tak, aby nadřazený runbook bude vědět, výstupní typ očekávat.

### <a name="authentication"></a>Authentication
    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Sada řádků na další [pověření](automation-credentials.md) a předplatné Azure, který se použije pro zbytek sady runbook.
Nejprve používáme **Get-AutomationPSCredential** získat asset, který uchovává přihlašovací údaje s přístupem ke spuštění a zastavení virtuálních počítačů v rámci předplatného Azure. **Přidat-AzureAccount** pak použije tento prostředek nastavit přihlašovací údaje.  Výstup je přiřazen fiktivní proměnné tak, aby není zahrnut ve výstupu sady runbook.  

Variabilní prostředek s ID se potom načte se odběru **Get-AutomationVariable** a předplatné s **Select-AzureSubscription**.

### <a name="get-vms"></a>Získat virtuální počítače
    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

**Get-AzureVM** se používá k načtení sady runbook bude fungovat s virtuální počítače.  Pokud je hodnota zadaná v **ServiceName** vstupní proměnné, pak jsou načteny pouze virtuální počítače s tímto názvem služby.  Pokud **ServiceName** je prázdný, pak jsou načteny všechny virtuální počítače.

### <a name="startstop-virtual-machines-and-send-output"></a>Spuštění a zastavení virtuálních počítačů a odeslání výstupu
    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Dalším krokem řádky prostřednictvím každý virtuální počítač.  První **PowerState** virtuálního počítače, které se kontroluje k zkontrolujte, jestli je už spuštěná nebo zastavená, v závislosti na sadě runbook.  Pokud je již ve stavu cíl, je odeslána zpráva výstup a skončení sady runbook.  Pokud ne, pak **Start-AzureVM** nebo **Stop-AzureVM** se používá k pokusu o spuštění nebo zastavení virtuálního počítače s výsledek požadavku uložit do proměnné.  Pak je odeslána zpráva do výstupního určující, zda byl úspěšně odeslán požadavek na spuštění nebo zastavení.

## <a name="next-steps"></a>Další kroky
* Další informace o práci s podřízené runbooky najdete v tématu [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md)
* Další informace o zprávách výstup při spuštění sady runbook a protokolování při jeho řešení využít najdete v tématu [Runbook výstup a zprávy v Azure Automation.](automation-runbook-output-and-messages.md)

