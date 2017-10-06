---
title: 'Kurz: Azure Active Directory integrace s Litmos | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Litmos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 026fd10058760f2d63d185ef4aa9d7de3b82525e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a><span data-ttu-id="bfc2a-103">Kurz: Azure Active Directory integrace s Litmos</span><span class="sxs-lookup"><span data-stu-id="bfc2a-103">Tutorial: Azure Active Directory integration with Litmos</span></span>

<span data-ttu-id="bfc2a-104">V tomto kurzu zjistíte, jak toointegrate Litmos s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bfc2a-104">In this tutorial, you learn how toointegrate Litmos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bfc2a-105">Integrace Litmos s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bfc2a-105">Integrating Litmos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bfc2a-106">Můžete ovládat ve službě Azure AD, který má přístup tooLitmos.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-106">You can control in Azure AD who has access tooLitmos.</span></span>
- <span data-ttu-id="bfc2a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLitmos (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-107">You can enable your users tooautomatically get signed-on tooLitmos (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="bfc2a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="bfc2a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bfc2a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bfc2a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bfc2a-110">Prerequisites</span></span>

<span data-ttu-id="bfc2a-111">Integrace služby Azure AD s Litmos tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="bfc2a-111">tooconfigure Azure AD integration with Litmos, you need hello following items:</span></span>

- <span data-ttu-id="bfc2a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="bfc2a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bfc2a-113">Litmos jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="bfc2a-113">A Litmos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bfc2a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bfc2a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="bfc2a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bfc2a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bfc2a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bfc2a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bfc2a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="bfc2a-118">Scenario description</span></span>
<span data-ttu-id="bfc2a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bfc2a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="bfc2a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bfc2a-121">Přidání Litmos z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="bfc2a-121">Adding Litmos from hello gallery</span></span>
2. <span data-ttu-id="bfc2a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bfc2a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-litmos-from-hello-gallery"></a><span data-ttu-id="bfc2a-123">Přidání Litmos z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="bfc2a-123">Adding Litmos from hello gallery</span></span>
<span data-ttu-id="bfc2a-124">tooconfigure hello integrace Litmos do Azure AD, je nutné tooadd Litmos hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-124">tooconfigure hello integration of Litmos into Azure AD, you need tooadd Litmos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bfc2a-125">**tooadd Litmos z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bfc2a-125">**tooadd Litmos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bfc2a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="bfc2a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bfc2a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="bfc2a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="bfc2a-133">Hello vyhledávacího pole zadejte **Litmos**, vyberte **Litmos** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-133">In hello search box, type **Litmos**, select **Litmos** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Litmos v seznamu výsledků hello](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="bfc2a-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bfc2a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="bfc2a-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Litmos podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="bfc2a-136">In this section, you configure and test Azure AD single sign-on with Litmos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bfc2a-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Litmos je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Litmos is tooa user in Azure AD.</span></span> <span data-ttu-id="bfc2a-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Litmos musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-138">In other words, a link relationship between an Azure AD user and hello related user in Litmos needs toobe established.</span></span>

<span data-ttu-id="bfc2a-139">V Litmos, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-139">In Litmos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bfc2a-140">tooconfigure a testu Azure AD jednotné přihlašování s Litmos, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="bfc2a-140">tooconfigure and test Azure AD single sign-on with Litmos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bfc2a-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bfc2a-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bfc2a-143">**[Vytvoření zkušebního uživatele Litmos](#create-a-litmos-test-user)**  -toohave protějšek Britta Simon v Litmos, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-143">**[Create a Litmos test user](#create-a-litmos-test-user)** - toohave a counterpart of Britta Simon in Litmos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bfc2a-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bfc2a-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="bfc2a-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="bfc2a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="bfc2a-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Litmos.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Litmos application.</span></span>

<span data-ttu-id="bfc2a-148">**tooconfigure Azure AD jednotné přihlašování s Litmos, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bfc2a-148">**tooconfigure Azure AD single sign-on with Litmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="bfc2a-149">V portálu Azure, na hello hello **Litmos** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-149">In hello Azure portal, on hello **Litmos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="bfc2a-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_samlbase.png)

3. <span data-ttu-id="bfc2a-153">Na hello **Litmos domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bfc2a-153">On hello **Litmos Domain and URLs** section, perform hello following steps:</span></span>

    ![Litmos domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_url.png)

    <span data-ttu-id="bfc2a-155">a.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-155">a.</span></span> <span data-ttu-id="bfc2a-156">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.litmos.com/account/Login`</span><span class="sxs-lookup"><span data-stu-id="bfc2a-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.litmos.com/account/Login`</span></span>

    <span data-ttu-id="bfc2a-157">b.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-157">b.</span></span> <span data-ttu-id="bfc2a-158">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.litmos.com/integration/samllogin`</span><span class="sxs-lookup"><span data-stu-id="bfc2a-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.litmos.com/integration/samllogin`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bfc2a-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-159">These values are not real.</span></span> <span data-ttu-id="bfc2a-160">Tyto hodnoty aktualizovat pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL, které jsou vysvětlené později v kurzu nebo kontaktujte [tým podpory Litmos](https://www.litmos.com/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-160">Update these values with hello actual Identifier and Reply URL, which are explained later in tutorial or contact [Litmos support team](https://www.litmos.com/contact-us/) tooget these values.</span></span>

4. <span data-ttu-id="bfc2a-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_certificate.png)

5. <span data-ttu-id="bfc2a-163">Jako součást konfigurace hello, je nutné toocustomize hello **atributy tokenu SAML** pro vaši aplikaci Litmos.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-163">As part of hello configuration, you need toocustomize hello **SAML Token Attributes** for your Litmos application.</span></span>

    ![Atribut části](./media/active-directory-saas-litmos-tutorial/tutorial_attribute.png)
           
    | <span data-ttu-id="bfc2a-165">Název atributu</span><span class="sxs-lookup"><span data-stu-id="bfc2a-165">Attribute Name</span></span>   | <span data-ttu-id="bfc2a-166">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="bfc2a-166">Attribute Value</span></span> |   
    | ---------------  | ----------------|
    | <span data-ttu-id="bfc2a-167">FirstName</span><span class="sxs-lookup"><span data-stu-id="bfc2a-167">FirstName</span></span> |<span data-ttu-id="bfc2a-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="bfc2a-168">user.givenname</span></span> |
    | <span data-ttu-id="bfc2a-169">Příjmení</span><span class="sxs-lookup"><span data-stu-id="bfc2a-169">LastName</span></span>  |<span data-ttu-id="bfc2a-170">User.Surname</span><span class="sxs-lookup"><span data-stu-id="bfc2a-170">user.surname</span></span> |
    | <span data-ttu-id="bfc2a-171">E-mail</span><span class="sxs-lookup"><span data-stu-id="bfc2a-171">Email</span></span> |<span data-ttu-id="bfc2a-172">User.Mail</span><span class="sxs-lookup"><span data-stu-id="bfc2a-172">user.mail</span></span> |

    <span data-ttu-id="bfc2a-173">a.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-173">a.</span></span> <span data-ttu-id="bfc2a-174">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-174">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Přidání atributu](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_04.png)

    ![Přidání atributu Dailog](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="bfc2a-177">b.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-177">b.</span></span> <span data-ttu-id="bfc2a-178">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-178">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="bfc2a-179">c.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-179">c.</span></span> <span data-ttu-id="bfc2a-180">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-180">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="bfc2a-181">d.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-181">d.</span></span> <span data-ttu-id="bfc2a-182">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-182">Click **Ok**.</span></span>     

6. <span data-ttu-id="bfc2a-183">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-183">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-litmos-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="bfc2a-185">V okně jiný prohlížeč, lokality společnosti Litmos tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-185">In a different browser window, sign-on tooyour Litmos company site as administrator.</span></span>

8. <span data-ttu-id="bfc2a-186">V navigačním panelu hello na levé straně hello, klikněte na tlačítko **účty**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-186">In hello navigation bar on hello left side, click **Accounts**.</span></span>
   
    ![Oddílu účtů na straně aplikace][22] 

9. <span data-ttu-id="bfc2a-188">Klikněte na tlačítko hello **integrace** kartě.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-188">Click hello **Integrations** tab.</span></span>
   
    ![Karta integrace][23] 

10. <span data-ttu-id="bfc2a-190">Na hello **integrace** kartě, posuňte se dolů příliš**3. stran integrace**a potom klikněte na **SAML 2.0** kartě.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-190">On hello **Integrations** tab, scroll down too**3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0 části][24] 

11. <span data-ttu-id="bfc2a-192">Zkopírujte hodnotu hello **hello SAML koncový bod je litmos:** a vložte jej do hello **adresa URL odpovědi** textového pole v hello **Litmos domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-192">Copy hello value under **hello SAML endpoint for litmos is:** and paste it into hello **Reply URL** textbox in hello **Litmos Domain and URLs** section in Azure portal.</span></span> 
   
    ![Koncový bod SAML][26] 

12. <span data-ttu-id="bfc2a-194">Ve vaší **Litmos** aplikace, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bfc2a-194">In your **Litmos** application, perform hello following steps:</span></span>
    
     ![Litmos aplikace][25] 
     
     <span data-ttu-id="bfc2a-196">a.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-196">a.</span></span> <span data-ttu-id="bfc2a-197">Klikněte na tlačítko **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-197">Click **Enable SAML**.</span></span>
    
     <span data-ttu-id="bfc2a-198">b.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-198">b.</span></span> <span data-ttu-id="bfc2a-199">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509 SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-199">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **SAML X.509 Certificate** textbox.</span></span>
     
     <span data-ttu-id="bfc2a-200">c.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-200">c.</span></span> <span data-ttu-id="bfc2a-201">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-201">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="bfc2a-202">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="bfc2a-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bfc2a-203">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bfc2a-204">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bfc2a-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="bfc2a-205">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="bfc2a-205">Create an Azure AD test user</span></span>

<span data-ttu-id="bfc2a-206">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="bfc2a-208">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bfc2a-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bfc2a-209">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-litmos-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="bfc2a-211">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-litmos-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="bfc2a-213">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="bfc2a-215">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="bfc2a-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png)

    <span data-ttu-id="bfc2a-217">a.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-217">a.</span></span> <span data-ttu-id="bfc2a-218">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bfc2a-219">b.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-219">b.</span></span> <span data-ttu-id="bfc2a-220">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="bfc2a-221">c.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-221">c.</span></span> <span data-ttu-id="bfc2a-222">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="bfc2a-223">d.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-223">d.</span></span> <span data-ttu-id="bfc2a-224">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-224">Click **Create**.</span></span>
  
### <a name="create-a-litmos-test-user"></a><span data-ttu-id="bfc2a-225">Vytvoření zkušebního uživatele Litmos</span><span class="sxs-lookup"><span data-stu-id="bfc2a-225">Create a Litmos test user</span></span>

<span data-ttu-id="bfc2a-226">Hello cílem této části je toocreate volal Britta Simon v Litmos uživatele.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-226">hello objective of this section is toocreate a user called Britta Simon in Litmos.</span></span>  
<span data-ttu-id="bfc2a-227">Hello Litmos aplikace podporuje pouze za běhu zřizování.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-227">hello Litmos application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="bfc2a-228">To znamená, uživatelský účet se automaticky vytvoří v případě potřeby během pokusu o aplikace hello tooaccess pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-228">This means, a user account is automatically created if necessary during an attempt tooaccess hello application using hello Access Panel.</span></span>

<span data-ttu-id="bfc2a-229">**toocreate uživatel volal Britta Simon v Litmos, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bfc2a-229">**toocreate a user called Britta Simon in Litmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="bfc2a-230">V okně jiný prohlížeč, lokality společnosti Litmos tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-230">In a different browser window, sign-on tooyour Litmos company site as administrator.</span></span>

2. <span data-ttu-id="bfc2a-231">V navigačním panelu hello na levé straně hello, klikněte na tlačítko **účty**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-231">In hello navigation bar on hello left side, click **Accounts**.</span></span>
   
    ![Oddílu účtů na straně aplikace][22] 

3. <span data-ttu-id="bfc2a-233">Klikněte na tlačítko hello **integrace** kartě.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-233">Click hello **Integrations** tab.</span></span>
   
    ![Karta integrace][23] 

4. <span data-ttu-id="bfc2a-235">Na hello **integrace** kartě, posuňte se dolů příliš**3. stran integrace**a potom klikněte na **SAML 2.0** kartě.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-235">On hello **Integrations** tab, scroll down too**3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0][24] 
    
5. <span data-ttu-id="bfc2a-237">Vyberte **generovat uživatelů**</span><span class="sxs-lookup"><span data-stu-id="bfc2a-237">Select **Autogenerate Users**</span></span>
   
    ![Generovat uživatelů][27] 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="bfc2a-239">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="bfc2a-239">Assign hello Azure AD test user</span></span>

<span data-ttu-id="bfc2a-240">V této části povolíte tak, že udělíte přístup tooLitmos toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLitmos.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="bfc2a-242">**tooassign Britta Simon tooLitmos, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="bfc2a-242">**tooassign Britta Simon tooLitmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="bfc2a-243">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="bfc2a-245">V seznamu aplikace hello vyberte **Litmos**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-245">In hello applications list, select **Litmos**.</span></span>

    ![v seznamu aplikace hello Hello Litmos odkaz](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_app.png)  

3. <span data-ttu-id="bfc2a-247">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="bfc2a-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-249">Click **Add** button.</span></span> <span data-ttu-id="bfc2a-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="bfc2a-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bfc2a-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bfc2a-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="bfc2a-255">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="bfc2a-255">Test single sign-on</span></span>

<span data-ttu-id="bfc2a-256">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-256">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="bfc2a-257">Po kliknutí na tlačítko hello Litmos dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Litmos aplikace.</span><span class="sxs-lookup"><span data-stu-id="bfc2a-257">When you click hello Litmos tile in hello Access Panel, you should get automatically signed-on tooyour Litmos application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bfc2a-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bfc2a-258">Additional resources</span></span>

* [<span data-ttu-id="bfc2a-259">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bfc2a-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bfc2a-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bfc2a-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png

