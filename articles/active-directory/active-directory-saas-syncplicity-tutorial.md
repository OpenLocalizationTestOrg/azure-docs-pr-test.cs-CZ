---
title: 'Kurz: Azure Active Directory integrace s Syncplicity | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Syncplicity."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 1321fa71bcd625d6ea754432bfb402d3919e38f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a><span data-ttu-id="d5d53-103">Kurz: Azure Active Directory integrace s Syncplicity</span><span class="sxs-lookup"><span data-stu-id="d5d53-103">Tutorial: Azure Active Directory integration with Syncplicity</span></span>

<span data-ttu-id="d5d53-104">V tomto kurzu zjistěte, jak integrovat Syncplicity s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d5d53-104">In this tutorial, you learn how to integrate Syncplicity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d5d53-105">Integrace Syncplicity s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d5d53-105">Integrating Syncplicity with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d5d53-106">Můžete řídit ve službě Azure AD, který má přístup k Syncplicity</span><span class="sxs-lookup"><span data-stu-id="d5d53-106">You can control in Azure AD who has access to Syncplicity</span></span>
- <span data-ttu-id="d5d53-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Syncplicity (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5d53-107">You can enable your users to automatically get signed-on to Syncplicity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d5d53-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d5d53-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d5d53-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d5d53-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5d53-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d5d53-110">Prerequisites</span></span>

<span data-ttu-id="d5d53-111">Konfigurace integrace Azure AD s Syncplicity, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d5d53-111">To configure Azure AD integration with Syncplicity, you need the following items:</span></span>

- <span data-ttu-id="d5d53-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5d53-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d5d53-113">Syncplicity jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d5d53-113">A Syncplicity single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d5d53-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5d53-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d5d53-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d5d53-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d5d53-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d5d53-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d5d53-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d5d53-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d5d53-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d5d53-118">Scenario description</span></span>
<span data-ttu-id="d5d53-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5d53-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d5d53-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d5d53-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d5d53-121">Přidání Syncplicity z Galerie</span><span class="sxs-lookup"><span data-stu-id="d5d53-121">Adding Syncplicity from the gallery</span></span>
2. <span data-ttu-id="d5d53-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d5d53-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-syncplicity-from-the-gallery"></a><span data-ttu-id="d5d53-123">Přidání Syncplicity z Galerie</span><span class="sxs-lookup"><span data-stu-id="d5d53-123">Adding Syncplicity from the gallery</span></span>
<span data-ttu-id="d5d53-124">Při konfiguraci integrace Syncplicity do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Syncplicity z galerie.</span><span class="sxs-lookup"><span data-stu-id="d5d53-124">To configure the integration of Syncplicity into Azure AD, you need to add Syncplicity from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d5d53-125">**Pokud chcete přidat Syncplicity z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5d53-125">**To add Syncplicity from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d5d53-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d5d53-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d5d53-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d5d53-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d5d53-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5d53-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d5d53-133">Do vyhledávacího pole zadejte **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-133">In the search box, type **Syncplicity**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. <span data-ttu-id="d5d53-135">Na panelu výsledků vyberte **Syncplicity**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5d53-135">In the results panel, select **Syncplicity**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d5d53-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d5d53-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d5d53-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Syncplicity podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="d5d53-138">In this section, you configure and test Azure AD single sign-on with Syncplicity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d5d53-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Syncplicity je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d5d53-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Syncplicity is to a user in Azure AD.</span></span> <span data-ttu-id="d5d53-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Syncplicity musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d5d53-140">In other words, a link relationship between an Azure AD user and the related user in Syncplicity needs to be established.</span></span>

<span data-ttu-id="d5d53-141">V Syncplicity, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="d5d53-141">In Syncplicity, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d5d53-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Syncplicity, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d5d53-142">To configure and test Azure AD single sign-on with Syncplicity, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d5d53-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d5d53-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d5d53-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d5d53-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d5d53-145">**[Vytvoření zkušebního uživatele Syncplicity](#creating-a-syncplicity-test-user)**  – Pokud chcete mít protějšek Britta Simon v Syncplicity propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d5d53-145">**[Creating a Syncplicity test user](#creating-a-syncplicity-test-user)** - to have a counterpart of Britta Simon in Syncplicity that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d5d53-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d5d53-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d5d53-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d5d53-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d5d53-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d5d53-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d5d53-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="d5d53-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Syncplicity application.</span></span>

<span data-ttu-id="d5d53-150">**Ke konfiguraci Azure AD jednotné přihlašování s Syncplicity, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5d53-150">**To configure Azure AD single sign-on with Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="d5d53-151">Na portálu Azure na **Syncplicity** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-151">In the Azure portal, on the **Syncplicity** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d5d53-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d5d53-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. <span data-ttu-id="d5d53-155">Na **Syncplicity domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5d53-155">On the **Syncplicity Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    <span data-ttu-id="d5d53-157">a.</span><span class="sxs-lookup"><span data-stu-id="d5d53-157">a.</span></span> <span data-ttu-id="d5d53-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.syncplicity.com`</span><span class="sxs-lookup"><span data-stu-id="d5d53-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.syncplicity.com`</span></span>

    <span data-ttu-id="d5d53-159">b.</span><span class="sxs-lookup"><span data-stu-id="d5d53-159">b.</span></span> <span data-ttu-id="d5d53-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.syncplicity.com/sp`</span><span class="sxs-lookup"><span data-stu-id="d5d53-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.syncplicity.com/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d5d53-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="d5d53-161">These values are not real.</span></span> <span data-ttu-id="d5d53-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="d5d53-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d5d53-163">Obraťte se na [tým podpory Syncplicity klienta](https://www.syncplicity.com/contact-us) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d5d53-163">Contact [Syncplicity Client support team](https://www.syncplicity.com/contact-us) to get these values.</span></span> 
 

4. <span data-ttu-id="d5d53-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="d5d53-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. <span data-ttu-id="d5d53-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d5d53-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d5d53-168">Na **Syncplicity konfigurace** klikněte na tlačítko **konfigurace Syncplicity** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d5d53-168">On the **Syncplicity Configuration** section, click **Configure Syncplicity** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d5d53-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d5d53-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. <span data-ttu-id="d5d53-171">Přihlaste se k vaší **Syncplicity** klienta.</span><span class="sxs-lookup"><span data-stu-id="d5d53-171">Sign in to your **Syncplicity** tenant.</span></span>

8. <span data-ttu-id="d5d53-172">V nabídce v horní části, klikněte na tlačítko **správce**, vyberte **nastavení**a potom klikněte na **vlastní domény a jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-172">In the menu on the top, click **admin**, select **settings**, and then click **Custom domain and single sign-on**.</span></span>
   
    <span data-ttu-id="d5d53-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span><span class="sxs-lookup"><span data-stu-id="d5d53-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span></span>

9. <span data-ttu-id="d5d53-174">Na **jednotné přihlašování (SSO)** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5d53-174">On the **Single Sign-On (SSO)** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="d5d53-175">![Jednotné přihlašování \(jednotného přihlašování\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span><span class="sxs-lookup"><span data-stu-id="d5d53-175">![Single Sign-On \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span></span>   

    <span data-ttu-id="d5d53-176">a.</span><span class="sxs-lookup"><span data-stu-id="d5d53-176">a.</span></span> <span data-ttu-id="d5d53-177">V **vlastní domény** textovému poli, zadejte název vaší domény.</span><span class="sxs-lookup"><span data-stu-id="d5d53-177">In the **Custom Domain** textbox, type the name of your domain.</span></span>
  
    <span data-ttu-id="d5d53-178">b.</span><span class="sxs-lookup"><span data-stu-id="d5d53-178">b.</span></span> <span data-ttu-id="d5d53-179">Vyberte **povoleno** jako **jednotné přihlašování stav**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-179">Select **Enabled** as **Single Sign-On Status**.</span></span>

    <span data-ttu-id="d5d53-180">c.</span><span class="sxs-lookup"><span data-stu-id="d5d53-180">c.</span></span> <span data-ttu-id="d5d53-181">V **Entity Id** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5d53-181">In the **Entity Id** textbox, Paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d5d53-182">d.</span><span class="sxs-lookup"><span data-stu-id="d5d53-182">d.</span></span> <span data-ttu-id="d5d53-183">V **přihlašovací adresa URL stránky** textovému poli, Vložit **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5d53-183">In the **Sign-in page URL** textbox, Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d5d53-184">e.</span><span class="sxs-lookup"><span data-stu-id="d5d53-184">e.</span></span> <span data-ttu-id="d5d53-185">V **adresa URL odhlašovací stránky** textovému poli, Vložit **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5d53-185">In the **Logout page URL** textbox, Paste the **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d5d53-186">f.</span><span class="sxs-lookup"><span data-stu-id="d5d53-186">f.</span></span> <span data-ttu-id="d5d53-187">V **certifikát zprostředkovatele Identity**, klikněte na tlačítko **zvolte soubor**a pak nahrajte certifikát, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5d53-187">In **Identity Provider Certificate**, click **Choose file**, and then upload the certificate which you have downloaded from the Azure portal.</span></span> 

    <span data-ttu-id="d5d53-188">g.</span><span class="sxs-lookup"><span data-stu-id="d5d53-188">g.</span></span> <span data-ttu-id="d5d53-189">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-189">Click **SAVE CHANGES**.</span></span>

> [!TIP]
> <span data-ttu-id="d5d53-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d5d53-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d5d53-191">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d5d53-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d5d53-192">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d5d53-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d5d53-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5d53-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="d5d53-194">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d5d53-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d5d53-196">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5d53-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d5d53-197">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d5d53-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d5d53-199">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d5d53-201">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5d53-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d5d53-203">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5d53-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d5d53-205">a.</span><span class="sxs-lookup"><span data-stu-id="d5d53-205">a.</span></span> <span data-ttu-id="d5d53-206">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d5d53-207">b.</span><span class="sxs-lookup"><span data-stu-id="d5d53-207">b.</span></span> <span data-ttu-id="d5d53-208">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d5d53-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d5d53-209">c.</span><span class="sxs-lookup"><span data-stu-id="d5d53-209">c.</span></span> <span data-ttu-id="d5d53-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d5d53-211">d.</span><span class="sxs-lookup"><span data-stu-id="d5d53-211">d.</span></span> <span data-ttu-id="d5d53-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-212">Click **Create**.</span></span>
 
### <a name="creating-a-syncplicity-test-user"></a><span data-ttu-id="d5d53-213">Vytvoření zkušebního uživatele Syncplicity</span><span class="sxs-lookup"><span data-stu-id="d5d53-213">Creating a Syncplicity test user</span></span>
<span data-ttu-id="d5d53-214">AAD uživatelé moci přihlásit musí být zřízená Syncplicity aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5d53-214">For AAD users to be able to sign in, they must be provisioned to Syncplicity application.</span></span> <span data-ttu-id="d5d53-215">Tato část popisuje, jak ve Syncplicity vytvořit uživatelské účty AAD.</span><span class="sxs-lookup"><span data-stu-id="d5d53-215">This section describes how to create AAD user accounts in Syncplicity.</span></span>

<span data-ttu-id="d5d53-216">**K poskytnutí uživatelského účtu do Syncplicity, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5d53-216">**To provision a user account to Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="d5d53-217">Přihlaste se k vaší **Syncplicity** klienta (například: `https://company.Syncplicity.com`).</span><span class="sxs-lookup"><span data-stu-id="d5d53-217">Log in to your **Syncplicity** tenant (for example: `https://company.Syncplicity.com`).</span></span>

2. <span data-ttu-id="d5d53-218">Klikněte na tlačítko **správce** a vyberte **uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-218">Click **admin** and select **user accounts**.</span></span>

3. <span data-ttu-id="d5d53-219">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-219">Click **ADD A USER**.</span></span>
   
    <span data-ttu-id="d5d53-220">![Správa uživatelů](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="d5d53-220">![Manage Users](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Manage Users")</span></span>

4. <span data-ttu-id="d5d53-221">Typ **e-mailových adres** účtu AAD, které chcete zřídit, vyberte **uživatele** jako **Role**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-221">Type the **Email addressess** of an AAD account you want to provision, select **User** as **Role**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="d5d53-222">![Informace o účtu](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "účet informace")</span><span class="sxs-lookup"><span data-stu-id="d5d53-222">![Account Information](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "Account Information")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="d5d53-223">Držitel účtu AAD získá e-mailu, včetně odkaz k potvrzení a aktivovat účet.</span><span class="sxs-lookup"><span data-stu-id="d5d53-223">The AAD account holder  gets an email including a link to confirm and activate the account.</span></span> 
    > 

5. <span data-ttu-id="d5d53-224">Vyberte skupinu ve vaší společnosti, nový uživatel by měl stane členem, a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-224">Select a group in your company that your new user should become a member of, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="d5d53-225">![Členství ve skupinách](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "členství ve skupinách")</span><span class="sxs-lookup"><span data-stu-id="d5d53-225">![Group Membership](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "Group Membership")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="d5d53-226">Pokud nebyly nalezeny žádné skupiny uvedeny, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-226">If there are no groups listed, click **NEXT**.</span></span> 
    > 

6. <span data-ttu-id="d5d53-227">Vyberte složky, kterou chcete umístit pod kontrolou společnosti Syncplicity na počítači uživatele a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-227">Select the folders you would like to place under Syncplicity’s control on the user’s computer, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="d5d53-228">![Složky Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity složek")</span><span class="sxs-lookup"><span data-stu-id="d5d53-228">![Syncplicity Folders](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity Folders")</span></span>

>[!NOTE]
><span data-ttu-id="d5d53-229">Můžete použít všechny ostatní Syncplicity uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Syncplicity zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="d5d53-229">You can use any other Syncplicity user account creation tools or APIs provided by Syncplicity to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d5d53-230">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5d53-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d5d53-231">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="d5d53-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Syncplicity.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d5d53-233">**Pokud chcete přiřadit Britta Simon Syncplicity, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d5d53-233">**To assign Britta Simon to Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="d5d53-234">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d5d53-236">V seznamu aplikací vyberte **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-236">In the applications list, select **Syncplicity**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. <span data-ttu-id="d5d53-238">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d5d53-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d5d53-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d5d53-240">Click **Add** button.</span></span> <span data-ttu-id="d5d53-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5d53-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d5d53-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d5d53-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d5d53-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5d53-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d5d53-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d5d53-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d5d53-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d5d53-246">Testing single sign-on</span></span>

<span data-ttu-id="d5d53-247">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d5d53-247">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d5d53-248">Když kliknete na dlaždici Syncplicity na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="d5d53-248">When you click the Syncplicity tile in the Access Panel, you should get automatically signed-on to your Syncplicity application.</span></span>
## <a name="additional-resources"></a><span data-ttu-id="d5d53-249">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d5d53-249">Additional resources</span></span>

* [<span data-ttu-id="d5d53-250">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d5d53-250">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d5d53-251">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d5d53-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

