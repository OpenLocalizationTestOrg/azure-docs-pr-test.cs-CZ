---
title: 'Kurz: Azure Active Directory integrace s UNIFI | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a UNIFI."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: af7cc1167b5c0cff2a1f4cdaa8a2b93f5a718f81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a><span data-ttu-id="66385-103">Kurz: Azure Active Directory integrace s UNIFI</span><span class="sxs-lookup"><span data-stu-id="66385-103">Tutorial: Azure Active Directory integration with UNIFI</span></span>

<span data-ttu-id="66385-104">V tomto kurzu zjistíte, jak toointegrate UNIFI službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="66385-104">In this tutorial, you learn how toointegrate UNIFI with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="66385-105">Integrace UNIFI s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="66385-105">Integrating UNIFI with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="66385-106">Můžete řídit ve službě Azure AD, který má přístup tooUNIFI</span><span class="sxs-lookup"><span data-stu-id="66385-106">You can control in Azure AD who has access tooUNIFI</span></span>
- <span data-ttu-id="66385-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooUNIFI (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="66385-107">You can enable your users tooautomatically get signed-on tooUNIFI (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="66385-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="66385-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="66385-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="66385-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66385-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="66385-110">Prerequisites</span></span>

<span data-ttu-id="66385-111">Integrace služby Azure AD s UNIFI tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="66385-111">tooconfigure Azure AD integration with UNIFI, you need hello following items:</span></span>

- <span data-ttu-id="66385-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="66385-112">An Azure AD subscription</span></span>
- <span data-ttu-id="66385-113">UNIFI jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="66385-113">A UNIFI single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="66385-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="66385-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="66385-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="66385-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="66385-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="66385-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="66385-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="66385-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="66385-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="66385-118">Scenario description</span></span>
<span data-ttu-id="66385-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="66385-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="66385-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="66385-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="66385-121">Přidání UNIFI z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="66385-121">Adding UNIFI from hello gallery</span></span>
2. <span data-ttu-id="66385-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="66385-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-unifi-from-hello-gallery"></a><span data-ttu-id="66385-123">Přidání UNIFI z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="66385-123">Adding UNIFI from hello gallery</span></span>
<span data-ttu-id="66385-124">tooconfigure hello integrace UNIFI do Azure AD, je nutné tooadd UNIFI hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="66385-124">tooconfigure hello integration of UNIFI into Azure AD, you need tooadd UNIFI from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="66385-125">**tooadd UNIFI z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="66385-125">**tooadd UNIFI from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="66385-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="66385-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="66385-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="66385-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="66385-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="66385-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="66385-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="66385-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="66385-133">Hello vyhledávacího pole zadejte **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="66385-133">In hello search box, type **UNIFI**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_search.png)

5. <span data-ttu-id="66385-135">Na panelu výsledků hello vyberte **UNIFI**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="66385-135">In hello results panel, select **UNIFI**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="66385-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="66385-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="66385-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UNIFI podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="66385-138">In this section, you configure and test Azure AD single sign-on with UNIFI based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="66385-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v UNIFI je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="66385-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UNIFI is tooa user in Azure AD.</span></span> <span data-ttu-id="66385-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v UNIFI musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="66385-140">In other words, a link relationship between an Azure AD user and hello related user in UNIFI needs toobe established.</span></span>

<span data-ttu-id="66385-141">V UNIFI, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="66385-141">In UNIFI, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="66385-142">tooconfigure a testu Azure AD jednotné přihlašování s UNIFI, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="66385-142">tooconfigure and test Azure AD single sign-on with UNIFI, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="66385-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="66385-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="66385-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66385-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="66385-145">**[Vytváření UNIFI testovací uživatel](#creating-a-unifi-test-user)**  -toohave protějšek Britta Simon v UNIFI, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="66385-145">**[Creating a UNIFI test user](#creating-a-unifi-test-user)** - toohave a counterpart of Britta Simon in UNIFI that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="66385-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="66385-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="66385-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="66385-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="66385-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="66385-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="66385-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci UNIFI.</span><span class="sxs-lookup"><span data-stu-id="66385-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UNIFI application.</span></span>

<span data-ttu-id="66385-150">**tooconfigure Azure AD jednotné přihlašování s UNIFI, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="66385-150">**tooconfigure Azure AD single sign-on with UNIFI, perform hello following steps:**</span></span>

1. <span data-ttu-id="66385-151">V portálu Azure, na hello hello **UNIFI** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="66385-151">In hello Azure portal, on hello **UNIFI** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="66385-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="66385-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_samlbase.png)

3. <span data-ttu-id="66385-155">Na hello **UNIFI domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="66385-155">On hello **UNIFI Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url1.png)

    <span data-ttu-id="66385-157">V hello **identifikátor** textovému poli, hodnota hello typu:`INVIEWlabs`</span><span class="sxs-lookup"><span data-stu-id="66385-157">In hello **Identifier** textbox, type hello value: `INVIEWlabs`</span></span> 

4. <span data-ttu-id="66385-158">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="66385-158">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url2.png)

    <span data-ttu-id="66385-160">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://app.discoverunifi.com/login`</span><span class="sxs-lookup"><span data-stu-id="66385-160">In hello **Sign-on URL** textbox, type hello URL: `https://app.discoverunifi.com/login`</span></span>

5. <span data-ttu-id="66385-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="66385-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_certificate.png) 

6. <span data-ttu-id="66385-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="66385-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="66385-165">Na hello **UNIFI konfigurace** klikněte na tlačítko **konfigurace UNIFI** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="66385-165">On hello **UNIFI Configuration** section, click **Configure UNIFI** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="66385-166">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="66385-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_configure.png)

8. <span data-ttu-id="66385-168">V okně prohlížeče jiných webových přihlásit tooyour **UNIFI** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="66385-168">In a different web browser window, sign on tooyour **UNIFI** company site as administrator.</span></span>

9. <span data-ttu-id="66385-169">Klikněte na hello **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="66385-169">Click on hello **Users**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/app1.png) 

10. <span data-ttu-id="66385-171">Klikněte na hello **přidat nového poskytovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="66385-171">Click on hello **Add New Identity Provider**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/app2.png)

11. <span data-ttu-id="66385-173">V hello **přidat zprostředkovatele Identity** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="66385-173">In hello **Add Identity Provider** section, perform hello following steps:</span></span>   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/app3.png) 

    <span data-ttu-id="66385-175">a.</span><span class="sxs-lookup"><span data-stu-id="66385-175">a.</span></span> <span data-ttu-id="66385-176">V hello **název zprostředkovatele** textovému poli, název typu hello hello zprostředkovatele Identity...</span><span class="sxs-lookup"><span data-stu-id="66385-176">In hello **Provider Name** textbox, type hello name of hello Identity Provider..</span></span>

    <span data-ttu-id="66385-177">b.</span><span class="sxs-lookup"><span data-stu-id="66385-177">b.</span></span> <span data-ttu-id="66385-178">V hello hello **adresa URL poskytovatele** textbox vložit hello **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="66385-178">In hello hello **Provider URL** textbox paste hello **SAML Single Sign-On Service URL** value, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="66385-179">c.</span><span class="sxs-lookup"><span data-stu-id="66385-179">c.</span></span> <span data-ttu-id="66385-180">Otevřete hello certifikát, který jste si stáhli z portálu Azure v poznámkovém bloku hello odebrat hello **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---** značku a potom vložte hello zbývající obsah Hello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="66385-180">Open hello Certificate that you have downloaded from hello Azure portal in notepad, remove hello **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste hello remaining content in hello **Certificate** textbox.</span></span>

    <span data-ttu-id="66385-181">d.</span><span class="sxs-lookup"><span data-stu-id="66385-181">d.</span></span> <span data-ttu-id="66385-182">Vyberte hello **je výchozí zprostředkovatel** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="66385-182">Select hello **is Default Provider** checkbox.</span></span>

> [!TIP]
> <span data-ttu-id="66385-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="66385-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="66385-184">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="66385-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="66385-185">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="66385-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="66385-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="66385-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="66385-187">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="66385-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="66385-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="66385-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="66385-190">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="66385-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="66385-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="66385-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="66385-194">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="66385-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="66385-196">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="66385-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="66385-198">a.</span><span class="sxs-lookup"><span data-stu-id="66385-198">a.</span></span> <span data-ttu-id="66385-199">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="66385-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="66385-200">b.</span><span class="sxs-lookup"><span data-stu-id="66385-200">b.</span></span> <span data-ttu-id="66385-201">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="66385-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="66385-202">c.</span><span class="sxs-lookup"><span data-stu-id="66385-202">c.</span></span> <span data-ttu-id="66385-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="66385-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="66385-204">d.</span><span class="sxs-lookup"><span data-stu-id="66385-204">d.</span></span> <span data-ttu-id="66385-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="66385-205">Click **Create**.</span></span>
 
### <a name="creating-a-unifi-test-user"></a><span data-ttu-id="66385-206">Vytvoření zkušebního uživatele UNIFI</span><span class="sxs-lookup"><span data-stu-id="66385-206">Creating a UNIFI test user</span></span>

<span data-ttu-id="66385-207">V této části vytvoříte názvem Britta Simon uživatele.</span><span class="sxs-lookup"><span data-stu-id="66385-207">In this section, you create a user called Britta Simon.</span></span> <span data-ttu-id="66385-208">**UNIFI** podporuje zřizování automatické uživatelů, takže nejsou potřeba žádné ruční kroky.</span><span class="sxs-lookup"><span data-stu-id="66385-208">**UNIFI** supports automatic user provisioning so no manual steps are required.</span></span> <span data-ttu-id="66385-209">Uživatelé se vytvoří automaticky po úspěšném ověření z hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="66385-209">Users are created automatically after successful authentication from hello Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="66385-210">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="66385-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="66385-211">V této části povolíte tak, že udělíte přístup tooUNIFI toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="66385-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUNIFI.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="66385-213">**tooassign Britta Simon tooUNIFI, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="66385-213">**tooassign Britta Simon tooUNIFI, perform hello following steps:**</span></span>

1. <span data-ttu-id="66385-214">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="66385-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="66385-216">V seznamu aplikace hello vyberte **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="66385-216">In hello applications list, select **UNIFI**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_app.png) 

3. <span data-ttu-id="66385-218">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="66385-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="66385-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="66385-220">Click **Add** button.</span></span> <span data-ttu-id="66385-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66385-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="66385-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="66385-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="66385-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66385-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="66385-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="66385-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="66385-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="66385-226">Testing single sign-on</span></span>

<span data-ttu-id="66385-227">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="66385-227">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="66385-228">Po kliknutí na tlačítko hello UNIFI dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour UNIFI aplikace.</span><span class="sxs-lookup"><span data-stu-id="66385-228">When you click hello UNIFI tile in hello Access Panel, you should get automatically signed-on tooyour UNIFI application.</span></span>
<span data-ttu-id="66385-229">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="66385-229">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="66385-230">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="66385-230">Additional resources</span></span>

* [<span data-ttu-id="66385-231">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="66385-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="66385-232">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="66385-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_203.png

