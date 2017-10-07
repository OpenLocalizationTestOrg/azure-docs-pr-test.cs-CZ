---
title: 'Kurz: Azure Active Directory integrace s Wingspan eTMF | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Wingspan eTMF."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ace320d3-521c-449c-992f-feabe7538de7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: ed5fda5318a0d3841af8b2db4fb74db550befaea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wingspan-etmf"></a><span data-ttu-id="dd12a-103">Kurz: Azure Active Directory integrace s Wingspan eTMF</span><span class="sxs-lookup"><span data-stu-id="dd12a-103">Tutorial: Azure Active Directory integration with Wingspan eTMF</span></span>

<span data-ttu-id="dd12a-104">V tomto kurzu zjistíte, jak toointegrate Wingspan eTMF s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dd12a-104">In this tutorial, you learn how toointegrate Wingspan eTMF with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dd12a-105">Integrace Wingspan eTMF s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="dd12a-105">Integrating Wingspan eTMF with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="dd12a-106">Můžete řídit ve službě Azure AD, který má přístup tooWingspan eTMF</span><span class="sxs-lookup"><span data-stu-id="dd12a-106">You can control in Azure AD who has access tooWingspan eTMF</span></span>
- <span data-ttu-id="dd12a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWingspan eTMF (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="dd12a-107">You can enable your users tooautomatically get signed-on tooWingspan eTMF (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dd12a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dd12a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="dd12a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dd12a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd12a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dd12a-110">Prerequisites</span></span>

<span data-ttu-id="dd12a-111">Integrace služby Azure AD s Wingspan eTMF tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="dd12a-111">tooconfigure Azure AD integration with Wingspan eTMF, you need hello following items:</span></span>

- <span data-ttu-id="dd12a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="dd12a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dd12a-113">Wingspan eTMF jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="dd12a-113">A Wingspan eTMF single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dd12a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="dd12a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dd12a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="dd12a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dd12a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="dd12a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dd12a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dd12a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dd12a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="dd12a-118">Scenario description</span></span>
<span data-ttu-id="dd12a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="dd12a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dd12a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="dd12a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dd12a-121">Přidání Wingspan eTMF z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="dd12a-121">Adding Wingspan eTMF from hello gallery</span></span>
2. <span data-ttu-id="dd12a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dd12a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wingspan-etmf-from-hello-gallery"></a><span data-ttu-id="dd12a-123">Přidání Wingspan eTMF z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="dd12a-123">Adding Wingspan eTMF from hello gallery</span></span>
<span data-ttu-id="dd12a-124">tooconfigure hello integrace Wingspan eTMF do Azure AD, je nutné tooadd Wingspan eTMF hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="dd12a-124">tooconfigure hello integration of Wingspan eTMF into Azure AD, you need tooadd Wingspan eTMF from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="dd12a-125">**tooadd Wingspan eTMF z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dd12a-125">**tooadd Wingspan eTMF from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd12a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dd12a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dd12a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="dd12a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="dd12a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd12a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="dd12a-133">Hello vyhledávacího pole zadejte **Wingspan eTMF**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-133">In hello search box, type **Wingspan eTMF**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_search.png)

5. <span data-ttu-id="dd12a-135">Na panelu výsledků hello vyberte **Wingspan eTMF**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd12a-135">In hello results panel, select **Wingspan eTMF**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dd12a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dd12a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dd12a-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wingspan eTMF podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="dd12a-138">In this section, you configure and test Azure AD single sign-on with Wingspan eTMF based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dd12a-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Wingspan eTMF je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dd12a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Wingspan eTMF is tooa user in Azure AD.</span></span> <span data-ttu-id="dd12a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Wingspan eTMF musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="dd12a-140">In other words, a link relationship between an Azure AD user and hello related user in Wingspan eTMF needs toobe established.</span></span>

<span data-ttu-id="dd12a-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Wingspan eTMF.</span><span class="sxs-lookup"><span data-stu-id="dd12a-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Wingspan eTMF.</span></span>

<span data-ttu-id="dd12a-142">tooconfigure a testu Azure AD jednotné přihlašování s Wingspan eTMF, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="dd12a-142">tooconfigure and test Azure AD single sign-on with Wingspan eTMF, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="dd12a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="dd12a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="dd12a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dd12a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dd12a-145">**[Vytvoření zkušebního uživatele eTMF Wingspan](#creating-a-wingspan-etmf-test-user)**  -toohave protějšek Britta Simon v eTMF Wingspan, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="dd12a-145">**[Creating a Wingspan eTMF test user](#creating-a-wingspan-etmf-test-user)** - toohave a counterpart of Britta Simon in Wingspan eTMF that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="dd12a-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dd12a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dd12a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="dd12a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dd12a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dd12a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dd12a-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci eTMF Wingspan.</span><span class="sxs-lookup"><span data-stu-id="dd12a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Wingspan eTMF application.</span></span>

<span data-ttu-id="dd12a-150">**tooconfigure Azure AD jednotné přihlašování s Wingspan eTMF, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dd12a-150">**tooconfigure Azure AD single sign-on with Wingspan eTMF, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd12a-151">V portálu Azure, na hello hello **Wingspan eTMF** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-151">In hello Azure portal, on hello **Wingspan eTMF** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="dd12a-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dd12a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_samlbase.png)

3. <span data-ttu-id="dd12a-155">Na hello **Wingspan eTMF domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="dd12a-155">On hello **Wingspan eTMF Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_url11.png)

    <span data-ttu-id="dd12a-157">a.</span><span class="sxs-lookup"><span data-stu-id="dd12a-157">a.</span></span> <span data-ttu-id="dd12a-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<customer name>.<instance name>.mywingspan.com/saml`</span><span class="sxs-lookup"><span data-stu-id="dd12a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<customer name>.<instance name>.mywingspan.com/saml`</span></span>

    <span data-ttu-id="dd12a-159">b.</span><span class="sxs-lookup"><span data-stu-id="dd12a-159">b.</span></span> <span data-ttu-id="dd12a-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://saml.<instance name>.wingspan.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="dd12a-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://saml.<instance name>.wingspan.com/shibboleth`</span></span>

    <span data-ttu-id="dd12a-161">c.</span><span class="sxs-lookup"><span data-stu-id="dd12a-161">c.</span></span> <span data-ttu-id="dd12a-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<customer name>.<instance name>.mywingspan.com/`</span><span class="sxs-lookup"><span data-stu-id="dd12a-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<customer name>.<instance name>.mywingspan.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="dd12a-163">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="dd12a-163">These values are not hello real.</span></span> <span data-ttu-id="dd12a-164">Tyto hodnoty aktualizujte s hello skutečná adresa URL přihlašování, identifikátor a odpovědi adresu URL včetně hello zákazníka skutečný název a název instance.</span><span class="sxs-lookup"><span data-stu-id="dd12a-164">Update these values with hello actual Sign-On URL, Identifier and Reply URL including hello actual customer name and instance name.</span></span> <span data-ttu-id="dd12a-165">Obraťte se na [tým podpory klienta eTMF Wingspan](http://www.wingspan.com/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dd12a-165">Contact [Wingspan eTMF Client support team](http://www.wingspan.com/contact-us/) tooget these values.</span></span> 

4. <span data-ttu-id="dd12a-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="dd12a-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_certificate.png) 

5. <span data-ttu-id="dd12a-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd12a-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dd12a-170">tooconfigure jednotného přihlašování na **Wingspan eTMF** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[Wingspan eTMF podporu](http://www.wingspan.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="dd12a-170">tooconfigure single sign-on on **Wingspan eTMF** side, you need toosend hello downloaded **Metadata XML** too[Wingspan eTMF support](http://www.wingspan.com/contact-us/).</span></span> <span data-ttu-id="dd12a-171">Jejich nastavit tuto možnost toohave hello jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="dd12a-171">They set this up toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="dd12a-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="dd12a-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="dd12a-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="dd12a-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="dd12a-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dd12a-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dd12a-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dd12a-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="dd12a-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="dd12a-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="dd12a-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dd12a-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd12a-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dd12a-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dd12a-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dd12a-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="dd12a-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dd12a-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dd12a-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dd12a-187">a.</span><span class="sxs-lookup"><span data-stu-id="dd12a-187">a.</span></span> <span data-ttu-id="dd12a-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dd12a-189">b.</span><span class="sxs-lookup"><span data-stu-id="dd12a-189">b.</span></span> <span data-ttu-id="dd12a-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dd12a-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dd12a-191">c.</span><span class="sxs-lookup"><span data-stu-id="dd12a-191">c.</span></span> <span data-ttu-id="dd12a-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="dd12a-193">d.</span><span class="sxs-lookup"><span data-stu-id="dd12a-193">d.</span></span> <span data-ttu-id="dd12a-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-194">Click **Create**.</span></span>
 
### <a name="creating-a-wingspan-etmf-test-user"></a><span data-ttu-id="dd12a-195">Vytvoření zkušebního uživatele eTMF Wingspan</span><span class="sxs-lookup"><span data-stu-id="dd12a-195">Creating a Wingspan eTMF test user</span></span>

<span data-ttu-id="dd12a-196">V této části vytvoříte volal Britta Simon v Wingspan eTMF uživatele.</span><span class="sxs-lookup"><span data-stu-id="dd12a-196">In this section, you create a user called Britta Simon in Wingspan eTMF.</span></span> <span data-ttu-id="dd12a-197">Práce s [Wingspan eTMF podporu](http://www.wingspan.com/contact-us/) tooadd hello uživatele v hello Wingspan eTMF aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd12a-197">Work with [Wingspan eTMF support](http://www.wingspan.com/contact-us/) tooadd hello users in hello Wingspan eTMF application.</span></span> <span data-ttu-id="dd12a-198">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dd12a-198">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="dd12a-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="dd12a-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="dd12a-200">V této části povolíte tak, že udělíte přístup tooWingspan eTMF toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dd12a-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWingspan eTMF.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="dd12a-202">**tooassign eTMF tooWingspan Britta Simon, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dd12a-202">**tooassign Britta Simon tooWingspan eTMF, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd12a-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="dd12a-205">V seznamu aplikace hello vyberte **Wingspan eTMF**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-205">In hello applications list, select **Wingspan eTMF**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_app.png) 

3. <span data-ttu-id="dd12a-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="dd12a-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="dd12a-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd12a-209">Click **Add** button.</span></span> <span data-ttu-id="dd12a-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dd12a-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="dd12a-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="dd12a-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="dd12a-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dd12a-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dd12a-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dd12a-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dd12a-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dd12a-215">Testing single sign-on</span></span>

<span data-ttu-id="dd12a-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="dd12a-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> 

<span data-ttu-id="dd12a-217">Klikněte na dlaždici eTMF Wingspan hello v hello přístupového panelu, bude přesměrován tooOrganization přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="dd12a-217">Click hello Wingspan eTMF tile in hello Access Panel, you will be redirected tooOrganization sign on page.</span></span> <span data-ttu-id="dd12a-218">Po úspěšném přihlášení bude přihlášeného tooyour Wingspan eTMF aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd12a-218">After successful login, you will be signed-on tooyour Wingspan eTMF application.</span></span> <span data-ttu-id="dd12a-219">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="dd12a-219">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd12a-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dd12a-220">Additional resources</span></span>

* [<span data-ttu-id="dd12a-221">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dd12a-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dd12a-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dd12a-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_203.png

