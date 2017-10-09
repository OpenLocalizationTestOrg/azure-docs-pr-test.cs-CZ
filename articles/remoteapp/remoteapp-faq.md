---
title: "aaaAzure vzdálené aplikace RemoteApp – nejčastější dotazy | Microsoft Docs"
description: "Přečtěte si odpovědi toohello nejvíce časté otázky k Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: swadhwa
editor: 
ms.assetid: bad66603-91f9-437f-8a70-236405d2a27f
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: da981fea9e38b4e74694aeaba5f97c8ed897ccd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp-faq"></a>Časté otázky k Azure RemoteAppu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Jste nám položili následující otázky o Azure Remoteappu hello. Máte další? Navštivte hello [fóra Remoteappu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) a dejte nám vědět, co potřebujete tooknow, nebo níže Zanechte komentář.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Nemůžete najít, co hledáte? Máte otázku, na kterou jste nenašli odpověď?
Pokud nemůžete najít hello informace budete potřebovat, nebo máte další dotaz, který jsme nejsou zde pokrývajících, přejděte prosím toohello [fóru vzdálené aplikace Azure RemoteApp](http://aka.ms/araforum) a položte svou otázku existuje. Do tohoto fóra pořád přidáváme další odpovědi.

## <a name="what-is-azure-remoteapp"></a>Co je Azure RemoteApp?
* **Co je Azure RemoteApp?** Vzdálené aplikace RemoteApp je že služba Azure pomáhá poskytovat zabezpečený vzdálený přístup tooapplications z mnoha různých uživatelských zařízení. Další informace o [Azure RemoteAppu](remoteapp-whatis.md).
* **Jaké jsou možnosti nasazení hello?** Existují dva typy kolekcí RemoteAppu: cloudová a hybridní. Který typ budete potřebovat, závisí na několika faktorech, například zda potřebujete připojení k doméně. O všech těchto rozhodujících faktorech mluvíme [zde](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Rychlé tipy k používání Azure RemoteAppu
* **Jak dlouho bude trvat, než mě odpojíte? Jak dlouho můžu být neaktivní, než mě hello spouštěcí?** 4 hodiny. Pokud jste vy nebo jeden z vašich uživatelů budete neaktivní 4 hodiny, budete z Azure RemoteAppu automaticky odhlášeni. Podívejte se na hello další výchozí nastavení v [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).
* **Můžu zkusit tuto službu zdarma?** Ano. 30 dní je k dispozici bezplatná zkušební verze. Po skončení zkušební hello, můžete přejít tooa placené účet (který můžete použít v produkčním prostředí) nebo přestat používat služby hello. Začít používat bezplatnou zkušební verzi přechodem příliš[portal.azure.com](http://portal.azure.com) -vytvořit novou instanci Remoteappu. Hello bezplatnou zkušební verzi můžete vytvořit 2 instance Remoteappu s 10 uživateli na každou instanci. Mějte na paměti, že tato zkušební verze funguje jen 30 dní.
  
  ## <a name="azure-remoteapp-subscription-details"></a>Podrobnosti o předplatném Azure RemoteAppu
* **Jaká jsou omezení služby hello?** Můžete další informace o hello výchozí nastavení a omezení služby Azure RemoteApp v služby [předplatné Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md). Dejte nám vědět, pokud máte další dotazy.
* **Počet uživatelů, kteří se mám toohave?** Minimálně 20 uživatelů. Ještě to zopakuji této toobe naprosto jasné - hello MINIMUM je 20. Poplatky vám budou účtovány za 20 uživatelů. 
* **Kolik stojí RemoteApp?** Podívejte se na [Podrobnosti o cenách Azure RemoteAppu ](https://azure.microsoft.com/pricing/details/remoteapp/).
* **Stojí jeden typ kolekce více než druhý?** Ano, může to tak být. Záleží na požadavcích vaší kolekce. Hybridní kolekce vyžaduje připojení z Azure Remoteappu tooyour do místní sítě. Pokud používáte stávající virtuální síť / síť ExpressRoute, je to bez dalších poplatků. Ale pokud používáte novou virtuální síť Azure a bránu nebo Expressroute, vám bude účtována pro hello [brány VPN](https://azure.microsoft.com/pricing/details/vpn-gateway) nebo [Express Route](https://azure.microsoft.com/pricing/details/expressroute/). Tyto poplatky (podrobně v odkazech hello) je nad vaše měsíční Azure RemoteApp nákladů.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Kolekce - co je podporováno, které byste měli používat a další
* **Jsou podporovány vlastní obchodní aplikace (LOB)?** Ano. Vytvoření toouse vlastní aplikaci v Azure Remoteappu [vlastního image šablony](remoteapp-create-custom-image.md)a odešlete ji toohello kolekce vzdálené aplikace RemoteApp.
* **Budou Mé vlastní obchodní aplikace fungovat v Azure Remoteappu?**  hello nejlepší způsob, jak toofigure, že tootest ho. Podívejte se na hello [centra pro kompatibilitu VP](http://www.rdcompatibility.com/compatibility/default.aspx).
* **Jaká metoda nasazení (cloudová nebo hybridní) je nejvhodnější pro moji organizaci?** Hybridní kolekce poskytují nejúplnější prostředí hello, pokud chcete plnou integraci s jednotné přihlašování (SSO) a zabezpečené místní připojení k síti. Cloudové kolekce poskytují tooisolate agilní a snadný způsob nasazení pomocí několika metod ověřování. Další informace o hello [možnosti nasazení](remoteapp-whatis.md).
* **Máme SQL nebo jinou databázi, buď místní nebo ve službě Azure. Který typ nasazení bychom měli použít?** To závisí na tom, kde se vaše databáze SQL nebo back-end nachází. Pokud hello databáze je v privátní síti, použijte hybridní kolekci hello. Pokud databáze hello je zveřejněné toohello Internet a umožňuje klientům připojení tooconnect tooit, můžete cloudové kolekce hello.
* **A co mapování jednotky USB a sériového portu, sdílení schránky a přesměrování tiskárny?** Všechny tyto funkce jsou v Azure RemoteAppu podporovány. Ve výchozím nastavení je sdílení schránky a přesměrování tiskárny povoleno. Další informace o přesměrování najdete [zde](remoteapp-redirection.md). 

## <a name="template-images"></a>Image šablony
* **Můžu použít cloud nebo existující virtuální počítač jako hello šablonu pro kolekci Remoteappu?** Ano! Můžete vytvořit bitovou kopii podle virtuální počítač Azure, použijte jednu z imagí hello součástí vašeho předplatného nebo vytvořit vlastní image. Podívejte se na hello [možnosti imagů Remoteappu](remoteapp-imageoptions.md).

## <a name="network-options"></a>Možnosti sítě
* **Hello hybridní kolekce vyžaduje virtuální síť. Můžeme použít stávající virtuální síť?** Je možné, pokud hello existující virtuální síť Azure VNET. Najdete v části "krok 1: nastavení virtuální sítě" v hello [pokynech k hybridní kolekci](remoteapp-create-hybrid-deployment.md) Další informace.
* **Můžu virtuální síť použít s cloudovou kolekcí?** To opravdu můžete. Další informace najdete v části [Vytvoření cloudové kolekce](remoteapp-create-cloud-deployment.md), zejména v kroku 1.

## <a name="authentication-options"></a>Možnosti ověřování
* **A co ověřování? Které metody jsou podporovány?**  hello Cloudová kolekce podporuje účty Microsoft a účty služby Azure Active Directory, které jsou také účty Office 365. Hello hybridní kolekce podporuje pouze účty Azure Active Directory, které byly synchronizovány (pomocí nástroje, například [Azure Active Directory Sync](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) z nasazení systému Windows Server Active Directory; konkrétně buď synchronizovaná s hello Možnosti synchronizace hesel nebo synchronizovaná s federací služby Active Directory Federation Services (AD FS), které jsou nakonfigurované. Můžete také nakonfigurovat službu [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

> [!NOTE]
> Uživatelé Azure Active Directory Hello musí pocházet z hello klienta, který je spojen s vaším předplatným. (Můžete zobrazit a změnit předplatné hello **nastavení** kartě hello portálu. V tématu [klienta Azure Active Directory hello změnu používaného Remoteappem](remoteapp-changetenant.md) Další informace.)
> 
> 

* **Proč nelze poskytnout Moje přístup účtu Azure Active Directory?**  hello uživatelé Azure Active Directory musí pocházet z hello adresář, který je spojen s vaším předplatným. Můžete zobrazit nebo upravit tento adresář na kartě nastavení hello hello portálu. V tématu [klienta Azure Active Directory hello změnu používaného Remoteappem](remoteapp-changetenant.md) Další informace.

## <a name="clients---what-device-can-i-use-tooaccess-azure-remoteapp"></a>Klienti – jaké zařízení můžete použít tooaccess Azure RemoteApp?
Můžete najít informace o funkčních klientech, včetně postupu pro instalaci různých klientů hello v [přístup k aplikacím v Azure Remoteappu](remoteapp-clients.md).

* **Která zařízení a operační systémy podporují hello klientské aplikace?**
  První, hello počítačích a tabletech: 
  
  * Windows 10 (náhled klienta)
  * Windows 8.1 a Windows 8
  * Windows 7 Service Pack 1
  * Mac OS X
  * Windows RT
  * tablety Android
  * iPady

    A telefony hello:
  * iPhone
  * telefon se systémem Android
  * telefon se systémem Windows
    
    [Stáhněte](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) si klienta RemoteApp.
* **Podporuje Azure RemoteApp tenké klienty?** Ano, hello následující tenké klienty systému Windows Embedded jsou podporovány:
  
  * Windows Embedded Standard 7
  * Windows Embedded 8 Standard
  * Windows Embedded 8.1 Industry Pro
  * Windows 10 IoT Enterprise
* **Jaká verze Windows serveru je podporována pro hello hostitele relace vzdálené plochy (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Podpora a zpětná vazba
* **Co je hello plán podpory pro RemoteApp?** Podpora pro správu fakturace a předplatného je k dispozici zdarma. Technická podpora je k dispozici prostřednictvím hello [plánů služby Azure](https://azure.microsoft.com/support/plans/). Bezplatnou podporu komunity můžete získat také na našem [diskusním fóru Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
* **Jak se odešlu svůj názor?** Navštivte hello [fóru pro zpětnou vazbu](https://feedback.azure.com/forums/247748-azure-remoteapp/).
* **Kdo může kontaktovat toolearn Další informace o Azure RemoteApp?** V přidání tooour [diskusní fórum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), který je otázky toopost skvělým místem, se můžete připojit hello týdně [zeptejte se odborníka hello](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), kde budeme mluvit o všechny věci vzdálené aplikace RemoteApp.
* **A co dokumentace k RemoteAppu?** Jsme rádi, že jste se zeptali. Kromě toho toohello nápovědu v zásuvce hello portálu (stačí kliknout na hello **?** na stránce portálu hello), hello následující články jsou k dispozici tooteach všechny ohledně vzdálené aplikace RemoteApp:
  
  * **Začínáme:**
    * [Co je RemoteApp?](remoteapp-whatis.md)
    * [Co jsou Image šablony vzdálené aplikace RemoteApp hello?](remoteapp-images.md)
    * [Jak funguje správa licencí?](remoteapp-licensing.md)
    * [Jak spolupracují aplikace RemoteApp a Office?](remoteapp-o365.md)
    * [Jak v RemoteAppu funguje přesměrování](remoteapp-redirection.md)?
  * **Nasazení:**
    * [Vytvoření vlastního image šablony](remoteapp-create-custom-image.md)
    * [Vytvoření hybridní kolekce](remoteapp-create-hybrid-deployment.md)
    * [Vytvoření cloudové kolekce](remoteapp-create-cloud-deployment.md)
    * [Konfigurace Azure Active Directory pro RemoteApp](remoteapp-ad.md)
    * [Publikování aplikace v RemoteAppu](remoteapp-publish.md)
  * **Správa:**
    
    * [Přidání uživatelů](remoteapp-user.md)
    * [Doporučené postupy pro konfiguraci a použití RemoteAppu](remoteapp-bestpractices.md)    
    
    Videa! O  RemoteAppu máme také řadu videí. Některých naleznete úvod ([tooAzure Úvod vzdálené aplikace RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), ostatní vás provedou nasazením ([cloudové nasazení](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) a [hybridní nasazení](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Podívejte se na ně!

### <a name="help-us-help-you"></a>Umožněte nám, abychom vám mohli pomoct
Věděli jste, že v přidání toorating v tomto článku a přidání komentářů pod, článkem můžete provést samotný článek toohello změny? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek a klikněte na tlačítko **upravit na Githubu** toomake změny – ty, převzato toous ke kontrole, a potom po na nich, uvidíte změny a vylepšení přímo tady.

