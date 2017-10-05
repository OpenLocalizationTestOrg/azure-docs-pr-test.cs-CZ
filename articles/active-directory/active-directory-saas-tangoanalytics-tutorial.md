---
title: 'Kurz: Azure Active Directory integrace s Tango Analytics | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Tango analýzy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2f7555d3-e9ba-40b2-9b3a-2f0ab38a4c08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 46f6facc3c86630a9252340b2e89634368c0263d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tango-analytics"></a><span data-ttu-id="3f847-103">Kurz: Azure Active Directory integrace s Tango Analytics</span><span class="sxs-lookup"><span data-stu-id="3f847-103">Tutorial: Azure Active Directory integration with Tango Analytics</span></span>

<span data-ttu-id="3f847-104">V tomto kurzu zjistěte, jak integrovat Tango Analytics s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3f847-104">In this tutorial, you learn how to integrate Tango Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f847-105">Integrace s Azure AD Tango Analytics poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3f847-105">Integrating Tango Analytics with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3f847-106">Můžete řídit ve službě Azure AD, který má přístup k analýze Tango</span><span class="sxs-lookup"><span data-stu-id="3f847-106">You can control in Azure AD who has access to Tango Analytics</span></span>
- <span data-ttu-id="3f847-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k analýze Tango (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f847-107">You can enable your users to automatically get signed-on to Tango Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f847-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3f847-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3f847-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f847-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f847-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3f847-110">Prerequisites</span></span>

<span data-ttu-id="3f847-111">Ke konfiguraci integrace služby Azure AD s Tango Analytics, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3f847-111">To configure Azure AD integration with Tango Analytics, you need the following items:</span></span>

- <span data-ttu-id="3f847-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f847-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f847-113">Analýzy Tango jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3f847-113">A Tango Analytics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f847-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3f847-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f847-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3f847-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f847-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3f847-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f847-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f847-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f847-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3f847-118">Scenario description</span></span>
<span data-ttu-id="3f847-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3f847-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f847-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3f847-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f847-121">Přidání Tango Analytics z Galerie</span><span class="sxs-lookup"><span data-stu-id="3f847-121">Adding Tango Analytics from the gallery</span></span>
2. <span data-ttu-id="3f847-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f847-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tango-analytics-from-the-gallery"></a><span data-ttu-id="3f847-123">Přidání Tango Analytics z Galerie</span><span class="sxs-lookup"><span data-stu-id="3f847-123">Adding Tango Analytics from the gallery</span></span>
<span data-ttu-id="3f847-124">Při konfiguraci integrace Tango Analytics do služby Azure AD, potřebujete přidat Tango Analytics z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3f847-124">To configure the integration of Tango Analytics into Azure AD, you need to add Tango Analytics from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3f847-125">**Pokud chcete přidat Tango Analytics z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3f847-125">**To add Tango Analytics from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3f847-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3f847-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f847-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3f847-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3f847-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3f847-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3f847-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f847-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3f847-133">Do vyhledávacího pole zadejte **Tango Analytics**.</span><span class="sxs-lookup"><span data-stu-id="3f847-133">In the search box, type **Tango Analytics**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_search.png)

5. <span data-ttu-id="3f847-135">Na panelu výsledků vyberte **Tango Analytics**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3f847-135">In the results panel, select **Tango Analytics**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f847-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f847-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f847-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tango Analytics podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3f847-138">In this section, you configure and test Azure AD single sign-on with Tango Analytics based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3f847-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v Tango Analytics je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f847-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tango Analytics is to a user in Azure AD.</span></span> <span data-ttu-id="3f847-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Tango Analytics musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3f847-140">In other words, a link relationship between an Azure AD user and the related user in Tango Analytics needs to be established.</span></span>

<span data-ttu-id="3f847-141">V Tango Analytics přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3f847-141">In Tango Analytics, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3f847-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tango Analytics, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3f847-142">To configure and test Azure AD single sign-on with Tango Analytics, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3f847-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3f847-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3f847-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f847-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f847-145">**[Vytvoření zkušebního uživatele Tango Analytics](#creating-a-tango-analytics-test-user)**  – Pokud chcete mít protějšek Britta Simon v Tango analýz, které je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f847-145">**[Creating a Tango Analytics test user](#creating-a-tango-analytics-test-user)** - to have a counterpart of Britta Simon in Tango Analytics that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f847-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3f847-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f847-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3f847-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f847-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f847-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f847-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Tango Analytics.</span><span class="sxs-lookup"><span data-stu-id="3f847-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tango Analytics application.</span></span>

<span data-ttu-id="3f847-150">**Ke konfiguraci Azure AD jednotné přihlašování s Tango Analytics, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3f847-150">**To configure Azure AD single sign-on with Tango Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="3f847-151">Na portálu Azure na **Tango Analytics** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3f847-151">In the Azure portal, on the **Tango Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3f847-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3f847-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_samlbase.png)

3. <span data-ttu-id="3f847-155">Na **Tango Analytics domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3f847-155">On the **Tango Analytics Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_url.png)

    <span data-ttu-id="3f847-157">a.</span><span class="sxs-lookup"><span data-stu-id="3f847-157">a.</span></span> <span data-ttu-id="3f847-158">V **identifikátor** textovému poli, zadejte hodnotu`TACORE_SSO`</span><span class="sxs-lookup"><span data-stu-id="3f847-158">In the **Identifier** textbox, type the value `TACORE_SSO`</span></span>

    <span data-ttu-id="3f847-159">b.</span><span class="sxs-lookup"><span data-stu-id="3f847-159">b.</span></span> <span data-ttu-id="3f847-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://mts.tangoanalytics.com/saml2/sp/acs/post`</span><span class="sxs-lookup"><span data-stu-id="3f847-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://mts.tangoanalytics.com/saml2/sp/acs/post`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f847-161">Adresa URL odpovědi hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="3f847-161">The Reply URL value is not real.</span></span> <span data-ttu-id="3f847-162">Aktualizujte s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3f847-162">Update this with the actual Reply URL.</span></span> <span data-ttu-id="3f847-163">Obraťte se na [Tango Analytics podporovat team](mailto:support@tangoanalytics.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3f847-163">Contact [Tango Analytics support team](mailto:support@tangoanalytics.com) to get this value.</span></span>

4. <span data-ttu-id="3f847-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3f847-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_certificate.png) 

5. <span data-ttu-id="3f847-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3f847-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f847-168">Konfigurace jednotného přihlašování na **Tango Analytics** straně, budete muset odeslat stažené **soubor XML s metadaty** k [Tango Analytics podporovat team](mailto:support@tangoanalytics.com).</span><span class="sxs-lookup"><span data-stu-id="3f847-168">To configure single sign-on on **Tango Analytics** side, you need to send the downloaded **Metadata XML** to [Tango Analytics support team](mailto:support@tangoanalytics.com).</span></span> <span data-ttu-id="3f847-169">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="3f847-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3f847-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3f847-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3f847-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3f847-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3f847-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f847-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f847-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f847-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f847-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f847-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3f847-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3f847-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3f847-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3f847-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f847-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3f847-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f847-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f847-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f847-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3f847-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f847-185">a.</span><span class="sxs-lookup"><span data-stu-id="3f847-185">a.</span></span> <span data-ttu-id="3f847-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f847-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f847-187">b.</span><span class="sxs-lookup"><span data-stu-id="3f847-187">b.</span></span> <span data-ttu-id="3f847-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f847-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f847-189">c.</span><span class="sxs-lookup"><span data-stu-id="3f847-189">c.</span></span> <span data-ttu-id="3f847-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3f847-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3f847-191">d.</span><span class="sxs-lookup"><span data-stu-id="3f847-191">d.</span></span> <span data-ttu-id="3f847-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3f847-192">Click **Create**.</span></span>
 
### <a name="creating-a-tango-analytics-test-user"></a><span data-ttu-id="3f847-193">Vytvoření zkušebního uživatele Tango Analytics</span><span class="sxs-lookup"><span data-stu-id="3f847-193">Creating a Tango Analytics test user</span></span>

<span data-ttu-id="3f847-194">V této části vytvoříte uživatele volal Britta Simon v Tango Analytics.</span><span class="sxs-lookup"><span data-stu-id="3f847-194">In this section, you create a user called Britta Simon in Tango Analytics.</span></span> <span data-ttu-id="3f847-195">Práce s [Tango Analytics podporovat team](mailto:support@tangoanalytics.com) přidat uživatele do Tango Analytics platform.</span><span class="sxs-lookup"><span data-stu-id="3f847-195">Work with [Tango Analytics support team](mailto:support@tangoanalytics.com) to add the users in the Tango Analytics platform.</span></span> <span data-ttu-id="3f847-196">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3f847-196">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3f847-197">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f847-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3f847-198">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k analýze Tango.</span><span class="sxs-lookup"><span data-stu-id="3f847-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tango Analytics.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3f847-200">**Pokud chcete přiřadit Britta Simon Tango analýzy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3f847-200">**To assign Britta Simon to Tango Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="3f847-201">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3f847-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3f847-203">V seznamu aplikací vyberte **Tango Analytics**.</span><span class="sxs-lookup"><span data-stu-id="3f847-203">In the applications list, select **Tango Analytics**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_app.png) 

3. <span data-ttu-id="3f847-205">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3f847-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3f847-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3f847-207">Click **Add** button.</span></span> <span data-ttu-id="3f847-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f847-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3f847-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3f847-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3f847-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f847-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f847-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f847-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f847-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3f847-213">Testing single sign-on</span></span>

<span data-ttu-id="3f847-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3f847-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3f847-215">Když kliknete na dlaždici Tango Analytics na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Tango Analytics.</span><span class="sxs-lookup"><span data-stu-id="3f847-215">When you click the Tango Analytics tile in the Access Panel, you should get automatically signed-on to your Tango Analytics application.</span></span>
<span data-ttu-id="3f847-216">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3f847-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f847-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3f847-217">Additional resources</span></span>

* [<span data-ttu-id="3f847-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3f847-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f847-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3f847-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_203.png

