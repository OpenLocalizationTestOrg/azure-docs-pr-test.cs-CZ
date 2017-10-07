---
title: 'Kurz: Azure Active Directory integrace s RFPIO | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a RFPIO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: e0c692276276edd8f859e73d81cf54d75a65957a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a><span data-ttu-id="b4398-103">Kurz: Azure Active Directory integrace s RFPIO</span><span class="sxs-lookup"><span data-stu-id="b4398-103">Tutorial: Azure Active Directory integration with RFPIO</span></span>

<span data-ttu-id="b4398-104">V tomto kurzu zjistíte, jak toointegrate RFPIO s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b4398-104">In this tutorial, you learn how toointegrate RFPIO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b4398-105">Integrace RFPIO s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b4398-105">Integrating RFPIO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b4398-106">Můžete určovat, kdo ve službě Azure AD, který má přístup tooRFPIO.</span><span class="sxs-lookup"><span data-stu-id="b4398-106">You can control who in Azure AD who has access tooRFPIO.</span></span>
- <span data-ttu-id="b4398-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRFPIO (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b4398-107">You can enable your users tooautomatically get signed-on tooRFPIO (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="b4398-108">Můžete spravovat vaše účty v jednom centrálním místě – hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b4398-108">You can manage your accounts in one central location--hello Azure portal.</span></span>

<span data-ttu-id="b4398-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b4398-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4398-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b4398-110">Prerequisites</span></span>

<span data-ttu-id="b4398-111">Integrace služby Azure AD s RFPIO tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b4398-111">tooconfigure Azure AD integration with RFPIO, you need hello following items:</span></span>

- <span data-ttu-id="b4398-112">Předplatné služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b4398-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="b4398-113">RFPIO jednotného přihlašování na povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="b4398-113">A RFPIO single sign-on-enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="b4398-114">Nedoporučujeme používat kroky hello tootest v produkčním prostředí v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="b4398-114">We don't recommend that you use a production environment tootest hello steps in this tutorial.</span></span>

<span data-ttu-id="b4398-115">tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="b4398-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="b4398-116">Pokud to není nezbytné, nepoužívejte produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4398-116">Do not use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="b4398-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b4398-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b4398-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b4398-118">Scenario description</span></span>
<span data-ttu-id="b4398-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4398-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b4398-120">Hello scénář, který je popsané v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b4398-120">hello scenario that's outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b4398-121">Přidání RFPIO z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="b4398-121">Adding RFPIO from hello gallery.</span></span>
2. <span data-ttu-id="b4398-122">Konfigurace a testování Azure AD jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b4398-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-rfpio-from-hello-gallery"></a><span data-ttu-id="b4398-123">Přidat RFPIO z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b4398-123">Add RFPIO from hello gallery</span></span>
<span data-ttu-id="b4398-124">tooconfigure hello integrace RFPIO do Azure AD, je nutné tooadd RFPIO hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b4398-124">tooconfigure hello integration of RFPIO into Azure AD, you need tooadd RFPIO from hello gallery tooyour list of managed SaaS apps.</span></span>

### <a name="tooadd-rfpio-from-hello-gallery"></a><span data-ttu-id="b4398-125">tooadd RFPIO z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b4398-125">tooadd RFPIO from hello gallery</span></span>

1. <span data-ttu-id="b4398-126">V hello  **[portál Azure](https://portal.azure.com)**, na hello levém navigačním podokně, vyberte hello **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b4398-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b4398-128">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b4398-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b4398-130">tooadd novou aplikaci, vyberte hello **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b4398-130">tooadd a new application, select hello **New application** button on hello top of dialog box.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b4398-132">Hello vyhledávacího pole zadejte **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="b4398-132">In hello search box, type **RFPIO**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. <span data-ttu-id="b4398-134">Na panelu výsledků hello vyberte **RFPIO**a potom vyberte hello **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4398-134">In hello results panel, select **RFPIO**, and then select hello **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b4398-136">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b4398-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="b4398-137">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s RFPIO podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b4398-137">In this section, you configure and test Azure AD single sign-on with RFPIO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b4398-138">Pro toowork jeden přihlašování Azure AD musí tooknow, jaký vztah hello je mezi příslušného uživatele v RFPIO a uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b4398-138">For single sign-on toowork, Azure AD needs tooknow what hello relationship is between counterpart user in RFPIO and a user in Azure AD.</span></span> <span data-ttu-id="b4398-139">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v RFPIO musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b4398-139">In other words, a link relationship between an Azure AD user and hello related user in RFPIO needs toobe established.</span></span>

<span data-ttu-id="b4398-140">V RFPIO, přiřadit hodnotu hello **uživatelské jméno** ve službě Azure AD jako hodnota hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b4398-140">In RFPIO, assign hello value of **user name** in Azure AD as hello value of  **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b4398-141">tooconfigure a testu Azure AD jednotné přihlašování s RFPIO, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b4398-141">tooconfigure and test Azure AD single sign-on with RFPIO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b4398-142">**[Konfigurovat Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**– tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b4398-142">**[Configure Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**--tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b4398-143">**[Vytvořit testovací uživatele Azure AD](#creating-an-azure-ad-test-user)**– tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b4398-143">**[Create an Azure AD test user](#creating-an-azure-ad-test-user)**-- tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b4398-144">**[Vytvoření zkušebního uživatele RFPIO](#creating-a-rfpio-test-user)**  – toohave protějšek Britta Simon v RFPIO, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b4398-144">**[Create a RFPIO test user](#creating-a-rfpio-test-user)** --toohave a counterpart of Britta Simon in RFPIO that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b4398-145">**[Přiřadit hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**tooenable – Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b4398-145">**[Assign hello Azure AD test user](#assigning-the-azure-ad-test-user)**--tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b4398-146">**[Test jednotného přihlašování](#testing-single-sign-on)**  – tooverify Pokud hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b4398-146">**[Test Single Sign-On](#testing-single-sign-on)** --tooverify if hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b4398-147">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b4398-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b4398-148">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci RFPIO.</span><span class="sxs-lookup"><span data-stu-id="b4398-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RFPIO application.</span></span>

<span data-ttu-id="b4398-149">**tooconfigure Azure AD jednotné přihlašování s RFPIO, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b4398-149">**tooconfigure Azure AD single sign-on with RFPIO, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4398-150">V portálu Azure, na hello hello **RFPIO** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b4398-150">In hello Azure portal, on hello **RFPIO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b4398-152">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b4398-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. <span data-ttu-id="b4398-154">Na hello **RFPIO domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="b4398-154">On hello **RFPIO Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    <span data-ttu-id="b4398-156">a.</span><span class="sxs-lookup"><span data-stu-id="b4398-156">a.</span></span> <span data-ttu-id="b4398-157">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://www.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="b4398-157">In hello **Identifier** textbox, type hello URL: `https://www.rfpio.com`</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    <span data-ttu-id="b4398-159">b.</span><span class="sxs-lookup"><span data-stu-id="b4398-159">b.</span></span> <span data-ttu-id="b4398-160">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="b4398-160">Check **Show advanced URL settings**.</span></span>

    <span data-ttu-id="b4398-161">c.</span><span class="sxs-lookup"><span data-stu-id="b4398-161">c.</span></span> <span data-ttu-id="b4398-162">V hello **předávání stavu** textového pole zadejte hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="b4398-162">In hello **Relay State** textbox enter a string value.</span></span> <span data-ttu-id="b4398-163">Obraťte se na [tým podpory RFPIO](https://www.rfpio.com/contact/) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b4398-163">Contact [RFPIO support team](https://www.rfpio.com/contact/) tooget this value.</span></span> 

4. <span data-ttu-id="b4398-164">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="b4398-164">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="b4398-165">Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="b4398-165">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    <span data-ttu-id="b4398-167">V hello **přihlásit na adrese URL** textovému poli, hello zadat adresu URL:`https://www.app.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="b4398-167">In hello **Sign on URL** textbox, type hello URL: `https://www.app.rfpio.com`</span></span>

5. <span data-ttu-id="b4398-168">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b4398-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. <span data-ttu-id="b4398-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b4398-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="b4398-172">V okně prohlížeče jiný web, přihlášení toohello **RFPIO** webu jako správce.</span><span class="sxs-lookup"><span data-stu-id="b4398-172">In a different web browser window, login toohello **RFPIO** website as an administrator.</span></span>

8. <span data-ttu-id="b4398-173">Klikněte na rozevírací rohu dolní hello.</span><span class="sxs-lookup"><span data-stu-id="b4398-173">Click on hello bottom left corner dropdown.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. <span data-ttu-id="b4398-175">Klikněte na hello **nastavení organizace**.</span><span class="sxs-lookup"><span data-stu-id="b4398-175">Click on hello **Organization Settings**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. <span data-ttu-id="b4398-177">Klikněte na hello **funkce a integrace**.</span><span class="sxs-lookup"><span data-stu-id="b4398-177">Click on hello **FEATURES & INTEGRATION**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. <span data-ttu-id="b4398-179">V hello **Konfigurace jednotného přihlašování SAML** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="b4398-179">In hello **SAML SSO Configuration** Click **Edit**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. <span data-ttu-id="b4398-181">V této části proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="b4398-181">In this Section perform following actions:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    <span data-ttu-id="b4398-183">a.</span><span class="sxs-lookup"><span data-stu-id="b4398-183">a.</span></span> <span data-ttu-id="b4398-184">Zkopírujte obsah hello hello **stáhnout soubor XML s metadaty** a vložte jej do hello **konfigurace identity** pole.</span><span class="sxs-lookup"><span data-stu-id="b4398-184">Copy hello content of hello **Downloaded Metadata XML** and paste it into hello **identity configuration** field.</span></span>

    > [!NOTE]
    ><span data-ttu-id="b4398-185">stáhnout obsah toocopy hello **soubor XML s metadaty** použití **Poznámkový blok ++** nebo správné **editoru XML**.</span><span class="sxs-lookup"><span data-stu-id="b4398-185">toocopy hello content of downloaded **Metadata XML** Use **Notepad++** or proper **XML Editor**.</span></span> 

    <span data-ttu-id="b4398-186">b.</span><span class="sxs-lookup"><span data-stu-id="b4398-186">b.</span></span> <span data-ttu-id="b4398-187">Klikněte na tlačítko **ověření**.</span><span class="sxs-lookup"><span data-stu-id="b4398-187">Click **Validate**.</span></span>

    <span data-ttu-id="b4398-188">c.</span><span class="sxs-lookup"><span data-stu-id="b4398-188">c.</span></span> <span data-ttu-id="b4398-189">Po kliknutím **ověřením**, podle **SAML(Enabled)** tooon.</span><span class="sxs-lookup"><span data-stu-id="b4398-189">After Clicking **Validate**, Flip **SAML(Enabled)** tooon.</span></span>

    <span data-ttu-id="b4398-190">d.</span><span class="sxs-lookup"><span data-stu-id="b4398-190">d.</span></span> <span data-ttu-id="b4398-191">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="b4398-191">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="b4398-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b4398-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b4398-193">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b4398-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b4398-194">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b4398-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b4398-195">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b4398-195">Create an Azure AD test user</span></span>
<span data-ttu-id="b4398-196">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b4398-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b4398-198">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b4398-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4398-199">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b4398-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b4398-201">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b4398-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b4398-203">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b4398-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b4398-205">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b4398-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b4398-207">a.</span><span class="sxs-lookup"><span data-stu-id="b4398-207">a.</span></span> <span data-ttu-id="b4398-208">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b4398-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b4398-209">b.</span><span class="sxs-lookup"><span data-stu-id="b4398-209">b.</span></span> <span data-ttu-id="b4398-210">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b4398-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b4398-211">c.</span><span class="sxs-lookup"><span data-stu-id="b4398-211">c.</span></span> <span data-ttu-id="b4398-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b4398-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b4398-213">d.</span><span class="sxs-lookup"><span data-stu-id="b4398-213">d.</span></span> <span data-ttu-id="b4398-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b4398-214">Click **Create**.</span></span>
 
### <a name="create-a-rfpio-test-user"></a><span data-ttu-id="b4398-215">Vytvoření zkušebního uživatele RFPIO</span><span class="sxs-lookup"><span data-stu-id="b4398-215">Create a RFPIO test user</span></span>

<span data-ttu-id="b4398-216">Uživatelé toolog tooenable Azure AD v tooRFPIO, se musí být zřízená do RFPIO.</span><span class="sxs-lookup"><span data-stu-id="b4398-216">tooenable Azure AD users toolog in tooRFPIO, they must be provisioned into RFPIO.</span></span>  
<span data-ttu-id="b4398-217">V případě hello RFPIO zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b4398-217">In hello case of RFPIO, provisioning is a manual task.</span></span>

<span data-ttu-id="b4398-218">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b4398-218">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4398-219">Přihlaste se tooyour RFPIO společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b4398-219">Log in tooyour RFPIO company site as an administrator.</span></span>

2. <span data-ttu-id="b4398-220">Klikněte na rozevírací rohu dolní hello.</span><span class="sxs-lookup"><span data-stu-id="b4398-220">Click on hello bottom left corner dropdown.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. <span data-ttu-id="b4398-222">Klikněte na hello **nastavení organizace**.</span><span class="sxs-lookup"><span data-stu-id="b4398-222">Click on hello **Organization Settings**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. <span data-ttu-id="b4398-224">Klikněte na tlačítko **členové týmu**.</span><span class="sxs-lookup"><span data-stu-id="b4398-224">Click **TEAM MEMBERS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. <span data-ttu-id="b4398-226">Klikněte na **přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="b4398-226">Click on **ADD MEMBERS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. <span data-ttu-id="b4398-228">V hello **přidat nové členy** části.</span><span class="sxs-lookup"><span data-stu-id="b4398-228">In hello **Add New Members** section.</span></span> <span data-ttu-id="b4398-229">Proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="b4398-229">Perform following actions:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app8.png)

    <span data-ttu-id="b4398-231">a.</span><span class="sxs-lookup"><span data-stu-id="b4398-231">a.</span></span> <span data-ttu-id="b4398-232">Zadejte **e-mailová adresa** v hello **zadejte jednu e-mailovou na každý řádek** pole.</span><span class="sxs-lookup"><span data-stu-id="b4398-232">Enter **Email address** in hello **Enter one email per line** field.</span></span>

    <span data-ttu-id="b4398-233">b.</span><span class="sxs-lookup"><span data-stu-id="b4398-233">b.</span></span> <span data-ttu-id="b4398-234">Vyberte Plese **Role** podle požadavků.</span><span class="sxs-lookup"><span data-stu-id="b4398-234">Plese select **Role** according your requirements.</span></span>

    <span data-ttu-id="b4398-235">c.</span><span class="sxs-lookup"><span data-stu-id="b4398-235">c.</span></span> <span data-ttu-id="b4398-236">Klikněte na tlačítko **přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="b4398-236">Click **ADD MEMBERS**.</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="b4398-237">Držitel účtu Azure Active Directory Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="b4398-237">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="b4398-238">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b4398-238">Assign hello Azure AD test user</span></span>

<span data-ttu-id="b4398-239">V této části povolíte tak, že udělíte přístup tooRFPIO toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b4398-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRFPIO.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b4398-241">**tooassign Britta Simon tooRFPIO, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b4398-241">**tooassign Britta Simon tooRFPIO, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4398-242">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b4398-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b4398-244">V seznamu aplikace hello vyberte **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="b4398-244">In hello applications list, select **RFPIO**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. <span data-ttu-id="b4398-246">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b4398-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b4398-248">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b4398-248">Click **Add** button.</span></span> <span data-ttu-id="b4398-249">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b4398-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b4398-251">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b4398-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b4398-252">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b4398-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b4398-253">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b4398-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b4398-254">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b4398-254">Test single sign-on</span></span>

<span data-ttu-id="b4398-255">V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b4398-255">In this section, you test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="b4398-256">Po kliknutí na tlačítko hello RFPIO dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour RFPIO aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4398-256">When you click hello RFPIO tile in hello Access Panel, you should get automatically signed-on tooyour RFPIO application.</span></span>
<span data-ttu-id="b4398-257">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b4398-257">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b4398-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b4398-258">Additional resources</span></span>

* [<span data-ttu-id="b4398-259">Seznam kurzů o toointegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b4398-259">List of tutorials about how toointegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b4398-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b4398-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

