---
title: 'Kurz: Azure Active Directory integrace s Condeco | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Condeco."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4601c17d-ad93-4865-8885-b378c4bbe82b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: eab1d25abc2fccdeffdf2706269bfd078eb5a09f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-condeco"></a><span data-ttu-id="32b86-103">Kurz: Azure Active Directory integrace s Condeco</span><span class="sxs-lookup"><span data-stu-id="32b86-103">Tutorial: Azure Active Directory integration with Condeco</span></span>

<span data-ttu-id="32b86-104">V tomto kurzu zjistěte, jak integrovat Condeco s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="32b86-104">In this tutorial, you learn how to integrate Condeco with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="32b86-105">Integrace Condeco s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="32b86-105">Integrating Condeco with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="32b86-106">Můžete řídit ve službě Azure AD, který má přístup k Condeco</span><span class="sxs-lookup"><span data-stu-id="32b86-106">You can control in Azure AD who has access to Condeco</span></span>
- <span data-ttu-id="32b86-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Condeco (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="32b86-107">You can enable your users to automatically get signed-on to Condeco (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="32b86-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="32b86-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="32b86-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="32b86-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32b86-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="32b86-110">Prerequisites</span></span>

<span data-ttu-id="32b86-111">Konfigurace integrace Azure AD s Condeco, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="32b86-111">To configure Azure AD integration with Condeco, you need the following items:</span></span>

- <span data-ttu-id="32b86-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="32b86-112">An Azure AD subscription</span></span>
- <span data-ttu-id="32b86-113">Condeco jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="32b86-113">A Condeco single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="32b86-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="32b86-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="32b86-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="32b86-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="32b86-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="32b86-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="32b86-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="32b86-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="32b86-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="32b86-118">Scenario description</span></span>
<span data-ttu-id="32b86-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="32b86-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="32b86-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="32b86-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="32b86-121">Přidání Condeco z Galerie</span><span class="sxs-lookup"><span data-stu-id="32b86-121">Adding Condeco from the gallery</span></span>
2. <span data-ttu-id="32b86-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="32b86-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-condeco-from-the-gallery"></a><span data-ttu-id="32b86-123">Přidání Condeco z Galerie</span><span class="sxs-lookup"><span data-stu-id="32b86-123">Adding Condeco from the gallery</span></span>
<span data-ttu-id="32b86-124">Při konfiguraci integrace Condeco do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Condeco z galerie.</span><span class="sxs-lookup"><span data-stu-id="32b86-124">To configure the integration of Condeco into Azure AD, you need to add Condeco from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="32b86-125">**Pokud chcete přidat Condeco z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="32b86-125">**To add Condeco from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="32b86-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="32b86-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="32b86-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="32b86-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="32b86-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="32b86-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="32b86-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32b86-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="32b86-133">Do vyhledávacího pole zadejte **Condeco**.</span><span class="sxs-lookup"><span data-stu-id="32b86-133">In the search box, type **Condeco**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_search.png)

5. <span data-ttu-id="32b86-135">Na panelu výsledků vyberte **Condeco**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="32b86-135">In the results panel, select **Condeco**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="32b86-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="32b86-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="32b86-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Condeco podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="32b86-138">In this section, you configure and test Azure AD single sign-on with Condeco based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="32b86-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Condeco je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="32b86-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Condeco is to a user in Azure AD.</span></span> <span data-ttu-id="32b86-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Condeco musí navázat.</span><span class="sxs-lookup"><span data-stu-id="32b86-140">In other words, a link relationship between an Azure AD user and the related user in Condeco needs to be established.</span></span>

<span data-ttu-id="32b86-141">V Condeco, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="32b86-141">In Condeco, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="32b86-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Condeco, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="32b86-142">To configure and test Azure AD single sign-on with Condeco, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="32b86-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="32b86-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="32b86-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="32b86-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="32b86-145">**[Vytvoření zkušebního uživatele Condeco](#creating-a-condeco-test-user)**  – Pokud chcete mít protějšek Britta Simon v Condeco propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="32b86-145">**[Creating a Condeco test user](#creating-a-condeco-test-user)** - to have a counterpart of Britta Simon in Condeco that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="32b86-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="32b86-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="32b86-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="32b86-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="32b86-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="32b86-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="32b86-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Condeco.</span><span class="sxs-lookup"><span data-stu-id="32b86-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Condeco application.</span></span>

<span data-ttu-id="32b86-150">**Ke konfiguraci Azure AD jednotné přihlašování s Condeco, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="32b86-150">**To configure Azure AD single sign-on with Condeco, perform the following steps:**</span></span>

1. <span data-ttu-id="32b86-151">Na portálu Azure na **Condeco** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="32b86-151">In the Azure portal, on the **Condeco** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="32b86-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="32b86-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_samlbase.png)

3. <span data-ttu-id="32b86-155">Na **Condeco domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="32b86-155">On the **Condeco Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_url.png)

    <span data-ttu-id="32b86-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.condecosoftware.com`</span><span class="sxs-lookup"><span data-stu-id="32b86-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.condecosoftware.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="32b86-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="32b86-158">This value is not real.</span></span> <span data-ttu-id="32b86-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="32b86-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="32b86-160">Obraťte se na [tým podpory Condeco klienta](mailTo:supportna@condecosoftware.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="32b86-160">Contact [Condeco Client support team](mailTo:supportna@condecosoftware.com) to get this value.</span></span> 
 
4. <span data-ttu-id="32b86-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="32b86-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_certificate.png) 

5. <span data-ttu-id="32b86-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="32b86-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-condeco-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="32b86-165">Konfigurace jednotného přihlašování na **Condeco** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Condeco](mailTo:supportna@condecosoftware.com).</span><span class="sxs-lookup"><span data-stu-id="32b86-165">To configure single sign-on on **Condeco** side, you need to send the downloaded **Metadata XML** to [Condeco support team](mailTo:supportna@condecosoftware.com).</span></span> <span data-ttu-id="32b86-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="32b86-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="32b86-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="32b86-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="32b86-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="32b86-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="32b86-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="32b86-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="32b86-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="32b86-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="32b86-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="32b86-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="32b86-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="32b86-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="32b86-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="32b86-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-condeco-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="32b86-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="32b86-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-condeco-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="32b86-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32b86-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-condeco-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="32b86-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="32b86-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-condeco-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="32b86-182">a.</span><span class="sxs-lookup"><span data-stu-id="32b86-182">a.</span></span> <span data-ttu-id="32b86-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="32b86-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="32b86-184">b.</span><span class="sxs-lookup"><span data-stu-id="32b86-184">b.</span></span> <span data-ttu-id="32b86-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="32b86-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="32b86-186">c.</span><span class="sxs-lookup"><span data-stu-id="32b86-186">c.</span></span> <span data-ttu-id="32b86-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="32b86-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="32b86-188">d.</span><span class="sxs-lookup"><span data-stu-id="32b86-188">d.</span></span> <span data-ttu-id="32b86-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="32b86-189">Click **Create**.</span></span>
 
### <a name="creating-a-condeco-test-user"></a><span data-ttu-id="32b86-190">Vytvoření zkušebního uživatele Condeco</span><span class="sxs-lookup"><span data-stu-id="32b86-190">Creating a Condeco test user</span></span>

<span data-ttu-id="32b86-191">Cílem této části je vytvoření uživatele v Condeco nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="32b86-191">The objective of this section is to create a user called Britta Simon in Condeco.</span></span> <span data-ttu-id="32b86-192">Condeco podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="32b86-192">Condeco supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="32b86-193">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="32b86-193">There is no action item for you in this section.</span></span> <span data-ttu-id="32b86-194">Nový uživatel se vytvoří během pokusu o přístup k Condeco, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="32b86-194">A new user is created during an attempt to access Condeco if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="32b86-195">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory Condeco](mailTo:supportna@condecosoftware.com).</span><span class="sxs-lookup"><span data-stu-id="32b86-195">If you need to create a user manually, you need to contact the [Condeco support team](mailTo:supportna@condecosoftware.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="32b86-196">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="32b86-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="32b86-197">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Condeco.</span><span class="sxs-lookup"><span data-stu-id="32b86-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Condeco.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="32b86-199">**Pokud chcete přiřadit Britta Simon Condeco, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="32b86-199">**To assign Britta Simon to Condeco, perform the following steps:**</span></span>

1. <span data-ttu-id="32b86-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="32b86-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="32b86-202">V seznamu aplikací vyberte **Condeco**.</span><span class="sxs-lookup"><span data-stu-id="32b86-202">In the applications list, select **Condeco**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_app.png) 

3. <span data-ttu-id="32b86-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="32b86-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="32b86-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="32b86-206">Click **Add** button.</span></span> <span data-ttu-id="32b86-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32b86-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="32b86-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="32b86-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="32b86-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32b86-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="32b86-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32b86-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="32b86-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="32b86-212">Testing single sign-on</span></span>

<span data-ttu-id="32b86-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="32b86-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="32b86-214">Když kliknete na dlaždici Condeco na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Condeco.</span><span class="sxs-lookup"><span data-stu-id="32b86-214">When you click the Condeco tile in the Access Panel, you should get automatically signed-on to your Condeco application.</span></span>
<span data-ttu-id="32b86-215">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="32b86-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="32b86-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="32b86-216">Additional resources</span></span>

* [<span data-ttu-id="32b86-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="32b86-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="32b86-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="32b86-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_203.png

