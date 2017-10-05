---
title: 'Kurz: Azure Active Directory integrace s Tableau Online | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Tableau Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 443fab1198a91a4d5749e6421f7b8603fc75a81e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a><span data-ttu-id="30279-103">Kurz: Azure Active Directory integrace s Tableau Online</span><span class="sxs-lookup"><span data-stu-id="30279-103">Tutorial: Azure Active Directory integration with Tableau Online</span></span>

<span data-ttu-id="30279-104">V tomto kurzu zjistěte, jak integrovat Tableau Online s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="30279-104">In this tutorial, you learn how to integrate Tableau Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="30279-105">Integrace Tableau Online s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="30279-105">Integrating Tableau Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="30279-106">Můžete řídit ve službě Azure AD, který má přístup k Tableau Online</span><span class="sxs-lookup"><span data-stu-id="30279-106">You can control in Azure AD who has access to Tableau Online</span></span>
- <span data-ttu-id="30279-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Tableau Online (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="30279-107">You can enable your users to automatically get signed-on to Tableau Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="30279-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="30279-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="30279-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="30279-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30279-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30279-110">Prerequisites</span></span>

<span data-ttu-id="30279-111">Konfigurace integrace Azure AD s Tableau Online, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="30279-111">To configure Azure AD integration with Tableau Online, you need the following items:</span></span>

- <span data-ttu-id="30279-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="30279-112">An Azure AD subscription</span></span>
- <span data-ttu-id="30279-113">Tableau Online jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="30279-113">A Tableau Online single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="30279-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="30279-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="30279-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="30279-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="30279-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="30279-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="30279-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30279-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="30279-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="30279-118">Scenario description</span></span>
<span data-ttu-id="30279-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="30279-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="30279-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="30279-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="30279-121">Přidání Tableau Online z Galerie</span><span class="sxs-lookup"><span data-stu-id="30279-121">Adding Tableau Online from the gallery</span></span>
2. <span data-ttu-id="30279-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="30279-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-online-from-the-gallery"></a><span data-ttu-id="30279-123">Přidání Tableau Online z Galerie</span><span class="sxs-lookup"><span data-stu-id="30279-123">Adding Tableau Online from the gallery</span></span>
<span data-ttu-id="30279-124">Při konfiguraci integrace služby Tableau Online do služby Azure AD, musíte přidat Tableau Online z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="30279-124">To configure the integration of Tableau Online into Azure AD, you need to add Tableau Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="30279-125">**Pokud chcete přidat Tableau Online z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="30279-125">**To add Tableau Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="30279-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="30279-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="30279-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="30279-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="30279-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="30279-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="30279-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="30279-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="30279-133">Do vyhledávacího pole zadejte **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="30279-133">In the search box, type **Tableau Online**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. <span data-ttu-id="30279-135">Na panelu výsledků vyberte **Tableau Online**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="30279-135">In the results panel, select **Tableau Online**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="30279-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="30279-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="30279-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tableau Online na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="30279-138">In this section, you configure and test Azure AD single sign-on with Tableau Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="30279-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Tableau Online je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="30279-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tableau Online is to a user in Azure AD.</span></span> <span data-ttu-id="30279-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="30279-140">In other words, a link relationship between an Azure AD user and the related user in Tableau Online needs to be established.</span></span>

<span data-ttu-id="30279-141">V Tableau Online přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="30279-141">In Tableau Online, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="30279-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tableau Online, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="30279-142">To configure and test Azure AD single sign-on with Tableau Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="30279-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="30279-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="30279-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30279-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="30279-145">**[Vytvoření zkušebního uživatele Tableau Online](#creating-a-tableau-online-test-user)**  – Pokud chcete mít protějšek Britta Simon v Tableau Online propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="30279-145">**[Creating a Tableau Online test user](#creating-a-tableau-online-test-user)** - to have a counterpart of Britta Simon in Tableau Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="30279-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="30279-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="30279-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="30279-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="30279-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="30279-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="30279-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="30279-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tableau Online application.</span></span>

<span data-ttu-id="30279-150">**Ke konfiguraci Azure AD jednotné přihlašování s Tableau Online, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="30279-150">**To configure Azure AD single sign-on with Tableau Online, perform the following steps:**</span></span>

1. <span data-ttu-id="30279-151">Na portálu Azure na **Tableau Online** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="30279-151">In the Azure portal, on the **Tableau Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="30279-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="30279-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. <span data-ttu-id="30279-155">Na **Tableau Online domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="30279-155">On the **Tableau Online Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    <span data-ttu-id="30279-157">a.</span><span class="sxs-lookup"><span data-stu-id="30279-157">a.</span></span> <span data-ttu-id="30279-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://sso.online.tableau.com`</span><span class="sxs-lookup"><span data-stu-id="30279-158">In the **Sign-on URL** textbox, type the URL: `https://sso.online.tableau.com`</span></span>

    <span data-ttu-id="30279-159">b.</span><span class="sxs-lookup"><span data-stu-id="30279-159">b.</span></span> <span data-ttu-id="30279-160">V **identifikátor** textovému poli, zadejte adresu URL:`https://sso.online.tableau.com/public/sp/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="30279-160">In the **Identifier** textbox, type the URL: `https://sso.online.tableau.com/public/sp/<instancename>`</span></span>

4. <span data-ttu-id="30279-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="30279-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. <span data-ttu-id="30279-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="30279-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="30279-165">V okně jiný prohlížeč přihlášení do aplikace Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="30279-165">In a different browser window, sign-on to your Tableau Online application.</span></span> <span data-ttu-id="30279-166">Přejděte na **nastavení** a potom **ověřování**.</span><span class="sxs-lookup"><span data-stu-id="30279-166">Go to **Settings** and then **Authentication**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. <span data-ttu-id="30279-168">Chcete-li povolit SAML, v části **typy ověřování** části.</span><span class="sxs-lookup"><span data-stu-id="30279-168">To enable SAML, Under **Authentication Types** section.</span></span> <span data-ttu-id="30279-169">Zkontrolujte **jednotné přihlašování s SAML** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="30279-169">Check the **Single sign-on with SAML** checkbox.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. <span data-ttu-id="30279-171">Posuňte se dolů, dokud **soubor Import metadat do Tableau Online** části.</span><span class="sxs-lookup"><span data-stu-id="30279-171">Scroll down until **Import metadata file into Tableau Online** section.</span></span>  <span data-ttu-id="30279-172">Klikněte na tlačítko Procházet a importovat soubor metadat, které jste si stáhli z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="30279-172">Click Browse and import the metadata file you have downloaded from Azure AD.</span></span> <span data-ttu-id="30279-173">Potom klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="30279-173">Then, click **Apply**.</span></span>
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. <span data-ttu-id="30279-175">V **odpovídat kontrolní výrazy** část, vložte s odpovídajícím názvem assertion zprostředkovatele Identity pro **e-mailová adresa**, **křestní jméno**, a **příjmení**.</span><span class="sxs-lookup"><span data-stu-id="30279-175">In the **Match assertions** section, insert the corresponding Identity Provider assertion name for **email address**, **first name**, and **last name**.</span></span> <span data-ttu-id="30279-176">Pokud chcete získat tyto informace z Azure AD:</span><span class="sxs-lookup"><span data-stu-id="30279-176">To get this information from Azure AD:</span></span> 
  
    <span data-ttu-id="30279-177">a.</span><span class="sxs-lookup"><span data-stu-id="30279-177">a.</span></span> <span data-ttu-id="30279-178">Na portálu Azure přejděte **Tableau Online** stránky integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="30279-178">In the Azure portal, go on the **Tableau Online** application integration page.</span></span>
    
    <span data-ttu-id="30279-179">b.</span><span class="sxs-lookup"><span data-stu-id="30279-179">b.</span></span> <span data-ttu-id="30279-180">V části atributy, vyberte **"Zobrazit a upravit všechny ostatní atributy uživatele"** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="30279-180">In the attributes section, Select the **"view and edit all other user attributes"** checkbox.</span></span> 
    
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    <span data-ttu-id="30279-182">c.</span><span class="sxs-lookup"><span data-stu-id="30279-182">c.</span></span> <span data-ttu-id="30279-183">Zkopírujte hodnotu oboru názvů pro tyto atributy: givenname, e-mailu a Přezdívka pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="30279-183">Copy the namespace value for these attributes: givenname, email and surname by using the following steps:</span></span>

   ![Azure AD jednotné přihlášení](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    <span data-ttu-id="30279-185">d.</span><span class="sxs-lookup"><span data-stu-id="30279-185">d.</span></span> <span data-ttu-id="30279-186">Klikněte na tlačítko **user.givenname** hodnota</span><span class="sxs-lookup"><span data-stu-id="30279-186">Click **user.givenname** value</span></span> 
    
    <span data-ttu-id="30279-187">e.</span><span class="sxs-lookup"><span data-stu-id="30279-187">e.</span></span> <span data-ttu-id="30279-188">Zkopírujte hodnotu z **obor názvů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="30279-188">Copy the value from the **namespace** textbox.</span></span>

   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    <span data-ttu-id="30279-190">f.</span><span class="sxs-lookup"><span data-stu-id="30279-190">f.</span></span> <span data-ttu-id="30279-191">Kopírování namesapce hodnoty pro e-mailu a příjmení, postupujte podle předchozích kroků.</span><span class="sxs-lookup"><span data-stu-id="30279-191">To copy the namesapce values for the email and surname follow the preceding steps.</span></span>

    <span data-ttu-id="30279-192">g.</span><span class="sxs-lookup"><span data-stu-id="30279-192">g.</span></span> <span data-ttu-id="30279-193">Přepněte do Tableau Online aplikace a pak nastavte **Tableau Online atributy** části následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="30279-193">Switch to the Tableau Online application, then set the **Tableau Online Attributes** section as follows:</span></span>
     * <span data-ttu-id="30279-194">E-mailu: **e-mailu** nebo **userprincipalname**</span><span class="sxs-lookup"><span data-stu-id="30279-194">Email: **mail** or **userprincipalname**</span></span>
     * <span data-ttu-id="30279-195">Křestní jméno: **givenname**</span><span class="sxs-lookup"><span data-stu-id="30279-195">First name: **givenname**</span></span>
     * <span data-ttu-id="30279-196">Příjmení: **Přezdívka**</span><span class="sxs-lookup"><span data-stu-id="30279-196">Last name: **surname**</span></span>
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> <span data-ttu-id="30279-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="30279-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="30279-199">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="30279-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="30279-200">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="30279-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="30279-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="30279-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="30279-202">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30279-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="30279-204">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="30279-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="30279-205">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="30279-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="30279-207">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="30279-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="30279-209">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="30279-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="30279-211">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="30279-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="30279-213">a.</span><span class="sxs-lookup"><span data-stu-id="30279-213">a.</span></span> <span data-ttu-id="30279-214">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="30279-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="30279-215">b.</span><span class="sxs-lookup"><span data-stu-id="30279-215">b.</span></span> <span data-ttu-id="30279-216">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="30279-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="30279-217">c.</span><span class="sxs-lookup"><span data-stu-id="30279-217">c.</span></span> <span data-ttu-id="30279-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="30279-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="30279-219">d.</span><span class="sxs-lookup"><span data-stu-id="30279-219">d.</span></span> <span data-ttu-id="30279-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="30279-220">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-online-test-user"></a><span data-ttu-id="30279-221">Vytvoření Tableau Online zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="30279-221">Creating a Tableau Online test user</span></span>

<span data-ttu-id="30279-222">V této části vytvoříte uživatele volal Britta Simon v Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="30279-222">In this section, you create a user called Britta Simon in Tableau Online.</span></span>

1. <span data-ttu-id="30279-223">Na **Tableau Online**, klikněte na tlačítko **nastavení** a potom **ověřování** části.</span><span class="sxs-lookup"><span data-stu-id="30279-223">On **Tableau Online**, click **Settings** and then **Authentication** section.</span></span> <span data-ttu-id="30279-224">Přejděte dolů k položce **vybrat uživatele** části.</span><span class="sxs-lookup"><span data-stu-id="30279-224">Scroll down to **Select Users** section.</span></span> <span data-ttu-id="30279-225">Klikněte na tlačítko **přidat uživatele** a potom **zadejte e-mailové adresy**.</span><span class="sxs-lookup"><span data-stu-id="30279-225">Click **Add Users** and then **Enter Email Addresses**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. <span data-ttu-id="30279-227">Vyberte **přidat uživatele pro jednotné přihlašování (SSO) ověřování**.</span><span class="sxs-lookup"><span data-stu-id="30279-227">Select **Add users for single sign-on (SSO) authentication**.</span></span> <span data-ttu-id="30279-228">V **zadejte e-mailové adresy** přidat textové polebritta.simon@contoso.com</span><span class="sxs-lookup"><span data-stu-id="30279-228">In the **Enter Email Addresses** textbox add britta.simon@contoso.com</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. <span data-ttu-id="30279-230">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="30279-230">Click **Create**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="30279-231">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="30279-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="30279-232">V této části povolíte Britta Simon tak, že udělíte přístup k Tableau Online používat Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="30279-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tableau Online.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="30279-234">**Pokud chcete přiřadit Britta Simon Tableau Online, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="30279-234">**To assign Britta Simon to Tableau Online, perform the following steps:**</span></span>

1. <span data-ttu-id="30279-235">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="30279-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="30279-237">V seznamu aplikací vyberte **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="30279-237">In the applications list, select **Tableau Online**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. <span data-ttu-id="30279-239">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="30279-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="30279-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="30279-241">Click **Add** button.</span></span> <span data-ttu-id="30279-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="30279-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="30279-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="30279-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="30279-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="30279-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="30279-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="30279-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="30279-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="30279-247">Testing single sign-on</span></span>

<span data-ttu-id="30279-248">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="30279-248">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="30279-249">Když kliknete na dlaždici Tableau Online na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="30279-249">When you click the Tableau Online tile in the Access Panel, you should get automatically signed-on to your Tableau Online application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30279-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="30279-250">Additional resources</span></span>

* [<span data-ttu-id="30279-251">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="30279-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="30279-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="30279-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

