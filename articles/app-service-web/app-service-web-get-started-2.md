---
title: "aaaAdd funkce tooyour první webové aplikace | Microsoft Docs"
description: "Přidejte zajímavé funkce tooyour první webové aplikace za pár minut."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 542671c2-22f0-4f20-8b4b-fa477264c492
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2016
ms.author: cephalin
ms.openlocfilehash: 46c9b118c2c188508cb0a369c691a79073b7d7b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-functionality-tooyour-first-web-app"></a>Přidání funkce tooyour první webové aplikace
V [nasazení vaší první aplikace tooAzure web v pěti minutách](app-service-web-get-started-dotnet.md), jste nasadili ukázkovou webovou aplikaci do [Azure App Service](../app-service/app-service-value-prop-what-is.md). V tomto článku rychle přidáte některé skvělé funkce tooyour nasazené webové aplikace. Během několik minut provedete tyto úkony:

* vynucení ověřování uživatelů
* automatické škálování aplikace
* přijímat výstrahy na hello výkon vaší aplikace

Bez ohledu na to, kterou jste nasadili v předchozím článku hello ukázkovou aplikaci můžete podle nich zorientujete v kurzu hello.

Hello tři aktivity obsažené v této kurzu jsou jenom několik příkladů hello mnoho užitečných funkcí, které získáte umístěním webové aplikace ve službě App Service. Mnoho funkcí hello jsou k dispozici v hello **volné** vrstvy (což je, co vaše první webová aplikace běží na), a můžete použít vaše zkušební kredity tootry na funkce, které vyžadují vyšší cenové úrovně. Ujišťujeme vás, které vaše webová aplikace zůstává ve **volné** vrstvy, pokud ji výslovně nezměníte tooa jinou cenovou úroveň.

> [!NOTE]
> jste vytvořili pomocí rozhraní příkazového řádku Azure Hello webová aplikace běží v **volné** vrstvy, která umožňuje pouze jednu instanci sdíleného virtuálního počítače s kvótou prostředků. Další informace o možnostech poskytovaných na úrovni **Free** naleznete v tématu [Omezení služby App Service](../azure-subscription-service-limits.md#app-service-limits).
> 
> 

## <a name="authenticate-your-users"></a>Ověřování uživatelů
Nyní si ukážeme, jak je snadné aplikace tooyour tooadd ověřování (podrobnější informace naleznete v [ověřování/autorizace služby App Service](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. V okně portálu hello pro vaši aplikaci, které jste právě otevřeli, klikněte na tlačítko **nastavení** > **ověřování / autorizace**.  
    ![Ověřování – okno nastavení](./media/app-service-web-get-started/aad-login-settings.png)
2. Klikněte na tlačítko **na** tooturn na ověřování.  
3. V části **Zprostředkovatelé ověřování** klikněte na možnost **Azure Active Directory**.  
    ![Ověřování – výběr možnosti Azure AD](./media/app-service-web-get-started/aad-login-config.png)
4. V hello **nastavení Azure Active Directory** okně klikněte na tlačítko **Express**, pak klikněte na tlačítko **OK**. Hello výchozí nastavení vytvoří novou aplikaci Azure AD ve výchozím adresáři.  
    ![Ověřování – expresní konfigurace](./media/app-service-web-get-started/aad-login-express.png)
5. Klikněte na **Uložit**.  
    ![Ověřování – uložení konfigurace](./media/app-service-web-get-started/aad-login-save.png)
   
    Po úspěšné hello změnu uvidíte hello oznámení zelený zvonek indikující, společně s přátelskou zprávou.
6. Zpět v hello okno portálu aplikace klikněte na hello **URL** propojení (nebo **Procházet** v řádku nabídek hello). Hello odkaz je adresa protokolu HTTP.  
    ![Ověřování – procházet tooURL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Ale po otevření aplikace hello na nové kartě, hello několikrát URL pole přesměrování a skončí u vaší aplikace s adresou protokolu HTTPS. Vidíte, že jste již přihlášen tooyour předplatného Azure a jste automaticky ověřeni v aplikaci hello.  
    ![Ověřování – uživatel je přihlášen](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Takže pokud nyní spustíte neověřenou relaci v jiném prohlížeči, uvidíte přihlašovací obrazovku, když přejdete toohello stejnou adresu URL.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
    Pokud jste ještě nikdy nepracovali se službou Azure Active Directory, výchozí adresář pravděpodobně nebude obsahovat žádné uživatele Azure AD. V takovém případě pravděpodobně hello jediný účet, zde je hello účet Microsoft s předplatným Azure. Který má proto byly automaticky přihlášeni v aplikaci toohello v hello stejný prohlížeč dříve.
    Můžete vytvořit tento stejný toolog účet Microsoft v této přihlašovací stránce.

Blahopřejeme, nyní ověřujete všechny přenosy tooyour webové aplikace.

Může dojít k tomu v hello **ověřování / autorizace** okno, které lze provádět mnoho dalších úkonů, například:

* Povolení přihlášení prostřednictvím sociální sítě
* Povolení více možností přihlášení
* Změnit hello výchozí chování při první návštěvě uživatelů tooyour aplikace

Služba App Service poskytuje že řešení klíč pro některé běžné ověřování hello potřebuje, takže není nutné logiku ověřování hello tooprovide sami.
Další informace naleznete v tématu [Ověřování/autorizace služby App Service](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Automatické škálování aplikace na základě poptávky
Dále umožňuje škálování aplikaci tak, aby automaticky přizpůsobovala svou kapacitu toorespond toouser vyžádání (podrobnější informace naleznete v [škálování aplikace v Azure](web-sites-scale.md) a [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Stručně řečeno, webové aplikace lze škálovat dvěma způsoby:

* [Vertikální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Získání větší kapacity procesoru, větší paměti, většího místa na disku a speciálních funkcí, jako jsou vyhrazené virtuální počítače, vlastní domény a certifikáty, přípravné sloty, automatické škálování a další. Škálovat tak, že změníte hello cenovou úroveň plánu služby App Service, ke kterému aplikace patří.
* [Horizontální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): zvýšení hello počet instancí virtuálního počítače, které spuštění aplikace.
  Je možné škálovat na tooas mnoho jako 50 instancí, v závislosti na cenovou úroveň.

Nyní bez dalších okolků nastavíme automatické škálování.

1. Nejprve vertikálně navýšíme kapacitu tooenable automatické škálování. V okně portálu hello vaší aplikace, klikněte na tlačítko **nastavení** > **vertikálně navýšit kapacitu (plán služby App Service)**.  
    ![Vertikální navýšení kapacity – okno nastavení](./media/app-service-web-get-started/scale-up-settings.png)
2. Posouvání a vyberte hello **S1 Standard** vrstvy, hello nejnižší úroveň podporující automatické škálování (na snímku obrazovky zakroužkováno) a pak klikněte na tlačítko **vyberte**.  
    ![Vertikální navýšení kapacity – volba úrovně](./media/app-service-web-get-started/scale-up-select.png)
   
    Dokončili jste vertikální navýšení kapacity.
   
   > [!IMPORTANT]
   > Tato úroveň čerpá vaše bezplatné zkušební kredity. Pokud máte účet platím za použití, způsobuje účet tooyour poplatky.
   > 
   > 
3. Nyní nakonfigurujeme automatické škálování. V okně portálu hello vaší aplikace, klikněte na tlačítko **nastavení** > **horizontální navýšení kapacity (plán služby App Service)**.  
    ![Horizontální navýšení kapacity – okno nastavení](./media/app-service-web-get-started/scale-out-settings.png)
4. Změna **škálovat podle** příliš**procento využití procesoru**. Hello posuvníky pod rozevíracím hello se aktualizují odpovídajícím způsobem. Poté definujte rozsah položky **Instance** od **1** do **2** a položku **Cílový rozsah** od **40** do **80**. Proveďte zadáním hello polí nebo přesunutím hello posuvníky.  
    ![Horizontální navýšení kapacity – konfigurace automatického škálování](./media/app-service-web-get-started/scale-out-configure.png)
   
    Na základě této konfigurace vaše aplikace automaticky horizontálně navýší kapacitu, překročí-li využití procesoru 80 %, a sníží kapacitu, poklesne-li využití procesoru pod 40 %.
5. Klikněte na tlačítko **Uložit** v řádku nabídek hello.

Blahopřejeme, aplikace nyní provádí automatické škálování.

Může dojít k tomu v hello **nastavení škálování** okno, které lze provádět mnoho dalších úkonů, například:

* Ruční škálování tooa konkrétní počet instancí
* Škálování podle jiných metrik výkonu, například podle procenta paměti či fronty disku
* Přizpůsobení chování škálování při aktivaci pravidla výkonu
* Automatické škálování podle plánu
* Nastavení chování automatického škálování pro budoucí událost

Další informace o vertikálním navýšení kapacity aplikace naleznete v tématu [Vertikální navýšení kapacity aplikace v Azure](web-sites-scale.md). Další informace o horizontálním navýšení kapacity naleznete v tématu [Ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Doručování upozornění týkajících se aplikace
Teď, když je aplikace automatické škálování, co se stane, když se dosáhne hello maximálnímu počtu instancí (2) a procesor překročí žádoucí míru využití (80 %)?
Můžete nastavit upozornění (podrobnější informace naleznete v [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) tooinform o této situaci, můžete další vertikálně například/na vaši aplikaci. Nyní rychle nastavíme upozornění pro tento scénář.

1. V okně portálu hello vaší aplikace, klikněte na tlačítko **nástroje** > **výstrahy**.  
    ![Upozornění – okno nastavení](./media/app-service-web-get-started/alert-settings.png)
2. Klikněte na možnost **Přidat upozornění**. Potom v hello **prostředků** pole, vyberte hello prostředek, který končí **(serverfarms)**. Toto je váš plán služby App Service.  
    ![Upozornění – přidání upozornění pro plán služby App Service](./media/app-service-web-get-started/alert-add.png)
3. V položce **Název** zadejte možnost `CPU Maxed`, v položce **Metrika** zadejte možnost **Procento procesoru** a v položce **Prahová hodnota** zadejte možnost `90`. Poté vyberte možnost **Vlastníci, přispěvatelé a čtenáři e-mailu** a klikněte na tlačítko **OK**.   
    ![Upozornění – konfigurace upozornění](./media/app-service-web-get-started/alert-configure.png)
   
    Jakmile Azure dokončí vytváření upozornění hello, se zobrazí v hello **výstrahy** okno.  
    ![Upozornění – zobrazení po dokončení](./media/app-service-web-get-started/alert-done.png)

Blahopřejeme, nyní vám jsou doručována upozornění.

Toto nastavení upozornění kontroluje využití procesoru každých pět minut. Pokud toto číslo překročí 90 %, obdržíte společně se všemi oprávněnými uživateli e-mailové upozornění. toosee každého, kdo je autorizovaný tooreceive hello výstrahy, přejděte zpět toohello okno portálu aplikace a klikněte na tlačítko hello **přístup** tlačítko.  
![Zobrazení příjemců upozornění](./media/app-service-web-get-started/alert-rbac.png)

Měli byste vidět, který **správci předplatného** jsou již hello **vlastníka** aplikace hello. Tato skupina bude zahrnovat vás, pokud jste správce účtu hello předplatného Azure (například svého zkušebního předplatného). Další informace o řízení přístupu na základě role Azure naleznete v tématu [Řízení přístupu na základě role Azure](../active-directory/role-based-access-control-configure.md).

> [!NOTE]
> Pravidla upozornění jsou funkcí platformy Azure. Další informace naleznete v tématu [Doručování oznámení o upozorněních](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).
> 
> 

## <a name="next-steps"></a>Další kroky
Na váš způsob tooconfigure hello výstrahu, může dojít k tomu bohatou sadu nástrojů v hello **nástroje** okno. Zde můžete řešit problémy, sledovat výkon, testovat ohrožení zabezpečení, spravovat prostředky, pracovat s konzolou hello virtuálních počítačů a přidávat užitečná rozšíření. Doporučujeme vám tooclick na každý z těchto toodiscover hello jednoduchými, avšak výkonnými nástrojům máte na dosah.

Zjistěte, jak toodo další nasazené aplikace. Následující výčet není vyčerpávající:

* [Zakoupení a konfigurace vlastního názvu domény](custom-dns-web-site-buydomains-web-app.md) – Kupte pro svou webovou aplikaci atraktivní doménu namísto domény *.azurewebsites.net. Můžete také použít doménu, kterou již máte.
* [Nastavení přípravných prostředí](web-sites-staged-publishing.md) -nasazení vaší aplikace tooa pracovní URL než ji umístíte do produkce. S jistotou aktualizujte svou živou webovou aplikaci. Nastavte propracované řešení DevOps s více nasazovacími sloty.
* [Nastavení průběžného nasazování](app-service-continuous-deployment.md) – Integrujte nasazení aplikace do systému správy zdrojů. Proveďte nasazení do Azure s každou potvrzenou změnou.
* [Přístup k místním prostředkům](web-sites-hybrid-connection-get-started.md) – Získejte přístup k stávající místní databázi nebo systému CRM.
* [Zálohování aplikace](web-sites-backup.md) – Nastavte zálohování a obnovení webové aplikace. Připravte se na neočekávaná selhání a zotavte se z nich.
* [Povolení diagnostických protokolů](web-sites-enable-diagnostic-log.md) -zaznamená hello čtení služby IIS z Azure nebo trasování aplikací. Přečtěte si je pomocí streamu, stáhněte si je nebo je přizpůsobte pro službu [Application Insights](../application-insights/app-insights-overview.md) k analýze na klíč.
* [Kontrola chyb zabezpečení aplikace](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
  kontrolovat vaše webová aplikace zranitelná moderními hrozbami pomocí služby poskytované společností [Tinfoil Security](https://www.tinfoilsecurity.com/).
* [Spouštění úloh na pozadí](../azure-functions/functions-overview.md) – Spouštějte úlohy zpracování dat, vytváření sestav atd.
* [Seznámení s fungováním služby App Service](../app-service/app-service-how-works-readme.md)

