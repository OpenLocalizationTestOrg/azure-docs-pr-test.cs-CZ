---
title: 'Kurz: Azure Active Directory integrace s ADP Globalview | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ADP Globalview."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: aee2d56f05b486d12facbc41c9503455094604ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a><span data-ttu-id="15c5c-103">Kurz: Azure Active Directory integrace s ADP Globalview</span><span class="sxs-lookup"><span data-stu-id="15c5c-103">Tutorial: Azure Active Directory integration with ADP Globalview</span></span>

<span data-ttu-id="15c5c-104">V tomto kurzu zjistíte, jak toointegrate ADP Globalview službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="15c5c-104">In this tutorial, you learn how toointegrate ADP Globalview with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="15c5c-105">Integrace ADP Globalview s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="15c5c-105">Integrating ADP Globalview with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="15c5c-106">Můžete řídit ve službě Azure AD, který má přístup tooADP Globalview</span><span class="sxs-lookup"><span data-stu-id="15c5c-106">You can control in Azure AD who has access tooADP Globalview</span></span>
- <span data-ttu-id="15c5c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooADP Globalview (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="15c5c-107">You can enable your users tooautomatically get signed-on tooADP Globalview (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="15c5c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="15c5c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="15c5c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="15c5c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15c5c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="15c5c-110">Prerequisites</span></span>

<span data-ttu-id="15c5c-111">Integrace služby Azure AD s ADP Globalview tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="15c5c-111">tooconfigure Azure AD integration with ADP Globalview, you need hello following items:</span></span>

- <span data-ttu-id="15c5c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="15c5c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="15c5c-113">ADP Globalview jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="15c5c-113">An ADP Globalview single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="15c5c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="15c5c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="15c5c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="15c5c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="15c5c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="15c5c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="15c5c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="15c5c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="15c5c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="15c5c-118">Scenario description</span></span>
<span data-ttu-id="15c5c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="15c5c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="15c5c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="15c5c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="15c5c-121">Přidání ADP Globalview z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="15c5c-121">Adding ADP Globalview from hello gallery</span></span>
2. <span data-ttu-id="15c5c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="15c5c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-globalview-from-hello-gallery"></a><span data-ttu-id="15c5c-123">Přidání ADP Globalview z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="15c5c-123">Adding ADP Globalview from hello gallery</span></span>
<span data-ttu-id="15c5c-124">tooconfigure hello integrace ADP Globalview do Azure AD, je nutné tooadd ADP Globalview hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="15c5c-124">tooconfigure hello integration of ADP Globalview into Azure AD, you need tooadd ADP Globalview from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="15c5c-125">**tooadd ADP Globalview z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="15c5c-125">**tooadd ADP Globalview from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="15c5c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="15c5c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="15c5c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="15c5c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="15c5c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="15c5c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="15c5c-133">Hello vyhledávacího pole zadejte **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-133">In hello search box, type **ADP Globalview**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. <span data-ttu-id="15c5c-135">Na panelu výsledků hello vyberte **ADP Globalview**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="15c5c-135">In hello results panel, select **ADP Globalview**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="15c5c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="15c5c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="15c5c-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s ADP Globalview podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="15c5c-138">In this section, you configure and test Azure AD single sign-on with ADP Globalview based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="15c5c-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v ADP Globalview je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15c5c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ADP Globalview is tooa user in Azure AD.</span></span> <span data-ttu-id="15c5c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ADP Globalview musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="15c5c-140">In other words, a link relationship between an Azure AD user and hello related user in ADP Globalview needs toobe established.</span></span>

<span data-ttu-id="15c5c-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ADP Globalview.</span><span class="sxs-lookup"><span data-stu-id="15c5c-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ADP Globalview.</span></span>

<span data-ttu-id="15c5c-142">tooconfigure a testu Azure AD jednotné přihlašování s ADP Globalview, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="15c5c-142">tooconfigure and test Azure AD single sign-on with ADP Globalview, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="15c5c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="15c5c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="15c5c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="15c5c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="15c5c-145">**[Vytváření testovacího uživatele ADP Globalview](#creating-an-adp-globalview-test-user)**  -toohave protějšek Britta Simon v ADP Globalview, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="15c5c-145">**[Creating an ADP Globalview test user](#creating-an-adp-globalview-test-user)** - toohave a counterpart of Britta Simon in ADP Globalview that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="15c5c-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="15c5c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="15c5c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="15c5c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="15c5c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="15c5c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="15c5c-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ADP Globalview.</span><span class="sxs-lookup"><span data-stu-id="15c5c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ADP Globalview application.</span></span>

<span data-ttu-id="15c5c-150">**tooconfigure Azure AD jednotné přihlašování s ADP Globalview, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="15c5c-150">**tooconfigure Azure AD single sign-on with ADP Globalview, perform hello following steps:**</span></span>

1. <span data-ttu-id="15c5c-151">V portálu Azure, na hello hello **ADP Globalview** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-151">In hello Azure portal, on hello **ADP Globalview** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="15c5c-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="15c5c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. <span data-ttu-id="15c5c-155">Na hello **ADP Globalview domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="15c5c-155">On hello **ADP Globalview Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_url.png)

     <span data-ttu-id="15c5c-157">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://<subdomain>.globalview.adp.com/federate` nebo`https://<subdomain>.globalview.adp.com/federate2`</span><span class="sxs-lookup"><span data-stu-id="15c5c-157">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.globalview.adp.com/federate` or `https://<subdomain>.globalview.adp.com/federate2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="15c5c-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="15c5c-158">hello value is not real.</span></span> <span data-ttu-id="15c5c-159">Aktualizujte hodnotu hello s hello skutečné identifikátor.</span><span class="sxs-lookup"><span data-stu-id="15c5c-159">Update hello value with hello actual Identifier.</span></span> <span data-ttu-id="15c5c-160">Obraťte se na [podporu ADP Globalview](https://www.adp.com/contact-us/overview.aspx) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="15c5c-160">Contact [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) tooget hello value.</span></span>
 
4. <span data-ttu-id="15c5c-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="15c5c-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. <span data-ttu-id="15c5c-163">Hello ADP GlobalView aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="15c5c-163">hello ADP GlobalView application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> 

6. <span data-ttu-id="15c5c-164">Hello následující snímek obrazovky ukazuje příklad pro ni.</span><span class="sxs-lookup"><span data-stu-id="15c5c-164">hello following screenshot shows an example for it.</span></span> <span data-ttu-id="15c5c-165">Hello deklarace identity názvy vždycky být **"PersonImmutableID"** a které jsme jste namapovali tooExtensionAttribute2, který obsahuje hodnotu hello hello EmployeeID hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="15c5c-165">hello claim names always be **"PersonImmutableID"** and hello value of which we have mapped tooExtensionAttribute2, which contains hello EmployeeID of hello user.</span></span> <span data-ttu-id="15c5c-166">Zde hello mapování uživatelů ze služby Azure AD tooADP GlobalView se provádí na hello EmployeeID ale můžete ji namapovat tooa jinou hodnotu také podle nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="15c5c-166">Here hello user mapping from Azure AD tooADP GlobalView is done on hello EmployeeID but you can map it tooa different value also based on your application settings.</span></span> <span data-ttu-id="15c5c-167">Můžete pracovat s hello ADP GlobalView team první toouse hello správný identifikátor uživatele a mapování danou hodnotu s hello **"PersonImmutableID"** deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="15c5c-167">You can work with hello ADP GlobalView team first toouse hello correct identifier of a user and map that value with hello **"PersonImmutableID"** claim.</span></span> <span data-ttu-id="15c5c-168">Můžete také mapovat hello e-mailu a UserID deklarace jak je znázorněno na obrázku hello.</span><span class="sxs-lookup"><span data-stu-id="15c5c-168">You can also map hello Email and UserID claim as shown in hello picture.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. <span data-ttu-id="15c5c-170">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="15c5c-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="15c5c-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="15c5c-171">Attribute Name</span></span> | <span data-ttu-id="15c5c-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="15c5c-172">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="15c5c-173">personalimmutableid</span><span class="sxs-lookup"><span data-stu-id="15c5c-173">personalimmutableid</span></span> | <span data-ttu-id="15c5c-174">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="15c5c-174">user.extensionattribute2</span></span> |
    | <span data-ttu-id="15c5c-175">E-mailu</span><span class="sxs-lookup"><span data-stu-id="15c5c-175">email</span></span>               | <span data-ttu-id="15c5c-176">User.Mail</span><span class="sxs-lookup"><span data-stu-id="15c5c-176">user.mail</span></span> |
    | <span data-ttu-id="15c5c-177">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="15c5c-177">userid</span></span>              | <span data-ttu-id="15c5c-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="15c5c-178">user.userprincipalname</span></span>|
    
    <span data-ttu-id="15c5c-179">a.</span><span class="sxs-lookup"><span data-stu-id="15c5c-179">a.</span></span> <span data-ttu-id="15c5c-180">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="15c5c-180">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="15c5c-183">b.</span><span class="sxs-lookup"><span data-stu-id="15c5c-183">b.</span></span> <span data-ttu-id="15c5c-184">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="15c5c-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="15c5c-185">c.</span><span class="sxs-lookup"><span data-stu-id="15c5c-185">c.</span></span> <span data-ttu-id="15c5c-186">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="15c5c-186">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="15c5c-187">d.</span><span class="sxs-lookup"><span data-stu-id="15c5c-187">d.</span></span> <span data-ttu-id="15c5c-188">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-188">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="15c5c-189">Před konfigurací hello kontrolního výrazu SAML, je nutné toocontact vaše [ADP Globalview podporu](https://www.adp.com/contact-us/overview.aspx) a požadovat hello hodnotu atributu hello jedinečný identifikátor pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="15c5c-189">Before you can configure hello SAML assertion, you need toocontact your [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="15c5c-190">Je nutné tuto hodnotu tooconfigure hello vlastních deklarací identity pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="15c5c-190">You need this value tooconfigure hello custom claim for your application.</span></span> 

8. <span data-ttu-id="15c5c-191">Na hello **ADP Globalview konfigurace** klikněte na tlačítko **konfigurace ADP Globalview** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="15c5c-191">On hello **ADP Globalview Configuration** section, click **Configure ADP Globalview** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="15c5c-192">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="15c5c-192">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. <span data-ttu-id="15c5c-194">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="15c5c-194">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="15c5c-196">tooconfigure jednotného přihlašování na **ADP Globalview** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[ADP Globalview podporu](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="15c5c-196">tooconfigure single sign-on on **ADP Globalview** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[ADP Globalview support](https://www.adp.com/contact-us/overview.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="15c5c-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="15c5c-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="15c5c-198">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="15c5c-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="15c5c-199">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="15c5c-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="15c5c-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="15c5c-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="15c5c-201">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="15c5c-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="15c5c-203">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="15c5c-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="15c5c-204">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="15c5c-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="15c5c-206">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="15c5c-208">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="15c5c-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="15c5c-210">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="15c5c-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="15c5c-212">a.</span><span class="sxs-lookup"><span data-stu-id="15c5c-212">a.</span></span> <span data-ttu-id="15c5c-213">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="15c5c-214">b.</span><span class="sxs-lookup"><span data-stu-id="15c5c-214">b.</span></span> <span data-ttu-id="15c5c-215">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="15c5c-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="15c5c-216">c.</span><span class="sxs-lookup"><span data-stu-id="15c5c-216">c.</span></span> <span data-ttu-id="15c5c-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="15c5c-218">d.</span><span class="sxs-lookup"><span data-stu-id="15c5c-218">d.</span></span> <span data-ttu-id="15c5c-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-219">Click **Create**.</span></span>
 
### <a name="creating-an-adp-globalview-test-user"></a><span data-ttu-id="15c5c-220">Vytvoření ADP Globalview testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="15c5c-220">Creating an ADP Globalview test user</span></span>

<span data-ttu-id="15c5c-221">Hello cílem této části je toocreate volal Britta Simon v ADP GlobalView uživatele.</span><span class="sxs-lookup"><span data-stu-id="15c5c-221">hello objective of this section is toocreate a user called Britta Simon in ADP GlobalView.</span></span> <span data-ttu-id="15c5c-222">Práce s [podporu ADP Globalview](https://www.adp.com/contact-us/overview.aspx) tooadd hello uživatele v hello ADP GlobalView účtu.</span><span class="sxs-lookup"><span data-stu-id="15c5c-222">Work with [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) tooadd hello users in hello ADP GlobalView account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="15c5c-223">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="15c5c-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="15c5c-224">V této části povolíte tak, že udělíte přístup tooADP Globalview toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="15c5c-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooADP Globalview.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="15c5c-226">**tooassign Britta Simon tooADP Globalview, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="15c5c-226">**tooassign Britta Simon tooADP Globalview, perform hello following steps:**</span></span>

1. <span data-ttu-id="15c5c-227">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="15c5c-229">V seznamu aplikace hello vyberte **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-229">In hello applications list, select **ADP Globalview**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. <span data-ttu-id="15c5c-231">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="15c5c-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="15c5c-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="15c5c-233">Click **Add** button.</span></span> <span data-ttu-id="15c5c-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="15c5c-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="15c5c-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="15c5c-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="15c5c-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="15c5c-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="15c5c-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="15c5c-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="15c5c-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="15c5c-239">Testing single sign-on</span></span>

<span data-ttu-id="15c5c-240">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="15c5c-240">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="15c5c-241">Po kliknutí na tlačítko hello ADP GlobalView dlaždici v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného ADP GlobalView aplikace.</span><span class="sxs-lookup"><span data-stu-id="15c5c-241">When you click hello ADP GlobalView tile in hello Access Panel, you should get automatically signed-on tooyour ADP GlobalView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15c5c-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="15c5c-242">Additional resources</span></span>

* [<span data-ttu-id="15c5c-243">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="15c5c-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="15c5c-244">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="15c5c-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_203.png

