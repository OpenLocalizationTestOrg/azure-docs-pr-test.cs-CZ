---
title: 'Kurz: Azure Active Directory integrace s 15Five | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a 15Five."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: ea36774747a0fcfa4ace1aefb2d46dba815d92c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a><span data-ttu-id="48184-103">Kurz: Azure Active Directory integrace s 15Five</span><span class="sxs-lookup"><span data-stu-id="48184-103">Tutorial: Azure Active Directory integration with 15Five</span></span>

<span data-ttu-id="48184-104">V tomto kurzu zjistěte, jak integrovat 15Five s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="48184-104">In this tutorial, you learn how to integrate 15Five with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="48184-105">Integrace 15Five s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="48184-105">Integrating 15Five with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="48184-106">Můžete řídit ve službě Azure AD, který má přístup k 15Five</span><span class="sxs-lookup"><span data-stu-id="48184-106">You can control in Azure AD who has access to 15Five</span></span>
- <span data-ttu-id="48184-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k 15Five (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="48184-107">You can enable your users to automatically get signed-on to 15Five (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="48184-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="48184-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="48184-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="48184-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48184-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="48184-110">Prerequisites</span></span>

<span data-ttu-id="48184-111">Konfigurace integrace Azure AD s 15Five, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="48184-111">To configure Azure AD integration with 15Five, you need the following items:</span></span>

- <span data-ttu-id="48184-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="48184-112">An Azure AD subscription</span></span>
- <span data-ttu-id="48184-113">15Five jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="48184-113">A 15Five single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="48184-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="48184-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="48184-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="48184-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="48184-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="48184-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="48184-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48184-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="48184-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="48184-118">Scenario description</span></span>
<span data-ttu-id="48184-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="48184-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="48184-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="48184-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="48184-121">Přidání 15Five z Galerie</span><span class="sxs-lookup"><span data-stu-id="48184-121">Adding 15Five from the gallery</span></span>
2. <span data-ttu-id="48184-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="48184-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-15five-from-the-gallery"></a><span data-ttu-id="48184-123">Přidání 15Five z Galerie</span><span class="sxs-lookup"><span data-stu-id="48184-123">Adding 15Five from the gallery</span></span>
<span data-ttu-id="48184-124">Při konfiguraci integrace 15Five do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS 15Five z galerie.</span><span class="sxs-lookup"><span data-stu-id="48184-124">To configure the integration of 15Five into Azure AD, you need to add 15Five from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="48184-125">**Pokud chcete přidat 15Five z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="48184-125">**To add 15Five from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="48184-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="48184-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="48184-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="48184-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="48184-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="48184-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="48184-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48184-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="48184-133">Do vyhledávacího pole zadejte **15Five**.</span><span class="sxs-lookup"><span data-stu-id="48184-133">In the search box, type **15Five**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/tutorial_15five_search.png)

5. <span data-ttu-id="48184-135">Na panelu výsledků vyberte **15Five**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="48184-135">In the results panel, select **15Five**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/tutorial_15five_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="48184-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="48184-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="48184-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 15Five podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="48184-138">In this section, you configure and test Azure AD single sign-on with 15Five based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="48184-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v 15Five je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="48184-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 15Five is to a user in Azure AD.</span></span> <span data-ttu-id="48184-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v 15Five musí navázat.</span><span class="sxs-lookup"><span data-stu-id="48184-140">In other words, a link relationship between an Azure AD user and the related user in 15Five needs to be established.</span></span>

<span data-ttu-id="48184-141">V 15Five, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="48184-141">In 15Five, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="48184-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s 15Five, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="48184-142">To configure and test Azure AD single sign-on with 15Five, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="48184-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="48184-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="48184-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48184-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="48184-145">**[Vytvoření zkušebního uživatele 15Five](#creating-a-15five-test-user)**  – Pokud chcete mít protějšek Britta Simon v 15Five propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="48184-145">**[Creating a 15Five test user](#creating-a-15five-test-user)** - to have a counterpart of Britta Simon in 15Five that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="48184-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="48184-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="48184-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="48184-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="48184-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="48184-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="48184-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci 15Five.</span><span class="sxs-lookup"><span data-stu-id="48184-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 15Five application.</span></span>

<span data-ttu-id="48184-150">**Ke konfiguraci Azure AD jednotné přihlašování s 15Five, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="48184-150">**To configure Azure AD single sign-on with 15Five, perform the following steps:**</span></span>

1. <span data-ttu-id="48184-151">Na portálu Azure na **15Five** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="48184-151">In the Azure portal, on the **15Five** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="48184-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="48184-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_15five_samlbase.png)

3. <span data-ttu-id="48184-155">Na **15Five domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="48184-155">On the **15Five Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_15five_url.png)

    <span data-ttu-id="48184-157">a.</span><span class="sxs-lookup"><span data-stu-id="48184-157">a.</span></span> <span data-ttu-id="48184-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.15five.com`</span><span class="sxs-lookup"><span data-stu-id="48184-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.15five.com`</span></span>

    <span data-ttu-id="48184-159">b.</span><span class="sxs-lookup"><span data-stu-id="48184-159">b.</span></span> <span data-ttu-id="48184-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.15five.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="48184-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.15five.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="48184-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="48184-161">These values are not real.</span></span> <span data-ttu-id="48184-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="48184-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="48184-163">Obraťte se na [tým podpory klienta 15Five](https://www.15five.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="48184-163">Contact [15Five Client support team](https://www.15five.com/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="48184-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="48184-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_15five_certificate.png) 

5. <span data-ttu-id="48184-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="48184-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="48184-168">Konfigurace jednotného přihlašování na **15Five** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory 15Five](https://www.15five.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="48184-168">To configure single sign-on on **15Five** side, you need to send the downloaded **Metadata XML** to [15Five support team](https://www.15five.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="48184-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="48184-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="48184-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="48184-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="48184-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="48184-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="48184-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="48184-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="48184-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48184-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="48184-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="48184-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="48184-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="48184-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="48184-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="48184-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="48184-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48184-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="48184-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="48184-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="48184-184">a.</span><span class="sxs-lookup"><span data-stu-id="48184-184">a.</span></span> <span data-ttu-id="48184-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="48184-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="48184-186">b.</span><span class="sxs-lookup"><span data-stu-id="48184-186">b.</span></span> <span data-ttu-id="48184-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="48184-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="48184-188">c.</span><span class="sxs-lookup"><span data-stu-id="48184-188">c.</span></span> <span data-ttu-id="48184-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="48184-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="48184-190">d.</span><span class="sxs-lookup"><span data-stu-id="48184-190">d.</span></span> <span data-ttu-id="48184-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="48184-191">Click **Create**.</span></span>
 
### <a name="creating-a-15five-test-user"></a><span data-ttu-id="48184-192">Vytvoření zkušebního uživatele 15Five</span><span class="sxs-lookup"><span data-stu-id="48184-192">Creating a 15Five test user</span></span>

<span data-ttu-id="48184-193">Pokud chcete povolit uživatelům Azure AD přihlášení k 15Five, musí být zřízená do 15Five.</span><span class="sxs-lookup"><span data-stu-id="48184-193">To enable Azure AD users to log in to 15Five, they must be provisioned into 15Five.</span></span> <span data-ttu-id="48184-194">Při zřizování 15Five, je ruční úlohy.</span><span class="sxs-lookup"><span data-stu-id="48184-194">When 15Five, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="48184-195">Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="48184-195">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="48184-196">Přihlaste se k vaší **15Five** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="48184-196">Log in to your **15Five** company site as administrator.</span></span>

2. <span data-ttu-id="48184-197">Přejděte na **spravovat společnosti**.</span><span class="sxs-lookup"><span data-stu-id="48184-197">Go to **Manage Company**.</span></span>
   
    <span data-ttu-id="48184-198">![Spravovat společnosti](./media/active-directory-saas-15five-tutorial/IC784675.png "spravovat společnosti")</span><span class="sxs-lookup"><span data-stu-id="48184-198">![Manage Company](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")</span></span>

3. <span data-ttu-id="48184-199">Přejděte na **osoby \> přidat osoby**.</span><span class="sxs-lookup"><span data-stu-id="48184-199">Go to **People \> Add People**.</span></span>
   
    <span data-ttu-id="48184-200">![Lidé](./media/active-directory-saas-15five-tutorial/IC784676.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="48184-200">![People](./media/active-directory-saas-15five-tutorial/IC784676.png "People")</span></span>

4. <span data-ttu-id="48184-201">V části Přidání nové osobě proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="48184-201">In the Add New Person section, perform the following steps:</span></span>
   
    <span data-ttu-id="48184-202">![Přidat nové osobě](./media/active-directory-saas-15five-tutorial/IC784677.png "přidat nové osobě")</span><span class="sxs-lookup"><span data-stu-id="48184-202">![Add New Person](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")</span></span>
   
    <span data-ttu-id="48184-203">a.</span><span class="sxs-lookup"><span data-stu-id="48184-203">a.</span></span> <span data-ttu-id="48184-204">Typ **křestní jméno**, **příjmení**, **název**, **e-mailová adresa** platného účtu Azure Active Directory chcete zřídit do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="48184-204">Type the **First Name**, **Last Name**, **Title**, **Email address** of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="48184-205">b.</span><span class="sxs-lookup"><span data-stu-id="48184-205">b.</span></span> <span data-ttu-id="48184-206">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="48184-206">Click **Done**.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="48184-207">Držitel účtu Azure AD obdrží e-mail, včetně odkaz pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="48184-207">The Azure AD account holder receives an email including a link to confirm the account before it becomes active.</span></span>
   
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="48184-208">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="48184-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="48184-209">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k 15Five.</span><span class="sxs-lookup"><span data-stu-id="48184-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 15Five.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="48184-211">**Pokud chcete přiřadit Britta Simon 15Five, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="48184-211">**To assign Britta Simon to 15Five, perform the following steps:**</span></span>

1. <span data-ttu-id="48184-212">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="48184-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="48184-214">V seznamu aplikací vyberte **15Five**.</span><span class="sxs-lookup"><span data-stu-id="48184-214">In the applications list, select **15Five**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-15five-tutorial/tutorial_15five_app.png) 

3. <span data-ttu-id="48184-216">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="48184-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="48184-218">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="48184-218">Click **Add** button.</span></span> <span data-ttu-id="48184-219">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48184-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="48184-221">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="48184-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="48184-222">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48184-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="48184-223">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48184-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="48184-224">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="48184-224">Testing single sign-on</span></span>

<span data-ttu-id="48184-225">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="48184-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="48184-226">Když kliknete na dlaždici 15Five na přístupovém panelu, měli byste obdržet přihlašovací stránku 15Five aplikace.</span><span class="sxs-lookup"><span data-stu-id="48184-226">When you click the 15Five tile in the Access Panel, you should get login page of 15Five application.</span></span>
<span data-ttu-id="48184-227">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="48184-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="48184-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="48184-228">Additional resources</span></span>

* [<span data-ttu-id="48184-229">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="48184-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="48184-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="48184-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-15five-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-15five-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-15five-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-15five-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-15five-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-15five-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-15five-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-15five-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-15five-tutorial/tutorial_general_203.png

