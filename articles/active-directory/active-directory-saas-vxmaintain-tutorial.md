---
title: 'Kurz: Integrovat Azure Active Directory vxMaintain | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a vxMaintain."
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
ms.openlocfilehash: 937ea276d898986fc5a953c96fddabdc8940309f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a><span data-ttu-id="3bd48-103">Kurz: Integrovat vxMaintain Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3bd48-103">Tutorial: Integrate Azure Active Directory with vxMaintain</span></span>

<span data-ttu-id="3bd48-104">V tomto kurzu zjistíte, jak toointegrate vxMaintain s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3bd48-104">In this tutorial, you learn how toointegrate vxMaintain with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3bd48-105">Tato integrace poskytuje několik výhod důležité.</span><span class="sxs-lookup"><span data-stu-id="3bd48-105">This integration provides several important benefits.</span></span> <span data-ttu-id="3bd48-106">Můžete:</span><span class="sxs-lookup"><span data-stu-id="3bd48-106">You can:</span></span>

- <span data-ttu-id="3bd48-107">Ovládací prvek ve službě Azure AD, který má přístup k toovxMaintain.</span><span class="sxs-lookup"><span data-stu-id="3bd48-107">Control in Azure AD who has access toovxMaintain.</span></span>
- <span data-ttu-id="3bd48-108">Povolte přihlášení uživatelů tooautomatically toovxMaintain s jednotné přihlašování (SSO) prostřednictvím jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3bd48-108">Enable your users tooautomatically sign in toovxMaintain with single sign-on (SSO) by using their Azure AD accounts.</span></span>
- <span data-ttu-id="3bd48-109">Spravovat účty v jednom centrálním místě: hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3bd48-109">Manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="3bd48-110">toolearn Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3bd48-110">toolearn more about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3bd48-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3bd48-111">Prerequisites</span></span>

<span data-ttu-id="3bd48-112">Integrace služby Azure AD s vxMaintain tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3bd48-112">tooconfigure Azure AD integration with vxMaintain, you need hello following items:</span></span>

- <span data-ttu-id="3bd48-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3bd48-113">An Azure AD subscription</span></span>
- <span data-ttu-id="3bd48-114">VxMaintain předplatné povolené jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3bd48-114">A vxMaintain SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3bd48-115">Při testování hello kroky v tomto kurzu, doporučujeme vám, nepoužívejte provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3bd48-115">When you test hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="3bd48-116">tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="3bd48-116">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="3bd48-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3bd48-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3bd48-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3bd48-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3bd48-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3bd48-119">Scenario description</span></span>
<span data-ttu-id="3bd48-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3bd48-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="3bd48-121">Hello scénář, který tento kurz popisuje se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3bd48-121">hello scenario that this tutorial outlines consists of two main building blocks:</span></span>

* <span data-ttu-id="3bd48-122">Přidání vxMaintain z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3bd48-122">Adding vxMaintain from hello gallery</span></span>
* <span data-ttu-id="3bd48-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3bd48-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="add-vxmaintain-from-hello-gallery"></a><span data-ttu-id="3bd48-124">Přidat vxMaintain z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3bd48-124">Add vxMaintain from hello gallery</span></span>
<span data-ttu-id="3bd48-125">integrace hello tooconfigure vxMaintain s Azure AD, je nutné tooadd vxMaintain hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3bd48-125">tooconfigure hello integration of vxMaintain with Azure AD, you need tooadd vxMaintain from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3bd48-126">tooadd vxMaintain z Galerie hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="3bd48-126">tooadd vxMaintain from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="3bd48-127">V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, vyberte hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3bd48-127">In hello [Azure portal](https://portal.azure.com), in hello left pane, select hello **Azure Active Directory** button.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="3bd48-129">Vyberte **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![podokno "Podnikové aplikace" Hello][2]
    
3. <span data-ttu-id="3bd48-131">tooadd aplikace, v hello **všechny aplikace** dialogové okno, vyberte **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-131">tooadd an application, in hello **All applications** dialog box, select **New application**.</span></span>

    ![Hello "Nové aplikace" tlačítko][3]

4. <span data-ttu-id="3bd48-133">Hello vyhledávacího pole zadejte **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-133">In hello search box, type **vxMaintain**.</span></span>

    ![Hello "Jednoho přihlašování v režimu" rozevíracího seznamu](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. <span data-ttu-id="3bd48-135">V seznamu výsledků hello vyberte **vxMaintain**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-135">In hello results list, select **vxMaintain**, and then select **Add**.</span></span>

    ![odkaz vxMaintain Hello](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3bd48-137">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3bd48-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="3bd48-138">V této části můžete nakonfigurovat a otestovat Azure AD SSO pomocí vxMaintain, podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3bd48-138">In this section, you configure and test Azure AD SSO by using vxMaintain, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3bd48-139">Azure AD pro jednotné přihlašování toowork musí tooknow hello vxMaintain protějšku toohello uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3bd48-139">For SSO toowork, Azure AD needs tooknow hello vxMaintain counterpart toohello Azure AD user.</span></span> <span data-ttu-id="3bd48-140">To znamená je potřeba vytvořit vztah propojení mezi hello uživatele Azure AD a hello odpovídající vxMaintain uživatele.</span><span class="sxs-lookup"><span data-stu-id="3bd48-140">That is, you must establish a link relationship between hello Azure AD user and hello corresponding vxMaintain user.</span></span>

<span data-ttu-id="3bd48-141">tooestablish hello odkaz vztah, přiřaďte hello vxMaintain **uživatelské jméno** hodnotu jako hello Azure AD **uživatelské jméno** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3bd48-141">tooestablish hello link relationship, assign hello vxMaintain **user name** value as hello Azure AD **Username** value.</span></span>

<span data-ttu-id="3bd48-142">tooconfigure a testování Azure AD jednotného přihlašování pomocí vxMaintain, dokončení hello následující stavební bloky.</span><span class="sxs-lookup"><span data-stu-id="3bd48-142">tooconfigure and test Azure AD SSO by using vxMaintain, complete hello following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="3bd48-143">Konfigurovat Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3bd48-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="3bd48-144">V této části můžete povolit jednotné přihlašování Azure AD v hello portálu Azure i nakonfigurovat jednotné přihlašování v aplikaci vxMaintain díky hello následující:</span><span class="sxs-lookup"><span data-stu-id="3bd48-144">In this section, you can both enable Azure AD SSO in hello Azure portal and configure SSO in your vxMaintain application by doing hello following:</span></span>

1. <span data-ttu-id="3bd48-145">V portálu Azure, na hello hello **vxMaintain** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-145">In hello Azure portal, on hello **vxMaintain** application integration page, select **Single sign-on**.</span></span>

    ![příkaz "Jednotného přihlašování" Hello][4]

2. <span data-ttu-id="3bd48-147">tooenable jednotné přihlašování, v hello **režimu přihlašování** rozevíracího seznamu vyberte **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-147">tooenable SSO, in hello **Single Sign-on Mode** drop-down list, select **SAML-based Sign-on**.</span></span>
 
    ![Hello příkaz "na základě SAML přihlášení"](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. <span data-ttu-id="3bd48-149">V části **vxMaintain domény a adresy URL**, hello následující:</span><span class="sxs-lookup"><span data-stu-id="3bd48-149">Under **vxMaintain Domain and URLs**, do hello following:</span></span>

    ![Hello vxMaintain oddílu domény a adresy URL](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    <span data-ttu-id="3bd48-151">a.</span><span class="sxs-lookup"><span data-stu-id="3bd48-151">a.</span></span> <span data-ttu-id="3bd48-152">V hello **identifikátor** pole, zadejte adresu URL, která má hello následující syntaxi:`https://<company name>.verisae.com`</span><span class="sxs-lookup"><span data-stu-id="3bd48-152">In hello **Identifier** box, type a URL that has hello following syntax: `https://<company name>.verisae.com`</span></span>

    <span data-ttu-id="3bd48-153">b.</span><span class="sxs-lookup"><span data-stu-id="3bd48-153">b.</span></span> <span data-ttu-id="3bd48-154">V hello **adresa URL odpovědi** pole, zadejte adresu URL, která má hello následující syntaxi:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span><span class="sxs-lookup"><span data-stu-id="3bd48-154">In hello **Reply URL** box, type a URL that has hello following syntax: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3bd48-155">Hello předchozí hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3bd48-155">hello preceding values are not real.</span></span> <span data-ttu-id="3bd48-156">Je aktualizovat skutečným identifikátorem hello a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3bd48-156">Update them with hello actual identifier and reply URL.</span></span> <span data-ttu-id="3bd48-157">tooobtain hello hodnoty, kontaktujte hello [tým podpory vxMaintain](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="3bd48-157">tooobtain hello values, contact hello [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>
 
4. <span data-ttu-id="3bd48-158">V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**a potom uložte hello metadata souboru tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="3bd48-158">Under **SAML Signing Certificate**, select **Metadata XML**, and then save hello metadata file tooyour computer.</span></span>

    ![Hello část "SAML podpisový certifikát"](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. <span data-ttu-id="3bd48-160">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-160">Select **Save**.</span></span>

    ![tlačítko Uložit Hello](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3bd48-162">tooconfigure **vxMaintain** jednotné přihlašování, odeslání hello Stáhnout **soubor XML s metadaty** souboru toohello [tým podpory vxMaintain](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="3bd48-162">tooconfigure **vxMaintain** SSO, send hello downloaded **Metadata XML** file toohello [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="3bd48-163">Při nastavování aplikace hello, si můžete přečíst stručným verzi hello předchozích pokynů v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3bd48-163">As you set up hello app, you can read a concise version of hello preceding instructions in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3bd48-164">Po přidání aplikace hello z hello **služby Active Directory** > **podnikové aplikace, které** části, vyberte hello **jednotné přihlašování** kartě a potom hello přístup vložené dokumentace z hello **konfigurace** části.</span><span class="sxs-lookup"><span data-stu-id="3bd48-164">After you add hello app from hello **Active Directory** > **Enterprise Applications** section, select hello **Single Sign-On** tab, and then access hello embedded documentation from hello **Configuration** section.</span></span> 
>
><span data-ttu-id="3bd48-165">toolearn Další informace o funkci embedded dokumentace hello, najdete v části [Správa jednotného přihlašování pro podnikové aplikace](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="3bd48-165">toolearn more about hello embedded documentation feature, see [Managing single sign-on for enterprise apps](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3bd48-166">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3bd48-166">Create an Azure AD test user</span></span>
<span data-ttu-id="3bd48-167">V této části vytvoříte testovacího uživatele Britta Simon v hello portálu Azure pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="3bd48-167">In this section, you create test user Britta Simon in hello Azure portal by doing hello following:</span></span>

![Hello Azure AD testovacího uživatele][100]

1. <span data-ttu-id="3bd48-169">V hello **portál Azure**, v levém podokně text hello, vyberte hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3bd48-169">In hello **Azure portal**, in hello left pane, select hello **Azure Active Directory** button.</span></span>

    ![tlačítko "Azure Active Directory" Hello](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3bd48-171">toodisplay seznam uživatelů, přejděte příliš**uživatelů a skupin** > **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-171">toodisplay a list of users, go too**Users and groups** > **All users**.</span></span>
    
    <span data-ttu-id="3bd48-172">![Hello "Všichni uživatelé" odkaz](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span><span class="sxs-lookup"><span data-stu-id="3bd48-172">![hello "All users" link](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span></span>  
    <span data-ttu-id="3bd48-173">Hello **všichni uživatelé** otevře se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3bd48-173">hello **All users** dialog box opens.</span></span> 

3. <span data-ttu-id="3bd48-174">tooopen hello **uživatele** dialogové okno, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-174">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3bd48-176">V hello **uživatele** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="3bd48-176">In hello **User** dialog box, do hello following:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3bd48-178">a.</span><span class="sxs-lookup"><span data-stu-id="3bd48-178">a.</span></span> <span data-ttu-id="3bd48-179">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-179">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3bd48-180">b.</span><span class="sxs-lookup"><span data-stu-id="3bd48-180">b.</span></span> <span data-ttu-id="3bd48-181">V hello **uživatelské jméno** pole typu hello e-mailovou adresu testovacího uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3bd48-181">In hello **User name** box, type hello email address of test user Britta Simon.</span></span>

    <span data-ttu-id="3bd48-182">c.</span><span class="sxs-lookup"><span data-stu-id="3bd48-182">c.</span></span> <span data-ttu-id="3bd48-183">Vyberte hello **zobrazit hesla** zaškrtávací políčko a potom Poznámka hello hodnotu, která byla vygenerována v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="3bd48-183">Select hello **Show Password** check box, and then note hello value that was generated in hello **Password** box.</span></span>

    <span data-ttu-id="3bd48-184">d.</span><span class="sxs-lookup"><span data-stu-id="3bd48-184">d.</span></span> <span data-ttu-id="3bd48-185">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-185">Select **Create**.</span></span>
 
### <a name="create-a-vxmaintain-test-user"></a><span data-ttu-id="3bd48-186">Vytvoření zkušebního uživatele vxMaintain</span><span class="sxs-lookup"><span data-stu-id="3bd48-186">Create a vxMaintain test user</span></span>

<span data-ttu-id="3bd48-187">V této části vytvoříte testovacího uživatele Britta Simon v vxMaintain.</span><span class="sxs-lookup"><span data-stu-id="3bd48-187">In this section, you create test user Britta Simon in vxMaintain.</span></span> <span data-ttu-id="3bd48-188">Uživatelé tooadd hello vxMaintain platformy, pracovat s [tým podpory vxMaintain](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="3bd48-188">tooadd users in hello vxMaintain platform, work with the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span> <span data-ttu-id="3bd48-189">Před použitím jednotného přihlašování k vytvoření a aktivace uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="3bd48-189">Before you use SSO, create and activate hello users.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="3bd48-190">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3bd48-190">Assign hello Azure AD test user</span></span>

<span data-ttu-id="3bd48-191">V této části povolíte tak, že udělíte přístup toovxMaintain testovacího uživatele Britta Simon toouse jednotného přihlašování k Azure.</span><span class="sxs-lookup"><span data-stu-id="3bd48-191">In this section, you enable test user Britta Simon toouse Azure SSO by granting access toovxMaintain.</span></span> <span data-ttu-id="3bd48-192">toodo tedy hello následující:</span><span class="sxs-lookup"><span data-stu-id="3bd48-192">toodo so, do hello following:</span></span>

![Testovací uživatel v seznamu zobrazovaný název hello][200] 

1. <span data-ttu-id="3bd48-194">V portálu Azure hello **aplikace** zobrazit, přejděte příliš**Directory** zobrazení > **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-194">In hello Azure portal **Applications** view, go too**Directory** view > **Enterprise applications** > **All applications**.</span></span>

    ![Hello "Všechny aplikace" odkaz][201] 

2. <span data-ttu-id="3bd48-196">V hello **aplikace** seznamu, vyberte **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-196">In hello **Applications** list, select **vxMaintain**.</span></span>

    ![odkaz vxMaintain Hello](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. <span data-ttu-id="3bd48-198">V levém podokně hello vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-198">In hello left pane, select **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="3bd48-200">Vyberte **přidat** a pak na hello **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-200">Select **Add** and then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][203]

5. <span data-ttu-id="3bd48-202">V hello **uživatelů a skupin** dialogové okno, v hello **uživatelé** seznamu, vyberte **Britta Simon**a potom vyberte hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3bd48-202">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**, and then select hello **Select** button.</span></span>

7. <span data-ttu-id="3bd48-203">V hello **přidat přiřazení** dialogové okno, vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="3bd48-203">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-your-azure-ad-single-sign-on"></a><span data-ttu-id="3bd48-204">Testování vaší služby Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3bd48-204">Test your Azure AD single sign-on</span></span>

<span data-ttu-id="3bd48-205">V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3bd48-205">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="3bd48-206">Výběr hello **vxMaintain** dlaždice v hello Panel přístupu by se měl přihlásit můžete tooyour vxMaintain aplikace automaticky.</span><span class="sxs-lookup"><span data-stu-id="3bd48-206">Selecting hello **vxMaintain** tile in hello Access Panel should sign you in tooyour vxMaintain application automatically.</span></span>

<span data-ttu-id="3bd48-207">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3bd48-207">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3bd48-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3bd48-208">Next steps</span></span>

* [<span data-ttu-id="3bd48-209">Seznam kurzů k integraci aplikací SaaS v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3bd48-209">List of tutorials on integrating SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3bd48-210">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3bd48-210">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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

