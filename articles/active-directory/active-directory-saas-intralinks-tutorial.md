---
title: 'Kurz: Azure Active Directory integrace s Intralinks | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Intralinks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ee7fd5b88ac806104002ffb41af11bab4fd1b2dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a><span data-ttu-id="b80cf-103">Kurz: Azure Active Directory integrace s Intralinks</span><span class="sxs-lookup"><span data-stu-id="b80cf-103">Tutorial: Azure Active Directory integration with Intralinks</span></span>

<span data-ttu-id="b80cf-104">V tomto kurzu zjistěte, jak integrovat Intralinks s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b80cf-104">In this tutorial, you learn how to integrate Intralinks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b80cf-105">Integrace Intralinks s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b80cf-105">Integrating Intralinks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b80cf-106">Můžete řídit ve službě Azure AD, který má přístup k Intralinks</span><span class="sxs-lookup"><span data-stu-id="b80cf-106">You can control in Azure AD who has access to Intralinks</span></span>
- <span data-ttu-id="b80cf-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Intralinks (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b80cf-107">You can enable your users to automatically get signed-on to Intralinks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b80cf-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b80cf-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b80cf-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b80cf-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b80cf-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b80cf-110">Prerequisites</span></span>

<span data-ttu-id="b80cf-111">Konfigurace integrace Azure AD s Intralinks, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b80cf-111">To configure Azure AD integration with Intralinks, you need the following items:</span></span>

- <span data-ttu-id="b80cf-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b80cf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b80cf-113">Intralinks jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b80cf-113">An Intralinks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b80cf-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b80cf-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b80cf-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b80cf-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b80cf-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b80cf-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b80cf-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b80cf-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b80cf-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b80cf-118">Scenario description</span></span>
<span data-ttu-id="b80cf-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b80cf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b80cf-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b80cf-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b80cf-121">Přidání Intralinks z Galerie</span><span class="sxs-lookup"><span data-stu-id="b80cf-121">Adding Intralinks from the gallery</span></span>
2. <span data-ttu-id="b80cf-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b80cf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intralinks-from-the-gallery"></a><span data-ttu-id="b80cf-123">Přidání Intralinks z Galerie</span><span class="sxs-lookup"><span data-stu-id="b80cf-123">Adding Intralinks from the gallery</span></span>
<span data-ttu-id="b80cf-124">Při konfiguraci integrace Intralinks do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Intralinks z galerie.</span><span class="sxs-lookup"><span data-stu-id="b80cf-124">To configure the integration of Intralinks into Azure AD, you need to add Intralinks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b80cf-125">**Pokud chcete přidat Intralinks z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b80cf-125">**To add Intralinks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b80cf-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b80cf-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b80cf-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b80cf-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b80cf-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b80cf-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b80cf-133">Do vyhledávacího pole zadejte **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-133">In the search box, type **Intralinks**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="b80cf-135">Na panelu výsledků vyberte **Intralinks**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b80cf-135">In the results panel, select **Intralinks**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b80cf-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b80cf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b80cf-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Intralinks podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b80cf-138">In this section, you configure and test Azure AD single sign-on with Intralinks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b80cf-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Intralinks je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b80cf-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Intralinks is to a user in Azure AD.</span></span> <span data-ttu-id="b80cf-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Intralinks musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b80cf-140">In other words, a link relationship between an Azure AD user and the related user in Intralinks needs to be established.</span></span>

<span data-ttu-id="b80cf-141">V Intralinks, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="b80cf-141">In Intralinks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b80cf-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Intralinks, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b80cf-142">To configure and test Azure AD single sign-on with Intralinks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b80cf-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b80cf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b80cf-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b80cf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b80cf-145">**[Vytváření testovacího uživatele Intralinks](#creating-an-intralinks-test-user)**  – Pokud chcete mít protějšek Britta Simon v Intralinks propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b80cf-145">**[Creating an Intralinks test user](#creating-an-intralinks-test-user)** - to have a counterpart of Britta Simon in Intralinks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b80cf-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b80cf-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b80cf-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b80cf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b80cf-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b80cf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b80cf-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Intralinks.</span><span class="sxs-lookup"><span data-stu-id="b80cf-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Intralinks application.</span></span>

<span data-ttu-id="b80cf-150">**Ke konfiguraci Azure AD jednotné přihlašování s Intralinks, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b80cf-150">**To configure Azure AD single sign-on with Intralinks, perform the following steps:**</span></span>

1. <span data-ttu-id="b80cf-151">Na portálu Azure na **Intralinks** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-151">In the Azure portal, on the **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b80cf-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b80cf-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. <span data-ttu-id="b80cf-155">Na **Intralinks domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b80cf-155">On the **Intralinks Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    <span data-ttu-id="b80cf-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span><span class="sxs-lookup"><span data-stu-id="b80cf-157">In the **Sign-on URL** textbox, type a URL using the following pattern:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b80cf-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="b80cf-158">This value is not real.</span></span> <span data-ttu-id="b80cf-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b80cf-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="b80cf-160">Obraťte se na [tým podpory Intralinks klienta](https://www.intralinks.com/contact-1) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b80cf-160">Contact [Intralinks Client support team](https://www.intralinks.com/contact-1) to get this value.</span></span> 
 
4. <span data-ttu-id="b80cf-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b80cf-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. <span data-ttu-id="b80cf-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b80cf-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b80cf-165">Konfigurace jednotného přihlašování na **Intralinks** straně, budete muset odeslat stažené **soubor XML s metadaty** [tým podpory Intralinks](https://www.intralinks.com/contact-1).</span><span class="sxs-lookup"><span data-stu-id="b80cf-165">To configure single sign-on on **Intralinks** side, you need to send the downloaded **Metadata XML** [Intralinks support team](https://www.intralinks.com/contact-1).</span></span> <span data-ttu-id="b80cf-166">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="b80cf-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="b80cf-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="b80cf-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b80cf-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="b80cf-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b80cf-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b80cf-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b80cf-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b80cf-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="b80cf-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b80cf-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b80cf-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b80cf-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b80cf-174">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b80cf-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b80cf-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b80cf-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b80cf-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b80cf-180">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b80cf-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b80cf-182">a.</span><span class="sxs-lookup"><span data-stu-id="b80cf-182">a.</span></span> <span data-ttu-id="b80cf-183">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b80cf-184">b.</span><span class="sxs-lookup"><span data-stu-id="b80cf-184">b.</span></span> <span data-ttu-id="b80cf-185">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b80cf-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b80cf-186">c.</span><span class="sxs-lookup"><span data-stu-id="b80cf-186">c.</span></span> <span data-ttu-id="b80cf-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b80cf-188">d.</span><span class="sxs-lookup"><span data-stu-id="b80cf-188">d.</span></span> <span data-ttu-id="b80cf-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-189">Click **Create**.</span></span>
 
### <a name="creating-an-intralinks-test-user"></a><span data-ttu-id="b80cf-190">Vytváření testovacího uživatele Intralinks</span><span class="sxs-lookup"><span data-stu-id="b80cf-190">Creating an Intralinks test user</span></span>

<span data-ttu-id="b80cf-191">V této části vytvoříte volal Britta Simon v Intralinks uživatele.</span><span class="sxs-lookup"><span data-stu-id="b80cf-191">In this section, you create a user called Britta Simon in Intralinks.</span></span> <span data-ttu-id="b80cf-192">Spojte se s [tým podpory Intralinks](https://www.intralinks.com/contact-1) přidat uživatele do Intralinks platformy.</span><span class="sxs-lookup"><span data-stu-id="b80cf-192">Please work with [Intralinks support team](https://www.intralinks.com/contact-1) to add the users in the Intralinks platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b80cf-193">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b80cf-193">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b80cf-194">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Intralinks.</span><span class="sxs-lookup"><span data-stu-id="b80cf-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Intralinks.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b80cf-196">**Pokud chcete přiřadit Britta Simon Intralinks, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b80cf-196">**To assign Britta Simon to Intralinks, perform the following steps:**</span></span>

1. <span data-ttu-id="b80cf-197">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b80cf-199">V seznamu aplikací vyberte **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-199">In the applications list, select **Intralinks**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. <span data-ttu-id="b80cf-201">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b80cf-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b80cf-203">Click **Add** button.</span></span> <span data-ttu-id="b80cf-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b80cf-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b80cf-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b80cf-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b80cf-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b80cf-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b80cf-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b80cf-208">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="add-intralinks-via-or-elite-application"></a><span data-ttu-id="b80cf-209">Přidání Intralinks prostřednictvím nebo Elite aplikace</span><span class="sxs-lookup"><span data-stu-id="b80cf-209">Add Intralinks VIA or Elite application</span></span>

<span data-ttu-id="b80cf-210">Intralinks používá stejnou identitu platformy jednotného přihlašování pro všechny ostatní aplikace Intralinks s výjimkou pozornosti Nexus aplikace.</span><span class="sxs-lookup"><span data-stu-id="b80cf-210">Intralinks uses the same SSO identity platform for all other Intralinks applications excluding Deal Nexus application.</span></span> <span data-ttu-id="b80cf-211">Takže pokud máte v úmyslu použít žádnou jinou aplikaci Intralinks pak nejprve budete muset nakonfigurovat jednotné přihlašování pro jednu primární Intralinks aplikaci pomocí výše uvedený postup.</span><span class="sxs-lookup"><span data-stu-id="b80cf-211">So if you plan to use any other Intralinks application then first you have to configure SSO for one Primary Intralinks application using the procedure described above.</span></span>

<span data-ttu-id="b80cf-212">Potom můžete provést následující postup pro přidání jiná aplikace Intralinks ve vašem klientovi, který můžete využít této primární aplikace pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b80cf-212">After that you can follow the below procedure to add another Intralinks application in your tenant which can leverage this primary application for SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="b80cf-213">Tato funkce je k dispozici pouze zákazníkům SKU služby Azure AD Premium a není dostupné pro zákazníky volné nebo základní SKU.</span><span class="sxs-lookup"><span data-stu-id="b80cf-213">This feature is available only to Azure AD Premium SKU Customers and not available for Free or Basic SKU customers.</span></span>

1. <span data-ttu-id="b80cf-214">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b80cf-214">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]


2. <span data-ttu-id="b80cf-216">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-216">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b80cf-217">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-217">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b80cf-219">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b80cf-219">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b80cf-221">Do vyhledávacího pole zadejte **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-221">In the search box, type **Intralinks**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="b80cf-223">Na **Intralinks přidat aplikaci** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b80cf-223">On **Intralinks Add app** perform the following steps:</span></span>

    ![Přidání Intralinks prostřednictvím nebo Elite aplikace](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    <span data-ttu-id="b80cf-225">a.</span><span class="sxs-lookup"><span data-stu-id="b80cf-225">a.</span></span> <span data-ttu-id="b80cf-226">V **název** textovému poli, zadejte odpovídající název aplikace, například **Intralinks Elite**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-226">In **Name** textbox, enter appropriate name of the application e.g. **Intralinks Elite**.</span></span>

    <span data-ttu-id="b80cf-227">b.</span><span class="sxs-lookup"><span data-stu-id="b80cf-227">b.</span></span> <span data-ttu-id="b80cf-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b80cf-228">Click **Add** button.</span></span>

6.  <span data-ttu-id="b80cf-229">Na portálu Azure na **Intralinks** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-229">In the Azure portal, on the **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

7. <span data-ttu-id="b80cf-231">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **propojené přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-231">On the **Single sign-on** dialog, select **Mode** as **Linked Sign-on**.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. <span data-ttu-id="b80cf-233">Získat SP iniciované URL jednotného přihlašování z [Intralinks team](https://www.intralinks.com/contact-1) pro jinou aplikaci Intralinks a zadejte ho v **konfigurovat přihlašovací adresa URL** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="b80cf-233">Get the the SP Initiated SSO URL from [Intralinks team](https://www.intralinks.com/contact-1) for the other Intralinks application and enter it in **Configure Sign-on URL** as shown below.</span></span> 
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     <span data-ttu-id="b80cf-235">Do textového pole Přihlašovací adresa URL zadejte adresu URL používá uživatelům přihlášení do aplikace Intralinks pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="b80cf-235">In the Sign On URL textbox, type the URL used by your users to sign-on to your Intralinks application using the following pattern:</span></span>
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. <span data-ttu-id="b80cf-236">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b80cf-236">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="b80cf-238">Přiřazení aplikace pro uživatele nebo skupiny, jak je uvedeno v části  **[přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**.</span><span class="sxs-lookup"><span data-stu-id="b80cf-238">Assign the application to user or groups as shown in the section **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)**.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="b80cf-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b80cf-239">Testing single sign-on</span></span>

<span data-ttu-id="b80cf-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b80cf-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b80cf-241">Když kliknete na dlaždici Intralinks na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Intralinks.</span><span class="sxs-lookup"><span data-stu-id="b80cf-241">When you click the Intralinks tile in the Access Panel, you should get automatically signed-on to your Intralinks application.</span></span>
<span data-ttu-id="b80cf-242">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b80cf-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b80cf-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b80cf-243">Additional resources</span></span>

* [<span data-ttu-id="b80cf-244">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b80cf-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b80cf-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b80cf-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png

