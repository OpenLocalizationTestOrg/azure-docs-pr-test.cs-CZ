---
title: 'Kurz: Azure Active Directory integrace s ZIVVER | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ZIVVER."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 64cb7ea0-df6c-4963-84d8-6f435980e2de
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d27c773baaae64922fc8ad2c8f65bab177769f67
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zivver"></a><span data-ttu-id="eeb42-103">Kurz: Azure Active Directory integrace s ZIVVER</span><span class="sxs-lookup"><span data-stu-id="eeb42-103">Tutorial: Azure Active Directory integration with ZIVVER</span></span>

<span data-ttu-id="eeb42-104">V tomto kurzu zjistěte, jak integrovat ZIVVER s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="eeb42-104">In this tutorial, you learn how to integrate ZIVVER with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eeb42-105">Integrace ZIVVER s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="eeb42-105">Integrating ZIVVER with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="eeb42-106">Můžete ovládat ve službě Azure AD, který má přístup k ZIVVER.</span><span class="sxs-lookup"><span data-stu-id="eeb42-106">You can control in Azure AD who has access to ZIVVER.</span></span>
- <span data-ttu-id="eeb42-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ZIVVER (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eeb42-107">You can enable your users to automatically get signed-on to ZIVVER (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="eeb42-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="eeb42-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="eeb42-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eeb42-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eeb42-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eeb42-110">Prerequisites</span></span>

<span data-ttu-id="eeb42-111">Konfigurace integrace Azure AD s ZIVVER, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="eeb42-111">To configure Azure AD integration with ZIVVER, you need the following items:</span></span>

- <span data-ttu-id="eeb42-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="eeb42-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eeb42-113">ZIVVER jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="eeb42-113">A ZIVVER single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eeb42-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="eeb42-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eeb42-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="eeb42-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eeb42-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="eeb42-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eeb42-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eeb42-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eeb42-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="eeb42-118">Scenario description</span></span>
<span data-ttu-id="eeb42-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="eeb42-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eeb42-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="eeb42-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eeb42-121">Přidání ZIVVER z Galerie</span><span class="sxs-lookup"><span data-stu-id="eeb42-121">Adding ZIVVER from the gallery</span></span>
2. <span data-ttu-id="eeb42-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="eeb42-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zivver-from-the-gallery"></a><span data-ttu-id="eeb42-123">Přidání ZIVVER z Galerie</span><span class="sxs-lookup"><span data-stu-id="eeb42-123">Adding ZIVVER from the gallery</span></span>
<span data-ttu-id="eeb42-124">Při konfiguraci integrace ZIVVER do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS ZIVVER z galerie.</span><span class="sxs-lookup"><span data-stu-id="eeb42-124">To configure the integration of ZIVVER into Azure AD, you need to add ZIVVER from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="eeb42-125">**Pokud chcete přidat ZIVVER z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eeb42-125">**To add ZIVVER from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="eeb42-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="eeb42-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="eeb42-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="eeb42-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="eeb42-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eeb42-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="eeb42-133">Do vyhledávacího pole zadejte **ZIVVER**, vyberte **ZIVVER** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="eeb42-133">In the search box, type **ZIVVER**, select **ZIVVER** from result panel then click **Add** button to add the application.</span></span>

    ![ZIVVER v seznamu výsledků](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="eeb42-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eeb42-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="eeb42-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ZIVVER podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="eeb42-136">In this section, you configure and test Azure AD single sign-on with ZIVVER based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="eeb42-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ZIVVER je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eeb42-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ZIVVER is to a user in Azure AD.</span></span> <span data-ttu-id="eeb42-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ZIVVER musí navázat.</span><span class="sxs-lookup"><span data-stu-id="eeb42-138">In other words, a link relationship between an Azure AD user and the related user in ZIVVER needs to be established.</span></span>

<span data-ttu-id="eeb42-139">V ZIVVER, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="eeb42-139">In ZIVVER, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="eeb42-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ZIVVER, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="eeb42-140">To configure and test Azure AD single sign-on with ZIVVER, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="eeb42-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="eeb42-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="eeb42-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eeb42-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eeb42-143">**[Vytvoření zkušebního uživatele ZIVVER](#create-a-zivver-test-user)**  – Pokud chcete mít protějšek Britta Simon v ZIVVER propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="eeb42-143">**[Create a ZIVVER test user](#create-a-zivver-test-user)** - to have a counterpart of Britta Simon in ZIVVER that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="eeb42-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eeb42-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eeb42-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="eeb42-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="eeb42-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eeb42-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="eeb42-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ZIVVER.</span><span class="sxs-lookup"><span data-stu-id="eeb42-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ZIVVER application.</span></span>

<span data-ttu-id="eeb42-148">**Ke konfiguraci Azure AD jednotné přihlašování s ZIVVER, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eeb42-148">**To configure Azure AD single sign-on with ZIVVER, perform the following steps:**</span></span>

1. <span data-ttu-id="eeb42-149">Na portálu Azure na **ZIVVER** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-149">In the Azure portal, on the **ZIVVER** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="eeb42-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eeb42-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_samlbase.png)

3. <span data-ttu-id="eeb42-153">Na **ZIVVER domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eeb42-153">On the **ZIVVER Domain and URLs** section, perform the following steps:</span></span>

    ![ZIVVER domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_url.png)

    <span data-ttu-id="eeb42-155">V **identifikátor** textovému poli, zadejte adresu URL:`https://app.zivver.com/SAML/Zivver`</span><span class="sxs-lookup"><span data-stu-id="eeb42-155">In the **Identifier** textbox, type the URL: `https://app.zivver.com/SAML/Zivver`</span></span>

4. <span data-ttu-id="eeb42-156">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="eeb42-156">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_certificate.png) 

5. <span data-ttu-id="eeb42-158">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eeb42-158">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-zivver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="eeb42-160">Konfigurace jednotného přihlašování na **ZIVVER** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory ZIVVER](https://support.zivver.com).</span><span class="sxs-lookup"><span data-stu-id="eeb42-160">To configure single sign-on on **ZIVVER** side, you need to send the downloaded **Metadata XML** to [ZIVVER support team](https://support.zivver.com).</span></span> <span data-ttu-id="eeb42-161">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="eeb42-161">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="eeb42-162">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="eeb42-162">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="eeb42-163">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="eeb42-163">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="eeb42-164">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="eeb42-164">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="eeb42-165">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="eeb42-165">Create an Azure AD test user</span></span>

<span data-ttu-id="eeb42-166">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eeb42-166">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="eeb42-168">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eeb42-168">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="eeb42-169">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eeb42-169">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-zivver-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="eeb42-171">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-171">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-zivver-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="eeb42-173">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eeb42-173">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-zivver-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="eeb42-175">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eeb42-175">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-zivver-tutorial/create_aaduser_04.png)

    <span data-ttu-id="eeb42-177">a.</span><span class="sxs-lookup"><span data-stu-id="eeb42-177">a.</span></span> <span data-ttu-id="eeb42-178">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-178">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eeb42-179">b.</span><span class="sxs-lookup"><span data-stu-id="eeb42-179">b.</span></span> <span data-ttu-id="eeb42-180">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eeb42-180">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="eeb42-181">c.</span><span class="sxs-lookup"><span data-stu-id="eeb42-181">c.</span></span> <span data-ttu-id="eeb42-182">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="eeb42-182">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="eeb42-183">d.</span><span class="sxs-lookup"><span data-stu-id="eeb42-183">d.</span></span> <span data-ttu-id="eeb42-184">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-184">Click **Create**.</span></span>
  
### <a name="create-a-zivver-test-user"></a><span data-ttu-id="eeb42-185">Vytvoření zkušebního uživatele ZIVVER</span><span class="sxs-lookup"><span data-stu-id="eeb42-185">Create a ZIVVER test user</span></span>

<span data-ttu-id="eeb42-186">V této části vytvoříte volal Britta Simon v ZIVVER uživatele.</span><span class="sxs-lookup"><span data-stu-id="eeb42-186">In this section, you create a user called Britta Simon in ZIVVER.</span></span> <span data-ttu-id="eeb42-187">Práce s [tým podpory ZIVVER](https://support.zivver.com) přidat uživatele do ZIVVER platformy.</span><span class="sxs-lookup"><span data-stu-id="eeb42-187">Work with [ZIVVER support team](https://support.zivver.com) to add the users in the ZIVVER platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="eeb42-188">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="eeb42-188">Assign the Azure AD test user</span></span>

<span data-ttu-id="eeb42-189">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ZIVVER.</span><span class="sxs-lookup"><span data-stu-id="eeb42-189">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ZIVVER.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="eeb42-191">**Pokud chcete přiřadit Britta Simon ZIVVER, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="eeb42-191">**To assign Britta Simon to ZIVVER, perform the following steps:**</span></span>

1. <span data-ttu-id="eeb42-192">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-192">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="eeb42-194">V seznamu aplikací vyberte **ZIVVER**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-194">In the applications list, select **ZIVVER**.</span></span>

    ![V seznamu aplikací na ZIVVER odkaz](./media/active-directory-saas-zivver-tutorial/tutorial_zivver_app.png)  

3. <span data-ttu-id="eeb42-196">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="eeb42-196">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="eeb42-198">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="eeb42-198">Click **Add** button.</span></span> <span data-ttu-id="eeb42-199">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eeb42-199">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="eeb42-201">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eeb42-201">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="eeb42-202">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eeb42-202">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eeb42-203">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="eeb42-203">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="eeb42-204">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="eeb42-204">Test single sign-on</span></span>

<span data-ttu-id="eeb42-205">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="eeb42-205">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="eeb42-206">Když kliknete na dlaždici ZIVVER na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ZIVVER.</span><span class="sxs-lookup"><span data-stu-id="eeb42-206">When you click the ZIVVER tile in the Access Panel, you should get automatically signed-on to your ZIVVER application.</span></span>
<span data-ttu-id="eeb42-207">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="eeb42-207">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="eeb42-208">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="eeb42-208">Additional resources</span></span>

* [<span data-ttu-id="eeb42-209">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eeb42-209">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eeb42-210">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="eeb42-210">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zivver-tutorial/tutorial_general_203.png

