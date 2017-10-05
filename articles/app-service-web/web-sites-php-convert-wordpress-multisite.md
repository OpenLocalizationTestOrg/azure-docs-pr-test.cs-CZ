---
title: "Převést WordPress na nasazení ve více lokalitách ve službě Azure App Service"
description: "Informace o provedení stávající webovou aplikaci WordPress vytvořena prostřednictvím Galerie v Azure a převést jej do nasazení ve více lokalitách WordPress"
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
ms.openlocfilehash: 4a15fb5e97d2ca57e5883c07651c372c54021c92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a><span data-ttu-id="1dc11-103">Převést WordPress na nasazení ve více lokalitách ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1dc11-103">Convert WordPress to Multisite in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="1dc11-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1dc11-104">Overview</span></span>
<span data-ttu-id="1dc11-105">*Podle [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies, Inc.][ms-open-tech]*</span><span class="sxs-lookup"><span data-stu-id="1dc11-105">*By [Ben Lobaugh][ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*</span></span>

<span data-ttu-id="1dc11-106">V tomto kurzu se dozvíte, jak využít stávající webovou aplikaci WordPress vytvořena prostřednictvím Galerie v Azure a převádět je do nasazení ve více lokalitách WordPress instalace.</span><span class="sxs-lookup"><span data-stu-id="1dc11-106">In this tutorial, you will learn how to take an existing WordPress web app created through the gallery in Azure and convert it into a WordPress Multisite install.</span></span> <span data-ttu-id="1dc11-107">Kromě toho se dozvíte, jak přiřadit vlastní doménu všechny podřízené weby v rámci vaší instalace.</span><span class="sxs-lookup"><span data-stu-id="1dc11-107">Additionally, you will learn how to assign a custom domain to each of the subsites within your install.</span></span>

<span data-ttu-id="1dc11-108">Předpokládá se, že máte existující instalaci WordPress.</span><span class="sxs-lookup"><span data-stu-id="1dc11-108">It is assumed that you have an existing installation of WordPress.</span></span> <span data-ttu-id="1dc11-109">Pokud ho použít nechcete, postupujte podle pokynů uvedených v [vytvoření webu WordPress z Galerie v Azure][website-from-gallery].</span><span class="sxs-lookup"><span data-stu-id="1dc11-109">If you do not, please follow the guidance provided in [Create a WordPress web site from the gallery in Azure][website-from-gallery].</span></span>

<span data-ttu-id="1dc11-110">Převádění existující WordPress instalovat jedné lokality do nasazení ve více lokalitách je obecně docela jednoduchá a řadu počáteční kroky pochází přímo z [vytvořit síť A] [ wordpress-codex-create-a-network] stránky na [ WordPress Codex](http://codex.wordpress.org).</span><span class="sxs-lookup"><span data-stu-id="1dc11-110">Converting an existing WordPress single site install to Multisite is generally fairly simple, and many of the initial steps here come straight from the [Create A Network][wordpress-codex-create-a-network] page on the [WordPress Codex](http://codex.wordpress.org).</span></span>

<span data-ttu-id="1dc11-111">Můžeme začít.</span><span class="sxs-lookup"><span data-stu-id="1dc11-111">Let's get started.</span></span>

## <a name="allow-multisite"></a><span data-ttu-id="1dc11-112">Povolit nasazení ve více lokalitách</span><span class="sxs-lookup"><span data-stu-id="1dc11-112">Allow Multisite</span></span>
<span data-ttu-id="1dc11-113">Nejprve musíte povolit nasazení ve více lokalitách pomocí `wp-config.php` soubor s **WP\_povolit\_nasazení ve více LOKALITÁCH** konstantní.</span><span class="sxs-lookup"><span data-stu-id="1dc11-113">You first need to enable Multisite through the `wp-config.php` file with the **WP\_ALLOW\_MULTISITE** constant.</span></span> <span data-ttu-id="1dc11-114">Existují dvě metody k úpravě souborů webové aplikace: první je přes FTP a druhá prostřednictvím Git.</span><span class="sxs-lookup"><span data-stu-id="1dc11-114">There are two methods to edit your web app files: the first is through FTP, and the second through Git.</span></span> <span data-ttu-id="1dc11-115">Pokud jste obeznámeni s jak nastavit některé z těchto metod, naleznete v následujících kurzech:</span><span class="sxs-lookup"><span data-stu-id="1dc11-115">If you are unfamiliar with how to setup either of these methods, please refer to the following tutorials:</span></span>

* <span data-ttu-id="1dc11-116">[Webové stránky PHP s MySQL a FTP][website-w-mysql-and-ftp-ftp-setup]</span><span class="sxs-lookup"><span data-stu-id="1dc11-116">[PHP web site with MySQL and FTP][website-w-mysql-and-ftp-ftp-setup]</span></span>
* <span data-ttu-id="1dc11-117">[Webové stránky PHP s MySQL a Git][website-w-mysql-and-git-git-setup]</span><span class="sxs-lookup"><span data-stu-id="1dc11-117">[PHP web site with MySQL and Git][website-w-mysql-and-git-git-setup]</span></span>

<span data-ttu-id="1dc11-118">Otevřete `wp-config.php` pomocí zvoleného editoru souborů a přidejte následující výše `/* That's all, stop editing! Happy blogging. */` řádku.</span><span class="sxs-lookup"><span data-stu-id="1dc11-118">Open the `wp-config.php` file with the editor of your choosing and add the following above the `/* That's all, stop editing! Happy blogging. */` line.</span></span>

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

<span data-ttu-id="1dc11-119">Ujistěte se, soubor uložte a odešlete ji zpět k serveru!</span><span class="sxs-lookup"><span data-stu-id="1dc11-119">Be sure to save the file and upload it back to the server!</span></span>

## <a name="network-setup"></a><span data-ttu-id="1dc11-120">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="1dc11-120">Network Setup</span></span>
<span data-ttu-id="1dc11-121">Přihlaste se k *wp-admin* oblast webové aplikace a měla by se zobrazit novou položku pod **nástroje** nabídky s názvem **nastavení sítě**.</span><span class="sxs-lookup"><span data-stu-id="1dc11-121">Log in to the *wp-admin* area of your web app and you should see a new item under the **Tools** menu called **Network Setup**.</span></span> <span data-ttu-id="1dc11-122">Klikněte na tlačítko **nastavení sítě** a zadejte podrobnosti vaší sítě.</span><span class="sxs-lookup"><span data-stu-id="1dc11-122">Click **Network Setup** and fill in the details of your network.</span></span>

![Obrazovka nastavení sítě][wordpress-network-setup]

<span data-ttu-id="1dc11-124">Tento kurz používá *podadresářích* lokality schématu, protože by měla vždycky fungovat a budeme zadávat vlastní domény pro každý podřízený web později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-124">This tutorial uses the *Sub-directories* site schema because it should always work, and we will be setting up custom domains for each subsite later in the tutorial.</span></span> <span data-ttu-id="1dc11-125">Ale musí být možné instalační program subdoména nainstalovat, pokud namapujete domény prostřednictvím [portálu Azure](https://portal.azure.com) a instalační program zástupný znak DNS správně.</span><span class="sxs-lookup"><span data-stu-id="1dc11-125">However, it should be possible to setup a subdomain install if you map a domain through the [Azure Portal](https://portal.azure.com) and setup wildcard DNS properly.</span></span>

<span data-ttu-id="1dc11-126">Další informace o subdomény vs podadresáře nastavení najdete v článku [typy nasazení ve více lokalitách sítě] [ wordpress-codex-types-of-networks] článek na WordPress Codex.</span><span class="sxs-lookup"><span data-stu-id="1dc11-126">For more information on sub-domain vs sub-directory setups see the [Types of multisite network][wordpress-codex-types-of-networks] article on the WordPress Codex.</span></span>

## <a name="enable-the-network"></a><span data-ttu-id="1dc11-127">Povolit síť</span><span class="sxs-lookup"><span data-stu-id="1dc11-127">Enable the Network</span></span>
<span data-ttu-id="1dc11-128">Síť je nyní nakonfigurována v databázi, ale je jeden zbývající krok k povolení funkce sítě.</span><span class="sxs-lookup"><span data-stu-id="1dc11-128">The network is now configured in the database, but there is one remaining step to enable the network functionality.</span></span> <span data-ttu-id="1dc11-129">Dokončení `wp-config.php` nastavení a ujistěte se, `web.config` správně směruje každou lokalitu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-129">Finalize the `wp-config.php` settings and ensure `web.config` properly routes each site.</span></span>

<span data-ttu-id="1dc11-130">Po kliknutí na tlačítko **nainstalovat** tlačítko *nastavení sítě* stránce WordPress bude pokus o aktualizaci `wp-config.php` a `web.config` soubory.</span><span class="sxs-lookup"><span data-stu-id="1dc11-130">After clicking the **Install** button on the *Network Setup* page, WordPress will attempt to update the `wp-config.php` and `web.config` files.</span></span> <span data-ttu-id="1dc11-131">Ale byste měli vždy zkontrolovat soubory, zajistěte, že aktualizace byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="1dc11-131">However, you should always check the files to ensure the updates were successful.</span></span> <span data-ttu-id="1dc11-132">Pokud ne, tato obrazovka bude představovat potřebné aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1dc11-132">If not, this screen will present you with the necessary updates.</span></span> <span data-ttu-id="1dc11-133">Upravit a uložit soubory.</span><span class="sxs-lookup"><span data-stu-id="1dc11-133">Edit and save the files.</span></span>

<span data-ttu-id="1dc11-134">Po provedení těchto aktualizací, které budete muset odhlaste se a přihlaste se zpět do řídicího panelu webové části správce.</span><span class="sxs-lookup"><span data-stu-id="1dc11-134">After making these updates you will need to log out and log back into the wp-admin dashboard.</span></span>

<span data-ttu-id="1dc11-135">Došlo by teď měly být další nabídky na panelu Správce s názvem bez přípony **osobní weby**.</span><span class="sxs-lookup"><span data-stu-id="1dc11-135">There should now be an additional menu on the admin bar labeled **My Sites**.</span></span> <span data-ttu-id="1dc11-136">Tato nabídka vám umožňuje řídit nové síti přes **správce sítě** řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-136">This menu allows you to control your new network through the **Network Admin** dashboard.</span></span>

## <a name="adding-custom-domains"></a><span data-ttu-id="1dc11-137">Přidání vlastních domén</span><span class="sxs-lookup"><span data-stu-id="1dc11-137">Adding custom domains</span></span>
<span data-ttu-id="1dc11-138">[WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] modulu plug-in umožňuje uloženy přidat vlastní domény k libovolné lokalitě v síti.</span><span class="sxs-lookup"><span data-stu-id="1dc11-138">The [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] plugin makes it a breeze to add custom domains to any site in your network.</span></span> <span data-ttu-id="1dc11-139">Aby modul plug-in ke správnému fungování budete muset provést některé další nastavení na portálu a také u doménového registrátora.</span><span class="sxs-lookup"><span data-stu-id="1dc11-139">In order for the plugin to operate properly, you need to do some additional setup on the Portal, and also at your domain registrar.</span></span>

## <a name="enable-domain-mapping-to-the-web-app"></a><span data-ttu-id="1dc11-140">Povolit mapování domény do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="1dc11-140">Enable domain mapping to the web app</span></span>
<span data-ttu-id="1dc11-141">**Volné** [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plán režim nepodporuje přidávání vlastních domén do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dc11-141">The **Free** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) plan mode does not support adding custom domains to Web Apps.</span></span> <span data-ttu-id="1dc11-142">Je nutné přepnout na **sdílené** nebo **standardní** režimu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-142">You will need to switch to **Shared** or **Standard** mode.</span></span> <span data-ttu-id="1dc11-143">Použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="1dc11-143">To do this:</span></span>

* <span data-ttu-id="1dc11-144">Přihlaste se k portálu Azure a vyhledejte vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dc11-144">Log in to the Azure Portal and locate your web app.</span></span> 
* <span data-ttu-id="1dc11-145">Klikněte na **škálovat** kartě v **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="1dc11-145">Click on the **Scale up** tab in **Settings**.</span></span>
* <span data-ttu-id="1dc11-146">V části **Obecné**, vyberte buď *SDÍLENÉ* nebo *standardní*</span><span class="sxs-lookup"><span data-stu-id="1dc11-146">Under **General**, select either *SHARED* or *STANDARD*</span></span>
* <span data-ttu-id="1dc11-147">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="1dc11-147">Click **Save**</span></span>

<span data-ttu-id="1dc11-148">Může se zobrazit zpráva s žádostí o změnu ověřit a potvrdit, že vaše webová aplikace mohla vzniknout teď náklady, v závislosti na využití a další konfigurace, které nastavíte.</span><span class="sxs-lookup"><span data-stu-id="1dc11-148">You may receive a message asking you to verify the change and acknowledge your web app may now incur a cost, depending upon usage and the other configuration you set.</span></span>

<span data-ttu-id="1dc11-149">Jak dlouho trvá několik sekund zpracovat nové nastavení, takže teď je vhodná doba na spuštění nastavení vaší domény.</span><span class="sxs-lookup"><span data-stu-id="1dc11-149">It takes a few seconds to process the new settings, so now is a good time to start setting up your domain.</span></span>

## <a name="verify-your-domain"></a><span data-ttu-id="1dc11-150">Ověřte svoji doménu</span><span class="sxs-lookup"><span data-stu-id="1dc11-150">Verify your domain</span></span>
<span data-ttu-id="1dc11-151">Před Azure Web Apps vám umožní chcete namapovat doménu k lokalitě, musíte nejdřív ověřte, zda jsou autorizaci mapování domény.</span><span class="sxs-lookup"><span data-stu-id="1dc11-151">Before Azure Web Apps will allow you to map a domain to the site, you first need to verify that you have the authorization to map the domain.</span></span> <span data-ttu-id="1dc11-152">To pokud chcete udělat, musíte přidat nový záznam CNAME do položky DNS.</span><span class="sxs-lookup"><span data-stu-id="1dc11-152">To do so, you must add a new CNAME record to your DNS entry.</span></span>

* <span data-ttu-id="1dc11-153">Přihlaste se k vaší doméně Správce DNS</span><span class="sxs-lookup"><span data-stu-id="1dc11-153">Log in to your domain's DNS manager</span></span>
* <span data-ttu-id="1dc11-154">Vytvořit nový záznam CNAME *awverify*</span><span class="sxs-lookup"><span data-stu-id="1dc11-154">Create a new CNAME *awverify*</span></span>
* <span data-ttu-id="1dc11-155">Bod *awverify* k *awverify. YOUR_DOMAIN.azurewebsites.NET*</span><span class="sxs-lookup"><span data-stu-id="1dc11-155">Point *awverify* to *awverify.YOUR_DOMAIN.azurewebsites.net*</span></span>

<span data-ttu-id="1dc11-156">To může trvat nějakou dobu, než se změny DNS přejděte do plný vliv, takže pokud tyto kroky nefungují okamžitě, přejděte zkontrolujte cup kávy, pak se vraťte a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-156">It may take some time for the DNS changes to go into full effect, so if the following steps do not work immediately, go make a cup of coffee, then come back and try again.</span></span>

## <a name="add-the-domain-to-the-web-app"></a><span data-ttu-id="1dc11-157">Přidání domény do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="1dc11-157">Add the domain to the web app</span></span>
<span data-ttu-id="1dc11-158">Vrátit do vaší webové aplikace prostřednictvím portálu Azure, klikněte na tlačítko **nastavení**a potom klikněte na **vlastní domény a SSL**.</span><span class="sxs-lookup"><span data-stu-id="1dc11-158">Return to your web app through the Azure Portal, click **Settings**, and then click **Custom domains and SSL**.</span></span>

<span data-ttu-id="1dc11-159">Když *nastavení SSL* se zobrazí, zobrazí se pole, které budete zadávat všechny domény, které chcete přiřadit k vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dc11-159">When the *SSL settings* are displayed, you will see the fields where you will input all the domains which you wish to assign to your web app.</span></span> <span data-ttu-id="1dc11-160">Pokud zde není uveden domény, nebude k dispozici pro mapování uvnitř WordPress, bez ohledu na to, jak doména DNS je instalační program.</span><span class="sxs-lookup"><span data-stu-id="1dc11-160">If a domain is not listed here, it will not be available for mapping inside WordPress, regardless of how the domain DNS is setup.</span></span>

![Dialogové okno vlastní domény spravovat][wordpress-manage-domains]

<span data-ttu-id="1dc11-162">Po zadání vaší domény do textového pole, bude Azure ověřit záznam CNAME, který jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="1dc11-162">After typing your domain into the text box, Azure will verify the CNAME record you created previously.</span></span> <span data-ttu-id="1dc11-163">Pokud DNS není plně rozšíří, se zobrazí červený indikátoru.</span><span class="sxs-lookup"><span data-stu-id="1dc11-163">If the DNS has not fully propagated, a red indicator will show.</span></span> <span data-ttu-id="1dc11-164">Pokud bylo úspěšné, zobrazí zelené zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="1dc11-164">If it was successful, you will see a green checkmark.</span></span> 

<span data-ttu-id="1dc11-165">Poznamenejte si adresu IP uvedených v dolní části dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="1dc11-165">Take note of the IP Address listed at the bottom of the dialog.</span></span> <span data-ttu-id="1dc11-166">Budete potřebovat k nastavení záznamu A pro doménu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-166">You will need this to setup the A record for your domain.</span></span>

## <a name="setup-the-domain-a-record"></a><span data-ttu-id="1dc11-167">Nastavení domény záznam</span><span class="sxs-lookup"><span data-stu-id="1dc11-167">Setup the domain A record</span></span>
<span data-ttu-id="1dc11-168">Pokud ostatní kroky byly úspěšné, mohou nyní přiřadit domény do Azure webové aplikace prostřednictvím záznam DNS A.</span><span class="sxs-lookup"><span data-stu-id="1dc11-168">If the other steps were successful, you may now assign the domain to your Azure web app through a DNS A record.</span></span> 

<span data-ttu-id="1dc11-169">Je důležité si uvědomit, webové aplikace Azure přijali CNAME a záznamy A, ale Tady můžete *musí* pomocí záznamu A povolit správné doméně mapování.</span><span class="sxs-lookup"><span data-stu-id="1dc11-169">It is important to note here that Azure web apps accept both CNAME and A records, however you *must* use an A record to enable proper domain mapping.</span></span> <span data-ttu-id="1dc11-170">Záznam CNAME nemůže předají do jiné CNAME, který je co Azure vytvořili pro vás s YOUR_DOMAIN.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="1dc11-170">A CNAME cannot be forwarded to another CNAME, which is what Azure created for you with YOUR_DOMAIN.azurewebsites.net.</span></span>

<span data-ttu-id="1dc11-171">Pomocí IP adresy z předchozího kroku, vraťte se na Správce DNS a nastavit záznam A tak, aby odkazoval na tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-171">Using the IP address from the previous step, return to your DNS manager and setup the A record to point to that IP.</span></span>

## <a name="install-and-setup-the-plugin"></a><span data-ttu-id="1dc11-172">Instalace a nastavení modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="1dc11-172">Install and setup the plugin</span></span>
<span data-ttu-id="1dc11-173">Nasazení ve více lokalitách WordPress aktuálně nemá předdefinovanou metodu pro mapování vlastních domén.</span><span class="sxs-lookup"><span data-stu-id="1dc11-173">WordPress Multisite currently does not have a built-in method to map custom domains.</span></span> <span data-ttu-id="1dc11-174">Existuje však o modul plug-in názvem [WordPress MU domény mapování] [ wordpress-plugin-wordpress-mu-domain-mapping] , přidá funkce pro vás.</span><span class="sxs-lookup"><span data-stu-id="1dc11-174">However, there is a plugin called [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] that adds the functionality for you.</span></span> <span data-ttu-id="1dc11-175">Přihlaste se na správce sítě část vaší lokality a nainstalujte **WordPress MU domény mapování** modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="1dc11-175">Log in to the Network Admin portion of your site and install the **WordPress MU Domain Mapping** plugin.</span></span>

<span data-ttu-id="1dc11-176">Po instalaci a aktivaci modul plug-in, navštivte **nastavení** > **domény mapování** nakonfigurovat modul plug-in.</span><span class="sxs-lookup"><span data-stu-id="1dc11-176">After installing and activating the plugin, visit **Settings** > **Domain Mapping** to configure the plugin.</span></span> <span data-ttu-id="1dc11-177">Do textového pole první *IP adresa serveru*, zadejte IP adresu, které jste použili k nastavení záznamu A pro doménu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-177">In the first textbox, *Server IP Address*, input the IP Address you used to setup the A record for the domain.</span></span> <span data-ttu-id="1dc11-178">Nastavte libovolné *možnosti domény* jste desire (výchozí hodnoty jsou často) a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1dc11-178">Set any *Domain Options* you desire (the defaults are often fine) and click **Save**.</span></span>

## <a name="map-the-domain"></a><span data-ttu-id="1dc11-179">Mapa domény</span><span class="sxs-lookup"><span data-stu-id="1dc11-179">Map the domain</span></span>
<span data-ttu-id="1dc11-180">Přejděte **řídicí panel** pro lokalitu, které chcete namapovat doménu k.</span><span class="sxs-lookup"><span data-stu-id="1dc11-180">Visit the **Dashboard** for the site you wish to map the domain to.</span></span> <span data-ttu-id="1dc11-181">Klikněte na **nástroje** > **domény mapování** a zadejte nové domény do textového pole a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1dc11-181">Click on **Tools** > **Domain Mapping** and type the new domain into the textbox and click **Add**.</span></span>

<span data-ttu-id="1dc11-182">Ve výchozím nastavení bude se v nové doméně přepsané téma, které doméně lokality generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="1dc11-182">By default, the new domain will be rewritten to the autogenerated site domain.</span></span> <span data-ttu-id="1dc11-183">Pokud chcete, aby veškerý provoz odeslaný do nové domény, zkontrolujte *primární domény pro tento blog* pole před uložením.</span><span class="sxs-lookup"><span data-stu-id="1dc11-183">If you want to have all traffic sent to the new domain, check the *Primary domain for this blog* box before saving.</span></span> <span data-ttu-id="1dc11-184">Neomezený počet domén můžete přidat k lokalitě, ale může být pouze jeden primární.</span><span class="sxs-lookup"><span data-stu-id="1dc11-184">You can add an unlimited number of domains to a site, but  only one can be primary.</span></span>

## <a name="do-it-again"></a><span data-ttu-id="1dc11-185">Provést akci</span><span class="sxs-lookup"><span data-stu-id="1dc11-185">Do it again</span></span>
<span data-ttu-id="1dc11-186">Webové aplikace Azure umožňují přidání neomezený počet domén do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1dc11-186">Azure Web Apps allow you to add an unlimited number of domains to a web app.</span></span> <span data-ttu-id="1dc11-187">Chcete-li přidat jiné domény, budete muset provést **ověřte svoji doménu** a **nastavení domény záznam** oddíly pro každou doménu.</span><span class="sxs-lookup"><span data-stu-id="1dc11-187">To add another domain you will need to execute the **Verify your domain** and **Setup the domain A record** sections for each domain.</span></span>    

> [!NOTE]
> <span data-ttu-id="1dc11-188">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1dc11-188">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="1dc11-189">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="1dc11-189">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="1dc11-190">Co se změnilo</span><span class="sxs-lookup"><span data-stu-id="1dc11-190">What's changed</span></span>
* <span data-ttu-id="1dc11-191">Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="1dc11-191">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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


