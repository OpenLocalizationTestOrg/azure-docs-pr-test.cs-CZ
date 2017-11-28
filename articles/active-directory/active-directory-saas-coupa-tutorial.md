---
title: 'Kurz: Azure Active Directory integrace s Coupa | Microsoft Docs'
description: "Zjistěte, jak toouse Coupa s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 87e98573718d27d408c886466a374a987f58faa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a><span data-ttu-id="50b59-103">Kurz: Azure Active Directory integrace s Coupa</span><span class="sxs-lookup"><span data-stu-id="50b59-103">Tutorial: Azure Active Directory integration with Coupa</span></span>
<span data-ttu-id="50b59-104">cílem Hello tohoto kurzu je tooshow hello integrace Azure a Coupa.</span><span class="sxs-lookup"><span data-stu-id="50b59-104">hello objective of this tutorial is tooshow hello integration of Azure and Coupa.</span></span>  
<span data-ttu-id="50b59-105">Hello scénáři uvedeném v tomto kurzu se předpokládá, že už máte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="50b59-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="50b59-106">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="50b59-106">A valid Azure subscription</span></span>
* <span data-ttu-id="50b59-107">Coupa jednotné přihlašování (SSO) povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="50b59-107">A Coupa single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="50b59-108">Po dokončení tohoto kurzu, budou uživatelé hello Azure AD jste přiřadili tooCoupa možné toosingle přihlášení do aplikace hello pomocí hello [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="50b59-108">After completing this tutorial, hello Azure AD users you have assigned tooCoupa will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="50b59-109">scénář Hello uvedených v tomto kurzu se skládá z následujících stavební bloky hello:</span><span class="sxs-lookup"><span data-stu-id="50b59-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

* <span data-ttu-id="50b59-110">Povolení integrace aplikace hello pro Coupa</span><span class="sxs-lookup"><span data-stu-id="50b59-110">Enabling hello application integration for Coupa</span></span>
* <span data-ttu-id="50b59-111">Konfigurace jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="50b59-111">Configuring single sign-on</span></span>
* <span data-ttu-id="50b59-112">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="50b59-112">Configuring user provisioning</span></span>
* <span data-ttu-id="50b59-113">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="50b59-113">Assigning users</span></span>

<span data-ttu-id="50b59-114">![Scénář](./media/active-directory-saas-coupa-tutorial/IC791897.png "scénář")</span><span class="sxs-lookup"><span data-stu-id="50b59-114">![Scenario](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")</span></span>

## <a name="enable-hello-application-integration-for-coupa"></a><span data-ttu-id="50b59-115">Povolit integraci aplikace hello pro Coupa</span><span class="sxs-lookup"><span data-stu-id="50b59-115">Enable hello application integration for Coupa</span></span>
<span data-ttu-id="50b59-116">Hello cílem této části je toooutline jak integrace aplikace hello tooenable pro Coupa.</span><span class="sxs-lookup"><span data-stu-id="50b59-116">hello objective of this section is toooutline how tooenable hello application integration for Coupa.</span></span>

<span data-ttu-id="50b59-117">**Integrace aplikace hello tooenable pro Coupa, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="50b59-117">**tooenable hello application integration for Coupa, perform hello following steps:**</span></span>

1. <span data-ttu-id="50b59-118">V hello portál Azure classic, na levém navigačním podokně hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="50b59-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="50b59-119">![Služby Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "služby Active Directory")</span><span class="sxs-lookup"><span data-stu-id="50b59-119">![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="50b59-120">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="50b59-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="50b59-121">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="50b59-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="50b59-122">![Aplikace](./media/active-directory-saas-coupa-tutorial/IC700994.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="50b59-122">![Applications](./media/active-directory-saas-coupa-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="50b59-123">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="50b59-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="50b59-124">![Přidat aplikaci](./media/active-directory-saas-coupa-tutorial/IC749321.png "přidat aplikaci")</span><span class="sxs-lookup"><span data-stu-id="50b59-124">![Add application](./media/active-directory-saas-coupa-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="50b59-125">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="50b59-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="50b59-126">![Přidání aplikace z gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "přidat aplikaci z gallerry")</span><span class="sxs-lookup"><span data-stu-id="50b59-126">![Add an application from gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="50b59-127">V hello **vyhledávacího pole**, typ **Coupa**.</span><span class="sxs-lookup"><span data-stu-id="50b59-127">In hello **search box**, type **Coupa**.</span></span>
   
   <span data-ttu-id="50b59-128">![Galerie aplikací](./media/active-directory-saas-coupa-tutorial/IC791898.png "galerii aplikací")</span><span class="sxs-lookup"><span data-stu-id="50b59-128">![Application Gallery](./media/active-directory-saas-coupa-tutorial/IC791898.png "Application Gallery")</span></span>
7. <span data-ttu-id="50b59-129">V podokně výsledků hello, vyberte **Coupa**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="50b59-129">In hello results pane, select **Coupa**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="50b59-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span><span class="sxs-lookup"><span data-stu-id="50b59-130">![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="50b59-131">Konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="50b59-131">Configure single sign-on</span></span>

<span data-ttu-id="50b59-132">Hello cílem této části je toooutline jak tooCoupa tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="50b59-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooCoupa with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="50b59-133">Konfigurace jednotného přihlašování pro Coupa vyžaduje tooretrieve hodnotu kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="50b59-133">Configuring single sign-on for Coupa requires you tooretrieve a thumbprint value from a certificate.</span></span> <span data-ttu-id="50b59-134">Pokud nejste obeznámeni s tímto postupem, přečtěte si téma [jak tooretrieve hodnota kryptografického otisku certifikátu](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="50b59-134">If you are not familiar with this procedure, see [How tooretrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span>

<span data-ttu-id="50b59-135">**tooconfigure jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="50b59-135">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="50b59-136">Přihlaste se na tooyour Coupa společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="50b59-136">Sign on tooyour Coupa company site as an administrator.</span></span>
2. <span data-ttu-id="50b59-137">Přejděte příliš**instalace \> řízení zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="50b59-137">Go too**Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="50b59-138">![Ovládací prvky zabezpečení](./media/active-directory-saas-coupa-tutorial/IC791900.png "kontrolních mechanismů pro zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="50b59-138">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
3. <span data-ttu-id="50b59-139">toodownload hello Coupa metadata souboru tooyour počítače, klikněte na **stahování a import metadat SP**.</span><span class="sxs-lookup"><span data-stu-id="50b59-139">toodownload hello Coupa metadata file tooyour computer, click **Download and import SP metadata**.</span></span>
   
   <span data-ttu-id="50b59-140">![Metadata Coupa SP](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadat")</span><span class="sxs-lookup"><span data-stu-id="50b59-140">![Coupa SP metadata](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")</span></span>
4. <span data-ttu-id="50b59-141">V okně jiný prohlížeč přihlaste toohello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="50b59-141">In a different browser window, sign on toohello Azure classic portal.</span></span>
5. <span data-ttu-id="50b59-142">Na hello **Coupa** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="50b59-142">On hello **Coupa** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="50b59-143">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791902.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="50b59-143">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791902.png "Configure Single Sign-On")</span></span>
6. <span data-ttu-id="50b59-144">Na hello **jak jste by například uživatelé toosign na tooCoupa** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="50b59-144">On hello **How would you like users toosign on tooCoupa** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="50b59-145">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791903.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="50b59-145">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791903.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="50b59-146">Na hello **konfigurace adresy URL aplikace** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="50b59-146">On hello **Configure App URL** page, perform hello following steps:</span></span>
   
   <span data-ttu-id="50b59-147">![Konfigurovat adresu URL aplikace](./media/active-directory-saas-coupa-tutorial/IC791904.png "konfigurovat adresu URL aplikace")</span><span class="sxs-lookup"><span data-stu-id="50b59-147">![Configure App URL](./media/active-directory-saas-coupa-tutorial/IC791904.png "Configure App URL")</span></span>   
   1. <span data-ttu-id="50b59-148">V hello **přihlašovací adresa URL** textovému poli, zadat adresu URL, které používá vaše uživatele toosign na tooyour Coupa aplikace (například: "*http://company.Coupa.com*").</span><span class="sxs-lookup"><span data-stu-id="50b59-148">In hello **Sign On URL** textbox, type URL used by your users toosign on tooyour Coupa application (e.g.: “*http://company.Coupa.com*”).</span></span>
   2. <span data-ttu-id="50b59-149">Otevřete váš stažený soubor metadat Coupa a poté zkopírujte hello **AssertionConsumerService index nebo adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="50b59-149">Open your downloaded Coupa metadata file, and then copy hello **AssertionConsumerService index/URL**.</span></span>
   3. <span data-ttu-id="50b59-150">V hello **adresa URL odpovědi Coupa** textovému poli, vložte hello **AssertionConsumerService index nebo adresa URL** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="50b59-150">In hello **Coupa Reply URL** textbox, paste hello **AssertionConsumerService index/URL** value.</span></span>
   4. <span data-ttu-id="50b59-151">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="50b59-151">Click **Next**.</span></span>
8. <span data-ttu-id="50b59-152">Na hello **nakonfigurovat jednotné přihlašování v Coupa** stránky, toodownload váš soubor metadat, klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="50b59-152">On hello **Configure single sign-on at Coupa** page, toodownload your metadata file, click **Download metadata**, and then save hello file locally on your computer.</span></span>
   
   <span data-ttu-id="50b59-153">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791905.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="50b59-153">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791905.png "Configure Single Sign-On")</span></span>
9. <span data-ttu-id="50b59-154">Na webu společnosti Coupa hello, přejděte příliš**instalace \> řízení zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="50b59-154">On hello Coupa company site, go too**Setup \> Security Control**.</span></span>
   
   <span data-ttu-id="50b59-155">![Ovládací prvky zabezpečení](./media/active-directory-saas-coupa-tutorial/IC791900.png "kontrolních mechanismů pro zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="50b59-155">![Security Controls](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")</span></span>
10. <span data-ttu-id="50b59-156">V hello **přihlásit pomocí přihlašovacích údajů Coupa** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="50b59-156">In hello **Log in using Coupa credentials** section, perform hello following steps:</span></span>  

   <span data-ttu-id="50b59-157">![Přihlaste se pomocí přihlašovacích údajů Coupa](./media/active-directory-saas-coupa-tutorial/IC791906.png "přihlásit pomocí přihlašovacích údajů Coupa")</span><span class="sxs-lookup"><span data-stu-id="50b59-157">![Log in using Coupa credentials](./media/active-directory-saas-coupa-tutorial/IC791906.png "Log in using Coupa credentials")</span></span> 
   1. <span data-ttu-id="50b59-158">Vyberte **Přihlaste se pomocí SAML**.</span><span class="sxs-lookup"><span data-stu-id="50b59-158">Select **Log in using SAML**.</span></span>
   2. <span data-ttu-id="50b59-159">Klikněte na tlačítko **Procházet** tooupload stažený Azure Active soubor metadat.</span><span class="sxs-lookup"><span data-stu-id="50b59-159">Click **Browse** tooupload your downloaded Azure Active metadata file.</span></span>
   3. <span data-ttu-id="50b59-160">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="50b59-160">Click **Save**.</span></span>
11. <span data-ttu-id="50b59-161">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="50b59-161">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
    
   <span data-ttu-id="50b59-162">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-coupa-tutorial/IC791907.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="50b59-162">![Configure Single Sign-On](./media/active-directory-saas-coupa-tutorial/IC791907.png "Configure Single Sign-On")</span></span>
    
## <a name="configure-user-provisioning"></a><span data-ttu-id="50b59-163">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="50b59-163">Configure user provisioning</span></span>

<span data-ttu-id="50b59-164">V pořadí tooenable Azure AD Uživatelé toolog do Coupa musí být zřízená do Coupa.</span><span class="sxs-lookup"><span data-stu-id="50b59-164">In order tooenable Azure AD users toolog into Coupa, they must be provisioned into Coupa.</span></span>  

* <span data-ttu-id="50b59-165">V případě hello Coupa zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="50b59-165">In hello case of Coupa, provisioning is a manual task.</span></span>

<span data-ttu-id="50b59-166">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="50b59-166">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="50b59-167">Přihlaste se tooyour **Coupa** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="50b59-167">Log in tooyour **Coupa** company site as administrator.</span></span>
2. <span data-ttu-id="50b59-168">V nabídce hello hello nahoře, klikněte na tlačítko **instalace**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="50b59-168">In hello menu on hello top, click **Setup**, and then click **Users**.</span></span>
   
   <span data-ttu-id="50b59-169">![Uživatelé](./media/active-directory-saas-coupa-tutorial/IC791908.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="50b59-169">![Users](./media/active-directory-saas-coupa-tutorial/IC791908.png "Users")</span></span>
3. <span data-ttu-id="50b59-170">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50b59-170">Click **Create**.</span></span>
   
   <span data-ttu-id="50b59-171">![Vytvoření uživatelů](./media/active-directory-saas-coupa-tutorial/IC791909.png "vytvoření uživatelů")</span><span class="sxs-lookup"><span data-stu-id="50b59-171">![Create Users](./media/active-directory-saas-coupa-tutorial/IC791909.png "Create Users")</span></span>
4. <span data-ttu-id="50b59-172">V hello **vytvořit uživateli** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="50b59-172">In hello **User Create** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="50b59-173">![Podrobné informace o uživateli](./media/active-directory-saas-coupa-tutorial/IC791910.png "podrobné informace o uživateli")</span><span class="sxs-lookup"><span data-stu-id="50b59-173">![User Details](./media/active-directory-saas-coupa-tutorial/IC791910.png "User Details")</span></span>
   
   1. <span data-ttu-id="50b59-174">Typ hello **přihlášení**, **křestní jméno**, **příjmení**, **ID přihlášení**, **e-mailu** atributy platný účet služby Azure Active Directory, že který má tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="50b59-174">Type hello **Login**, **First name**, **Last Name**, **Single Sign-On ID**, **Email** attributes of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   2. <span data-ttu-id="50b59-175">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50b59-175">Click **Create**.</span></span>   
   >[!NOTE]
   ><span data-ttu-id="50b59-176">Držitel účtu Azure Active Directory Hello dostane e-mail s účtem odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="50b59-176">hello Azure Active Directory account holder will get an email with a link tooconfirm hello account before it becomes active.</span></span> 
   > 

>[!NOTE]
><span data-ttu-id="50b59-177">Můžete použít všechny ostatní Coupa uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Coupa tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="50b59-177">You can use any other Coupa user account creation tools or APIs provided by Coupa tooprovision AAD user accounts.</span></span> 
> 

## <a name="assign-users"></a><span data-ttu-id="50b59-178">Přiřazení uživatelů</span><span class="sxs-lookup"><span data-stu-id="50b59-178">Assign users</span></span>
<span data-ttu-id="50b59-179">tootest konfiguraci, je nutné uživatele toogrant hello Azure AD můžete určit tooallow tooit přístup k vaší aplikace pomocí jejich přiřazení.</span><span class="sxs-lookup"><span data-stu-id="50b59-179">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

<span data-ttu-id="50b59-180">**tooassign tooCoupa uživatele, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="50b59-180">**tooassign users tooCoupa, perform hello following steps:**</span></span>

1. <span data-ttu-id="50b59-181">V hello portál Azure classic vytvořte testovací účet.</span><span class="sxs-lookup"><span data-stu-id="50b59-181">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="50b59-182">Na hello ** Coupa ** stránky integrace aplikací, klikněte na tlačítko **přiřazení uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="50b59-182">On hello **Coupa **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="50b59-183">![Přiřazení uživatelů](./media/active-directory-saas-coupa-tutorial/IC791911.png "přiřazení uživatelů")</span><span class="sxs-lookup"><span data-stu-id="50b59-183">![Assign Users](./media/active-directory-saas-coupa-tutorial/IC791911.png "Assign Users")</span></span>
3. <span data-ttu-id="50b59-184">Vyberte svého testovacího uživatele, klikněte na **přiřadit**a potom klikněte na **Ano** tooconfirm vaše přiřazení.</span><span class="sxs-lookup"><span data-stu-id="50b59-184">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="50b59-185">![Ano](./media/active-directory-saas-coupa-tutorial/IC767830.png "Ano")</span><span class="sxs-lookup"><span data-stu-id="50b59-185">![Yes](./media/active-directory-saas-coupa-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="50b59-186">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="50b59-186">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="50b59-187">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="50b59-187">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

