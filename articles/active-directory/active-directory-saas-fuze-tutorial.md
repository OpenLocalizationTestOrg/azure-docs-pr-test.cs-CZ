---
title: 'Kurz: Azure Active Directory integrace s Fuze | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Fuze."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9780b4bf-1fd1-48c1-9ceb-f750225ae08a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: d0ea8c6456824e348301ed8bf1f5e00f4bfa8121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuze"></a><span data-ttu-id="04797-103">Kurz: Azure Active Directory integrace s Fuze</span><span class="sxs-lookup"><span data-stu-id="04797-103">Tutorial: Azure Active Directory integration with Fuze</span></span>

<span data-ttu-id="04797-104">V tomto kurzu zjistíte, jak toointegrate Fuze s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="04797-104">In this tutorial, you learn how toointegrate Fuze with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="04797-105">Integrace Fuze s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="04797-105">Integrating Fuze with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="04797-106">Můžete řídit ve službě Azure AD, který má přístup tooFuze</span><span class="sxs-lookup"><span data-stu-id="04797-106">You can control in Azure AD who has access tooFuze</span></span>
- <span data-ttu-id="04797-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFuze (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="04797-107">You can enable your users tooautomatically get signed-on tooFuze (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="04797-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="04797-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="04797-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="04797-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04797-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="04797-110">Prerequisites</span></span>

<span data-ttu-id="04797-111">Integrace služby Azure AD s Fuze tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="04797-111">tooconfigure Azure AD integration with Fuze, you need hello following items:</span></span>

- <span data-ttu-id="04797-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="04797-112">An Azure AD subscription</span></span>
- <span data-ttu-id="04797-113">Fuze jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="04797-113">A Fuze single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="04797-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="04797-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="04797-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="04797-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="04797-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="04797-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="04797-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="04797-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="04797-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="04797-118">Scenario description</span></span>
<span data-ttu-id="04797-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="04797-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="04797-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="04797-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="04797-121">Přidání Fuze z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="04797-121">Adding Fuze from hello gallery</span></span>
2. <span data-ttu-id="04797-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="04797-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-fuze-from-hello-gallery"></a><span data-ttu-id="04797-123">Přidání Fuze z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="04797-123">Adding Fuze from hello gallery</span></span>
<span data-ttu-id="04797-124">tooconfigure hello integrace Fuze do Azure AD, je nutné tooadd Fuze hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="04797-124">tooconfigure hello integration of Fuze into Azure AD, you need tooadd Fuze from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="04797-125">**tooadd Fuze z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="04797-125">**tooadd Fuze from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="04797-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="04797-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="04797-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="04797-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="04797-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="04797-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="04797-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04797-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="04797-133">Hello vyhledávacího pole zadejte **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="04797-133">In hello search box, type **Fuze**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_000.png)

5. <span data-ttu-id="04797-135">Na panelu výsledků hello vyberte **Fuze**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="04797-135">In hello results panel, select **Fuze**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="04797-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="04797-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="04797-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Fuze podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="04797-138">In this section, you configure and test Azure AD single sign-on with Fuze based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="04797-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Fuze je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="04797-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Fuze is tooa user in Azure AD.</span></span> <span data-ttu-id="04797-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Fuze musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="04797-140">In other words, a link relationship between an Azure AD user and hello related user in Fuze needs toobe established.</span></span>

<span data-ttu-id="04797-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Fuze.</span><span class="sxs-lookup"><span data-stu-id="04797-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Fuze.</span></span>

<span data-ttu-id="04797-142">tooconfigure a testu Azure AD jednotné přihlašování s Fuze, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="04797-142">tooconfigure and test Azure AD single sign-on with Fuze, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="04797-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="04797-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="04797-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="04797-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="04797-145">**[Vytvoření zkušebního uživatele Fuze](#creating-a-fuze-test-user)**  -toohave protějšek Britta Simon v Fuze, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="04797-145">**[Creating a Fuze test user](#creating-a-fuze-test-user)** - toohave a counterpart of Britta Simon in Fuze that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="04797-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="04797-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="04797-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="04797-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="04797-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="04797-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="04797-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Fuze.</span><span class="sxs-lookup"><span data-stu-id="04797-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Fuze application.</span></span>

<span data-ttu-id="04797-150">**tooconfigure Azure AD jednotné přihlašování s Fuze, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="04797-150">**tooconfigure Azure AD single sign-on with Fuze, perform hello following steps:**</span></span>

1. <span data-ttu-id="04797-151">V hello Azure Management portal na hello **Fuze** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="04797-151">In hello Azure Management portal, on hello **Fuze** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="04797-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="04797-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_01.png)

3. <span data-ttu-id="04797-155">Na hello **Fuze domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="04797-155">On hello **Fuze Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_020.png)
    
    <span data-ttu-id="04797-157">V hello **přihlásit na adrese URL** textovému poli, zadejte adresu URL hello přihlášení jako:`https://www.thinkingphones.com/jetspeed/portal/`</span><span class="sxs-lookup"><span data-stu-id="04797-157">In hello **Sign on URL** textbox, type hello Sign-on URL as: `https://www.thinkingphones.com/jetspeed/portal/`</span></span>

4.  <span data-ttu-id="04797-158">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04797-158">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="04797-160">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor xml hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="04797-160">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello xml file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_05.png) 

6. <span data-ttu-id="04797-162">tooconfigure jednotného přihlašování na **Fuze** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Fuze](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="04797-162">tooconfigure single sign-on on **Fuze** side, you need toosend hello downloaded **Metadata XML** too[Fuze support team](https://www.fuze.com/support).</span></span> <span data-ttu-id="04797-163">To bude nastavené v pořadí toohave hello jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="04797-163">They will set this up in order toohave hello SAML SSO connection set properly on both sides.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="04797-164">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="04797-164">Creating an Azure AD test user</span></span>
<span data-ttu-id="04797-165">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="04797-165">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="04797-167">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="04797-167">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="04797-168">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="04797-168">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="04797-170">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="04797-170">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="04797-172">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04797-172">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="04797-174">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="04797-174">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="04797-176">a.</span><span class="sxs-lookup"><span data-stu-id="04797-176">a.</span></span> <span data-ttu-id="04797-177">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="04797-177">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="04797-178">b.</span><span class="sxs-lookup"><span data-stu-id="04797-178">b.</span></span> <span data-ttu-id="04797-179">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="04797-179">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="04797-180">c.</span><span class="sxs-lookup"><span data-stu-id="04797-180">c.</span></span> <span data-ttu-id="04797-181">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="04797-181">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="04797-182">d.</span><span class="sxs-lookup"><span data-stu-id="04797-182">d.</span></span> <span data-ttu-id="04797-183">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04797-183">Click **Create**.</span></span> 


### <a name="creating-a-fuze-test-user"></a><span data-ttu-id="04797-184">Vytvoření zkušebního uživatele Fuze</span><span class="sxs-lookup"><span data-stu-id="04797-184">Creating a Fuze test user</span></span>

<span data-ttu-id="04797-185">Fuze aplikace podporuje úplné těsně v čas uživatele zřizovat, takže uživatelé budou automaticky vytvořeny při jejich přihlášení.</span><span class="sxs-lookup"><span data-stu-id="04797-185">Fuze application supports full Just in time user provision, so users will get created automatically when they sign-in.</span></span> <span data-ttu-id="04797-186">Další informace, kontaktujte prosím Fuze [podporu](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="04797-186">For any other clarification, please contact Fuze [support](https://www.fuze.com/support).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="04797-187">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="04797-187">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="04797-188">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooFuze svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="04797-188">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFuze.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="04797-190">**tooassign Britta Simon tooFuze, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="04797-190">**tooassign Britta Simon tooFuze, perform hello following steps:**</span></span>

1. <span data-ttu-id="04797-191">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="04797-191">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="04797-193">V seznamu aplikace hello vyberte **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="04797-193">In hello applications list, select **Fuze**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_50.png) 

3. <span data-ttu-id="04797-195">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="04797-195">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="04797-197">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04797-197">Click **Add** button.</span></span> <span data-ttu-id="04797-198">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04797-198">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="04797-200">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="04797-200">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="04797-201">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04797-201">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="04797-202">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04797-202">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="testing-single-sign-on"></a><span data-ttu-id="04797-203">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="04797-203">Testing single sign-on</span></span>

<span data-ttu-id="04797-204">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="04797-204">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="04797-205">Když kliknete na dlaždici Fuze hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Fuze aplikace.</span><span class="sxs-lookup"><span data-stu-id="04797-205">When you click hello Fuze tile in hello Access Panel, you should get automatically signed-on tooyour Fuze application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="04797-206">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="04797-206">Additional resources</span></span>

* [<span data-ttu-id="04797-207">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="04797-207">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="04797-208">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="04797-208">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_203.png