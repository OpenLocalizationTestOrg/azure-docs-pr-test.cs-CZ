---
title: "Migrace firemní webové aplikace do Azure App Service"
description: "Ukazuje, jak používat webové aplikace migrace pomocníka rychle migrovat existující webů IIS do Azure App Service Web Apps"
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
ms.openlocfilehash: 18d6a8da38b42dcf5c1500f7fc26638aea26a809
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-an-enterprise-web-app-to-azure-app-service"></a>Migrace firemní webové aplikace do Azure App Service
Můžete snadno migrovat vaší existující weby, které spustit v Internetové informační služby (IIS) 6 nebo novější k [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). 

> [!IMPORTANT]
> Windows Server 2003, bylo dosaženo konce podporu na 14. července 2015. Pokud jsou aktuálně hostuje své weby na serveru se službou IIS, je systém Windows Server 2003, webové aplikace je nízkým rizikem, nízkonákladové, a nízkým třením způsob ochrany online své weby a webové aplikace migrace pomocníka můžete automatizovat proces migrace pro vás. 
> 
> 

[Webové aplikace migrace pomocníka](https://www.movemetothecloud.net/) můžete analyzovat instalace serveru služby IIS, identifikovat servery, které lze migrovat do služby App Service, zvýrazněte všechny prvky, které nelze migrovat nebo jsou nepodporované na platformu a potom migrovat své weby a přidružené databáze do Azure.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="elements-verified-during-compatibility-analysis"></a>Elementy ověřit během analýzy kompatibility
Pomocník pro migraci vytvoří sestava připravenosti k identifikaci všechny potenciální příčiny problém nebo blokující problémy, které mohou bránit úspěšné migraci ze služby IIS místně na Azure App Service Web Apps. Některé z klíčových položek zajímat jsou:

* Port vazby – webové aplikace podporuje pouze Port 80 pro protokol HTTP a Port 443 pro komunikaci přes protokol HTTPS. Konfigurace jiný port bude ignorována a provoz se směruje na 80 nebo 443. 
* Ověřování – webové aplikace podporuje anonymní ověřování ve výchozím nastavení a ověřování pomocí formulářů je-li zadána jiná aplikace. Ověřování systému Windows lze použít pouze integrací s Azure Active Directory a AD FS. Všechny ostatní způsoby ověřování – například základní ověřování - nejsou aktuálně podporovány. 
* Globální mezipaměť sestavení (GAC) – GAC není podporována ve službě Web Apps. Pokud aplikace odkazuje na sestavení, které obvykle nasadíte do mezipaměti GAC, musíte nasadit do složky bin aplikace ve službě Web Apps. 
* IIS5 Režimu kompatibility – to není podporováno ve službě Web Apps. 
* Fondy aplikací – webové aplikace, každá lokalita a její podřízené aplikace spustit ve stejném fondu aplikací. Pokud váš web má několik podřízených aplikací využívá více fondů aplikací, konsolidovat je do jediného fondu aplikací s společná nastavení nebo migrovat každou aplikaci, aby samostatné webové aplikace.
* COM – součásti – Web Apps neumožňuje registraci komponenty modelu COM na platformě. Pokud vaše weby či aplikace využívat se u všech součástí modelu COM, musíte je přepisovat ve spravovaném kódu a nasadit je s webu nebo aplikace.
* Rozšíření ISAPI – webové aplikace může podporovat použití rozšíření ISAPI. Je třeba provést následující akce:
  
  * nasazení knihoven DLL s vaší webové aplikace 
  * registrace knihovny DLL pomocí [souboru Web.config](http://www.iis.net/configreference/system.webserver/isapifilters)
  * Umístěte soubor applicationHost.xdt v kořenovém adresáři webu s obsahem uvedených v "Povolení arbitrart rozšíření ISAPI načíst" [tohoto článku](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples) 
    
  
    
    Další příklady použití transformace dokumentů XML s vašeho webu, najdete v části [transformace webu Microsoft Azure](http://blogs.msdn.com/b/waws/archive/2014/06/17/transform-your-microsoft-azure-web-site.aspx).
* Ostatní součásti, například SharePoint, rozšíření serveru titulní stránky (FPSE), FTP, nebude se migrovat certifikáty SSL.

## <a name="how-to-use-the-web-apps-migration-assistant"></a>Postup použití pomocníka webové aplikace migrace
Tato část kroky prostřednictvím příklad o migraci několik weby využívající databázi SQL serveru a systémem Windows Server 2003 R2 (IIS 6.0) na místním počítači:

1. Ve službě IIS serveru nebo klientského počítače přejděte na [https://www.movemetothecloud.net/](https://www.movemetothecloud.net/) 
   
   ![](./media/web-sites-migration-from-iis-server/migration-tool-homepage.png)
2. Instalace webové aplikace migrace pomocníka kliknutím na **vyhrazený Server služby IIS** tlačítko. Další možnosti v blízké budoucnosti bude možnosti. 
3. Klikněte na tlačítko **nainstalovat nástroj** tlačítko Instalace webové aplikace migrace pomocníka na váš počítač.
   
   ![](./media/web-sites-migration-from-iis-server/install-page.png)
   
   > [!NOTE]
   > Můžete také kliknout na **stáhnout offline instalace** ke stažení souboru ZIP pro instalaci na serverech, které nejsou připojené k Internetu. Nebo můžete kliknout na **nahrát existující sestavy připravenosti migrace**, což je upřesňující možnost pro práci s existující migrace připravenosti sestavy dříve generovaný (vysvětlení později).
   > 
   > 
4. V **aplikaci nainstalovat** obrazovky, klikněte na tlačítko **nainstalovat** k instalaci na váš počítač. Je také nainstaluje odpovídající závislosti, jako je nasazení webu, DacFX a služby IIS, v případě potřeby. 
   
   ![](./media/web-sites-migration-from-iis-server/install-progress.png)
   
   Po instalaci webové aplikace migrace pomocníka se automaticky spustí.
5. Zvolte **migrovat lokality a databáze ze vzdáleného serveru do Azure**. Zadejte přihlašovací údaje pro správu vzdáleného serveru a klikněte na **pokračovat**. 
   
   ![](./media/web-sites-migration-from-iis-server/migrate-from-remote.png)
   
   Samozřejmě můžete migrovat z místního serveru. Možnost vzdálené je užitečné, když chcete migrovat weby z provozní server služby IIS.
   
   V tomto okamžiku bude kontrolovat nástroj pro migraci konfiguraci serveru služby IIS, například weby, aplikace, fondů aplikací a závislosti identifikovat candidate weby pro migraci. 
6. Následující snímek obrazovky ukazuje tři weby – **Default Web Site**, **TimeTracker**, a **CommerceNet4**. Všechny mají přidružené databáze, která chcete migrovat. Vyberte všechny servery, které chcete vyhodnotit a pak klikněte na tlačítko **Další**.
   
   ![](./media/web-sites-migration-from-iis-server/select-migration-candidates.png)
7. Klikněte na tlačítko **nahrát** nahrát sestava připravenosti. Pokud kliknete na tlačítko **uložte soubor místně**, můžete později spustit nástroj pro migraci a nahrát sestava uložené připravenosti, jak si předtím poznamenali.
   
   ![](./media/web-sites-migration-from-iis-server/upload-readiness-report.png)
   
   Jakmile nahrajete sestava připravenosti, provede analýzu připravenosti Azure a zobrazí výsledky. Přečtěte si podrobnosti hodnocení pro každý web a zajistěte, aby pochopili nebo vyřešili všechny problémy, než budete pokračovat. 
   
   ![](./media/web-sites-migration-from-iis-server/readiness-assessment.png)
8. Klikněte na tlačítko **zahájení migrace** a spusťte tak migraci. Jste nyní přesměrováni na Azure a přihlaste se k účtu. Je důležité, aby přihlásíte pomocí účtu, který má aktivní předplatné Azure. Pokud nemáte účet Azure, pak můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/pricing/free-trial/?WT.srch=1&WT.mc_ID=SEM_). 
9. Vyberte účet klienta, předplatné a oblasti, kterou chcete použít pro migrované službě Azure web apps a databáze a potom klikněte na **spuštění migrace**. Můžete vybrat weby pro migraci později.
   
   ![](./media/web-sites-migration-from-iis-server/choose-tenant-account.png)
10. Na další obrazovce vám provádět změny do výchozího nastavení migrace, jako například:
    
    * použít existující databázi SQL Azure nebo vytvořte novou databázi SQL Azure a nakonfigurujte jeho přihlašovací údaje
    * Výběrem webů pro migraci
    * Zadejte názvy pro Azure webových aplikací a jejich propojené databáze SQL
    * přizpůsobení globální nastavení a nastavení na úrovni webu
    
    Následující snímek obrazovky ukazuje všechny weby vybrána pro migraci s výchozím nastavením.
    
    ![](./media/web-sites-migration-from-iis-server/migration-settings.png)
    
    > [!NOTE]
    > **povolit Azure Active Directory** integruje zaškrtnout políčko vlastní nastavení webové aplikace Azure s [Azure Active Directory](../active-directory/active-directory-whatis.md) ( **výchozí adresář**). Další informace o synchronizaci Azure Active Directory s vaší místní Active Directory najdete v tématu [integrace adresáře](http://msdn.microsoft.com/library/jj573653).
    > 
    > 
11. Jakmile provedete všechny požadované změny, klikněte na možnost **vytvořit** zahájíte proces migrace. Nástroj pro migraci bude vytvoření Azure SQL Database a webové aplikace Azure a pak publikování obsahu webu a databází. Průběh migrace jasně zobrazen v nástroj pro migraci a zobrazí se souhrn obrazovka na konci, které podrobnosti lokality migrovali, jestli byl úspěšný, obsahuje odkazy na nově vytvořený službě Azure web apps. 
    
    Pokud dojde k chybě během migrace, nástroj pro migraci jasně označí selhání a vrácení změn. Můžete také moci odesílat zprávy o chybách přímo na technický tým kliknutím **odeslat zprávu o chybách** tlačítko, zásobník volání zaznamenané chyby a sestavení tělo zprávy. 
    
    ![](./media/web-sites-migration-from-iis-server/migration-error-report.png)
    
    Pokud migrace úspěšná bez chyb, můžete také kliknutím **odeslat zpětnou vazbu** tlačítko sdělit svůj názor žádné přímo. 
12. Kliknutím na odkazy webové aplikace Azure a ověřit, zda migrace proběhla úspěšně.
13. Teď můžete spravovat migrované webové aplikace v Azure App Service. Chcete-li to provést, přihlaste se k [portálu Azure](https://portal.azure.com).
14. Na portálu Azure otevřete okno webové aplikace zobrazíte vašich migrovaných webů (zobrazené jako webové aplikace) a potom kliknout na kterékoli z nich mohli začít spravovat webové aplikace, jako je například konfigurace průběžné publikování, vytvoření zálohy, automatické škálování a sledování využití nebo výkonu.
    
    ![](./media/web-sites-migration-from-iis-server/TimeTrackerMigrated.png)

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

