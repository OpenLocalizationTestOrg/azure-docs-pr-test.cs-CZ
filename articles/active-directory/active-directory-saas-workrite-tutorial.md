---
title: 'Kurz: Azure Active Directory integrace s Workrite | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Workrite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2a5c2956-a011-4d5c-877b-80679b6587b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 4358c4c621634c17cbbd7fa1c72f12746b8e4a2a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workrite"></a><span data-ttu-id="c0004-103">Kurz: Azure Active Directory integrace s Workrite</span><span class="sxs-lookup"><span data-stu-id="c0004-103">Tutorial: Azure Active Directory integration with Workrite</span></span>

<span data-ttu-id="c0004-104">V tomto kurzu zjistěte, jak integrovat Workrite s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c0004-104">In this tutorial, you learn how to integrate Workrite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c0004-105">Integrace Workrite s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c0004-105">Integrating Workrite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c0004-106">Můžete ovládat ve službě Azure AD, který má přístup k Workrite.</span><span class="sxs-lookup"><span data-stu-id="c0004-106">You can control in Azure AD who has access to Workrite.</span></span>
- <span data-ttu-id="c0004-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Workrite (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c0004-107">You can enable your users to automatically get signed-on to Workrite (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c0004-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0004-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="c0004-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c0004-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0004-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0004-110">Prerequisites</span></span>

<span data-ttu-id="c0004-111">Konfigurace integrace Azure AD s Workrite, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c0004-111">To configure Azure AD integration with Workrite, you need the following items:</span></span>

- <span data-ttu-id="c0004-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0004-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c0004-113">Workrite jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c0004-113">A Workrite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c0004-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0004-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c0004-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c0004-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c0004-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c0004-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c0004-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c0004-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c0004-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c0004-118">Scenario description</span></span>
<span data-ttu-id="c0004-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0004-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c0004-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c0004-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c0004-121">Přidání Workrite z Galerie</span><span class="sxs-lookup"><span data-stu-id="c0004-121">Adding Workrite from the gallery</span></span>
2. <span data-ttu-id="c0004-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0004-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workrite-from-the-gallery"></a><span data-ttu-id="c0004-123">Přidání Workrite z Galerie</span><span class="sxs-lookup"><span data-stu-id="c0004-123">Adding Workrite from the gallery</span></span>
<span data-ttu-id="c0004-124">Při konfiguraci integrace Workrite do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Workrite z galerie.</span><span class="sxs-lookup"><span data-stu-id="c0004-124">To configure the integration of Workrite into Azure AD, you need to add Workrite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c0004-125">**Pokud chcete přidat Workrite z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0004-125">**To add Workrite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c0004-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c0004-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="c0004-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c0004-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c0004-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0004-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="c0004-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0004-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="c0004-133">Do vyhledávacího pole zadejte **Workrite**, vyberte **Workrite** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c0004-133">In the search box, type **Workrite**, select **Workrite** from result panel then click **Add** button to add the application.</span></span>

    ![Workrite v seznamu výsledků](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c0004-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0004-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c0004-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workrite podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c0004-136">In this section, you configure and test Azure AD single sign-on with Workrite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c0004-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Workrite je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c0004-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Workrite is to a user in Azure AD.</span></span> <span data-ttu-id="c0004-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Workrite musí navázat.</span><span class="sxs-lookup"><span data-stu-id="c0004-138">In other words, a link relationship between an Azure AD user and the related user in Workrite needs to be established.</span></span>

<span data-ttu-id="c0004-139">V Workrite, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c0004-139">In Workrite, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c0004-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workrite, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c0004-140">To configure and test Azure AD single sign-on with Workrite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c0004-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c0004-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c0004-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0004-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c0004-143">**[Vytvoření zkušebního uživatele Workrite](#create-a-workrite-test-user)**  – Pokud chcete mít protějšek Britta Simon v Workrite propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c0004-143">**[Create a Workrite test user](#create-a-workrite-test-user)** - to have a counterpart of Britta Simon in Workrite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c0004-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0004-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c0004-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c0004-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c0004-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0004-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c0004-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Workrite.</span><span class="sxs-lookup"><span data-stu-id="c0004-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workrite application.</span></span>

<span data-ttu-id="c0004-148">**Ke konfiguraci Azure AD jednotné přihlašování s Workrite, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0004-148">**To configure Azure AD single sign-on with Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="c0004-149">Na portálu Azure na **Workrite** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c0004-149">In the Azure portal, on the **Workrite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="c0004-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0004-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_samlbase.png)

3. <span data-ttu-id="c0004-153">Na **Workrite domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c0004-153">On the **Workrite Domain and URLs** section, perform the following steps:</span></span>

    ![Workrite domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_url.png)

    <span data-ttu-id="c0004-155">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="c0004-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c0004-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="c0004-156">This value is not real.</span></span> <span data-ttu-id="c0004-157">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0004-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="c0004-158">Obraťte se na [tým podpory Workrite klienta](mailto:support@workrite.co.uk) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c0004-158">Contact [Workrite Client support team](mailto:support@workrite.co.uk) to get this value.</span></span>

4. <span data-ttu-id="c0004-159">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="c0004-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_certificate.png) 

5. <span data-ttu-id="c0004-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0004-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-workrite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c0004-163">Na **Workrite konfigurace** klikněte na tlačítko **konfigurace Workrite** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c0004-163">On the **Workrite Configuration** section, click **Configure Workrite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c0004-164">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c0004-164">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace Workrite](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_configure.png) 

7. <span data-ttu-id="c0004-166">Konfigurace jednotného přihlašování na **Workrite** straně, budete muset odeslat stažené **Certificate(Base64), adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [Workrite tým podpory](mailto:support@workrite.co.uk).</span><span class="sxs-lookup"><span data-stu-id="c0004-166">To configure single sign-on on **Workrite** side, you need to send the downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Workrite support team](mailto:support@workrite.co.uk).</span></span>

> [!TIP]
> <span data-ttu-id="c0004-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c0004-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c0004-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c0004-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c0004-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c0004-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c0004-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0004-170">Create an Azure AD test user</span></span>

<span data-ttu-id="c0004-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0004-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="c0004-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0004-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c0004-174">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0004-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-workrite-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c0004-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c0004-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-workrite-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c0004-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0004-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-workrite-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c0004-180">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c0004-180">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-workrite-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c0004-182">a.</span><span class="sxs-lookup"><span data-stu-id="c0004-182">a.</span></span> <span data-ttu-id="c0004-183">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c0004-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c0004-184">b.</span><span class="sxs-lookup"><span data-stu-id="c0004-184">b.</span></span> <span data-ttu-id="c0004-185">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0004-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="c0004-186">c.</span><span class="sxs-lookup"><span data-stu-id="c0004-186">c.</span></span> <span data-ttu-id="c0004-187">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="c0004-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="c0004-188">d.</span><span class="sxs-lookup"><span data-stu-id="c0004-188">d.</span></span> <span data-ttu-id="c0004-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c0004-189">Click **Create**.</span></span>
 
### <a name="create-a-workrite-test-user"></a><span data-ttu-id="c0004-190">Vytvoření zkušebního uživatele Workrite</span><span class="sxs-lookup"><span data-stu-id="c0004-190">Create a Workrite test user</span></span>

<span data-ttu-id="c0004-191">Cílem této části je vytvoření uživatele v Workrite nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0004-191">The objective of this section is to create a user called Britta Simon in Workrite.</span></span>

<span data-ttu-id="c0004-192">**Vytvoření uživatele v Workrite nazývá Britta Simon, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0004-192">**To create a user called Britta Simon in Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="c0004-193">Přihlásit se k serveru vaší společnosti workrite jako správce.</span><span class="sxs-lookup"><span data-stu-id="c0004-193">Sign on to your workrite company site as administrator.</span></span>

2. <span data-ttu-id="c0004-194">V navigačním podokně klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="c0004-194">In the navigation pane, click **Admin**.</span></span>
   
    ![Správce řízení][400]

3. <span data-ttu-id="c0004-196">Přejděte na rychlé odkazy a potom klikněte na **vytvořte uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c0004-196">Go to Quick Links, and then click **Create a User**.</span></span>
   
    ![Vytvořit oddíl uživatele][401]

4. <span data-ttu-id="c0004-198">Na **vytvořit uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c0004-198">On the **Create User** dialog, perform the following steps:</span></span>
   
    ![Vytvořit uživatele Dailog][402]
    
    <span data-ttu-id="c0004-200">a.</span><span class="sxs-lookup"><span data-stu-id="c0004-200">a.</span></span> <span data-ttu-id="c0004-201">V **e-mailu** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="c0004-201">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="c0004-202">b.</span><span class="sxs-lookup"><span data-stu-id="c0004-202">b.</span></span> <span data-ttu-id="c0004-203">V **křestní jméno** textovému poli, zadejte jméno uživatele jako Britta.</span><span class="sxs-lookup"><span data-stu-id="c0004-203">In the **First Name** textbox, type the firstname of user like Britta.</span></span>

    <span data-ttu-id="c0004-204">c.</span><span class="sxs-lookup"><span data-stu-id="c0004-204">c.</span></span> <span data-ttu-id="c0004-205">V **Přezdívka** textovému poli, zadejte příjmení uživatele jako Simon.</span><span class="sxs-lookup"><span data-stu-id="c0004-205">In the **Surname** textbox, type the surname of user like Simon.</span></span>
    
    <span data-ttu-id="c0004-206">d.</span><span class="sxs-lookup"><span data-stu-id="c0004-206">d.</span></span> <span data-ttu-id="c0004-207">Vyberte **správce klienta** jako **zvolte roli**.</span><span class="sxs-lookup"><span data-stu-id="c0004-207">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="c0004-208">e.</span><span class="sxs-lookup"><span data-stu-id="c0004-208">e.</span></span> <span data-ttu-id="c0004-209">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c0004-209">Click **Save**.</span></span>   

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c0004-210">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0004-210">Assign the Azure AD test user</span></span>

<span data-ttu-id="c0004-211">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Workrite.</span><span class="sxs-lookup"><span data-stu-id="c0004-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workrite.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="c0004-213">**Pokud chcete přiřadit Britta Simon Workrite, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c0004-213">**To assign Britta Simon to Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="c0004-214">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0004-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c0004-216">V seznamu aplikací vyberte **Workrite**.</span><span class="sxs-lookup"><span data-stu-id="c0004-216">In the applications list, select **Workrite**.</span></span>

    ![V seznamu aplikací na Workrite odkaz](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_app.png)  

3. <span data-ttu-id="c0004-218">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c0004-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="c0004-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0004-220">Click **Add** button.</span></span> <span data-ttu-id="c0004-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0004-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="c0004-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c0004-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c0004-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0004-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c0004-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0004-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c0004-226">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0004-226">Test single sign-on</span></span>

<span data-ttu-id="c0004-227">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="c0004-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="c0004-228">Když kliknete na dlaždici Workrite na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Workrite.</span><span class="sxs-lookup"><span data-stu-id="c0004-228">When you click the Workrite tile in the Access Panel, you should get automatically signed-on to your Workrite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0004-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c0004-229">Additional resources</span></span>

* [<span data-ttu-id="c0004-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c0004-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c0004-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c0004-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_402.png

