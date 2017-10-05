---
title: 'Kurz: Azure Active Directory integrace s Heroku | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Heroku."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d7d72ec6-4a60-4524-8634-26d8fbbcc833
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: d30605e4757b484f327a784b73f939b62ef59373
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-heroku"></a><span data-ttu-id="2f22d-103">Kurz: Azure Active Directory integrace s Heroku</span><span class="sxs-lookup"><span data-stu-id="2f22d-103">Tutorial: Azure Active Directory integration with Heroku</span></span>

<span data-ttu-id="2f22d-104">V tomto kurzu zjistěte, jak integrovat Heroku s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2f22d-104">In this tutorial, you learn how to integrate Heroku with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2f22d-105">Integrace Heroku s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2f22d-105">Integrating Heroku with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2f22d-106">Můžete řídit ve službě Azure AD, který má přístup k Heroku</span><span class="sxs-lookup"><span data-stu-id="2f22d-106">You can control in Azure AD who has access to Heroku</span></span>
- <span data-ttu-id="2f22d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Heroku (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f22d-107">You can enable your users to automatically get signed-on to Heroku (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2f22d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2f22d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2f22d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2f22d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f22d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2f22d-110">Prerequisites</span></span>

<span data-ttu-id="2f22d-111">Konfigurace integrace Azure AD s Heroku, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="2f22d-111">To configure Azure AD integration with Heroku, you need the following items:</span></span>

- <span data-ttu-id="2f22d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f22d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2f22d-113">Heroku jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2f22d-113">A Heroku single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2f22d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f22d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2f22d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2f22d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2f22d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2f22d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2f22d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2f22d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2f22d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2f22d-118">Scenario description</span></span>
<span data-ttu-id="2f22d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f22d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2f22d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2f22d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2f22d-121">Přidání Heroku z Galerie</span><span class="sxs-lookup"><span data-stu-id="2f22d-121">Adding Heroku from the gallery</span></span>
2. <span data-ttu-id="2f22d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f22d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-heroku-from-the-gallery"></a><span data-ttu-id="2f22d-123">Přidání Heroku z Galerie</span><span class="sxs-lookup"><span data-stu-id="2f22d-123">Adding Heroku from the gallery</span></span>
<span data-ttu-id="2f22d-124">Při konfiguraci integrace Heroku do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Heroku z galerie.</span><span class="sxs-lookup"><span data-stu-id="2f22d-124">To configure the integration of Heroku into Azure AD, you need to add Heroku from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2f22d-125">**Pokud chcete přidat Heroku z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f22d-125">**To add Heroku from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2f22d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2f22d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2f22d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2f22d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2f22d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f22d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2f22d-133">Do vyhledávacího pole zadejte **Heroku**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-133">In the search box, type **Heroku**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_search.png)

5. <span data-ttu-id="2f22d-135">Na panelu výsledků vyberte **Heroku**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2f22d-135">In the results panel, select **Heroku**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2f22d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f22d-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="2f22d-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Heroku podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2f22d-138">In this section, you configure and test Azure AD single sign-on with Heroku based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2f22d-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Heroku je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f22d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Heroku is to a user in Azure AD.</span></span> <span data-ttu-id="2f22d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Heroku musí navázat.</span><span class="sxs-lookup"><span data-stu-id="2f22d-140">In other words, a link relationship between an Azure AD user and the related user in Heroku needs to be established.</span></span>

<span data-ttu-id="2f22d-141">V Heroku, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="2f22d-141">In Heroku, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2f22d-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Heroku, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="2f22d-142">To configure and test Azure AD single sign-on with Heroku, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2f22d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="2f22d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2f22d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2f22d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2f22d-145">**[Vytvoření zkušebního uživatele Heroku](#creating-a-heroku-test-user)**  – Pokud chcete mít protějšek Britta Simon v Heroku propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2f22d-145">**[Creating a Heroku test user](#creating-a-heroku-test-user)** - to have a counterpart of Britta Simon in Heroku that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2f22d-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2f22d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2f22d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2f22d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2f22d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f22d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2f22d-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Heroku.</span><span class="sxs-lookup"><span data-stu-id="2f22d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Heroku application.</span></span>

<span data-ttu-id="2f22d-150">**Ke konfiguraci Azure AD jednotné přihlašování s Heroku, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f22d-150">**To configure Azure AD single sign-on with Heroku, perform the following steps:**</span></span>

1. <span data-ttu-id="2f22d-151">Na portálu Azure na **Heroku** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-151">In the Azure portal, on the **Heroku** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2f22d-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2f22d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_samlbase.png)

3. <span data-ttu-id="2f22d-155">Na **Heroku domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f22d-155">On the **Heroku Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_url.png)

    <span data-ttu-id="2f22d-157">a.</span><span class="sxs-lookup"><span data-stu-id="2f22d-157">a.</span></span> <span data-ttu-id="2f22d-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="2f22d-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>    
    `https://sso.heroku.com/saml/<company-name>/init`

    <span data-ttu-id="2f22d-159">b.</span><span class="sxs-lookup"><span data-stu-id="2f22d-159">b.</span></span> <span data-ttu-id="2f22d-160">V **identifikátoru adresy URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="2f22d-160">In the **Identifier URL** textbox, type a URL using the following pattern:</span></span>            
    `https://sso.heroku.com/saml/<company-name>`

    > [!NOTE]
    ><span data-ttu-id="2f22d-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2f22d-161">These values are not real.</span></span> <span data-ttu-id="2f22d-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2f22d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2f22d-163">Tyto hodnoty můžete získat od Heroku týmu, který je popsán v pozdějších částech tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="2f22d-163">You get these values from Heroku team, which is described in later sections of this article.</span></span> 
        
4. <span data-ttu-id="2f22d-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2f22d-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_certificate.png) 

5. <span data-ttu-id="2f22d-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2f22d-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2f22d-168">Pokud chcete povolit jednotné přihlašování v Heroku, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f22d-168">To enable SSO in Heroku, perform the following steps:</span></span>
   
    <span data-ttu-id="2f22d-169">a.</span><span class="sxs-lookup"><span data-stu-id="2f22d-169">a.</span></span> <span data-ttu-id="2f22d-170">Přihlaste se k účtu Heroku jako správce.</span><span class="sxs-lookup"><span data-stu-id="2f22d-170">Log in to the Heroku account as an administrator.</span></span>

    <span data-ttu-id="2f22d-171">b.</span><span class="sxs-lookup"><span data-stu-id="2f22d-171">b.</span></span> <span data-ttu-id="2f22d-172">Klikněte na kartu **Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-172">Click the **Settings** tab.</span></span>

    <span data-ttu-id="2f22d-173">c.</span><span class="sxs-lookup"><span data-stu-id="2f22d-173">c.</span></span> <span data-ttu-id="2f22d-174">Na **jediné přihlášení na stránce**, klikněte na tlačítko **nahrát Metadata**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-174">On the **Single Sign On Page**, click **Upload Metadata**.</span></span>

    <span data-ttu-id="2f22d-175">d.</span><span class="sxs-lookup"><span data-stu-id="2f22d-175">d.</span></span> <span data-ttu-id="2f22d-176">Nahrajte soubor metadat, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2f22d-176">Upload the metadata file, which you have downloaded from the Azure portal.</span></span>

    <span data-ttu-id="2f22d-177">e.</span><span class="sxs-lookup"><span data-stu-id="2f22d-177">e.</span></span> <span data-ttu-id="2f22d-178">Po úspěšné instalaci správci najdete v potvrzovacím dialogovém okně a zobrazí se adresa URL jednotného přihlášení pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="2f22d-178">When the setup is successful, administrators see a confirmation dialog and the URL of the SSO Login for end users is displayed.</span></span> 

    <span data-ttu-id="2f22d-179">f.</span><span class="sxs-lookup"><span data-stu-id="2f22d-179">f.</span></span> <span data-ttu-id="2f22d-180">Kopírování **Heroku přihlašovací adresa URL** a **Heroku Entity ID** hodnoty a vraťte se zpátky a **Heroku domény a adresy URL** části na portálu Azure a vložte tyto hodnoty do  **Adresa Url přihlašování** a **identifikátor** textových polí v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2f22d-180">Copy the **Heroku Login URL** and **Heroku Entity ID** values and go back to **Heroku Domain and URLs** section in Azure portal and paste these values into the **Sign-On Url** and **Identifier** textboxes respectively.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_52.png) 
    
8. <span data-ttu-id="2f22d-182">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-182">Click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="2f22d-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="2f22d-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2f22d-184">Po přidání této aplikace z **Active Directory podnikové aplikace** jednoduše klikněte na tlačítko **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím  **Konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="2f22d-184">After adding this app from the **Active Directory Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2f22d-185">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2f22d-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2f22d-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f22d-186">Creating an Azure AD test user</span></span>

<span data-ttu-id="2f22d-187">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2f22d-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2f22d-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f22d-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2f22d-190">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2f22d-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2f22d-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2f22d-194">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f22d-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2f22d-196">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f22d-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2f22d-198">a.</span><span class="sxs-lookup"><span data-stu-id="2f22d-198">a.</span></span> <span data-ttu-id="2f22d-199">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2f22d-200">b.</span><span class="sxs-lookup"><span data-stu-id="2f22d-200">b.</span></span> <span data-ttu-id="2f22d-201">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2f22d-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2f22d-202">c.</span><span class="sxs-lookup"><span data-stu-id="2f22d-202">c.</span></span> <span data-ttu-id="2f22d-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2f22d-204">d.</span><span class="sxs-lookup"><span data-stu-id="2f22d-204">d.</span></span> <span data-ttu-id="2f22d-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-205">Click **Create**.</span></span>
 
### <a name="creating-a-heroku-test-user"></a><span data-ttu-id="2f22d-206">Vytvoření zkušebního uživatele Heroku</span><span class="sxs-lookup"><span data-stu-id="2f22d-206">Creating a Heroku test user</span></span>

<span data-ttu-id="2f22d-207">V této části vytvoříte volal Britta Simon v Heroku uživatele.</span><span class="sxs-lookup"><span data-stu-id="2f22d-207">In this section, you create a user called Britta Simon in Heroku.</span></span> <span data-ttu-id="2f22d-208">Heroku podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="2f22d-208">Heroku supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="2f22d-209">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="2f22d-209">There is no action item for you in this section.</span></span> <span data-ttu-id="2f22d-210">Nový uživatel se vytvoří při přístupu k Heroku, pokud uživatel ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2f22d-210">A new user is created when accessing Heroku if the user doesn't exist yet.</span></span> <span data-ttu-id="2f22d-211">Po zřízení účtu koncový uživatel obdrží e-mail o ověření a je třeba ji kliknutím na odkaz potvrzení.</span><span class="sxs-lookup"><span data-stu-id="2f22d-211">After the account is provisioned, the end user receives a verification email and needs to click the acknowledgement link.</span></span>

>[!NOTE]
><span data-ttu-id="2f22d-212">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory Heroku klienta](https://www.heroku.com/support).</span><span class="sxs-lookup"><span data-stu-id="2f22d-212">If you need to create a user manually, you need to contact the [Heroku Client support team](https://www.heroku.com/support).</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2f22d-213">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f22d-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2f22d-214">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Heroku.</span><span class="sxs-lookup"><span data-stu-id="2f22d-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Heroku.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2f22d-216">**Pokud chcete přiřadit Britta Simon Heroku, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f22d-216">**To assign Britta Simon to Heroku, perform the following steps:**</span></span>

1. <span data-ttu-id="2f22d-217">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2f22d-219">V seznamu aplikací vyberte **Heroku**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-219">In the applications list, select **Heroku**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_app.png) 

3. <span data-ttu-id="2f22d-221">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2f22d-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2f22d-223">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2f22d-223">Click **Add** button.</span></span> <span data-ttu-id="2f22d-224">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f22d-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2f22d-226">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2f22d-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2f22d-227">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f22d-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2f22d-228">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f22d-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2f22d-229">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f22d-229">Testing single sign-on</span></span>

<span data-ttu-id="2f22d-230">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2f22d-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2f22d-231">Když kliknete na dlaždici Heroku na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Heroku.</span><span class="sxs-lookup"><span data-stu-id="2f22d-231">When you click the Heroku tile in the Access Panel, you should get automatically signed-on to your Heroku application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f22d-232">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2f22d-232">Additional resources</span></span>

* [<span data-ttu-id="2f22d-233">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2f22d-233">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2f22d-234">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2f22d-234">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_203.png
