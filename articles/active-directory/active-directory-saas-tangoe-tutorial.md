---
title: "Kurz: Azure Active Directory integrace s Tangoe příkaz Premium Mobile | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Tangoe příkaz Premium Mobile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 7bd1cd6afade58a4a47a173b249f92eb84e42112
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a><span data-ttu-id="120db-103">Kurz: Azure Active Directory integrace s Tangoe příkaz Premium Mobile</span><span class="sxs-lookup"><span data-stu-id="120db-103">Tutorial: Azure Active Directory integration with Tangoe Command Premium Mobile</span></span>

<span data-ttu-id="120db-104">V tomto kurzu zjistíte, jak toointegrate Tangoe příkaz Premium mobilní s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="120db-104">In this tutorial, you learn how toointegrate Tangoe Command Premium Mobile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="120db-105">Integrace Mobile Premium příkaz Tangoe s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="120db-105">Integrating Tangoe Command Premium Mobile with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="120db-106">Můžete řídit ve službě Azure AD, který má přístup tooTangoe příkaz Premium Mobile</span><span class="sxs-lookup"><span data-stu-id="120db-106">You can control in Azure AD who has access tooTangoe Command Premium Mobile</span></span>
- <span data-ttu-id="120db-107">Vaši uživatelé tooautomatically get přihlášeného tooTangoe příkaz Mobile Premium (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="120db-107">You can enable your users tooautomatically get signed-on tooTangoe Command Premium Mobile (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="120db-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="120db-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="120db-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="120db-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="120db-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="120db-110">Prerequisites</span></span>

<span data-ttu-id="120db-111">tooconfigure integrace Azure AD s Tangoe příkaz Premium Mobile, musíte hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="120db-111">tooconfigure Azure AD integration with Tangoe Command Premium Mobile, you need hello following items:</span></span>

- <span data-ttu-id="120db-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="120db-112">An Azure AD subscription</span></span>
- <span data-ttu-id="120db-113">Tangoe příkaz Premium Mobile jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="120db-113">A Tangoe Command Premium Mobile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="120db-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="120db-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="120db-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="120db-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="120db-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="120db-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="120db-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="120db-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="120db-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="120db-118">Scenario description</span></span>
<span data-ttu-id="120db-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="120db-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="120db-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="120db-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="120db-121">Přidání Mobile Premium Tangoe příkaz z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="120db-121">Add Tangoe Command Premium Mobile from hello gallery</span></span>
2. <span data-ttu-id="120db-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="120db-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tangoe-command-premium-mobile-from-hello-gallery"></a><span data-ttu-id="120db-123">Přidání Mobile Premium Tangoe příkaz z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="120db-123">Add Tangoe Command Premium Mobile from hello gallery</span></span>
<span data-ttu-id="120db-124">tooconfigure hello integrace Mobile Premium příkaz Tangoe do Azure AD, je nutné tooadd Tangoe příkaz Premium Mobile hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="120db-124">tooconfigure hello integration of Tangoe Command Premium Mobile into Azure AD, you need tooadd Tangoe Command Premium Mobile from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="120db-125">**tooadd Mobile Premium Tangoe příkaz z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="120db-125">**tooadd Tangoe Command Premium Mobile from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="120db-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="120db-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="120db-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="120db-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="120db-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="120db-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="120db-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="120db-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="120db-133">Hello vyhledávacího pole zadejte **Tangoe příkaz Premium Mobile**, vyberte **Tangoe příkaz Premium Mobile** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="120db-133">In hello search box, type **Tangoe Command Premium Mobile**, select **Tangoe Command Premium Mobile** from result panel then click **Add** button tooadd hello application.</span></span>

    ![<span data-ttu-id="120db-134">Přidání Mobile Premium Tangoe příkaz z Galerie</span><span class="sxs-lookup"><span data-stu-id="120db-134">Add Tangoe Command Premium Mobile from gallery</span></span> ](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="120db-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="120db-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="120db-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Tangoe příkaz Premium Mobile podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="120db-136">In this section, you configure and test Azure AD single sign-on with Tangoe Command Premium Mobile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="120db-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Tangoe příkaz Premium Mobile je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="120db-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tangoe Command Premium Mobile is tooa user in Azure AD.</span></span> <span data-ttu-id="120db-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Tangoe příkaz Premium Mobile musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="120db-138">In other words, a link relationship between an Azure AD user and hello related user in Tangoe Command Premium Mobile needs toobe established.</span></span>

<span data-ttu-id="120db-139">V modulu snap-in Mobile Premium příkaz Tangoe přiřadit hello hodnotu hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="120db-139">In Tangoe Command Premium Mobile, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="120db-140">tooconfigure a testu Azure AD jednotné přihlašování s Tangoe příkaz Premium Mobile, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="120db-140">tooconfigure and test Azure AD single sign-on with Tangoe Command Premium Mobile, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="120db-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="120db-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="120db-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="120db-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="120db-143">**[Vytvoření zkušebního uživatele Mobile Premium příkaz Tangoe](#create-a-tangoe-command-premium-mobile-test-user)**  -toohave protějšek Britta Simon v Tangoe příkaz Premium Mobile, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="120db-143">**[Create a Tangoe Command Premium Mobile test user](#create-a-tangoe-command-premium-mobile-test-user)** - toohave a counterpart of Britta Simon in Tangoe Command Premium Mobile that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="120db-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="120db-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="120db-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="120db-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="120db-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="120db-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="120db-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v Tangoe příkaz Premium mobilní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="120db-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tangoe Command Premium Mobile application.</span></span>

<span data-ttu-id="120db-148">**tooconfigure Azure AD jednotné přihlašování s Tangoe příkaz Premium mobilních, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="120db-148">**tooconfigure Azure AD single sign-on with Tangoe Command Premium Mobile, perform hello following steps:**</span></span>

1. <span data-ttu-id="120db-149">V portálu Azure, na hello hello **Tangoe příkaz Premium Mobile** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="120db-149">In hello Azure portal, on hello **Tangoe Command Premium Mobile** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="120db-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="120db-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Na základě SAML přihlášení](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_samlbase.png)

3. <span data-ttu-id="120db-153">Na hello **Tangoe příkaz Premium Mobile domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="120db-153">On hello **Tangoe Command Premium Mobile Domain and URLs** section, perform hello following steps:</span></span>

    ![Příkaz Tangoe Premium mobilní domény a adresy URL](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_url.png)

    <span data-ttu-id="120db-155">a.</span><span class="sxs-lookup"><span data-stu-id="120db-155">a.</span></span> <span data-ttu-id="120db-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span><span class="sxs-lookup"><span data-stu-id="120db-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span></span>

    <span data-ttu-id="120db-157">b.</span><span class="sxs-lookup"><span data-stu-id="120db-157">b.</span></span> <span data-ttu-id="120db-158">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://sso.tangoe.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="120db-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://sso.tangoe.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="120db-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="120db-159">These values are not real.</span></span> <span data-ttu-id="120db-160">Tyto hodnoty aktualizujte hello skutečná adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="120db-160">Update these values with hello actual  Reply URL and Sign-On URL.</span></span> <span data-ttu-id="120db-161">Obraťte se na [tým podpory Tangoe příkaz Premium mobilního klienta](https://www.tangoe.com/contact-2/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="120db-161">Contact [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) tooget these values.</span></span> 

4. <span data-ttu-id="120db-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="120db-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_certificate.png) 

5. <span data-ttu-id="120db-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="120db-164">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-tangoe-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="120db-166">Na hello **Konfigurace mobilních Premium příkaz Tangoe** klikněte na tlačítko **konfigurace Tangoe příkaz Premium Mobile** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="120db-166">On hello **Tangoe Command Premium Mobile Configuration** section, click **Configure Tangoe Command Premium Mobile** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="120db-167">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="120db-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Tangoe příkaz Premium mobilní konfigurační oddíl](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_configure.png) 

7. <span data-ttu-id="120db-169">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na vaše [tým podpory Tangoe příkaz Premium mobilního klienta](https://www.tangoe.com/contact-2/) a zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="120db-169">tooget SSO configured for your application, contact your [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) and provide hello following:</span></span>

   - <span data-ttu-id="120db-170">soubor stažený metadat Hello</span><span class="sxs-lookup"><span data-stu-id="120db-170">hello downloaded metadata file</span></span>
   - <span data-ttu-id="120db-171">Hello **SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="120db-171">hello **SAML Entity ID**</span></span>
   - <span data-ttu-id="120db-172">Hello **SAML jeden přihlašování adresa URL služby**</span><span class="sxs-lookup"><span data-stu-id="120db-172">hello **SAML Single Sign-On Service URL**</span></span>
   - <span data-ttu-id="120db-173">Hello **Sign-Out adresy URL**</span><span class="sxs-lookup"><span data-stu-id="120db-173">hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="120db-174">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="120db-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="120db-175">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="120db-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="120db-176">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="120db-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="120db-177">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="120db-177">Create an Azure AD test user</span></span>
<span data-ttu-id="120db-178">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="120db-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="120db-180">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="120db-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="120db-181">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="120db-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tangoe-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="120db-183">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="120db-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Uživatelé a skupiny -> všichni uživatelé](./media/active-directory-saas-tangoe-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="120db-185">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="120db-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Přidání uživatele](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="120db-187">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="120db-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno stránka uživatel](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="120db-189">a.</span><span class="sxs-lookup"><span data-stu-id="120db-189">a.</span></span> <span data-ttu-id="120db-190">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="120db-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="120db-191">b.</span><span class="sxs-lookup"><span data-stu-id="120db-191">b.</span></span> <span data-ttu-id="120db-192">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="120db-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="120db-193">c.</span><span class="sxs-lookup"><span data-stu-id="120db-193">c.</span></span> <span data-ttu-id="120db-194">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="120db-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="120db-195">d.</span><span class="sxs-lookup"><span data-stu-id="120db-195">d.</span></span> <span data-ttu-id="120db-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="120db-196">Click **Create**.</span></span>
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a><span data-ttu-id="120db-197">Vytvoření zkušebního uživatele Tangoe příkaz Premium Mobile</span><span class="sxs-lookup"><span data-stu-id="120db-197">Create a Tangoe Command Premium Mobile test user</span></span>

<span data-ttu-id="120db-198">V této části vytvoříte uživatele názvem Britta Simon v Tangoe příkaz Premium Mobile.</span><span class="sxs-lookup"><span data-stu-id="120db-198">In this section, you create a user called Britta Simon in Tangoe Command Premium Mobile.</span></span> 

<span data-ttu-id="120db-199">Tangoe příkaz Premium mobilní aplikace, musí všechny toobe uživatelé hello zřízené v hello aplikace před provedením jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="120db-199">Tangoe Command Premium Mobile application needs all hello users toobe provisioned in hello application before doing Single Sign On.</span></span> <span data-ttu-id="120db-200">Proto prosím práci s hello [tým podpory Tangoe příkaz Premium mobilního klienta](https://www.tangoe.com/contact-2/) tooprovision tito uživatelé do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="120db-200">So please work with hello [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) tooprovision all these users into hello application.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="120db-201">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="120db-201">Assign hello Azure AD test user</span></span>

<span data-ttu-id="120db-202">V této části povolíte tak, že udělíte přístup tooTangoe příkaz Premium Mobile Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="120db-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTangoe Command Premium Mobile.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="120db-204">**tooassign Britta Simon tooTangoe příkaz Premium mobilních, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="120db-204">**tooassign Britta Simon tooTangoe Command Premium Mobile, perform hello following steps:**</span></span>

1. <span data-ttu-id="120db-205">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="120db-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="120db-207">V seznamu aplikace hello vyberte **Tangoe příkaz Premium Mobile**.</span><span class="sxs-lookup"><span data-stu-id="120db-207">In hello applications list, select **Tangoe Command Premium Mobile**.</span></span>

    ![Tangoe příkaz Premium Mobile v seznamu aplikací](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_app.png) 

3. <span data-ttu-id="120db-209">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="120db-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="120db-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="120db-211">Click **Add** button.</span></span> <span data-ttu-id="120db-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="120db-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="120db-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="120db-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="120db-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="120db-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="120db-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="120db-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="120db-217">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="120db-217">Test single sign-on</span></span>

<span data-ttu-id="120db-218">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="120db-218">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="120db-219">Když kliknete na dlaždici Tangoe příkaz Premium Mobile hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Tangoe příkaz Premium mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="120db-219">When you click hello Tangoe Command Premium Mobile tile in hello Access Panel, you should get automatically signed-on tooyour Tangoe Command Premium Mobile application.</span></span> <span data-ttu-id="120db-220">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="120db-220">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="120db-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="120db-221">Additional resources</span></span>

* [<span data-ttu-id="120db-222">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="120db-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="120db-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="120db-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png

