---
title: 'Kurz: Azure Active Directory integrace s Kronos | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Kronos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 16fd5c203162d10b78f51b00d79017adaf8632c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a><span data-ttu-id="6ed66-103">Kurz: Azure Active Directory integrace s Kronos</span><span class="sxs-lookup"><span data-stu-id="6ed66-103">Tutorial: Azure Active Directory integration with Kronos</span></span>

<span data-ttu-id="6ed66-104">V tomto kurzu zjistíte, jak toointegrate Kronos s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6ed66-104">In this tutorial, you learn how toointegrate Kronos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6ed66-105">Integrace Kronos s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6ed66-105">Integrating Kronos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6ed66-106">Můžete řídit ve službě Azure AD, který má přístup tooKronos</span><span class="sxs-lookup"><span data-stu-id="6ed66-106">You can control in Azure AD who has access tooKronos</span></span>
- <span data-ttu-id="6ed66-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooKronos (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6ed66-107">You can enable your users tooautomatically get signed-on tooKronos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6ed66-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6ed66-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6ed66-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6ed66-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ed66-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6ed66-110">Prerequisites</span></span>

<span data-ttu-id="6ed66-111">Integrace služby Azure AD s Kronos tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6ed66-111">tooconfigure Azure AD integration with Kronos, you need hello following items:</span></span>

- <span data-ttu-id="6ed66-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6ed66-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6ed66-113">A **Kronos pracovníků centrální** předplatné povolené jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6ed66-113">A **Kronos Workforce Central** SSO enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6ed66-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6ed66-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6ed66-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6ed66-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6ed66-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6ed66-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6ed66-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6ed66-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6ed66-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6ed66-118">Scenario description</span></span>
<span data-ttu-id="6ed66-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6ed66-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6ed66-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6ed66-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6ed66-121">Přidání Kronos z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6ed66-121">Adding Kronos from hello gallery</span></span>
2. <span data-ttu-id="6ed66-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6ed66-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kronos-from-hello-gallery"></a><span data-ttu-id="6ed66-123">Přidání Kronos z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6ed66-123">Adding Kronos from hello gallery</span></span>
<span data-ttu-id="6ed66-124">tooconfigure hello integrace Kronos do Azure AD, je nutné tooadd Kronos hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6ed66-124">tooconfigure hello integration of Kronos into Azure AD, you need tooadd Kronos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6ed66-125">**tooadd Kronos z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6ed66-125">**tooadd Kronos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ed66-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6ed66-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6ed66-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6ed66-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6ed66-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6ed66-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6ed66-133">Hello vyhledávacího pole zadejte **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-133">In hello search box, type **Kronos**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_search.png)

5. <span data-ttu-id="6ed66-135">Na panelu výsledků hello vyberte **Kronos**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ed66-135">In hello results panel, select **Kronos**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6ed66-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6ed66-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6ed66-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kronos podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="6ed66-138">In this section, you configure and test Azure AD single sign-on with Kronos based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6ed66-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Kronos je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6ed66-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kronos is tooa user in Azure AD.</span></span> <span data-ttu-id="6ed66-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Kronos musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6ed66-140">In other words, a link relationship between an Azure AD user and hello related user in Kronos needs toobe established.</span></span>

<span data-ttu-id="6ed66-141">V Kronos, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6ed66-141">In Kronos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6ed66-142">tooconfigure a testu Azure AD jednotné přihlašování s Kronos, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6ed66-142">tooconfigure and test Azure AD single sign-on with Kronos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6ed66-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6ed66-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6ed66-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6ed66-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6ed66-145">**[Vytvoření zkušebního uživatele Kronos](#creating-a-kronos-test-user)**  -toohave protějšek Britta Simon v Kronos, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6ed66-145">**[Creating a Kronos test user](#creating-a-kronos-test-user)** - toohave a counterpart of Britta Simon in Kronos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6ed66-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6ed66-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6ed66-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6ed66-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6ed66-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6ed66-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6ed66-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Kronos.</span><span class="sxs-lookup"><span data-stu-id="6ed66-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kronos application.</span></span>

<span data-ttu-id="6ed66-150">**tooconfigure Azure AD jednotné přihlašování s Kronos, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6ed66-150">**tooconfigure Azure AD single sign-on with Kronos, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ed66-151">V portálu Azure, na hello hello **Kronos** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-151">In hello Azure portal, on hello **Kronos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6ed66-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6ed66-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_samlbase.png)

3. <span data-ttu-id="6ed66-155">Na hello **Kronos domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6ed66-155">On hello **Kronos Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_url.png)

    <span data-ttu-id="6ed66-157">a.</span><span class="sxs-lookup"><span data-stu-id="6ed66-157">a.</span></span> <span data-ttu-id="6ed66-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.kronos.net/`</span><span class="sxs-lookup"><span data-stu-id="6ed66-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.kronos.net/`</span></span>

    <span data-ttu-id="6ed66-159">b.</span><span class="sxs-lookup"><span data-stu-id="6ed66-159">b.</span></span> <span data-ttu-id="6ed66-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span><span class="sxs-lookup"><span data-stu-id="6ed66-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6ed66-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6ed66-161">These values are not real.</span></span> <span data-ttu-id="6ed66-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6ed66-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="6ed66-163">Obraťte se na [tým podpory Kronos](https://www.kronos.in/contact/en-in/form) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6ed66-163">Contact [Kronos support team](https://www.kronos.in/contact/en-in/form) tooget these values.</span></span>
 
4. <span data-ttu-id="6ed66-164">Aplikace Kronos očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="6ed66-164">Your Kronos application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="6ed66-165">Práce s [tým podpory Kronos](https://www.kronos.in/contact/en-in/form) první tooidentify hello správný identifikátor uživatele, který je namapován do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6ed66-165">Work with [Kronos support team](https://www.kronos.in/contact/en-in/form) first tooidentify hello correct user identifier, which is mapped into hello application.</span></span> <span data-ttu-id="6ed66-166">Také proveďte hello pokyny o hello atribut, který chtějí toouse pro toto mapování.</span><span class="sxs-lookup"><span data-stu-id="6ed66-166">Also please take hello guidance about hello attribute, which they want toouse for this mapping.</span></span>
 
     <span data-ttu-id="6ed66-167">Společnost Microsoft doporučuje používat hello **"NameIdentifier"** atribut jako identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="6ed66-167">Microsoft recommends using hello **"NameIdentifier"** attribute as user identifier.</span></span> <span data-ttu-id="6ed66-168">Můžete spravovat hello hodnoty těchto atributů z hello **"Atributy uživatele"** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ed66-168">You can manage hello values of these attributes from hello **"User Attributes"** section on application integration page.</span></span>
     
     <span data-ttu-id="6ed66-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="6ed66-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="6ed66-170">Zde jsme jste namapovali hello **uživatelský identifikátor (nameid)** s **ExtractMailPrefix()** funkce **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-170">Here we have mapped hello **User Identifier (nameid)** with **ExtractMailPrefix()** function of **user.userprincipalname**.</span></span> <span data-ttu-id="6ed66-171">Díky tomu hodnotu předpony hello e-mailů hello uživatele, který je hello jedinečné ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="6ed66-171">This gives hello prefix value of email of hello user which is hello unique User ID.</span></span> <span data-ttu-id="6ed66-172">Ta se odešle toohello Kronos aplikace v každé úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="6ed66-172">This is sent toohello Kronos application in every successful response.</span></span> 
     
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_attribute.png)

5. <span data-ttu-id="6ed66-174">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="6ed66-174">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="6ed66-175">a.</span><span class="sxs-lookup"><span data-stu-id="6ed66-175">a.</span></span> <span data-ttu-id="6ed66-176">Hello uživatelský identifikátor rozevíracího seznamu vyberte **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-176">In hello User Identifier dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="6ed66-177">b.</span><span class="sxs-lookup"><span data-stu-id="6ed66-177">b.</span></span> <span data-ttu-id="6ed66-178">V hello **e-mailu** rozevíracího seznamu vyberte **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-178">In hello **Mail** dropdown list, select **user.userprincipalname**.</span></span>

6. <span data-ttu-id="6ed66-179">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6ed66-179">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_certificate.png) 

7. <span data-ttu-id="6ed66-181">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6ed66-181">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6ed66-183">tooconfigure jednotného přihlašování na **Kronos** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Kronos](https://www.kronos.in/contact/en-in/form).</span><span class="sxs-lookup"><span data-stu-id="6ed66-183">tooconfigure single sign-on on **Kronos** side, you need toosend hello downloaded **Metadata XML** too[Kronos support team](https://www.kronos.in/contact/en-in/form).</span></span> 

> [!TIP]
> <span data-ttu-id="6ed66-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6ed66-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6ed66-185">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6ed66-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6ed66-186">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6ed66-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6ed66-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6ed66-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="6ed66-188">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6ed66-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6ed66-190">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6ed66-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ed66-191">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6ed66-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6ed66-193">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6ed66-195">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6ed66-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6ed66-197">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6ed66-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6ed66-199">a.</span><span class="sxs-lookup"><span data-stu-id="6ed66-199">a.</span></span> <span data-ttu-id="6ed66-200">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6ed66-201">b.</span><span class="sxs-lookup"><span data-stu-id="6ed66-201">b.</span></span> <span data-ttu-id="6ed66-202">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6ed66-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6ed66-203">c.</span><span class="sxs-lookup"><span data-stu-id="6ed66-203">c.</span></span> <span data-ttu-id="6ed66-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6ed66-205">d.</span><span class="sxs-lookup"><span data-stu-id="6ed66-205">d.</span></span> <span data-ttu-id="6ed66-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-206">Click **Create**.</span></span>
 
### <a name="creating-a-kronos-test-user"></a><span data-ttu-id="6ed66-207">Vytvoření zkušebního uživatele Kronos</span><span class="sxs-lookup"><span data-stu-id="6ed66-207">Creating a Kronos test user</span></span>

<span data-ttu-id="6ed66-208">V této části vytvoříte volal Britta Simon v Kronos uživatele.</span><span class="sxs-lookup"><span data-stu-id="6ed66-208">In this section, you create a user called Britta Simon in Kronos.</span></span> <span data-ttu-id="6ed66-209">Kronos aplikace, musí všechny toobe uživatelé hello zřízené v hello aplikace před provedením jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6ed66-209">Kronos application needs all hello users toobe provisioned in hello application before doing SSO.</span></span> 

<span data-ttu-id="6ed66-210">Práce s hello [tým podpory Kronos](https://www.kronos.in/contact/en-in/form) tooprovision tito uživatelé do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6ed66-210">Work with hello [Kronos support team](https://www.kronos.in/contact/en-in/form) tooprovision all these users into hello application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6ed66-211">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6ed66-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6ed66-212">V této části povolíte tak, že udělíte přístup tooKronos toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6ed66-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKronos.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6ed66-214">**tooassign Britta Simon tooKronos, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6ed66-214">**tooassign Britta Simon tooKronos, perform hello following steps:**</span></span>

1. <span data-ttu-id="6ed66-215">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6ed66-217">V seznamu aplikace hello vyberte **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-217">In hello applications list, select **Kronos**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_app.png) 

3. <span data-ttu-id="6ed66-219">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6ed66-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6ed66-221">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6ed66-221">Click **Add** button.</span></span> <span data-ttu-id="6ed66-222">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6ed66-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6ed66-224">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6ed66-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6ed66-225">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6ed66-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6ed66-226">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6ed66-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6ed66-227">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6ed66-227">Testing single sign-on</span></span>

<span data-ttu-id="6ed66-228">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6ed66-228">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="6ed66-229">Po kliknutí na tlačítko hello Kronos dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Kronos aplikace.</span><span class="sxs-lookup"><span data-stu-id="6ed66-229">When you click hello Kronos tile in hello Access Panel, you should get automatically signed-on tooyour Kronos application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ed66-230">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6ed66-230">Additional resources</span></span>

* [<span data-ttu-id="6ed66-231">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6ed66-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6ed66-232">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6ed66-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png

