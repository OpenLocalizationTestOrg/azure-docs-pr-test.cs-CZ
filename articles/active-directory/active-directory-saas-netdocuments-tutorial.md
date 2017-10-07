---
title: 'Kurz: Azure Active Directory integrace s NetDocuments | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a NetDocuments."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ee9887553595a2492642aed4cb4abcd11d9cf599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a><span data-ttu-id="c0106-103">Kurz: Azure Active Directory integrace s NetDocuments</span><span class="sxs-lookup"><span data-stu-id="c0106-103">Tutorial: Azure Active Directory integration with NetDocuments</span></span>

<span data-ttu-id="c0106-104">V tomto kurzu zjistíte, jak toointegrate NetDocuments s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c0106-104">In this tutorial, you learn how toointegrate NetDocuments with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c0106-105">Integrace NetDocuments s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c0106-105">Integrating NetDocuments with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c0106-106">Můžete řídit ve službě Azure AD, který má přístup tooNetDocuments</span><span class="sxs-lookup"><span data-stu-id="c0106-106">You can control in Azure AD who has access tooNetDocuments</span></span>
- <span data-ttu-id="c0106-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNetDocuments (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0106-107">You can enable your users tooautomatically get signed-on tooNetDocuments (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c0106-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c0106-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c0106-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c0106-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0106-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0106-110">Prerequisites</span></span>

<span data-ttu-id="c0106-111">Integrace služby Azure AD s NetDocuments tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c0106-111">tooconfigure Azure AD integration with NetDocuments, you need hello following items:</span></span>

- <span data-ttu-id="c0106-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0106-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c0106-113">NetDocuments jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c0106-113">A NetDocuments single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c0106-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0106-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c0106-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c0106-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c0106-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c0106-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c0106-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c0106-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c0106-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c0106-118">Scenario description</span></span>
<span data-ttu-id="c0106-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0106-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c0106-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c0106-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c0106-121">Přidání NetDocuments z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c0106-121">Adding NetDocuments from hello gallery</span></span>
2. <span data-ttu-id="c0106-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0106-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netdocuments-from-hello-gallery"></a><span data-ttu-id="c0106-123">Přidání NetDocuments z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c0106-123">Adding NetDocuments from hello gallery</span></span>
<span data-ttu-id="c0106-124">tooconfigure hello integrace NetDocuments do Azure AD, je nutné tooadd NetDocuments hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c0106-124">tooconfigure hello integration of NetDocuments into Azure AD, you need tooadd NetDocuments from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c0106-125">**tooadd NetDocuments z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c0106-125">**tooadd NetDocuments from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c0106-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c0106-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c0106-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c0106-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c0106-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0106-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c0106-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0106-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c0106-133">Hello vyhledávacího pole zadejte **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="c0106-133">In hello search box, type **NetDocuments**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_search.png)

5. <span data-ttu-id="c0106-135">Na panelu výsledků hello vyberte **NetDocuments**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0106-135">In hello results panel, select **NetDocuments**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c0106-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0106-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c0106-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s NetDocuments podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c0106-138">In this section, you configure and test Azure AD single sign-on with NetDocuments based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c0106-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v NetDocuments je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c0106-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in NetDocuments is tooa user in Azure AD.</span></span> <span data-ttu-id="c0106-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v NetDocuments musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c0106-140">In other words, a link relationship between an Azure AD user and hello related user in NetDocuments needs toobe established.</span></span>

<span data-ttu-id="c0106-141">V NetDocuments, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c0106-141">In NetDocuments, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c0106-142">tooconfigure a testu Azure AD jednotné přihlašování s NetDocuments, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c0106-142">tooconfigure and test Azure AD single sign-on with NetDocuments, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c0106-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c0106-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c0106-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0106-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c0106-145">**[Vytvoření zkušebního uživatele NetDocuments](#creating-a-netdocuments-test-user)**  -toohave protějšek Britta Simon v NetDocuments, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c0106-145">**[Creating a NetDocuments test user](#creating-a-netdocuments-test-user)** - toohave a counterpart of Britta Simon in NetDocuments that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c0106-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0106-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c0106-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c0106-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c0106-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0106-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c0106-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="c0106-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your NetDocuments application.</span></span>

<span data-ttu-id="c0106-150">**tooconfigure Azure AD jednotné přihlašování s NetDocuments, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c0106-150">**tooconfigure Azure AD single sign-on with NetDocuments, perform hello following steps:**</span></span>

1. <span data-ttu-id="c0106-151">V portálu Azure, na hello hello **NetDocuments** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c0106-151">In hello Azure portal, on hello **NetDocuments** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c0106-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0106-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

3. <span data-ttu-id="c0106-155">Na hello **NetDocuments domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c0106-155">On hello **NetDocuments Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_url.png)

    <span data-ttu-id="c0106-157">a.</span><span class="sxs-lookup"><span data-stu-id="c0106-157">a.</span></span> <span data-ttu-id="c0106-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="c0106-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    <span data-ttu-id="c0106-159">b.</span><span class="sxs-lookup"><span data-stu-id="c0106-159">b.</span></span> <span data-ttu-id="c0106-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="c0106-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c0106-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c0106-161">These values are not real.</span></span> <span data-ttu-id="c0106-162">Tyto hodnoty aktualizujte hello skutečná adresa URL přihlašování a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c0106-162">Update these values with hello actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="c0106-163">Obraťte se na [tým podpory NetDocuments](https://support.netdocuments.com/hc/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c0106-163">Contact [NetDocuments support team](https://support.netdocuments.com/hc/) tooget these values.</span></span>
 
4. <span data-ttu-id="c0106-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c0106-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

5. <span data-ttu-id="c0106-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0106-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c0106-168">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="c0106-168">In a different web browser window, log into your NetDocuments company site as an administrator.</span></span>

7. <span data-ttu-id="c0106-169">Přejděte příliš**správce**.</span><span class="sxs-lookup"><span data-stu-id="c0106-169">Go too**Admin**.</span></span>

8. <span data-ttu-id="c0106-170">Klikněte na tlačítko **přidat a odebrat uživatele a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="c0106-170">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="c0106-171">![Úložiště](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "úložiště")</span><span class="sxs-lookup"><span data-stu-id="c0106-171">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

9. <span data-ttu-id="c0106-172">Klikněte na tlačítko **konfigurovat pokročilé možnosti ověřování**.</span><span class="sxs-lookup"><span data-stu-id="c0106-172">Click **Configure advanced authentication options**.</span></span>
    
    <span data-ttu-id="c0106-173">![Nakonfigurujte rozšířené možnosti ověřování](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "konfigurovat pokročilé možnosti ověřování")</span><span class="sxs-lookup"><span data-stu-id="c0106-173">![Configure advanced authentication options](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "Configure advanced authentication options")</span></span>

10. <span data-ttu-id="c0106-174">Na hello **federované Identity** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c0106-174">On hello **Federated Identity** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="c0106-175">![Federované Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "federovaný Identitty")</span><span class="sxs-lookup"><span data-stu-id="c0106-175">![Federated Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Federated Identitty")</span></span>
   
    <span data-ttu-id="c0106-176">a.</span><span class="sxs-lookup"><span data-stu-id="c0106-176">a.</span></span> <span data-ttu-id="c0106-177">Jako **federované identity serveru typ**, vyberte **Active Directory Federation Services**.</span><span class="sxs-lookup"><span data-stu-id="c0106-177">As **Federated identity server type**, select **Active Directory Federation Services**.</span></span>
   
    <span data-ttu-id="c0106-178">b.</span><span class="sxs-lookup"><span data-stu-id="c0106-178">b.</span></span> <span data-ttu-id="c0106-179">Klikněte na tlačítko **zvolte soubor**, tooupload hello stáhnout soubor metadat, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0106-179">Click **Choose file**, tooupload hello downloaded metadata file which you have downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="c0106-180">c.</span><span class="sxs-lookup"><span data-stu-id="c0106-180">c.</span></span> <span data-ttu-id="c0106-181">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0106-181">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="c0106-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c0106-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c0106-183">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c0106-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c0106-184">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c0106-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c0106-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0106-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="c0106-186">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c0106-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c0106-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c0106-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c0106-189">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c0106-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c0106-191">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c0106-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c0106-193">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c0106-193">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c0106-195">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c0106-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c0106-197">a.</span><span class="sxs-lookup"><span data-stu-id="c0106-197">a.</span></span> <span data-ttu-id="c0106-198">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c0106-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c0106-199">b.</span><span class="sxs-lookup"><span data-stu-id="c0106-199">b.</span></span> <span data-ttu-id="c0106-200">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c0106-200">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c0106-201">c.</span><span class="sxs-lookup"><span data-stu-id="c0106-201">c.</span></span> <span data-ttu-id="c0106-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c0106-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c0106-203">d.</span><span class="sxs-lookup"><span data-stu-id="c0106-203">d.</span></span> <span data-ttu-id="c0106-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c0106-204">Click **Create**.</span></span>
 
### <a name="creating-a-netdocuments-test-user"></a><span data-ttu-id="c0106-205">Vytvoření zkušebního uživatele NetDocuments</span><span class="sxs-lookup"><span data-stu-id="c0106-205">Creating a NetDocuments test user</span></span>

<span data-ttu-id="c0106-206">Uživatelé toolog tooenable Azure AD v tooNetDocuments, se musí být zřízená do NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="c0106-206">tooenable Azure AD users toolog in tooNetDocuments, they must be provisioned into NetDocuments.</span></span>  
<span data-ttu-id="c0106-207">V případě hello NetDocuments zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="c0106-207">In hello case of NetDocuments, provisioning is a manual task.</span></span>

<span data-ttu-id="c0106-208">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c0106-208">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="c0106-209">SING na tooyour **NetDocuments** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="c0106-209">Sing on tooyour **NetDocuments** company site as administrator.</span></span>

2. <span data-ttu-id="c0106-210">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="c0106-210">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="c0106-211">![Správce](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "správce")</span><span class="sxs-lookup"><span data-stu-id="c0106-211">![Admin](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "Admin")</span></span>

3. <span data-ttu-id="c0106-212">Klikněte na tlačítko **přidat a odebrat uživatele a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="c0106-212">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="c0106-213">![Úložiště](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "úložiště")</span><span class="sxs-lookup"><span data-stu-id="c0106-213">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

4. <span data-ttu-id="c0106-214">V hello **e-mailovou adresu** textovému poli, typ hello e-mailovou adresu platný účet služby Azure Active Directory tooprovision a pak klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c0106-214">In hello **Email Address** textbox, type hello email address of a valid Azure Active Directory account you want tooprovision, and then click **Add User**.</span></span>
   
    <span data-ttu-id="c0106-215">![E-mailová adresa](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "e-mailová adresa")</span><span class="sxs-lookup"><span data-stu-id="c0106-215">![Email Address](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "Email Address")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="c0106-216">Držitel účtu Azure Active Directory Hello dostane e-mail, který obsahuje účet odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="c0106-216">hello Azure Active Directory account holder will get an email that includes a link tooconfirm hello account before it becomes active.</span></span> <span data-ttu-id="c0106-217">Můžete použít všechny ostatní NetDocuments uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované NetDocuments tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c0106-217">You can use any other NetDocuments user account creation tools or APIs provided by NetDocuments tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c0106-218">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c0106-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c0106-219">V této části povolíte tak, že udělíte přístup tooNetDocuments toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c0106-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNetDocuments.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c0106-221">**tooassign Britta Simon tooNetDocuments, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c0106-221">**tooassign Britta Simon tooNetDocuments, perform hello following steps:**</span></span>

1. <span data-ttu-id="c0106-222">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0106-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c0106-224">V seznamu aplikace hello vyberte **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="c0106-224">In hello applications list, select **NetDocuments**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_app.png) 

3. <span data-ttu-id="c0106-226">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c0106-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c0106-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c0106-228">Click **Add** button.</span></span> <span data-ttu-id="c0106-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0106-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c0106-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c0106-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c0106-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0106-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c0106-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0106-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c0106-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c0106-234">Testing single sign-on</span></span>

<span data-ttu-id="c0106-235">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c0106-235">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c0106-236">Po kliknutí na tlačítko hello NetDocuments dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour NetDocuments aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0106-236">When you click hello NetDocuments tile in hello Access Panel, you should get automatically signed-on tooyour NetDocuments application.</span></span>
<span data-ttu-id="c0106-237">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c0106-237">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0106-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c0106-238">Additional resources</span></span>

* [<span data-ttu-id="c0106-239">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c0106-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c0106-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c0106-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_203.png

