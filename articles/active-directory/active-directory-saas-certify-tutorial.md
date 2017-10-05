---
title: 'Kurz: Azure Active Directory integrace s Certify | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Certify."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b36e020-175a-4534-b341-85260739f889
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bbb2357d17535de438555a0b1f8256b134c8a40e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-certify"></a><span data-ttu-id="95f9c-103">Kurz: Azure Active Directory integrace s Certify</span><span class="sxs-lookup"><span data-stu-id="95f9c-103">Tutorial: Azure Active Directory integration with Certify</span></span>

<span data-ttu-id="95f9c-104">V tomto kurzu zjistěte, jak integrovat Certify s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="95f9c-104">In this tutorial, you learn how to integrate Certify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="95f9c-105">Integrace Certify s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="95f9c-105">Integrating Certify with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="95f9c-106">Můžete řídit ve službě Azure AD, který má přístup k Certify</span><span class="sxs-lookup"><span data-stu-id="95f9c-106">You can control in Azure AD who has access to Certify</span></span>
- <span data-ttu-id="95f9c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Certify (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="95f9c-107">You can enable your users to automatically get signed-on to Certify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="95f9c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="95f9c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="95f9c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="95f9c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95f9c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="95f9c-110">Prerequisites</span></span>

<span data-ttu-id="95f9c-111">Konfigurace integrace Azure AD s Certify, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="95f9c-111">To configure Azure AD integration with Certify, you need the following items:</span></span>

- <span data-ttu-id="95f9c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="95f9c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="95f9c-113">Certify jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="95f9c-113">A Certify single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="95f9c-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="95f9c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="95f9c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="95f9c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="95f9c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="95f9c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="95f9c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="95f9c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="95f9c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="95f9c-118">Scenario description</span></span>
<span data-ttu-id="95f9c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="95f9c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="95f9c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="95f9c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="95f9c-121">Přidání Certify z Galerie</span><span class="sxs-lookup"><span data-stu-id="95f9c-121">Adding Certify from the gallery</span></span>
2. <span data-ttu-id="95f9c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="95f9c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-certify-from-the-gallery"></a><span data-ttu-id="95f9c-123">Přidání Certify z Galerie</span><span class="sxs-lookup"><span data-stu-id="95f9c-123">Adding Certify from the gallery</span></span>
<span data-ttu-id="95f9c-124">Při konfiguraci integrace Certify do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Certify z galerie.</span><span class="sxs-lookup"><span data-stu-id="95f9c-124">To configure the integration of Certify into Azure AD, you need to add Certify from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="95f9c-125">**Pokud chcete přidat Certify z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95f9c-125">**To add Certify from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="95f9c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="95f9c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="95f9c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="95f9c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="95f9c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95f9c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="95f9c-133">Do vyhledávacího pole zadejte **Certify**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-133">In the search box, type **Certify**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-certify-tutorial/tutorial_certify_search.png)

5. <span data-ttu-id="95f9c-135">Na panelu výsledků vyberte **Certify**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="95f9c-135">In the results panel, select **Certify**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-certify-tutorial/tutorial_certify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="95f9c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="95f9c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="95f9c-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Certify podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="95f9c-138">In this section, you configure and test Azure AD single sign-on with Certify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="95f9c-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Certify je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95f9c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Certify is to a user in Azure AD.</span></span> <span data-ttu-id="95f9c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Certify musí navázat.</span><span class="sxs-lookup"><span data-stu-id="95f9c-140">In other words, a link relationship between an Azure AD user and the related user in Certify needs to be established.</span></span>

<span data-ttu-id="95f9c-141">V Certify přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="95f9c-141">In Certify, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="95f9c-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Certify, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="95f9c-142">To configure and test Azure AD single sign-on with Certify, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="95f9c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="95f9c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="95f9c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95f9c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="95f9c-145">**[Vytvoření zkušebního uživatele Certify](#creating-a-certify-test-user)**  – Pokud chcete mít protějšek Britta Simon v Certify propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="95f9c-145">**[Creating a Certify test user](#creating-a-certify-test-user)** - to have a counterpart of Britta Simon in Certify that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="95f9c-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95f9c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="95f9c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="95f9c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="95f9c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="95f9c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="95f9c-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Certify.</span><span class="sxs-lookup"><span data-stu-id="95f9c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Certify application.</span></span>

<span data-ttu-id="95f9c-150">**Ke konfiguraci Azure AD jednotné přihlašování s Certify, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95f9c-150">**To configure Azure AD single sign-on with Certify, perform the following steps:**</span></span>

1. <span data-ttu-id="95f9c-151">Na portálu Azure na **Certify** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-151">In the Azure portal, on the **Certify** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="95f9c-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="95f9c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-certify-tutorial/tutorial_certify_samlbase.png)

3. <span data-ttu-id="95f9c-155">Na **certifikovat domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="95f9c-155">On the **Certify Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-certify-tutorial/tutorial_certify_url.png)

    <span data-ttu-id="95f9c-157">V **identifikátor** textovému poli, zadejte adresu URL:`https://www.certify.com`</span><span class="sxs-lookup"><span data-stu-id="95f9c-157">In the **Identifier** textbox, type the URL: `https://www.certify.com`</span></span>

4. <span data-ttu-id="95f9c-158">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="95f9c-158">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-certify-tutorial/tutorial_certify_certificate.png) 

5. <span data-ttu-id="95f9c-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95f9c-160">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-certify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="95f9c-162">Na **certifikovat konfigurace** klikněte na tlačítko **konfigurace certifikovat** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="95f9c-162">On the **Certify Configuration** section, click **Configure Certify** to open **Configure sign-on** window.</span></span> <span data-ttu-id="95f9c-163">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="95f9c-163">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-certify-tutorial/tutorial_certify_configure.png) 

7. <span data-ttu-id="95f9c-165">Konfigurace jednotného přihlašování na **Certify** straně, budete muset odeslat stažené **Certificate(Raw)** a **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory Certify](mailto:support@certify.com).</span><span class="sxs-lookup"><span data-stu-id="95f9c-165">To configure single sign-on on **Certify** side, you need to send the downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Certify support team](mailto:support@certify.com).</span></span> <span data-ttu-id="95f9c-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="95f9c-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="95f9c-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="95f9c-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="95f9c-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="95f9c-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="95f9c-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="95f9c-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="95f9c-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="95f9c-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="95f9c-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95f9c-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="95f9c-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95f9c-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="95f9c-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="95f9c-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-certify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="95f9c-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-certify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="95f9c-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95f9c-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-certify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="95f9c-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="95f9c-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-certify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="95f9c-182">a.</span><span class="sxs-lookup"><span data-stu-id="95f9c-182">a.</span></span> <span data-ttu-id="95f9c-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="95f9c-184">b.</span><span class="sxs-lookup"><span data-stu-id="95f9c-184">b.</span></span> <span data-ttu-id="95f9c-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="95f9c-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="95f9c-186">c.</span><span class="sxs-lookup"><span data-stu-id="95f9c-186">c.</span></span> <span data-ttu-id="95f9c-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="95f9c-188">d.</span><span class="sxs-lookup"><span data-stu-id="95f9c-188">d.</span></span> <span data-ttu-id="95f9c-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-189">Click **Create**.</span></span>
 
### <a name="creating-a-certify-test-user"></a><span data-ttu-id="95f9c-190">Vytvoření zkušebního uživatele Certify</span><span class="sxs-lookup"><span data-stu-id="95f9c-190">Creating a Certify test user</span></span>

<span data-ttu-id="95f9c-191">Cílem této části je vytvoření uživatele v Certify nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95f9c-191">The objective of this section is to create a user called Britta Simon in Certify.</span></span> <span data-ttu-id="95f9c-192">Certifikujte podporuje v běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="95f9c-192">Certify supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="95f9c-193">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="95f9c-193">There is no action item for you in this section.</span></span> <span data-ttu-id="95f9c-194">Vytvoří se nový uživatel během pokusu o přístup k Certify, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="95f9c-194">A new user will be created during an attempt to access Certify if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="95f9c-195">Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktovat [tým podpory Certify](mailto:support@certify.com).</span><span class="sxs-lookup"><span data-stu-id="95f9c-195">If you need to create an user manually, you need to contact the [Certify support team](mailto:support@certify.com).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="95f9c-196">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="95f9c-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="95f9c-197">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Certify.</span><span class="sxs-lookup"><span data-stu-id="95f9c-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Certify.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="95f9c-199">**Pokud chcete přiřadit Britta Simon Certify, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="95f9c-199">**To assign Britta Simon to Certify, perform the following steps:**</span></span>

1. <span data-ttu-id="95f9c-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="95f9c-202">V seznamu aplikací vyberte **Certify**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-202">In the applications list, select **Certify**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-certify-tutorial/tutorial_certify_app.png) 

3. <span data-ttu-id="95f9c-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="95f9c-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="95f9c-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95f9c-206">Click **Add** button.</span></span> <span data-ttu-id="95f9c-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95f9c-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="95f9c-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="95f9c-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="95f9c-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95f9c-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="95f9c-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="95f9c-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="95f9c-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="95f9c-212">Testing single sign-on</span></span>

<span data-ttu-id="95f9c-213">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="95f9c-213">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="95f9c-214">Když kliknete na dlaždici Certify na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Certify.</span><span class="sxs-lookup"><span data-stu-id="95f9c-214">When you click the Certify tile in the Access Panel, you should get automatically signed-on to your Certify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="95f9c-215">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="95f9c-215">Additional resources</span></span>

* [<span data-ttu-id="95f9c-216">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95f9c-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="95f9c-217">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="95f9c-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-certify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-certify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-certify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-certify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-certify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-certify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-certify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-certify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-certify-tutorial/tutorial_general_203.png

