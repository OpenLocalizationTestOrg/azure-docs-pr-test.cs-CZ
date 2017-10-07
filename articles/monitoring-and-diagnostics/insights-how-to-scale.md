---
title: "aaaScale instance počet ručně nebo automaticky škálovat pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak tooscale vašich služeb Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: ancav
ms.openlocfilehash: 8cb78f18416bd3caecce52702a8d630aa434d776
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-instance-count-manually-or-automatically"></a>Ruční nebo automatické škálování počtu instancí
V hello [portálu Azure](https://portal.azure.com/), můžete ručně nastavit hello počet instancí služby nebo můžete nastavit parametry toohave automaticky škálovat na základě poptávky. To je obvykle označují tooas *škálovat* nebo *škálování v*.

Před škálování podle počtu instancí, je třeba zvážit, zda je ovlivňován škálování **cenová úroveň** přidání tooinstance počet. Jiné cenové úrovně může mít odlišné počty jader a paměti, a proto budou mít lepší výkon pro hello stejný počet instancí (což je *škálovat* nebo *snižovat*). Tento článek se zabývá konkrétně *škálování v* a *out*.

Je možné škálovat hello portálu, a můžete také hello [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) nebo [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooadjust ruční nebo automatické škálování.

> [!NOTE]
> Tento článek popisuje, jak toocreate na škálování nastavení na portál hello [http://portal.azure.com](http://portal.azure.com). Nastavení automatického škálování vytvořená na portálu nelze upravit, portál classic hello ([http://manage.windowsazure.com](http://manage.windowsazure.com)).
> 
> 

## <a name="scaling-manually"></a>Škálování ručně
1. V hello [portálu Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet**, toohello prostředků, jako má tooscale, pak přejděte **plán služby App Service**.
2. Klikněte na tlačítko **Nastavení > horizontální navýšení kapacity (plán služby App Service).**
3. Hello horní části hello **škálování** okno se zobrazí historii akcí škálování služby hello.
   
    ![Okno škálování](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)
   
   > [!NOTE]
   > V tomto grafu se zobrazí pouze akce, které provádí automatické škálování. Pokud ručně upravit počet instancí hello, hello změny se neprojeví v tomto grafu.
   > 
   > 
4. Můžete upravit ručně hello číslo **instance** pomocí posuvníku.
5. Klikněte na tlačítko hello **Uložit** příkazu a bude škálovat toothat počet instancí téměř okamžitě.

## <a name="scaling-based-on-a-pre-set-metric"></a>Škálování podle předem nastavte metriku
Pokud chcete hello počet instancí tooautomatically upravit podle metriky, vyberte hello metriku, které chcete v hello **škálovat podle** rozevíracího seznamu. Například **plán služby App Service** je možné škálovat podle **procento využití procesoru**.

1. Když vyberete metriky získáte jezdce nebo není textových polí tooenter hello počet instancí chcete tooscale mezi:
   
    ![Okno škálování s procento využití procesoru](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)
   
    Škálování bude trvat nikdy služby menší než nebo větší hello hranice, které jste nastavili, bez ohledu na to zatížení.
2. Druhý zvolte cílový rozsah hello metrika hello. Například, pokud jste zvolili **procento využití procesoru**, můžete nastavit cíl pro CPU průměrné hello ve všech instancí hello ve službě. Horizontální navýšení kapacity se stane, když hello průměrné využití procesoru překročí maximální hello, které definujete, podobně měřítka ve dojde vždy, když CPU průměrné hello klesne pod hello minimální.
3. Klikněte na tlačítko hello **Uložit** příkaz. Škálování zkontroluje každých několik minut toomake zda jsou v rozsahu instance hello a cíl pro vaše metriku. Když vaše služba přijímá další provoz, zobrazí se více instancí bez jakékoli akce.

## <a name="scale-based-on-other-metrics"></a>Škálování podle jiných metrik
Je možné škálovat podle metriky než hello přednastavení, které se zobrazují v hello **škálovat podle** rozevíracího seznamu a to i mají komplexní sadu možností horizontálního rozšíření kapacity a škálování v pravidlech.

### <a name="adding-or-changing-a-rule"></a>Přidání nebo změna pravidla
1. Zvolte hello **pravidla plánu a výkonu** v hello **škálovat podle** rozevírací: ![pravidla výkonu](./media/insights-how-to-scale/Insights_PerformanceRules.png)
2. Pokud jste dřív měli škálování, zobrazí se v zobrazení hello přesná pravidla, které jste měli.
3. tooscale založené na jiné metriky kliknutím hello **přidat pravidlo** řádek. Můžete také kliknout na jeden z hello stávající řádky toochange z hello metrika jste dřív měli toohello metrika chcete tooscale podle.
   ![Přidat pravidlo](./media/insights-how-to-scale/Insights_AddRule.png)
4. Teď musíte tooselect metriku, které chcete tooscale podle. Když vyberete metriky existuje několik věcí tooconsider:
   
   * Hello *prostředků* hello metrika pochází z. Obvykle to bude možné hello stejné jako hello prostředek, který se škálování. Pokud chcete tooscale podle hello hloubka fronty úložiště, je hello prostředků však hello fronty, který chcete tooscale podle.
   * Hello *název metriky* sám sebe.
   * Hello *časové agregace* hello metriky. To je, jak hello dat je zkombinujte přes hello *doba trvání*.
5. Po výběru vaší metrika zvolíte hello prahovou hodnotu pro metriku hello a operátor hello. Řekněme například, vám může **větší než** **80 %**.
6. Zvolte hello akce, které chcete tootake. Existuje několik různých typ akce:
   
   * Zvyšuje nebo snižuje podle – to přidá nebo odebere hello **hodnotu** počet instancí, které definujete
   * Zvýšení nebo snížení procento – počet instancí hello se změní podle procento. Například by mohlo 25 v hello **hodnotu** pole, a pokud jste měli aktuálně instancí 8, 2 by byl přidán.
   * Zvýšení nebo snížení příliš – to nastaví hello instance počet toohello **hodnotu** definujete.
7. Nakonec můžete nástrojů dolů – jak dlouho by měl po hello předchozí škálování akce tooscale znovu čekat toto pravidlo.
8. Po dokončení konfigurace pravidla dosáhl **OK**.
9. Po nakonfigurování všechna pravidla hello chcete být jisti hello toohit **Uložit** příkaz.

### <a name="scaling-with-multiple-steps"></a>Škálování s více kroků
výše uvedené příklady Hello jsou poměrně základní. Ale pokud chcete toobe agresivnější o škálování nahoru (nebo dolů), můžete i přidat více škálování pravidel pro hello stejné metriky. Můžete například definovat dvě pravidla škálování na procento využití procesoru:

1. Škálování instancí 1, pokud procento využití procesoru je nad 60 %
2. Pokud je procento procesoru vyšší než 85 % škálovat 3 instancí

![Více pravidel škálování](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

S tímto pravidlem další Pokud zatížení překročí 85 % před akce škálování, zobrazí se dva další instance namísto jeden.

## <a name="scale-based-on-a-schedule"></a>Škálování podle plánu
Ve výchozím nastavení když vytvoříte pravidlo škálování bude vždy platit. Můžete uvidíte, že když kliknete na záhlaví profil hello:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Ale můžete chtít toohave agresivnější škálování během hello dne nebo týdne hello, než na hello víkendu. Může i vypnout služby zcela mimo pracovní dobu.

1. toodo, na profil hello máte, vyberte **opakování** místo **vždy,** a zvolte hello krát, které chcete profil tooapply hello.
2. Například toohave profilu, která se použije během hello týden v hello **dní** zrušte zaškrtnutí políčka rozevírací **sobota** a **neděle**.
3. toohave profilu, která se použije během denní, nastavte hello hello **počáteční čas** toohello denní dobu, které chcete toostart v.
   
    ![Výchozí opakování](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)
4. Klikněte na **OK**.
5. Dále je nutné profil hello tooadd chcete tooapply v jinou dobu. Klikněte na tlačítko hello **přidat profil** řádek.
    ![Zaměstnání](./media/insights-how-to-scale/Insights_ProfileOffWork.png)
6. Název vaší nové, druhý profil, například může zavolat **vypnout pracovní**.
7. Potom vyberte **opakování** znovu a zvolte rozsah počet instance hello chcete během této doby.
8. Jako s hello výchozí profil, zvolte hello **dní** chcete tento profil tooapply na a hello **počáteční čas** dni Dobrý den.
   
   > [!NOTE]
   > Škálování použije hello letní úspory pravidla pro podle toho, co **časové pásmo** vyberete. Však během letní čas hello UTC posun zobrazí hello posun základní časového pásma, není hello letního času UTC úspory posun.
   > 
   > 
9. Klikněte na **OK**.
10. Nyní, budete potřebovat tooadd ať pravidla, které chcete tooapply během druhý profil. Klikněte na tlačítko **přidat pravidlo**, a poté může vytvořit hello stejné pravidlo, budete mít během hello výchozí profil.
    
    ![Přidat pravidlo toooff práce](./media/insights-how-to-scale/Insights_RuleOffWork.png)
11. Se stát, že toocreate obou pravidlo pro škálování a škálování v, nebo během hello profil hello počet instancí bude pouze růst (nebo snížit).
12. Nakonec klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky
* [Monitorování služby metriky](insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.
* [Povolit monitorování a Diagnostika](insights-how-to-use-diagnostics.md) toocollect podrobné vysoká frekvence metriky pro vaši službu.
* [Přijímejte oznámení o výstrahách](insights-receive-alert-notifications.md) vždy, když nastanou provozní události nebo když metriky překročí prahovou hodnotu.
* [Monitorování výkonu aplikací](../application-insights/app-insights-azure-web-apps.md) Pokud chcete toounderstand přesně jak kód provádí v cloudu hello.
* [Zobrazení událostí a protokolu aktivity](insights-debugging-with-events.md) toolearn vše, co se stalo ve službě.
* [Sledování dostupnosti a odezvy žádné webové stránce](../application-insights/app-insights-monitor-web-app-availability.md) s Application Insights, můžete zjistit, pokud stránka je vypnutý.

