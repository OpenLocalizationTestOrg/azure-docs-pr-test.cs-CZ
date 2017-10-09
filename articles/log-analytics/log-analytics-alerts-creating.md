---
title: "výstrahy aaaCreating v OMS Log Analytics | Microsoft Docs"
description: "Výstrahy v analýzy protokolů zjišťovat důležité informace ve svém úložišti OMS a zda lze proaktivně upozorňují na problémy nebo vyvolání akce tooattempt toocorrect je.  Tento článek popisuje, jak toocreate pravidlo výstrahy a podrobnosti hello různé akce jejich trvat."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: 3d035b2426dda9645b19e6c993dc26a2d95a2a78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-alert-rules-in-log-analytics"></a>Práce s pravidla výstrah v analýzy protokolů
Výstrahy jsou vytvářeny pravidla výstrah, které automaticky spustit vyhledávání protokolu v pravidelných intervalech.  Uživatel vytvořit záznam výstrahy, pokud výsledky hello odpovídají konkrétním kritériím.  pravidlo Hello potom automaticky spustit jeden nebo další akce tooproactively oznámíme vám hello výstraha nebo vyvolat jiný proces.   

Tento článek popisuje procesy toocreate hello a upravit pomocí portálu OMS hello pravidla výstrah.  Podrobnosti o hello různá nastavení a jak tooimplement vyžaduje logiky najdete v tématu [vysvětlení výstrah v analýzy protokolů](log-analytics-alerts.md).

>[!NOTE]
> Momentálně nelze vytvořit nebo upravit pravidlo výstrahy pomocí hello portálu Azure. 

## <a name="create-an-alert-rule"></a>Vytvořit pravidlo výstrahy

pravidlo výstrahy pomocí portálu OMS hello, začněte vytvořením protokolu toocreate vyhledejte hello záznamy, které by měla vyvolat výstrahu hello.  Hello **výstraha** tlačítko pak bude k dispozici, můžete vytvořit a nakonfigurovat pravidlo výstrahy hello.

>[!NOTE]
> Maximálně 250 pravidla výstrah lze vytvořit aktuálně v pracovním prostoru OMS. 

1. Na stránce Přehled OMS hello, klikněte na tlačítko **hledání protokolů**.
2. Vytvořit nový dotaz vyhledávání protokolu nebo zvolte hledání, uložený protokol. 
3. Klikněte na tlačítko **výstrahy** hello horní části hello tooopen stránku hello **přidat pravidlo výstrahy** obrazovky.
4. Konfigurace pravidla výstrah hello pomocí informací v [podrobnosti pravidla výstrah](#details-of-alert-rules) níže.
6. Klikněte na tlačítko **Uložit** pravidlo výstrahy toocomplete hello.  Zahájí se okamžitě spuštěna.


## <a name="edit-an-alert-rule"></a>Upravit pravidlo výstrahy
Můžete získat seznam všech pravidel výstrah v hello **výstrahy** nabídky v analýzy protokolů **nastavení**.  

![Správa výstrah](./media/log-analytics-alerts/configure.png)

1. V hello OMS konzoly vyberte hello **nastavení** dlaždici.
2. Vyberte **výstrahy**.

Z tohoto hlediska můžete provádět různé akce.

* Zakázat pravidlo výběrem **vypnout** další tooit.
* Upravte pravidlo výstrahy kliknutím na tlačítko Další tooit pro ikonu tužky hello.
* Odebrat pravidlo výstrahy kliknutím hello **X** další tooit ikonu. 

## <a name="details-of-alert-rules"></a>Podrobnosti o pravidla výstrah
Při vytváření nebo upravit pravidlo výstrahy na portálu OMS hello pracujete s hello **přidat pravidlo výstrah** nebo **upravit pravidlo výstrahy** stránky.  Hello následující tabulky popisují hello pole na této obrazovce.

![Pravidlo výstrahy](media/log-analytics-alerts/add-alert-rule.png)

### <a name="alert-information"></a>Informace o výstrahách
Toto jsou základního nastavení pro pravidlo výstrahy hello a hello výstrahy, kterou vytvoří.

| Vlastnost | Popis |
|:--- |:---|
| Name (Název) | Jedinečný název tooidentify hello pravidlo výstrahy. Tento název je součástí žádné výstrahy vytvořené pravidlem hello.  |
| Popis | Volitelný popis pravidla výstrahy hello. |
| Závažnost |Závažnost žádné výstrahy vytvořených tímto pravidlem. |

### <a name="search-query-and-time-window"></a>Vyhledávací dotaz a časového okna
Hello vyhledávání dotazu a časového okna, které vracejí hello záznamy, které se vyhodnotí toodetermine, pokud by se měl vytvořit žádné výstrahy.

| Vlastnost | Popis |
|:--- |:---|
| Vyhledávací dotaz. | Toto je hello dotaz, který se má spustit.  vrácené tímto dotazem záznamy Hello bude použité toodetermine, zda je vytvořena výstraha.<br><br>Vyberte **použít aktuální vyhledávací dotaz** toouse hello aktuální dotaz nebo vyberte existující uložené hledání hello seznamu.  Hello syntaxe dotazu je uvedený v hello textového pole, kde ji můžete upravit v případě potřeby. |
| Časový interval |Určuje hello časový rozsah pro dotaz hello.  Hello dotaz vrátí pouze záznamy, které byly vytvořeny v tomto rozsahu hello aktuální čas.  To může být libovolná hodnota 5 minut až 24 hodin.  Mělo by být větší než nebo rovna toohello četnost výstrah.  <br><br> Například pokud hello okno bude nastaveno too60 minut a hello dotazů, když se spustí: 15: 00, bude vrácen pouze záznamy vytvořené 12:15:00 až 1:15 hodin. |

Když poskytujete hello časový interval pro pravidlo výstrahy hello, zobrazí se počet hello existující záznamy, které by odpovídaly kritériím hledání hello pro dané časové okno.  To může pomoct určit frekvenci hello, který vám poskytne hello počet výsledků, které byste očekávali.

### <a name="schedule"></a>Plán
Definuje, jak často hello vyhledávání spuštění dotazu.

| Vlastnost | Popis |
|:--- |:---|
| Frekvence výstrah | Určuje, jak často hello dotaz by měl být spuštěn. Může být libovolná hodnota 5 minut až 24 hodin. Musí být rovna tooor menší než hello časový interval.  Pokud hodnota hello je větší než hello časový interval, riskujete záznamů je vynechán.<br><br>Představte si třeba časové okno 30 minut a četnost 60 minut.  Pokud dotaz hello běží v 1:00, vrátí záznamy 12:30 až 1:00 PM.  Hello by spuštění dotazu hello je 2:00, když měla by vrátit záznamy 1:30 až 2:00.  Všechny záznamy vytvořené 1:00 až 1:30 by nikdy vyhodnotí. |


### <a name="generate-alert-based-on"></a>Generují výstrahu založenou na
Definuje hello, které by se měl vytvořit kritéria, která se vyhodnotí proti hello výsledky hello vyhledávání dotazu toodetermine Pokud výstrahu.  Tyto podrobnosti se bude lišit v závislosti na typu hello pravidlo výstrahy, které vyberete.  Můžete získat podrobnosti o pro hello typů jiné pravidlo výstrahy z [vysvětlení výstrah v analýzy protokolů](log-analytics-alerts.md).

| Vlastnost | Popis |
|:--- |:---|
| Potlačit upozornění | Při zapnutí potlačení pro pravidlo výstrahy hello akce pro pravidlo hello jsou zakázány definované dobu, po vytvoření nová výstraha. pravidlo Hello pořád běží a vytvoří výstrahy záznamů, pokud je splněna kritéria hello. Toto je tooallow čas toocorrect hello problém bez spuštění duplicitní akce. |

#### <a name="number-of-results-alert-rules"></a>Počet výsledků pravidla výstrah

| Vlastnost | Popis |
|:--- |:---|
| Počet výsledků |Výstraha se vytvoří, pokud hello počet záznamů vrácených dotazem hello je buď **větší než** nebo **menší než** hello hodnotu zadáte.  |

#### <a name="metric-measurement-alert-rules"></a>Pravidla výstrah metriky měření

| Vlastnost | Popis |
|:--- |:---|
| Agregovaná hodnota | Prahová hodnota každý agregovaná hodnota ve výsledcích hello musí být větší než toobe považuje za porušení. |
| Aktivační událost upozornění na základě | Hello počet porušení u výstrahy toobe vytvořili.  Můžete zadat **celkový počet narušení** pro libovolnou kombinaci narušení napříč hello výsledky nastavit nebo **po sobě jdoucích narušení** toorequire, který hello narušení se musí vyskytovat v po sobě jdoucích vzorků. |

### <a name="actions"></a>Akce
Pravidla výstrah vždy vytvoří [výstrahy záznam](#alert-records) při dosáhla prahová hodnota hello.  Můžete také definovat jeden nebo více odpovědí toobe spustit jako je e-mailu nebo spuštění sady runbook.



#### <a name="email-actions"></a>Akce e-mailu
E-mailu akce Odeslat e-mail s hello podrobností výstrahy tooone hello nebo další příjemce.

| Vlastnost | Popis |
|:--- |:---|
| E-mailové oznámení |Zadejte **Ano** Pokud chcete e-mailu toobe odeslat v případě, že hello výstrahy. |
| Předmět |Subjektu v e-mailu hello.  Nelze upravit hello textu hello pošty. |
| Příjemce |Adresy všech příjemců e-mailu.  Pokud zadáte víc než jednou adresou pak samostatné hello adresy oddělte středníkem (;). |

#### <a name="webhook-actions"></a>Akce Webhooku
Akce Webhooku povolit tooinvoke externího procesu prostřednictvím jedné žádosti HTTP POST.

| Vlastnost | Popis |
|:--- |:---|
| Webhook |Zadejte **Ano** Pokud chcete toocall webhook při aktivaci výstrahy hello. |
| Adresa URL Webhooku |Hello adresa URL webhooku hello. |
| Zahrnout vlastní datovou část JSON |Tuto možnost vyberte, pokud chcete, aby tooreplace hello výchozí datovou část s vlastní datovou část. |
| Zadejte vlastní datovou část JSON |Hello vlastní datovou část webhooku hello.  Najdete v předchozí části Podrobnosti. |

#### <a name="runbook-actions"></a>Akce sady Runbook
Runbook akce spuštění sady runbook ve službě Azure Automation. 

>[!NOTE]
> Musíte mít nainstalován v pracovním prostoru pro tuto akci toobe povoleno řešení služby Automation hello. 


| Vlastnost | Popis |
|:--- |:---|
| Runbook | Zadejte **Ano** Pokud chcete toostart runbook služby automatizace Azure, když se aktivuje výstraha hello.  |
| Účet Automation | Určuje hello účet Automation, jsou z vybrané sady runbook.  Toto je účet hello akce, který má propojené toohello prostoru. |
| Vyberte sadu runbook | Vyberte sadu runbook hello, že chcete toostart, když se vytvoří výstraha. |
| Spusťte na | Vyberte **Azure** toorun hello runbooku v cloudu hello.  Vyberte **hybridní pracovní proces** toorun hello runbook na agenta s [hybridní pracovní proces Runbooku](../automation/automation-hybrid-runbook-worker.md ) nainstalována.  |




## <a name="next-steps"></a>Další kroky
* Nainstalujte hello [řešení pro správu výstrah](log-analytics-solution-alert-management.md) tooanalyze výstrahy vytvořené v analýzy protokolů společně s výstrahy shromážděných ze System Center Operations Manager (SCOM).
* Další informace o [protokolu hledání](log-analytics-log-searches.md) , mohou generovat výstrahy.
* Dokončete průvodce pro [konfigurace webook](log-analytics-alerts-webhooks.md) s pravidlo výstrahy.  
* Zjistěte, jak toowrite [sady runbook ve službě Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problémy identifikovat pomocí výstrah.

