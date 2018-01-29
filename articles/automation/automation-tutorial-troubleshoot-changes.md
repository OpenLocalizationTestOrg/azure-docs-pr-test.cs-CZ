---
title: "Řešení potíží se změnami na virtuálním počítači Azure | Dokumentace Microsoftu"
description: "Potíže se změnami na virtuálním počítači Azure můžete řešit pomocí řešení Change Tracking."
services: automation
keywords: change, tracking, automation
author: jennyhunter-msft
ms.author: jehunte
ms.date: 12/14/2017
ms.topic: tutorial
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 0aefa175d676bd7e98841d3a1e9ff5a8c90b7deb
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="troubleshoot-changes-in-your-environment"></a>Řešení potíží se změnami ve vašem prostředí

V tomto kurzu zjistíte, jak řešit potíže se změnami na virtuálním počítači Azure. Když povolíte řešení Change Tracking, můžete sledovat změny softwaru, souborů, linuxových procesů démon, služeb systému Windows a klíčů registru Windows na vašich počítačích.
Identifikace těchto změn konfigurace vám může pomoct přesně určit provozní problémy v celém prostředí.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Připojení virtuálního počítače k řešení Change Tracking a Inventory
> * Prohledávání protokolů změn pro zastavené služby
> * Konfigurace sledování změn
> * Povolení připojení protokolu aktivit
> * Aktivace události
> * Zobrazení změn

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* Předplatné Azure. Pokud ještě žádné nemáte, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Účet Automation](automation-offering-get-started.md), který bude obsahovat sledovací proces, runbooky akcí a úlohu sledovacího procesu.
* [Virtuální počítač](../virtual-machines/windows/quick-create-portal.md) pro připojení.

## <a name="log-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Povolení řešení Change Tracking a Inventory

Pro účely tohoto kurzu je nejprve potřeba povolit pro váš virtuální počítač řešení Change Tracking a Inventory. Pokud jste už dříve pro virtuální počítač povolili jiné řešení automatizace, tento krok není nezbytný.

1. V nabídce vlevo vyberte **Virtuální počítače** a ze seznamu vyberte virtuální počítač.
1. V nabídce vlevo v části **Operace** klikněte na **Inventory**. Otevře se stránka **Povolit řešení Change Tracking a Inventory**.

Provede se ověření, pomocí kterého se určí, jestli jsou pro tento virtuální počítač povolená řešení Change Tracking a Inventory.
Toto ověření zahrnuje kontroly pracovního prostoru Log Analytics a propojeného účtu Automation a kontrolu, jestli se řešení nachází v tomto pracovním prostoru.

Pracovní prostor [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) slouží ke shromažďování dat generovaných funkcemi a službami, jako je řešení Inventory.
Tento pracovní prostor poskytuje možnost kontroly a analýzy dat z několika zdrojů na jednom místě.

Proces ověřování také zkontroluje, jestli je virtuální počítač zřízený s agentem Microsoft Monitoring Agent (MMA) a procesem hybrid worker.
Agent slouží ke komunikaci s virtuálním počítačem a získávání informací o nainstalovaném softwaru.
Proces ověřování také zkontroluje, jestli je virtuální počítač zřízený s agentem Microsoft Monitoring Agent (MMA) a hybridním pracovním procesem runbooku Automation.

Pokud se nesplní tyto požadavky, zobrazí se banner nabízející možnost povolit řešení.

![Banner konfigurace připojení k řešení Change Tracking a Inventory](./media/automation-tutorial-troubleshoot-changes/enableinventory.png)

Pokud chcete řešení povolit, klikněte na banner.
Pokud po ověření chyběla některá z následujících požadovaných součástí, automaticky se přidá:

* Pracovní prostor [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)
* [Automation](./automation-offering-get-started.md)
* Povolený [hybridní pracovní proces runbooku](./automation-hybrid-runbook-worker.md) na virtuálním počítači

Otevře se obrazovka řešení **Change Tracking a Inventory**. Nakonfigurujte umístění, pracovní prostor Log Analytics a účet Automation, které se mají použít, a klikněte na **Povolit**. Pokud se pole zobrazují šedě, znamená to, že pro daný virtuální počítač je povolené jiné řešení automatizace a musí se použít stejný pracovní prostor a účet Automation.

![Okno pro povolení řešení Change Tracking](./media/automation-tutorial-troubleshoot-changes/installed-software-enable.png)

Povolení řešení může trvat až 15 minut. Během této doby byste neměli zavírat okno prohlížeče.
Po povolení řešení začnou do Log Analytics proudit informace o nainstalovaném softwaru a změnách na virtuálních počítačích.
Zpřístupnění dat pro analýzu může trvat 30 minut až 6 hodin.


## <a name="using-change-tracking-in-log-analytics"></a>Použití řešení Change Tracking v Log Analytics

Řešení Change Tracking generuje data protokolu, která se odesílají do Log Analytics. Pokud chcete v protokolech hledat spouštěním dotazů, v horní části okna **Change Tracking** vyberte **Log Analytics**.
Data řešení Change Tracking se ukládají jako typ **ConfigurationChange** (Změna konfigurace). Následující ukázka dotazu Log Analytics vrátí všechny zastavené služby systému Windows.

```
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Další informace o provozu a prohledávání souborů protokolů v Log Analytics najdete na stránce [Azure Log Analytics](https://docs.loganalytics.io/index).

## <a name="configure-change-tracking"></a>Konfigurace řešení Change Tracking

Change Tracking poskytuje možnost sledovat změny konfigurace na vašem virtuálním počítači. Následující kroky ukazují, jak nakonfigurovat sledování klíčů registru a souborů.
 
Pokud chcete zvolit, jaké soubory a klíče registru se mají shromažďovat a sledovat, vyberte **Upravit nastavení** v horní části stránky **Change Tracking**.

> [!NOTE]
> Řešení Inventory i Change Tracking používají stejná nastavení shromažďování a nastavení se konfigurují na úrovni pracovního prostoru.

V okně **Konfigurace pracovního prostoru** přidejte klíče registru systému Windows, soubory Windows nebo soubory Linuxu, které chcete sledovat, jak je uvedeno v dalších třech částech.

### <a name="add-a-windows-registry-key"></a>Přidání klíče registru systému Windows

1. Na kartě **Registr systému Windows** vyberte **Přidat**.
    Otevře se okno **Přidat registr systému Windows pro řešení Change Tracking**.

   ![Přidání registru pro řešení Change Tracking](./media/automation-vm-change-tracking/change-add-registry.png)

2. V části **Povoleno** vyberte **Ano**.
3. Do pole **Název položky** zadejte popisný název.
4. (Volitelné) Do pole **Skupina** zadejte název skupiny.
5. Do pole **Klíč registru systému Windows** zadejte název klíče registru, který chcete sledovat.
6. Vyberte **Uložit**.

### <a name="add-a-windows-file"></a>Přidání souboru Windows

1. Na kartě **Soubory Windows** vyberte **Přidat**. Otevře se okno **Přidat soubor Windows pro řešení Change Tracking**.

   ![Change Tracking a soubor Windows](./media/automation-vm-change-tracking/change-add-win-file.png)

2. V části **Povoleno** vyberte **Ano**.
3. Do pole **Název položky** zadejte popisný název.
4. (Volitelné) Do pole **Skupina** zadejte název skupiny.
5. Do pole **Vstupní cesta** zadejte úplnou cestu a název soubor, který chcete sledovat.
6. Vyberte **Uložit**.

### <a name="add-a-linux-file"></a>Přidání souboru Linuxu

1. Na kartě **Soubory Linuxu** vyberte **Přidat**. Otevře se okno **Přidat soubor Linuxu pro řešení Change Tracking**.

   ![Přidání souboru Linuxu pro řešení Change Tracking](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. V části **Povoleno** vyberte **Ano**.
3. Do pole **Název položky** zadejte popisný název.
4. (Volitelné) Do pole **Skupina** zadejte název skupiny.
5. Do pole **Vstupní cesta** zadejte úplnou cestu a název soubor, který chcete sledovat.
6. V poli **Typ cesty** vyberte **Soubor** nebo **Adresář**.
7. V části **Rekurze** vyberte **Zapnuto**, pokud chcete sledovat změny pro zadanou cestu a všechny její vedlejší cesty. Pokud chcete sledovat pouze vybranou cestu nebo soubor, vyberte **Vypnuto**.
8. V části **Použít Sudo** vyberte **Zapnuto**, pokud chcete sledovat soubory, které pro přístup vyžadují příkaz `sudo`. Jinak vyberte **Vypnuto**.
9. Vyberte **Uložit**.

## <a name="enable-activity-log-connection"></a>Povolení připojení protokolu aktivit

Na stránce **Change Tracking** na vašem virtuálním počítači vyberte **Správa připojení protokolu aktivit**. Tato úloha otevře stránku **Protokol aktivit Azure**. Vyberte **Připojit** a propojte řešení Change Tracking s protokolem aktivit Azure pro váš virtuální počítač.

Když je toto nastavení povolené, přejděte na stránku **Přehled** vašeho virtuálního počítače a výběrem **Zastavit** virtuální počítač zastavte. Po zobrazení výzvy vyberte **Ano** a zastavte virtuální počítač. Až bude přidělení vašeho virtuálního počítače zrušeno, vyberte **Spustit** a restartujte ho.

Zastavení a spuštění virtuálního počítače zapíše tuto událost do jeho protokolu aktivit Vraťte se na stránku **Change Tracking**. Vyberte **Události** v dolní části stránky. Po chvíli se události zobrazí v grafu a tabulce. Podobně jako v předchozím kroku je možné každou událost vybrat a zobrazit o ní podrobné informace.

![Zobrazení podrobnosti o změnách na portálu](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

## <a name="view-changes"></a>Zobrazení změn

Jakmile budou řešení Change Tracking a Inventory povolená, můžete zobrazit výsledky na stránce **Change Tracking**.

Ve vašem virtuálním počítači v části **OPERACE** vyberte **Change Tracking**.

![Snímek obrazovky, který obsahuje seznam změn do virtuálního počítače](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

Tento graf ukazuje změny, ke kterým došlo v průběhu času.
Po přidání připojení protokolu aktivit zobrazuje čára grafu úplně nahoře události protokolu aktivit Azure.
Jednotlivé řádky grafu reprezentují různé typy sledovatelných změn.
Tyto typy jsou linuxové procesy démon, soubory, klíče registru systému Windows, software a služby pro Windows.
Karta Změny zobrazuje podrobnosti o změnách znázorněných ve vizualizaci v sestupném pořadí podle času, kdy ke změně došlo (nejnovější je první).
Na kartě **Události** zobrazuje tabulka připojené události protokolu aktivit a související podrobnosti, přičemž nejnovější události jsou uvedené jako první.

Ve výsledcích vidíte, že došlo k několika změnám systému, včetně změn služeb a softwaru. Pomocí filtrů v horní části stránky můžete výsledky filtrovat podle **typu změny** nebo časového rozsahu.

Vyberte nějakou změnu **WindowsServices** (Služby pro Windows), tím se otevře okno **Podrobnosti o změně**. Okno s podrobnostmi o změně zobrazuje podrobnosti o změně spolu s hodnotami před a po změně. V tomto případě se služba Ochrana softwaru zastavila.

![Zobrazení podrobnosti o změnách na portálu](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Připojení virtuálního počítače k řešení Change Tracking a Inventory
> * Prohledávání protokolů změn pro zastavené služby
> * Konfigurace sledování změn
> * Povolení připojení protokolu aktivit
> * Aktivace události
> * Zobrazení změn

Další informace najdete v přehledu řešení Change Tracking a Inventory.

> [!div class="nextstepaction"]
> [Řešení Change Tracking a Inventory](../log-analytics/log-analytics-change-tracking.md?toc=%2fazure%2fautomation%2ftoc.json)
