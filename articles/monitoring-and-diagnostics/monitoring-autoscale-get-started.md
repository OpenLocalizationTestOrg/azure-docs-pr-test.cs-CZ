---
title: "aaaGet začít s škálování v Azure | Microsoft Docs"
description: "Zjistěte, jak tooscale prostředku v Azure."
author: rajram
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: rajram
ms.openlocfilehash: 6b3c3f4529018dcaf9691c538fec63dfbb3cea06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-autoscale-in-azure"></a>Začínáme s škálování v Azure
Tento článek popisuje, jak tooset nastavení automatického škálování prostředku v portálu Microsoft Azure hello.

Azure monitorování škálování se vztahují pouze sady škálování počítače toovirtual, cloudové služby, plány služby Azure App Service a služby App Service Environment. 

## <a name="discover-hello-autoscale-settings-in-your-subscription"></a>Zjištění nastavení automatického škálování hello v rámci vašeho předplatného
Můžete zjistit všechny prostředky hello, na které se vztahuje v Azure monitorování škálování. Pomocí následujících kroků pro podrobný návod hello:

1. Otevřete hello [portálu Azure.][1]
2. Klikněte v levém podokně hello hello ikonu monitorování Azure.
  ![Otevřete Azure monitorování][2]
3. Klikněte na tlačítko **škálování** tooview všechny hello prostředky pro škálování, které se vztahuje, včetně jejich aktuálního stavu škálování.
  ![Zjistit škálování v Azure monitorování][3]

V horní tooscope hello dolů hello seznamu tooselect prostředky v určité skupiny zdrojů, konkrétní typy prostředků nebo konkrétní prostředek, můžete použít podokno filtru hello.

Pro každý prostředek zjistíte hello aktuální počet instancí a stav automatického škálování hello. Hello stav škálování může být:

- **Není nakonfigurováno**: jste nepovolili automatické škálování ještě pro tento prostředek.
- **Povolit**: jste povolili automatické škálování pro tento prostředek.
- **Zakázané**: Zakázání automatického škálování pro tento prostředek.

## <a name="create-your-first-autoscale-setting"></a>Vytvoření vaší první nastavení automatického škálování

Pojďme nyní projít jednoduché podrobný návod toocreate vaše první nastavení automatického škálování.

1. Otevřete hello **škálování** okno v Azure monitorování a vyberte prostředek, který má tooscale. (hello následující kroky použít plán služby App Service přidružených k webové aplikaci. Můžete [vytvořte první webové aplikace ASP.NET v Azure během 5 minut.] [4])
2. Všimněte si, že je aktuální počet instancí hello 1. Klikněte na tlačítko **povolit škálování**.
  ![Nastavení škálování pro novou webovou aplikaci][5]
3. Zadejte název pro nastavení možností horizontálního rozšíření hello a pak klikněte na tlačítko **přidat pravidlo**. Všimněte si, možnosti hello škálování pravidlo, které se otevřou jako kontext podokně na pravé straně hello. Ve výchozím nastavení nastaví tato tooscale možnost hello instanci počet o 1, pokud hello procento využití procesoru hello prostředku přesahuje 70 procent. Nechte na jeho výchozí hodnoty a klikněte na **přidat**.
  ![Vytvoření nastavení škálování pro webovou aplikaci][6]
4. Nyní jste vytvořili vaší první pravidlo škálování. Všimněte si, že hello UX doporučuje osvědčených postupů a informací, že "se doporučuje toohave alespoň jeden škálování v pravidle." toodo tak:
  
    a. Klikněte na tlačítko **přidat pravidlo**. 

    b. Nastavit **operátor** příliš**menší než**.

    c. Nastavit **prahová hodnota** příliš**20**.

    d. Nastavit **operace** příliš**snížit počet podle**.

   Teď byste měli mít nastavení škálování, měřítka na více systémů nebo měřítka v podle využití procesoru.
   ![Škálování podle využití procesoru][8]
5. Klikněte na **Uložit**.

Blahopřejeme! Nyní jste úspěšně vytvořili vaší první tooautoscale nastavení škálování webové aplikace podle využití procesoru.

> [!NOTE] 
> Hello stejné kroky jsou příslušné tooget začít s škálování virtuálních počítačů sady nebo cloudové služby role.

## <a name="other-considerations"></a>Další důležité informace
### <a name="scale-based-on-a-schedule"></a>Škálování podle plánu
Kromě toho tooscale podle využití procesoru, můžete nastavit vaše škálování odlišně pro konkrétní dny v týdnu hello.

1. Klikněte na tlačítko **přidat podmínku škálování**.
2. Nastavení škálování hello pravidla režimu a hello je hello stejné jako hello výchozí podmínku.
3. Vyberte **opakujte konkrétní dny** hello plánu.
4. Vyberte hello dny a čas zahájení a ukončení hello kdy má být použita podmínka škálování hello.

![Podmínka škálování podle plánu][9]
### <a name="scale-differently-on-specific-dates"></a>Jinak škálování na konkrétní kalendářní data
Kromě toho tooscale podle využití procesoru, můžete nastavit vaše škálování odlišně pro konkrétní kalendářní data.

1. Klikněte na tlačítko **přidat podmínku škálování**.
2. Nastavení škálování hello pravidla režimu a hello je hello stejné jako hello výchozí podmínku.
3. Vyberte **zadejte počáteční a koncové datum** hello plánu.
4. Vyberte hello počáteční a koncové datum a čas zahájení a ukončení hello kdy má být použita podmínka škálování hello.

![Škálování podmínky na základě dat][10]

### <a name="view-hello-scale-history-of-your-resource"></a>Zobrazit historii hello škálování prostředku
Vždy, když prostředek je škálovat nahoru nebo dolů, je v protokolu aktivit hello zaprotokoluje událost. Můžete zobrazit historii hello škálování prostředku pro hello posledních 24 hodin přepnutím toohello **historie spouštění** kartě.

![Historie spouštění][11]

Pokud chcete, aby tooview hello dokončení škálování historie (pro až too90 dnů), vyberte **kliknutím sem toosee podrobnosti**. Protokol aktivit Hello otevře s škálování předem vybraná pro prostředek a kategorie.

### <a name="view-hello-scale-definition-of-your-resource"></a>Zobrazení hello škálování definice prostředku
Při automatickém škálování je prostředek Azure Resource Manager. Můžete zobrazit hello škálování definice ve formátu JSON přepínání toohello **JSON** kartě.

![Definice škálování][12]

Můžete provádět změny v kódu JSON přímo, pokud je to nutné. Tyto změny se projeví po uložení je.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Zakažte automatické škálování a ručně škálovat vaše instance
Je možné doby kdy chcete toodisable vaše aktuální nastavení škálování a ručně škálovat prostředek.

Klikněte na tlačítko hello **zakázat automatické škálování** tlačítka v horní části hello.
![Zakázat automatické škálování][13]

> [!NOTE] 
> Tato možnost zakáže konfiguraci. Však můžete získat zpět tooit po znovu povolte automatické škálování. 

Teď můžete nastavit hello počet instancí, které chcete tooscale toomanually.

![Ruční škálování sady][14]

Kliknutím můžete kdykoli vrátit tooAutoscale **povolit škálování** a potom **Uložit**.

## <a name="next-steps"></a>Další kroky
- [Vytvoření aktivity protokolu výstrahy toomonitor všechny operace škálování modul vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Vytvoření aktivity protokolu výstrahy toomonitor všechny neúspěšné operace škálování nebo škálovatelnou škálování na vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

