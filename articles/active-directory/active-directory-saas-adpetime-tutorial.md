---
title: 'Kurz: Azure Active Directory integrace s ADP eTime | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ADP eTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 9a5c7d14a18220f8c7a5b14055c30662ecd8f14d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a><span data-ttu-id="f6bdb-103">Kurz: Azure Active Directory integrace s ADP eTime</span><span class="sxs-lookup"><span data-stu-id="f6bdb-103">Tutorial: Azure Active Directory integration with ADP eTime</span></span>

<span data-ttu-id="f6bdb-104">V tomto kurzu zjistíte, jak toointegrate ADP eTime službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f6bdb-104">In this tutorial, you learn how toointegrate ADP eTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f6bdb-105">Integrace ADP eTime s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f6bdb-105">Integrating ADP eTime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f6bdb-106">Můžete řídit ve službě Azure AD, který má přístup tooADP eTime</span><span class="sxs-lookup"><span data-stu-id="f6bdb-106">You can control in Azure AD who has access tooADP eTime</span></span>
- <span data-ttu-id="f6bdb-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooADP eTime (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f6bdb-107">You can enable your users tooautomatically get signed-on tooADP eTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f6bdb-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f6bdb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f6bdb-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f6bdb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f6bdb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f6bdb-110">Prerequisites</span></span>

<span data-ttu-id="f6bdb-111">Integrace služby Azure AD s ADP eTime tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f6bdb-111">tooconfigure Azure AD integration with ADP eTime, you need hello following items:</span></span>

- <span data-ttu-id="f6bdb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f6bdb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f6bdb-113">ADP eTime jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f6bdb-113">An ADP eTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f6bdb-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f6bdb-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f6bdb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f6bdb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f6bdb-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f6bdb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f6bdb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f6bdb-118">Scenario description</span></span>
<span data-ttu-id="f6bdb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f6bdb-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f6bdb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f6bdb-121">Přidání ADP eTime z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f6bdb-121">Adding ADP eTime from hello gallery</span></span>
2. <span data-ttu-id="f6bdb-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f6bdb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-etime-from-hello-gallery"></a><span data-ttu-id="f6bdb-123">Přidání ADP eTime z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f6bdb-123">Adding ADP eTime from hello gallery</span></span>
<span data-ttu-id="f6bdb-124">tooconfigure hello integrace ADP eTime do Azure AD, je nutné tooadd ADP eTime hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-124">tooconfigure hello integration of ADP eTime into Azure AD, you need tooadd ADP eTime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f6bdb-125">**eTime tooadd ADP z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f6bdb-125">**tooadd ADP eTime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f6bdb-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f6bdb-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f6bdb-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f6bdb-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f6bdb-133">Hello vyhledávacího pole zadejte **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-133">In hello search box, type **ADP eTime**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. <span data-ttu-id="f6bdb-135">Na panelu výsledků hello vyberte **ADP eTime**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-135">In hello results panel, select **ADP eTime**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f6bdb-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f6bdb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f6bdb-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ADP eTime podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f6bdb-138">In this section, you configure and test Azure AD single sign-on with ADP eTime based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f6bdb-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v ADP eTime je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ADP eTime is tooa user in Azure AD.</span></span> <span data-ttu-id="f6bdb-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ADP eTime musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-140">In other words, a link relationship between an Azure AD user and hello related user in ADP eTime needs toobe established.</span></span>

<span data-ttu-id="f6bdb-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ADP eTime.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ADP eTime.</span></span>

<span data-ttu-id="f6bdb-142">tooconfigure a testu Azure AD jednotné přihlašování s ADP eTime, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f6bdb-142">tooconfigure and test Azure AD single sign-on with ADP eTime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f6bdb-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f6bdb-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f6bdb-145">**[Vytváření testovacího uživatele ADP eTime](#creating-an-adp-etime-test-user)**  -toohave protějšek Britta Simon v eTime ADP, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-145">**[Creating an ADP eTime test user](#creating-an-adp-etime-test-user)** - toohave a counterpart of Britta Simon in ADP eTime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f6bdb-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f6bdb-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f6bdb-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f6bdb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f6bdb-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci eTime ADP.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ADP eTime application.</span></span>

<span data-ttu-id="f6bdb-150">**tooconfigure Azure AD jednotné přihlašování s ADP eTime, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f6bdb-150">**tooconfigure Azure AD single sign-on with ADP eTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="f6bdb-151">V portálu Azure, na hello hello **ADP eTime** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-151">In hello Azure portal, on hello **ADP eTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f6bdb-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. <span data-ttu-id="f6bdb-155">Na hello **ADP eTime domény a adresy URL** část, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="f6bdb-155">On hello **ADP eTime Domain and URLs** section, perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    <span data-ttu-id="f6bdb-157">a.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-157">a.</span></span> <span data-ttu-id="f6bdb-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<servername>.adp.com`</span><span class="sxs-lookup"><span data-stu-id="f6bdb-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<servername>.adp.com`</span></span>

    <span data-ttu-id="f6bdb-159">b.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-159">b.</span></span> <span data-ttu-id="f6bdb-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="f6bdb-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span></span> 
 
    > [!NOTE] 
    > <span data-ttu-id="f6bdb-161">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-161">These values are not hello real.</span></span> <span data-ttu-id="f6bdb-162">Tyto hodnoty aktualizujte hello skutečná adresa URL odpovědi a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-162">Update these values with hello actual Reply URL and Identifier.</span></span> <span data-ttu-id="f6bdb-163">Obraťte se na [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-163">Contact [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) tooget these values.</span></span>

4. <span data-ttu-id="f6bdb-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. <span data-ttu-id="f6bdb-166">Hello ADP eTime aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-166">hello ADP eTime application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="f6bdb-167">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-167">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="f6bdb-168">Hello názvu deklarací identit budou vždy **"PersonImmutableID"** a které jsme jste namapovali tooExtensionAttribute2, který obsahuje hodnotu hello hello EmployeeID hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-168">hello claim name will always be **"PersonImmutableID"** and hello value of which we have mapped tooExtensionAttribute2 which contains hello EmployeeID of hello user.</span></span> 

    <span data-ttu-id="f6bdb-169">Zde hello mapování uživatelů ze služby Azure AD tooADP eTime bude provedeno v hello EmployeeID ale můžete namapovat jinou hodnotu této tooa také podle nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-169">Here hello user mapping from Azure AD tooADP eTime will be done on hello EmployeeID but you can map this tooa different value also based on your application settings.</span></span> <span data-ttu-id="f6bdb-170">Proto prosím práci s [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) první toouse hello správný identifikátor uživatele a mapovat danou hodnotu s hello **"PersonImmutableID"** deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-170">So please work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) first toouse hello correct identifier of a user and map that value with hello **"PersonImmutableID"** claim.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. <span data-ttu-id="f6bdb-172">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f6bdb-172">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="f6bdb-173">Název atributu</span><span class="sxs-lookup"><span data-stu-id="f6bdb-173">Attribute Name</span></span> | <span data-ttu-id="f6bdb-174">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="f6bdb-174">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="f6bdb-175">PersonImmutableID</span><span class="sxs-lookup"><span data-stu-id="f6bdb-175">PersonImmutableID</span></span> | <span data-ttu-id="f6bdb-176">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="f6bdb-176">user.extensionattribute2</span></span> |
    
    <span data-ttu-id="f6bdb-177">a.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-177">a.</span></span> <span data-ttu-id="f6bdb-178">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-178">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="f6bdb-181">b.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-181">b.</span></span> <span data-ttu-id="f6bdb-182">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-182">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="f6bdb-183">c.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-183">c.</span></span> <span data-ttu-id="f6bdb-184">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-184">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="f6bdb-185">d.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-185">d.</span></span> <span data-ttu-id="f6bdb-186">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-186">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f6bdb-187">Před konfigurací hello kontrolního výrazu SAML, je nutné toocontact vaše [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) a požadovat hello hodnotu atributu hello jedinečný identifikátor pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-187">Before you can configure hello SAML assertion, you need toocontact your [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="f6bdb-188">Je nutné tuto hodnotu tooconfigure hello vlastních deklarací identity pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-188">You need this value tooconfigure hello custom claim for your application.</span></span> 

7. <span data-ttu-id="f6bdb-189">Na hello **ADP eTime konfigurace** klikněte na tlačítko **konfigurace ADP eTime** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-189">On hello **ADP eTime Configuration** section, click **Configure ADP eTime** tooopen **Configure sign-on** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. <span data-ttu-id="f6bdb-191">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-191">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="f6bdb-193">tooconfigure jednotného přihlašování na **ADP eTime** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="f6bdb-193">tooconfigure single sign-on on **ADP eTime** side, you need toosend hello downloaded **Metadata XML** too[ADP eTime support team](https://www.adp.com/contact-us/overview.aspx).</span></span> 

> [!TIP]
> <span data-ttu-id="f6bdb-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="f6bdb-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f6bdb-195">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f6bdb-196">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f6bdb-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f6bdb-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f6bdb-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="f6bdb-198">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f6bdb-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f6bdb-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f6bdb-201">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f6bdb-203">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f6bdb-205">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f6bdb-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f6bdb-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f6bdb-209">a.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-209">a.</span></span> <span data-ttu-id="f6bdb-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f6bdb-211">b.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-211">b.</span></span> <span data-ttu-id="f6bdb-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f6bdb-213">c.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-213">c.</span></span> <span data-ttu-id="f6bdb-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f6bdb-215">d.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-215">d.</span></span> <span data-ttu-id="f6bdb-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-216">Click **Create**.</span></span>
 
### <a name="creating-an-adp-etime-test-user"></a><span data-ttu-id="f6bdb-217">Vytvoření ADP eTime testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f6bdb-217">Creating an ADP eTime test user</span></span>

<span data-ttu-id="f6bdb-218">Hello cílem této části je toocreate volal Britta Simon v ADP eTime uživatele.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-218">hello objective of this section is toocreate a user called Britta Simon in ADP eTime.</span></span> <span data-ttu-id="f6bdb-219">Práce s [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) tooadd hello uživatelé v hello ADP eTime účtu.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-219">Work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) tooadd hello users in hello ADP eTime account.</span></span> 
   
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f6bdb-220">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f6bdb-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f6bdb-221">V této části povolíte tak, že udělíte přístup tooADP eTime toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooADP eTime.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f6bdb-223">**tooassign eTime tooADP Britta Simon, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f6bdb-223">**tooassign Britta Simon tooADP eTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="f6bdb-224">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f6bdb-226">V seznamu aplikace hello vyberte **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-226">In hello applications list, select **ADP eTime**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. <span data-ttu-id="f6bdb-228">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f6bdb-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-230">Click **Add** button.</span></span> <span data-ttu-id="f6bdb-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f6bdb-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f6bdb-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f6bdb-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f6bdb-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f6bdb-236">Testing single sign-on</span></span>

<span data-ttu-id="f6bdb-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f6bdb-238">Po kliknutí na tlačítko hello ADP eTime dlaždice v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného ADP eTime aplikace.</span><span class="sxs-lookup"><span data-stu-id="f6bdb-238">When you click hello ADP eTime tile in hello Access Panel, you should get automatically signed-on tooyour ADP eTime application.</span></span>
<span data-ttu-id="f6bdb-239">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f6bdb-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f6bdb-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f6bdb-240">Additional resources</span></span>

* [<span data-ttu-id="f6bdb-241">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f6bdb-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f6bdb-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f6bdb-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

