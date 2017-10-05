---
title: 'Kurz: Azure Active Directory integrace s RealtimeBoard | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a RealtimeBoard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a37fc1c0-4bae-4173-989b-00de53a0076f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: jeedes
ms.openlocfilehash: d3ba8cb1f7e1d4332f7912848e8b6902d9acf909
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-realtimeboard"></a><span data-ttu-id="74624-103">Kurz: Azure Active Directory integrace s RealtimeBoard</span><span class="sxs-lookup"><span data-stu-id="74624-103">Tutorial: Azure Active Directory integration with RealtimeBoard</span></span>

<span data-ttu-id="74624-104">V tomto kurzu zjistěte, jak integrovat RealtimeBoard s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="74624-104">In this tutorial, you learn how to integrate RealtimeBoard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="74624-105">Integrace RealtimeBoard s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="74624-105">Integrating RealtimeBoard with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="74624-106">Můžete ovládat ve službě Azure AD, který má přístup k RealtimeBoard.</span><span class="sxs-lookup"><span data-stu-id="74624-106">You can control in Azure AD who has access to RealtimeBoard.</span></span>
- <span data-ttu-id="74624-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k RealtimeBoard (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74624-107">You can enable your users to automatically get signed-on to RealtimeBoard (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="74624-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="74624-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="74624-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="74624-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74624-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74624-110">Prerequisites</span></span>

<span data-ttu-id="74624-111">Konfigurace integrace Azure AD s RealtimeBoard, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="74624-111">To configure Azure AD integration with RealtimeBoard, you need the following items:</span></span>

- <span data-ttu-id="74624-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="74624-112">An Azure AD subscription</span></span>
- <span data-ttu-id="74624-113">RealtimeBoard jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="74624-113">A RealtimeBoard single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="74624-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="74624-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="74624-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="74624-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="74624-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="74624-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="74624-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="74624-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="74624-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="74624-118">Scenario description</span></span>
<span data-ttu-id="74624-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="74624-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="74624-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="74624-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="74624-121">Přidání RealtimeBoard z Galerie</span><span class="sxs-lookup"><span data-stu-id="74624-121">Adding RealtimeBoard from the gallery</span></span>
2. <span data-ttu-id="74624-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="74624-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-realtimeboard-from-the-gallery"></a><span data-ttu-id="74624-123">Přidání RealtimeBoard z Galerie</span><span class="sxs-lookup"><span data-stu-id="74624-123">Adding RealtimeBoard from the gallery</span></span>
<span data-ttu-id="74624-124">Při konfiguraci integrace RealtimeBoard do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS RealtimeBoard z galerie.</span><span class="sxs-lookup"><span data-stu-id="74624-124">To configure the integration of RealtimeBoard into Azure AD, you need to add RealtimeBoard from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="74624-125">**Pokud chcete přidat RealtimeBoard z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="74624-125">**To add RealtimeBoard from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="74624-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="74624-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="74624-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="74624-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="74624-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="74624-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="74624-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74624-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="74624-133">Do vyhledávacího pole zadejte **RealtimeBoard**, vyberte **RealtimeBoard** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74624-133">In the search box, type **RealtimeBoard**, select **RealtimeBoard** from result panel then click **Add** button to add the application.</span></span>

    ![RealtimeBoard v seznamu výsledků](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="74624-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="74624-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="74624-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s RealtimeBoard podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="74624-136">In this section, you configure and test Azure AD single sign-on with RealtimeBoard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="74624-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v RealtimeBoard je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74624-137">For single sign-on to work, Azure AD needs to know what the counterpart user in RealtimeBoard is to a user in Azure AD.</span></span> <span data-ttu-id="74624-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v RealtimeBoard musí navázat.</span><span class="sxs-lookup"><span data-stu-id="74624-138">In other words, a link relationship between an Azure AD user and the related user in RealtimeBoard needs to be established.</span></span>

<span data-ttu-id="74624-139">V RealtimeBoard, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="74624-139">In RealtimeBoard, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="74624-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s RealtimeBoard, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="74624-140">To configure and test Azure AD single sign-on with RealtimeBoard, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="74624-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="74624-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="74624-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74624-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="74624-143">**[Vytvoření zkušebního uživatele RealtimeBoard](#create-a-realtimeboard-test-user)**  – Pokud chcete mít protějšek Britta Simon v RealtimeBoard propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="74624-143">**[Create a RealtimeBoard test user](#create-a-realtimeboard-test-user)** - to have a counterpart of Britta Simon in RealtimeBoard that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="74624-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="74624-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="74624-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="74624-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="74624-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="74624-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="74624-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci RealtimeBoard.</span><span class="sxs-lookup"><span data-stu-id="74624-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RealtimeBoard application.</span></span>

<span data-ttu-id="74624-148">**Ke konfiguraci Azure AD jednotné přihlašování s RealtimeBoard, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="74624-148">**To configure Azure AD single sign-on with RealtimeBoard, perform the following steps:**</span></span>

1. <span data-ttu-id="74624-149">Na portálu Azure na **RealtimeBoard** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="74624-149">In the Azure portal, on the **RealtimeBoard** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="74624-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="74624-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_samlbase.png)

3. <span data-ttu-id="74624-153">Na **RealtimeBoard domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="74624-153">On the **RealtimeBoard Domain and URLs** section, if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![RealtimeBoard domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_url.png)

    <span data-ttu-id="74624-155">V **identifikátor** textovému poli, zadejte adresu URL jako:`https://realtimeboard.com/`</span><span class="sxs-lookup"><span data-stu-id="74624-155">In the **Identifier** textbox, type a URL as: `https://realtimeboard.com/`</span></span>

4. <span data-ttu-id="74624-156">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="74624-156">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_url2.png)

    <span data-ttu-id="74624-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://realtimeboard.com/sso/saml`</span><span class="sxs-lookup"><span data-stu-id="74624-158">In the **Sign-on URL** textbox, type a URL as: `https://realtimeboard.com/sso/saml`</span></span>

5. <span data-ttu-id="74624-159">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="74624-159">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_certificate.png) 

6. <span data-ttu-id="74624-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74624-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="74624-163">Konfigurace jednotného přihlašování na **RealtimeBoard** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory RealtimeBoard](mailto:support@realtimeboard.com).</span><span class="sxs-lookup"><span data-stu-id="74624-163">To configure single sign-on on **RealtimeBoard** side, you need to send the downloaded **Metadata XML** to [RealtimeBoard support team](mailto:support@realtimeboard.com).</span></span> <span data-ttu-id="74624-164">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="74624-164">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="74624-165">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="74624-165">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="74624-166">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="74624-166">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="74624-167">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="74624-167">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="74624-168">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="74624-168">Create an Azure AD test user</span></span>

<span data-ttu-id="74624-169">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74624-169">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="74624-171">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="74624-171">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="74624-172">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74624-172">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="74624-174">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="74624-174">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="74624-176">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74624-176">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="74624-178">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="74624-178">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-realtimeboard-tutorial/create_aaduser_04.png)

    <span data-ttu-id="74624-180">a.</span><span class="sxs-lookup"><span data-stu-id="74624-180">a.</span></span> <span data-ttu-id="74624-181">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="74624-181">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="74624-182">b.</span><span class="sxs-lookup"><span data-stu-id="74624-182">b.</span></span> <span data-ttu-id="74624-183">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74624-183">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="74624-184">c.</span><span class="sxs-lookup"><span data-stu-id="74624-184">c.</span></span> <span data-ttu-id="74624-185">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="74624-185">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="74624-186">d.</span><span class="sxs-lookup"><span data-stu-id="74624-186">d.</span></span> <span data-ttu-id="74624-187">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="74624-187">Click **Create**.</span></span>
 
### <a name="create-a-realtimeboard-test-user"></a><span data-ttu-id="74624-188">Vytvoření zkušebního uživatele RealtimeBoard</span><span class="sxs-lookup"><span data-stu-id="74624-188">Create a RealtimeBoard test user</span></span>

<span data-ttu-id="74624-189">Cílem této části je vytvoření uživatele v RealtimeBoard nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74624-189">The objective of this section is to create a user called Britta Simon in RealtimeBoard.</span></span> <span data-ttu-id="74624-190">RealtimeBoard podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="74624-190">RealtimeBoard supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="74624-191">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="74624-191">There is no action item for you in this section.</span></span> <span data-ttu-id="74624-192">Pokud uživatel v RealtimeBoard ještě neexistuje, vytvoří se při pokusu o přístup k RealtimeBoard novou.</span><span class="sxs-lookup"><span data-stu-id="74624-192">If a user doesn't already exist in RealtimeBoard, a new one is created when you attempt to access RealtimeBoard.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="74624-193">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="74624-193">Assign the Azure AD test user</span></span>

<span data-ttu-id="74624-194">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu RealtimeBoard.</span><span class="sxs-lookup"><span data-stu-id="74624-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RealtimeBoard.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="74624-196">**Pokud chcete přiřadit Britta Simon RealtimeBoard, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="74624-196">**To assign Britta Simon to RealtimeBoard, perform the following steps:**</span></span>

1. <span data-ttu-id="74624-197">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="74624-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="74624-199">V seznamu aplikací vyberte **RealtimeBoard**.</span><span class="sxs-lookup"><span data-stu-id="74624-199">In the applications list, select **RealtimeBoard**.</span></span>

    ![V seznamu aplikací na RealtimeBoard odkaz](./media/active-directory-saas-realtimeboard-tutorial/tutorial_realtimeboard_app.png)  

3. <span data-ttu-id="74624-201">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="74624-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="74624-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="74624-203">Click **Add** button.</span></span> <span data-ttu-id="74624-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74624-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="74624-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="74624-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="74624-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74624-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="74624-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="74624-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="74624-209">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="74624-209">Test single sign-on</span></span>

<span data-ttu-id="74624-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="74624-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="74624-211">Když kliknete na dlaždici RealtimeBoard na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci RealtimeBoard.</span><span class="sxs-lookup"><span data-stu-id="74624-211">When you click the RealtimeBoard tile in the Access Panel, you should get automatically signed-on to your RealtimeBoard application.</span></span>
<span data-ttu-id="74624-212">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="74624-212">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="74624-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="74624-213">Additional resources</span></span>

* [<span data-ttu-id="74624-214">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="74624-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="74624-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="74624-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-realtimeboard-tutorial/tutorial_general_203.png

