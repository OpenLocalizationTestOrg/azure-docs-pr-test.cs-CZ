---
title: "aaaSecurity sledování v Azure Security Center | Microsoft Docs"
description: "Tento článek pomůže tooget práce s funkcemi sledováním v Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: yurid
ms.openlocfilehash: 43c2a8864d5fe27ba44b0d7bc979db970305ec17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Sledování stavu zabezpečení v Azure Security Center
Tento článek vám pomůže používat hello možnosti monitorování v Azure Security Center toomonitor dodržování zásad.

## <a name="what-is-security-health-monitoring"></a>Co je sledování stavu zabezpečení?
Myslíme si často monitorování jako pozorování a čekání událost toooccur, aby jsme mohly reagovat toohello situaci. Sledování zabezpečení odkazuje toohaving proaktivní strategie, kdy se auditují vaše prostředky tooidentify systémy, které nesplňují organizační standardy nebo osvědčené postupy.

## <a name="monitoring-security-health"></a>Sledování stavu zabezpečení
Po povolení [zásady zabezpečení](security-center-policies.md) pro prostředky předplatného, Security Center analyzuje hello zabezpečení vaše prostředky tooidentify potenciální ohrožení zabezpečení. Informace o konfiguraci vaší sítě jsou k dispozici okamžitě. To může trvat hodinu nebo Další informace o konfiguraci virtuálního počítače, jako je například zabezpečení aktualizovat stav a konfiguraci operačního systému, toobecome k dispozici. Můžete zobrazit hello stav zabezpečení vašich prostředků a všechny problémy v hello **prevence** části. Můžete také zobrazit seznam těchto problémů na hello **doporučení** dlaždici.

Další informace o tom, tooapply doporučení, přečtěte si [implementace doporučení zabezpečení v Azure Security Center](security-center-recommendations.md).

V části hello **prevence** části, můžete monitorovat hello stav zabezpečení vašich prostředků. V následujícím příkladu hello, se zobrazí v dlaždici všechny prostředky (výpočetní, sítě, úložiště a data a aplikace) je celkový počet hello problémy, které byly zjištěny.

![Dlaždice stavu zabezpečení prostředků](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute"></a>Monitorování služby Compute
Když kliknete na tlačítko **výpočetní** dlaždici hello **výpočetní** okno, které se otevře zobrazuje tři karty:

- **Přehled:** Doporučení pro monitorování a virtuální počítač
- **Virtuální počítače:** Seznam všech virtuálních počítačů a jejich aktuálního stavu zabezpečení
- **Cloudová služby:** Seznam všech webových a pracovních rolí monitorovaných pomocí služby Security Center.

![Chybějící aktualizace systému podle virtuálních počítačů](./media/security-center-monitoring/security-center-monitoring-fig1-new002-2017.png)

V každé kartě může mít více oddílů, a v každém oddílu můžete vybrat jednotlivé možnosti toosee další podrobnosti o hello doporučené kroky tooaddress tento konkrétní problém. 

#### <a name="monitoring-recommendations"></a>Doporučení pro monitorování
Tato část uvádí celkový počet virtuálních počítačů, které byly inicializovány pro shromažďování dat a jejich aktuální stav hello. Po všechny virtuální počítače mají shromažďování dat inicializováno, budou připravené tooreceive zásady zabezpečení Security Center. Když kliknete na tuto položku, hello **agenta virtuálního počítače je chybí nebo neodpovídá** otevře se okno. 

![Chybějící aktualizace systému podle virtuálních počítačů](./media/security-center-monitoring/security-center-monitoring-fig1-new003-2017.png)


#### <a name="virtual-machine-recommendations"></a>Doporučení pro virtuální počítače
Tato část obsahuje sadu [doporučení pro každý virtuální počítač](security-center-virtual-machine-recommendations.md) monitorovaný pomocí Azure Security Center. Hello první sloupec obsahuje doporučení hello. druhý sloupec Hello zobrazuje celkový počet virtuálních počítačů, které jsou ovlivněné tímto doporučením hello. Hello třetí sloupci se zobrazuje závažnost hello hello problému, jak ukazuje následující snímek obrazovky hello.

![Doporučení pro virtuální počítače](./media/security-center-monitoring/security-center-monitoring-fig1-new004-2017.png)

> [!NOTE]
> Pouze virtuální počítače, které mají alespoň jeden veřejný koncový bod se zobrazují v hello **sítě stavu** okno v hello **topologie sítě** seznamu.
>
>

Každé doporučení obsahuje sadu akcí, které můžete provést, když na ni kliknete. Například pokud kliknete na tlačítko **chybějící aktualizace systému**, hello **chybějící aktualizace systému** otevře se okno. Zobrazí se seznam hello virtuálních počítačů, které jsou chybějící opravy a závažnost chybějící aktualizace hello text hello, jak je znázorněno v následující hello snímek.

![Chybějící aktualizace systému pro virtuální počítače](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

Hello **chybějící aktualizace systému** ukazovat tabulku s hello následující informace:

* **VIRTUÁLNÍ počítač**: název hello hello virtuálního počítače, kterému chybí aktualizace.
* **AKTUALIZACE systému**: hello počet aktualizací systému, které chybí.
* **ČAS poslední kontroly**: čas hello Security Center naposledy hledala hello virtuálního počítače pro aktualizace.
* **Stav**: hello aktuální stav doporučení hello:
  * **Otevřete**: dosud nebylo řešeno hello doporučení.
  * **V průběhu**: hello doporučení je v současné době použité toothose prostředky a není třeba žádné akce.
  * **Vyřešit**: hello doporučení již byla dokončena. (Pokud hello problém byl vyřešen, položka hello je nedostupné)
* **ZÁVAŽNOST**: Popisuje hello závažnost tohoto konkrétního doporučení:
  * **Vysoká**: Ohrožení zabezpečení existuje u významného prostředku (aplikace, virtuální počítač nebo skupina zabezpečení sítě) a vyžaduje pozornost.
  * **Střední**: nekritické nebo další kroky jsou požadované toocomplete procesu nebo odstranění ohrožení.
  * **Nízká**: Ohrožení zabezpečení by se mělo řešit, ale nevyžaduje okamžitou pozornost. (Ve výchozím nastavení, nejsou přítomny nízkou doporučení, ale můžete filtrovat podle nízkou doporučení, pokud chcete, aby tooview jejich.)

Podrobnosti o doporučení hello tooview, klikněte na název hello hello virtuálního počítače. Jak je znázorněno v hello následující snímek obrazovky s hello seznam aktualizací, otevře se nové okno pro tento virtuální počítač.

![Chybějící aktualizace systému pro konkrétní virtuální počítač](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [!NOTE]
> Hello bezpečnostní doporučení uvedená tady jsou stejné jako v hello hello **doporučení** okno. V tématu hello [implementace doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) Další informace o tom najdete v článku tooresolve doporučení. Tento krok platí nejen pro virtuální počítače, ale i na všechny prostředky, které jsou k dispozici v hello **stav prostředku** dlaždici.
>
>

#### <a name="virtual-machines-section"></a>Část virtuálních počítačů
část virtuálních počítačů Hello získáte přehled o všech virtuálních počítačů a doporučení. Každý sloupec představuje jednu sadu doporučení, jak je uvedeno v hello následující snímek obrazovky:

![Přehled všech virtuálních počítačů a doporučení](./media/security-center-monitoring/security-center-monitoring-fig1-new005-2017.png)

Hello ikonu, která se zobrazí pod každé doporučení pomáhá tooquickly můžete identifikovat hello virtuálních počítačů, které vyžadují pozornost a hello typ doporučení.

V předchozím příkladu hello má jeden virtuální počítač kritické doporučení týkající se služby endpoint protection. tooget Další informace o hello virtuální počítač, klikněte na něj. Nové okno, které se otevře představuje tento virtuální počítač, jak je uvedeno v následující snímek obrazovky hello.

![Podrobné informace o zabezpečení virtuálního počítače](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Toto okno obsahuje podrobné informace o zabezpečení hello hello virtuálního počítače. V dolní části hello tohoto okna uvidíte, že hello doporučené akce a hello závažnost jednotlivých problémů.

#### <a name="cloud-services-section"></a>Část cloudových služeb
Pro cloudové služby doporučení se vytvoří při hello verze operačního systému je zastaralý jak ukazuje následující snímek obrazovky hello:

![Stav pro cloudové služby](./media/security-center-monitoring/security-center-monitoring-fig1-new006-2017.png)

V případě, kdy máte doporučení (která není hello případ předchozí příklad hello) je nutné kroky hello toofollow ve verzi operačního systému hello doporučení tooupdate hello. Když je k dispozici aktualizace, bude mít výstrahy (červené nebo oranžová - závisí na hello závažnost problému hello). Po kliknutí na toto upozornění řádky hello WebRole1 (běží Windows Server s vaší webové aplikaci automaticky nasadí tooIIS) nebo WorkerRole1 (běží Windows Server s vaší webové aplikaci automaticky nasadí tooIIS), otevře se nové okno s další podrobnosti o této doporučení, jak ukazuje následující snímek obrazovky hello:

![Podrobnosti cloudové služby](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

Klikněte na tlačítko toosee více doporučený vysvětlení tohoto doporučení **verze aktualizace operačního systému** pod hello **popis** sloupce. Hello **verze operačního systému aktualizaci (Preview)** otevře se okno s dalšími podrobnostmi.

![Doporučení pro Cloud Services](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Monitorování virtuálních sítí
Když kliknete na tlačítko **sítě** dlaždici hello **sítě** otevře se okno s dalšími podrobnostmi, jak ukazuje následující snímek obrazovky hello:

![Okno Sítě](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Doporučení pro sítě
Jako hello informací o stavu prostředků virtuálního počítače, toto okno obsahuje souhrnný seznam problémů v hello horní části okna hello a seznam monitorovaných sítí dolní hello.

Hello část rozpisu stavu sítí uvádí možné problémy zabezpečení a nabízí [doporučení](security-center-network-recommendations.md). Možné problémy mohou zahrnovat:

* Není nainstalována brána firewall příští generace (NGFW).
* Nejsou povolené skupiny zabezpečení sítě v podsítích.
* Nejsou povolené skupiny zabezpečení sítě ve virtuálních počítačích.
* Omezený externí přístup prostřednictvím veřejného externího koncového bodu.
* Internetové koncové body, které jsou v pořádku

Po kliknutí na tlačítko doporučení, otevře se nové okno s dalšími podrobnostmi o hello doporučení, jak ukazuje následující příklad hello.

![Podrobnosti o doporučení v okně sítě hello](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

V tomto příkladu hello **nakonfigurovat chybějící skupiny zabezpečení sítě pro podsítě** okno se seznamem podsítě a sítě virtuálních počítačů, které chybí ochranu skupiny zabezpečení. Pokud kliknete na tlačítko toowhich podsíť hello chcete skupinu zabezpečení sítě hello tooapply, otevře se další okno.

V hello **zvolit skupinu zabezpečení sítě** okno, můžete vybrat hello nejvíce příslušné sítě skupiny zabezpečení pro hello podsíť, nebo můžete vytvořit novou skupinu zabezpečení sítě.

#### <a name="internet-facing-endpoints-section"></a>Část internetových koncových bodů
V hello **internetové koncové body** části uvidíte hello virtuálních počítačů, které jsou aktuálně nakonfigurované s internetovým koncového bodu a jeho aktuální stav.

![Virtuální počítače nakonfigurované s internetovým koncovým bodem a stav](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Tato tabulka má název hello koncový bod, který představuje hello virtuálního počítače, hello internetové IP adresu a hello aktuální stav závažnosti hello skupinu zabezpečení sítě a hello NGFW. Hello tabulka je řazená podle závažnosti:

* Červená (nahoře): Vysoká priorita, mělo by se řešit okamžitě.
* Oranžová: Střední priorita, mělo by se řešit co nejdříve.
* Zelená (poslední): Stav v pořádku.

#### <a name="networking-topology-section"></a>Část topologie sítě
Hello **topologie sítě** část obsahuje hierarchické zobrazení prostředků hello, jak ukazuje následující snímek obrazovky hello:

![Hierarchické zobrazení prostředků v části topologie sítě](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Tato tabulka je řazená podle závažnosti (virtuální počítače a podsítě):

* Červená (nahoře): Vysoká priorita, mělo by se řešit okamžitě.
* Oranžová: Střední priorita, mělo by se řešit co nejdříve.
* Zelená (poslední): Stav v pořádku.

V tomto zobrazení topologie obsahuje první úroveň hello [virtuální sítě](../virtual-network/virtual-networks-overview.md), [brány virtuální sítě](/vpn-gateway/vpn-gateway-site-to-site-create.md), a [virtuální sítě (klasické)](/virtual-network/virtual-networks-create-vnet-classic-pportal.md). Hello druhé úrovni jsou podsítě a třetí úroveň hello má hello virtuální počítače, které patří toothose podsítě. Hello pravém sloupci je aktuální stav hello hello skupina zabezpečení sítě pro tyto prostředky, jak je znázorněno v hello následující ukázka:

![Stav skupiny zabezpečení sítě hello v část topologie sítě](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Hello dolní část toto okna obsahuje hello doporučení pro tento virtuální počítač, který je podobný toowhat je popsáno výše. Můžete kliknutím na Další doporučení toolearn nebo použít řízení hello potřeby zabezpečení nebo konfigurace.

### <a name="monitor-storage--data"></a>Monitorování úložiště a dat

Když kliknete na tlačítko **& úložiště dat** v hello **prevence** části hello **datové prostředky** otevře se okno s doporučeními pro SQL a úložiště. Je také [doporučení](security-center-sql-service-recommendations.md) pro hello obecný stav databáze hello. Další informace o šifrování úložiště najdete v tématu [Povolení šifrování účtu úložiště Azure v Azure Security Center](security-center-enable-encryption-for-storage-account.md).

![Datové prostředky](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

V části **SQL doporučení**, můžete kliknout na každé doporučení a get více podrobností o další akce tooresolve problém. Hello následující příklad ukazuje hello rozšíření hello **databáze auditování a detekce hrozeb v databázích SQL** doporučení.

![Podrobnosti o doporučení SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

Hello **povolení auditování a detekce hrozeb v databázích SQL** okno obsahuje hello následující informace:

* Seznam databází SQL
* Hello serveru, na kterém jsou umístěny
* Informace o tom, zda bylo toto nastavení zděděno ze serveru hello nebo pokud je v této databázi jedinečné
* aktuální stav Hello
* Hello závažnost problému hello

Po kliknutí na tlačítko hello databáze tooaddress toto doporučení, hello **auditování a detekce hrozeb** jak je znázorněno v hello následující obrazovka, otevře se okno.

![Okno Auditování a detekce hrozeb](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

tooenable auditování, vyberte **ON** pod hello **auditování** možnost.

### <a name="monitor-applications"></a>Monitorování aplikací

Pokud má vaše úloha Azure aplikace umístěné v [virtuálních počítačů (vytvořeny prostřednictvím Správce Azure Resource Manager)](../azure-resource-manager/resource-manager-deployment-model.md) s odhalenými webovými porty (porty TCP 80 a 443), můžete sledovat v Centru zabezpečení těchto tooidentify potenciální potíže se zabezpečením a doporučené kroky k nápravě. Když kliknete na tlačítko hello **aplikace** dlaždici hello **aplikace** otevře se okno s řadou doporučení v hello **aplikace doporučení** části. Také ukazuje rozpis aplikací hello na hostitele nebo virtuální IP adresu, jak ukazuje následující snímek obrazovky hello.

![Stav zabezpečení aplikací](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Stejně jako jste s hello ostatních doporučení, můžete kliknout na doporučení toosee další podrobnosti o problému hello a jak tooremediate. Hello příkladu v hello následující obrázek je aplikace, která byla identifikována jako nezabezpečená webová aplikace. Když vyberete hello aplikace, která není považovaná za zabezpečené, otevře se další okno s hello následující možnost k dispozici:

![Podrobnosti o aplikaci, která není zabezpečená](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Toto okno bude mít seznam všech doporučení pro tuto aplikaci. Když kliknete na tlačítko hello **přidání brány firewall webových aplikací** doporučení, hello **přidání brány Firewall webových aplikací** otevře se okno Možnosti, jak můžete tooinstall brány firewall webových aplikací (firewall webových aplikací) z partnerského jako ukazuje následující snímek obrazovky hello.

![Dialogové okno Přidat firewall webových aplikací](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Viz také
V tomto článku jste se naučili jak toouse funkcemi sledováním v Azure Security Center. toolearn Další informace o službě Azure Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md): Zjistěte, jak tooconfigure nastavení zabezpečení v Azure Security Center.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md): Zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md): Zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md): Přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/): Přečtěte si příspěvky o zabezpečení a dodržování předpisů Azure.
