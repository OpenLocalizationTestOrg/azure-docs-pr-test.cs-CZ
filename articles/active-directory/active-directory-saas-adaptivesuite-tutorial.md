---
title: "Kurz: Azure Active Directory integrace s adaptivní Suite | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a adaptivní Suite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 5d7ba2f4c7d814e3aaa1bf804ddc5030380ccb2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a><span data-ttu-id="05766-103">Kurz: Azure Active Directory integrace s adaptivní Suite</span><span class="sxs-lookup"><span data-stu-id="05766-103">Tutorial: Azure Active Directory integration with Adaptive Suite</span></span>

<span data-ttu-id="05766-104">V tomto kurzu zjistěte, jak integrovat adaptivní Suite s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="05766-104">In this tutorial, you learn how to integrate Adaptive Suite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="05766-105">Integrace s Azure AD adaptivní Suite nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="05766-105">Integrating Adaptive Suite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="05766-106">Můžete řídit ve službě Azure AD, který má přístup k adaptivní Suite</span><span class="sxs-lookup"><span data-stu-id="05766-106">You can control in Azure AD who has access to Adaptive Suite</span></span>
- <span data-ttu-id="05766-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k adaptivní Suite (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="05766-107">You can enable your users to automatically get signed-on to Adaptive Suite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="05766-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="05766-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="05766-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="05766-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05766-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="05766-110">Prerequisites</span></span>

<span data-ttu-id="05766-111">Konfigurace integrace Azure AD s adaptivní Suite, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="05766-111">To configure Azure AD integration with Adaptive Suite, you need the following items:</span></span>

- <span data-ttu-id="05766-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="05766-112">An Azure AD subscription</span></span>
- <span data-ttu-id="05766-113">Adaptivní Suite jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="05766-113">An Adaptive Suite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="05766-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="05766-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="05766-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="05766-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="05766-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="05766-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="05766-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="05766-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="05766-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="05766-118">Scenario description</span></span>
<span data-ttu-id="05766-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="05766-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="05766-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="05766-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="05766-121">Přidání adaptivní Suite z Galerie</span><span class="sxs-lookup"><span data-stu-id="05766-121">Adding Adaptive Suite from the gallery</span></span>
2. <span data-ttu-id="05766-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="05766-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adaptive-suite-from-the-gallery"></a><span data-ttu-id="05766-123">Přidání adaptivní Suite z Galerie</span><span class="sxs-lookup"><span data-stu-id="05766-123">Adding Adaptive Suite from the gallery</span></span>
<span data-ttu-id="05766-124">Pokud chcete nakonfigurovat integraci sady adaptivní do služby Azure AD, potřebujete přidat adaptivní Suite z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="05766-124">To configure the integration of Adaptive Suite into Azure AD, you need to add Adaptive Suite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="05766-125">**Pokud chcete přidat adaptivní Suite z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="05766-125">**To add Adaptive Suite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="05766-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="05766-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="05766-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="05766-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="05766-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="05766-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="05766-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="05766-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="05766-133">Do vyhledávacího pole zadejte **adaptivní Suite**.</span><span class="sxs-lookup"><span data-stu-id="05766-133">In the search box, type **Adaptive Suite**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. <span data-ttu-id="05766-135">Na panelu výsledků vyberte **adaptivní Suite**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="05766-135">In the results panel, select **Adaptive Suite**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="05766-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="05766-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="05766-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s adaptivní Suite podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="05766-138">In this section, you configure and test Azure AD single sign-on with Adaptive Suite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="05766-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v sadě adaptivní je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="05766-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adaptive Suite is to a user in Azure AD.</span></span> <span data-ttu-id="05766-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v sadě adaptivní musí navázat.</span><span class="sxs-lookup"><span data-stu-id="05766-140">In other words, a link relationship between an Azure AD user and the related user in Adaptive Suite needs to be established.</span></span>

<span data-ttu-id="05766-141">V adaptivní Suite přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="05766-141">In Adaptive Suite, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="05766-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s adaptivní Suite, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="05766-142">To configure and test Azure AD single sign-on with Adaptive Suite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="05766-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="05766-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="05766-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="05766-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="05766-145">**[Vytváření testovacího uživatele adaptivní Suite](#creating-an-adaptive-suite-test-user)**  – Pokud chcete mít protějšek Britta Simon adaptivní sady, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="05766-145">**[Creating an Adaptive Suite test user](#creating-an-adaptive-suite-test-user)** - to have a counterpart of Britta Simon in Adaptive Suite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="05766-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="05766-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="05766-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="05766-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="05766-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="05766-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="05766-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci adaptivní Suite.</span><span class="sxs-lookup"><span data-stu-id="05766-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Adaptive Suite application.</span></span>

<span data-ttu-id="05766-150">**Ke konfiguraci Azure AD jednotné přihlašování s adaptivní Suite, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="05766-150">**To configure Azure AD single sign-on with Adaptive Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="05766-151">Na portálu Azure na **adaptivní Suite** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="05766-151">In the Azure portal, on the **Adaptive Suite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="05766-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="05766-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. <span data-ttu-id="05766-155">Na **adaptivní Suite domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="05766-155">On the **Adaptive Suite Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    <span data-ttu-id="05766-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span><span class="sxs-lookup"><span data-stu-id="05766-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span></span>

    >[!NOTE]
    > <span data-ttu-id="05766-158">Tuto hodnotu můžete získat z adaptivní sady **nastavení jednotného přihlašování SAML** stránky.</span><span class="sxs-lookup"><span data-stu-id="05766-158">You can get this value from the Adaptive Suite’s **SAML SSO Settings** page.</span></span>
    >  

4. <span data-ttu-id="05766-159">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="05766-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. <span data-ttu-id="05766-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="05766-161">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="05766-163">Na **adaptivní Suite konfigurace** klikněte na tlačítko **konfigurace adaptivní Suite** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="05766-163">On the **Adaptive Suite Configuration** section, click **Configure Adaptive Suite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="05766-164">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="05766-164">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. <span data-ttu-id="05766-166">V okně prohlížeče jiný web Přihlaste se na váš web společnosti adaptivní Suite jako správce.</span><span class="sxs-lookup"><span data-stu-id="05766-166">In a different web browser window, log in to your Adaptive Suite company site as an administrator.</span></span>

8. <span data-ttu-id="05766-167">Přejděte na **správce**.</span><span class="sxs-lookup"><span data-stu-id="05766-167">Go to **Admin**.</span></span>
   
    <span data-ttu-id="05766-168">![Správce](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "správce")</span><span class="sxs-lookup"><span data-stu-id="05766-168">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>

9. <span data-ttu-id="05766-169">V **uživatelů a rolí** klikněte na tlačítko **spravovat nastavení jednotného přihlašování SAML**.</span><span class="sxs-lookup"><span data-stu-id="05766-169">In the **Users and Roles** section, click **Manage SAML SSO Settings**.</span></span>
   
    <span data-ttu-id="05766-170">![Spravovat nastavení jednotného přihlašování SAML](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "spravovat nastavení jednotného přihlašování SAML")</span><span class="sxs-lookup"><span data-stu-id="05766-170">![Manage SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "Manage SAML SSO Settings")</span></span>

10. <span data-ttu-id="05766-171">Na **nastavení jednotného přihlašování SAML** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="05766-171">On the **SAML SSO Settings** page, perform the following steps:</span></span>
   
    <span data-ttu-id="05766-172">![Nastavení jednotného přihlašování SAML](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "nastavení jednotného přihlašování SAML")</span><span class="sxs-lookup"><span data-stu-id="05766-172">![SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO Settings")</span></span>

    <span data-ttu-id="05766-173">a.</span><span class="sxs-lookup"><span data-stu-id="05766-173">a.</span></span> <span data-ttu-id="05766-174">V **název zprostředkovatele Identity** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="05766-174">In the **Identity provider name** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="05766-175">b.</span><span class="sxs-lookup"><span data-stu-id="05766-175">b.</span></span> <span data-ttu-id="05766-176">Vložení **SAML Entity ID** hodnota zkopírována z portálu Azure do **zprostředkovatele Identity Entity ID** textové pole.</span><span class="sxs-lookup"><span data-stu-id="05766-176">Paste the **SAML Entity ID** value copied from Azure portal into the **Identity provider Entity ID** textbox.</span></span>
  
    <span data-ttu-id="05766-177">c.</span><span class="sxs-lookup"><span data-stu-id="05766-177">c.</span></span> <span data-ttu-id="05766-178">Vložení **SAML jeden přihlašování adresa URL služby** hodnota zkopírována z portálu Azure do **zprostředkovatele Identity jednotného přihlašování k adrese URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="05766-178">Paste the **SAML Single Sign-On Service URL** value copied from Azure portal into the **Identity provider SSO URL** textbox.</span></span>
  
    <span data-ttu-id="05766-179">d.</span><span class="sxs-lookup"><span data-stu-id="05766-179">d.</span></span> <span data-ttu-id="05766-180">Vložení **SAML jeden přihlašování adresa URL služby** hodnota zkopírována z portálu Azure do **vlastní adresa URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="05766-180">Paste the **SAML Single Sign-On Service URL** value copied from Azure portal into the **Custom logout URL** textbox.</span></span>
  
    <span data-ttu-id="05766-181">e.</span><span class="sxs-lookup"><span data-stu-id="05766-181">e.</span></span> <span data-ttu-id="05766-182">Chcete-li nahrát stažený certifikát, klikněte na tlačítko **zvolte soubor**.</span><span class="sxs-lookup"><span data-stu-id="05766-182">To upload your downloaded certificate, click **Choose file**.</span></span>
  
    <span data-ttu-id="05766-183">f.</span><span class="sxs-lookup"><span data-stu-id="05766-183">f.</span></span> <span data-ttu-id="05766-184">Vyberte pro následující:</span><span class="sxs-lookup"><span data-stu-id="05766-184">Select the following, for:</span></span>
    * <span data-ttu-id="05766-185">**Id uživatele SAML**, vyberte **adaptivní Statistika uživatelského jména**.</span><span class="sxs-lookup"><span data-stu-id="05766-185">**SAML user id**, select **User’s Adaptive Insights user name**.</span></span>
    * <span data-ttu-id="05766-186">**Umístění id uživatele SAML**, vyberte **id uživatele v NameID předmětu**.</span><span class="sxs-lookup"><span data-stu-id="05766-186">**SAML user id location**, select **User id in NameID of Subject**.</span></span>
    * <span data-ttu-id="05766-187">**Formát SAML NameID**, vyberte **e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="05766-187">**SAML NameID format**, select **Email address**.</span></span>
    * <span data-ttu-id="05766-188">**Povolit SAML**, vyberte **povolit jednotné přihlašování SAML a přímé přihlášení adaptivní Insights**.</span><span class="sxs-lookup"><span data-stu-id="05766-188">**Enable SAML**, select **Allow SAML SSO and direct Adaptive Insights login**.</span></span>
    
    <span data-ttu-id="05766-189">g.</span><span class="sxs-lookup"><span data-stu-id="05766-189">g.</span></span> <span data-ttu-id="05766-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="05766-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="05766-191">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="05766-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="05766-192">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="05766-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="05766-193">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="05766-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="05766-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="05766-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="05766-195">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="05766-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="05766-197">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="05766-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="05766-198">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="05766-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="05766-200">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="05766-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="05766-202">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="05766-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="05766-204">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="05766-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="05766-206">a.</span><span class="sxs-lookup"><span data-stu-id="05766-206">a.</span></span> <span data-ttu-id="05766-207">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="05766-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="05766-208">b.</span><span class="sxs-lookup"><span data-stu-id="05766-208">b.</span></span> <span data-ttu-id="05766-209">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="05766-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="05766-210">c.</span><span class="sxs-lookup"><span data-stu-id="05766-210">c.</span></span> <span data-ttu-id="05766-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="05766-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="05766-212">d.</span><span class="sxs-lookup"><span data-stu-id="05766-212">d.</span></span> <span data-ttu-id="05766-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="05766-213">Click **Create**.</span></span>
 
### <a name="creating-an-adaptive-suite-test-user"></a><span data-ttu-id="05766-214">Vytváření testovacího uživatele adaptivní Suite</span><span class="sxs-lookup"><span data-stu-id="05766-214">Creating an Adaptive Suite test user</span></span>

<span data-ttu-id="05766-215">Pokud chcete povolit uživatelům Azure AD přihlášení k adaptivní Suite, musí být zřízená do adaptivní Suite.</span><span class="sxs-lookup"><span data-stu-id="05766-215">To enable Azure AD users to log in to Adaptive Suite, they must be provisioned into Adaptive Suite.</span></span>  

* <span data-ttu-id="05766-216">V případě adaptivní Suite zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="05766-216">In the case of Adaptive Suite, provisioning is a manual task.</span></span>

<span data-ttu-id="05766-217">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="05766-217">**To configure user provisioning, perform the following steps:**</span></span> 

1. <span data-ttu-id="05766-218">Přihlaste se k vaší **adaptivní Suite** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="05766-218">Log in to your **Adaptive Suite** company site as an administrator.</span></span>
2. <span data-ttu-id="05766-219">Přejděte na **správce**.</span><span class="sxs-lookup"><span data-stu-id="05766-219">Go to **Admin**.</span></span>
   
   <span data-ttu-id="05766-220">![Správce](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "správce")</span><span class="sxs-lookup"><span data-stu-id="05766-220">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>
3. <span data-ttu-id="05766-221">V **uživatelů a rolí** klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="05766-221">In the **Users and Roles** section, click **Add User**.</span></span>
   
   <span data-ttu-id="05766-222">![Přidat uživatele](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="05766-222">![Add User](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "Add User")</span></span>
4. <span data-ttu-id="05766-223">V **nového uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="05766-223">In the **New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="05766-224">![Odeslání](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "odeslání")</span><span class="sxs-lookup"><span data-stu-id="05766-224">![Submit](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "Submit")</span></span>   

   <span data-ttu-id="05766-225">a.</span><span class="sxs-lookup"><span data-stu-id="05766-225">a.</span></span> <span data-ttu-id="05766-226">Typ **název**, **přihlášení**, **e-mailu**, **heslo** platný Azure Active Directory uživatele, který chcete mají být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="05766-226">Type the **Name**, **Login**, **Email**, **Password** of a valid Azure Active Directory user you want to provision into the related textboxes.</span></span>
  
   <span data-ttu-id="05766-227">b.</span><span class="sxs-lookup"><span data-stu-id="05766-227">b.</span></span> <span data-ttu-id="05766-228">Vyberte **Role**.</span><span class="sxs-lookup"><span data-stu-id="05766-228">Select a **Role**.</span></span>
  
   <span data-ttu-id="05766-229">c.</span><span class="sxs-lookup"><span data-stu-id="05766-229">c.</span></span> <span data-ttu-id="05766-230">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="05766-230">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="05766-231">Můžete použít všechny ostatní adaptivní Suite uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované adaptivní Suite zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="05766-231">You can use any other Adaptive Suite user account creation tools or APIs provided by Adaptive Suite to provision AAD user accounts.</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="05766-232">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="05766-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="05766-233">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k adaptivní Suite.</span><span class="sxs-lookup"><span data-stu-id="05766-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Adaptive Suite.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="05766-235">**Pokud chcete přiřadit Britta Simon adaptivní Suite, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="05766-235">**To assign Britta Simon to Adaptive Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="05766-236">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="05766-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="05766-238">V seznamu aplikací vyberte **adaptivní Suite**.</span><span class="sxs-lookup"><span data-stu-id="05766-238">In the applications list, select **Adaptive Suite**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. <span data-ttu-id="05766-240">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="05766-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="05766-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="05766-242">Click **Add** button.</span></span> <span data-ttu-id="05766-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="05766-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="05766-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="05766-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="05766-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="05766-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="05766-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="05766-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="05766-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="05766-248">Testing single sign-on</span></span>

<span data-ttu-id="05766-249">Cílem této části je vyzkoušet Microsoft Azure AD jednotné přihlašování v konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="05766-249">The objective of this section is to test your Microsoft Azure AD Single Sign-On configuration using the Access Panel.</span></span>

<span data-ttu-id="05766-250">Když kliknete na dlaždici adaptivní Suite na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci adaptivní Suite.</span><span class="sxs-lookup"><span data-stu-id="05766-250">When you click the Adaptive Suite tile in the Access Panel, you should get automatically signed-on to your Adaptive Suite application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="05766-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="05766-251">Additional resources</span></span>

* [<span data-ttu-id="05766-252">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="05766-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="05766-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="05766-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

