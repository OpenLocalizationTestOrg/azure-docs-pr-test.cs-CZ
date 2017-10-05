---
title: 'Kurz: Azure Active Directory integrace s QuickHelp | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a QuickHelp."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: 1c72b0ddee636090129dab7a5c7ec6ffd452434a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a><span data-ttu-id="92d7e-103">Kurz: Azure Active Directory integrace s QuickHelp</span><span class="sxs-lookup"><span data-stu-id="92d7e-103">Tutorial: Azure Active Directory integration with QuickHelp</span></span>

<span data-ttu-id="92d7e-104">V tomto kurzu zjistěte, jak integrovat QuickHelp s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="92d7e-104">In this tutorial, you learn how to integrate QuickHelp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="92d7e-105">Integrace QuickHelp s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="92d7e-105">Integrating QuickHelp with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="92d7e-106">Můžete řídit ve službě Azure AD, který má přístup k QuickHelp</span><span class="sxs-lookup"><span data-stu-id="92d7e-106">You can control in Azure AD who has access to QuickHelp</span></span>
- <span data-ttu-id="92d7e-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k QuickHelp (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="92d7e-107">You can enable your users to automatically get signed-on to QuickHelp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="92d7e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="92d7e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="92d7e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="92d7e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92d7e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="92d7e-110">Prerequisites</span></span>

<span data-ttu-id="92d7e-111">Konfigurace integrace Azure AD s QuickHelp, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="92d7e-111">To configure Azure AD integration with QuickHelp, you need the following items:</span></span>

- <span data-ttu-id="92d7e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="92d7e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="92d7e-113">QuickHelp jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="92d7e-113">A QuickHelp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="92d7e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="92d7e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="92d7e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="92d7e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="92d7e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="92d7e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="92d7e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="92d7e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="92d7e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="92d7e-118">Scenario description</span></span>
<span data-ttu-id="92d7e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="92d7e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="92d7e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="92d7e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="92d7e-121">Přidání QuickHelp z Galerie</span><span class="sxs-lookup"><span data-stu-id="92d7e-121">Adding QuickHelp from the gallery</span></span>
2. <span data-ttu-id="92d7e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="92d7e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-quickhelp-from-the-gallery"></a><span data-ttu-id="92d7e-123">Přidání QuickHelp z Galerie</span><span class="sxs-lookup"><span data-stu-id="92d7e-123">Adding QuickHelp from the gallery</span></span>
<span data-ttu-id="92d7e-124">Při konfiguraci integrace QuickHelp do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS QuickHelp z galerie.</span><span class="sxs-lookup"><span data-stu-id="92d7e-124">To configure the integration of QuickHelp into Azure AD, you need to add QuickHelp from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="92d7e-125">**Pokud chcete přidat QuickHelp z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92d7e-125">**To add QuickHelp from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="92d7e-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="92d7e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="92d7e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="92d7e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="92d7e-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92d7e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="92d7e-133">Do vyhledávacího pole zadejte **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-133">In the search box, type **QuickHelp**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_search.png)

5. <span data-ttu-id="92d7e-135">Na panelu výsledků vyberte **QuickHelp**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="92d7e-135">In the results panel, select **QuickHelp**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="92d7e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="92d7e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="92d7e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s QuickHelp podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="92d7e-138">In this section, you configure and test Azure AD single sign-on with QuickHelp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="92d7e-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v QuickHelp je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92d7e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in QuickHelp is to a user in Azure AD.</span></span> <span data-ttu-id="92d7e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v QuickHelp musí navázat.</span><span class="sxs-lookup"><span data-stu-id="92d7e-140">In other words, a link relationship between an Azure AD user and the related user in QuickHelp needs to be established.</span></span>

<span data-ttu-id="92d7e-141">V QuickHelp, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="92d7e-141">In QuickHelp, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="92d7e-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s QuickHelp, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="92d7e-142">To configure and test Azure AD single sign-on with QuickHelp, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="92d7e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="92d7e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="92d7e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="92d7e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="92d7e-145">**[Vytvoření zkušebního uživatele QuickHelp](#creating-a-quickhelp-test-user)**  – Pokud chcete mít protějšek Britta Simon v QuickHelp propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="92d7e-145">**[Creating a QuickHelp test user](#creating-a-quickhelp-test-user)** - to have a counterpart of Britta Simon in QuickHelp that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="92d7e-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="92d7e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="92d7e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="92d7e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="92d7e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="92d7e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="92d7e-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci QuickHelp.</span><span class="sxs-lookup"><span data-stu-id="92d7e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your QuickHelp application.</span></span>

<span data-ttu-id="92d7e-150">**Ke konfiguraci Azure AD jednotné přihlašování s QuickHelp, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92d7e-150">**To configure Azure AD single sign-on with QuickHelp, perform the following steps:**</span></span>

1. <span data-ttu-id="92d7e-151">Na portálu Azure na **QuickHelp** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-151">In the Azure portal, on the **QuickHelp** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="92d7e-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="92d7e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

3. <span data-ttu-id="92d7e-155">Na **QuickHelp domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="92d7e-155">On the **QuickHelp Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_url.png)

    <span data-ttu-id="92d7e-157">a.</span><span class="sxs-lookup"><span data-stu-id="92d7e-157">a.</span></span> <span data-ttu-id="92d7e-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://quickhelp.com/<instancename>/#/Login`</span><span class="sxs-lookup"><span data-stu-id="92d7e-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://quickhelp.com/<instancename>/#/Login`</span></span>

    <span data-ttu-id="92d7e-159">b.</span><span class="sxs-lookup"><span data-stu-id="92d7e-159">b.</span></span> <span data-ttu-id="92d7e-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.quickhelp.com`</span><span class="sxs-lookup"><span data-stu-id="92d7e-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.quickhelp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="92d7e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="92d7e-161">These values are not real.</span></span> <span data-ttu-id="92d7e-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="92d7e-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="92d7e-163">Obraťte se na [tým podpory QuickHelp klienta](https://support.quickhelp.com/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="92d7e-163">Contact [QuickHelp Client support team](https://support.quickhelp.com/) to get these values.</span></span> 
 
4. <span data-ttu-id="92d7e-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="92d7e-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

5. <span data-ttu-id="92d7e-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="92d7e-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="92d7e-168">Přihlašování k webu společnosti QuickHelp jako správce.</span><span class="sxs-lookup"><span data-stu-id="92d7e-168">Sign-on to your QuickHelp company site as administrator.</span></span>

7. <span data-ttu-id="92d7e-169">V nabídce v horní části, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-169">In the menu on the top, click **Admin**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][21]

8. <span data-ttu-id="92d7e-171">V **QuickHelp správce** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-171">In the **QuickHelp Admin** menu, click **Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][22]

9. <span data-ttu-id="92d7e-173">Klikněte na tlačítko **nastavení ověřování**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-173">Click **Authentication Settings**.</span></span>

10. <span data-ttu-id="92d7e-174">Na **nastavení ověřování** proveďte následující kroky</span><span class="sxs-lookup"><span data-stu-id="92d7e-174">On the **Authentication Settings** page, perform the following steps</span></span>
   
    ![Konfigurovat jednotné přihlašování][23]
   
    <span data-ttu-id="92d7e-176">a.</span><span class="sxs-lookup"><span data-stu-id="92d7e-176">a.</span></span> <span data-ttu-id="92d7e-177">Jako **typ jednotného přihlašování k**, vyberte **WSFederation**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-177">As **SSO Type**, select **WSFederation**.</span></span>
   
    <span data-ttu-id="92d7e-178">b.</span><span class="sxs-lookup"><span data-stu-id="92d7e-178">b.</span></span> <span data-ttu-id="92d7e-179">Chcete-li nahrát soubor stažený Azure metadata, klikněte na tlačítko **Procházet**, přejděte na soubor, pak klikněte na tlačítko Ukončit **nahrát Metadata**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-179">To upload your downloaded Azure metadata file, click **Browse**, navigate to the file, end then click **Upload Metadata**.</span></span>
   
    <span data-ttu-id="92d7e-180">c.</span><span class="sxs-lookup"><span data-stu-id="92d7e-180">c.</span></span> <span data-ttu-id="92d7e-181">V **e-mailu** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="92d7e-181">In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
   
    <span data-ttu-id="92d7e-182">d.</span><span class="sxs-lookup"><span data-stu-id="92d7e-182">d.</span></span> <span data-ttu-id="92d7e-183">V **křestní jméno** textovému poli, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="92d7e-183">In the **First Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
   
    <span data-ttu-id="92d7e-184">e.</span><span class="sxs-lookup"><span data-stu-id="92d7e-184">e.</span></span> <span data-ttu-id="92d7e-185">V **příjmení** textovému poli, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="92d7e-185">In the **Last Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
   
    <span data-ttu-id="92d7e-186">f.</span><span class="sxs-lookup"><span data-stu-id="92d7e-186">f.</span></span> <span data-ttu-id="92d7e-187">V **panelu akcí**, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-187">In the **Action Bar**, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="92d7e-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="92d7e-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="92d7e-189">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="92d7e-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="92d7e-190">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="92d7e-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="92d7e-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="92d7e-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="92d7e-192">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="92d7e-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="92d7e-194">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92d7e-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="92d7e-195">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="92d7e-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="92d7e-197">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="92d7e-199">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92d7e-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="92d7e-201">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="92d7e-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="92d7e-203">a.</span><span class="sxs-lookup"><span data-stu-id="92d7e-203">a.</span></span> <span data-ttu-id="92d7e-204">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="92d7e-205">b.</span><span class="sxs-lookup"><span data-stu-id="92d7e-205">b.</span></span> <span data-ttu-id="92d7e-206">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="92d7e-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="92d7e-207">c.</span><span class="sxs-lookup"><span data-stu-id="92d7e-207">c.</span></span> <span data-ttu-id="92d7e-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="92d7e-209">d.</span><span class="sxs-lookup"><span data-stu-id="92d7e-209">d.</span></span> <span data-ttu-id="92d7e-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-210">Click **Create**.</span></span>
 
### <a name="creating-a-quickhelp-test-user"></a><span data-ttu-id="92d7e-211">Vytvoření zkušebního uživatele QuickHelp</span><span class="sxs-lookup"><span data-stu-id="92d7e-211">Creating a QuickHelp test user</span></span>

<span data-ttu-id="92d7e-212">Cílem této části je vytvoření uživatele v QuickHelp nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="92d7e-212">The objective of this section is to create a user called Britta Simon in QuickHelp.</span></span>
<span data-ttu-id="92d7e-213">Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v QuickHelp pro uživatele ve službě Azure AD je Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92d7e-213">For single sign-on to work, Azure AD needs to know what the counterpart user in QuickHelp to a user in Azure AD is.</span></span> <span data-ttu-id="92d7e-214">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v QuickHelp musí navázat.</span><span class="sxs-lookup"><span data-stu-id="92d7e-214">In other words, a link relationship between an Azure AD user and the related user in QuickHelp needs to be established.</span></span>

<span data-ttu-id="92d7e-215">QuickHelp podporuje zřizování za běhu.</span><span class="sxs-lookup"><span data-stu-id="92d7e-215">QuickHelp supports just-in-time provisioning.</span></span> <span data-ttu-id="92d7e-216">To znamená, v případě potřeby, uživatelský účet je automaticky vytvořen ve QuickHelp a účet je propojený s účtem Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92d7e-216">This means, if necessary, a user account is automatically created in QuickHelp and the account is linked to the Azure AD account.</span></span>

<span data-ttu-id="92d7e-217">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="92d7e-217">There is no action item for you in this section.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="92d7e-218">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="92d7e-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="92d7e-219">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu QuickHelp.</span><span class="sxs-lookup"><span data-stu-id="92d7e-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to QuickHelp.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="92d7e-221">**Pokud chcete přiřadit Britta Simon QuickHelp, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="92d7e-221">**To assign Britta Simon to QuickHelp, perform the following steps:**</span></span>

1. <span data-ttu-id="92d7e-222">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="92d7e-224">V seznamu aplikací vyberte **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-224">In the applications list, select **QuickHelp**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_app.png) 

3. <span data-ttu-id="92d7e-226">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="92d7e-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="92d7e-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="92d7e-228">Click **Add** button.</span></span> <span data-ttu-id="92d7e-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92d7e-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="92d7e-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="92d7e-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="92d7e-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92d7e-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="92d7e-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="92d7e-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="92d7e-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="92d7e-234">Testing single sign-on</span></span>

<span data-ttu-id="92d7e-235">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="92d7e-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="92d7e-236">Když kliknete na dlaždici QuickHelp na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci QuickHelp.</span><span class="sxs-lookup"><span data-stu-id="92d7e-236">When you click the QuickHelp tile in the Access Panel, you should get automatically signed-on to your QuickHelp application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="92d7e-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="92d7e-237">Additional resources</span></span>

* [<span data-ttu-id="92d7e-238">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="92d7e-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="92d7e-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="92d7e-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
