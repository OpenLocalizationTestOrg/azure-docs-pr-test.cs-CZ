---
title: "Co je nového v nástroji Správa podniková aplikace v Azure Active Directory | Microsoft Docs"
description: "Zjistěte, co je nového v nástroji Správa podniková aplikace v Azure Active Directory."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: 0c32a6719292aa903aa32dfdc4a31114e7a28346
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a><span data-ttu-id="3da50-103">Co je nového v nástroji Správa podniková aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3da50-103">What's new in Enterprise Application management in Azure Active Directory</span></span> 

<span data-ttu-id="3da50-104">Azure Active Directory (Azure AD) je vylepšené enterprise nástroje pro správu aplikací, s nové funkce a možnosti, aby Správa aplikací jednodušší a efektivní.</span><span class="sxs-lookup"><span data-stu-id="3da50-104">Azure Active Directory (Azure AD) has enhanced enterprise application management tools, with new features and capabilities to make managing apps simpler and efficient.</span></span>

<span data-ttu-id="3da50-105">Zde jsou některé rozšířením pro službu Azure AD v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3da50-105">Following are some of the enhancements for Azure AD in the [Azure portal](https://portal.azure.com).</span></span>

- <span data-ttu-id="3da50-106">Vylepšené aplikace galerii prostředí s vytvoření modelu zjednodušené aplikací a podpora pro všechny typy aplikací, které jste použili.</span><span class="sxs-lookup"><span data-stu-id="3da50-106">An improved application gallery experience, with a simplified application creation model and support for all the application types that you’re used to.</span></span> 
- <span data-ttu-id="3da50-107">Zcela nový rychlý start prostředí, které vám může pomoct zprovoznění pilotní nasazení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3da50-107">A brand-new quick start experience that can help you get going with a pilot of your application.</span></span> 
- <span data-ttu-id="3da50-108">Nakonfigurujte zásady samoobslužné služby pomocí několika kliknutí.</span><span class="sxs-lookup"><span data-stu-id="3da50-108">Configure self-service policies with just a few clicks.</span></span> 
- <span data-ttu-id="3da50-109">Vylepšení proxy aplikace jednotného přihlašování konfigurace a přineste si vlastní prostředí aplikace umožňuje získat více done než před.</span><span class="sxs-lookup"><span data-stu-id="3da50-109">Improvements to application proxy, single sign-on configuration, and bring your own application experiences, allowing you to get more done than before.</span></span>

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a><span data-ttu-id="3da50-110">Vylepšení galerii aplikací Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3da50-110">Improvements to the Azure Active Directory Application Gallery</span></span>

<span data-ttu-id="3da50-111">Vaše oblíbené aplikace přidat, ať už z [galerii aplikací](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), vlastních aplikací, které jste rozšíření do cloudu nebo nové aplikace, které vyvíjíte.</span><span class="sxs-lookup"><span data-stu-id="3da50-111">Add your favorite applications whether they are from the [application gallery](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), custom applications you’re extending to the cloud, or new applications you’re developing.</span></span>  <span data-ttu-id="3da50-112">Můžete začít používat s toto nové prostředí kliknutím **přidat** na **podnikové aplikace, které** přehled nebo **všechny aplikace** okna.</span><span class="sxs-lookup"><span data-stu-id="3da50-112">You can get started with this new experience by clicking **Add** on the **Enterprise Applications** overview or **All applications** blades.</span></span>
 
  ![Přidání aplikace](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

<span data-ttu-id="3da50-114">Jednou v galerii, uvidíte, že všechny naše vybrané aplikace, které podporují zřizování uživatelů zobrazí přední a center.</span><span class="sxs-lookup"><span data-stu-id="3da50-114">Once in the gallery, you’ll see all our featured applications which support user provisioning displayed front-and-center.</span></span>  <span data-ttu-id="3da50-115">Můžete procházet nejrůznějším různých kategorií přejdete do aplikace, které se zajímáte o nebo možnosti vyhledávání můžete rychle najít aplikací, které chcete integrovat.</span><span class="sxs-lookup"><span data-stu-id="3da50-115">You can browse all sorts of different categories to drill into the applications you care about, or you can use the search experience to rapidly find the applications you want to integrate.</span></span>

  ![Galerii aplikací](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a><span data-ttu-id="3da50-117">Přidat vlastní aplikace z jednoho místa</span><span class="sxs-lookup"><span data-stu-id="3da50-117">Add custom applications from one place</span></span>

<span data-ttu-id="3da50-118">Kromě přidání předem integrovaných aplikací z galerie, všechny, které narazí konfiguraci vlastní aplikace měla použitá při na portálu classic správu jsou nyní v nového portálu.</span><span class="sxs-lookup"><span data-stu-id="3da50-118">In addition to adding pre-integrated applications from the gallery, all the custom application configuration experiences that you were used to in the classic management portal are now possible in the new portal.</span></span> <span data-ttu-id="3da50-119">Zda jsou rozšíření aplikace v místním prostředí pomocí proxy aplikace, integraci vlastní heslo nebo federovaného jednotného přihlašování k aplikaci, nebo vytvoření zcela nové aplikace pomocí klíče registru aplikace, můžete získat k němu z jediné jeden.</span><span class="sxs-lookup"><span data-stu-id="3da50-119">Whether you are extending an application from on-premises using the application proxy, integrating your own password or federated SSO application, or creating a brand-new application using the application registry, you can get to it all from this one single place.</span></span>

  ![Přidání vlastní aplikace](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
<span data-ttu-id="3da50-121">**Abyste mohli začít přidání vlastní aplikace**:</span><span class="sxs-lookup"><span data-stu-id="3da50-121">**To get started adding your own application**:</span></span>

1. <span data-ttu-id="3da50-122">Klikněte **přidat odkaz na vaši vlastní** v horní části galerii aplikací.</span><span class="sxs-lookup"><span data-stu-id="3da50-122">Click the **add your own link** at the top of the application gallery.</span></span> 
2. <span data-ttu-id="3da50-123">Uvidíte dvě možnosti před jste: **nasadit existující aplikaci** nebo **vytvořte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="3da50-123">You’ll see two options in front of you: **deploy an existing application** or **develop a new application**.</span></span> <span data-ttu-id="3da50-124">Přečtěte si informace o rozdílu mezi dvě možnosti a jejich použití.</span><span class="sxs-lookup"><span data-stu-id="3da50-124">Read on to learn the difference between the two options and how to use them.</span></span>

### <a name="deploying-existing-applications"></a><span data-ttu-id="3da50-125">Existující nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="3da50-125">Deploying existing applications</span></span>

1. <span data-ttu-id="3da50-126">Pokud máte k dispozici již spuštěné aplikace a chcete jen na jeho integraci do Azure AD pro jednotné přihlašování nebo zřizování, zvolte **nasadit existující aplikaci** možnost.</span><span class="sxs-lookup"><span data-stu-id="3da50-126">If you’ve got an application running already and just want to integrate it into Azure AD for single-sign on or provisioning, choose the **Deploy an existing application** option.</span></span> <span data-ttu-id="3da50-127">Vyberte název vaší aplikace, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3da50-127">Pick a name for your application, click **Add**.</span></span>
2. <span data-ttu-id="3da50-128">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="3da50-128">That's it!</span></span> <span data-ttu-id="3da50-129">Místo nepotřebuje vědět všechny podrobnosti o aplikaci předem, můžete nyní nastavit jak novou aplikaci funguje tak, že procházení v levé nabídce a konfigurace aplikace libovolně kdykoli.</span><span class="sxs-lookup"><span data-stu-id="3da50-129">Instead of needing to know all the details about your application up front, you can now set up how your new application works by navigating through the left menu and configuring the application to your liking at any time.</span></span>

  ![Přidání stávající aplikace s jedním kliknutím](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a><span data-ttu-id="3da50-131">Vývoj nových aplikací</span><span class="sxs-lookup"><span data-stu-id="3da50-131">Developing new applications</span></span>

1. <span data-ttu-id="3da50-132">Pokud vytváříte novou aplikaci, je snadný způsob získání registru aplikace pravé z galerie:</span><span class="sxs-lookup"><span data-stu-id="3da50-132">If you’re developing a new application, there's an easy way for you to get to the Application Registry right from the gallery:</span></span>
2. <span data-ttu-id="3da50-133">Klikněte na **přidat vlastní** možnost z Galerie aplikace, vyberte **vyvíjet existující aplikaci** volba a zobrazí se odkaz rychlé právo na používání aplikací přidat.</span><span class="sxs-lookup"><span data-stu-id="3da50-133">Click on the **add your own** option from the Application Gallery, select the **develop an existing application** choice, and you’ll see a quick link right to the application add experience.</span></span>

  ![Přidání aplikace nově vyvinutých stačí jen pár kliknutí](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
><span data-ttu-id="3da50-135">Jakmile přidáte aplikace pomocí klíče registru aplikace, zobrazí se zobrazí v seznamu podnikové aplikace, kde budete moci konfigurovat jednotné přihlašování a spravovat zásady přístupu pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3da50-135">Once you add an application using the Application Registry, you’ll see it show up in the list of Enterprise Applications where you’ll be able to configure single sign-on and manage access policies for your new application.</span></span>

  ![Správa přístupu k nové aplikaci v podnikových aplikací](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quick-start-get-going-with-your-new-application-right-away"></a><span data-ttu-id="3da50-137">Rychlý start: Začínáme s novou aplikaci hned</span><span class="sxs-lookup"><span data-stu-id="3da50-137">Quick start: Get going with your new application right away</span></span> 

<span data-ttu-id="3da50-138">Po přidání aplikace, jestli se předem integrované nebo vlastní aplikace, vytvořili jsme šité na míru prostředí úvodní, abyste si rychle založena na nové prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="3da50-138">After you’ve added an application, whether it be pre-integrated or your own app, we’ve created a tailored quick-start experience to get you grounded in the new applications experience quickly.</span></span> <span data-ttu-id="3da50-139">Pokud budete postupovat systematičtěji podle jednotlivých možností, budete jsme vás provede procesem uživatelského rozhraní a ukazují, jak co nejrychleji začít s pilotní nasazení nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="3da50-139">If you follow each option systematically, we’ll walk you through the UI and show you how to get going with a pilot of your new application as quickly as possible.</span></span> 
 
  ![Nové aplikace rychlý start prostředí](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 <span data-ttu-id="3da50-141">Toto nové prostředí úvodní kdykoli a pro každou aplikaci, můžete taky navštívit kliknutím na **úvodní** z levé navigační nabídce aplikace.</span><span class="sxs-lookup"><span data-stu-id="3da50-141">You can visit this new quick start experience at any time, and for any application, by clicking on **Quick start** from the application left navigation menu.</span></span>


## <a name="updated-application-proxy-configuration"></a><span data-ttu-id="3da50-142">Konfigurace proxy aktualizované aplikace</span><span class="sxs-lookup"><span data-stu-id="3da50-142">Updated application proxy configuration</span></span>
<span data-ttu-id="3da50-143">Nyní Pojďme indikované jeden z nové aplikace, které jste přidali běží v místním prostředí a chcete integrovat se službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3da50-143">Now, let’s say one of the new applications you added is running in your on-premises environment and you want to integrate it with Azure AD.</span></span>  <span data-ttu-id="3da50-144">Jedním z nástrojů nových informací o nové rozhraní konfigurace aplikací v nové službě Azure AD portál je, že rozdělením aplikace přihlašování v režimu z jeho konfigurace proxy aplikace nyní můžete snadno vystavit heslo jednotného přihlašování nebo federovaných aplikací, které běží ve vaší podnikové síti přímo do cloudu, aniž by bylo nutné vytvořit více instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="3da50-144">One of the cool new things about the new application configuration experience in the new Azure AD portal is that by splitting the application’s sign-on mode from its application proxy configuration, you can now easily expose password SSO or federated applications that are running in your corporate network right to the cloud, without having to create multiple instances of the application.</span></span>

<span data-ttu-id="3da50-145">Kromě toho teď můžete také konfigurovat žádné nové aplikace, které jste přidali pro použití se službou Azure AD Application Proxy přímo z portálu nový, včetně aplikací, které podporují nativní prostředí ověřování Windows.</span><span class="sxs-lookup"><span data-stu-id="3da50-145">In addition to this, you can now also configure any of the new applications you’ve added for use with the Azure AD Application Proxy right from the new portal, including applications which support native Windows authentication experiences.</span></span>

  ![Konfigurace aplikace použít možnost přihlášení integrované ověřování systému Windows](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 

<span data-ttu-id="3da50-147">Chcete začít konfigurovat nativní aplikaci ověřování systému Windows s Proxy aplikace:</span><span class="sxs-lookup"><span data-stu-id="3da50-147">To get started configuring a native Windows authentication application with the Application Proxy:</span></span>
1. <span data-ttu-id="3da50-148">Kliknutím na položku jednoho navigačního přihlašování a zvolte **integrované ověřování systému Windows** z okna Nastavení přihlášení a nakonfigurujte nastavení libovolně.</span><span class="sxs-lookup"><span data-stu-id="3da50-148">Click on the Single sign-on navigation item and choose **Integrated Windows Authentication** from the sign-on settings blade and configure the settings to your liking.</span></span>
2. <span data-ttu-id="3da50-149">Nad podporuje tyto nové režimy ověřování, teď můžete také nahrát certifikáty z vlastní domény pro podporu aplikací běžících na zabezpečené koncové body v rámci vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="3da50-149">On top of supporting these new authentication modes, you can now also upload certificates from custom domains to support applications running on secure endpoints within your organization.</span></span>  
 
   ![Nahrávání certifikátu pro použití s Proxy aplikace](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

3. <span data-ttu-id="3da50-151">Pokud chcete nahrát nový certifikát pro vaše oblíbené místní aplikace, klikněte na **proxy aplikace** možnost z levé navigační nabídky, klikněte na tlačítko **certifikát** selektor a nahrání souboru certifikátu můžeme použít k šifrování požadavky z našich koncového bodu cloudu do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3da50-151">To upload a new certificate for your favorite on-premises application, click on the **Application proxy** option from the left navigation menu, click the **Certificate** selector, and upload a certificate file we can use to encrypt requests from our cloud endpoint to your application.</span></span>

## <a name="advanced-federated-single-sign-on-configuration"></a><span data-ttu-id="3da50-152">Pokročilé federované přihlašování jednotné</span><span class="sxs-lookup"><span data-stu-id="3da50-152">Advanced federated single sign-on configuration</span></span>

<span data-ttu-id="3da50-153">U těch, které používáte federovaným aplikacím dnes existuje mnoho nových funkcí v okně přihlašování v konfiguraci na základě SAML.</span><span class="sxs-lookup"><span data-stu-id="3da50-153">For those of you using federated applications today, there are many new features in the SAML-based sign-on configuration blade.</span></span> <span data-ttu-id="3da50-154">Abyste mohli začít teď můžete plně přizpůsobit, přidat, odebrat a mapovat existující uživatelské atributy vystaví jako deklarace identity v tokenech SAML.</span><span class="sxs-lookup"><span data-stu-id="3da50-154">To start with, now you can fully customize, add, remove, and map the existing user attributes issued as claims in SAML tokens.</span></span>
 
  ![Přizpůsobení atributy SAML token uživatele předaný federovaných aplikací](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)


<span data-ttu-id="3da50-156">Zkontrolujte, že se nový federovaný Konfigurace jednotného přihlašování:</span><span class="sxs-lookup"><span data-stu-id="3da50-156">To check that out the new federated SSO configuration:</span></span>
1. <span data-ttu-id="3da50-157">Otevřete federovaných aplikací **jednotného přihlašování** okno z levé navigační nabídku a ujistěte se, že '*na základě SAML přihlašování** vybrat režim.</span><span class="sxs-lookup"><span data-stu-id="3da50-157">Open a federated application’s **single sign-on** blade from the left navigation menu and make sure the ‘*SAML-based Sign-on** mode is selected.</span></span> 
2. <span data-ttu-id="3da50-158">Existuje, povolit políčka pod jednou **uživatelské atributy** záhlaví upravit všechny atributy, které jsou součástí tokenu SAML předaný tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3da50-158">Once there, enable the checkbox under the **User Attributes** heading to modify all of the attributes included in the SAML token passed to that application.</span></span>

<span data-ttu-id="3da50-159">Můžete můžete také vytvořit, výměny a spravovat certifikáty používané pro federované jednotné přihlašování, jakož i upravit, který získá upozorněni, když váš certifikát již brzy vyprší.</span><span class="sxs-lookup"><span data-stu-id="3da50-159">You can also create, rollover, and manage certificates used for federated single sign-on, as well as edit who gets notified when your certificate is about to expire.</span></span> <span data-ttu-id="3da50-160">Zobrazí se tyto nové možnosti v části **certifikáty** záhlaví na stejné jeden přihlašování v okně.</span><span class="sxs-lookup"><span data-stu-id="3da50-160">You’ll see these new options under the **Certificates** heading on the same Single sign-on blade.</span></span>
 
  ![Vytvoření nového certifikátu, přizpůsobení vypršení platnosti oznámení e-mailu a možnosti podepsání certifikátu](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-paramenter"></a><span data-ttu-id="3da50-162">Stav předávání paramenter</span><span class="sxs-lookup"><span data-stu-id="3da50-162">Relay State paramenter</span></span>
<span data-ttu-id="3da50-163">Nakonec jsme jste také rozšířit sadu parametrů SAML URL podporujeme zahrnout **parametru State předávání**, což je stránce uživatelům budou zobrazovat uvnitř federovaných aplikací po dokončení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3da50-163">Finally, we’ve also extended the set of SAML URL parameters we support to include the **Relay State parameter**, which is the page your users will land on inside of a federated application once the sign-in is completed.</span></span> <span data-ttu-id="3da50-164">Toto je velmi užitečná nastavení konfigurace, pokud chcete uživatelům odeslat na konkrétní místo v aplikaci je nelze vrátit probíhající rychle.</span><span class="sxs-lookup"><span data-stu-id="3da50-164">This is very useful setting to configure if you want to send your users to a specific place within the application to get them going quickly.</span></span>

  ![Nastavení parametru SAML předávání stavu](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
<span data-ttu-id="3da50-166">**Nastavení parametru stavu předávání**:</span><span class="sxs-lookup"><span data-stu-id="3da50-166">**To set the relay state parameter**:</span></span>

1. <span data-ttu-id="3da50-167">Povolit **zobrazit upřesňující nastavení adresy URL** políčko v části **domény a adresy URL** záhlaví na jednotné přihlašování v okně konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3da50-167">Enable the **Show advanced URL settings** check-box under the **Domain and URLs** heading on the single-sign on configuration blade.</span></span> 
2. <span data-ttu-id="3da50-168">Jakmile to uděláte, zobrazí se, že sadu nové adrese URL vstupních polí se zobrazují, které vám umožní nastavit tyto a další nastavení adresy URL SAML.</span><span class="sxs-lookup"><span data-stu-id="3da50-168">Once you do this, you’ll see a set of new URL input boxes appear which will allow you to set this, and other, SAML URL settings.</span></span>

## <a name="bring-your-own-password-sso-applications"></a><span data-ttu-id="3da50-169">Přineste vlastní heslo aplikace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3da50-169">Bring your own password SSO applications</span></span>

<span data-ttu-id="3da50-170">Víme, že ne každé aplikace podporuje federační okamžitě po nasazení.</span><span class="sxs-lookup"><span data-stu-id="3da50-170">We know that not every application supports federation right out of the box.</span></span> <span data-ttu-id="3da50-171">Může být například jeden nové aplikace, které jste přidali má vlastní přihlašovací obrazovku, kterou budou uživatelé používat uživatelské jméno a heslo pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3da50-171">For example, maybe one of the new applications you added has a custom login screen that your users use a username and password to sign in to.</span></span> <span data-ttu-id="3da50-172">Tyto typy aplikací můžete stále integraci s Azure AD pomocí našich **přineste si vlastní aplikace** funkci, která je nyní k dispozici, můžete nakonfigurovat v nového portálu.</span><span class="sxs-lookup"><span data-stu-id="3da50-172">You can still integrate these types of applications with Azure AD using our **Bring your own applications** feature, which is now available for you to configure in the new portal.</span></span>
 
  ![Integrace vlastního hesla vaulting aplikací s Azure AD](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

<span data-ttu-id="3da50-174">**Rezervovat funkci "přineste vlastní aplikace,**:</span><span class="sxs-lookup"><span data-stu-id="3da50-174">**To check out the 'Bring your own applications' feature**:</span></span>

1. <span data-ttu-id="3da50-175">Po nastavení jedné přihlašování v režimu pro novou vlastní aplikaci, kterou jste přidali do **založené na heslech přihlašování**, zadejte adresu URL, kde aplikace vykreslí jeho obrazovka pro přihlášení a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3da50-175">After you set the single sign-on mode for a new custom application that you’ve added to **Password-based Sign-on**, enter the URL where the application renders its login screen and click **Save**.</span></span>  
2. <span data-ttu-id="3da50-176">Jakmile to uděláte, jsme budete automaticky scrape tuto adresu URL pro uživatelské jméno a heslo vstupní pole a vám umožní používat Azure AD bezpečně přenést hesla k dané aplikaci pomocí rozšíření přístup k panelu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3da50-176">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span>

## <a name="configure-self-service-application-access"></a><span data-ttu-id="3da50-177">Konfigurace přístupu k aplikaci Samoobslužné služby</span><span class="sxs-lookup"><span data-stu-id="3da50-177">Configure self-service application access</span></span>

<span data-ttu-id="3da50-178">Po přidání spoustu nové aplikace, možná budete chtít povolit uživatelům procházet a přidejte k vlastní panelů přístup k těmto aplikacím, aniž by museli jste Nepokoušejte se jako správce.</span><span class="sxs-lookup"><span data-stu-id="3da50-178">After you’ve added lots of new applications, maybe you want to allow your users to browse and add those applications to their own access panels, without needing to bother you as an administrator.</span></span> <span data-ttu-id="3da50-179">Nyní tato nejnovější verze, můžete nakonfigurovat a spravovat přístup k aplikaci Samoobslužné služby přímo z nového portálu.</span><span class="sxs-lookup"><span data-stu-id="3da50-179">Now, with this latest release, you can configure and manage self-service application access right from the new portal.</span></span>

  ![Konfigurace samoobslužné služby aplikace přístup k zadání hesla aplikace jednotného přihlašování](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
<span data-ttu-id="3da50-181">**Konfigurovat a spravovat přístup k aplikaci Samoobslužné služby**:</span><span class="sxs-lookup"><span data-stu-id="3da50-181">**To configure and manage self-service application access**:</span></span>

1. <span data-ttu-id="3da50-182">Chcete-li začít, můžete vybrat **samoobslužné služby** levé navigační nabídku a nastavit možnost z aplikace **povolit uživatelům žádat o přístup k této aplikaci?** možnost k '**Ano**'.</span><span class="sxs-lookup"><span data-stu-id="3da50-182">To get started, you can select the **Self-service** option from the application’s left navigation menu and set the **Allow users to request access to this application?** option to ‘**Yes**’.</span></span> 
2. <span data-ttu-id="3da50-183">To vám umožní nakonfigurovat, kdo může schválit přístup k této aplikaci a kteří uživatelé samoobslužné služby skupiny budou přidáni.</span><span class="sxs-lookup"><span data-stu-id="3da50-183">This will enable you to configure who is allowed to approve access to this application, and which group self-service users will be added.</span></span> <span data-ttu-id="3da50-184">Kromě toho pokud aplikace je nakonfigurovaná pro heslo jednotného přihlašování, se také zobrazí jinou možnost, která umožňuje volitelně povolit tyto schvalovatelů spravovat hesla přiřazené k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3da50-184">In addition, if the application is configured for password single-sign on, you’ll also see another option which lets you optionally allow those approvers to manage the passwords assigned to the application.</span></span>

##<a name="feedback"></a><span data-ttu-id="3da50-185">Váš názor</span><span class="sxs-lookup"><span data-stu-id="3da50-185">Feedback</span></span>

<span data-ttu-id="3da50-186">Věříme, že je jako vylepšenou prostředí Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3da50-186">We hope you like using the improved Azure AD experience.</span></span> <span data-ttu-id="3da50-187">Prosím udržovat zpětnou vazbu, než dorazí!</span><span class="sxs-lookup"><span data-stu-id="3da50-187">Please keep the feedback coming!</span></span> <span data-ttu-id="3da50-188">POST vaše názory a návrhy pro zlepšení **portál pro správu** části našich [fóru pro zpětnou vazbu](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).</span><span class="sxs-lookup"><span data-stu-id="3da50-188">Post your feedback and ideas for improvement in the **Admin Portal** section of our [feedback forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).</span></span>  <span data-ttu-id="3da50-189">Jsme se vzrušení o vytváření nástrojů nové vlastní položky každý den a použijte vaše pokyny na obrazec a definovat, co se máme zaměřit příště.</span><span class="sxs-lookup"><span data-stu-id="3da50-189">We’re excited about building cool new stuff every day, and use your guidance to shape and define what we build next.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3da50-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3da50-190">Next steps</span></span>

<span data-ttu-id="3da50-191">Další podrobnosti najdete v tématu [Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md).</span><span class="sxs-lookup"><span data-stu-id="3da50-191">For more details, see [Managing Applications with Azure Active Directory](active-directory-enable-sso-scenario.md).</span></span>



