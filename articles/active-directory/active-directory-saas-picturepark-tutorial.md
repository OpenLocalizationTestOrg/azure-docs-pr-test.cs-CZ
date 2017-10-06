---
title: 'Kurz: Azure Active Directory integrace s Picturepark | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Picturepark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 3d826d3f73aad2f0d123f8697c6caafad7bc926a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a><span data-ttu-id="56fb3-103">Kurz: Azure Active Directory integrace s Picturepark</span><span class="sxs-lookup"><span data-stu-id="56fb3-103">Tutorial: Azure Active Directory integration with Picturepark</span></span>

<span data-ttu-id="56fb3-104">V tomto kurzu zjistíte, jak toointegrate Picturepark s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="56fb3-104">In this tutorial, you learn how toointegrate Picturepark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="56fb3-105">Integrace Picturepark s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="56fb3-105">Integrating Picturepark with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="56fb3-106">Můžete řídit ve službě Azure AD, který má přístup tooPicturepark</span><span class="sxs-lookup"><span data-stu-id="56fb3-106">You can control in Azure AD who has access tooPicturepark</span></span>
- <span data-ttu-id="56fb3-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPicturepark (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="56fb3-107">You can enable your users tooautomatically get signed-on tooPicturepark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="56fb3-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="56fb3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="56fb3-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="56fb3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56fb3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="56fb3-110">Prerequisites</span></span>

<span data-ttu-id="56fb3-111">Integrace služby Azure AD s Picturepark tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="56fb3-111">tooconfigure Azure AD integration with Picturepark, you need hello following items:</span></span>

- <span data-ttu-id="56fb3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="56fb3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="56fb3-113">Picturepark jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="56fb3-113">A Picturepark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="56fb3-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="56fb3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="56fb3-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="56fb3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="56fb3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="56fb3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="56fb3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="56fb3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="56fb3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="56fb3-118">Scenario description</span></span>
<span data-ttu-id="56fb3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="56fb3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="56fb3-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="56fb3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="56fb3-121">Přidání Picturepark z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="56fb3-121">Adding Picturepark from hello gallery</span></span>
2. <span data-ttu-id="56fb3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="56fb3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-picturepark-from-hello-gallery"></a><span data-ttu-id="56fb3-123">Přidání Picturepark z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="56fb3-123">Adding Picturepark from hello gallery</span></span>
<span data-ttu-id="56fb3-124">tooconfigure hello integrace Picturepark do Azure AD, je nutné tooadd Picturepark hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="56fb3-124">tooconfigure hello integration of Picturepark into Azure AD, you need tooadd Picturepark from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="56fb3-125">**tooadd Picturepark z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="56fb3-125">**tooadd Picturepark from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="56fb3-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="56fb3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="56fb3-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="56fb3-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="56fb3-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="56fb3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="56fb3-133">Hello vyhledávacího pole zadejte **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-133">In hello search box, type **Picturepark**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. <span data-ttu-id="56fb3-135">Na panelu výsledků hello vyberte **Picturepark**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="56fb3-135">In hello results panel, select **Picturepark**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="56fb3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="56fb3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="56fb3-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Picturepark podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="56fb3-138">In this section, you configure and test Azure AD single sign-on with Picturepark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="56fb3-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Picturepark je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56fb3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Picturepark is tooa user in Azure AD.</span></span> <span data-ttu-id="56fb3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Picturepark musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="56fb3-140">In other words, a link relationship between an Azure AD user and hello related user in Picturepark needs toobe established.</span></span>

<span data-ttu-id="56fb3-141">V Picturepark, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="56fb3-141">In Picturepark, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="56fb3-142">tooconfigure a testu Azure AD jednotné přihlašování s Picturepark, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="56fb3-142">tooconfigure and test Azure AD single sign-on with Picturepark, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="56fb3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="56fb3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="56fb3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="56fb3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="56fb3-145">**[Vytvoření zkušebního uživatele Picturepark](#creating-a-picturepark-test-user)**  -toohave protějšek Britta Simon v Picturepark, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="56fb3-145">**[Creating a Picturepark test user](#creating-a-picturepark-test-user)** - toohave a counterpart of Britta Simon in Picturepark that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="56fb3-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="56fb3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="56fb3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="56fb3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="56fb3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="56fb3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="56fb3-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Picturepark.</span><span class="sxs-lookup"><span data-stu-id="56fb3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Picturepark application.</span></span>

<span data-ttu-id="56fb3-150">**tooconfigure Azure AD jednotné přihlašování s Picturepark, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="56fb3-150">**tooconfigure Azure AD single sign-on with Picturepark, perform hello following steps:**</span></span>

1. <span data-ttu-id="56fb3-151">V portálu Azure, na hello hello **Picturepark** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-151">In hello Azure portal, on hello **Picturepark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="56fb3-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="56fb3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. <span data-ttu-id="56fb3-155">Na hello **Picturepark domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="56fb3-155">On hello **Picturepark Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    <span data-ttu-id="56fb3-157">a.</span><span class="sxs-lookup"><span data-stu-id="56fb3-157">a.</span></span> <span data-ttu-id="56fb3-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.picturepark.com`</span><span class="sxs-lookup"><span data-stu-id="56fb3-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.picturepark.com`</span></span>

    <span data-ttu-id="56fb3-159">b.</span><span class="sxs-lookup"><span data-stu-id="56fb3-159">b.</span></span> <span data-ttu-id="56fb3-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="56fb3-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span> 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > <span data-ttu-id="56fb3-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="56fb3-161">These values are not real.</span></span> <span data-ttu-id="56fb3-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="56fb3-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="56fb3-163">Obraťte se na [tým podpory Picturepark klienta](https://picturepark.com/about/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="56fb3-163">Contact [Picturepark Client support team](https://picturepark.com/about/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="56fb3-164">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="56fb3-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. <span data-ttu-id="56fb3-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="56fb3-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="56fb3-168">Na hello **Picturepark konfigurace** klikněte na tlačítko **konfigurace Picturepark** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="56fb3-168">On hello **Picturepark Configuration** section, click **Configure Picturepark** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="56fb3-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="56fb3-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. <span data-ttu-id="56fb3-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Picturepark.</span><span class="sxs-lookup"><span data-stu-id="56fb3-171">In a different web browser window, log into your Picturepark company site as an administrator.</span></span>

8. <span data-ttu-id="56fb3-172">V panelu nástrojů hello hello nahoře, klikněte na **nástroje pro správu**a potom klikněte na **konzoly pro správu**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-172">In hello toolbar on hello top, click **Administrative tools**, and then click **Management Console**.</span></span>
   
    <span data-ttu-id="56fb3-173">![Konzola pro správu](./media/active-directory-saas-picturepark-tutorial/ic795062.png "konzoly pro správu")</span><span class="sxs-lookup"><span data-stu-id="56fb3-173">![Management Console](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")</span></span>

9. <span data-ttu-id="56fb3-174">Klikněte na tlačítko **ověřování**a potom klikněte na **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-174">Click **Authentication**, and then click **Identity providers**.</span></span>
   
    <span data-ttu-id="56fb3-175">![Ověřování](./media/active-directory-saas-picturepark-tutorial/ic795063.png "ověřování")</span><span class="sxs-lookup"><span data-stu-id="56fb3-175">![Authentication](./media/active-directory-saas-picturepark-tutorial/ic795063.png "Authentication")</span></span>

10. <span data-ttu-id="56fb3-176">V hello **konfigurace zprostředkovatele Identity** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="56fb3-176">In hello **Identity provider configuration** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="56fb3-177">![Konfigurace zprostředkovatele identity](./media/active-directory-saas-picturepark-tutorial/ic795064.png "konfigurace zprostředkovatele Identity")</span><span class="sxs-lookup"><span data-stu-id="56fb3-177">![Identity provider configuration](./media/active-directory-saas-picturepark-tutorial/ic795064.png "Identity provider configuration")</span></span>
   
    <span data-ttu-id="56fb3-178">a.</span><span class="sxs-lookup"><span data-stu-id="56fb3-178">a.</span></span> <span data-ttu-id="56fb3-179">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-179">Click **Add**.</span></span>
  
    <span data-ttu-id="56fb3-180">b.</span><span class="sxs-lookup"><span data-stu-id="56fb3-180">b.</span></span> <span data-ttu-id="56fb3-181">Zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="56fb3-181">Type a name for your configuration.</span></span>
   
    <span data-ttu-id="56fb3-182">c.</span><span class="sxs-lookup"><span data-stu-id="56fb3-182">c.</span></span> <span data-ttu-id="56fb3-183">Vyberte **nastavit jako výchozí**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-183">Select **Set as default**.</span></span>
   
    <span data-ttu-id="56fb3-184">d.</span><span class="sxs-lookup"><span data-stu-id="56fb3-184">d.</span></span> <span data-ttu-id="56fb3-185">V **vystavitele URI** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="56fb3-185">In **Issuer URI** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="56fb3-186">e.</span><span class="sxs-lookup"><span data-stu-id="56fb3-186">e.</span></span> <span data-ttu-id="56fb3-187">V **důvěryhodné vystavitele jezdec tiskových** textovému poli, vložte hodnotu hello **kryptografický otisk** který jste zkopírovali z **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="56fb3-187">In **Trusted Issuer Thumb Print** textbox, paste hello value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span> 

11. <span data-ttu-id="56fb3-188">Klikněte na tlačítko **JoinDefaultUsersGroup**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-188">Click **JoinDefaultUsersGroup**.</span></span>

12. <span data-ttu-id="56fb3-189">tooset hello **Emailaddress** atribut v hello **deklarace identity** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-189">tooset hello **Emailaddress** attribute in hello **Claim** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` and click **Save**.</span></span>

      <span data-ttu-id="56fb3-190">![Konfigurace](./media/active-directory-saas-picturepark-tutorial/ic795065.png "konfigurace")</span><span class="sxs-lookup"><span data-stu-id="56fb3-190">![Configuration](./media/active-directory-saas-picturepark-tutorial/ic795065.png "Configuration")</span></span>

> [!TIP]
> <span data-ttu-id="56fb3-191">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="56fb3-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="56fb3-192">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="56fb3-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="56fb3-193">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="56fb3-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="56fb3-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="56fb3-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="56fb3-195">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="56fb3-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="56fb3-197">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="56fb3-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="56fb3-198">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="56fb3-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="56fb3-200">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="56fb3-202">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="56fb3-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="56fb3-204">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="56fb3-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="56fb3-206">a.</span><span class="sxs-lookup"><span data-stu-id="56fb3-206">a.</span></span> <span data-ttu-id="56fb3-207">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="56fb3-208">b.</span><span class="sxs-lookup"><span data-stu-id="56fb3-208">b.</span></span> <span data-ttu-id="56fb3-209">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="56fb3-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="56fb3-210">c.</span><span class="sxs-lookup"><span data-stu-id="56fb3-210">c.</span></span> <span data-ttu-id="56fb3-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="56fb3-212">d.</span><span class="sxs-lookup"><span data-stu-id="56fb3-212">d.</span></span> <span data-ttu-id="56fb3-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-213">Click **Create**.</span></span>
 
### <a name="creating-a-picturepark-test-user"></a><span data-ttu-id="56fb3-214">Vytvoření zkušebního uživatele Picturepark</span><span class="sxs-lookup"><span data-stu-id="56fb3-214">Creating a Picturepark test user</span></span>

<span data-ttu-id="56fb3-215">V pořadí tooenable Azure AD Uživatelé toolog do Picturepark musí být zřízená do Picturepark.</span><span class="sxs-lookup"><span data-stu-id="56fb3-215">In order tooenable Azure AD users toolog into Picturepark, they must be provisioned into Picturepark.</span></span> <span data-ttu-id="56fb3-216">V případě hello Picturepark zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="56fb3-216">In hello case of Picturepark, provisioning is a manual task.</span></span>

<span data-ttu-id="56fb3-217">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="56fb3-217">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="56fb3-218">Přihlaste se tooyour **Picturepark** klienta.</span><span class="sxs-lookup"><span data-stu-id="56fb3-218">Log in tooyour **Picturepark** tenant.</span></span>

2. <span data-ttu-id="56fb3-219">V panelu nástrojů hello hello nahoře, klikněte na **nástroje pro správu**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-219">In hello toolbar on hello top, click **Administrative tools**, and then click **Users**.</span></span>
   
    <span data-ttu-id="56fb3-220">![Uživatelé](./media/active-directory-saas-picturepark-tutorial/ic795067.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="56fb3-220">![Users](./media/active-directory-saas-picturepark-tutorial/ic795067.png "Users")</span></span>

3. <span data-ttu-id="56fb3-221">V hello **přehled uživatelů** , klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-221">In hello **Users overview** tab, click **New**.</span></span>
   
    <span data-ttu-id="56fb3-222">![Správa uživatelů](./media/active-directory-saas-picturepark-tutorial/ic795068.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="56fb3-222">![User management](./media/active-directory-saas-picturepark-tutorial/ic795068.png "User management")</span></span>

4. <span data-ttu-id="56fb3-223">Na hello **vytvořit uživatele** dialogu hello provést následující kroky platného Azure Active Directory uživatele chcete tooprovision:</span><span class="sxs-lookup"><span data-stu-id="56fb3-223">On hello **Create User** dialog, perform hello following steps of a valid Azure Active Directory User you want tooprovision:</span></span>
   
    <span data-ttu-id="56fb3-224">![Vytvoření uživatele](./media/active-directory-saas-picturepark-tutorial/ic795069.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="56fb3-224">![Create User](./media/active-directory-saas-picturepark-tutorial/ic795069.png "Create User")</span></span>
   
    <span data-ttu-id="56fb3-225">a.</span><span class="sxs-lookup"><span data-stu-id="56fb3-225">a.</span></span> <span data-ttu-id="56fb3-226">V hello **e-mailovou adresu** textovému poli, typ hello **e-mailová adresa** hello uživatele  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="56fb3-226">In hello **Email Address** textbox, type hello **email address** of hello user **BrittaSimon@contoso.com**.</span></span>  
   
    <span data-ttu-id="56fb3-227">b.</span><span class="sxs-lookup"><span data-stu-id="56fb3-227">b.</span></span> <span data-ttu-id="56fb3-228">V hello **heslo** a **Potvrdit heslo** textová pole, typ hello **heslo** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="56fb3-228">In hello **Password** and **Confirm Password** textboxes, type hello **password** of BrittaSimon.</span></span> 
   
    <span data-ttu-id="56fb3-229">c.</span><span class="sxs-lookup"><span data-stu-id="56fb3-229">c.</span></span> <span data-ttu-id="56fb3-230">V hello **křestní jméno** textovému poli, typ hello **křestní jméno** hello uživatele **Britta**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-230">In hello **First Name** textbox, type hello **First Name** of hello user **Britta**.</span></span> 
   
    <span data-ttu-id="56fb3-231">d.</span><span class="sxs-lookup"><span data-stu-id="56fb3-231">d.</span></span> <span data-ttu-id="56fb3-232">V hello **příjmení** textovému poli, typ hello **příjmení** hello uživatele **Simon**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-232">In hello **Last Name** textbox, type hello **Last Name** of hello user **Simon**.</span></span>
   
    <span data-ttu-id="56fb3-233">e.</span><span class="sxs-lookup"><span data-stu-id="56fb3-233">e.</span></span> <span data-ttu-id="56fb3-234">V hello **společnosti** textovému poli, typ hello **název společnosti** hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="56fb3-234">In hello **Company** textbox, type hello **Company name** of hello user.</span></span> 
   
    <span data-ttu-id="56fb3-235">f.</span><span class="sxs-lookup"><span data-stu-id="56fb3-235">f.</span></span> <span data-ttu-id="56fb3-236">V hello **země** textovému poli, vyberte hello **země** hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="56fb3-236">In hello **Country** textbox, select hello **Country** of hello user.</span></span>
  
    <span data-ttu-id="56fb3-237">g.</span><span class="sxs-lookup"><span data-stu-id="56fb3-237">g.</span></span> <span data-ttu-id="56fb3-238">V hello **ZIP** textovému poli, typ hello **PSČ** města hello.</span><span class="sxs-lookup"><span data-stu-id="56fb3-238">In hello **ZIP** textbox, type hello **ZIP code** of hello city.</span></span>
   
    <span data-ttu-id="56fb3-239">h.</span><span class="sxs-lookup"><span data-stu-id="56fb3-239">h.</span></span> <span data-ttu-id="56fb3-240">V hello **města** textovému poli, typ hello **název města** hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="56fb3-240">In hello **City** textbox, type hello **City name** of hello user.</span></span>

    <span data-ttu-id="56fb3-241">i.</span><span class="sxs-lookup"><span data-stu-id="56fb3-241">i.</span></span> <span data-ttu-id="56fb3-242">Vyberte **jazyk**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-242">Select a **Language**.</span></span>
   
    <span data-ttu-id="56fb3-243">j.</span><span class="sxs-lookup"><span data-stu-id="56fb3-243">j.</span></span> <span data-ttu-id="56fb3-244">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-244">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="56fb3-245">Můžete použít všechny ostatní Picturepark uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Picturepark tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56fb3-245">You can use any other Picturepark user account creation tools or APIs provided by Picturepark tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="56fb3-246">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="56fb3-246">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="56fb3-247">V této části povolíte tak, že udělíte přístup tooPicturepark toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="56fb3-247">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPicturepark.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="56fb3-249">**tooassign Britta Simon tooPicturepark, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="56fb3-249">**tooassign Britta Simon tooPicturepark, perform hello following steps:**</span></span>

1. <span data-ttu-id="56fb3-250">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-250">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="56fb3-252">V seznamu aplikace hello vyberte **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-252">In hello applications list, select **Picturepark**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. <span data-ttu-id="56fb3-254">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="56fb3-254">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="56fb3-256">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="56fb3-256">Click **Add** button.</span></span> <span data-ttu-id="56fb3-257">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="56fb3-257">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="56fb3-259">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="56fb3-259">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="56fb3-260">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="56fb3-260">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="56fb3-261">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="56fb3-261">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="56fb3-262">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="56fb3-262">Testing single sign-on</span></span>

<span data-ttu-id="56fb3-263">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="56fb3-263">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="56fb3-264">Když kliknete na dlaždici Picturepark hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Picturepark aplikace.</span><span class="sxs-lookup"><span data-stu-id="56fb3-264">When you click hello Picturepark tile in hello Access Panel, you should get automatically signed-on tooyour Picturepark application.</span></span> <span data-ttu-id="56fb3-265">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="56fb3-265">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56fb3-266">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="56fb3-266">Additional resources</span></span>

* [<span data-ttu-id="56fb3-267">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="56fb3-267">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="56fb3-268">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="56fb3-268">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png

