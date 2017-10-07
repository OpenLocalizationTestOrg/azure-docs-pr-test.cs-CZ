---
title: "aaaLearn o funkcí v edicích služby BizTalk Services | Microsoft Docs"
description: "Porovnání možností hello edic hello BizTalk Services: Free, Developer, Basic, Standard a Premium. MABS, WABS."
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c589629f-06b1-44bb-b8ca-1db71826ea59
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 81626fa743a7190e7c78a0fd90b3054a08982b02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-editions-chart"></a>BizTalk Services: Tabulka edic

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Služba Azure BizTalk Services je dostupná v několika edicích. Použijte tento článek toodetermine, která edice je vhodná pro vaši situaci a obchodní potřeby.

## <a name="compare-hello-editions"></a>Porovnejte edice hello
**Free (Preview)**

Dokáže vytvořit a spravovat hybridní připojení. Hybridní připojení se snadný způsob tooconnect webu Azure tooan místní systém, jako je SQL Server.

**Developer**

Zahrnuje hybridní připojení, zpracování zpráv EAI a EDI se snadnou použitelným portálem pro správu obchodních partnerů a podporu společných schémat EDI a rozšířeného zpracování EDI přes X12 a AS2. Můžete vytvářet společné scénáře EAI připojení služby v cloudu hello s žádné tooread protokoly HTTP/S, REST, FTP, WCF a SFTP a zápis zpráv.  Připojení k systémům LOB tooon místní využívat adaptéry SAP, Oracle eBusiness, Oracle DB, Siebel a SQL Server připravenou k použití. Používá prostředí zaměřené na vývojáře s nástroji sady Visual Studio, které umožňují snadný vývoj a nasazení. Omezené toodevelopment testovací účely a pouze s žádné o úrovni služeb smlouvy (SLA).

**Basic**

Obsahuje většinu hello vývojáře funkce hybridních připojení, mosty EAI, smlouvy EDI a sady BizTalk Adapter Pack připojení. Kromě toho nabízí vysokou dostupnost a možnost tooscale hello s smlouvy o úrovni služeb (SLA).

**Standard**

Obsahuje všechny hello základní funkce hybridních připojení, mosty EAI, smlouvy EDI a sady BizTalk Adapter Pack připojení. Kromě toho nabízí vysokou dostupnost a možnost tooscale hello s smlouvy o úrovni služeb (SLA).

**Premium**

Obsahuje všechny hello standardní funkce hybridních připojení, mosty EAI, smlouvy EDI a sady BizTalk Adapter Pack připojení. Také zahrnuje archivaci, vysokou dostupnost a možnost tooscale hello s smlouvy o úrovni služeb (SLA).

## <a name="editions-chart"></a>Tabulka edic
Hello následující tabulce jsou uvedeny rozdíly hello.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Free (Preview)</th>
        <th>Developer</th>
        <th>Basic</th>
        <th>Standard</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Počáteční cena</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Ceník služby Azure BizTalk Services</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full"> Cenová kalkulačka funkcí Azure</a></td>
</tr>
<tr>
<td><strong>Výchozí minimální konfigurace</strong></td>
<td>1 jednotka Free</td>
<td>1 jednotka Developer</td>
<td>1 jednotka Basic</td>
<td>1 jednotka Standard</td>
<td>1 jednotka Premium</td>
</tr>
<tr>
<td><strong>Škálování</strong></td>
<td>Bez škálování</td>
<td>Bez škálování</td>
<td>Ano, v přírůstcích po 1 jednotce Basic</td>
<td>Ano, v přírůstcích po 1 jednotce Standard</td>
<td>Ano, v přírůstcích po 1 jednotce Premium</td>
</tr>
<tr>
<td><strong>Maximální povolené horizontální navýšení kapacity</strong></td>
<td>Bez škálování</td>
<td>Bez škálování</td>
<td>Too8 jednotky</td>
<td>Too8 jednotky</td>
<td>Too8 jednotky</td>
</tr>
<tr>
<td><strong>Mosty EAI na jednotku</strong></td>
<td>Nezahrnuje</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI, AS2</strong>
<br/><br/>
Zahrnuje smlouvy TPM</td>
<td>Nezahrnuje</td>
<td>Zahrnuje. 10 smluv na jednotku</td>
<td>Zahrnuje. 50 smluv na jednotku</td>
<td>Zahrnuje. 250 smluv na jednotku</td>
<td>Zahrnuje. 1000 smluv na jednotku</td>
</tr>
<tr>
<td><strong>Hybridní připojení na jednotku</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Přenos dat hybridním připojením (v GB) na jednotku</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>Připojení služby BizTalk Adapter Service, tooon místním systémům LOB</strong></td>
<td>Nezahrnuje</td>
<td>1 připojení</td>
<td>2 připojení</td>
<td>5 připojení</td>
<td>25 připojení</td>
</tr>
<tr>
<td align="left"><strong>Podporované protokoly/systémy:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Service Bus (SB)</li>
<li>Azure Blob</li>
<li>Rozhraní REST API</li>
</ul>
</td>
<td>Nezahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
</tr>
<tr>
<td><strong>Vysoká dostupnost</strong>
<br/><br/>
Informace o smlouvách o úrovni služeb (SLA) najdete v článku <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Ceník služby Azure BizTalk Services</a>.
</td>
<td>Nezahrnuje</td>
<td>Nezahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
</tr>
<tr>
<td><strong>Zálohování a obnovení</strong></td>
<td>Nezahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
</tr>
<tr>
<td><strong>Sledování</strong></td>
<td>Nezahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
</tr>
<tr>
<td><strong>Archivace</strong><br/><br/>
Zahrnuje neodvolatelnost příjmu a stahování sledovaných zpráv</td>
<td>Nezahrnuje</td>
<td>Zahrnuje</td>
<td>Nezahrnuje</td>
<td>Nezahrnuje</td>
<td>Zahrnuje</td>
</tr>
<tr>
<td><strong>Použití vlastního kódu</strong></td>
<td>Nezahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
</tr>
<tr>
<td><strong>Použití transformací, včetně vlastních transformací XSLT</strong></td>
<td>Nezahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
<td>Zahrnuje</td>
</tr>
</table>

> [!NOTE]
> V zájmu odolnosti proti selhání hardwaru s sebou funkce vysoké dostupnosti nese přítomnost víc virtuálních počítačů v rámci jedné jednotky BizTalk.
> 
> 

## <a name="faqs"></a>Nejčastější dotazy
#### <a name="what-is-a-biztalk-unit"></a>Co je to jednotka BizTalk?
"Jednotka" je nejnižší úroveň nasazení služby Azure BizTalk Services hello. Každá edice se dodává s jednotkou, která má různou výpočetní kapacitu a paměť. Třeba jednotka Basic má vyšší výpočetní kapacitu než jednotka Developer, jednotka Standard má vyšší výpočetní kapacitu než jednotka Basic a tak dál. Horizontální snížení kapacity služeb BizTalk probíhá po jednotkách.

#### <a name="what-is-hello-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Jaký je rozdíl hello BizTalk Services a virtuálním Počítačem Azure BizTalk?
Služba BizTalk Services nabízí true Architektura platformy jako služba (PaaS) pro sestavování integračních řešení v cloudu hello. U modelu PaaS hello soustředit se na hello aplikační logiku a nechat veškerou hello infrastruktury správy tooMicrosoft, včetně:

* Bez nutnosti toomanage nebo opravy virtuálních počítačů.
* Microsoft zajišťuje dostupnost.
* Můžete řídit škálování na vyžádání pomocí jednoduše vyžádat větší nebo menší kapacitu prostřednictvím hello portálu Azure.

BizTalk Server na virtuálních počítačích Azure nabízí architekturu podle modelu Infrastruktura jako služba (IaaS). Vytváření virtuálních počítačů a nakonfigurujete je stejně jako v místním prostředí, což jednodušší toorun existujících aplikací v cloudu hello beze změn kódu. V modelu IaaS jsou stále zodpovědná za konfiguraci hello virtuální počítače, správu hello virtuálních počítačů (třeba instalaci softwaru a oprav operačního systému) a architektury hello aplikaci na vysokou dostupnost.

Pokud chcete sestavit nová integrační řešení, která sníží pracnost správy infrastruktury, použijte službu BizTalk Services. Pokud hledáte tooquickly migraci stávajících řešení BizTalk nebo vyhledávání toodevelop na vyžádání prostředí a testovací BizTalk serveru aplikací, použijte BizTalk Server na virtuální počítač Azure.

#### <a name="what-is-hello-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Co je hello rozdíl mezi služba BizTalk Adapter Service a hybridními připojeními?
Hello služba BizTalk Adapter Service používá služby Azure BizTalk. Hello služba BizTalk Adapter Service používá hello sady BizTalk Adapter Pack tooconnect tooan v místním obchodnímu systému (LOB) systému. Hybridní připojení poskytuje tooconnect snadný a pohodlný způsob aplikace Azure, jako hello funkce Web Apps v Azure App Service a Azure Mobile Services tooan místních prostředků.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-hello-limit-is-reached"></a>Co znamená „Přenos dat hybridním připojením (v GB) na jednotku“? Je to údaj za minutu, hodinu, den, týden, nebo měsíc? Co se stane, když je dosaženo limitu hello?
Hello hybridní připojení cena za jednotku závisí na edici služby BizTalk Services hello. Jednoduše řečeno, cena závisí na tom, kolik dat přenášíte. Třeba přenesení 10 GB dat denně stojí méně než přenos 100 GB denně. Použití hello [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/?scenario=full) pro konkrétní náklady toodetermine BizTalk Services. Hello limity se většinou vynucují denně. Pokud hello limit překročíte, další přenosy výši hello míra 1 USD za GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-hello-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Po vytvoření ve službě BizTalk Services smlouvu, proč hello počet mostů se nezvyšuje o dva, a ne o jeden?
Každá smlouva se skládá ze dvou různých mostů: z komunikačního mostu na straně odesílání a z komunikačního mostu na straně příjmu.

#### <a name="what-happens-when-i-hit-hello-quota-limit-on-hello-number-of-bridges-or-agreements"></a>Co se stane, když dosáhnu maximální kvóty hello hello počtu mostů nebo smluv?
Nebylo možné toodeploy žádné nové mosty nebo vytvářet nové smlouvy. Další toodeploy, musíte tooscale toomore jednotky služby BizTalk hello nebo upgradu tooa vyšší verzi.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-tooanother"></a>Jak migrovat z jedné vrstvy služby BizTalk Services tooanother?
edice Free Hello nemůže být migrován nebo "Rozšířená" tooanother vrstvy a nelze jej zálohovat a obnovit tooanother vrstvy. Pokud potřebujete jinou vrstvu, vytvořte novou službu BizTalk pomocí nové vrstvy hello. Všechny artefakty vytvořené pomocí edice Free hello, včetně hybridních připojení, toobe nutné znovu vytvořit v hello novou službu BizTalk. 

Pro zbývající edice hello použijte hello zálohování a obnovení pro migraci artefaktů z jedné vrstvy tooanother. Můžete třeba zálohovat artefakty na vrstvě Standard hello a potom je obnovit toohello úroveň Premium. [Služba BizTalk Services: Zálohování a obnovení](biztalk-backup-restore.md) popisuje hello podporované cesty migrace a uvádí, jaké artefakty se dají zálohovat. Upozorňujeme, že hybridní připojení se nezálohují. Po zálohování a obnovení tooa novou vrstvu, pak znovu hello hybridní připojení.  

#### <a name="is-hello-biztalk-adapter-service-included-in-hello-service-how-do-i-receive-hello-software"></a>Služba BizTalk Adapter Service hello součástí hello služby? Jak získám hello softwaru?
Ano, jsou součástí hello Azure BizTalk Services SDK hello služba BizTalk Adapter Service se hello sady BizTalk Adapter Pack [Stáhnout](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Další kroky
toocreate Azure BizTalk Services v hello portálu Azure přejděte příliš[BizTalk Services: zřízení pomocí portálu Azure hello](biztalk-provision-services.md). vytváření aplikací, přejděte příliš toostart[služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Další zdroje
* [BizTalk Services: Zřízení pomocí hello portálu Azure](biztalk-provision-services.md)<br/>
* [BizTalk Services: Tabulka stavů zřízení](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Karty Řídicí panel, Sledování a Škálování](biztalk-dashboard-monitor-scale-tabs.md)<br/>
* [BizTalk Services: Zálohování a obnovení](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Omezování](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Název a klíč vystavitele](biztalk-issuer-name-issuer-key.md)<br/>
* [Jak začít používat hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>

