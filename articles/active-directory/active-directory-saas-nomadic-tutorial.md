---
title: 'Kurz: Azure Active Directory integrace s Nomadic | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Nomadic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13d02b1c-d98a-40b1-824f-afa45a2deb6a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 1817a1395c2ffa7268abfff268d9d041f7f21a57
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nomadic"></a><span data-ttu-id="3c77f-103">Kurz: Azure Active Directory integrace s Nomadic</span><span class="sxs-lookup"><span data-stu-id="3c77f-103">Tutorial: Azure Active Directory integration with Nomadic</span></span>

<span data-ttu-id="3c77f-104">V tomto kurzu zjistěte, jak integrovat Nomadic s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3c77f-104">In this tutorial, you learn how to integrate Nomadic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3c77f-105">Integrace Nomadic s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3c77f-105">Integrating Nomadic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3c77f-106">Můžete ovládat ve službě Azure AD, který má přístup k Nomadic.</span><span class="sxs-lookup"><span data-stu-id="3c77f-106">You can control in Azure AD who has access to Nomadic.</span></span>
- <span data-ttu-id="3c77f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Nomadic (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3c77f-107">You can enable your users to automatically get signed-on to Nomadic (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="3c77f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3c77f-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="3c77f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3c77f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c77f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3c77f-110">Prerequisites</span></span>

<span data-ttu-id="3c77f-111">Konfigurace integrace Azure AD s Nomadic, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3c77f-111">To configure Azure AD integration with Nomadic, you need the following items:</span></span>

- <span data-ttu-id="3c77f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3c77f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3c77f-113">Nomadic jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3c77f-113">A Nomadic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3c77f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c77f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3c77f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3c77f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3c77f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3c77f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3c77f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3c77f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3c77f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3c77f-118">Scenario description</span></span>
<span data-ttu-id="3c77f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3c77f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3c77f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3c77f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3c77f-121">Přidat Nomadic z Galerie</span><span class="sxs-lookup"><span data-stu-id="3c77f-121">Add Nomadic from the gallery</span></span>
2. <span data-ttu-id="3c77f-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3c77f-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-nomadic-from-the-gallery"></a><span data-ttu-id="3c77f-123">Přidat Nomadic z Galerie</span><span class="sxs-lookup"><span data-stu-id="3c77f-123">Add Nomadic from the gallery</span></span>
<span data-ttu-id="3c77f-124">Při konfiguraci integrace Nomadic do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Nomadic z galerie.</span><span class="sxs-lookup"><span data-stu-id="3c77f-124">To configure the integration of Nomadic into Azure AD, you need to add Nomadic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3c77f-125">**Pokud chcete přidat Nomadic z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3c77f-125">**To add Nomadic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3c77f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3c77f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="3c77f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3c77f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="3c77f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c77f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="3c77f-133">Do vyhledávacího pole zadejte **Nomadic**, vyberte **Nomadic** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3c77f-133">In the search box, type **Nomadic**, select **Nomadic** from result panel then click **Add** button to add the application.</span></span>

    ![Nomadic v seznamu výsledků](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3c77f-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3c77f-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="3c77f-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Nomadic podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3c77f-136">In this section, you configure and test Azure AD single sign-on with Nomadic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3c77f-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Nomadic je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3c77f-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Nomadic is to a user in Azure AD.</span></span> <span data-ttu-id="3c77f-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Nomadic musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3c77f-138">In other words, a link relationship between an Azure AD user and the related user in Nomadic needs to be established.</span></span>

<span data-ttu-id="3c77f-139">V Nomadic, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3c77f-139">In Nomadic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3c77f-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Nomadic, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3c77f-140">To configure and test Azure AD single sign-on with Nomadic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3c77f-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3c77f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3c77f-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3c77f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3c77f-143">**[Vytvoření Nomadic zkušebního uživatele](#create-a-nomadic-test-user)**  – Pokud chcete mít protějšek Britta Simon v Nomadic propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c77f-143">**[Create a Nomadic test user](#create-a-nomadic-test-user)** - to have a counterpart of Britta Simon in Nomadic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3c77f-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c77f-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3c77f-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3c77f-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="3c77f-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3c77f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="3c77f-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Nomadic.</span><span class="sxs-lookup"><span data-stu-id="3c77f-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Nomadic application.</span></span>

<span data-ttu-id="3c77f-148">**Ke konfiguraci Azure AD jednotné přihlašování s Nomadic, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3c77f-148">**To configure Azure AD single sign-on with Nomadic, perform the following steps:**</span></span>

1. <span data-ttu-id="3c77f-149">Na portálu Azure na **Nomadic** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-149">In the Azure portal, on the **Nomadic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="3c77f-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3c77f-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_samlbase.png)

3. <span data-ttu-id="3c77f-153">Na **Nomadic domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3c77f-153">On the **Nomadic Domain and URLs** section, perform the following steps:</span></span>

    ![Nomadic domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_url.png)

    <span data-ttu-id="3c77f-155">a.</span><span class="sxs-lookup"><span data-stu-id="3c77f-155">a.</span></span> <span data-ttu-id="3c77f-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.nomadic.fm/signin`</span><span class="sxs-lookup"><span data-stu-id="3c77f-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.nomadic.fm/signin`</span></span>

    <span data-ttu-id="3c77f-157">b.</span><span class="sxs-lookup"><span data-stu-id="3c77f-157">b.</span></span> <span data-ttu-id="3c77f-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://<company name>.nomadic.fm/auth/saml2/sp`,`https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span><span class="sxs-lookup"><span data-stu-id="3c77f-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.nomadic.fm/auth/saml2/sp`, `https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3c77f-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3c77f-159">These values are not real.</span></span> <span data-ttu-id="3c77f-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="3c77f-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3c77f-161">Obraťte se na [tým podpory Nomadic klienta](mailto:help@nomadic.fm) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3c77f-161">Contact [Nomadic Client support team](mailto:help@nomadic.fm) to get these values.</span></span> 
 


4. <span data-ttu-id="3c77f-162">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3c77f-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_certificate.png) 

5. <span data-ttu-id="3c77f-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c77f-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-nomadic-tutorial/tutorial_general_400.png)

6.  <span data-ttu-id="3c77f-166">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [tým podpory Nomadic](mailto:help@nomadic.fm) a jim poskytnout stažený **metadata**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-166">To get SSO configured for your application, contact [Nomadic support team](mailto:help@nomadic.fm) and provide them with the downloaded **metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="3c77f-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3c77f-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3c77f-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3c77f-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3c77f-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3c77f-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3c77f-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3c77f-170">Create an Azure AD test user</span></span>

<span data-ttu-id="3c77f-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3c77f-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="3c77f-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3c77f-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3c77f-174">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c77f-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-nomadic-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="3c77f-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-nomadic-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="3c77f-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c77f-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-nomadic-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="3c77f-180">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3c77f-180">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-nomadic-tutorial/create_aaduser_04.png)

    <span data-ttu-id="3c77f-182">a.</span><span class="sxs-lookup"><span data-stu-id="3c77f-182">a.</span></span> <span data-ttu-id="3c77f-183">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3c77f-184">b.</span><span class="sxs-lookup"><span data-stu-id="3c77f-184">b.</span></span> <span data-ttu-id="3c77f-185">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3c77f-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="3c77f-186">c.</span><span class="sxs-lookup"><span data-stu-id="3c77f-186">c.</span></span> <span data-ttu-id="3c77f-187">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="3c77f-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="3c77f-188">d.</span><span class="sxs-lookup"><span data-stu-id="3c77f-188">d.</span></span> <span data-ttu-id="3c77f-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-189">Click **Create**.</span></span>
 
### <a name="create-a-nomadic-test-user"></a><span data-ttu-id="3c77f-190">Vytvoření Nomadic zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="3c77f-190">Create a Nomadic test user</span></span>

<span data-ttu-id="3c77f-191">V této části vytvoříte volal Britta Simon v Nomadic uživatele.</span><span class="sxs-lookup"><span data-stu-id="3c77f-191">In this section, you create a user called Britta Simon in Nomadic.</span></span> <span data-ttu-id="3c77f-192">Spojte se s [tým podpory Nomadic](mailto:help@nomadic.fm) přidat uživatele do Nomadic platformy.</span><span class="sxs-lookup"><span data-stu-id="3c77f-192">Please work with [Nomadic support team](mailto:help@nomadic.fm) to add the users in the Nomadic platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="3c77f-193">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3c77f-193">Assign the Azure AD test user</span></span>

<span data-ttu-id="3c77f-194">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Nomadic.</span><span class="sxs-lookup"><span data-stu-id="3c77f-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Nomadic.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="3c77f-196">**Pokud chcete přiřadit Britta Simon Nomadic, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3c77f-196">**To assign Britta Simon to Nomadic, perform the following steps:**</span></span>

1. <span data-ttu-id="3c77f-197">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3c77f-199">V seznamu aplikací vyberte **Nomadic**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-199">In the applications list, select **Nomadic**.</span></span>

    ![Odkaz Nomadic v seznamu aplikací](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_app.png)  

3. <span data-ttu-id="3c77f-201">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3c77f-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="3c77f-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3c77f-203">Click **Add** button.</span></span> <span data-ttu-id="3c77f-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c77f-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="3c77f-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3c77f-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3c77f-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c77f-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3c77f-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3c77f-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="3c77f-209">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3c77f-209">Test single sign-on</span></span>

<span data-ttu-id="3c77f-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3c77f-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3c77f-211">Když kliknete na dlaždici Nomadic na přístupovém panelu jste měli získat automaticky přihlášení k aplikaci Nomadic.</span><span class="sxs-lookup"><span data-stu-id="3c77f-211">When you click the Nomadic tile in the Access Panel, you should get automatically signed-on to your Nomadic application.</span></span>
<span data-ttu-id="3c77f-212">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3c77f-212">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3c77f-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3c77f-213">Additional resources</span></span>

* [<span data-ttu-id="3c77f-214">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3c77f-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3c77f-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3c77f-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_203.png

