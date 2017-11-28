---
title: "Kurz: Azure Active Directory integrace s TigerText zabezpečení Messenger | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TigerText zabezpečení Messenger."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: a3d7bb9598658c75c567c15751740d885fe4fc27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a><span data-ttu-id="99fc8-103">Kurz: Azure Active Directory integrace s TigerText zabezpečení Messenger</span><span class="sxs-lookup"><span data-stu-id="99fc8-103">Tutorial: Azure Active Directory integration with TigerText Secure Messenger</span></span>

<span data-ttu-id="99fc8-104">V tomto kurzu zjistíte, jak toointegrate TigerText zabezpečeného Kurýrní službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="99fc8-104">In this tutorial, you learn how toointegrate TigerText Secure Messenger with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="99fc8-105">Integrace TigerText zabezpečení Messenger s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="99fc8-105">Integrating TigerText Secure Messenger with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="99fc8-106">Můžete řídit ve službě Azure AD, který má přístup tooTigerText zabezpečené Messenger</span><span class="sxs-lookup"><span data-stu-id="99fc8-106">You can control in Azure AD who has access tooTigerText Secure Messenger</span></span>
- <span data-ttu-id="99fc8-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTigerText zabezpečení Messenger (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="99fc8-107">You can enable your users tooautomatically get signed-on tooTigerText Secure Messenger (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="99fc8-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="99fc8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="99fc8-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="99fc8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99fc8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="99fc8-110">Prerequisites</span></span>

<span data-ttu-id="99fc8-111">tooconfigure integrace Azure AD s TigerText zabezpečení Messenger, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="99fc8-111">tooconfigure Azure AD integration with TigerText Secure Messenger, you need hello following items:</span></span>

- <span data-ttu-id="99fc8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="99fc8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="99fc8-113">Zabezpečení Messenger TigerText jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="99fc8-113">A TigerText Secure Messenger single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="99fc8-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="99fc8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="99fc8-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="99fc8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="99fc8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="99fc8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="99fc8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="99fc8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="99fc8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="99fc8-118">Scenario description</span></span>
<span data-ttu-id="99fc8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="99fc8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="99fc8-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="99fc8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="99fc8-121">Přidat TigerText zabezpečení Messenger z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="99fc8-121">Add TigerText Secure Messenger from hello gallery</span></span>
2. <span data-ttu-id="99fc8-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="99fc8-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tigertext-secure-messenger-from-hello-gallery"></a><span data-ttu-id="99fc8-123">Přidat TigerText zabezpečení Messenger z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="99fc8-123">Add TigerText Secure Messenger from hello gallery</span></span>
<span data-ttu-id="99fc8-124">tooconfigure hello integrace TigerText zabezpečení Messenger do Azure AD, je nutné tooadd TigerText zabezpečení Messenger hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="99fc8-124">tooconfigure hello integration of TigerText Secure Messenger into Azure AD, you need tooadd TigerText Secure Messenger from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="99fc8-125">**tooadd TigerText zabezpečení Messenger z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="99fc8-125">**tooadd TigerText Secure Messenger from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="99fc8-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="99fc8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="99fc8-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="99fc8-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="99fc8-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="99fc8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="99fc8-133">Hello vyhledávacího pole zadejte **TigerText zabezpečení Messenger**, vyberte **TigerText zabezpečení Messenger** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="99fc8-133">In hello search box, type **TigerText Secure Messenger**, select **TigerText Secure Messenger** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Přidat z Galerie](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="99fc8-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="99fc8-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="99fc8-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TigerText zabezpečení Messenger podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="99fc8-136">In this section, you configure and test Azure AD single sign-on with TigerText Secure Messenger based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="99fc8-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello protějšku uživatele ve službě Messenger zabezpečení TigerText je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99fc8-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TigerText Secure Messenger is tooa user in Azure AD.</span></span> <span data-ttu-id="99fc8-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v programu Messenger zabezpečení TigerText musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="99fc8-138">In other words, a link relationship between an Azure AD user and hello related user in TigerText Secure Messenger needs toobe established.</span></span>

<span data-ttu-id="99fc8-139">V zabezpečené Messenger TigerText přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="99fc8-139">In TigerText Secure Messenger, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="99fc8-140">tooconfigure a testu Azure AD jednotné přihlašování s TigerText zabezpečení Messenger, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="99fc8-140">tooconfigure and test Azure AD single sign-on with TigerText Secure Messenger, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="99fc8-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="99fc8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="99fc8-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99fc8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="99fc8-143">**[Vytvoření zkušebního uživatele Messenger zabezpečení TigerText](#create-a-tigertext-secure-messenger-test-user)**  -toohave protějšek Britta Simon v zabezpečené Messenger TigerText, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="99fc8-143">**[Create a TigerText Secure Messenger test user](#create-a-tigertext-secure-messenger-test-user)** - toohave a counterpart of Britta Simon in TigerText Secure Messenger that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="99fc8-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="99fc8-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="99fc8-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="99fc8-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="99fc8-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="99fc8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="99fc8-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci TigerText zabezpečení Messenger.</span><span class="sxs-lookup"><span data-stu-id="99fc8-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TigerText Secure Messenger application.</span></span>

<span data-ttu-id="99fc8-148">**tooconfigure Azure AD jednotné přihlašování s TigerText zabezpečení Messenger, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="99fc8-148">**tooconfigure Azure AD single sign-on with TigerText Secure Messenger, perform hello following steps:**</span></span>

1. <span data-ttu-id="99fc8-149">V portálu Azure, na hello hello **TigerText zabezpečení Messenger** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-149">In hello Azure portal, on hello **TigerText Secure Messenger** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="99fc8-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="99fc8-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Na základě SAML přihlášení](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. <span data-ttu-id="99fc8-153">Na hello **TigerText zabezpečení domény Messenger a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="99fc8-153">On hello **TigerText Secure Messenger Domain and URLs** section, perform hello following steps:</span></span>

    ![Část TigerText zabezpečení domény Messenger a adresy URL](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    <span data-ttu-id="99fc8-155">a.</span><span class="sxs-lookup"><span data-stu-id="99fc8-155">a.</span></span> <span data-ttu-id="99fc8-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://home.tigertext.com`</span><span class="sxs-lookup"><span data-stu-id="99fc8-156">In hello **Sign-on URL** textbox, type URL as: `https://home.tigertext.com`</span></span>

    <span data-ttu-id="99fc8-157">b.</span><span class="sxs-lookup"><span data-stu-id="99fc8-157">b.</span></span> <span data-ttu-id="99fc8-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span><span class="sxs-lookup"><span data-stu-id="99fc8-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="99fc8-159">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="99fc8-159">This value is not real.</span></span> <span data-ttu-id="99fc8-160">Aktualizujte tuto hodnotu s hello skutečné identifikátor.</span><span class="sxs-lookup"><span data-stu-id="99fc8-160">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="99fc8-161">Obraťte se na [tým podpory TigerText zabezpečení Messenger klienta](mailTo:prosupport@tigertext.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="99fc8-161">Contact [TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="99fc8-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="99fc8-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. <span data-ttu-id="99fc8-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="99fc8-164">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="99fc8-166">tooget jednotné přihlašování nakonfigurovaný pro aplikace, obraťte se na [tým podpory zabezpečení Messenger TigerText](mailTo:prosupport@tigertext.com) a poskytněte hello **stažené metadata**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-166">tooget single sign-on configured for your application, contact [TigerText Secure Messenger support team](mailTo:prosupport@tigertext.com) and provide them hello **Downloaded metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="99fc8-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="99fc8-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="99fc8-168">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="99fc8-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="99fc8-169">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="99fc8-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="99fc8-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="99fc8-170">Create an Azure AD test user</span></span>
<span data-ttu-id="99fc8-171">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="99fc8-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="99fc8-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="99fc8-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="99fc8-174">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="99fc8-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="99fc8-176">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Uživatelé a skupiny -> všichni uživatelé](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="99fc8-178">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="99fc8-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="99fc8-180">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="99fc8-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="99fc8-182">a.</span><span class="sxs-lookup"><span data-stu-id="99fc8-182">a.</span></span> <span data-ttu-id="99fc8-183">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="99fc8-184">b.</span><span class="sxs-lookup"><span data-stu-id="99fc8-184">b.</span></span> <span data-ttu-id="99fc8-185">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="99fc8-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="99fc8-186">c.</span><span class="sxs-lookup"><span data-stu-id="99fc8-186">c.</span></span> <span data-ttu-id="99fc8-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="99fc8-188">d.</span><span class="sxs-lookup"><span data-stu-id="99fc8-188">d.</span></span> <span data-ttu-id="99fc8-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-189">Click **Create**.</span></span>
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a><span data-ttu-id="99fc8-190">Vytvoření zkušebního uživatele TigerText zabezpečení Messenger</span><span class="sxs-lookup"><span data-stu-id="99fc8-190">Create a TigerText Secure Messenger test user</span></span>

<span data-ttu-id="99fc8-191">V této části vytvoříte volal Britta Simon v TigerText uživatele.</span><span class="sxs-lookup"><span data-stu-id="99fc8-191">In this section, you create a user called Britta Simon in TigerText.</span></span> <span data-ttu-id="99fc8-192">Prosím oslovení příliš[tým podpory TigerText zabezpečení Messenger klienta](mailTo:prosupport@tigertext.com) tooadd hello uživatelé v platformě TigerText hello.</span><span class="sxs-lookup"><span data-stu-id="99fc8-192">Please reach out too[TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) tooadd hello users in hello TigerText platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="99fc8-193">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="99fc8-193">Assign hello Azure AD test user</span></span>

<span data-ttu-id="99fc8-194">V této části můžete povolit Britta Simon toouse Azure jednotné přihlašování a udělení přístupu tooTigerText zabezpečení Messenger.</span><span class="sxs-lookup"><span data-stu-id="99fc8-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTigerText Secure Messenger.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="99fc8-196">**tooassign tooTigerText Britta Simon zabezpečení Messenger, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="99fc8-196">**tooassign Britta Simon tooTigerText Secure Messenger, perform hello following steps:**</span></span>

1. <span data-ttu-id="99fc8-197">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="99fc8-199">V seznamu aplikace hello vyberte **TigerText zabezpečení Messenger**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-199">In hello applications list, select **TigerText Secure Messenger**.</span></span>

    ![TigerText zabezpečení Messenger v seznamu aplikací](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. <span data-ttu-id="99fc8-201">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="99fc8-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="99fc8-203">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="99fc8-203">Click **Add** button.</span></span> <span data-ttu-id="99fc8-204">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="99fc8-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="99fc8-206">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="99fc8-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="99fc8-207">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="99fc8-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="99fc8-208">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="99fc8-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="99fc8-209">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="99fc8-209">Test single sign-on</span></span>

<span data-ttu-id="99fc8-210">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="99fc8-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="99fc8-211">Když kliknete na dlaždici TigerText hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour TigerText aplikace.</span><span class="sxs-lookup"><span data-stu-id="99fc8-211">When you click hello TigerText tile in hello Access Panel, you should get automatically signed-on tooyour TigerText application.</span></span> <span data-ttu-id="99fc8-212">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="99fc8-212">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="99fc8-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="99fc8-213">Additional resources</span></span>

* [<span data-ttu-id="99fc8-214">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99fc8-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="99fc8-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="99fc8-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

