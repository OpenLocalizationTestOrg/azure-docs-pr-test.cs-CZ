---
title: "Kurz: Azure Active Directory integrace s tlačí se k | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a tlačí se k."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 59d4019545d39ec76bf401696338140f430630c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a><span data-ttu-id="92c13-103">Kurz: Azure Active Directory integrace s tlačí se k</span><span class="sxs-lookup"><span data-stu-id="92c13-103">Tutorial: Azure Active Directory integration with Huddle</span></span>

<span data-ttu-id="92c13-104">V tomto kurzu zjistěte, jak integrovat tlačí se k službě Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="92c13-104">In this tutorial, you learn how to integrate Huddle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="92c13-105">Integrace tlačí se k službě Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="92c13-105">Integrating Huddle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="92c13-106">Můžete řídit ve službě Azure AD, který má přístup k tlačí se k</span><span class="sxs-lookup"><span data-stu-id="92c13-106">You can control in Azure AD who has access to Huddle</span></span>
- <span data-ttu-id="92c13-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k tlačí se k (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="92c13-107">You can enable your users to automatically get signed-on to Huddle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="92c13-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="92c13-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="92c13-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="92c13-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92c13-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="92c13-110">Prerequisites</span></span>

<span data-ttu-id="92c13-111">Chcete-li nakonfigurovat tlačí se k integraci Azure AD, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="92c13-111">To configure Azure AD integration with Huddle, you need the following items:</span></span>

- <span data-ttu-id="92c13-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="92c13-112">An Azure AD subscription</span></span>
- <span data-ttu-id="92c13-113">Tlačí se k jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="92c13-113">A Huddle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="92c13-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="92c13-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="92c13-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="92c13-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="92c13-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="92c13-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="92c13-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="92c13-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="92c13-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="92c13-118">Scenario description</span></span>

<span data-ttu-id="92c13-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="92c13-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="92c13-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="92c13-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="92c13-121">Přidání tlačí se k z Galerie</span><span class="sxs-lookup"><span data-stu-id="92c13-121">Adding Huddle from the gallery</span></span>
2. <span data-ttu-id="92c13-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="92c13-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-huddle-from-the-gallery"></a><span data-ttu-id="92c13-123">Přidání tlačí se k z Galerie</span><span class="sxs-lookup"><span data-stu-id="92c13-123">Adding Huddle from the gallery</span></span>
<span data-ttu-id="92c13-124">Chcete-li nakonfigurovat integraci tlačí se k do služby Azure AD, přidejte tlačí se k z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="92c13-124">To configure the integration of Huddle into Azure AD, you need to add Huddle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="92c13-125">**Pokud chcete přidat tlačí se k z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92c13-125">**To add Huddle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="92c13-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="92c13-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="92c13-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="92c13-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="92c13-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="92c13-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="92c13-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92c13-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="92c13-133">Do vyhledávacího pole zadejte **tlačí se k**.</span><span class="sxs-lookup"><span data-stu-id="92c13-133">In the search box, type **Huddle**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_search.png)

5. <span data-ttu-id="92c13-135">Na panelu výsledků vyberte **tlačí se k**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92c13-135">In the results panel, select **Huddle**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="92c13-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="92c13-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="92c13-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s tlačí se k podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="92c13-138">In this section, you configure and test Azure AD single sign-on with Huddle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="92c13-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v tlačí se k je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92c13-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Huddle is to a user in Azure AD.</span></span> <span data-ttu-id="92c13-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v tlačí se k musí navázat.</span><span class="sxs-lookup"><span data-stu-id="92c13-140">In other words, a link relationship between an Azure AD user and the related user in Huddle needs to be established.</span></span>

<span data-ttu-id="92c13-141">V tlačí se k, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="92c13-141">In Huddle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="92c13-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s tlačí se k, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="92c13-142">To configure and test Azure AD single sign-on with Huddle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="92c13-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="92c13-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>

2. <span data-ttu-id="92c13-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="92c13-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>

3. <span data-ttu-id="92c13-145">**[Vytvoření zkušebního uživatele tlačí se k](#creating-a-huddle-test-user)**  – Pokud chcete mít protějšek Britta Simon v tlačí se k propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="92c13-145">**[Creating a Huddle test user](#creating-a-huddle-test-user)** - to have a counterpart of Britta Simon in Huddle that is linked to the Azure AD representation of user.</span></span>

4. <span data-ttu-id="92c13-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="92c13-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>

5. <span data-ttu-id="92c13-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="92c13-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="92c13-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="92c13-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="92c13-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v tlačí se k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92c13-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Huddle application.</span></span>

<span data-ttu-id="92c13-150">**Ke konfiguraci Azure AD jednotné přihlašování s tlačí se k, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92c13-150">**To configure Azure AD single sign-on with Huddle, perform the following steps:**</span></span>

1. <span data-ttu-id="92c13-151">Na portálu Azure na **tlačí se k** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="92c13-151">In the Azure portal, on the **Huddle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="92c13-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="92c13-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_samlbase.png)

3. <span data-ttu-id="92c13-155">Na **tlačí se k doméně a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="92c13-155">On the **Huddle Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_url.png)

    <span data-ttu-id="92c13-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://<company name>.huddle.com`</span><span class="sxs-lookup"><span data-stu-id="92c13-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<company name>.huddle.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="92c13-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="92c13-158">This value is not real.</span></span> <span data-ttu-id="92c13-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="92c13-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="92c13-160">Obraťte se na [tým podpory tlačí se k klienta](https://huddle.zendesk.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="92c13-160">Contact [Huddle Client support team](https://huddle.zendesk.com) to get this value.</span></span> 

4. <span data-ttu-id="92c13-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="92c13-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_certificate.png) 

5. <span data-ttu-id="92c13-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="92c13-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-huddle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="92c13-165">Na **tlačí se k konfigurace** klikněte na tlačítko **konfigurace tlačí se k** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="92c13-165">On the **Huddle Configuration** section, click **Configure Huddle** to open **Configure sign-on** window.</span></span> <span data-ttu-id="92c13-166">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="92c13-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_configure.png) 
    
7. <span data-ttu-id="92c13-168">Na straně tlačí se k nakonfigurovat jednotné přihlašování, budete muset odeslat stažené **certifikát**, **SAML jeden přihlašování adresa URL služby**, a **SAML Entity ID** k [tým podpory tlačí se k klienta](https://huddle.zendesk.com).</span><span class="sxs-lookup"><span data-stu-id="92c13-168">To configure single sign-on on Huddle side, you need to send the downloaded  **Certificate**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** to [Huddle Client support team](https://huddle.zendesk.com).</span></span> <span data-ttu-id="92c13-169">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="92c13-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>  
   
    >[!NOTE]
    > <span data-ttu-id="92c13-170">Jednotné přihlašování musí být povolena tlačí se k tým podpory.</span><span class="sxs-lookup"><span data-stu-id="92c13-170">Single sign-on needs to be enabled by the Huddle support team.</span></span> <span data-ttu-id="92c13-171">Po dokončení konfigurace zobrazí oznámení.</span><span class="sxs-lookup"><span data-stu-id="92c13-171">You get a notification when the configuration has been completed.</span></span> 
    > 

> [!TIP]
> <span data-ttu-id="92c13-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="92c13-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="92c13-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="92c13-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="92c13-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="92c13-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
   
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="92c13-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="92c13-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="92c13-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="92c13-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="92c13-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92c13-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="92c13-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="92c13-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="92c13-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="92c13-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="92c13-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92c13-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="92c13-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="92c13-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="92c13-187">a.</span><span class="sxs-lookup"><span data-stu-id="92c13-187">a.</span></span> <span data-ttu-id="92c13-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="92c13-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="92c13-189">b.</span><span class="sxs-lookup"><span data-stu-id="92c13-189">b.</span></span> <span data-ttu-id="92c13-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="92c13-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="92c13-191">c.</span><span class="sxs-lookup"><span data-stu-id="92c13-191">c.</span></span> <span data-ttu-id="92c13-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="92c13-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="92c13-193">d.</span><span class="sxs-lookup"><span data-stu-id="92c13-193">d.</span></span> <span data-ttu-id="92c13-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="92c13-194">Click **Create**.</span></span>
 
### <a name="creating-a-huddle-test-user"></a><span data-ttu-id="92c13-195">Vytvoření zkušebního uživatele tlačí se k</span><span class="sxs-lookup"><span data-stu-id="92c13-195">Creating a Huddle test user</span></span>

<span data-ttu-id="92c13-196">Pokud chcete povolit uživatelům přihlášení do tlačí se k Azure AD, musí být zřízená do tlačí se k.</span><span class="sxs-lookup"><span data-stu-id="92c13-196">To enable Azure AD users to log in to Huddle, they must be provisioned into Huddle.</span></span> <span data-ttu-id="92c13-197">V případě tlačí se k zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="92c13-197">In the case of Huddle, provisioning is a manual task.</span></span>

<span data-ttu-id="92c13-198">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92c13-198">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="92c13-199">Přihlaste se k vaší **tlačí se k** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="92c13-199">Log in to your **Huddle** company site as administrator.</span></span>
2. <span data-ttu-id="92c13-200">Klikněte na tlačítko **prostoru**.</span><span class="sxs-lookup"><span data-stu-id="92c13-200">Click **Workspace**.</span></span>
3. <span data-ttu-id="92c13-201">Klikněte na tlačítko **osoby \> pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="92c13-201">Click **People \> Invite People**.</span></span>
   
   <span data-ttu-id="92c13-202">![Lidé](./media/active-directory-saas-huddle-tutorial/IC787838.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="92c13-202">![People](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")</span></span>

4. <span data-ttu-id="92c13-203">V **vytvořit nová pozvánka** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="92c13-203">In the **Create a new invitation** section, perform the following steps:</span></span>
   
   <span data-ttu-id="92c13-204">![Nová Pozvánka](./media/active-directory-saas-huddle-tutorial/IC787839.png "nová pozvánka")</span><span class="sxs-lookup"><span data-stu-id="92c13-204">![New Invitation](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")</span></span>
   
   <span data-ttu-id="92c13-205">a.</span><span class="sxs-lookup"><span data-stu-id="92c13-205">a.</span></span> <span data-ttu-id="92c13-206">V **vyberte tým pro pozvat** seznamu, vyberte **team**.</span><span class="sxs-lookup"><span data-stu-id="92c13-206">In the **Choose a team to invite people to join** list, select **team**.</span></span>

   <span data-ttu-id="92c13-207">b.</span><span class="sxs-lookup"><span data-stu-id="92c13-207">b.</span></span> <span data-ttu-id="92c13-208">Typ **e-mailovou adresu** platný Azure AD účet chcete zřídit v k **zadejte e-mailová adresa osoby, které byste chtěli pozvat** textové pole.</span><span class="sxs-lookup"><span data-stu-id="92c13-208">Type the **Email Address** of a valid Azure AD account you want to provision in to **Enter email address for people you'd like to invite** textbox.</span></span>

   <span data-ttu-id="92c13-209">c.</span><span class="sxs-lookup"><span data-stu-id="92c13-209">c.</span></span> <span data-ttu-id="92c13-210">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="92c13-210">Click **Invite**.</span></span>   
   
    >[!NOTE]
    > <span data-ttu-id="92c13-211">Držitel účtu Azure AD budou obdrží e-mail zahrnutím odkazu pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="92c13-211">The Azure AD account holder will receive an email including a link to confirm the account before it becomes active.</span></span> 
    > 

>[!NOTE]
><span data-ttu-id="92c13-212">Ostatní tlačí se k nástroje pro tvorbu účet uživatele nebo rozhraní API poskytovaných tlačí se k můžete použít ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92c13-212">You can use any other Huddle user account creation tools or APIs provided by Huddle to provision Azure AD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="92c13-213">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="92c13-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="92c13-214">V této části povolíte Britta Simon k používání Azure jednotné přihlašování v tlačí se k udělení přístupu.</span><span class="sxs-lookup"><span data-stu-id="92c13-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Huddle.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="92c13-216">**Chcete-li tlačí se k přiřadit Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92c13-216">**To assign Britta Simon to Huddle, perform the following steps:**</span></span>

1. <span data-ttu-id="92c13-217">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="92c13-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="92c13-219">V seznamu aplikací vyberte **tlačí se k**.</span><span class="sxs-lookup"><span data-stu-id="92c13-219">In the applications list, select **Huddle**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_app.png) 

3. <span data-ttu-id="92c13-221">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="92c13-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="92c13-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="92c13-223">Click **Add** button.</span></span> <span data-ttu-id="92c13-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92c13-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="92c13-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="92c13-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="92c13-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92c13-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="92c13-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92c13-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="92c13-229">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="92c13-229">Testing single sign-on</span></span>

<span data-ttu-id="92c13-230">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="92c13-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="92c13-231">Když kliknete na dlaždici tlačí se k na přístupovém panelu, měli byste obdržet automaticky přihlašovací stránku tlačí se k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92c13-231">When you click the Huddle tile in the Access Panel, you should get automatically login page of Huddle application.</span></span>
<span data-ttu-id="92c13-232">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="92c13-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92c13-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="92c13-233">Additional resources</span></span>

* [<span data-ttu-id="92c13-234">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="92c13-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="92c13-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="92c13-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_04.png
[100]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_203.png
