---
title: "Kurz: Azure Active Directory integrace s SAP HANA Cloudová platforma | Microsoft Docs"
description: "Zjistěte, jak toouse SAP HANA Cloudová platforma s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: cc6610969b1c7b08f776e6969a5977fc75eb9ab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a><span data-ttu-id="bf33b-103">Kurz: Integrace Azure Active Directory se službou SAP HANA Cloud Platform</span><span class="sxs-lookup"><span data-stu-id="bf33b-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform</span></span>
<span data-ttu-id="bf33b-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="bf33b-104">hello objective of this tutorial is tooshow hello integration of Azure and SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="bf33b-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="bf33b-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="bf33b-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="bf33b-106">A valid Azure subscription</span></span>
* <span data-ttu-id="bf33b-107">Účet SAP HANA Cloudová platforma</span><span class="sxs-lookup"><span data-stu-id="bf33b-107">A SAP HANA Cloud Platform account</span></span>

<span data-ttu-id="bf33b-108">Po dokončení tohoto kurzu, hello uživatele Azure AD, které jste přiřadili tooSAP HANA Cloudová platforma bude možné toosingle přihlášení do aplikace hello pomocí hello [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bf33b-108">After completing this tutorial, hello Azure AD users you have assigned tooSAP HANA Cloud Platform will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

>[!IMPORTANT]
><span data-ttu-id="bf33b-109">Potřebujete toodeploy vlastní aplikace nebo odebírat aplikace tooan na vaše SAP HANA Cloudová platforma jeden účet tootest přihlásit.</span><span class="sxs-lookup"><span data-stu-id="bf33b-109">You need toodeploy your own application or subscribe tooan application on your SAP HANA Cloud Platform account tootest single sign on.</span></span> <span data-ttu-id="bf33b-110">V tomto kurzu je aplikace nasazená v účtu hello.</span><span class="sxs-lookup"><span data-stu-id="bf33b-110">In this tutorial, an application is deployed in hello account.</span></span>
> 
> 

<span data-ttu-id="bf33b-111">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="bf33b-111">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="bf33b-112">Povolení integrace aplikace hello pro SAP HANA Cloudová platforma</span><span class="sxs-lookup"><span data-stu-id="bf33b-112">Enabling hello application integration for SAP HANA Cloud Platform</span></span>
2. <span data-ttu-id="bf33b-113">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="bf33b-113">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="bf33b-114">Přiřazení role uživatele tooa</span><span class="sxs-lookup"><span data-stu-id="bf33b-114">Assigning a role tooa user</span></span>
4. <span data-ttu-id="bf33b-115">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="bf33b-115">Assigning users</span></span>

<span data-ttu-id="bf33b-116">![Scénář](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="bf33b-116">![Scenario](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-sap-hana-cloud-platform"></a><span data-ttu-id="bf33b-117">Povolení integrace aplikace hello pro SAP HANA Cloudová platforma</span><span class="sxs-lookup"><span data-stu-id="bf33b-117">Enabling hello application integration for SAP HANA Cloud Platform</span></span>
<span data-ttu-id="bf33b-118">Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="bf33b-118">hello objective of this section is toooutline how tooenable hello application integration for SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="bf33b-119">**Integrace aplikace hello tooenable pro SAP HANA Cloudová platforma, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bf33b-119">**tooenable hello application integration for SAP HANA Cloud Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf33b-120">V hello Azure Management Portal, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-120">In hello Azure Management Portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="bf33b-121">![Služby Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="bf33b-121">![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="bf33b-122">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="bf33b-122">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="bf33b-123">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="bf33b-123">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    <span data-ttu-id="bf33b-124">![Aplikace](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="bf33b-124">![Applications](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="bf33b-125">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="bf33b-125">Click **Add** at hello bottom of hello page.</span></span>
   
    <span data-ttu-id="bf33b-126">![Přidat aplikaci](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="bf33b-126">![Add application](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="bf33b-127">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-127">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    <span data-ttu-id="bf33b-128">![Přidání aplikace z gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="bf33b-128">![Add an application from gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="bf33b-129">V hello **vyhledávacího pole**, typ **SAP HANA Cloudová platforma**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-129">In hello **search box**, type **SAP HANA Cloud Platform**.</span></span>
   
    <span data-ttu-id="bf33b-130">![Galerie aplikací](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="bf33b-130">![Application Gallery](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Application Gallery")</span></span>
7. <span data-ttu-id="bf33b-131">V podokně výsledků hello, vyberte **SAP HANA Cloudová platforma**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf33b-131">In hello results pane, select **SAP HANA Cloud Platform**, and then click **Complete** tooadd hello application.</span></span>
   
    <span data-ttu-id="bf33b-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span><span class="sxs-lookup"><span data-stu-id="bf33b-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="bf33b-133">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bf33b-133">Configure single sign-on</span></span>

<span data-ttu-id="bf33b-134">Hello cílem této části je toooutline jak tooenable uživatelé tooauthenticate tooSAP HANA Cloudová platforma ke svému účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="bf33b-134">hello objective of this section is toooutline how tooenable users tooauthenticate tooSAP HANA Cloud Platform with their account in Azure AD using federation based on hello SAML protocol.</span></span>

<span data-ttu-id="bf33b-135">V rámci tohoto postupu jsou požadované tooupload klienta SAP HANA Cloudová platforma tooyour kódování base-64 kódovaného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="bf33b-135">As part of this procedure, you are required tooupload a base-64 encoded certificate tooyour SAP HANA Cloud Platform tenant.</span></span>  

<span data-ttu-id="bf33b-136">Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="bf33b-136">If you are not familiar with this procedure, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="bf33b-137">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bf33b-137">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf33b-138">V portálu Azure classic, na hello hello **SAP HANA Cloudová platforma** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bf33b-138">In hello Azure classic portal, on hello **SAP HANA Cloud Platform** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="bf33b-139">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="bf33b-139">![Configure single sign-on](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="bf33b-140">Na hello **jak jste by například uživatelé toosign na tooSAP HANA Cloudová platforma** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-140">On hello **How would you like users toosign on tooSAP HANA Cloud Platform** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="bf33b-141">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="bf33b-141">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="bf33b-142">V okně prohlížeče jiných webových přihlaste toohello SAP HANA cloudové platformy řídící panel na https://account. \<hostitele na šířku\>.ondemand.com/cockpit (např: *https://account.hanatrial.ondemand.com/cockpit*).</span><span class="sxs-lookup"><span data-stu-id="bf33b-142">In a different web browser window, sign on toohello SAP HANA Cloud Platform Cockpit at https://account.\<landscape host\>.ondemand.com/cockpit (e.g.: *https://account.hanatrial.ondemand.com/cockpit*).</span></span>
4. <span data-ttu-id="bf33b-143">Klikněte na tlačítko hello **důvěřovat** kartě.</span><span class="sxs-lookup"><span data-stu-id="bf33b-143">Click hello **Trust** tab.</span></span>
   
    <span data-ttu-id="bf33b-144">![Vztah důvěryhodnosti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "vztahu důvěryhodnosti")</span><span class="sxs-lookup"><span data-stu-id="bf33b-144">![Trust](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Trust")</span></span>
5. <span data-ttu-id="bf33b-145">V části Správa vztahu důvěryhodnosti proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bf33b-145">In trust management section, perform hello following steps:</span></span>
   
    <span data-ttu-id="bf33b-146">![Získat Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "získat Metadata")</span><span class="sxs-lookup"><span data-stu-id="bf33b-146">![Get Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Get Metadata")</span></span>
   
   1. <span data-ttu-id="bf33b-147">Klikněte na tlačítko hello **místní poskytovatele služeb** kartě.</span><span class="sxs-lookup"><span data-stu-id="bf33b-147">Click hello **Local Service Provider** tab.</span></span>
   2. <span data-ttu-id="bf33b-148">Klikněte na tlačítko toodownload hello SAP HANA Cloudová platforma soubor metadat, **získat Metadata**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-148">toodownload hello SAP HANA Cloud Platform metadata file, click **Get Metadata**.</span></span>
6. <span data-ttu-id="bf33b-149">Na portálu Azure Active classic hello na hello **konfigurace adresy URL aplikace** stránky, proveďte hello následující kroky a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-149">In hello Azure Active classic portal, on hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
    <span data-ttu-id="bf33b-150">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="bf33b-150">![Configure App URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configure App URL")</span></span>
   
   1. <span data-ttu-id="bf33b-151">V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá vaše toosign uživatelů do vašeho **SAP HANA Cloudová platforma** aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf33b-151">In hello **Sign On URL** textbox, type hello URL used by your users toosign into your **SAP HANA Cloud Platform** application.</span></span> <span data-ttu-id="bf33b-152">Toto je adresa URL účtu konkrétní hello chráněného prostředku v aplikaci SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="bf33b-152">This is hello account-specific URL of a protected resource in your SAP HANA Cloud Platform application.</span></span> <span data-ttu-id="bf33b-153">Hello adresa URL je založena na následujících vzor hello: *https://\<applicationName\>\<accountName\>.\< na šířku hostitele\>.ondemand.com/\<cesta\_k\_chráněné\_prostředků\>*  (např: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span><span class="sxs-lookup"><span data-stu-id="bf33b-153">hello URL is based on hello following pattern: *https://\<applicationName\>\<accountName\>.\<landscape host\>.ondemand.com/\<path\_to\_protected\_resource\>* (e.g.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="bf33b-154">Toto je hello URL ve vaší aplikace SAP HANA Cloudová platforma, která vyžaduje tooauthenticate hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="bf33b-154">This is hello URL in your SAP HANA Cloud Platform application that requires hello user tooauthenticate.</span></span>
     > 

   2. <span data-ttu-id="bf33b-155">Otevřete soubor metadat SAP HANA Cloudová platforma hello stáhli a vyhledejte hello **ns3:AssertionConsumerService** značky.</span><span class="sxs-lookup"><span data-stu-id="bf33b-155">Open hello downloaded SAP HANA Cloud Platform metadata file, and then locate hello **ns3:AssertionConsumerService** tag.</span></span>
   3. <span data-ttu-id="bf33b-156">Zkopírujte hodnotu hello hello **umístění** atributů a pak ji vložit do hello **SAP HANA cloudové platformy adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="bf33b-156">Copy hello value of hello **Location** attribute, and then paste it into hello **SAP HANA Cloud Platform Reply URL** textbox.</span></span>

7. <span data-ttu-id="bf33b-157">Na hello **nakonfigurovat jednotné přihlašování v SAP HANA Cloudová platforma** stránky, toodownload metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="bf33b-157">On hello **Configure single sign-on at SAP HANA Cloud Platform** page, toodownload your metadata, click **Download metadata**, and then save hello file on your computer.</span></span>
   
    <span data-ttu-id="bf33b-158">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="bf33b-158">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configure Single Sign-On")</span></span>
8. <span data-ttu-id="bf33b-159">Na hello SAP HANA cloudové platformy řídicí panel, v hello **místní poskytovatele služeb** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bf33b-159">On hello SAP HANA Cloud Platform Cockpit, in hello **Local Service Provider** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="bf33b-160">![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "důvěřovat správy")</span><span class="sxs-lookup"><span data-stu-id="bf33b-160">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Trust Management")</span></span>
   
  1. <span data-ttu-id="bf33b-161">Klikněte na **Upravit**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-161">Click **Edit**.</span></span>
  2. <span data-ttu-id="bf33b-162">Jako **typ konfigurace**, vyberte **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-162">As **Configuration Type**, select **Custom**.</span></span>
  3. <span data-ttu-id="bf33b-163">Jako **místní název zprostředkovatele**, ponechte výchozí hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="bf33b-163">As **Local Provider Name**, leave hello default value.</span></span>
  4. <span data-ttu-id="bf33b-164">toogenerate **podpisový klíč** a **certifikát pro podpis** dvojice klíčů, klikněte na tlačítko **Generování páru klíč**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-164">toogenerate a **Signing Key** and a **Signing Certificate** key pair, click **Generate Key Pair**.</span></span>
  5. <span data-ttu-id="bf33b-165">Jako **hlavní šíření**, vyberte **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-165">As **Principal Propagation**, select **Disabled**.</span></span>
  6. <span data-ttu-id="bf33b-166">Jako **vynucení ověřování**, vyberte **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-166">As **Force Authentication**, select **Disabled**.</span></span>
  7. <span data-ttu-id="bf33b-167">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-167">Click **Save**.</span></span>

9. <span data-ttu-id="bf33b-168">Klikněte na tlačítko hello **důvěryhodného zprostředkovatele Identity** a pak klikněte **přidání důvěryhodného zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-168">Click hello **Trusted Identity Provider** tab, and then click **Add Trusted Identity Provider**.</span></span>
   
    <span data-ttu-id="bf33b-169">![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "důvěřovat správy")</span><span class="sxs-lookup"><span data-stu-id="bf33b-169">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Trust Management")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="bf33b-170">toomanage hello seznamu zprostředkovatelů identity důvěryhodný, je nutné toohave vybrali hello vlastní typ konfigurace v hello části místní poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="bf33b-170">toomanage hello list of trusted identity providers, you need toohave chosen hello Custom configuration type in hello Local Service Provider section.</span></span> <span data-ttu-id="bf33b-171">Výchozí typ konfigurace je nutné upravovat a implicitní vztah důvěryhodnosti toohello SAP ID služby.</span><span class="sxs-lookup"><span data-stu-id="bf33b-171">For Default configuration type, you have a non-editable and implicit trust toohello SAP ID Service.</span></span> <span data-ttu-id="bf33b-172">Pro None nemáte žádné nastavení důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="bf33b-172">For None, you don't have any trust settings.</span></span>
    > 
    > 

10. <span data-ttu-id="bf33b-173">Klikněte na tlačítko hello **Obecné** a pak klikněte **Procházet** tooupload hello stáhnout soubor metadat.</span><span class="sxs-lookup"><span data-stu-id="bf33b-173">Click hello **General** tab, and then click **Browse** tooupload hello downloaded metadata file.</span></span>
    
    <span data-ttu-id="bf33b-174">![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "důvěřovat správy")</span><span class="sxs-lookup"><span data-stu-id="bf33b-174">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Trust Management")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="bf33b-175">Po nahrání souboru metadat hello, hello hodnoty pro **adresy jednotného přihlašování**, **jednu adresu URL odhlašovací** a **certifikát pro podpis** jsou naplněny automaticky.</span><span class="sxs-lookup"><span data-stu-id="bf33b-175">After uploading hello metadata file, hello values for **Single Sign-on URL**, **Single Logout URL** and **Signing Certificate** are populated automatically.</span></span>
    > 
    > 

11. <span data-ttu-id="bf33b-176">Klikněte na tlačítko hello **atributy** kartě.</span><span class="sxs-lookup"><span data-stu-id="bf33b-176">Click hello **Attributes** tab.</span></span>
12. <span data-ttu-id="bf33b-177">Na hello **atributy** kartu, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="bf33b-177">On hello **Attributes** tab, perform hello following step:</span></span>
    
    <span data-ttu-id="bf33b-178">![Atributy](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="bf33b-178">![Attributes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attributes")</span></span> 
  * <span data-ttu-id="bf33b-179">Klikněte na tlačítko **Add Assertion-Based atribut**a poté přidejte následující atributy založené na assertion hello:</span><span class="sxs-lookup"><span data-stu-id="bf33b-179">Click **Add Assertion-Based Attribute**, and then add hello following assertion-based attributes:</span></span>
       
    | <span data-ttu-id="bf33b-180">Kontrolní výraz atribut</span><span class="sxs-lookup"><span data-stu-id="bf33b-180">Assertion Attribute</span></span> | <span data-ttu-id="bf33b-181">Objekt zabezpečení atribut</span><span class="sxs-lookup"><span data-stu-id="bf33b-181">Principal Attribute</span></span> |
    | --- | --- |
    | <span data-ttu-id="bf33b-182">http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/givenName</span><span class="sxs-lookup"><span data-stu-id="bf33b-182">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname</span></span> |<span data-ttu-id="bf33b-183">FirstName</span><span class="sxs-lookup"><span data-stu-id="bf33b-183">firstname</span></span> |
    | <span data-ttu-id="bf33b-184">http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Surname</span><span class="sxs-lookup"><span data-stu-id="bf33b-184">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname</span></span> |<span data-ttu-id="bf33b-185">Příjmení</span><span class="sxs-lookup"><span data-stu-id="bf33b-185">lastname</span></span> |
    | <span data-ttu-id="bf33b-186">http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/EmailAddress</span><span class="sxs-lookup"><span data-stu-id="bf33b-186">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress</span></span> |<span data-ttu-id="bf33b-187">E-mailu</span><span class="sxs-lookup"><span data-stu-id="bf33b-187">email</span></span> 
   
     >[!NOTE]
     ><span data-ttu-id="bf33b-188">Konfigurace Hello hello atributů závisí na tom, jak aplikace hello na HCP jsou vyvinuté, tj. atributy, které budou očekávat při hello odpověď SAML a pod které názvem (hlavní atribut) přistupují tento atribut v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="bf33b-188">hello configuration of hello Attributes depends on how hello application(s) on HCP are developed, i.e. which attribute(s) they expect in hello SAML response and under which name (Principal Attribute) they access this attribute in hello code.</span></span>
     > 
     >  

    1.  <span data-ttu-id="bf33b-189">Hello **výchozí atribut** v hello je snímek jen pro účely obrázku.</span><span class="sxs-lookup"><span data-stu-id="bf33b-189">hello **Default Attribute** in hello screenshot is just for illustration purposes.</span></span> <span data-ttu-id="bf33b-190">Není nutné toomake hello scénář pracovní.</span><span class="sxs-lookup"><span data-stu-id="bf33b-190">It is not required toomake hello scenario work.</span></span>   
    2.  <span data-ttu-id="bf33b-191">Hello názvy a hodnoty pro **hlavní atribut** ukazuje hello snímek závisí na tom, jak je aplikace hello vyvinuta.</span><span class="sxs-lookup"><span data-stu-id="bf33b-191">hello names and values for **Principal Attribute** shown in hello screenshot depend on how hello application is developed.</span></span> <span data-ttu-id="bf33b-192">Je možné, že vaše aplikace vyžaduje jiný mapování.</span><span class="sxs-lookup"><span data-stu-id="bf33b-192">It is possible that your application requires different mappings.</span></span>
     
13. <span data-ttu-id="bf33b-193">V portálu Azure classic, na hello hello **nakonfigurovat jednotné přihlašování v SAP HANA Cloudová platforma** Dialogový stránky, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-193">In hello Azure classic portal, on hello **Configure single sign-on at SAP HANA Cloud Platform** dialogue page, select hello single sign-on configuration confirmation, and then click **Complete**.</span></span>
    
    <span data-ttu-id="bf33b-194">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="bf33b-194">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configure Single Sign-On")</span></span>

###<a name="assertion-based-groups"></a><span data-ttu-id="bf33b-195">Skupiny založené na výrazu</span><span class="sxs-lookup"><span data-stu-id="bf33b-195">Assertion-based groups</span></span>
<span data-ttu-id="bf33b-196">Volitelný krok můžete nakonfigurovat skupiny založené na assertion pro zprostředkovatele Identity Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bf33b-196">As an optional step, you can configure assertion-based groups for your Azure Active Directory Identity Provider.</span></span>

<span data-ttu-id="bf33b-197">Pomocí skupin na SAP HANA Cloudová platforma umožňuje toodynamically přiřadit jeden nebo více uživatelů tooone nebo více rolí v aplikacích SAP HANA Cloudová platforma určit podle hodnot atributů v hello SAML 2.0 kontrolní výraz.</span><span class="sxs-lookup"><span data-stu-id="bf33b-197">Using groups on SAP HANA Cloud Platform allows you toodynamically assign one or more users tooone or more roles in your SAP HANA Cloud Platform applications, determined by values of attributes in hello SAML 2.0 assertion.</span></span> 

<span data-ttu-id="bf33b-198">Například pokud hello kontrolní výraz obsahuje atribut hello "*kontrakt = dočasné*", může být vhodné všechny skupiny přidané toohello toobe ovlivnění uživatelé"*dočasné*".</span><span class="sxs-lookup"><span data-stu-id="bf33b-198">For example, if hello assertion contains hello attribute "*contract=temporary*", you may want all affected users toobe added toohello group "*TEMPORARY*".</span></span> <span data-ttu-id="bf33b-199">skupiny Hello "*dočasné*" může obsahovat jednu nebo více rolí z jedné nebo více aplikací, které jsou nasazené v účtu SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="bf33b-199">hello group "*TEMPORARY*" may contain one or more roles from one or more applications deployed in your SAP HANA Cloud Platform account.</span></span>
 
<span data-ttu-id="bf33b-200">Skupiny založené na assertion použijte, pokud chcete přiřadit toosimultaneously mnoho tooone uživatele nebo více rolí aplikací ve vašem účtu SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="bf33b-200">Use assertion-based groups when you want toosimultaneously assign many users tooone or more roles of applications in your SAP HANA Cloud Platform account.</span></span> <span data-ttu-id="bf33b-201">Pokud chcete tooassign pouze jednoho či malý počet uživatelů toospecific rolí, doporučujeme, abyste jim přiřadil přímo v hello "**autorizací**" kartě řídící panel SAP HANA Cloudová platforma hello.</span><span class="sxs-lookup"><span data-stu-id="bf33b-201">If you want tooassign only a single or small number of users toospecific roles, we recommend assigning them directly in hello “**Authorizations**” tab of hello SAP HANA Cloud Platform cockpit.</span></span>

## <a name="assign-a-role-tooa-user"></a><span data-ttu-id="bf33b-202">Přiřazení role uživatele tooa</span><span class="sxs-lookup"><span data-stu-id="bf33b-202">Assign a role tooa user</span></span>
<span data-ttu-id="bf33b-203">V pořadí tooenable Azure AD Uživatelé toolog do SAP HANA cloudové platformy je nutné přiřadit role v toothem SAP HANA Cloudová platforma hello.</span><span class="sxs-lookup"><span data-stu-id="bf33b-203">In order tooenable Azure AD users toolog into SAP HANA Cloud Platform, you must assign roles in hello SAP HANA Cloud Platform toothem.</span></span>

<span data-ttu-id="bf33b-204">**tooassign tooa role uživatele, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bf33b-204">**tooassign a role tooa user, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf33b-205">Přihlaste se tooyour **SAP HANA Cloudová platforma** řídící panel.</span><span class="sxs-lookup"><span data-stu-id="bf33b-205">Log in tooyour **SAP HANA Cloud Platform** cockpit.</span></span>
2. <span data-ttu-id="bf33b-206">Proveďte následující hello:</span><span class="sxs-lookup"><span data-stu-id="bf33b-206">Perform hello following:</span></span>
   
   <span data-ttu-id="bf33b-207">![Povolení](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "autorizací")</span><span class="sxs-lookup"><span data-stu-id="bf33b-207">![Authorizations](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Authorizations")</span></span>
   
  1. <span data-ttu-id="bf33b-208">Klikněte na tlačítko **autorizace**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-208">Click **Authorization**.</span></span>
  2. <span data-ttu-id="bf33b-209">Klikněte na tlačítko hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="bf33b-209">Click hello **Users** tab.</span></span>
  3. <span data-ttu-id="bf33b-210">V hello **uživatele** textovému poli, typ hello uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="bf33b-210">In hello **User** textbox, type hello user’s email address.</span></span>
  4. <span data-ttu-id="bf33b-211">Klikněte na tlačítko **přiřadit** tooassign hello uživatele tooa role.</span><span class="sxs-lookup"><span data-stu-id="bf33b-211">Click **Assign** tooassign hello user tooa role.</span></span>
  5. <span data-ttu-id="bf33b-212">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-212">Click **Save**.</span></span>

## <a name="assign-users"></a><span data-ttu-id="bf33b-213">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="bf33b-213">Assign users</span></span>
<span data-ttu-id="bf33b-214">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="bf33b-214">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="bf33b-215">**tooassign uživatelé tooSAP HANA Cloudová platforma, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bf33b-215">**tooassign users tooSAP HANA Cloud Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="bf33b-216">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="bf33b-216">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="bf33b-217">Na hello **SAP HANA Cloudová platforma** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="bf33b-217">On hello **SAP HANA Cloud Platform** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="bf33b-218">![Přiřazení uživatelů](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="bf33b-218">![Assign Users](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Assign Users")</span></span>
3. <span data-ttu-id="bf33b-219">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="bf33b-219">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="bf33b-220">![Ano](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="bf33b-220">![Yes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="bf33b-221">Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bf33b-221">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="bf33b-222">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bf33b-222">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

