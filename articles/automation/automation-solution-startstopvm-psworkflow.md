---
title: "aaaStarting a zastavení virtuálních počítačů s Azure Automation – pracovní postup prostředí PowerShell | Microsoft Docs"
description: "Grafické verze scénáře Azure Automation, včetně sady runbook toostart a ukončení klasické virtuální počítače."
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
redirect_document_id: False
ms.openlocfilehash: 273631c7fc5ddb989b3bbdc82b470ac3af6ee482
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

Toto je hello verze sady runbook PowerShell Workflow tohoto scénáře. Je také k dispozici pomocí [grafické runbooky](automation-solution-startstopvm-graphical.md).

## <a name="getting-hello-scenario"></a>Získávání scénář hello
Tento scénář se skládá z dvě runbooky pracovních postupů Powershellu, které si můžete stáhnout z hello následující odkazy.  V tématu hello [grafické verze](automation-solution-startstopvm-graphical.md) tohoto scénáře pro grafické runbooky toohello odkazy.

| Runbook | Odkaz | Typ | Popis |
|:--- |:--- |:--- |:--- |
| Počáteční AzureVMs |[Spustit virtuální počítače Azure Classic](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |Pracovní postup prostředí PowerShell |Spustí všechny klasické virtuální počítače v Azure subscriptionor všechny virtuální počítače s názvem konkrétní služby. |
| Stop-AzureVMs |[Zastavte virtuální počítače Azure Classic](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |Pracovní postup prostředí PowerShell |Zastaví všechny virtuální počítače na účtu automation nebo všechny virtuální počítače s názvem konkrétní služby. |

## <a name="installing-and-configuring-hello-scenario"></a>Instalace a konfigurace hello scénář
### <a name="1-install-hello-runbooks"></a>1. Instalace sady runbook hello
Po stažení hello sady runbook, můžete je importovat pomocí postupu hello v [import Runbooku](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-hello-description-and-requirements"></a>2. Zkontrolujte hello popis a požadavky
sady runbook Hello zahrnují komentáři nápovědy text, který obsahuje popis a požadované prostředky.  Můžete také získat hello stejné informace z tohoto článku.

### <a name="3-configure-assets"></a>3. Konfigurace prostředků
sady runbook Hello vyžadují hello následující prostředky, které musíte vytvořit a naplnit s příslušnými hodnotami.

| Typ prostředku | Název prostředku | Popis |
|:--- |:--- |:--- |:--- |
| Přihlašovací údaj |AzureCredential |Obsahuje přihlašovací údaje pro účet, který má autority toostart a ukončete virtuální počítače v hello předplatného Azure.  Alternativně můžete zadat jiný prostředek přihlašovacích údajů v hello **pověření** parametr hello **Add-AzureAccount** aktivity. |
| Proměnná |AzureSubscriptionId |Obsahuje ID předplatného hello vašeho předplatného Azure. |

## <a name="using-hello-scenario"></a>Pomocí hello scénáře
### <a name="parameters"></a>Parametry
Hello runbooky mají hello následující parametry.  Zadejte hodnoty všech povinných parametrů a volitelně můžete zadat hodnoty pro ostatní parametry v závislosti na požadavcích.

| Parametr | Typ | Povinné | Popis |
|:--- |:--- |:--- |:--- |
| ServiceName |Řetězec |Ne |Pokud je zadána hodnota, jsou všechny virtuální počítače s tímto názvem služby spustit nebo zastavit.  Pokud není zadána žádná hodnota, jsou všechny klasické virtuální počítače v hello předplatného Azure spustit nebo zastavit. |
| AzureSubscriptionIdAssetName |Řetězec |Ne |Obsahuje název hello hello [variabilní prostředek](#installing-and-configuring-the-scenario) obsahující ID předplatného hello vašeho předplatného Azure.  Pokud nezadáte hodnotu, *AzureSubscriptionId* se používá. |
| AzureCredentialAssetName |Řetězec |Ne |Obsahuje název hello hello [asset přihlašovacích údajů](#installing-and-configuring-the-scenario) obsahující hello přihlašovací údaje pro hello runbook toouse.  Pokud nezadáte hodnotu, *AzureCredential* se používá. |

### <a name="starting-hello-runbooks"></a>Spouštění sad runbook hello
Můžete použít některou z metod hello v [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md) toostart buď hello sady runbook v tomto scénáři.

Následující vzorové příkazy Hello používá prostředí Windows PowerShell toorun **StartAzureVMs** toostart všechny virtuální počítače s názvem služby hello *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Výstup
Hello sady runbook bude [na výstup zprávu](automation-runbook-output-and-messages.md) pro každý virtuální počítač označující, zda text hello spuštění nebo zastavení instrukce byla úspěšně odeslána.  Můžete vyhledat konkrétní řetězec v hello výstup toodetermine hello výsledek pro každou sadu runbook.  řetězce výstup Hello jsou uvedeny v následující tabulce hello.

| Runbook | Podmínka | Zpráva |
|:--- |:--- |:--- |
| Počáteční AzureVMs |Virtuální počítač je již spuštěna. |Můjvp je již spuštěna. |
| Počáteční AzureVMs |Žádost o spuštění pro virtuální počítač, byla úspěšně odeslána |Můjvp byla spuštěna. |
| Počáteční AzureVMs |Žádost o spuštění pro virtuální počítač se nezdařilo |Toostart Můjvp se nezdařilo |
| Stop-AzureVMs |Virtuální počítač je již zastaveno. |Můjvp je již zastaveno. |
| Stop-AzureVMs |Zastavení virtuálního počítače byla úspěšně odeslána žádost |Můjvp byla zastavena. |
| Stop-AzureVMs |Žádost o zastavení pro virtuální počítač se nezdařilo. |Toostop Můjvp se nezdařilo |

Například následující fragment kódu ze sady runbook hello pokusí toostart všechny virtuální počítače s názvem služby hello *MyServiceName*.  Pokud některé z hello žádosti o selhání, spusťte můžete provedeny chyby akce.

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action tootake in case of success.
        }
        else {
            # Action tootake in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Podrobné rozdělení
Toto je podrobný rozpis hello sady runbook v tomto scénáři.  Tyto informace můžete použít tooeither přizpůsobení sady runbook hello nebo jenom toolearn z nich vlastní scénáře automatizace pro vytváření obsahu.

### <a name="parameters"></a>Parametry
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

Hello pracovní postup spuštěn získáním hello hodnoty pro hello [vstupní parametry](#using-the-scenario).  Pokud nejsou zadány hello asset názvy se používají výchozí názvy.

### <a name="output"></a>Výstup
    # Returns strings with status messages
    [OutputType([String])]

Tento řádek deklaruje, že bude výstup hello hello runbook řetězec.  Tento není povinný, ale je vhodné, když se hello runbook používá jako [podřízeného runbooku](automation-child-runbooks.md) tak, aby věděli, nadřízený runbook výstup hello zadejte tooexpect.

### <a name="authentication"></a>Authentication
    # Connect tooAzure and select hello subscription toowork against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Další řádky Hello nastavit hello [pověření](automation-credentials.md) a předplatné Azure, který se použije pro hello zbytek hello runbook.
Nejprve používáme **Get-AutomationPSCredential** tooget hello asset, který obsahuje hello přihlašovací údaje s přístupem k toostart a ukončete virtuální počítače v hello předplatného Azure. **Přidat-AzureAccount** pak používá toto pověření hello tooset asset.  výstup Hello je přiřazena tooa fiktivní proměnné, aby není zahrnut ve výstupu runbook hello.  

Hello variabilní prostředek s ID se potom načte s předplatným hello **Get-AutomationVariable** a hello předplatné s **Select-AzureSubscription**.

### <a name="get-vms"></a>Získat virtuální počítače
    # If there is a specific cloud service, then get all VMs in hello service,
    # otherwise get all VMs in hello subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

**Get-AzureVM** je použité tooretrieve hello runbook bude fungovat s hello virtuálních počítačů.  Pokud je hodnota zadaná v hello **ServiceName** vstupní proměnné a pak pouze hello virtuální počítače s tímto názvem služby jsou načteny.  Pokud **ServiceName** je prázdný, pak jsou načteny všechny virtuální počítače.

### <a name="startstop-virtual-machines-and-send-output"></a>Spuštění a zastavení virtuálních počítačů a odeslání výstupu
    # Start each of hello stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # hello VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # hello VM needs toobe started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # hello VM failed toostart, so send notice
                Write-Output ($VM.InstanceName + " failed toostart")
            }
            else
            {
                # hello VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Další řádky Hello projděte každý virtuální počítač.  Nejprve hello **PowerState** hello virtuálního počítače je zaškrtnuté toosee Pokud už je spuštěná nebo zastavená, v závislosti na hello runbook.  Pokud je již ve stavu hello cíl, je odeslána zpráva, toooutput a ukončení runbook hello.  Pokud ne, pak **Start-AzureVM** nebo **Stop-AzureVM** je použité tooattempt toostart nebo zastavení hello virtuální počítač s tímto výsledkem hello hello požadavek uložené tooa proměnné.  Zprávu se pak odešlou toooutput zadání zda byl úspěšně odeslán požadavek toostart hello nebo zastavení.

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o práci s podřízené runbooky, najdete v části [podřízené runbooky ve službě Azure Automation](automation-child-runbooks.md)
* Další informace o toolearn výstup zprávy v průběhu spuštění sady runbook a protokolování toohelp řešení najdete v tématu [Runbook výstup a zprávy v Azure Automation.](automation-runbook-output-and-messages.md)

