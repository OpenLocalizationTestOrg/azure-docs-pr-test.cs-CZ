---
title: 'Kurz: Azure Active Directory integrace s Lesson.ly | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Lesson.ly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 23b339dcc26471b42aaa7e1baadcb42500d6b7e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a><span data-ttu-id="cec6e-103">Kurz: Azure Active Directory integrace s Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="cec6e-103">Tutorial: Azure Active Directory integration with Lesson.ly</span></span>

<span data-ttu-id="cec6e-104">V tomto kurzu zjistíte, jak toointegrate Lesson.ly s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cec6e-104">In this tutorial, you learn how toointegrate Lesson.ly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cec6e-105">Integrace Lesson.ly s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cec6e-105">Integrating Lesson.ly with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cec6e-106">Můžete řídit ve službě Azure AD, který má přístup tooLesson.ly</span><span class="sxs-lookup"><span data-stu-id="cec6e-106">You can control in Azure AD who has access tooLesson.ly</span></span>
- <span data-ttu-id="cec6e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLesson.ly (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cec6e-107">You can enable your users tooautomatically get signed-on tooLesson.ly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cec6e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cec6e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cec6e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cec6e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cec6e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cec6e-110">Prerequisites</span></span>

<span data-ttu-id="cec6e-111">Integrace služby Azure AD s Lesson.ly tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="cec6e-111">tooconfigure Azure AD integration with Lesson.ly, you need hello following items:</span></span>

- <span data-ttu-id="cec6e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cec6e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cec6e-113">Lesson.ly jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cec6e-113">A Lesson.ly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cec6e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cec6e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cec6e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cec6e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cec6e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="cec6e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cec6e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cec6e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cec6e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cec6e-118">Scenario description</span></span>
<span data-ttu-id="cec6e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cec6e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cec6e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cec6e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cec6e-121">Přidání Lesson.ly z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cec6e-121">Adding Lesson.ly from hello gallery</span></span>
2. <span data-ttu-id="cec6e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cec6e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lessonly-from-hello-gallery"></a><span data-ttu-id="cec6e-123">Přidání Lesson.ly z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cec6e-123">Adding Lesson.ly from hello gallery</span></span>
<span data-ttu-id="cec6e-124">tooconfigure hello integrace Lesson.ly do Azure AD, je nutné tooadd Lesson.ly hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cec6e-124">tooconfigure hello integration of Lesson.ly into Azure AD, you need tooadd Lesson.ly from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cec6e-125">**tooadd Lesson.ly z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cec6e-125">**tooadd Lesson.ly from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cec6e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cec6e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cec6e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cec6e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cec6e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cec6e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cec6e-133">Hello vyhledávacího pole zadejte **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-133">In hello search box, type **Lesson.ly**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_search.png)

5. <span data-ttu-id="cec6e-135">Na panelu výsledků hello vyberte **Lesson.ly**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cec6e-135">In hello results panel, select **Lesson.ly**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cec6e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cec6e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cec6e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lesson.ly podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cec6e-138">In this section, you configure and test Azure AD single sign-on with Lesson.ly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cec6e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Lesson.ly je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cec6e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lesson.ly is tooa user in Azure AD.</span></span> <span data-ttu-id="cec6e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Lesson.ly musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="cec6e-140">In other words, a link relationship between an Azure AD user and hello related user in Lesson.ly needs toobe established.</span></span>

<span data-ttu-id="cec6e-141">V Lesson.ly, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="cec6e-141">In Lesson.ly, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cec6e-142">tooconfigure a testu Azure AD jednotné přihlašování s Lesson.ly, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="cec6e-142">tooconfigure and test Azure AD single sign-on with Lesson.ly, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cec6e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cec6e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cec6e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cec6e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cec6e-145">**[Vytvoření zkušebního uživatele Lesson.ly](#creating-a-lessonly-test-user)**  -toohave protějšek Britta Simon v Lesson.ly, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="cec6e-145">**[Creating a Lesson.ly test user](#creating-a-lessonly-test-user)** - toohave a counterpart of Britta Simon in Lesson.ly that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cec6e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cec6e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cec6e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="cec6e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cec6e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cec6e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cec6e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="cec6e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lesson.ly application.</span></span>

<span data-ttu-id="cec6e-150">**tooconfigure Azure AD jednotné přihlašování s Lesson.ly, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cec6e-150">**tooconfigure Azure AD single sign-on with Lesson.ly, perform hello following steps:**</span></span>

1. <span data-ttu-id="cec6e-151">V portálu Azure, na hello hello **Lesson.ly** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-151">In hello Azure portal, on hello **Lesson.ly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cec6e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cec6e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_samlbase.png)

3. <span data-ttu-id="cec6e-155">Na hello **Lesson.ly domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cec6e-155">On hello **Lesson.ly Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_url.png)

    <span data-ttu-id="cec6e-157">a.</span><span class="sxs-lookup"><span data-stu-id="cec6e-157">a.</span></span> <span data-ttu-id="cec6e-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="cec6e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/signin`|
    | `https://<companyname>.lessonly.com/signin`|

    >[!NOTE]
    ><span data-ttu-id="cec6e-159">Při odkazování na obecný názvů, které **NázevSpolečnosti** potřebuje nahradit skutečným názvem toobe.</span><span class="sxs-lookup"><span data-stu-id="cec6e-159">When referencing a generic name that **companyname** needs toobe replaced by an actual name.</span></span>
    
    <span data-ttu-id="cec6e-160">b.</span><span class="sxs-lookup"><span data-stu-id="cec6e-160">b.</span></span> <span data-ttu-id="cec6e-161">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="cec6e-161">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/auth/saml/metadata`|
    | `https://<companyname>.lessonly.com/auth/saml/metadata`|

    > [!NOTE] 
    > <span data-ttu-id="cec6e-162">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="cec6e-162">These values are not real.</span></span> <span data-ttu-id="cec6e-163">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="cec6e-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cec6e-164">Obraťte se na [tým podpory Lesson.ly klienta](mailto:dev@lessonly.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cec6e-164">Contact [Lesson.ly Client support team](mailto:dev@lessonly.com) tooget these values.</span></span> 

4. <span data-ttu-id="cec6e-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cec6e-165">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_certificate.png)

5. <span data-ttu-id="cec6e-167">Hello Lesson.ly aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu SAML** configuration.hello následující snímek obrazovky ukazuje příklad pro Tento.</span><span class="sxs-lookup"><span data-stu-id="cec6e-167">hello Lesson.ly application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.hello following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png)
           
6. <span data-ttu-id="cec6e-169">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello předcházející bitové kopie a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cec6e-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>

    | <span data-ttu-id="cec6e-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="cec6e-170">Attribute Name</span></span>   | <span data-ttu-id="cec6e-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="cec6e-171">Attribute Value</span></span> |
    | ---------------  | ----------------|
    | <span data-ttu-id="cec6e-172">název urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="cec6e-172">urn:oid:2.5.4.42</span></span> |<span data-ttu-id="cec6e-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="cec6e-173">user.givenname</span></span> |
    | <span data-ttu-id="cec6e-174">název urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="cec6e-174">urn:oid:2.5.4.4</span></span>  |<span data-ttu-id="cec6e-175">User.Surname</span><span class="sxs-lookup"><span data-stu-id="cec6e-175">user.surname</span></span> |
    | <span data-ttu-id="cec6e-176">název urn: oid:0.9.2342.19200300.1001.3</span><span class="sxs-lookup"><span data-stu-id="cec6e-176">urn:oid:0.9.2342.19200300.1001.3</span></span> |<span data-ttu-id="cec6e-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="cec6e-177">user.mail</span></span> |

    <span data-ttu-id="cec6e-178">a.</span><span class="sxs-lookup"><span data-stu-id="cec6e-178">a.</span></span> <span data-ttu-id="cec6e-179">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cec6e-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="cec6e-182">b.</span><span class="sxs-lookup"><span data-stu-id="cec6e-182">b.</span></span> <span data-ttu-id="cec6e-183">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="cec6e-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="cec6e-184">c.</span><span class="sxs-lookup"><span data-stu-id="cec6e-184">c.</span></span> <span data-ttu-id="cec6e-185">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="cec6e-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="cec6e-186">d.</span><span class="sxs-lookup"><span data-stu-id="cec6e-186">d.</span></span> <span data-ttu-id="cec6e-187">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-187">Click **Ok**.</span></span>     

7. <span data-ttu-id="cec6e-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cec6e-188">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="cec6e-190">Na hello **Lesson.ly konfigurace** klikněte na tlačítko **konfigurace Lesson.ly** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="cec6e-190">On hello **Lesson.ly Configuration** section, click **Configure Lesson.ly** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cec6e-191">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="cec6e-191">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_configure.png)

9. <span data-ttu-id="cec6e-193">tooconfigure jednotného přihlašování na **Lesson.ly** straně, je nutné stáhnout hello toosend **Certificate(Base64)** a **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[tým podpory Lesson.ly](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="cec6e-193">tooconfigure single sign-on on **Lesson.ly** side, you need toosend hello downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

> [!TIP]
> <span data-ttu-id="cec6e-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="cec6e-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cec6e-195">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="cec6e-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cec6e-196">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cec6e-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cec6e-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cec6e-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="cec6e-198">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="cec6e-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cec6e-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cec6e-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cec6e-201">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cec6e-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cec6e-203">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cec6e-205">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="cec6e-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cec6e-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cec6e-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cec6e-209">a.</span><span class="sxs-lookup"><span data-stu-id="cec6e-209">a.</span></span> <span data-ttu-id="cec6e-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cec6e-211">b.</span><span class="sxs-lookup"><span data-stu-id="cec6e-211">b.</span></span> <span data-ttu-id="cec6e-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cec6e-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cec6e-213">c.</span><span class="sxs-lookup"><span data-stu-id="cec6e-213">c.</span></span> <span data-ttu-id="cec6e-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cec6e-215">d.</span><span class="sxs-lookup"><span data-stu-id="cec6e-215">d.</span></span> <span data-ttu-id="cec6e-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-216">Click **Create**.</span></span>
 
### <a name="creating-a-lessonly-test-user"></a><span data-ttu-id="cec6e-217">Vytvoření zkušebního uživatele Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="cec6e-217">Creating a Lesson.ly test user</span></span>

<span data-ttu-id="cec6e-218">Hello cílem této části je toocreate volal Britta Simon v Lesson.ly uživatele.</span><span class="sxs-lookup"><span data-stu-id="cec6e-218">hello objective of this section is toocreate a user called Britta Simon in Lesson.ly.</span></span> <span data-ttu-id="cec6e-219">Lesson.ly podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="cec6e-219">Lesson.ly supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="cec6e-220">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="cec6e-220">There is no action item for you in this section.</span></span> <span data-ttu-id="cec6e-221">Pokud ještě neexistuje, se během pokusu o tooaccess Lesson.ly vytvoří nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="cec6e-221">A new user will be created during an attempt tooaccess Lesson.ly if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="cec6e-222">Pokud potřebujete toocreate uživatelé ručně, je nutné toocontact hello [tým podpory Lesson.ly](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="cec6e-222">If you need toocreate an user manually, you need toocontact hello [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cec6e-223">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="cec6e-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cec6e-224">V této části povolíte tak, že udělíte přístup tooLesson.ly toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cec6e-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLesson.ly.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cec6e-226">**tooassign Britta Simon tooLesson.ly, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cec6e-226">**tooassign Britta Simon tooLesson.ly, perform hello following steps:**</span></span>

1. <span data-ttu-id="cec6e-227">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cec6e-229">V seznamu aplikace hello vyberte **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-229">In hello applications list, select **Lesson.ly**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_app.png) 

3. <span data-ttu-id="cec6e-231">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cec6e-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cec6e-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cec6e-233">Click **Add** button.</span></span> <span data-ttu-id="cec6e-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cec6e-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cec6e-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="cec6e-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cec6e-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cec6e-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cec6e-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cec6e-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cec6e-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cec6e-239">Testing single sign-on</span></span>

<span data-ttu-id="cec6e-240">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cec6e-240">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cec6e-241">Když kliknete na dlaždici Lesson.ly hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Lesson.ly aplikace.</span><span class="sxs-lookup"><span data-stu-id="cec6e-241">When you click hello Lesson.ly tile in hello Access Panel, you should get automatically signed-on tooyour Lesson.ly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cec6e-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cec6e-242">Additional resources</span></span>

* [<span data-ttu-id="cec6e-243">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cec6e-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cec6e-244">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cec6e-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png

