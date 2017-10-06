---
title: aaaWhat je Azure Automation | Microsoft Docs
description: "Seznamte se s hodnotou Azure Automation nabízí a získejte odpovědi toocommon otázky, aby je mohli začít vytváření, pomoci runbooků a Azure Automation DSC."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "co je automatizace, azure automation, příklady azure automation"
ms.assetid: 0cf1f3e8-dd30-4f33-b52a-e148e97802a9
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 1e5a90e272d6b2beb7b5007e2fea2c110dbd79b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-overview"></a>Přehled Azure Automation
Automatizace Microsoft Azure poskytuje způsob, jak pro uživatele tooautomate hello ruční, dlouhotrvajících, problematických a často se opakujících úloh, které se běžně provádějí v cloudovém a podnikovém prostředí. Šetří čas a zvyšuje spolehlivost hello běžných administrativních úloh a umí je dokonce naplánovat toobe automaticky v pravidelných intervalech prováděly. Procesy můžete automatizovat pomocí runbooků nebo můžete automatizovat správu konfigurace pomocí DSC (požadovaný stav konfigurace). Tento článek poskytuje stručný přehled Azure Automation a odpovědi na některé běžné dotazy. Tooother články v této knihovně najdete podrobnější informace o dalších tématech hello.

## <a name="automating-processes-with-runbooks"></a>Automatizace procesů pomocí runbooků
Runbook představuje sadu úloh, které provádějí určité automatizované procesy ve službě Azure Automation. Může být jednoduchý proces, například spuštění virtuálního počítače a vytvoření položky protokolu, nebo můžete mít složitý runbook, který kombinuje jiné menší runbooky tooperform velmi složitý proces na několika prostředcích nebo dokonce více cloudy a místní prostředí.  

Například můžete mít existující ruční proces pro zkracování databáze SQL, pokud se přiblíží maximální velikosti, která zahrnuje několik kroků, například připojování toohello serveru, připojuje toohello databázi, získání aktuální velikost databáze hello, zkontrolujte, zda Prahová hodnota byla překročena a pak zkrácení a upozornit uživatele. Místo ručního provádění jednotlivých kroků můžete vytvořit runbook, který bude všechny tyto úlohy provádět jako jeden proces. By spuštění sady runbook hello, zadejte hello požadované informace, jako je název serveru SQL Server hello, názvu databáze a příjemce e-mailu a pak si v klidu počkáte dokončení procesu hello. 

## <a name="what-can-runbooks-automate"></a>Co může runbook zautomatizovat?
Runbooky v Azure Automation staví na prostředí Windows PowerShell nebo na pracovním postupu Windows PowerShell, aby mohly dělat všechno, co zvládne PowerShell. Pokud aplikace nebo služba používá rozhraní API, může s ním runbook pracovat. Pokud máte modul prostředí PowerShell pro hello aplikaci, můžete načíst tento modul do Azure Automation a zahrnout tyto rutiny v runbooku. Runbooky služby automatizace Azure přístup všem cloudovým prostředkům nebo externím prostředkům, které jsou přístupné z cloudu hello a spustit v hello cloudu Azure. Pomocí [hybridní pracovní proces Runbooku](automation-hybrid-runbook-worker.md), runbooky běžet ve vaší místní data center toomanage místních prostředků. 

## <a name="getting-runbooks-from-hello-community"></a>Získávání runbooků od komunity hello
Hello [Galerie Runbooků](automation-runbook-gallery.md#runbooks-in-runbook-gallery) obsahuje runbooky od společnosti Microsoft a hello komunity, který můžete použít beze změny ve vašem prostředí nebo si je přizpůsobit pro vlastní účely. Lze je také užitečné tooas odkazy toolearn jak toocreate vlastních runbooků. Můžete je přidat vlastní toohello galerii runbooků si myslíte, že pro ostatní uživatele může být užitečné. 

## <a name="creating-runbooks-with-azure-automation"></a>Vytvoření runbooků pomocí Azure Automation
Můžete [vytvářet vlastní runbooky](automation-creating-importing-runbook.md) z nuly nebo upravovat runbooky z hello [Galerie Runbooků](http://msdn.microsoft.com/library/azure/dn781422.aspx) vlastní požadavky. Existují čtyři různé [typy runbooků](automation-runbook-types.md), ze kterých si můžete vybrat podle svých požadavků a podle prostředí PowerShell. Pokud dáváte přednost toowork přímo s hello kódu PowerShell, pak můžete použít [Powershellový runbook](automation-runbook-types.md#powershell-runbooks) nebo [runbook pracovního postupu Powershellu](automation-runbook-types.md#powershell-workflow-runbooks) upravíte v režimu offline nebo se hello [textový editor](http://msdn.microsoft.com/library/azure/dn879137.aspx) v hello portálu Azure. Pokud dáváte přednost tooedit sady runbook, aniž by musel být vystavené toohello základní kód, pak můžete vytvořit [grafický runbook](automation-runbook-types.md#graphical-runbooks) pomocí hello [grafický editor](automation-graphical-authoring-intro.md) v hello portálu Azure. 

Raději se díváte tooreading? Podíváme se na hello níže uvedené video z relace Microsoft Ignite v květnu 2015. Poznámka: Hello konceptů a funkcí, které jsou popsané v tomto videu jsou sice správné, že Azure Automation má posunula hodně dopředu a vzhledem k tomu, že záznamu tohoto videa, teď má rozsáhlejší uživatelské rozhraní v hello portál Azure a podporuje další funkce.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3451/player]
> 
> 

## <a name="automating-configuration-management-with-desired-state-configuration"></a>Automatizace správy konfigurací pomocí DSC (konfigurace požadovaného stavu)
[DSC prostředí PowerShell](https://technet.microsoft.com/library/dn249912.aspx) je platforma pro správu, který vám umožní toomanage, nasazovat a vynucovat konfiguraci pro fyzické hostitele a virtuální počítače pomocí deklarativní syntaxe Powershellu. Konfigurace můžete definovat na centrálním serveru vyžádané replikace s DSC, kterou můžou cílové počítače automaticky načíst a použít. DSC poskytuje sadu rutin prostředí PowerShell můžete použít toomanage konfigurací a prostředků.  

[Azure Automation DSC](automation-dsc-overview.md) je cloudové řešení pro DSC PowerShellu, které poskytuje služby nezbytné pro podniková prostředí.  Můžete spravovat svoje prostředky DSC v Azure Automation a použití konfigurace toovirtual nebo fyzické počítače, které je načtou ze serveru pro vyžádání obsahu DSC v cloudu Azure hello.  Poskytuje také služby generování sestav, které informují o důležitých událostech, například když se uzly odchylují od svých přiřazených konfigurací a když se používá nová konfigurace. 

## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Vytvoření vlastních konfigurací DSC pomocí Azure Automation
[Konfigurace DSC](automation-dsc-overview.md) zadejte hello požadovaného stavu uzlu.  Můžete použít více uzlů hello stejné konfigurace tooassure, že si všechny udrží identický stav.  Konfiguraci můžete vytvořit pomocí libovolného textového editoru na místním počítači a následně ji naimportovat do Azure Automation, kde ji můžete zkompilovat a použít na uzly.

## <a name="getting-modules-and-configurations"></a>Získávání modulů a konfigurací
Můžete získat [moduly Powershellu](automation-runbook-gallery.md#modules-in-powershell-gallery) obsahující rutiny, které můžete použít v runboocích a konfiguracích DSC z hello [Galerie prostředí PowerShell](http://www.powershellgallery.com/). Můžete spustit tento Galerie z hello portál Azure a naimportovat moduly přímo do Azure Automation, nebo můžete stáhnout a naimportovat je ručně. Hello moduly nemůžete instalovat přímo z hello portálu Azure, ale můžete je stáhnout nainstalovat je stejně jako ostatní moduly. 

## <a name="example-practical-applications-of-azure-automation"></a>Příklad použití Azure Automation v praxi
Tady jsou příklady několika co jsou hello druhů scénářů s Azure Automation. 

* Vytvoření a zkopírování virtuálních počítačů v různých předplatných Azure. 
* Naplánovaný soubor se zkopíruje z kontejneru Azure Blob Storage tooan místního počítače. 
* Automatizace funkcí zabezpečení, například odepření požadavků od klienta při zjištění útoku DoS. 
* Zajištění, aby počítače neustále dodržovaly nakonfigurované zásady zabezpečení.
* Správa průběžného nasazování kódu aplikace v cloudové a místní infrastruktuře. 
* Sestavení doménové struktury služby Active Directory v Azure pro prostředí laboratoře. 
* Zkrácení tabulky v databázi SQL, pokud se databáze blíží k dosažení maximální velikosti. 
* Vzdálená aktualizace nastavení prostředí pro web Azure. 

## <a name="how-does-azure-automation-relate-tooother-automation-tools"></a>Jak Azure Automation souvisí tooother nástrojům pro automatizaci?
[Service Management Automation (SMA)](http://technet.microsoft.com/library/dn469260.aspx) je určený tooautomate úlohy správy v privátním cloudu hello. Instaluje se místně v datovém centru jako součást sady [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/). SMA a Azure Automation používají hello stejný formát runbooků založený na prostředí Windows PowerShell a pracovním postupem prostředí Windows PowerShell, ale SMA nepodporuje [grafické runbooky](automation-graphical-authoring-intro.md).  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) slouží k automatizaci místních prostředků. Používá jiný formát runbooků než Azure Automation a Service Management Automation a má sady runbook grafické rozhraní toocreate bez nutnosti jakéhokoli skriptování. Jeho runbooky se skládají z aktivit z integračních balíčků, které jsou napsané konkrétně pro orchestrátor. 

## <a name="where-can-i-get-more-information"></a>Zdroje dalších informací
Různé druhy prostředků jsou k dispozici pro jste toolearn Další informace o Azure Automation a vytváření vlastních runbooků. 

* **Knihovna Azure Automation** je místem, kde se právě teď nacházíte. Hello články v této knihovně nabízejí ucelenou dokumentaci o hello konfiguraci a správě Azure Automation a o vytváření vlastních runbooků. 
* [Rutiny Azure PowerShell](http://msdn.microsoft.com/library/jj156055.aspx) obsahují informace pro automatizaci operace v Azure pomocí Windows PowerShellu. Sady Runbook použijte tyto rutiny toowork s prostředky Azure. 
* [Blog o správě](https://azure.microsoft.com/blog/tag/azure-automation/) poskytuje hello nejnovější informace o Azure Automation a dalších technologiích pro správu od společnosti Microsoft. Musí se přihlásíte toothis blog toostay až toodate s hello nejnovější od týmu Azure Automation hello. 
* [Fórum Automation](http://go.microsoft.com/fwlink/p/?LinkId=390561) vám umožní toopost dotazy týkající se Azure Automation toobe Microsoftu a komunitě hello. 
* [Rutiny Azure Automation](https://msdn.microsoft.com/library/mt244122.aspx) poskytují informace pro automatizaci úloh správy. Obsahuje rutiny toomanage automatizace účtů, prostředků, runbooků a DSC.

## <a name="can-i-provide-feedback"></a>Posílání připomínek
**Budeme velmi rádi za vaše připomínky.** Pokud hledáte řešení runbooku služby Azure Automation nebo hledáte modul integrace, zadejte v Centru skriptů požadavek na skript. Pokud máte požadavky k připomínkám nebo funkcím Azure Automation, odešlete je na [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback). Děkujeme! 

