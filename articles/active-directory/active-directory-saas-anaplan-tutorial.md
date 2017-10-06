---
title: 'Kurz: Azure Active Directory integrace s Anaplan | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Anaplan."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4a9c2914-6c8c-4a88-93e3-3753afb40e6b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jeedes
ms.openlocfilehash: 38eb210d090e7aa720060680259124e941e30541
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-anaplan"></a><span data-ttu-id="3eb46-103">Kurz: Azure Active Directory integrace s Anaplan</span><span class="sxs-lookup"><span data-stu-id="3eb46-103">Tutorial: Azure Active Directory integration with Anaplan</span></span>

<span data-ttu-id="3eb46-104">V tomto kurzu zjistíte, jak toointegrate Anaplan s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3eb46-104">In this tutorial, you learn how toointegrate Anaplan with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3eb46-105">Integrace Anaplan s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3eb46-105">Integrating Anaplan with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3eb46-106">Můžete řídit ve službě Azure AD, který má přístup tooAnaplan</span><span class="sxs-lookup"><span data-stu-id="3eb46-106">You can control in Azure AD who has access tooAnaplan</span></span>
- <span data-ttu-id="3eb46-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAnaplan (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3eb46-107">You can enable your users tooautomatically get signed-on tooAnaplan (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3eb46-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3eb46-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3eb46-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3eb46-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3eb46-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3eb46-110">Prerequisites</span></span>

<span data-ttu-id="3eb46-111">Integrace služby Azure AD s Anaplan tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3eb46-111">tooconfigure Azure AD integration with Anaplan, you need hello following items:</span></span>

- <span data-ttu-id="3eb46-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3eb46-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3eb46-113">Anaplan jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3eb46-113">An Anaplan single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3eb46-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3eb46-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3eb46-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3eb46-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3eb46-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3eb46-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3eb46-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3eb46-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3eb46-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3eb46-118">Scenario description</span></span>
<span data-ttu-id="3eb46-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3eb46-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3eb46-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3eb46-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3eb46-121">Přidání Anaplan z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3eb46-121">Adding Anaplan from hello gallery</span></span>
2. <span data-ttu-id="3eb46-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3eb46-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-anaplan-from-hello-gallery"></a><span data-ttu-id="3eb46-123">Přidání Anaplan z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3eb46-123">Adding Anaplan from hello gallery</span></span>
<span data-ttu-id="3eb46-124">tooconfigure hello integrace Anaplan do Azure AD, je nutné tooadd Anaplan hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3eb46-124">tooconfigure hello integration of Anaplan into Azure AD, you need tooadd Anaplan from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3eb46-125">**tooadd Anaplan z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3eb46-125">**tooadd Anaplan from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3eb46-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3eb46-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3eb46-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3eb46-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3eb46-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3eb46-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3eb46-133">Hello vyhledávacího pole zadejte **Anaplan**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-133">In hello search box, type **Anaplan**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_search.png)

5. <span data-ttu-id="3eb46-135">Na panelu výsledků hello vyberte **Anaplan**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3eb46-135">In hello results panel, select **Anaplan**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3eb46-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3eb46-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3eb46-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Anaplan podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3eb46-138">In this section, you configure and test Azure AD single sign-on with Anaplan based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3eb46-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Anaplan je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3eb46-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Anaplan is tooa user in Azure AD.</span></span> <span data-ttu-id="3eb46-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Anaplan musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="3eb46-140">In other words, a link relationship between an Azure AD user and hello related user in Anaplan needs toobe established.</span></span>

<span data-ttu-id="3eb46-141">V Anaplan, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="3eb46-141">In Anaplan, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3eb46-142">tooconfigure a testu Azure AD jednotné přihlašování s Anaplan, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="3eb46-142">tooconfigure and test Azure AD single sign-on with Anaplan, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3eb46-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="3eb46-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3eb46-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3eb46-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3eb46-145">**[Vytváření testovacího uživatele Anaplan](#creating-an-anaplan-test-user)**  -toohave protějšek Britta Simon v Anaplan, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="3eb46-145">**[Creating an Anaplan test user](#creating-an-anaplan-test-user)** - toohave a counterpart of Britta Simon in Anaplan that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3eb46-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3eb46-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3eb46-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="3eb46-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3eb46-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3eb46-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3eb46-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Anaplan.</span><span class="sxs-lookup"><span data-stu-id="3eb46-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Anaplan application.</span></span>

<span data-ttu-id="3eb46-150">**tooconfigure Azure AD jednotné přihlašování s Anaplan, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3eb46-150">**tooconfigure Azure AD single sign-on with Anaplan, perform hello following steps:**</span></span>

1. <span data-ttu-id="3eb46-151">V portálu Azure, na hello hello **Anaplan** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-151">In hello Azure portal, on hello **Anaplan** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3eb46-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3eb46-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_samlbase.png)

3. <span data-ttu-id="3eb46-155">Na hello **Anaplan domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3eb46-155">On hello **Anaplan Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_url.png)

    <span data-ttu-id="3eb46-157">a.</span><span class="sxs-lookup"><span data-stu-id="3eb46-157">a.</span></span> <span data-ttu-id="3eb46-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://sdp.anaplan.com/frontdoor/saml/<tenant name>`</span><span class="sxs-lookup"><span data-stu-id="3eb46-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://sdp.anaplan.com/frontdoor/saml/<tenant name>`</span></span>

    <span data-ttu-id="3eb46-159">b.</span><span class="sxs-lookup"><span data-stu-id="3eb46-159">b.</span></span> <span data-ttu-id="3eb46-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.anaplan.com`</span><span class="sxs-lookup"><span data-stu-id="3eb46-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.anaplan.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3eb46-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3eb46-161">These values are not real.</span></span> <span data-ttu-id="3eb46-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="3eb46-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3eb46-163">Obraťte se na [tým podpory Anaplan klienta](mailto:support@anaplan.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3eb46-163">Contact [Anaplan Client support team](mailto:support@anaplan.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="3eb46-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3eb46-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_certificate.png) 

5. <span data-ttu-id="3eb46-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3eb46-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-anaplan-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3eb46-168">Na hello **Anaplan konfigurace** klikněte na tlačítko **konfigurace Anaplan** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3eb46-168">On hello **Anaplan Configuration** section, click **Configure Anaplan** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3eb46-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3eb46-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_configure.png) 

7. <span data-ttu-id="3eb46-171">tooconfigure jednotného přihlašování na **Anaplan** straně, je nutné stáhnout hello toosend **soubor XML s metadaty**, **SAML Entity ID**, **SAML jeden přihlašování adresa URL služby**  a **Sign-Out URL** příliš[tým podpory Anaplan](mailto:support@anaplan.com).</span><span class="sxs-lookup"><span data-stu-id="3eb46-171">tooconfigure single sign-on on **Anaplan** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL**   too[Anaplan support team](mailto:support@anaplan.com).</span></span> <span data-ttu-id="3eb46-172">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="3eb46-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3eb46-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="3eb46-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3eb46-174">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="3eb46-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3eb46-175">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3eb46-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3eb46-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3eb46-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="3eb46-177">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="3eb46-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3eb46-179">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3eb46-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3eb46-180">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3eb46-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3eb46-182">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3eb46-184">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="3eb46-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3eb46-186">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3eb46-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3eb46-188">a.</span><span class="sxs-lookup"><span data-stu-id="3eb46-188">a.</span></span> <span data-ttu-id="3eb46-189">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3eb46-190">b.</span><span class="sxs-lookup"><span data-stu-id="3eb46-190">b.</span></span> <span data-ttu-id="3eb46-191">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3eb46-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3eb46-192">c.</span><span class="sxs-lookup"><span data-stu-id="3eb46-192">c.</span></span> <span data-ttu-id="3eb46-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3eb46-194">d.</span><span class="sxs-lookup"><span data-stu-id="3eb46-194">d.</span></span> <span data-ttu-id="3eb46-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-195">Click **Create**.</span></span>
 
### <a name="creating-an-anaplan-test-user"></a><span data-ttu-id="3eb46-196">Vytváření testovacího uživatele Anaplan</span><span class="sxs-lookup"><span data-stu-id="3eb46-196">Creating an Anaplan test user</span></span>

<span data-ttu-id="3eb46-197">V této části vytvoříte volal Britta Simon v Anaplan uživatele.</span><span class="sxs-lookup"><span data-stu-id="3eb46-197">In this section, you create a user called Britta Simon in Anaplan.</span></span> <span data-ttu-id="3eb46-198">Spojte se s [tým podpory Anaplan](mailto:support@anaplan.com) tooadd hello uživatelé v platformě Anaplan hello.</span><span class="sxs-lookup"><span data-stu-id="3eb46-198">Please work with [Anaplan support team](mailto:support@anaplan.com) tooadd hello users in hello Anaplan platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3eb46-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3eb46-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3eb46-200">V této části povolíte tak, že udělíte přístup tooAnaplan toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3eb46-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAnaplan.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3eb46-202">**tooassign Britta Simon tooAnaplan, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3eb46-202">**tooassign Britta Simon tooAnaplan, perform hello following steps:**</span></span>

1. <span data-ttu-id="3eb46-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3eb46-205">V seznamu aplikace hello vyberte **Anaplan**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-205">In hello applications list, select **Anaplan**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_app.png) 

3. <span data-ttu-id="3eb46-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3eb46-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3eb46-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3eb46-209">Click **Add** button.</span></span> <span data-ttu-id="3eb46-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3eb46-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3eb46-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="3eb46-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3eb46-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3eb46-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3eb46-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3eb46-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3eb46-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3eb46-215">Testing single sign-on</span></span>

<span data-ttu-id="3eb46-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3eb46-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3eb46-217">Když kliknete na dlaždici Anaplan hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Anaplan aplikace.</span><span class="sxs-lookup"><span data-stu-id="3eb46-217">When you click hello Anaplan tile in hello Access Panel, you should get automatically signed-on tooyour Anaplan application.</span></span>

<span data-ttu-id="3eb46-218">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3eb46-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3eb46-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3eb46-219">Additional resources</span></span>

* [<span data-ttu-id="3eb46-220">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3eb46-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3eb46-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3eb46-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_203.png

