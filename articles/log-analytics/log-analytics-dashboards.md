---
title: "aaaCreate vlastní řídicí panel v Azure Log Analytics | Microsoft Docs"
description: "Tato příručka vám pomůže pochopit, jak řídicí panely analýzy protokolů můžete vizualizovat všechny svoje uložený protokol vyhledávání, která poskytuje jeden objektivu tooview prostředí."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 73fcf131a91c743d473f37d5a40d52eaf78a7ba3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Vytvořit vlastní řídicí panel pro použití v analýzy protokolů

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak nelze vytvořit nové řídicí panely nebo upravit existující řídicí panely. 

Tato příručka vám pomůže pochopit, jak řídicí panely analýzy protokolů můžete vizualizovat všechny svoje uložený protokol vyhledávání, která poskytuje jeden objektivu tooview prostředí.

![Příklad řídicího panelu](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Všechny hello vlastní řídicí panely, které vytvoříte na portálu OMS hello jsou také k dispozici v hello OMS mobilní aplikace. Viz následující stránky pro další informace o aplikacích hello hello.

* [Mobilní aplikace OMS z hello Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [OMS mobilních aplikací z Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobilní řídicí panel](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Vytvoření Můj řídicí panel
toobegin, přejděte toohello OMS Přehled stránky. Uvidíte hello **vlastní řídicí panel** dlaždice na levé straně hello. Klikněte na něj toodrill dolů do vašeho řídicího panelu.

![Přehled](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Přidat dlaždice
V řídicí panely dlaždice se používá technologii uložený protokol hledání. OMS se dodává s mnoha předem provedené uložený protokol hledání, abyste mohli začít. Použití hello následující kroky tohoto outline jak toobegin.

V zobrazení řídicího panelu Moje hello, jednoduše klikněte **přizpůsobit** tooenter přizpůsobit režimu.

![Obrazové](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Hello panely, které se otevře na pravé straně hello hello stránky zobrazí všechny pracovního prostoru uložený protokol hledání. toovisualize uložený protokol hledání jako dlaždici, najeďte myší na uloženého hledání a potom klikněte na hello **plus** symbol.

![Přidat dlaždice 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Když kliknete na tlačítko hello **plus** symbolů, nová dlaždice se zobrazí v zobrazení řídicího panelu Moje hello.

![Přidat dlaždice 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Upravit dlaždici
V zobrazení řídicího panelu Moje hello, jednoduše klikněte **přizpůsobit** tooenter přizpůsobit režimu. Klikněte na dlaždici hello chcete tooedit. Hello pravém panelu změny tooedit a poskytuje výběr možností:

![Upravit vedle sebe](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Upravit vedle sebe](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Vizualizace dlaždice
Existují tři druhy toochoose vizualizace dlaždice z:

| Typ grafu | Jak funguje |
| --- | --- |
| ![Pruhový graf](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Časová osa výsledků hledání uložený protokol zobrazí jako pruhový graf nebo seznam výsledků podle pole v závislosti na tom, pokud vyhledávání protokolu agreguje výsledky podle pole, nebo ne. |
| ![Metrika](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Zobrazí vaše přístupů výsledek hledání celkový protokolu jako číslo v dlaždici. Metriky dlaždice povolit tooset prahovou hodnotu, která bude zvýrazněte hello dlaždice, když je dosaženo prahové hodnoty hello. |
| ![řádek](./media/log-analytics-dashboards/oms-dashboards-line.png) |Jako spojnicový graf zobrazuje časová osa přístupů vaší uložený protokol hledání výsledek s hodnotami. |

### <a name="threshold"></a>Prahová hodnota
Na dlaždici pomocí hello metriky vizualizace můžete vytvořit prahovou hodnotu. Vyberte na toocreate prahovou hodnotu na dlaždici hello. Zvolte, zda toohighlight hello dlaždice, pokud je hodnota hello nad nebo pod prahovou hodnotou hello vybrali, pak nastavit mezní hodnotu hello níže.

## <a name="organizing-hello-dashboard"></a>Uspořádání hello řídicí panel
tooorganize přejděte toohello vlastní řídicí panel zobrazení řídicího panelu a klikněte na **přizpůsobit** tooenter přizpůsobit režimu. Klikněte na tlačítko a přetáhněte dlaždici hello chcete toomove a přesunout ho toowhere chcete, aby vaše toobe dlaždice.

![Uspořádání řídicího panelu](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Odebrat dlaždici
tooremove dlaždici, přejděte toohello vlastní řídicí panel zobrazit a klikněte na tlačítko **přizpůsobit** tooenter přizpůsobit režimu. Vyberte hello dlaždice, které tooremove, a potom na pravém panelu hello vyberte **odebrat dlaždici**.

![Odebrat dlaždici](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Další kroky
* Vytvoření [výstrahy](log-analytics-alerts.md) v oznámení toogenerate analýzy protokolů a tooremediate problémů.
