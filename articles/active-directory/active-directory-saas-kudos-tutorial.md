---
title: 'Kurz: Azure Active Directory integrace s Kudos | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Kudos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: c1b481463574461f9948db2b83843fefa5d74e99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a><span data-ttu-id="ff39e-103">Kurz: Azure Active Directory integrace s Kudos</span><span class="sxs-lookup"><span data-stu-id="ff39e-103">Tutorial: Azure Active Directory integration with Kudos</span></span>

<span data-ttu-id="ff39e-104">V tomto kurzu zjistíte, jak toointegrate Kudos s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ff39e-104">In this tutorial, you learn how toointegrate Kudos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ff39e-105">Integrace Kudos s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ff39e-105">Integrating Kudos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ff39e-106">Můžete řídit ve službě Azure AD, který má přístup tooKudos</span><span class="sxs-lookup"><span data-stu-id="ff39e-106">You can control in Azure AD who has access tooKudos</span></span>
- <span data-ttu-id="ff39e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooKudos (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff39e-107">You can enable your users tooautomatically get signed-on tooKudos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ff39e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ff39e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ff39e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ff39e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff39e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ff39e-110">Prerequisites</span></span>

<span data-ttu-id="ff39e-111">Integrace služby Azure AD s Kudos tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ff39e-111">tooconfigure Azure AD integration with Kudos, you need hello following items:</span></span>

- <span data-ttu-id="ff39e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff39e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ff39e-113">Kudos jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ff39e-113">A Kudos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ff39e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff39e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ff39e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ff39e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ff39e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ff39e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ff39e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff39e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ff39e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ff39e-118">Scenario description</span></span>
<span data-ttu-id="ff39e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff39e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ff39e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ff39e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ff39e-121">Přidání Kudos z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ff39e-121">Adding Kudos from hello gallery</span></span>
2. <span data-ttu-id="ff39e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff39e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kudos-from-hello-gallery"></a><span data-ttu-id="ff39e-123">Přidání Kudos z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ff39e-123">Adding Kudos from hello gallery</span></span>
<span data-ttu-id="ff39e-124">tooconfigure hello integrace Kudos do Azure AD, je nutné tooadd Kudos hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ff39e-124">tooconfigure hello integration of Kudos into Azure AD, you need tooadd Kudos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ff39e-125">**tooadd Kudos z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff39e-125">**tooadd Kudos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff39e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff39e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ff39e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ff39e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ff39e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff39e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ff39e-133">Hello vyhledávacího pole zadejte **Kudos**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-133">In hello search box, type **Kudos**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_search.png)

5. <span data-ttu-id="ff39e-135">Na panelu výsledků hello vyberte **Kudos**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff39e-135">In hello results panel, select **Kudos**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ff39e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff39e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ff39e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kudos podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ff39e-138">In this section, you configure and test Azure AD single sign-on with Kudos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ff39e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Kudos je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff39e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kudos is tooa user in Azure AD.</span></span> <span data-ttu-id="ff39e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Kudos musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ff39e-140">In other words, a link relationship between an Azure AD user and hello related user in Kudos needs toobe established.</span></span>

<span data-ttu-id="ff39e-141">V Kudos, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="ff39e-141">In Kudos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ff39e-142">tooconfigure a testu Azure AD jednotné přihlašování s Kudos, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ff39e-142">tooconfigure and test Azure AD single sign-on with Kudos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ff39e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ff39e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ff39e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff39e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ff39e-145">**[Vytvoření zkušebního uživatele Kudos](#creating-a-kudos-test-user)**  -toohave protějšek Britta Simon v Kudos, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff39e-145">**[Creating a Kudos test user](#creating-a-kudos-test-user)** - toohave a counterpart of Britta Simon in Kudos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ff39e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff39e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ff39e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ff39e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ff39e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff39e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ff39e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Kudos.</span><span class="sxs-lookup"><span data-stu-id="ff39e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kudos application.</span></span>

<span data-ttu-id="ff39e-150">**tooconfigure Azure AD jednotné přihlašování s Kudos, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff39e-150">**tooconfigure Azure AD single sign-on with Kudos, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff39e-151">V portálu Azure, na hello hello **Kudos** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-151">In hello Azure portal, on hello **Kudos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ff39e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff39e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_samlbase.png)

3. <span data-ttu-id="ff39e-155">Na hello **Kudos domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ff39e-155">On hello **Kudos Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_url.png)

    <span data-ttu-id="ff39e-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company>.kudosnow.com`</span><span class="sxs-lookup"><span data-stu-id="ff39e-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.kudosnow.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="ff39e-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="ff39e-158">This value is not real.</span></span> <span data-ttu-id="ff39e-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff39e-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="ff39e-160">Obraťte se na [tým podpory Kudos klienta](http://success.kudosnow.com/home) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ff39e-160">Contact [Kudos Client support team](http://success.kudosnow.com/home) tooget this value.</span></span> 
 
4. <span data-ttu-id="ff39e-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ff39e-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_certificate.png) 

5. <span data-ttu-id="ff39e-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff39e-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ff39e-165">Na hello **Kudos konfigurace** klikněte na tlačítko **konfigurace Kudos** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ff39e-165">On hello **Kudos Configuration** section, click **Configure Kudos** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ff39e-166">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ff39e-166">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_configure.png) 

7. <span data-ttu-id="ff39e-168">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Kudos.</span><span class="sxs-lookup"><span data-stu-id="ff39e-168">In a different web browser window, log into your Kudos company site as an administrator.</span></span>

8. <span data-ttu-id="ff39e-169">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-169">In hello menu on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="ff39e-170">![Nastavení](./media/active-directory-saas-kudos-tutorial/ic787806.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="ff39e-170">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

9. <span data-ttu-id="ff39e-171">Klikněte na tlačítko **integrace \> jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-171">Click **Integrations \> SSO**.</span></span>

10. <span data-ttu-id="ff39e-172">V hello **jednotného přihlašování k** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ff39e-172">In hello **SSO** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="ff39e-173">![JEDNOTNÉ PŘIHLAŠOVÁNÍ](./media/active-directory-saas-kudos-tutorial/ic787807.png "JEDNOTNÉHO PŘIHLAŠOVÁNÍ")</span><span class="sxs-lookup"><span data-stu-id="ff39e-173">![SSO](./media/active-directory-saas-kudos-tutorial/ic787807.png "SSO")</span></span>
   
    <span data-ttu-id="ff39e-174">a.</span><span class="sxs-lookup"><span data-stu-id="ff39e-174">a.</span></span> <span data-ttu-id="ff39e-175">V **přihlásit na adrese URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ff39e-175">In **Sign on URL** textbox, paste hello value of  **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="ff39e-176">b.</span><span class="sxs-lookup"><span data-stu-id="ff39e-176">b.</span></span> <span data-ttu-id="ff39e-177">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textbox</span><span class="sxs-lookup"><span data-stu-id="ff39e-177">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 certificate** textbox</span></span>
   
    <span data-ttu-id="ff39e-178">c.</span><span class="sxs-lookup"><span data-stu-id="ff39e-178">c.</span></span> <span data-ttu-id="ff39e-179">V **odhlášení tooURL**, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ff39e-179">In **Logout tooURL**, paste hello value of  **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="ff39e-180">d.</span><span class="sxs-lookup"><span data-stu-id="ff39e-180">d.</span></span> <span data-ttu-id="ff39e-181">V hello **vaše adresa URL Kudos** textovému poli, zadejte název vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="ff39e-181">In hello **Your Kudos URL** textbox, type your company name.</span></span>
   
    <span data-ttu-id="ff39e-182">e.</span><span class="sxs-lookup"><span data-stu-id="ff39e-182">e.</span></span> <span data-ttu-id="ff39e-183">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="ff39e-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ff39e-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ff39e-185">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ff39e-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ff39e-186">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ff39e-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ff39e-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff39e-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="ff39e-188">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ff39e-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ff39e-190">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff39e-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff39e-191">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff39e-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ff39e-193">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ff39e-195">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="ff39e-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ff39e-197">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff39e-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ff39e-199">a.</span><span class="sxs-lookup"><span data-stu-id="ff39e-199">a.</span></span> <span data-ttu-id="ff39e-200">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ff39e-201">b.</span><span class="sxs-lookup"><span data-stu-id="ff39e-201">b.</span></span> <span data-ttu-id="ff39e-202">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ff39e-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ff39e-203">c.</span><span class="sxs-lookup"><span data-stu-id="ff39e-203">c.</span></span> <span data-ttu-id="ff39e-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ff39e-205">d.</span><span class="sxs-lookup"><span data-stu-id="ff39e-205">d.</span></span> <span data-ttu-id="ff39e-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-206">Click **Create**.</span></span>
 
### <a name="creating-a-kudos-test-user"></a><span data-ttu-id="ff39e-207">Vytvoření zkušebního uživatele Kudos</span><span class="sxs-lookup"><span data-stu-id="ff39e-207">Creating a Kudos test user</span></span>

<span data-ttu-id="ff39e-208">V pořadí tooenable Azure AD Uživatelé toolog do Kudos musí být zřízená do Kudos.</span><span class="sxs-lookup"><span data-stu-id="ff39e-208">In order tooenable Azure AD users toolog into Kudos, they must be provisioned into Kudos.</span></span> 

<span data-ttu-id="ff39e-209">V případě hello Kudos zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ff39e-209">In hello case of Kudos, provisioning is a manual task.</span></span>

<span data-ttu-id="ff39e-210">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff39e-210">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff39e-211">Přihlaste se tooyour **Kudos** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="ff39e-211">Log in tooyour **Kudos** company site as administrator.</span></span>

2. <span data-ttu-id="ff39e-212">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-212">In hello menu on hello top, click **Settings**.</span></span>
   
   <span data-ttu-id="ff39e-213">![Nastavení](./media/active-directory-saas-kudos-tutorial/ic787806.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="ff39e-213">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

3. <span data-ttu-id="ff39e-214">Klikněte na tlačítko **uživatele správce**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-214">Click **User Admin**.</span></span>

4. <span data-ttu-id="ff39e-215">Klikněte na tlačítko hello **uživatelé** a pak klikněte **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-215">Click hello **Users** tab, and then click **Add a User**.</span></span>
   
   <span data-ttu-id="ff39e-216">![Uživatel správce](./media/active-directory-saas-kudos-tutorial/ic787809.png "správce uživatele")</span><span class="sxs-lookup"><span data-stu-id="ff39e-216">![User Admin](./media/active-directory-saas-kudos-tutorial/ic787809.png "User Admin")</span></span>

5. <span data-ttu-id="ff39e-217">V hello **přidat uživatele** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ff39e-217">In hello **Add a User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="ff39e-218">![Přidat uživatele](./media/active-directory-saas-kudos-tutorial/ic787810.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="ff39e-218">![Add a User](./media/active-directory-saas-kudos-tutorial/ic787810.png "Add a User")</span></span>
   
    <span data-ttu-id="ff39e-219">a.</span><span class="sxs-lookup"><span data-stu-id="ff39e-219">a.</span></span> <span data-ttu-id="ff39e-220">Typ hello **křestní jméno**, **příjmení**, **e-mailu** a další podrobnosti o platný účet služby Azure Active Directory do hello chcete tooprovision související textových polí.</span><span class="sxs-lookup"><span data-stu-id="ff39e-220">Type hello **First Name**, **Last Name**, **Email** and other details of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="ff39e-221">b.</span><span class="sxs-lookup"><span data-stu-id="ff39e-221">b.</span></span> <span data-ttu-id="ff39e-222">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-222">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="ff39e-223">Můžete použít všechny ostatní Kudos uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Kudos tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="ff39e-223">You can use any other Kudos user account creation tools or APIs provided by Kudos tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ff39e-224">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ff39e-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ff39e-225">V této části povolíte tak, že udělíte přístup tooKudos toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff39e-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKudos.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ff39e-227">**tooassign Britta Simon tooKudos, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff39e-227">**tooassign Britta Simon tooKudos, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff39e-228">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ff39e-230">V seznamu aplikace hello vyberte **Kudos**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-230">In hello applications list, select **Kudos**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_app.png) 

3. <span data-ttu-id="ff39e-232">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ff39e-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ff39e-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff39e-234">Click **Add** button.</span></span> <span data-ttu-id="ff39e-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff39e-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ff39e-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ff39e-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ff39e-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff39e-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ff39e-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff39e-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ff39e-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff39e-240">Testing single sign-on</span></span>

<span data-ttu-id="ff39e-241">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ff39e-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ff39e-242">Když kliknete na dlaždici Kudos hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Kudos aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff39e-242">When you click hello Kudos tile in hello Access Panel, you should get automatically signed-on tooyour Kudos application.</span></span> <span data-ttu-id="ff39e-243">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ff39e-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff39e-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ff39e-244">Additional resources</span></span>

* [<span data-ttu-id="ff39e-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff39e-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ff39e-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ff39e-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_203.png

