---
title: 'Kurz: Azure Active Directory integrace s Bime | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Bime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 1213725028dd8ce90f22fa6e9c50ffabebc8f3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a><span data-ttu-id="c9457-103">Kurz: Azure Active Directory integrace s Bime</span><span class="sxs-lookup"><span data-stu-id="c9457-103">Tutorial: Azure Active Directory integration with Bime</span></span>

<span data-ttu-id="c9457-104">V tomto kurzu zjistíte, jak toointegrate Bime s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c9457-104">In this tutorial, you learn how toointegrate Bime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c9457-105">Integrace Bime s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c9457-105">Integrating Bime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c9457-106">Můžete řídit ve službě Azure AD, který má přístup tooBime</span><span class="sxs-lookup"><span data-stu-id="c9457-106">You can control in Azure AD who has access tooBime</span></span>
- <span data-ttu-id="c9457-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBime (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9457-107">You can enable your users tooautomatically get signed-on tooBime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c9457-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c9457-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c9457-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9457-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9457-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c9457-110">Prerequisites</span></span>

<span data-ttu-id="c9457-111">Integrace služby Azure AD s Bime tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c9457-111">tooconfigure Azure AD integration with Bime, you need hello following items:</span></span>

- <span data-ttu-id="c9457-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9457-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9457-113">Bime jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c9457-113">A Bime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c9457-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c9457-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c9457-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c9457-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9457-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c9457-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9457-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9457-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9457-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c9457-118">Scenario description</span></span>
<span data-ttu-id="c9457-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c9457-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c9457-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c9457-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9457-121">Přidání Bime z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c9457-121">Adding Bime from hello gallery</span></span>
2. <span data-ttu-id="c9457-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9457-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bime-from-hello-gallery"></a><span data-ttu-id="c9457-123">Přidání Bime z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c9457-123">Adding Bime from hello gallery</span></span>
<span data-ttu-id="c9457-124">tooconfigure hello integrace Bime do Azure AD, je nutné tooadd Bime hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c9457-124">tooconfigure hello integration of Bime into Azure AD, you need tooadd Bime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c9457-125">**tooadd Bime z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c9457-125">**tooadd Bime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9457-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c9457-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c9457-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c9457-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c9457-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c9457-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c9457-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9457-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c9457-133">Hello vyhledávacího pole zadejte **Bime**.</span><span class="sxs-lookup"><span data-stu-id="c9457-133">In hello search box, type **Bime**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/tutorial_bime_search.png)

5. <span data-ttu-id="c9457-135">Na panelu výsledků hello vyberte **Bime**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9457-135">In hello results panel, select **Bime**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/tutorial_bime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c9457-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9457-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c9457-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Bime podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c9457-138">In this section, you configure and test Azure AD single sign-on with Bime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c9457-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Bime je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9457-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bime is tooa user in Azure AD.</span></span> <span data-ttu-id="c9457-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Bime musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c9457-140">In other words, a link relationship between an Azure AD user and hello related user in Bime needs toobe established.</span></span>

<span data-ttu-id="c9457-141">V Bime, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c9457-141">In Bime, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c9457-142">tooconfigure a testu Azure AD jednotné přihlašování s Bime, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c9457-142">tooconfigure and test Azure AD single sign-on with Bime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c9457-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c9457-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c9457-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9457-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9457-145">**[Vytvoření zkušebního uživatele Bime](#creating-a-bime-test-user)**  -toohave protějšek Britta Simon v Bime, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c9457-145">**[Creating a Bime test user](#creating-a-bime-test-user)** - toohave a counterpart of Britta Simon in Bime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c9457-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c9457-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9457-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c9457-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c9457-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9457-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c9457-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Bime.</span><span class="sxs-lookup"><span data-stu-id="c9457-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bime application.</span></span>

<span data-ttu-id="c9457-150">**tooconfigure Azure AD jednotné přihlašování s Bime, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c9457-150">**tooconfigure Azure AD single sign-on with Bime, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9457-151">V portálu Azure, na hello hello **Bime** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c9457-151">In hello Azure portal, on hello **Bime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c9457-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c9457-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_samlbase.png)

3. <span data-ttu-id="c9457-155">Na hello **Bime domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c9457-155">On hello **Bime Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_url.png)

    <span data-ttu-id="c9457-157">a.</span><span class="sxs-lookup"><span data-stu-id="c9457-157">a.</span></span> <span data-ttu-id="c9457-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="c9457-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    <span data-ttu-id="c9457-159">b.</span><span class="sxs-lookup"><span data-stu-id="c9457-159">b.</span></span> <span data-ttu-id="c9457-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="c9457-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c9457-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c9457-161">These values are not real.</span></span> <span data-ttu-id="c9457-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c9457-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c9457-163">Obraťte se na [tým podpory Bime klienta](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c9457-163">Contact [Bime Client support team](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) tooget these values.</span></span> 
 
4. <span data-ttu-id="c9457-164">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnotu z hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c9457-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value from hello certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_certificate.png) 

5. <span data-ttu-id="c9457-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9457-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c9457-168">Na hello **Bime konfigurace** klikněte na tlačítko **konfigurace Bime** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c9457-168">On hello **Bime Configuration** section, click **Configure Bime** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c9457-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c9457-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_configure.png) 

7. <span data-ttu-id="c9457-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Bime.</span><span class="sxs-lookup"><span data-stu-id="c9457-171">In a different web browser window, log into your Bime company site as an administrator.</span></span>

8. <span data-ttu-id="c9457-172">V panelu nástrojů hello, klikněte na **správce**a potom **účet**.</span><span class="sxs-lookup"><span data-stu-id="c9457-172">In hello toolbar, click **Admin**, and then **Account**.</span></span>
   
    <span data-ttu-id="c9457-173">![Správce](./media/active-directory-saas-bime-tutorial/ic775558.png "správce")</span><span class="sxs-lookup"><span data-stu-id="c9457-173">![Admin](./media/active-directory-saas-bime-tutorial/ic775558.png "Admin")</span></span>

9. <span data-ttu-id="c9457-174">Na stránce konfigurace účtu hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c9457-174">On hello account configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="c9457-175">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/ic775559.png "nakonfigurovat jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="c9457-175">![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/ic775559.png "Configure Single Sign-On")</span></span>
   
    <span data-ttu-id="c9457-176">a.</span><span class="sxs-lookup"><span data-stu-id="c9457-176">a.</span></span> <span data-ttu-id="c9457-177">Vyberte **ověřování povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="c9457-177">Select **Enable SAML authentication**.</span></span>

    <span data-ttu-id="c9457-178">b.</span><span class="sxs-lookup"><span data-stu-id="c9457-178">b.</span></span> <span data-ttu-id="c9457-179">V hello **vzdálené adresy URL pro přihlášení** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c9457-179">In hello **Remote Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="c9457-180">c.</span><span class="sxs-lookup"><span data-stu-id="c9457-180">c.</span></span>  <span data-ttu-id="c9457-181">Vložení hello **kryptografický otisk** hodnotu z portálu Azure do hello **otisků prstů certifikátů** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c9457-181">Paste hello **Thumbprint** value from Azure portal into hello **Certificate Fingerprint** textbox.</span></span>       
   
    <span data-ttu-id="c9457-182">d.</span><span class="sxs-lookup"><span data-stu-id="c9457-182">d.</span></span> <span data-ttu-id="c9457-183">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c9457-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="c9457-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c9457-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c9457-185">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c9457-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c9457-186">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c9457-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c9457-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9457-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="c9457-188">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c9457-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c9457-190">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c9457-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9457-191">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c9457-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c9457-193">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c9457-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c9457-195">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c9457-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c9457-197">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c9457-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c9457-199">a.</span><span class="sxs-lookup"><span data-stu-id="c9457-199">a.</span></span> <span data-ttu-id="c9457-200">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9457-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9457-201">b.</span><span class="sxs-lookup"><span data-stu-id="c9457-201">b.</span></span> <span data-ttu-id="c9457-202">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c9457-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c9457-203">c.</span><span class="sxs-lookup"><span data-stu-id="c9457-203">c.</span></span> <span data-ttu-id="c9457-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c9457-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c9457-205">d.</span><span class="sxs-lookup"><span data-stu-id="c9457-205">d.</span></span> <span data-ttu-id="c9457-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c9457-206">Click **Create**.</span></span>
 
### <a name="creating-a-bime-test-user"></a><span data-ttu-id="c9457-207">Vytvoření zkušebního uživatele Bime</span><span class="sxs-lookup"><span data-stu-id="c9457-207">Creating a Bime test user</span></span>

<span data-ttu-id="c9457-208">V pořadí tooenable Azure AD Uživatelé toolog v tooBime musí být zřízená do Bime.</span><span class="sxs-lookup"><span data-stu-id="c9457-208">In order tooenable Azure AD users toolog in tooBime, they must be provisioned into Bime.</span></span> <span data-ttu-id="c9457-209">V případě hello Bime zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="c9457-209">In hello case of Bime, provisioning is a manual task.</span></span>

<span data-ttu-id="c9457-210">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c9457-210">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9457-211">Přihlaste se tooyour **Bime** klienta.</span><span class="sxs-lookup"><span data-stu-id="c9457-211">Log in tooyour **Bime** tenant.</span></span>

2. <span data-ttu-id="c9457-212">V panelu nástrojů hello, klikněte na **správce**a potom **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c9457-212">In hello toolbar, click **Admin**, and then **Users**.</span></span>
   
    <span data-ttu-id="c9457-213">![Správce](./media/active-directory-saas-bime-tutorial/ic775561.png "správce")</span><span class="sxs-lookup"><span data-stu-id="c9457-213">![Admin](./media/active-directory-saas-bime-tutorial/ic775561.png "Admin")</span></span>

3. <span data-ttu-id="c9457-214">V hello **seznamu uživatelů**, klikněte na tlačítko **přidat nové uživatele** ("+").</span><span class="sxs-lookup"><span data-stu-id="c9457-214">In hello **Users List**, click **Add New User** (“+”).</span></span>
   
    <span data-ttu-id="c9457-215">![Uživatelé](./media/active-directory-saas-bime-tutorial/ic775562.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="c9457-215">![Users](./media/active-directory-saas-bime-tutorial/ic775562.png "Users")</span></span>

4. <span data-ttu-id="c9457-216">Na hello **podrobné informace o uživateli** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c9457-216">On hello **User Details** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="c9457-217">![Podrobné informace o uživateli](./media/active-directory-saas-bime-tutorial/ic775563.png "podrobné informace o uživateli")</span><span class="sxs-lookup"><span data-stu-id="c9457-217">![User Details](./media/active-directory-saas-bime-tutorial/ic775563.png "User Details")</span></span>
   
    <span data-ttu-id="c9457-218">a.</span><span class="sxs-lookup"><span data-stu-id="c9457-218">a.</span></span> <span data-ttu-id="c9457-219">V hello **křestní jméno** textovému poli, zadejte hello křestní jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="c9457-219">In hello **First name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="c9457-220">b.</span><span class="sxs-lookup"><span data-stu-id="c9457-220">b.</span></span> <span data-ttu-id="c9457-221">V hello **příjmení** textovému poli, zadejte příjmení uživatele jako hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="c9457-221">In hello **Last name** textbox, enter hello last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="c9457-222">c.</span><span class="sxs-lookup"><span data-stu-id="c9457-222">c.</span></span> <span data-ttu-id="c9457-223">V hello **e-mailu** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="c9457-223">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="c9457-224">d.</span><span class="sxs-lookup"><span data-stu-id="c9457-224">d.</span></span> <span data-ttu-id="c9457-225">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c9457-225">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="c9457-226">Můžete použít všechny ostatní Bime uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Bime tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c9457-226">You can use any other Bime user account creation tools or APIs provided by Bime tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c9457-227">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c9457-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c9457-228">V této části povolíte tak, že udělíte přístup tooBime toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c9457-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBime.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c9457-230">**tooassign Britta Simon tooBime, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c9457-230">**tooassign Britta Simon tooBime, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9457-231">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c9457-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c9457-233">V seznamu aplikace hello vyberte **Bime**.</span><span class="sxs-lookup"><span data-stu-id="c9457-233">In hello applications list, select **Bime**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bime-tutorial/tutorial_bime_app.png) 

3. <span data-ttu-id="c9457-235">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c9457-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c9457-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9457-237">Click **Add** button.</span></span> <span data-ttu-id="c9457-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9457-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c9457-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c9457-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c9457-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9457-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9457-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9457-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c9457-243">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9457-243">Testing single sign-on</span></span>

<span data-ttu-id="c9457-244">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c9457-244">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c9457-245">Když kliknete na dlaždici Bime hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Bime aplikace.</span><span class="sxs-lookup"><span data-stu-id="c9457-245">When you click hello Bime tile in hello Access Panel, you should get automatically signed-on tooyour Bime application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9457-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c9457-246">Additional resources</span></span>

* [<span data-ttu-id="c9457-247">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c9457-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9457-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c9457-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bime-tutorial/tutorial_general_203.png

