---
title: "Kurz: Azure Active Directory integrace s přímým | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Direct."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c2cd1f0-d14c-42f0-94a8-9b800008b285
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 84582492592613320bd3ec2bdffe08519852d7c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-direct"></a><span data-ttu-id="9e956-103">Kurz: Azure Active Directory integrace s protokolem Direct</span><span class="sxs-lookup"><span data-stu-id="9e956-103">Tutorial: Azure Active Directory integration with Direct</span></span>

<span data-ttu-id="9e956-104">V tomto kurzu zjistěte, jak integrovat přímo s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9e956-104">In this tutorial, you learn how to integrate Direct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9e956-105">Integrace přímo s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9e956-105">Integrating Direct with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9e956-106">Můžete řídit ve službě Azure AD, který má přístup k přímé</span><span class="sxs-lookup"><span data-stu-id="9e956-106">You can control in Azure AD who has access to Direct</span></span>
- <span data-ttu-id="9e956-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k přímé (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e956-107">You can enable your users to automatically get signed-on to Direct (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9e956-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9e956-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9e956-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9e956-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e956-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9e956-110">Prerequisites</span></span>

<span data-ttu-id="9e956-111">Ke konfiguraci integrace služby Azure AD s přímým, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9e956-111">To configure Azure AD integration with Direct, you need the following items:</span></span>

- <span data-ttu-id="9e956-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e956-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9e956-113">Přímé jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9e956-113">A Direct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9e956-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e956-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9e956-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9e956-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9e956-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9e956-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9e956-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9e956-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9e956-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9e956-118">Scenario description</span></span>
<span data-ttu-id="9e956-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9e956-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9e956-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9e956-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9e956-121">Přidání přímo z Galerie</span><span class="sxs-lookup"><span data-stu-id="9e956-121">Adding Direct from the gallery</span></span>
2. <span data-ttu-id="9e956-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e956-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-direct-from-the-gallery"></a><span data-ttu-id="9e956-123">Přidání přímo z Galerie</span><span class="sxs-lookup"><span data-stu-id="9e956-123">Adding Direct from the gallery</span></span>
<span data-ttu-id="9e956-124">Při konfiguraci integrace Direct do služby Azure AD, musíte přidat přímo z Galerie do seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9e956-124">To configure the integration of Direct into Azure AD, you need to add Direct from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9e956-125">**Pokud chcete přidat přímo z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e956-125">**To add Direct from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9e956-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9e956-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9e956-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9e956-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9e956-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e956-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9e956-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e956-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9e956-133">Do vyhledávacího pole zadejte **přímé**.</span><span class="sxs-lookup"><span data-stu-id="9e956-133">In the search box, type **Direct**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_search.png)

5. <span data-ttu-id="9e956-135">Na panelu výsledků vyberte **přímé**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e956-135">In the results panel, select **Direct**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9e956-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e956-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9e956-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Direct podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9e956-138">In this section, you configure and test Azure AD single sign-on with Direct based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9e956-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Direct je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9e956-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Direct is to a user in Azure AD.</span></span> <span data-ttu-id="9e956-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v přímém musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9e956-140">In other words, a link relationship between an Azure AD user and the related user in Direct needs to be established.</span></span>

<span data-ttu-id="9e956-141">V přímo, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="9e956-141">In Direct, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9e956-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s přímým, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9e956-142">To configure and test Azure AD single sign-on with Direct, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9e956-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9e956-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9e956-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e956-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9e956-145">**[Vytváření uživatele – přímý test](#creating-a-direct-test-user)**  – Pokud chcete mít protějšek Britta Simon v přímo, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9e956-145">**[Creating a Direct test user](#creating-a-direct-test-user)** - to have a counterpart of Britta Simon in Direct that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9e956-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e956-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9e956-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9e956-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9e956-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e956-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9e956-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Direct.</span><span class="sxs-lookup"><span data-stu-id="9e956-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Direct application.</span></span>

<span data-ttu-id="9e956-150">**Ke konfiguraci Azure AD jednotné přihlašování s přímým, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e956-150">**To configure Azure AD single sign-on with Direct, perform the following steps:**</span></span>

1. <span data-ttu-id="9e956-151">Na portálu Azure na **přímé** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9e956-151">In the Azure portal, on the **Direct** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9e956-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e956-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_samlbase.png)

3. <span data-ttu-id="9e956-155">Na **přímé domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="9e956-155">On the **Direct Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_url.png)

    <span data-ttu-id="9e956-157">V **identifikátor** textovému poli, zadejte adresu URL:`https://direct4b.com/`</span><span class="sxs-lookup"><span data-stu-id="9e956-157">In the **Identifier** textbox, type the URL: `https://direct4b.com/`</span></span>

4. <span data-ttu-id="9e956-158">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="9e956-158">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_url1.png)

     <span data-ttu-id="9e956-160">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://direct4b.com/sso`</span><span class="sxs-lookup"><span data-stu-id="9e956-160">In the **Sign-on URL** textbox, type the URL: `https://direct4b.com/sso`</span></span> 
    
5. <span data-ttu-id="9e956-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9e956-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_certificate.png) 

6. <span data-ttu-id="9e956-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9e956-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="9e956-165">Konfigurace jednotného přihlašování na **přímé** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým přímé podpory](https://direct4b.com/ja/support.html#inquiry).</span><span class="sxs-lookup"><span data-stu-id="9e956-165">To configure single sign-on on **Direct** side, you need to send the downloaded **Metadata XML** to [Direct support team](https://direct4b.com/ja/support.html#inquiry).</span></span> 

> [!TIP]
> <span data-ttu-id="9e956-166">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9e956-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9e956-167">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9e956-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9e956-168">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9e956-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9e956-169">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e956-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="9e956-170">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e956-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9e956-172">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e956-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9e956-173">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9e956-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9e956-175">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9e956-175">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9e956-177">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e956-177">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9e956-179">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9e956-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9e956-181">a.</span><span class="sxs-lookup"><span data-stu-id="9e956-181">a.</span></span> <span data-ttu-id="9e956-182">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9e956-182">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9e956-183">b.</span><span class="sxs-lookup"><span data-stu-id="9e956-183">b.</span></span> <span data-ttu-id="9e956-184">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9e956-184">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9e956-185">c.</span><span class="sxs-lookup"><span data-stu-id="9e956-185">c.</span></span> <span data-ttu-id="9e956-186">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9e956-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9e956-187">d.</span><span class="sxs-lookup"><span data-stu-id="9e956-187">d.</span></span> <span data-ttu-id="9e956-188">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9e956-188">Click **Create**.</span></span>
 
### <a name="creating-a-direct-test-user"></a><span data-ttu-id="9e956-189">Vytváření uživatele – přímý test</span><span class="sxs-lookup"><span data-stu-id="9e956-189">Creating a Direct test user</span></span>

<span data-ttu-id="9e956-190">V této části vytvoříte uživatele volal Britta Simon v přímo.</span><span class="sxs-lookup"><span data-stu-id="9e956-190">In this section, you create a user called Britta Simon in Direct.</span></span> <span data-ttu-id="9e956-191">Práce s [tým přímé podpory](https://direct4b.com/ja/support.html#inquiry) přidat uživatele do přímou platformu.</span><span class="sxs-lookup"><span data-stu-id="9e956-191">Work with [Direct support team](https://direct4b.com/ja/support.html#inquiry) to add the users in the Direct platform.</span></span> <span data-ttu-id="9e956-192">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9e956-192">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9e956-193">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e956-193">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9e956-194">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k Direct.</span><span class="sxs-lookup"><span data-stu-id="9e956-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Direct.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9e956-196">**Britta Simon přiřadit přímo, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9e956-196">**To assign Britta Simon to Direct, perform the following steps:**</span></span>

1. <span data-ttu-id="9e956-197">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9e956-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9e956-199">V seznamu aplikací vyberte **přímé**.</span><span class="sxs-lookup"><span data-stu-id="9e956-199">In the applications list, select **Direct**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-direct-tutorial/tutorial_direct_app.png) 

3. <span data-ttu-id="9e956-201">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9e956-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9e956-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9e956-203">Click **Add** button.</span></span> <span data-ttu-id="9e956-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e956-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9e956-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9e956-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9e956-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e956-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9e956-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9e956-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9e956-209">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9e956-209">Testing single sign-on</span></span>

<span data-ttu-id="9e956-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9e956-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

1. <span data-ttu-id="9e956-211">Pokud chcete otestovat v **IDP iniciované režimu**:</span><span class="sxs-lookup"><span data-stu-id="9e956-211">If you wish to test in **IDP Initiated Mode**:</span></span>

    <span data-ttu-id="9e956-212">Když kliknete **přímé** dlaždici na přístupovém panelu, měli byste obdržet automaticky přihlášení k vaší **přímé** aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e956-212">When you click the **Direct** tile in the Access Panel, you should get automatically signed-on to your **Direct** application.</span></span>

2. <span data-ttu-id="9e956-213">Pokud chcete otestovat v **SP iniciované režimu**:</span><span class="sxs-lookup"><span data-stu-id="9e956-213">If you wish to test in **SP Initiated Mode**:</span></span>
    
    <span data-ttu-id="9e956-214">a.</span><span class="sxs-lookup"><span data-stu-id="9e956-214">a.</span></span> <span data-ttu-id="9e956-215">Klikněte na **přímé** dlaždici na přístupovém panelu a budete přesměrováni na stránku pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9e956-215">Click on the **Direct** tile in the Access Panel and you will be redirected to the application sign-on page.</span></span>

    <span data-ttu-id="9e956-216">b.</span><span class="sxs-lookup"><span data-stu-id="9e956-216">b.</span></span> <span data-ttu-id="9e956-217">Zadejte vaše `subdomain` zobrazí textové pole a stiskněte měli získat '次へ (Další) a budete automaticky přihlášení k vaší **přímé** aplikace.</span><span class="sxs-lookup"><span data-stu-id="9e956-217">Input your `subdomain` in the textbox displayed and press '次へ (Next)' and you should get automatically signed-on to your **Direct** application .</span></span>
    
<span data-ttu-id="9e956-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9e956-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e956-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9e956-219">Additional resources</span></span>

* [<span data-ttu-id="9e956-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9e956-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9e956-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9e956-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-direct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-direct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-direct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-direct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-direct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-direct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-direct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-direct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-direct-tutorial/tutorial_general_203.png

