---
title: "Kurz: Azure Active Directory integrace s pomůže Scout | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a pomáhají Scout."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 84cee39c28a0f7e6b9878441e504131795673020
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a><span data-ttu-id="30a79-103">Kurz: Azure Active Directory integrace s pomůže Scout</span><span class="sxs-lookup"><span data-stu-id="30a79-103">Tutorial: Azure Active Directory integration with Help Scout</span></span>

<span data-ttu-id="30a79-104">V tomto kurzu zjistěte, jak integrovat Scout pomoci s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="30a79-104">In this tutorial, you learn how to integrate Help Scout with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="30a79-105">Integrace s Azure AD pomáhají Scout byste získat následující výhody:</span><span class="sxs-lookup"><span data-stu-id="30a79-105">You get the following benefits from integrating Help Scout with Azure AD:</span></span>

- <span data-ttu-id="30a79-106">Ve službě Azure AD můžete řídit, kdo má přístup k nápovědě Scout.</span><span class="sxs-lookup"><span data-stu-id="30a79-106">In Azure AD, you can control who has access to Help Scout.</span></span>
- <span data-ttu-id="30a79-107">Uživatelům pomůžou Scout můžete automaticky přihlásit pomocí jednotného přihlašování a účtu uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="30a79-107">You can automatically sign in your users to Help Scout by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="30a79-108">Můžete spravovat své účty pomocí nich centrální umístění, portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="30a79-108">You can manage your accounts in one, central location, the Azure portal.</span></span>

<span data-ttu-id="30a79-109">Další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="30a79-109">To learn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30a79-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30a79-110">Prerequisites</span></span>

<span data-ttu-id="30a79-111">K nastavení integrace Azure AD s pomůže Scout, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="30a79-111">To set up Azure AD integration with Help Scout, you need the following items:</span></span>

- <span data-ttu-id="30a79-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="30a79-112">An Azure AD subscription</span></span>
- <span data-ttu-id="30a79-113">Pomůže Scout předplatné, se jednotné přihlašování zapnutý</span><span class="sxs-lookup"><span data-stu-id="30a79-113">A Help Scout subscription, with single sign-on turned on</span></span> 

> [!NOTE]
> <span data-ttu-id="30a79-114">Pokud testujete kroky v tomto kurzu, doporučujeme, abyste si je vyzkoušeli v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="30a79-114">If you test the steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="30a79-115">Doporučení pro testování kroky v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="30a79-115">Recommendations for testing the steps in this tutorial:</span></span>

- <span data-ttu-id="30a79-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="30a79-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="30a79-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30a79-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="30a79-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="30a79-118">Scenario description</span></span>
<span data-ttu-id="30a79-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="30a79-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="30a79-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="30a79-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="30a79-121">Přidejte pomůže Scout z galerie.</span><span class="sxs-lookup"><span data-stu-id="30a79-121">Add Help Scout from the gallery.</span></span>
2. <span data-ttu-id="30a79-122">Nastavte a otestujte Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="30a79-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-help-scout-from-the-gallery"></a><span data-ttu-id="30a79-123">Přidat pomůže Scout z Galerie</span><span class="sxs-lookup"><span data-stu-id="30a79-123">Add Help Scout from the gallery</span></span>
<span data-ttu-id="30a79-124">K nastavení integrace Scout pomoci s Azure AD v galerii, přidejte pomůže Scout si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="30a79-124">To set up the integration of Help Scout with Azure AD, in the gallery, add Help Scout to your list of managed SaaS apps.</span></span>

<span data-ttu-id="30a79-125">Chcete-li přidat pomůže Scout z galerie:</span><span class="sxs-lookup"><span data-stu-id="30a79-125">To add Help Scout from the gallery:</span></span>

1. <span data-ttu-id="30a79-126">V [portál Azure](https://portal.azure.com), v nabídce vlevo vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="30a79-126">In the [Azure portal](https://portal.azure.com), in the left menu, select **Azure Active Directory**.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="30a79-128">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="30a79-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Stránku podnikových aplikací][2]
    
3. <span data-ttu-id="30a79-130">Chcete-li přidat novou aplikaci, vyberte **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="30a79-130">To add a new application, select **New application**.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="30a79-132">Do vyhledávacího pole zadejte **pomoci Scout**.</span><span class="sxs-lookup"><span data-stu-id="30a79-132">In the search box, enter **Help Scout**.</span></span> <span data-ttu-id="30a79-133">Ve výsledcích hledání vyberte **pomoci Scout**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="30a79-133">In the search results, select **Help Scout**, and then select **Add**.</span></span>

    ![Nápověda Scout v seznamu výsledků](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="30a79-135">Nastavte a otestujte Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="30a79-135">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="30a79-136">V této části můžete nastavit a testu Azure AD jednotné přihlašování s pomůže Scout podle testovacího uživatele s názvem *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="30a79-136">In this section, you set up and test Azure AD single sign-on with Help Scout based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="30a79-137">Pro jednotné přihlašování pro práci Azure AD musí znát příslušného uživatele Azure AD v nápovědě Scout.</span><span class="sxs-lookup"><span data-stu-id="30a79-137">For single sign-on to work, Azure AD needs to know the Azure AD counterpart user in Help Scout.</span></span> <span data-ttu-id="30a79-138">Je nutné vytvořit vztah propojení mezi uživatele Azure AD a související uživatele v nápovědě Scout.</span><span class="sxs-lookup"><span data-stu-id="30a79-138">A link relationship between an Azure AD user and the related user in Help Scout must be established.</span></span>

<span data-ttu-id="30a79-139">K navázání vztahu odkaz, v nápovědě Scout pro **uživatelské jméno**, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="30a79-139">To establish the link relationship, in Help Scout, for **Username**, assign the value of the **user name** in Azure AD.</span></span>

<span data-ttu-id="30a79-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s pomůže Scout, proveďte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="30a79-140">To configure and test Azure AD single sign-on with Help Scout, complete the following tasks:</span></span>

1. <span data-ttu-id="30a79-141">[Nastavení Azure AD jednotné přihlašování](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="30a79-141">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="30a79-142">Nastaví uživatele tak, aby tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="30a79-142">Sets up a user to use this feature.</span></span>
2. <span data-ttu-id="30a79-143">[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="30a79-143">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="30a79-144">Testy Azure AD jednotné přihlašování s uživatelem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30a79-144">Tests Azure AD single sign-on with the user Britta Simon.</span></span>
3. <span data-ttu-id="30a79-145">[Vytvoření zkušebního uživatele pomůže Scout](#create-a-help-scout-test-user).</span><span class="sxs-lookup"><span data-stu-id="30a79-145">[Create a Help Scout test user](#create-a-help-scout-test-user).</span></span> <span data-ttu-id="30a79-146">Vytvoří protějšek Britta Simon v pomoci Scout, který je propojený s Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="30a79-146">Creates a counterpart of Britta Simon in Help Scout that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="30a79-147">[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="30a79-147">[Assign the Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="30a79-148">Nastaví Britta Simon používat Azure AD jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="30a79-148">Sets up Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="30a79-149">[Test jednotného přihlašování](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="30a79-149">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="30a79-150">Ověřuje, že konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="30a79-150">Verifies that the configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="30a79-151">Nastavení Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="30a79-151">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="30a79-152">V této části můžete nastavit Azure AD jednotného přihlašování na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="30a79-152">In this section, you set up Azure AD single sign-on in the Azure portal.</span></span> <span data-ttu-id="30a79-153">Potom nastavíte jednotné přihlašování v aplikaci pomůže Scout.</span><span class="sxs-lookup"><span data-stu-id="30a79-153">Then, you set up single sign-on in your Help Scout application.</span></span>

<span data-ttu-id="30a79-154">Nastavení Azure AD jednotné přihlašování s Scout pomáhají:</span><span class="sxs-lookup"><span data-stu-id="30a79-154">To set up Azure AD single sign-on with Help Scout:</span></span>

1. <span data-ttu-id="30a79-155">Na portálu Azure na **pomoci Scout** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="30a79-155">In the Azure portal, on the **Help Scout** application integration page, select **Single sign-on**.</span></span>
 
    ![Nastavit odkaz přihlášení][4]

2. <span data-ttu-id="30a79-157">Na **jednotného přihlašování** stránky, pro **režimu**, vyberte **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="30a79-157">On the **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. <span data-ttu-id="30a79-159">V části **pomoci Scout domény a adresy URL**, pokud chcete nastavit aplikaci v režimu spouštěná IDP, dokončení následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="30a79-159">Under **Help Scout Domain and URLs**, if you want to set up the application in IDP-initiated mode, complete the following steps:</span></span>

    1. <span data-ttu-id="30a79-160">V **identifikátor** zadejte adresu URL, která má následující vzoru:`urn:auth0:helpscout:<instancename>`</span><span class="sxs-lookup"><span data-stu-id="30a79-160">In the **Identifier** box, enter a URL that has the following pattern: `urn:auth0:helpscout:<instancename>`</span></span>

    2. <span data-ttu-id="30a79-161">V **adresa URL odpovědi** zadejte adresu URL, která má následující vzoru:`https://helpscout.auth0.com/login/callback?connection=<instancename>`</span><span class="sxs-lookup"><span data-stu-id="30a79-161">In the **Reply URL** box, enter a URL that has the following pattern: `https://helpscout.auth0.com/login/callback?connection=<instancename>`</span></span>

    ![Scout domény a adresy URL jeden přihlašování informace nápovědy](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. <span data-ttu-id="30a79-163">Pokud chcete nastavit aplikaci v režimu spouštěná SP, vyberte **zobrazit upřesňující nastavení adresy URL** zaškrtněte políčko a potom proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="30a79-163">If you want to set up the application in SP-initiated mode, select the **Show advanced URL settings** check box, and then do the following:</span></span>

    * <span data-ttu-id="30a79-164">V **přihlásit na adrese URL** zadejte adresu URL, která má následující formát:`https://secure.helpscout.net/members/login/`</span><span class="sxs-lookup"><span data-stu-id="30a79-164">In the **Sign on URL** box, enter a URL that has the following format: `https://secure.helpscout.net/members/login/`</span></span>

    ![Scout domény a adresy URL jeden přihlašování informace nápovědy](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > <span data-ttu-id="30a79-166">Hodnoty v těchto adres URL jsou pouze jako ukázka.</span><span class="sxs-lookup"><span data-stu-id="30a79-166">The values in these URLs are for demonstration only.</span></span> <span data-ttu-id="30a79-167">Aktualizujte hodnoty s skutečného identifikátoru adresy URL a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="30a79-167">Update the values with the actual identifier URL and reply URL.</span></span> <span data-ttu-id="30a79-168">Chcete-li získat tyto hodnoty, obraťte se na [tým podpory pomůže Scout](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="30a79-168">To get these values, contact [Help Scout support team](mailto:help@helpscout.com).</span></span> 

5. <span data-ttu-id="30a79-169">V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="30a79-169">Under **SAML Signing Certificate**, select **Metadata XML**, and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. <span data-ttu-id="30a79-171">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="30a79-171">Select **Save**.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="30a79-173">Nastavení jednotného přihlašování na straně pomůže Scout, pošlete stažené metadata souboru XML na [tým podpory pomůže Scout](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="30a79-173">To set up single sign-on on the Help Scout side, send the downloaded metadata XML file to the [Help Scout support team](mailto:help@helpscout.com).</span></span> <span data-ttu-id="30a79-174">Tým podpory pomůže Scout platí toto nastavení tak, aby SAML jeden přihlašování připojení je správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="30a79-174">The Help Scout support team applies this setting so that the SAML single sign-on connection is set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="30a79-175">Můžete si přečíst stručným verzi tyto pokyny v [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="30a79-175">You can read a concise version of these instructions in the [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="30a79-176">Po přidání aplikace tak, že vyberete **služby Active Directory** > **podnikové aplikace, které**, vyberte **jednotné přihlašování** kartě. Dostanete embedded dokumentaci v **konfigurace** části, v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="30a79-176">After you add the app by selecting **Active Directory** > **Enterprise Applications**, select the **Single Sign-On** tab. You can access the embedded documentation in the **Configuration** section, at the bottom of the page.</span></span> <span data-ttu-id="30a79-177">Další informace najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="30a79-177">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="30a79-178">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="30a79-178">Create an Azure AD test user</span></span>

<span data-ttu-id="30a79-179">V této části portálu Azure můžete vytvořit testovacího uživatele s názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30a79-179">In this section, in the Azure portal, you create a test user named Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="30a79-181">Vytvoření zkušebního uživatele ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="30a79-181">To create a test user in Azure AD:</span></span>

1. <span data-ttu-id="30a79-182">Na portálu Azure v levé nabídce vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="30a79-182">In the Azure portal, in the left menu, select **Azure Active Directory**.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="30a79-184">Chcete-li zobrazit seznam uživatelů, vyberte **uživatelů a skupin**a potom vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="30a79-184">To display the list of users, select **Users and groups**, and then select **All users**.</span></span>

    ![Vyberte uživatele a skupiny a pak vyberte možnost Všichni uživatelé](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="30a79-186">Chcete-li otevřít **uživatele** dialogové okno, v horní části **všichni uživatelé** vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="30a79-186">To open the **User** dialog box, at the top of the **All Users** page, select **Add**.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="30a79-188">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="30a79-188">In the **User** dialog box, complete the following steps:</span></span>

    1. <span data-ttu-id="30a79-189">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="30a79-189">In the **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="30a79-190">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30a79-190">In the **User name** box, enter the email address of user Britta Simon.</span></span>

    3. <span data-ttu-id="30a79-191">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="30a79-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    4. <span data-ttu-id="30a79-192">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="30a79-192">Select **Create**.</span></span>

        ![Dialogové okno uživatele](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a><span data-ttu-id="30a79-194">Vytvoření zkušebního uživatele pomůže Scout</span><span class="sxs-lookup"><span data-stu-id="30a79-194">Create a Help Scout test user</span></span>

<span data-ttu-id="30a79-195">Objekt v této části je vytvoření uživatele s názvem Britta Simon v nápovědě Scout.</span><span class="sxs-lookup"><span data-stu-id="30a79-195">The object of this section is to create a user named Britta Simon in Help Scout.</span></span> <span data-ttu-id="30a79-196">Nápověda Scout podporuje za běhu (JIT) zřizování, který je ve výchozím nastavení zapnutá.</span><span class="sxs-lookup"><span data-stu-id="30a79-196">Help Scout supports just-in-time (JIT) provisioning, which is turned on by default.</span></span>

<span data-ttu-id="30a79-197">V této části se nevyžaduje žádné akce ani na dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="30a79-197">In this section, there's no action or task to complete.</span></span> <span data-ttu-id="30a79-198">Pokud uživatel v nápovědě Scout ještě neexistuje, vytvoří se nový při pokusu o přístup k nápovědě Scout.</span><span class="sxs-lookup"><span data-stu-id="30a79-198">If a user doesn't already exist in Help Scout, a new one is created when you attempt to access Help Scout.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="30a79-199">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="30a79-199">Assign the Azure AD test user</span></span>

<span data-ttu-id="30a79-200">V této části povolit uživatele Britta Simon používat Azure AD jednotné přihlašování pomocí udělení přístupu uživatelskému účtu pomůže Scout.</span><span class="sxs-lookup"><span data-stu-id="30a79-200">In this section, you allow the user Britta Simon to use Azure AD single sign-on by granting the user account access to Help Scout.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="30a79-202">Přiřazení Britta Simon Scout pomáhají:</span><span class="sxs-lookup"><span data-stu-id="30a79-202">To assign Britta Simon to Help Scout:</span></span>

1. <span data-ttu-id="30a79-203">Na portálu Azure otevřete zobrazení aplikace a pak přejděte do zobrazení adresáře.</span><span class="sxs-lookup"><span data-stu-id="30a79-203">In the Azure portal, open the applications view, and then go to the directory view.</span></span> <span data-ttu-id="30a79-204">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="30a79-204">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="30a79-206">V seznamu aplikací vyberte **pomoci Scout**.</span><span class="sxs-lookup"><span data-stu-id="30a79-206">In the applications list, select **Help Scout**.</span></span>

    ![Na odkaz Nápověda Scout v seznamu aplikací](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. <span data-ttu-id="30a79-208">V nabídce vlevo vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="30a79-208">In the left menu, select **Users and groups**.</span></span>

    ![Propojení uživatelů a skupin][202]

4. <span data-ttu-id="30a79-210">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="30a79-210">Select **Add**.</span></span> <span data-ttu-id="30a79-211">Potom na **přidat přiřazení** vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="30a79-211">Then, on the **Add Assignment** page, select **Users and groups**.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="30a79-213">Na **uživatelů a skupin** v seznamu uživatelů, vyberte na stránce **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="30a79-213">On the **Users and groups** page, in the list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="30a79-214">Na **uživatelů a skupin** vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="30a79-214">On the **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="30a79-215">Na **přidat přiřazení** vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="30a79-215">On the **Add Assignment** page, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="30a79-216">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="30a79-216">Test single sign-on</span></span>

<span data-ttu-id="30a79-217">V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="30a79-217">In this section, you test your Azure AD single sign-on configuration by using the access panel.</span></span>

<span data-ttu-id="30a79-218">Když vyberete dlaždici pomůže Scout na přístupovém panelu, můžete by měl být automaticky přihlášeni do vaší aplikace pomůže Scout.</span><span class="sxs-lookup"><span data-stu-id="30a79-218">When you select the Help Scout tile in the access panel, you should be automatically signed in to your Help Scout application.</span></span>

<span data-ttu-id="30a79-219">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="30a79-219">For more information about the access panel, see [Introduction to the access panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="30a79-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="30a79-220">Additional resources</span></span>

* [<span data-ttu-id="30a79-221">Seznam kurzů k integraci aplikací SaaS v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="30a79-221">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="30a79-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="30a79-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

