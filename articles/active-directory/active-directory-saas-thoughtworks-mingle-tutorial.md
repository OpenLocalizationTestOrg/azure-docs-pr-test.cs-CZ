---
title: 'Kurz: Azure Active Directory integrace s Thoughtworks Mingle | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Thoughtworks Mingle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c17f8e13d2db3de7d228d9b27128d134f98d6cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a><span data-ttu-id="147c8-103">Kurz: Azure Active Directory integrace s Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="147c8-103">Tutorial: Azure Active Directory integration with Thoughtworks Mingle</span></span>

<span data-ttu-id="147c8-104">V tomto kurzu zjistíte, jak toointegrate Thoughtworks Mingle s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="147c8-104">In this tutorial, you learn how toointegrate Thoughtworks Mingle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="147c8-105">Integrace Thoughtworks Mingle s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="147c8-105">Integrating Thoughtworks Mingle with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="147c8-106">Můžete řídit ve službě Azure AD, který má přístup tooThoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="147c8-106">You can control in Azure AD who has access tooThoughtworks Mingle</span></span>
- <span data-ttu-id="147c8-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooThoughtworks Mingle (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="147c8-107">You can enable your users tooautomatically get signed-on tooThoughtworks Mingle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="147c8-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="147c8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="147c8-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="147c8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="147c8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="147c8-110">Prerequisites</span></span>

<span data-ttu-id="147c8-111">Integrace služby Azure AD s Thoughtworks Mingle tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="147c8-111">tooconfigure Azure AD integration with Thoughtworks Mingle, you need hello following items:</span></span>

- <span data-ttu-id="147c8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="147c8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="147c8-113">Thoughtworks Mingle jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="147c8-113">A Thoughtworks Mingle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="147c8-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="147c8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="147c8-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="147c8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="147c8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="147c8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="147c8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="147c8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="147c8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="147c8-118">Scenario description</span></span>
<span data-ttu-id="147c8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="147c8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="147c8-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="147c8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="147c8-121">Přidání Thoughtworks Mingle z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="147c8-121">Adding Thoughtworks Mingle from hello gallery</span></span>
2. <span data-ttu-id="147c8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="147c8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thoughtworks-mingle-from-hello-gallery"></a><span data-ttu-id="147c8-123">Přidání Thoughtworks Mingle z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="147c8-123">Adding Thoughtworks Mingle from hello gallery</span></span>
<span data-ttu-id="147c8-124">tooconfigure hello integrace Thoughtworks Mingle do Azure AD, je nutné tooadd Thoughtworks Mingle hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="147c8-124">tooconfigure hello integration of Thoughtworks Mingle into Azure AD, you need tooadd Thoughtworks Mingle from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="147c8-125">**tooadd Thoughtworks Mingle z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="147c8-125">**tooadd Thoughtworks Mingle from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="147c8-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="147c8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="147c8-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="147c8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="147c8-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="147c8-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="147c8-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="147c8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="147c8-133">Hello vyhledávacího pole zadejte **Thoughtworks Mingle**, vyberte **Thoughtworks Mingle** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="147c8-133">In hello search box, type **Thoughtworks Mingle**, select **Thoughtworks Mingle** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Thoughtworks Mingle v seznamu výsledků hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="147c8-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="147c8-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="147c8-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Thoughtworks Mingle podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="147c8-136">In this section, you configure and test Azure AD single sign-on with Thoughtworks Mingle based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="147c8-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Thoughtworks Mingle je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="147c8-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Thoughtworks Mingle is tooa user in Azure AD.</span></span> <span data-ttu-id="147c8-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Thoughtworks Mingle musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="147c8-138">In other words, a link relationship between an Azure AD user and hello related user in Thoughtworks Mingle needs toobe established.</span></span>

<span data-ttu-id="147c8-139">V Thoughtworks Mingle, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="147c8-139">In Thoughtworks Mingle, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="147c8-140">tooconfigure a testu Azure AD jednotné přihlašování s Thoughtworks Mingle, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="147c8-140">tooconfigure and test Azure AD single sign-on with Thoughtworks Mingle, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="147c8-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="147c8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="147c8-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="147c8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="147c8-143">**[Vytvoření zkušebního uživatele Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)**  -toohave protějšek Britta Simon v Thoughtworks Mingle, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="147c8-143">**[Create a Thoughtworks Mingle test user](#create-a-thoughtworks-mingle-test-user)** - toohave a counterpart of Britta Simon in Thoughtworks Mingle that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="147c8-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="147c8-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="147c8-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="147c8-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="147c8-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="147c8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="147c8-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Thoughtworks Mingle.</span><span class="sxs-lookup"><span data-stu-id="147c8-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Thoughtworks Mingle application.</span></span>

<span data-ttu-id="147c8-148">**tooconfigure Azure AD jednotné přihlašování s Thoughtworks Mingle, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="147c8-148">**tooconfigure Azure AD single sign-on with Thoughtworks Mingle, perform hello following steps:**</span></span>

1. <span data-ttu-id="147c8-149">V portálu Azure, na hello hello **Thoughtworks Mingle** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="147c8-149">In hello Azure portal, on hello **Thoughtworks Mingle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="147c8-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="147c8-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. <span data-ttu-id="147c8-153">Na hello **Thoughtworks Mingle domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="147c8-153">On hello **Thoughtworks Mingle Domain and URLs** section, perform hello following steps:</span></span>

    ![Thoughtworks Mingle domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    <span data-ttu-id="147c8-155">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.mingle.thoughtworks.com`</span><span class="sxs-lookup"><span data-stu-id="147c8-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.mingle.thoughtworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="147c8-156">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="147c8-156">hello value is not real.</span></span> <span data-ttu-id="147c8-157">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="147c8-157">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="147c8-158">Obraťte se na [tým podpory klienta Mingle Thoughtworks](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="147c8-158">Contact [Thoughtworks Mingle Client support team](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello value.</span></span> 
 
4. <span data-ttu-id="147c8-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="147c8-159">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. <span data-ttu-id="147c8-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="147c8-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="147c8-163">Přihlaste se tooyour **Thoughtworks Mingle** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="147c8-163">Log in tooyour **Thoughtworks Mingle** company site as administrator.</span></span>

7. <span data-ttu-id="147c8-164">Klikněte na tlačítko hello **správce** kartě a potom klikněte na **Konfigurace jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="147c8-164">Click hello **Admin** tab, and then, click **SSO Config**.</span></span>
   
    <span data-ttu-id="147c8-165">![Karta správce](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "Konfigurace jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="147c8-165">![Admin tab](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")</span></span>

8. <span data-ttu-id="147c8-166">V hello **Konfigurace jednotného přihlašování k** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="147c8-166">In hello **SSO Config** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="147c8-167">![Konfigurace jednotného přihlašování k](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "Konfigurace jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="147c8-167">![SSO Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")</span></span>
    
    <span data-ttu-id="147c8-168">a.</span><span class="sxs-lookup"><span data-stu-id="147c8-168">a.</span></span> <span data-ttu-id="147c8-169">Soubor metadat hello tooupload, klikněte na tlačítko **zvolte soubor**.</span><span class="sxs-lookup"><span data-stu-id="147c8-169">tooupload hello metadata file, click **Choose file**.</span></span> 

    <span data-ttu-id="147c8-170">b.</span><span class="sxs-lookup"><span data-stu-id="147c8-170">b.</span></span> <span data-ttu-id="147c8-171">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="147c8-171">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="147c8-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="147c8-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="147c8-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="147c8-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="147c8-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="147c8-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="147c8-175">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="147c8-175">Create an Azure AD test user</span></span>
<span data-ttu-id="147c8-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="147c8-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="147c8-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="147c8-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="147c8-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="147c8-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="147c8-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="147c8-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="147c8-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="147c8-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="147c8-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="147c8-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="147c8-187">a.</span><span class="sxs-lookup"><span data-stu-id="147c8-187">a.</span></span> <span data-ttu-id="147c8-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="147c8-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="147c8-189">b.</span><span class="sxs-lookup"><span data-stu-id="147c8-189">b.</span></span> <span data-ttu-id="147c8-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="147c8-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="147c8-191">c.</span><span class="sxs-lookup"><span data-stu-id="147c8-191">c.</span></span> <span data-ttu-id="147c8-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="147c8-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="147c8-193">d.</span><span class="sxs-lookup"><span data-stu-id="147c8-193">d.</span></span> <span data-ttu-id="147c8-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="147c8-194">Click **Create**.</span></span>
 
### <a name="create-a-thoughtworks-mingle-test-user"></a><span data-ttu-id="147c8-195">Vytvoření zkušebního uživatele Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="147c8-195">Create a Thoughtworks Mingle test user</span></span>

<span data-ttu-id="147c8-196">Pro Azure AD Uživatelé toobe možné toosign v musí být zřízená toohello Thoughtworks Mingle aplikace pomocí jejich názvy uživatele Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="147c8-196">For Azure AD users toobe able toosign in, they must be provisioned toohello Thoughtworks Mingle application using their Azure Active Directory user names.</span></span> <span data-ttu-id="147c8-197">V případě hello Thoughtworks Mingle zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="147c8-197">In hello case of Thoughtworks Mingle, provisioning is a manual task.</span></span>

<span data-ttu-id="147c8-198">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="147c8-198">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="147c8-199">Přihlaste se tooyour Thoughtworks Mingle společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="147c8-199">Log in tooyour Thoughtworks Mingle company site as administrator.</span></span>

2. <span data-ttu-id="147c8-200">Klikněte na tlačítko **profil**.</span><span class="sxs-lookup"><span data-stu-id="147c8-200">Click **Profile**.</span></span>
   
    <span data-ttu-id="147c8-201">![Svůj první projekt](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "prvního projektu")</span><span class="sxs-lookup"><span data-stu-id="147c8-201">![Your First Project](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "Your First Project")</span></span>

3. <span data-ttu-id="147c8-202">Klikněte na tlačítko hello **správce** a pak klikněte **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="147c8-202">Click hello **Admin** tab, and then click **Users**.</span></span>
   
    <span data-ttu-id="147c8-203">![Uživatelé](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="147c8-203">![Users](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "Users")</span></span>

4. <span data-ttu-id="147c8-204">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="147c8-204">Click **New User**.</span></span>
   
    <span data-ttu-id="147c8-205">![Nový uživatel](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="147c8-205">![New User](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "New User")</span></span>

5. <span data-ttu-id="147c8-206">Na hello **nového uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="147c8-206">On hello **New User** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="147c8-207">![Dialogové okno Nový uživatel](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="147c8-207">![New User dialog](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "New User")</span></span>  
 
    <span data-ttu-id="147c8-208">a.</span><span class="sxs-lookup"><span data-stu-id="147c8-208">a.</span></span> <span data-ttu-id="147c8-209">Typ hello **přihlašovací jméno**, **zobrazovaný název**, **zvolte heslo**, **potvrzení hesla** platný Azure AD účet chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="147c8-209">Type hello **Sign-in name**, **Display name**, **Choose password**, **Confirm password** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span> 

    <span data-ttu-id="147c8-210">b.</span><span class="sxs-lookup"><span data-stu-id="147c8-210">b.</span></span> <span data-ttu-id="147c8-211">Jako **typ uživatele**, vyberte **úplné uživatelské**.</span><span class="sxs-lookup"><span data-stu-id="147c8-211">As **User type**, select **Full user**.</span></span>

    <span data-ttu-id="147c8-212">c.</span><span class="sxs-lookup"><span data-stu-id="147c8-212">c.</span></span> <span data-ttu-id="147c8-213">Klikněte na tlačítko **vytvořit tento profil**.</span><span class="sxs-lookup"><span data-stu-id="147c8-213">Click **Create This Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="147c8-214">Můžete použít všechny ostatní Thoughtworks Mingle uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Thoughtworks Mingle tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="147c8-214">You can use any other Thoughtworks Mingle user account creation tools or APIs provided by Thoughtworks Mingle tooprovision AAD user accounts.</span></span>
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="147c8-215">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="147c8-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="147c8-216">V této části povolíte tak, že udělíte přístup tooThoughtworks Mingle toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="147c8-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooThoughtworks Mingle.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="147c8-218">**tooassign Britta Simon tooThoughtworks Mingle, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="147c8-218">**tooassign Britta Simon tooThoughtworks Mingle, perform hello following steps:**</span></span>

1. <span data-ttu-id="147c8-219">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="147c8-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="147c8-221">V seznamu aplikace hello vyberte **Thoughtworks Mingle**.</span><span class="sxs-lookup"><span data-stu-id="147c8-221">In hello applications list, select **Thoughtworks Mingle**.</span></span>

    ![v seznamu aplikace hello odkaz Thoughtworks Mingle Hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. <span data-ttu-id="147c8-223">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="147c8-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="147c8-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="147c8-225">Click **Add** button.</span></span> <span data-ttu-id="147c8-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="147c8-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="147c8-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="147c8-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="147c8-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="147c8-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="147c8-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="147c8-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="147c8-231">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="147c8-231">Test single sign-on</span></span>

<span data-ttu-id="147c8-232">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="147c8-232">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="147c8-233">Po kliknutí na tlačítko hello Thoughtworks Mingle dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Thoughtworks Mingle aplikace.</span><span class="sxs-lookup"><span data-stu-id="147c8-233">When you click hello Thoughtworks Mingle tile in hello Access Panel, you should get automatically signed-on tooyour Thoughtworks Mingle application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="147c8-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="147c8-234">Additional resources</span></span>

* [<span data-ttu-id="147c8-235">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="147c8-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="147c8-236">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="147c8-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

