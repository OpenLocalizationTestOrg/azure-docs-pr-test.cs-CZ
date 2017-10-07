---
title: 'Kurz: Azure Active Directory integrace s ScaleX Enterprise | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ScaleX Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: e398b98d9e0957b5f92c82359651c345d22c3a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a><span data-ttu-id="0018a-103">Kurz: Azure Active Directory integrace s ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="0018a-103">Tutorial: Azure Active Directory integration with ScaleX Enterprise</span></span>

<span data-ttu-id="0018a-104">V tomto kurzu zjistíte, jak toointegrate ScaleX Enterprise s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0018a-104">In this tutorial, you learn how toointegrate ScaleX Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0018a-105">Integrace ScaleX Enterprise s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0018a-105">Integrating ScaleX Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0018a-106">Můžete řídit ve službě Azure AD, který má přístup tooScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="0018a-106">You can control in Azure AD who has access tooScaleX Enterprise</span></span>
- <span data-ttu-id="0018a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooScaleX Enterprise (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0018a-107">You can enable your users tooautomatically get signed-on tooScaleX Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0018a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0018a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0018a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="0018a-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="0018a-110">Co je přístup k aplikaci a jednotné přihlašování s [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0018a-110">What is application access and single sign-on with [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0018a-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0018a-111">Prerequisites</span></span>

<span data-ttu-id="0018a-112">tooconfigure integrace Azure AD s ScaleX Enterprise, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="0018a-112">tooconfigure Azure AD integration with ScaleX Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="0018a-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0018a-113">An Azure AD subscription</span></span>
- <span data-ttu-id="0018a-114">ScaleX Enterprise jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0018a-114">A ScaleX Enterprise single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0018a-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0018a-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0018a-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0018a-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0018a-117">Nepoužívejte provozním prostředí, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="0018a-117">Do not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="0018a-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0018a-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0018a-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0018a-119">Scenario description</span></span>
<span data-ttu-id="0018a-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0018a-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0018a-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0018a-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0018a-122">Přidání ScaleX Enterprise z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0018a-122">Adding ScaleX Enterprise from hello gallery</span></span>
2. <span data-ttu-id="0018a-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0018a-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-scalex-enterprise-from-hello-gallery"></a><span data-ttu-id="0018a-124">Přidání ScaleX Enterprise z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0018a-124">Adding ScaleX Enterprise from hello gallery</span></span>
<span data-ttu-id="0018a-125">tooconfigure hello integrace ScaleX Enterprise v tooAzure AD, je nutné tooadd ScaleX Enterprise hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0018a-125">tooconfigure hello integration of ScaleX Enterprise in tooAzure AD, you need tooadd ScaleX Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0018a-126">**tooadd ScaleX Enterprise z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0018a-126">**tooadd ScaleX Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0018a-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0018a-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0018a-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0018a-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0018a-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0018a-130">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0018a-132">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0018a-132">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0018a-134">Hello vyhledávacího pole zadejte **ScaleX Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="0018a-134">In hello search box, type **ScaleX Enterprise**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. <span data-ttu-id="0018a-136">Na panelu výsledků hello vyberte **ScaleX Enterprise**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0018a-136">In hello results panel, select **ScaleX Enterprise**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0018a-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0018a-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0018a-139">V této části nakonfigurujete a testovací Azure AD jednotného přihlašování k ScaleX organizace podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0018a-139">In this section, you configure and test Azure AD single sign-on with ScaleX Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0018a-140">Pro toowork jeden přihlašování Azure AD musí tooknow hello protějšku uživatele v podniku ScaleX je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0018a-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ScaleX Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="0018a-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v podniku ScaleX musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="0018a-141">In other words, a link relationship between an Azure AD user and hello related user in ScaleX Enterprise needs toobe established.</span></span>

<span data-ttu-id="0018a-142">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** ve ScaleX podniku.</span><span class="sxs-lookup"><span data-stu-id="0018a-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ScaleX Enterprise.</span></span>

<span data-ttu-id="0018a-143">tooconfigure a testování Azure AD jednotného přihlašování k ScaleX organizace, budete potřebovat následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="0018a-143">tooconfigure and test Azure AD single sign-on with ScaleX Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0018a-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="0018a-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0018a-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0018a-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0018a-146">**[Vytvoření zkušebního uživatele ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  -toohave protějšek Britta Simon v ScaleX organizace, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="0018a-146">**[Creating a ScaleX Enterprise test user](#creating-a-scalex-enterprise-test-user)** - toohave a counterpart of Britta Simon in ScaleX Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0018a-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0018a-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0018a-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="0018a-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0018a-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0018a-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0018a-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ScaleX Enterprise.</span><span class="sxs-lookup"><span data-stu-id="0018a-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ScaleX Enterprise application.</span></span>

<span data-ttu-id="0018a-151">**tooconfigure Azure AD jednotné přihlašování s ScaleX Enterprise, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0018a-151">**tooconfigure Azure AD single sign-on with ScaleX Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="0018a-152">V portálu Azure, na hello hello **ScaleX Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0018a-152">In hello Azure portal, on hello **ScaleX Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0018a-154">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0018a-154">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. <span data-ttu-id="0018a-156">Na hello **ScaleX podnikové domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="0018a-156">On hello **ScaleX Enterprise Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    <span data-ttu-id="0018a-158">a.</span><span class="sxs-lookup"><span data-stu-id="0018a-158">a.</span></span> <span data-ttu-id="0018a-159">V hello **identifikátor** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://platform.rescale.com/saml2/<company id>/`</span><span class="sxs-lookup"><span data-stu-id="0018a-159">In hello **Identifier** textbox, type hello value using hello following pattern: `https://platform.rescale.com/saml2/<company id>/`</span></span>

    <span data-ttu-id="0018a-160">b.</span><span class="sxs-lookup"><span data-stu-id="0018a-160">b.</span></span> <span data-ttu-id="0018a-161">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://platform.rescale.com/saml2/<company id>/acs/`</span><span class="sxs-lookup"><span data-stu-id="0018a-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://platform.rescale.com/saml2/<company id>/acs/`</span></span>

4. <span data-ttu-id="0018a-162">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="0018a-162">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    <span data-ttu-id="0018a-164">V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://platform.rescale.com/saml2/<company id>/sso/`</span><span class="sxs-lookup"><span data-stu-id="0018a-164">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://platform.rescale.com/saml2/<company id>/sso/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="0018a-165">Tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0018a-165">These are not hello real values.</span></span> <span data-ttu-id="0018a-166">Tyto hodnoty aktualizujte s hello skutečné identifikátor, adresa URL odpovědi nebo adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0018a-166">Update these values with hello actual Identifier, Reply URL or Sign-On URL.</span></span> <span data-ttu-id="0018a-167">Obraťte se na [tým podpory klient systému Enterprise ScaleX](http://info.rescale.com/contact_sales) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0018a-167">Contact [ScaleX Enterprise Client support team](http://info.rescale.com/contact_sales) tooget these values.</span></span> 

5. <span data-ttu-id="0018a-168">Aplikace ScaleX očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste toomodify vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="0018a-168">Your ScaleX application expects hello SAML assertions in a specific format, which requires you toomodify custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="0018a-169">Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** hello tooopen políčko vlastní atributy nastavení.</span><span class="sxs-lookup"><span data-stu-id="0018a-169">Click **View and edit all other user attributes** checkbox tooopen hello custom attributes settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    <span data-ttu-id="0018a-171">a.</span><span class="sxs-lookup"><span data-stu-id="0018a-171">a.</span></span> <span data-ttu-id="0018a-172">Klikněte pravým tlačítkem na atribut hello **název** a klikněte na příkaz odstranit.</span><span class="sxs-lookup"><span data-stu-id="0018a-172">Right click hello attribute **name** and click delete.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    <span data-ttu-id="0018a-174">b.</span><span class="sxs-lookup"><span data-stu-id="0018a-174">b.</span></span> <span data-ttu-id="0018a-175">Klikněte na tlačítko **emailaddress** atribut tooopen hello Upravit atribut okno.</span><span class="sxs-lookup"><span data-stu-id="0018a-175">Click **emailaddress** attribute tooopen hello Edit Attribute window.</span></span> <span data-ttu-id="0018a-176">Změnit svou hodnotu z **user.mail** příliš**user.userprincipalname** a klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="0018a-176">Change its value from **user.mail** too**user.userprincipalname** and click Ok.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. <span data-ttu-id="0018a-178">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0018a-178">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. <span data-ttu-id="0018a-180">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0018a-180">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="0018a-182">Na hello **ScaleX podniková konfigurace** klikněte na tlačítko **konfigurace ScaleX Enterprise** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0018a-182">On hello **ScaleX Enterprise Configuration** section, click **Configure ScaleX Enterprise** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0018a-183">Kopírování hello **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0018a-183">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. <span data-ttu-id="0018a-185">tooconfigure jednotného přihlašování na **ScaleX Enterprise** straně, web společnosti ScaleX Enterprise toohello přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="0018a-185">tooconfigure single sign-on on **ScaleX Enterprise** side, login toohello ScaleX Enterprise company website as an administrator.</span></span>

9. <span data-ttu-id="0018a-186">V nabídce hello v hello horní pravé a vyberte **Contoso správy**.</span><span class="sxs-lookup"><span data-stu-id="0018a-186">Click hello menu in hello upper right and select **Contoso Administration**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0018a-187">Contoso je jenom jako příklad.</span><span class="sxs-lookup"><span data-stu-id="0018a-187">Contoso is just an example.</span></span> <span data-ttu-id="0018a-188">To by měl být skutečný název vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="0018a-188">This should be your actual Company Name.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. <span data-ttu-id="0018a-190">Vyberte **integrace** hello horní nabídce a výběrem **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0018a-190">Select **Integrations** from hello top menu and select **Single Sign-On**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. <span data-ttu-id="0018a-192">Vyplňte formulář hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0018a-192">Complete hello form as follows:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    <span data-ttu-id="0018a-194">a.</span><span class="sxs-lookup"><span data-stu-id="0018a-194">a.</span></span> <span data-ttu-id="0018a-195">Vyberte **"Vytvořit každý uživatel, který může ověřit pomocí jednotného přihlašování."**</span><span class="sxs-lookup"><span data-stu-id="0018a-195">Select **“Create any user who can authenticate with SSO.”**</span></span>

    <span data-ttu-id="0018a-196">b.</span><span class="sxs-lookup"><span data-stu-id="0018a-196">b.</span></span> <span data-ttu-id="0018a-197">**Poskytovatel služeb saml**: Vložit hodnotu hello ***urn: oasis: názvy: tc: SAML:2.0:nameid-formátu: trvalé***</span><span class="sxs-lookup"><span data-stu-id="0018a-197">**Service Provider saml**: Paste hello value ***urn:oasis:names:tc:SAML:2.0:nameid-format:persistent***</span></span>

    <span data-ttu-id="0018a-198">c.</span><span class="sxs-lookup"><span data-stu-id="0018a-198">c.</span></span> <span data-ttu-id="0018a-199">**Název pole zprostředkovatele Identity e-mailu v odpovědi ACS**: Vložit hodnotu hello`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="0018a-199">**Name of Identity Provider email field in ACS response**: Paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>

    <span data-ttu-id="0018a-200">d.</span><span class="sxs-lookup"><span data-stu-id="0018a-200">d.</span></span> <span data-ttu-id="0018a-201">**ID EntityDescriptor Entity zprostředkovatele identity:** vložení hello **SAML Entity ID** hodnota zkopírována z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0018a-201">**Identity Provider EntityDescriptor Entity ID:** Paste hello **SAML Entity ID** value copied from hello Azure portal.</span></span>

    <span data-ttu-id="0018a-202">e.</span><span class="sxs-lookup"><span data-stu-id="0018a-202">e.</span></span> <span data-ttu-id="0018a-203">**Adresa URL SingleSignOnService zprostředkovatele identity:** vložení hello **SAML jeden přihlašování adresa URL služby** z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0018a-203">**Identity Provider SingleSignOnService URL:** Paste hello **SAML Single Sign-On Service URL** from hello Azure portal.</span></span>

    <span data-ttu-id="0018a-204">f.</span><span class="sxs-lookup"><span data-stu-id="0018a-204">f.</span></span> <span data-ttu-id="0018a-205">**Certifikát identity poskytovatele veřejných X509:** otevřete hello X509 certifikát stažený z hello Azure v programu Poznámkový blok a vložit obsah hello v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="0018a-205">**Identity Provider public X509 certificate:** Open hello X509 certificate downloaded from hello Azure in notepad and paste hello contents in this box.</span></span> <span data-ttu-id="0018a-206">Zajistěte, že žádná zalomení řádků hello střední hello obsahu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="0018a-206">Ensure there are no line breaks in hello middle of hello certificate contents.</span></span>
    
    <span data-ttu-id="0018a-207">g.</span><span class="sxs-lookup"><span data-stu-id="0018a-207">g.</span></span> <span data-ttu-id="0018a-208">Zaškrtněte následující zaškrtávací políčka hello: **povoleno, šifrování NameID a AuthnRequests přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="0018a-208">Check hello following checkboxes: **Enabled, Encrypt NameID and Sign AuthnRequests.**</span></span>

    <span data-ttu-id="0018a-209">h.</span><span class="sxs-lookup"><span data-stu-id="0018a-209">h.</span></span> <span data-ttu-id="0018a-210">Klikněte na tlačítko **nastavení jednotného přihlašování k aktualizaci** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="0018a-210">Click **Update SSO Settings** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="0018a-211">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="0018a-211">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0018a-212">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="0018a-212">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0018a-213">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0018a-213">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0018a-214">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0018a-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="0018a-215">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="0018a-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0018a-217">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0018a-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0018a-218">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0018a-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0018a-220">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0018a-220">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0018a-222">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0018a-222">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0018a-224">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0018a-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0018a-226">a.</span><span class="sxs-lookup"><span data-stu-id="0018a-226">a.</span></span> <span data-ttu-id="0018a-227">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0018a-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0018a-228">b.</span><span class="sxs-lookup"><span data-stu-id="0018a-228">b.</span></span> <span data-ttu-id="0018a-229">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0018a-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0018a-230">c.</span><span class="sxs-lookup"><span data-stu-id="0018a-230">c.</span></span> <span data-ttu-id="0018a-231">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0018a-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0018a-232">d.</span><span class="sxs-lookup"><span data-stu-id="0018a-232">d.</span></span> <span data-ttu-id="0018a-233">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0018a-233">Click **Create**.</span></span>
 
### <a name="creating-a-scalex-enterprise-test-user"></a><span data-ttu-id="0018a-234">Vytvoření zkušebního uživatele ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="0018a-234">Creating a ScaleX Enterprise test user</span></span>

<span data-ttu-id="0018a-235">Uživatelé toolog tooenable Azure AD v tooScaleX Enterprise, se musí být zřízená v tooScaleX Enterprise.</span><span class="sxs-lookup"><span data-stu-id="0018a-235">tooenable Azure AD users toolog in tooScaleX Enterprise, they must be provisioned in tooScaleX Enterprise.</span></span> <span data-ttu-id="0018a-236">V případě hello ScaleX podniku zřizování je automatická úloha a nejsou potřeba žádné ruční kroky.</span><span class="sxs-lookup"><span data-stu-id="0018a-236">In hello case of ScaleX Enterprise, provisioning is an automatic task and no manual steps are required.</span></span> <span data-ttu-id="0018a-237">Každý uživatel, který může úspěšně ověřit pomocí přihlašovacích údajů jednotné přihlašování se automaticky zřídí na hello ScaleX straně.</span><span class="sxs-lookup"><span data-stu-id="0018a-237">Any user who can successfully authenticate with SSO credentials will be automatically provisioned on hello ScaleX side.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0018a-238">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="0018a-238">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0018a-239">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte Enterprise tooScaleX přístupu uživatele.</span><span class="sxs-lookup"><span data-stu-id="0018a-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting user access tooScaleX Enterprise.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0018a-241">**tooassign Britta Simon tooScaleX Enterprise, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0018a-241">**tooassign Britta Simon tooScaleX Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="0018a-242">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0018a-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0018a-244">V seznamu aplikace hello vyberte **ScaleX Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="0018a-244">In hello applications list, select **ScaleX Enterprise**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. <span data-ttu-id="0018a-246">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0018a-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0018a-248">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0018a-248">Click **Add** button.</span></span> <span data-ttu-id="0018a-249">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0018a-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0018a-251">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="0018a-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0018a-252">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0018a-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0018a-253">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0018a-253">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="0018a-254">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0018a-254">Testing single sign-on</span></span>

<span data-ttu-id="0018a-255">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0018a-255">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0018a-256">Klikněte na tlačítko hello ScaleX Enterprise dlaždici v hello přístupového panelu, zobrazí se automaticky přihlášeného tooyour ScaleX podniková aplikace.</span><span class="sxs-lookup"><span data-stu-id="0018a-256">Click hello ScaleX Enterprise tile in hello Access Panel, you will get automatically signed-on tooyour ScaleX Enterprise application.</span></span> <span data-ttu-id="0018a-257">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="0018a-257">For more information about hello Access Panel, see [Introduction toohello Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="0018a-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0018a-258">Additional resources</span></span>

* [<span data-ttu-id="0018a-259">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0018a-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0018a-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0018a-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

