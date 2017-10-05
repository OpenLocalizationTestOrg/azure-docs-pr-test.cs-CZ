---
title: 'Kurz: Azure Active Directory integrace s Moxi zaujmout | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Moxi zapojení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1135a879-8f00-43b0-ac8a-831593d9586d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jeedes
ms.openlocfilehash: 25b5e377d8d0d504860ab9a8c4dac49c9ca5b104
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxi-engage"></a><span data-ttu-id="a9f2a-103">Kurz: Azure Active Directory integrace s Moxi zapojení</span><span class="sxs-lookup"><span data-stu-id="a9f2a-103">Tutorial: Azure Active Directory integration with Moxi Engage</span></span>

<span data-ttu-id="a9f2a-104">V tomto kurzu zjistěte, jak integrovat zaujmout Moxi s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a9f2a-104">In this tutorial, you learn how to integrate Moxi Engage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a9f2a-105">Integrace Moxi zaujmout s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a9f2a-105">Integrating Moxi Engage with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a9f2a-106">Můžete řídit ve službě Azure AD, který má přístup k zapojení Moxi</span><span class="sxs-lookup"><span data-stu-id="a9f2a-106">You can control in Azure AD who has access to Moxi Engage</span></span>
- <span data-ttu-id="a9f2a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Moxi zaujmout (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a9f2a-107">You can enable your users to automatically get signed-on to Moxi Engage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a9f2a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a9f2a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a9f2a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a9f2a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9f2a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a9f2a-110">Prerequisites</span></span>

<span data-ttu-id="a9f2a-111">Konfigurace integrace Azure AD s Moxi zapojení, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a9f2a-111">To configure Azure AD integration with Moxi Engage, you need the following items:</span></span>

- <span data-ttu-id="a9f2a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a9f2a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a9f2a-113">Moxi zaujmout jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a9f2a-113">A Moxi Engage single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a9f2a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a9f2a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a9f2a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a9f2a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a9f2a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a9f2a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a9f2a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a9f2a-118">Scenario description</span></span>
<span data-ttu-id="a9f2a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a9f2a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a9f2a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a9f2a-121">Přidání Moxi zaujmout z Galerie</span><span class="sxs-lookup"><span data-stu-id="a9f2a-121">Adding Moxi Engage from the gallery</span></span>
2. <span data-ttu-id="a9f2a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a9f2a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moxi-engage-from-the-gallery"></a><span data-ttu-id="a9f2a-123">Přidání Moxi zaujmout z Galerie</span><span class="sxs-lookup"><span data-stu-id="a9f2a-123">Adding Moxi Engage from the gallery</span></span>
<span data-ttu-id="a9f2a-124">Chcete-li nakonfigurovat integraci Moxi zapojení do služby Azure AD, přidejte zaujmout Moxi z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-124">To configure the integration of Moxi Engage into Azure AD, you need to add Moxi Engage from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a9f2a-125">**Pokud chcete přidat zaujmout Moxi z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a9f2a-125">**To add Moxi Engage from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a9f2a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a9f2a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a9f2a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a9f2a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a9f2a-133">Do vyhledávacího pole zadejte **Moxi zaujmout**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-133">In the search box, type **Moxi Engage**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_search.png)

5. <span data-ttu-id="a9f2a-135">Na panelu výsledků vyberte **Moxi zaujmout**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-135">In the results panel, select **Moxi Engage**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a9f2a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a9f2a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a9f2a-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Moxi zapojení podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="a9f2a-138">In this section, you configure and test Azure AD single sign-on with Moxi Engage based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a9f2a-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Moxi zapojení je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Moxi Engage is to a user in Azure AD.</span></span> <span data-ttu-id="a9f2a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Moxi zapojení je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-140">In other words, a link relationship between an Azure AD user and the related user in Moxi Engage needs to be established.</span></span>

<span data-ttu-id="a9f2a-141">V Moxi zapojení, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-141">In Moxi Engage, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a9f2a-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Moxi zapojení, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a9f2a-142">To configure and test Azure AD single sign-on with Moxi Engage, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a9f2a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a9f2a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a9f2a-145">**[Vytvoření zkušebního uživatele zaujmout Moxi](#creating-a-moxi-engage-test-user)**  – Pokud chcete mít protějšek Britta Simon v Moxi zaujmout propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-145">**[Creating a Moxi Engage test user](#creating-a-moxi-engage-test-user)** - to have a counterpart of Britta Simon in Moxi Engage that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a9f2a-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a9f2a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a9f2a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a9f2a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a9f2a-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Moxi zapojení.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Moxi Engage application.</span></span>

<span data-ttu-id="a9f2a-150">**Ke konfiguraci Azure AD jednotné přihlašování s Moxi zapojení, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a9f2a-150">**To configure Azure AD single sign-on with Moxi Engage, perform the following steps:**</span></span>

1. <span data-ttu-id="a9f2a-151">Na portálu Azure na **Moxi zaujmout** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-151">In the Azure portal, on the **Moxi Engage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a9f2a-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_samlbase.png)

3. <span data-ttu-id="a9f2a-155">Na **Moxi zaujmout domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a9f2a-155">On the **Moxi Engage Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_url.png)

    <span data-ttu-id="a9f2a-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://svc.<moxiworks-integration-domain>/service/v1/auth/inbound/saml/aad`</span><span class="sxs-lookup"><span data-stu-id="a9f2a-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://svc.<moxiworks-integration-domain>/service/v1/auth/inbound/saml/aad`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a9f2a-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-158">This value is not real.</span></span> <span data-ttu-id="a9f2a-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="a9f2a-160">Obraťte se na [tým podpory zapojení klienta Moxi](mailto:support@moxiworks.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-160">Contact [Moxi Engage Client support team](mailto:support@moxiworks.com) to get this value.</span></span> 
 
4. <span data-ttu-id="a9f2a-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_certificate.png) 

5. <span data-ttu-id="a9f2a-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxiengage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a9f2a-165">Konfigurace jednotného přihlašování na **Moxi zaujmout** straně, budete muset odeslat stažené **soubor XML s metadaty** k [Moxi zaujmout tým podpory](mailto:support@moxiworks.com).</span><span class="sxs-lookup"><span data-stu-id="a9f2a-165">To configure single sign-on on **Moxi Engage** side, you need to send the downloaded **Metadata XML** to [Moxi Engage support team](mailto:support@moxiworks.com).</span></span> <span data-ttu-id="a9f2a-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="a9f2a-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a9f2a-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a9f2a-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a9f2a-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a9f2a-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a9f2a-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a9f2a-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="a9f2a-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a9f2a-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a9f2a-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a9f2a-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxiengage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a9f2a-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxiengage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a9f2a-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxiengage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a9f2a-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a9f2a-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-moxiengage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a9f2a-182">a.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-182">a.</span></span> <span data-ttu-id="a9f2a-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a9f2a-184">b.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-184">b.</span></span> <span data-ttu-id="a9f2a-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a9f2a-186">c.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-186">c.</span></span> <span data-ttu-id="a9f2a-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a9f2a-188">d.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-188">d.</span></span> <span data-ttu-id="a9f2a-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-189">Click **Create**.</span></span>
 
### <a name="creating-a-moxi-engage-test-user"></a><span data-ttu-id="a9f2a-190">Vytvoření zkušebního uživatele Moxi zapojení</span><span class="sxs-lookup"><span data-stu-id="a9f2a-190">Creating a Moxi Engage test user</span></span>

<span data-ttu-id="a9f2a-191">V této části vytvoříte volal Britta Simon v Moxi zaujmout uživatele.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-191">In this section, you create a user called Britta Simon in Moxi Engage.</span></span> <span data-ttu-id="a9f2a-192">Práce s [Moxi zaujmout tým podpory](mailto:support@moxiworks.com) přidat uživatele do platformy Moxi zapojení.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-192">Work with [Moxi Engage support team](mailto:support@moxiworks.com) to add the users in the Moxi Engage platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a9f2a-193">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a9f2a-193">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a9f2a-194">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Moxi zapojení.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Moxi Engage.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a9f2a-196">**Přiřadit Britta Simon Moxi používat, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a9f2a-196">**To assign Britta Simon to Moxi Engage, perform the following steps:**</span></span>

1. <span data-ttu-id="a9f2a-197">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a9f2a-199">V seznamu aplikací vyberte **Moxi zaujmout**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-199">In the applications list, select **Moxi Engage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_app.png) 

3. <span data-ttu-id="a9f2a-201">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a9f2a-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-203">Click **Add** button.</span></span> <span data-ttu-id="a9f2a-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a9f2a-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a9f2a-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a9f2a-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a9f2a-209">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a9f2a-209">Testing single sign-on</span></span>

<span data-ttu-id="a9f2a-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a9f2a-211">Když kliknete na dlaždici zaujmout Moxi na přístupovém panelu, měli byste obdržet automatické přihlášení k Moxi zapojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="a9f2a-211">When you click the Moxi Engage tile in the Access Panel, you should get automatic login to Moxi Engage application.</span></span>
<span data-ttu-id="a9f2a-212">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a9f2a-212">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a9f2a-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a9f2a-213">Additional resources</span></span>

* [<span data-ttu-id="a9f2a-214">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a9f2a-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a9f2a-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a9f2a-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_203.png

