---
title: 'Kurz: Azure Active Directory integrace s RunMyProcess | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a RunMyProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f02acda015aeb8d131d8e3ef88bf50c4e8e94750
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a><span data-ttu-id="d4610-103">Kurz: Azure Active Directory integrace s RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="d4610-103">Tutorial: Azure Active Directory integration with RunMyProcess</span></span>

<span data-ttu-id="d4610-104">V tomto kurzu zjistíte, jak toointegrate RunMyProcess s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d4610-104">In this tutorial, you learn how toointegrate RunMyProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d4610-105">Integrace RunMyProcess s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d4610-105">Integrating RunMyProcess with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d4610-106">Můžete řídit ve službě Azure AD, který má přístup tooRunMyProcess</span><span class="sxs-lookup"><span data-stu-id="d4610-106">You can control in Azure AD who has access tooRunMyProcess</span></span>
- <span data-ttu-id="d4610-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRunMyProcess (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d4610-107">You can enable your users tooautomatically get signed-on tooRunMyProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d4610-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d4610-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d4610-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d4610-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4610-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d4610-110">Prerequisites</span></span>

<span data-ttu-id="d4610-111">Integrace služby Azure AD s RunMyProcess tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d4610-111">tooconfigure Azure AD integration with RunMyProcess, you need hello following items:</span></span>

- <span data-ttu-id="d4610-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d4610-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d4610-113">RunMyProcess jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d4610-113">A RunMyProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d4610-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d4610-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d4610-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d4610-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d4610-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d4610-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d4610-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební:[nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d4610-117">If you don't have an Azure AD trial environment, you can get a one-month trial here:[Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d4610-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d4610-118">Scenario description</span></span>
<span data-ttu-id="d4610-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d4610-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d4610-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d4610-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d4610-121">Přidání RunMyProcess z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d4610-121">Adding RunMyProcess from hello gallery</span></span>
2. <span data-ttu-id="d4610-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d4610-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-runmyprocess-from-hello-gallery"></a><span data-ttu-id="d4610-123">Přidání RunMyProcess z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d4610-123">Adding RunMyProcess from hello gallery</span></span>
<span data-ttu-id="d4610-124">tooconfigure hello integrace RunMyProcess do Azure AD, je nutné tooadd RunMyProcess hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d4610-124">tooconfigure hello integration of RunMyProcess into Azure AD, you need tooadd RunMyProcess from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d4610-125">**tooadd RunMyProcess z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d4610-125">**tooadd RunMyProcess from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4610-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d4610-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d4610-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d4610-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d4610-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d4610-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d4610-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d4610-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d4610-133">Hello vyhledávacího pole zadejte **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="d4610-133">In hello search box, type **RunMyProcess**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_search.png)

5. <span data-ttu-id="d4610-135">Na panelu výsledků hello vyberte **RunMyProcess**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d4610-135">In hello results panel, select **RunMyProcess**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d4610-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d4610-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d4610-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s RunMyProcess podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d4610-138">In this section, you configure and test Azure AD single sign-on with RunMyProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d4610-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v RunMyProcess je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d4610-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RunMyProcess is tooa user in Azure AD.</span></span> <span data-ttu-id="d4610-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v RunMyProcess musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="d4610-140">In other words, a link relationship between an Azure AD user and hello related user in RunMyProcess needs toobe established.</span></span>

<span data-ttu-id="d4610-141">V RunMyProcess, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="d4610-141">In RunMyProcess, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d4610-142">tooconfigure a testu Azure AD jednotné přihlašování s RunMyProcess, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="d4610-142">tooconfigure and test Azure AD single sign-on with RunMyProcess, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d4610-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="d4610-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d4610-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d4610-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d4610-145">**[Vytvoření zkušebního uživatele RunMyProcess](#creating-a-runmyprocess-test-user)**  -toohave protějšek Britta Simon v RunMyProcess, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="d4610-145">**[Creating a RunMyProcess test user](#creating-a-runmyprocess-test-user)** - toohave a counterpart of Britta Simon in RunMyProcess that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d4610-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d4610-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d4610-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="d4610-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d4610-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d4610-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d4610-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="d4610-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RunMyProcess application.</span></span>

<span data-ttu-id="d4610-150">**tooconfigure Azure AD jednotné přihlašování s RunMyProcess, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d4610-150">**tooconfigure Azure AD single sign-on with RunMyProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4610-151">V portálu Azure, na hello hello **RunMyProcess** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d4610-151">In hello Azure portal, on hello **RunMyProcess** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d4610-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d4610-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

3. <span data-ttu-id="d4610-155">Na hello **RunMyProcess domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d4610-155">On hello **RunMyProcess Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    <span data-ttu-id="d4610-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://live.runmyprocess.com/live/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="d4610-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://live.runmyprocess.com/live/<tenant id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d4610-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="d4610-158">hello value is not real.</span></span> <span data-ttu-id="d4610-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d4610-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="d4610-160">Obraťte se na [tým podpory RunMyProcess klienta](mailto:support@runmyprocess.com) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d4610-160">Contact [RunMyProcess Client support team](mailto:support@runmyprocess.com) tooget hello value.</span></span> 

4. <span data-ttu-id="d4610-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d4610-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

5. <span data-ttu-id="d4610-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d4610-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d4610-165">Na hello **RunMyProcess konfigurace** klikněte na tlačítko **konfigurace RunMyProcess** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d4610-165">On hello **RunMyProcess Configuration** section, click **Configure RunMyProcess** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d4610-166">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d4610-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

7. <span data-ttu-id="d4610-168">V okně prohlížeče jiný web, klienta RunMyProcess tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="d4610-168">In a different web browser window, sign-on tooyour RunMyProcess tenant as an administrator.</span></span>

8. <span data-ttu-id="d4610-169">V levém navigačním panelu klikněte na **účet** a vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="d4610-169">In left navigation panel, click **Account** and select **Configuration**.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

9. <span data-ttu-id="d4610-171">Přejděte příliš**metodu ověřování** části a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4610-171">Go too**Authentication method** section and perform below steps:</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    <span data-ttu-id="d4610-173">a.</span><span class="sxs-lookup"><span data-stu-id="d4610-173">a.</span></span> <span data-ttu-id="d4610-174">Jako **metoda**, vyberte **přihlášení SSO se Samlv2**.</span><span class="sxs-lookup"><span data-stu-id="d4610-174">As **Method**, select **SSO with Samlv2**.</span></span> 

    <span data-ttu-id="d4610-175">b.</span><span class="sxs-lookup"><span data-stu-id="d4610-175">b.</span></span> <span data-ttu-id="d4610-176">V hello **jednotného přihlašování k přesměrování** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4610-176">In hello **SSO redirect** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d4610-177">c.</span><span class="sxs-lookup"><span data-stu-id="d4610-177">c.</span></span> <span data-ttu-id="d4610-178">V hello **přesměrování odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4610-178">In hello **Logout redirect** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="d4610-179">d.</span><span class="sxs-lookup"><span data-stu-id="d4610-179">d.</span></span> <span data-ttu-id="d4610-180">V hello **formát Id názvu** textovému poli, hodnota typu hello **formát názvu identifikátor** jako **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="d4610-180">In hello **Name Id Format** textbox, type hello value of **Name Identifier Format** as **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="d4610-181">e.</span><span class="sxs-lookup"><span data-stu-id="d4610-181">e.</span></span> <span data-ttu-id="d4610-182">Zkopírujte obsah hello hello stažený certifikát souboru a pak ji vložit do hello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d4610-182">Copy hello content of hello downloaded certificate file and then paste it into hello **Certificate** textbox.</span></span> 
 
    <span data-ttu-id="d4610-183">f.</span><span class="sxs-lookup"><span data-stu-id="d4610-183">f.</span></span> <span data-ttu-id="d4610-184">Klikněte na tlačítko **Uložit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d4610-184">Click **Save** icon.</span></span>

> [!TIP]
> <span data-ttu-id="d4610-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="d4610-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d4610-186">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="d4610-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d4610-187">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d4610-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d4610-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d4610-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="d4610-189">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="d4610-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d4610-191">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d4610-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4610-192">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d4610-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d4610-194">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d4610-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d4610-196">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="d4610-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d4610-198">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4610-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d4610-200">a.</span><span class="sxs-lookup"><span data-stu-id="d4610-200">a.</span></span> <span data-ttu-id="d4610-201">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d4610-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d4610-202">b.</span><span class="sxs-lookup"><span data-stu-id="d4610-202">b.</span></span> <span data-ttu-id="d4610-203">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d4610-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d4610-204">c.</span><span class="sxs-lookup"><span data-stu-id="d4610-204">c.</span></span> <span data-ttu-id="d4610-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d4610-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d4610-206">d.</span><span class="sxs-lookup"><span data-stu-id="d4610-206">d.</span></span> <span data-ttu-id="d4610-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d4610-207">Click **Create**.</span></span>
 
### <a name="creating-a-runmyprocess-test-user"></a><span data-ttu-id="d4610-208">Vytvoření zkušebního uživatele RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="d4610-208">Creating a RunMyProcess test user</span></span>

<span data-ttu-id="d4610-209">V pořadí tooenable Azure AD Uživatelé toolog v tooRunMyProcess musí být zřízená do RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="d4610-209">In order tooenable Azure AD users toolog in tooRunMyProcess, they must be provisioned into RunMyProcess.</span></span> <span data-ttu-id="d4610-210">V případě hello RunMyProcess zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d4610-210">In hello case of RunMyProcess, provisioning is a manual task.</span></span>

<span data-ttu-id="d4610-211">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d4610-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4610-212">Přihlaste se tooyour RunMyProcess společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="d4610-212">Log in tooyour RunMyProcess company site as an administrator.</span></span>

2. <span data-ttu-id="d4610-213">Klikněte na tlačítko **účet** a vyberte **uživatelé** v levém navigačním panelu klikněte **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="d4610-213">Click **Account** and select **Users** in left navigation panel, then click **New User**.</span></span>
   
    <span data-ttu-id="d4610-214">![Nový uživatel](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="d4610-214">![New User](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "New User")</span></span>

3. <span data-ttu-id="d4610-215">V hello **uživatelská nastavení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d4610-215">In hello **User Settings** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d4610-216">![Profil](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "profilu")</span><span class="sxs-lookup"><span data-stu-id="d4610-216">![Profile](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profile")</span></span> 
  
    <span data-ttu-id="d4610-217">a.</span><span class="sxs-lookup"><span data-stu-id="d4610-217">a.</span></span> <span data-ttu-id="d4610-218">Typ hello **název** a **e-mailu** platný Azure AD účtu chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="d4610-218">Type hello **Name** and **E-mail** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span> 

    <span data-ttu-id="d4610-219">b.</span><span class="sxs-lookup"><span data-stu-id="d4610-219">b.</span></span> <span data-ttu-id="d4610-220">Vyberte **IDE jazyk**, **jazyk**, a **profil**.</span><span class="sxs-lookup"><span data-stu-id="d4610-220">Select an **IDE language**, **Language**, and **Profile**.</span></span> 

    <span data-ttu-id="d4610-221">c.</span><span class="sxs-lookup"><span data-stu-id="d4610-221">c.</span></span> <span data-ttu-id="d4610-222">Vyberte **odeslat účet vytvoření e-mailu toome**.</span><span class="sxs-lookup"><span data-stu-id="d4610-222">Select **Send account creation e-mail toome**.</span></span> 

    <span data-ttu-id="d4610-223">d.</span><span class="sxs-lookup"><span data-stu-id="d4610-223">d.</span></span> <span data-ttu-id="d4610-224">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d4610-224">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="d4610-225">Můžete použít všechny ostatní RunMyProcess uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované RunMyProcess tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="d4610-225">You can use any other RunMyProcess user account creation tools or APIs provided by RunMyProcess tooprovision Azure Active Directory user accounts.</span></span> 
    > 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d4610-226">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d4610-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d4610-227">V této části povolíte tak, že udělíte přístup tooRunMyProcess toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d4610-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRunMyProcess.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d4610-229">**tooassign Britta Simon tooRunMyProcess, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d4610-229">**tooassign Britta Simon tooRunMyProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4610-230">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d4610-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d4610-232">V seznamu aplikace hello vyberte **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="d4610-232">In hello applications list, select **RunMyProcess**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

3. <span data-ttu-id="d4610-234">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d4610-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d4610-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d4610-236">Click **Add** button.</span></span> <span data-ttu-id="d4610-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d4610-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d4610-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="d4610-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d4610-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d4610-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d4610-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d4610-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d4610-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d4610-242">Testing single sign-on</span></span>

<span data-ttu-id="d4610-243">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="d4610-243">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="d4610-244">Po kliknutí na tlačítko hello RunMyProcess dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour RunMyProcess aplikace.</span><span class="sxs-lookup"><span data-stu-id="d4610-244">When you click hello RunMyProcess tile in hello Access Panel, you should get automatically signed-on tooyour RunMyProcess application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4610-245">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d4610-245">Additional resources</span></span>

* [<span data-ttu-id="d4610-246">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4610-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d4610-247">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d4610-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png

