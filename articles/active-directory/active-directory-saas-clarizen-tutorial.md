---
title: 'Kurz: Azure Active Directory integrace s Clarizen | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Clarizen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: f24ccda3b90e5df9a203a444dfda905043b30276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a><span data-ttu-id="9599c-103">Kurz: Azure Active Directory integrace s Clarizen</span><span class="sxs-lookup"><span data-stu-id="9599c-103">Tutorial: Azure Active Directory integration with Clarizen</span></span>

<span data-ttu-id="9599c-104">V tomto kurzu zjistíte, jak Azure Active Directory (Azure AD) s Clarizen toointegrate.</span><span class="sxs-lookup"><span data-stu-id="9599c-104">In this tutorial, you learn how toointegrate Azure Active Directory (Azure AD) with Clarizen.</span></span> <span data-ttu-id="9599c-105">Tato integrace nabízí hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9599c-105">This integration gives you hello following benefits:</span></span>

- <span data-ttu-id="9599c-106">Můžete ovládat, ve službě Azure AD, který má přístup tooClarizen.</span><span class="sxs-lookup"><span data-stu-id="9599c-106">You can control, in Azure AD, who has access tooClarizen.</span></span>
- <span data-ttu-id="9599c-107">Můžete povolit vaši uživatelé toobe automaticky přihlášeni v tooClarizen (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9599c-107">You can enable your users toobe automatically signed in tooClarizen (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9599c-108">Můžete spravovat vaše účty v jednom centrálním místě, hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9599c-108">You can manage your accounts in one central location, hello Azure portal.</span></span>

<span data-ttu-id="9599c-109">Hello scénář v tomto kurzu se skládá z dvě hlavní úlohy:</span><span class="sxs-lookup"><span data-stu-id="9599c-109">hello scenario in this tutorial consists of two main tasks:</span></span>

1. <span data-ttu-id="9599c-110">Přidejte Clarizen z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="9599c-110">Add Clarizen from hello gallery.</span></span>
2. <span data-ttu-id="9599c-111">Konfigurace a otestování Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9599c-111">Configure and test Azure AD single sign-on.</span></span>

<span data-ttu-id="9599c-112">Pokud chcete další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9599c-112">If you want more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9599c-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9599c-113">Prerequisites</span></span>
<span data-ttu-id="9599c-114">Integrace služby Azure AD s Clarizen tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9599c-114">tooconfigure Azure AD integration with Clarizen, you need hello following items:</span></span>

- <span data-ttu-id="9599c-115">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9599c-115">An Azure AD subscription</span></span>
- <span data-ttu-id="9599c-116">Clarizen odběr, který je povolený pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9599c-116">A Clarizen subscription that's enabled for single sign-on</span></span>

<span data-ttu-id="9599c-117">tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="9599c-117">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="9599c-118">Testování Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9599c-118">Test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9599c-119">Nepoužívejte produkční prostředí, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="9599c-119">Don't use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9599c-120">Pokud nemáte testovacím prostředí s Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9599c-120">If you don't have an Azure AD test environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="add-clarizen-from-hello-gallery"></a><span data-ttu-id="9599c-121">Přidat Clarizen z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9599c-121">Add Clarizen from hello gallery</span></span>
<span data-ttu-id="9599c-122">tooconfigure hello integrace Clarizen do Azure AD, přidejte Clarizen hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9599c-122">tooconfigure hello integration of Clarizen into Azure AD, add Clarizen from hello gallery tooyour list of managed SaaS apps.</span></span>

1. <span data-ttu-id="9599c-123">V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, klikněte na hello **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9599c-123">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory – ikona][1]

2. <span data-ttu-id="9599c-125">Klikněte na tlačítko **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9599c-125">Click **Enterprise applications**.</span></span> <span data-ttu-id="9599c-126">Pak klikněte na tlačítko **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9599c-126">Then click **All applications**.</span></span>

    ![Kliknutím na tlačítko "Podnikové aplikace" a "Všechny aplikace"][2]

3. <span data-ttu-id="9599c-128">Klikněte na tlačítko hello **přidat** tlačítka v horní části hello dialogového okna hello.</span><span class="sxs-lookup"><span data-stu-id="9599c-128">Click hello **Add** button at hello top of hello dialog box.</span></span>

    ![tlačítko "Přidat" Hello][3]

4. <span data-ttu-id="9599c-130">Hello vyhledávacího pole zadejte **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="9599c-130">In hello search box, type **Clarizen**.</span></span>

    ![Zadejte "Clarizen" hello vyhledávacího pole](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. <span data-ttu-id="9599c-132">V podokně výsledků hello, vyberte **Clarizen**a potom klikněte na **přidat** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9599c-132">In hello results pane, select **Clarizen**, and then click **Add** tooadd hello application.</span></span>

    ![Výběr Clarizen v podokně výsledků hello](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9599c-134">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9599c-134">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9599c-135">V následujících částech hello nakonfigurovat a otestovat Azure AD jednotné přihlašování s Clarizen na základě hello testovací uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9599c-135">In hello following sections, you configure and test Azure AD single sign-on with Clarizen based on hello test user Britta Simon.</span></span>

<span data-ttu-id="9599c-136">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Clarizen je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9599c-136">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Clarizen is tooa user in Azure AD.</span></span> <span data-ttu-id="9599c-137">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Clarizen musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9599c-137">In other words, a link relationship between an Azure AD user and hello related user in Clarizen needs toobe established.</span></span> <span data-ttu-id="9599c-138">Vytvořit tento odkaz vztah přiřazením hello hodnotu **uživatelské jméno** ve službě Azure AD jako hodnota hello **uživatelské jméno** v Clarizen.</span><span class="sxs-lookup"><span data-stu-id="9599c-138">You establish this link relationship by assigning hello value of **user name** in Azure AD as hello value of **Username** in Clarizen.</span></span>

<span data-ttu-id="9599c-139">tooconfigure a testování Azure AD jednotné přihlašování s Clarizen, dokončení hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="9599c-139">tooconfigure and test Azure AD single sign-on with Clarizen, complete hello following building blocks:</span></span>

1. <span data-ttu-id="9599c-140">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9599c-140">**[Configure Azure AD single sign-on](#configure-azure-ad-single-sign-on)** tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9599c-141">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9599c-141">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9599c-142">**[Vytvoření zkušebního uživatele Clarizen](#create-a-clarizen-test-user)**  toohave protějšek Britta Simon v Clarizen, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9599c-142">**[Create a Clarizen test user](#create-a-clarizen-test-user)** toohave a counterpart of Britta Simon in Clarizen that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9599c-143">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9599c-143">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9599c-144">**[Test jednotného přihlašování](#test-single-sign-on)**  tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9599c-144">**[Test single sign-on](#test-single-sign-on)** tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9599c-145">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9599c-145">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="9599c-146">Povolení služby Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Clarizen.</span><span class="sxs-lookup"><span data-stu-id="9599c-146">Enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Clarizen application.</span></span>

1. <span data-ttu-id="9599c-147">V portálu Azure, na hello hello **Clarizen** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9599c-147">In hello Azure portal, on hello **Clarizen** application integration page, click **Single sign-on**.</span></span>

    ![Kliknutím na tlačítko "Single sign-on"][4]

2. <span data-ttu-id="9599c-149">V hello **jednotného přihlašování** dialogové okno, pro **režimu**, vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9599c-149">In hello **Single sign-on** dialog box, for **Mode**, select **SAML-based Sign-on** tooenable single sign-on.</span></span>

    ![Výběr "na základě SAML přihlášení"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. <span data-ttu-id="9599c-151">V hello **Clarizen domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9599c-151">In hello **Clarizen Domain and URLs** section, perform hello following steps:</span></span>

    ![Pole pro adresu URL identifikátor dotazů a odpovědí](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    <span data-ttu-id="9599c-153">a.</span><span class="sxs-lookup"><span data-stu-id="9599c-153">a.</span></span> <span data-ttu-id="9599c-154">V hello **identifikátor** pole Hodnota typu hello jako: **Clarizen**</span><span class="sxs-lookup"><span data-stu-id="9599c-154">In hello **Identifier** box, type hello value as: **Clarizen**</span></span>

    <span data-ttu-id="9599c-155">b.</span><span class="sxs-lookup"><span data-stu-id="9599c-155">b.</span></span> <span data-ttu-id="9599c-156">V hello **adresa URL odpovědi** zadejte adresu URL pomocí hello následující vzor: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span><span class="sxs-lookup"><span data-stu-id="9599c-156">In hello **Reply URL** box, type a URL by using hello following pattern: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span></span>

    > [!NOTE]
    > <span data-ttu-id="9599c-157">Tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9599c-157">These are not hello real values.</span></span> <span data-ttu-id="9599c-158">Máte toouse hello skutečné identifikátor a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9599c-158">You have toouse hello actual identifier and reply URL.</span></span> <span data-ttu-id="9599c-159">Tady doporučujeme použít hello jedinečnou hodnotu řetězce, jak hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9599c-159">Here we suggest that you use hello unique value of a string as hello identifier.</span></span> <span data-ttu-id="9599c-160">tooget hello skutečnými hodnotami, kontaktujte hello [tým podpory Clarizen](https://success.clarizen.com/hc/en-us/requests/new).</span><span class="sxs-lookup"><span data-stu-id="9599c-160">tooget hello actual values, contact hello [Clarizen support team](https://success.clarizen.com/hc/en-us/requests/new).</span></span>

4. <span data-ttu-id="9599c-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="9599c-161">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Kliknutím na tlačítko "Vytvořit nový certifikát"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. <span data-ttu-id="9599c-163">V hello **vytvořit nový certifikát** dialogové okno pole, klikněte na ikonu hello kalendáře a vyberte datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="9599c-163">In hello **Create New Certificate** dialog box, click hello calendar icon and select an expiry date.</span></span> <span data-ttu-id="9599c-164">Potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9599c-164">Then click **Save**.</span></span>

    ![Výběr a ukládání datum vypršení platnosti](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="9599c-166">V hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9599c-166">In hello **SAML Signing Certificate** section, select **Make new certificate active**, and then click **Save**.</span></span>

    ![Výběrem zaškrtávacího políčka hello pro vytvoření nového certifikátu hello active](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. <span data-ttu-id="9599c-168">V hello **certifikát výměny** dialogové okno, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9599c-168">In hello **Rollover certificate** dialog box, click **OK**.</span></span>

    ![Kliknutím na tlačítko "OK" tooconfirm, které chcete certifikát hello toomake active](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9599c-170">V hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9599c-170">In hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Kliknutím na tlačítko Stáhnout hello toostart "Certifikát (Base64)"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. <span data-ttu-id="9599c-172">V hello **Clarizen konfigurace** klikněte na tlačítko **konfigurace Clarizen** tooopen hello **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9599c-172">In hello **Clarizen Configuration** section, click **Configure Clarizen** tooopen hello **Configure sign-on** window.</span></span>

    ![Kliknutím na tlačítko "Konfigurace Clarizen"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    ![Okno "Konfigurace přihlášení", včetně souborů a adresy URL](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. <span data-ttu-id="9599c-175">V okně prohlížeče jiný web přihlaste jako správce tooyour Clarizen společnosti lokality.</span><span class="sxs-lookup"><span data-stu-id="9599c-175">In a different web browser window, sign in tooyour Clarizen company site as an administrator.</span></span>

11. <span data-ttu-id="9599c-176">Klikněte na své uživatelské jméno a potom klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="9599c-176">Click your username, and then click **Settings**.</span></span>

    <span data-ttu-id="9599c-177">![Kliknutím na "Nastavení" v části vaše uživatelské jméno](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="9599c-177">![Clicking "Settings" under your username](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "Settings")</span></span>

12. <span data-ttu-id="9599c-178">Klikněte na tlačítko hello **globální nastavení** kartě. Pak další příliš**federovaného ověřování**, klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="9599c-178">Click hello **Global Settings** tab. Then, next too**Federated Authentication**, click **edit**.</span></span>

    <span data-ttu-id="9599c-179">![Karta "Globální nastavení"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globální nastavení")</span><span class="sxs-lookup"><span data-stu-id="9599c-179">!["Global Settings" tab](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "Global Settings")</span></span>

13. <span data-ttu-id="9599c-180">V hello **federovaného ověřování** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9599c-180">In hello **Federated Authentication** dialog box, perform hello following steps:</span></span>

    <span data-ttu-id="9599c-181">![Dialogové okno "Federovaného ověřování"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "federovaného ověřování")</span><span class="sxs-lookup"><span data-stu-id="9599c-181">!["Federated Authentication" dialog box](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")</span></span>

    <span data-ttu-id="9599c-182">a.</span><span class="sxs-lookup"><span data-stu-id="9599c-182">a.</span></span> <span data-ttu-id="9599c-183">Vyberte **povolit federovaného ověřování**.</span><span class="sxs-lookup"><span data-stu-id="9599c-183">Select **Enable Federated Authentication**.</span></span>

    <span data-ttu-id="9599c-184">b.</span><span class="sxs-lookup"><span data-stu-id="9599c-184">b.</span></span> <span data-ttu-id="9599c-185">Klikněte na tlačítko **nahrát** tooupload stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="9599c-185">Click **Upload** tooupload your downloaded certificate.</span></span>

    <span data-ttu-id="9599c-186">c.</span><span class="sxs-lookup"><span data-stu-id="9599c-186">c.</span></span> <span data-ttu-id="9599c-187">V hello **přihlašovací adresa URL** zadejte hodnotu hello **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9599c-187">In hello **Sign-in URL** box, enter hello value of **SAML Single Sign-On Service URL** from hello Azure AD application configuration window.</span></span>

    <span data-ttu-id="9599c-188">d.</span><span class="sxs-lookup"><span data-stu-id="9599c-188">d.</span></span> <span data-ttu-id="9599c-189">V hello **Sign-out URL** zadejte hodnotu hello **Sign-Out URL** z okna konfigurace aplikace hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9599c-189">In hello **Sign-out URL** box, enter hello value of **Sign-Out URL** from hello Azure AD application configuration window.</span></span>

    <span data-ttu-id="9599c-190">e.</span><span class="sxs-lookup"><span data-stu-id="9599c-190">e.</span></span> <span data-ttu-id="9599c-191">Vyberte **použít POST**.</span><span class="sxs-lookup"><span data-stu-id="9599c-191">Select **Use POST**.</span></span>

    <span data-ttu-id="9599c-192">f.</span><span class="sxs-lookup"><span data-stu-id="9599c-192">f.</span></span> <span data-ttu-id="9599c-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9599c-193">Click **Save**.</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9599c-194">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9599c-194">Create an Azure AD test user</span></span>
<span data-ttu-id="9599c-195">V hello portálu Azure vytvoření zkušebního uživatele volat Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9599c-195">In hello Azure portal, create a test user called Britta Simon.</span></span>

![Jméno a e-mailovou adresu uživatele testovací hello Azure AD][100]

1. <span data-ttu-id="9599c-197">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9599c-197">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory – ikona](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="9599c-199">Klikněte na tlačítko **uživatelů a skupin**a potom klikněte na **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9599c-199">Click **Users and groups**, and then click **All users** toodisplay hello list of users.</span></span>

    ![Kliknutím na tlačítko "Uživatelé a skupiny" a "Všichni uživatelé"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="9599c-201">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9599c-201">At hello top of hello dialog box, click **Add** tooopen hello **User** dialog box.</span></span>

    ![tlačítko "Přidat" Hello](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="9599c-203">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9599c-203">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno "User" s název, e-mailovou adresu a heslo vyplněno](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    <span data-ttu-id="9599c-205">a.</span><span class="sxs-lookup"><span data-stu-id="9599c-205">a.</span></span> <span data-ttu-id="9599c-206">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9599c-206">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9599c-207">b.</span><span class="sxs-lookup"><span data-stu-id="9599c-207">b.</span></span> <span data-ttu-id="9599c-208">V hello **uživatelské jméno** pole typu hello e-mailovou adresu hello Britta Simon účtu.</span><span class="sxs-lookup"><span data-stu-id="9599c-208">In hello **User name** box, type hello email address of hello Britta Simon account.</span></span>

    <span data-ttu-id="9599c-209">c.</span><span class="sxs-lookup"><span data-stu-id="9599c-209">c.</span></span> <span data-ttu-id="9599c-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9599c-210">Select **Show Password** and write down hello value of **Password**.</span></span>

    <span data-ttu-id="9599c-211">d.</span><span class="sxs-lookup"><span data-stu-id="9599c-211">d.</span></span> <span data-ttu-id="9599c-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9599c-212">Click **Create**.</span></span>

### <a name="create-a-clarizen-test-user"></a><span data-ttu-id="9599c-213">Vytvoření zkušebního uživatele Clarizen</span><span class="sxs-lookup"><span data-stu-id="9599c-213">Create a Clarizen test user</span></span>
<span data-ttu-id="9599c-214">tooenable toosign uživatele Azure AD v tooClarizen, je třeba zřídit uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="9599c-214">tooenable Azure AD users toosign in tooClarizen, you must provision user accounts.</span></span> <span data-ttu-id="9599c-215">V případě hello Clarizen zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="9599c-215">In hello case of Clarizen, provisioning is a manual task.</span></span>

1. <span data-ttu-id="9599c-216">Přihlaste se tooyour Clarizen společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="9599c-216">Sign in tooyour Clarizen company site as an administrator.</span></span>

2. <span data-ttu-id="9599c-217">Klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="9599c-217">Click **People**.</span></span>

    <span data-ttu-id="9599c-218">![Kliknutím na tlačítko "Uživatelé"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="9599c-218">![Clicking "People"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="9599c-219">Klikněte na tlačítko **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9599c-219">Click **Invite User**.</span></span>

    <span data-ttu-id="9599c-220">![Tlačítko "Pozvat uživatele"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="9599c-220">!["Invite User" button](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="9599c-221">V hello **pozvat osoby** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9599c-221">In hello **Invite People** dialog box, perform hello following steps:</span></span>

    <span data-ttu-id="9599c-222">![Dialogové okno "Pozvat osoby"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "pozvat osoby")</span><span class="sxs-lookup"><span data-stu-id="9599c-222">!["Invite People" dialog box](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="9599c-223">a.</span><span class="sxs-lookup"><span data-stu-id="9599c-223">a.</span></span> <span data-ttu-id="9599c-224">V hello **e-mailu** pole typu hello e-mailovou adresu hello Britta Simon účtu.</span><span class="sxs-lookup"><span data-stu-id="9599c-224">In hello **Email** box, type hello email address of hello Britta Simon account.</span></span>

    <span data-ttu-id="9599c-225">b.</span><span class="sxs-lookup"><span data-stu-id="9599c-225">b.</span></span> <span data-ttu-id="9599c-226">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="9599c-226">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9599c-227">Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="9599c-227">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9599c-228">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9599c-228">Assign hello Azure AD test user</span></span>
<span data-ttu-id="9599c-229">Povolte Britta Simon toouse Azure jednotné přihlašování udělení jeho tooClarizen přístup.</span><span class="sxs-lookup"><span data-stu-id="9599c-229">Enable Britta Simon toouse Azure single sign-on by granting her access tooClarizen.</span></span>

![Přiřazené testovacího uživatele][200]

1. <span data-ttu-id="9599c-231">V hello portálu Azure, otevřete zobrazení aplikace hello, procházet toohello directory zobrazení, klikněte na **podnikové aplikace, které**a potom klikněte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9599c-231">In hello Azure portal, open hello applications view, browse toohello directory view, click **Enterprise applications**, and then click **All applications**.</span></span>

    ![Kliknutím na tlačítko "Podnikové aplikace" a "Všechny aplikace"][201]

2. <span data-ttu-id="9599c-233">V seznamu aplikace hello vyberte **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="9599c-233">In hello applications list, select **Clarizen**.</span></span>

    ![Výběr Clarizen v seznamu hello](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. <span data-ttu-id="9599c-235">V levém podokně hello, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9599c-235">In hello left pane, click **Users and groups**.</span></span>

    ![Kliknutím na tlačítko "Uživatelé a skupiny"][202]

4. <span data-ttu-id="9599c-237">Klikněte na tlačítko hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9599c-237">Click hello **Add** button.</span></span> <span data-ttu-id="9599c-238">Potom v hello **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9599c-238">Then, in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![tlačítko "Přidat" Hello a dialogové okno "Přidat přiřazení" hello][203]

5. <span data-ttu-id="9599c-240">V hello **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9599c-240">In hello **Users and groups** dialog box, select **Britta Simon** in hello list of users.</span></span>

6. <span data-ttu-id="9599c-241">V hello **uživatelů a skupin** dialogovém okně klikněte na hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9599c-241">In hello **Users and groups** dialog box, click hello **Select** button.</span></span>

7. <span data-ttu-id="9599c-242">V hello **přidat přiřazení** dialogovém okně klikněte na hello **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9599c-242">In hello **Add Assignment** dialog box, click hello **Assign** button.</span></span>

### <a name="test-single-sign-on"></a><span data-ttu-id="9599c-243">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9599c-243">Test single sign-on</span></span>
<span data-ttu-id="9599c-244">Otestujte vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9599c-244">Test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="9599c-245">Když kliknete na dlaždici Clarizen hello v hello přístupového panelu, by měl být automaticky přihlášeni tooyour Clarizen aplikace.</span><span class="sxs-lookup"><span data-stu-id="9599c-245">When you click hello Clarizen tile in hello Access Panel, you should be automatically signed in tooyour Clarizen application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9599c-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9599c-246">Additional resources</span></span>

* [<span data-ttu-id="9599c-247">Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9599c-247">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9599c-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9599c-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
