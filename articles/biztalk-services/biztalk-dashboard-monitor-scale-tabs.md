---
title: "aaaDashboard, monitorování, měřítko, konfigurovat a hybridní připojení ve službě BizTalk Services | Microsoft Docs"
description: "Další informace o ovládacích prvcích hello a sledovat výkon na hello klasického portálu karty služby BizTalk Services: řídicí panel, sledování, škálování, konfigurovat a hybridní připojení. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c9fafdad20489571ee3849bbacd2c2b10933154f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="review-hello-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Zkontrolujte hello karty řídicí panel, sledování, škálování, konfigurovat a hybridní připojení.

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Po vytvoření služby BizTalk a nasazení aplikace, můžete změnit některá nastavení služby BizTalk hello a monitorování výkonu aplikace hello. 

Když otevřete hello portál Azure classic, je automaticky umístí na hello **všechny položky** tooview kartě službě BizTalk, vyberte svoji službu BizTalk v hello **všechny položky** kartě nebo vyberte hello **BIZTALK SERVICES** kartě; a potom vyberte název vaší služby BizTalk.

Otevře se nové okno s hello následující karty. Toto téma popisuje těchto karet.

## <a name="quickstart-quickstartquickstart"></a>(Rychlý start![Rychlý start][Quickstart])
V závislosti na hello edici služby BizTalk nemusí být k dispozici všechny možnosti uvedené. 

<table border="1">
    <tr>
        <td><strong>Získat nástroje hello</strong></td>
        <td>Stáhněte si hello BizTalk Services SDK tooinstall hello šablony projektů Visual Studio ve svém vývojovém počítači místně. Tyto šablony vytvořit hello <strong>BizTalk Services</strong> (mostu) a hello <strong>artefaktů služby BizTalk</strong> projektů sady Visual Studio (transformace), které jsou nasazené tooyour služby BizTalk.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Jak začít používat hello Azure BizTalk Services SDK </a> a <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">hello instalace sady SDK Azure BizTalk Services</a> seznamy hello kroky tooget spuštěna.
        </td>
    </tr>
    <tr>
        <td><strong>Vytvořte partnery smlouvy</strong></td>
        <td>Otevře se okno hello portálu služby Azure BizTalk hostované v Azure, kde je přidat partnery a vytvořit X12 AS2, smlouvy EDIFACT EDI a.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> seznamy hello kroky tooget spuštěna.
        </td>
    </tr>

<tr>
        <td><strong>Další informace o BizTalk Services</strong></td>
        <td>Přejděte toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">učení center</a> toolearn Další informace o Azure BizTalk Services.</td>
</tr>
</table>


Hello hlavním panelu v dolní části hello můžete:

<table border="1">

<tr>
<td><strong>Spravovat</strong> nasazení vaší aplikace</td>
<td>Otevřete portál služby Azure BizTalk Services hello. Hello portál služeb BizTalk je hello vstupu tooEDI konfigurace, včetně přidání partnery a vytvoření X12 AS2, EDIFACT smlouvy a.
<br/><br/>
Toto je stejný jako hello <strong>vytvořte smlouvy partnera</strong> na hello <strong>rychlý Start</strong> kartě.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> poskytuje další informace o hello portál služby BizTalk.</td>
</tr>

<tr>
<td><strong>Informace o připojení</strong> z hello Namespace řízení přístupu</td>
<td>Když vyberete informace o připojení, potom hello Namespace řízení přístupu, výchozí vystavitele a výchozí klíč jsou zobrazeny. Můžete zkopírovat tyto hodnoty.
<br/><br/>
Můžete také otevřít hello portál pro řízení přístupu. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Vytvoření ovládacího prvku přístup Namespace</a> poskytuje další informace o hello portál pro řízení přístupu.</td>
</tr>

<tr>
<td><strong>Klíče synchronizovat</strong> v hello účtu úložiště</td>
<td>Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč. Tyto šifrovací klíče řízení přístupu tooyour účet úložiště. Vaše služba BizTalk automaticky používá hello primární klíč. <strong>Klíče synchronizovat</strong> povolit uživatelům tooswitch mezi hello primární klíč a sekundární klíč hello bez přerušení hello služby BizTalk.
<br/><br/>
Například chcete hello služby BizTalk toouse nový primární klíč pro hello účet úložiště. toodo toto:
<br/><br/>
<ol>
<li>Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>. Vyberte hello sekundární klíč. Když to uděláte, spustí hello služby BizTalk, pomocí hello sekundární klíč.</li>
<li>V hello portál Azure classic vyberte svůj účet úložiště a obnovit hello primární klíč. Pamatujte si, že se vaše služba BizTalk používá hello sekundární klíč.</li>
<li>Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>. Nyní vyberte hello primární klíč. To je hello nové můžete znovu vygenerovat primární klíč.</li>
<li>V hello portál Azure classic vyberte svůj účet úložiště a znovu vygenerovat sekundární klíč hello.</li>
</ol>
<br/>
Tento proces se nazývá "výměny klíčů". účelem Hello je tooenable uživatelé tooswitch mezi hello primární klíč a sekundární klíč hello bez přerušení hello služby BizTalk.</td>
</tr>

<tr>
<td><strong>Odstranit</strong> vaší aplikace</td>
<td>Když vyberete odstranit, vaše služba BizTalk a jsou odebrány všechny položky, které jsou nasazené tooit.</td>
</tr>
</table>


## <a name="dashboard"></a>Řídicí panel
V závislosti na hello edici služby BizTalk nemusí být k dispozici všechny možnosti uvedené. 

Když vyberete název vaší služby BizTalk, zobrazí se karta řídicího panelu hello. Na řídicím panelu můžete:

##### <a name="usage-overview-shows-hello-number-of-used-hybrid-connections"></a>Přehled využití: Zobrazuje hello počet použitých hybridní připojení
Také zobrazuje hello využití dat v GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metriky grafu: Zobrazuje pevnou seznam metrik výkonu.
Tyto metriky zadejte v reálném čase hodnoty týkající se stavu hello hello služby BizTalk. Můžete také hello **relativní** nebo **absolutní** hodnoty a hello čas rozsah **Interval** hello metrik, která se zobrazí v grafu hello. 

Popis tyto metriky výkonu, přejděte příliš[dostupné metriky](#Metrics) v tomto tématu.

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Rychlého přehledu: Seznamy vlastnosti vaší služby BizTalk
<table border="1">

<tr>
<td><strong>Aktualizujte přihlašovací údaje databáze sledování</strong></td>
<td>Změny hello uživatelské jméno a heslo použít toolog do hello sledování databáze.</td>
</tr>
<tr>
<td><strong>Aktualizovat certifikát SSL</strong></td>
<td>Můžete aktualizovat hello služby BizTalk toouse jiný certifikát SSL. Když automaticky vytvoří certifikát podepsaný svým držitelem SSL můžete <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">vytvořit hello služby BizTalk</a>.</td>
</tr>
<tr>
<td><strong>Stažení certifikátu</strong></td>
<td>Můžete si stáhnout certifikát SSL hello používá vaše služba BizTalk tooa místního počítače.</td>
</tr>
<tr>
<td><strong>Stav</strong></td>
<td>Zobrazí aktuální stav hello vaší služby BizTalk. V tématu <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Tabulka stavů služby</a>. </td>
</tr>
<tr>
<td><strong>Adresa URL služby</strong></td>
<td>Adresa URL Hello pro vaši službu BizTalk. Toto je stejný hello jako hello <strong>adresa URL domény</strong> zadali při vytváření služby BizTalk.</td>
</tr>
<tr>
<td><strong>Veřejná virtuální IP (VIP) Adresa</strong></td>
<td>přidělit IP adresu Hello tooyour služby BizTalk. To se používá pro všechny vstupní koncové body a je hello zdrojové adresy odchozího provozu. Tato IP adresa patří tooyour služby BizTalk, dokud se vytvoří. Pokud odstraníte hello služby BizTalk, je přiřazená IP adresa hello tooanother služby BizTalk.</td>
</tr>
<tr>
<td><strong>Namespace služby ACS</strong></td>
<td>Ověřuje se hello služby BizTalk.</td>
</tr>
<tr>
<td><strong>Edice</strong></td>
<td>Uvádí hello edice zadá, když je vytvořena hello služby BizTalk.</td>
</tr>
<tr>
<td><strong>Umístění</strong></td>
<td>Zobrazí hello zeměpisnou oblast, který je hostitelem služby BizTalk.</td>
</tr>
<tr>
<td><strong>Vytvořit</strong></td>
<td>Zobrazí hello datum a čas hello služba BizTalk byla vytvořena.</td>
</tr>
<tr>
<td><strong>Sledování databáze</strong></td>
<td>Název databáze SQL Azure Hello, který ukládá hello sledování tabulky, které používá vaše služba BizTalk. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Požadavky na Explained</a> poskytuje podrobné informace o hello sledování databáze.</td>
</tr>
<tr>
<td><strong>Monitorování/archivaci úložiště</strong></td>
<td>název účtu úložiště Azure Hello uchovávající hello monitorování výstup svoji službu BizTalk.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Požadavky na Explained</a> poskytuje podrobné informace o hello účet úložiště.</td>
</tr>
<tr>
<td><strong>Název odběru</strong></td>
<td>Uvádí hello odběr, který je hostitelem služby BizTalk. předplatné Hello řídí toohello přístup k portálu Azure classic.</td>
</tr>
<tr>
<td><strong>ID předplatného</strong></td>
<td>Když je vytvořen předplatné, se automaticky vygeneroval ID předplatného. Při použití rozhraní REST API, musíte tooenter hello ID předplatného.</td>
</tr>
</table>

[BizTalk Services: Zřízení pomocí Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) seznamy hello kroky toocreate služby BizTalk.

##### <a name="manage-connection-information-sync-keys-and-delete-in-hello-task-bar"></a>Spravovat informace o připojení, synchronizace klíčů a odstranit hello hlavním panelu:
<table border="1">

<tr>
<td><strong>Spravovat</strong> nasazení vaší aplikace</td>
<td>Otevře hello portálu služby Azure BizTalk. Hello portál služeb BizTalk je hello vstupu tooEDI konfigurace, včetně přidání partnery a vytvoření X12 AS2, EDIFACT smlouvy a.
<br/><br/>
Toto je stejný jako hello <strong>vytvořte smlouvy partnera</strong> na hello <strong>rychlý Start</strong> kartě.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv EDI na portálu služby BizTalk</a> poskytuje další informace o hello portál služby BizTalk.</td>
</tr>
<tr>
<td><strong>Informace o připojení</strong> z hello Namespace řízení přístupu</td>
<td>Zobrazí hello Namespace řízení přístupu, výchozí vystavitele a klíč výchozí hodnoty; které je možné zkopírovat.
<br/><br/>
Můžete také otevřít hello portál pro řízení přístupu. Tento portál pro řízení přístupu je hello stejné jako možnost služby Active Directory v levém navigačním podokně hello hello.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Správa vašeho Namespace ACS</a> poskytuje další informace o hello portál pro řízení přístupu.</td>
</tr>
<tr>
<td><strong>Klíče synchronizovat</strong> v hello účtu úložiště</td>
<td>Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč. Tyto šifrovací klíče řízení přístupu tooyour účet úložiště. Vaše služba BizTalk automaticky používá hello primární klíč. <strong>Klíče synchronizovat</strong> povolit uživatelům tooswitch mezi hello primární klíč a sekundární klíč hello bez přerušení hello služby BizTalk.
<br/><br/>
Například chcete hello služby BizTalk toouse nový primární klíč pro hello účet úložiště. toodo toto:
<br/><br/>
<ol>
<li>Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>. Vyberte hello sekundární klíč. Když to uděláte, spustí hello služby BizTalk, pomocí hello sekundární klíč.</li>
<li>V hello portál Azure classic vyberte svůj účet úložiště a obnovit hello primární klíč. Pamatujte si, že se vaše služba BizTalk používá hello sekundární klíč.</li>
<li>Vyberte svoji službu BizTalk a vyberte <strong>synchronizace klíčů</strong>. Nyní vyberte hello primární klíč. To je hello nové můžete znovu vygenerovat primární klíč.</li>
<li>V hello portál Azure classic vyberte svůj účet úložiště a znovu vygenerovat sekundární klíč hello.</li>
</ol>
<br/>
Tento proces se nazývá "výměny klíčů". účelem Hello je tooenable uživatelé tooswitch mezi hello primární klíč a sekundární klíč hello bez přerušení hello služby BizTalk.</td>
</tr>

<tr>
<td><strong>Odstranit</strong> vaší aplikace</td>
<td>Vaše služba BizTalk a všechny položky, které jsou nasazené tooit se odeberou.</td>
</tr>
</table>


## <a name="monitor"></a>Monitorování
Doporučení se netýká toohello edice Free.

Když vyberete název vaší služby BizTalk, karta sledovat hello je k dispozici a zobrazí hello následující:

##### <a name="metric-graph-displays-hello-selected-performance-metrics"></a>Metriky grafu: Zobrazí hello vybrali metriky výkonu
Tyto metriky zadejte v reálném čase hodnoty týkající se stavu hello hello služby BizTalk. Zvolíte, které metriky výkonu zobrazují. Nesmí být delší než šest metriky výkonu lze zobrazit najednou. 

Můžete také hello **relativní** nebo **absolutní** hodnoty a hello čas rozsah **Interval** hello metrik, která se zobrazí. 

##### <a name="tooremove-or-display-metrics-in-hello-graph"></a>Metrika tooremove nebo zobrazení v grafu hello:
1. Vyberte hello **monitorování** kartě.
2. Vyberte **přidat metriky** hello hlavním panelu:  
   ![Vyberte Přidat metriky][AddMetrics]
3. Zkontrolujte chcete toodisplay metriky výkonu hello.
4. Vyberte hello zaškrtnutí tooreturn toohello **monitorování** kartě.
5. Vyberte hello kruh další toohello metriky toodisplay tuto metriku hodnota v grafu hello.  
   
    Například hello **využití procesoru** metrika je zobrazeno šedě; není v grafu hello zobrazí jeho výstup:  
   ![Je zobrazeno šedě metriky využití procesoru][GrayedMetric]  
   
    Vyberte hello šedě kruh tooenable hello **využití procesoru** metriky toodisplay jeho výstup v grafu hello:  
   ![Využití procesoru metrika je povoleno.][EnabledMetric]
6. Vyberte tooremove metrika z hello zobrazení grafu a seznamu hello **odstranit metrika** hello hlavním panelu. tooadd hello metriky back toohello seznamu, vyberte **přidat metriky** hello hlavním panelu, zkontrolujte metrika hello a vyberte hello zaškrtnutí tooreturn toohello **monitorování** kartě. Vyberte hello šedě metrika hello tooenable kruh.

## <a name="Metrics"></a>Dostupné metriky
k dispozici jsou následující čítače výkonu nebo metriky Hello:

<table border="1">

<tr>
<td><strong>Latence RountdTrip</strong></td>
<td>Zobrazí hello Průměrná doba v milisekundách (ms) tooprocess, je přijetí zprávu od hello čas uvítací zprávu, dokud hello služby BizTalk mezi všechny mostů plně zpracovává uvítací zprávu. Započítávají se pouze zprávy úspěšně zpracoval.<br/><br/>
Pokud dojde k hello následující události, je vytvořen časové razítko:
<ul>
<li>Zpráva zadá hello brány</li>
<li>Zpráva je cílový směrované toohello</li>
<li>Cílový odpověď přijata</li>
<li>Cílový potvrzení odpovědi odeslané toohello brány</li>
</ul>
<br/>
Tato metrika uvádí hello výsledek hello následující výpočet:
<br/><br/>
[Cílové potvrzení odpovědi odeslané toohello brány] - [zpráva zadá hello brány]</td>
</tr>
<tr>
<td><strong>Selhání u zdroje</strong></td>
<td>Zobrazí celkový počet zpráv, které se nezdařily hello podle hello služby BizTalk při stahování zprávy ze zdroje hello koncové body.</td>
</tr>
<tr>
<td><strong>Využití procesoru</strong></td>
<td>Uvádí hello průměrný % času procesoru všech instancí rolí.</td>
</tr>
<tr>
<td><strong>Čekací doba zpracování</strong></td>
<td>Zobrazí hello Průměrná doba v milisekundách (ms) tooprocess zprávu zpracovat hello služby BizTalk mezi všechny mostů, s výjimkou hello čas věnovaný cíle. Započítávají se pouze zprávy úspěšně zpracoval.<br/><br/>
Pokud dojde k každý hello následující události, se vytvoří časové razítko:

<ul>
<li>Zpráva zadá hello brány</li>
<li>Zpráva je cílový směrované toohello</li>
<li>Cílový odpověď přijata</li>
<li>Cílový potvrzení odpovědi odeslané toohello brány</li>
</ul>
<br/>Tato metrika uvádí hello výsledek hello následující výpočet:<br/><br/>
[Cílové potvrzení odpovědi odeslané toohello brány] - [zpráva zadá hello brány] - [cílové odpověď přijata] + [zpráva je cílový směrované toohello]</td>
</tr>
<tr>
<td><strong>Selhání v procesu</strong></td>
<td>Zobrazí celkový počet zpráv, které se nezdařilo během zpracování v časovém intervalu hello služby BizTalk mezi všechny mostů hello hello.</td>
</tr>
<tr>
<td><strong>Zprávy odeslané</strong></td>
<td>Zobrazí hello celkový počet zprávy odeslané přes všechny mostů hello služby BizTalk v časovém intervalu. Tato metrika se zvýší, když se dosáhne cílové trasy hello zprávy odeslané z kanálu. Tato metrika neznamená, že je zpráva úspěšně zpracována.<br/><br/>
V případě požadavku a odpovědi hello metrika se zvýší, když cílový trasy hello odesílá zpět toohello kanál potvrzení přijetí.</td>
</tr>
<tr>
<td><strong>Přijatých zpráv</strong></td>
<td>Zobrazí hello celkový počet zpráv přijatých hello služby BizTalk mezi všechny mostů v časovém intervalu. Tato metrika se zvýší, když novou zprávu přijme hello kanálu.</td>
</tr>
<tr>
<td><strong>Zprávy v procesu</strong></td>
<td>Zobrazí hello celkový počet zpráv, které jsou aktuálně zpracovávaných hello služby BizTalk v časovém intervalu.</td>
</tr>
<tr>
<td><strong>Počet zpracovaných zpráv</strong></td>
<td>Zobrazí hello celkový počet zpráv úspěšně zpracoval hello služby BizTalk mezi všechny mostů v časovém intervalu. Tato metrika se zvýší, když zprávu úspěšně přijme hello kanálu a úspěšně směrované toohello cíl.</td>
</tr>
</table>


## <a name="scale"></a>Měřítko
Na kartě hello škálování můžete přidat nebo odečíst hello počet jednotek, které používá vaše služba BizTalk. Ve výchozím nastavení není nakonfigurované jednotky. Další jednotky lze přidat tooscale svoji službu BizTalk. Pokud zvýšíte hello škálování, jsou zvýšit propustnost. Hello objem prostředků také zvyšuje, včetně nasazené mostech, smlouvy, obchodní připojení a výpočetní výkon. Například můžete zvýšit hello škále od 1 jednotka too2 jednotky. V takovém případě můžete nasadit dvojité hello počet mostů, double hello smluv, double hello LOB připojení a dvojité hello výpočetní výkon.

Některých edicích BizTalk nenabízejí možnost škálování. V takovém případě je povolená jednu jednotku. toodetermine je možné rozšířit vaše edice, kolik jednotek odkazovat příliš[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md).

Zvýšením počtu hello jednotky může mít vliv na ceny. Pokud zvýšíte hello jednotky, pak výběrem **Uložit** zobrazí zpráva s informacemi, pokud je ovlivněno fakturace. Potom vyberte toocontinue. Pokud zvýšíte počet jednotek hello, hello stav služby BizTalk se změní z aktivní tooUpdating. V hello aktualizace stavu pokračuje svoji službu BizTalk toorun.

[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md) definuje "Jednotka".

## <a name="configure"></a>Konfigurace
Nevztahuje tooHybrid připojení.

Nastaví stav zálohování tooNone hello nebo automaticky. Pokud nastavíte tooNone, se automaticky vytvoří žádné zálohy. Pokud nastavíte tooAutomatic, nakonfigurujte umístění zálohy hello, hello četnost zálohování hello a jak dlouho tookeep hello záložní soubory. 

[Služba BizTalk Services: Zálohování a obnovení](biztalk-backup-restore.md) poskytuje podrobnosti hello. 

## <a name="HybridConnections"></a>Hybridní připojení
Hybridní připojení slouží k připojení aplikací Azure, jako je Web Apps nebo Mobile Apps v Azure App Service, tooan místnímu prostředku, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP a většina vlastních webových služeb. Hybridní připojení se spravují ve službě BizTalk Services v hello portál Azure classic.

toocreate hybridní připojení ve službě Azure App Service najdete v části [přístup k místním prostředkům v Azure App Service pomocí hybridních připojení](../app-service-web/web-sites-hybrid-connection-get-started.md).

toocreate nebo Správa hybridních připojení ve službě Azure BizTalk Services najdete v tématu [hybridní připojení](integration-hybrid-connection-overview.md).

## <a name="next"></a>Další
Teď, když jste obeznámeni s různých kartách hello, můžete se další informace o funkcích služby Azure BizTalk Services hello:

* [BizTalk Services: Omezování](biztalk-throttling-thresholds.md)  
* [BizTalk Services: Název a klíč vystavitele](biztalk-issuer-name-issuer-key.md)  
* [BizTalk Services: Zálohování a obnovení](biztalk-backup-restore.md)

## <a name="see-also"></a>Viz také
* [Hybridní připojení](integration-hybrid-connection-overview.md)  
* [BizTalk Services: Developer, Basic, Standard a Premium tabulka edic](biztalk-editions-feature-chart.md)  
* [BizTalk Services: Zřízení pomocí Azure portál classic](biztalk-provision-services.md)  
* [BizTalk Services: Tabulka stav služby BizTalk](biztalk-service-state-chart.md)  
* [Jak začít používat hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

