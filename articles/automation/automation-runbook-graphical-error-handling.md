---
title: "aaaError zpracování v grafické runbooky Azure Automation | Microsoft Docs"
description: "Tento článek popisuje, jak tooimplement chyba zpracování logiky v grafické runbooky Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/26/2016
ms.author: magoedte
ms.openlocfilehash: b9ff01361d2ebd9c0174b074a7a290b1cc2fd1c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Zpracování chyb v grafických runboocích Azure Automation

Hlavní tooconsider klíče runbook návrhu je identifikace různé problémy, které může docházet k sadě runbook. Tyto problémy mohou zahrnovat úspěch, očekávané chybové stavy a neočekávané chybové podmínky.

Runbooky by měly zahrnovat zpracování chyb. toovalidate hello výstup aktivity nebo zpracování chyby s grafickými runbooky, můžete pomocí aktivity kód prostředí Windows PowerShell, definovat podmíněnou logiku na odkaz výstup hello hello aktivity nebo použít jinou metodu.          

Často Pokud neukončující chybu, která vyskytuje v aktivitě sady runbook, aktivit, které řídí zpracování bez ohledu na to hello chyby. Hello chyba je pravděpodobně toogenerate výjimku, ale toorun je stále povolené hello další aktivity. Toto je hello tak, že prostředí PowerShell je navrženou toohandle chyby.    

Hello typů chyb prostředí PowerShell, které se můžou vyskytnout během provádění se ukončuje nebo jiných ukončován. Hello rozdíly mezi ukončující a jiných ukončován chyby jsou následující:

* **Ukončení chyba**: závažné chyby během provádění, která zcela ukončí příkaz hello (nebo spuštění skriptu). Příkladem jsou neexistující rutiny, chyby syntaxe znemožňující spuštění rutiny nebo jiné závažné chyby.

* **Ukončení bez chyby**:-závažnou chybu, který umožňuje spuštění toocontinue ohledu na selhání hello. Jedná se například o operační chyby, jako je nenalezení souboru nebo problémy s oprávněním.

Došlo k vylepšení grafické runbooky služby automatizace Azure s zpracování chyb tooinclude schopností hello. Nyní můžete přepnout výjimky na neukončující chyby a vytvořit chybová propojení mezi aktivitami. Tento proces umožňuje Autor sady runbook toocatch chyby a spravovat zjištěné nebo neočekávané podmínky.  

## <a name="when-toouse-error-handling"></a>Při zpracování chyb toouse

Vždy, když je důležité aktivita, která vyvolá výjimku nebo výjimce, je důležité tooprevent hello další aktivitu v sadě runbook z chyby hello zpracování a toohandle správně. To je obzvláště důležité v případě, kdy na vašich runboocích závisí podnikový proces nebo proces servisních operací.

Pro každou aktivitu, která může dojít k chybě můžete přidat Autor sady runbook hello k chyba odkazu tooany dalších aktivit.  Cílová aktivita Hello můžou být jakéhokoli typu, včetně kódu aktivity vyvolání rutiny, vyvolání jinou sadu runbook a tak dále.

Kromě toho hello cílová aktivita může mít odchozí propojení. Může se jednat o běžná propojení nebo chybová propojení. To znamená, že autor sady runbook hello můžete implementovat komplexní logiku zpracování chyb bez opětné řazení tooa code aktivity. Hello doporučuje postupem je toocreate vyhrazené runbook zpracování chyb s běžné funkce, ale není to povinné. Logika zpracování chyb v aktivitě kódu prostředí PowerShell, které není hello pouze možnost.  

Například vezměte v úvahu sadu runbook, která se pokusí toostart virtuálního počítače a instalace aplikace na něm. Pokud hello virtuálních počítačů nelze správně spustit, provádí dvě akce:

1. Odešle oznámení o tomto problému.
2. Spustí jiný runbook, který automaticky zřídí nový virtuální počítač.

Jedno řešení je toohave k chybě odkazu tooan aktivita zpracovává krok jeden. Například může připojit hello **Write-Warning** rutiny tooan aktivity pro kroku 2, jako je například hello **Start-AzureRmAutomationRunbook** rutiny.

Toto chování pro použití v mnoha sady runbook může také generalize umístěním tyto dvě aktivity v samostatných chyba zpracování sady runbook a pokynů hello navrhované dříve. Před voláním této sady runbook zpracování chyb, můžete vytvořit vlastní zprávu od hello data v původní runbook hello a předejte ji jako parametr toohello zpracování chyb runbooku.

## <a name="how-toouse-error-handling"></a>Jak toouse zpracování chyb

V nastavení konfigurace každé aktivity je možnost přepnout výjimky na neukončující chyby. Standardně je toto nastavení zakázáno. Doporučujeme vám povolit toto nastavení na žádnou aktivitu, kam chcete toohandle chyby.  

Povolením této konfiguraci jsou zajistit, že ukončující i jiných ukončován chyby v aktivitě hello jsou zpracovávány jako neukončující chyby a může být zpracována s odkazem k chybě.  

Po konfiguraci tohoto nastavení, vytvořte aktivitu, která zpracovává chyby hello. Pokud všechny chyby vytvoří aktivita, pak hello odchozí chyba, kterou dodržíte odkazy a hello regulární odkazy nejsou, ani když hello aktivita vytváří také regulární výstup.<br><br> ![Příklad chybového propojení v runbooku Automation](media/automation-runbook-graphical-error-handling/error-link-example.png)

V následujícím příkladu hello načte runbook proměnné, která obsahuje název počítače hello virtuálního počítače. Pokusí toostart hello virtuální počítač s další aktivitu hello.<br><br> ![Příklad zpracování chyb v runbooku Automation](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

Hello **Get-AutomationVariable** aktivity a **Start-AzureRmVm** jsou nakonfigurované tooconvert tooerrors výjimky.  Pokud jsou problémy, získávání hello proměnnou nebo počáteční hello virtuálních počítačů a potom se generují chyby.<br><br> ![Nastavení aktivity pro zpracování chyb v runbooku Automation](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Chyba propojení toku z těchto aktivit tooa jeden **Chyba správy** aktivitu (aktivita kód). Tato aktivita je nakonfigurován s jednoduchý výraz prostředí PowerShell, který používá hello *Throw* – klíčové slovo toostop zpracování spolu s *$Error.Exception.Message* tooget uvítací zprávu, která popisuje hello aktuální výjimku.<br><br> ![Příklad kódu pro zpracování chyb v runbooku Automation](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Další kroky

* toolearn informace o propojení a typy odkazu v grafické runbooky najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md#links-and-workflow).

* Další informace o spuštění sady runbook, jak toomonitor úlohy a další technické podrobnosti najdete v tématu toolearn [sledovat úlohy runbooku](automation-runbook-execution.md).
