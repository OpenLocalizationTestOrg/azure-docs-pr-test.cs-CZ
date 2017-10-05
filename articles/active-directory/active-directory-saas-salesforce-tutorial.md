---
title: 'Kurz: Azure Active Directory integrace s Salesforce | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 639e40ca7e406a1726033e9f5c5363c289087589
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a><span data-ttu-id="60f5f-103">Kurz: Azure Active Directory integrace s Salesforce</span><span class="sxs-lookup"><span data-stu-id="60f5f-103">Tutorial: Azure Active Directory integration with Salesforce</span></span>

<span data-ttu-id="60f5f-104">V tomto kurzu zjistěte, jak integrovat Salesforce s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="60f5f-104">In this tutorial, you learn how to integrate Salesforce with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="60f5f-105">Integrace služby Salesforce s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="60f5f-105">Integrating Salesforce with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="60f5f-106">Můžete řídit ve službě Azure AD, který má přístup do služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="60f5f-106">You can control in Azure AD who has access to Salesforce</span></span>
- <span data-ttu-id="60f5f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Salesforce (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="60f5f-107">You can enable your users to automatically get signed-on to Salesforce (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="60f5f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="60f5f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="60f5f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="60f5f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60f5f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="60f5f-110">Prerequisites</span></span>

<span data-ttu-id="60f5f-111">Ke konfiguraci integrace služby Azure AD pomocí služby Salesforce, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="60f5f-111">To configure Azure AD integration with Salesforce, you need the following items:</span></span>

- <span data-ttu-id="60f5f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="60f5f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="60f5f-113">Služby Salesforce jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="60f5f-113">A Salesforce single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="60f5f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="60f5f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="60f5f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="60f5f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="60f5f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="60f5f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="60f5f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="60f5f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="60f5f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="60f5f-118">Scenario description</span></span>
<span data-ttu-id="60f5f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="60f5f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="60f5f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="60f5f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="60f5f-121">Přidání Salesforce z Galerie</span><span class="sxs-lookup"><span data-stu-id="60f5f-121">Adding Salesforce from the gallery</span></span>
2. <span data-ttu-id="60f5f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="60f5f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-from-the-gallery"></a><span data-ttu-id="60f5f-123">Přidání Salesforce z Galerie</span><span class="sxs-lookup"><span data-stu-id="60f5f-123">Adding Salesforce from the gallery</span></span>
<span data-ttu-id="60f5f-124">Při konfiguraci integrace služby Salesforce do služby Azure AD potřebujete přidat Salesforce z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="60f5f-124">To configure the integration of Salesforce into Azure AD, you need to add Salesforce from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="60f5f-125">**Pokud chcete přidat Salesforce z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60f5f-125">**To add Salesforce from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="60f5f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="60f5f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="60f5f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="60f5f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="60f5f-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f5f-131">Click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="60f5f-133">Do vyhledávacího pole zadejte **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-133">In the search box, type **Salesforce**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. <span data-ttu-id="60f5f-135">Na panelu výsledků vyberte **Salesforce**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="60f5f-135">In the results panel, select **Salesforce**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="60f5f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="60f5f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="60f5f-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Salesforce podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="60f5f-138">In this section, you configure and test Azure AD single sign-on with Salesforce based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="60f5f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Salesforce je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="60f5f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Salesforce is to a user in Azure AD.</span></span> <span data-ttu-id="60f5f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Salesforce musí navázat.</span><span class="sxs-lookup"><span data-stu-id="60f5f-140">In other words, a link relationship between an Azure AD user and the related user in Salesforce needs to be established.</span></span>

<span data-ttu-id="60f5f-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="60f5f-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Salesforce.</span></span>

<span data-ttu-id="60f5f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí služby Salesforce, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="60f5f-142">To configure and test Azure AD single sign-on with Salesforce, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="60f5f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="60f5f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="60f5f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60f5f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="60f5f-145">**[Vytvoření zkušebního uživatele Salesforce](#creating-a-salesforce-test-user)**  – Pokud chcete mít protějšek Britta Simon v Salesforce, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="60f5f-145">**[Creating a Salesforce test user](#creating-a-salesforce-test-user)** - to have a counterpart of Britta Simon in Salesforce that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="60f5f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="60f5f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="60f5f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="60f5f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="60f5f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="60f5f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="60f5f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Salesforce.</span><span class="sxs-lookup"><span data-stu-id="60f5f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Salesforce application.</span></span>

<span data-ttu-id="60f5f-150">**Ke konfiguraci Azure AD jednotné přihlašování pomocí služby Salesforce, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60f5f-150">**To configure Azure AD single sign-on with Salesforce, perform the following steps:**</span></span>

1. <span data-ttu-id="60f5f-151">Na portálu Azure na **Salesforce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-151">In the Azure portal, on the **Salesforce** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="60f5f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="60f5f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. <span data-ttu-id="60f5f-155">Na **Salesforce domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60f5f-155">On the **Salesforce Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    <span data-ttu-id="60f5f-157">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="60f5f-157">In the **Sign-on URL** textbox, type the value using the following pattern:</span></span> 
   * <span data-ttu-id="60f5f-158">Účet organizace:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="60f5f-158">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
   * <span data-ttu-id="60f5f-159">Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="60f5f-159">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="60f5f-160">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="60f5f-160">These values are not the real.</span></span> <span data-ttu-id="60f5f-161">Tyto hodnoty aktualizujte s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="60f5f-161">Update these values with the actual Sign-on URL.</span></span> <span data-ttu-id="60f5f-162">Obraťte se na [tým podpory služby Salesforce klienta](https://help.salesforce.com/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="60f5f-162">Contact [Salesforce Client support team](https://help.salesforce.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="60f5f-163">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="60f5f-163">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. <span data-ttu-id="60f5f-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60f5f-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="60f5f-167">Na **Salesforce konfigurace** klikněte na tlačítko **konfigurace Salesforce** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="60f5f-167">On the **Salesforce Configuration** section, click **Configure Salesforce** to open **Configure sign-on** window.</span></span> <span data-ttu-id="60f5f-168">Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="60f5f-168">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span> 

    <span data-ttu-id="60f5f-169">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="60f5f-169">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span></span>
7.  <span data-ttu-id="60f5f-170">V prohlížeči a přihlaste se ke svému účtu Salesforce správce otevřete novou kartu.</span><span class="sxs-lookup"><span data-stu-id="60f5f-170">Open a new tab in your browser and log in to your Salesforce administrator account.</span></span>

8.  <span data-ttu-id="60f5f-171">V části **správce** navigačním podokně klikněte na tlačítko **ovládacích prvků zabezpečení** rozbalte související část.</span><span class="sxs-lookup"><span data-stu-id="60f5f-171">Under the **Administrator** navigation pane, click **Security Controls** to expand the related section.</span></span> <span data-ttu-id="60f5f-172">Pak klikněte na tlačítko **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-172">Then click **Single Sign-On Settings**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  <span data-ttu-id="60f5f-174">Na **nastavení jednotného přihlašování** klikněte na tlačítko **upravit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60f5f-174">On the **Single Sign-On Settings** page, click the **Edit** button.</span></span>
    <span data-ttu-id="60f5f-175">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span><span class="sxs-lookup"><span data-stu-id="60f5f-175">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span></span>

      > [!NOTE]
      > <span data-ttu-id="60f5f-176">Pokud nelze povolit nastavení jednotného přihlašování pro váš účet Salesforce, budete možná muset kontaktovat [tým podpory služby Salesforce klienta](https://help.salesforce.com/support).</span><span class="sxs-lookup"><span data-stu-id="60f5f-176">If you are unable to enable Single Sign-On settings for your Salesforce account, you may need to contact [Salesforce Client support team](https://help.salesforce.com/support).</span></span> 

10. <span data-ttu-id="60f5f-177">Vyberte **povoleno SAML**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-177">Select **SAML Enabled**, and then click **Save**.</span></span>

      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. <span data-ttu-id="60f5f-179">Chcete-li nakonfigurovat SAML jeden přihlašování nastavení, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-179">To configure your SAML single sign-on settings, click **New**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. <span data-ttu-id="60f5f-181">Na **SAML jeden přihlašování nastavení upravit** stránky, proveďte následující konfigurace:</span><span class="sxs-lookup"><span data-stu-id="60f5f-181">On the **SAML Single Sign-On Setting Edit** page, make the following configurations:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    <span data-ttu-id="60f5f-183">a.</span><span class="sxs-lookup"><span data-stu-id="60f5f-183">a.</span></span> <span data-ttu-id="60f5f-184">Pro **název** pole, zadejte popisný název pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="60f5f-184">For the **Name** field, type in a friendly name for this configuration.</span></span> <span data-ttu-id="60f5f-185">Poskytuje hodnotu pro **název** automaticky vyplnit **název rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="60f5f-185">Providing a value for **Name** automatically populate the **API Name** textbox.</span></span>

    <span data-ttu-id="60f5f-186">b.</span><span class="sxs-lookup"><span data-stu-id="60f5f-186">b.</span></span> <span data-ttu-id="60f5f-187">Vložení **SMAL Entity ID** hodnoty do **vystavitele** pole v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="60f5f-187">Paste **SMAL Entity ID** value into the **Issuer** field in Salesforce.</span></span>

    <span data-ttu-id="60f5f-188">c.</span><span class="sxs-lookup"><span data-stu-id="60f5f-188">c.</span></span> <span data-ttu-id="60f5f-189">V **textové pole Entity Id**, zadejte název domény vaší služby Salesforce pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="60f5f-189">In the **Entity Id textbox**, type your Salesforce domain name using the following pattern:</span></span>
      
      * <span data-ttu-id="60f5f-190">Účet organizace:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="60f5f-190">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
      * <span data-ttu-id="60f5f-191">Vývojářský účet:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="60f5f-191">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>
      
    <span data-ttu-id="60f5f-192">d.</span><span class="sxs-lookup"><span data-stu-id="60f5f-192">d.</span></span> <span data-ttu-id="60f5f-193">Klikněte na tlačítko **Procházet** nebo **zvolit soubor** otevřete **vybrat soubor k odeslání** dialogovém okně, vyberte svůj certifikát Salesforce a pak klikněte na tlačítko **otevřete** na kterou odešlete certifikát.</span><span class="sxs-lookup"><span data-stu-id="60f5f-193">Click **Browse** or **Choose File** to open the **Choose File to Upload** dialog, select your Salesforce certificate, and then click **Open** to upload the certificate.</span></span>

    <span data-ttu-id="60f5f-194">e.</span><span class="sxs-lookup"><span data-stu-id="60f5f-194">e.</span></span> <span data-ttu-id="60f5f-195">Pro **typ Identity SAML**, vyberte **kontrolní výraz obsahuje uživatelské jméno uživatele salesforce.com**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-195">For **SAML Identity Type**, select **Assertion contains User's salesforce.com username**.</span></span>

    <span data-ttu-id="60f5f-196">f.</span><span class="sxs-lookup"><span data-stu-id="60f5f-196">f.</span></span> <span data-ttu-id="60f5f-197">Pro **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier příkaz subjektu**</span><span class="sxs-lookup"><span data-stu-id="60f5f-197">For **SAML Identity Location**, select **Identity is in the NameIdentifier element of the Subject statement**</span></span>

    <span data-ttu-id="60f5f-198">g.</span><span class="sxs-lookup"><span data-stu-id="60f5f-198">g.</span></span> <span data-ttu-id="60f5f-199">Vložení **jeden přihlašování adresa URL služby** do **adresu URL pro přihlášení zprostředkovatele Identity** pole v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="60f5f-199">Paste **Single Sign-On Service URL** into the **Identity Provider Login URL** field in Salesforce.</span></span>
    
    <span data-ttu-id="60f5f-200">h.</span><span class="sxs-lookup"><span data-stu-id="60f5f-200">h.</span></span> <span data-ttu-id="60f5f-201">Pro **zprostředkovatele iniciované žádosti vazby služby**, vyberte **přesměrování protokolu HTTP**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-201">For **Service Provider Initiated Request Binding**, select **HTTP Redirect**.</span></span>
    
    <span data-ttu-id="60f5f-202">i.</span><span class="sxs-lookup"><span data-stu-id="60f5f-202">i.</span></span> <span data-ttu-id="60f5f-203">Nakonec klikněte na **Uložit** použít SAML jeden přihlašování nastavení.</span><span class="sxs-lookup"><span data-stu-id="60f5f-203">Finally, click **Save** to apply your SAML single sign-on settings.</span></span>

13. <span data-ttu-id="60f5f-204">V levém navigačním podokně v Salesforce, klikněte na tlačítko **Správa domén** rozbalte související část, a potom klikněte na **Moje domény**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-204">On the left navigation pane in Salesforce, click **Domain Management** to expand the related section, and then click **My Domain**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. <span data-ttu-id="60f5f-206">Přejděte dolů k položce **konfiguraci ověřování** části a klikněte na tlačítko **upravit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60f5f-206">Scroll down to the **Authentication Configuration** section, and click the **Edit** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. <span data-ttu-id="60f5f-208">V **ověřovací služby** část, vyberte popisný název konfigurace jednotného přihlašování SAML a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-208">In the **Authentication Service** section, select the friendly name of your SAML SSO configuration, and then click **Save**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > <span data-ttu-id="60f5f-210">Pokud je vybrána více než jedna služba ověřování, jsou uživatelé vyzváni k vyberte služby ověřování, která se má při inicializaci jednotné přihlašování pro vaše prostředí Salesforce přihlásit.</span><span class="sxs-lookup"><span data-stu-id="60f5f-210">If more than one authentication service is selected, users are prompted to select which authentication service they like to sign in with while initiating single sign-on to your Salesforce environment.</span></span> <span data-ttu-id="60f5f-211">Pokud nechcete, aby mohla nastat, pak byste měli **nechte nezaškrtnuté všechny ostatní služby ověřování**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-211">If you don’t want it to happen, then you should **leave all other authentication services unchecked**.</span></span>
<CE>    
> [!TIP]
> <span data-ttu-id="60f5f-212">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="60f5f-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="60f5f-213">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="60f5f-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="60f5f-214">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="60f5f-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="60f5f-215">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="60f5f-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="60f5f-216">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60f5f-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="60f5f-218">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60f5f-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="60f5f-219">V levém navigačním podokně v **portál Azure**, klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="60f5f-219">On the left navigation pane in the **Azure portal**, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="60f5f-221">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-221">To display the list of users, Go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="60f5f-223">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f5f-223">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="60f5f-225">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="60f5f-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="60f5f-227">a.</span><span class="sxs-lookup"><span data-stu-id="60f5f-227">a.</span></span> <span data-ttu-id="60f5f-228">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="60f5f-229">b.</span><span class="sxs-lookup"><span data-stu-id="60f5f-229">b.</span></span> <span data-ttu-id="60f5f-230">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="60f5f-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="60f5f-231">c.</span><span class="sxs-lookup"><span data-stu-id="60f5f-231">c.</span></span> <span data-ttu-id="60f5f-232">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="60f5f-233">d.</span><span class="sxs-lookup"><span data-stu-id="60f5f-233">d.</span></span> <span data-ttu-id="60f5f-234">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-234">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-test-user"></a><span data-ttu-id="60f5f-235">Vytvoření zkušebního uživatele Salesforce</span><span class="sxs-lookup"><span data-stu-id="60f5f-235">Creating a Salesforce test user</span></span>

<span data-ttu-id="60f5f-236">V této části se uživatel volá Britta Simon vytvoří v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="60f5f-236">In this section, a user called Britta Simon is created in Salesforce.</span></span> <span data-ttu-id="60f5f-237">Salesforce podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="60f5f-237">Salesforce supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="60f5f-238">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="60f5f-238">There is no action item for you in this section.</span></span> <span data-ttu-id="60f5f-239">Pokud uživatel v Salesforce ještě neexistuje, vytvoří se nový při pokusu o přístup k Salesforce.</span><span class="sxs-lookup"><span data-stu-id="60f5f-239">If a user doesn't already exist in Salesforce, a new one is created when you attempt to access Salesforce.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="60f5f-240">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="60f5f-240">Assigning the Azure AD test user</span></span>

<span data-ttu-id="60f5f-241">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup do služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="60f5f-241">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Salesforce.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="60f5f-243">**Pokud chcete přiřadit Britta Simon Salesforce, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="60f5f-243">**To assign Britta Simon to Salesforce, perform the following steps:**</span></span>

1. <span data-ttu-id="60f5f-244">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-244">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="60f5f-246">V seznamu aplikací vyberte **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-246">In the applications list, select **Salesforce**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. <span data-ttu-id="60f5f-248">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-248">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="60f5f-250">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="60f5f-250">Click **Add** button.</span></span> <span data-ttu-id="60f5f-251">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f5f-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="60f5f-253">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="60f5f-253">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="60f5f-254">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f5f-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="60f5f-255">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="60f5f-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="60f5f-256">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="60f5f-256">Testing single sign-on</span></span>

<span data-ttu-id="60f5f-257">K testování vaše nastavení jednotného přihlašování, otevřete Panel přístupu na [https://myapps.microsoft.com](https://myapps.microsoft.com/), pak se přihlaste do testovací účet a klikněte na tlačítko **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="60f5f-257">To test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), then sign into the test account, and click **Salesforce**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60f5f-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="60f5f-258">Additional resources</span></span>

* [<span data-ttu-id="60f5f-259">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="60f5f-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="60f5f-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="60f5f-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="60f5f-261">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="60f5f-261">Configure User Provisioning</span></span>](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

