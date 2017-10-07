---
title: 'Kurz: Azure Active Directory integrace s Onit | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Onit."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 9e12449e5bf7f169b3cadfaa12438ac5d52ed8f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a><span data-ttu-id="f62dd-103">Kurz: Azure Active Directory integrace s Onit</span><span class="sxs-lookup"><span data-stu-id="f62dd-103">Tutorial: Azure Active Directory integration with Onit</span></span>

<span data-ttu-id="f62dd-104">V tomto kurzu zjistíte, jak toointegrate Onit s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f62dd-104">In this tutorial, you learn how toointegrate Onit with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f62dd-105">Integrace Onit s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f62dd-105">Integrating Onit with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f62dd-106">Můžete ovládat ve službě Azure AD, který má přístup tooOnit.</span><span class="sxs-lookup"><span data-stu-id="f62dd-106">You can control in Azure AD who has access tooOnit.</span></span>
- <span data-ttu-id="f62dd-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooOnit (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f62dd-107">You can enable your users tooautomatically get signed-on tooOnit (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f62dd-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f62dd-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="f62dd-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f62dd-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f62dd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f62dd-110">Prerequisites</span></span>

<span data-ttu-id="f62dd-111">Integrace služby Azure AD s Onit tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f62dd-111">tooconfigure Azure AD integration with Onit, you need hello following items:</span></span>

- <span data-ttu-id="f62dd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f62dd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f62dd-113">Onit jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f62dd-113">An Onit single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f62dd-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f62dd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f62dd-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f62dd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f62dd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f62dd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f62dd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f62dd-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f62dd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f62dd-118">Scenario description</span></span>

<span data-ttu-id="f62dd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f62dd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f62dd-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f62dd-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f62dd-121">Přidání Onit z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f62dd-121">Adding Onit from hello gallery</span></span>
2. <span data-ttu-id="f62dd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f62dd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-onit-from-hello-gallery"></a><span data-ttu-id="f62dd-123">Přidání Onit z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f62dd-123">Adding Onit from hello gallery</span></span>
<span data-ttu-id="f62dd-124">tooconfigure hello integrace Onit do Azure AD, je nutné tooadd Onit hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f62dd-124">tooconfigure hello integration of Onit into Azure AD, you need tooadd Onit from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f62dd-125">**tooadd Onit z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f62dd-125">**tooadd Onit from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f62dd-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f62dd-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="f62dd-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f62dd-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="f62dd-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f62dd-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="f62dd-133">Hello vyhledávacího pole zadejte **Onit**, vyberte **Onit** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f62dd-133">In hello search box, type **Onit**, select **Onit** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Onit v seznamu výsledků hello](./media/active-directory-saas-onit-tutorial/tutorial_onit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f62dd-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f62dd-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f62dd-136">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Onit podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f62dd-136">In this section, you configure and test Azure AD single sign-on with Onit based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f62dd-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Onit je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f62dd-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Onit is tooa user in Azure AD.</span></span> <span data-ttu-id="f62dd-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Onit musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f62dd-138">In other words, a link relationship between an Azure AD user and hello related user in Onit needs toobe established.</span></span>

<span data-ttu-id="f62dd-139">V Onit, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="f62dd-139">In Onit, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f62dd-140">tooconfigure a testu Azure AD jednotné přihlašování s Onit, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f62dd-140">tooconfigure and test Azure AD single sign-on with Onit, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f62dd-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f62dd-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f62dd-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f62dd-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f62dd-143">**[Vytvořit testovací uživatele s Onit](#create-an-onit-test-user)**  -toohave protějšek Britta Simon v Onit, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="f62dd-143">**[Create an Onit test user](#create-an-onit-test-user)** - toohave a counterpart of Britta Simon in Onit that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f62dd-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f62dd-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f62dd-145">**[Test jednotného přihlašování](#test-single-sign-on)**  tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f62dd-145">**[Test single sign-on](#test-single-sign-on)** tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f62dd-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f62dd-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f62dd-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Onit.</span><span class="sxs-lookup"><span data-stu-id="f62dd-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Onit application.</span></span>

<span data-ttu-id="f62dd-148">**tooconfigure Azure AD jednotné přihlašování s Onit, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f62dd-148">**tooconfigure Azure AD single sign-on with Onit, perform hello following steps:**</span></span>

1. <span data-ttu-id="f62dd-149">V portálu Azure, na hello hello **Onit** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-149">In hello Azure portal, on hello **Onit** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="f62dd-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f62dd-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-onit-tutorial/tutorial_onit_samlbase.png)

3. <span data-ttu-id="f62dd-153">Na hello **Onit domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f62dd-153">On hello **Onit Domain and URLs** section, perform hello following steps:</span></span>

    ![Onit domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-onit-tutorial/tutorial_onit_url.png)

    <span data-ttu-id="f62dd-155">a.</span><span class="sxs-lookup"><span data-stu-id="f62dd-155">a.</span></span> <span data-ttu-id="f62dd-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="f62dd-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<sub-domain>.onit.com`</span></span>

    <span data-ttu-id="f62dd-157">b.</span><span class="sxs-lookup"><span data-stu-id="f62dd-157">b.</span></span> <span data-ttu-id="f62dd-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="f62dd-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<sub-domain>.onit.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f62dd-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f62dd-159">These values are not real.</span></span> <span data-ttu-id="f62dd-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="f62dd-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f62dd-161">Obraťte se na [tým podpory Onit klienta](https://www.onit.com/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f62dd-161">Contact [Onit Client support team](https://www.onit.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="f62dd-162">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="f62dd-162">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-onit-tutorial/tutorial_onit_certificate.png) 

5. <span data-ttu-id="f62dd-164">Aplikace Onit očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="f62dd-164">Onit application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="f62dd-165">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f62dd-165">Please configure hello following claims for this application.</span></span> <span data-ttu-id="f62dd-166">Můžete spravovat hello hodnoty těchto atributů z hello **"Atrribute"** karta aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f62dd-166">You can manage hello values of these attributes from hello **"Atrribute"** tab of hello application.</span></span> <span data-ttu-id="f62dd-167">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="f62dd-167">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-onit-tutorial/tutorial_onit_attribute.png) 

6. <span data-ttu-id="f62dd-169">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f62dd-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="f62dd-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="f62dd-170">Attribute Name</span></span> | <span data-ttu-id="f62dd-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="f62dd-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="f62dd-172">E-mailu</span><span class="sxs-lookup"><span data-stu-id="f62dd-172">email</span></span> | <span data-ttu-id="f62dd-173">User.Mail</span><span class="sxs-lookup"><span data-stu-id="f62dd-173">user.mail</span></span> |
    
    <span data-ttu-id="f62dd-174">a.</span><span class="sxs-lookup"><span data-stu-id="f62dd-174">a.</span></span> <span data-ttu-id="f62dd-175">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f62dd-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-onit-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-onit-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="f62dd-178">b.</span><span class="sxs-lookup"><span data-stu-id="f62dd-178">b.</span></span> <span data-ttu-id="f62dd-179">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="f62dd-179">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="f62dd-180">c.</span><span class="sxs-lookup"><span data-stu-id="f62dd-180">c.</span></span> <span data-ttu-id="f62dd-181">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="f62dd-181">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="f62dd-182">d.</span><span class="sxs-lookup"><span data-stu-id="f62dd-182">d.</span></span> <span data-ttu-id="f62dd-183">Nechte hello **Namespace** prázdné.</span><span class="sxs-lookup"><span data-stu-id="f62dd-183">Leave hello **Namespace** blank.</span></span>
    
    <span data-ttu-id="f62dd-184">e.</span><span class="sxs-lookup"><span data-stu-id="f62dd-184">e.</span></span> <span data-ttu-id="f62dd-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-185">Click **Ok**.</span></span>

7. <span data-ttu-id="f62dd-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f62dd-186">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-onit-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f62dd-188">Na hello **Onit konfigurace** klikněte na tlačítko **konfigurace Onit** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f62dd-188">On hello **Onit Configuration** section, click **Configure Onit** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f62dd-189">Kopírování hello **Sign-Out adresu URL, SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f62dd-189">Copy hello **Sign-Out URL, SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Onit](./media/active-directory-saas-onit-tutorial/tutorial_onit_configure.png)

9. <span data-ttu-id="f62dd-191">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Onit.</span><span class="sxs-lookup"><span data-stu-id="f62dd-191">In a different web browser window, log into your Onit company site as an administrator.</span></span>

10. <span data-ttu-id="f62dd-192">V nabídce hello hello nahoře, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-192">In hello menu on hello top, click **Administration**.</span></span>
   
   <span data-ttu-id="f62dd-193">![Správa](./media/active-directory-saas-onit-tutorial/IC791174.png "správy")</span><span class="sxs-lookup"><span data-stu-id="f62dd-193">![Administration](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")</span></span>
11. <span data-ttu-id="f62dd-194">Klikněte na tlačítko **úpravy Corporation**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-194">Click **Edit Corporation**.</span></span>
   
   <span data-ttu-id="f62dd-195">![Upravit Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Corporation úpravy")</span><span class="sxs-lookup"><span data-stu-id="f62dd-195">![Edit Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Edit Corporation")</span></span>
   
12. <span data-ttu-id="f62dd-196">Klikněte na tlačítko hello **zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="f62dd-196">Click hello **Security** tab.</span></span>
    
    <span data-ttu-id="f62dd-197">![Informace o společnosti upravit](./media/active-directory-saas-onit-tutorial/IC791176.png "informace společnosti úpravy")</span><span class="sxs-lookup"><span data-stu-id="f62dd-197">![Edit Company Information](./media/active-directory-saas-onit-tutorial/IC791176.png "Edit Company Information")</span></span>

13. <span data-ttu-id="f62dd-198">Na hello **zabezpečení** kartu, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f62dd-198">On hello **Security** tab, perform hello following steps:</span></span>

    <span data-ttu-id="f62dd-199">![Jednotné přihlašování](./media/active-directory-saas-onit-tutorial/IC791177.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="f62dd-199">![Single Sign-On](./media/active-directory-saas-onit-tutorial/IC791177.png "Single Sign-On")</span></span>

    <span data-ttu-id="f62dd-200">a.</span><span class="sxs-lookup"><span data-stu-id="f62dd-200">a.</span></span> <span data-ttu-id="f62dd-201">Jako **strategie ověřování**, vyberte **jednotné přihlašování a heslo**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-201">As **Authentication Strategy**, select **Single Sign On and Password**.</span></span>
    
    <span data-ttu-id="f62dd-202">b.</span><span class="sxs-lookup"><span data-stu-id="f62dd-202">b.</span></span> <span data-ttu-id="f62dd-203">V **Idp cílová adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f62dd-203">In **Idp Target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f62dd-204">c.</span><span class="sxs-lookup"><span data-stu-id="f62dd-204">c.</span></span> <span data-ttu-id="f62dd-205">V **adresy URL odhlašovací Idp** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f62dd-205">In **Idp logout URL** textbox, paste hello value of  **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f62dd-206">d.</span><span class="sxs-lookup"><span data-stu-id="f62dd-206">d.</span></span> <span data-ttu-id="f62dd-207">V **Idp Cert otisk (SHA1)** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f62dd-207">In **Idp Cert Fingerprint (SHA1)** textbox, paste hello  **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="f62dd-208">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="f62dd-208">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f62dd-209">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="f62dd-209">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f62dd-210">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f62dd-210">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f62dd-211">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f62dd-211">Create an Azure AD test user</span></span>

<span data-ttu-id="f62dd-212">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f62dd-212">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="f62dd-214">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f62dd-214">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f62dd-215">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f62dd-215">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-onit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f62dd-217">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-217">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-onit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f62dd-219">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f62dd-219">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-onit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f62dd-221">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f62dd-221">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-onit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="f62dd-223">a.</span><span class="sxs-lookup"><span data-stu-id="f62dd-223">a.</span></span> <span data-ttu-id="f62dd-224">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-224">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f62dd-225">b.</span><span class="sxs-lookup"><span data-stu-id="f62dd-225">b.</span></span> <span data-ttu-id="f62dd-226">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f62dd-226">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="f62dd-227">c.</span><span class="sxs-lookup"><span data-stu-id="f62dd-227">c.</span></span> <span data-ttu-id="f62dd-228">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="f62dd-228">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="f62dd-229">d.</span><span class="sxs-lookup"><span data-stu-id="f62dd-229">d.</span></span> <span data-ttu-id="f62dd-230">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-230">Click **Create**.</span></span>
 
### <a name="create-an-onit-test-user"></a><span data-ttu-id="f62dd-231">Vytvořit uživatele s Onit testu</span><span class="sxs-lookup"><span data-stu-id="f62dd-231">Create an Onit test user</span></span>

<span data-ttu-id="f62dd-232">V pořadí tooenable Azure AD Uživatelé toolog do Onit musí být zřízená do Onit.</span><span class="sxs-lookup"><span data-stu-id="f62dd-232">In order tooenable Azure AD users toolog into Onit, they must be provisioned into Onit.</span></span>  

<span data-ttu-id="f62dd-233">V případě hello Onit zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="f62dd-233">In hello case of Onit, provisioning is a manual task.</span></span>

<span data-ttu-id="f62dd-234">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f62dd-234">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="f62dd-235">Přihlaste se tooyour **Onit** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="f62dd-235">Sign on tooyour **Onit** company site as an administrator.</span></span>
2. <span data-ttu-id="f62dd-236">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-236">Click **Add User**.</span></span>
   
   <span data-ttu-id="f62dd-237">![Správa](./media/active-directory-saas-onit-tutorial/IC791180.png "správy")</span><span class="sxs-lookup"><span data-stu-id="f62dd-237">![Administration](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")</span></span>
3. <span data-ttu-id="f62dd-238">Na hello **přidat uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f62dd-238">On hello **Add User** dialog page, perform hello following steps:</span></span>
   
   <span data-ttu-id="f62dd-239">![Přidat uživatele](./media/active-directory-saas-onit-tutorial/IC791181.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="f62dd-239">![Add User](./media/active-directory-saas-onit-tutorial/IC791181.png "Add User")</span></span>
   
  1. <span data-ttu-id="f62dd-240">Typ hello **název** a hello **e-mailovou adresu** platný Azure AD účtu chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="f62dd-240">Type hello **Name** and hello **Email Address** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span>
  2. <span data-ttu-id="f62dd-241">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-241">Click **Create**.</span></span>    
   
 > [!NOTE]
 > <span data-ttu-id="f62dd-242">Držitel účtu Azure Active Directory Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="f62dd-242">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="f62dd-243">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f62dd-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="f62dd-244">V této části povolíte tak, že udělíte přístup tooOnit toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f62dd-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOnit.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="f62dd-246">**tooassign Britta Simon tooOnit, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f62dd-246">**tooassign Britta Simon tooOnit, perform hello following steps:**</span></span>

1. <span data-ttu-id="f62dd-247">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f62dd-249">V seznamu aplikace hello vyberte **Onit**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-249">In hello applications list, select **Onit**.</span></span>

    ![v seznamu aplikace hello Hello Onit odkaz](./media/active-directory-saas-onit-tutorial/tutorial_onit_app.png)  

3. <span data-ttu-id="f62dd-251">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f62dd-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="f62dd-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f62dd-253">Click **Add** button.</span></span> <span data-ttu-id="f62dd-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f62dd-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="f62dd-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f62dd-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f62dd-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f62dd-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f62dd-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f62dd-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f62dd-259">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f62dd-259">Test single sign-on</span></span>

<span data-ttu-id="f62dd-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f62dd-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f62dd-261">Když kliknete na dlaždici Onit hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Onit aplikace.</span><span class="sxs-lookup"><span data-stu-id="f62dd-261">When you click hello Onit tile in hello Access Panel, you should get automatically signed-on tooyour Onit application.</span></span>
<span data-ttu-id="f62dd-262">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f62dd-262">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f62dd-263">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f62dd-263">Additional resources</span></span>

* [<span data-ttu-id="f62dd-264">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f62dd-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f62dd-265">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f62dd-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-onit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-onit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-onit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-onit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-onit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-onit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-onit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-onit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-onit-tutorial/tutorial_general_203.png

