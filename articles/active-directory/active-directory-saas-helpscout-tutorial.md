---
title: "Kurz: Azure Active Directory integrace s pomůže Scout | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a pomáhají Scout."
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
ms.openlocfilehash: 58edd140eb1eb5980796ca743b5f7acd891729a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a><span data-ttu-id="2d2b0-103">Kurz: Azure Active Directory integrace s pomůže Scout</span><span class="sxs-lookup"><span data-stu-id="2d2b0-103">Tutorial: Azure Active Directory integration with Help Scout</span></span>

<span data-ttu-id="2d2b0-104">V tomto kurzu zjistíte, jak pomoci toointegrate Scout službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-104">In this tutorial, you learn how toointegrate Help Scout with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2d2b0-105">Můžete získat hello z integrace s Azure AD pomáhají Scout následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-105">You get hello following benefits from integrating Help Scout with Azure AD:</span></span>

- <span data-ttu-id="2d2b0-106">Ve službě Azure AD můžete řídit, kdo má přístup tooHelp Scout.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-106">In Azure AD, you can control who has access tooHelp Scout.</span></span>
- <span data-ttu-id="2d2b0-107">Vaši uživatelé tooHelp Scout můžete automaticky přihlásit pomocí jednotného přihlašování a účtu uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-107">You can automatically sign in your users tooHelp Scout by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="2d2b0-108">Můžete spravovat své účty pomocí nich centrální umístění, hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-108">You can manage your accounts in one, central location, hello Azure portal.</span></span>

<span data-ttu-id="2d2b0-109">toolearn Další informace o softwaru, služba (SaaS) aplikace integraci s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-109">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d2b0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2d2b0-110">Prerequisites</span></span>

<span data-ttu-id="2d2b0-111">tooset až integrace Azure AD s pomůže Scout, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-111">tooset up Azure AD integration with Help Scout, you need hello following items:</span></span>

- <span data-ttu-id="2d2b0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d2b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2d2b0-113">Pomůže Scout předplatné, se jednotné přihlašování zapnutý</span><span class="sxs-lookup"><span data-stu-id="2d2b0-113">A Help Scout subscription, with single sign-on turned on</span></span> 

> [!NOTE]
> <span data-ttu-id="2d2b0-114">Pokud testujete hello kroky v tomto kurzu, doporučujeme, abyste si je vyzkoušeli v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-114">If you test hello steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="2d2b0-115">Doporučení pro testování hello kroky v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-115">Recommendations for testing hello steps in this tutorial:</span></span>

- <span data-ttu-id="2d2b0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="2d2b0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2d2b0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2d2b0-118">Scenario description</span></span>
<span data-ttu-id="2d2b0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="2d2b0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2d2b0-121">Přidejte pomůže Scout z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-121">Add Help Scout from hello gallery.</span></span>
2. <span data-ttu-id="2d2b0-122">Nastavte a otestujte Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-help-scout-from-hello-gallery"></a><span data-ttu-id="2d2b0-123">Přidat pomůže Scout z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2d2b0-123">Add Help Scout from hello gallery</span></span>
<span data-ttu-id="2d2b0-124">tooset až hello integrace Scout pomoci s Azure AD v galerii hello přidat pomůže Scout tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-124">tooset up hello integration of Help Scout with Azure AD, in hello gallery, add Help Scout tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2d2b0-125">tooadd pomoci Scout z Galerie hello:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-125">tooadd Help Scout from hello gallery:</span></span>

1. <span data-ttu-id="2d2b0-126">V hello [portál Azure](https://portal.azure.com), v levé nabídce text hello, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-126">In hello [Azure portal](https://portal.azure.com), in hello left menu, select **Azure Active Directory**.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="2d2b0-128">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![stránka aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="2d2b0-130">tooadd novou aplikaci, vyberte **novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-130">tooadd a new application, select **New application**.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="2d2b0-132">Hello vyhledávacího pole zadejte **pomoci Scout**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-132">In hello search box, enter **Help Scout**.</span></span> <span data-ttu-id="2d2b0-133">Ve výsledcích hledání hello, vyberte **pomoci Scout**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-133">In hello search results, select **Help Scout**, and then select **Add**.</span></span>

    ![Nápověda Scout v seznamu výsledků hello](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="2d2b0-135">Nastavte a otestujte Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d2b0-135">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="2d2b0-136">V této části můžete nastavit a testu Azure AD jednotné přihlašování s pomůže Scout podle testovacího uživatele s názvem *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-136">In this section, you set up and test Azure AD single sign-on with Help Scout based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="2d2b0-137">Pro toowork jeden přihlašování musí Azure AD tooknow hello Azure AD příslušného uživatele v pomoci Scout.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-137">For single sign-on toowork, Azure AD needs tooknow hello Azure AD counterpart user in Help Scout.</span></span> <span data-ttu-id="2d2b0-138">Je nutné vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské hello v nápovědě Scout.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-138">A link relationship between an Azure AD user and hello related user in Help Scout must be established.</span></span>

<span data-ttu-id="2d2b0-139">tooestablish hello propojení vztahu v rámci pomoci Scout pro **uživatelské jméno**, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-139">tooestablish hello link relationship, in Help Scout, for **Username**, assign hello value of hello **user name** in Azure AD.</span></span>

<span data-ttu-id="2d2b0-140">tooconfigure a testování Azure AD jednotné přihlašování s pomůže Scout dokončení hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-140">tooconfigure and test Azure AD single sign-on with Help Scout, complete hello following tasks:</span></span>

1. <span data-ttu-id="2d2b0-141">[Nastavení Azure AD jednotné přihlašování](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-141">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="2d2b0-142">Nastaví uživatele toouse tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-142">Sets up a user toouse this feature.</span></span>
2. <span data-ttu-id="2d2b0-143">[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-143">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="2d2b0-144">Testy Azure AD jednotné přihlašování s uživatelem hello Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-144">Tests Azure AD single sign-on with hello user Britta Simon.</span></span>
3. <span data-ttu-id="2d2b0-145">[Vytvoření zkušebního uživatele pomůže Scout](#create-a-help-scout-test-user).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-145">[Create a Help Scout test user](#create-a-help-scout-test-user).</span></span> <span data-ttu-id="2d2b0-146">Vytvoří protějšek Britta Simon v pomoci Scout, který je propojený toohello reprezentace hello uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-146">Creates a counterpart of Britta Simon in Help Scout that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="2d2b0-147">[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-147">[Assign hello Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="2d2b0-148">Nastaví Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-148">Sets up Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2d2b0-149">[Test jednotného přihlašování](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-149">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="2d2b0-150">Ověřuje, že konfigurace hello funguje.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-150">Verifies that hello configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="2d2b0-151">Nastavení Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d2b0-151">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="2d2b0-152">V této části můžete nastavit Azure AD jednotné přihlašování v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-152">In this section, you set up Azure AD single sign-on in hello Azure portal.</span></span> <span data-ttu-id="2d2b0-153">Potom nastavíte jednotné přihlašování v aplikaci pomůže Scout.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-153">Then, you set up single sign-on in your Help Scout application.</span></span>

<span data-ttu-id="2d2b0-154">tooset do Azure AD jednotné přihlašování s pomůže Scout:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-154">tooset up Azure AD single sign-on with Help Scout:</span></span>

1. <span data-ttu-id="2d2b0-155">V portálu Azure, na hello hello **pomoci Scout** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-155">In hello Azure portal, on hello **Help Scout** application integration page, select **Single sign-on**.</span></span>
 
    ![Nastavit odkaz přihlášení][4]

2. <span data-ttu-id="2d2b0-157">Na hello **jednotného přihlašování** stránky, pro **režimu**, vyberte **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-157">On hello **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. <span data-ttu-id="2d2b0-159">V části **pomoci Scout domény a adresy URL**, pokud chcete tooset si aplikace hello v rozšíření IDP spouštěná režimu dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-159">Under **Help Scout Domain and URLs**, if you want tooset up hello application in IDP-initiated mode, complete hello following steps:</span></span>

    1. <span data-ttu-id="2d2b0-160">V hello **identifikátor** zadejte adresu URL, která má následující vzor hello:`urn:auth0:helpscout:<instancename>`</span><span class="sxs-lookup"><span data-stu-id="2d2b0-160">In hello **Identifier** box, enter a URL that has hello following pattern: `urn:auth0:helpscout:<instancename>`</span></span>

    2. <span data-ttu-id="2d2b0-161">V hello **adresa URL odpovědi** zadejte adresu URL, která má následující vzor hello:`https://helpscout.auth0.com/login/callback?connection=<instancename>`</span><span class="sxs-lookup"><span data-stu-id="2d2b0-161">In hello **Reply URL** box, enter a URL that has hello following pattern: `https://helpscout.auth0.com/login/callback?connection=<instancename>`</span></span>

    ![Scout domény a adresy URL jeden přihlašování informace nápovědy](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. <span data-ttu-id="2d2b0-163">Pokud chcete tooset až hello aplikace v režimu spouštěná SP, vyberte hello **zobrazit upřesňující nastavení adresy URL** zaškrtněte políčko a potom hello následující:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-163">If you want tooset up hello application in SP-initiated mode, select hello **Show advanced URL settings** check box, and then do hello following:</span></span>

    * <span data-ttu-id="2d2b0-164">V hello **přihlásit na adrese URL** zadejte adresu URL, která má hello následující formát:`https://secure.helpscout.net/members/login/`</span><span class="sxs-lookup"><span data-stu-id="2d2b0-164">In hello **Sign on URL** box, enter a URL that has hello following format: `https://secure.helpscout.net/members/login/`</span></span>

    ![Scout domény a adresy URL jeden přihlašování informace nápovědy](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > <span data-ttu-id="2d2b0-166">Hello hodnoty v těchto adres URL jsou pouze jako ukázka.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-166">hello values in these URLs are for demonstration only.</span></span> <span data-ttu-id="2d2b0-167">Aktualizujte hodnoty hello s hello skutečného identifikátoru adresy URL a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-167">Update hello values with hello actual identifier URL and reply URL.</span></span> <span data-ttu-id="2d2b0-168">Obraťte se na tooget tyto hodnoty [tým podpory pomůže Scout](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-168">tooget these values, contact [Help Scout support team](mailto:help@helpscout.com).</span></span> 

5. <span data-ttu-id="2d2b0-169">V části **SAML podpisový certifikát**, vyberte **soubor XML s metadaty**a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-169">Under **SAML Signing Certificate**, select **Metadata XML**, and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. <span data-ttu-id="2d2b0-171">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-171">Select **Save**.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="2d2b0-173">tooset až jedním přihlašování na straně hello pomůže Scout, odesílat hello stáhnout metadata XML soubor toohello [tým podpory pomůže Scout](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-173">tooset up single sign-on on hello Help Scout side, send hello downloaded metadata XML file toohello [Help Scout support team](mailto:help@helpscout.com).</span></span> <span data-ttu-id="2d2b0-174">tým podpory pomůže Scout Hello platí toto nastavení tak, aby hello SAML jednoho přihlášení připojení je správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-174">hello Help Scout support team applies this setting so that hello SAML single sign-on connection is set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="2d2b0-175">Můžete si přečíst stručným verzi tyto pokyny v hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="2d2b0-175">You can read a concise version of these instructions in hello [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="2d2b0-176">Po přidání aplikace hello výběrem **služby Active Directory** > **podnikové aplikace, které**, vyberte hello **jednotné přihlašování** kartě. Můžete získat přístup k dokumentaci hello vložených v hello **konfigurace** oddíl na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-176">After you add hello app by selecting **Active Directory** > **Enterprise Applications**, select hello **Single Sign-On** tab. You can access hello embedded documentation in hello **Configuration** section, at hello bottom of hello page.</span></span> <span data-ttu-id="2d2b0-177">Další informace najdete v tématu [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-177">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="2d2b0-178">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d2b0-178">Create an Azure AD test user</span></span>

<span data-ttu-id="2d2b0-179">V této části v hello portál Azure můžete vytvořit testovacího uživatele s názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-179">In this section, in hello Azure portal, you create a test user named Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="2d2b0-181">toocreate testovacího uživatele ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-181">toocreate a test user in Azure AD:</span></span>

1. <span data-ttu-id="2d2b0-182">V hello portál Azure, v levé nabídce hello, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-182">In hello Azure portal, in hello left menu, select **Azure Active Directory**.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="2d2b0-184">toodisplay hello seznam uživatelů, vyberte **uživatelů a skupin**a potom vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-184">toodisplay hello list of users, select **Users and groups**, and then select **All users**.</span></span>

    ![Vyberte uživatele a skupiny a pak vyberte možnost Všichni uživatelé](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="2d2b0-186">tooopen hello **uživatele** dialogové okno, hello horní části hello **všichni uživatelé** vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-186">tooopen hello **User** dialog box, at hello top of hello **All Users** page, select **Add**.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="2d2b0-188">V hello **uživatele** dialogové okno, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-188">In hello **User** dialog box, complete hello following steps:</span></span>

    1. <span data-ttu-id="2d2b0-189">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-189">In hello **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="2d2b0-190">V hello **uživatelské jméno** zadejte hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-190">In hello **User name** box, enter hello email address of user Britta Simon.</span></span>

    3. <span data-ttu-id="2d2b0-191">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    4. <span data-ttu-id="2d2b0-192">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-192">Select **Create**.</span></span>

        ![Dialogové okno uživatelského Hello](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a><span data-ttu-id="2d2b0-194">Vytvoření zkušebního uživatele pomůže Scout</span><span class="sxs-lookup"><span data-stu-id="2d2b0-194">Create a Help Scout test user</span></span>

<span data-ttu-id="2d2b0-195">objekt Hello této části je toocreate uživatele s názvem Britta Simon v nápovědě Scout.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-195">hello object of this section is toocreate a user named Britta Simon in Help Scout.</span></span> <span data-ttu-id="2d2b0-196">Nápověda Scout podporuje za běhu (JIT) zřizování, který je ve výchozím nastavení zapnutá.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-196">Help Scout supports just-in-time (JIT) provisioning, which is turned on by default.</span></span>

<span data-ttu-id="2d2b0-197">V této části je toocomplete žádná akce nebo úloh.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-197">In this section, there's no action or task toocomplete.</span></span> <span data-ttu-id="2d2b0-198">Pokud uživatel v nápovědě Scout ještě neexistuje, nový vytvoří při pokusíte tooaccess pomoci Scout.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-198">If a user doesn't already exist in Help Scout, a new one is created when you attempt tooaccess Help Scout.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="2d2b0-199">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2d2b0-199">Assign hello Azure AD test user</span></span>

<span data-ttu-id="2d2b0-200">V této části můžete povolit uživateli hello Britta Simon toouse Azure AD jednotné přihlašování, poskytněte hello uživatele účtu přístup tooHelp Scout.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-200">In this section, you allow hello user Britta Simon toouse Azure AD single sign-on by granting hello user account access tooHelp Scout.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="2d2b0-202">tooassign tooHelp Britta Simon Scout:</span><span class="sxs-lookup"><span data-stu-id="2d2b0-202">tooassign Britta Simon tooHelp Scout:</span></span>

1. <span data-ttu-id="2d2b0-203">V hello portálu Azure otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-203">In hello Azure portal, open hello applications view, and then go toohello directory view.</span></span> <span data-ttu-id="2d2b0-204">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-204">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2d2b0-206">V seznamu aplikace hello vyberte **pomoci Scout**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-206">In hello applications list, select **Help Scout**.</span></span>

    ![Hello pomůže Scout odkaz v seznamu aplikace hello](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. <span data-ttu-id="2d2b0-208">V levé nabídce hello, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-208">In hello left menu, select **Users and groups**.</span></span>

    ![Hello uživatelé a skupiny odkaz][202]

4. <span data-ttu-id="2d2b0-210">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-210">Select **Add**.</span></span> <span data-ttu-id="2d2b0-211">Potom na hello **přidat přiřazení** vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-211">Then, on hello **Add Assignment** page, select **Users and groups**.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="2d2b0-213">Na hello **uživatelů a skupin** v hello seznam uživatelů, vyberte na stránce **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-213">On hello **Users and groups** page, in hello list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="2d2b0-214">Na hello **uživatelů a skupin** vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-214">On hello **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="2d2b0-215">Na hello **přidat přiřazení** vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-215">On hello **Add Assignment** page, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="2d2b0-216">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2d2b0-216">Test single sign-on</span></span>

<span data-ttu-id="2d2b0-217">V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-217">In this section, you test your Azure AD single sign-on configuration by using hello access panel.</span></span>

<span data-ttu-id="2d2b0-218">Když vyberete hello pomůže Scout dlaždice v hello přístupového panelu, by měl být automaticky přihlášeni tooyour pomoci Scout aplikace.</span><span class="sxs-lookup"><span data-stu-id="2d2b0-218">When you select hello Help Scout tile in hello access panel, you should be automatically signed in tooyour Help Scout application.</span></span>

<span data-ttu-id="2d2b0-219">Další informace o na přístupovém panelu najdete v tématu [Úvod toohello přístupový panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2d2b0-219">For more information about the access panel, see [Introduction toohello access panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2d2b0-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2d2b0-220">Additional resources</span></span>

* [<span data-ttu-id="2d2b0-221">Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2d2b0-221">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2d2b0-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2d2b0-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

