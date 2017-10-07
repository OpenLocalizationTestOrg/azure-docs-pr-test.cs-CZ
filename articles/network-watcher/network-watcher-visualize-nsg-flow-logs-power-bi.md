---
title: "protokoly aaaVisualizing toku skupinu zabezpečení sítě Azure s Power BI | Microsoft Docs"
description: "Tato stránka popisuje, jak toovisualize NSG toku protokoly s Power BI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: d9adcf256df8fed68c39be1a026ca64cc6b5c6d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a>Skupina zabezpečení sítě visualizing toku protokoly s Power BI

Skupina zabezpečení sítě toku protokoly povolit tooview informace o příchozí a odchozí provoz IP na skupiny zabezpečení sítě. Tyto protokoly toku zobrazit odchozí a příchozí toky na základě na pravidlo, hello seskupování hello toku platí pro, 5-řazené kolekce členů informace o toku hello (zdrojové nebo cílové IP adresy, zdrojový nebo cílový Port, protokol), a pokud bylo povolené nebo zakázané hello přenosy.

Může být obtížné toogain přehledy toku protokolování dat tak, že ručně hello soubory protokolu. V tomto článku Poskytujeme řešení toovisualize vaší poslední toku protokoly a další informace o přenosu v síti.

## <a name="scenario"></a>Scénář

V následujícím scénáři hello se nám připojit Power BI desktop toohello úložiště účet, který jsme nakonfigurovali jako hello jímku pro naše data NSG toku protokolování. Po tooour účet úložiště se nám připojit, Power BI se stáhne a analyzuje hello protokoly tooprovide vizuální reprezentace hello provoz, který je zaznamenána podle skupin zabezpečení sítě.

Pomocí hello vizuály zadaný v hello šablonu, kterou můžete zkontrolovat:

* Horní Talkers
* Časových řad toku dat rozhodnutím směr a pravidla
* Toky podle adresy MAC rozhraní sítě
* Toky NSG a pravidla
* Toky podle cílový Port

poskytuje šablony Hello je upravitelné, takže můžete upravit ho tooadd nová data, vizuály, nebo upravit dotazy toosuit vašim potřebám.

## <a name="setup"></a>Nastavení

Než začnete, musíte mít síťové zabezpečení skupiny toku povoleným protokolováním na jeden nebo více skupin zabezpečení sítě ve vašem účtu. Pokyny k povolení zabezpečení sítě toku protokolů, najdete v následujícím článku toohello: [Úvod tooflow protokolování pro skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).

Je také nutné mít hello Power BI Desktop nainstalovaného klienta na počítači a dostatek volného místa na váš počítač toodownload a zatížení hello data protokolu, která existuje v účtu úložiště.

![Diagram aplikace Visio][1]

### <a name="steps"></a>Kroky

1. Stáhněte a otevřete hello následující šablony Power BI v Power BI Desktop aplikace hello [PowerBI sledovací proces sítě toku protokoly šablony](https://aka.ms/networkwatcherpowerbiflowlogstemplate)
1. Zadejte hello požadované parametry dotazu
    1. **StorageAccountName** – určuje toohello název účtu úložiště hello obsahující toku NSG hello protokoly, které chcete vytvořit tooload a vizualizaci.
    1. **NumberOfLogFiles** – určuje hello počet souborů protokolu, že chcete vytvořit toodownload a vizualizovat v Power BI. Například pokud je zadán 50, hello 50 nejnovějších souborů protokolu. Vypnuto máme 2 skupin Nsg povolené a nakonfigurované toosend NSG toku protokoly toothis účet a potom hello posledních 25 hodin protokolů lze zobrazit.

    ![hlavní Power BI][2]

1. Zadejte hello přístupový klíč účtu úložiště. Platný přístupových klíčů můžete najít tak, že přejdete tooyour účet úložiště v Azure portálu a vyberete hello **přístupové klíče** z nabídky nastavení hello. Klikněte na tlačítko **Connect** pak změny.

    ![Přístupové klávesy][3]

    ![přístupový klíč 2][4]

4.  Protokoly jsou stáhnout a analyzovat a mohou nyní využívat předem vytvořené vizuály hello.

## <a name="understanding-hello-visuals"></a>Principy vizuály hello

Zadaný v hello šablony jsou sadu vizuální prvky, které pomáhají smysl Dobrý den data protokolu toku NSG. Hello následující obrázky znázorňují ukázka co řídicí panel hello vypadá jako při naplněný daty. Níže budeme každý visual podrobněji prozkoumat 

![powerbi][5]
 
Hello horní Talkers visual ukazuje hello IP adresy, které spustili hello většina připojení přes hello zadané období. velikost Hello hello polí odpovídá toohello relativní počet připojení. 

![toptalkers][6]

Hello následující časové řady grafy zobrazit hello počet toky období hello. horní grafu Hello je oddělených směr toku hello a hello nižší je oddělených hello rozhodnutí provedené (povolit nebo zakázat). Tento vizuál můžete prozkoumat vaše provoz trendů v čase a sledujte jakékoli neobvyklé špičky nebo odmítnout v provozu nebo segmentace přenosů dat.

![flowsoverperiod][7]

Hello následující grafy zobrazují hello toky na síťové rozhraní s hello horní oddělených směr toku a hello nižší segmentované podle rozhodnutí provedeny. Pomocí těchto informací je možné získat přehled o kterém virtuální počítače oznamovat hello většina relativní tooothers, a pokud provoz tooa konkrétní virtuální počítač se povolený nebo zakázaný.

![flowspernic][8]

Následující graf wheel prstenec Hello ukazuje rozpis toky podle cílový Port. Pomocí těchto informací můžete zobrazit hello nejčastěji používaná cílové porty používané v rámci hello zadané období.

![prstenec][9]

Hello následující pruhový graf zobrazuje hello toku NSG a pravidla. Tyto informace a uvidíte hello skupiny Nsg zodpovědná za hello většina provozu a rozpis hello přenosů na skupinu NSG pravidlem.

![barchart][10]
 
Hello následující informační grafy zobrazované informace o skupinách Nsg hello k dispozici v protokolech hello, hello toků zachytit po dobu hello a datum hello hello nejdřívější protokolu zaznamenat. Tyto informace získáte představu o jaké skupiny Nsg zaprotokolování a hello rozsah toků.

![infochart1][11]

![infochart2][12]

Tato šablona zahrnuje hello následující průřezy tooallow můžete pouze data hello tooview vás zajímají nejvíc. Můžete filtrovat podle skupin prostředků, skupiny Nsg a pravidla. Můžete také filtrovat podle informace 5 řazené kolekce členů, rozhodnutí a hello době hello protokolu byla zapsána.

![průřezy][13]

## <a name="conclusion"></a>Závěr

Jsme vám ukázal v tomto scénáři, pomocí protokolů toku skupiny zabezpečení sítě poskytuje sledovací proces sítě a Power BI jsme jsou možné toovisualize a pochopit hello provoz. Pomocí šablony hello poskytuje Power BI stáhne hello protokoly přímo ze služby storage a zpracovává je místně. Doba trvání tooload hello šablony se liší v závislosti na hello počet souborů požadovaných a stahovat celková velikost souborů.

Zaregistrované volné toocustomize, tato šablona pro vaše potřeby. Existuje mnoho mnoho způsobů, můžete Power BI s sítě toku protokolů skupiny zabezpečení. 

## <a name="notes"></a>Poznámky

* Protokoly ve výchozím nastavení se ukládají do`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`

    * Pokud existuje další data v jiném adresáři jejich hello toopull dotazy a zpracování dat hello je třeba upravit.

* Hello poskytuje šablony se nedoporučuje pro použití s více než 1 GB protokolů.

* Pokud máte velké množství protokoly, doporučujeme vám, že byste prozkoumat řešení pomocí jiného úložiště dat jako Data Lake nebo SQL server.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s hello Elastick zásobníku navštivte stránky [vizualizovat tooand vzory pro provoz sítě z virtuálních počítačů pomocí nástroje s otevřeným zdrojem](network-watcher-using-open-source-tools.md)

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
