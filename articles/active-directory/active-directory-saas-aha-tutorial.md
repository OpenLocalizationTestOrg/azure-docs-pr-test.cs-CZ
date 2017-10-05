---
title: 'Kurz: Azure Active Directory integrace s Aha! | Dokumentace Microsoftu'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Aha!."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7723864b2e1ab2d5b69d86f0fa18416b9d3f9aa3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a><span data-ttu-id="e7b1c-104">Kurz: Azure Active Directory integrace s Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-104">Tutorial: Azure Active Directory integration with Aha!</span></span>

<span data-ttu-id="e7b1c-105">V tomto kurzu zjistíte, jak integrovat Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-105">In this tutorial, you learn how to integrate Aha!</span></span> <span data-ttu-id="e7b1c-106">s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e7b1c-106">with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e7b1c-107">Integrace Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-107">Integrating Aha!</span></span> <span data-ttu-id="e7b1c-108">s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e7b1c-108">with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e7b1c-109">Můžete řídit ve službě Azure AD, který má přístup k Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-109">You can control in Azure AD who has access to Aha!</span></span>
- <span data-ttu-id="e7b1c-110">Můžete povolit uživatelům, aby automaticky získat přihlášení k Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-110">You can enable your users to automatically get signed-on to Aha!</span></span> <span data-ttu-id="e7b1c-111">(Jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e7b1c-111">(Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e7b1c-112">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e7b1c-112">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e7b1c-113">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e7b1c-113">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7b1c-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e7b1c-114">Prerequisites</span></span>

<span data-ttu-id="e7b1c-115">Konfigurace integrace Azure AD s Aha!, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e7b1c-115">To configure Azure AD integration with Aha!, you need the following items:</span></span>

- <span data-ttu-id="e7b1c-116">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e7b1c-116">An Azure AD subscription</span></span>
- <span data-ttu-id="e7b1c-117">Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-117">An Aha!</span></span> <span data-ttu-id="e7b1c-118">jednotné přihlašování v předplatném povolené</span><span class="sxs-lookup"><span data-stu-id="e7b1c-118">single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e7b1c-119">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-119">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e7b1c-120">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e7b1c-120">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e7b1c-121">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-121">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e7b1c-122">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e7b1c-122">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e7b1c-123">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e7b1c-123">Scenario description</span></span>
<span data-ttu-id="e7b1c-124">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-124">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e7b1c-125">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e7b1c-125">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e7b1c-126">Přidání Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-126">Adding Aha!</span></span> <span data-ttu-id="e7b1c-127">z Galerie</span><span class="sxs-lookup"><span data-stu-id="e7b1c-127">from the gallery</span></span>
2. <span data-ttu-id="e7b1c-128">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7b1c-128">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-aha-from-the-gallery"></a><span data-ttu-id="e7b1c-129">Přidání Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-129">Adding Aha!</span></span> <span data-ttu-id="e7b1c-130">z Galerie</span><span class="sxs-lookup"><span data-stu-id="e7b1c-130">from the gallery</span></span>
<span data-ttu-id="e7b1c-131">Konfigurace integrace Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-131">To configure the integration of Aha!</span></span> <span data-ttu-id="e7b1c-132">do služby Azure AD je nutné přidat Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-132">into Azure AD, you need to add Aha!</span></span> <span data-ttu-id="e7b1c-133">z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-133">from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e7b1c-134">**Chcete-li přidat Aha! z Galerie proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e7b1c-134">**To add Aha! from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e7b1c-135">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-135">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e7b1c-137">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-137">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e7b1c-138">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-138">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e7b1c-140">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-140">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e7b1c-142">Do vyhledávacího pole zadejte **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-142">In the search box, type **Aha!**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/tutorial_aha_search.png)

5. <span data-ttu-id="e7b1c-144">Na panelu výsledků vyberte **Aha!**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-144">In the results panel, select **Aha!**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e7b1c-146">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7b1c-146">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e7b1c-147">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-147">In this section, you configure and test Azure AD single sign-on with Aha!</span></span> <span data-ttu-id="e7b1c-148">na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e7b1c-148">based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e7b1c-149">Azure AD pro jednotné přihlašování pro práci, musí vědět, jaké příslušného uživatele v Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-149">For single sign-on to work, Azure AD needs to know what the counterpart user in Aha!</span></span> <span data-ttu-id="e7b1c-150">je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-150">is to a user in Azure AD.</span></span> <span data-ttu-id="e7b1c-151">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-151">In other words, a link relationship between an Azure AD user and the related user in Aha!</span></span> <span data-ttu-id="e7b1c-152">musí být vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-152">needs to be established.</span></span>

<span data-ttu-id="e7b1c-153">V Aha!, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-153">In Aha!, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e7b1c-154">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Aha!, je potřeba provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e7b1c-154">To configure and test Azure AD single sign-on with Aha!, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e7b1c-155">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-155">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e7b1c-156">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-156">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e7b1c-157">**[Vytvoření Aha! testovací uživatel](#creating-an-aha-test-user)**  – Pokud chcete mít protějšek Britta Simon v Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-157">**[Creating an Aha! test user](#creating-an-aha-test-user)** - to have a counterpart of Britta Simon in Aha!</span></span> <span data-ttu-id="e7b1c-158">připojený k Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-158">that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e7b1c-159">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-159">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e7b1c-160">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-160">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e7b1c-161">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7b1c-161">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e7b1c-162">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-162">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Aha!</span></span> <span data-ttu-id="e7b1c-163">Aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-163">application.</span></span>

<span data-ttu-id="e7b1c-164">**Ke konfiguraci Azure AD jednotné přihlašování s Aha!, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e7b1c-164">**To configure Azure AD single sign-on with Aha!, perform the following steps:**</span></span>

1. <span data-ttu-id="e7b1c-165">Na portálu Azure na **Aha!**</span><span class="sxs-lookup"><span data-stu-id="e7b1c-165">In the Azure portal, on the **Aha!**</span></span> <span data-ttu-id="e7b1c-166">Stránka integrace aplikace, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-166">application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e7b1c-168">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-168">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_aha_samlbase.png)

3. <span data-ttu-id="e7b1c-170">Na **Aha! Domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e7b1c-170">On the **Aha! Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_aha_url.png)

    <span data-ttu-id="e7b1c-172">a.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-172">a.</span></span> <span data-ttu-id="e7b1c-173">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.aha.io/session/new`</span><span class="sxs-lookup"><span data-stu-id="e7b1c-173">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.aha.io/session/new`</span></span>

    <span data-ttu-id="e7b1c-174">b.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-174">b.</span></span> <span data-ttu-id="e7b1c-175">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.aha.io`</span><span class="sxs-lookup"><span data-stu-id="e7b1c-175">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.aha.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e7b1c-176">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-176">These values are not real.</span></span> <span data-ttu-id="e7b1c-177">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-177">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e7b1c-178">Obraťte se na [Aha! Tým podpory klienta](https://www.aha.io/company/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-178">Contact [Aha! Client support team](https://www.aha.io/company/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="e7b1c-179">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-179">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_aha_certificate.png) 

5. <span data-ttu-id="e7b1c-181">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-181">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e7b1c-183">V okně prohlížeče jiný web Přihlaste se k vaší Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-183">In a different web browser window, log in to your Aha!</span></span> <span data-ttu-id="e7b1c-184">web společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-184">company site as an administrator.</span></span>

7. <span data-ttu-id="e7b1c-185">V nabídce v horní části, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-185">In the menu on the top, click **Settings**.</span></span>

    <span data-ttu-id="e7b1c-186">![Nastavení](./media/active-directory-saas-aha-tutorial/IC798950.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="e7b1c-186">![Settings](./media/active-directory-saas-aha-tutorial/IC798950.png "Settings")</span></span>

8. <span data-ttu-id="e7b1c-187">Klikněte na tlačítko **účet**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-187">Click **Account**.</span></span>
   
    <span data-ttu-id="e7b1c-188">![Profil](./media/active-directory-saas-aha-tutorial/IC798951.png "profilu")</span><span class="sxs-lookup"><span data-stu-id="e7b1c-188">![Profile](./media/active-directory-saas-aha-tutorial/IC798951.png "Profile")</span></span>

9. <span data-ttu-id="e7b1c-189">Klikněte na tlačítko **zabezpečení a jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-189">Click **Security and single sign-on**.</span></span>
   
    <span data-ttu-id="e7b1c-190">![Zabezpečení a jednotné přihlašování](./media/active-directory-saas-aha-tutorial/IC798952.png "zabezpečení a jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e7b1c-190">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798952.png "Security and single sign-on")</span></span>

10. <span data-ttu-id="e7b1c-191">V **jednotné přihlašování** části jako **zprostředkovatele Identity**, vyberte **SAML2.0**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-191">In **Single Sign-On** section, as **Identity Provider**, select **SAML2.0**.</span></span>
   
    <span data-ttu-id="e7b1c-192">![Zabezpečení a jednotné přihlašování](./media/active-directory-saas-aha-tutorial/IC798953.png "zabezpečení a jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e7b1c-192">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798953.png "Security and single sign-on")</span></span>

11. <span data-ttu-id="e7b1c-193">Na **jednotné přihlašování** konfigurace proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e7b1c-193">On the **Single Sign-On** configuration page, perform the following steps:</span></span>
    
    <span data-ttu-id="e7b1c-194">![Jednotné přihlašování](./media/active-directory-saas-aha-tutorial/IC798954.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e7b1c-194">![Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798954.png "Single Sign-On")</span></span>
    
       <span data-ttu-id="e7b1c-195">a.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-195">a.</span></span> <span data-ttu-id="e7b1c-196">V **název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-196">In the **Name** textbox, type a name for your configuration.</span></span>

       <span data-ttu-id="e7b1c-197">b.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-197">b.</span></span> <span data-ttu-id="e7b1c-198">Pro **nakonfigurovat**, vyberte **soubor metadat**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-198">For **Configure using**, select **Metadata File**.</span></span>
   
       <span data-ttu-id="e7b1c-199">c.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-199">c.</span></span> <span data-ttu-id="e7b1c-200">Chcete-li nahrát soubor stažený metadata, klikněte na tlačítko **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-200">To upload your downloaded metadata file, click **Browse**.</span></span>
   
       <span data-ttu-id="e7b1c-201">d.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-201">d.</span></span> <span data-ttu-id="e7b1c-202">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-202">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="e7b1c-203">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-203">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e7b1c-204">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-204">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e7b1c-205">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e7b1c-205">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e7b1c-206">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e7b1c-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="e7b1c-207">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-207">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e7b1c-209">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e7b1c-209">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e7b1c-210">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-210">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e7b1c-212">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-212">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e7b1c-214">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-214">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e7b1c-216">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e7b1c-216">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e7b1c-218">a.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-218">a.</span></span> <span data-ttu-id="e7b1c-219">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-219">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e7b1c-220">b.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-220">b.</span></span> <span data-ttu-id="e7b1c-221">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-221">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e7b1c-222">c.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-222">c.</span></span> <span data-ttu-id="e7b1c-223">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-223">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e7b1c-224">d.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-224">d.</span></span> <span data-ttu-id="e7b1c-225">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-225">Click **Create**.</span></span>
 
### <a name="creating-an-aha-test-user"></a><span data-ttu-id="e7b1c-226">Vytvoření Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-226">Creating an Aha!</span></span> <span data-ttu-id="e7b1c-227">testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="e7b1c-227">test user</span></span>

<span data-ttu-id="e7b1c-228">Povolit uživatelům Azure AD přihlášení do Aha!, musí být zřízená do Aha!.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-228">To enable Azure AD users to log in to Aha!, they must be provisioned into Aha!.</span></span>  

<span data-ttu-id="e7b1c-229">V případě Aha!, zřizování je automatizaci úloh.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-229">In the case of Aha!, provisioning is an automated task.</span></span> <span data-ttu-id="e7b1c-230">Neexistuje žádná položka akce za vás.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-230">There is no action item for you.</span></span>

<span data-ttu-id="e7b1c-231">Uživatelé jsou automaticky vytvářeny v případě potřeby při prvním jeden přihlašování pokusu.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-231">Users are automatically created if necessary during the first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="e7b1c-232">Můžete použít jiné Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-232">You can use any other Aha!</span></span> <span data-ttu-id="e7b1c-233">Nástroje pro tvorbu účet uživatele nebo rozhraní API poskytovaných Aha!</span><span class="sxs-lookup"><span data-stu-id="e7b1c-233">user account creation tools or APIs provided by Aha!</span></span> <span data-ttu-id="e7b1c-234">Chcete-li zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-234">to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e7b1c-235">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e7b1c-235">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e7b1c-236">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Aha!.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-236">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Aha!.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e7b1c-238">**Britta Simon přiřadit Aha!, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e7b1c-238">**To assign Britta Simon to Aha!, perform the following steps:**</span></span>

1. <span data-ttu-id="e7b1c-239">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-239">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e7b1c-241">V seznamu aplikací vyberte **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-241">In the applications list, select **Aha!**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aha-tutorial/tutorial_aha_app.png) 

3. <span data-ttu-id="e7b1c-243">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-243">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e7b1c-245">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-245">Click **Add** button.</span></span> <span data-ttu-id="e7b1c-246">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e7b1c-248">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-248">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e7b1c-249">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e7b1c-250">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e7b1c-251">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7b1c-251">Testing single sign-on</span></span>

<span data-ttu-id="e7b1c-252">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="e7b1c-252">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="e7b1c-253">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e7b1c-253">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7b1c-254">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e7b1c-254">Additional resources</span></span>

* [<span data-ttu-id="e7b1c-255">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e7b1c-255">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e7b1c-256">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e7b1c-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aha-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aha-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aha-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aha-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aha-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aha-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aha-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aha-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aha-tutorial/tutorial_general_203.png

