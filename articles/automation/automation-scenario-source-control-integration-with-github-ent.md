---
title: "aaaAzure integrace ovládacích prvků zdrojového automatizace s Enterprise Githubu | Microsoft Docs"
description: "Popisuje hello podrobnosti o tom, tooconfigure integraci se sadou GitHub Enterprise pro zdrojového kódu runbooků služeb automatizace."
services: automation
documentationCenter: 
authors: mgoedtel
manager: jwhit
editor: 
ms.assetid: e01d817c-7d38-421c-adf5-647a4b526eb4
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: magoedte
ms.openlocfilehash: 915d36ccabb72fdee1dba663049a0b331249cd73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure Automation scénář – integrace ovládacích prvků zdrojového automatizace s Enterprise Githubu

Automatizace aktuálně podporuje integrace ovládacích prvků zdrojového, což vám umožní tooassociate sady runbook v automatizace účet tooa Githubu úložiště řízení zdrojů.  Však zákazníky, kteří nainstalovali [Githubu Enterprise](https://enterprise.github.com/home) toosupport jejich DevOps postupů a také vhodné toouse ho toomanage hello životní cyklus sady runbook, které jsou vyvinuté tooautomate podnikové procesy a správu služeb operace.  

V tomto scénáři máte počítač se systémem Windows ve vašem datovém centru, který je nakonfigurovaný jako hybridní pracovní proces Runbooku s moduly Azure Resource Manageru hello a nainstalované nástroje Git.  Hello hybridní pracovní počítače je klon hello místní úložiště Git.  Při spuštění sady runbook hello hello hybridní pracovní proces, adresář Git hello se synchronizují a obsah souboru runbook hello importují do hello účet Automation.

Tento článek popisuje, jak tooset tuto konfiguraci v prostředí Azure Automation. Začneme nakonfigurováním automatizace s hello zabezpečovací přihlašovací údaje, sady runbook vyžaduje toosupport tento scénář a nasazení hybridní pracovní proces Runbooku ve vašich datech center toorun hello sady runbook a přístup k vaší toosynchronize úložiště GitHub Enterprise sady runbook pomocí vašeho účtu Automation.  


## <a name="getting-hello-scenario"></a>Získávání scénář hello

Tento scénář se skládá ze dvou Powershellové runbooky, které můžete importovat přímo z hello [Galerie Runbooků](automation-runbook-gallery.md) v hello portál Azure nebo stáhnout z hello [Galerie prostředí PowerShell](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooky

Runbook | Popis| 
--------|------------|
Export-RunAsCertificateToHybridWorker | Runbook Exportuje certifikát RunAs z pracovního procesu automatizace účet tooa hybridní tak, aby sady runbook na pracovní hello můžete ověřit pomocí Azure v sadách runbook tooimport pořadí do hello účet Automation.| 
Synchronizace LocalGitFolderToAutomationAccount | Synchronizace sady Runbook hello místní Git složky v počítači hybridní hello a poté importovat do účtu Automation hello souborů služby runbook hello (*.ps1).|

### <a name="credentials"></a>Přihlašovací údaje

Přihlašovací údaj | Popis|
-----------|------------|
GitHRWCredential | Asset přihlašovacích údajů vytvoříte toocontain hello uživatelské jméno a heslo pro uživatele s oprávněními toohello hybridní pracovní proces.|

## <a name="installing-and-configuring-this-scenario"></a>Instalace a konfigurace tohoto scénáře

### <a name="prerequisites"></a>Požadavky

1. Hello synchronizace LocalGitFolderToAutomationAccount runbook se ověří pomocí hello [účet spustit v Azure jako](automation-sec-configure-azure-runas-account.md). 

2. Microsoft Operations Management Suite (OMS) prostoru s hello řešení služby Azure Automation povolené a nakonfigurované je také nutný.  Pokud nemáte ten, který je přidružen tooinstall použitý účet Automation hello a nakonfigurujte tento scénář, je vytvořen a nakonfigurován pro vás při spuštění hello **New-OnPremiseHybridWorker.ps1** skript z hybridní hello pracovní proces runbooku.        

    > [!NOTE]
    > Aktuálně hello následující oblasti podporují pouze automatizace integrace s OMS - **Austrálie – jihovýchod**, **východní USA 2**, **jihovýchodní Asie**, a **– západ Evropa**. 

3. Počítač, který může sloužit jako vyhrazené pracovní proces Runbooku hybridní, který bude také hostitelem hello Githubu softwaru a udržovat hello runbook soubory (*runbook*.ps1) v zdrojový adresář na hello souboru systému toosynchronize mezi Githubu a Účet Automation.

### <a name="import-and-publish-hello-runbooks"></a>Import a publikování sad runbook hello

tooimport hello *Export RunAsCertificateToHybridWorker* a *synchronizace LocalGitFolderToAutomationAccount* runbooků hello Galerie Runbooků z vašeho účtu Automation v hello portálu Azure postupujte podle pokynů hello v [importovat Runbook z Galerie Runbooků hello](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Publikování sady runbook hello poté, co byly úspěšně importovány do vašeho účtu Automation.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Nasaďte a nakonfigurujte hybridní pracovní proces Runbooku

Pokud nemáte Hybrid Runbook Worker už nasazená ve vašem datovém centru, zkontrolujte požadavky na hello a postupujte podle kroků hello automatizované instalace postupem hello v [Azure automatizace procesů Hybrid Runbook Worker - automatizovat instalaci Konfigurace a](automation-hybrid-runbook-worker.md#automated-deployment).  Úspěšně jste nainstalovali hello hybridní pracovní proces na počítači, proveďte hello následující kroky toocomplete jeho konfigurace toosupport tento scénář.

1. Přihlášení toohello hostování hello hybridní pracovní proces Runbooku role počítače pomocí účtu, který má práva místního správce a vytvořit soubory adresáře toohold hello Git sady runbook.  Klon hello interní Git úložiště toohello adresář.
2. Pokud již jste vytvořili účet RunAs, nebo chcete toocreate novou vyhrazený pro tento účel, z hello portálu Azure přejděte tooAutomation účty, vyberte svůj účet Automation a vytvořte [asset přihlašovacích údajů](automation-credentials.md) , obsahuje hello uživatelské jméno a heslo pro uživatele s oprávněními toohello hybridní pracovní proces.  
3. Z vašeho účtu Automation [upravit hello runbook](automation-edit-textual-runbook.md)**Export RunAsCertificateToHybridWorker** a upravit hello hodnotu pro proměnnou hello *$Password* s silné heslo.    Po úpravě hello hodnotu, klikněte na tlačítko **publikovat** toohave hello verzi konceptu sady runbook hello publikována. 
5. Spuštění sady runbook hello **Export RunAsCertificateToHybridWorker**a v hello **spuštění Runbooku** okno, v části možnost hello **spustit nastavení** možnost hello **Hybridní pracovní proces** a v hello rozevíracího seznamu vyberte hello skupinu hybridních pracovních procesů jste vytvořili dříve v tomto scénáři.  

    To, aby se runbook na pracovní hello ověřit s Azure pomocí připojení spustit jako hello v pořadí toomanage prostředků Azure (zejména v tomto scénáři – import sad runbook toohello účet Automation) Exportuje certifikát toohello hybridního pracovního procesu.

4. Z vašeho účtu Automation hello vyberte dříve vytvořenou skupinu hybridních pracovních procesů a [zadejte účet Spustit jako](automation-hrw-run-runbooks.md#runas-account) pro skupinu hybridních pracovních procesů hello a asset přihlašovacích údajů hello pokusit právě nebo již jste vytvořili.  To zaručuje, že dané sady runbook hello synchronizaci můžete spustit příkazy Gitu. 
5. Spuštění sady runbook hello **synchronizace LocalGitFolderToAutomationAccount**, poskytovat hello následující požadované vstupní parametr hodnoty a v hello **spuštění Runbooku** okno, v části možnost hello **spustit nastavení** možnost hello **hybridní pracovní proces** a v hello rozevíracího seznamu vyberte hello skupinu hybridních pracovních procesů jste dříve vytvořili pro tento scénář:
    * *ResourceGroup* – hello název vaší skupiny prostředků, které jsou spojené s vaším účtem Automation.
    * *AutomationAccountName* hello – název účtu Automation
    * *GitPath* – hello místní složky nebo souboru na hello kde nastavení toopull nejnovější změny do Git hybridní pracovní proces Runbooku

    To bude synchronizovat místní složky Git hello v hello hybridní pracovní počítače a pak naimportujete soubory .ps1 hello z hello zdrojový adresář toohello účet Automation.

    ![Spuštění synchronizace LocalGitFolderToAutomationAccount Runbook](media/automation-scenario-source-control-integration-with-github-ent/start-runbook-synclocalgitfoldertoautoacct.png)<br>

7. Zobrazení souhrnu podrobnosti úlohy pro hello runbook výběrem z hello **Runbooky** okno v účtu Automation a pak vyberte hello **úlohy** dlaždici.  Potvrďte byla úspěšně dokončena výběrem hello **všechny protokoly** dlaždice a datový proud prostudovali podrobné protokolu hello.  

## <a name="next-steps"></a>Další kroky

-  tooknow Další informace o typech runbooků, jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md)
-  Další informace o funkci podpory powershellových skriptů najdete v článku [Nativní podpora powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
