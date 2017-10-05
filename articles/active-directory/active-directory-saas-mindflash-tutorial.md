---
title: 'Kurz: Azure Active Directory integrace s Mindflash | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Mindflash."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdf91993-aaaa-4598-89b7-77ef8ca065d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 90de7b6a82d88f9407a35fbfebe8a652928d76cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mindflash"></a><span data-ttu-id="01961-103">Kurz: Azure Active Directory integrace s Mindflash</span><span class="sxs-lookup"><span data-stu-id="01961-103">Tutorial: Azure Active Directory integration with Mindflash</span></span>

<span data-ttu-id="01961-104">V tomto kurzu zjistěte, jak integrovat Mindflash s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="01961-104">In this tutorial, you learn how to integrate Mindflash with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="01961-105">Integrace Mindflash s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="01961-105">Integrating Mindflash with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="01961-106">Můžete řídit ve službě Azure AD, který má přístup k Mindflash</span><span class="sxs-lookup"><span data-stu-id="01961-106">You can control in Azure AD who has access to Mindflash</span></span>
- <span data-ttu-id="01961-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Mindflash (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="01961-107">You can enable your users to automatically get signed-on to Mindflash (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="01961-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="01961-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="01961-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="01961-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01961-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01961-110">Prerequisites</span></span>

<span data-ttu-id="01961-111">Konfigurace integrace Azure AD s Mindflash, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="01961-111">To configure Azure AD integration with Mindflash, you need the following items:</span></span>

- <span data-ttu-id="01961-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="01961-112">An Azure AD subscription</span></span>
- <span data-ttu-id="01961-113">Mindflash jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="01961-113">A Mindflash single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="01961-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="01961-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="01961-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="01961-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="01961-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="01961-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="01961-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="01961-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="01961-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="01961-118">Scenario description</span></span>
<span data-ttu-id="01961-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="01961-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="01961-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="01961-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="01961-121">Přidání Mindflash z Galerie</span><span class="sxs-lookup"><span data-stu-id="01961-121">Adding Mindflash from the gallery</span></span>
2. <span data-ttu-id="01961-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="01961-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mindflash-from-the-gallery"></a><span data-ttu-id="01961-123">Přidání Mindflash z Galerie</span><span class="sxs-lookup"><span data-stu-id="01961-123">Adding Mindflash from the gallery</span></span>
<span data-ttu-id="01961-124">Při konfiguraci integrace Mindflash do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Mindflash z galerie.</span><span class="sxs-lookup"><span data-stu-id="01961-124">To configure the integration of Mindflash into Azure AD, you need to add Mindflash from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="01961-125">**Pokud chcete přidat Mindflash z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01961-125">**To add Mindflash from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="01961-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="01961-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="01961-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="01961-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="01961-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01961-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="01961-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01961-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="01961-133">Do vyhledávacího pole zadejte **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="01961-133">In the search box, type **Mindflash**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_search.png)

5. <span data-ttu-id="01961-135">Na panelu výsledků vyberte **Mindflash**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="01961-135">In the results panel, select **Mindflash**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="01961-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="01961-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="01961-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mindflash podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="01961-138">In this section, you configure and test Azure AD single sign-on with Mindflash based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="01961-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Mindflash je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01961-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mindflash is to a user in Azure AD.</span></span> <span data-ttu-id="01961-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Mindflash musí navázat.</span><span class="sxs-lookup"><span data-stu-id="01961-140">In other words, a link relationship between an Azure AD user and the related user in Mindflash needs to be established.</span></span>

<span data-ttu-id="01961-141">V Mindflash, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="01961-141">In Mindflash, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="01961-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mindflash, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="01961-142">To configure and test Azure AD single sign-on with Mindflash, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="01961-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="01961-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="01961-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01961-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="01961-145">**[Vytvoření zkušebního uživatele Mindflash](#creating-a-mindflash-test-user)**  – Pokud chcete mít protějšek Britta Simon v Mindflash propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="01961-145">**[Creating a Mindflash test user](#creating-a-mindflash-test-user)** - to have a counterpart of Britta Simon in Mindflash that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="01961-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01961-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="01961-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="01961-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="01961-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01961-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="01961-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Mindflash.</span><span class="sxs-lookup"><span data-stu-id="01961-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mindflash application.</span></span>

<span data-ttu-id="01961-150">**Ke konfiguraci Azure AD jednotné přihlašování s Mindflash, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01961-150">**To configure Azure AD single sign-on with Mindflash, perform the following steps:**</span></span>

1. <span data-ttu-id="01961-151">Na portálu Azure na **Mindflash** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="01961-151">In the Azure portal, on the **Mindflash** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="01961-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01961-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_samlbase.png)

3. <span data-ttu-id="01961-155">Na **Mindflash domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01961-155">On the **Mindflash Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_url.png)

    <span data-ttu-id="01961-157">a.</span><span class="sxs-lookup"><span data-stu-id="01961-157">a.</span></span> <span data-ttu-id="01961-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="01961-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.mindflash.com`</span></span>

    <span data-ttu-id="01961-159">b.</span><span class="sxs-lookup"><span data-stu-id="01961-159">b.</span></span> <span data-ttu-id="01961-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="01961-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.mindflash.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="01961-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="01961-161">These values are not real.</span></span> <span data-ttu-id="01961-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="01961-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="01961-163">Obraťte se na [tým podpory Mindflash klienta](https://www.mindflash.com/contact/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="01961-163">Contact [Mindflash Client support team](https://www.mindflash.com/contact/) to get these values.</span></span> 
 


4. <span data-ttu-id="01961-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="01961-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_certificate.png) 

5. <span data-ttu-id="01961-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01961-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mindflash-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="01961-168">Konfigurace jednotného přihlašování na **Mindflash** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Mindflash](https://www.mindflash.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="01961-168">To configure single sign-on on **Mindflash** side, you need to send the downloaded **Metadata XML** to [Mindflash support team](https://www.mindflash.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="01961-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="01961-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="01961-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="01961-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="01961-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="01961-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="01961-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="01961-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="01961-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01961-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="01961-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01961-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="01961-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="01961-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="01961-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="01961-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="01961-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01961-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="01961-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01961-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="01961-184">a.</span><span class="sxs-lookup"><span data-stu-id="01961-184">a.</span></span> <span data-ttu-id="01961-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="01961-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="01961-186">b.</span><span class="sxs-lookup"><span data-stu-id="01961-186">b.</span></span> <span data-ttu-id="01961-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="01961-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="01961-188">c.</span><span class="sxs-lookup"><span data-stu-id="01961-188">c.</span></span> <span data-ttu-id="01961-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="01961-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="01961-190">d.</span><span class="sxs-lookup"><span data-stu-id="01961-190">d.</span></span> <span data-ttu-id="01961-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01961-191">Click **Create**.</span></span>
 
### <a name="creating-a-mindflash-test-user"></a><span data-ttu-id="01961-192">Vytvoření zkušebního uživatele Mindflash</span><span class="sxs-lookup"><span data-stu-id="01961-192">Creating a Mindflash test user</span></span>

<span data-ttu-id="01961-193">Pokud chcete povolit uživatelům Azure AD přihlášení do Mindflash, musí být zřízená do Mindflash.</span><span class="sxs-lookup"><span data-stu-id="01961-193">In order to enable Azure AD users to log into Mindflash, they must be provisioned into Mindflash.</span></span> <span data-ttu-id="01961-194">V případě Mindflash zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="01961-194">In the case of Mindflash, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="01961-195">Ke zřízení uživatelských účtů, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01961-195">To provision a user accounts, perform the following steps:</span></span>

1. <span data-ttu-id="01961-196">Přihlaste se k vaší **Mindflash** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="01961-196">Log in to your **Mindflash** company site as an administrator.</span></span>

2. <span data-ttu-id="01961-197">Přejděte na **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="01961-197">Go to **Manage Users**.</span></span>
   
    <span data-ttu-id="01961-198">![Správa uživatelů](./media/active-directory-saas-mindflash-tutorial/ic787140.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="01961-198">![Manage Users](./media/active-directory-saas-mindflash-tutorial/ic787140.png "Manage Users")</span></span>

3. <span data-ttu-id="01961-199">Klikněte **přidat uživatele**a potom klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="01961-199">Click the **Add Users**, and then click **New**.</span></span>

4. <span data-ttu-id="01961-200">V **přidat nové uživatele** část, proveďte následující kroky platný Azure chcete zřídit účet AD:</span><span class="sxs-lookup"><span data-stu-id="01961-200">In the **Add New Users** section, perform the following steps of a valid Azure AD account you want to provision:</span></span>
   
    <span data-ttu-id="01961-201">![Přidání nových uživatelů](./media/active-directory-saas-mindflash-tutorial/ic787141.png "přidávat nové uživatele")</span><span class="sxs-lookup"><span data-stu-id="01961-201">![Add New Users](./media/active-directory-saas-mindflash-tutorial/ic787141.png "Add New Users")</span></span>
   
    <span data-ttu-id="01961-202">a.</span><span class="sxs-lookup"><span data-stu-id="01961-202">a.</span></span> <span data-ttu-id="01961-203">V **křestní jméno** textovému poli, typ **křestní jméno** uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="01961-203">In the **First name** textbox, type **First name** of the user as **Britta**.</span></span>

    <span data-ttu-id="01961-204">b.</span><span class="sxs-lookup"><span data-stu-id="01961-204">b.</span></span> <span data-ttu-id="01961-205">V **příjmení** textovému poli, typ **příjmení** uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="01961-205">In the **Last name** textbox, type **Last name** of the user as **Simon**.</span></span>
    
    <span data-ttu-id="01961-206">c.</span><span class="sxs-lookup"><span data-stu-id="01961-206">c.</span></span> <span data-ttu-id="01961-207">V **e-mailu** textovému poli, typ **e-mailovou adresu** uživatele jako  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="01961-207">In the **Email** textbox, type **Email Address** of the user as **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="01961-208">b.</span><span class="sxs-lookup"><span data-stu-id="01961-208">b.</span></span> <span data-ttu-id="01961-209">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="01961-209">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="01961-210">Můžete použít všechny ostatní Mindflash uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Mindflash zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="01961-210">You can use any other Mindflash user account creation tools or APIs provided by Mindflash to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="01961-211">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="01961-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="01961-212">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Mindflash.</span><span class="sxs-lookup"><span data-stu-id="01961-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mindflash.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="01961-214">**Pokud chcete přiřadit Britta Simon Mindflash, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="01961-214">**To assign Britta Simon to Mindflash, perform the following steps:**</span></span>

1. <span data-ttu-id="01961-215">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01961-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="01961-217">V seznamu aplikací vyberte **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="01961-217">In the applications list, select **Mindflash**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_app.png) 

3. <span data-ttu-id="01961-219">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="01961-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="01961-221">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01961-221">Click **Add** button.</span></span> <span data-ttu-id="01961-222">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01961-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="01961-224">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="01961-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="01961-225">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01961-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="01961-226">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01961-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="01961-227">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01961-227">Testing single sign-on</span></span>

<span data-ttu-id="01961-228">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="01961-228">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="01961-229">Když kliknete na dlaždici Mindflash na přístupovém panelu, měli byste obdržet přihlašovací stránku Mindflash aplikace.</span><span class="sxs-lookup"><span data-stu-id="01961-229">When you click the Mindflash tile in the Access Panel, you should get login page of Mindflash application.</span></span>
<span data-ttu-id="01961-230">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="01961-230">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01961-231">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="01961-231">Additional resources</span></span>

* [<span data-ttu-id="01961-232">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="01961-232">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="01961-233">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="01961-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_203.png

