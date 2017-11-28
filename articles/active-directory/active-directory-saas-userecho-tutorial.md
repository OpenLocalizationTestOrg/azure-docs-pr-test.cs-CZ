---
title: 'Kurz: Azure Active Directory integrace s UserEcho | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a UserEcho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: efe4a94ed6e5d22d153565d4782850eac4dff37b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a><span data-ttu-id="18324-103">Kurz: Azure Active Directory integrace s UserEcho</span><span class="sxs-lookup"><span data-stu-id="18324-103">Tutorial: Azure Active Directory integration with UserEcho</span></span>

<span data-ttu-id="18324-104">V tomto kurzu zjistíte, jak toointegrate UserEcho s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="18324-104">In this tutorial, you learn how toointegrate UserEcho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="18324-105">Integrace UserEcho s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="18324-105">Integrating UserEcho with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="18324-106">Můžete řídit ve službě Azure AD, který má přístup tooUserEcho</span><span class="sxs-lookup"><span data-stu-id="18324-106">You can control in Azure AD who has access tooUserEcho</span></span>
- <span data-ttu-id="18324-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooUserEcho (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="18324-107">You can enable your users tooautomatically get signed-on tooUserEcho (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="18324-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="18324-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="18324-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="18324-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18324-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="18324-110">Prerequisites</span></span>

<span data-ttu-id="18324-111">Integrace služby Azure AD s UserEcho tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="18324-111">tooconfigure Azure AD integration with UserEcho, you need hello following items:</span></span>

- <span data-ttu-id="18324-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="18324-112">An Azure AD subscription</span></span>
- <span data-ttu-id="18324-113">UserEcho jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="18324-113">A UserEcho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="18324-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="18324-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="18324-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="18324-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="18324-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="18324-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="18324-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="18324-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="18324-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="18324-118">Scenario description</span></span>
<span data-ttu-id="18324-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="18324-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="18324-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="18324-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="18324-121">Přidání UserEcho z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="18324-121">Adding UserEcho from hello gallery</span></span>
2. <span data-ttu-id="18324-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="18324-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-userecho-from-hello-gallery"></a><span data-ttu-id="18324-123">Přidání UserEcho z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="18324-123">Adding UserEcho from hello gallery</span></span>
<span data-ttu-id="18324-124">tooconfigure hello integrace UserEcho do Azure AD, je nutné tooadd UserEcho hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="18324-124">tooconfigure hello integration of UserEcho into Azure AD, you need tooadd UserEcho from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="18324-125">**tooadd UserEcho z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="18324-125">**tooadd UserEcho from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="18324-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="18324-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="18324-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="18324-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="18324-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="18324-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="18324-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="18324-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="18324-133">Hello vyhledávacího pole zadejte **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="18324-133">In hello search box, type **UserEcho**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_search.png)

5. <span data-ttu-id="18324-135">Na panelu výsledků hello vyberte **UserEcho**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="18324-135">In hello results panel, select **UserEcho**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="18324-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="18324-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="18324-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UserEcho podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="18324-138">In this section, you configure and test Azure AD single sign-on with UserEcho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="18324-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v UserEcho je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="18324-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UserEcho is tooa user in Azure AD.</span></span> <span data-ttu-id="18324-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v UserEcho musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="18324-140">In other words, a link relationship between an Azure AD user and hello related user in UserEcho needs toobe established.</span></span>

<span data-ttu-id="18324-141">V UserEcho, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="18324-141">In UserEcho, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="18324-142">tooconfigure a testu Azure AD jednotné přihlašování s UserEcho, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="18324-142">tooconfigure and test Azure AD single sign-on with UserEcho, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="18324-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="18324-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="18324-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="18324-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="18324-145">**[Vytvoření zkušebního uživatele UserEcho](#creating-a-userecho-test-user)**  -toohave protějšek Britta Simon v UserEcho, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="18324-145">**[Creating a UserEcho test user](#creating-a-userecho-test-user)** - toohave a counterpart of Britta Simon in UserEcho that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="18324-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18324-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="18324-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="18324-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="18324-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="18324-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="18324-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci UserEcho.</span><span class="sxs-lookup"><span data-stu-id="18324-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UserEcho application.</span></span>

<span data-ttu-id="18324-150">**tooconfigure Azure AD jednotné přihlašování s UserEcho, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="18324-150">**tooconfigure Azure AD single sign-on with UserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="18324-151">V portálu Azure, na hello hello **UserEcho** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="18324-151">In hello Azure portal, on hello **UserEcho** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="18324-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18324-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_samlbase.png)

3. <span data-ttu-id="18324-155">Na hello **UserEcho domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="18324-155">On hello **UserEcho Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_url.png)

    <span data-ttu-id="18324-157">a.</span><span class="sxs-lookup"><span data-stu-id="18324-157">a.</span></span> <span data-ttu-id="18324-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.userecho.com/`</span><span class="sxs-lookup"><span data-stu-id="18324-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.userecho.com/`</span></span>

    <span data-ttu-id="18324-159">b.</span><span class="sxs-lookup"><span data-stu-id="18324-159">b.</span></span> <span data-ttu-id="18324-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.userecho.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="18324-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.userecho.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="18324-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="18324-161">These values are not real.</span></span> <span data-ttu-id="18324-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="18324-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="18324-163">Obraťte se na [tým podpory UserEcho klienta](https://feedback.userecho.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="18324-163">Contact [UserEcho Client support team](https://feedback.userecho.com/) tooget these values.</span></span> 

4. <span data-ttu-id="18324-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="18324-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_certificate.png) 

5. <span data-ttu-id="18324-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="18324-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="18324-168">Na hello **UserEcho konfigurace** klikněte na tlačítko **konfigurace UserEcho** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="18324-168">On hello **UserEcho Configuration** section, click **Configure UserEcho** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="18324-169">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="18324-169">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_configure.png) 

7. <span data-ttu-id="18324-171">V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti UserEcho tooyour.</span><span class="sxs-lookup"><span data-stu-id="18324-171">In another browser window, sign on tooyour UserEcho company site as an administrator.</span></span>

8. <span data-ttu-id="18324-172">Hello nástrojů v horní části hello, klikněte na tlačítko nabídky hello tooexpand název vaše uživatele a pak klikněte na **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="18324-172">In hello toolbar on hello top, click your user name tooexpand hello menu, and then click **Setup**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

9. <span data-ttu-id="18324-174">Klikněte na tlačítko **integrace**.</span><span class="sxs-lookup"><span data-stu-id="18324-174">Click **Integrations**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 

10. <span data-ttu-id="18324-176">Klikněte na tlačítko **webu**a potom klikněte na **jednotné přihlašování (typu SAML2)**.</span><span class="sxs-lookup"><span data-stu-id="18324-176">Click **Website**, and then click **Single sign-on (SAML2)**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 

11. <span data-ttu-id="18324-178">Na hello **jednotné přihlašování (SAML)** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="18324-178">On hello **Single sign-on (SAML)** page, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)
    
    <span data-ttu-id="18324-180">a.</span><span class="sxs-lookup"><span data-stu-id="18324-180">a.</span></span> <span data-ttu-id="18324-181">Jako **povoleno SAML**, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="18324-181">As **SAML-enabled**, select **Yes**.</span></span>
    
    <span data-ttu-id="18324-182">b.</span><span class="sxs-lookup"><span data-stu-id="18324-182">b.</span></span> <span data-ttu-id="18324-183">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **URL jednotné přihlašování SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="18324-183">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **SAML SSO URL** textbox.</span></span>
    
    <span data-ttu-id="18324-184">c.</span><span class="sxs-lookup"><span data-stu-id="18324-184">c.</span></span> <span data-ttu-id="18324-185">Vložení **Sign-Out URL**, který jste zkopírovali z hello portálu Azure do hello **adresy URL vzdálené logoout** textové pole.</span><span class="sxs-lookup"><span data-stu-id="18324-185">Paste **Sign-Out URL**, which you have copied from hello Azure portal into hello **Remote logoout URL** textbox.</span></span>
    
    <span data-ttu-id="18324-186">d.</span><span class="sxs-lookup"><span data-stu-id="18324-186">d.</span></span> <span data-ttu-id="18324-187">Otevřete stažený certifikát v poznámkovém bloku hello kopírování obsahu a pak ji vložit do hello **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="18324-187">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **X.509 Certificate** textbox.</span></span>
    
    <span data-ttu-id="18324-188">e.</span><span class="sxs-lookup"><span data-stu-id="18324-188">e.</span></span> <span data-ttu-id="18324-189">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="18324-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="18324-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="18324-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="18324-191">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="18324-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="18324-192">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="18324-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="18324-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="18324-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="18324-194">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="18324-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="18324-196">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="18324-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="18324-197">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="18324-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="18324-199">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="18324-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="18324-201">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="18324-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="18324-203">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="18324-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="18324-205">a.</span><span class="sxs-lookup"><span data-stu-id="18324-205">a.</span></span> <span data-ttu-id="18324-206">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="18324-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="18324-207">b.</span><span class="sxs-lookup"><span data-stu-id="18324-207">b.</span></span> <span data-ttu-id="18324-208">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="18324-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="18324-209">c.</span><span class="sxs-lookup"><span data-stu-id="18324-209">c.</span></span> <span data-ttu-id="18324-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="18324-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="18324-211">d.</span><span class="sxs-lookup"><span data-stu-id="18324-211">d.</span></span> <span data-ttu-id="18324-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="18324-212">Click **Create**.</span></span>
 
### <a name="creating-a-userecho-test-user"></a><span data-ttu-id="18324-213">Vytvoření zkušebního uživatele UserEcho</span><span class="sxs-lookup"><span data-stu-id="18324-213">Creating a UserEcho test user</span></span>

<span data-ttu-id="18324-214">Hello cílem této části je toocreate volal Britta Simon v UserEcho uživatele.</span><span class="sxs-lookup"><span data-stu-id="18324-214">hello objective of this section is toocreate a user called Britta Simon in UserEcho.</span></span>

<span data-ttu-id="18324-215">**toocreate uživatel volal Britta Simon v UserEcho, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="18324-215">**toocreate a user called Britta Simon in UserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="18324-216">Web společnosti UserEcho tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="18324-216">Sign-on tooyour UserEcho company site as an administrator.</span></span>

2. <span data-ttu-id="18324-217">Hello nástrojů v horní části hello, klikněte na tlačítko nabídky hello tooexpand název vaše uživatele a pak klikněte na **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="18324-217">In hello toolbar on hello top, click your user name tooexpand hello menu, and then click **Setup**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

3. <span data-ttu-id="18324-219">Klikněte na tlačítko **uživatelé**, tooexpand hello **uživatelé** části.</span><span class="sxs-lookup"><span data-stu-id="18324-219">Click **Users**, tooexpand hello **Users** section.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

4. <span data-ttu-id="18324-221">Klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="18324-221">Click **Users**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

5. <span data-ttu-id="18324-223">Klikněte na tlačítko **pozvat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="18324-223">Click **Invite a new user**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)

6. <span data-ttu-id="18324-225">Na hello **pozvat nového uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="18324-225">On hello **Invite a new user** dialog, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    <span data-ttu-id="18324-227">a.</span><span class="sxs-lookup"><span data-stu-id="18324-227">a.</span></span> <span data-ttu-id="18324-228">V hello **název** textovému poli, název typu hello uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="18324-228">In hello **Name** textbox, type name of hello user like Britta Simon.</span></span>
    
    <span data-ttu-id="18324-229">b.</span><span class="sxs-lookup"><span data-stu-id="18324-229">b.</span></span>  <span data-ttu-id="18324-230">V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="18324-230">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="18324-231">c.</span><span class="sxs-lookup"><span data-stu-id="18324-231">c.</span></span> <span data-ttu-id="18324-232">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="18324-232">Click **Invite**.</span></span>

<span data-ttu-id="18324-233">Pozvánka se odesílají tooBritta, která umožňuje jeho toostart pomocí UserEcho.</span><span class="sxs-lookup"><span data-stu-id="18324-233">An invitation is sent tooBritta, which enables her toostart using UserEcho.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="18324-234">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="18324-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="18324-235">V této části povolíte tak, že udělíte přístup tooUserEcho toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18324-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUserEcho.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="18324-237">**tooassign Britta Simon tooUserEcho, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="18324-237">**tooassign Britta Simon tooUserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="18324-238">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="18324-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="18324-240">V seznamu aplikace hello vyberte **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="18324-240">In hello applications list, select **UserEcho**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_app.png) 

3. <span data-ttu-id="18324-242">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="18324-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="18324-244">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="18324-244">Click **Add** button.</span></span> <span data-ttu-id="18324-245">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18324-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="18324-247">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="18324-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="18324-248">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18324-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="18324-249">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="18324-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="18324-250">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="18324-250">Testing single sign-on</span></span>

<span data-ttu-id="18324-251">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="18324-251">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="18324-252">Když kliknete na dlaždici UserEcho hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour UserEcho aplikace.</span><span class="sxs-lookup"><span data-stu-id="18324-252">When you click hello UserEcho tile in hello Access Panel, you should get automatically signed-on tooyour UserEcho application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="18324-253">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="18324-253">Additional resources</span></span>

* [<span data-ttu-id="18324-254">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="18324-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="18324-255">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="18324-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png

