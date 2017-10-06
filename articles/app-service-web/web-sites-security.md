---
title: aaaSecure aplikace v Azure App Service
description: "Zjistěte, jak toosecure webovou aplikaci, back-end mobilní aplikace nebo aplikace API v Azure App Service."
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
ms.openlocfilehash: fceef7963b29516f33602dcd50591d14309bf188
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-an-app-in-azure-app-service"></a>Zabezpečení aplikace ve službě Azure App Service
Tento článek vám pomůže začít pracovat na zabezpečení vaší webové aplikace, back-end mobilní aplikace nebo aplikace API v Azure App Service. 

Zabezpečení v Azure App Service má dvě úrovně: 

* **Zabezpečení infrastruktury a platformy** -důvěryhodnosti Azure toohave hello služby budete potřebovat tooactually spustit věcí bezpečně v cloudu hello.
* **Zabezpečení aplikací** -bezpečně potřebujete toodesign hello aplikace. To zahrnuje jak integrovat se službou Azure Active Directory, jak spravovat certifikáty a jak byste si ověřit, že může bezpečně kontaktovat toodifferent služby. 

#### <a name="infrastructure-and-platform-security"></a>Zabezpečení infrastruktury a platformy
Protože udržuje hello virtuálních počítačích Azure, úložiště, připojení k síti, webové rozhraní, správy a funkcí pro integraci a mnohé další služby App Service je aktivně zabezpečené a zesílené zabezpečení projde intenzivního dodržování předpisů a kontroluje, zda na toomake pravidelné spouštění který:

* Aplikace služby App Service jsou izolované od obou hello Internet a z hello ostatním zákazníkům prostředků Azure.
* Komunikace mezi aplikaci aplikační služby a další prostředky Azure (např. SQL Database) ve skupině prostředků tajné klíče (například připojovací řetězce) zůstává v rámci Azure a nemá křížové žádné hranice sítě. Tajné klíče jsou vždy šifrována.
* Veškerá komunikace mezi aplikace App Service a externím prostředkům, jako je správa prostředí PowerShell, rozhraní příkazového řádku, sadami SDK služby Azure, rozhraní REST API a hybridní připojení, jsou správně šifrována.
* 24 hodin threat management chrání prostředky služby App Service před malwarem, distributed denial of service (DDoS), man-in-the-middle typu MITM () a dalšími hrozbami. 

Další informace o infrastruktury a platformy zabezpečení v Azure najdete v tématu [Centrum zabezpečení Azure](https://azure.microsoft.com/support/trust-center/security/).

#### <a name="application-security"></a>Zabezpečení aplikací
Sice zodpovídají za zabezpečení hello infrastruktury a platformy, které vaše aplikace běží v Azure, je vaše odpovědnosti toosecure vaše vlastní aplikace. Jinými slovy, budete potřebovat toodevelop, nasazovat a spravovat kódu aplikace a obsah zabezpečené způsobem. Bez tohoto kódu aplikace nebo obsah může být ohrožena toothreats, jako:

* Injektáž SQL
* Zneužití relace
* Křížové lokality skriptování
* MITM úrovni aplikace
* DDoS úrovni aplikace

Úplnou diskusi o aspekty zabezpečení webových aplikací je nad rámec tohoto dokumentu hello. Jako výchozí bod pro další pokyny k zabezpečení vaší aplikace, najdete v části hello [aplikace otevřete webový projekt zabezpečení (OWASP)](https://www.owasp.org/index.php/Main_Page), konkrétně hello [prvních 10 projektu.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), které seznamy hello aktuální nejlepších 10 kritické webové aplikace zabezpečení nedostatky, počítáno od OWASP členy.

## <a name="perform-penetration-testing-on-your-app"></a>Provedení průnikům testování v aplikaci
Jedním z nejjednodušší tooget způsoby hello začít s testování pro chyby zabezpečení v aplikaci aplikační služby je toouse hello [integrace s Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) ohrožení zabezpečení jedním kliknutím tooperform kontrolu ve vaší aplikaci. Můžete zobrazit výsledky testů hello v sestavě snadno pochopit a zjistěte, jak toofix jednotlivé chyby zabezpečení s podrobnými pokyny.

Pokud dáváte přednost tooperform vlastní průnikům testy nebo mají toouse jiný skener suite nebo poskytovatele, je třeba postupovat podle hello [Azure průnikům testování procesu schvalování](https://security-forms.azure.com/penetration-testing/terms) a získat průnikům hello potřeby tooperform předchozí schválení testy.

## <a name="https"></a>Zabezpečené komunikace se zákazníky
Pokud používáte hello  **\*. azurewebsites.net** název domény, které jsou vytvořené pro aplikaci aplikační služby, můžete okamžitě pomocí protokolu HTTPS, který získáte certifikát SSL pro všechny  **\*. azurewebsites.net** názvy domén. Pokud váš web používá [vlastní název domény](app-service-web-tutorial-custom-domain.md), můžete nahrát certifikát SSL příliš[povolit protokol HTTPS](app-service-web-tutorial-custom-ssl.md) pro vlastní doménu, hello.

Povolení [HTTPS](https://en.wikipedia.org/wiki/HTTPS) může pomoct chránit před útoky MITM na hello komunikace mezi vaší aplikace a její uživatele.

## <a name="secure-data-tier"></a>Zabezpečení datové vrstvy
Služby App Service vysoce integruje se službou SQL Database, tak, aby všechny hello připojovací řetězce jsou šifrované v panelu hello a jsou dešifrovat jenom na hello virtuálních počítačů spuštění této aplikace hello *a* pouze když hello aplikace běží. Kromě toho Azure SQL Database zahrnuje mnoho funkcí toohelp zabezpečení secure data aplikací z internetových hrozeb, včetně [šifrování na rest](https://msdn.microsoft.com/library/dn948096.aspx), [vždy šifrována](https://msdn.microsoft.com/library/mt163865.aspx), [ Dynamická Data maskování](../sql-database/sql-database-dynamic-data-masking-get-started.md), a [detekce hrozby](../sql-database/sql-database-threat-detection.md). Pokud máte citlivá data nebo požadavky na dodržování předpisů, přečtěte si téma [zabezpečení databáze SQL](../sql-database/sql-database-security-overview.md) Další informace o tom, toosecure vaše data.

Pokud používáte poskytovatele databáze třetích stran, jako je například ClearDB, byste se měli obrátit s hello zprostředkovatele dokumentace přímo na osvědčené postupy zabezpečení.  

## <a name="develop"></a>Zabezpečený vývoj a nasazení
### <a name="publishing-profiles-and-publish-settings"></a>Publikování profily a nastavení publikování
Při vývoji aplikací, provádění úloh správy, nebo automatizaci úloh pomocí nástrojů, jako třeba **Visual Studio**, **Web Matrix**, **prostředí Azure PowerShell** nebo hello **Rozhraní příkazového řádku azure (Azure CLI)**, můžete použít buď *nastavení publikování* souboru nebo *profil publikování*. Jak typy souborů je ověření pomocí Azure a by měly být zabezpečeny tooprevent neoprávněného přístupu.

* A **nastavení publikování** soubor obsahuje
  
  * Svoje ID předplatného Azure
  * Certifikát správy, který vám umožní tooperform úlohy správy pro vaše předplatné *bez nutnosti tooprovide názvu účtu nebo hesla*.
* A **profil publikování** soubor obsahuje
  
  * Informace o publikování tooyour aplikace

Pokud používáte nástroj, který používá soubor nastavení publikování nebo soubor profil publikování, importovat hello obsahující nastavení publikování hello nebo profilu do hello nástroje a potom **odstranit** hello souboru. Pokud je nutné zachovat hello souboru, tooshare s ostatními práce na projektu hello například uložit na bezpečném místě, jako *šifrované* adresáři s omezenými oprávněními.

Kromě toho můžete Ujistěte se, že jsou zabezpečené přihlašovací údaje hello importovat. Například **prostředí Azure PowerShell** a hello **rozhraní příkazového řádku Azure (Azure CLI)** obě ukládání importované informace ve vaší **domovský adresář** ( *~*  v systémech Linux nebo OS X a */users/uživatelské_jméno* v systémech Windows.) Vyšší bezpečnost můžete příliš**šifrování** těchto umístění pomocí šifrování nástrojů, které jsou k dispozici pro operační systém.

### <a name="configuration-settings-and-connection-strings"></a>Nastavení konfigurace a připojovací řetězce
Je běžný postup toostore připojovací řetězce, pověření pro ověření a jinými důvěrnými informacemi v konfiguračních souborech. Tyto soubory mohou být bohužel zveřejněné na webu, nebo zaškrtnutí do veřejného úložiště, vystavení tyto informace. Jednoduché hledání na [Githubu](https://github.com), může například odkrýt obrovském množství konfigurační soubory s zveřejněné tajných klíčů v hello veřejného úložiště.

Hello osvědčeným postupem je tookeep tyto informace z vaší aplikace konfigurační soubory. App Service umožňuje ukládat informace o konfiguraci v rámci prostředí runtime hello jako **nastavení aplikace** a **připojovací řetězce**. Hello hodnoty jsou zveřejněné tooyour aplikace za běhu prostřednictvím *proměnné prostředí* pro většinu programovacích jazyků. Pro aplikace .NET jsou tyto hodnoty vsunout do vaší konfigurace .NET za běhu. Kromě těchto situacích se zůstanou šifrovaná tato nastavení konfigurace není-li zobrazit nebo nakonfigurovat je pomocí hello [portálu Azure](https://portal.azure.com) nebo nástroje, jako je například PowerShell nebo hello rozhraní příkazového řádku Azure. 

Ukládání informací o konfiguraci ve službě App Service umožňuje pro aplikace hello správce toolock dolů citlivé informace pro produkční aplikace hello. Vývojáři můžou pro vývoj aplikací používat samostatnou sadu nastavení konfigurace a nastavení hello můžete být automaticky nahrazována hello nastavení ve službě App Service. Vývojáři není i hello potřebovat tajné klíče hello tooknow nakonfigurovaný pro hello produkční aplikaci. Další informace o konfiguraci nastavení aplikace a připojovací řetězce ve službě App Service najdete v tématu [konfigurace webové aplikace](web-sites-configure.md).

### <a name="ftps"></a>FTPS
Aplikační služba Azure poskytuje zabezpečené systém souborů toohello přístup FTP pro vaši aplikaci prostřednictvím **FTPS**. To vám umožní kódu aplikace hello toosecurely přístup na hello webové aplikace a také diagnostiky protokoly. Doporučuje se vždycky používat FTPS místo FTP. 

Hello FTPS odkaz pro vaši aplikaci jsou k dispozici s hello následující kroky:

1. Otevřete hello [portálu Azure](https://portal.azure.com).
2. Vyberte **procházet všechny**.
3. Z hello **Procházet** vyberte **App Services**.
4. Z hello **App Services** okně, vyberte hello požadované aplikace.
5. V okně aplikace hello vyberte **všechna nastavení**.
6. Z hello **nastavení** vyberte **vlastnosti**.
7. Hello FTP a FTPS odkazy jsou k dispozici na hello **nastavení** okno. 

Další informace o FTPS najdete v tématu [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Další kroky
Další informace o zabezpečení hello hello platformy Azure, informace o vytváření sestav **zneužití nebo bezpečnostního incidentu**, nebo tooinform společnosti Microsoft, který budete provádět **průnikům testování** z vaší lokality, najdete v části zabezpečení hello hello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

Další informace o **web.config** nebo **applicationhost.config** souborů v aplikacích App Service naleznete v [možnosti konfigurace odemkne ve službě Azure App Service web apps](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Informace o protokolování informace pro aplikace služby App Service, která mohou být užitečné při rozpoznání útoků, najdete v části [povolit protokolování diagnostiky](web-sites-enable-diagnostic-log.md).

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

