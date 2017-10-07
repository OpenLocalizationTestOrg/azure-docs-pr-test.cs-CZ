---
title: "aaaConfiguration nejčastější dotazy pro webové aplikace Azure | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se konfigurace a potíží se správou pro hello funkce Web Apps služby Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 5aa405582813291a4aaf144d2f821ecb20a5edcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-and-management-faqs-for-web-apps-in-azure"></a>Konfigurace a správy nejčastější dotazy pro webové aplikace v Azure

Tento článek obsahuje odpovědi toofrequently kladené dotazy (FAQ) o konfiguraci a správě problémů hello [funkce Web Apps služby Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="are-there-limitations-i-should-be-aware-of-if-i-want-toomove-app-service-resources"></a>Existují omezení, které I měli vědět, pokud chcete prostředky toomove služby App Service?

Pokud máte v plánu toomove služby App Service prostředky tooa novou skupinu prostředků nebo předplatného, existuje několik omezení toobe vědět. Další informace najdete v tématu [omezení služby App Service](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="how-do-i-use-a-custom-domain-name-for-my-web-app"></a>Použití vlastního názvu domény pro webová aplikace

Odpovědi toocommon dotazy týkající se používání vlastního názvu domény s vaší webové aplikace Azure, najdete v tématu naše sedm dvouminutové video [přidání vlastního názvu domény](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name). nabízí Hello video návod, jak tooadd vlastního názvu domény. Popisuje, jak toouse vlastní adresou URL místo hello *. azurewebsites.net URL s vaší webové aplikace služby App Service. Také můžete zobrazit podrobný návod k [jak toomap vlastního názvu domény](web-sites-custom-domain-name.md).


## <a name="how-do-i-purchase-a-new-custom-domain-for-my-web-app"></a>Jak zakoupit novou vlastní doménu pro webová aplikace?

jak toopurchase a nastavit vlastní doménu pro webové aplikace služby App Service najdete v části toolearn [zakoupení a konfigurace vlastního názvu domény ve službě App Service](custom-dns-web-site-buydomains-web-app.md).


## <a name="how-do-i-upload-and-configure-an-existing-ssl-certificate-for-my-web-app"></a>Jak nahrání a konfigurace existující certifikát SSL pro webová aplikace?

jak tooupload a nastavení stávající vlastní certifikát SSL, najdete v části toolearn [vazby stávající vlastní protokol SSL certifikát tooan webové aplikace Azure](app-service-web-tutorial-custom-ssl.md#upload).


## <a name="how-do-i-purchase-and-configure-a-new-ssl-certificate-in-azure-for-my-web-app"></a>Jak zakoupit a konfigurovat nový certifikát SSL pro moje webovou aplikaci v Azure?

jak toopurchase a nastavit certifikát SSL pro webovou aplikaci služby App Service, najdete v části toolearn [přidat tooyour certifikát SSL aplikace App Service](web-sites-purchase-ssl-web-site.md).


## <a name="how-do-i-move-application-insights-resources"></a>Přesunutí prostředků Application Insights

V současné době nepodporuje Azure Application Insights hello operaci přesunutí. Pokud vaše původní skupina prostředků obsahuje prostředek Application Insights, nemůžete přesunout prostředku. Pokud prostředek Application Insights hello zahrnete, když zkusíte toomove aplikace služby App Service, přesuňte hello celé operace selže. Ale Application Insights a hello plánu není nutné toobe v App Service hello stejné skupině prostředků jako hello aplikace pro toofunction aplikace hello správně.

Další informace najdete v tématu [omezení služby App Service](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="where-can-i-find-a-guidance-checklist-and-learn-more-about-resource-move-operations"></a>Kde můžu najít kontrolní seznam pokyny a další informace o prostředku přesunout operace?

[Omezení služby App Service](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations) ukazuje, jak toomove prostředky tooeither nové předplatné nebo tooa nový prostředek skupiny v hello stejné předplatné. Můžete získat informace o hello prostředků přesunutí kontrolní seznam, další služby, které podporují operace přesunutí hello a další informace o omezení služby App Service a další témata.

## <a name="how-do-i-set-hello-server-time-zone-for-my-web-app"></a>Jak nastavit hello serveru časové pásmo pro webová aplikace?

tooset hello serveru časové pásmo pro webové aplikace:

1. V hello portál Azure, v rámci vašeho předplatného služby App Service, přejděte toohello **nastavení aplikace** nabídky.
2. V části **nastavení aplikace**, přidejte toto nastavení:
    * Klíč = WEBSITE_TIME_ZONE
    * Hodnota = *chcete hello časové pásmo*
3. Vyberte **Uložit**.

## <a name="why-do-my-continuous-webjobs-sometimes-fail"></a>Proč moje nepřetržité webové úlohy někdy nezdaří?

Ve výchozím nastavení jsou uvolněna webové aplikace, pokud jsou na nastavenou dobu nečinnosti. To umožňuje ušetřit prostředky systému hello. V plánech Basic a Standard, můžete zapnout hello **Always On** nastavení tookeep hello webové aplikace načíst všechny hello čas. Pokud nepřetržité webové úlohy běží vaše webová aplikace, třeba zapnout **Always On**, nebo hello WebJobs nemusí fungovat spolehlivě. Další informace najdete v tématu [vytvořit nepřetržitě běží webové úlohy](web-sites-create-web-jobs.md#CreateContinuous).

## <a name="how-do-i-get-hello-outbound-ip-address-for-my-web-app"></a>Jak získat hello odchozí IP adresu pro webová aplikace?

seznam hello tooget odchozí IP adres pro webovou aplikaci:

1. V hello portál Azure, v okně vaší webové aplikace, přejděte toohello **vlastnosti** nabídky.
2. Vyhledejte **odchozí ip adresy**.

Zobrazí se seznam Hello odchozí IP adres.

Pokud váš web hostovaný ve službě App Service Environment pro PowerApps, toolearn jak tooget odchozí IP adresu, najdete v části [odchozí síťové adresy](app-service-app-service-environment-network-architecture-overview.md#outbound-network-addresses).

## <a name="how-do-i-get-a-reserved-or-dedicated-inbound-ip-address-for-my-web-app"></a>Jak získám vyhrazené vyhrazené příchozí IP adresu nebo webová aplikace?

tooset vyhrazené nebo jinak vyhrazená IP adresa pro příchozí volání tooyour aplikace Azure web, nainstalujte a nakonfigurujte certifikát protokolu SSL založenou na protokolu IP.

Všimněte si, že toouse vyhrazená nebo rezervovanou adresou IP pro příchozí volání, plán služby App Service musí být v plánu služby základní nebo vyšší.

## <a name="can-i-export-my-app-service-certificate-toouse-outside-azure-such-as-for-a-website-hosted-elsewhere"></a>Můžete exportovat, jako je pro web hostovaný jinde Moje toouse certifikát služby App Service mimo Azure? 

Certifikáty App Service jsou považovány za prostředky Azure. Nejsou určený toouse mimo služeb Azure. Nelze exportovat je toouse mimo Azure. Další informace najdete v tématu [nejčastější dotazy pro certifikáty App Service a vlastní domény](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).

## <a name="can-i-export-my-app-service-certificate-toouse-with-other-azure-cloud-services"></a>Můžete exportovat Moje toouse certifikát služby App Service s jinými službami Azure cloud?

portál Hello nabízí prvotřídní prostředí pro nasazení prostřednictvím aplikace služby Azure Key Vault tooApp certifikát služby App Service. Však budeme mít přijímá požadavky od zákazníků toouse tyto certifikáty mimo hello platformě App Service, například pomocí Azure Virtual Machines. toolearn jak toocreate místní kopii App Service PFX certifikátu, můžete použít certifikát hello s další prostředky Azure, najdete v části [vytvořit místní kopii PFX certifikátu služby App Service](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).

Další informace najdete v tématu [nejčastější dotazy pro certifikáty App Service a vlastní domény](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).


## <a name="why-do-i-see-hello-message-partially-succeeded-when-i-try-tooback-up-my-web-app"></a>Proč se při pokusu o tooback se webová aplikace zobrazuje uvítací zprávu "Částečně bylo úspěšné"?

Obvyklou příčinou selhání zálohování je, že některé soubory se používá aplikace hello. Soubory, které jsou používány jsou zamčené při provádění zálohování hello. To zabrání zálohovaných tyto soubory a může mít za následek stav "Částečně bylo úspěšné". Vám může potenciálně tomu zabránit tak, že soubory vyloučíte z procesu zálohování hello. Můžete zvolit tooback se jenom to, co je potřeba. Další informace najdete v tématu [zálohování právě hello důležité části serveru s webové aplikace Azure](http://www.zainrizvi.io/2015/06/05/creating-partial-backups-of-your-site-with-azure-web-apps/).

## <a name="how-do-i-remove-a-header-from-hello-http-response"></a>Odebrání hlavičku z hello odpovědi HTTP

záhlaví hello tooremove z hello odpověď HTTP, aktualizujte soubor web.config. Další informace najdete v tématu [odebrat záhlaví standardní server na vaše weby Azure](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

## <a name="is-app-service-compliant-with-pci-standard-30-and-31"></a>Je kompatibilní s PCI standardní 3.0 a 3.1 služby App Service?

V současné době hello funkce Web Apps služby Azure App Service je v souladu s PCI Data zabezpečení DSS (Standard) verze 3.0 úroveň 1. PCI DSS verze 3.1 je na našem plán. Plánování již probíhá jak bude pokračovat přijetí hello nejnovější standard.

PCI DSS verze 3.1 certifikační vyžaduje zakázání zabezpečení TLS (Transport Layer) 1.0. V současné době zákaz protokolu TLS 1.0 není možnost pro většinu plány služby App Service. Pokud používáte služby App Service Environment nebo jsou ochotni toomigrate vaše úlohy tooApp Service Environment, ale můžete získat větší kontrolu nad prostředí. To zahrnuje zákaz protokolu TLS 1.0 ve obrátíte na podporu Azure. V hello blízké budoucnosti plánujeme toomake přístupné toousers těchto nastavení.

Další informace najdete v tématu [Microsoft Azure App Service webové aplikace dodržování PCI standardní 3.0 a 3.1](https://support.microsoft.com/help/3124528).

## <a name="how-do-i-use-hello-staging-environment-and-deployment-slots"></a>Použití hello pracovní prostředí a nasazovací sloty

V plánech Standard a Premium služby App Service když nasazujete tooApp vaší webové aplikace služby, můžete nasadit tooa samostatné nasazovací slot místo toohello výchozí produkční slot. Nasazovací sloty jsou za chodu webových aplikací, které mají své vlastní názvy hostitelů. Webové aplikace obsah a konfiguraci elementy lze vzájemně zaměněny mezi dvěma sloty nasazení, včetně hello produkční slot.

Další informace o používání nasazovací sloty najdete v tématu [nastavení pracovní prostředí ve službě App Service](web-sites-staged-publishing.md).

## <a name="how-do-i-access-and-review-webjob-logs"></a>Jak přístup a prohlédněte si protokoly webovou úlohu?

protokoly webové úlohy tooreview:

1. Přihlaste se tooyour [Kudu webu](https://*yourwebsitename*.scm.azurewebsites.net).
2. Vyberte hello webové úlohy.
3. Vyberte hello **přepnutí výstup** tlačítko.
4. toodownload hello výstupního souboru, vyberte hello **Stáhnout** odkaz.
5. Pro jednotlivé běží, vyberte **jednotlivých vyvolání**.
6. Vyberte hello **přepnutí výstup** tlačítko.
7. Vyberte hello odkaz ke stažení.

## <a name="im-trying-toouse-hybrid-connections-with-sql-server-why-do-i-see-hello-message-systemoverflowexception-arithmetic-operation-resulted-in-an-overflow"></a>Pokouším toouse hybridní připojení se serverem SQL Server. Proč se zobrazuje zpráva hello "System.OverflowException: Výsledkem Přetečení aritmetické operace"?

Pokud používáte hybridní připojení tooaccess systému SQL Server, aktualizace rozhraní Microsoft .NET 10. května 2016, může dojít k toofail připojení. Může se tato zpráva:

```
Exception: System.Data.Entity.Core.EntityException: hello underlying provider failed on Open. —> System.OverflowException: Arithmetic operation resulted in an overflow. or (64 bit Web app) System.OverflowException: Array dimensions exceeded supported range, at System.Data.SqlClient.TdsParser.ConsumePreLoginHandshake
```

### <a name="resolution"></a>Řešení

Pracujeme tooupdate správce hybridního připojení toofix tento problém. Alternativní řešení, najdete v části [chyba hybridní připojení se serverem SQL Server: System.OverflowException: Výsledkem Přetečení aritmetické operace](https://blogs.msdn.microsoft.com/waws/2016/05/17/hybrid-connection-error-with-sql-server-system-overflowexception-arithmetic-operation-resulted-in-an-overflow/).

## <a name="how-do-i-add-or-edit-a-url-rewrite-rule"></a>Jak přidat nebo upravit pravidlo přepisování adresu URL?

tooadd nebo upravit pravidlo přepsání adresy URL:

1. Nastavení Správce Internetové informační služby (IIS) tak, aby se připojí tooyour webové aplikace App Service. jak zjistit, tooconnect služby Správce služby IIS tooApp toolearn [vzdálené správy Azure webů pomocí Správce služby IIS](https://azure.microsoft.com/blog/remote-administration-of-windows-azure-websites-using-iis-manager/).
2. Ve Správci služby IIS přidat nebo upravit pravidlo přepsání adresy URL. toolearn jak tooadd nebo upravit adresu URL, přepište pravidlo, najdete v části [modul pro přepisování pravidel přepisování vytvořit pro adresu URL hello](https://www.iis.net/learn/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module).

## <a name="how-do-i-control-inbound-traffic-tooapp-service"></a>Jak řídit příchozí provoz tooApp služby?

Na úrovni serveru hello máte dvě možnosti pro řízení tooApp příchozí komunikaci služby:

* Zapněte funkci Dynamická omezení IP adres. jak zjistit, tooturn na dynamická omezení IP adres, toolearn [omezení domény a IP adresy pro weby Azure](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).
* Zapněte modulu zabezpečení. toolearn jak tooturn na modulu zabezpečení, najdete v části [brány firewall webových aplikací ModSecurity na webech Azure](https://azure.microsoft.com/blog/modsecurity-for-azure-websites/).

Pokud používáte služby App Service Environment, můžete použít [brány firewall Barracuda](https://azure.microsoft.com/blog/configuring-barracuda-web-application-firewall-for-azure-app-service-environment/).

## <a name="how-do-i-block-ports-in-an-app-service-web-app"></a>Jak blokovat porty ve webové aplikaci služby App Service?

Ve hello sdílené služby App Service klienta prostředí, není možné tooblock určité porty kvůli hello povaha hello infrastruktury. Porty TCP 4016, 4018 a 4020 také může být otevřené pro vzdálené ladění v sadě Visual Studio.

V App Service Environment máte plnou kontrolu nad příchozí a odchozí přenosy. Můžete vytvořit skupiny zabezpečení sítě toorestrict nebo blokování určité porty. Další informace o App Service Environment, najdete v části [Představení služby App Service Environment](https://azure.microsoft.com/blog/introducing-app-service-environment/).

## <a name="how-do-i-capture-an-f12-trace"></a>Jak zaznamenat při trasování F12?

Máte dvě možnosti pro zaznamenání při F12 trasování:

* Trasování F12 HTTP
* Výstup konzoly F12

### <a name="f12-http-trace"></a>Trasování F12 HTTP

1. V Internet Exploreru přejděte tooyour webu. Je důležité toosign před hello další kroky. Hello trasování F12, jinak hodnota zaznamená citlivá data přihlášení.
2. Stisknutím klávesy F12.
3. Ověřte, že hello **sítě** vybraná karta, a potom vyberte hello zelená **přehrání** tlačítko.
4. Kroky, které reprodukujte problém hello hello.
5. Vyberte hello red **Zastavit** tlačítko.
6. Vyberte hello **Uložit** tlačítko (ikona disku) a uložte soubor HAR hello (v Internet Exploreru a Edge) *nebo* klikněte pravým tlačítkem na soubor HAR hello a potom vyberte **uložit jako HAR s obsahem** () v prohlížeči Chrome).

### <a name="f12-console-output"></a>Výstup konzoly F12

1. Vyberte hello **konzoly** kartě.
2. Pro každé kartě, která obsahuje větší než nula. položky, vyberte kartu hello (**chyba**, **upozornění**, nebo **informace**). Pokud není vybraná karta hello, ikona Karta hello je šedé nebo černé při přesunutí kurzoru hello od ho.
3. Klikněte pravým tlačítkem myši v oblasti zpráv hello hello podokně a pak vyberte **zkopírujte všechny**.
4. Hello vložte zkopírovaný text v souboru a uložte soubor hello.

tooview soubor HAR, můžete použít hello [HAR prohlížeč](http://www.softwareishard.com/har/viewer/).

## <a name="why-do-i-get-an-error-when-i-try-tooconnect-an-app-service-web-app-tooa-virtual-network-that-is-connected-tooexpressroute"></a>Proč se zobrazí chybu při tooconnect aplikační službu webové aplikace tooa virtuální síť, která je připojená tooExpressRoute?

Pokud se pokusíte tooconnect službě Azure web app tooa virtuální síť, která připojil tooAzure ExpressRoute, dojde k chybě. Dobrý den zobrazí se následující zpráva: "Brány není brána sítě VPN."

V současné době nemůže mít point-to-site VPN připojení tooa virtuální síť, která je připojená tooExpressRoute. Point-to-site VPN a ExpressRoute nemůžou existovat pro hello stejné virtuální síti. Další informace najdete v tématu [ExpressRoute a VPN typu site-to-site připojení limity a omezení](../expressroute/expressroute-howto-coexist-classic.md#limits-and-limitations).

## <a name="how-do-i-connect-an-app-service-web-app-tooa-virtual-network-that-has-a-static-routing-policy-based-gateway"></a>Jak se připojit aplikaci služby webové aplikace tooa virtuální síť, která má statické směrování brány (zásadové)?

V současné době připojení službě App Service webové aplikace tooa virtuální síť, která má statické směrování brány (zásadové) není podporováno. Pokud cílový virtuální síti již existuje, musí mít povoleny v rámci brány dynamického směrování, aby mohla být připojené tooan aplikace VPN point-to-site. Pokud vaše brána je nastavený toostatic směrování, nelze povolit síť VPN point-to-site. 

Další informace najdete v tématu [integraci aplikace s virtuální sítě Azure](web-sites-integrate-with-vnet.md#getting-started).

## <a name="in-my-app-service-environment-why-can-i-create-only-one-app-service-plan-even-though-i-have-two-workers-available"></a>V mé App Service Environment, proč lze vytvořit pouze jeden plán služby App Service, přestože má dva pracovníci, které jsou k dispozici?

tooprovide odolnost proti chybám, App Service Environment vyžaduje, že každý fond pracovních procesů musí mít alespoň jeden další výpočetních prostředků. Hello další výpočetní prostředek nelze přiřadit zatížení.

Další informace najdete v tématu [jak toocreate služby App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).

## <a name="why-do-i-see-timeouts-when-i-try-toocreate-an-app-service-environment"></a>Proč se zobrazuje časové limity, když se pokouším toocreate služby App Service Environment?

V některých případech vytváření služby App Service Environment se nezdaří. V takovém případě zobrazí následující chyba v hello protokoly aktivity hello:
```
ResourceID: /subscriptions/{SubscriptionID}/resourceGroups/Default-Networking/providers/Microsoft.Web/hostingEnvironments/{ASEname}
Error:{"error":{"code":"ResourceDeploymentFailure","message":"hello resource provision operation did not complete within hello allowed timeout period.”}}
```

tooresolve, ujistěte se, aby byly splněné žádné z hello následující podmínky:
* podsíť Hello je příliš malá.
* Hello podsíť není prázdný.
* ExpressRoute zabraňuje hello požadavků na připojení síťové App Service Environment.
* Chybný skupinu zabezpečení sítě brání hello požadavků na připojení síťové App Service Environment.
* Vynucené tunelování je zapnutý.

Další informace najdete v tématu [častých problémů při nasazování (vytvoření) nové služby Azure App Service Environment](https://blogs.msdn.microsoft.com/waws/2016/05/13/most-frequent-issues-when-deploying-creating-a-new-azure-app-service-environment-ase/).

## <a name="why-cant-i-delete-my-app-service-plan"></a>Proč nelze odstranit mé plán služby App Service?

Plán služby App Service nelze odstranit, pokud jsou všechny aplikace služby App Service přidružené hello plán služby App Service. Chcete odstranit plán služby App Service, odeberte všechny přidružené aplikace služby App Service z hello plán služby App Service.

## <a name="how-do-i-schedule-a-webjob"></a>Jak naplánovat webovou úlohu?

Plánované webové úlohy můžete vytvořit pomocí procesu Cron výrazů:

1. Vytvořte soubor settings.job.
2. V tomto souboru JSON zahrnout pomocí výraz Cron vlastnost plánu: 
    ```
    { "schedule": "{second}
    {minute} {hour} {day}
    {month} {day of hello week}" }
    ```

Další informace o plánované webové úlohy najdete v tématu [plánované webové úlohy vytvoření pomocí výrazu Cron](web-sites-create-web-jobs.md#CreateScheduledCRON).

## <a name="how-do-i-perform-penetration-testing-for-my-app-service-app"></a>Jak provádět průnikům testování pro Moje aplikace App Service?

testování průnikům tooperform [odeslat žádost o](https://security-forms.azure.com/penetration-testing/terms).

## <a name="how-do-i-configure-a-custom-domain-name-for-an-app-service-web-app-that-uses-traffic-manager"></a>Jak lze nakonfigurovat vlastní název domény pro webové aplikace služby App Service, která používá Traffic Manager?

jak zjistit, toouse vlastního názvu domény s aplikaci služby App Service, která používá Azure Traffic Manager pro vyrovnávání zatížení, toolearn [nakonfigurovat vlastní název domény pro webové aplikace Azure s nástrojem Traffic Manager](web-sites-traffic-manager-custom-domain-name.md).

## <a name="my-app-service-certificate-is-flagged-for-fraud-how-do-i-resolve-this"></a>Moje certifikát služby App Service je označeny pro podvodů. Jak to lze vyřešit?

Během ověření domény hello nákupu certifikát služby App Service můžete se setkat hello následující zprávou:

"Svůj certifikát byl označen příznakem pro možném podvodu. požadavek Hello aktuálně probíhá kontrola. Pokud certifikát hello se nestane použitelné do 24 hodin, kontaktujte prosím podporu Azure."

Jako hello zpráva znamená, tento proces ověření podvod může trvat až toocomplete too24 hodin. Během této doby bude pokračovat toosee uvítací zprávu.

Pokud váš certifikát služby App Service pokračuje tooshow tuto zprávu po 24 hodinách, spusťte následující skript prostředí PowerShell hello. Hello skriptu kontakty hello [poskytovatel certifikátu](https://www.godaddy.com/) přímo tooresolve hello problém.

```
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <subId>
$actionProperties = @{
    "Name"= "<Customer Email Address>"
    };
Invoke-AzureRmResourceAction -ResourceGroupName "<App Service Certificate Resource Group Name>" -ResourceType Microsoft.CertificateRegistration/certificateOrders -ResourceName "<App Service Certificate Resource Name>" -Action resendRequestEmails -Parameters $actionProperties -ApiVersion 2015-08-01 -Force   
```

## <a name="how-do-authentication-and-authorization-work-in-app-service"></a>Jak ověřování a autorizace fungují ve službě App Service?

Podrobnou dokumentaci pro ověřování a autorizace ve službě App Service najdete v tématu [zabezpečení služby App Service](../app-service/app-service-security-readme.md). dokumentace Hello také obsahuje informace o nastavení služby App Service toouse různé identifikovat zprostředkovatele přihlášení:
* [Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Facebook](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md)
* [Google](../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md)
* [Účet Microsoft](../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Twitter](../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="how-do-i-redirect-hello-default-azurewebsitesnet-domain-toomy-azure-web-apps-custom-domain"></a>Jak přesměrovat hello výchozí *. azurewebsites.net domény toomy webové aplikace Azure na vlastní domény?

Když vytvoříte nový web pomocí webových aplikací v Azure, výchozí *sitename*. azurewebsites.net domény je přiřazen tooyour lokality. Pokud přidáte lokalitu tooyour název vlastního hostitele a nechcete, aby uživatelé toobe možné tooaccess výchozí *. azurewebsites.net domény, můžete přesměrovat hello výchozí adresa URL. toolearn způsobu tooredirect všechny přenosy z vašeho webu výchozí domény tooyour vlastní doménu, najdete v [hello výchozí domény tooyour vlastní doména pro přesměrování ve službě Azure web apps](http://www.zainrizvi.io/2016/04/07/block-default-azure-websites-domain/).

## <a name="how-do-i-determine-which-version-of-net-version-is-installed-in-app-service"></a>Jak určit, jaká verze rozhraní .NET verze je nainstalována ve službě App Service?

Hello nejrychlejší způsob, jak toofind hello verze rozhraní Microsoft .NET, který je nainstalován ve službě App Service je pomocí konzoly Kudu hello. Hello Kudu konzoly můžete přistupovat z hello portálu nebo pomocí adresy URL hello aplikace služby App Service. Podrobné pokyny najdete v tématu [určení hello nainstalované rozhraní .NET verze ve službě App Service](https://blogs.msdn.microsoft.com/waws/2016/11/02/how-to-determine-the-installed-net-version-in-azure-app-services/).

## <a name="why-isnt-autoscale-working-as-expected"></a>Proč není škálování fungují dle očekávání?

Pokud nebyl v škálovat škálování Azure nebo škálovat na více systémů instanci webové aplikace hello podle očekávání, může být příčinou do scénář, ve kterém jsme záměrně zvolte není tooscale tooavoid nekonečné smyčce z důvodu příliš "netřepotá." Obvykle se to stane, když není k dispozici odpovídající okraje mezi prahovými hodnotami hello Škálováním na více systémů a škálování. jak tooavoid "netřepotá" a tooread o ostatní osvědčené postupy škálování najdete v části toolearn [škálování osvědčené postupy](../monitoring-and-diagnostics/insights-autoscale-best-practices.md#autoscale-best-practices).

## <a name="why-does-autoscale-sometimes-scale-only-partially"></a>Proč škálování někdy škálovat jenom částečně?

Škálování se aktivuje při překročení předkonfigurované hranice. V některých případech můžete si všimnout kapacity hello je vyplněno pouze částečně porovnání toowhat vašim očekáváním. Tato situace může nastat, hello počet instancí, které chcete nejsou k dispozici. V tomto scénáři doplní škálování částečně pomocí hello dostupný počet instancí. Škálování pak spustí hello obnovte rovnováhu logiku tooget větší kapacitu. Přiděluje hello zbývající instancí. Všimněte si, že to může trvat několik minut.

Pokud nevidíte hello očekávaný počet instancí za několik minut, může to být způsobeno částečné doplnění hello byl dostatek toobring hello metriky v rámci hranice hello. Nebo může mít škálování zmenšování, protože bylo dosaženo hello nižší metriky hranic.

Pokud žádná z těchto podmínek použití a hello problém přetrvává, odešlete žádost o podporu.

## <a name="how-do-i-turn-on-http-compression-for-my-content"></a>Jak zapnout komprese protokolu HTTP pro moje obsahu?

tooturn na komprese pro statické a dynamické typy obsahu, přidejte následující soubor web.config na úrovni aplikace toohello kód hello:

```
<system.webServer>
<urlCompression doStaticCompression="true" doDynamicCompression="true" />
< /system.webServer>
```

Také můžete zadat konkrétní dynamické hello a statické MIME typy, které chcete toocompress. Další informace najdete v tématu naše dotaz ve fóru odpovědi tooa v [httpCompression nastavení na jednoduchý Azure web](https://social.msdn.microsoft.com/Forums/azure/890b6d25-f7dd-4272-8970-da7798bcf25d/httpcompression-settings-on-a-simple-azure-website?forum=windowsazurewebsitespreview).

## <a name="how-do-i-migrate-from-an-on-premises-environment-tooapp-service"></a>Jak migrovat z prostředí tooApp místní služby?

toomigrate lokality ze systému Windows a Linux servery tooApp webové služby, můžete použít Azure App Service migrace pomocníka. Nástroj pro migraci Hello vytvoří webové aplikace a databáze v Azure podle potřeby a následně publikuje hello obsah. Další informace najdete v tématu [Azure App Service migrace pomocníka](https://www.movemetothecloud.net/).
