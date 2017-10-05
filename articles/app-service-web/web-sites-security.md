---
title: "Zabezpečení aplikace ve službě Azure App Service"
description: "Zjistěte, jak zabezpečit webové aplikace, back-end mobilní aplikace nebo aplikace API v Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 5ce560b4-42d7-4b20-935c-0445fd539e39
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: b70d74441f3d6d9793ae516b3f04e36e786a9a8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-an-app-in-azure-app-service"></a>Zabezpečení aplikace ve službě Azure App Service
Tento článek vám pomůže začít pracovat na zabezpečení vaší webové aplikace, back-end mobilní aplikace nebo aplikace API v Azure App Service. 

Zabezpečení v Azure App Service má dvě úrovně: 

* **Zabezpečení infrastruktury a platformy** -důvěřujete Azure tak, aby měl služby budete muset ve skutečnosti spuštěna věcí bezpečně v cloudu.
* **Zabezpečení aplikací** -potřebujete bezpečně návrhu aplikace. To zahrnuje jak integrovat se službou Azure Active Directory, jak spravovat certifikáty a jak byste si ověřit, že může bezpečně kontaktovat u různých služeb. 

#### <a name="infrastructure-and-platform-security"></a>Zabezpečení infrastruktury a platformy
Protože udržuje virtuálních počítačích Azure, úložiště, připojení k síti, webové rozhraní, správy a funkcí pro integraci a mnohé další služby App Service je aktivně zabezpečené a zesílené zabezpečení projde intenzivního dodržování předpisů a kontroluje pravidelné spouštění zajistit který:

* Aplikace služby App Service jsou izolovány z obou Internet a z dalších zákazníků prostředků Azure.
* Komunikace mezi aplikaci aplikační služby a další prostředky Azure (např. SQL Database) ve skupině prostředků tajné klíče (například připojovací řetězce) zůstává v rámci Azure a nemá křížové žádné hranice sítě. Tajné klíče jsou vždy šifrována.
* Veškerá komunikace mezi aplikace App Service a externím prostředkům, jako je správa prostředí PowerShell, rozhraní příkazového řádku, sadami SDK služby Azure, rozhraní REST API a hybridní připojení, jsou správně šifrována.
* 24 hodin threat management chrání prostředky služby App Service před malwarem, distributed denial of service (DDoS), man-in-the-middle typu MITM () a dalšími hrozbami. 

Další informace o infrastruktury a platformy zabezpečení v Azure najdete v tématu [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/security/).

#### <a name="application-security"></a>Zabezpečení aplikací
Sice zodpovídají za zabezpečení infrastruktury a platformy, které vaše aplikace běží v Azure, je vaší povinností zabezpečit vaše vlastní aplikace. Jinými slovy musíte k vývoji, nasadit a spravovat kódu aplikace a obsah zabezpečené způsobem. Bez tohoto kódu aplikace nebo obsah může být zranitelný vůči hrozbám, jako:

* Injektáž SQL
* Zneužití relace
* Křížové lokality skriptování
* MITM úrovni aplikace
* DDoS úrovni aplikace

Úplnou diskusi o aspekty zabezpečení webových aplikací je nad rámec tohoto dokumentu. Jako výchozí bod pro další informace o zabezpečení vaší aplikace, najdete v článku [aplikace otevřete webový projekt zabezpečení (OWASP)](https://www.owasp.org/index.php/Main_Page), konkrétně [prvních 10 projektu.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), které jsou uvedené aktuální top 10 kritické webové aplikace zabezpečení nedostatky, počítáno od OWASP členy.

## <a name="perform-penetration-testing-on-your-app"></a>Provedení průnikům testování v aplikaci
Jedním z nejjednodušších způsobů, jak začít pracovat s testování pro chyby zabezpečení v aplikaci aplikační služby je potřeba použít [integrace s Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) k ohrožení zabezpečení jedním kliknutím kontrolu ve vaší aplikaci. Můžete zobrazit výsledky testů ve zprávě o snadno pochopit a zjistěte, jak opravit jednotlivé chyby zabezpečení s podrobnými pokyny.

Pokud dáváte přednost proveďte vlastní testy průnikům nebo chcete použít jiný skener suite nebo poskytovatele, postupujte podle [Azure průnikům testování procesu schvalování](https://security-forms.azure.com/penetration-testing/terms) a získat předchozí schválení k provedení požadované průnikům testů.

## <a name="https"></a>Zabezpečené komunikace se zákazníky
Pokud použijete  **\*. azurewebsites.net** název domény, které jsou vytvořené pro aplikaci aplikační služby, můžete okamžitě pomocí protokolu HTTPS, který získáte certifikát SSL pro všechny  **\*. azurewebsites.net** názvy domén. Pokud váš web používá [vlastní název domény](app-service-web-tutorial-custom-domain.md), můžete nahrát certifikát SSL k [povolit protokol HTTPS](app-service-web-tutorial-custom-ssl.md) pro vlastní doménu.

Povolení [HTTPS](https://en.wikipedia.org/wiki/HTTPS) může pomoct chránit před útoky MITM na komunikace mezi vaší aplikace a její uživatele.

## <a name="secure-data-tier"></a>Zabezpečení datové vrstvy
Služby App Service vysoce se integruje se službou SQL Database, tak, aby všechny připojovací řetězce jsou šifrované v panelu a jsou dešifrovat jenom na virtuálním počítači, na kterém aplikace běží na *a* pouze, při spuštění aplikace. Kromě toho Azure SQL Database zahrnuje mnoho funkcí zabezpečení a pomáhá vám zabezpečit vaše data aplikací z internetových hrozeb, včetně [šifrování na rest](https://msdn.microsoft.com/library/dn948096.aspx), [vždy šifrována](https://msdn.microsoft.com/library/mt163865.aspx), [ Dynamická Data maskování](../sql-database/sql-database-dynamic-data-masking-get-started.md), a [detekce hrozby](../sql-database/sql-database-threat-detection.md). Pokud máte citlivá data nebo požadavky na dodržování předpisů, přečtěte si téma [zabezpečení databáze SQL](../sql-database/sql-database-security-overview.md) Další informace o tom, jak zabezpečit vaše data.

Pokud používáte poskytovatele databáze třetích stran, jako je například ClearDB, byste se měli obrátit se zprostředkovatele dokumentace přímo na osvědčené postupy zabezpečení.  

## <a name="develop"></a>Zabezpečený vývoj a nasazení
### <a name="publishing-profiles-and-publish-settings"></a>Publikování profily a nastavení publikování
Při vývoji aplikací, provádění úloh správy, nebo automatizaci úloh pomocí nástrojů, jako třeba **Visual Studio**, **Web Matrix**, **prostředí Azure PowerShell** nebo  **Rozhraní příkazového řádku Azure (Azure CLI)**, můžete použít buď *nastavení publikování* souboru nebo *profil publikování*. Oba typy souborů je ověření pomocí Azure a by měly být zabezpečeny před neoprávněným přístupem.

* A **nastavení publikování** soubor obsahuje
  
  * Svoje ID předplatného Azure
  * Certifikát správy, který umožňuje provádět úlohy správy pro vaše předplatné *bez nutnosti poskytnout názvu účtu nebo hesla*.
* A **profil publikování** soubor obsahuje
  
  * Informace pro publikování do vaší aplikace

Pokud používáte nástroj, který používá soubor nastavení publikování nebo soubor profil publikování, importovat soubor obsahující nastavení publikování nebo profilu do nástroje a potom **odstranit** soubor. Pokud je nutné zachovat soubor sdílet s ostatními uživateli práce na projekt například uložit na bezpečném místě, jako *šifrované* adresáři s omezenými oprávněními.

Kromě toho můžete Ujistěte se, že jsou zabezpečené importované přihlašovací údaje. Například **prostředí Azure PowerShell** a **rozhraní příkazového řádku Azure (Azure CLI)** obě ukládání importované informace ve vaší **domovský adresář** ( *~*  v systémech Linux nebo OS X a */users/uživatelské_jméno* v systémech Windows.) Vyšší bezpečnost, budete pravděpodobně chtít **šifrování** těchto umístění pomocí šifrování nástrojů, které jsou k dispozici pro operační systém.

### <a name="configuration-settings-and-connection-strings"></a>Nastavení konfigurace a připojovací řetězce
Je obvyklé, že k uložení připojovací řetězce, pověření pro ověření a jinými důvěrnými informacemi v konfiguračních souborech. Tyto soubory mohou být bohužel zveřejněné na webu, nebo zaškrtnutí do veřejného úložiště, vystavení tyto informace. Jednoduché hledání na [Githubu](https://github.com), může například odkrýt obrovském množství konfigurační soubory s zveřejněné tajných klíčů v veřejného úložiště.

Osvědčeným postupem je udržovat tyto informace z vaší aplikace konfigurační soubory. App Service umožňuje ukládat informace o konfiguraci jako součást běhové prostředí jako **nastavení aplikace** a **připojovací řetězce**. Hodnoty jsou umístěny do vaší aplikace za běhu prostřednictvím *proměnné prostředí* pro většinu programovacích jazyků. Pro aplikace .NET jsou tyto hodnoty vsunout do vaší konfigurace .NET za běhu. Kromě těchto situacích se zůstanou šifrovaná tato nastavení konfigurace není-li zobrazit nebo nakonfigurovat je pomocí [portálu Azure](https://portal.azure.com) nebo nástroje, jako je například PowerShell nebo rozhraní příkazového řádku Azure. 

Ukládání informací o konfiguraci ve službě App Service umožňuje pro aplikace Správce zamknout citlivé informace pro aplikace pro produkční. Vývojáři můžou pro vývoj aplikací používat samostatnou sadu nastavení konfigurace a nastavení může být automaticky nahrazována nastavení ve službě App Service. Ani vývojáři potřebovat znát tajné klíče, nakonfigurované pro produkční aplikaci. Další informace o konfiguraci nastavení aplikace a připojovací řetězce ve službě App Service najdete v tématu [konfigurace webové aplikace](web-sites-configure.md).

### <a name="ftps"></a>FTPS
Aplikační služba Azure poskytuje zabezpečený FTP přístup k systému souborů pro vaši aplikaci prostřednictvím **FTPS**. To umožňuje bezpečný přístup k kód aplikace na webovou aplikaci, jakož i diagnostické protokoly. Doporučuje se vždycky používat FTPS místo FTP. 

Odkaz FTPS pro vaši aplikaci najdete v následujících krocích:

1. Otevřete [portál Azure](https://portal.azure.com).
2. Vyberte **procházet všechny**.
3. Z **Procházet** vyberte **App Services**.
4. Z **App Services** okně, vyberte požadované aplikace.
5. V okně aplikace, vyberte **všechna nastavení**.
6. Z **nastavení** vyberte **vlastnosti**.
7. FTP a FTPS odkazy jsou k dispozici na **nastavení** okno. 

Další informace o FTPS najdete v tématu [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Další kroky
Další informace o zabezpečení Azure platformy, informace o vytváření sestav **zneužití nebo bezpečnostního incidentu**, nebo k informování společnosti Microsoft, který budete provádět **průnikům testování** vašeho webu najdete v části zabezpečení [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

Další informace o **web.config** nebo **applicationhost.config** souborů v aplikacích App Service naleznete v [možnosti konfigurace odemkne ve službě Azure App Service web apps](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Informace o protokolování informace pro aplikace služby App Service, která mohou být užitečné při rozpoznání útoků, najdete v části [povolit protokolování diagnostiky](web-sites-enable-diagnostic-log.md).

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte na [vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

