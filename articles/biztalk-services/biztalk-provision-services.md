---
title: "aaaCreate služby Azure BizTalk Services v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak tooprovision nebo vytvoření služby Azure BizTalk Services v hello portál Azure; MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 6781cadada8ac9c84e1fe045d2b0f995811f75b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-biztalk-services-using-hello-azure-portal"></a>Vytvoření služby BizTalk Services pomocí hello portálu Azure

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]


> [!TIP]
> toosign v toohello portálu Azure, potřebujete účet Azure a předplatné Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podívejte se na stránku [bezplatné zkušební verze Azure](http://go.microsoft.com/fwlink/p/?LinkID=239738).


## <a name="CreateService"></a>Vytvoření služby BizTalk
V závislosti na hello edice zvolíte může být k dispozici veškeré nastavení služby BizTalk.

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V hello spodním navigačním podokně vyberte **nový**:  
   ![Vyberte tlačítko Nový hello][NEWButton]
3. Vyberte možnosti **APP SERVICES** > **BIZTALK SERVICE** (Služba BizTalk) > **CUSTOM CREATE** (Vytvořit vlastní):  
   ![Výběr služby BizTalk a možnosti Custom Create (Vytvořit vlastní)][NewBizTalkService]
4. Zadejte nastavení služby BizTalk hello:
   
    <table border="1">
    <tr>
    <td><strong>BizTalk service name (Název služby BizTalk)</strong></td>
    <td>Můžete zadat libovolný název, ale buďte konkrétní. Možné příklady:<br/><br/>
    <em>moje_firma</em>.biztalk.windows.net<br/>
    <em>moje_firma_moje_aplikace</em>.biztalk.windows.net<br/>
    <em>moje_aplikace</em>.biztalk.windows.net<br/><br/>". biztalk.windows.net" je automaticky přidané toohello název zadáte. Tím se vytvoří adresu URL, která je služeb vaší BizTalk, jako je třeba použít tooaccess <strong>https://<em>Moje_aplikace</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edice</strong></td>
    <td>Pokud jste ve fázi testování/vývoje hello, vyberte možnost <strong>vývojáře</strong>. Pokud jste v produkční fázi hello, použijte hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Tabulka edic</a> toodetermine Pokud <strong>Premium</strong>, <strong>standardní</strong>, nebo <strong>Basic</strong>je pro vaši obchodní situaci hello nejvhodnější.
    </td>
    </tr>
    <tr>
    <td><strong>Oblast</strong></td>
    <td>Vyberte zeměpisnou oblast toohost hello svoji službu BizTalk.</td>
    </tr>
    <tr>
    <td><strong>Domain URL (Adresa URL domény)</strong></td>
    <td><strong>Volitelné</strong>. Ve výchozím nastavení, je adresa URL domény hello <em>Název_vaší_služby_biztalk</em>. biztalk.windows.net. Můžete ale zadat i vlastní doménu. Pokud máte třeba doménu <em>contoso</em>, můžete zadat: <br/><br/>
    <em>moje_firma</em>.contoso.com<br/>
    <em>moje_firma_moje_aplikace</em>.contoso.com<br/>
    <em>moje_aplikace</em>.contoso.com<br/>
    <em>nazev_vasi_sluzby_BizTalk</em>.contoso.com<br/>
    </td>
    </tr>
    </table>
Vyberte šipku další hello.
5. Zadejte nastavení databáze a hello úložiště:  <table border="1">
    <tr>
    <td><strong>Monitoring/Archiving storage account (Účet úložiště pro monitorování/archivaci)</strong></td>
    <td>Vyberte stávající účet úložiště nebo vytvořte nový. <br/><br/>Pokud vytvoříte nový účet úložiště, zadejte hello <strong>název účtu úložiště</strong>.</td>
    </tr>
    <tr>
    <td><strong>Tracking database (Databáze sledování)</strong></td>
    <td>Pokud použijete existující službu Azure SQL Database, nesmí ji používat žádná jiná služba BizTalk. Musíte hello přihlašovací jméno a heslo zadané při vytvoření Server databáze SQL Azure.<br/><br/><strong>TIP</strong> databázi vytvořit hello sledování a účet úložiště pro monitorování/archivaci v hello stejné oblasti jako hello služby BizTalk.</td>
    </tr>
    </table>
Vyberte šipku další hello.
6. Zadejte nastavení databáze hello:  <table border="1">
    <tr>
    <td><strong>Název</strong></td>
    <td>K dispozici v případě <strong>vytvoření nové instance databáze SQL</strong> je vybraný v předchozí úvodní obrazovka.
    <br/><br/>
Zadejte toobe název databáze SQL používat svoji službu BizTalk.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td>K dispozici v případě <strong>vytvoření nové instance databáze SQL</strong> je vybraný v předchozí úvodní obrazovka.
    <br/><br/>
Vyberte existující server služby SQL Database nebo vytvořte nový.</td>
    </tr>
    <tr>
    <td><strong>Server login name (Jméno pro přihlášení na server)</strong></td>
    <td>Zadejte hello přihlašovací uživatelské jméno.</td>
    </tr>
    <tr>
    <td><strong>Server login password (Heslo pro přihlášení na server)</strong></td>
    <td>Zadejte heslo pro přihlášení hello.</td>
    </tr>
    <tr>
    <td><strong>Oblast</strong></td>
    <td>Tato možnost je dostupná, jenom pokud je vybraná možnost <strong>Create a new SQL Database instance</strong> (Vytvořit novou instanci služby SQL Database). Vyberte zeměpisnou oblast toohost hello vaší databázi SQL.</td>
    </tr>
    </table>

Vyberte hello zaškrtnutí toocomplete hello průvodce. Zobrazí se ikona průběhu Hello:  
![Po dokončení se zobrazí ikona průběhu][ProgressComplete]

Po dokončení hello služby BizTalk Azure je vytvořená a připravená pro vaše aplikace. Hello výchozí nastavení je dostatečné. Pokud chcete toochange hello výchozí nastavení, vyberte **BIZTALK SERVICES** v levém navigačním podokně text hello a potom vyberte svoji službu BizTalk. Další nastavení, které se zobrazují v hello [karty řídicí panel, sledování a škálování](biztalk-dashboard-monitor-scale-tabs.md) v horní části hello.

V závislosti na stavu hello hello služby BizTalk nejsou některé operace, které nelze dokončit. Seznam těchto operací najdete příliš[BizTalk Services: Tabulka stavů](biztalk-service-state-chart.md).

## <a name="post-provisioning-steps"></a>Kroky pro zřízení
* [Nainstalujte certifikát hello v místním počítači.](#InstallCert)
* [Přidání certifikátu pro produkční prostředí](#AddCert)
* [Získání oboru názvů řízení přístupu hello](#ACS)

#### <a name="InstallCert"></a>Nainstalujte certifikát hello v místním počítači.
V rámci zřizování služby BizTalk se vytvoří certifikát podepsaný svým držitelem a přidruží se k vašemu předplatnému služby BizTalk. Musíte tento certifikát stáhnout a nainstalovat na počítačích, ze kterých buď nasazujete aplikace služby BizTalk, nebo posílat zprávy tooa koncového bodu služby BizTalk.

1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Vyberte **BIZTALK SERVICES** v levém navigačním podokně text hello a potom vyberte své předplatné služby BizTalk.
3. Vyberte hello **řídicí panel** kartě.
4. Vyberte **Download SSL Certificate** (Stáhnout certifikát SSL):  
   ![Úprava certifikátu SSL][QuickGlance]
5. Dvakrát klikněte na certifikát hello a spusťte přes hello Průvodce tooinstall hello certifikát. Zajistěte, aby certifikát hello hello instalujete **důvěryhodné kořenové certifikační úřady** uložit.

#### <a name="AddCert"></a>Přidání certifikátu pro produkční prostředí
Hello podepsaný svým držitelem, který se automaticky vytvoří při vytváření služby BizTalk Services je určena pro použití ve vývojovém prostředí pouze. V produkčních scénářích ho nahraďte certifikátem pro produkční prostředí.

1. Na hello **řídicí panel** vyberte **certifikát SSL aktualizace**.
2. Procházet tooyour soukromý certifikát SSL (*CertificateName*.pfx), obsahuje název vaší služby BizTalk, zadejte heslo hello a pak klikněte na tlačítko zaškrtnutí hello.

#### <a name="ACS"></a>Získání oboru názvů řízení přístupu hello
1. Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Vyberte **BIZTALK SERVICES** v levém navigačním podokně text hello a potom vyberte svoji službu BizTalk.
3. Hello hlavním panelu vyberte **informace o připojení**:  
   ![Výběr možnosti Informace o připojení][ACSConnectInfo]
4. Zkopírujte hodnoty řízení přístupu hello.

Tento obor názvů řízení přístupu zadáváte při nasazení projektu služby BizTalk v sadě Visual Studio. obor názvů řízení přístupu Hello se automaticky vytvoří pro vaši službu BizTalk.

Hello hodnoty řízení přístupu můžete použít s libovolnou aplikací. Při vytváření služby Azure BizTalk Services tento obor názvů řízení přístupu řídí ověřování hello s nasazením služby BizTalk. Zvolte, pokud chcete toochange hello předplatné nebo spravovat obor názvů hello **služby ACTIVE DIRECTORY** v hello levém navigačním podokně a potom zvolte svůj obor názvů. Možnosti jsou uvedené na hlavním panelu Hello.

Kliknutím na tlačítko **spravovat** otevře hello portál pro správu řízení přístupu. V hello portál pro správu řízení přístupu, hello používá služba BizTalk **identity služby**:  
![Identita služby ACS v hello portál pro správu řízení přístupu][ACSServiceIdentities]

Hello identita služby Řízení přístupu je sada přihlašovacích údajů, které umožňuje aplikacím nebo klientům tooauthenticate přímo pomocí řízení přístupu a získání tokenu.

> [!IMPORTANT]
> Hello služba BizTalk používá **vlastníka** hello výchozí identitu služby a hello **heslo** hodnotu. Pokud místo hello hodnoty heslo použijete hodnotu symetrický klíč hello, hello následující může dojít k chybě.<br/><br/>*Účet služby pro správu řízení přístupu toohello se nejde připojit s hello zadané přihlašovací údaje*
> 
> 

V článku [Správa oboru názvů služby ACS](https://msdn.microsoft.com/library/azure/hh674478.aspx) najdete některé pokyny a doporučení.

## <a name="requirements-explained"></a>Vysvětlení požadavků
Tyto požadavky se nevztahují toohello edice Free.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Co potřebujete?</strong></td>
        <td><strong>Proč to potřebujete?</strong></td>
</tr>
<tr>
<td>Předplatné Azure</td>
<td>Hello předplatné Určuje, kdo může přihlásit toohello portálu Azure. Hello držitel účtu vytvoří předplatné hello na <a HREF="https://account.windowsazure.com/Subscriptions"> předplatných Azure</a>.
<br/><br/>
Hello účet Azure může mít několik odběrů a můžete spravovat každý, kdo je povoleno. Třeba váš držitel účtu Azure vytvoří předplatné s názvem <em>Predplatne_služby_biztalk</em> a poskytuje hello správcům služby BizTalk v rámci vaší společnosti (například ContosoBTSAdmins@live.com) přístup k toothis předplatné. V tomto scénáři správcům služby BizTalk hello přihlásit toohello portál Azure a máte úplný správce práva tooall hello hostované služby v hello předplatného, včetně služby Azure BizTalk Services. Hello správcům služby BizTalk nejsou držiteli účtu Azure hello a proto nemají přístup tooany fakturační informace.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Správa předplatných a účtů úložiště v hello portál Azure</a> poskytuje další informace.
</td>
</tr>
<tr>
<td>Azure SQL Database</td>
<td>Ukládá hello tabulek, zobrazení a uložených procedur používaných hello služby BizTalk, včetně dat sledování hello.
<br/><br/>
Když vytváříte službu BizTalk, můžete použít existující server SQL Azure nebo službu Azure SQL Database nebo můžete automaticky vytvořit nový server nebo databázi.
<br/><br/>
Hello škálování databáze SQL se automaticky nakonfiguruje. Obvykle hello stačí výchozí škálování pro službu BizTalk. Změna měřítka hello má vliv na ceny. Další informace najdete v tématu <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">o účtech a cenách služby Azure SQL Database</a>
.<br/><br/>
<strong>Poznámky</strong>
<br/>
<ul>
<li> Při vytváření nového serveru SQL Azure a služby SQL Database automaticky dojde k povolení služeb Azure. Hello služba BizTalk vyžaduje Azure Services povolit.</li>
<li>Pokud vytvoříte novou databázi SQL Azure na existující Server SQL Azure, hello pravidla brány firewall hello serveru se nezmění. V důsledku toho je možné, že jiné služby Azure nejsou povoleny databáze serveru toohello přístup.</li>
</ul>
</td>
</tr>
<tr>
<td>Obor názvů řízení přístupu</td>
<td>Ověřuje se u služby Azure BizTalk Services. Tento obor názvů řízení přístupu zadáváte při nasazení projektu služby BizTalk v sadě Visual Studio. Při vytváření služby BizTalk se automaticky vytvoří obor názvů řízení přístupu hello.</td>
</tr>

<tr>
<td>Účet služby Azure Storage</td>
<td>Poskytuje přístup k tootables, objekty BLOB a fronty používané vaší služby BizTalk toosave hello následující:

<ul>
<li>Protokolové soubory hello tohoto monitorování služby BizTalk. Hello výstup monitorování se také zobrazí v hello **monitorování** ve hello portálu Azure.</li>
<li>Při vytváření X12 nebo smlouvy AS2 mezi partnery, můžete povolit vlastnosti zprávy, které funkce toostore hello archivaci. Tato data se uloží do hello účet úložiště.</li>
</ul>
<br/>
Při vytváření služby BizTalk můžete použít stávající účet služby Storage nebo automaticky vytvořit nový.
<br/><br/>
Výchozí nastavení služby Storage Hello je dostatečné pro službu BizTalk.
<br/><br/>
Při vytváření účtu služby Storage se automaticky vytvoří primární a sekundární klíč. Tyto klíče řídí přístup tooyour účet úložiště. Hello služba BizTalk automaticky používá hello primární klíč.
<br/><br/>
Další informace najdete v článku o <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">službě Storage</a>.
</td>
</tr>

<tr>
<td>Privátní certifikát SSL</td>
<td>
Při vytváření služby Azure BizTalk se vytvoří také adresa URL HTTPS, která obsahuje název vaší služby BizTalk. Tato adresa URL je automaticky nakonfigurované toouse jenom pro vývojové certifikát podepsaný svým držitelem. V produkčním prostředí budete potřebovat privátní certifikát SSL.
<br/><br/>
<strong>Důležité informace o certifikátu SSL</strong>

<ul>
<li>Datum vypršení platnosti certifikátu Hello musí být menší než 5 let.</li>
<li>Všechny privátní certifikáty vyžadují heslo. Heslo si zapamatujte a jako osvědčený postup také doporučujeme ho sdělit vašim správcům.</li>
<li>Certifikáty podepsané svým držitelem se používají v testovacím/vývojovém prostředí. Pokud používáte certifikáty podepsané svým držitelem, importujte hello certifikát tooyour osobním úložišti certifikátů a hello úložiště certifikátů důvěryhodných kořenových certifikačních autorit.</li>
</ul>
<br/>Při odesílání hello produkční certifikát požadavek tooyour certifikační autority, zadejte následující vlastnosti certifikátu hello:
<br/>

<ul>
<li><strong>Rozšířené použití klíče</strong>: Služba Azure BizTalk Services vyžaduje minimálně ověření serveru.</li>
<li><strong>Běžný název</strong>: Zadejte hello plně kvalifikovaný název domény (FQDN) adresy URL služby Azure BizTalk. Projděte si část <a HREF="#CreateService">Vytvoření služby BizTalk</a> v tomto článku.</li>
</ul>
<br/>
Po vytvoření služby BizTalk hello lze přidat nový nebo jiný certifikát.
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting tooany location.--->



## <a name="hybrid-connections"></a>Hybridní připojení
Při vytváření služby BizTalk Azure hello **hybridní připojení** karta je k dispozici:

![Karta Hybridní připojení][HybridConnectionTab]

Hybridní připojení jsou použité tooconnect Azure web nebo mobilní služby Azure tooany místní prostředek, který používá statický port TCP, jako je například SQL Server, MySQL, webové rozhraní API HTTP, Mobile Services a většina vlastních webových služeb.  Hybridní připojení a hello služba BizTalk Adapter Service se liší. Hello služba BizTalk Adapter Service je použité tooconnect služby Azure BizTalk Services tooan místní obchodnímu systému (LOB) systém.

 V tématu [hybridní připojení](integration-hybrid-connection-overview.md) toolearn další, včetně vytváření a správě hybridních připojení.

## <a name="next-steps"></a>Další kroky
Je teď vytvořená služby BizTalk, seznamte se s hello různých [BizTalk Services: karty řídicí panel, sledování a škálování](biztalk-dashboard-monitor-scale-tabs.md). Služba BizTalk je připravená pro vaše aplikace. vytváření aplikací, přejděte příliš toostart[služby Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Viz také
* [BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: Tabulka stavů](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Zálohování a obnovení](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Omezování](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Název a klíč vystavitele](biztalk-issuer-name-issuer-key.md)<br/>
* [Jak začít používat hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Hybridní připojení](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
