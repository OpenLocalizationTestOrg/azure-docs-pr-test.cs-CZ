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
# <a name="migrate-an-enterprise-web-app-to-azure-app-service"></a><span data-ttu-id="2fd55-103">Migrace firemní webové aplikace do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2fd55-103">Migrate an enterprise web app to Azure App Service</span></span>
<span data-ttu-id="2fd55-104">Můžete snadno migrovat vaší existující weby, které spustit v Internetové informační služby (IIS) 6 nebo novější k [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="2fd55-104">You can easily migrate your existing websites that run on Internet Information Service (IIS) 6 or later to [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="2fd55-105">Windows Server 2003, bylo dosaženo konce podporu na 14. července 2015.</span><span class="sxs-lookup"><span data-stu-id="2fd55-105">Windows Server 2003 reached end of support on July 14th 2015.</span></span> <span data-ttu-id="2fd55-106">Pokud jsou aktuálně hostuje své weby na serveru se službou IIS, je systém Windows Server 2003, webové aplikace je nízkým rizikem, nízkonákladové, a nízkým třením způsob ochrany online své weby a webové aplikace migrace pomocníka můžete automatizovat proces migrace pro vás.</span><span class="sxs-lookup"><span data-stu-id="2fd55-106">If you are currently hosting your websites on an IIS server that is Windows Server 2003, Web Apps is a low-risk, low-cost, and low-friction way to keep your websites online, and Web Apps Migration Assistant can help automate the migration process for you.</span></span> 
> 
> 

<span data-ttu-id="2fd55-107">[Webové aplikace migrace pomocníka](https://www.movemetothecloud.net/) můžete analyzovat instalace serveru služby IIS, identifikovat servery, které lze migrovat do služby App Service, zvýrazněte všechny prvky, které nelze migrovat nebo jsou nepodporované na platformu a potom migrovat své weby a přidružené databáze do Azure.</span><span class="sxs-lookup"><span data-stu-id="2fd55-107">[Web Apps Migration Assistant](https://www.movemetothecloud.net/) can analyze your IIS server installation, identify which sites can be migrated to App Service, highlight any elements that cannot be migrated or are unsupported on the platform, and then migrate your websites and associated databases to Azure.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="elements-verified-during-compatibility-analysis"></a><span data-ttu-id="2fd55-108">Elementy ověřit během analýzy kompatibility</span><span class="sxs-lookup"><span data-stu-id="2fd55-108">Elements Verified During Compatibility Analysis</span></span>
<span data-ttu-id="2fd55-109">Pomocník pro migraci vytvoří sestava připravenosti k identifikaci všechny potenciální příčiny problém nebo blokující problémy, které mohou bránit úspěšné migraci ze služby IIS místně na Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="2fd55-109">The Migration Assistant creates a readiness report to identify any potential causes for concern or blocking issues which may prevent a successful migration from on-premises IIS to Azure App Service Web Apps.</span></span> <span data-ttu-id="2fd55-110">Některé z klíčových položek zajímat jsou:</span><span class="sxs-lookup"><span data-stu-id="2fd55-110">Some of the key items to be aware of are:</span></span>

* <span data-ttu-id="2fd55-111">Port vazby – webové aplikace podporuje pouze Port 80 pro protokol HTTP a Port 443 pro komunikaci přes protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2fd55-111">Port Bindings – Web Apps only supports Port 80 for HTTP and Port 443 for HTTPS traffic.</span></span> <span data-ttu-id="2fd55-112">Konfigurace jiný port bude ignorována a provoz se směruje na 80 nebo 443.</span><span class="sxs-lookup"><span data-stu-id="2fd55-112">Different port configurations will be ignored and traffic will be routed to 80 or 443.</span></span> 
* <span data-ttu-id="2fd55-113">Ověřování – webové aplikace podporuje anonymní ověřování ve výchozím nastavení a ověřování pomocí formulářů je-li zadána jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="2fd55-113">Authentication – Web Apps supports Anonymous Authentication by default and Forms Authentication where specified by an application.</span></span> <span data-ttu-id="2fd55-114">Ověřování systému Windows lze použít pouze integrací s Azure Active Directory a AD FS.</span><span class="sxs-lookup"><span data-stu-id="2fd55-114">Windows Authentication can be used by integrating with Azure Active Directory and ADFS only.</span></span> <span data-ttu-id="2fd55-115">Všechny ostatní způsoby ověřování – například základní ověřování - nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="2fd55-115">All other forms of authentication - for example, Basic Authentication - are not currently supported.</span></span> 
* <span data-ttu-id="2fd55-116">Globální mezipaměť sestavení (GAC) – GAC není podporována ve službě Web Apps.</span><span class="sxs-lookup"><span data-stu-id="2fd55-116">Global Assembly Cache (GAC) – The GAC is not supported in Web Apps.</span></span> <span data-ttu-id="2fd55-117">Pokud aplikace odkazuje na sestavení, které obvykle nasadíte do mezipaměti GAC, musíte nasadit do složky bin aplikace ve službě Web Apps.</span><span class="sxs-lookup"><span data-stu-id="2fd55-117">If your application references assemblies which you usually deploy to the GAC, you will need to deploy to the application bin folder in Web Apps.</span></span> 
* <span data-ttu-id="2fd55-118">IIS5 Režimu kompatibility – to není podporováno ve službě Web Apps.</span><span class="sxs-lookup"><span data-stu-id="2fd55-118">IIS5 Compatibility Mode – This is not supported in Web Apps.</span></span> 
* <span data-ttu-id="2fd55-119">Fondy aplikací – webové aplikace, každá lokalita a její podřízené aplikace spustit ve stejném fondu aplikací.</span><span class="sxs-lookup"><span data-stu-id="2fd55-119">Application Pools – In Web Apps, each site and its child applications run in the same application pool.</span></span> <span data-ttu-id="2fd55-120">Pokud váš web má několik podřízených aplikací využívá více fondů aplikací, konsolidovat je do jediného fondu aplikací s společná nastavení nebo migrovat každou aplikaci, aby samostatné webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2fd55-120">If your site has multiple child applications utilizing multiple application pools, consolidate them to a single application pool with common settings or migrate each application to a separate web app.</span></span>
* <span data-ttu-id="2fd55-121">COM – součásti – Web Apps neumožňuje registraci komponenty modelu COM na platformě.</span><span class="sxs-lookup"><span data-stu-id="2fd55-121">COM Components – Web Apps does not allow the registration of COM Components on the platform.</span></span> <span data-ttu-id="2fd55-122">Pokud vaše weby či aplikace využívat se u všech součástí modelu COM, musíte je přepisovat ve spravovaném kódu a nasadit je s webu nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="2fd55-122">If your websites or applications make use of any COM Components, you must rewrite them in managed code and deploy them with the website or application.</span></span>
* <span data-ttu-id="2fd55-123">Rozšíření ISAPI – webové aplikace může podporovat použití rozšíření ISAPI.</span><span class="sxs-lookup"><span data-stu-id="2fd55-123">ISAPI Extensions – Web Apps can support the use of ISAPI Extensions.</span></span> <span data-ttu-id="2fd55-124">Je třeba provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="2fd55-124">You need to do the following:</span></span>
  
  * <span data-ttu-id="2fd55-125">nasazení knihoven DLL s vaší webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2fd55-125">deploy the DLLs with your web app</span></span> 
  * <span data-ttu-id="2fd55-126">registrace knihovny DLL pomocí [souboru Web.config](http://www.iis.net/configreference/system.webserver/isapifilters)</span><span class="sxs-lookup"><span data-stu-id="2fd55-126">register the DLLs using [Web.config](http://www.iis.net/configreference/system.webserver/isapifilters)</span></span>
  * <span data-ttu-id="2fd55-127">Umístěte soubor applicationHost.xdt v kořenovém adresáři webu s obsahem uvedených v "Povolení arbitrart rozšíření ISAPI načíst" [tohoto článku](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)</span><span class="sxs-lookup"><span data-stu-id="2fd55-127">place an applicationHost.xdt file in the site root with the content outlined in "Allowing arbitrart ISAPI extensions to be loaded" [section of this article](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)</span></span> 
    
  
    
    <span data-ttu-id="2fd55-128">Další příklady použití transformace dokumentů XML s vašeho webu, najdete v části [transformace webu Microsoft Azure](http://blogs.msdn.com/b/waws/archive/2014/06/17/transform-your-microsoft-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="2fd55-128">For more examples of how to use XML Document Transformations with your website, see [Transform your Microsoft Azure Web Site](http://blogs.msdn.com/b/waws/archive/2014/06/17/transform-your-microsoft-azure-web-site.aspx).</span></span>
* <span data-ttu-id="2fd55-129">Ostatní součásti, například SharePoint, rozšíření serveru titulní stránky (FPSE), FTP, nebude se migrovat certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="2fd55-129">Other components like SharePoint, front page server extensions (FPSE), FTP, SSL certificates will not be migrated.</span></span>

## <a name="how-to-use-the-web-apps-migration-assistant"></a><span data-ttu-id="2fd55-130">Postup použití pomocníka webové aplikace migrace</span><span class="sxs-lookup"><span data-stu-id="2fd55-130">How to use the Web Apps Migration Assistant</span></span>
<span data-ttu-id="2fd55-131">Tato část kroky prostřednictvím příklad o migraci několik weby využívající databázi SQL serveru a systémem Windows Server 2003 R2 (IIS 6.0) na místním počítači:</span><span class="sxs-lookup"><span data-stu-id="2fd55-131">This section steps through an example to to migrate a few websites that use a SQL Server database and running on an on-premises Windows Server 2003 R2 (IIS 6.0) machine:</span></span>

1. <span data-ttu-id="2fd55-132">Ve službě IIS serveru nebo klientského počítače přejděte na [https://www.movemetothecloud.net/](https://www.movemetothecloud.net/)</span><span class="sxs-lookup"><span data-stu-id="2fd55-132">On the IIS server or your client machine navigate to [https://www.movemetothecloud.net/](https://www.movemetothecloud.net/)</span></span> 
   
   ![](./media/web-sites-migration-from-iis-server/migration-tool-homepage.png)
2. <span data-ttu-id="2fd55-133">Instalace webové aplikace migrace pomocníka kliknutím na **vyhrazený Server služby IIS** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2fd55-133">Install Web Apps Migration Assistant by clicking on the **Dedicated IIS Server** button.</span></span> <span data-ttu-id="2fd55-134">Další možnosti v blízké budoucnosti bude možnosti.</span><span class="sxs-lookup"><span data-stu-id="2fd55-134">More options will be options in the near future.</span></span> 
3. <span data-ttu-id="2fd55-135">Klikněte na tlačítko **nainstalovat nástroj** tlačítko Instalace webové aplikace migrace pomocníka na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="2fd55-135">Click the **Install Tool** button to install Web Apps Migration Assistant on your machine.</span></span>
   
   ![](./media/web-sites-migration-from-iis-server/install-page.png)
   
   > [!NOTE]
   > <span data-ttu-id="2fd55-136">Můžete také kliknout na **stáhnout offline instalace** ke stažení souboru ZIP pro instalaci na serverech, které nejsou připojené k Internetu.</span><span class="sxs-lookup"><span data-stu-id="2fd55-136">You can also click **Download for offline install** to download a ZIP file for installing on servers not connected to the internet.</span></span> <span data-ttu-id="2fd55-137">Nebo můžete kliknout na **nahrát existující sestavy připravenosti migrace**, což je upřesňující možnost pro práci s existující migrace připravenosti sestavy dříve generovaný (vysvětlení později).</span><span class="sxs-lookup"><span data-stu-id="2fd55-137">Or, you can click **Upload an existing migration readiness report**, which is an advanced option to work with an existing migration readiness report that you previously generated (explained later).</span></span>
   > 
   > 
4. <span data-ttu-id="2fd55-138">V **aplikaci nainstalovat** obrazovky, klikněte na tlačítko **nainstalovat** k instalaci na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="2fd55-138">In the **Application Install** screen, click **Install** to install on your machine.</span></span> <span data-ttu-id="2fd55-139">Je také nainstaluje odpovídající závislosti, jako je nasazení webu, DacFX a služby IIS, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="2fd55-139">It will also install corresponding dependencies like Web Deploy, DacFX, and IIS, if needed.</span></span> 
   
   ![](./media/web-sites-migration-from-iis-server/install-progress.png)
   
   <span data-ttu-id="2fd55-140">Po instalaci webové aplikace migrace pomocníka se automaticky spustí.</span><span class="sxs-lookup"><span data-stu-id="2fd55-140">Once installed, Web Apps Migration Assistant automatically starts.</span></span>
5. <span data-ttu-id="2fd55-141">Zvolte **migrovat lokality a databáze ze vzdáleného serveru do Azure**.</span><span class="sxs-lookup"><span data-stu-id="2fd55-141">Choose **Migrate sites and databases from a remote server to Azure**.</span></span> <span data-ttu-id="2fd55-142">Zadejte přihlašovací údaje pro správu vzdáleného serveru a klikněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="2fd55-142">Enter the administrative credentials for the remote server and click **Continue**.</span></span> 
   
   ![](./media/web-sites-migration-from-iis-server/migrate-from-remote.png)
   
   <span data-ttu-id="2fd55-143">Samozřejmě můžete migrovat z místního serveru.</span><span class="sxs-lookup"><span data-stu-id="2fd55-143">You can of course choose to migrate from the local server.</span></span> <span data-ttu-id="2fd55-144">Možnost vzdálené je užitečné, když chcete migrovat weby z provozní server služby IIS.</span><span class="sxs-lookup"><span data-stu-id="2fd55-144">The remote option is useful when you want to migrate websites from a production IIS server.</span></span>
   
   <span data-ttu-id="2fd55-145">V tomto okamžiku bude kontrolovat nástroj pro migraci konfiguraci serveru služby IIS, například weby, aplikace, fondů aplikací a závislosti identifikovat candidate weby pro migraci.</span><span class="sxs-lookup"><span data-stu-id="2fd55-145">At this point the migration tool will inspect the your IIS server's configuration, such as Sites, Applications, Application Pools, and dependencies to identify candidate websites for migration.</span></span> 
6. <span data-ttu-id="2fd55-146">Následující snímek obrazovky ukazuje tři weby – **Default Web Site**, **TimeTracker**, a **CommerceNet4**.</span><span class="sxs-lookup"><span data-stu-id="2fd55-146">The screenshot below shows three websites – **Default Web Site**, **TimeTracker**, and **CommerceNet4**.</span></span> <span data-ttu-id="2fd55-147">Všechny mají přidružené databáze, která chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="2fd55-147">All of them have an associated database that we want to migrate.</span></span> <span data-ttu-id="2fd55-148">Vyberte všechny servery, které chcete vyhodnotit a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2fd55-148">Select all of the sites you would like to assess and then click **Next**.</span></span>
   
   ![](./media/web-sites-migration-from-iis-server/select-migration-candidates.png)
7. <span data-ttu-id="2fd55-149">Klikněte na tlačítko **nahrát** nahrát sestava připravenosti.</span><span class="sxs-lookup"><span data-stu-id="2fd55-149">Click **Upload** to upload the readiness report.</span></span> <span data-ttu-id="2fd55-150">Pokud kliknete na tlačítko **uložte soubor místně**, můžete později spustit nástroj pro migraci a nahrát sestava uložené připravenosti, jak si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="2fd55-150">If you click **save file locally**, you can run the migration tool again later and upload the saved readiness report as noted earlier.</span></span>
   
   ![](./media/web-sites-migration-from-iis-server/upload-readiness-report.png)
   
   <span data-ttu-id="2fd55-151">Jakmile nahrajete sestava připravenosti, provede analýzu připravenosti Azure a zobrazí výsledky.</span><span class="sxs-lookup"><span data-stu-id="2fd55-151">Once you upload the readiness report, Azure performs readiness analysis and shows you the results.</span></span> <span data-ttu-id="2fd55-152">Přečtěte si podrobnosti hodnocení pro každý web a zajistěte, aby pochopili nebo vyřešili všechny problémy, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="2fd55-152">Read the assessment details for each website and make sure that you understand or have addressed all issues before you proceed.</span></span> 
   
   ![](./media/web-sites-migration-from-iis-server/readiness-assessment.png)
8. <span data-ttu-id="2fd55-153">Klikněte na tlačítko **zahájení migrace** a spusťte tak migraci. Jste nyní přesměrováni na Azure a přihlaste se k účtu.</span><span class="sxs-lookup"><span data-stu-id="2fd55-153">Click **Begin Migration** to start the migration.You will now be redirected to Azure to log into your account.</span></span> <span data-ttu-id="2fd55-154">Je důležité, aby přihlásíte pomocí účtu, který má aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2fd55-154">It is important that you log in with an account that has an active Azure Subscription.</span></span> <span data-ttu-id="2fd55-155">Pokud nemáte účet Azure, pak můžete si zaregistrovat bezplatnou zkušební verzi [zde](https://azure.microsoft.com/pricing/free-trial/?WT.srch=1&WT.mc_ID=SEM_).</span><span class="sxs-lookup"><span data-stu-id="2fd55-155">If you do not have an Azure account then you can sign up for a free trial [here](https://azure.microsoft.com/pricing/free-trial/?WT.srch=1&WT.mc_ID=SEM_).</span></span> 
9. <span data-ttu-id="2fd55-156">Vyberte účet klienta, předplatné a oblasti, kterou chcete použít pro migrované službě Azure web apps a databáze a potom klikněte na **spuštění migrace**.</span><span class="sxs-lookup"><span data-stu-id="2fd55-156">Select the tenant account, Azure subscription and region to use for your migrated Azure web apps and databases, and then click **Start Migration**.</span></span> <span data-ttu-id="2fd55-157">Můžete vybrat weby pro migraci později.</span><span class="sxs-lookup"><span data-stu-id="2fd55-157">You can select the websites to migrate later.</span></span>
   
   ![](./media/web-sites-migration-from-iis-server/choose-tenant-account.png)
10. <span data-ttu-id="2fd55-158">Na další obrazovce vám provádět změny do výchozího nastavení migrace, jako například:</span><span class="sxs-lookup"><span data-stu-id="2fd55-158">On the next screen you can make changes to the default migration settings, such as:</span></span>
    
    * <span data-ttu-id="2fd55-159">použít existující databázi SQL Azure nebo vytvořte novou databázi SQL Azure a nakonfigurujte jeho přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="2fd55-159">use an existing Azure SQL Database or create a new Azure SQL Database, and configure its credentials</span></span>
    * <span data-ttu-id="2fd55-160">Výběrem webů pro migraci</span><span class="sxs-lookup"><span data-stu-id="2fd55-160">select the websites to migrate</span></span>
    * <span data-ttu-id="2fd55-161">Zadejte názvy pro Azure webových aplikací a jejich propojené databáze SQL</span><span class="sxs-lookup"><span data-stu-id="2fd55-161">define names for the Azure web apps and their linked SQL databases</span></span>
    * <span data-ttu-id="2fd55-162">přizpůsobení globální nastavení a nastavení na úrovni webu</span><span class="sxs-lookup"><span data-stu-id="2fd55-162">customize the global settings and site-level settings</span></span>
    
    <span data-ttu-id="2fd55-163">Následující snímek obrazovky ukazuje všechny weby vybrána pro migraci s výchozím nastavením.</span><span class="sxs-lookup"><span data-stu-id="2fd55-163">The screenshot below shows all the websites selected for migration with the default settings.</span></span>
    
    ![](./media/web-sites-migration-from-iis-server/migration-settings.png)
    
    > [!NOTE]
    > <span data-ttu-id="2fd55-164">**povolit Azure Active Directory** integruje zaškrtnout políčko vlastní nastavení webové aplikace Azure s [Azure Active Directory](../active-directory/active-directory-whatis.md) ( **výchozí adresář**).</span><span class="sxs-lookup"><span data-stu-id="2fd55-164">the **Enable Azure Active Directory** checkbox in custom settings integrates the Azure web app with [Azure Active Directory](../active-directory/active-directory-whatis.md) (the **Default Directory**).</span></span> <span data-ttu-id="2fd55-165">Další informace o synchronizaci Azure Active Directory s vaší místní Active Directory najdete v tématu [integrace adresáře](http://msdn.microsoft.com/library/jj573653).</span><span class="sxs-lookup"><span data-stu-id="2fd55-165">For more information on syncing Azure Active Directory with your on-premises Active Directory, see [Directory integration](http://msdn.microsoft.com/library/jj573653).</span></span>
    > 
    > 
11. <span data-ttu-id="2fd55-166">Jakmile provedete všechny požadované změny, klikněte na možnost **vytvořit** zahájíte proces migrace.</span><span class="sxs-lookup"><span data-stu-id="2fd55-166">Once you make all the desired changes, click **Create** to start the migration process.</span></span> <span data-ttu-id="2fd55-167">Nástroj pro migraci bude vytvoření Azure SQL Database a webové aplikace Azure a pak publikování obsahu webu a databází.</span><span class="sxs-lookup"><span data-stu-id="2fd55-167">The migration tool will create the Azure SQL Database and Azure web app, and then publish the website content and databases.</span></span> <span data-ttu-id="2fd55-168">Průběh migrace jasně zobrazen v nástroj pro migraci a zobrazí se souhrn obrazovka na konci, které podrobnosti lokality migrovali, jestli byl úspěšný, obsahuje odkazy na nově vytvořený službě Azure web apps.</span><span class="sxs-lookup"><span data-stu-id="2fd55-168">The migration progress is clearly shown in the migration tool, and you will see a summary screen at the end, which details the sites migrated, whether they were successful, links to the newly-created Azure web apps.</span></span> 
    
    <span data-ttu-id="2fd55-169">Pokud dojde k chybě během migrace, nástroj pro migraci jasně označí selhání a vrácení změn.</span><span class="sxs-lookup"><span data-stu-id="2fd55-169">If any error occurs during migration, the migration tool will clearly indicate the failure and rollback the changes.</span></span> <span data-ttu-id="2fd55-170">Můžete také moci odesílat zprávy o chybách přímo na technický tým kliknutím **odeslat zprávu o chybách** tlačítko, zásobník volání zaznamenané chyby a sestavení tělo zprávy.</span><span class="sxs-lookup"><span data-stu-id="2fd55-170">You will also be able to send the error report directly to the engineering team by clicking the **Send Error Report** button, with the captured failure call stack and build message body.</span></span> 
    
    ![](./media/web-sites-migration-from-iis-server/migration-error-report.png)
    
    <span data-ttu-id="2fd55-171">Pokud migrace úspěšná bez chyb, můžete také kliknutím **odeslat zpětnou vazbu** tlačítko sdělit svůj názor žádné přímo.</span><span class="sxs-lookup"><span data-stu-id="2fd55-171">If migrate succeeds without errors, you can also click the **Give Feedback** button to provide any feedback directly.</span></span> 
12. <span data-ttu-id="2fd55-172">Kliknutím na odkazy webové aplikace Azure a ověřit, zda migrace proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="2fd55-172">Click the links to the Azure web apps and verify that the migration has succeeded.</span></span>
13. <span data-ttu-id="2fd55-173">Teď můžete spravovat migrované webové aplikace v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2fd55-173">You can now manage the migrated web apps in Azure App Service.</span></span> <span data-ttu-id="2fd55-174">Chcete-li to provést, přihlaste se k [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2fd55-174">To do this, log into the [Azure Portal](https://portal.azure.com).</span></span>
14. <span data-ttu-id="2fd55-175">Na portálu Azure otevřete okno webové aplikace zobrazíte vašich migrovaných webů (zobrazené jako webové aplikace) a potom kliknout na kterékoli z nich mohli začít spravovat webové aplikace, jako je například konfigurace průběžné publikování, vytvoření zálohy, automatické škálování a sledování využití nebo výkonu.</span><span class="sxs-lookup"><span data-stu-id="2fd55-175">In the Azure Portal, open the Web Apps blade to see your migrated websites (shown as web apps), then click on any one of them to start managing the web app, such as configuring continuous publishing, creating backups, autoscaling, and monitoring usage or performance.</span></span>
    
    ![](./media/web-sites-migration-from-iis-server/TimeTrackerMigrated.png)

> [!NOTE]
> <span data-ttu-id="2fd55-176">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2fd55-176">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="2fd55-177">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="2fd55-177">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="2fd55-178">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="2fd55-178">What's changed</span></span>
* <span data-ttu-id="2fd55-179">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="2fd55-179">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>
