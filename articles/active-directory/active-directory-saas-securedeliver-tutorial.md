---
title: "Kurz: Azure Active Directory integrace s ZABEZPEČENÉ DORUČOVÁNÍ | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ZABEZPEČENÉ DORUČOVÁNÍ."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fccd5668-fe6f-4e6d-a9ce-ba4f321c33d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 13699a9abb8be24054b0810a8178cd5ef170c338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a><span data-ttu-id="9f3ff-103">Kurz: Azure Active Directory integrace s ZABEZPEČENÉ DORUČOVÁNÍ</span><span class="sxs-lookup"><span data-stu-id="9f3ff-103">Tutorial: Azure Active Directory integration with SECURE DELIVER</span></span>

<span data-ttu-id="9f3ff-104">V tomto kurzu zjistíte, jak ZABEZPEČIT toointegrate DORUČIT službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9f3ff-104">In this tutorial, you learn how toointegrate SECURE DELIVER with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9f3ff-105">Integrace ZABEZPEČENÉ DORUČOVÁNÍ s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9f3ff-105">Integrating SECURE DELIVER with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9f3ff-106">Můžete řídit ve službě Azure AD, který má přístup tooSECURE DORUČIT</span><span class="sxs-lookup"><span data-stu-id="9f3ff-106">You can control in Azure AD who has access tooSECURE DELIVER</span></span>
- <span data-ttu-id="9f3ff-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSECURE DORUČIT (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f3ff-107">You can enable your users tooautomatically get signed-on tooSECURE DELIVER (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9f3ff-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9f3ff-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9f3ff-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9f3ff-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f3ff-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f3ff-110">Prerequisites</span></span>

<span data-ttu-id="9f3ff-111">tooconfigure integrace Azure AD s ZABEZPEČENÉ DORUČOVÁNÍ, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9f3ff-111">tooconfigure Azure AD integration with SECURE DELIVER, you need hello following items:</span></span>

- <span data-ttu-id="9f3ff-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f3ff-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9f3ff-113">ZABEZPEČENÍ poskytovat jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9f3ff-113">A SECURE DELIVER single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9f3ff-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9f3ff-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9f3ff-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9f3ff-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9f3ff-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9f3ff-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9f3ff-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9f3ff-118">Scenario description</span></span>
<span data-ttu-id="9f3ff-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9f3ff-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9f3ff-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9f3ff-121">Přidání ZABEZPEČENÉ DORUČOVÁNÍ z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9f3ff-121">Adding SECURE DELIVER from hello gallery</span></span>
2. <span data-ttu-id="9f3ff-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f3ff-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-secure-deliver-from-hello-gallery"></a><span data-ttu-id="9f3ff-123">Přidání ZABEZPEČENÉ DORUČOVÁNÍ z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9f3ff-123">Adding SECURE DELIVER from hello gallery</span></span>
<span data-ttu-id="9f3ff-124">tooconfigure hello integrace ZABEZPEČENÉ DORUČOVÁNÍ do Azure AD, je nutné tooadd zabezpečeného DORUČIT hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-124">tooconfigure hello integration of SECURE DELIVER into Azure AD, you need tooadd SECURE DELIVER from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9f3ff-125">**tooadd zabezpečeného DORUČIT z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f3ff-125">**tooadd SECURE DELIVER from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f3ff-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9f3ff-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9f3ff-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9f3ff-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9f3ff-133">Hello vyhledávacího pole zadejte **ZABEZPEČENÉ DORUČOVÁNÍ**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-133">In hello search box, type **SECURE DELIVER**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_search.png)

5. <span data-ttu-id="9f3ff-135">Na panelu výsledků hello vyberte **ZABEZPEČENÉ DORUČOVÁNÍ**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-135">In hello results panel, select **SECURE DELIVER**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9f3ff-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f3ff-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9f3ff-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ZABEZPEČENÉ DORUČOVÁNÍ podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9f3ff-138">In this section, you configure and test Azure AD single sign-on with SECURE DELIVER based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9f3ff-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ZABEZPEČENÉ DORUČOVÁNÍ je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SECURE DELIVER is tooa user in Azure AD.</span></span> <span data-ttu-id="9f3ff-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ZABEZPEČENÉ DORUČOVÁNÍ musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-140">In other words, a link relationship between an Azure AD user and hello related user in SECURE DELIVER needs toobe established.</span></span>

<span data-ttu-id="9f3ff-141">V ZABEZPEČENÉ DORUČOVÁNÍ, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-141">In SECURE DELIVER, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9f3ff-142">tooconfigure a testu Azure AD jednotné přihlašování s ZABEZPEČENÉ DORUČOVÁNÍ, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9f3ff-142">tooconfigure and test Azure AD single sign-on with SECURE DELIVER, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9f3ff-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9f3ff-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9f3ff-145">**[Vytvoření zkušebního uživatele ZABEZPEČENÉ DORUČOVÁNÍ](#creating-a-secure-deliver-test-user)**  -toohave protějšek Britta Simon v ZABEZPEČENÉ DORUČOVÁNÍ, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-145">**[Creating a SECURE DELIVER test user](#creating-a-secure-deliver-test-user)** - toohave a counterpart of Britta Simon in SECURE DELIVER that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9f3ff-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9f3ff-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9f3ff-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f3ff-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9f3ff-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ZABEZPEČENÉ DORUČOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SECURE DELIVER application.</span></span>

<span data-ttu-id="9f3ff-150">**tooconfigure Azure AD jednotné přihlašování s ZABEZPEČENÉ DORUČOVÁNÍ, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f3ff-150">**tooconfigure Azure AD single sign-on with SECURE DELIVER, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f3ff-151">V portálu Azure, na hello hello **ZABEZPEČENÉ DORUČOVÁNÍ** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-151">In hello Azure portal, on hello **SECURE DELIVER** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9f3ff-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_samlbase.png)

3. <span data-ttu-id="9f3ff-155">Na hello **ZABEZPEČENÉ DORUČOVÁNÍ domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f3ff-155">On hello **SECURE DELIVER Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_url.png)

    <span data-ttu-id="9f3ff-157">a.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-157">a.</span></span> <span data-ttu-id="9f3ff-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span><span class="sxs-lookup"><span data-stu-id="9f3ff-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span></span>

    <span data-ttu-id="9f3ff-159">b.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-159">b.</span></span> <span data-ttu-id="9f3ff-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span><span class="sxs-lookup"><span data-stu-id="9f3ff-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9f3ff-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-161">These values are not real.</span></span> <span data-ttu-id="9f3ff-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9f3ff-163">Obraťte se na [tým podpory zabezpečení klienta poskytovat](mailto:iw-sd-support@fujifilm.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-163">Contact [SECURE DELIVER Client support team](mailto:iw-sd-support@fujifilm.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="9f3ff-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_certificate.png) 

5. <span data-ttu-id="9f3ff-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9f3ff-168">Na hello **ZABEZPEČENÉ DORUČOVÁNÍ konfigurace** klikněte na tlačítko **nakonfigurovat ZABEZPEČENÉ DORUČOVÁNÍ** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-168">On hello **SECURE DELIVER Configuration** section, click **Configure SECURE DELIVER** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9f3ff-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9f3ff-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_configure.png) 

7. <span data-ttu-id="9f3ff-171">tooconfigure jednotného přihlašování na **ZABEZPEČENÉ DORUČOVÁNÍ** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[ZABEZPEČENÉ DORUČOVÁNÍ tým podpory](mailto:iw-sd-support@fujifilm.com).</span><span class="sxs-lookup"><span data-stu-id="9f3ff-171">tooconfigure single sign-on on **SECURE DELIVER** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com).</span></span> <span data-ttu-id="9f3ff-172">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="9f3ff-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="9f3ff-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9f3ff-174">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9f3ff-175">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9f3ff-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9f3ff-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f3ff-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="9f3ff-177">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9f3ff-179">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f3ff-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f3ff-180">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9f3ff-182">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9f3ff-184">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9f3ff-186">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f3ff-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9f3ff-188">a.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-188">a.</span></span> <span data-ttu-id="9f3ff-189">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9f3ff-190">b.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-190">b.</span></span> <span data-ttu-id="9f3ff-191">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9f3ff-192">c.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-192">c.</span></span> <span data-ttu-id="9f3ff-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9f3ff-194">d.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-194">d.</span></span> <span data-ttu-id="9f3ff-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-195">Click **Create**.</span></span>
 
### <a name="creating-a-secure-deliver-test-user"></a><span data-ttu-id="9f3ff-196">Vytvoření zkušebního uživatele ZABEZPEČENÉ DORUČOVÁNÍ</span><span class="sxs-lookup"><span data-stu-id="9f3ff-196">Creating a SECURE DELIVER test user</span></span>

<span data-ttu-id="9f3ff-197">Hello cílem této části je toocreate uživatel volal Britta Simon v ZABEZPEČENÉ DORUČOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-197">hello objective of this section is toocreate a user called Britta Simon in SECURE DELIVER.</span></span> <span data-ttu-id="9f3ff-198">Práce s [ZABEZPEČENÉ DORUČOVÁNÍ tým podpory](mailto:iw-sd-support@fujifilm.com) tooadd hello uživatele v hello ZABEZPEČENÉ DORUČOVÁNÍ účtu.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-198">Work with [SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com) tooadd hello users in hello SECURE DELIVER account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9f3ff-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9f3ff-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9f3ff-200">V této části povolíte tak, že udělíte přístup tooSECURE DORUČIT Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSECURE DELIVER.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9f3ff-202">**tooassign tooSECURE Britta Simon DORUČIT, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f3ff-202">**tooassign Britta Simon tooSECURE DELIVER, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f3ff-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9f3ff-205">V seznamu aplikace hello vyberte **ZABEZPEČENÉ DORUČOVÁNÍ**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-205">In hello applications list, select **SECURE DELIVER**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_app.png) 

3. <span data-ttu-id="9f3ff-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9f3ff-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-209">Click **Add** button.</span></span> <span data-ttu-id="9f3ff-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9f3ff-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9f3ff-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9f3ff-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9f3ff-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f3ff-215">Testing single sign-on</span></span>

<span data-ttu-id="9f3ff-216">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-216">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="9f3ff-217">Po kliknutí na tlačítko hello ZABEZPEČENÉ DORUČOVÁNÍ dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ZABEZPEČENÉ DORUČOVÁNÍ aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f3ff-217">When you click hello SECURE DELIVER tile in hello Access Panel, you should get automatically signed-on tooyour SECURE DELIVER application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f3ff-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9f3ff-218">Additional resources</span></span>

* [<span data-ttu-id="9f3ff-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f3ff-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9f3ff-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9f3ff-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png

