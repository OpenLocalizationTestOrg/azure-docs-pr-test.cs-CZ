---
title: 'Kurz: Azure Active Directory integrace s Ariba | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Ariba."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 45a8364c-55d1-4dc7-b079-9eb2a701842d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: a1b17129c1298b59c155a0890e41e41ff9544a24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ariba"></a><span data-ttu-id="5cd8e-103">Kurz: Azure Active Directory integrace s Ariba</span><span class="sxs-lookup"><span data-stu-id="5cd8e-103">Tutorial: Azure Active Directory integration with Ariba</span></span>

<span data-ttu-id="5cd8e-104">V tomto kurzu zjistíte, jak toointegrate Ariba s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5cd8e-104">In this tutorial, you learn how toointegrate Ariba with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5cd8e-105">Integrace Ariba s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5cd8e-105">Integrating Ariba with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5cd8e-106">Můžete řídit ve službě Azure AD, který má přístup tooAriba</span><span class="sxs-lookup"><span data-stu-id="5cd8e-106">You can control in Azure AD who has access tooAriba</span></span>
- <span data-ttu-id="5cd8e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAriba (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cd8e-107">You can enable your users tooautomatically get signed-on tooAriba (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5cd8e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5cd8e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5cd8e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5cd8e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5cd8e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5cd8e-110">Prerequisites</span></span>

<span data-ttu-id="5cd8e-111">Integrace služby Azure AD s Ariba tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5cd8e-111">tooconfigure Azure AD integration with Ariba, you need hello following items:</span></span>

- <span data-ttu-id="5cd8e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cd8e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5cd8e-113">Ariba jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5cd8e-113">An Ariba single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5cd8e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5cd8e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5cd8e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5cd8e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5cd8e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5cd8e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5cd8e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5cd8e-118">Scenario description</span></span>
<span data-ttu-id="5cd8e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5cd8e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5cd8e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5cd8e-121">Přidání Ariba z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5cd8e-121">Adding Ariba from hello gallery</span></span>
2. <span data-ttu-id="5cd8e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cd8e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ariba-from-hello-gallery"></a><span data-ttu-id="5cd8e-123">Přidání Ariba z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5cd8e-123">Adding Ariba from hello gallery</span></span>
<span data-ttu-id="5cd8e-124">tooconfigure hello integrace Ariba do Azure AD, je nutné tooadd Ariba hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-124">tooconfigure hello integration of Ariba into Azure AD, you need tooadd Ariba from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5cd8e-125">**tooadd Ariba z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5cd8e-125">**tooadd Ariba from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5cd8e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5cd8e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5cd8e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5cd8e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5cd8e-133">Hello vyhledávacího pole zadejte **Ariba**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-133">In hello search box, type **Ariba**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_search.png)

5. <span data-ttu-id="5cd8e-135">Na panelu výsledků hello vyberte **Ariba**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-135">In hello results panel, select **Ariba**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5cd8e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cd8e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5cd8e-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Ariba podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5cd8e-138">In this section, you configure and test Azure AD single sign-on with Ariba based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5cd8e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Ariba je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Ariba is tooa user in Azure AD.</span></span> <span data-ttu-id="5cd8e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Ariba musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-140">In other words, a link relationship between an Azure AD user and hello related user in Ariba needs toobe established.</span></span>

<span data-ttu-id="5cd8e-141">V Ariba, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-141">In Ariba, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5cd8e-142">tooconfigure a testu Azure AD jednotné přihlašování s Ariba, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5cd8e-142">tooconfigure and test Azure AD single sign-on with Ariba, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5cd8e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5cd8e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5cd8e-145">**[Vytváření testovacího uživatele Ariba](#creating-an-ariba-test-user)**  -toohave protějšek Britta Simon v Ariba, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-145">**[Creating an Ariba test user](#creating-an-ariba-test-user)** - toohave a counterpart of Britta Simon in Ariba that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5cd8e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5cd8e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5cd8e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cd8e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5cd8e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Ariba.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Ariba application.</span></span>

<span data-ttu-id="5cd8e-150">**tooconfigure Azure AD jednotné přihlašování s Ariba, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5cd8e-150">**tooconfigure Azure AD single sign-on with Ariba, perform hello following steps:**</span></span>

1. <span data-ttu-id="5cd8e-151">V portálu Azure, na hello hello **Ariba** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-151">In hello Azure portal, on hello **Ariba** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5cd8e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_samlbase.png)

3. <span data-ttu-id="5cd8e-155">Na hello **Ariba domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5cd8e-155">On hello **Ariba Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_url.png)

    <span data-ttu-id="5cd8e-157">a.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-157">a.</span></span> <span data-ttu-id="5cd8e-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://<subdomain>.sourcing.ariba.com` nebo`https://<subdomain>.supplier.ariba.com`</span><span class="sxs-lookup"><span data-stu-id="5cd8e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.sourcing.ariba.com` or `https://<subdomain>.supplier.ariba.com`</span></span>

    <span data-ttu-id="5cd8e-159">b.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-159">b.</span></span> <span data-ttu-id="5cd8e-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://<subdomain>.procurement-2.ariba.com`</span><span class="sxs-lookup"><span data-stu-id="5cd8e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://<subdomain>.procurement-2.ariba.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5cd8e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-161">These values are not real.</span></span> <span data-ttu-id="5cd8e-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5cd8e-163">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="5cd8e-164">Obraťte se na tým podpory Ariba klienta v **1-866-218-2155** tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-164">Contact Ariba Client support team at **1-866-218-2155** tooget these values.</span></span> 
 

4. <span data-ttu-id="5cd8e-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate  file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_certificate.png) 

5. <span data-ttu-id="5cd8e-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5cd8e-169">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, použijte na tým podpory Ariba **1-866-218-2155** a budete vám další pomůže na způsobu tooprovide je hello stahování **certifikátu (Base64)** souboru.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-169">tooget SSO configured for your application, call Ariba support team on **1-866-218-2155** and they'll assist you further on how tooprovide them hello downloaded **Certificate (Base64)** file.</span></span>  
 
> [!TIP]
> <span data-ttu-id="5cd8e-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5cd8e-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5cd8e-171">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5cd8e-172">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5cd8e-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5cd8e-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5cd8e-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="5cd8e-174">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5cd8e-176">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5cd8e-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5cd8e-177">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5cd8e-179">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5cd8e-181">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5cd8e-183">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5cd8e-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ariba-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5cd8e-185">a.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-185">a.</span></span> <span data-ttu-id="5cd8e-186">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5cd8e-187">b.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-187">b.</span></span> <span data-ttu-id="5cd8e-188">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5cd8e-189">c.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-189">c.</span></span> <span data-ttu-id="5cd8e-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5cd8e-191">d.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-191">d.</span></span> <span data-ttu-id="5cd8e-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-192">Click **Create**.</span></span>
 
### <a name="creating-an-ariba-test-user"></a><span data-ttu-id="5cd8e-193">Vytvoření Ariba testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5cd8e-193">Creating an Ariba test user</span></span>

<span data-ttu-id="5cd8e-194">Hello cílem této části je toocreate volal Britta Simon v Ariba uživatele.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-194">hello objective of this section is toocreate a user called Britta Simon in Ariba.</span></span> <span data-ttu-id="5cd8e-195">Práce s Ariba tým podpory v **1-866-218-2155** tooadd hello uživatele v hello Ariba systému.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-195">Work with Ariba support team at **1-866-218-2155** tooadd hello users in hello Ariba system.</span></span> 

 >[!NOTE]
 ><span data-ttu-id="5cd8e-196">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello Ariba tým podpory v **1-866-218-2155**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-196">If you need toocreate a user manually, you need toocontact hello Ariba support team at **1-866-218-2155**.</span></span>
 >  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5cd8e-197">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5cd8e-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5cd8e-198">V této části povolíte tak, že udělíte přístup tooAriba toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAriba.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5cd8e-200">**tooassign Britta Simon tooAriba, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5cd8e-200">**tooassign Britta Simon tooAriba, perform hello following steps:**</span></span>

1. <span data-ttu-id="5cd8e-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5cd8e-203">V seznamu aplikace hello vyberte **Ariba**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-203">In hello applications list, select **Ariba**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_app.png) 

3. <span data-ttu-id="5cd8e-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5cd8e-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-207">Click **Add** button.</span></span> <span data-ttu-id="5cd8e-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5cd8e-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5cd8e-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5cd8e-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5cd8e-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5cd8e-213">Testing single sign-on</span></span>
<span data-ttu-id="5cd8e-214">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-214">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="5cd8e-215">Když kliknete na dlaždici Ariba hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Ariba aplikace.</span><span class="sxs-lookup"><span data-stu-id="5cd8e-215">When you click hello Ariba tile in hello Access Panel, you should get automatically signed-on tooyour Ariba application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5cd8e-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5cd8e-216">Additional resources</span></span>

* [<span data-ttu-id="5cd8e-217">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5cd8e-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5cd8e-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5cd8e-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_203.png

