---
title: "aaaStart a zastavení virtuálních počítačů během řešení počítačem nepracujete [Preview] | Microsoft Docs"
description: "řešení pro správu virtuálních počítačů Hello spustí a zastaví virtuální počítače Azure Resource Manager podle plánu a proaktivnímu monitorování z analýzy protokolů."
services: automation
documentationCenter: 
authors: mgoedtel
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: magoedte
ms.openlocfilehash: 6cbe16dfb40bf13f29d9e58ca0bc8c5c7979879d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="startstop-vms-during-off-hours-preview-solution-in-automation"></a>Řešení pro spouštění/zastavování virtuálních počítačů v době mimo špičku [Preview] ve službě Automation

Hello spuštění a zastavení virtuálních počítačů během řešení počítačem nepracujete [Preview] spustí a zastaví virtuální počítače Azure Resource Manager podle plánu, uživatelem definované a poskytuje vhled do hello úspěšnost hello automatizace úloh, které spuštění a zastavení virtuálních počítačů s OMS Analýzy protokolů.  

## <a name="prerequisites"></a>Požadavky

- sady runbook Hello pracovat [účet spustit v Azure jako](automation-offering-get-started.md#authentication-methods).  Hello účet Spustit jako je metoda ověřování hello upřednostňovaný, protože se používá ověření certifikátu místo hesla, která může vypršení platnosti nebo často mění.  

- Toto řešení můžete spravovat pouze virtuální počítače, které jsou v hello stejnému předplatnému jako níž se nachází hello účet Automation.  

- Toto řešení jenom nasadí toohello následující oblastí Azure - Austrálie – jihovýchod, východní USA, jihovýchodní Asie a západní Evropa.  Hello sady runbook, které spravovat hello virtuálních počítačů plánu, můžete vybrat virtuální počítače v libovolné oblasti.  

- toosend e-mailová oznámení, když hello spuštění a zastavení virtuálního počítače sady runbook dokončení, je podnikové třídy předplatné služeb Office 365.  

## <a name="solution-components"></a>Součásti řešení

Toto řešení se skládá z následujících prostředků, které budou naimportovány a přidat účet Automation tooyour hello.

### <a name="runbooks"></a>Runbooky

Runbook | Popis|
--------|------------|
CleanSolution-MS-Mgmt-VM | Tento runbook odebere všechny obsažené prostředky i plány, když přejdete toodelete hello řešení ze svého předplatného.|  
SendMailO365-MS-Mgmt | Tento runbook odešle e-mail prostřednictvím Office 365 Exchange.|
StartByResourceGroup-MS-Mgmt-VM | Tato sada runbook je určený toostart virtuálních počítačů (obě classic a ARM na základě virtuálních počítačů), se nachází v daný seznam skupin prostředků Azure.
StopByResourceGroup-MS-Mgmt-VM | Tato sada runbook je určený toostop virtuálních počítačů (obě classic a ARM na základě virtuálních počítačů), se nachází v daný seznam skupin prostředků Azure.|
<br>

### <a name="variables"></a>Proměnné

Proměnná | Popis|
---------|------------|
Runbook **SendMailO365-MS-Mgmt** ||
SendMailO365-IsSendEmail-MS-Mgmt | Určuje, zda runbooky StartByResourceGroup-MS-Mgmt-VM a StopByResourceGroup-MS-Mgmt-VM mohou po dokončení odeslat e-mailové oznámení.  Vyberte **True** tooenable a **False** toodisable e-mailové výstrahy. Výchozí je hodnota **False**.| 
Runbook **StartByResourceGroup-MS-Mgmt-VM** ||
StartByResourceGroup-ExcludeList-MS-Mgmt-VM | Zadejte toobe názvy virtuálního počítače vyloučeny z operace správy; oddělte názvy semi-colon(;) bez mezer. V hodnotách se rozlišují malá a velká písmena a je podporován zástupný znak (hvězdička).|
StartByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt | Text, který může být připojena toohello začátek textu hello e-mailové zprávy.|
StartByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Určuje název hello hello účet Automation, který obsahuje runbook hello e-mailu.  **Neprovádějte žádné změny této proměnné.**|
StartByResourceGroup-SendMailO365-EmailRunbookName-MS-Mgmt | Určuje název hello runbook hello e-mailu.  To je používán hello StartByResourceGroup-MS-Mgmt-VM a StopByResourceGroup-MS-Mgmt-VM sady runbook toosend e-mailu.  **Neprovádějte žádné změny této proměnné.**|
StartByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Určuje název hello hello skupiny prostředků, který obsahuje runbook hello e-mailu.  **Neprovádějte žádné změny této proměnné.**|
StartByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Určuje text hello hello řádek předmětu e-mailů hello.|  
StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Určuje hello příjemci e-mailů hello.  Zadejte názvy pomocí semi-colon(;) bez mezer.|
StartByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Zadejte toobe názvy virtuálního počítače vyloučeny z operace správy; oddělte názvy semi-colon(;) bez mezer. V hodnotách se rozlišují malá a velká písmena a je podporován zástupný znak (hvězdička).  Výchozí hodnota (hvězdička) bude obsahovat všechny skupiny prostředků v předplatném hello.|
StartByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Určuje hello odběr, který obsahuje virtuální počítače toobe spravuje toto řešení.  Tato hodnota musí být hello stejného předplatného, které se nachází hello účet Automation tohoto řešení.|
Runbook **StopByResourceGroup-MS-Mgmt-VM** ||
StopByResourceGroup-ExcludeList-MS-Mgmt-VM | Zadejte toobe názvy virtuálního počítače vyloučeny z operace správy; oddělte názvy semi-colon(;) bez mezer. V hodnotách se rozlišují malá a velká písmena a je podporován zástupný znak (hvězdička).|
StopByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt | Text, který může být připojena toohello začátek textu hello e-mailové zprávy.|
StopByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt | Určuje název hello hello účet Automation, který obsahuje runbook hello e-mailu.  **Neprovádějte žádné změny této proměnné.**|
StopByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt | Určuje název hello hello skupiny prostředků, který obsahuje runbook hello e-mailu.  **Neprovádějte žádné změny této proměnné.**|
StopByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt | Určuje text hello hello řádek předmětu e-mailů hello.|  
StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt | Určuje hello příjemci e-mailů hello.  Zadejte názvy pomocí semi-colon(;) bez mezer.|
StopByResourceGroup-TargetResourceGroups-MS-Mgmt-VM | Zadejte toobe názvy virtuálního počítače vyloučeny z operace správy; oddělte názvy semi-colon(;) bez mezer. V hodnotách se rozlišují malá a velká písmena a je podporován zástupný znak (hvězdička).  Výchozí hodnota (hvězdička) bude obsahovat všechny skupiny prostředků v předplatném hello.|
StopByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM | Určuje hello odběr, který obsahuje virtuální počítače toobe spravuje toto řešení.  Tato hodnota musí být hello stejného předplatného, které se nachází hello účet Automation tohoto řešení.|  
<br>

### <a name="schedules"></a>Plány

Plán | Popis|
---------|------------|
StartByResourceGroup-Schedule-MS-Mgmt | Plán pro sadu runbook StartByResourceGroup, který provádí hello spuštění virtuálních počítačů, které spravuje toto řešení. Při vytváření, bude výchozí tooUTC časové pásmo.|
StopByResourceGroup-Schedule-MS-Mgmt | Plán pro sadu runbook StopByResourceGroup, který provádí hello vypnutí virtuálních počítačů, které spravuje toto řešení. Při vytváření, bude výchozí tooUTC časové pásmo.|

### <a name="credentials"></a>Přihlašovací údaje

Přihlašovací údaj | Popis|
-----------|------------|
O365Credential | Určuje platný účet toosend e-mail uživatele služeb Office 365.  Je požadován, pouze pokud je příliš nastavit proměnnou SendMailO365-IsSendEmail-MS-Mgmt**True**.

## <a name="configuration"></a>Konfigurace

Proveďte následující kroky tooadd hello spuštění a zastavení virtuálních počítačů během tooyour řešení počítačem nepracujete [Preview] účet Automation hello a pak nakonfigurujte hello proměnné toocustomize hello řešení.

1. Hello domovské obrazovky v hello portálu Azure, vyberte hello **Marketplace** dlaždici.  Pokud hello dlaždice připnuté tooyour domovské obrazovky, hello levém navigačním podokně, vyberte **nový**.  
2. V okně hello Marketplace, zadejte **spustit virtuální počítač** v hello vyhledávacího pole a pak vyberte hello řešení **spuštění a zastavení virtuálních počítačů, která [Preview]** z výsledků hledání hello.  
3. V hello **spuštění a zastavení virtuálních počítačů, která [Preview]** vybrané okno pro hello řešení, zkontrolovat hello souhrnné informace a potom klikněte na **vytvořit**.  
4. Hello **přidat řešení** otevře se okno, kde je řešení hello výzvami tooconfigure předtím, než můžete ho importovat do vašeho předplatného automatizace.<br><br> ![Okno Přidat řešení správy virtuálních počítačů](media/automation-solution-vm-management/vm-management-solution-add-solution-blade.png)<br><br>
5.  Na hello **přidat řešení** vyberte **prostoru** a zde vyberte pracovní prostor OMS, která je propojená toohello stejného předplatného Azure, který hello účet Automation je v nebo vytvořit nový pracovní prostor OMS.  Pokud nemáte pracovním prostorem OMS, můžete si vybrat **vytvořit nový pracovní prostor** a na hello **pracovním prostorem OMS** okno provést hello následující: 
   - Zadejte název nové hello **pracovním prostorem OMS**.
   - Vyberte **předplatné** toolink tooby výběr z rozevíracího seznamu hello Pokud hello výchozí vybraný není vhodná.
   - Pro možnost **Skupina prostředků** můžete vytvořit novou skupinu prostředků nebo vybrat existující skupinu prostředků.  
   - Vyberte **Umístění**.  Aktuálně jsou hello pouze umístění zadané pro výběr **Austrálie – jihovýchod**, **východní USA**, **jihovýchodní Asie**, a **západní Evropa**.
   - Vyberte možnost u položky **Cenová úroveň**.  Hello řešení je k dispozici v dvou vrstev: uvolněte a OMS placené vrstvy.  úroveň free Hello může mít na hello množství dat shromážděných denně, doba uchování dat a minut runtime úlohy sady runbook.  Hello OMS placené úroveň nemá na hello množství dat denně shromážděných omezení.  

        > [!NOTE]
        > Když jako možnost zobrazí hello samostatné placené vrstvy, není použitelný.  Pokud vyberte ho a pokračujte hello vytvoření tohoto řešení v rámci vašeho předplatného, se nezdaří.  Problém bude odstraněn po oficiálním vydání tohoto řešení.<br>Pokud použijete toto řešení, bude využívat pouze příjem protokolů a minuty úloh služby Automation.  řešení Hello nepřidá další OMS uzly tooyour prostředí.  

6. Po zadání hello požadované informace na hello **pracovním prostorem OMS** okně klikněte na tlačítko **vytvořit**.  Při ověření hello informace a při vytváření pracovního prostoru hello, můžete sledovat průběh v části **oznámení** nabídce hello.  Bude vrácen toohello **přidat řešení** okno.  
7. Na hello **přidat řešení** vyberte **účet Automation**.  Pokud vytváříte nový pracovní prostor OMS, se bude vyžadovat tooalso vytvořit nový účet Automation, který bude přidružena ke hello nový pracovní prostor OMS dříve, zadaný včetně vašeho předplatného Azure, skupinu prostředků a oblast.  Můžete vybrat **vytvořit účet Automation** a na hello **účet Automation přidat** okno, zadejte následující hello: 
  - V hello **název** pole, zadejte název hello hello účet Automation.

    Všechny ostatní možnosti se vyplní automaticky v závislosti na vybrané pracovní prostor OMS hello a tyto možnosti nelze změnit.  Účet spustit v Azure jako je hello výchozí metodou ověřování pro runbooky hello zahrnuté v tomto řešení.  Po kliknutí na tlačítko **OK**, možnosti konfigurace hello se ověří a vytvoření hello účet Automation.  V části jeho průběh můžete sledovat **oznámení** nabídce hello. 

    Můžete také vybrat existující účet služby Automation Spustit jako.  Všimněte si, že hello účet, který jste vybrali již nelze pracovním prostorem OMS propojené tooanother jinak zpráva zobrazí v okně tooinform hello je.  Pokud už je propojená, budete potřebovat tooselect jiný účet Automation spustit jako nebo vytvořte novou.<br><br> ![Účet již propojené tooOMS automatizace pracovního prostoru](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Nakonec na hello **přidat řešení** vyberte **konfigurace** a hello **parametry** otevře se okno.  Na hello **parametry** okně, zobrazí se výzva k:  
   - Zadejte hello **cílové názvy ResourceGroup**, což je název skupiny prostředků, která obsahuje virtuální počítače toobe spravuje toto řešení.  Můžete zadat více než jeden název a jednotlivé názvy oddělit středníky (v hodnotách se rozlišují malá a velká písmena).  Pomocí zástupného znaku je podporována, pokud chcete, aby tootarget virtuální počítače ve všech skupinách prostředků v předplatném hello.
   - Vyberte **plán** tedy opakovaného datum a čas pro spuštění a zastavení hello Virtuálního počítače v hello cílové skupiny prostředků.  Ve výchozím nastavení je plán hello nakonfigurované toohello UTC časové pásmo a vybrat jinou oblast, není k dispozici.  Pokud chcete po dokončení konfigurace řešení hello tooconfigure hello tooyour konkrétní časové pásmo plánu, najdete v části [Modifying hello spuštění a vypnutí plán](#modifying-the-startup-and-shutdown-schedule) níže.    

10. Jakmile dokončíte konfiguraci počáteční nastavení hello hello řešení, vyberte **vytvořit**.  Všechna nastavení budou ověřena a potom pokusí toodeploy hello řešení v rámci vašeho předplatného.  Tento proces může trvat několik sekund toocomplete a vy můžete sledovat v jeho průběhu **oznámení** nabídce hello. 

## <a name="collection-frequency"></a>Četnost shromažďování dat

Automatizace úloh protokolu a úlohy datový proud dat je do úložiště OMS hello konzumována každých pět minut.  

## <a name="using-hello-solution"></a>Pomocí řešení hello

Když přidáte hello řešení správy virtuálních počítačů, ve vašem hello pracovní prostor OMS **StartStopVM zobrazení** dlaždice se přidá tooyour OMS řídicího panelu.  Tuto dlaždici zobrazí počet a grafické znázornění hello úlohy sady runbook pro hello řešení, které byly zahájeny a byly úspěšně dokončeny.<br><br> ![Dlaždice Zobrazení StartStopVM správy virtuálních počítačů](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

V účtu Automation, můžete používat a spravovat řešení hello výběrem hello **řešení** dlaždici a potom z hello **řešení** okně Výběr hello řešení **[[[Start-Stop-VM Pracovní prostor]** hello seznamu.<br><br> ![Seznam řešení služby Automation](media/automation-solution-vm-management/vm-management-solution-autoaccount-solution-list.png)  

Výběr řešení hello zobrazí hello **Start-Stop-VM [prostoru]** řešení okno, kde můžete zkontrolovat důležité podrobnosti, jako je například hello **StartStopVM** dlaždici, jako je třeba v pracovním prostoru OMS, který Zobrazí počet a grafické znázornění hello úlohy sady runbook pro hello řešení, které byly zahájeny a byly úspěšně dokončeny.<br><br> ![Okno řešení pro virtuální počítače služby Automation](media/automation-solution-vm-management/vm-management-solution-solution-blade.png)  

Odsud můžete také otevřít pracovní prostor OMS a provádět další analýzu záznamů úlohy hello.  Stačí kliknout na **všechna nastavení**a v hello **nastavení** vyberte **rychlý Start** a pak v hello **rychlý Start** okně vyberte  **Portál OMS**.   Otevře se nová karta nebo nová relace prohlížeče a zobrazí se pracovní prostor OMS přidružený k vašemu účtu a předplatnému služby Automation.  


### <a name="configuring-e-mail-notifications"></a>Konfigurace e-mailových oznámení

tooenable e-mailová oznámení, když hello spuštění a zastavení virtuálního počítače sady runbook dokončení, budete potřebovat toomodify hello **O365Credential** přihlašovacích údajů a minimálně hello následující proměnné:

 - SendMailO365-IsSendEmail-MS-Mgmt
 - StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt
 - StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt

tooconfigure hello **O365Credential** přihlašovacích údajů, proveďte následující kroky hello:

1. Z vašeho účtu automation, klikněte na tlačítko **všechna nastavení** hello horní části okna hello. 
2. Na hello **nastavení** okno části hello **prostředky Automation**, vyberte **prostředky**. 
3. Na hello **prostředky** okně, vyberte hello **pověření** dlaždici a z hello **pověření** okně, vyberte hello **O365Credential**.  
4. Zadejte platné uživatelské jméno Office 365 a heslo a pak klikněte na tlačítko **Uložit** toosave změny.  

proměnné hello tooconfigure zvýrazněná dříve, proveďte následující kroky hello:

1. Z vašeho účtu automation, klikněte na tlačítko **všechna nastavení** hello horní části okna hello. 
2. Na hello **nastavení** okno části hello **prostředky Automation**, vyberte **prostředky**. 
3. Na hello **prostředky** okně, vyberte hello **proměnné** dlaždici a z hello **proměnné** okně vyberte hello proměnnou uvedené výše a potom upravte jeho hodnota následující hello Popis pro něj zadané v hello [proměnná](##variables) výše v části.  
4. Klikněte na tlačítko **Uložit** toosave hello změny toohello proměnné.   

### <a name="modifying-hello-startup-and-shutdown-schedule"></a>Změny plánu hello spuštění a vypnutí

Správa hello spuštění a vypnutí plán v tomto řešení odpovídá hello stejné kroky, jak je uvedeno v [plánování runbooku ve službě Azure Automation](automation-schedules.md).  Mějte na paměti, nelze změnit konfiguraci plánu hello.  Budete potřebovat toodisable hello existující plán a pak vytvořte novou a potom na odkaz toohello **StartByResourceGroup-MS-Mgmt-VM** nebo **StopByResourceGroup-MS-Mgmt-VM** sady runbook, které chcete hello Naplánujte tooapply k.   

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics

Automatizace vytvoří dva typy záznamů v úložišti OMS hello.

### <a name="job-logs"></a>Protokoly úloh

Vlastnost | Popis|
----------|----------|
Volající |  Kdo je inicioval hello operaci.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy.|
Kategorie | Klasifikace datový typ hello.  Pro automatizaci hello hodnota je JobLogs.|
CorrelationId | Identifikátor GUID, který je hello Id korelace hello úlohy sady runbook.|
JobId | Identifikátor GUID, který je hello Id úlohy runbooku hello.|
operationName | Určuje typ hello operaci provést, v Azure.  Pro automatizaci bude hodnota hello úlohy.|
resourceId | Určuje typ prostředku hello v Azure.  Pro automatizaci hello hodnota je účet Automation hello související se sadou runbook hello.|
ResourceGroup | Určuje název skupiny prostředků hello hello úlohy sady runbook.|
ResourceProvider | Určuje hello služba Azure, která poskytuje hello prostředky, které můžete nasadit a spravovat.  Pro automatizaci hello hodnota je Azure Automation.|
ResourceType | Určuje typ prostředku hello v Azure.  Pro automatizaci hello hodnota je účet Automation hello související se sadou runbook hello.|
resultType | Hello stav úlohy runbooku hello.  Možné hodnoty:<br>- Spuštěno<br>- Zastaveno<br>- Pozastaveno<br>- Neúspěch<br>- Úspěch|
resultDescription | Popisuje stav výsledek úlohy sady runbook hello.  Možné hodnoty:<br>- Úloha se spustila<br>- Zpracování úlohy se nezdařilo<br>- Úloha je dokončená|
RunbookName | Určuje název hello hello sady runbook.|
SourceSystem | Určuje hello zdrojovém systému pro odeslaná data hello.  Pro automatizaci, bude hodnota hello: OpsManager|
StreamType | Určuje typ hello události. Možné hodnoty:<br>- Podrobné<br>- Výstup<br>- Chyba<br>- Varování|
SubscriptionId | Určuje ID předplatného hello hello úlohy.
Čas | Datum a čas, kdy spustit úlohy runbooku hello.|


### <a name="job-streams"></a>Datové proudy úlohy

Vlastnost | Popis|
----------|----------|
Volající |  Kdo je inicioval hello operaci.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy.|
Kategorie | Klasifikace datový typ hello.  Pro automatizaci hello hodnota je JobStreams.|
JobId | Identifikátor GUID, který je hello Id úlohy runbooku hello.|
operationName | Určuje typ hello operaci provést, v Azure.  Pro automatizaci bude hodnota hello úlohy.|
ResourceGroup | Určuje název skupiny prostředků hello hello úlohy sady runbook.|
resourceId | Určuje Id prostředku hello v Azure.  Pro automatizaci hello hodnota je účet Automation hello související se sadou runbook hello.|
ResourceProvider | Určuje hello služba Azure, která poskytuje hello prostředky, které můžete nasadit a spravovat.  Pro automatizaci hello hodnota je Azure Automation.|
ResourceType | Určuje typ prostředku hello v Azure.  Pro automatizaci hello hodnota je účet Automation hello související se sadou runbook hello.|
resultType | výsledek Hello hello úlohy sady runbook v hello čas hello událostí byl vygenerován.  Možné hodnoty:<br>- Probíhá zpracování|
resultDescription | Zahrnuje hello výstupního datového proudu z runbooku hello.|
RunbookName | Hello název sady runbook hello.|
SourceSystem | Určuje hello zdrojovém systému pro odeslaná data hello.  Pro automatizaci bude hodnota hello OpsManager|
StreamType | Typ Hello stream úloh. Možné hodnoty:<br>- Průběh<br>- Výstup<br>- Varování<br>- Chyba<br>- Ladění<br>- Podrobné|
Čas | Datum a čas, kdy spustit úlohy runbooku hello.|

Při vyhledávání všech protokolu, který vrátí záznamy kategorii **JobLogs** nebo **JobStreams**, můžete vybrat hello **JobLogs** nebo **JobStreams** zobrazení, která se zobrazí sada dlaždice shrnutí aktualizací hello vrácený hello vyhledávání.

## <a name="sample-log-searches"></a>Ukázky hledání v protokolech

Hello následující tabulka obsahuje ukázkový protokol hledání záznamů úlohy shromažďují toto řešení. 

Dotaz | Popis|
----------|----------|
Najít úlohy runbooku StartVM, které byly úspěšně dokončeny | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Succeeded &#124; measure count() by JobId_g|
Najít úlohy runbooku StopVM, které byly úspěšně dokončeny | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Failed &#124; measure count() by JobId_g
Zobrazit stav úloh v čase pro runbooky StartVM a StopVM | Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" OR "StopByResourceGroup-MS-Mgmt-VM" NOT(ResultType="started") | measure Count() by ResultType interval 1day|

## <a name="removing-hello-solution"></a>Odebrání hello řešení

Pokud se rozhodnete, že již nepotřebujete toouse hello řešení všechny další, odstraňte jej z hello účet Automation.  Odstraňování řešení hello se jenom odeberou hello sady runbook, neodstraní hello plány nebo proměnné, které byly vytvořeny při přidání hello řešení.  Tyto prostředky budete potřebovat toodelete ručně Pokud je nepoužíváte pomocí jiné sady runbook.  

toodelete hello řešení, proveďte následující kroky hello:

1.  Z vašeho účtu automation vyberte hello **řešení** dlaždici.  
2.  Na hello **řešení** okně, vyberte hello řešení **Start-Stop-VM [prostoru]**.  Na hello **VMManagementSolution [prostoru]** okno z hello nabídce klikněte na tlačítko **odstranit**.<br><br> ![Odstranit řešení pro správu virtuálních počítačů](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  V hello **odstranit řešení** okně potvrďte chcete toodelete hello řešení.
4.  Při ověření hello informace a řešení hello je odstranit, můžete sledovat průběh v části **oznámení** nabídce hello.  Bude vrácen toohello **VMManagementSolution [prostoru]** okno po spuštění hello proces tooremove řešení.  

účet Automation Hello a pracovním prostorem OMS nejsou odstraněna jako součást tohoto procesu.  Pokud nechcete, aby pracovní prostor OMS hello tooretain, budete potřebovat toomanually ho odstranit.  Můžete to provést také z hello portálu Azure.   Hello domovské obrazovky v hello portálu Azure, vyberte **analýzy protokolů** a pak na hello **analýzy protokolů** , vyberte hello prostoru a klikněte na **odstranit** na v nabídce hello okno nastavení pracovního prostoru Hello.  
      
## <a name="next-steps"></a>Další kroky

- toolearn Další informace o tom, jak tooconstruct jiný vyhledávací dotazy a zkontrolujte hello automatizace úlohy protokoly s analýzy protokolů, najdete v části [hledání přihlásit analýzy protokolů](../log-analytics/log-analytics-log-searches.md)
- Další informace o spuštění sady runbook, jak toomonitor úlohy a další technické podrobnosti najdete v tématu toolearn [sledovat úlohy runbooku](automation-runbook-execution.md)
- toolearn informace o OMS analýzy protokolů a datových zdrojů kolekce, najdete v části [shromažďování Azure úložiště dat v přehledu analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)






   

