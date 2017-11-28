---
title: 'Kurz: Azure Active Directory integrace s eKincare | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a eKincare."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 57f56d14-83cf-4cbb-b342-fac4fc60078f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 16129e3384132bb34744aadf088bb65f07ed7a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ekincare"></a><span data-ttu-id="6e4cd-103">Kurz: Azure Active Directory integrace s eKincare</span><span class="sxs-lookup"><span data-stu-id="6e4cd-103">Tutorial: Azure Active Directory integration with eKincare</span></span>

<span data-ttu-id="6e4cd-104">V tomto kurzu zjistíte, jak toointegrate eKincare s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6e4cd-104">In this tutorial, you learn how toointegrate eKincare with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6e4cd-105">Integrace eKincare s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6e4cd-105">Integrating eKincare with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6e4cd-106">Můžete řídit ve službě Azure AD, který má přístup tooeKincare</span><span class="sxs-lookup"><span data-stu-id="6e4cd-106">You can control in Azure AD who has access tooeKincare</span></span>
- <span data-ttu-id="6e4cd-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooeKincare (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e4cd-107">You can enable your users tooautomatically get signed-on tooeKincare (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6e4cd-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6e4cd-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6e4cd-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6e4cd-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e4cd-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6e4cd-110">Prerequisites</span></span>

<span data-ttu-id="6e4cd-111">Integrace služby Azure AD s eKincare tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6e4cd-111">tooconfigure Azure AD integration with eKincare, you need hello following items:</span></span>

- <span data-ttu-id="6e4cd-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e4cd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6e4cd-113">EKincare jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6e4cd-113">A eKincare single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6e4cd-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6e4cd-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6e4cd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6e4cd-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6e4cd-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6e4cd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6e4cd-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6e4cd-118">Scenario description</span></span>
<span data-ttu-id="6e4cd-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6e4cd-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6e4cd-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6e4cd-121">Přidání eKincare z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6e4cd-121">Adding eKincare from hello gallery</span></span>
2. <span data-ttu-id="6e4cd-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e4cd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ekincare-from-hello-gallery"></a><span data-ttu-id="6e4cd-123">Přidání eKincare z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6e4cd-123">Adding eKincare from hello gallery</span></span>
<span data-ttu-id="6e4cd-124">tooconfigure hello integrace eKincare do Azure AD, je nutné tooadd eKincare hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-124">tooconfigure hello integration of eKincare into Azure AD, you need tooadd eKincare from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6e4cd-125">**tooadd eKincare z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e4cd-125">**tooadd eKincare from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4cd-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6e4cd-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6e4cd-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6e4cd-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6e4cd-133">Hello vyhledávacího pole zadejte **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-133">In hello search box, type **eKincare**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_search.png)

5. <span data-ttu-id="6e4cd-135">Na panelu výsledků hello vyberte **eKincare**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-135">In hello results panel, select **eKincare**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6e4cd-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e4cd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6e4cd-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s eKincare podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="6e4cd-138">In this section, you configure and test Azure AD single sign-on with eKincare based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6e4cd-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v eKincare je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in eKincare is tooa user in Azure AD.</span></span> <span data-ttu-id="6e4cd-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v eKincare musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-140">In other words, a link relationship between an Azure AD user and hello related user in eKincare needs toobe established.</span></span>

<span data-ttu-id="6e4cd-141">V eKincare, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-141">In eKincare, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6e4cd-142">tooconfigure a testu Azure AD jednotné přihlašování s eKincare, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6e4cd-142">tooconfigure and test Azure AD single sign-on with eKincare, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6e4cd-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6e4cd-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6e4cd-145">**[Vytvoření zkušebního uživatele eKincare](#creating-a-ekincare-test-user)**  -toohave protějšek Britta Simon v eKincare, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-145">**[Creating a eKincare test user](#creating-a-ekincare-test-user)** - toohave a counterpart of Britta Simon in eKincare that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6e4cd-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6e4cd-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6e4cd-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e4cd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6e4cd-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci eKincare.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your eKincare application.</span></span>

<span data-ttu-id="6e4cd-150">**tooconfigure Azure AD jednotné přihlašování s eKincare, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e4cd-150">**tooconfigure Azure AD single sign-on with eKincare, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4cd-151">V portálu Azure, na hello hello **eKincare** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-151">In hello Azure portal, on hello **eKincare** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6e4cd-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_samlbase.png)

3. <span data-ttu-id="6e4cd-155">Na hello **eKincare domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6e4cd-155">On hello **eKincare Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_url.png)

    <span data-ttu-id="6e4cd-157">a.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-157">a.</span></span> <span data-ttu-id="6e4cd-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.ekincare.com/`</span><span class="sxs-lookup"><span data-stu-id="6e4cd-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.ekincare.com/`</span></span>

    <span data-ttu-id="6e4cd-159">b.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-159">b.</span></span> <span data-ttu-id="6e4cd-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.ekincare.com/hul/saml`</span><span class="sxs-lookup"><span data-stu-id="6e4cd-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<instancename>.ekincare.com/hul/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6e4cd-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-161">These values are not real.</span></span> <span data-ttu-id="6e4cd-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="6e4cd-163">Obraťte se na [tým podpory eKincare](mailto:tech@ekincare.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-163">Contact [eKincare support team](mailto:tech@ekincare.com) tooget these values.</span></span>
 
4. <span data-ttu-id="6e4cd-164">aplikace eKincare očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-164">eKincare application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="6e4cd-165">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="6e4cd-166">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="6e4cd-167">Hello následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-167">hello following screenshot shows an example for this configuration.</span></span>

    <span data-ttu-id="6e4cd-168">Hello názvu deklarací identit budou vždy **"employeeid"** a hello hodnoty, které jsme jste namapovali toouser.extensionattribute1, který obsahuje hello employeeid hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-168">hello claim name will always be **"employeeid"** and hello value of which we have mapped toouser.extensionattribute1, that contains hello employeeid of hello user.</span></span> <span data-ttu-id="6e4cd-169">Hello jednofaktorovému název další dva deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-169">hello other two claims' name i.e</span></span> <span data-ttu-id="6e4cd-170">**"kódu organizace"** a **"název organizace"** bude vždy stejnou a jejich hodnoty obsahovat podrobnosti hello hello organizace hello uživatele v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-170">**"organizationid"** and **"organizationname"** will always be same and their values contain hello details of hello organization of hello user respectively.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/attribute.png)
    
5. <span data-ttu-id="6e4cd-172">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6e4cd-172">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="6e4cd-173">Název atributu</span><span class="sxs-lookup"><span data-stu-id="6e4cd-173">Attribute Name</span></span> | <span data-ttu-id="6e4cd-174">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="6e4cd-174">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="6e4cd-175">Číslo zaměstnance</span><span class="sxs-lookup"><span data-stu-id="6e4cd-175">employeeid</span></span> | <span data-ttu-id="6e4cd-176">*User.extensionattribute1*</span><span class="sxs-lookup"><span data-stu-id="6e4cd-176">*user.extensionattribute1*</span></span> |
    | <span data-ttu-id="6e4cd-177">kódu organizace</span><span class="sxs-lookup"><span data-stu-id="6e4cd-177">organizationid</span></span> | <span data-ttu-id="6e4cd-178">*"uniquevalue"*</span><span class="sxs-lookup"><span data-stu-id="6e4cd-178">*"uniquevalue"*</span></span> |
    | <span data-ttu-id="6e4cd-179">název organizace</span><span class="sxs-lookup"><span data-stu-id="6e4cd-179">organizationname</span></span> | <span data-ttu-id="6e4cd-180">*User.CompanyName*</span><span class="sxs-lookup"><span data-stu-id="6e4cd-180">*user.companyname*</span></span> |

    <span data-ttu-id="6e4cd-181">a.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-181">a.</span></span> <span data-ttu-id="6e4cd-182">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-182">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/05.png)
    
    <span data-ttu-id="6e4cd-185">b.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-185">b.</span></span> <span data-ttu-id="6e4cd-186">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-186">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="6e4cd-187">c.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-187">c.</span></span> <span data-ttu-id="6e4cd-188">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-188">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="6e4cd-189">d.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-189">d.</span></span> <span data-ttu-id="6e4cd-190">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="6e4cd-190">Click **Ok**</span></span>

6. <span data-ttu-id="6e4cd-191">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-191">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_certificate.png) 

7. <span data-ttu-id="6e4cd-193">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-193">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6e4cd-195">tooconfigure jednotného přihlašování na **eKincare** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory eKincare](mailto:tech@ekincare.com).</span><span class="sxs-lookup"><span data-stu-id="6e4cd-195">tooconfigure single sign-on on **eKincare** side, you need toosend hello downloaded **Metadata XML** too[eKincare support team](mailto:tech@ekincare.com).</span></span> <span data-ttu-id="6e4cd-196">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-196">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="6e4cd-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6e4cd-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6e4cd-198">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6e4cd-199">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6e4cd-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6e4cd-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e4cd-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="6e4cd-201">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6e4cd-203">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e4cd-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4cd-204">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6e4cd-206">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6e4cd-208">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6e4cd-210">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6e4cd-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ekincare-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6e4cd-212">a.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-212">a.</span></span> <span data-ttu-id="6e4cd-213">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6e4cd-214">b.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-214">b.</span></span> <span data-ttu-id="6e4cd-215">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6e4cd-216">c.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-216">c.</span></span> <span data-ttu-id="6e4cd-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6e4cd-218">d.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-218">d.</span></span> <span data-ttu-id="6e4cd-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-219">Click **Create**.</span></span>
 
### <a name="creating-a-ekincare-test-user"></a><span data-ttu-id="6e4cd-220">Vytvoření zkušebního uživatele eKincare</span><span class="sxs-lookup"><span data-stu-id="6e4cd-220">Creating a eKincare test user</span></span>

<span data-ttu-id="6e4cd-221">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele v aplikaci hello automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-221">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6e4cd-222">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6e4cd-222">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6e4cd-223">V této části povolíte tak, že udělíte přístup tooeKincare toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-223">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooeKincare.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6e4cd-225">**tooassign Britta Simon tooeKincare, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e4cd-225">**tooassign Britta Simon tooeKincare, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4cd-226">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-226">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6e4cd-228">V seznamu aplikace hello vyberte **eKincare**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-228">In hello applications list, select **eKincare**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ekincare-tutorial/tutorial_ekincare_app.png) 

3. <span data-ttu-id="6e4cd-230">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-230">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6e4cd-232">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-232">Click **Add** button.</span></span> <span data-ttu-id="6e4cd-233">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6e4cd-235">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-235">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6e4cd-236">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6e4cd-237">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6e4cd-238">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e4cd-238">Testing single sign-on</span></span>

<span data-ttu-id="6e4cd-239">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-239">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6e4cd-240">Když kliknete na dlaždici eKincare hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour eKincare aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e4cd-240">When you click hello eKincare tile in hello Access Panel, you should get automatically signed-on tooyour eKincare application.</span></span>
<span data-ttu-id="6e4cd-241">Další informace o na přístupovém panelu najdete v tématu [Úvod toohello přístupového panelu](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="6e4cd-241">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e4cd-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6e4cd-242">Additional resources</span></span>

* [<span data-ttu-id="6e4cd-243">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e4cd-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6e4cd-244">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6e4cd-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ekincare-tutorial/tutorial_general_203.png

