---
title: 'Kurz: Azure Active Directory integrace s FM:Systems | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a FM:Systems."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f78c58c5-6e98-458b-8991-78624a245665
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 3a597d228f6c9234ec2fd2644ec3ac50b98f3b6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fmsystems"></a><span data-ttu-id="803af-103">Kurz: Azure Active Directory integrace s FM:Systems</span><span class="sxs-lookup"><span data-stu-id="803af-103">Tutorial: Azure Active Directory integration with FM:Systems</span></span>

<span data-ttu-id="803af-104">V tomto kurzu zjistěte, jak integrovat FM:Systems s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="803af-104">In this tutorial, you learn how to integrate FM:Systems with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="803af-105">Integrace FM:Systems s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="803af-105">Integrating FM:Systems with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="803af-106">Můžete řídit ve službě Azure AD, který má přístup k FM:Systems</span><span class="sxs-lookup"><span data-stu-id="803af-106">You can control in Azure AD who has access to FM:Systems</span></span>
- <span data-ttu-id="803af-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k FM:Systems (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="803af-107">You can enable your users to automatically get signed-on to FM:Systems (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="803af-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="803af-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="803af-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="803af-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="803af-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="803af-110">Prerequisites</span></span>

<span data-ttu-id="803af-111">Konfigurace integrace Azure AD s FM:Systems, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="803af-111">To configure Azure AD integration with FM:Systems, you need the following items:</span></span>

- <span data-ttu-id="803af-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="803af-112">An Azure AD subscription</span></span>
- <span data-ttu-id="803af-113">FM:Systems jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="803af-113">An FM:Systems single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="803af-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="803af-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="803af-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="803af-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="803af-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="803af-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="803af-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="803af-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="803af-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="803af-118">Scenario description</span></span>
<span data-ttu-id="803af-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="803af-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="803af-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="803af-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="803af-121">Přidání FM:Systems z Galerie</span><span class="sxs-lookup"><span data-stu-id="803af-121">Adding FM:Systems from the gallery</span></span>
2. <span data-ttu-id="803af-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="803af-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-fmsystems-from-the-gallery"></a><span data-ttu-id="803af-123">Přidání FM:Systems z Galerie</span><span class="sxs-lookup"><span data-stu-id="803af-123">Adding FM:Systems from the gallery</span></span>
<span data-ttu-id="803af-124">Při konfiguraci integrace FM:Systems do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS FM:Systems z galerie.</span><span class="sxs-lookup"><span data-stu-id="803af-124">To configure the integration of FM:Systems into Azure AD, you need to add FM:Systems from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="803af-125">**Pokud chcete přidat FM:Systems z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="803af-125">**To add FM:Systems from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="803af-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="803af-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="803af-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="803af-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="803af-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="803af-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="803af-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="803af-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="803af-133">Do vyhledávacího pole zadejte **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="803af-133">In the search box, type **FM:Systems**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_search.png)

5. <span data-ttu-id="803af-135">Na panelu výsledků vyberte **FM:Systems**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="803af-135">In the results panel, select **FM:Systems**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="803af-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="803af-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="803af-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FM:Systems podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="803af-138">In this section, you configure and test Azure AD single sign-on with FM:Systems based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="803af-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v FM:Systems je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="803af-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FM:Systems is to a user in Azure AD.</span></span> <span data-ttu-id="803af-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v FM:Systems musí navázat.</span><span class="sxs-lookup"><span data-stu-id="803af-140">In other words, a link relationship between an Azure AD user and the related user in FM:Systems needs to be established.</span></span>

<span data-ttu-id="803af-141">V FM:Systems, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="803af-141">In FM:Systems, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="803af-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s FM:Systems, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="803af-142">To configure and test Azure AD single sign-on with FM:Systems, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="803af-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="803af-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="803af-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="803af-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="803af-145">**[Vytváření testovacího uživatele FM:Systems](#creating-an-fmsystems-test-user)**  – Pokud chcete mít protějšek Britta Simon v FM:Systems propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="803af-145">**[Creating an FM:Systems test user](#creating-an-fmsystems-test-user)** - to have a counterpart of Britta Simon in FM:Systems that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="803af-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="803af-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="803af-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="803af-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="803af-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="803af-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="803af-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="803af-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your FM:Systems application.</span></span>

<span data-ttu-id="803af-150">**Ke konfiguraci Azure AD jednotné přihlašování s FM:Systems, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="803af-150">**To configure Azure AD single sign-on with FM:Systems, perform the following steps:**</span></span>

1. <span data-ttu-id="803af-151">Na portálu Azure na **FM:Systems** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="803af-151">In the Azure portal, on the **FM:Systems** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="803af-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="803af-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_samlbase.png)

3. <span data-ttu-id="803af-155">Na **FM:Systems domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="803af-155">On the **FM:Systems Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_url.png)

    <span data-ttu-id="803af-157">V **adresa URL odpovědi** textovému poli, zadejte vaše FM:Systems **adresa URL odpovědi**, zadejte adresu URL, pomocí následujícího vzorce:`https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span><span class="sxs-lookup"><span data-stu-id="803af-157">In the **Reply URL** textbox, type your FM:Systems **Reply URL**, type the URL using the following pattern: `https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="803af-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="803af-158">This value is not real.</span></span> <span data-ttu-id="803af-159">Aktualizujte tuto hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="803af-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="803af-160">Obraťte se na [tým podpory FM:Systems](https://fmsystems.com/ask-us/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="803af-160">Contact [FM:Systems support team](https://fmsystems.com/ask-us/) to get this value.</span></span>
 
4. <span data-ttu-id="803af-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="803af-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_certificate.png) 

5. <span data-ttu-id="803af-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="803af-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="803af-165">Konfigurace jednotného přihlašování na **FM:Systems** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory FM:Systems](https://fmsystems.com/ask-us/).</span><span class="sxs-lookup"><span data-stu-id="803af-165">To configure single sign-on on **FM:Systems** side, you need to send the downloaded **Metadata XML** to [FM:Systems support team](https://fmsystems.com/ask-us/).</span></span> <span data-ttu-id="803af-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="803af-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span> <span data-ttu-id="803af-167">Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="803af-167">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="803af-168">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="803af-168">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="803af-169">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="803af-169">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="803af-170">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="803af-170">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="803af-171">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="803af-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="803af-172">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="803af-172">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="803af-174">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="803af-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="803af-175">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="803af-175">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="803af-177">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="803af-177">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="803af-179">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="803af-179">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="803af-181">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="803af-181">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="803af-183">a.</span><span class="sxs-lookup"><span data-stu-id="803af-183">a.</span></span> <span data-ttu-id="803af-184">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="803af-184">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="803af-185">b.</span><span class="sxs-lookup"><span data-stu-id="803af-185">b.</span></span> <span data-ttu-id="803af-186">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="803af-186">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="803af-187">c.</span><span class="sxs-lookup"><span data-stu-id="803af-187">c.</span></span> <span data-ttu-id="803af-188">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="803af-188">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="803af-189">d.</span><span class="sxs-lookup"><span data-stu-id="803af-189">d.</span></span> <span data-ttu-id="803af-190">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="803af-190">Click **Create**.</span></span>
 
### <a name="creating-an-fmsystems-test-user"></a><span data-ttu-id="803af-191">Vytváření testovacího uživatele FM:Systems</span><span class="sxs-lookup"><span data-stu-id="803af-191">Creating an FM:Systems test user</span></span>

1. <span data-ttu-id="803af-192">V okně webového prohlížeče Přihlaste se jako správce k serveru vaší společnosti FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="803af-192">In a web browser window, log into your FM:Systems company site as an administrator.</span></span>

2. <span data-ttu-id="803af-193">Přejděte na **systému správy \> spravovat zabezpečení \> uživatelé \> seznam uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="803af-193">Go to **System Administration \> Manage Security \> Users \> User list**.</span></span>
   
    <span data-ttu-id="803af-194">![Správa systému](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "správu systému")</span><span class="sxs-lookup"><span data-stu-id="803af-194">![System Administration](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "System Administration")</span></span>

3. <span data-ttu-id="803af-195">Klikněte na tlačítko **vytvořit nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="803af-195">Click **Create new user**.</span></span>
   
    <span data-ttu-id="803af-196">![Vytvoření nového uživatele](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "vytvořit nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="803af-196">![Create New User](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "Create New User")</span></span>

4. <span data-ttu-id="803af-197">V **vytvořit uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="803af-197">In the **Create User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="803af-198">![Vytvoření uživatele](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="803af-198">![Create User](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "Create User")</span></span>
   
    <span data-ttu-id="803af-199">a.</span><span class="sxs-lookup"><span data-stu-id="803af-199">a.</span></span> <span data-ttu-id="803af-200">Typ **uživatelské jméno**, **heslo**, **potvrzení hesla**, **e-mailu** a **ID zaměstnance** z platný účet služby Azure Active Directory, který má být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="803af-200">Type the **UserName**, the **Password**, **Confirm Password**, **E-mail** and the **Employee ID** of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="803af-201">b.</span><span class="sxs-lookup"><span data-stu-id="803af-201">b.</span></span> <span data-ttu-id="803af-202">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="803af-202">Click **Next**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="803af-203">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="803af-203">Assigning the Azure AD test user</span></span>

<span data-ttu-id="803af-204">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="803af-204">In this section, you enable Britta Simon to use Azure single sign-on by granting access to FM:Systems.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="803af-206">**Pokud chcete přiřadit Britta Simon FM:Systems, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="803af-206">**To assign Britta Simon to FM:Systems, perform the following steps:**</span></span>

1. <span data-ttu-id="803af-207">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="803af-207">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="803af-209">V seznamu aplikací vyberte **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="803af-209">In the applications list, select **FM:Systems**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_app.png) 

3. <span data-ttu-id="803af-211">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="803af-211">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="803af-213">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="803af-213">Click **Add** button.</span></span> <span data-ttu-id="803af-214">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="803af-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="803af-216">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="803af-216">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="803af-217">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="803af-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="803af-218">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="803af-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="803af-219">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="803af-219">Testing single sign-on</span></span>

<span data-ttu-id="803af-220">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="803af-220">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="803af-221">Když kliknete na dlaždici FM:Systems na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="803af-221">When you click the FM:Systems tile in the Access Panel, you should get automatically signed-on to your FM:Systems application.</span></span>
<span data-ttu-id="803af-222">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="803af-222">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="803af-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="803af-223">Additional resources</span></span>

* [<span data-ttu-id="803af-224">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="803af-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="803af-225">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="803af-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_203.png

