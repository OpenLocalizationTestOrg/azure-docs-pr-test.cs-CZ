---
title: 'Kurz: Azure Active Directory integrace s InsideView | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a InsideView."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: f2b0a1d4bc44f8d0cd57c61e2d78950cb6a99854
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a><span data-ttu-id="29b28-103">Kurz: Azure Active Directory integrace s InsideView</span><span class="sxs-lookup"><span data-stu-id="29b28-103">Tutorial: Azure Active Directory integration with InsideView</span></span>

<span data-ttu-id="29b28-104">V tomto kurzu zjistěte, jak integrovat InsideView s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="29b28-104">In this tutorial, you learn how to integrate InsideView with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="29b28-105">Integrace InsideView s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="29b28-105">Integrating InsideView with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="29b28-106">Můžete řídit ve službě Azure AD, který má přístup k InsideView</span><span class="sxs-lookup"><span data-stu-id="29b28-106">You can control in Azure AD who has access to InsideView</span></span>
- <span data-ttu-id="29b28-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k InsideView (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="29b28-107">You can enable your users to automatically get signed-on to InsideView (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="29b28-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="29b28-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="29b28-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="29b28-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29b28-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="29b28-110">Prerequisites</span></span>

<span data-ttu-id="29b28-111">Konfigurace integrace Azure AD s InsideView, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="29b28-111">To configure Azure AD integration with InsideView, you need the following items:</span></span>

- <span data-ttu-id="29b28-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="29b28-112">An Azure AD subscription</span></span>
- <span data-ttu-id="29b28-113">InsideView jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="29b28-113">A InsideView single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="29b28-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="29b28-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="29b28-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="29b28-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="29b28-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="29b28-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="29b28-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="29b28-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="29b28-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="29b28-118">Scenario description</span></span>
<span data-ttu-id="29b28-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="29b28-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="29b28-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="29b28-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="29b28-121">Přidání InsideView z Galerie</span><span class="sxs-lookup"><span data-stu-id="29b28-121">Adding InsideView from the gallery</span></span>
2. <span data-ttu-id="29b28-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="29b28-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-insideview-from-the-gallery"></a><span data-ttu-id="29b28-123">Přidání InsideView z Galerie</span><span class="sxs-lookup"><span data-stu-id="29b28-123">Adding InsideView from the gallery</span></span>
<span data-ttu-id="29b28-124">Chcete-li nakonfigurovat integraci InsideView v do Azure AD, přidejte InsideView z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="29b28-124">To configure the integration of InsideView in to Azure AD, you need to add InsideView from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="29b28-125">**Pokud chcete přidat InsideView z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="29b28-125">**To add InsideView from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="29b28-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="29b28-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="29b28-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="29b28-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="29b28-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="29b28-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="29b28-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="29b28-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="29b28-133">Do vyhledávacího pole zadejte **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="29b28-133">In the search box, type **InsideView**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_search.png)

5. <span data-ttu-id="29b28-135">Na panelu výsledků vyberte **InsideView**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29b28-135">In the results panel, select **InsideView**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="29b28-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="29b28-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="29b28-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s InsideView podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="29b28-138">In this section, you configure and test Azure AD single sign-on with InsideView based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="29b28-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v InsideView je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29b28-139">For single sign-on to work, Azure AD needs to know what the counterpart user in InsideView is to a user in Azure AD.</span></span> <span data-ttu-id="29b28-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v InsideView musí navázat.</span><span class="sxs-lookup"><span data-stu-id="29b28-140">In other words, a link relationship between an Azure AD user and the related user in InsideView needs to be established.</span></span>

<span data-ttu-id="29b28-141">V InsideView, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="29b28-141">In InsideView, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="29b28-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s InsideView, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="29b28-142">To configure and test Azure AD single sign-on with InsideView, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="29b28-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="29b28-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="29b28-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="29b28-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="29b28-145">**[Vytvoření zkušebního uživatele InsideView](#creating-a-insideview-test-user)**  – Pokud chcete mít protějšek Britta Simon v InsideView propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="29b28-145">**[Creating a InsideView test user](#creating-a-insideview-test-user)** - to have a counterpart of Britta Simon in InsideView that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="29b28-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="29b28-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="29b28-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="29b28-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="29b28-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="29b28-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="29b28-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci InsideView.</span><span class="sxs-lookup"><span data-stu-id="29b28-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your InsideView application.</span></span>

<span data-ttu-id="29b28-150">**Ke konfiguraci Azure AD jednotné přihlašování s InsideView, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="29b28-150">**To configure Azure AD single sign-on with InsideView, perform the following steps:**</span></span>

1. <span data-ttu-id="29b28-151">Na portálu Azure na **InsideView** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="29b28-151">In the Azure portal, on the **InsideView** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="29b28-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="29b28-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_samlbase.png)

3. <span data-ttu-id="29b28-155">Na **InsideView domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="29b28-155">On the **InsideView Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_url.png)
    
    <span data-ttu-id="29b28-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://my.insideview.com/iv/<STS Name>/login.iv`</span><span class="sxs-lookup"><span data-stu-id="29b28-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://my.insideview.com/iv/<STS Name>/login.iv`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="29b28-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="29b28-158">This value is not real.</span></span> <span data-ttu-id="29b28-159">Aktualizujte tuto hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="29b28-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="29b28-160">Obraťte se na [tým podpory InsideView ](mailto:support@insideview.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="29b28-160">Contact [InsideView support team ](mailto:support@insideview.com) to get this value.</span></span>
 
4. <span data-ttu-id="29b28-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="29b28-161">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_certificate.png) 

5. <span data-ttu-id="29b28-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29b28-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="29b28-165">Na **InsideView konfigurace** klikněte na tlačítko **konfigurace InsideView** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="29b28-165">On the **InsideView Configuration** section, click **Configure InsideView** to open **Configure sign-on** window.</span></span> <span data-ttu-id="29b28-166">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="29b28-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_configure.png) 

7. <span data-ttu-id="29b28-168">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti InsideView jako správce.</span><span class="sxs-lookup"><span data-stu-id="29b28-168">In a different web browser window, log in to your InsideView company site as an administrator.</span></span>

8. <span data-ttu-id="29b28-169">Na panelu nástrojů v horní části klikněte na tlačítko **správce**, **SingleSignOn nastavení**a potom klikněte na **přidat SAML**.</span><span class="sxs-lookup"><span data-stu-id="29b28-169">In the toolbar on the top, click **Admin**, **SingleSignOn Settings**, and then click **Add SAML**.</span></span>
   
   <span data-ttu-id="29b28-170">![Jednotné přihlašování SAML nastavení](./media/active-directory-saas-insideview-tutorial/ic794135.png "jednotného přihlašování SAML nastavení")</span><span class="sxs-lookup"><span data-stu-id="29b28-170">![SAML Single Sign On Settings](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML Single Sign On Settings")</span></span>

9. <span data-ttu-id="29b28-171">V **přidat nové SAML** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="29b28-171">In the **Add a New SAML** section, perform the following steps:</span></span>

    <span data-ttu-id="29b28-172">![Přidat nové SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "přidat nové SAML")</span><span class="sxs-lookup"><span data-stu-id="29b28-172">![Add a New SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "Add a New SAML")</span></span>
   
    <span data-ttu-id="29b28-173">a.</span><span class="sxs-lookup"><span data-stu-id="29b28-173">a.</span></span> <span data-ttu-id="29b28-174">V **název služby tokenů zabezpečení** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="29b28-174">In the **STS Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="29b28-175">b.</span><span class="sxs-lookup"><span data-stu-id="29b28-175">b.</span></span> <span data-ttu-id="29b28-176">V **nevyžádané koncový bod SamlP/WS-Fed** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="29b28-176">In **SamlP/WS-Fed Unsolicited EndPoint** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="29b28-177">c.</span><span class="sxs-lookup"><span data-stu-id="29b28-177">c.</span></span> <span data-ttu-id="29b28-178">Otevření kódování base-64 kódovaného certifikátu, který jste si stáhli z portálu Azure, zkopírujte obsah ho do schránky a vložte jej do **certifikát služby tokenů zabezpečení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="29b28-178">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **STS Certificate** textbox.</span></span>

    <span data-ttu-id="29b28-179">d.</span><span class="sxs-lookup"><span data-stu-id="29b28-179">d.</span></span> <span data-ttu-id="29b28-180">V **mapování Id uživatele Crm** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="29b28-180">In the **Crm User Id Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
        
    <span data-ttu-id="29b28-181">e.</span><span class="sxs-lookup"><span data-stu-id="29b28-181">e.</span></span> <span data-ttu-id="29b28-182">V **Crm e-mailu mapování** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="29b28-182">In the **Crm Email Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="29b28-183">f.</span><span class="sxs-lookup"><span data-stu-id="29b28-183">f.</span></span> <span data-ttu-id="29b28-184">V **Crm křestní jméno mapování** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="29b28-184">In the **Crm First Name Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="29b28-185">g.</span><span class="sxs-lookup"><span data-stu-id="29b28-185">g.</span></span> <span data-ttu-id="29b28-186">V **Crm lastName mapování** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="29b28-186">In the **Crm lastName Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>  

    <span data-ttu-id="29b28-187">h.</span><span class="sxs-lookup"><span data-stu-id="29b28-187">h.</span></span> <span data-ttu-id="29b28-188">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="29b28-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="29b28-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="29b28-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="29b28-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="29b28-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="29b28-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="29b28-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="29b28-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="29b28-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="29b28-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="29b28-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="29b28-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="29b28-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="29b28-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="29b28-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="29b28-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="29b28-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="29b28-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="29b28-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="29b28-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="29b28-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="29b28-204">a.</span><span class="sxs-lookup"><span data-stu-id="29b28-204">a.</span></span> <span data-ttu-id="29b28-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="29b28-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="29b28-206">b.</span><span class="sxs-lookup"><span data-stu-id="29b28-206">b.</span></span> <span data-ttu-id="29b28-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="29b28-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="29b28-208">c.</span><span class="sxs-lookup"><span data-stu-id="29b28-208">c.</span></span> <span data-ttu-id="29b28-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="29b28-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="29b28-210">d.</span><span class="sxs-lookup"><span data-stu-id="29b28-210">d.</span></span> <span data-ttu-id="29b28-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="29b28-211">Click **Create**.</span></span>
 
### <a name="creating-a-insideview-test-user"></a><span data-ttu-id="29b28-212">Vytvoření zkušebního uživatele InsideView</span><span class="sxs-lookup"><span data-stu-id="29b28-212">Creating a InsideView test user</span></span>

<span data-ttu-id="29b28-213">Pokud chcete povolit uživatelům Azure AD přihlášení k InsideView, se musí být zřízená v k InsideView.</span><span class="sxs-lookup"><span data-stu-id="29b28-213">To enable Azure AD users to log in to InsideView, they must be provisioned in to InsideView.</span></span> <span data-ttu-id="29b28-214">V případě InsideView zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="29b28-214">In the case of InsideView, provisioning is a manual task.</span></span>

<span data-ttu-id="29b28-215">Obraťte se na uživatele nebo kontaktů vytvořených v InsideView získáte [tým podpory InsideView](mailto:support@insideview.com).</span><span class="sxs-lookup"><span data-stu-id="29b28-215">To get users or contacts created in InsideView, Contact [InsideView support team](mailto:support@insideview.com).</span></span>

>[!NOTE]
><span data-ttu-id="29b28-216">Můžete použít všechny ostatní InsideView uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované InsideView ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29b28-216">You can use any other InsideView user account creation tools or APIs provided by InsideView to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="29b28-217">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="29b28-217">Assigning the Azure AD test user</span></span>

<span data-ttu-id="29b28-218">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu InsideView.</span><span class="sxs-lookup"><span data-stu-id="29b28-218">In this section, you enable Britta Simon to use Azure single sign-on by granting access to InsideView.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="29b28-220">**Pokud chcete přiřadit Britta Simon InsideView, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="29b28-220">**To assign Britta Simon to InsideView, perform the following steps:**</span></span>

1. <span data-ttu-id="29b28-221">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="29b28-221">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="29b28-223">V seznamu aplikací vyberte **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="29b28-223">In the applications list, select **InsideView**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_app.png) 

3. <span data-ttu-id="29b28-225">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="29b28-225">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="29b28-227">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29b28-227">Click **Add** button.</span></span> <span data-ttu-id="29b28-228">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="29b28-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="29b28-230">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="29b28-230">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="29b28-231">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="29b28-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="29b28-232">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="29b28-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="29b28-233">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="29b28-233">Testing single sign-on</span></span>

<span data-ttu-id="29b28-234">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="29b28-234">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="29b28-235">Když kliknete na dlaždici InsideView na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci InsideView.</span><span class="sxs-lookup"><span data-stu-id="29b28-235">When you click the InsideView tile in the Access Panel, you should get automatically signed-on to your InsideView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="29b28-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="29b28-236">Additional resources</span></span>

* [<span data-ttu-id="29b28-237">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="29b28-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="29b28-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="29b28-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_203.png

