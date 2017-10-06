---
title: "aaaAzure automatizace procesů Hybrid Runbook Worker | Microsoft Docs"
description: "Tento článek obsahuje informace o instalaci a použití hybridní pracovní proces Runbooku, což je funkce služby Azure Automation, který vám umožní toorun runbooky na počítačích ve vaší místní datacenter nebo poskytovatele cloudových služeb."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: ccee35e8324149a79ff692a867e5ce7801299bcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Automatizaci prostředků v datovém centru nebo v cloudu s hybridní pracovní proces Runbooku
Runbooky ve službě Azure Automation nemají přístup k prostředkům v ostatních cloudů nebo v místním prostředí, protože spustit i v cloudu Azure hello.  Funkce Hello hybridní pracovní proces Runbooku automatizace Azure umožňuje toorun runbooky přímo na počítači hello hostování hello role s prostředky v prostředí toomanage hello těchto místních prostředků. Sady Runbook jsou uložené a spravované ve službě Azure Automation a potom doručovaná tooone nebo více určenými počítači.  

Tato funkce je zobrazená v hello následující bitové kopie:<br>  

![Přehled hybridních služby Runbook Worker](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Technický přehled aspektů hello hybridní pracovní proces Runbooku role a nasazení, naleznete v části [přehled architektury automatizace](automation-offering-get-started.md#automation-architecture-overview).    

## <a name="hybrid-runbook-worker-groups"></a>Skupinám hybrid Runbook Worker
Každý hybridní pracovní proces Runbooku je členem skupiny hybridní pracovní proces Runbooku, který zadáte při instalaci agenta hello.  Skupina může obsahovat jednoho agenta, ale můžete nainstalovat více agentů ve skupině pro vysokou dostupnost.

Při spuštění sady runbook pro Hybrid Runbook Worker, určete hello skupinu, která běží na.  Hello členy skupiny hello určit, které pracovní služeb hello požadavku.  Nelze zadat konkrétní pracovního procesu.

## <a name="relationship-tooservice-management-automation"></a>Vztah tooService Management Automation
[Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) vám umožní toorun hello stejné sady runbook, které podporuje Azure Automation v místním datovém centru. SMA se obvykle nasazuje společně s Windows Azure Pack, jako Windows Azure Pack obsahuje grafické rozhraní pro správu SMA. Na rozdíl od Azure Automation SMA vyžaduje místní instalaci, která zahrnuje webové servery toohost hello rozhraní API, runbooky toocontain databáze a konfigurace SMA a úlohy sady runbook tooexecute pracovní procesy Runbook Worker. Automatizace Azure poskytuje tyto služby v cloudu hello a pouze vyžaduje, abyste toomaintain hello procesy Hybrid Runbook Worker ve vašem místním prostředí.

Pokud jste stávajícího uživatele SMA, můžete přesunout vaše toobe automatizace sady runbook tooAzure použít s hybridní pracovní proces Runbooku s žádné změny, za předpokladu, že budou fungovat tooresources své vlastní ověřování, jak je popsáno v [spuštění sad runbook na hybridního Runbooku Pracovní](automation-hrw-run-runbooks.md).  Runbooky ve službě SMA spustit v kontextu hello hello účtu služby na serveru hello pracovního procesu, který může poskytnout, ověření pro sady runbook hello.

Můžete použít následující kritéria toodetermine, zda je vhodnější pro své požadavky na Azure Automation Hybrid Runbook Worker nebo Service Management Automation hello.

* SMA vyžaduje místní instalaci jeho základní součásti, které jsou připojené tooWindows Azure Pack, pokud je potřeba doplnil o grafické rozhraní. Další místní prostředky, je potřeba s vyšší náklady na údržbu než Azure Automation, který potřebuje pouze agenta nainstalovaného na místní runbook Worker. Hello agenti jsou spravovány agentem Operations Management Suite, další snižují náklady na údržbu.
* Automatizace Azure ukládá jeho runbooky v cloudu hello a zajišťuje jejich tooon místní hybridní pracovní procesy Runbooku. Pokud vaše zásady zabezpečení nedovolují toto chování, měli byste použít SMA.
* SMA se dodává s nástrojem System Center; a proto vyžaduje licenci System Center 2012 R2. Služby Azure Automation je založen na modelu vrstvené předplatného.
* Automatizace Azure má pokročilé funkce jako je například grafické runbooky, které nejsou k dispozici ve službě SMA.

## <a name="installing-hello-windows-hybrid-runbook-worker"></a>Instalace hello Windows hybridní pracovní proces Runbooku 

tooinstall a nakonfigurovat Windows Hybrid Runbook Worker, existují dvě metody k dispozici.  Hello doporučená metoda používá Automation runbook toocompletely automatizovat proces hello nutný tooconfigure počítači se systémem Windows.  druhé metody Hello je následující a podrobný postup toomanually instalace a konfigurace hello role.  

> [!NOTE]
> toomanage hello konfigurace serverů podpora hello hybridní pracovní proces Runbooku role s potřeby konfigurace stavu (DSC), je nutné tooadd je jako uzly DSC.  Další informace o připojování je pro správu s DSC, najdete v části [registrace počítačů pro správu Azure Automation DSC](automation-dsc-onboarding.md).           
><br>
>Pokud povolíte hello [řešení pro správu aktualizací](../operations-management-suite/oms-solution-update-management.md), všechny počítače se systémem Windows připojené tooyour pracovním prostorem OMS je automaticky nastaven na sady runbook toosupport hybridní pracovní proces Runbooku zahrnuté v tomto řešení.  Však není registrován u žádné skupiny hybridní pracovní proces již definována v účtu Automation.  Hello můžete přidat počítač tooa hybridní pracovní proces Runbooku skupiny ve vaší toosupport účet Automation runbooků služeb automatizace tak dlouho, dokud používáte hello stejný účet pro řešení hello i členství ve skupině hybridní pracovní proces Runbooku.  Tato funkce byla přidána tooversion 7.2.12024.0 hello hybridní pracovní proces Runbooku.  

Hello zkontrolujte následující informace o hello [požadavky na hardware a software](automation-offering-get-started.md#hybrid-runbook-worker) a [informace pro přípravu síti](automation-offering-get-started.md#network-planning) než začnete nasazovat hybridní pracovní proces Runbooku.  Po úspěšném nasazení služby runbook worker, zkontrolujte [spuštění sad runbook na hybridní pracovní proces Runbooku](automation-hrw-run-runbooks.md) toolearn jak tooconfigure vaší sady runbook tooautomate procesů v vašeho datového centra místní nebo jiné cloudové prostředí.  
 
### <a name="automated-deployment"></a>Automatické nasazení

Proveďte následující hello kroky tooautomate hello instalaci a konfiguraci role pracovního procesu hybridní Windows hello.  

1. Stáhnout hello *New-OnPremiseHybridWorker.ps1* skript z hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/1.0/DisplayScript) přímo z počítače hello rolí hybridní pracovní proces Runbooku hello nebo z jiného počítače ve vaší prostředí a zkopírujte ho toohello pracovního procesu.  

    Hello *New-OnPremiseHybridWorker.ps1* skript vyžaduje hello během provádění následující parametry:

  * *AutomationAccountName* (povinné) - hello název účtu Automation.  
  * *Název skupiny prostředků* (povinné) - hello název skupiny prostředků hello spojené s vaším účtem Automation.  
  * *HybridGroupName* (povinné) - hello název skupiny hybridní pracovní proces Runbooku, který zadáte jako cíl pro sady runbook hello podporuje tento scénář. 
  *  *ID předplatného* (povinné) - hello Id předplatného Azure, který váš účet automatizace je v.
  *  *WorkspaceName* (volitelné) – hello OMS název pracovního prostoru.  Pokud nemáte pracovním prostorem OMS, hello skript vytvoří a nakonfiguruje jeden.  

     > [!NOTE]
     > Aktuálně jsou hello pouze automatizace oblasti, které podporuje integraci s OMS - **Austrálie – jihovýchod**, **východní USA 2**, **jihovýchodní Asie**, a **– západ Evropa**.  Pokud není váš účet Automation v jednom z těchto oblastí, hello skript vytvoří pracovní prostor služby OMS ale ho vás varuje, že ho nelze propojit je společně.
     > 
2. V počítači, spusťte **prostředí Windows PowerShell** z hello **spustit** obrazovky v režimu správce.  
3. Hello prostředí příkazového řádku Powershellu přejděte toohello složky, která obsahuje skript hello stáhli a provést změny hello hodnot pro parametry *- AutomationAccountName*, *- ResourceGroupName* , *- HybridGroupName*, *- SubscriptionId*, a *- WorkspaceName*.

     > [!NOTE] 
     > Po spuštění skriptu hello jste výzvami tooauthenticate s Azure.  Můžete **musí** Přihlaste se pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.  
     >  
    
        .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> `
        -ResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
        -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfOMSWorkspace>

4. Jsou výzvami tooagree tooinstall **NuGet** a jsou výzvami tooauthenticate pomocí svých přihlašovacích údajů Azure.<br><br> ![Spuštění skriptu New-OnPremiseHybridWorker](media/automation-hybrid-runbook-worker/new-onpremisehybridworker-scriptoutput.png)

5. Po dokončení skriptu hello hello skupinám Hybrid Worker okno se zobrazí nová skupina hello a počet členů, nebo pokud existující skupiny, se zvýší hello počet členů.  Můžete vybrat skupinu hello seznamu hello na hello **skupinám Hybrid Worker** okno a vyberte hello **hybridní pracovní procesy** dlaždici.  Na hello **hybridní pracovní procesy** okně uvidíte každého člena skupiny hello uvedené.  

### <a name="manual-deployment"></a>Ruční nasazení 
Proveďte první dva kroky hello jednou pro vaše prostředí automatizace a potom opakujte hello zbývající kroky pro každý počítač pracovního procesu.

#### <a name="1-create-operations-management-suite-workspace"></a>1. Vytvořit pracovní prostor služby Operations Management Suite
Pokud již nemáte pracovní prostor služby Operations Management Suite, vytvořte jednu pomocí pokynů v [pracovního prostoru Správa](../log-analytics/log-analytics-manage-access.md). Pokud již účet máte, můžete použít existujícímu pracovnímu prostoru.

#### <a name="2-add-automation-solution-toooperations-management-suite-workspace"></a>2. Přidat tooOperations řešení automatizace pracovního prostoru Management Suite
Řešení přidat funkce tooOperations Management Suite.  Hello řešení služby Automation přidá funkce pro Azure Automation, včetně podpory pro hybridní pracovní proces Runbooku.  Když přidáte hello řešení tooyour prostoru, automaticky vynutí pracovní součásti toohello agenta počítač, který budete instalovat v dalším kroku hello.

Postupujte podle pokynů hello [tooadd řešení pomocí Galerie řešení hello](../log-analytics/log-analytics-add-solutions.md) tooadd hello **automatizace** pracovní prostor služby Operations Management Suite tooyour řešení.

#### <a name="3-install-hello-microsoft-monitoring-agent"></a>3. Instalace agenta Microsoft Monitoring Agent hello
počítače tooOperations Management Suite se připojuje Hello agenta Microsoft Monitoring Agent.  Při instalaci agenta hello na místním počítači a připojte ho tooyour prostoru, bude automaticky stahovat hello komponent potřebných pro hybridní pracovní proces Runbooku.

Postupujte podle pokynů hello [tooLog počítače připojit Windows Analytics](../log-analytics/log-analytics-windows-agents.md) tooinstall hello agenta v počítači místní hello.  Tento proces můžete opakovat pro více počítačů tooadd prostředí více tooyour pracovních procesů.

Když hello agent byl úspěšně připojen tooOperations Management Suite, objeví se na hello **připojené zdroje** kartě hello Operations Management Suite **nastavení** podokně.  Můžete ověřit tohoto agenta hello správně stáhla řešení služby Automation hello, pokud obsahuje složku s názvem **AzureAutomationFiles** v C:\Program Files\Microsoft Monitoring Agent\Agent.  verze hello tooconfirm hello hybridní pracovní proces Runbooku, můžete přejít tooC:\Program Files\Microsoft monitorování Agent\Agent\AzureAutomation\ a Poznámka hello \\ *verze* podsložky.   

#### <a name="4-install-hello-runbook-environment-and-connect-tooazure-automation"></a>4. Nainstalovat prostředí runbooku hello a připojte tooAzure automatizace
Když přidáte agenta tooOperations Management Suite, hello řešení služby Automation vynutí hello **HybridRegistration** modulu PowerShell, který obsahuje hello **Add-HybridRunbookWorker** rutiny.  Pomocí této rutiny tooinstall hello runbook prostředí v počítači hello a zaregistrovat ho u automatizace Azure.

Otevřete relaci prostředí PowerShell v režimu správce a spusťte následující příkazy tooimport hello modulu hello.

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

Spusťte hello **Add-HybridRunbookWorker** rutiny pomocí hello následující syntaxi:

    Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>

Můžete získat hello informace požadované pro tuto rutinu z hello **Správa klíčů** okno v hello portálu Azure.  Otevřete toto okno výběrem hello **klíče** možnost hello **nastavení** okno ve vašem účtu Automation.

![Přehled hybridních služby Runbook Worker](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** je název hello hello skupinu hybridních pracovních procesů Runbook. Pokud tato skupina již existuje v účtu automation hello, po přidání počítače aktuální hello tooit.  Pokud již neexistuje, pak se přidá.
* **Koncový bod** je hello **URL** pole hello **Správa klíčů** okno.
* **Token** je hello **primární přístupový klíč** v hello **Správa klíčů** okno.  

Použití hello **-Verbose** přepínač s **Add-HybridRunbookWorker** tooreceive podrobné informace o instalaci hello.

#### <a name="5-install-powershell-modules"></a>5. Instalace modulů prostředí PowerShell
Sady Runbook můžete použít některou z hello aktivity a rutin, které jsou definované v hello moduly nainstalované ve vašem prostředí Azure Automation.  Tyto moduly nejsou automaticky nasazené tooon místní počítače, když, proto je nutné nainstalovat ručně.  Výjimka Hello je hello Azure modul, který je nainstalován ve výchozím nastavení poskytuje toocmdlets přístup pro všechny služby Azure a aktivity pro Azure Automation.

Vzhledem k tomu, že primárním účelem hello funkce Hybrid Runbook Worker hello je toomanage místní prostředky, je pravděpodobně třeba tooinstall hello moduly, které podporují tyto prostředky.  Můžete se podívat příliš[instalaci modulů](http://msdn.microsoft.com/library/dd878350.aspx) informace o instalaci moduly prostředí Windows PowerShell.  Moduly, které jsou nainstalovány musí být v umístění odkazuje proměnná prostředí PSModulePath tak, aby automaticky importují podle hello hybridní pracovní proces.  Další informace najdete v tématu [Modifying hello PSModulePath instalační cestu](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx). 

## <a name="removing-hybrid-runbook-worker"></a>Odebrání hybridní pracovní proces Runbooku 
Jeden nebo více procesy Hybrid Runbook Worker můžete odebrat ze skupiny nebo můžete odebrat hello skupiny, v závislosti na vaše požadavky.  tooremove hybridní pracovní proces Runbooku z místního počítače, proveďte následující kroky hello.

1. V hello portálu Azure přejděte tooyour účet Automation.  
2. Z hello **nastavení** vyberte **klíče** a poznamenejte si hello hodnot pro pole **URL** a **primární přístupový klíč**.  Tyto informace jsou potřeba pro další krok hello.
3. Otevřete relaci prostředí PowerShell v režimu správce a spusťte následující hello command - `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Použití hello **-Verbose** přepínač pro podrobný protokol procesu odebrání hello.

> [!NOTE]
> Hello agenta Microsoft Monitoring Agent nebude odstraněn ze hello počítače, jenom hello funkce a konfigurace role hello hybridní pracovní proces Runbooku.  

## <a name="remove-hybrid-worker-groups"></a>Odebrat skupiny hybridní pracovní proces
tooremove skupiny, musíte nejprve tooremove hello hybridní pracovní proces Runbooku z každý počítač, který je členem skupiny hello hello postupem uvedena výše, a pak proveďte následující kroky tooremove hello skupiny hello.  

1. V hello portálu Azure otevřete účet Automation hello.
2. Vyberte hello **skupinám Hybrid Worker** dlaždici a v hello **skupinám Hybrid Worker** okně, vyberte hello skupiny chcete toodelete.  Po výběru hello konkrétní skupinu, hello **skupinu hybridních pracovních procesů** zobrazí se okno Vlastnosti.<br> ![Skupina hybridních pracovních procesů Runbook okno](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
3. V okně Vlastnosti hello hello vybrané skupiny, klikněte na tlačítko **odstranit**.  Zobrazí se zpráva s dotazem tooconfirm tuto akci, vyberte **Ano** Pokud jste si jistí, chcete tooproceed.<br> ![Dialogové okno potvrzení odstranění skupiny](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Tento proces může trvat několik sekund toocomplete a vy můžete sledovat v jeho průběhu **oznámení** nabídce hello.  

## <a name="troubleshooting"></a>Řešení potíží 
Hello hybridní pracovní proces Runbooku závisí na hello toocommunicate agenta Microsoft Monitoring Agent s Automation účet tooregister hello worker, obdrží úlohy sady runbook a zprávy o stavu. V případě selhání registrace hello pracovní Zde jsou některé možné příčiny chyby hello:  

1. hybridní pracovní proces Hello je za proxy nebo brány firewall.  
    Ověřte, zda má počítač hello odchozí přístup too*.azure-automation.net na portu 443.  

2. Hello počítače hello hybridní pracovní proces běží na má menší než minimální hardwarové hello [požadavky](automation-offering-get-started.md#hybrid-runbook-worker).  
    Počítače se systémem hello hybridní pracovní proces Runbooku by měl splňovat minimální požadavky na hardware hello před označením ho toohost tuto funkci. V závislosti na využití prostředků hello jiné procesy na pozadí a kolizí způsobené sady runbook během provádění, jinak, hello počítač se stane přetížen a způsobit zpoždění úlohy sady runbook nebo vypršení časových limitů.
   Potvrďte hello počítač určený toorun hello hybridní pracovní proces Runbooku funkce splňuje minimální hardwarové požadavky hello.  Pokud ano, sledujte všechny korelace mezi hello výkon procesů Hybrid Runbook Worker a Windows toodetermine využití procesoru a paměti.  Pokud je paměť nebo zatížení procesoru, to může znamenat tooupgrade nutné hello nebo přidáním dalších procesorů nebo paměti tooaddress zvýšení hello problémové místo prostředků a hello chybu vyřešit. Nebo vyberte jiný výpočtový prostředek, který může podporovat hello minimální požadavky a škálování při vytížení indikovat, že se o zvýšení nezbytné.
    
3. Hello službu Microsoft Monitoring Agent není spuštěna.  
    Pokud není spuštěná služba Microsoft Monitoring Agent Windows hello, nebude hello hybridní pracovní proces Runbooku komunikaci se službou Azure Automation.  Ověřte tak, že zadáte následující příkaz v prostředí PowerShell hello je spuštěn hello agent: `get-service healthservice`.  Pokud je hello služba zastavená, zadejte následující příkaz v prostředí PowerShell toostart hello služby hello: `start-service healthservice`.  

4. V hello **aplikace a Správce služby Logs\Operations** protokolu událostí se zobrazí události 4502 a obsahující EventMessage **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**s hello následující popis: *hello certifikát předložený hello služby <wsid>. oms.opinsights.azure.com nevydala certifikační autorita používaná pro služby společnosti Microsoft. Pokud používají proxy server, který zabrání komunikace TLS/SSL, obraťte se na správce toosee vaší sítě. Hello článku KB3126513 obsahuje další informace o připojení k řešení potíží.*
    Příčinou může být vašeho proxy serveru nebo síťové brány firewall blockking komunikace tooMicrosoft Azure.  Ověřte, zda má počítač hello odchozí přístup too*.azure-automation.net na porty 443.

Protokoly se ukládají místně na každém hybridní pracovní proces na C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.  Můžete zkontrolovat, zda existují jakékoli upozornění nebo chybové události zapisovat toohello **aplikace a služby Logs\Microsoft-SMA\Operations** a **aplikace a Správce služby Logs\Operations** protokolu událostí může naznačovat připojení nebo jiné problém ovlivňující registrace hello role tooAzure Automation nebo problém při provádění operací se Normální.  

## <a name="next-steps"></a>Další kroky
Zkontrolujte [spuštění sad runbook na hybridní pracovní proces Runbooku](automation-hrw-run-runbooks.md) toolearn jak tooconfigure vaší sady runbook tooautomate procesů v vašeho datového centra místní nebo jiné cloudové prostředí.
