---
title: "aaaTroubleshooting problémy běžné Azure Automation | Microsoft Docs"
description: "Tento článek obsahuje informace o řešení potíží s toohelp a opravte běžné chyby Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
tags: top-support-issue
keywords: "Chyba automatizace, řešení potíží problém"
ms.assetid: 5f3cfe61-70b0-4e9c-b892-d02daaeee07d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: sngun; v-reagie
ms.openlocfilehash: eb7d1cc5726f2b7a86c860e8f0c8340fa4221b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-common-issues-in-azure-automation"></a>Odstraňování běžných problémů ve službě Azure Automation 
Tento článek obsahuje nápovědu k odstraňování běžných chyb, může docházet v Azure Automation a navrhne tooresolve možná řešení potíží je.

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Chyby ověřování při práci se sadami runbook automatizace Azure.
### <a name="scenario-sign-in-tooazure-account-failed"></a>Scénář: Přihlaste tooAzure, účet se nezdařila.
**Chyba:** při práci s rutiny Add-AzureAccount nebo Login-AzureRmAccount hello se zobrazí chyba hello "Unknown_user_type: Neznámý typ uživatele".

**Důvod chyby hello:** k této chybě dojde, pokud název asset přihlašovacích údajů hello je neplatný nebo pokud hello uživatelské jméno a heslo, kterou jste použili asset přihlašovacích údajů automatizace hello toosetup nejsou platné.

**Tipy pro odstraňování potíží:** v pořadí toodetermine čem je problém, proveďte následující kroky hello:  

1. Ujistěte se, že nemáte žádné speciální znaky, včetně hello  **@**  znak v hello automatizace pověření název prostředku, že používáte tooconnect tooAzure.  
2. Zkontrolujte, které můžete hello uživatelské jméno a heslo, které jsou uložené v přihlašovacích údajů Azure Automation hello ve svém místním prostředí PowerShell ISE editoru. Můžete provést spuštěním následující rutiny v prostředí PowerShell ISE hello hello:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred
3. Pokud ověření selže místně, znamená to, zda nebyly správně nastavená přihlašovacích údajů Azure Active Directory. Odkazovat příliš[ověřování pomocí služby Azure Active Directory tooAzure](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blog post tooget hello Azure Active Directory účet správně nastavit.  

### <a name="scenario-unable-toofind-hello-azure-subscription"></a>Scénář: Nelze toofind hello předplatného Azure
**Chyba:** obdržíte chybu hello "hello odběr s názvem ``<subscription name>`` nebyl nalezen" při práci s rutinami Select-AzureSubscription nebo Select-AzureRmSubscription hello.

**Důvod chyby hello:** k této chybě dojde, pokud název odběru hello není platný nebo pokud hello uživatele Azure Active Directory, který se pokouší podrobnosti o předplatném hello tooget není nakonfigurován jako správce předplatného hello.

**Tipy pro odstraňování potíží:** v pořadí toodetermine, když máte správně ověřit tooAzure a máte předplatné toohello přístup se pokoušíte tooselect, trvat hello následující kroky:  

1. Ujistěte se, že spustíte hello **Add-AzureAccount** před spuštěním hello **Select-AzureSubscription** rutiny.  
2. Pokud se pořád zobrazí tato chybová zpráva, upravit kód přidáním hello **Get-AzureSubscription** rutiny s dodržením hello **Add-AzureAccount** rutiny a pak spusťte hello kód.  Nyní ověřte, zda výstup hello Get-AzureSubscription obsahuje podrobnosti o vašem předplatném.  

   * Pokud nevidíte všechny podrobnosti o předplatném ve výstupu hello, znamená to, zda text hello odběr není inicializován ještě.  
   * Pokud se zobrazí podrobnosti o předplatném hello ve výstupu hello, potvrďte, že používáte hello správné předplatné název nebo ID s hello **Select-AzureSubscription** rutiny.   

### <a name="scenario-authentication-tooazure-failed-because-multi-factor-authentication-is-enabled"></a>Scénář: Ověřování tooAzure se nezdařila, protože je povolené vícefaktorového ověřování
**Chyba:** obdržíte chybu hello "Add-AzureAccount: AADSTS50079: registrace silného ověřování (výš) se vyžaduje" při ověřování tooAzure s Azure uživatelské jméno a heslo.

**Důvod chyby hello:** Pokud máte služby Multi-Factor authentication na vašem účtu Azure, nemůžete použít tooAzure tooauthenticate uživatele Azure Active Directory.  Místo toho musíte toouse certifikát nebo tooAzure hlavní tooauthenticate služby.

**Tipy pro odstraňování potíží:** toouse certifikát s hello rutiny pro správu služby Azure, najdete příliš[vytvoření a přidání certifikátu toomanage Azure services.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) toouse hlavní název služby pomocí rutin Azure Resource Manager, najdete v příliš[vytváření služby objektu zabezpečení pomocí portálu Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md) a [ověřování hlavní název služby pomocí Azure Resource Manageru.](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Běžné chyby při práci se sadami runbook
### <a name="scenario-hello-runbook-job-start-was-attempted-three-times-but-it-failed-toostart-each-time"></a>Scénář: zahájení práce hello sady runbook došlo k pokusu o třikrát, ale neúspěšně toostart pokaždé, když
**Chyba:** runbook selže s chybou hello "" hello úlohy se pokusili třikrát ale se nezdařilo."

**Důvod chyby hello:** tato chyba může být způsobeno hello následujících důvodů:  

1. Limit paměti.  Budeme mít zdokumentovaný omezení množství paměti přidělené tooa izolovaného prostoru [omezení služby Automation](../azure-subscription-service-limits.md#automation-limits) , úloha může selhat ho pokud používá více než 400 MB paměti. 

2. Modul není kompatibilní.  Tato situace může nastat, pokud modul závislosti nejsou správné, a pokud tomu tak není, vaše sada runbook obvykle vrací "Příkaz nebyl nalezen" nebo "Nelze vytvořit vazbu parametru" zprávy. 

**Tipy pro odstraňování potíží:** některé z následujících řešení hello bude hello problém vyřešit:  

* Navrhované metody toowork v rámci omezení paměti hello jsou toosplit hello zatížení mezi několika runbooky, není zpracovat tolik data v paměti, není toowrite nepotřebné výstup z vaší sady runbook nebo zvažte kolik kontrolní body zapisovat do vašeho prostředí PowerShell runbooky pracovních postupů.  

* Potřebujete Azure moduly pro tooupdate postupem hello [jak tooupdate modulů prostředí Azure PowerShell ve službě Azure Automation](automation-update-azure-modules.md).  


### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Scénář: Runbook nezdaří z důvodu deserializovaný objekt
**Chyba:** runbook selže s chybou hello "nelze vytvořit vazbu parametru ``<ParameterName>``. Nelze převést hello ``<ParameterType>`` hodnotu typu Deserialized ``<ParameterType>`` tootype ``<ParameterType>``".

**Důvod chyby hello:** Pokud vaše sada runbook v pracovním postupu Powershellu, je uložený na komplexní objekty v deserializovaný formát v pořadí toopersist vašemu runbook stavu Pokud hello pracovní postup se pozastaví.  

**Tipy k řešení potíží:**  
Některé z následujících tří řešení hello bude řešení tohoto problému:

1. Pokud jsou potrubí komplexní objekty z jedné rutiny tooanother, zalomení tyto rutiny v InlineScript.  
2. Předejte hello name nebo value, které potřebujete z hello komplexní objekt neprochází hello celý objekt.  
3. Použijte Powershellový runbook místo runbook pracovního postupu Powershellu.  

### <a name="scenario-runbook-job-failed-because-hello-allocated-quota-exceeded"></a>Scénář: Runbook úloha se nezdařila, protože hello přidělené kvóta byla překročena.
**Chyba:** úlohu runbook selže s chybou hello "hello pro hello měsíční celkový čas běhu úloh dosáhlo kvóty pro toto předplatné".

**Důvod chyby hello:** tato chyba nastane, když hello provádění úlohy překračuje hello 500 minutu volné kvótu pro váš účet. Tato kvóta se vztahuje tooall typy spuštění úlohy například testování úlohu, spouštění úlohy z portálu hello, provádění úlohy pomocí webhooků a plánování úloh tooexecute pomocí portálu Azure buď hello nebo ve vašem datovém centru. informace o cenách pro automatizaci najdete toolearn [ceny automatizace](https://azure.microsoft.com/pricing/details/automation/).

**Tipy pro odstraňování potíží:** Pokud budete chtít toouse více než 500 minut za měsíc, budete potřebovat toochange zpracování předplatného z hello úroveň Free toohello základní vrstvě. Úroveň Basic toohello můžete upgradovat pomocí trvá hello následující kroky:  

1. Přihlaste se tooyour předplatného Azure  
2. Vyberte účet Automation hello, že chcete tooupgrade  
3. Klikněte na **nastavení** > **cenová úroveň a využití** > **cenová úroveň**  
4. Na hello **zvolte cenovou úroveň** vyberte **základní**    

### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Scénář: Rutiny nebyl rozpoznán při provádění sady runbook
**Chyba:** úlohu runbook selže s chybou hello "``<cmdlet name>``: hello termín ``<cmdlet name>`` nebyl rozpoznán jako hello název rutiny, funkce, soubor skriptu nebo spustitelného programu."

**Důvod chyby hello:** této chybě dojde, pokud modul PowerShell hello nelze najít hello rutiny, které používáte ve vaší sadě runbook.  To může být způsobeno hello modul, který obsahuje rutinu hello chybí hello účtu, dojde ke konfliktu názvů s název runbooku, nebo rutinu hello také existuje v jiném modulu a automatizace nelze přeložit název hello.

**Tipy pro odstraňování potíží:** některé z následujících řešení hello bude hello problém vyřešit:  

* Zkontrolujte, zda jste správně zadali název rutiny hello.  
* Zajistěte, aby rutiny hello existuje v účtu Automation a že nedošlo ke konfliktům. tooverify Pokud rutina hello je k dispozici, otevřete sadu runbook v režimu úprav a hledání hello rutiny mají toofind v hello knihovně nebo spustit **Get-Command ``<CommandName>``** .  Jakmile ověříte rutinu hello je k dispozici toohello účet, a že nejsou žádné název je v konfliktu s jinými rutinami nebo sady runbook, přidejte ho toohello plátno a ujistěte se, zda používáte platný parametr nastavit v sadě runbook.  
* Pokud nemáte ke konfliktu názvů, a rutina hello je k dispozici ve dvou různých modulů, můžete to vyřešit pomocí hello plně kvalifikovaný název rutiny hello. Například můžete použít **ModuleName\CmdletName**.  
* Pokud jsou prováděny hello runbook místně v skupinu hybridních pracovních procesů, ujistěte se, že hello rutinu modulu nebo je nainstalován na počítači hello, který je hostitelem hello hybridní pracovní proces.

### <a name="scenario-a-long-running-runbook-consistently-fails-with-hello-exception-hello-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-hello-same-checkpoint"></a>Scénář: Dlouho spuštěná sada runbook konzistentně selže s výjimkou hello: "hello úlohy nemůže pokračovat, systémem, protože bylo opakovaně vyloučeno z hello stejné kontrolní bod".
**Důvod chyby hello:** jedná se o chování návrhu kvůli toohello "Úloha dostatečný podíl" monitorování procesů v rámci Azure Automation, která automaticky pozastaví runbook, pokud se provede déle než 3 hodiny. Ale hello chybovou zprávu vrácenou neposkytuje "jaké další" možnosti. Sady runbook můžete pozastavit několik důvodů. Pozastaví dojít většinou kvůli tooerrors. Například nezachycenou výjimku v runbooku, selhání sítě nebo havárie v hello systémem hello runbook služby Runbook Worker, budou všechny způsobit toobe hello runbook pozastavený a začít od svého posledního kontrolního bodu při obnovení.

**Tipy pro odstraňování potíží:** hello zdokumentovaný tooavoid řešení tohoto problému je toouse kontrolní body v pracovním postupu.  toolearn informace naleznete v příliš[pracovních postupů PowerShell Learning](automation-powershell-workflow.md#checkpoints).  Podrobnější vysvětlení "Úloha dostatečný podíl" a kontrolního bodu najdete v tomto článku blog [pomocí kontrolních bodů v sadách Runbook](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>Běžné chyby při importu modulů
### <a name="scenario-module-fails-tooimport-or-cmdlets-cant-be-executed-after-importing"></a>Scénář: Modul selže tooimport nebo rutin nelze provést po importu
**Chyba:** modul selže tooimport nebo importuje úspěšně, ale žádné rutiny se extrahují.

**Důvod chyby hello:** některé běžné důvodů, proč, modul nemusí úspěšně importovat tooAzure Automation jsou:  

* Struktura Hello neodpovídá hello struktura, Automation potřebuje, ho toobe v.  
* modul Hello je závislá na jiný modul, který ještě nebyl nasazený tooyour účet Automation.  
* modul Hello chybí jeho závislosti ve složce hello.  
* Hello **New-AzureRmAutomationModule** rutiny, která má být modul hello použité tooupload a nedali hello úložiště úplnou cestu nebo nebyly načteny hello modulu pomocí adresy URL veřejně přístupná.  

**Tipy k řešení potíží:**  
Některé z následujících řešení hello opraví hello problému:  

* Ujistěte se, že Tenhle modul hello následuje hello následující formát:  
  ModuleName.Zip  **->**  ModuleName nebo číslo verze  **->**  (ModuleName.psm1, ModuleName.psd1)
* Otevřete soubor .psd1 hello a zjistěte, zda text hello modulu všechny závislosti.  Pokud ano, nahrajte tyto moduly toohello účet Automation.  
* Ujistěte se, že jsou všechny odkazované knihoven DLL umístěny ve složce modulu hello.  

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Běžné chyby při práci s potřeby konfigurace stavu (DSC)
### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Scénář: Uzel je ve stavu selhání s chybou "Nebyl nalezen."
**Chyba:** hello uzel obsahuje sestavu s **se nezdařilo** stavu a obsahující chyby hello "hello pokus o tooget hello akce ze serveru https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``) / GetDscAction se nezdařila, protože platnou konfigurací ``<guid>`` nebyl nalezen. "

**Důvod chyby hello:** této chybě obvykle dochází, když hello uzlu je přiřazen název konfigurace tooa (například ABC) namísto název konfigurace uzlu (například ABC. Webový server).  

**Tipy k řešení potíží:**  

* Ujistěte se, že přiřazujete hello uzel s "název konfigurace uzlu" a není hello konfigurace "název".  
* Můžete přiřadit uzel tooa konfigurace uzlu pomocí Azure portálu nebo pomocí rutiny prostředí PowerShell.

  * V pořadí tooassign uzlu tooa konfigurace uzlu pomocí portálu Azure, otevřete hello **uzly DSC** okno, pak vyberte uzel a klikněte na **konfigurace uzlu přiřazení** tlačítko.  
  * V pořadí tooassign uzlu tooa konfigurace uzlu pomocí rutiny prostředí PowerShell, použijte **Set-AzureRmAutomationDscNode** rutiny

### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Scénář: Žádná konfigurace uzlu (soubory MOF) byly vytvořeny při kompilaci konfigurace
**Chyba:** pozastaví úlohu kompilace DSC s chybou hello: "kompilace byl úspěšně dokončen, ale byly vygenerovány žádné .mofs konfigurace uzlu".

**Důvod chyby hello:** při hello výraz, který následuje hello **uzlu** – klíčové slovo v konfiguraci hello DSC vyhodnotí příliš$ hodnotu null, pak se žádné konfigurace uzlů se vydává.    

**Tipy k řešení potíží:**  
Některé z následujících řešení hello opraví hello problému:  

* Ujistěte se, že hello výraz další toohello **uzlu** – klíčové slovo v definici konfigurace hello není příliš vyhodnocení $ hodnotu null.  
* Pokud předáváte ConfigurationData při kompilaci konfigurace hello, ujistěte se, že předáváte hello očekávaných hodnot vyžaduje, aby konfigurace hello [ConfigurationData](automation-dsc-compile.md#configurationdata).

### <a name="scenario--hello-dsc-node-report-becomes-stuck-in-progress-state"></a>Scénář: hello sestavy uzlu DSC stane zablokované stav "v průběhu"
**Chyba:** DSC agenta výstupy "Nalezena žádná instance s danými hodnotami vlastností."

**Důvod chyby hello:** upgradu vaší verzi WMF a mají poškozen rozhraní WMI.  

**Tipy pro odstraňování potíží:** postupujte podle pokynů hello v hello [DSC známé problémy a omezení](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) toofix hello problém.

### <a name="scenario--unable-toouse-a-credential-in-a-dsc-configuration"></a>Scénář: Nelze toouse pověření v konfiguraci DSC
**Chyba:** úlohu kompilace DSC byla pozastavena s chybou hello: "System.InvalidOperationException Chyba při zpracování vlastnost pověření typu ``<some resource name>``: převádění a uložení zašifrované heslo jako prostý text je povoleno pouze v případě "PSDscAllowPlainTextPassword je nastavené tootrue".

**Důvod chyby hello:** jste použili pověření v konfiguraci, ale neposkytli správné **ConfigurationData** tooset **PSDscAllowPlainTextPassword** tootrue pro každý uzel konfigurace.  

**Tipy k řešení potíží:**  

* Ujistěte se, že toopass v hello správné **ConfigurationData** tooset **PSDscAllowPlainTextPassword** tootrue pro každý uzel, konfigurace uvedené v konfiguraci hello. Další informace najdete v části příliš[prostředky v Azure Automation DSC](automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Další kroky
Pokud jste postupovali podle hello výše uvedené kroky řešení potíží a nelze najít odpověď hello, můžete zkontrolovat hello další možnosti podpory níže.

* Získáte pomoc od odborníků na Azure. Odeslat váš problém toohello [fórech MSDN Azure nebo Stack Overflow.](https://azure.microsoft.com/support/forums/).
* Soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na tlačítko **získat podporu** pod **a technické podpory fakturace**.
* Zveřejnit skriptů požadavek na [centra skriptů](https://azure.microsoft.com/documentation/scripts/) Pokud hledáte řešení runbooku s Azure Automation nebo modul integrace.
* Zveřejnit připomínkám nebo funkcím požadavky pro Azure Automation na [User Voice](https://feedback.azure.com/forums/34192--general-feedback).
