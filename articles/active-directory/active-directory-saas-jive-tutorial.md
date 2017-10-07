---
title: 'Kurz: Azure Active Directory integrace s Jive | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9fc5659a-c116-4a1b-a601-333325a26b46
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: f22bf78a55e8a4a9ea2f0020ef2f535be88b6302
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jive"></a><span data-ttu-id="8d475-103">Kurz: Azure Active Directory integrace s Jive</span><span class="sxs-lookup"><span data-stu-id="8d475-103">Tutorial: Azure Active Directory integration with Jive</span></span>

<span data-ttu-id="8d475-104">V tomto kurzu zjistíte, jak toointegrate Jive s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8d475-104">In this tutorial, you learn how toointegrate Jive with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8d475-105">Integrace Jive s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8d475-105">Integrating Jive with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8d475-106">Můžete řídit ve službě Azure AD, který má přístup tooJive</span><span class="sxs-lookup"><span data-stu-id="8d475-106">You can control in Azure AD who has access tooJive</span></span>
- <span data-ttu-id="8d475-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooJive (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d475-107">You can enable your users tooautomatically get signed-on tooJive (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8d475-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8d475-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8d475-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8d475-109">If you want tooknow more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d475-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8d475-110">Prerequisites</span></span>

<span data-ttu-id="8d475-111">Integrace služby Azure AD s Jive tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8d475-111">tooconfigure Azure AD integration with Jive, you need hello following items:</span></span>

- <span data-ttu-id="8d475-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d475-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8d475-113">Jive jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8d475-113">A Jive single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8d475-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8d475-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8d475-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8d475-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8d475-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8d475-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8d475-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8d475-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8d475-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8d475-118">Scenario description</span></span>
<span data-ttu-id="8d475-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8d475-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8d475-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8d475-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8d475-121">Přidání Jive z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8d475-121">Adding Jive from hello gallery</span></span>
2. <span data-ttu-id="8d475-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8d475-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jive-from-hello-gallery"></a><span data-ttu-id="8d475-123">Přidání Jive z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8d475-123">Adding Jive from hello gallery</span></span>
<span data-ttu-id="8d475-124">integrace hello tooconfigure Jive do služby Azure AD, je nutné tooadd Jive hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8d475-124">tooconfigure hello integration of Jive into Azure AD, you need tooadd Jive from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8d475-125">**tooadd Jive z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d475-125">**tooadd Jive from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d475-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8d475-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8d475-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8d475-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8d475-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8d475-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8d475-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8d475-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8d475-133">Hello vyhledávacího pole zadejte **Jive**.</span><span class="sxs-lookup"><span data-stu-id="8d475-133">In hello search box, type **Jive**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_search.png)

5. <span data-ttu-id="8d475-135">Na panelu výsledků hello vyberte **Jive**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d475-135">In hello results panel, select **Jive**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8d475-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8d475-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8d475-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jive podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8d475-138">In this section, you configure and test Azure AD single sign-on with Jive based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8d475-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Jive je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8d475-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jive is tooa user in Azure AD.</span></span> <span data-ttu-id="8d475-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Jive musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8d475-140">In other words, a link relationship between an Azure AD user and hello related user in Jive needs toobe established.</span></span>

<span data-ttu-id="8d475-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Jive.</span><span class="sxs-lookup"><span data-stu-id="8d475-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Jive.</span></span>

<span data-ttu-id="8d475-142">tooconfigure a testu Azure AD jednotné přihlašování s Jive, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="8d475-142">tooconfigure and test Azure AD single sign-on with Jive, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8d475-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8d475-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8d475-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8d475-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8d475-145">**[Vytvoření zkušebního uživatele Jive](#creating-a-jive-test-user)**  -toohave protějšek Britta Simon v Jive, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8d475-145">**[Creating a Jive test user](#creating-a-jive-test-user)** - toohave a counterpart of Britta Simon in Jive that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8d475-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8d475-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8d475-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8d475-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8d475-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8d475-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8d475-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Jive.</span><span class="sxs-lookup"><span data-stu-id="8d475-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jive application.</span></span>

<span data-ttu-id="8d475-150">**tooconfigure Azure AD jednotné přihlašování s Jive, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d475-150">**tooconfigure Azure AD single sign-on with Jive, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d475-151">V portálu Azure, na hello hello **Jive** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8d475-151">In hello Azure portal, on hello **Jive** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8d475-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8d475-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_samlbase.png)

3. <span data-ttu-id="8d475-155">Na hello **Jive domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8d475-155">On hello **Jive Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_url.png)

    <span data-ttu-id="8d475-157">a.</span><span class="sxs-lookup"><span data-stu-id="8d475-157">a.</span></span> <span data-ttu-id="8d475-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instance name>.jivecustom.com`</span><span class="sxs-lookup"><span data-stu-id="8d475-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instance name>.jivecustom.com`</span></span>

    <span data-ttu-id="8d475-159">b.</span><span class="sxs-lookup"><span data-stu-id="8d475-159">b.</span></span> <span data-ttu-id="8d475-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instance name>.jiveon.com`</span><span class="sxs-lookup"><span data-stu-id="8d475-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instance name>.jiveon.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8d475-161">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="8d475-161">These values are not hello real.</span></span> <span data-ttu-id="8d475-162">Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8d475-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="8d475-163">Obraťte se na [tým podpory Jive klienta](https://www.jivesoftware.com/services-support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8d475-163">Contact [Jive Client support team](https://www.jivesoftware.com/services-support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="8d475-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8d475-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_certificate.png) 

5. <span data-ttu-id="8d475-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8d475-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8d475-168">tooconfigure jednotného přihlašování na **Jive** klienta Jive tooyour postranní, přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="8d475-168">tooconfigure single sign-on on **Jive** side, sign-on tooyour Jive tenant as an administrator.</span></span>

7. <span data-ttu-id="8d475-169">V nabídce hello hello nahoře klikněte na možnost "**Saml**."</span><span class="sxs-lookup"><span data-stu-id="8d475-169">In hello menu on hello top, Click "**Saml**."</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    <span data-ttu-id="8d475-171">a.</span><span class="sxs-lookup"><span data-stu-id="8d475-171">a.</span></span> <span data-ttu-id="8d475-172">Vyberte **povoleno** pod hello **Obecné** kartě.</span><span class="sxs-lookup"><span data-stu-id="8d475-172">Select **Enabled** under hello **General** tab.</span></span>   
    <span data-ttu-id="8d475-173">b.</span><span class="sxs-lookup"><span data-stu-id="8d475-173">b.</span></span> <span data-ttu-id="8d475-174">Klikněte na tlačítko hello "**uložit všechna nastavení saml**" tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8d475-174">Click hello "**Save all saml settings**" button.</span></span>

8. <span data-ttu-id="8d475-175">Přejděte toohello "**Idp Metadata**" kartě.</span><span class="sxs-lookup"><span data-stu-id="8d475-175">Navigate toohello "**Idp Metadata**" tab.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)
   
    <span data-ttu-id="8d475-177">a.</span><span class="sxs-lookup"><span data-stu-id="8d475-177">a.</span></span> <span data-ttu-id="8d475-178">Zkopírujte hello obsah souboru XML metadat hello stáhli a pak ji vložit do hello **zprostředkovatele Identity (IDP) Metadata** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8d475-178">Copy hello content of hello downloaded metadata XML file, and then paste it into hello **Identity Provider (IDP) Metadata** textbox.</span></span>
    
    <span data-ttu-id="8d475-179">b.</span><span class="sxs-lookup"><span data-stu-id="8d475-179">b.</span></span> <span data-ttu-id="8d475-180">Klikněte na tlačítko hello "**uložit všechna nastavení saml**" tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8d475-180">Click hello "**Save all saml settings**" button.</span></span> 

9. <span data-ttu-id="8d475-181">Přejděte toohello "**mapování atributů uživatele**" kartě.</span><span class="sxs-lookup"><span data-stu-id="8d475-181">Go toohello "**User Attribute Mapping**" tab.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)
   
    <span data-ttu-id="8d475-183">a.</span><span class="sxs-lookup"><span data-stu-id="8d475-183">a.</span></span> <span data-ttu-id="8d475-184">V hello **e-mailu** textové pole, kopírování a vložení hello název atributu pro **e-mailu** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8d475-184">In hello **Email** textbox, copy and paste hello attribute name of **mail** value.</span></span>
   
    <span data-ttu-id="8d475-185">b.</span><span class="sxs-lookup"><span data-stu-id="8d475-185">b.</span></span> <span data-ttu-id="8d475-186">V hello **křestní jméno** textové pole, kopírování a vložení hello název atributu pro **givenname** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8d475-186">In hello **First Name** textbox, copy and paste hello attribute name of **givenname** value.</span></span>
   
    <span data-ttu-id="8d475-187">c.</span><span class="sxs-lookup"><span data-stu-id="8d475-187">c.</span></span> <span data-ttu-id="8d475-188">V hello **příjmení** textové pole, kopírování a vložení hello název atributu pro **Přezdívka** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8d475-188">In hello **Last Name** textbox, copy and paste hello attribute name of **surname** value.</span></span>

> [!TIP]
> <span data-ttu-id="8d475-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="8d475-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8d475-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="8d475-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8d475-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8d475-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8d475-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8d475-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="8d475-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8d475-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8d475-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d475-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d475-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8d475-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8d475-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8d475-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8d475-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="8d475-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8d475-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8d475-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8d475-204">a.</span><span class="sxs-lookup"><span data-stu-id="8d475-204">a.</span></span> <span data-ttu-id="8d475-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8d475-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8d475-206">b.</span><span class="sxs-lookup"><span data-stu-id="8d475-206">b.</span></span> <span data-ttu-id="8d475-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8d475-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8d475-208">c.</span><span class="sxs-lookup"><span data-stu-id="8d475-208">c.</span></span> <span data-ttu-id="8d475-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8d475-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8d475-210">d.</span><span class="sxs-lookup"><span data-stu-id="8d475-210">d.</span></span> <span data-ttu-id="8d475-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8d475-211">Click **Create**.</span></span>
 
### <a name="creating-a-jive-test-user"></a><span data-ttu-id="8d475-212">Vytvoření zkušebního uživatele Jive</span><span class="sxs-lookup"><span data-stu-id="8d475-212">Creating a Jive test user</span></span>

<span data-ttu-id="8d475-213">Práce s [tým podpory Jive klienta](https://www.jivesoftware.com/services-support/) tooadd hello uživatelé v platformě Jive hello.</span><span class="sxs-lookup"><span data-stu-id="8d475-213">Work with [Jive Client support team](https://www.jivesoftware.com/services-support/) tooadd hello users in hello Jive platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8d475-214">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8d475-214">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8d475-215">V této části povolíte tak, že udělíte přístup tooJive toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8d475-215">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJive.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8d475-217">**tooassign Britta Simon tooJive, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8d475-217">**tooassign Britta Simon tooJive, perform hello following steps:**</span></span>

1. <span data-ttu-id="8d475-218">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8d475-218">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8d475-220">V seznamu aplikace hello vyberte **Jive**.</span><span class="sxs-lookup"><span data-stu-id="8d475-220">In hello applications list, select **Jive**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_app.png) 

3. <span data-ttu-id="8d475-222">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8d475-222">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8d475-224">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8d475-224">Click **Add** button.</span></span> <span data-ttu-id="8d475-225">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8d475-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8d475-227">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8d475-227">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8d475-228">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8d475-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8d475-229">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8d475-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8d475-230">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8d475-230">Testing single sign-on</span></span>

<span data-ttu-id="8d475-231">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8d475-231">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8d475-232">Když kliknete na dlaždici Jive hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Jive aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d475-232">When you click hello Jive tile in hello Access Panel, you should get automatically signed-on tooyour Jive application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d475-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8d475-233">Additional resources</span></span>

* [<span data-ttu-id="8d475-234">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8d475-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8d475-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8d475-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="8d475-236">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="8d475-236">Configure User Provisioning</span></span>](active-directory-saas-jive-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jive-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png

