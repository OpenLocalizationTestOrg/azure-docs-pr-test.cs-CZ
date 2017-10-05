---
title: "Kurz: Azure Active Directory integrace s SAP HANA Cloudová platforma | Microsoft Docs"
description: "Další informace o použití SAP HANA Cloudová platforma s Azure Active Directory umožňující jednotné přihlašování, automatického zřizování a další!"
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
ms.openlocfilehash: e03bc2410a8d57363c558f723b3bfd0e69b3f4c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a><span data-ttu-id="83a90-103">Kurz: Integrace Azure Active Directory se službou SAP HANA Cloud Platform</span><span class="sxs-lookup"><span data-stu-id="83a90-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform</span></span>
<span data-ttu-id="83a90-104">Cílem tohoto kurzu je zobrazit integraci Azure a Cloudová platforma SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="83a90-104">The objective of this tutorial is to show the integration of Azure and SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="83a90-105">Scénář uvedených v tomto kurzu se předpokládá, že už máte následující položky:</span><span class="sxs-lookup"><span data-stu-id="83a90-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="83a90-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="83a90-106">A valid Azure subscription</span></span>
* <span data-ttu-id="83a90-107">Účet SAP HANA Cloudová platforma</span><span class="sxs-lookup"><span data-stu-id="83a90-107">A SAP HANA Cloud Platform account</span></span>

<span data-ttu-id="83a90-108">Po dokončení tohoto kurzu, bude moct jednotné přihlašování do aplikace pomocí uživatele Azure AD, které jste přiřadili SAP HANA Cloudová platforma [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="83a90-108">After completing this tutorial, the Azure AD users you have assigned to SAP HANA Cloud Platform will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

>[!IMPORTANT]
><span data-ttu-id="83a90-109">Potřebujete nasadit vlastní aplikaci nebo přihlášení k aplikaci na vašem účtu SAP HANA Cloudová platforma k testování jednotné přihlašování v odběru.</span><span class="sxs-lookup"><span data-stu-id="83a90-109">You need to deploy your own application or subscribe to an application on your SAP HANA Cloud Platform account to test single sign on.</span></span> <span data-ttu-id="83a90-110">V tomto kurzu je aplikace nasazená v účtu.</span><span class="sxs-lookup"><span data-stu-id="83a90-110">In this tutorial, an application is deployed in the account.</span></span>
> 
> 

<span data-ttu-id="83a90-111">Scénář uvedených v tomto kurzu se skládá z následujících stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="83a90-111">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="83a90-112">Povolení integrace aplikace pro SAP HANA Cloudová platforma</span><span class="sxs-lookup"><span data-stu-id="83a90-112">Enabling the application integration for SAP HANA Cloud Platform</span></span>
2. <span data-ttu-id="83a90-113">Konfigurace jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="83a90-113">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="83a90-114">Přiřazení role pro uživatele</span><span class="sxs-lookup"><span data-stu-id="83a90-114">Assigning a role to a user</span></span>
4. <span data-ttu-id="83a90-115">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="83a90-115">Assigning users</span></span>

<span data-ttu-id="83a90-116">![Scénář](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="83a90-116">![Scenario](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a><span data-ttu-id="83a90-117">Povolení integrace aplikace pro SAP HANA Cloudová platforma</span><span class="sxs-lookup"><span data-stu-id="83a90-117">Enabling the application integration for SAP HANA Cloud Platform</span></span>
<span data-ttu-id="83a90-118">Cílem této části se popisují postup povolení integrace aplikace pro SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="83a90-118">The objective of this section is to outline how to enable the application integration for SAP HANA Cloud Platform.</span></span>

<span data-ttu-id="83a90-119">**Pokud chcete povolit integraci aplikací pro SAP HANA Cloudová platforma, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83a90-119">**To enable the application integration for SAP HANA Cloud Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="83a90-120">V portálu pro správu Azure, v levém navigačním podokně klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="83a90-120">In the Azure Management Portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="83a90-121">![Služby Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="83a90-121">![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="83a90-122">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="83a90-122">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="83a90-123">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="83a90-123">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="83a90-124">![Aplikace](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="83a90-124">![Applications](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="83a90-125">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="83a90-125">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="83a90-126">![Přidat aplikaci](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="83a90-126">![Add application](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="83a90-127">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="83a90-127">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="83a90-128">![Přidání aplikace z gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="83a90-128">![Add an application from gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="83a90-129">V **vyhledávacího pole**, typ **SAP HANA Cloudová platforma**.</span><span class="sxs-lookup"><span data-stu-id="83a90-129">In the **search box**, type **SAP HANA Cloud Platform**.</span></span>
   
    <span data-ttu-id="83a90-130">![Galerie aplikací](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="83a90-130">![Application Gallery](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Application Gallery")</span></span>
7. <span data-ttu-id="83a90-131">V podokně výsledků vyberte **SAP HANA Cloudová platforma**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="83a90-131">In the results pane, select **SAP HANA Cloud Platform**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="83a90-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span><span class="sxs-lookup"><span data-stu-id="83a90-132">![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="83a90-133">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="83a90-133">Configure single sign-on</span></span>

<span data-ttu-id="83a90-134">Cílem této části se popisují postup povolení uživatelů k ověřování a SAP HANA Cloudovou platformu pro svůj účet ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="83a90-134">The objective of this section is to outline how to enable users to authenticate to SAP HANA Cloud Platform with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="83a90-135">V rámci tohoto postupu je nutné odeslat kódování base-64 kódovaného certifikátu klienta SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="83a90-135">As part of this procedure, you are required to upload a base-64 encoded certificate to your SAP HANA Cloud Platform tenant.</span></span>  

<span data-ttu-id="83a90-136">Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak převést binární certifikát do textového souboru](http://youtu.be/PlgrzUZ-Y1o)</span><span class="sxs-lookup"><span data-stu-id="83a90-136">If you are not familiar with this procedure, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)</span></span>

<span data-ttu-id="83a90-137">**Pokud chcete konfigurovat jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83a90-137">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="83a90-138">Na portálu Azure classic na **SAP HANA Cloudová platforma** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="83a90-138">In the Azure classic portal, on the **SAP HANA Cloud Platform** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="83a90-139">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="83a90-139">![Configure single sign-on](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configure single sign-on")</span></span>
2. <span data-ttu-id="83a90-140">Na **jak chcete uživatelům se přihlásit SAP HANA Cloudová platforma** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="83a90-140">On the **How would you like users to sign on to SAP HANA Cloud Platform** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="83a90-141">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="83a90-141">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="83a90-142">V okně prohlížeče jiný web Přihlaste se k SAP HANA cloudové platformy řídicím panelu v https://account. \<hostitele na šířku\>.ondemand.com/cockpit (např: *https://account.hanatrial.ondemand.com/cockpit*).</span><span class="sxs-lookup"><span data-stu-id="83a90-142">In a different web browser window, sign on to the SAP HANA Cloud Platform Cockpit at https://account.\<landscape host\>.ondemand.com/cockpit (e.g.: *https://account.hanatrial.ondemand.com/cockpit*).</span></span>
4. <span data-ttu-id="83a90-143">Klikněte **důvěřovat** kartě.</span><span class="sxs-lookup"><span data-stu-id="83a90-143">Click the **Trust** tab.</span></span>
   
    <span data-ttu-id="83a90-144">![Vztah důvěryhodnosti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "vztahu důvěryhodnosti")</span><span class="sxs-lookup"><span data-stu-id="83a90-144">![Trust](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Trust")</span></span>
5. <span data-ttu-id="83a90-145">V části Správa vztahu důvěryhodnosti proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="83a90-145">In trust management section, perform the following steps:</span></span>
   
    <span data-ttu-id="83a90-146">![Získat Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "získat Metadata")</span><span class="sxs-lookup"><span data-stu-id="83a90-146">![Get Metadata](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Get Metadata")</span></span>
   
   1. <span data-ttu-id="83a90-147">Klikněte **místní poskytovatele služeb** kartě.</span><span class="sxs-lookup"><span data-stu-id="83a90-147">Click the **Local Service Provider** tab.</span></span>
   2. <span data-ttu-id="83a90-148">Chcete-li stáhnout soubor metadat SAP HANA Cloudová platforma, klikněte na tlačítko **získat Metadata**.</span><span class="sxs-lookup"><span data-stu-id="83a90-148">To download the SAP HANA Cloud Platform metadata file, click **Get Metadata**.</span></span>
6. <span data-ttu-id="83a90-149">Na portálu Azure Active classic na **konfigurace adresy URL aplikace** stránky, proveďte následující kroky a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="83a90-149">In the Azure Active classic portal, on the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
    <span data-ttu-id="83a90-150">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="83a90-150">![Configure App URL](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configure App URL")</span></span>
   
   1. <span data-ttu-id="83a90-151">V **přihlašovací adresa URL** textové pole, zadejte adresu URL používají vaši uživatelé pro přihlášení do vaší **SAP HANA Cloudová platforma** aplikace.</span><span class="sxs-lookup"><span data-stu-id="83a90-151">In the **Sign On URL** textbox, type the URL used by your users to sign into your **SAP HANA Cloud Platform** application.</span></span> <span data-ttu-id="83a90-152">Toto je adresa URL účtu konkrétní chráněného prostředku v aplikaci SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="83a90-152">This is the account-specific URL of a protected resource in your SAP HANA Cloud Platform application.</span></span> <span data-ttu-id="83a90-153">Adresa URL je založena na následující vzoru: *https://\<applicationName\>\<accountName\>.\< na šířku hostitele\>.ondemand.com/\<cesta\_k\_chráněné\_prostředků\>*  (např: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span><span class="sxs-lookup"><span data-stu-id="83a90-153">The URL is based on the following pattern: *https://\<applicationName\>\<accountName\>.\<landscape host\>.ondemand.com/\<path\_to\_protected\_resource\>* (e.g.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="83a90-154">Toto je adresa URL v aplikaci SAP HANA Cloudová platforma, která vyžaduje ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="83a90-154">This is the URL in your SAP HANA Cloud Platform application that requires the user to authenticate.</span></span>
     > 

   2. <span data-ttu-id="83a90-155">Otevřete stažený soubor metadat SAP HANA Cloudová platforma a vyhledejte **ns3:AssertionConsumerService** značky.</span><span class="sxs-lookup"><span data-stu-id="83a90-155">Open the downloaded SAP HANA Cloud Platform metadata file, and then locate the **ns3:AssertionConsumerService** tag.</span></span>
   3. <span data-ttu-id="83a90-156">Zkopírujte hodnotu **umístění** atribut a vložte ji do **SAP HANA cloudové platformy adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="83a90-156">Copy the value of the **Location** attribute, and then paste it into the **SAP HANA Cloud Platform Reply URL** textbox.</span></span>

7. <span data-ttu-id="83a90-157">Na **nakonfigurovat jednotné přihlašování v SAP HANA Cloudová platforma** stránku a stáhnout metadata, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor v počítači.</span><span class="sxs-lookup"><span data-stu-id="83a90-157">On the **Configure single sign-on at SAP HANA Cloud Platform** page, to download your metadata, click **Download metadata**, and then save the file on your computer.</span></span>
   
    <span data-ttu-id="83a90-158">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="83a90-158">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configure Single Sign-On")</span></span>
8. <span data-ttu-id="83a90-159">Na SAP HANA cloudové platformy řídicím panelu v **místní poskytovatele služeb** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="83a90-159">On the SAP HANA Cloud Platform Cockpit, in the **Local Service Provider** section, perform the following steps:</span></span>
   
    <span data-ttu-id="83a90-160">![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "důvěřovat správy")</span><span class="sxs-lookup"><span data-stu-id="83a90-160">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Trust Management")</span></span>
   
  1. <span data-ttu-id="83a90-161">Klikněte na **Upravit**.</span><span class="sxs-lookup"><span data-stu-id="83a90-161">Click **Edit**.</span></span>
  2. <span data-ttu-id="83a90-162">Jako **typ konfigurace**, vyberte **vlastní**.</span><span class="sxs-lookup"><span data-stu-id="83a90-162">As **Configuration Type**, select **Custom**.</span></span>
  3. <span data-ttu-id="83a90-163">Jako **místní název zprostředkovatele**, ponechte výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="83a90-163">As **Local Provider Name**, leave the default value.</span></span>
  4. <span data-ttu-id="83a90-164">Generovat **podpisový klíč** a **certifikát pro podpis** dvojice klíčů, klikněte na tlačítko **Generování páru klíč**.</span><span class="sxs-lookup"><span data-stu-id="83a90-164">To generate a **Signing Key** and a **Signing Certificate** key pair, click **Generate Key Pair**.</span></span>
  5. <span data-ttu-id="83a90-165">Jako **hlavní šíření**, vyberte **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="83a90-165">As **Principal Propagation**, select **Disabled**.</span></span>
  6. <span data-ttu-id="83a90-166">Jako **vynucení ověřování**, vyberte **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="83a90-166">As **Force Authentication**, select **Disabled**.</span></span>
  7. <span data-ttu-id="83a90-167">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="83a90-167">Click **Save**.</span></span>

9. <span data-ttu-id="83a90-168">Klikněte **důvěryhodného zprostředkovatele Identity** a pak klikněte **přidání důvěryhodného zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="83a90-168">Click the **Trusted Identity Provider** tab, and then click **Add Trusted Identity Provider**.</span></span>
   
    <span data-ttu-id="83a90-169">![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "důvěřovat správy")</span><span class="sxs-lookup"><span data-stu-id="83a90-169">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Trust Management")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="83a90-170">Ke správě seznamu zprostředkovatelů identity důvěryhodné, budete muset vybrali vlastní konfigurace zadejte v části místní poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="83a90-170">To manage the list of trusted identity providers, you need to have chosen the Custom configuration type in the Local Service Provider section.</span></span> <span data-ttu-id="83a90-171">Výchozí typ konfigurace je nutné upravovat a implicitní vztah důvěryhodnosti ke službě ID SAP.</span><span class="sxs-lookup"><span data-stu-id="83a90-171">For Default configuration type, you have a non-editable and implicit trust to the SAP ID Service.</span></span> <span data-ttu-id="83a90-172">Pro None nemáte žádné nastavení důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="83a90-172">For None, you don't have any trust settings.</span></span>
    > 
    > 

10. <span data-ttu-id="83a90-173">Klikněte **Obecné** a pak klikněte **Procházet** nahrát soubor stažený metadat.</span><span class="sxs-lookup"><span data-stu-id="83a90-173">Click the **General** tab, and then click **Browse** to upload the downloaded metadata file.</span></span>
    
    <span data-ttu-id="83a90-174">![Důvěřování správě](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "důvěřovat správy")</span><span class="sxs-lookup"><span data-stu-id="83a90-174">![Trust Management](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Trust Management")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="83a90-175">Po nahrání souboru metadat, hodnoty **adresy jednotného přihlašování**, **jednu adresu URL odhlašovací** a **certifikát pro podpis** jsou naplněny automaticky.</span><span class="sxs-lookup"><span data-stu-id="83a90-175">After uploading the metadata file, the values for **Single Sign-on URL**, **Single Logout URL** and **Signing Certificate** are populated automatically.</span></span>
    > 
    > 

11. <span data-ttu-id="83a90-176">Klikněte na kartu **Atributy**.</span><span class="sxs-lookup"><span data-stu-id="83a90-176">Click the **Attributes** tab.</span></span>
12. <span data-ttu-id="83a90-177">Na **atributy** kartu, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="83a90-177">On the **Attributes** tab, perform the following step:</span></span>
    
    <span data-ttu-id="83a90-178">![Atributy](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="83a90-178">![Attributes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attributes")</span></span> 
  * <span data-ttu-id="83a90-179">Klikněte na tlačítko **Add Assertion-Based atribut**a poté přidejte následující na základě assertion atributy:</span><span class="sxs-lookup"><span data-stu-id="83a90-179">Click **Add Assertion-Based Attribute**, and then add the following assertion-based attributes:</span></span>
       
    | <span data-ttu-id="83a90-180">Kontrolní výraz atribut</span><span class="sxs-lookup"><span data-stu-id="83a90-180">Assertion Attribute</span></span> | <span data-ttu-id="83a90-181">Objekt zabezpečení atribut</span><span class="sxs-lookup"><span data-stu-id="83a90-181">Principal Attribute</span></span> |
    | --- | --- |
    | <span data-ttu-id="83a90-182">http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/givenName</span><span class="sxs-lookup"><span data-stu-id="83a90-182">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname</span></span> |<span data-ttu-id="83a90-183">FirstName</span><span class="sxs-lookup"><span data-stu-id="83a90-183">firstname</span></span> |
    | <span data-ttu-id="83a90-184">http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Surname</span><span class="sxs-lookup"><span data-stu-id="83a90-184">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname</span></span> |<span data-ttu-id="83a90-185">Příjmení</span><span class="sxs-lookup"><span data-stu-id="83a90-185">lastname</span></span> |
    | <span data-ttu-id="83a90-186">http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/EmailAddress</span><span class="sxs-lookup"><span data-stu-id="83a90-186">http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress</span></span> |<span data-ttu-id="83a90-187">E-mailu</span><span class="sxs-lookup"><span data-stu-id="83a90-187">email</span></span> 
   
     >[!NOTE]
     ><span data-ttu-id="83a90-188">Konfigurace atributů závisí na tom, jak aplikace na HCP jsou vyvinuté, tj. atributy, které se očekává v odpovědi SAML a pod které názvem (hlavní atribut) přistupují tento atribut v kódu.</span><span class="sxs-lookup"><span data-stu-id="83a90-188">The configuration of the Attributes depends on how the application(s) on HCP are developed, i.e. which attribute(s) they expect in the SAML response and under which name (Principal Attribute) they access this attribute in the code.</span></span>
     > 
     >  

    1.  <span data-ttu-id="83a90-189">**Výchozí atribut** na snímku obrazovky je jen pro účely obrázku.</span><span class="sxs-lookup"><span data-stu-id="83a90-189">The **Default Attribute** in the screenshot is just for illustration purposes.</span></span> <span data-ttu-id="83a90-190">Chcete-li tento scénář fungovat není potřeba.</span><span class="sxs-lookup"><span data-stu-id="83a90-190">It is not required to make the scenario work.</span></span>   
    2.  <span data-ttu-id="83a90-191">Názvy a hodnoty pro **hlavní atribut** ukazuje snímek obrazovky závisí na tom, jak je aplikace vyvinuté.</span><span class="sxs-lookup"><span data-stu-id="83a90-191">The names and values for **Principal Attribute** shown in the screenshot depend on how the application is developed.</span></span> <span data-ttu-id="83a90-192">Je možné, že vaše aplikace vyžaduje jiný mapování.</span><span class="sxs-lookup"><span data-stu-id="83a90-192">It is possible that your application requires different mappings.</span></span>
     
13. <span data-ttu-id="83a90-193">Na portálu Azure classic na **nakonfigurovat jednotné přihlašování v SAP HANA Cloudová platforma** Dialogový stránky, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="83a90-193">In the Azure classic portal, on the **Configure single sign-on at SAP HANA Cloud Platform** dialogue page, select the single sign-on configuration confirmation, and then click **Complete**.</span></span>
    
    <span data-ttu-id="83a90-194">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="83a90-194">![Configure Single Sign-On](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configure Single Sign-On")</span></span>

###<a name="assertion-based-groups"></a><span data-ttu-id="83a90-195">Skupiny založené na výrazu</span><span class="sxs-lookup"><span data-stu-id="83a90-195">Assertion-based groups</span></span>
<span data-ttu-id="83a90-196">Volitelný krok můžete nakonfigurovat skupiny založené na assertion pro zprostředkovatele Identity Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="83a90-196">As an optional step, you can configure assertion-based groups for your Azure Active Directory Identity Provider.</span></span>

<span data-ttu-id="83a90-197">Použití skupin na SAP HANA Cloudová platforma umožňuje dynamicky přiřadit jeden nebo více uživatelů k jedné nebo více rolí v aplikacích SAP HANA Cloudová platforma, určit podle hodnot atributů v kontrolního výrazu SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="83a90-197">Using groups on SAP HANA Cloud Platform allows you to dynamically assign one or more users to one or more roles in your SAP HANA Cloud Platform applications, determined by values of attributes in the SAML 2.0 assertion.</span></span> 

<span data-ttu-id="83a90-198">Například, pokud kontrolní výraz obsahuje atribut "*kontrakt = dočasné*", může být vhodné všechny ovlivnění uživatelé mají být přidány do skupiny"*dočasné*".</span><span class="sxs-lookup"><span data-stu-id="83a90-198">For example, if the assertion contains the attribute "*contract=temporary*", you may want all affected users to be added to the group "*TEMPORARY*".</span></span> <span data-ttu-id="83a90-199">Skupiny "*dočasné*" může obsahovat jednu nebo více rolí z jedné nebo více aplikací, které jsou nasazené v účtu SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="83a90-199">The group "*TEMPORARY*" may contain one or more roles from one or more applications deployed in your SAP HANA Cloud Platform account.</span></span>
 
<span data-ttu-id="83a90-200">Skupiny založené na assertion použijte, pokud chcete současně přiřadit jednu nebo více rolí aplikací ve vašem účtu SAP HANA Cloudová platforma mnoha uživateli.</span><span class="sxs-lookup"><span data-stu-id="83a90-200">Use assertion-based groups when you want to simultaneously assign many users to one or more roles of applications in your SAP HANA Cloud Platform account.</span></span> <span data-ttu-id="83a90-201">Pokud chcete přiřadit jenom na jeden nebo malý počet uživatelů pro konkrétní role, doporučujeme, abyste jim přiřadil přímo v "**autorizací**" kartě řídící panel SAP HANA Cloudová platforma.</span><span class="sxs-lookup"><span data-stu-id="83a90-201">If you want to assign only a single or small number of users to specific roles, we recommend assigning them directly in the “**Authorizations**” tab of the SAP HANA Cloud Platform cockpit.</span></span>

## <a name="assign-a-role-to-a-user"></a><span data-ttu-id="83a90-202">Přiřazení role uživatele</span><span class="sxs-lookup"><span data-stu-id="83a90-202">Assign a role to a user</span></span>
<span data-ttu-id="83a90-203">Pokud chcete povolit uživatelům Azure AD Přihlaste se k SAP HANA Cloudová platforma, je třeba přiřadit role v SAP HANA Cloudová platforma k nim.</span><span class="sxs-lookup"><span data-stu-id="83a90-203">In order to enable Azure AD users to log into SAP HANA Cloud Platform, you must assign roles in the SAP HANA Cloud Platform to them.</span></span>

<span data-ttu-id="83a90-204">**Pokud chcete přiřadit role pro uživatele, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83a90-204">**To assign a role to a user, perform the following steps:**</span></span>

1. <span data-ttu-id="83a90-205">Přihlaste se k vaší **SAP HANA Cloudová platforma** řídící panel.</span><span class="sxs-lookup"><span data-stu-id="83a90-205">Log in to your **SAP HANA Cloud Platform** cockpit.</span></span>
2. <span data-ttu-id="83a90-206">Postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="83a90-206">Perform the following:</span></span>
   
   <span data-ttu-id="83a90-207">![Povolení](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "autorizací")</span><span class="sxs-lookup"><span data-stu-id="83a90-207">![Authorizations](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Authorizations")</span></span>
   
  1. <span data-ttu-id="83a90-208">Klikněte na tlačítko **autorizace**.</span><span class="sxs-lookup"><span data-stu-id="83a90-208">Click **Authorization**.</span></span>
  2. <span data-ttu-id="83a90-209">Klikněte **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="83a90-209">Click the **Users** tab.</span></span>
  3. <span data-ttu-id="83a90-210">V **uživatele** textovému poli, zadejte e-mailovou adresu uživatele.</span><span class="sxs-lookup"><span data-stu-id="83a90-210">In the **User** textbox, type the user’s email address.</span></span>
  4. <span data-ttu-id="83a90-211">Klikněte na tlačítko **přiřadit** přiřadit uživatele k roli.</span><span class="sxs-lookup"><span data-stu-id="83a90-211">Click **Assign** to assign the user to a role.</span></span>
  5. <span data-ttu-id="83a90-212">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="83a90-212">Click **Save**.</span></span>

## <a name="assign-users"></a><span data-ttu-id="83a90-213">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="83a90-213">Assign users</span></span>
<span data-ttu-id="83a90-214">Chcete-li otestovat vaši konfiguraci, přidělte uživatelům Azure AD, že které chcete povolit přístup aplikace k němu pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="83a90-214">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="83a90-215">**Přiřazení uživatelů k SAP HANA Cloudová platforma, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="83a90-215">**To assign users to SAP HANA Cloud Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="83a90-216">Na portálu Azure classic vytvořte zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="83a90-216">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="83a90-217">Na **SAP HANA Cloudová platforma** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="83a90-217">On the **SAP HANA Cloud Platform** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="83a90-218">![Přiřazení uživatelů](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="83a90-218">![Assign Users](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Assign Users")</span></span>
3. <span data-ttu-id="83a90-219">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** k potvrzení vaší přiřazení.</span><span class="sxs-lookup"><span data-stu-id="83a90-219">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="83a90-220">![Ano](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="83a90-220">![Yes](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="83a90-221">Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="83a90-221">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="83a90-222">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="83a90-222">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

