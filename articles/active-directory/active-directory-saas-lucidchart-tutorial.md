---
title: 'Kurz: Azure Active Directory integrace s Lucidchart | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Lucidchart."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 774e5f423097650a3cae8e8ca13b2c65b8470736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a><span data-ttu-id="346f2-103">Kurz: Azure Active Directory integrace s Lucidchart</span><span class="sxs-lookup"><span data-stu-id="346f2-103">Tutorial: Azure Active Directory integration with Lucidchart</span></span>

<span data-ttu-id="346f2-104">V tomto kurzu zjistíte, jak toointegrate Lucidchart s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="346f2-104">In this tutorial, you learn how toointegrate Lucidchart with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="346f2-105">Integrace Lucidchart s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="346f2-105">Integrating Lucidchart with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="346f2-106">Můžete řídit ve službě Azure AD, který má přístup tooLucidchart</span><span class="sxs-lookup"><span data-stu-id="346f2-106">You can control in Azure AD who has access tooLucidchart</span></span>
- <span data-ttu-id="346f2-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLucidchart (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="346f2-107">You can enable your users tooautomatically get signed-on tooLucidchart (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="346f2-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="346f2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="346f2-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="346f2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="346f2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="346f2-110">Prerequisites</span></span>

<span data-ttu-id="346f2-111">Integrace služby Azure AD s Lucidchart tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="346f2-111">tooconfigure Azure AD integration with Lucidchart, you need hello following items:</span></span>

- <span data-ttu-id="346f2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="346f2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="346f2-113">Lucidchart jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="346f2-113">A Lucidchart single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="346f2-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="346f2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="346f2-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="346f2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="346f2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="346f2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="346f2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="346f2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="346f2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="346f2-118">Scenario description</span></span>
<span data-ttu-id="346f2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="346f2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="346f2-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="346f2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="346f2-121">Přidání Lucidchart z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="346f2-121">Adding Lucidchart from hello gallery</span></span>
2. <span data-ttu-id="346f2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="346f2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lucidchart-from-hello-gallery"></a><span data-ttu-id="346f2-123">Přidání Lucidchart z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="346f2-123">Adding Lucidchart from hello gallery</span></span>
<span data-ttu-id="346f2-124">tooconfigure hello integrace Lucidchart do Azure AD, je nutné tooadd Lucidchart hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="346f2-124">tooconfigure hello integration of Lucidchart into Azure AD, you need tooadd Lucidchart from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="346f2-125">**tooadd Lucidchart z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="346f2-125">**tooadd Lucidchart from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="346f2-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="346f2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="346f2-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="346f2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="346f2-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="346f2-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="346f2-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="346f2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="346f2-133">Hello vyhledávacího pole zadejte **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="346f2-133">In hello search box, type **Lucidchart**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_search.png)

5. <span data-ttu-id="346f2-135">Na panelu výsledků hello vyberte **Lucidchart**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="346f2-135">In hello results panel, select **Lucidchart**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="346f2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="346f2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="346f2-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lucidchart podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="346f2-138">In this section, you configure and test Azure AD single sign-on with Lucidchart based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="346f2-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Lucidchart je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="346f2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lucidchart is tooa user in Azure AD.</span></span> <span data-ttu-id="346f2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Lucidchart musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="346f2-140">In other words, a link relationship between an Azure AD user and hello related user in Lucidchart needs toobe established.</span></span>

<span data-ttu-id="346f2-141">V Lucidchart, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="346f2-141">In Lucidchart, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="346f2-142">tooconfigure a testu Azure AD jednotné přihlašování s Lucidchart, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="346f2-142">tooconfigure and test Azure AD single sign-on with Lucidchart, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="346f2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="346f2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="346f2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="346f2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="346f2-145">**[Vytvoření zkušebního uživatele Lucidchart](#creating-a-lucidchart-test-user)**  -toohave protějšek Britta Simon v Lucidchart, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="346f2-145">**[Creating a Lucidchart test user](#creating-a-lucidchart-test-user)** - toohave a counterpart of Britta Simon in Lucidchart that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="346f2-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="346f2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="346f2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="346f2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="346f2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="346f2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="346f2-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="346f2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lucidchart application.</span></span>

<span data-ttu-id="346f2-150">**tooconfigure Azure AD jednotné přihlašování s Lucidchart, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="346f2-150">**tooconfigure Azure AD single sign-on with Lucidchart, perform hello following steps:**</span></span>

1. <span data-ttu-id="346f2-151">V portálu Azure, na hello hello **Lucidchart** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="346f2-151">In hello Azure portal, on hello **Lucidchart** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="346f2-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="346f2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

3. <span data-ttu-id="346f2-155">Na hello **Lucidchart domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="346f2-155">On hello **Lucidchart Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_url.png)

    <span data-ttu-id="346f2-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://chart2.office.lucidchart.com/saml/sso/azure`</span><span class="sxs-lookup"><span data-stu-id="346f2-157">In hello **Sign-on URL** textbox, type a URL as: `https://chart2.office.lucidchart.com/saml/sso/azure`</span></span>

4. <span data-ttu-id="346f2-158">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="346f2-158">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

5. <span data-ttu-id="346f2-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="346f2-160">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="346f2-162">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="346f2-162">In a different web browser window, log into your Lucidchart company site as an administrator.</span></span>

7. <span data-ttu-id="346f2-163">V nabídce hello hello nahoře, klikněte na tlačítko **Team**.</span><span class="sxs-lookup"><span data-stu-id="346f2-163">In hello menu on hello top, click **Team**.</span></span>
   
    <span data-ttu-id="346f2-164">![Tým](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span><span class="sxs-lookup"><span data-stu-id="346f2-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span></span>

8. <span data-ttu-id="346f2-165">Klikněte na tlačítko **aplikace \> spravovat SAML**.</span><span class="sxs-lookup"><span data-stu-id="346f2-165">Click **Applications \> Manage SAML**.</span></span>
   
    <span data-ttu-id="346f2-166">![Spravovat SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "spravovat SAML")</span><span class="sxs-lookup"><span data-stu-id="346f2-166">![Manage SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "Manage SAML")</span></span>

9. <span data-ttu-id="346f2-167">Na hello **nastavení ověřování SAML** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="346f2-167">On hello **SAML Authentication Settings** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="346f2-168">a.</span><span class="sxs-lookup"><span data-stu-id="346f2-168">a.</span></span> <span data-ttu-id="346f2-169">Vyberte **povolit ověřování SAML**a potom klikněte na **volitelné**.</span><span class="sxs-lookup"><span data-stu-id="346f2-169">Select **Enable SAML Authentication**, and then click **Optional**.</span></span>

    <span data-ttu-id="346f2-170">![Nastavení ověřování SAML](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "nastavení ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="346f2-170">![SAML Authentication Settings](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML Authentication Settings")</span></span>
 
    <span data-ttu-id="346f2-171">b.</span><span class="sxs-lookup"><span data-stu-id="346f2-171">b.</span></span> <span data-ttu-id="346f2-172">V hello **domény** textovému poli, zadejte doménu a pak klikněte na tlačítko **změnit certifikát**.</span><span class="sxs-lookup"><span data-stu-id="346f2-172">In hello **Domain** textbox, type your domain, and then click **Change Certificate**.</span></span>

    <span data-ttu-id="346f2-173">![Změna certifikátu](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "změna certifikátu")</span><span class="sxs-lookup"><span data-stu-id="346f2-173">![Change Certificate](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "Change Certificate")</span></span>
 
    <span data-ttu-id="346f2-174">c.</span><span class="sxs-lookup"><span data-stu-id="346f2-174">c.</span></span> <span data-ttu-id="346f2-175">Otevřete soubor stažený metadat, hello kopírování obsahu a pak ji vložit do hello **nahrát Metadata** textové pole.</span><span class="sxs-lookup"><span data-stu-id="346f2-175">Open your downloaded metadata file, copy hello content, and then paste it into hello **Upload Metadata** textbox.</span></span>

    <span data-ttu-id="346f2-176">![Nahrát Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "nahrát metadat")</span><span class="sxs-lookup"><span data-stu-id="346f2-176">![Upload Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "Upload Metadata")</span></span>
 
    <span data-ttu-id="346f2-177">d.</span><span class="sxs-lookup"><span data-stu-id="346f2-177">d.</span></span> <span data-ttu-id="346f2-178">Vyberte **automaticky přidat nový tým uživatelé toohello**a potom klikněte na **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="346f2-178">Select **Automatically Add new users toohello team**, and then click **Save changes**.</span></span>

    <span data-ttu-id="346f2-179">![Uložit změny](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "uložit změny")</span><span class="sxs-lookup"><span data-stu-id="346f2-179">![Save Changes](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "Save Changes")</span></span>

> [!TIP]
> <span data-ttu-id="346f2-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="346f2-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="346f2-181">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="346f2-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="346f2-182">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="346f2-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="346f2-183">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="346f2-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="346f2-184">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="346f2-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="346f2-186">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="346f2-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="346f2-187">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="346f2-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="346f2-189">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="346f2-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="346f2-191">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="346f2-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="346f2-193">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="346f2-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="346f2-195">a.</span><span class="sxs-lookup"><span data-stu-id="346f2-195">a.</span></span> <span data-ttu-id="346f2-196">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="346f2-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="346f2-197">b.</span><span class="sxs-lookup"><span data-stu-id="346f2-197">b.</span></span> <span data-ttu-id="346f2-198">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="346f2-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="346f2-199">c.</span><span class="sxs-lookup"><span data-stu-id="346f2-199">c.</span></span> <span data-ttu-id="346f2-200">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="346f2-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="346f2-201">d.</span><span class="sxs-lookup"><span data-stu-id="346f2-201">d.</span></span> <span data-ttu-id="346f2-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="346f2-202">Click **Create**.</span></span>
 
### <a name="creating-a-lucidchart-test-user"></a><span data-ttu-id="346f2-203">Vytvoření zkušebního uživatele Lucidchart</span><span class="sxs-lookup"><span data-stu-id="346f2-203">Creating a Lucidchart test user</span></span>

<span data-ttu-id="346f2-204">Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooLucidchart.</span><span class="sxs-lookup"><span data-stu-id="346f2-204">There is no action item for you tooconfigure user provisioning tooLucidchart.</span></span>  <span data-ttu-id="346f2-205">Když se uživatel s přiřazenou pokusí toolog do Lucidchart pomocí hello přístupového panelu, Lucidchart ověří, zda existuje hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="346f2-205">When an assigned user tries toolog into Lucidchart using hello access panel, Lucidchart checks whether hello user exists.</span></span>  

<span data-ttu-id="346f2-206">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="346f2-206">If there is no user account available yet, it is automatically created by Lucidchart.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="346f2-207">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="346f2-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="346f2-208">V této části povolíte tak, že udělíte přístup tooLucidchart toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="346f2-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLucidchart.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="346f2-210">**tooassign Britta Simon tooLucidchart, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="346f2-210">**tooassign Britta Simon tooLucidchart, perform hello following steps:**</span></span>

1. <span data-ttu-id="346f2-211">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="346f2-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="346f2-213">V seznamu aplikace hello vyberte **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="346f2-213">In hello applications list, select **Lucidchart**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_app.png) 

3. <span data-ttu-id="346f2-215">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="346f2-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="346f2-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="346f2-217">Click **Add** button.</span></span> <span data-ttu-id="346f2-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="346f2-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="346f2-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="346f2-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="346f2-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="346f2-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="346f2-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="346f2-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="346f2-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="346f2-223">Testing single sign-on</span></span>

<span data-ttu-id="346f2-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="346f2-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="346f2-225">Když kliknete na dlaždici Lucidchart hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Lucidchart aplikace.</span><span class="sxs-lookup"><span data-stu-id="346f2-225">When you click hello Lucidchart tile in hello Access Panel, you should get automatically signed-on tooyour Lucidchart application.</span></span>
<span data-ttu-id="346f2-226">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="346f2-226">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="346f2-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="346f2-227">Additional resources</span></span>

* [<span data-ttu-id="346f2-228">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="346f2-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="346f2-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="346f2-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_203.png

