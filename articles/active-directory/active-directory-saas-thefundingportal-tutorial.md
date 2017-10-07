---
title: "Kurz: Integrace Azure Active Directory s hello finančních prostředků portálu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a hello finančních prostředků portálu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 9f4329e02f91eb6d8034f17646ac7d15afe503e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hello-funding-portal"></a><span data-ttu-id="269cb-103">Kurz: Integrace Azure Active Directory s hello finančních prostředků portálu</span><span class="sxs-lookup"><span data-stu-id="269cb-103">Tutorial: Azure Active Directory integration with hello Funding Portal</span></span>

<span data-ttu-id="269cb-104">V tomto kurzu zjistíte, jak toointegrate hello portál finančních prostředků pomocí Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="269cb-104">In this tutorial, you learn how toointegrate hello Funding Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="269cb-105">Integrace hello portál finančních prostředků s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="269cb-105">Integrating hello Funding Portal with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="269cb-106">Můžete řídit ve službě Azure AD, který má přístup toohello financovaní portálu</span><span class="sxs-lookup"><span data-stu-id="269cb-106">You can control in Azure AD who has access toohello Funding Portal</span></span>
- <span data-ttu-id="269cb-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného toohello financovaní portálu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="269cb-107">You can enable your users tooautomatically get signed-on toohello Funding Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="269cb-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="269cb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="269cb-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="269cb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="269cb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="269cb-110">Prerequisites</span></span>

<span data-ttu-id="269cb-111">integrace tooconfigure Azure AD s hello financovaní portál, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="269cb-111">tooconfigure Azure AD integration with hello Funding Portal, you need hello following items:</span></span>

- <span data-ttu-id="269cb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="269cb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="269cb-113">Předplatné povolené hello finančních prostředků portál jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="269cb-113">A hello Funding Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="269cb-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="269cb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="269cb-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="269cb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="269cb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="269cb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="269cb-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="269cb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="269cb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="269cb-118">Scenario description</span></span>
<span data-ttu-id="269cb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="269cb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="269cb-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="269cb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="269cb-121">Přidání hello portál financovaní z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="269cb-121">Adding hello Funding Portal from hello gallery</span></span>
2. <span data-ttu-id="269cb-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="269cb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hello-funding-portal-from-hello-gallery"></a><span data-ttu-id="269cb-123">Přidání hello portál financovaní z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="269cb-123">Adding hello Funding Portal from hello gallery</span></span>
<span data-ttu-id="269cb-124">tooconfigure hello integrace hello financovaní portálu do Azure AD, je nutné tooadd hello financovaní portál hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="269cb-124">tooconfigure hello integration of hello Funding Portal into Azure AD, you need tooadd hello Funding Portal from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="269cb-125">**tooadd hello portál finančních prostředků z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="269cb-125">**tooadd hello Funding Portal from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="269cb-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="269cb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="269cb-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="269cb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="269cb-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="269cb-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="269cb-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="269cb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="269cb-133">Hello vyhledávacího pole zadejte **hello finančních prostředků portál**.</span><span class="sxs-lookup"><span data-stu-id="269cb-133">In hello search box, type **hello Funding Portal**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_search.png)

5. <span data-ttu-id="269cb-135">Na panelu výsledků hello vyberte **hello finančních prostředků portál**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="269cb-135">In hello results panel, select **hello Funding Portal**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="269cb-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="269cb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="269cb-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s hello finančních prostředků portál založený na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="269cb-138">In this section, you configure and test Azure AD single sign-on with hello Funding Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="269cb-139">Pro toowork jeden přihlašování, musí Azure AD tooknow hello příslušného uživatele v hello finančních prostředků portál je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="269cb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in hello Funding Portal is tooa user in Azure AD.</span></span> <span data-ttu-id="269cb-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v hello finančních prostředků portál musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="269cb-140">In other words, a link relationship between an Azure AD user and hello related user in hello Funding Portal needs toobe established.</span></span>

<span data-ttu-id="269cb-141">V hello finančních prostředků portál, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="269cb-141">In hello Funding Portal, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="269cb-142">tooconfigure a testu Azure AD jednotné přihlašování s hello financovaní portál, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="269cb-142">tooconfigure and test Azure AD single sign-on with hello Funding Portal, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="269cb-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="269cb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="269cb-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="269cb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="269cb-145">**[Vytváření testovacího uživatele finančních prostředků portál hello](#creating-the-funding-portal-test-user)**  -toohave protějšek Britta Simon v hello financovaní portál, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="269cb-145">**[Creating hello Funding Portal test user](#creating-the-funding-portal-test-user)** - toohave a counterpart of Britta Simon in hello Funding Portal that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="269cb-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="269cb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="269cb-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="269cb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="269cb-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="269cb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="269cb-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší hello portál finančních prostředků aplikace.</span><span class="sxs-lookup"><span data-stu-id="269cb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your hello Funding Portal application.</span></span>

<span data-ttu-id="269cb-150">**tooconfigure Azure AD jednotné přihlašování s hello finančních prostředků portál, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="269cb-150">**tooconfigure Azure AD single sign-on with hello Funding Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="269cb-151">V portálu Azure, na hello hello **hello finančních prostředků portál** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="269cb-151">In hello Azure portal, on hello **hello Funding Portal** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="269cb-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="269cb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

3. <span data-ttu-id="269cb-155">Na hello **hello finančních prostředků portál domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="269cb-155">On hello **hello Funding Portal Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    <span data-ttu-id="269cb-157">a.</span><span class="sxs-lookup"><span data-stu-id="269cb-157">a.</span></span> <span data-ttu-id="269cb-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.regenteducation.net/`</span><span class="sxs-lookup"><span data-stu-id="269cb-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.regenteducation.net/`</span></span>

    <span data-ttu-id="269cb-159">b.</span><span class="sxs-lookup"><span data-stu-id="269cb-159">b.</span></span> <span data-ttu-id="269cb-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.regenteducation.net`</span><span class="sxs-lookup"><span data-stu-id="269cb-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.regenteducation.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="269cb-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="269cb-161">These values are not real.</span></span> <span data-ttu-id="269cb-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="269cb-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="269cb-163">Obraťte se na [hello tým podpory finančních prostředků klienta portál](mailto:info@regenteducation.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="269cb-163">Contact [hello Funding Portal Client support team](mailto:info@regenteducation.com) tooget these values.</span></span> 

4. <span data-ttu-id="269cb-164">Hello aplikace finančních prostředků portálu očekává hello SAML kontrolní výrazy toocontain atribut s názvem "externalId1".</span><span class="sxs-lookup"><span data-stu-id="269cb-164">hello Funding Portal application expects hello SAML assertions toocontain an attribute named "externalId1".</span></span> <span data-ttu-id="269cb-165">Hodnota Hello "externalId1" by měla být rozpoznaný studentID.</span><span class="sxs-lookup"><span data-stu-id="269cb-165">hello value of "externalId1" should be a recognized studentID.</span></span> <span data-ttu-id="269cb-166">Nakonfigurujte hello "externalId1" deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="269cb-166">Configure hello "externalId1" claim for this application.</span></span> <span data-ttu-id="269cb-167">Můžete spravovat hello hodnoty těchto atributů z hello **uživatelské atributy** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="269cb-167">You can manage hello values of these attributes from hello **User Attributes** of hello application.</span></span> <span data-ttu-id="269cb-168">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="269cb-168">hello following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

5. <span data-ttu-id="269cb-170">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="269cb-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>

    | <span data-ttu-id="269cb-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="269cb-171">Attribute Name</span></span> | <span data-ttu-id="269cb-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="269cb-172">Attribute Value</span></span> |
    | ------------------- | ---------------- |
    | <span data-ttu-id="269cb-173">externalId1</span><span class="sxs-lookup"><span data-stu-id="269cb-173">externalId1</span></span> | <span data-ttu-id="269cb-174">User.extensionattribute1</span><span class="sxs-lookup"><span data-stu-id="269cb-174">user.extensionattribute1</span></span> |

    <span data-ttu-id="269cb-175">a.</span><span class="sxs-lookup"><span data-stu-id="269cb-175">a.</span></span> <span data-ttu-id="269cb-176">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="269cb-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="269cb-179">b.</span><span class="sxs-lookup"><span data-stu-id="269cb-179">b.</span></span> <span data-ttu-id="269cb-180">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="269cb-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="269cb-181">c.</span><span class="sxs-lookup"><span data-stu-id="269cb-181">c.</span></span> <span data-ttu-id="269cb-182">Z hello **hodnota atributu** seznamu, vyberte hello atribut chcete toouse týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="269cb-182">From hello **Attribute Value** list, select hello attribute you want toouse for your implementation.</span></span> <span data-ttu-id="269cb-183">Například pokud jste uložili hello StudentID hodnotu v hello ExtensionAttribute1, pak vyberte user.extensionattribute1.</span><span class="sxs-lookup"><span data-stu-id="269cb-183">For example, if you have stored hello StudentID value in hello ExtensionAttribute1, then select user.extensionattribute1.</span></span>
    
    <span data-ttu-id="269cb-184">d.</span><span class="sxs-lookup"><span data-stu-id="269cb-184">d.</span></span> <span data-ttu-id="269cb-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="269cb-185">Click **Ok**.</span></span>
 
6. <span data-ttu-id="269cb-186">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="269cb-186">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

7. <span data-ttu-id="269cb-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="269cb-188">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="269cb-190">tooconfigure jednotného přihlašování na **hello finančních prostředků portál** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[hello tým podpory finančních prostředků portál](mailto:info@regenteducation.com).</span><span class="sxs-lookup"><span data-stu-id="269cb-190">tooconfigure single sign-on on **hello Funding Portal** side, you need toosend hello downloaded **Metadata XML** too[hello Funding Portal support team](mailto:info@regenteducation.com).</span></span> <span data-ttu-id="269cb-191">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="269cb-191">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="269cb-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="269cb-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="269cb-193">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="269cb-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="269cb-194">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="269cb-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="269cb-195">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="269cb-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="269cb-196">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="269cb-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="269cb-198">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="269cb-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="269cb-199">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="269cb-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="269cb-201">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="269cb-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="269cb-203">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="269cb-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="269cb-205">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="269cb-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="269cb-207">a.</span><span class="sxs-lookup"><span data-stu-id="269cb-207">a.</span></span> <span data-ttu-id="269cb-208">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="269cb-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="269cb-209">b.</span><span class="sxs-lookup"><span data-stu-id="269cb-209">b.</span></span> <span data-ttu-id="269cb-210">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="269cb-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="269cb-211">c.</span><span class="sxs-lookup"><span data-stu-id="269cb-211">c.</span></span> <span data-ttu-id="269cb-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="269cb-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="269cb-213">d.</span><span class="sxs-lookup"><span data-stu-id="269cb-213">d.</span></span> <span data-ttu-id="269cb-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="269cb-214">Click **Create**.</span></span>
 
### <a name="creating-hello-funding-portal-test-user"></a><span data-ttu-id="269cb-215">Vytváření hello finančních prostředků portál testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="269cb-215">Creating hello Funding Portal test user</span></span>

<span data-ttu-id="269cb-216">V této části vytvoříte uživatele volal Britta Simon v hello financovaní portálu.</span><span class="sxs-lookup"><span data-stu-id="269cb-216">In this section, you create a user called Britta Simon in hello Funding Portal.</span></span> <span data-ttu-id="269cb-217">Práce s [hello tým podpory finančních prostředků portál](mailto:info@regenteducation.com) tooadd hello testovacího uživatele a povolení jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="269cb-217">Work with [hello Funding Portal support team](mailto:info@regenteducation.com) tooadd hello test user and enable SSO.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="269cb-218">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="269cb-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="269cb-219">V této části povolíte tak, že udělíte přístup toohello financovaní portál Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="269cb-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access toohello Funding Portal.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="269cb-221">**tooassign toohello Britta Simon financovaní portál, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="269cb-221">**tooassign Britta Simon toohello Funding Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="269cb-222">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="269cb-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="269cb-224">V seznamu aplikace hello vyberte **hello finančních prostředků portál**.</span><span class="sxs-lookup"><span data-stu-id="269cb-224">In hello applications list, select **hello Funding Portal**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

3. <span data-ttu-id="269cb-226">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="269cb-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="269cb-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="269cb-228">Click **Add** button.</span></span> <span data-ttu-id="269cb-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="269cb-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="269cb-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="269cb-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="269cb-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="269cb-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="269cb-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="269cb-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="269cb-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="269cb-234">Testing single sign-on</span></span>

<span data-ttu-id="269cb-235">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="269cb-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="269cb-236">Po kliknutí na tlačítko hello hello finančních prostředků portál dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour hello portál finančních prostředků aplikace.</span><span class="sxs-lookup"><span data-stu-id="269cb-236">When you click hello hello Funding Portal tile in hello Access Panel, you should get automatically signed-on tooyour hello Funding Portal application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="269cb-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="269cb-237">Additional resources</span></span>

* [<span data-ttu-id="269cb-238">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="269cb-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="269cb-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="269cb-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png

