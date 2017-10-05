---
title: 'Kurz: Azure Active Directory integrace s Kontiki | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Kontiki."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8d5e5413-da4c-40d8-b1d0-f03ecfef030b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 8c441656a1a52d1e1e75b7d0f7025f4331bf9dc1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kontiki"></a><span data-ttu-id="bc13d-103">Kurz: Azure Active Directory integrace s Kontiki</span><span class="sxs-lookup"><span data-stu-id="bc13d-103">Tutorial: Azure Active Directory integration with Kontiki</span></span>

<span data-ttu-id="bc13d-104">V tomto kurzu zjistěte, jak integrovat Kontiki s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bc13d-104">In this tutorial, you learn how to integrate Kontiki with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bc13d-105">Integrace Kontiki s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bc13d-105">Integrating Kontiki with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bc13d-106">Můžete řídit ve službě Azure AD, který má přístup k Kontiki</span><span class="sxs-lookup"><span data-stu-id="bc13d-106">You can control in Azure AD who has access to Kontiki</span></span>
- <span data-ttu-id="bc13d-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Kontiki (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc13d-107">You can enable your users to automatically get signed-on to Kontiki (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bc13d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bc13d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bc13d-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bc13d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc13d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bc13d-110">Prerequisites</span></span>

<span data-ttu-id="bc13d-111">Konfigurace integrace Azure AD s Kontiki, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="bc13d-111">To configure Azure AD integration with Kontiki, you need the following items:</span></span>

- <span data-ttu-id="bc13d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc13d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bc13d-113">Kontiki jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="bc13d-113">A Kontiki single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bc13d-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc13d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bc13d-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="bc13d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bc13d-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="bc13d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bc13d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bc13d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bc13d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="bc13d-118">Scenario description</span></span>
<span data-ttu-id="bc13d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bc13d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bc13d-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="bc13d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bc13d-121">Přidání Kontiki z Galerie</span><span class="sxs-lookup"><span data-stu-id="bc13d-121">Adding Kontiki from the gallery</span></span>
2. <span data-ttu-id="bc13d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc13d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kontiki-from-the-gallery"></a><span data-ttu-id="bc13d-123">Přidání Kontiki z Galerie</span><span class="sxs-lookup"><span data-stu-id="bc13d-123">Adding Kontiki from the gallery</span></span>
<span data-ttu-id="bc13d-124">Při konfiguraci integrace Kontiki do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Kontiki z galerie.</span><span class="sxs-lookup"><span data-stu-id="bc13d-124">To configure the integration of Kontiki into Azure AD, you need to add Kontiki from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bc13d-125">**Pokud chcete přidat Kontiki z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bc13d-125">**To add Kontiki from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bc13d-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bc13d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bc13d-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bc13d-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="bc13d-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc13d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="bc13d-133">Do vyhledávacího pole zadejte **Kontiki**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-133">In the search box, type **Kontiki**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kontiki-tutorial/tutorial_kontiki_search.png)

5. <span data-ttu-id="bc13d-135">Na panelu výsledků vyberte **Kontiki**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bc13d-135">In the results panel, select **Kontiki**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kontiki-tutorial/tutorial_kontiki_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bc13d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc13d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bc13d-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kontiki podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="bc13d-138">In this section, you configure and test Azure AD single sign-on with Kontiki based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bc13d-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Kontiki je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc13d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kontiki is to a user in Azure AD.</span></span> <span data-ttu-id="bc13d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Kontiki musí navázat.</span><span class="sxs-lookup"><span data-stu-id="bc13d-140">In other words, a link relationship between an Azure AD user and the related user in Kontiki needs to be established.</span></span>

<span data-ttu-id="bc13d-141">V Kontiki, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="bc13d-141">In Kontiki, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bc13d-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kontiki, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="bc13d-142">To configure and test Azure AD single sign-on with Kontiki, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bc13d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="bc13d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bc13d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bc13d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bc13d-145">**[Vytvoření zkušebního uživatele Kontiki](#creating-a-kontiki-test-user)**  – Pokud chcete mít protějšek Britta Simon v Kontiki propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="bc13d-145">**[Creating a Kontiki test user](#creating-a-kontiki-test-user)** - to have a counterpart of Britta Simon in Kontiki that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bc13d-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bc13d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bc13d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="bc13d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bc13d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc13d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bc13d-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Kontiki.</span><span class="sxs-lookup"><span data-stu-id="bc13d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kontiki application.</span></span>

<span data-ttu-id="bc13d-150">**Ke konfiguraci Azure AD jednotné přihlašování s Kontiki, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bc13d-150">**To configure Azure AD single sign-on with Kontiki, perform the following steps:**</span></span>

1. <span data-ttu-id="bc13d-151">Na portálu Azure na **Kontiki** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-151">In the Azure portal, on the **Kontiki** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="bc13d-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bc13d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kontiki-tutorial/tutorial_kontiki_samlbase.png)

3. <span data-ttu-id="bc13d-155">Na **Kontiki domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bc13d-155">On the **Kontiki Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kontiki-tutorial/tutorial_kontiki_url.png)

     <span data-ttu-id="bc13d-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.mc.eval.kontiki.com`</span><span class="sxs-lookup"><span data-stu-id="bc13d-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.mc.eval.kontiki.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bc13d-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="bc13d-158">This value is not real.</span></span> <span data-ttu-id="bc13d-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bc13d-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="bc13d-160">Obraťte se na [tým podpory Kontiki klienta](http://customersupport.kontiki.com/enterprise/contactsupport.html) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bc13d-160">Contact [Kontiki Client support team](http://customersupport.kontiki.com/enterprise/contactsupport.html) to get the value.</span></span> 
 
4. <span data-ttu-id="bc13d-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="bc13d-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kontiki-tutorial/tutorial_kontiki_certificate.png) 

5. <span data-ttu-id="bc13d-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bc13d-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kontiki-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="bc13d-165">Konfigurace jednotného přihlašování na **Kontiki** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Kontiki](http://customersupport.kontiki.com/enterprise/contactsupport.html).</span><span class="sxs-lookup"><span data-stu-id="bc13d-165">To configure single sign-on on **Kontiki** side, you need to send the downloaded **Metadata XML** to [Kontiki support team](http://customersupport.kontiki.com/enterprise/contactsupport.html).</span></span> <span data-ttu-id="bc13d-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="bc13d-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="bc13d-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="bc13d-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bc13d-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="bc13d-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bc13d-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bc13d-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bc13d-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc13d-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="bc13d-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bc13d-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="bc13d-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bc13d-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bc13d-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bc13d-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kontiki-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bc13d-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kontiki-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bc13d-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc13d-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kontiki-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bc13d-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bc13d-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kontiki-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bc13d-182">a.</span><span class="sxs-lookup"><span data-stu-id="bc13d-182">a.</span></span> <span data-ttu-id="bc13d-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bc13d-184">b.</span><span class="sxs-lookup"><span data-stu-id="bc13d-184">b.</span></span> <span data-ttu-id="bc13d-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bc13d-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bc13d-186">c.</span><span class="sxs-lookup"><span data-stu-id="bc13d-186">c.</span></span> <span data-ttu-id="bc13d-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bc13d-188">d.</span><span class="sxs-lookup"><span data-stu-id="bc13d-188">d.</span></span> <span data-ttu-id="bc13d-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-189">Click **Create**.</span></span>
 
### <a name="creating-a-kontiki-test-user"></a><span data-ttu-id="bc13d-190">Vytvoření zkušebního uživatele Kontiki</span><span class="sxs-lookup"><span data-stu-id="bc13d-190">Creating a Kontiki test user</span></span>

<span data-ttu-id="bc13d-191">Neexistuje žádná položka akce můžete nakonfigurovat na Kontiki zřizování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bc13d-191">There is no action item for you to configure user provisioning to Kontiki.</span></span> <span data-ttu-id="bc13d-192">Když přiřazený uživatel se pokusí přihlásit k Kontiki pomocí přístupového panelu, Kontiki ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="bc13d-192">When an assigned user tries to log in to Kontiki using the access panel, Kontiki checks whether the user exists.</span></span>  

>[!NOTE]
><span data-ttu-id="bc13d-193">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Kontiki.</span><span class="sxs-lookup"><span data-stu-id="bc13d-193">If there is no user account available yet, it is automatically created by Kontiki.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bc13d-194">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc13d-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bc13d-195">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Kontiki.</span><span class="sxs-lookup"><span data-stu-id="bc13d-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kontiki.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="bc13d-197">**Pokud chcete přiřadit Britta Simon Kontiki, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="bc13d-197">**To assign Britta Simon to Kontiki, perform the following steps:**</span></span>

1. <span data-ttu-id="bc13d-198">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="bc13d-200">V seznamu aplikací vyberte **Kontiki**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-200">In the applications list, select **Kontiki**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kontiki-tutorial/tutorial_kontiki_app.png) 

3. <span data-ttu-id="bc13d-202">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="bc13d-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="bc13d-204">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bc13d-204">Click **Add** button.</span></span> <span data-ttu-id="bc13d-205">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc13d-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="bc13d-207">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="bc13d-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bc13d-208">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc13d-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bc13d-209">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bc13d-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bc13d-210">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bc13d-210">Testing single sign-on</span></span>

<span data-ttu-id="bc13d-211">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bc13d-211">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bc13d-212">Když kliknete na dlaždici Kontiki na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Kontiki.</span><span class="sxs-lookup"><span data-stu-id="bc13d-212">When you click the Kontiki tile in the Access Panel, you should get automatically signed-on to your Kontiki application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc13d-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bc13d-213">Additional resources</span></span>

* [<span data-ttu-id="bc13d-214">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bc13d-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bc13d-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bc13d-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kontiki-tutorial/tutorial_general_203.png

