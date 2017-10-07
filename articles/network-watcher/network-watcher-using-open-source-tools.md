---
title: "aaaVisualize typy provozu sítě s otevřeným zdrojem nástroje a sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka popisuje, jak zachytit toouse sledovací proces sítě paketů s Capanalysis toovisualize provoz vzory tooand z virtuálních počítačů."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fca9a226729162cd90d412c7b699ac54d2257a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-network-traffic-patterns-tooand-from-your-vms-using-open-source-tools"></a>Vizualizace tooand vzory pro provoz sítě z virtuálních počítačů pomocí nástroje s otevřeným zdrojem

Zachycení paketu obsahovat dat sítě, která umožňují tooperform sítě forenzní účely a hloubkové kontroly paketů. Existuje mnoho otevře source nástroje, které můžete použít tooanalyze paketu zachycení toogain přehledy o vaší síti. Jeden takový nástroj je CapAnalysis, nástroj vizualizace zachytávání paketů na s otevřeným zdrojem. Vizualizace dat zachytávání paketů je výhodný způsob tooquickly odvozena Statistika na vzory a anomálie v rámci vaší sítě. Vizualizace také umožňují sdílení tyto přehledy snadné použití způsobem.

Sledovací proces sítě Azure poskytuje že Hello možnost toocapture, které tato data cenné tím, že jste tooperform paketu zachytí ve vaší síti. V tomto článku poskytujeme návod, jak toovisualize a získat přehled o paketu zaznamená CapAnalysis pomocí sledovací proces sítě.

## <a name="scenario"></a>Scénář

Máte jednoduché webové aplikace nasazené na virtuálním počítači v Azure chcete toouse s otevřeným zdrojem nástroje toovisualize identifikovat jeho síťový provoz tooquickly toku vzory a možné anomálie. S sledovací proces sítě můžete získat zachytáváním paketů prostředí sítě a uložte ho přímo na vašem účtu úložiště. CapAnalysis můžete ingestování hello zachytáváním paketů přímo z objektu blob úložiště hello a vizualizovat její obsah.

![scénář][1]

## <a name="steps"></a>Kroky

### <a name="install-capanalysis"></a>Nainstalujte CapAnalysis

tooinstall CapAnalysis na virtuálním počítači, najdete pokyny oficiální toohello sem https://www.capanalysis.net/ca/how-to-install-capanalysis.
V pořadí CapAnalysis vzdálený přístup, potřebujeme tooopen port 9877 na vašem virtuálním počítači tak, že přidáte nové pravidlo příchozí zabezpečení. Další informace o vytváření pravidla ve skupinách zabezpečení sítě, naleznete příliš[vytvořit pravidla v existující skupině](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg). Jakmile hello pravidlo byl úspěšně přidán, musí být schopný tooaccess CapAnalysis z`http://<PublicIP>:9877`

### <a name="use-azure-network-watcher-toostart-a-packet-capture-session"></a>Pomocí paketu zaznamenání relace toostart sledovací proces sítě Azure

Sledovací proces sítě vám umožní toocapture pakety tootrack provoz do/z virtuálního počítače. Najdete pokyny toohello v [zachytávali sledovací proces sítě spravovat pakety](network-watcher-packet-capture-manage-portal.md) toostart relaci zachytávání paketů. Tento zachytáváním paketů se uloží úložiště objektů blob toobe přístup CapAnalysis.

### <a name="upload-a-packet-capture-toocapanalysis"></a>Nahrát tooCapAnalysis zachytávání paketů
Můžete nahrát přímo zachytáváním paketů, provedenou sledovací proces sítě pomocí karty "Import z adresy URL" hello a poskytuje odkaz toohello úložiště blob, kde je uložený zachytáváním paketů hello.

Pokud poskytuje odkaz tooCapAnalysis, ujistěte se, že tooappend adresu URL typu SAS token toohello úložiště objektů blob.  toodo, přejděte tooShared přístupový podpis z účtu úložiště hello, určit hello povolené oprávnění a stiskněte klávesu toocreate tlačítko hello generovat SAS token. Pak můžete připojit tuto SAS token toohello paketu zachycení úložiště objektů blob adresu URL.

Hello výsledné adresa URL bude vypadat přibližně takto: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>Analýza paketů zaznamená

CapAnalysis nabízí různé možnosti toovisualize zachytáváním paketů, každý poskytnete analysis z hlediska různých. S tyto souhrny visual můžete pochopit trendy provoz vaší sítě a rychle rozpoznat všechny neobvyklé aktivity. Několik z těchto funkcí jsou uvedeny v následující seznamu hello:

1. Tok tabulky

    Tato tabulka poskytuje hello seznamu toků v datech hello paketů, hello časové razítko přidružené hello toky a hello různých protokolů, které jsou přidružené k hello toku, stejně jako zdrojové a cílové IP.

    ![stránka capanalysis toku][5]

1. Přehled protokolu

    V tomto podokně můžete tooquickly najdete hello distribuce síťový provoz přes hello různé protokoly a zeměpisných oblastí.

    ![Přehled protokolu capanalysis][6]

1. Statistika

    V tomto podokně umožňuje vám statistiku provozu tooview sítě – počet bajtů odeslaných a přijatých ze zdrojové a cílové IP adresy, toky pro každou z hello zdrojové a cílové IP adresy, používá protokol pro různé toky a doba trvání hello toků.

    ![capanalysis statistiky][7]

1. geomap

    V tomto podokně poskytuje zobrazení mapy síťových přenosů, pomocí barev škálování toohello objem provozu z každé země. Můžete vybrat statistické údaje, zvýrazněné zemích tooview další toku třeba hello podíl data odeslané a přijaté z IP adresy v dané země.

    ![geomap][8]

1. Filtry

    CapAnalysis poskytuje sadu filtrů pro rychlou analýzu konkrétní paketů. Například můžete toofilter hello dat pomocí protokolu toogain konkrétní Statistika na tuto podmnožinu provozu.

    ![filtry][11]

    Navštivte [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) toolearn Další informace o všech CapAnalysis možnosti.

## <a name="conclusion"></a>Závěr

Funkce zachytávání paketů sledovací proces sítě vám umožní toocapture hello data potřebné tooperform sítě forenzních a až lépe porozumíte síťový provoz. V tomto scénáři jsme vám ukázal, jak paketu obrazovek z sledovací proces sítě lze snadno integrovat s otevřeným zdrojem vizualizace nástroje. Pomocí nástroje s otevřeným zdrojem, jako je zaznamená CapAnalysis toovisualize paketů, můžete provádění hloubkové kontroly paketů a rychlou identifikaci trendů v rámci síťového provozu.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o toku protokolů NSG, navštivte [protokolů NSG toku](network-watcher-nsg-flow-logging-overview.md)

Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
