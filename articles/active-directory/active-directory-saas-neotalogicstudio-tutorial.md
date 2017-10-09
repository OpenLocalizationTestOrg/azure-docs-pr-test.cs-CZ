---
title: 'Kurz: Azure Active Directory integrace s Neota logiku Studio | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Neota logiku Studio."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 842605e6-a91d-42cc-a0bb-e23e67173ae2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: jeedes
ms.openlocfilehash: a479ee49618de8275132dbc2c21a8ef723d92d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-neota-logic-studio"></a><span data-ttu-id="79987-103">Kurz: Azure Active Directory integrace s Neota logiku Studio</span><span class="sxs-lookup"><span data-stu-id="79987-103">Tutorial: Azure Active Directory integration with Neota Logic Studio</span></span>

<span data-ttu-id="79987-104">V tomto kurzu zjistíte, jak toointegrate Studio logiku Neota s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="79987-104">In this tutorial, you learn how toointegrate Neota Logic Studio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="79987-105">Integrace s Azure AD Neota logiku Studio poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="79987-105">Integrating Neota Logic Studio with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="79987-106">Můžete řídit ve službě Azure AD, který má přístup tooNeota Studio logiky</span><span class="sxs-lookup"><span data-stu-id="79987-106">You can control in Azure AD who has access tooNeota Logic Studio</span></span>
- <span data-ttu-id="79987-107">Vaši uživatelé tooautomatically get přihlášeného tooNeota logiku Studio (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="79987-107">You can enable your users tooautomatically get signed-on tooNeota Logic Studio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="79987-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="79987-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="79987-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="79987-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="79987-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="79987-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79987-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="79987-111">Prerequisites</span></span>

<span data-ttu-id="79987-112">tooconfigure integrace Azure AD s Neota logiku Studio, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="79987-112">tooconfigure Azure AD integration with Neota Logic Studio, you need hello following items:</span></span>

- <span data-ttu-id="79987-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="79987-113">An Azure AD subscription</span></span>
- <span data-ttu-id="79987-114">Neota logiku Studio jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="79987-114">A Neota Logic Studio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="79987-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="79987-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="79987-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="79987-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="79987-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="79987-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="79987-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="79987-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="79987-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="79987-119">Scenario description</span></span>

<span data-ttu-id="79987-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="79987-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="79987-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="79987-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="79987-122">Přidání logiky Studio Neota z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="79987-122">Adding Neota Logic Studio from hello gallery</span></span>
2. <span data-ttu-id="79987-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="79987-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-neota-logic-studio-from-hello-gallery"></a><span data-ttu-id="79987-124">Přidání logiky Studio Neota z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="79987-124">Adding Neota Logic Studio from hello gallery</span></span>

<span data-ttu-id="79987-125">tooconfigure hello integrace Neota logiku Studio do Azure AD, je nutné tooadd Neota logiku Studio hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="79987-125">tooconfigure hello integration of Neota Logic Studio into Azure AD, you need tooadd Neota Logic Studio from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="79987-126">**tooadd Neota logiku Studio z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="79987-126">**tooadd Neota Logic Studio from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="79987-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="79987-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="79987-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="79987-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="79987-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="79987-130">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="79987-132">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="79987-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="79987-134">Hello vyhledávacího pole zadejte **Neota logiku Studio**.</span><span class="sxs-lookup"><span data-stu-id="79987-134">In hello search box, type **Neota Logic Studio**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_search.png)

5. <span data-ttu-id="79987-136">Na panelu výsledků hello vyberte **Neota logiku Studio**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="79987-136">In hello results panel, select **Neota Logic Studio**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="79987-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="79987-138">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="79987-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Neota Studio logiku podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="79987-139">In this section, you configure and test Azure AD single sign-on with Neota Logic Studio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="79987-140">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Neota logiku Studio je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79987-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Neota Logic Studio is tooa user in Azure AD.</span></span> <span data-ttu-id="79987-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Neota logiku Studio musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="79987-141">In other words, a link relationship between an Azure AD user and hello related user in Neota Logic Studio needs toobe established.</span></span>

<span data-ttu-id="79987-142">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Neota logiku Studio.</span><span class="sxs-lookup"><span data-stu-id="79987-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Neota Logic Studio.</span></span>

<span data-ttu-id="79987-143">tooconfigure a testu Azure AD jednotné přihlašování s Neota logiku Studio, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="79987-143">tooconfigure and test Azure AD single sign-on with Neota Logic Studio, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="79987-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="79987-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="79987-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79987-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="79987-146">**[Vytvoření zkušebního uživatele Studio logiku Neota](#creating-a-neota-logic-studio-test-user)**  -toohave protějšek Britta Simon v Studio Neota logiku, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="79987-146">**[Creating a Neota Logic Studio test user](#creating-a-neota-logic-studio-test-user)** - toohave a counterpart of Britta Simon in Neota Logic Studio that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="79987-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="79987-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="79987-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="79987-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="79987-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="79987-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="79987-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci logiky Studio Neota.</span><span class="sxs-lookup"><span data-stu-id="79987-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Neota Logic Studio application.</span></span>

<span data-ttu-id="79987-151">**tooconfigure Azure AD jednotné přihlašování s Neota Studio logiku, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="79987-151">**tooconfigure Azure AD single sign-on with Neota Logic Studio, perform hello following steps:**</span></span>

1. <span data-ttu-id="79987-152">V portálu Azure, na hello hello **Neota logiku Studio** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="79987-152">In hello Azure portal, on hello **Neota Logic Studio** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="79987-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="79987-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_samlbase.png)

3. <span data-ttu-id="79987-156">Na hello **Neota logiku Studio domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="79987-156">On hello **Neota Logic Studio Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_url.png)

    <span data-ttu-id="79987-158">a.</span><span class="sxs-lookup"><span data-stu-id="79987-158">a.</span></span> <span data-ttu-id="79987-159">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<sub domain>.neotalogic.com/a/<sub application>`</span><span class="sxs-lookup"><span data-stu-id="79987-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<sub domain>.neotalogic.com/a/<sub application>`</span></span>

    <span data-ttu-id="79987-160">b.</span><span class="sxs-lookup"><span data-stu-id="79987-160">b.</span></span> <span data-ttu-id="79987-161">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<sub domain>.neotalogic.com/wb`</span><span class="sxs-lookup"><span data-stu-id="79987-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<sub domain>.neotalogic.com/wb`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="79987-162">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="79987-162">These values are not hello real.</span></span> <span data-ttu-id="79987-163">Aktualizovat tyto hodnoty s hello skutečná adresa URL přihlašování & identifikátor.</span><span class="sxs-lookup"><span data-stu-id="79987-163">Update these values with hello actual Sign-On URL & Identifier.</span></span> <span data-ttu-id="79987-164">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="79987-164">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="79987-165">Obraťte se na [tým podpory klienta studia logiku Neota](https://www.neotalogic.com/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="79987-165">Contact [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="79987-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="79987-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_certificate.png) 

5. <span data-ttu-id="79987-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="79987-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="79987-170">tooget jednotné přihlašování, které jsou nakonfigurované pro aplikace, obraťte se na [Neota logiku Studio podpora](https://www.neotalogic.com/contact-us/) týmu a poskytněte s Stáhnout **soubor XML s metadaty** souboru.</span><span class="sxs-lookup"><span data-stu-id="79987-170">tooget SSO configured for your application, Contact [Neota Logic Studio support](https://www.neotalogic.com/contact-us/) team and provide them with downloaded **Metadata XML** file.</span></span>

> [!TIP]
> <span data-ttu-id="79987-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="79987-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="79987-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="79987-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="79987-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="79987-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="79987-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="79987-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="79987-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="79987-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="79987-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="79987-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="79987-178">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="79987-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="79987-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="79987-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="79987-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="79987-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="79987-184">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="79987-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="79987-186">a.</span><span class="sxs-lookup"><span data-stu-id="79987-186">a.</span></span> <span data-ttu-id="79987-187">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="79987-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="79987-188">b.</span><span class="sxs-lookup"><span data-stu-id="79987-188">b.</span></span> <span data-ttu-id="79987-189">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="79987-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="79987-190">c.</span><span class="sxs-lookup"><span data-stu-id="79987-190">c.</span></span> <span data-ttu-id="79987-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="79987-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="79987-192">d.</span><span class="sxs-lookup"><span data-stu-id="79987-192">d.</span></span> <span data-ttu-id="79987-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="79987-193">Click **Create**.</span></span>
 
### <a name="creating-a-neota-logic-studio-test-user"></a><span data-ttu-id="79987-194">Vytvoření zkušebního uživatele Neota logiku Studio</span><span class="sxs-lookup"><span data-stu-id="79987-194">Creating a Neota Logic Studio test user</span></span>

<span data-ttu-id="79987-195">V této části vytvoříte uživatele volal Britta Simon v Neota logiku Studio.</span><span class="sxs-lookup"><span data-stu-id="79987-195">In this section, you create a user called Britta Simon in Neota Logic Studio.</span></span> <span data-ttu-id="79987-196">Práce s [tým podpory klienta studia logiku Neota](https://www.neotalogic.com/contact-us/) pro přidání uživatelů hello hello Neota logiku Studio platformy.</span><span class="sxs-lookup"><span data-stu-id="79987-196">work with [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to add hello users in hello Neota Logic Studio platform.</span></span> <span data-ttu-id="79987-197">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="79987-197">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="79987-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="79987-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="79987-199">V této části povolíte tak, že udělíte přístup tooNeota logiku Studio Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="79987-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNeota Logic Studio.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="79987-201">**tooassign Britta Simon tooNeota Studio logiku, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="79987-201">**tooassign Britta Simon tooNeota Logic Studio, perform hello following steps:**</span></span>

1. <span data-ttu-id="79987-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="79987-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="79987-204">V seznamu aplikace hello vyberte **Neota logiku Studio**.</span><span class="sxs-lookup"><span data-stu-id="79987-204">In hello applications list, select **Neota Logic Studio**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_app.png) 

3. <span data-ttu-id="79987-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="79987-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="79987-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="79987-208">Click **Add** button.</span></span> <span data-ttu-id="79987-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79987-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="79987-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="79987-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="79987-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79987-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="79987-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="79987-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="79987-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="79987-214">Testing single sign-on</span></span>

<span data-ttu-id="79987-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="79987-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="79987-216">Klikněte na dlaždici Neota logiku Studio hello v hello přístupového panelu, budou přihlašovací stránku přesměrovaného tooOrganization.</span><span class="sxs-lookup"><span data-stu-id="79987-216">Click hello Neota Logic Studio tile in hello Access Panel, you will be redirected tooOrganization sign-on page.</span></span> <span data-ttu-id="79987-217">Po úspěšném přihlášení bude přihlášeného tooyour Studio Neota logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="79987-217">After successful login, you will be signed-on tooyour Neota Logic Studio application.</span></span> <span data-ttu-id="79987-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="79987-218">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

<span data-ttu-id="79987-219">Další informace o hello přístupového panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="79987-219">For more information about hello Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="79987-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="79987-220">Additional resources</span></span>

* [<span data-ttu-id="79987-221">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="79987-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="79987-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="79987-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_203.png

