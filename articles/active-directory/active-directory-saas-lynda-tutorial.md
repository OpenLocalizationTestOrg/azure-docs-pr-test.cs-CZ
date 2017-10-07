---
title: 'Kurz: Azure Active Directory integrace s Lynda.com | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Lynda.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6c92789-8b64-4049-bac9-8cb928398433
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: fb8d7824e5121da79e9248393b0cbcb0efaffec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lyndacom"></a><span data-ttu-id="f4e6b-103">Kurz: Azure Active Directory integrace s Lynda.com</span><span class="sxs-lookup"><span data-stu-id="f4e6b-103">Tutorial: Azure Active Directory integration with Lynda.com</span></span>

<span data-ttu-id="f4e6b-104">V tomto kurzu zjistíte, jak toointegrate Lynda.com s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f4e6b-104">In this tutorial, you learn how toointegrate Lynda.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f4e6b-105">Integrace Lynda.com s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f4e6b-105">Integrating Lynda.com with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f4e6b-106">Můžete řídit ve službě Azure AD, který má přístup tooLynda.com</span><span class="sxs-lookup"><span data-stu-id="f4e6b-106">You can control in Azure AD who has access tooLynda.com</span></span>
- <span data-ttu-id="f4e6b-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLynda.com (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4e6b-107">You can enable your users tooautomatically get signed-on tooLynda.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f4e6b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f4e6b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f4e6b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f4e6b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4e6b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f4e6b-110">Prerequisites</span></span>

<span data-ttu-id="f4e6b-111">Integrace služby Azure AD s Lynda.com tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f4e6b-111">tooconfigure Azure AD integration with Lynda.com, you need hello following items:</span></span>

- <span data-ttu-id="f4e6b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4e6b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f4e6b-113">Lynda.com jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f4e6b-113">A Lynda.com single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f4e6b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f4e6b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f4e6b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f4e6b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f4e6b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f4e6b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f4e6b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f4e6b-118">Scenario description</span></span>
<span data-ttu-id="f4e6b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f4e6b-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f4e6b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f4e6b-121">Přidání Lynda.com z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f4e6b-121">Adding Lynda.com from hello gallery</span></span>
2. <span data-ttu-id="f4e6b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f4e6b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lyndacom-from-hello-gallery"></a><span data-ttu-id="f4e6b-123">Přidání Lynda.com z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f4e6b-123">Adding Lynda.com from hello gallery</span></span>
<span data-ttu-id="f4e6b-124">tooconfigure hello integrace Lynda.com do Azure AD, je nutné tooadd Lynda.com hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-124">tooconfigure hello integration of Lynda.com into Azure AD, you need tooadd Lynda.com from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f4e6b-125">**tooadd Lynda.com z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f4e6b-125">**tooadd Lynda.com from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f4e6b-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f4e6b-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f4e6b-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f4e6b-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f4e6b-133">Hello vyhledávacího pole zadejte **Lynda.com**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-133">In hello search box, type **Lynda.com**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_search.png)

5. <span data-ttu-id="f4e6b-135">Na panelu výsledků hello vyberte **Lynda.com**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-135">In hello results panel, select **Lynda.com**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f4e6b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f4e6b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f4e6b-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lynda.com podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f4e6b-138">In this section, you configure and test Azure AD single sign-on with Lynda.com based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f4e6b-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Lynda.com je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lynda.com is tooa user in Azure AD.</span></span> <span data-ttu-id="f4e6b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Lynda.com musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-140">In other words, a link relationship between an Azure AD user and hello related user in Lynda.com needs toobe established.</span></span>

<span data-ttu-id="f4e6b-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Lynda.com.</span></span>

<span data-ttu-id="f4e6b-142">tooconfigure a testu Azure AD jednotné přihlašování s Lynda.com, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f4e6b-142">tooconfigure and test Azure AD single sign-on with Lynda.com, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f4e6b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f4e6b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f4e6b-145">**[Vytvoření zkušebního uživatele Lynda.com](#creating-a-lyndacom-test-user)**  -toohave protějšek Britta Simon v Lynda.com, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-145">**[Creating a Lynda.com test user](#creating-a-lyndacom-test-user)** - toohave a counterpart of Britta Simon in Lynda.com that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f4e6b-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f4e6b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f4e6b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f4e6b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f4e6b-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lynda.com application.</span></span>

<span data-ttu-id="f4e6b-150">**tooconfigure Azure AD jednotné přihlašování s Lynda.com, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f4e6b-150">**tooconfigure Azure AD single sign-on with Lynda.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="f4e6b-151">V portálu Azure, na hello hello **Lynda.com** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-151">In hello Azure portal, on hello **Lynda.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f4e6b-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_samlbase.png)

3. <span data-ttu-id="f4e6b-155">Na hello **Lynda.com domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f4e6b-155">On hello **Lynda.com Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_url.png)

    <span data-ttu-id="f4e6b-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.lynda.com/Shibboleth.sso/InCommon?providerId=<url>&target=<url> `</span><span class="sxs-lookup"><span data-stu-id="f4e6b-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.lynda.com/Shibboleth.sso/InCommon?providerId=<url>&target=<url> `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f4e6b-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-158">This value is not real.</span></span> <span data-ttu-id="f4e6b-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="f4e6b-160">Obraťte se na [tým podpory Lynda.com klienta](https://www.linkedin.com/help/lynda/ask) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-160">Contact [Lynda.com Client support team](https://www.linkedin.com/help/lynda/ask) tooget these values.</span></span> 
 
4. <span data-ttu-id="f4e6b-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_certificate.png) 

5. <span data-ttu-id="f4e6b-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f4e6b-165">tooconfigure jednotného přihlašování na **Lynda.com** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** [Lynda.com podporu](https://www.linkedin.com/help/lynda/ask).</span><span class="sxs-lookup"><span data-stu-id="f4e6b-165">tooconfigure single sign-on on **Lynda.com** side, you need toosend hello downloaded **Metadata XML** [Lynda.com support](https://www.linkedin.com/help/lynda/ask).</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f4e6b-166">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4e6b-166">Creating an Azure AD test user</span></span>
<span data-ttu-id="f4e6b-167">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-167">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f4e6b-169">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f4e6b-169">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f4e6b-170">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-170">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f4e6b-172">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-172">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f4e6b-174">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-174">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f4e6b-176">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f4e6b-176">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f4e6b-178">a.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-178">a.</span></span> <span data-ttu-id="f4e6b-179">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-179">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f4e6b-180">b.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-180">b.</span></span> <span data-ttu-id="f4e6b-181">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-181">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f4e6b-182">c.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-182">c.</span></span> <span data-ttu-id="f4e6b-183">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-183">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f4e6b-184">d.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-184">d.</span></span> <span data-ttu-id="f4e6b-185">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-185">Click **Create**.</span></span>
 
### <a name="creating-a-lyndacom-test-user"></a><span data-ttu-id="f4e6b-186">Vytvoření zkušebního uživatele Lynda.com</span><span class="sxs-lookup"><span data-stu-id="f4e6b-186">Creating a Lynda.com test user</span></span>

<span data-ttu-id="f4e6b-187">Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooLynda.com.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-187">There is no action item for you tooconfigure user provisioning tooLynda.com.</span></span>  
<span data-ttu-id="f4e6b-188">Když se uživatel s přiřazenou pokusí toolog v tooLynda.com pomocí hello přístupového panelu, Lynda.com ověří, zda existuje hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-188">When an assigned user tries toolog in tooLynda.com using hello access panel, Lynda.com checks whether hello user exists.</span></span>  

<span data-ttu-id="f4e6b-189">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-189">If there is no user account available yet, it is automatically created by Lynda.com.</span></span>

>[!NOTE]
><span data-ttu-id="f4e6b-190">Můžete použít všechny ostatní Lynda.com uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Lynda.com tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-190">You can use any other Lynda.com user account creation tools or APIs provided by Lynda.com tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f4e6b-191">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f4e6b-191">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f4e6b-192">V této části povolíte tak, že udělíte přístup tooLynda.com toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-192">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLynda.com.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f4e6b-194">**tooassign Britta Simon tooLynda.com, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f4e6b-194">**tooassign Britta Simon tooLynda.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="f4e6b-195">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-195">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f4e6b-197">V seznamu aplikace hello vyberte **Lynda.com**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-197">In hello applications list, select **Lynda.com**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_app.png) 

3. <span data-ttu-id="f4e6b-199">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-199">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f4e6b-201">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-201">Click **Add** button.</span></span> <span data-ttu-id="f4e6b-202">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-202">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f4e6b-204">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-204">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f4e6b-205">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-205">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f4e6b-206">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-206">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f4e6b-207">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f4e6b-207">Testing single sign-on</span></span>

<span data-ttu-id="f4e6b-208">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="f4e6b-208">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="f4e6b-209">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f4e6b-209">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f4e6b-210">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f4e6b-210">Additional resources</span></span>

* [<span data-ttu-id="f4e6b-211">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f4e6b-211">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f4e6b-212">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f4e6b-212">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_203.png

