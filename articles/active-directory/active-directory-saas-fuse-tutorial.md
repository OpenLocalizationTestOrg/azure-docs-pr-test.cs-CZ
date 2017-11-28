---
title: 'Kurz: Azure Active Directory integrace s pojistka | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a pojistka."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ef34f58-863a-4b37-875c-e8efa3e18bb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 720ed8af0b5de1e3bee5a40353ca0ee661766864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuse"></a><span data-ttu-id="063b6-103">Kurz: Azure Active Directory integrace s pojistka</span><span class="sxs-lookup"><span data-stu-id="063b6-103">Tutorial: Azure Active Directory integration with Fuse</span></span>

<span data-ttu-id="063b6-104">V tomto kurzu zjistíte, jak toointegrate pojistka službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="063b6-104">In this tutorial, you learn how toointegrate Fuse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="063b6-105">Integrace pojistka s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="063b6-105">Integrating Fuse with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="063b6-106">Můžete ovládat ve službě Azure AD, který má přístup tooFuse.</span><span class="sxs-lookup"><span data-stu-id="063b6-106">You can control in Azure AD who has access tooFuse.</span></span>
- <span data-ttu-id="063b6-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFuse (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="063b6-107">You can enable your users tooautomatically get signed-on tooFuse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="063b6-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="063b6-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="063b6-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="063b6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="063b6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="063b6-110">Prerequisites</span></span>

<span data-ttu-id="063b6-111">integrace tooconfigure Azure AD s pojistka, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="063b6-111">tooconfigure Azure AD integration with Fuse, you need hello following items:</span></span>

- <span data-ttu-id="063b6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="063b6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="063b6-113">Pojistka jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="063b6-113">A Fuse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="063b6-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="063b6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="063b6-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="063b6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="063b6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="063b6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="063b6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="063b6-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="063b6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="063b6-118">Scenario description</span></span>
<span data-ttu-id="063b6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="063b6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="063b6-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="063b6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="063b6-121">Přidat pojistka z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="063b6-121">Add Fuse from hello gallery</span></span>
2. <span data-ttu-id="063b6-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="063b6-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-fuse-from-hello-gallery"></a><span data-ttu-id="063b6-123">Přidat pojistka z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="063b6-123">Add Fuse from hello gallery</span></span>
<span data-ttu-id="063b6-124">tooconfigure hello integrace pojistka do Azure AD, je nutné tooadd pojistka hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="063b6-124">tooconfigure hello integration of Fuse into Azure AD, you need tooadd Fuse from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="063b6-125">**tooadd pojistka z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="063b6-125">**tooadd Fuse from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="063b6-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="063b6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="063b6-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="063b6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="063b6-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="063b6-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="063b6-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="063b6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="063b6-133">Hello vyhledávacího pole zadejte **pojistka**, vyberte **pojistka** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="063b6-133">In hello search box, type **Fuse**, select **Fuse** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Pojistka seznam výsledků hello](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="063b6-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="063b6-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="063b6-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s pojistka podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="063b6-136">In this section, you configure and test Azure AD single sign-on with Fuse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="063b6-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v pojistka je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="063b6-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Fuse is tooa user in Azure AD.</span></span> <span data-ttu-id="063b6-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v toobe pojistka musí navázat.</span><span class="sxs-lookup"><span data-stu-id="063b6-138">In other words, a link relationship between an Azure AD user and hello related user in Fuse needs toobe established.</span></span>

<span data-ttu-id="063b6-139">V pojistka, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="063b6-139">In Fuse, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="063b6-140">tooconfigure a testu Azure AD jednotné přihlašování s pojistka, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="063b6-140">tooconfigure and test Azure AD single sign-on with Fuse, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="063b6-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="063b6-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="063b6-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="063b6-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="063b6-143">**[Vytvoření zkušebního uživatele pojistka](#create-a-fuse-test-user)**  -toohave protějšek Britta Simon v pojistka, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="063b6-143">**[Create a Fuse test user](#create-a-fuse-test-user)** - toohave a counterpart of Britta Simon in Fuse that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="063b6-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="063b6-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="063b6-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="063b6-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="063b6-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="063b6-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="063b6-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci pojistka.</span><span class="sxs-lookup"><span data-stu-id="063b6-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Fuse application.</span></span>

<span data-ttu-id="063b6-148">**tooconfigure Azure AD jednotné přihlašování s pojistka, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="063b6-148">**tooconfigure Azure AD single sign-on with Fuse, perform hello following steps:**</span></span>

1. <span data-ttu-id="063b6-149">V portálu Azure, na hello hello **pojistka** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="063b6-149">In hello Azure portal, on hello **Fuse** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="063b6-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="063b6-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_samlbase.png)

3. <span data-ttu-id="063b6-153">Na hello **pojistka domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="063b6-153">On hello **Fuse Domain and URLs** section, perform hello following steps:</span></span>

    ![Pojistka adresy URL jeden přihlašování informace o doméně a](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_url.png)
    
    <span data-ttu-id="063b6-155">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant name>.fusion-universal.com/`</span><span class="sxs-lookup"><span data-stu-id="063b6-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant name>.fusion-universal.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="063b6-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="063b6-156">This value is not real.</span></span> <span data-ttu-id="063b6-157">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="063b6-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="063b6-158">Obraťte se na [tým podpory pojistka klienta](mailto:support@fusion-universal.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="063b6-158">Contact [Fuse Client support team](mailto:support@fusion-universal.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="063b6-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="063b6-159">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_certificate.png) 

5. <span data-ttu-id="063b6-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="063b6-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-fuse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="063b6-163">Na hello **pojistka konfigurace** klikněte na tlačítko **konfigurace pojistka** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="063b6-163">On hello **Fuse Configuration** section, click **Configure Fuse** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="063b6-164">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="063b6-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace pojistka](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_configure.png) 

7. <span data-ttu-id="063b6-166">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [tým podpory pojistka](mailto:support@fusion-universal.com) a poskytnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="063b6-166">tooget SSO configured for your application, contact [Fuse support team](mailto:support@fusion-universal.com) and provide them with hello following:</span></span>

    * <span data-ttu-id="063b6-167">Hello Stáhnout **soubor certifikátu (Raw)**</span><span class="sxs-lookup"><span data-stu-id="063b6-167">hello downloaded **Certificate (Raw) file**</span></span>
    * <span data-ttu-id="063b6-168">Hello **SAML jeden přihlašování adresa URL služby**</span><span class="sxs-lookup"><span data-stu-id="063b6-168">hello **SAML Single Sign-On Service URL**</span></span>
    * <span data-ttu-id="063b6-169">Hello **SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="063b6-169">hello **SAML Entity ID**</span></span>
    * <span data-ttu-id="063b6-170">Hello **Sign-Out adresy URL**</span><span class="sxs-lookup"><span data-stu-id="063b6-170">hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="063b6-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="063b6-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="063b6-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="063b6-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="063b6-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="063b6-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="063b6-174">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="063b6-174">Create an Azure AD test user</span></span>

<span data-ttu-id="063b6-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="063b6-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="063b6-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="063b6-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="063b6-178">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="063b6-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-fuse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="063b6-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="063b6-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-fuse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="063b6-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="063b6-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-fuse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="063b6-184">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="063b6-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-fuse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="063b6-186">a.</span><span class="sxs-lookup"><span data-stu-id="063b6-186">a.</span></span> <span data-ttu-id="063b6-187">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="063b6-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="063b6-188">b.</span><span class="sxs-lookup"><span data-stu-id="063b6-188">b.</span></span> <span data-ttu-id="063b6-189">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="063b6-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="063b6-190">c.</span><span class="sxs-lookup"><span data-stu-id="063b6-190">c.</span></span> <span data-ttu-id="063b6-191">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="063b6-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="063b6-192">d.</span><span class="sxs-lookup"><span data-stu-id="063b6-192">d.</span></span> <span data-ttu-id="063b6-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="063b6-193">Click **Create**.</span></span>
 
### <a name="create-a-fuse-test-user"></a><span data-ttu-id="063b6-194">Vytvoření zkušebního uživatele pojistka</span><span class="sxs-lookup"><span data-stu-id="063b6-194">Create a Fuse test user</span></span>

<span data-ttu-id="063b6-195">V této části vytvoříte volal Britta Simon v pojistka uživatele.</span><span class="sxs-lookup"><span data-stu-id="063b6-195">In this section, you create a user called Britta Simon in Fuse.</span></span> <span data-ttu-id="063b6-196">Spojte se s [tým podpory pojistka](mailto:support@fusion-universal.com) tooadd hello uživatelé v platformě pojistka hello.</span><span class="sxs-lookup"><span data-stu-id="063b6-196">Please work with [Fuse support team](mailto:support@fusion-universal.com) tooadd hello users in hello Fuse platform.</span></span>


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="063b6-197">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="063b6-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="063b6-198">V této části povolíte tak, že udělíte přístup tooFuse toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="063b6-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFuse.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="063b6-200">**tooassign Britta Simon tooFuse, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="063b6-200">**tooassign Britta Simon tooFuse, perform hello following steps:**</span></span>

1. <span data-ttu-id="063b6-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="063b6-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="063b6-203">V seznamu aplikace hello vyberte **pojistka**.</span><span class="sxs-lookup"><span data-stu-id="063b6-203">In hello applications list, select **Fuse**.</span></span>

    ![Hello pojistka odkaz v seznamu aplikace hello](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_app.png)  

3. <span data-ttu-id="063b6-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="063b6-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="063b6-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="063b6-207">Click **Add** button.</span></span> <span data-ttu-id="063b6-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="063b6-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="063b6-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="063b6-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="063b6-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="063b6-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="063b6-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="063b6-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="063b6-213">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="063b6-213">Test single sign-on</span></span>

<span data-ttu-id="063b6-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="063b6-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="063b6-215">Když kliknete na dlaždici pojistka hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour pojistka aplikace.</span><span class="sxs-lookup"><span data-stu-id="063b6-215">When you click hello Fuse tile in hello Access Panel, you should get automatically signed-on tooyour Fuse application.</span></span>
<span data-ttu-id="063b6-216">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="063b6-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="063b6-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="063b6-217">Additional resources</span></span>

* [<span data-ttu-id="063b6-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="063b6-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="063b6-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="063b6-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_203.png

