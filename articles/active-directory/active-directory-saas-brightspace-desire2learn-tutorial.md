---
title: 'Kurz: Azure Active Directory integrace s Brightspace podle Desire2Learn | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Brightspace podle Desire2Learn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e2d3065b-1f6c-4c45-af78-0d5da3266999
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 7076b476ba71c5d94ae4728e5f6032b0d7e047ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a><span data-ttu-id="04c86-103">Kurz: Azure Active Directory integrace s Brightspace podle Desire2Learn</span><span class="sxs-lookup"><span data-stu-id="04c86-103">Tutorial: Azure Active Directory integration with Brightspace by Desire2Learn</span></span>

<span data-ttu-id="04c86-104">V tomto kurzu zjistěte, jak integrovat Brightspace podle Desire2Learn s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="04c86-104">In this tutorial, you learn how to integrate Brightspace by Desire2Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="04c86-105">Integrace Brightspace podle Desire2Learn s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="04c86-105">Integrating Brightspace by Desire2Learn with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="04c86-106">Můžete řídit ve službě Azure AD, který má přístup k Brightspace podle Desire2Learn</span><span class="sxs-lookup"><span data-stu-id="04c86-106">You can control in Azure AD who has access to Brightspace by Desire2Learn</span></span>
- <span data-ttu-id="04c86-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Brightspace podle Desire2Learn (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="04c86-107">You can enable your users to automatically get signed-on to Brightspace by Desire2Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="04c86-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="04c86-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="04c86-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="04c86-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04c86-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="04c86-110">Prerequisites</span></span>

<span data-ttu-id="04c86-111">Konfigurace integrace Azure AD s Brightspace podle Desire2Learn, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="04c86-111">To configure Azure AD integration with Brightspace by Desire2Learn, you need the following items:</span></span>

- <span data-ttu-id="04c86-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="04c86-112">An Azure AD subscription</span></span>
- <span data-ttu-id="04c86-113">Předplatné povolené Brightspace podle Desire2Learn jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="04c86-113">A Brightspace by Desire2Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="04c86-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="04c86-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="04c86-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="04c86-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="04c86-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="04c86-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="04c86-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="04c86-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="04c86-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="04c86-118">Scenario description</span></span>
<span data-ttu-id="04c86-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="04c86-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="04c86-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="04c86-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="04c86-121">Přidání Brightspace podle Desire2Learn z Galerie</span><span class="sxs-lookup"><span data-stu-id="04c86-121">Adding Brightspace by Desire2Learn from the gallery</span></span>
2. <span data-ttu-id="04c86-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="04c86-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-brightspace-by-desire2learn-from-the-gallery"></a><span data-ttu-id="04c86-123">Přidání Brightspace podle Desire2Learn z Galerie</span><span class="sxs-lookup"><span data-stu-id="04c86-123">Adding Brightspace by Desire2Learn from the gallery</span></span>
<span data-ttu-id="04c86-124">Při konfiguraci integrace Brightspace podle Desire2Learn do služby Azure AD potřebujete přidat Brightspace podle Desire2Learn z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="04c86-124">To configure the integration of Brightspace by Desire2Learn into Azure AD, you need to add Brightspace by Desire2Learn from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="04c86-125">**Pokud chcete přidat Brightspace podle Desire2Learn z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="04c86-125">**To add Brightspace by Desire2Learn from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="04c86-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="04c86-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="04c86-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="04c86-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="04c86-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="04c86-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="04c86-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04c86-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="04c86-133">Do vyhledávacího pole zadejte **Brightspace podle Desire2Learn**.</span><span class="sxs-lookup"><span data-stu-id="04c86-133">In the search box, type **Brightspace by Desire2Learn**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_search.png)

5. <span data-ttu-id="04c86-135">Na panelu výsledků vyberte **Brightspace podle Desire2Learn**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04c86-135">In the results panel, select **Brightspace by Desire2Learn**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="04c86-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="04c86-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="04c86-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s Brightspace podle Desire2Learn podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="04c86-138">In this section, you configure and test Azure AD single sign-on with Brightspace by Desire2Learn based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="04c86-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Brightspace podle Desire2Learn je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="04c86-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Brightspace by Desire2Learn is to a user in Azure AD.</span></span> <span data-ttu-id="04c86-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Brightspace podle Desire2Learn musí navázat.</span><span class="sxs-lookup"><span data-stu-id="04c86-140">In other words, a link relationship between an Azure AD user and the related user in Brightspace by Desire2Learn needs to be established.</span></span>

<span data-ttu-id="04c86-141">V Brightspace podle Desire2Learn přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="04c86-141">In Brightspace by Desire2Learn, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="04c86-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Brightspace podle Desire2Learn, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="04c86-142">To configure and test Azure AD single sign-on with Brightspace by Desire2Learn, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="04c86-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="04c86-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="04c86-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="04c86-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="04c86-145">**[Vytváření Brightspace uživatelem test Desire2Learn](#creating-a-brightspace-by-desire2learn-test-user)**  – Pokud chcete mít protějšek Britta Simon v Brightspace podle Desire2Learn propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="04c86-145">**[Creating a Brightspace by Desire2Learn test user](#creating-a-brightspace-by-desire2learn-test-user)** - to have a counterpart of Britta Simon in Brightspace by Desire2Learn that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="04c86-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="04c86-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="04c86-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="04c86-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="04c86-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="04c86-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="04c86-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaše Brightspace Desire2Learn aplikací.</span><span class="sxs-lookup"><span data-stu-id="04c86-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Brightspace by Desire2Learn application.</span></span>

<span data-ttu-id="04c86-150">**Ke konfiguraci Azure AD jednotné přihlašování s Brightspace podle Desire2Learn, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="04c86-150">**To configure Azure AD single sign-on with Brightspace by Desire2Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="04c86-151">Na portálu Azure na **Brightspace podle Desire2Learn** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="04c86-151">In the Azure portal, on the **Brightspace by Desire2Learn** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="04c86-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="04c86-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_samlbase.png)

3. <span data-ttu-id="04c86-155">Na **Brightspace Desire2Learn domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="04c86-155">On the **Brightspace by Desire2Learn Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_url.png)

    <span data-ttu-id="04c86-157">a.</span><span class="sxs-lookup"><span data-stu-id="04c86-157">a.</span></span> <span data-ttu-id="04c86-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="04c86-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.tenants.brightspace.com/samlLogin`|
    | `https://<companyname>.desire2learn.com/shibboleth-sp`|

    <span data-ttu-id="04c86-159">b.</span><span class="sxs-lookup"><span data-stu-id="04c86-159">b.</span></span> <span data-ttu-id="04c86-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span><span class="sxs-lookup"><span data-stu-id="04c86-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="04c86-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="04c86-161">These values are not real.</span></span> <span data-ttu-id="04c86-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="04c86-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="04c86-163">Obraťte se na [Brightspace tým podpory Desire2Learn](https://www.d2l.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="04c86-163">Contact [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/) to get these values.</span></span>
 


4. <span data-ttu-id="04c86-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="04c86-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_certificate.png) 

5. <span data-ttu-id="04c86-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04c86-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="04c86-168">Konfigurace jednotného přihlašování na **Brightspace podle Desire2Learn** straně, budete muset odeslat stažené **soubor XML s metadaty** k [Brightspace tým podpory Desire2Learn](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="04c86-168">To configure single sign-on on **Brightspace by Desire2Learn** side, you need to send the downloaded **Metadata XML** to [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="04c86-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="04c86-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="04c86-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="04c86-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="04c86-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="04c86-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="04c86-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="04c86-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="04c86-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="04c86-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="04c86-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="04c86-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="04c86-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="04c86-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="04c86-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="04c86-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="04c86-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04c86-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="04c86-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="04c86-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="04c86-184">a.</span><span class="sxs-lookup"><span data-stu-id="04c86-184">a.</span></span> <span data-ttu-id="04c86-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="04c86-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="04c86-186">b.</span><span class="sxs-lookup"><span data-stu-id="04c86-186">b.</span></span> <span data-ttu-id="04c86-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="04c86-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="04c86-188">c.</span><span class="sxs-lookup"><span data-stu-id="04c86-188">c.</span></span> <span data-ttu-id="04c86-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="04c86-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="04c86-190">d.</span><span class="sxs-lookup"><span data-stu-id="04c86-190">d.</span></span> <span data-ttu-id="04c86-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04c86-191">Click **Create**.</span></span>
 
### <a name="creating-a-brightspace-by-desire2learn-test-user"></a><span data-ttu-id="04c86-192">Vytváření Brightspace Desire2Learn testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="04c86-192">Creating a Brightspace by Desire2Learn test user</span></span>

<span data-ttu-id="04c86-193">Pokud chcete povolit uživatelům Azure AD přihlášení do Brightspace podle Desire2Learn, se musí být zřízená do Brightspace podle Desire2Learn.</span><span class="sxs-lookup"><span data-stu-id="04c86-193">In order to enable Azure AD users to log into Brightspace by Desire2Learn, they must be provisioned into Brightspace by Desire2Learn.</span></span>  

<span data-ttu-id="04c86-194">V případě Brightspace podle Desire2Learn uživatelské účty musí být vytvořený vaše [Brightspace tým podpory Desire2Learn](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="04c86-194">In the case of Brightspace by Desire2Learn, the user accounts need to be created by your [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

>[!NOTE]
><span data-ttu-id="04c86-195">Můžete použít jiné Brightspace Desire2Learn nástroje pro vytvoření účtu uživatele nebo rozhraní API poskytované Brightspace podle Desire2Learn zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="04c86-195">You can use any other Brightspace by Desire2Learn user account creation tools or APIs provided by Brightspace by Desire2Learn to provision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="04c86-196">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="04c86-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="04c86-197">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Brightspace podle Desire2Learn.</span><span class="sxs-lookup"><span data-stu-id="04c86-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Brightspace by Desire2Learn.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="04c86-199">**Pokud chcete přiřadit Britta Simon Brightspace podle Desire2Learn, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="04c86-199">**To assign Britta Simon to Brightspace by Desire2Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="04c86-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="04c86-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="04c86-202">V seznamu aplikací vyberte **Brightspace podle Desire2Learn**.</span><span class="sxs-lookup"><span data-stu-id="04c86-202">In the applications list, select **Brightspace by Desire2Learn**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_app.png) 

3. <span data-ttu-id="04c86-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="04c86-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="04c86-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04c86-206">Click **Add** button.</span></span> <span data-ttu-id="04c86-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04c86-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="04c86-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="04c86-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="04c86-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04c86-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="04c86-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04c86-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="04c86-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="04c86-212">Testing single sign-on</span></span>

<span data-ttu-id="04c86-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="04c86-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="04c86-214">Když kliknete Brightspace podle Desire2Learn dlaždice na přístupovém panelu, jste měli získat automaticky přihlášení k vaší Brightspace Desire2Learn aplikací.</span><span class="sxs-lookup"><span data-stu-id="04c86-214">When you click the Brightspace by Desire2Learn tile in the Access Panel, you should get automatically signed-on to your Brightspace by Desire2Learn application.</span></span>
<span data-ttu-id="04c86-215">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="04c86-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04c86-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="04c86-216">Additional resources</span></span>

* [<span data-ttu-id="04c86-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="04c86-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="04c86-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="04c86-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_203.png
