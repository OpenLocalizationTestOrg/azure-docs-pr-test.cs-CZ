---
title: "aaaCalling Runbook služby Azure Automation z protokolu analýzy oznámení na | Microsoft Docs"
description: "Tento článek obsahuje přehled o tooinvoke runbook služby automatizace Microsoft OMS Log Analytics výstraze."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: 8b745d6e6c2b0294d676e010f52855cd51741cf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Volání runbooku Azure Automation z upozornění OMS Log Analytics

Při výstraha je nakonfigurovaný v toocreate analýzy protokolů selže záznam výstrahy, pokud se výsledky shodují konkrétní kritéria, například provozu v případě delších Špička při využití procesoru nebo je určitá aplikace proces kritické toohello funkce obchodní aplikace a zapíše je odpovídající událost v protokolu událostí systému Windows hello, tuto výstrahu můžete automaticky spustit runbook služby automatizace v tooauto pokusu o-opravit problém hello.  

Existují dvě možnosti toocall sady runbook při konfiguraci hello výstrahy.  Konkrétně:

1. Pomocí webhooku.
   * Toto je hello pouze možnost k dispozici, pokud vaším pracovním prostorem OMS není propojené tooan účet Automation.
   * Pokud už máte pracovní prostor služby Automation účet propojené tooan OMS, tato možnost je stále k dispozici.  

2. Přímým výběrem runbooku.
   * Tato možnost je dostupná jenom v případě, že je pracovní prostor OMS hello propojené tooan účet Automation.  

## <a name="calling-a-runbook-using-a-webhook"></a>Volání runbooku pomocí webhooku

Webhook, jehož vám umožní toostart konkrétní runbook ve službě Azure Automation prostřednictvím jedné žádosti HTTP.  Před konfigurací hello [analýzy protokolů výstraha](../log-analytics/log-analytics-alerts.md#alert-rules) toocall hello runbooku pomocí webhook, jehož jako výstrah akce, budete potřebovat toofirst vytvoření webhooku hello sady runbook, který bude volán pomocí této metody.  Zkontrolujte a postupujte podle kroků hello v hello [vytvořit webhook, jehož](automation-webhooks.md#creating-a-webhook) článek a zapamatovat si adresa URL webhooku hello toorecord tak, aby můžete použít při konfiguraci hello pravidlo výstrahy.   

## <a name="calling-a-runbook-directly"></a>Přímé volání runbooku

Pokud máte hello automatizace & ovládacího prvku nabídka nainstalován a nakonfigurován v vaším pracovním prostorem OMS při konfiguraci hello Runbook akce možnost hello výstrahy, můžete zobrazit všechny sady runbook z hello **vyberte sadu runbook** rozevíracího seznamu a vyberte hello konkrétní sady runbook, chcete toorun v odpovědi toohello výstrahy.  Hello vybrané sady runbook můžete spustit v pracovním prostoru v hello cloudu Azure nebo na hybridní pracovní proces runbooku.  Při hello výstraha je vytvořený pomocí hello runbook možnost, vytvoří webhook, jehož hello sady runbook.  Hello webhooku můžete zobrazit, pokud přejdete toohello účet Automation a přejděte toohello webhooku okno hello vybrané sady runbook.  Pokud odstraníte hello výstrahu, není odstranit hello webhook, ale hello uživatele můžete ručně odstranit hello webhook.  Ho se nejedná o problém, pokud hello webhooku se neodstraní, je, že právě osamocené položku, která bude případně nutné toobe v pořadí toomaintain uspořádány účet Automation.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>Vlastnosti runbooku (pro obě možnosti)

Obě metody pro volání hello runbooku z hello analýzy protokolů výstrahy mají různé chování charakteristiky, které je třeba toobe rozumí před konfigurací pravidel výstrah.  

* Musíte mít vstupní parametr runbooku s názvem **WebhookData**, který je typu **Objekt**.  Může být povinný nebo volitelný.  Výstraha Hello předá hello vyhledávání výsledky toohello runbooku pomocí tento vstupní parametr.

        param  
         (  
          [Parameter (Mandatory=$true)]  
          [object] $WebhookData  
         )

*  Musíte mít kód tooconvert hello WebhookData tooa prostředí PowerShell objekt.

    `$SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value`

    *$SearchResults* bude pole objektů; každého objektu obsahuje pole hello hodnotami z výsledek jeden hledání

### <a name="webhookdata-inconsistencies-between-hello-webhook-option-and-runbook-option"></a>WebhookData nekonzistence mezi hello webhooku možnost a možnost sady runbook

* Při konfiguraci výstrah toocall Webhook, jehož, zadejte adresu URL webhooku jste vytvořili pro sady runbook a klikněte na tlačítko hello **Test Webhooku** tlačítko.  Hello výsledné WebhookData odeslané toohello runbook buď neobsahuje *. SearchResult* nebo *. SearchResults*.

*  Pokud uložíte výstrahy, když hello výstraha aktivuje a volá hello webhooku, obsahuje hello WebhookData odeslané toohello runbook *. SearchResult*.
* Pokud vytvoříte výstrahu a nakonfigurovat ji toocall runbook (které také vytvoří webhook, jehož), když hello výstrahy aktivačních událostí, odešle WebhookData toohello runbook, která obsahuje *. SearchResults*.

Proto v předchozím příkladu kódu hello, budete potřebovat tooget *. SearchResult* Pokud hello výstrahu vyvolá webhook, jehož a bude nutné tooget *. SearchResults* Pokud hello výstraha přímo volá sadu runbook.

## <a name="example-walkthrough"></a>Ukázka podrobného postupu

Popisuje, jak to funguje s použitím následujících grafický runbook příklad, který spouští službu systému Windows hello.<br><br> ![Spuštění grafického runbooku služby pro Windows](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

Sada runbook Hello má jeden vstupní parametr typu **objekt** který je volán **WebhookData** a zahrnuje data webhooku hello předat z výstrahy obsahující hello *. SearchResults*.<br><br> ![Vstupní parametry runbooku](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

V tomto příkladu v analýzy protokolů jsme vytvořili dva vlastní pole, *SvcDisplayName_CF* a *SvcState_CF*, hello tooextract služby zobrazovaný název a hello stavu služby hello (tj. spuštěná nebo zastavená) z událostí hello zapisují toohello protokolu událostí systému.  Potom jsme hello následující vyhledávací dotaz vytvořit pravidlo výstrahy: `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` tak, aby jsme můžete zjistit při zastavení hello službu zařazování tisku v systému Windows hello.  Může být jakoukoli službu, které vás zajímají, ale v tomto příkladu jsme odkazují na jednu z hello existující služby, které jsou součástí operačního systému Windows hello.  výstrahy akce Hello je nakonfigurované tooexecute náš runbook použité v tomto příkladu a spustit na hello hybridní pracovní proces Runbooku, které jsou povolené na hello cílových systémů.   

Hello kód aktivity sady runbook **získat název služby z LA** převede hello řetězec ve formátu JSON do služby typ objektu a na položku hello filtr *SvcDisplayName_CF* tooextract hello zobrazovaný název hello Windows služby a to předat do hello další aktivitu, která ověří hello služba je zastavena před pokusem o toorestart ho.  *SvcDisplayName_CF* je [vlastní pole](../log-analytics/log-analytics-custom-fields.md) vytvořené v toodemonstrate analýzy protokolů v tomto příkladu.

    $SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value
    $SearchResults.SvcDisplayName_CF  

Při zastavení služby hello, pravidlo výstrahy hello v analýzy protokolů zjištění shody a aktivovat sadu runbook hello a odeslat runbook toohello hello kontext výstrahy. Hello runbook bude trvat akce tooverify hello služba zastavena, a pokud tak pokus o toorestart hello služby a ověřte ji správně spuštěna a výstup hello výsledků.     

Případně pokud nemáte pracovní prostor OMS tooyour propojený účet Automation, můžete by nakonfigurovat pravidlo výstrahy hello webhooku akce tootrigger hello runbooku a nakonfigurovat hello runbook tooconvert hello řetězec ve formátu JSON a filtr na *. SearchResult* pokynů hello již bylo zmíněno dříve.    

## <a name="next-steps"></a>Další kroky

* Další informace o výstrahy v analýzy protokolů toolearn a jak toocreate, najdete v části [výstrahy v analýzy protokolů](../log-analytics/log-analytics-alerts.md).

* jak zjistit, tootrigger sady runbook pomocí webhooku, toounderstand [Azure Automation webhooky](automation-webhooks.md).
