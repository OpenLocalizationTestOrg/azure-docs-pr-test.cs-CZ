---
title: 'Kurz: Azure Active Directory integrace s BGS Online | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BGS Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4fd6b29b-1b46-4fd1-9f5e-16b1c9d892cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: d1abd3f8e2980e03fc092613183a261880fbce38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bgs-online"></a><span data-ttu-id="23480-103">Kurz: Azure Active Directory integrace s BGS Online</span><span class="sxs-lookup"><span data-stu-id="23480-103">Tutorial: Azure Active Directory integration with BGS Online</span></span>

<span data-ttu-id="23480-104">V tomto kurzu zjistěte, jak integrovat BGS Online s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="23480-104">In this tutorial, you learn how to integrate BGS Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="23480-105">Integrace BGS Online s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="23480-105">Integrating BGS Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="23480-106">Můžete řídit ve službě Azure AD, který má přístup k BGS Online</span><span class="sxs-lookup"><span data-stu-id="23480-106">You can control in Azure AD who has access to BGS Online</span></span>
- <span data-ttu-id="23480-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k BGS Online (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="23480-107">You can enable your users to automatically get signed-on to BGS Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="23480-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="23480-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="23480-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="23480-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="23480-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="23480-110">Prerequisites</span></span>

<span data-ttu-id="23480-111">Konfigurace integrace Azure AD s BGS Online, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="23480-111">To configure Azure AD integration with BGS Online, you need the following items:</span></span>

- <span data-ttu-id="23480-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="23480-112">An Azure AD subscription</span></span>
- <span data-ttu-id="23480-113">BGS Online jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="23480-113">A BGS Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="23480-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="23480-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="23480-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="23480-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="23480-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="23480-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="23480-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="23480-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="23480-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="23480-118">Scenario description</span></span>
<span data-ttu-id="23480-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="23480-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="23480-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="23480-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="23480-121">Přidání BGS Online z Galerie</span><span class="sxs-lookup"><span data-stu-id="23480-121">Adding BGS Online from the gallery</span></span>
2. <span data-ttu-id="23480-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="23480-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bgs-online-from-the-gallery"></a><span data-ttu-id="23480-123">Přidání BGS Online z Galerie</span><span class="sxs-lookup"><span data-stu-id="23480-123">Adding BGS Online from the gallery</span></span>
<span data-ttu-id="23480-124">Při konfiguraci integrace služby BGS Online do služby Azure AD, musíte přidat BGS Online z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="23480-124">To configure the integration of BGS Online into Azure AD, you need to add BGS Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="23480-125">**Pokud chcete přidat BGS Online z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="23480-125">**To add BGS Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="23480-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="23480-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="23480-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="23480-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="23480-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="23480-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="23480-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23480-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="23480-133">Do vyhledávacího pole zadejte **BGS Online**.</span><span class="sxs-lookup"><span data-stu-id="23480-133">In the search box, type **BGS Online**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_search.png)

5. <span data-ttu-id="23480-135">Na panelu výsledků vyberte **BGS Online**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23480-135">In the results panel, select **BGS Online**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="23480-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="23480-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="23480-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BGS Online na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="23480-138">In this section, you configure and test Azure AD single sign-on with BGS Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="23480-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BGS Online je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="23480-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BGS Online is to a user in Azure AD.</span></span> <span data-ttu-id="23480-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v BGS Online.</span><span class="sxs-lookup"><span data-stu-id="23480-140">In other words, a link relationship between an Azure AD user and the related user in BGS Online needs to be established.</span></span>

<span data-ttu-id="23480-141">V BGS Online přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="23480-141">In BGS Online, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="23480-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BGS Online, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="23480-142">To configure and test Azure AD single sign-on with BGS Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="23480-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="23480-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="23480-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="23480-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="23480-145">**[Vytvoření zkušebního uživatele BGS Online](#creating-a-bgs-online-test-user)**  – Pokud chcete mít protějšek Britta Simon v BGS Online propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="23480-145">**[Creating a BGS Online test user](#creating-a-bgs-online-test-user)** - to have a counterpart of Britta Simon in BGS Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="23480-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23480-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="23480-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="23480-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="23480-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23480-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="23480-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci BGS Online.</span><span class="sxs-lookup"><span data-stu-id="23480-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BGS Online application.</span></span>

<span data-ttu-id="23480-150">**Ke konfiguraci Azure AD jednotné přihlašování s BGS Online, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="23480-150">**To configure Azure AD single sign-on with BGS Online, perform the following steps:**</span></span>

1. <span data-ttu-id="23480-151">Na portálu Azure na **BGS Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="23480-151">In the Azure portal, on the **BGS Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="23480-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23480-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_samlbase.png)

3. <span data-ttu-id="23480-155">Na **BGS Online domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="23480-155">On the **BGS Online Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_url.png)

    <span data-ttu-id="23480-157">a.</span><span class="sxs-lookup"><span data-stu-id="23480-157">a.</span></span> <span data-ttu-id="23480-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="23480-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>

    <span data-ttu-id="23480-159">Pro produkční prostředí použijte tento vzor`https://<company name>.millwardbrown.report`</span><span class="sxs-lookup"><span data-stu-id="23480-159">For production environment, use this pattern `https://<company name>.millwardbrown.report`</span></span> 

    <span data-ttu-id="23480-160">Pro testovací prostředí použijte tento vzor`https://millwardbrown.marketingtracker.nl/mt5/`</span><span class="sxs-lookup"><span data-stu-id="23480-160">For test environment, use this pattern `https://millwardbrown.marketingtracker.nl/mt5/`</span></span>

    <span data-ttu-id="23480-161">b.</span><span class="sxs-lookup"><span data-stu-id="23480-161">b.</span></span> <span data-ttu-id="23480-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="23480-162">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    
    <span data-ttu-id="23480-163">Pro produkční prostředí použijte tento vzor`https://<company name>.millwardbrown.report/sso/saml/AssertionConsumerService.aspx`</span><span class="sxs-lookup"><span data-stu-id="23480-163">For production environment, use this pattern `https://<company name>.millwardbrown.report/sso/saml/AssertionConsumerService.aspx`</span></span> 
      
    <span data-ttu-id="23480-164">Pro testovací prostředí použijte tento vzor`https://millwardbrown.marketingtracker.nl/mt5/sso/saml/AssertionConsumerService.aspx`</span><span class="sxs-lookup"><span data-stu-id="23480-164">For test environment, use this pattern `https://millwardbrown.marketingtracker.nl/mt5/sso/saml/AssertionConsumerService.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="23480-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="23480-165">These values are not real.</span></span> <span data-ttu-id="23480-166">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="23480-166">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="23480-167">Obraťte se na [BGS Online tým podpory](mailTo:bgsdashboardteam@millwardbrown.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="23480-167">Contact [BGS Online support team](mailTo:bgsdashboardteam@millwardbrown.com) to get these values.</span></span>
 

4. <span data-ttu-id="23480-168">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="23480-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_certificate.png) 

5. <span data-ttu-id="23480-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="23480-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bgsonline-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="23480-172">Na **Online konfigurace BGS** klikněte na tlačítko **konfigurace Online BGS** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="23480-172">On the **BGS Online Configuration** section, click **Configure BGS Online** to open **Configure sign-on** window.</span></span> <span data-ttu-id="23480-173">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="23480-173">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_configure.png) 

7. <span data-ttu-id="23480-175">Konfigurace jednotného přihlašování na **BGS Online** straně, budete muset odeslat stažené **soubor XML s metadaty** a **SAML jeden přihlašování adresa URL služby** k [BGS Online tým podpory](mailto:bgsdashboardteam@millwardbrown.com).</span><span class="sxs-lookup"><span data-stu-id="23480-175">To configure single sign-on on **BGS Online** side, you need to send the downloaded **Metadata XML** and **SAML Single Sign-On Service URL** to [BGS Online support team](mailto:bgsdashboardteam@millwardbrown.com).</span></span> 


> [!TIP]
> <span data-ttu-id="23480-176">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="23480-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="23480-177">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="23480-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="23480-178">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="23480-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="23480-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="23480-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="23480-180">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="23480-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="23480-182">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="23480-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="23480-183">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="23480-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="23480-185">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="23480-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="23480-187">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23480-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="23480-189">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="23480-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bgsonline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="23480-191">a.</span><span class="sxs-lookup"><span data-stu-id="23480-191">a.</span></span> <span data-ttu-id="23480-192">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="23480-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="23480-193">b.</span><span class="sxs-lookup"><span data-stu-id="23480-193">b.</span></span> <span data-ttu-id="23480-194">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="23480-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="23480-195">c.</span><span class="sxs-lookup"><span data-stu-id="23480-195">c.</span></span> <span data-ttu-id="23480-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="23480-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="23480-197">d.</span><span class="sxs-lookup"><span data-stu-id="23480-197">d.</span></span> <span data-ttu-id="23480-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="23480-198">Click **Create**.</span></span>
 
### <a name="creating-a-bgs-online-test-user"></a><span data-ttu-id="23480-199">Vytvoření BGS Online zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="23480-199">Creating a BGS Online test user</span></span>

<span data-ttu-id="23480-200">V této části vytvoříte uživatele volal Britta Simon v BGS Online.</span><span class="sxs-lookup"><span data-stu-id="23480-200">In this section, you create a user called Britta Simon in BGS Online.</span></span> <span data-ttu-id="23480-201">Práce s [BGS Online tým podpory](mailto:bgsdashboardteam@millwardbrown.com) přidat uživatele do BGS Online platformy.</span><span class="sxs-lookup"><span data-stu-id="23480-201">Work with [BGS Online support team](mailto:bgsdashboardteam@millwardbrown.com) to add the users in the BGS Online platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="23480-202">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="23480-202">Assigning the Azure AD test user</span></span>

<span data-ttu-id="23480-203">V této části povolíte Britta Simon tak, že udělíte přístup k BGS Online používat Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23480-203">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BGS Online.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="23480-205">**Pokud chcete přiřadit Britta Simon BGS Online, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="23480-205">**To assign Britta Simon to BGS Online, perform the following steps:**</span></span>

1. <span data-ttu-id="23480-206">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="23480-206">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="23480-208">V seznamu aplikací vyberte **BGS Online**.</span><span class="sxs-lookup"><span data-stu-id="23480-208">In the applications list, select **BGS Online**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bgsonline-tutorial/tutorial_bgsonline_app.png) 

3. <span data-ttu-id="23480-210">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="23480-210">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="23480-212">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="23480-212">Click **Add** button.</span></span> <span data-ttu-id="23480-213">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23480-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="23480-215">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="23480-215">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="23480-216">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23480-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="23480-217">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="23480-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="23480-218">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23480-218">Testing single sign-on</span></span>

<span data-ttu-id="23480-219">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="23480-219">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="23480-220">Když kliknete na dlaždici BGS Online na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci BGS Online.</span><span class="sxs-lookup"><span data-stu-id="23480-220">When you click the BGS Online tile in the Access Panel, you should get automatically signed-on to your BGS Online application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="23480-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="23480-221">Additional resources</span></span>

* [<span data-ttu-id="23480-222">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="23480-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="23480-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="23480-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bgsonline-tutorial/tutorial_general_203.png

