---
title: 'Kurz: Azure Active Directory integrace s DocuSign | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 29c99fdf39d366df90abc070f7b836320935035c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a><span data-ttu-id="e31b4-103">Kurz: Azure Active Directory integrace s DocuSign</span><span class="sxs-lookup"><span data-stu-id="e31b4-103">Tutorial: Azure Active Directory integration with DocuSign</span></span>

<span data-ttu-id="e31b4-104">V tomto kurzu zjistěte, jak integrovat DocuSign s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e31b4-104">In this tutorial, you learn how to integrate DocuSign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e31b4-105">Integrace DocuSign s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e31b4-105">Integrating DocuSign with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e31b4-106">Můžete řídit ve službě Azure AD, který má přístup k DocuSign</span><span class="sxs-lookup"><span data-stu-id="e31b4-106">You can control in Azure AD who has access to DocuSign</span></span>
- <span data-ttu-id="e31b4-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k DocuSign (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e31b4-107">You can enable your users to automatically get signed-on to DocuSign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e31b4-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e31b4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e31b4-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e31b4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e31b4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e31b4-110">Prerequisites</span></span>

<span data-ttu-id="e31b4-111">Konfigurace integrace Azure AD s DocuSign, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e31b4-111">To configure Azure AD integration with DocuSign, you need the following items:</span></span>

- <span data-ttu-id="e31b4-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e31b4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e31b4-113">DocuSign jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e31b4-113">A DocuSign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e31b4-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e31b4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e31b4-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e31b4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e31b4-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e31b4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e31b4-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e31b4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e31b4-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e31b4-118">Scenario description</span></span>
<span data-ttu-id="e31b4-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e31b4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e31b4-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e31b4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e31b4-121">Přidání DocuSign z Galerie</span><span class="sxs-lookup"><span data-stu-id="e31b4-121">Adding DocuSign from the gallery</span></span>
2. <span data-ttu-id="e31b4-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e31b4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-docusign-from-the-gallery"></a><span data-ttu-id="e31b4-123">Přidání DocuSign z Galerie</span><span class="sxs-lookup"><span data-stu-id="e31b4-123">Adding DocuSign from the gallery</span></span>
<span data-ttu-id="e31b4-124">Při konfiguraci integrace DocuSign do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS DocuSign z galerie.</span><span class="sxs-lookup"><span data-stu-id="e31b4-124">To configure the integration of DocuSign into Azure AD, you need to add DocuSign from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e31b4-125">**Pokud chcete přidat DocuSign z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e31b4-125">**To add DocuSign from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e31b4-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e31b4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e31b4-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e31b4-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e31b4-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e31b4-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e31b4-133">Do vyhledávacího pole zadejte **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-133">In the search box, type **DocuSign**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. <span data-ttu-id="e31b4-135">Na panelu výsledků vyberte **DocuSign**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e31b4-135">In the results panel, select **DocuSign**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e31b4-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e31b4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e31b4-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s DocuSign podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e31b4-138">In this section, you configure and test Azure AD single sign-on with DocuSign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e31b4-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v DocuSign je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e31b4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in DocuSign is to a user in Azure AD.</span></span> <span data-ttu-id="e31b4-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v DocuSign musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e31b4-140">In other words, a link relationship between an Azure AD user and the related user in DocuSign needs to be established.</span></span>

<span data-ttu-id="e31b4-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v DocuSign.</span><span class="sxs-lookup"><span data-stu-id="e31b4-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in DocuSign.</span></span>

<span data-ttu-id="e31b4-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s DocuSign, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e31b4-142">To configure and test Azure AD single sign-on with DocuSign, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e31b4-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e31b4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e31b4-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e31b4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e31b4-145">**[Vytvoření zkušebního uživatele DocuSign](#creating-a-docusign-test-user)**  – Pokud chcete mít protějšek Britta Simon v DocuSign propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e31b4-145">**[Creating a DocuSign test user](#creating-a-docusign-test-user)** - to have a counterpart of Britta Simon in DocuSign that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e31b4-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e31b4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e31b4-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e31b4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e31b4-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e31b4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e31b4-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci DocuSign.</span><span class="sxs-lookup"><span data-stu-id="e31b4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your DocuSign application.</span></span>

<span data-ttu-id="e31b4-150">**Ke konfiguraci Azure AD jednotné přihlašování s DocuSign, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e31b4-150">**To configure Azure AD single sign-on with DocuSign, perform the following steps:**</span></span>

1. <span data-ttu-id="e31b4-151">Na portálu Azure na **DocuSign** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-151">In the Azure portal, on the **DocuSign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e31b4-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e31b4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. <span data-ttu-id="e31b4-155">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base 64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="e31b4-155">On the **SAML Signing Certificate** section, click **Certificate(Base 64)** and then save certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. <span data-ttu-id="e31b4-157">Na **DocuSign konfigurace** části portálu Azure, klikněte na tlačítko **konfigurace DocuSign** otevřete konfigurovat přihlašování okno.</span><span class="sxs-lookup"><span data-stu-id="e31b4-157">On the **DocuSign Configuration** section of Azure portal, Click **Configure DocuSign** to open Configure sign-on window.</span></span> <span data-ttu-id="e31b4-158">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e31b4-158">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. <span data-ttu-id="e31b4-160">V okně prohlížeče jiný web a přihlášení k vaší **portál pro správu DocuSign** jako správce.</span><span class="sxs-lookup"><span data-stu-id="e31b4-160">In a different web browser window, login to your **DocuSign admin portal** as an administrator.</span></span>

6. <span data-ttu-id="e31b4-161">V navigační nabídce na levé straně, klikněte na tlačítko **domény**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-161">In the navigation menu on the left, click **Domains**.</span></span>
   
    ![Konfigurace jednotného přihlašování][51]

7. <span data-ttu-id="e31b4-163">V pravém podokně klikněte na tlačítko **deklarace identity domény**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-163">On the right pane, click **Claim Domain**.</span></span>
   
    ![Konfigurace jednotného přihlašování][52]

8. <span data-ttu-id="e31b4-165">Na **deklarace identity domény** dialogové okno, v **název domény** textovému poli, zadejte doménu vaší společnosti a pak klikněte na tlačítko **deklarace identity**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-165">On the **Claim a domain** dialog, in the **Domain Name** textbox, type your company domain, and then click **Claim**.</span></span> <span data-ttu-id="e31b4-166">Ujistěte se, že ověření domény a stav je aktivní.</span><span class="sxs-lookup"><span data-stu-id="e31b4-166">Make sure that you verify the domain and the status is active.</span></span>
   
    ![Konfigurace jednotného přihlašování][53]

9. <span data-ttu-id="e31b4-168">V nabídce na levé straně klikněte na tlačítko **poskytovatelů identit**</span><span class="sxs-lookup"><span data-stu-id="e31b4-168">In menu on the left side, click **Identity Providers**</span></span>  
   
    ![Konfigurace jednotného přihlašování][54]
10. <span data-ttu-id="e31b4-170">V pravém podokně klikněte na **přidat zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-170">In the right pane, click **Add Identity Provider**.</span></span> 
   
    ![Konfigurace jednotného přihlašování][55]

11. <span data-ttu-id="e31b4-172">Na **nastavení zprostředkovatele Identity** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e31b4-172">On the **Identity Provider Settings** page, perform the following steps:</span></span>
   
    ![Konfigurace jednotného přihlašování][56]

    <span data-ttu-id="e31b4-174">a.</span><span class="sxs-lookup"><span data-stu-id="e31b4-174">a.</span></span> <span data-ttu-id="e31b4-175">V **název** textovému poli, zadejte jedinečný název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e31b4-175">In the **Name** textbox, type a unique name for your configuration.</span></span> <span data-ttu-id="e31b4-176">Nepoužívejte mezery.</span><span class="sxs-lookup"><span data-stu-id="e31b4-176">Do not use spaces.</span></span>

    <span data-ttu-id="e31b4-177">b.</span><span class="sxs-lookup"><span data-stu-id="e31b4-177">b.</span></span> <span data-ttu-id="e31b4-178">Vložení **SAML Entity ID** do **vystavitele zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e31b4-178">Paste **SAML Entity ID** into the **Identity Provider Issuer** textbox.</span></span>

    <span data-ttu-id="e31b4-179">c.</span><span class="sxs-lookup"><span data-stu-id="e31b4-179">c.</span></span> <span data-ttu-id="e31b4-180">Vložení **SAML jeden přihlašování adresa URL služby** do **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e31b4-180">Paste **SAML Single Sign-On Service URL** into the **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="e31b4-181">d.</span><span class="sxs-lookup"><span data-stu-id="e31b4-181">d.</span></span> <span data-ttu-id="e31b4-182">Vložení **Sign-Out URL** do **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e31b4-182">Paste **Sign-Out URL** into the **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="e31b4-183">e.</span><span class="sxs-lookup"><span data-stu-id="e31b4-183">e.</span></span> <span data-ttu-id="e31b4-184">Vyberte **přihlásit ověřovacího požadavku**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-184">Select **Sign AuthN Request**.</span></span>

    <span data-ttu-id="e31b4-185">f.</span><span class="sxs-lookup"><span data-stu-id="e31b4-185">f.</span></span> <span data-ttu-id="e31b4-186">Jako **odeslání ověřovacího požadavku pomocí**, vyberte **POST**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-186">As **Send AuthN request by**, select **POST**.</span></span>

    <span data-ttu-id="e31b4-187">g.</span><span class="sxs-lookup"><span data-stu-id="e31b4-187">g.</span></span> <span data-ttu-id="e31b4-188">Jako **odeslán požadavek na odhlášení podle**, vyberte **získat**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-188">As **Send logout request by**, select **GET**.</span></span>

12. <span data-ttu-id="e31b4-189">V **vlastní atribut mapování** zvolte pole, které chcete propojit s Azure AD deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="e31b4-189">In the **Custom Attribute Mapping** section, choose the field you want to map with Azure AD Claim.</span></span> <span data-ttu-id="e31b4-190">V tomto příkladu **emailaddress** deklarace identity je namapována na hodnotu **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-190">In this example, the **emailaddress** claim is mapped with the value of **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span> <span data-ttu-id="e31b4-191">Je výchozí název deklarace identity z Azure AD pro deklarace identity e-mailu.</span><span class="sxs-lookup"><span data-stu-id="e31b4-191">It is the default claim name from Azure AD for email claim.</span></span> 
   
    > [!NOTE]
    > <span data-ttu-id="e31b4-192">Použít příslušné **uživatelský identifikátor** k mapování uživatelů ze služby Azure AD pro mapování uživatele DocuSign.</span><span class="sxs-lookup"><span data-stu-id="e31b4-192">Use the appropriate **User identifier** to map the user from Azure AD to DocuSign user mapping.</span></span> <span data-ttu-id="e31b4-193">Vyberte správné pole a zadejte odpovídající hodnotu na základě nastavení vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="e31b4-193">Select the proper Field and enter the appropriate value based on your organization settings.</span></span>
          
    ![Konfigurace jednotného přihlašování][57]

13. <span data-ttu-id="e31b4-195">V **certifikát zprostředkovatele Identity** klikněte na tlačítko **přidat certifikát**a pak nahrajte certifikát, který jste si stáhli z portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e31b4-195">In the **Identity Provider Certificate** section, click **Add Certificate**, and then upload the certificate you have downloaded from Azure AD portal.</span></span>   
   
    ![Konfigurace jednotného přihlašování][58]

14. <span data-ttu-id="e31b4-197">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-197">Click **Save**.</span></span>

15. <span data-ttu-id="e31b4-198">V **zprostředkovatelů Identity** klikněte na tlačítko **akce**a potom klikněte na **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-198">In the **Identity Providers** section, click **Actions**, and then click **Endpoints**.</span></span>   
   
    ![Konfigurace jednotného přihlašování][59]
 
16. <span data-ttu-id="e31b4-200">V **zobrazit SAML 2.0 koncové body** části na **portál pro správu DocuSign**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e31b4-200">In the **View SAML 2.0 Endpoints** section on **DocuSign admin portal**, perform the following steps:</span></span>
   
    ![Konfigurace jednotného přihlašování][60]
   
    <span data-ttu-id="e31b4-202">a.</span><span class="sxs-lookup"><span data-stu-id="e31b4-202">a.</span></span> <span data-ttu-id="e31b4-203">Kopírování **adresa URL vystavitele pro zprostředkovatele služby**a vložte do **identifikátor** textové pole na **DocuSign domény a adresy URL** části portálu Azure následující vzoru: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span><span class="sxs-lookup"><span data-stu-id="e31b4-203">Copy the **Service Provider Issuer URL**, and then paste into the **Identifier** textbox on **DocuSign Domain and URLs** section of the Azure portal following the pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span></span>
   
    <span data-ttu-id="e31b4-204">b.</span><span class="sxs-lookup"><span data-stu-id="e31b4-204">b.</span></span> <span data-ttu-id="e31b4-205">Kopírování **adresa URL služby zprostředkovatele přihlášení**a vložte do **přihlašovací adresa URL** textové pole na **DocuSign domény a adresy URL** části portálu Azure následující vzoru: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span><span class="sxs-lookup"><span data-stu-id="e31b4-205">Copy the **Service Provider Login URL**, and then paste into the **Sign On URL** textbox on **DocuSign Domain and URLs** section of the Azure portal following the pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    <span data-ttu-id="e31b4-207">c.</span><span class="sxs-lookup"><span data-stu-id="e31b4-207">c.</span></span>  <span data-ttu-id="e31b4-208">Klikněte na tlačítko **zavřít**</span><span class="sxs-lookup"><span data-stu-id="e31b4-208">Click **Close**</span></span>
    
17. <span data-ttu-id="e31b4-209">Na portálu Azure klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-209">On the Azure portal, click **Save**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="e31b4-211">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e31b4-211">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e31b4-212">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e31b4-212">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e31b4-213">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e31b4-213">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e31b4-214">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e31b4-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="e31b4-215">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e31b4-215">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e31b4-217">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e31b4-217">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e31b4-218">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e31b4-218">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e31b4-220">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-220">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e31b4-222">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e31b4-222">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e31b4-224">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e31b4-224">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e31b4-226">a.</span><span class="sxs-lookup"><span data-stu-id="e31b4-226">a.</span></span> <span data-ttu-id="e31b4-227">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-227">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e31b4-228">b.</span><span class="sxs-lookup"><span data-stu-id="e31b4-228">b.</span></span> <span data-ttu-id="e31b4-229">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e31b4-229">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e31b4-230">c.</span><span class="sxs-lookup"><span data-stu-id="e31b4-230">c.</span></span> <span data-ttu-id="e31b4-231">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-231">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e31b4-232">d.</span><span class="sxs-lookup"><span data-stu-id="e31b4-232">d.</span></span> <span data-ttu-id="e31b4-233">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-233">Click **Create**.</span></span>
 
### <a name="creating-a-docusign-test-user"></a><span data-ttu-id="e31b4-234">Vytvoření zkušebního uživatele DocuSign</span><span class="sxs-lookup"><span data-stu-id="e31b4-234">Creating a DocuSign test user</span></span>

<span data-ttu-id="e31b4-235">Aplikace podporuje **těsně v zřizování uživatelů čas** a po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e31b4-235">Application supports **Just in time user provisioning** and after authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e31b4-236">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e31b4-236">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e31b4-237">V této části povolíte Britta Simon používat tak, že udělíte přístup k DocuSign Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e31b4-237">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to DocuSign.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e31b4-239">**Pokud chcete přiřadit Britta Simon DocuSign, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e31b4-239">**To assign Britta Simon to DocuSign, perform the following steps:**</span></span>

1. <span data-ttu-id="e31b4-240">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-240">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e31b4-242">V seznamu aplikací vyberte **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-242">In the applications list, select **DocuSign**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. <span data-ttu-id="e31b4-244">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e31b4-244">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e31b4-246">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e31b4-246">Click **Add** button.</span></span> <span data-ttu-id="e31b4-247">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e31b4-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e31b4-249">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e31b4-249">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e31b4-250">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e31b4-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e31b4-251">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e31b4-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e31b4-252">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e31b4-252">Testing single sign-on</span></span>

<span data-ttu-id="e31b4-253">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e31b4-253">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e31b4-254">Když kliknete na dlaždici DocuSign na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci DocuSign.</span><span class="sxs-lookup"><span data-stu-id="e31b4-254">When you click the DocuSign tile in the Access Panel, you should get automatically signed-on to your DocuSign application.</span></span>
<span data-ttu-id="e31b4-255">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e31b4-255">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e31b4-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e31b4-256">Additional resources</span></span>

* [<span data-ttu-id="e31b4-257">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e31b4-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e31b4-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e31b4-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="e31b4-259">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="e31b4-259">Configure User Provisioning</span></span>](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png

