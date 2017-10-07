---
title: "aaaMigrate aplikace tooAzure webové rozlehlé sítě služby App Service"
description: "Ukazuje, jak webové aplikace migrace pomocníka tooquickly toouse migrovat existující služby IIS weby tooAzure App Service Web Apps"
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: 
ms.assetid: 2e846fc0-37cc-42e6-ac57-ff442ef16e85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 7d66c5b799f0eefe85cbd9ba596ee0a05167f295
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-enterprise-web-app-tooazure-app-service"></a>Migrace aplikací tooAzure webové rozlehlé sítě služby App Service
Můžete snadno migrovat vaší existující weby, které spustit v Internetové informační služby (IIS) 6 nebo novější příliš[App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). 

> [!IMPORTANT]
> Windows Server 2003, bylo dosaženo konce podporu na 14. července 2015. Pokud se aktuálně hostuje své weby na serveru služby IIS, která je Windows Server 2003, webové aplikace je nízkým rizikem, nízkými náklady a nízkým třením způsob tookeep, které vaše online weby a webové aplikace migrace pomocníka pomůžou automatizovat proces migrace hello za vás. 
> 
> 

[Webové aplikace migrace pomocníka](https://www.movemetothecloud.net/) můžete analyzovat instalace serveru služby IIS, identifikovat servery, které může být migrované tooApp služby, zvýrazněte všechny prvky, které nelze migrovat nebo jsou nepodporované na platformě hello a potom migrovat své weby a tooAzure přidružených databázích.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="elements-verified-during-compatibility-analysis"></a>Elementy ověřit během analýzy kompatibility
Hello Pomocník pro migraci vytvoří tooidentify sestava připravenosti způsobí, že všechny potenciální potíže nebo blokující problémy, které mohou bránit úspěšné migraci z místní služby IIS tooAzure App Service Web Apps. Některé toobe klíčové body hello vědět, jsou:

* Port vazby – webové aplikace podporuje pouze Port 80 pro protokol HTTP a Port 443 pro komunikaci přes protokol HTTPS. Jiný port konfigurace bude ignorována a provoz se směruje too80 nebo 443. 
* Ověřování – webové aplikace podporuje anonymní ověřování ve výchozím nastavení a ověřování pomocí formulářů je-li zadána jiná aplikace. Ověřování systému Windows lze použít pouze integrací s Azure Active Directory a AD FS. Všechny ostatní způsoby ověřování – například základní ověřování - nejsou aktuálně podporovány. 
* Globální mezipaměť sestavení (GAC) – hello GAC není podporováno ve službě Web Apps. Pokud aplikace odkazuje na sestavení což obvykle nasadit toohello GAC, budete potřebovat složce bin aplikace toohello toodeploy ve službě Web Apps. 
* IIS5 Režimu kompatibility – to není podporováno ve službě Web Apps. 
* Fondy aplikací – ve službě Web Apps, každá lokalita a její podřízené aplikace spuštěný v hello stejný fond aplikací. Pokud váš web má několik podřízených aplikací využívá více fondů aplikací, je konsolidovat tooa jeden fond aplikací s společná nastavení nebo migrovat každé aplikace tooa samostatné webové aplikace.
* COM – součásti – Web Apps neumožňuje hello registraci komponenty modelu COM na platformě hello. Pokud vaše weby či aplikace využívat se u všech součástí modelu COM, musíte je přepisovat ve spravovaném kódu a nasadit je s hello webu nebo aplikace.
* Rozšíření ISAPI – webové aplikace může podporovat použití hello rozšíření ISAPI. Je třeba toodo hello následující:
  
  * nasazení knihoven DLL hello s vaší webové aplikace 
  * registrace knihovny DLL hello pomocí [souboru Web.config](http://www.iis.net/configreference/system.webserver/isapifilters)
  * Umístěte soubor applicationHost.xdt v kořenovém adresáři hello webu s obsahem hello uvedených v "Povolení toobe rozšíření ISAPI arbitrart načíst" [tohoto článku](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples) 
    
  
    
    Další příklady najdete v části toouse transformace dokumentů XML s vaším webem [transformace webu Microsoft Azure](http://blogs.msdn.com/b/waws/archive/2014/06/17/transform-your-microsoft-azure-web-site.aspx).
* Ostatní součásti, například SharePoint, rozšíření serveru titulní stránky (FPSE), FTP, nebude se migrovat certifikáty SSL.

## <a name="how-toouse-hello-web-apps-migration-assistant"></a>Jak hello webové aplikace migrace pomocníka toouse
V této části kroky prostřednictvím tootoomigrate příklad několik weby využívající databázi systému SQL Server a systémem Windows Server 2003 R2 (IIS 6.0) na místním počítači:

1. Na hello IIS serveru nebo klientského počítače přejděte příliš[https://www.movemetothecloud.net/](https://www.movemetothecloud.net/) 
   
   ![](./media/web-sites-migration-from-iis-server/migration-tool-homepage.png)
2. Instalace webové aplikace migrace pomocníka kliknutím na hello **vyhrazený Server služby IIS** tlačítko. Další možnosti budou možnosti v blízké budoucnosti hello. 
3. Klikněte na tlačítko hello **nainstalovat nástroj** tlačítko tooinstall webové aplikace migrace pomocníka na váš počítač.
   
   ![](./media/web-sites-migration-from-iis-server/install-page.png)
   
   > [!NOTE]
   > Můžete také kliknout na **stáhnout offline instalace** toodownload ZIP souborů pro instalaci na serverech není připojené toohello Internetu. Nebo můžete kliknout na **nahrát existující sestavy připravenosti migrace**, který je upřesňující možnost toowork s existující migrace připravenosti sestavy dříve generovaný (vysvětlení později).
   > 
   > 
4. V hello **aplikaci nainstalovat** obrazovky, klikněte na tlačítko **nainstalovat** tooinstall na váš počítač. Je také nainstaluje odpovídající závislosti, jako je nasazení webu, DacFX a služby IIS, v případě potřeby. 
   
   ![](./media/web-sites-migration-from-iis-server/install-progress.png)
   
   Po instalaci webové aplikace migrace pomocníka se automaticky spustí.
5. Zvolte **migrovat lokality a databáze ze vzdáleného serveru tooAzure**. Zadejte pověření pro správu hello hello vzdáleného serveru a klikněte na tlačítko **pokračovat**. 
   
   ![](./media/web-sites-migration-from-iis-server/migrate-from-remote.png)
   
   Samozřejmě můžete toomigrate z místního serveru hello. možnost vzdálené Hello je užitečné, když chcete weby toomigrate z provozní server služby IIS.
   
   V tomto okamžiku bude nástroj pro migraci hello kontrolovat hello konfiguraci serveru služby IIS, například weby, aplikace, fondů aplikací a závislosti tooidentify candidate webů pro migraci. 
6. Následující snímek obrazovky Hello zobrazuje tři weby – **Default Web Site**, **TimeTracker**, a **CommerceNet4**. Všechny mají databázi přidružené, že má být toomigrate. Vyberte všechny servery hello chcete vytvořit tooassess a pak klikněte na tlačítko **Další**.
   
   ![](./media/web-sites-migration-from-iis-server/select-migration-candidates.png)
7. Klikněte na tlačítko **nahrát** sestava připravenosti tooupload hello. Pokud kliknete na tlačítko **uložte soubor místně**, můžete spustit nástroj pro migraci hello později a nahrávání hello připravenosti sestava uložena jako si předtím poznamenali.
   
   ![](./media/web-sites-migration-from-iis-server/upload-readiness-report.png)
   
   Jakmile nahrajete hello sestava připravenosti, Azure provádí analýzu připravenosti a ukazuje hello výsledky. Přečíst podrobnosti o hodnocení hello pro každý web a zajistěte, aby pochopili nebo vyřešili všechny problémy, než budete pokračovat. 
   
   ![](./media/web-sites-migration-from-iis-server/readiness-assessment.png)
8. Klikněte na tlačítko **zahájení migrace** toostart hello migrace. Přesměrovaného tooAzure toolog teď bude do vašeho účtu. Je důležité, aby přihlásíte pomocí účtu, který má aktivní předplatné Azure. Pokud nemáte účet Azure, pak můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/pricing/free-trial/?WT.srch=1&WT.mc_ID=SEM_). 
9. Vyberte účet klienta hello, předplatného Azure a toouse oblast pro migrované službě Azure web apps a databáze a pak klikněte na tlačítko **spuštění migrace**. Můžete vybrat weby toomigrate hello později.
   
   ![](./media/web-sites-migration-from-iis-server/choose-tenant-account.png)
10. Na další obrazovce hello můžete provádět změny toohello výchozí migrace nastavení, jako například:
    
    * použít existující databázi SQL Azure nebo vytvořte novou databázi SQL Azure a nakonfigurujte jeho přihlašovací údaje
    * Vyberte weby toomigrate hello
    * Zadejte názvy hello webové aplikace Azure a jejich propojené databáze SQL
    * přizpůsobení hello globální nastavení a nastavení na úrovni webu
    
    Následující snímek obrazovky Hello ukazuje všechny weby hello vybrána pro migraci s výchozím nastavením hello.
    
    ![](./media/web-sites-migration-from-iis-server/migration-settings.png)
    
    > [!NOTE]
    > Hello **povolit Azure Active Directory** zaškrtnout políčko vlastní nastavení integruje hello webové aplikace Azure s [Azure Active Directory](../active-directory/active-directory-whatis.md) (hello **výchozí adresář**). Další informace o synchronizaci Azure Active Directory s vaší místní Active Directory najdete v tématu [integrace adresáře](http://msdn.microsoft.com/library/jj573653).
    > 
    > 
11. Jakmile provedete všechny potřeby hello změny, klikněte na možnost **vytvořit** proces migrace toostart hello. Nástroj pro migraci Hello bude vytvoření webové aplikace hello Azure SQL Database a Azure a pak publikovat obsah webu hello a databází. průběh migrace Hello jasně zobrazen v hello nástroj pro migraci a zobrazí se souhrn obrazovka hello end, které lokality hello podrobnosti migrovat, jestli byli úspěšní, odkazy toohello nově vytvořené webové aplikace Azure. 
    
    Pokud žádné dojde k chybě během migrace, bude nástroj pro migraci hello jasně znamenat hello selhání a vrácení hello změny. Také budete moct toosend hello chybách přímo toohello inženýrství team kliknutím hello **odeslat zprávu o chybách** tlačítko zásobníkem volání hello zaznamenané chyby a sestavení tělo zprávy. 
    
    ![](./media/web-sites-migration-from-iis-server/migration-error-report.png)
    
    Pokud migrace úspěšná bez chyb, můžete také kliknout na hello **odeslat zpětnou vazbu** tlačítko tooprovide žádné zpětnou vazbu přímo. 
12. Klikněte na tlačítko hello odkazy toohello webové aplikace Azure a ověřit, že hello migrace proběhla úspěšně.
13. Teď můžete spravovat hello migrovat webové aplikace v Azure App Service. toodo se přihlaste se k hello [portálu Azure](https://portal.azure.com).
14. V hello portálu Azure, otevřete toosee okně webové aplikace hello vašich migrovaných webů (zobrazené jako webové aplikace), a potom klikněte na kterékoli z nich toostart Správa hello webové aplikace, jako je například konfigurace průběžné publikování, vytváření záloh, automatické škálování a sledování využití nebo výkon.
    
    ![](./media/web-sites-migration-from-iis-server/TimeTrackerMigrated.png)

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

