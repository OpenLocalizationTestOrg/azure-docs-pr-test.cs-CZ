---
title: "Kurz: Azure Active Directory integrace s konkrétně | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a konkrétně."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: b0477ca6fa52a21abea7de458f8a99a01e8c25c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a><span data-ttu-id="f5193-103">Kurz: Azure Active Directory integrace s konkrétně</span><span class="sxs-lookup"><span data-stu-id="f5193-103">Tutorial: Azure Active Directory integration with Namely</span></span>

<span data-ttu-id="f5193-104">V tomto kurzu zjistíte, jak toointegrate konkrétně v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f5193-104">In this tutorial, you learn how toointegrate Namely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f5193-105">Konkrétně integraci s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f5193-105">Integrating Namely with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f5193-106">Můžete řídit ve službě Azure AD, který má přístup tooNamely</span><span class="sxs-lookup"><span data-stu-id="f5193-106">You can control in Azure AD who has access tooNamely</span></span>
- <span data-ttu-id="f5193-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNamely (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5193-107">You can enable your users tooautomatically get signed-on tooNamely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f5193-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f5193-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f5193-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f5193-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5193-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f5193-110">Prerequisites</span></span>

<span data-ttu-id="f5193-111">integrace tooconfigure Azure AD s konkrétně, budete potřebovat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f5193-111">tooconfigure Azure AD integration with Namely, you need hello following items:</span></span>

- <span data-ttu-id="f5193-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5193-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f5193-113">Konkrétně jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f5193-113">A Namely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f5193-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f5193-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f5193-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f5193-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f5193-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f5193-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f5193-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f5193-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f5193-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f5193-118">Scenario description</span></span>
<span data-ttu-id="f5193-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f5193-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f5193-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f5193-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f5193-121">Přidání konkrétně z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f5193-121">Adding Namely from hello gallery</span></span>
2. <span data-ttu-id="f5193-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f5193-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-namely-from-hello-gallery"></a><span data-ttu-id="f5193-123">Přidání konkrétně z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f5193-123">Adding Namely from hello gallery</span></span>
<span data-ttu-id="f5193-124">tooconfigure hello integrace konkrétně do Azure AD, je nutné tooadd konkrétně z hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f5193-124">tooconfigure hello integration of Namely into Azure AD, you need tooadd Namely from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f5193-125">**tooadd konkrétně z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f5193-125">**tooadd Namely from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5193-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f5193-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f5193-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f5193-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f5193-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f5193-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f5193-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f5193-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f5193-133">Hello vyhledávacího pole zadejte **konkrétně**.</span><span class="sxs-lookup"><span data-stu-id="f5193-133">In hello search box, type **Namely**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. <span data-ttu-id="f5193-135">Na panelu výsledků hello vyberte **konkrétně**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5193-135">In hello results panel, select **Namely**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f5193-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f5193-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f5193-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s, a to podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="f5193-138">In this section, you configure and test Azure AD single sign-on with Namely based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f5193-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v konkrétně je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f5193-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Namely is tooa user in Azure AD.</span></span> <span data-ttu-id="f5193-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v konkrétně musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f5193-140">In other words, a link relationship between an Azure AD user and hello related user in Namely needs toobe established.</span></span>

<span data-ttu-id="f5193-141">V konkrétně, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="f5193-141">In Namely, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f5193-142">tooconfigure a testování Azure AD jednotné přihlašování s konkrétně, budete potřebovat toocomplete hello následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f5193-142">tooconfigure and test Azure AD single sign-on with Namely, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f5193-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f5193-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f5193-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f5193-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f5193-145">**[Vytváření konkrétně testovací uživatel](#creating-a-namely-test-user)**  -toohave protějšek Britta Simon v konkrétně to znamená propojené toohello reprezentace uživatele Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f5193-145">**[Creating a Namely test user](#creating-a-namely-test-user)** - toohave a counterpart of Britta Simon in Namely that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f5193-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f5193-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f5193-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f5193-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f5193-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f5193-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f5193-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v konkrétně aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5193-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Namely application.</span></span>

<span data-ttu-id="f5193-150">**tooconfigure Azure AD jednotné přihlašování s konkrétně, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f5193-150">**tooconfigure Azure AD single sign-on with Namely, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5193-151">V portálu Azure, na hello hello **konkrétně** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f5193-151">In hello Azure portal, on hello **Namely** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f5193-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f5193-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. <span data-ttu-id="f5193-155">Na hello **konkrétně domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f5193-155">On hello **Namely Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    <span data-ttu-id="f5193-157">a.</span><span class="sxs-lookup"><span data-stu-id="f5193-157">a.</span></span> <span data-ttu-id="f5193-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.namely.com`</span><span class="sxs-lookup"><span data-stu-id="f5193-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.namely.com`</span></span>

    <span data-ttu-id="f5193-159">b.</span><span class="sxs-lookup"><span data-stu-id="f5193-159">b.</span></span> <span data-ttu-id="f5193-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.namely.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="f5193-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.namely.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f5193-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f5193-161">These values are not real.</span></span> <span data-ttu-id="f5193-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="f5193-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f5193-163">Obraťte se na [tým podpory konkrétně klienta](https://www.namely.com/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f5193-163">Contact [Namely Client support team](https://www.namely.com/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="f5193-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f5193-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. <span data-ttu-id="f5193-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f5193-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f5193-168">Na hello **konkrétně konfigurace** klikněte na tlačítko **konfigurace konkrétně** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f5193-168">On hello **Namely Configuration** section, click **Configure Namely** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f5193-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f5193-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. <span data-ttu-id="f5193-171">Přihlašování tooyour v jiném okně prohlížeče, a to společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="f5193-171">In another browser window, sign on tooyour Namely company site as an administrator.</span></span>

8. <span data-ttu-id="f5193-172">V panelu nástrojů hello hello nahoře, klikněte na **společnosti**.</span><span class="sxs-lookup"><span data-stu-id="f5193-172">In hello toolbar on hello top, click **Company**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. <span data-ttu-id="f5193-174">Klikněte na tlačítko hello **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="f5193-174">Click hello **Settings** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. <span data-ttu-id="f5193-176">Klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="f5193-176">Click **SAML**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. <span data-ttu-id="f5193-178">Na hello **SAML nastavení** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5193-178">On hello **SAML Settings** page, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    <span data-ttu-id="f5193-180">a.</span><span class="sxs-lookup"><span data-stu-id="f5193-180">a.</span></span> <span data-ttu-id="f5193-181">Klikněte na tlačítko **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="f5193-181">Click **Enable SAML**.</span></span> 

    <span data-ttu-id="f5193-182">b.</span><span class="sxs-lookup"><span data-stu-id="f5193-182">b.</span></span> <span data-ttu-id="f5193-183">V hello **adresa url jednotné přihlašování zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f5193-183">In hello **Identity provider SSO url** textbox,  paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="f5193-184">c.</span><span class="sxs-lookup"><span data-stu-id="f5193-184">c.</span></span> <span data-ttu-id="f5193-185">Otevřete stažený certifikát v poznámkovém bloku hello kopírování obsahu a pak ji vložit do hello **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f5193-185">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **Identity provider certificate** textbox.</span></span>
     
    <span data-ttu-id="f5193-186">d.</span><span class="sxs-lookup"><span data-stu-id="f5193-186">d.</span></span> <span data-ttu-id="f5193-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f5193-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="f5193-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="f5193-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f5193-189">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="f5193-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f5193-190">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f5193-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f5193-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5193-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="f5193-192">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f5193-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f5193-194">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f5193-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5193-195">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f5193-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f5193-197">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f5193-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f5193-199">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="f5193-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f5193-201">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5193-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f5193-203">a.</span><span class="sxs-lookup"><span data-stu-id="f5193-203">a.</span></span> <span data-ttu-id="f5193-204">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f5193-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f5193-205">b.</span><span class="sxs-lookup"><span data-stu-id="f5193-205">b.</span></span> <span data-ttu-id="f5193-206">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f5193-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f5193-207">c.</span><span class="sxs-lookup"><span data-stu-id="f5193-207">c.</span></span> <span data-ttu-id="f5193-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f5193-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f5193-209">d.</span><span class="sxs-lookup"><span data-stu-id="f5193-209">d.</span></span> <span data-ttu-id="f5193-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f5193-210">Click **Create**.</span></span>
 
### <a name="creating-a-namely-test-user"></a><span data-ttu-id="f5193-211">Vytváření konkrétně testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="f5193-211">Creating a Namely test user</span></span>

<span data-ttu-id="f5193-212">Hello cílem této části je toocreate volal Britta Simon v konkrétně uživatele.</span><span class="sxs-lookup"><span data-stu-id="f5193-212">hello objective of this section is toocreate a user called Britta Simon in Namely.</span></span>

<span data-ttu-id="f5193-213">**toocreate uživatel volal Britta Simon v konkrétně, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f5193-213">**toocreate a user called Britta Simon in Namely, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5193-214">Přihlášení tooyour konkrétně společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="f5193-214">Sign-on tooyour Namely company site as an administrator.</span></span>

2. <span data-ttu-id="f5193-215">V panelu nástrojů hello hello nahoře, klikněte na **osoby**.</span><span class="sxs-lookup"><span data-stu-id="f5193-215">In hello toolbar on hello top, click **People**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. <span data-ttu-id="f5193-217">Klikněte na tlačítko hello **Directory** kartě.</span><span class="sxs-lookup"><span data-stu-id="f5193-217">Click hello **Directory** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. <span data-ttu-id="f5193-219">Klikněte na tlačítko **přidat nové osobě**.</span><span class="sxs-lookup"><span data-stu-id="f5193-219">Click **Add New Person**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. <span data-ttu-id="f5193-221">Na hello **přidat nové osobě** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f5193-221">On hello **Add New Person** dialog, perform hello following steps:</span></span>

    <span data-ttu-id="f5193-222">a.</span><span class="sxs-lookup"><span data-stu-id="f5193-222">a.</span></span> <span data-ttu-id="f5193-223">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="f5193-223">In hello **First name** textbox, type **Britta**.</span></span>

    <span data-ttu-id="f5193-224">b.</span><span class="sxs-lookup"><span data-stu-id="f5193-224">b.</span></span> <span data-ttu-id="f5193-225">V hello **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="f5193-225">In hello **Last name** textbox, type **Simon**.</span></span>

    <span data-ttu-id="f5193-226">c.</span><span class="sxs-lookup"><span data-stu-id="f5193-226">c.</span></span> <span data-ttu-id="f5193-227">V hello **e-mailu** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f5193-227">In hello **Email** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f5193-228">d.</span><span class="sxs-lookup"><span data-stu-id="f5193-228">d.</span></span> <span data-ttu-id="f5193-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f5193-229">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f5193-230">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f5193-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f5193-231">V této části povolíte tak, že udělíte přístup tooNamely toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f5193-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNamely.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f5193-233">**tooassign Britta Simon tooNamely, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f5193-233">**tooassign Britta Simon tooNamely, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5193-234">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f5193-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f5193-236">V seznamu aplikace hello vyberte **konkrétně**.</span><span class="sxs-lookup"><span data-stu-id="f5193-236">In hello applications list, select **Namely**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. <span data-ttu-id="f5193-238">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f5193-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f5193-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f5193-240">Click **Add** button.</span></span> <span data-ttu-id="f5193-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5193-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f5193-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f5193-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f5193-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5193-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f5193-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5193-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f5193-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f5193-246">Testing single sign-on</span></span>

<span data-ttu-id="f5193-247">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="f5193-247">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="f5193-248">Po kliknutí na tlačítko hello konkrétně dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour konkrétně aplikace</span><span class="sxs-lookup"><span data-stu-id="f5193-248">When you click hello Namely tile in hello Access Panel, you should get automatically signed-on tooyour Namely application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f5193-249">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f5193-249">Additional resources</span></span>

* [<span data-ttu-id="f5193-250">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f5193-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f5193-251">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f5193-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

