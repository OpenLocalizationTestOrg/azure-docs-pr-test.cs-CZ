---
title: "Kurz: Azure Active Directory integrace s skleníkových | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a skleníkových."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: d3aba4aab8ded8749db2bf8197f57a6763008c60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a><span data-ttu-id="d913f-103">Kurz: Azure Active Directory integrace s skleníkových</span><span class="sxs-lookup"><span data-stu-id="d913f-103">Tutorial: Azure Active Directory integration with Greenhouse</span></span>

<span data-ttu-id="d913f-104">V tomto kurzu zjistěte, jak integrovat skleníkových s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d913f-104">In this tutorial, you learn how to integrate Greenhouse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d913f-105">Integrace skleníkových s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d913f-105">Integrating Greenhouse with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d913f-106">Můžete ovládat ve službě Azure AD, kdo má přístup k skleníkových.</span><span class="sxs-lookup"><span data-stu-id="d913f-106">You can control in Azure AD who has access to Greenhouse.</span></span>
- <span data-ttu-id="d913f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k skleníkových (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d913f-107">You can enable your users to automatically get signed-on to Greenhouse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d913f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d913f-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="d913f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d913f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d913f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d913f-110">Prerequisites</span></span>

<span data-ttu-id="d913f-111">Konfigurace integrace Azure AD s skleníkových, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d913f-111">To configure Azure AD integration with Greenhouse, you need the following items:</span></span>

- <span data-ttu-id="d913f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d913f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d913f-113">Skleníkových jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d913f-113">A Greenhouse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d913f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d913f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d913f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d913f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d913f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d913f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d913f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d913f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d913f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d913f-118">Scenario description</span></span>
<span data-ttu-id="d913f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d913f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d913f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d913f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d913f-121">Přidání skleníkových z Galerie</span><span class="sxs-lookup"><span data-stu-id="d913f-121">Adding Greenhouse from the gallery</span></span>
2. <span data-ttu-id="d913f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d913f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-greenhouse-from-the-gallery"></a><span data-ttu-id="d913f-123">Přidání skleníkových z Galerie</span><span class="sxs-lookup"><span data-stu-id="d913f-123">Adding Greenhouse from the gallery</span></span>
<span data-ttu-id="d913f-124">Při konfiguraci integrace skleníkových do služby Azure AD, potřebujete přidat skleníkových z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d913f-124">To configure the integration of Greenhouse into Azure AD, you need to add Greenhouse from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d913f-125">**Pokud chcete přidat skleníkových z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d913f-125">**To add Greenhouse from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d913f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d913f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="d913f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d913f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d913f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d913f-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="d913f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d913f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="d913f-133">Do vyhledávacího pole zadejte **skleníkových**, vyberte **skleníkových** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d913f-133">In the search box, type **Greenhouse**, select **Greenhouse** from result panel then click **Add** button to add the application.</span></span>

    ![Skleníkových v seznamu výsledků](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d913f-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d913f-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d913f-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s skleníkových podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d913f-136">In this section, you configure and test Azure AD single sign-on with Greenhouse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d913f-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v skleníkových je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d913f-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Greenhouse is to a user in Azure AD.</span></span> <span data-ttu-id="d913f-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v skleníkových musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d913f-138">In other words, a link relationship between an Azure AD user and the related user in Greenhouse needs to be established.</span></span>

<span data-ttu-id="d913f-139">V skleníkových, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="d913f-139">In Greenhouse, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d913f-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s skleníkových, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d913f-140">To configure and test Azure AD single sign-on with Greenhouse, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d913f-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d913f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d913f-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d913f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d913f-143">**[Vytvoření zkušebního uživatele skleníkových](#create-a-greenhouse-test-user)**  – Pokud chcete mít protějšek Britta Simon v skleníkových propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="d913f-143">**[Create a Greenhouse test user](#create-a-greenhouse-test-user)** - to have a counterpart of Britta Simon in Greenhouse that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d913f-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d913f-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d913f-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d913f-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d913f-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d913f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d913f-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci skleníkových.</span><span class="sxs-lookup"><span data-stu-id="d913f-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Greenhouse application.</span></span>

<span data-ttu-id="d913f-148">**Ke konfiguraci Azure AD jednotné přihlašování s skleníkových, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d913f-148">**To configure Azure AD single sign-on with Greenhouse, perform the following steps:**</span></span>

1. <span data-ttu-id="d913f-149">Na portálu Azure na **skleníkových** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d913f-149">In the Azure portal, on the **Greenhouse** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="d913f-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d913f-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_samlbase.png)

3. <span data-ttu-id="d913f-153">Na **skleníkových domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d913f-153">On the **Greenhouse Domain and URLs** section, perform the following steps:</span></span>

    ![Skleníkových domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_url.png)

    <span data-ttu-id="d913f-155">a.</span><span class="sxs-lookup"><span data-stu-id="d913f-155">a.</span></span> <span data-ttu-id="d913f-156">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="d913f-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.greenhouse.io`</span></span>

    <span data-ttu-id="d913f-157">b.</span><span class="sxs-lookup"><span data-stu-id="d913f-157">b.</span></span> <span data-ttu-id="d913f-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="d913f-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.greenhouse.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d913f-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="d913f-159">These values are not real.</span></span> <span data-ttu-id="d913f-160">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="d913f-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d913f-161">Obraťte se na [tým podpory skleníkových klienta](https://www.greenhouse.io/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="d913f-161">Contact [Greenhouse Client support team](https://www.greenhouse.io/contact) to get these values.</span></span> 
 


4. <span data-ttu-id="d913f-162">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d913f-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_certificate.png) 

5. <span data-ttu-id="d913f-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d913f-164">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-greenhouse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d913f-166">Konfigurace jednotného přihlašování na **skleníkových** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory skleníkových](http://www.greenhouse.io/contact).</span><span class="sxs-lookup"><span data-stu-id="d913f-166">To configure single sign-on on **Greenhouse** side, you need to send the downloaded **Metadata XML** to [Greenhouse support team](http://www.greenhouse.io/contact).</span></span>

> [!TIP]
> <span data-ttu-id="d913f-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d913f-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d913f-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d913f-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d913f-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d913f-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d913f-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d913f-170">Create an Azure AD test user</span></span>

<span data-ttu-id="d913f-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d913f-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="d913f-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d913f-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d913f-174">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d913f-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d913f-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d913f-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d913f-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d913f-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d913f-180">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d913f-180">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d913f-182">a.</span><span class="sxs-lookup"><span data-stu-id="d913f-182">a.</span></span> <span data-ttu-id="d913f-183">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d913f-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d913f-184">b.</span><span class="sxs-lookup"><span data-stu-id="d913f-184">b.</span></span> <span data-ttu-id="d913f-185">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d913f-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="d913f-186">c.</span><span class="sxs-lookup"><span data-stu-id="d913f-186">c.</span></span> <span data-ttu-id="d913f-187">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="d913f-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="d913f-188">d.</span><span class="sxs-lookup"><span data-stu-id="d913f-188">d.</span></span> <span data-ttu-id="d913f-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d913f-189">Click **Create**.</span></span>
 
### <a name="create-a-greenhouse-test-user"></a><span data-ttu-id="d913f-190">Vytvoření zkušebního uživatele skleníkových</span><span class="sxs-lookup"><span data-stu-id="d913f-190">Create a Greenhouse test user</span></span>

<span data-ttu-id="d913f-191">Pokud chcete povolit uživatelům Azure AD přihlášení do skleníkových, musí být zřízená do skleníkových.</span><span class="sxs-lookup"><span data-stu-id="d913f-191">In order to enable Azure AD users to log into Greenhouse, they must be provisioned into Greenhouse.</span></span> <span data-ttu-id="d913f-192">V případě skleníkových zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d913f-192">In the case of Greenhouse, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="d913f-193">Můžete použít všechny ostatní skleníkových uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované skleníkových zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="d913f-193">You can use any other Greenhouse user account creation tools or APIs provided by Greenhouse to provision AAD user accounts.</span></span> 

<span data-ttu-id="d913f-194">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d913f-194">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="d913f-195">Přihlaste se k vaší **skleníkových** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="d913f-195">Log in to your **Greenhouse** company site as an administrator.</span></span>

2. <span data-ttu-id="d913f-196">V nabídce v horní části, klikněte na tlačítko **konfigurace**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d913f-196">In the menu on the top, click **Configure**, and then click **Users**.</span></span>
   
   <span data-ttu-id="d913f-197">![Uživatelé](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="d913f-197">![Users](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "Users")</span></span>

3. <span data-ttu-id="d913f-198">Klikněte na tlačítko **noví uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d913f-198">Click **New Users**.</span></span>
   
   <span data-ttu-id="d913f-199">![Nový uživatel](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="d913f-199">![New User](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "New User")</span></span>

4. <span data-ttu-id="d913f-200">V **přidat nové uživatele** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d913f-200">In the **Add New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="d913f-201">![Přidání nového uživatele](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "přidat nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="d913f-201">![Add New User](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "Add New User")</span></span>

   <span data-ttu-id="d913f-202">a.</span><span class="sxs-lookup"><span data-stu-id="d913f-202">a.</span></span> <span data-ttu-id="d913f-203">V **zadejte e-mailů uživatele** textovému poli, zadejte e-mailovou adresu chcete zřídit platný účet služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d913f-203">In the **Enter user emails** textbox, type the email address of a valid Azure Active Directory account you want to provision.</span></span>

   <span data-ttu-id="d913f-204">b.</span><span class="sxs-lookup"><span data-stu-id="d913f-204">b.</span></span> <span data-ttu-id="d913f-205">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d913f-205">Click **Save**.</span></span>    
   
      >[!NOTE]
      ><span data-ttu-id="d913f-206">Držiteli účtu Azure Active Directory obdrží e-mail zahrnutím odkazu pro potvrzení účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="d913f-206">The Azure Active Directory account holders will receive an email including a link to confirm the account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="d913f-207">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d913f-207">Assign the Azure AD test user</span></span>

<span data-ttu-id="d913f-208">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu skleníkových.</span><span class="sxs-lookup"><span data-stu-id="d913f-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Greenhouse.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="d913f-210">**Pokud chcete přiřadit Britta Simon skleníkových, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d913f-210">**To assign Britta Simon to Greenhouse, perform the following steps:**</span></span>

1. <span data-ttu-id="d913f-211">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d913f-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d913f-213">V seznamu aplikací vyberte **skleníkových**.</span><span class="sxs-lookup"><span data-stu-id="d913f-213">In the applications list, select **Greenhouse**.</span></span>

    ![V seznamu aplikací na skleníkových odkaz](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_app.png)  

3. <span data-ttu-id="d913f-215">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d913f-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="d913f-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d913f-217">Click **Add** button.</span></span> <span data-ttu-id="d913f-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d913f-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="d913f-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d913f-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d913f-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d913f-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d913f-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d913f-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d913f-223">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d913f-223">Test single sign-on</span></span>

<span data-ttu-id="d913f-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d913f-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d913f-225">Když kliknete na dlaždici skleníkových na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci skleníkových.</span><span class="sxs-lookup"><span data-stu-id="d913f-225">When you click the Greenhouse tile in the Access Panel, you should get automatically signed-on to your Greenhouse application.</span></span>
<span data-ttu-id="d913f-226">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d913f-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d913f-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d913f-227">Additional resources</span></span>

* [<span data-ttu-id="d913f-228">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d913f-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d913f-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d913f-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_203.png

