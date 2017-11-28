---
title: 'Kurz: Azure Active Directory integrace s Workrite | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Workrite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2a5c2956-a011-4d5c-877b-80679b6587b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: a663374ae3c8b102b53d8cf05a9cb083b80dbb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workrite"></a><span data-ttu-id="9ce2c-103">Kurz: Azure Active Directory integrace s Workrite</span><span class="sxs-lookup"><span data-stu-id="9ce2c-103">Tutorial: Azure Active Directory integration with Workrite</span></span>

<span data-ttu-id="9ce2c-104">V tomto kurzu zjistíte, jak toointegrate Workrite s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ce2c-104">In this tutorial, you learn how toointegrate Workrite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9ce2c-105">Integrace Workrite s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9ce2c-105">Integrating Workrite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9ce2c-106">Můžete ovládat ve službě Azure AD, který má přístup tooWorkrite.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-106">You can control in Azure AD who has access tooWorkrite.</span></span>
- <span data-ttu-id="9ce2c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWorkrite (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-107">You can enable your users tooautomatically get signed-on tooWorkrite (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9ce2c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="9ce2c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9ce2c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ce2c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ce2c-110">Prerequisites</span></span>

<span data-ttu-id="9ce2c-111">Integrace služby Azure AD s Workrite tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9ce2c-111">tooconfigure Azure AD integration with Workrite, you need hello following items:</span></span>

- <span data-ttu-id="9ce2c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ce2c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9ce2c-113">Workrite jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9ce2c-113">A Workrite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9ce2c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9ce2c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9ce2c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9ce2c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9ce2c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ce2c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9ce2c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9ce2c-118">Scenario description</span></span>
<span data-ttu-id="9ce2c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9ce2c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9ce2c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9ce2c-121">Přidání Workrite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9ce2c-121">Adding Workrite from hello gallery</span></span>
2. <span data-ttu-id="9ce2c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ce2c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workrite-from-hello-gallery"></a><span data-ttu-id="9ce2c-123">Přidání Workrite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9ce2c-123">Adding Workrite from hello gallery</span></span>
<span data-ttu-id="9ce2c-124">tooconfigure hello integrace Workrite do Azure AD, je nutné tooadd Workrite hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-124">tooconfigure hello integration of Workrite into Azure AD, you need tooadd Workrite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9ce2c-125">**tooadd Workrite z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ce2c-125">**tooadd Workrite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ce2c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="9ce2c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9ce2c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="9ce2c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="9ce2c-133">Hello vyhledávacího pole zadejte **Workrite**, vyberte **Workrite** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-133">In hello search box, type **Workrite**, select **Workrite** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Workrite v seznamu výsledků hello](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9ce2c-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ce2c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="9ce2c-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workrite podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9ce2c-136">In this section, you configure and test Azure AD single sign-on with Workrite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9ce2c-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Workrite je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workrite is tooa user in Azure AD.</span></span> <span data-ttu-id="9ce2c-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Workrite musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-138">In other words, a link relationship between an Azure AD user and hello related user in Workrite needs toobe established.</span></span>

<span data-ttu-id="9ce2c-139">V Workrite, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-139">In Workrite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9ce2c-140">tooconfigure a testu Azure AD jednotné přihlašování s Workrite, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9ce2c-140">tooconfigure and test Azure AD single sign-on with Workrite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9ce2c-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9ce2c-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9ce2c-143">**[Vytvoření zkušebního uživatele Workrite](#create-a-workrite-test-user)**  -toohave protějšek Britta Simon v Workrite, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-143">**[Create a Workrite test user](#create-a-workrite-test-user)** - toohave a counterpart of Britta Simon in Workrite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9ce2c-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9ce2c-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9ce2c-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ce2c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9ce2c-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Workrite.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workrite application.</span></span>

<span data-ttu-id="9ce2c-148">**tooconfigure Azure AD jednotné přihlašování s Workrite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ce2c-148">**tooconfigure Azure AD single sign-on with Workrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ce2c-149">V portálu Azure, na hello hello **Workrite** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-149">In hello Azure portal, on hello **Workrite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="9ce2c-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_samlbase.png)

3. <span data-ttu-id="9ce2c-153">Na hello **Workrite domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9ce2c-153">On hello **Workrite Domain and URLs** section, perform hello following steps:</span></span>

    ![Workrite domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_url.png)

    <span data-ttu-id="9ce2c-155">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="9ce2c-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9ce2c-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-156">This value is not real.</span></span> <span data-ttu-id="9ce2c-157">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="9ce2c-158">Obraťte se na [tým podpory Workrite klienta](mailto:support@workrite.co.uk) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-158">Contact [Workrite Client support team](mailto:support@workrite.co.uk) tooget this value.</span></span>

4. <span data-ttu-id="9ce2c-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_certificate.png) 

5. <span data-ttu-id="9ce2c-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-workrite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9ce2c-163">Na hello **Workrite konfigurace** klikněte na tlačítko **konfigurace Workrite** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-163">On hello **Workrite Configuration** section, click **Configure Workrite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9ce2c-164">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9ce2c-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Workrite](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_configure.png) 

7. <span data-ttu-id="9ce2c-166">tooconfigure jednotného přihlašování na **Workrite** straně, je nutné stáhnout hello toosend **Certificate(Base64), adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[Workrite tým podpory](mailto:support@workrite.co.uk).</span><span class="sxs-lookup"><span data-stu-id="9ce2c-166">tooconfigure single sign-on on **Workrite** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Workrite support team](mailto:support@workrite.co.uk).</span></span>

> [!TIP]
> <span data-ttu-id="9ce2c-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="9ce2c-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9ce2c-168">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9ce2c-169">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9ce2c-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9ce2c-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ce2c-170">Create an Azure AD test user</span></span>

<span data-ttu-id="9ce2c-171">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="9ce2c-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ce2c-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ce2c-174">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-workrite-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="9ce2c-176">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-workrite-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="9ce2c-178">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-workrite-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="9ce2c-180">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9ce2c-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-workrite-tutorial/create_aaduser_04.png)

    <span data-ttu-id="9ce2c-182">a.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-182">a.</span></span> <span data-ttu-id="9ce2c-183">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9ce2c-184">b.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-184">b.</span></span> <span data-ttu-id="9ce2c-185">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="9ce2c-186">c.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-186">c.</span></span> <span data-ttu-id="9ce2c-187">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="9ce2c-188">d.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-188">d.</span></span> <span data-ttu-id="9ce2c-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-189">Click **Create**.</span></span>
 
### <a name="create-a-workrite-test-user"></a><span data-ttu-id="9ce2c-190">Vytvoření zkušebního uživatele Workrite</span><span class="sxs-lookup"><span data-stu-id="9ce2c-190">Create a Workrite test user</span></span>

<span data-ttu-id="9ce2c-191">Hello cílem této části je toocreate volal Britta Simon v Workrite uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-191">hello objective of this section is toocreate a user called Britta Simon in Workrite.</span></span>

<span data-ttu-id="9ce2c-192">**toocreate uživatel volal Britta Simon v Workrite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ce2c-192">**toocreate a user called Britta Simon in Workrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ce2c-193">Přihlaste se na webu společnosti workrite tooyour jako správce.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-193">Sign on tooyour workrite company site as administrator.</span></span>

2. <span data-ttu-id="9ce2c-194">V navigačním podokně hello, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-194">In hello navigation pane, click **Admin**.</span></span>
   
    ![Správce řízení][400]

3. <span data-ttu-id="9ce2c-196">Přejděte tooQuick odkazy a pak klikněte na tlačítko **vytvořte uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-196">Go tooQuick Links, and then click **Create a User**.</span></span>
   
    ![Vytvořit oddíl uživatele][401]

4. <span data-ttu-id="9ce2c-198">Na hello **vytvořit uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9ce2c-198">On hello **Create User** dialog, perform hello following steps:</span></span>
   
    ![Vytvořit uživatele Dailog][402]
    
    <span data-ttu-id="9ce2c-200">a.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-200">a.</span></span> <span data-ttu-id="9ce2c-201">V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-201">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="9ce2c-202">b.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-202">b.</span></span> <span data-ttu-id="9ce2c-203">V hello **křestní jméno** textovému poli, firstname hello typ uživatele jako Britta.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-203">In hello **First Name** textbox, type hello firstname of user like Britta.</span></span>

    <span data-ttu-id="9ce2c-204">c.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-204">c.</span></span> <span data-ttu-id="9ce2c-205">V hello **Přezdívka** textovému poli, typ hello Přezdívka uživatele jako Simon.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-205">In hello **Surname** textbox, type hello surname of user like Simon.</span></span>
    
    <span data-ttu-id="9ce2c-206">d.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-206">d.</span></span> <span data-ttu-id="9ce2c-207">Vyberte **správce klienta** jako **zvolte roli**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-207">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="9ce2c-208">e.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-208">e.</span></span> <span data-ttu-id="9ce2c-209">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-209">Click **Save**.</span></span>   

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9ce2c-210">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9ce2c-210">Assign hello Azure AD test user</span></span>

<span data-ttu-id="9ce2c-211">V této části povolíte tak, že udělíte přístup tooWorkrite toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkrite.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="9ce2c-213">**tooassign Britta Simon tooWorkrite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ce2c-213">**tooassign Britta Simon tooWorkrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ce2c-214">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9ce2c-216">V seznamu aplikace hello vyberte **Workrite**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-216">In hello applications list, select **Workrite**.</span></span>

    ![v seznamu aplikace hello Hello Workrite odkaz](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_app.png)  

3. <span data-ttu-id="9ce2c-218">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="9ce2c-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-220">Click **Add** button.</span></span> <span data-ttu-id="9ce2c-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="9ce2c-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9ce2c-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9ce2c-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9ce2c-226">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ce2c-226">Test single sign-on</span></span>

<span data-ttu-id="9ce2c-227">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="9ce2c-228">Když kliknete na dlaždici Workrite hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Workrite aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ce2c-228">When you click hello Workrite tile in hello Access Panel, you should get automatically signed-on tooyour Workrite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ce2c-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9ce2c-229">Additional resources</span></span>

* [<span data-ttu-id="9ce2c-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9ce2c-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9ce2c-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9ce2c-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_402.png

