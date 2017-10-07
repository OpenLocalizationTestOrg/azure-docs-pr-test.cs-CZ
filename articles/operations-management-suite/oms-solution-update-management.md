---
title: "aaaUpdate řešení pro správu v OMS | Microsoft Docs"
description: "Tento článek je určený toohelp víte, jak toouse toto řešení toomanage aktualizace pro systém Windows a Linux počítače."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: e33ce6f9-d9b0-4a03-b94e-8ddedcc595d2
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/27/2017
ms.author: magoedte
ms.openlocfilehash: 2dd321913bf049ab1996fd60a2f74b8417084dcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-management-solution-in-oms"></a>Aktualizace řešení pro správu v OMS

![Symbol správy aktualizací](./media/oms-solution-update-management/update-management-symbol.png)

Hello řešení správy aktualizací v OMS můžete toomanage aktualizací zabezpečení operačního systému pro Windows a Linux počítače nasazené v Azure, místní prostředí nebo jiných poskytovatelů cloudu.  Můžete rychle vyhodnocovat hello stavu k dispozici aktualizace na všech počítačích agenta a spravovat hello procesu instalace požadovaných aktualizací pro servery.


## <a name="solution-overview"></a>Přehled řešení
Počítačů spravovaných přes OMS použijte hello k provedení nasazení hodnocení a aktualizace:

* Agenta OMS pro Windows nebo Linux
* Konfiguraci požadovaného stavu (DSC) PowerShellu pro Linux
* Funkci Hybrid Runbook Worker služby Automation
* Microsoft Update nebo Windows Server Update Services pro počítače s Windows

Následující diagramy Hello ukazuje, že koncepční zobrazení hello chování a data toku s jak hello řešení vyhodnocuje a použije tooall aktualizace zabezpečení připojení serveru Windows a Linux počítačů v pracovním prostoru.    

#### <a name="windows-server"></a>Windows Server
![Proces správy aktualizací pro Windows Server](media/oms-solution-update-management/update-mgmt-windows-updateworkflow.png)

#### <a name="linux"></a>Linux
![Proces správy aktualizací pro Linux](media/oms-solution-update-management/update-mgmt-linux-updateworkflow.png)

Po hello počítač provede kontrolu shody aktualizací, hello OMS agent předává informace hello v hromadné tooOMS. Na počítači okno kontroly dodržování předpisů hello probíhá ve výchozím nastavení každých 12 hodin.  Kromě toho plánu vyhledávání toohello hello kontroly shody aktualizací se zahájí do 15 minut, pokud hello Microsoft Monitoring Agent (MMA) je restartovat, předchozí tooupdate instalace a po instalaci aktualizace.  Počítač se systémem Linux hello kontroly dodržování předpisů se ve výchozím nastavení provádí každých 3 hodiny a kontrolu kompatibility se zahájí do 15 minut, pokud je restartován agenta MMA hello.  

Hello informace o kompatibilitě se pak zpracovat a souhrnu v řídicí panely hello součástí řešení hello nebo vyhledávat pomocí uživatelem definované nebo pre-definovaných dotazy.  Hello řešení sestavy jak aktuální hello počítače je založena na tom, jaký zdroj jsou nakonfigurované toosynchronize s.  Pokud je počítač se systémem Windows hello nakonfigurované tooreport tooWSUS, v závislosti na tom, kdy WSUS poslední synchronizovat se službou Microsoft Update, výsledky hello může lišit od co Microsoft Updates zobrazuje.  Hello stejná pro počítače Linux, které jsou nakonfigurované tooreport tooa místní úložiště a veřejného úložiště.   

Můžete nasadit a instalaci aktualizací softwaru do počítačů, které vyžadují aktualizace hello vytvořením plánované nasazení.  Aktualizace klasifikované jako *volitelné* nejsou zahrnuty v oboru hello nasazení pro počítače se systémem Windows, pouze požadované aktualizace.  Hello plánované nasazení definuje jaké cílových počítačích obdrží hello příslušné aktualizace, buď explicitně zadat počítače, nebo výběrem [skupinu počítačů](../log-analytics/log-analytics-computer-groups.md) který je založen na protokolu hledání konkrétní sadu počítače.  Také určete plán tooapprove a určit dobu čas, kdy jsou aktualizace povoleno toobe nainstalovány v zařízení.  Aktualizace se instalují podle runbooků ve službě Azure Automation.  Tyto runbooky není možné zobrazit a nevyžadují žádnou konfiguraci.  Při nasazení aktualizace, vytvoří plán, spustí hlavní aktualizace runbooku na hello určeného dobu hello zahrnuté počítače.  Tento hlavní runbook spouští podřízený runbook na každém agentovi, který provádí instalaci požadovaných aktualizací.       

V hello datum a čas zadaný v nasazení aktualizace hello hello cílových počítačích spustí hello nasazení paralelně.  Kontroly je první provádět tooverify hello aktualizací se stále vyžadují a nainstaluje je.  Je důležité toonote pro klientských počítačů WSUS, pokud hello aktualizace nejsou schváleny ve službě WSUS, hello aktualizace nasazení se nezdaří.  výsledky Hello hello použít aktualizací jsou předávány tooOMS toobe zpracovat a souhrnu v řídicí panely hello nebo hello hledání hello události.     

## <a name="prerequisites"></a>Požadavky
* řešení Hello podporuje provádění vyhodnocení aktualizace pro systém Windows Server 2008 a vyšší a aktualizovat nasazení pro Windows Server 2008 R2 SP1 a vyšší.  Možnosti instalace Server Core a Nano Server nejsou podporované.

    > [!NOTE]
    > Podpora pro nasazení aktualizace tooWindows Server 2008 R2 SP1 vyžaduje rozhraní .NET Framework 4.5 a WMF 5.0 nebo novější.
    >  
* Klientské operační systémy Windows nejsou podporované.  
* Agenty se systémem Windows musí být nakonfigurované toocommunicate serveru Windows Server Update Services (WSUS) nebo mají přístup tooMicrosoft aktualizace.  

    > [!NOTE]
    > agent webu Windows Hello nelze spravovat souběžně nástrojem System Center Configuration Manager.  
    >
* CentOS 6 (x86/x64) a 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) a 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) a 12 (x64)  
* Ubuntu 12.04 LTS a novější x86/x64   
    > [!NOTE]  
    > aktualizace tooavoid aplikovány mimo okno údržby na Ubuntu, překonfigurujte Unattended Upgrade balíčku toodisable automatické aktualizace. Informace o tom tooconfigure, najdete v tématu [tématu Automatické aktualizace v hello Průvodce Ubuntu Server](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

* Agenty Linux musí mít přístup tooan aktualizace úložiště.  

    > [!NOTE]
    > OMS agenta pro Linux nakonfigurována pracovních prostorů OMS toomultiple tooreport není podporován s tímto řešením.  
    >

Další informace o tom, jak tooinstall hello OMS agenta pro Linux a stáhnout nejnovější verzi hello, najdete v části příliš[Operations Management Suite agenta pro Linux](https://github.com/microsoft/oms-agent-for-linux).  Informace o tom, jak tooinstall hello OMS agenta pro Windows, přečtěte si [Operations Management Suite agenta pro Windows](../log-analytics/log-analytics-windows-agents.md).  

### <a name="permissions"></a>Oprávnění
V pořadí toocreate nasazení aktualizací, musíte toobe udělena role Přispěvatel hello v účtu Automation a pracovní prostor analýzy protokolů.  

## <a name="solution-components"></a>Součásti řešení
Toto řešení se skládá z hello následující prostředky, které jsou přidány tooyour účet Automation a přímo připojených agentů nebo Operations Manager připojené skupiny pro správu.

### <a name="management-packs"></a>Sady Management Pack
Pokud pracovní prostor OMS tooan připojené skupině pro správu System Center Operations Manager se hello následující sady management Pack jsou nainstalovány v nástroji Operations Manager.  Tyto sady Management Pack se po přidání tohoto řešení nainstalují také na přímo připojené počítače s Windows. Není co tooconfigure nebo spravovat pomocí těchto sad management Pack.

* Aktualizace Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Aktualizace sady pro správu nasazení

Další informace o tom, jak jsou aktualizovány sady management Pack řešení najdete v tématu [tooLog připojení nástroje Operations Manager Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="hybrid-worker-groups"></a>Skupiny Hybrid Worker
Povolíte-li toto řešení, všechny počítače se systémem Windows přímo spojeny tooyour pracovním prostorem OMS je automaticky nakonfigurované jako sady runbook hybridní pracovní proces Runbooku toosupport hello zahrnuté v tomto řešení.  Pro každý počítač systému Windows, spravuje hello řešení, zobrazí se v části hello skupinám Hybrid Runbook Worker okno účtu Automation hello následující zásady vytváření názvů hello *Hostname FQDN_GUID*.  Tyto skupiny nemůžete vybrat jako cíl runbooků ve vašem účtu, jinak se runbooky nezdaří. Tyto skupiny jsou jenom zamýšlený toosupport hello řešení pro správu.   

Můžete však přidat hello Windows počítače tooa hybridní pracovní proces Runbooku skupinu na vaše toosupport účet Automation runbooků služeb automatizace, tak dlouho, dokud používáte stejný účet pro řešení hello i členství ve skupině hybridní pracovní proces Runbooku hello.  Tato funkce byla přidána tooversion 7.2.12024.0 hello hybridní pracovní proces Runbooku.  

## <a name="configuration"></a>Konfigurace
Proveďte následující kroky tooadd hello správy aktualizací řešení tooyour pracovním prostorem OMS hello a potvrďte, že se hlásí agenty. Pracovní prostor již připojené tooyour agentů systému Windows se přidávají automaticky žádnou další konfiguraci.

Můžete nasadit řešení hello pomocí hello následující metody:

* Z Azure Marketplace v hello portálu Azure tak, že vyberete hello automatizace a řízení nabídky nebo řešení správy aktualizací
* Z hello OMS Galerie řešení v pracovním prostoru OMS

Pokud již máte účet Automation a pracovním prostorem OMS propojené společně v hello stejnou skupinu prostředků a oblasti, výběr automatizace a řízení bude ověřte konfiguraci a pouze instalaci hello řešení a jeho konfiguraci v obou služeb.  Výběr řešení správy aktualizací hello z Azure Marketplace doručí hello stejné chování.  Pokud nemáte buď službám nasazeným v rámci vašeho předplatného, postupujte podle kroků hello v hello **vytvořte nové řešení** okno a potvrďte chcete tooinstall hello jiných předem vybraná doporučená řešení.  Volitelně můžete přidat tooyour řešení správy aktualizací hello pracovním prostorem OMS pomocí hello kroky popsané v [řešení přidat OMS](../log-analytics/log-analytics-add-solutions.md) z hello Galerie řešení.  

### <a name="confirm-oms-agents-and-operations-manager-management-group-connected-toooms"></a>Potvrdit agentů OMS a tooOMS připojené skupiny správy nástroje Operations Manager

tooconfirm přímo připojené OMS agenta pro Linux a Windows komunikuje se OMS, po několika minutách můžete spustit hello následující hledání protokolů:

* Linux – `Type=Heartbeat OSType=Linux | top 500000 | dedup SourceComputerId | Sort Computer | display Table`.  

* Windows – `Type=Heartbeat OSType=Windows | top 500000 | dedup SourceComputerId | Sort Computer | display Table`

Na počítači s Windows můžete zkontrolovat následující připojení agenta tooverify s OMS hello:

1.  Otevřete Microsoft Monitoring Agent v Ovládacích panelech a na hello **Azure Log Analytics (OMS)** kartě, hello agenta zobrazí zpráva s oznámením: **hello agenta Microsoft Monitoring Agent byl úspěšně připojen toohello Microsoft Služba Operations Management Suite**.   
2.  Otevřete hello protokolu událostí systému Windows, přejděte příliš**aplikace a Správce služby Logs\Operations** a vyhledejte události ID 3000 a 5002 ze zdroje konektor služby.  Tyto události označují hello počítač zaregistrován s pracovním prostorem OMS hello a používá konfiguraci.  

Pokud hello agent není možné toocommunicate s hello OMS služby a je nakonfigurovaný toocommunicate s hello Internetu prostřednictvím brány firewall nebo proxy server, potvrzení hello brány firewall nebo proxy serveru je správně nakonfigurovaná kontrolou [sítě konfigurace pro agenta Windows](../log-analytics/log-analytics-windows-agents.md#network) nebo [konfiguraci sítě pro agenta systému Linux](../log-analytics/log-analytics-agent-linux.md#network).

> [!NOTE]
> Pokud vaše systémy Linux jsou nakonfigurované toocommunicate proxy nebo brány OMS a jsou registrace toto řešení, aktualizujte prosím hello *proxy.conf* oprávnění toogrant hello omiuser skupiny oprávnění ke čtení pro soubor hello provádění hello následující příkazy:  
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`  
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`


Stav nově přidaných agentů systému Linux bude po provedení vyhodnocení **Aktualizovaný**.  Tento proces může trvat až too6 hodin.

Skupina pro správu komunikuje s OMS, najdete v části tooconfirm nástroje Operations Manager [ověření integrace nástroje Operations Manager s OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms).

## <a name="data-collection"></a>Shromažďování dat
### <a name="supported-agents"></a>Podporovaní agenti
Hello následující tabulka popisuje hello připojené zdroje, které podporuje toto řešení.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| Agenti systému Windows |Ano |řešení Hello shromažďuje informace o aktualizacích systému z agentů v systému Windows a zahájí instalaci požadovaných aktualizací. |
| Agenti systému Linux |Ano |řešení Hello shromažďuje informace o aktualizacích systému od agentů Linux a zahájí instalaci požadovaných aktualizací v podporovaných distribucích. |
| Skupina pro správu Operations Manageru |Ano |řešení Hello shromažďuje informace o aktualizacích systému od agentů v připojené skupiny pro správu.<br>Přímé připojení z tooLog agenta nástroje Operations Manager hello Analytics se nevyžaduje. Data se předají z hello správy skupiny toohello OMS úložiště. |
| Účet služby Azure Storage |Ne |Úložiště Azure neobsahuje informace o aktualizacích systému. |

### <a name="collection-frequency"></a>Četnost shromažďování dat
Pro každý spravovaný počítač s Windows se kontrola provádí dvakrát denně. Každých 15 minut hello rozhraní API systému Windows je volána, tooquery pro hello toodetermine čas poslední aktualizace Pokud došlo ke změně stavu a v takovém případě se zahájí kontrolu dodržování předpisů.  Pro každý spravovaný počítač s Linuxem se kontrola provádí každé tři hodiny.

Ho může trvat 30 minut až hodin too6 pro hello řídicí panel toodisplay aktualizovat data ze spravovaných počítačů.   

## <a name="using-hello-solution"></a>Pomocí řešení hello
Když přidáte hello správy aktualizací řešení tooyour pracovním prostorem OMS, hello **správy aktualizací** dlaždice se přidá tooyour OMS řídicího panelu. Tuto dlaždici zobrazí počet a grafické znázornění hello počet počítačů ve vašem prostředí a jejich dodržování předpisů aktualizací.<br><br>
![Dlaždice Souhrn Správy aktualizací](media/oms-solution-update-management/update-management-summary-tile.png)  


## <a name="viewing-update-assessments"></a>Zobrazení posouzení aktualizací
Klikněte na hello **správy aktualizací** dlaždice tooopen hello **správy aktualizací** řídicího panelu.<br><br> ![Řídicí panel Souhrn Správy aktualizací](./media/oms-solution-update-management/update-management-dashboard.png)<br>

Tento řídicí panel poskytuje podrobný přehled stavu aktualizací rozdělený podle typu operačního systému a klasifikace aktualizace – důležitá aktualizace, aktualizace zabezpečení, nebo jiná (například aktualizace definic). Hello má za následek každou dlaždici na tomto řídicím panelu se projeví pouze aktualizace, které jsou schváleny pro nasazení, která je založena na základě hello počítače synchronizace zdroje.   Hello **nasazení aktualizací** dlaždici při výběru, vás přesměruje toohello nasazení aktualizací stránky, kde můžete zobrazit plány nasazení právě probíhá dokončené nasazení, nebo naplánovat nové nasazení.  

Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím na dlaždici konkrétní hello nebo toorun dotazu určité kategorie a předem definovaných kritérií, vyberte ze seznamu hello k dispozici v části hello **běžné dotazy aktualizace** sloupce.    

## <a name="installing-updates"></a>Instalace aktualizací
Jakmile aktualizace byly vyhodnoceny pro všechny hello Linux a počítače se systémem Windows v pracovním prostoru, můžete máte požadované aktualizace jsou nainstalované vytvořením *nasazení aktualizace*.  Nasazení aktualizací je plánovaná instalace požadovaných aktualizací pro jeden nebo více počítačů.  Zadejte hello datum a čas pro nasazení hello kromě tooa počítač nebo skupinu počítačů, které mají být zahrnuty v oboru hello nasazení.  toolearn Další informace o skupiny počítačů, najdete v části [skupiny počítačů v analýzy protokolů](../log-analytics/log-analytics-computer-groups.md).  Když zahrnete skupiny počítačů ve vašem nasazení aktualizace, memnbership skupiny je jenom jednou vyhodnocovány v hello čas vytvoření plánu.  Skupiny tooa následné změny se neprojeví.  toowork řešení, odstraňte hello naplánované aktualizace nasazení a znovu ji vytvořte.

> [!NOTE]
> Virtuální počítače Windows nasadit z Azure Marketplace ve výchozím nastavení jsou nastaveny tooreceive automatické aktualizace z aktualizační služby Windows hello.  Toto chování se nezmění po přidání tohoto řešení nebo tooyour prostoru virtuálních počítačů Windows.  Když je aktivně spravovaná aktualizace s tímto řešením, hello výchozí chování (Automatické aktualizace) se použijí.  

Pro virtuální počítače vytvořené z hello na vyžádání Red Hat Enterprise Linux (RHEL) bitové kopie k dispozici v Azure Marketplace, jsou registrované tooaccess hello [Red Hat aktualizace infrastruktury (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) nasazené v Azure.  Jakýkoli jiný distribuční Linux musí být aktualizován z hello distribucích souboru online úložiště následující jejich podporovaných metod.  

### <a name="viewing-update-deployments"></a>Zobrazení nasazení aktualizace
Klikněte na tlačítko hello **nasazení aktualizace** dlaždice tooview hello seznamu existující nasazení aktualizací.  Jsou seskupené podle stavu – **Naplánované**, **Spuštěné** a **Dokončeno**.<br><br> ![Stránka Plán nasazení aktualizace](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Vlastnosti Hello zobrazí pro každé nasazení aktualizace jsou popsané v následující tabulka hello.

| Vlastnost | Popis |
| --- | --- |
| Name (Název) |Název hello nasazení aktualizace. |
| Plán |Typ plánu  Dostupné možnosti jsou *Jednou*, *Týdenní opakování* nebo *Měsíční opakování*. |
| Čas spuštění |Datum a čas, které hello nasazení aktualizace je naplánované toostart. |
| Doba trvání |Počet minut hello nasazení aktualizace je povolené toorun.  Pokud všechny aktualizace nejsou nainstalované v rámci této hodnotě duration, pak musí čekat hello zbývající aktualizace hello další nasazení aktualizace. |
| Servery |Počet počítačů ovlivněných hello nasazení aktualizace.  |
| Status |Aktuální stav hello nasazení aktualizace.<br><br>Možné hodnoty:<br>- Není spuštěné<br>- Spuštěné<br>- Dokončeno |

Vyberte dokončené nasazení aktualizace tooview hello podrobností obrazovky který hello následující tabulka obsahuje sloupce hello.  Tyto sloupce nebude vyplněný, když má hello nasazení aktualizace není dosud spuštěna.<br><br> ![Přehled výsledků nasazení aktualizace](./media/oms-solution-update-management/update-management-deploymentresults-dashboard.png)

| Sloupec | Popis |
| --- | --- |
| **Zobrazení počítačů** | |
| Počítače s Windows |Určuje počet hello počítače se systémem Windows v hello nasazení aktualizace podle stavu.  Klikněte na stav toorun protokolu vyhledávání vrací všechny aktualizace záznamů s tímto stavem pro hello nasazení aktualizace. |
| Počítače s Linuxem |Uvádí hello počet počítačů se systémem Linux v hello nasazení aktualizace podle stavu.  Klikněte na stav toorun protokolu vyhledávání vrací všechny aktualizace záznamů s tímto stavem pro hello nasazení aktualizace. |
| Stav instalace počítače |Obsahuje seznam hello počítačům účastnící se hello nasazení aktualizací a procento hello aktualizací, které úspěšně nainstalován. Klikněte na jednu z toorun hello položky protokolu vyhledávání vrací všechny chybějící a důležité aktualizace. |
| **Zobrazení aktualizací** | |
| Aktualizace pro Windows |Obsahuje seznam aktualizací systému Windows, které jsou součástí hello nasazení aktualizace a jejich stav instalace pro jednotlivé aktualizace.  Vyberte aktualizaci toorun protokolu vyhledávání vrací všechny aktualizovat záznamy pro tuto konkrétní aktualizaci nebo kliknutím na stav toorun hello protokolu vyhledávání vrací všechny záznamy pro hello nasazení aktualizace. |
| Aktualizace pro Linux |Obsahuje seznam aktualizací Linux, které jsou součástí hello nasazení aktualizace a jejich stav instalace pro jednotlivé aktualizace.  Vyberte aktualizaci toorun protokolu vyhledávání vrací všechny aktualizovat záznamy pro tuto konkrétní aktualizaci nebo kliknutím na stav toorun hello protokolu vyhledávání vrací všechny záznamy pro hello nasazení aktualizace. |

### <a name="creating-an-update-deployment"></a>Vytvoření nasazení aktualizace
Vytvořit nové nasazení aktualizace kliknutím na tlačítko hello **přidat** tlačítko hello horní části hello obrazovky tooopen hello **nové nasazení aktualizace** stránky.  Je nutné zadat hodnoty pro vlastnosti hello v hello následující tabulka.

| Vlastnost | Popis |
| --- | --- |
| Name (Název) |Nasazení aktualizace tooidentify hello jedinečný název. |
| Časové pásmo |Toouse časové pásmo pro čas spuštění hello. |
| Typ plánu | Typ plánu  Dostupné možnosti jsou *Jednou*, *Týdenní opakování* nebo *Měsíční opakování*.  
| Čas spuštění |Datum a čas toostart hello nasazení aktualizace. **Poznámka:** hello skončí nejdřív. můžete spustit nasazení je 30 minut od aktuálního času, pokud potřebujete toodeploy okamžitě. |
| Doba trvání |Počet minut hello nasazení aktualizace je povolené toorun.  Pokud všechny aktualizace nejsou nainstalované v rámci této hodnotě duration, pak musí čekat hello zbývající aktualizace hello další nasazení aktualizace. |
| Počítače |Názvy počítačů nebo skupin tooinclude počítače a cíl v hello nasazení aktualizace.  Hello rozevíracím seznamu vyberte jeden nebo více položek. |

<br><br> ![Stránka Nasazení nové aktualizace](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Časové rozmezí
Ve výchozím nastavení hello rozsah dat hello analyzovat v hello řešení pro správu aktualizací je ze všech připojených skupin pro správu generované v rámci hello poslední 1 den.

Vyberte toochange hello časové rozmezí dat hello **na základě dat** hello horní části řídicího panelu hello. Můžete vybrat záznamy vytvořeny nebo aktualizovány v rámci hello posledních 7 dnů, 1 den nebo 6 hodin. Nebo můžete vybrat **Vlastní** a zadat vlastní rozsah dat.

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Hello řešení pro správu aktualizací vytvoří dva typy záznamů v úložišti OMS hello.

### <a name="update-records"></a>Záznamy typu Aktualizace
Záznam typu **Aktualizace** se vytvoří pro každou aktualizaci, která je nainstalovaná nebo která je potřeba v každém počítači. Aktualizace záznamů mít hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
| --- | --- |
| Typ |*Aktualizace* |
| SourceSystem |Hello zdroj, který schválení instalace aktualizace hello.<br>Možné hodnoty:<br>- Microsoft Update<br>- Windows Update<br>- SCCM<br>- Servery Linux (získané ze správců balíčků) |
| Schválené |Určuje, zda text hello aktualizace byla schválena pro instalaci.<br> Pro servery Linux je tento postup momentálně volitelné, protože OMS nespravuje opravy. |
| Klasifikace pro Windows |Klasifikace aktualizace hello.<br>Možné hodnoty:<br>-    Aplikace<br>- Důležité aktualizace<br>- Aktualizace definic<br>- Balíčky funkcí<br>– Aktualizace zabezpečení<br>- Aktualizace Service Pack<br>- Kumulativní aktualizace<br>- Aktualizace |
| Klasifikace pro Linux |Cassification hello aktualizace.<br>Možné hodnoty:<br>- Důležité aktualizace<br>– Aktualizace zabezpečení<br>- Další aktualizace |
| Počítač |Název počítače hello. |
| InstallTimeAvailable |Určuje, zda je k dispozici z jiných doba instalace hello agenty, kteří nainstalováni hello stejné aktualizace. |
| InstallTimePredictionSeconds |Odhadovaná doba instalace v sekundách podle dalších agentů, které nainstalované hello stejnou aktualizaci. |
| KBID |ID článku hello KB, který popisuje hello aktualizace. |
| ManagementGroupName |Název skupiny pro správu hello SCOM agenty.  Pro ostatní agenty to je AOI-<workspace ID>. |
| MSRCBulletinID |ID bulletinu zabezpečení Microsoft hello popisující hello aktualizace. |
| MSRCSeverity |Závažnost hello bulletinu zabezpečení.<br>Možné hodnoty:<br>- Kritická<br>- Důležitá<br>- Střední |
| Nepovinné |Určuje, zda text hello aktualizace volitelné. |
| Produkt |Název hello produktu hello aktualizace je pro.  Klikněte na tlačítko **zobrazení** tooopen hello článek v prohlížeči. |
| PackageSeverity |Hello závažnost ohrožení zabezpečení hello pevné v této aktualizaci vykazované hello Linux distro dodavateli. |
| PublishDate |Datum a čas, který hello aktualizace byla nainstalována. |
| RebootBehavior |Určuje, pokud aktualizace hello vynutí restartování.<br>Možné hodnoty:<br>- canrequestreboot<br>- neverreboots |
| RevisionNumber |Číslo revize hello aktualizace. |
| SourceComputerId |Identifikátor GUID toouniquely identifikovat hello počítače. |
| TimeGenerated |Datum a čas, který hello záznam byl naposledy aktualizován. |
| Název |Název hello aktualizace. |
| UpdateID |Identifikátor GUID toouniquely identifikovat hello aktualizace. |
| UpdateState |Určuje, zda je na tomto počítači nainstalována aktualizace hello.<br>Možné hodnoty:<br>-Nainstalován - hello aktualizace je v tomto počítači nainstalována.<br>-Aktualizace hello potřeby – není nainstalována a je potřeba na tomto počítači. |

Při vyhledávání všech protokolu, který vrátí záznamy s typem **aktualizace** vyberete hello **aktualizace** zobrazení, která se zobrazí sada dlaždice shrnutí aktualizací hello vrácený hello vyhledávání. Kliknutím na hello položek v hello **chybí a použitých aktualizací** a **požadované a volitelné aktualizace** dlaždice tooscope hello zobrazení toothat sadu aktualizací. Vyberte hello **seznamu** nebo **tabulky** zobrazení jednotlivých záznamů tooreturn hello.<br>

![Zobrazení aktualizace hledání v protokolu s typem záznamu Aktualizace](./media/oms-solution-update-management/update-la-view-updates.png)  

V hello **tabulky** zobrazení, můžete kliknutím na hello **KBID** pro všechny záznamů tooopen prohlížeče s článku hello KB. Díky tomu můžete tooquickly přečtěte si informace o hello podrobnosti o konkrétní aktualizaci hello.<br>

![Zobrazení tabulky hledání v protokolu s aktualizacemi typu záznamů Dlaždice](./media/oms-solution-update-management/update-la-view-table.png)

V hello **seznamu** zobrazení, klikněte na tlačítko hello **zobrazení** odkaz Další toohello KBID tooopen hello KB článku.<br>

![Zobrazení seznamu hledání v protokolu s aktualizacemi typu záznamů Dlaždice](./media/oms-solution-update-management/update-la-view-list.png)

### <a name="updatesummary-records"></a>Záznamy UpdateSummary
Pro každý počítač s agentem Windows se vytvoří záznam typu **UpdateSummary** . Tento záznam se aktualizuje pokaždé, když je kontrolován hello počítač aktualizací. **UpdateSummary** záznamy mají vlastnosti hello v hello následující tabulka.

| Vlastnost | Popis |
| --- | --- |
| Typ |UpdateSummary |
| SourceSystem |OpsManager |
| Počítač |Název počítače hello. |
| CriticalUpdatesMissing |Počet kritické aktualizace na hello počítači chybí. |
| ManagementGroupName |Název skupiny pro správu hello SCOM agenty. Pro ostatní agenty to je AOI-<workspace ID>. |
| NETRuntimeVersion |Verze runtime rozhraní .NET hello hello v počítači nainstalována. |
| OldestMissingSecurityUpdateBucket |Bucket toocategorize hello čas od hello nejstarší chybějící zabezpečení v tomto počítači byla aktualizace publikována.<br>Možné hodnoty:<br>- Starší<br>-    před 180 dny<br>- před 150 dny<br>-    před 120 dny<br>- před 90 dny<br>- před 60 dny<br>-    před 30 dny<br>-    Poslední |
| OldestMissingSecurityUpdateInDays |Počet dní od hello nejstarší chybějící zabezpečení v tomto počítači byla aktualizace publikována. |
| OsVersion |Verze hello operační systém nainstalovaný na počítači hello. |
| OtherUpdatesMissing |Počet jiné aktualizace na hello počítači chybí. |
| SecurityUpdatesMissing |Počet aktualizací zabezpečení na počítači hello chybí. |
| SourceComputerId |Identifikátor GUID toouniquely identifikovat hello počítače. |
| TimeGenerated |Datum a čas, který hello záznam byl naposledy aktualizován. |
| TotalUpdatesMissing |Celkový počet aktualizací na hello počítači chybí. |
| WindowsUpdateAgentVersion |Číslo verze agenta Windows Update hello v počítači hello. |
| WindowsUpdateSetting |Nastavení pro jak hello počítač bude instalovat důležité aktualizace.<br>Možné hodnoty:<br>- Zakázáno<br>- Upozornění před instalací<br>- Plánovaná instalace |
| WSUSServer |Adresa URL služby WSUS serveru, je-li počítač hello nakonfigurovat toouse jeden. |

## <a name="sample-log-searches"></a>Ukázky hledání v protokolech
Hello následující tabulka obsahuje ukázkový protokol hledání aktualizace záznamů shromažďují toto řešení.

| Dotaz | Popis |
| --- | --- |
| Type:Update OSType!=Linux UpdateState=Needed Optional=false Approved!=false &#124; measure count() by Computer |Počítače serveru s Windows, které potřebují aktualizace |
| Type:Update OSType=Linux UpdateState!="Not needed" &#124; measure count() by Computer |Servery s Linuxem, které potřebují aktualizace | 
| Type=Update UpdateState=Needed Optional=false &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |Všechny počítače s chybějícími aktualizacemi |
| Type=Update UpdateState=Needed Optional=false Computer="COMPUTER01.contoso.com" &#124; select Computer,Title,KBID,Product,UpdateSeverity,PublishedDate |Chybějící aktualizace v určitém počítači (nahraďte hodnotu názvem svého počítače)|
| Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") |Všechny počítače s chybějícími důležitými aktualizacemi nebo aktualizacemi zabezpečení | 
| Type=Update UpdateState=Needed Optional=false (Classification="Security Updates" OR Classification="Critical Updates") Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual &#124; Distinct Computer} &#124; Distinct KBID |Důležité aktualizace nebo aktualizace zabezpečení vyžadované počítači, kde se aktualizace používají ručně |
| Type=Event EventLevelName=error Computer IN {Type=Update (Classification="Security Updates" OR Classification="Critical Updates") UpdateState=Needed Optional=false &#124; Distinct Computer} |Chybové události pro počítače s chybějícími požadovanými důležitými aktualizacemi nebo aktualizacemi zabezpečení |
| Type=Update Optional=false Classification="Update Rollups" UpdateState=Needed &#124; select Computer,Title,KBID,Classification,UpdateSeverity,PublishedDate |Všechny počítače s chybějícími kumulativními aktualizacemi | 
| Type=Update UpdateState=Needed Optional=false &#124; Distinct Title |Konkrétní chybějící aktualizace ve všech počítačích | 
| Type:UpdateRunProgress InstallationStatus=failed &#124; measure count() by Computer, Title, UpdateRunName |Počítač serveru s Windows a aktualizacemi, které se nezdařily při hromadné postupné aktualizaci | 
| Type:UpdateRunProgress InstallationStatus=failed &#124; measure count() by Computer, Product, UpdateRunName |Server s Linuxem a aktualizacemi, které se nezdařily při hromadné postupné aktualizaci | 
| Type=UpdateSummary &#124; measure count() by WSUSServer |Členství počítačů WSUS | 
| Type=UpdateSummary &#124; measure count() by WindowsUpdateSetting |Automatická konfigurace aktualizace | 
| Type=UpdateSummary WindowsUpdateSetting=Manual |Počítače se zakázanými automatickými aktualizacemi | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" &#124; measure count() by Computer |Seznam všech počítačů hello Linux, které mají k dispozici aktualizace balíčku | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") &#124; measure count() by Computer |Seznam všech počítačů hello Linux, které mají k dispozici aktualizace balíčku, který řeší chybu zabezpečení, důležité aktualizace nebo zabezpečení | 
| Type=Update and OSType=Linux and UpdateState!="Not needed" |Seznam všech balíčků, které mají k dispozici aktualizaci | 
| Type=Update  and OSType=Linux and UpdateState!="Not needed" and (Classification="Critical Updates" OR Classification="Security Updates") |Seznam všech balíčků pro řešení kritické chyby nebo chyby zabezpečení | 
| Type:UpdateRunProgress &#124; measure Count() by UpdateRunName |Vypsat nasazení aktualizací, která provedla úpravy počítačů | 
| Type:UpdateRunProgress UpdateRunName="DeploymentName" &#124; measure Count() by Computer |Počítače aktualizované při této hromadné postupné aktualizaci (nahraďte hodnotu názvem vašeho nasazení aktualizací) | 
| Type=Update and OSType=Linux and OSName = Ubuntu &#124; measure count() by Computer |Seznam všech počítačů "Ubuntu" hello s nejsou k dispozici žádné aktualizace. | 

## <a name="troubleshooting"></a>Řešení potíží

Tato část obsahuje informace o toohelp řešení problémů s hello řešení pro správu aktualizací.  

### <a name="how-do-i-troubleshoot-onboarding-issues"></a>Jak řešit potíže s připojováním?
Pokud dojde k potížím při pokusu o tooonboard hello řešení nebo virtuální počítač, zkontrolujte hello **aplikace a Správce služby Logs\Operations** protokolu událostí pro události se události ID 4502 a událostí zprávu obsahující **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.  Následující tabulka Hello označuje specifické chybové zprávy a možným řešením pro každou.  

| Zpráva | Důvod | Řešení |   
|----------|----------|----------|  
| Nelze tooRegister počítače pro správu oprava<br>registrace se nezdařila s výjimkou<br>System.InvalidOperationException: {"Zpráva":"Počítač už je<br>registrovaný tooa jiný účet. "} | Počítač je již zařazený nemá tooanother prostoru pro správu aktualizací | Vyčistit stará artefaktů podle [odstraňuje se skupina runbook hybridní hello](../automation/automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups)|  
| Nelze zaregistrovat příliš počítače pro správu oprava<br>registrace se nezdařila s výjimkou<br>System.Net.Http.HttpRequestException: Došlo k chybě při odesílání požadavku hello. ---><br>System.Net.WebException: základní připojení hello<br>bylo uzavřeno: Došlo k neočekávané<br>chybě při příjmu. ---> System.ComponentModel.Win32Exception:<br>Nelze komunikovat Hello klient a server,<br>protože nepoužívají společný algoritmus. | Proxy server, brána nebo brána firewall blokuje komunikaci | [Zkontrolujte požadavky sítě](../automation/automation-offering-get-started.md#network-planning)|  
| Nelze tooRegister počítače pro správu oprava<br>registrace se nezdařila s výjimkou<br>Newtonsoft.Json.JsonReaderException: Chyba při analýze hodnoty kladného nekončena. | Proxy server, brána nebo brána firewall blokuje komunikaci | [Zkontrolujte požadavky sítě](../automation/automation-offering-get-started.md#network-planning)| 
| Hello certifikát předložený hello služby <wsid>. oms.opinsights.azure.com<br>nebyl vydaný certifikační autoritou<br>používanou pro služby Microsoft. Obraťte se na<br>váš správce toosee sítě Pokud používají proxy server, který zabrání<br>komunikaci prostřednictvím protokolu TLS/SSL. |Proxy server, brána nebo brána firewall blokuje komunikaci | [Zkontrolujte požadavky sítě](../automation/automation-offering-get-started.md#network-planning)|  
| Nelze tooRegister počítače pro správu oprava<br>registrace se nezdařila s výjimkou<br>AgentService.HybridRegistration.<br>PowerShell.Certificates.CertificateCreationException:<br>Nepodařilo toocreate certifikát podepsaný svým držitelem. ---><br>System.UnauthorizedAccessException: Přístup byl odepřen. | Chyba při generování certifikátu podepsaného svým držitelem | Ověřte, že má systémový účet<br>toofolder přístup pro čtení:<br>**C:\ProgramData\Microsoft\**<br>**Crypto\RSA**|  

### <a name="how-do-i-troubleshoot-update-deployments"></a>Jak mám řešit problémy s nasazeními aktualizací?
Můžete zobrazit výsledky hello hello runbook zodpovědná za nasazení aktualizací hello součástí hello naplánované aktualizace nasazení v okně úlohy hello účtu Automation, který je spojen s pracovním prostorem OMS hello podpora tohoto řešení.  Hello runbook **oprava MicrosoftOMSComputer** je podřízené sady runbook, která je cílena konkrétní spravovaný počítač, a kontrola podrobný datový proud hello nabídne podrobné informace o tomto nasazení.  výstup Hello se zobrazí, který vyžaduje aktualizace se dají použít, stahovat stav, stav instalace a další podrobnosti.<br><br> ![Stav úlohy nasazení aktualizací](media/oms-solution-update-management/update-la-patchrunbook-outputstream.png)<br>

Další informace najdete v tématu [Výstup a zprávy runbooku Automation](../automation/automation-runbook-output-and-messages.md).   

## <a name="next-steps"></a>Další kroky
* Pomocí protokolu vyhledávání v [analýzy protokolů](../log-analytics/log-analytics-log-searches.md) tooview podrobná data aktualizace.
* [Vytvářejte vlastní řídicí panely](../log-analytics/log-analytics-dashboards.md) zobrazující shodu aktualizace pro vaše spravované počítače.
* [Vytvářejte výstrahy](../log-analytics/log-analytics-alerts.md) při zjištění, že v počítačích chybí důležité aktualizace nebo že má počítač zakázané automatické aktualizace.  
