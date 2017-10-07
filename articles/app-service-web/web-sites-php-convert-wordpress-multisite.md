---
title: aaaConvert tooMultisite WordPress v Azure App Service
description: "Zjistěte, jak tootake stávající webovou aplikaci WordPress vytvořena prostřednictvím Galerie hello v Azure a převeďte ho tooWordPress nasazení ve více lokalitách"
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: fe52dbf4-179c-42f1-adf9-d6a9af920c39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 1153f0a8043de875f081704cd0a124776758878c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-wordpress-toomultisite-in-azure-app-service"></a><span data-ttu-id="2b018-103">Převést tooMultisite WordPress v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2b018-103">Convert WordPress tooMultisite in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="2b018-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2b018-104">Overview</span></span>
<span data-ttu-id="2b018-105">*Podle [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies, Inc.][ms-open-tech]*</span><span class="sxs-lookup"><span data-stu-id="2b018-105">*By [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*</span></span>

<span data-ttu-id="2b018-106">V tomto kurzu se dozvíte, jak tootake stávající webovou aplikaci WordPress vytvořena prostřednictvím hello Galerie ve službě Azure a převést jej do WordPress ve víc lokalitách nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="2b018-106">In this tutorial, you will learn how tootake an existing WordPress web app created through hello gallery in Azure and convert it into a WordPress Multisite install.</span></span> <span data-ttu-id="2b018-107">Kromě toho se dozvíte, jak tooassign vlastní domény tooeach Dobrý den podřízené weby v rámci vaší instalace.</span><span class="sxs-lookup"><span data-stu-id="2b018-107">Additionally, you will learn how tooassign a custom domain tooeach of hello subsites within your install.</span></span>

<span data-ttu-id="2b018-108">Předpokládá se, že máte existující instalaci WordPress.</span><span class="sxs-lookup"><span data-stu-id="2b018-108">It is assumed that you have an existing installation of WordPress.</span></span> <span data-ttu-id="2b018-109">Pokud ho použít nechcete, postupujte podle pokynů hello uvedených v [vytvoření webu WordPress z Galerie hello v Azure][website-from-gallery].</span><span class="sxs-lookup"><span data-stu-id="2b018-109">If you do not, please follow hello guidance provided in [Create a WordPress web site from hello gallery in Azure][website-from-gallery].</span></span>

<span data-ttu-id="2b018-110">Převod existující WordPress tooMultisite instalace jedné lokality je obecně velmi jednoduché a řadu hello zde počáteční kroky pochází přímo z hello [vytvořit síť A] [ wordpress-codex-create-a-network] stránky na hello [WordPress Codex](http://codex.wordpress.org).</span><span class="sxs-lookup"><span data-stu-id="2b018-110">Converting an existing WordPress single site install tooMultisite is generally fairly simple, and many of hello initial steps here come straight from hello [Create A Network][wordpress-codex-create-a-network] page on hello [WordPress Codex](http://codex.wordpress.org).</span></span>

<span data-ttu-id="2b018-111">Můžeme začít.</span><span class="sxs-lookup"><span data-stu-id="2b018-111">Let's get started.</span></span>

## <a name="allow-multisite"></a><span data-ttu-id="2b018-112">Povolit nasazení ve více lokalitách</span><span class="sxs-lookup"><span data-stu-id="2b018-112">Allow Multisite</span></span>
<span data-ttu-id="2b018-113">Je nutné nejprve tooenable nasazení ve více lokalitách prostřednictvím hello `wp-config.php` soubor s hello **WP\_povolit\_nasazení ve více LOKALITÁCH** konstantní.</span><span class="sxs-lookup"><span data-stu-id="2b018-113">You first need tooenable Multisite through hello `wp-config.php` file with hello **WP\_ALLOW\_MULTISITE** constant.</span></span> <span data-ttu-id="2b018-114">Existují dvě metody tooedit souborů vaší webové aplikace: hello je nejdřív prostřednictvím FTP a hello druhý prostřednictvím Git.</span><span class="sxs-lookup"><span data-stu-id="2b018-114">There are two methods tooedit your web app files: hello first is through FTP, and hello second through Git.</span></span> <span data-ttu-id="2b018-115">Pokud jste obeznámeni s postupy toosetup žádnou z těchto metod naleznete toohello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="2b018-115">If you are unfamiliar with how toosetup either of these methods, please refer toohello following tutorials:</span></span>

* <span data-ttu-id="2b018-116">[Webové stránky PHP s MySQL a FTP][website-w-mysql-and-ftp-ftp-setup]</span><span class="sxs-lookup"><span data-stu-id="2b018-116">[PHP web site with MySQL and FTP][website-w-mysql-and-ftp-ftp-setup]</span></span>
* <span data-ttu-id="2b018-117">[Webové stránky PHP s MySQL a Git][website-w-mysql-and-git-git-setup]</span><span class="sxs-lookup"><span data-stu-id="2b018-117">[PHP web site with MySQL and Git][website-w-mysql-and-git-git-setup]</span></span>

<span data-ttu-id="2b018-118">Otevřete hello `wp-config.php` soubor s hello editor dle vašeho výběru a přidejte následující hello výše hello `/* That's all, stop editing! Happy blogging. */` řádku.</span><span class="sxs-lookup"><span data-stu-id="2b018-118">Open hello `wp-config.php` file with hello editor of your choosing and add hello following above hello `/* That's all, stop editing! Happy blogging. */` line.</span></span>

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

<span data-ttu-id="2b018-119">Být aby toosave hello soubor a nahrajte ho back toohello serveru!</span><span class="sxs-lookup"><span data-stu-id="2b018-119">Be sure toosave hello file and upload it back toohello server!</span></span>

## <a name="network-setup"></a><span data-ttu-id="2b018-120">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="2b018-120">Network Setup</span></span>
<span data-ttu-id="2b018-121">Přihlaste se toohello *wp správce* oblast webové aplikace a měla by se zobrazit novou položku pod hello **nástroje** nabídky s názvem **nastavení sítě**.</span><span class="sxs-lookup"><span data-stu-id="2b018-121">Log in toohello *wp-admin* area of your web app and you should see a new item under hello **Tools** menu called **Network Setup**.</span></span> <span data-ttu-id="2b018-122">Klikněte na tlačítko **nastavení sítě** a vyplňte hello podrobnosti o síti.</span><span class="sxs-lookup"><span data-stu-id="2b018-122">Click **Network Setup** and fill in hello details of your network.</span></span>

![Obrazovka nastavení sítě][wordpress-network-setup]

<span data-ttu-id="2b018-124">Tento kurz používá hello *podadresářích* lokality schématu, protože by měla vždycky fungovat a budeme zadávat vlastní domény pro každý podřízený web později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="2b018-124">This tutorial uses hello *Sub-directories* site schema because it should always work, and we will be setting up custom domains for each subsite later in hello tutorial.</span></span> <span data-ttu-id="2b018-125">Však bude možné toosetup subdoména nainstalovat, pokud namapovat doménu prostřednictvím hello [portálu Azure](https://portal.azure.com) a instalační program zástupný znak DNS správně.</span><span class="sxs-lookup"><span data-stu-id="2b018-125">However, it should be possible toosetup a subdomain install if you map a domain through hello [Azure Portal](https://portal.azure.com) and setup wildcard DNS properly.</span></span>

<span data-ttu-id="2b018-126">Další informace o subdomény vs podadresáře nastavení najdete v části hello [typy nasazení ve více lokalitách sítě] [ wordpress-codex-types-of-networks] článek na hello WordPress Codex.</span><span class="sxs-lookup"><span data-stu-id="2b018-126">For more information on sub-domain vs sub-directory setups see hello [Types of multisite network][wordpress-codex-types-of-networks] article on hello WordPress Codex.</span></span>

## <a name="enable-hello-network"></a><span data-ttu-id="2b018-127">Povolit hello sítě</span><span class="sxs-lookup"><span data-stu-id="2b018-127">Enable hello Network</span></span>
<span data-ttu-id="2b018-128">Hello sítě je nyní nakonfigurována v hello databázi, ale není jeden zbývající krok tooenable hello síťové funkce.</span><span class="sxs-lookup"><span data-stu-id="2b018-128">hello network is now configured in hello database, but there is one remaining step tooenable hello network functionality.</span></span> <span data-ttu-id="2b018-129">Dokončení hello `wp-config.php` nastavení a ujistěte se, `web.config` správně směruje každou lokalitu.</span><span class="sxs-lookup"><span data-stu-id="2b018-129">Finalize hello `wp-config.php` settings and ensure `web.config` properly routes each site.</span></span>

<span data-ttu-id="2b018-130">Po kliknutí na hello **nainstalovat** na hello tlačítko *nastavení sítě* stránce WordPress pokusí tooupdate hello `wp-config.php` a `web.config` soubory.</span><span class="sxs-lookup"><span data-stu-id="2b018-130">After clicking hello **Install** button on hello *Network Setup* page, WordPress will attempt tooupdate hello `wp-config.php` and `web.config` files.</span></span> <span data-ttu-id="2b018-131">Ale byste měli vždy zkontrolovat hello soubory tooensure hello aktualizace byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="2b018-131">However, you should always check hello files tooensure hello updates were successful.</span></span> <span data-ttu-id="2b018-132">Pokud ne, tato obrazovka bude představovat hello potřebné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2b018-132">If not, this screen will present you with hello necessary updates.</span></span> <span data-ttu-id="2b018-133">Upravit a uložit soubory hello.</span><span class="sxs-lookup"><span data-stu-id="2b018-133">Edit and save hello files.</span></span>

<span data-ttu-id="2b018-134">Po provedení těchto aktualizací, budete potřebovat toolog se a přihlaste zpět do řídicího panelu webové části správce hello.</span><span class="sxs-lookup"><span data-stu-id="2b018-134">After making these updates you will need toolog out and log back into hello wp-admin dashboard.</span></span>

<span data-ttu-id="2b018-135">Došlo by teď měly být další nabídky na panelu Správce hello s názvem bez přípony **osobní weby**.</span><span class="sxs-lookup"><span data-stu-id="2b018-135">There should now be an additional menu on hello admin bar labeled **My Sites**.</span></span> <span data-ttu-id="2b018-136">Tato nabídka vám umožní toocontrol nové síti přes hello **správce sítě** řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="2b018-136">This menu allows you toocontrol your new network through hello **Network Admin** dashboard.</span></span>

## <a name="adding-custom-domains"></a><span data-ttu-id="2b018-137">Přidání vlastních domén</span><span class="sxs-lookup"><span data-stu-id="2b018-137">Adding custom domains</span></span>
<span data-ttu-id="2b018-138">Hello [WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] modul plug-in je uloženy tooadd vlastní domény tooany lokality ve vaší síti.</span><span class="sxs-lookup"><span data-stu-id="2b018-138">hello [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] plugin makes it a breeze tooadd custom domains tooany site in your network.</span></span> <span data-ttu-id="2b018-139">Pro modul plug-in toooperate hello správně, je třeba splnit toodo některé další nastavení na hello portálu a také u doménového registrátora.</span><span class="sxs-lookup"><span data-stu-id="2b018-139">In order for hello plugin toooperate properly, you need toodo some additional setup on hello Portal, and also at your domain registrar.</span></span>

## <a name="enable-domain-mapping-toohello-web-app"></a><span data-ttu-id="2b018-140">Povolit domény mapování toohello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2b018-140">Enable domain mapping toohello web app</span></span>
<span data-ttu-id="2b018-141">Hello **volné** [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plán režim nepodporuje přidávání tooWeb vlastní domény aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b018-141">hello **Free** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plan mode does not support adding custom domains tooWeb Apps.</span></span> <span data-ttu-id="2b018-142">Budete potřebovat tooswitch příliš**sdílené** nebo **standardní** režimu.</span><span class="sxs-lookup"><span data-stu-id="2b018-142">You will need tooswitch too**Shared** or **Standard** mode.</span></span> <span data-ttu-id="2b018-143">toodo toto:</span><span class="sxs-lookup"><span data-stu-id="2b018-143">toodo this:</span></span>

* <span data-ttu-id="2b018-144">Přihlaste se toohello portálu Azure a vyhledejte vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b018-144">Log in toohello Azure Portal and locate your web app.</span></span> 
* <span data-ttu-id="2b018-145">Klikněte na hello **škálovat** kartě v **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="2b018-145">Click on hello **Scale up** tab in **Settings**.</span></span>
* <span data-ttu-id="2b018-146">V části **Obecné**, vyberte buď *SDÍLENÉ* nebo *standardní*</span><span class="sxs-lookup"><span data-stu-id="2b018-146">Under **General**, select either *SHARED* or *STANDARD*</span></span>
* <span data-ttu-id="2b018-147">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="2b018-147">Click **Save**</span></span>

<span data-ttu-id="2b018-148">Může se zobrazí zpráva s dotazem, tooverify hello změnu a informovanost o tom vaší webové aplikace může nyní zpoplatněná náklady, v závislosti na využití a hello ostatní konfigurace, které nastavíte.</span><span class="sxs-lookup"><span data-stu-id="2b018-148">You may receive a message asking you tooverify hello change and acknowledge your web app may now incur a cost, depending upon usage and hello other configuration you set.</span></span>

<span data-ttu-id="2b018-149">Bude trvat několik sekund tooprocess hello nové nastavení, takže teď je vhodná doba toostart, nastavení vaší domény.</span><span class="sxs-lookup"><span data-stu-id="2b018-149">It takes a few seconds tooprocess hello new settings, so now is a good time toostart setting up your domain.</span></span>

## <a name="verify-your-domain"></a><span data-ttu-id="2b018-150">Ověřte svoji doménu</span><span class="sxs-lookup"><span data-stu-id="2b018-150">Verify your domain</span></span>
<span data-ttu-id="2b018-151">Před Azure Web Apps vám umožní toomap lokalitu toohello domény, musíte nejprve tooverify, abyste měli hello autorizace toomap hello domény.</span><span class="sxs-lookup"><span data-stu-id="2b018-151">Before Azure Web Apps will allow you toomap a domain toohello site, you first need tooverify that you have hello authorization toomap hello domain.</span></span> <span data-ttu-id="2b018-152">toodo Ano, musíte přidat nový záznam CNAME tooyour záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="2b018-152">toodo so, you must add a new CNAME record tooyour DNS entry.</span></span>

* <span data-ttu-id="2b018-153">Přihlášení správce DNS tooyour domény</span><span class="sxs-lookup"><span data-stu-id="2b018-153">Log in tooyour domain's DNS manager</span></span>
* <span data-ttu-id="2b018-154">Vytvořit nový záznam CNAME *awverify*</span><span class="sxs-lookup"><span data-stu-id="2b018-154">Create a new CNAME *awverify*</span></span>
* <span data-ttu-id="2b018-155">Bod *awverify* příliš*awverify. YOUR_DOMAIN.azurewebsites.NET*</span><span class="sxs-lookup"><span data-stu-id="2b018-155">Point *awverify* too*awverify.YOUR_DOMAIN.azurewebsites.net*</span></span>

<span data-ttu-id="2b018-156">Ho může trvat nějakou dobu hello DNS změny toogo úplné platit, takže pokud hello následující kroky nefungují okamžitě, přejděte zkontrolujte cup kávy, pak se vraťte a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="2b018-156">It may take some time for hello DNS changes toogo into full effect, so if hello following steps do not work immediately, go make a cup of coffee, then come back and try again.</span></span>

## <a name="add-hello-domain-toohello-web-app"></a><span data-ttu-id="2b018-157">Přidání hello domény toohello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2b018-157">Add hello domain toohello web app</span></span>
<span data-ttu-id="2b018-158">Klikněte na tlačítko návratový tooyour webové aplikace prostřednictvím portálu Azure, hello **nastavení**a potom klikněte na **vlastní domény a SSL**.</span><span class="sxs-lookup"><span data-stu-id="2b018-158">Return tooyour web app through hello Azure Portal, click **Settings**, and then click **Custom domains and SSL**.</span></span>

<span data-ttu-id="2b018-159">Když hello *nastavení SSL* se zobrazí, zobrazí se pole hello, kde budete zadávat všechny hello domén, které chcete tooassign tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b018-159">When hello *SSL settings* are displayed, you will see hello fields where you will input all hello domains which you wish tooassign tooyour web app.</span></span> <span data-ttu-id="2b018-160">Pokud zde není uveden domény, nebude k dispozici pro mapování uvnitř WordPress, bez ohledu na to, jak hello domény DNS je instalační program.</span><span class="sxs-lookup"><span data-stu-id="2b018-160">If a domain is not listed here, it will not be available for mapping inside WordPress, regardless of how hello domain DNS is setup.</span></span>

![Dialogové okno vlastní domény spravovat][wordpress-manage-domains]

<span data-ttu-id="2b018-162">Po zadání vaší domény do textového pole pro hello, ověří Azure hello záznam CNAME, který jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2b018-162">After typing your domain into hello text box, Azure will verify hello CNAME record you created previously.</span></span> <span data-ttu-id="2b018-163">Pokud hello DNS není plně rozšíří, se zobrazí červený indikátoru.</span><span class="sxs-lookup"><span data-stu-id="2b018-163">If hello DNS has not fully propagated, a red indicator will show.</span></span> <span data-ttu-id="2b018-164">Pokud bylo úspěšné, zobrazí zelené zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="2b018-164">If it was successful, you will see a green checkmark.</span></span> 

<span data-ttu-id="2b018-165">Poznamenejte si hello, IP adresa je uvedena v dolní části hello hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2b018-165">Take note of hello IP Address listed at hello bottom of hello dialog.</span></span> <span data-ttu-id="2b018-166">Je nutné tento toosetup hello záznam pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="2b018-166">You will need this toosetup hello A record for your domain.</span></span>

## <a name="setup-hello-domain-a-record"></a><span data-ttu-id="2b018-167">Instalační program hello záznam A domény</span><span class="sxs-lookup"><span data-stu-id="2b018-167">Setup hello domain A record</span></span>
<span data-ttu-id="2b018-168">Pokud hello další kroky byly úspěšné, můžete nyní může přiřadit hello domény tooyour webové aplikace Azure prostřednictvím záznam DNS A.</span><span class="sxs-lookup"><span data-stu-id="2b018-168">If hello other steps were successful, you may now assign hello domain tooyour Azure web app through a DNS A record.</span></span> 

<span data-ttu-id="2b018-169">Je zde důležité toonote, webové aplikace Azure přijali CNAME a záznamy A, ale můžete *musí* použít mapování A záznamů tooenable správné doméně.</span><span class="sxs-lookup"><span data-stu-id="2b018-169">It is important toonote here that Azure web apps accept both CNAME and A records, however you *must* use an A record tooenable proper domain mapping.</span></span> <span data-ttu-id="2b018-170">Záznam CNAME nemůže být předán tooanother CNAME, který je co Azure vytvořili pro vás s YOUR_DOMAIN.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="2b018-170">A CNAME cannot be forwarded tooanother CNAME, which is what Azure created for you with YOUR_DOMAIN.azurewebsites.net.</span></span>

<span data-ttu-id="2b018-171">Pomocí IP adresy hello hello v předchozím kroku, vrátí tooyour Správce DNS a hello instalační program záznamů toopoint toothat IP.</span><span class="sxs-lookup"><span data-stu-id="2b018-171">Using hello IP address from hello previous step, return tooyour DNS manager and setup hello A record toopoint toothat IP.</span></span>

## <a name="install-and-setup-hello-plugin"></a><span data-ttu-id="2b018-172">Instalace a nastavení hello modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="2b018-172">Install and setup hello plugin</span></span>
<span data-ttu-id="2b018-173">Nasazení ve více lokalitách WordPress aktuálně nemá předdefinovanou metodu vlastní domény toomap.</span><span class="sxs-lookup"><span data-stu-id="2b018-173">WordPress Multisite currently does not have a built-in method toomap custom domains.</span></span> <span data-ttu-id="2b018-174">Existuje však o modul plug-in názvem [WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] který přidává funkce hello za vás.</span><span class="sxs-lookup"><span data-stu-id="2b018-174">However, there is a plugin called [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] that adds hello functionality for you.</span></span> <span data-ttu-id="2b018-175">V protokolu v části správce sítě toohello vašeho webu a nainstalujte hello **WordPress MU domény mapování** modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="2b018-175">Log in toohello Network Admin portion of your site and install hello **WordPress MU Domain Mapping** plugin.</span></span>

<span data-ttu-id="2b018-176">Po instalaci a aktivaci hello modul plug-in, navštivte **nastavení** > **domény mapování** tooconfigure hello modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="2b018-176">After installing and activating hello plugin, visit **Settings** > **Domain Mapping** tooconfigure hello plugin.</span></span> <span data-ttu-id="2b018-177">V hello první textovému poli *IP adresa serveru*, vstupní hello adresa IP použitá toosetup hello záznam pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="2b018-177">In hello first textbox, *Server IP Address*, input hello IP Address you used toosetup hello A record for hello domain.</span></span> <span data-ttu-id="2b018-178">Nastavte libovolné *možnosti domény* požadavky (hello výchozí hodnoty jsou často) a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2b018-178">Set any *Domain Options* you desire (hello defaults are often fine) and click **Save**.</span></span>

## <a name="map-hello-domain"></a><span data-ttu-id="2b018-179">Mapa hello domény</span><span class="sxs-lookup"><span data-stu-id="2b018-179">Map hello domain</span></span>
<span data-ttu-id="2b018-180">Navštivte hello **řídicí panel** pro lokalitu hello chcete toomap hello domény.</span><span class="sxs-lookup"><span data-stu-id="2b018-180">Visit hello **Dashboard** for hello site you wish toomap hello domain to.</span></span> <span data-ttu-id="2b018-181">Klikněte na **nástroje** > **domény mapování** a typ hello nové domény do textového pole hello a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2b018-181">Click on **Tools** > **Domain Mapping** and type hello new domain into hello textbox and click **Add**.</span></span>

<span data-ttu-id="2b018-182">Ve výchozím nastavení bude nové domény hello doméně lokality přepsaná toohello generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="2b018-182">By default, hello new domain will be rewritten toohello autogenerated site domain.</span></span> <span data-ttu-id="2b018-183">Pokud chcete toohave všechny přenosy odesílané toohello nové domény, zkontrolujte hello *primární domény pro tento blog* pole před uložením.</span><span class="sxs-lookup"><span data-stu-id="2b018-183">If you want toohave all traffic sent toohello new domain, check hello *Primary domain for this blog* box before saving.</span></span> <span data-ttu-id="2b018-184">Můžete přidat libovolný počet domén tooa lokality, ale může být pouze jeden primární.</span><span class="sxs-lookup"><span data-stu-id="2b018-184">You can add an unlimited number of domains tooa site, but  only one can be primary.</span></span>

## <a name="do-it-again"></a><span data-ttu-id="2b018-185">Provést akci</span><span class="sxs-lookup"><span data-stu-id="2b018-185">Do it again</span></span>
<span data-ttu-id="2b018-186">Webové aplikace Azure umožňují tooadd neomezený počet domén tooa webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b018-186">Azure Web Apps allow you tooadd an unlimited number of domains tooa web app.</span></span> <span data-ttu-id="2b018-187">tooadd jiné domény, budete potřebovat tooexecute hello **ověřte svoji doménu** a **nastavení domény A záznam hello** oddíly pro každou doménu.</span><span class="sxs-lookup"><span data-stu-id="2b018-187">tooadd another domain you will need tooexecute hello **Verify your domain** and **Setup hello domain A record** sections for each domain.</span></span>    

> [!NOTE]
> <span data-ttu-id="2b018-188">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="2b018-188">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="2b018-189">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="2b018-189">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="2b018-190">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="2b018-190">What's changed</span></span>
* <span data-ttu-id="2b018-191">Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="2b018-191">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png


