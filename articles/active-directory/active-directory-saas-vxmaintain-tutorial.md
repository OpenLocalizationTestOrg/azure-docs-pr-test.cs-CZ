---
title: 'Kurz: Integrovat Azure Active Directory vxMaintain | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a vxMaintain."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: ad87534af448356b8cc80d8ddd278bfb8a9165e7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a><span data-ttu-id="5afa5-103">Kurz: Integrovat vxMaintain Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5afa5-103">Tutorial: Integrate Azure Active Directory with vxMaintain</span></span>

<span data-ttu-id="5afa5-104">V tomto kurzu zjistěte, jak integrovat vxMaintain s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5afa5-104">In this tutorial, you learn how to integrate vxMaintain with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5afa5-105">Tato integrace poskytuje několik výhod důležité.</span><span class="sxs-lookup"><span data-stu-id="5afa5-105">This integration provides several important benefits.</span></span> <span data-ttu-id="5afa5-106">Můžete:</span><span class="sxs-lookup"><span data-stu-id="5afa5-106">You can:</span></span>

- <span data-ttu-id="5afa5-107">Řízení ve službě Azure AD, který má přístup k vxMaintain.</span><span class="sxs-lookup"><span data-stu-id="5afa5-107">Control in Azure AD who has access to vxMaintain.</span></span>
- <span data-ttu-id="5afa5-108">Povolte uživatelům automaticky se přihlaste k vxMaintain s jednotné přihlašování (SSO) pomocí svých účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5afa5-108">Enable your users to automatically sign in to vxMaintain with single sign-on (SSO) by using their Azure AD accounts.</span></span>
- <span data-ttu-id="5afa5-109">Spravovat účty v jednom centrálním místě: portál Azure.</span><span class="sxs-lookup"><span data-stu-id="5afa5-109">Manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="5afa5-110">Další informace o integraci aplikací SaaS v Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5afa5-110">To learn more about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5afa5-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5afa5-111">Prerequisites</span></span>

<span data-ttu-id="5afa5-112">Konfigurace integrace Azure AD s vxMaintain, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5afa5-112">To configure Azure AD integration with vxMaintain, you need the following items:</span></span>

- <span data-ttu-id="5afa5-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5afa5-113">An Azure AD subscription</span></span>
- <span data-ttu-id="5afa5-114">VxMaintain předplatné povolené jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5afa5-114">A vxMaintain SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5afa5-115">Při testování kroky v tomto kurzu, doporučujeme vám, nepoužívejte provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5afa5-115">When you test the steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="5afa5-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="5afa5-116">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="5afa5-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5afa5-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5afa5-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5afa5-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5afa5-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5afa5-119">Scenario description</span></span>
<span data-ttu-id="5afa5-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5afa5-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="5afa5-121">Scénář, který tento kurz popisuje se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5afa5-121">The scenario that this tutorial outlines consists of two main building blocks:</span></span>

* <span data-ttu-id="5afa5-122">Přidání vxMaintain z Galerie</span><span class="sxs-lookup"><span data-stu-id="5afa5-122">Adding vxMaintain from the gallery</span></span>
* <span data-ttu-id="5afa5-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5afa5-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="add-vxmaintain-from-the-gallery"></a><span data-ttu-id="5afa5-124">Přidat vxMaintain z Galerie</span><span class="sxs-lookup"><span data-stu-id="5afa5-124">Add vxMaintain from the gallery</span></span>
<span data-ttu-id="5afa5-125">Chcete-li nakonfigurovat integraci vxMaintain s Azure AD, přidejte vxMaintain z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5afa5-125">To configure the integration of vxMaintain with Azure AD, you need to add vxMaintain from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5afa5-126">Pokud chcete přidat vxMaintain z galerie, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5afa5-126">To add vxMaintain from the gallery, do the following:</span></span>

1. <span data-ttu-id="5afa5-127">V [portál Azure](https://portal.azure.com), v levém podokně, vyberte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5afa5-127">In the [Azure portal](https://portal.azure.com), in the left pane, select the **Azure Active Directory** button.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="5afa5-129">Vyberte **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![V podokně "Podnikové aplikace"][2]
    
3. <span data-ttu-id="5afa5-131">Chcete-li přidat aplikaci, v **všechny aplikace** dialogové okno, vyberte **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-131">To add an application, in the **All applications** dialog box, select **New application**.</span></span>

    !["Nová aplikace" tlačítko][3]

4. <span data-ttu-id="5afa5-133">Do vyhledávacího pole zadejte **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-133">In the search box, type **vxMaintain**.</span></span>

    ![Rozevíracím seznamu "Jednoho přihlašování v režimu"](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. <span data-ttu-id="5afa5-135">V seznamu výsledků vyberte **vxMaintain**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-135">In the results list, select **vxMaintain**, and then select **Add**.</span></span>

    ![Odkaz vxMaintain](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5afa5-137">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5afa5-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="5afa5-138">V této části můžete nakonfigurovat a otestovat Azure AD SSO pomocí vxMaintain, podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5afa5-138">In this section, you configure and test Azure AD SSO by using vxMaintain, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5afa5-139">Pro jednotné přihlašování pro práci Azure AD musí znát vxMaintain protějškem uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5afa5-139">For SSO to work, Azure AD needs to know the vxMaintain counterpart to the Azure AD user.</span></span> <span data-ttu-id="5afa5-140">To znamená je potřeba vytvořit vztah propojení mezi uživatele Azure AD a odpovídající vxMaintain uživatele.</span><span class="sxs-lookup"><span data-stu-id="5afa5-140">That is, you must establish a link relationship between the Azure AD user and the corresponding vxMaintain user.</span></span>

<span data-ttu-id="5afa5-141">Vytvořit vztah odkaz, přiřaďte mu vxMaintain **uživatelské jméno** hodnotu jako Azure AD **uživatelské jméno** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5afa5-141">To establish the link relationship, assign the vxMaintain **user name** value as the Azure AD **Username** value.</span></span>

<span data-ttu-id="5afa5-142">Nakonfigurovat a otestovat Azure AD SSO pomocí vxMaintain, dokončete následující stavební bloky.</span><span class="sxs-lookup"><span data-stu-id="5afa5-142">To configure and test Azure AD SSO by using vxMaintain, complete the following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="5afa5-143">Konfigurovat Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5afa5-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="5afa5-144">V této části můžete povolit jednotné přihlašování Azure AD na portálu Azure i nakonfigurovat jednotné přihlašování v aplikaci vxMaintain následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5afa5-144">In this section, you can both enable Azure AD SSO in the Azure portal and configure SSO in your vxMaintain application by doing the following:</span></span>

1. <span data-ttu-id="5afa5-145">Na portálu Azure na **vxMaintain** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-145">In the Azure portal, on the **vxMaintain** application integration page, select **Single sign-on**.</span></span>

    ![Příkaz "Jednotného přihlašování"][4]

2. <span data-ttu-id="5afa5-147">Pro povolení jednotného přihlašování, v **režimu přihlašování** rozevíracího seznamu vyberte **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-147">To enable SSO, in the **Single Sign-on Mode** drop-down list, select **SAML-based Sign-on**.</span></span>
 
    ![Příkaz "na základě SAML přihlášení"](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. <span data-ttu-id="5afa5-149">V části **vxMaintain domény a adresy URL**, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5afa5-149">Under **vxMaintain Domain and URLs**, do the following:</span></span>

    ![VxMaintain oddílu domény a adresy URL](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    <span data-ttu-id="5afa5-151">a.</span><span class="sxs-lookup"><span data-stu-id="5afa5-151">a.</span></span> <span data-ttu-id="5afa5-152">V **identifikátor** zadejte adresu URL, která má následující syntaxi:`https://<company name>.verisae.com`</span><span class="sxs-lookup"><span data-stu-id="5afa5-152">In the **Identifier** box, type a URL that has the following syntax: `https://<company name>.verisae.com`</span></span>

    <span data-ttu-id="5afa5-153">b.</span><span class="sxs-lookup"><span data-stu-id="5afa5-153">b.</span></span> <span data-ttu-id="5afa5-154">V **adresa URL odpovědi** zadejte adresu URL, která má následující syntaxi:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span><span class="sxs-lookup"><span data-stu-id="5afa5-154">In the **Reply URL** box, type a URL that has the following syntax: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5afa5-155">Předchozí hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5afa5-155">The preceding values are not real.</span></span> <span data-ttu-id="5afa5-156">Je aktualizovat skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="5afa5-156">Update them with the actual identifier and reply URL.</span></span> <span data-ttu-id="5afa5-157">Chcete-li získat hodnoty, obraťte se [tým podpory vxMaintain](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="5afa5-157">To obtain the values, contact the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>
 
4. <span data-ttu-id="5afa5-158">V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**a potom uložte soubor metadat pro váš počítač.</span><span class="sxs-lookup"><span data-stu-id="5afa5-158">Under **SAML Signing Certificate**, select **Metadata XML**, and then save the metadata file to your computer.</span></span>

    ![V části "SAML podpisový certifikát"](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. <span data-ttu-id="5afa5-160">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-160">Select **Save**.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5afa5-162">Ke konfiguraci **vxMaintain** jednotné přihlašování, odesílání stažené **soubor XML s metadaty** do souboru [tým podpory vxMaintain](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="5afa5-162">To configure **vxMaintain** SSO, send the downloaded **Metadata XML** file to the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="5afa5-163">Jak nastavit aplikaci si můžete přečíst stručným verzi podle předchozích pokynů v [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5afa5-163">As you set up the app, you can read a concise version of the preceding instructions in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="5afa5-164">Po přidání aplikace z **služby Active Directory** > **podnikové aplikace, které** vyberte **jednotné přihlašování** kartě a potom přejdete embedded dokumentace z **konfigurace** části.</span><span class="sxs-lookup"><span data-stu-id="5afa5-164">After you add the app from the **Active Directory** > **Enterprise Applications** section, select the **Single Sign-On** tab, and then access the embedded documentation from the **Configuration** section.</span></span> 
>
><span data-ttu-id="5afa5-165">Další informace o funkci embedded dokumentaci najdete v tématu [Správa jednotného přihlašování pro podnikové aplikace](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="5afa5-165">To learn more about the embedded documentation feature, see [Managing single sign-on for enterprise apps](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5afa5-166">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5afa5-166">Create an Azure AD test user</span></span>
<span data-ttu-id="5afa5-167">V této části vytvoříte testovacího uživatele Britta Simon na portálu Azure, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5afa5-167">In this section, you create test user Britta Simon in the Azure portal by doing the following:</span></span>

![Testovací uživatele Azure AD][100]

1. <span data-ttu-id="5afa5-169">V **portál Azure**, v levém podokně, vyberte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5afa5-169">In the **Azure portal**, in the left pane, select the **Azure Active Directory** button.</span></span>

    ![Tlačítko "Azure Active Directory"](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5afa5-171">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** > **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-171">To display a list of users, go to **Users and groups** > **All users**.</span></span>
    
    <span data-ttu-id="5afa5-172">![Na odkaz "Všichni uživatelé"](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span><span class="sxs-lookup"><span data-stu-id="5afa5-172">![The "All users" link](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span></span>  
    <span data-ttu-id="5afa5-173">**Všichni uživatelé** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5afa5-173">The **All users** dialog box opens.</span></span> 

3. <span data-ttu-id="5afa5-174">Chcete-li otevřít **uživatele** dialogové okno, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-174">To open the **User** dialog box, select **Add**.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5afa5-176">V **uživatele** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5afa5-176">In the **User** dialog box, do the following:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5afa5-178">a.</span><span class="sxs-lookup"><span data-stu-id="5afa5-178">a.</span></span> <span data-ttu-id="5afa5-179">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-179">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5afa5-180">b.</span><span class="sxs-lookup"><span data-stu-id="5afa5-180">b.</span></span> <span data-ttu-id="5afa5-181">V **uživatelské jméno** zadejte e-mailovou adresu testovacího uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5afa5-181">In the **User name** box, type the email address of test user Britta Simon.</span></span>

    <span data-ttu-id="5afa5-182">c.</span><span class="sxs-lookup"><span data-stu-id="5afa5-182">c.</span></span> <span data-ttu-id="5afa5-183">Vyberte **zobrazit hesla** zaškrtněte políčko a poznamenejte si hodnotu, která byla vygenerována v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="5afa5-183">Select the **Show Password** check box, and then note the value that was generated in the **Password** box.</span></span>

    <span data-ttu-id="5afa5-184">d.</span><span class="sxs-lookup"><span data-stu-id="5afa5-184">d.</span></span> <span data-ttu-id="5afa5-185">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-185">Select **Create**.</span></span>
 
### <a name="create-a-vxmaintain-test-user"></a><span data-ttu-id="5afa5-186">Vytvoření zkušebního uživatele vxMaintain</span><span class="sxs-lookup"><span data-stu-id="5afa5-186">Create a vxMaintain test user</span></span>

<span data-ttu-id="5afa5-187">V této části vytvoříte testovacího uživatele Britta Simon v vxMaintain.</span><span class="sxs-lookup"><span data-stu-id="5afa5-187">In this section, you create test user Britta Simon in vxMaintain.</span></span> <span data-ttu-id="5afa5-188">Přidat uživatele v platformě vxMaintain, pracovat [tým podpory vxMaintain](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="5afa5-188">To add users in the vxMaintain platform, work with the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span> <span data-ttu-id="5afa5-189">Před použitím jednotného přihlašování k vytvoření a aktivace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5afa5-189">Before you use SSO, create and activate the users.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="5afa5-190">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5afa5-190">Assign the Azure AD test user</span></span>

<span data-ttu-id="5afa5-191">V této části povolíte testovacího uživatele Britta Simon chcete použít k udělení přístupu k vxMaintain jednotného přihlašování k Azure.</span><span class="sxs-lookup"><span data-stu-id="5afa5-191">In this section, you enable test user Britta Simon to use Azure SSO by granting access to vxMaintain.</span></span> <span data-ttu-id="5afa5-192">Chcete-li to provést, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5afa5-192">To do so, do the following:</span></span>

![Testovací uživatel v seznamu zobrazovaný název][200] 

1. <span data-ttu-id="5afa5-194">Na portálu Azure **aplikace** zobrazení, přejděte na **Directory** zobrazení > **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-194">In the Azure portal **Applications** view, go to **Directory** view > **Enterprise applications** > **All applications**.</span></span>

    ![Na odkaz "Všechny aplikace"][201] 

2. <span data-ttu-id="5afa5-196">V **aplikace** seznamu, vyberte **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-196">In the **Applications** list, select **vxMaintain**.</span></span>

    ![Odkaz vxMaintain](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. <span data-ttu-id="5afa5-198">V levém podokně vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-198">In the left pane, select **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="5afa5-200">Vyberte **přidat** a pak na **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-200">Select **Add** and then, in the **Add Assignment** pane, select **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][203]

5. <span data-ttu-id="5afa5-202">V **uživatelů a skupin** v dialogovém **uživatelé** seznamu, vyberte **Britta Simon**a pak vyberte **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5afa5-202">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**, and then select the **Select** button.</span></span>

7. <span data-ttu-id="5afa5-203">V **přidat přiřazení** dialogové okno, vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="5afa5-203">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-your-azure-ad-single-sign-on"></a><span data-ttu-id="5afa5-204">Testování vaší služby Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5afa5-204">Test your Azure AD single sign-on</span></span>

<span data-ttu-id="5afa5-205">V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5afa5-205">In this section, you test your Azure AD SSO configuration by using the Access Panel.</span></span>

<span data-ttu-id="5afa5-206">Výběr **vxMaintain** dlaždice na přístupovém panelu musí přihlásit do aplikace vxMaintain automaticky.</span><span class="sxs-lookup"><span data-stu-id="5afa5-206">Selecting the **vxMaintain** tile in the Access Panel should sign you in to your vxMaintain application automatically.</span></span>

<span data-ttu-id="5afa5-207">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5afa5-207">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5afa5-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5afa5-208">Next steps</span></span>

* [<span data-ttu-id="5afa5-209">Seznam kurzů k integraci aplikací SaaS v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5afa5-209">List of tutorials on integrating SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5afa5-210">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5afa5-210">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png
