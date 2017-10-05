---
title: 'Kurz: Azure Active Directory integrace s AppBlade | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a AppBlade."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 7820a70b34b6d25ba81b17c472159d08904335d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a><span data-ttu-id="2feaa-103">Kurz: Azure Active Directory integrace s AppBlade</span><span class="sxs-lookup"><span data-stu-id="2feaa-103">Tutorial: Azure Active Directory integration with AppBlade</span></span>

<span data-ttu-id="2feaa-104">V tomto kurzu zjistěte, jak integrovat AppBlade s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2feaa-104">In this tutorial, you learn how to integrate AppBlade with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2feaa-105">Integrace AppBlade s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2feaa-105">Integrating AppBlade with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2feaa-106">Můžete řídit ve službě Azure AD, který má přístup k AppBlade</span><span class="sxs-lookup"><span data-stu-id="2feaa-106">You can control in Azure AD who has access to AppBlade</span></span>
- <span data-ttu-id="2feaa-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k AppBlade (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2feaa-107">You can enable your users to automatically get signed-on to AppBlade (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2feaa-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2feaa-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2feaa-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2feaa-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2feaa-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2feaa-110">Prerequisites</span></span>

<span data-ttu-id="2feaa-111">Konfigurace integrace Azure AD s AppBlade, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="2feaa-111">To configure Azure AD integration with AppBlade, you need the following items:</span></span>

- <span data-ttu-id="2feaa-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2feaa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2feaa-113">AppBlade jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2feaa-113">An AppBlade single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2feaa-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2feaa-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2feaa-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2feaa-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2feaa-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2feaa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2feaa-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2feaa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2feaa-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2feaa-118">Scenario description</span></span>
<span data-ttu-id="2feaa-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2feaa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2feaa-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2feaa-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2feaa-121">Přidání AppBlade z Galerie</span><span class="sxs-lookup"><span data-stu-id="2feaa-121">Adding AppBlade from the gallery</span></span>
2. <span data-ttu-id="2feaa-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2feaa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appblade-from-the-gallery"></a><span data-ttu-id="2feaa-123">Přidání AppBlade z Galerie</span><span class="sxs-lookup"><span data-stu-id="2feaa-123">Adding AppBlade from the gallery</span></span>
<span data-ttu-id="2feaa-124">Při konfiguraci integrace AppBlade do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS AppBlade z galerie.</span><span class="sxs-lookup"><span data-stu-id="2feaa-124">To configure the integration of AppBlade into Azure AD, you need to add AppBlade from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2feaa-125">**Pokud chcete přidat AppBlade z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2feaa-125">**To add AppBlade from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2feaa-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2feaa-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2feaa-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2feaa-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2feaa-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2feaa-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2feaa-133">Do vyhledávacího pole zadejte **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-133">In the search box, type **AppBlade**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_search.png)

5. <span data-ttu-id="2feaa-135">Na panelu výsledků vyberte **AppBlade**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2feaa-135">In the results panel, select **AppBlade**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2feaa-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2feaa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2feaa-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s AppBlade podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2feaa-138">In this section, you configure and test Azure AD single sign-on with AppBlade based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2feaa-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v AppBlade je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2feaa-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AppBlade is to a user in Azure AD.</span></span> <span data-ttu-id="2feaa-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v AppBlade musí navázat.</span><span class="sxs-lookup"><span data-stu-id="2feaa-140">In other words, a link relationship between an Azure AD user and the related user in AppBlade needs to be established.</span></span>

<span data-ttu-id="2feaa-141">V AppBlade, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="2feaa-141">In AppBlade, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2feaa-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s AppBlade, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="2feaa-142">To configure and test Azure AD single sign-on with AppBlade, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2feaa-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="2feaa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2feaa-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2feaa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2feaa-145">**[Vytváření testovacího uživatele AppBlade](#creating-an-appblade-test-user)**  – Pokud chcete mít protějšek Britta Simon v AppBlade propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2feaa-145">**[Creating an AppBlade test user](#creating-an-appblade-test-user)** - to have a counterpart of Britta Simon in AppBlade that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2feaa-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2feaa-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2feaa-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2feaa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2feaa-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2feaa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2feaa-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci AppBlade.</span><span class="sxs-lookup"><span data-stu-id="2feaa-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AppBlade application.</span></span>

<span data-ttu-id="2feaa-150">**Ke konfiguraci Azure AD jednotné přihlašování s AppBlade, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2feaa-150">**To configure Azure AD single sign-on with AppBlade, perform the following steps:**</span></span>

1. <span data-ttu-id="2feaa-151">Na portálu Azure na **AppBlade** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-151">In the Azure portal, on the **AppBlade** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2feaa-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2feaa-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_samlbase.png)

3. <span data-ttu-id="2feaa-155">Na **AppBlade domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2feaa-155">On the **AppBlade Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_url.png)

    <span data-ttu-id="2feaa-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.appblade.com/saml/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="2feaa-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.appblade.com/saml/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2feaa-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="2feaa-158">This value is not real.</span></span> <span data-ttu-id="2feaa-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2feaa-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="2feaa-160">Obraťte se na [tým podpory AppBlade klienta](mailto:support@appblade.com) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2feaa-160">Contact [AppBlade Client support team](mailto:support@appblade.com) to get the value.</span></span> 
 
4. <span data-ttu-id="2feaa-161">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2feaa-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_certificate.png) 

5. <span data-ttu-id="2feaa-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2feaa-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2feaa-165">Konfigurace jednotného přihlašování na **AppBlade** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory AppBlade](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="2feaa-165">To configure single sign-on on **AppBlade** side, you need to send the downloaded **Metadata XML** to [AppBlade support team](mailto:support@appblade.com).</span></span> <span data-ttu-id="2feaa-166">Také, požádejte o konfiguraci **URL vystavitele jednotného přihlašování k** jako `https://appblade.com/saml`.</span><span class="sxs-lookup"><span data-stu-id="2feaa-166">Also, please ask them to configure the **SSO Issuer URL** as `https://appblade.com/saml`.</span></span> <span data-ttu-id="2feaa-167">Toto nastavení je povinné pro jednotné přihlašování pro práci.</span><span class="sxs-lookup"><span data-stu-id="2feaa-167">This setting is required for single sign-on to work.</span></span>


> [!TIP]
> <span data-ttu-id="2feaa-168">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="2feaa-168">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2feaa-169">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="2feaa-169">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2feaa-170">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2feaa-170">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2feaa-171">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2feaa-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="2feaa-172">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2feaa-172">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2feaa-174">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2feaa-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2feaa-175">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2feaa-175">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2feaa-177">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-177">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2feaa-179">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2feaa-179">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2feaa-181">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2feaa-181">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2feaa-183">a.</span><span class="sxs-lookup"><span data-stu-id="2feaa-183">a.</span></span> <span data-ttu-id="2feaa-184">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-184">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2feaa-185">b.</span><span class="sxs-lookup"><span data-stu-id="2feaa-185">b.</span></span> <span data-ttu-id="2feaa-186">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2feaa-186">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2feaa-187">c.</span><span class="sxs-lookup"><span data-stu-id="2feaa-187">c.</span></span> <span data-ttu-id="2feaa-188">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-188">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2feaa-189">d.</span><span class="sxs-lookup"><span data-stu-id="2feaa-189">d.</span></span> <span data-ttu-id="2feaa-190">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-190">Click **Create**.</span></span>
 
### <a name="creating-an-appblade-test-user"></a><span data-ttu-id="2feaa-191">Vytváření testovacího uživatele AppBlade</span><span class="sxs-lookup"><span data-stu-id="2feaa-191">Creating an AppBlade test user</span></span>

<span data-ttu-id="2feaa-192">Cílem této části je vytvoření uživatele v AppBlade nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2feaa-192">The objective of this section is to create a user called Britta Simon in AppBlade.</span></span> <span data-ttu-id="2feaa-193">AppBlade podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="2feaa-193">AppBlade supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="2feaa-194">**Ujistěte se, že je pro zřizování uživatelů AppBlade nakonfigurován název vaší domény. Poté, co pouze za běhu uživatel zřizování funguje.**</span><span class="sxs-lookup"><span data-stu-id="2feaa-194">**Make sure that your domain name is configured with AppBlade for user provisioning. After that only the just-in-time user provisioning works.**</span></span>

<span data-ttu-id="2feaa-195">Pokud má uživatel e-mailovou adresu konče nakonfiguroval AppBlade pro váš účet domény, pak uživatel se automaticky připojí k účtu jako člena s úrovní oprávnění, které zadáte, který je jedním z "Basic" (základní uživatel, který můžete nainstalovat pouze aplikace) , "Člen týmu" (uživatel, který můžete nahrát nové verze aplikace a řízení projektů), nebo "Správce" (Správce úplná oprávnění k účtu).</span><span class="sxs-lookup"><span data-stu-id="2feaa-195">If the user has an email address ending with the domain configured by AppBlade for your account, then the user will automatically join the account as a member with the permission level you specify, which is one of "Basic" (a basic user who can only install applications), "Team Member" (a user who can upload new app versions and manage projects), or "Administrator" (full admin privileges to the account).</span></span> <span data-ttu-id="2feaa-196">Obvykle jednu by zvolte Basic a pak zvýšení úrovně ručně přes (AppBlade musí konfigurovat buď k přihlášení na základě e-mailu správce předem nebo povýšit uživatele po přihlášení jménem zákazníka) přihlašovací jméno správce.</span><span class="sxs-lookup"><span data-stu-id="2feaa-196">Normally one would choose Basic and then promote users manually via an Admin login (AppBlade needs to configure either an email-based admin login in advance or promote a user on behalf of the customer after login).</span></span>

<span data-ttu-id="2feaa-197">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="2feaa-197">There is no action item for you in this section.</span></span> <span data-ttu-id="2feaa-198">Nový uživatel se vytvoří během pokusu o přístup k AppBlade, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="2feaa-198">A new user is created during an attempt to access AppBlade if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="2feaa-199">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory AppBlade](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="2feaa-199">If you need to create a user manually, you need to contact the [AppBlade support team](mailto:support@appblade.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2feaa-200">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2feaa-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2feaa-201">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu AppBlade.</span><span class="sxs-lookup"><span data-stu-id="2feaa-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AppBlade.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2feaa-203">**Pokud chcete přiřadit Britta Simon AppBlade, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2feaa-203">**To assign Britta Simon to AppBlade, perform the following steps:**</span></span>

1. <span data-ttu-id="2feaa-204">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2feaa-206">V seznamu aplikací vyberte **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-206">In the applications list, select **AppBlade**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_app.png) 

3. <span data-ttu-id="2feaa-208">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2feaa-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2feaa-210">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2feaa-210">Click **Add** button.</span></span> <span data-ttu-id="2feaa-211">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2feaa-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2feaa-213">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2feaa-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2feaa-214">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2feaa-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2feaa-215">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2feaa-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2feaa-216">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2feaa-216">Testing single sign-on</span></span>

<span data-ttu-id="2feaa-217">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2feaa-217">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="2feaa-218">Když kliknete na dlaždici AppBlade na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci AppBlade.</span><span class="sxs-lookup"><span data-stu-id="2feaa-218">When you click the AppBlade tile in the Access Panel, you should get automatically signed-on to your AppBlade application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2feaa-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2feaa-219">Additional resources</span></span>

* [<span data-ttu-id="2feaa-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2feaa-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2feaa-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2feaa-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_203.png

