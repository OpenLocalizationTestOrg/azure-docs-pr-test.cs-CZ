---
title: 'Kurz: Azure Active Directory integrace s Rightscale | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Rightscale."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 222c4414a9f736a3589b4cdd0ed934696f6c31ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a><span data-ttu-id="5846a-103">Kurz: Azure Active Directory integrace s Rightscale</span><span class="sxs-lookup"><span data-stu-id="5846a-103">Tutorial: Azure Active Directory integration with Rightscale</span></span>

<span data-ttu-id="5846a-104">V tomto kurzu zjistěte, jak integrovat Rightscale s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5846a-104">In this tutorial, you learn how to integrate Rightscale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5846a-105">Integrace Rightscale s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5846a-105">Integrating Rightscale with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5846a-106">Můžete řídit ve službě Azure AD, který má přístup k Rightscale</span><span class="sxs-lookup"><span data-stu-id="5846a-106">You can control in Azure AD who has access to Rightscale</span></span>
- <span data-ttu-id="5846a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Rightscale (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5846a-107">You can enable your users to automatically get signed-on to Rightscale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5846a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5846a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5846a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5846a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5846a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5846a-110">Prerequisites</span></span>

<span data-ttu-id="5846a-111">Konfigurace integrace Azure AD s Rightscale, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5846a-111">To configure Azure AD integration with Rightscale, you need the following items:</span></span>

- <span data-ttu-id="5846a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5846a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5846a-113">Rightscale jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5846a-113">A Rightscale single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5846a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5846a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5846a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5846a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5846a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5846a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5846a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5846a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5846a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5846a-118">Scenario description</span></span>
<span data-ttu-id="5846a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5846a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5846a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5846a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5846a-121">Přidání Rightscale z Galerie</span><span class="sxs-lookup"><span data-stu-id="5846a-121">Adding Rightscale from the gallery</span></span>
2. <span data-ttu-id="5846a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5846a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightscale-from-the-gallery"></a><span data-ttu-id="5846a-123">Přidání Rightscale z Galerie</span><span class="sxs-lookup"><span data-stu-id="5846a-123">Adding Rightscale from the gallery</span></span>
<span data-ttu-id="5846a-124">Při konfiguraci integrace Rightscale do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Rightscale z galerie.</span><span class="sxs-lookup"><span data-stu-id="5846a-124">To configure the integration of Rightscale into Azure AD, you need to add Rightscale from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5846a-125">**Pokud chcete přidat Rightscale z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5846a-125">**To add Rightscale from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5846a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5846a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5846a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5846a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5846a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5846a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5846a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5846a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5846a-133">Do vyhledávacího pole zadejte **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="5846a-133">In the search box, type **Rightscale**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_search.png)

5. <span data-ttu-id="5846a-135">Na panelu výsledků vyberte **Rightscale**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5846a-135">In the results panel, select **Rightscale**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5846a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5846a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5846a-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Rightscale podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5846a-138">In this section, you configure and test Azure AD single sign-on with Rightscale based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5846a-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Rightscale je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5846a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Rightscale is to a user in Azure AD.</span></span> <span data-ttu-id="5846a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Rightscale musí navázat.</span><span class="sxs-lookup"><span data-stu-id="5846a-140">In other words, a link relationship between an Azure AD user and the related user in Rightscale needs to be established.</span></span>

<span data-ttu-id="5846a-141">V Rightscale, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5846a-141">In Rightscale, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5846a-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Rightscale, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5846a-142">To configure and test Azure AD single sign-on with Rightscale, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5846a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5846a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5846a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5846a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5846a-145">**[Vytvoření zkušebního uživatele Rightscale](#creating-a-rightscale-test-user)**  – Pokud chcete mít protějšek Britta Simon v Rightscale propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5846a-145">**[Creating a Rightscale test user](#creating-a-rightscale-test-user)** - to have a counterpart of Britta Simon in Rightscale that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5846a-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5846a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5846a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5846a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5846a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5846a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5846a-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Rightscale.</span><span class="sxs-lookup"><span data-stu-id="5846a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Rightscale application.</span></span>

<span data-ttu-id="5846a-150">**Ke konfiguraci Azure AD jednotné přihlašování s Rightscale, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5846a-150">**To configure Azure AD single sign-on with Rightscale, perform the following steps:**</span></span>

1. <span data-ttu-id="5846a-151">Na portálu Azure na **Rightscale** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5846a-151">In the Azure portal, on the **Rightscale** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5846a-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5846a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_samlbase.png)

3. <span data-ttu-id="5846a-155">Na **Rightscale domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP iniciované režimu** není nutné provádět žádné kroky, protože aplikace je již předem integrované se službou Azure.</span><span class="sxs-lookup"><span data-stu-id="5846a-155">On the **Rightscale Domain and URLs** section, if you wish to configure the application in **IDP initiated mode** you do not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url.png)

4. <span data-ttu-id="5846a-157">Na **Rightscale domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **SP iniciované režimu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5846a-157">On the **Rightscale Domain and URLs** section, if you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url1.png)

    <span data-ttu-id="5846a-159">a.</span><span class="sxs-lookup"><span data-stu-id="5846a-159">a.</span></span> <span data-ttu-id="5846a-160">Klikněte na **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="5846a-160">Click on the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="5846a-161">b.</span><span class="sxs-lookup"><span data-stu-id="5846a-161">b.</span></span> <span data-ttu-id="5846a-162">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://login.rightscale.com/`</span><span class="sxs-lookup"><span data-stu-id="5846a-162">In the **Sign On URL** textbox, type the URL: `https://login.rightscale.com/`</span></span>

5. <span data-ttu-id="5846a-163">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="5846a-163">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_certificate.png) 

6. <span data-ttu-id="5846a-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5846a-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="5846a-167">Na **Rightscale konfigurace** klikněte na tlačítko **konfigurace Rightscale** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5846a-167">On the **Rightscale Configuration** section, click **Configure Rightscale** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5846a-168">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5846a-168">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    <span data-ttu-id="5846a-169">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="5846a-169">![Configure Single Sign-On](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span></span>
8. <span data-ttu-id="5846a-170">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte k přihlašování ke klientovi RightScale jako správce.</span><span class="sxs-lookup"><span data-stu-id="5846a-170">To get SSO configured for your application, you need to sign-on to your RightScale tenant as an administrator.</span></span>

    <span data-ttu-id="5846a-171">a.</span><span class="sxs-lookup"><span data-stu-id="5846a-171">a.</span></span> <span data-ttu-id="5846a-172">V nabídce v horní části, klikněte **nastavení** a vyberte **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5846a-172">In the menu on the top, click the **Settings** tab and select **Single Sign-On**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    <span data-ttu-id="5846a-174">b.</span><span class="sxs-lookup"><span data-stu-id="5846a-174">b.</span></span> <span data-ttu-id="5846a-175">Klikněte "**nové**" tlačítko Přidat **vaše zprostředkovatelů Identity SAML**.</span><span class="sxs-lookup"><span data-stu-id="5846a-175">Click the "**new**" button to add **Your SAML Identity Providers**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
 
    <span data-ttu-id="5846a-177">c.</span><span class="sxs-lookup"><span data-stu-id="5846a-177">c.</span></span> <span data-ttu-id="5846a-178">Do textového pole z **zobrazovaný název**, zadejte název vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="5846a-178">In the textbox of **Display Name**, input your company name.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
 
    <span data-ttu-id="5846a-180">d.</span><span class="sxs-lookup"><span data-stu-id="5846a-180">d.</span></span> <span data-ttu-id="5846a-181">Vyberte **spouštěná RightScale povolit jednotné přihlašování pomocí zjišťování nápovědu** a vstupní vaše **název domény** v pod textové pole.</span><span class="sxs-lookup"><span data-stu-id="5846a-181">Select **Allow RightScale-initiated SSO using a discovery hint** and input your **domain name** in the below textbox.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    <span data-ttu-id="5846a-183">e.</span><span class="sxs-lookup"><span data-stu-id="5846a-183">e.</span></span> <span data-ttu-id="5846a-184">Vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure do **koncového bodu jednotného přihlašování SAML** v RightScale.</span><span class="sxs-lookup"><span data-stu-id="5846a-184">Paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal into **SAML SSO Endpoint** in RightScale.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    <span data-ttu-id="5846a-186">f.</span><span class="sxs-lookup"><span data-stu-id="5846a-186">f.</span></span> <span data-ttu-id="5846a-187">Vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure do **SAML EntityID** v RightScale.</span><span class="sxs-lookup"><span data-stu-id="5846a-187">Paste the value of **SAML Entity ID** which you have copied from Azure portal into **SAML EntityID** in RightScale.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    <span data-ttu-id="5846a-189">g.</span><span class="sxs-lookup"><span data-stu-id="5846a-189">g.</span></span> <span data-ttu-id="5846a-190">Klikněte na tlačítko **prohlížeče** tlačítko odeslat certifikát, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5846a-190">Click **Browser** button to upload the certificate which you downloaded from Azure portal.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    <span data-ttu-id="5846a-192">h.</span><span class="sxs-lookup"><span data-stu-id="5846a-192">h.</span></span> <span data-ttu-id="5846a-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5846a-193">Click **Save**.</span></span>
<CE>
> [!TIP]
> <span data-ttu-id="5846a-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5846a-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5846a-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5846a-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5846a-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5846a-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5846a-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5846a-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="5846a-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5846a-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5846a-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5846a-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5846a-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5846a-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5846a-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5846a-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5846a-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5846a-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5846a-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5846a-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5846a-209">a.</span><span class="sxs-lookup"><span data-stu-id="5846a-209">a.</span></span> <span data-ttu-id="5846a-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5846a-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5846a-211">b.</span><span class="sxs-lookup"><span data-stu-id="5846a-211">b.</span></span> <span data-ttu-id="5846a-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5846a-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5846a-213">c.</span><span class="sxs-lookup"><span data-stu-id="5846a-213">c.</span></span> <span data-ttu-id="5846a-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5846a-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5846a-215">d.</span><span class="sxs-lookup"><span data-stu-id="5846a-215">d.</span></span> <span data-ttu-id="5846a-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5846a-216">Click **Create**.</span></span>
 
### <a name="creating-a-rightscale-test-user"></a><span data-ttu-id="5846a-217">Vytvoření zkušebního uživatele Rightscale</span><span class="sxs-lookup"><span data-stu-id="5846a-217">Creating a Rightscale test user</span></span>

<span data-ttu-id="5846a-218">V této části vytvoříte volal Britta Simon v RightScale uživatele.</span><span class="sxs-lookup"><span data-stu-id="5846a-218">In this section, you create a user called Britta Simon in RightScale.</span></span> <span data-ttu-id="5846a-219">Práce s [tým podpory Rightscale klienta](mailto:support@rightscale.com) přidat uživatele do RightScale platformy.</span><span class="sxs-lookup"><span data-stu-id="5846a-219">Work with [Rightscale Client support team](mailto:support@rightscale.com) to add the users in the RightScale platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5846a-220">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5846a-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5846a-221">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Rightscale.</span><span class="sxs-lookup"><span data-stu-id="5846a-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Rightscale.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5846a-223">**Pokud chcete přiřadit Britta Simon Rightscale, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5846a-223">**To assign Britta Simon to Rightscale, perform the following steps:**</span></span>

1. <span data-ttu-id="5846a-224">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5846a-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5846a-226">V seznamu aplikací vyberte **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="5846a-226">In the applications list, select **Rightscale**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_app.png) 

3. <span data-ttu-id="5846a-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5846a-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5846a-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5846a-230">Click **Add** button.</span></span> <span data-ttu-id="5846a-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5846a-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5846a-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5846a-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5846a-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5846a-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5846a-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5846a-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5846a-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5846a-236">Testing single sign-on</span></span>

<span data-ttu-id="5846a-237">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="5846a-237">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="5846a-238">Když kliknete na dlaždici RightScale na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci RightScale.</span><span class="sxs-lookup"><span data-stu-id="5846a-238">When you click the RightScale tile in the Access Panel, you should get automatically signed-on to your RightScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5846a-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5846a-239">Additional resources</span></span>

* [<span data-ttu-id="5846a-240">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5846a-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5846a-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5846a-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png

