---
title: 'Kurz: Azure Active Directory integrace s EverBridge | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a EverBridge."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: f5a97fc8df978dd55a73ae53516a82f884c14bec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a><span data-ttu-id="64bae-103">Kurz: Azure Active Directory integrace s EverBridge</span><span class="sxs-lookup"><span data-stu-id="64bae-103">Tutorial: Azure Active Directory integration with EverBridge</span></span>

<span data-ttu-id="64bae-104">V tomto kurzu zjistěte, jak integrovat EverBridge s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="64bae-104">In this tutorial, you learn how to integrate EverBridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="64bae-105">Integrace EverBridge s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="64bae-105">Integrating EverBridge with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="64bae-106">Můžete řídit ve službě Azure AD, který má přístup k EverBridge</span><span class="sxs-lookup"><span data-stu-id="64bae-106">You can control in Azure AD who has access to EverBridge</span></span>
- <span data-ttu-id="64bae-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k EverBridge (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="64bae-107">You can enable your users to automatically get signed-on to EverBridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="64bae-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="64bae-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="64bae-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="64bae-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64bae-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="64bae-110">Prerequisites</span></span>

<span data-ttu-id="64bae-111">Konfigurace integrace Azure AD s EverBridge, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="64bae-111">To configure Azure AD integration with EverBridge, you need the following items:</span></span>

- <span data-ttu-id="64bae-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="64bae-112">An Azure AD subscription</span></span>
- <span data-ttu-id="64bae-113">EverBridge jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="64bae-113">An EverBridge single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="64bae-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="64bae-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="64bae-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="64bae-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="64bae-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="64bae-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="64bae-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64bae-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="64bae-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="64bae-118">Scenario description</span></span>
<span data-ttu-id="64bae-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="64bae-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="64bae-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="64bae-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="64bae-121">Přidání EverBridge z Galerie</span><span class="sxs-lookup"><span data-stu-id="64bae-121">Adding EverBridge from the gallery</span></span>
2. <span data-ttu-id="64bae-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="64bae-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-everbridge-from-the-gallery"></a><span data-ttu-id="64bae-123">Přidání EverBridge z Galerie</span><span class="sxs-lookup"><span data-stu-id="64bae-123">Adding EverBridge from the gallery</span></span>
<span data-ttu-id="64bae-124">Při konfiguraci integrace EverBridge do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS EverBridge z galerie.</span><span class="sxs-lookup"><span data-stu-id="64bae-124">To configure the integration of EverBridge into Azure AD, you need to add EverBridge from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="64bae-125">**Pokud chcete přidat EverBridge z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="64bae-125">**To add EverBridge from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="64bae-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="64bae-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="64bae-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="64bae-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="64bae-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="64bae-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="64bae-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64bae-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="64bae-133">Do vyhledávacího pole zadejte **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="64bae-133">In the search box, type **EverBridge**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_search.png)

5. <span data-ttu-id="64bae-135">Na panelu výsledků vyberte **EverBridge**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="64bae-135">In the results panel, select **EverBridge**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="64bae-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="64bae-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="64bae-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s EverBridge podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="64bae-138">In this section, you configure and test Azure AD single sign-on with EverBridge based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="64bae-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v EverBridge je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64bae-139">For single sign-on to work, Azure AD needs to know what the counterpart user in EverBridge is to a user in Azure AD.</span></span> <span data-ttu-id="64bae-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v EverBridge musí navázat.</span><span class="sxs-lookup"><span data-stu-id="64bae-140">In other words, a link relationship between an Azure AD user and the related user in EverBridge needs to be established.</span></span>

<span data-ttu-id="64bae-141">V EverBridge, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="64bae-141">In EverBridge, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="64bae-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s EverBridge, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="64bae-142">To configure and test Azure AD single sign-on with EverBridge, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="64bae-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="64bae-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="64bae-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64bae-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="64bae-145">**[Vytváření testovacího uživatele EverBridge](#creating-an-everbridge-test-user)**  – Pokud chcete mít protějšek Britta Simon v EverBridge propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="64bae-145">**[Creating an EverBridge test user](#creating-an-everbridge-test-user)** - to have a counterpart of Britta Simon in EverBridge that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="64bae-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="64bae-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="64bae-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="64bae-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="64bae-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="64bae-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="64bae-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci EverBridge.</span><span class="sxs-lookup"><span data-stu-id="64bae-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your EverBridge application.</span></span>

<span data-ttu-id="64bae-150">**Ke konfiguraci Azure AD jednotné přihlašování s EverBridge, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="64bae-150">**To configure Azure AD single sign-on with EverBridge, perform the following steps:**</span></span>

1. <span data-ttu-id="64bae-151">Na portálu Azure na **EverBridge** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="64bae-151">In the Azure portal, on the **EverBridge** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="64bae-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="64bae-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_samlbase.png)

3. <span data-ttu-id="64bae-155">Na **EverBridge domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64bae-155">On the **EverBridge Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_url.png)

    <span data-ttu-id="64bae-157">a.</span><span class="sxs-lookup"><span data-stu-id="64bae-157">a.</span></span> <span data-ttu-id="64bae-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://sso.everbridge.net/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="64bae-158">In the **Identifier** textbox, type a URL using the following pattern: `https://sso.everbridge.net/<companyname>`</span></span>

    <span data-ttu-id="64bae-159">b.</span><span class="sxs-lookup"><span data-stu-id="64bae-159">b.</span></span> <span data-ttu-id="64bae-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span><span class="sxs-lookup"><span data-stu-id="64bae-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="64bae-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="64bae-161">These values are not real.</span></span> <span data-ttu-id="64bae-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="64bae-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="64bae-163">Obraťte se na [tým podpory EverBridge](mailto:support@everbridge.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="64bae-163">Contact [EverBridge support team](mailto:support@everbridge.com) to get these values.</span></span>
 
4. <span data-ttu-id="64bae-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="64bae-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_certificate.png) 

5. <span data-ttu-id="64bae-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="64bae-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="64bae-168">Na **EverBridge konfigurace** klikněte na tlačítko **konfigurace EverBridge** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="64bae-168">On the **EverBridge Configuration** section, click **Configure EverBridge** to open **Configure sign-on** window.</span></span> <span data-ttu-id="64bae-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="64bae-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_configure.png) 

6. <span data-ttu-id="64bae-171">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte k přihlašování ke klientovi Everbridge jako správce.</span><span class="sxs-lookup"><span data-stu-id="64bae-171">To get SSO configured for your application, you need to sign-on to your Everbridge tenant as an administrator.</span></span>

7. <span data-ttu-id="64bae-172">V nabídce v horní části, klikněte **nastavení** a vyberte **jednotné přihlašování** pod **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="64bae-172">In the menu on the top, click the **Settings** tab and select **Single Sign-On** under **Security**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)
   
    <span data-ttu-id="64bae-174">a.</span><span class="sxs-lookup"><span data-stu-id="64bae-174">a.</span></span> <span data-ttu-id="64bae-175">V **název** textovému poli, zadejte název zprostředkovatele identifikátor (například: název vaší společnosti).</span><span class="sxs-lookup"><span data-stu-id="64bae-175">In the **Name** textbox, type the name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="64bae-176">b.</span><span class="sxs-lookup"><span data-stu-id="64bae-176">b.</span></span> <span data-ttu-id="64bae-177">V **název rozhraní API** textovému poli, zadejte název rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="64bae-177">In the **API Name** textbox, type the name of API.</span></span>
   
    <span data-ttu-id="64bae-178">c.</span><span class="sxs-lookup"><span data-stu-id="64bae-178">c.</span></span> <span data-ttu-id="64bae-179">Klikněte na tlačítko **zvolit soubor** tlačítko Nahrát soubor metadat, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="64bae-179">Click **Choose File** button to upload the metadata file which you downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="64bae-180">d.</span><span class="sxs-lookup"><span data-stu-id="64bae-180">d.</span></span> <span data-ttu-id="64bae-181">V umístění Identity SAML, vyberte **identita je v elementu NameIdentifier příkaz subjektu**.</span><span class="sxs-lookup"><span data-stu-id="64bae-181">In the SAML Identity Location, select **Identity is in the NameIdentifier element of the Subject statement**.</span></span>
   
    <span data-ttu-id="64bae-182">e.</span><span class="sxs-lookup"><span data-stu-id="64bae-182">e.</span></span> <span data-ttu-id="64bae-183">V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu adresy URL jednotné přihlašování SAML z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64bae-183">In the **Identity Provider Login URL** textbox, paste the value of SAML SSO URL from Azure AD.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)
   
    <span data-ttu-id="64bae-185">f.</span><span class="sxs-lookup"><span data-stu-id="64bae-185">f.</span></span> <span data-ttu-id="64bae-186">Zprostředkovatele iniciované žádosti vazby služby, vyberte **přesměrování protokolu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="64bae-186">In the Service Provider Initiated Request Binding, select **HTTP Redirect**.</span></span>

    <span data-ttu-id="64bae-187">g.</span><span class="sxs-lookup"><span data-stu-id="64bae-187">g.</span></span> <span data-ttu-id="64bae-188">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="64bae-188">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="64bae-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="64bae-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="64bae-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="64bae-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="64bae-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="64bae-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="64bae-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="64bae-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="64bae-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64bae-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="64bae-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="64bae-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="64bae-196">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="64bae-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="64bae-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="64bae-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="64bae-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64bae-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="64bae-202">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64bae-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="64bae-204">a.</span><span class="sxs-lookup"><span data-stu-id="64bae-204">a.</span></span> <span data-ttu-id="64bae-205">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="64bae-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="64bae-206">b.</span><span class="sxs-lookup"><span data-stu-id="64bae-206">b.</span></span> <span data-ttu-id="64bae-207">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="64bae-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="64bae-208">c.</span><span class="sxs-lookup"><span data-stu-id="64bae-208">c.</span></span> <span data-ttu-id="64bae-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="64bae-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="64bae-210">d.</span><span class="sxs-lookup"><span data-stu-id="64bae-210">d.</span></span> <span data-ttu-id="64bae-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="64bae-211">Click **Create**.</span></span>
 
### <a name="creating-an-everbridge-test-user"></a><span data-ttu-id="64bae-212">Vytváření testovacího uživatele EverBridge</span><span class="sxs-lookup"><span data-stu-id="64bae-212">Creating an EverBridge test user</span></span>

<span data-ttu-id="64bae-213">V této části vytvoříte volal Britta Simon v Everbridge uživatele.</span><span class="sxs-lookup"><span data-stu-id="64bae-213">In this section, you create a user called Britta Simon in Everbridge.</span></span> <span data-ttu-id="64bae-214">Práce s [tým podpory EverBridge](mailto:support@everbridge.com) přidat uživatele do Everbridge platformy.</span><span class="sxs-lookup"><span data-stu-id="64bae-214">Work with [EverBridge support team](mailto:support@everbridge.com) to add the users in the Everbridge platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="64bae-215">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="64bae-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="64bae-216">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu EverBridge.</span><span class="sxs-lookup"><span data-stu-id="64bae-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to EverBridge.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="64bae-218">**Pokud chcete přiřadit Britta Simon EverBridge, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="64bae-218">**To assign Britta Simon to EverBridge, perform the following steps:**</span></span>

1. <span data-ttu-id="64bae-219">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="64bae-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="64bae-221">V seznamu aplikací vyberte **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="64bae-221">In the applications list, select **EverBridge**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_app.png) 

3. <span data-ttu-id="64bae-223">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="64bae-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="64bae-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="64bae-225">Click **Add** button.</span></span> <span data-ttu-id="64bae-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64bae-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="64bae-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="64bae-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="64bae-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64bae-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="64bae-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64bae-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="64bae-231">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="64bae-231">Testing single sign-on</span></span>

<span data-ttu-id="64bae-232">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="64bae-232">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="64bae-233">Když kliknete na dlaždici Everbridge na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Everbridge.</span><span class="sxs-lookup"><span data-stu-id="64bae-233">When you click the Everbridge tile in the Access Panel, you should get automatically signed-on to your Everbridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64bae-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="64bae-234">Additional resources</span></span>

* [<span data-ttu-id="64bae-235">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="64bae-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="64bae-236">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="64bae-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png

