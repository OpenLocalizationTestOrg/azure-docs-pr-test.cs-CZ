---
title: "Kurz: Azure Active Directory integrace s CS hvězdiček | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a CS hvězdiček."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5704d151-afb8-40a4-b286-8bacd4f279ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: acc9160f86e58c7af4779a8bab5627dc5c5ad721
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cs-stars"></a><span data-ttu-id="ada52-103">Kurz: Azure Active Directory integrace s CS hvězdičkami</span><span class="sxs-lookup"><span data-stu-id="ada52-103">Tutorial: Azure Active Directory integration with CS Stars</span></span>

<span data-ttu-id="ada52-104">V tomto kurzu zjistěte, jak integrovat CS hvězdiček s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ada52-104">In this tutorial, you learn how to integrate CS Stars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ada52-105">Integrace s Azure AD CS hvězdiček poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ada52-105">Integrating CS Stars with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ada52-106">Můžete řídit ve službě Azure AD, který má přístup k CS hvězdičkami</span><span class="sxs-lookup"><span data-stu-id="ada52-106">You can control in Azure AD who has access to CS Stars</span></span>
- <span data-ttu-id="ada52-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k CS hvězdiček (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ada52-107">You can enable your users to automatically get signed-on to CS Stars (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ada52-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ada52-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ada52-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ada52-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ada52-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ada52-110">Prerequisites</span></span>

<span data-ttu-id="ada52-111">Chcete-li nakonfigurovat integrace Azure AD CS hvězdiček, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ada52-111">To configure Azure AD integration with CS Stars, you need the following items:</span></span>

- <span data-ttu-id="ada52-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ada52-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ada52-113">CS hvězdiček jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ada52-113">A CS Stars single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ada52-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ada52-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ada52-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ada52-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ada52-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ada52-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ada52-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ada52-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ada52-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ada52-118">Scenario description</span></span>
<span data-ttu-id="ada52-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ada52-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ada52-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ada52-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ada52-121">Přidání hvězdiček CS z Galerie</span><span class="sxs-lookup"><span data-stu-id="ada52-121">Adding CS Stars from the gallery</span></span>
2. <span data-ttu-id="ada52-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ada52-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cs-stars-from-the-gallery"></a><span data-ttu-id="ada52-123">Přidání hvězdiček CS z Galerie</span><span class="sxs-lookup"><span data-stu-id="ada52-123">Adding CS Stars from the gallery</span></span>
<span data-ttu-id="ada52-124">Při konfiguraci integrace CS hvězdiček do služby Azure AD potřebujete přidat hvězdiček CS z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ada52-124">To configure the integration of CS Stars into Azure AD, you need to add CS Stars from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ada52-125">**Pokud chcete přidat hvězdiček CS z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ada52-125">**To add CS Stars from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ada52-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ada52-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ada52-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ada52-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ada52-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ada52-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ada52-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ada52-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ada52-133">Do vyhledávacího pole zadejte **CS hvězdiček**.</span><span class="sxs-lookup"><span data-stu-id="ada52-133">In the search box, type **CS Stars**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_search.png)

5. <span data-ttu-id="ada52-135">Na panelu výsledků vyberte **CS hvězdiček**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ada52-135">In the results panel, select **CS Stars**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ada52-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ada52-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ada52-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s CS hvězdiček podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="ada52-138">In this section, you configure and test Azure AD single sign-on with CS Stars based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ada52-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v CS hvězdiček je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ada52-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CS Stars is to a user in Azure AD.</span></span> <span data-ttu-id="ada52-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v CS hvězdiček musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ada52-140">In other words, a link relationship between an Azure AD user and the related user in CS Stars needs to be established.</span></span>

<span data-ttu-id="ada52-141">V CS hvězdiček, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ada52-141">In CS Stars, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ada52-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí hvězdiček CS, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ada52-142">To configure and test Azure AD single sign-on with CS Stars, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ada52-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ada52-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ada52-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ada52-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ada52-145">**[Vytvoření zkušebního uživatele CS hvězdiček](#creating-a-cs-stars-test-user)**  – Pokud chcete mít protějšek Britta Simon v CS hvězdiček propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ada52-145">**[Creating a CS Stars test user](#creating-a-cs-stars-test-user)** - to have a counterpart of Britta Simon in CS Stars that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ada52-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ada52-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ada52-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ada52-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ada52-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ada52-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ada52-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci CS hvězdiček.</span><span class="sxs-lookup"><span data-stu-id="ada52-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CS Stars application.</span></span>

<span data-ttu-id="ada52-150">**Ke konfiguraci Azure AD jednotné přihlašování s CS hvězdiček, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ada52-150">**To configure Azure AD single sign-on with CS Stars, perform the following steps:**</span></span>

1. <span data-ttu-id="ada52-151">Na portálu Azure na **CS hvězdiček** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ada52-151">In the Azure portal, on the **CS Stars** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ada52-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ada52-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_samlbase.png)

3. <span data-ttu-id="ada52-155">Na **CS hvězdiček domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ada52-155">On the **CS Stars Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_url.png)

    <span data-ttu-id="ada52-157">a.</span><span class="sxs-lookup"><span data-stu-id="ada52-157">a.</span></span> <span data-ttu-id="ada52-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="ada52-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`</span></span>

    <span data-ttu-id="ada52-159">b.</span><span class="sxs-lookup"><span data-stu-id="ada52-159">b.</span></span> <span data-ttu-id="ada52-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.csstars.com/enterprise/`</span><span class="sxs-lookup"><span data-stu-id="ada52-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.csstars.com/enterprise/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ada52-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ada52-161">These values are not real.</span></span> <span data-ttu-id="ada52-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ada52-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ada52-163">Obraťte se na [tým podpory CS hvězdiček klienta](http://www.marshclearsight.com/support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="ada52-163">Contact [CS Stars Client support team](http://www.marshclearsight.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="ada52-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ada52-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_certificate.png) 

5. <span data-ttu-id="ada52-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ada52-166">Click **Save** button.</span></span>

    <span data-ttu-id="ada52-167">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cs-stars-tutorial/tutorial_general_400.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="ada52-167">![Configure Single Sign-On](./media/active-directory-saas-cs-stars-tutorial/tutorial_general_400.png) 
<CS></span></span>
6. <span data-ttu-id="ada52-168">Konfigurace jednotného přihlašování na **CS hvězdiček** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory CS hvězdiček](http://www.marshclearsight.com/support/).</span><span class="sxs-lookup"><span data-stu-id="ada52-168">To configure single sign-on on **CS Stars** side, you need to send the downloaded **Metadata XML** to [CS Stars support team](http://www.marshclearsight.com/support/).</span></span> 
<CE>

> [!TIP]
> <span data-ttu-id="ada52-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ada52-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ada52-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ada52-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ada52-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ada52-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ada52-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ada52-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="ada52-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ada52-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ada52-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ada52-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ada52-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ada52-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ada52-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ada52-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ada52-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ada52-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ada52-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ada52-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ada52-184">a.</span><span class="sxs-lookup"><span data-stu-id="ada52-184">a.</span></span> <span data-ttu-id="ada52-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ada52-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ada52-186">b.</span><span class="sxs-lookup"><span data-stu-id="ada52-186">b.</span></span> <span data-ttu-id="ada52-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ada52-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ada52-188">c.</span><span class="sxs-lookup"><span data-stu-id="ada52-188">c.</span></span> <span data-ttu-id="ada52-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ada52-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ada52-190">d.</span><span class="sxs-lookup"><span data-stu-id="ada52-190">d.</span></span> <span data-ttu-id="ada52-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ada52-191">Click **Create**.</span></span>
 
### <a name="creating-a-cs-stars-test-user"></a><span data-ttu-id="ada52-192">Vytvoření zkušebního uživatele CS hvězdičkami</span><span class="sxs-lookup"><span data-stu-id="ada52-192">Creating a CS Stars test user</span></span>

<span data-ttu-id="ada52-193">Cílem této části je vytvoření uživatele volal Britta Simon v CS hvězdiček.</span><span class="sxs-lookup"><span data-stu-id="ada52-193">The objective of this section is to create a user called Britta Simon in CS Stars.</span></span>

<span data-ttu-id="ada52-194">Pokud chcete získat uživatelem vytvořené v CS hvězdiček, budete muset kontaktovat vaše [tým podpory CS hvězdiček](http://www.marshclearsight.com/support/).</span><span class="sxs-lookup"><span data-stu-id="ada52-194">To get a user created in CS Stars, you need to contact your [CS Stars support team](http://www.marshclearsight.com/support/).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ada52-195">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ada52-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ada52-196">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu CS hvězdiček.</span><span class="sxs-lookup"><span data-stu-id="ada52-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CS Stars.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ada52-198">**Pokud chcete přiřadit Britta Simon CS hvězdiček, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ada52-198">**To assign Britta Simon to CS Stars, perform the following steps:**</span></span>

1. <span data-ttu-id="ada52-199">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ada52-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ada52-201">V seznamu aplikací vyberte **CS hvězdiček**.</span><span class="sxs-lookup"><span data-stu-id="ada52-201">In the applications list, select **CS Stars**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_app.png) 

3. <span data-ttu-id="ada52-203">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ada52-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ada52-205">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ada52-205">Click **Add** button.</span></span> <span data-ttu-id="ada52-206">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ada52-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ada52-208">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ada52-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ada52-209">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ada52-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ada52-210">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ada52-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ada52-211">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ada52-211">Testing single sign-on</span></span>

<span data-ttu-id="ada52-212">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ada52-212">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="ada52-213">Když kliknete na dlaždici CS hvězdiček na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci CS hvězdiček.</span><span class="sxs-lookup"><span data-stu-id="ada52-213">When you click the CS Stars tile in the Access Panel, you should get automatically signed-on to your CS Stars application.</span></span>
 

## <a name="additional-resources"></a><span data-ttu-id="ada52-214">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ada52-214">Additional resources</span></span>

* [<span data-ttu-id="ada52-215">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ada52-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ada52-216">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ada52-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_203.png

