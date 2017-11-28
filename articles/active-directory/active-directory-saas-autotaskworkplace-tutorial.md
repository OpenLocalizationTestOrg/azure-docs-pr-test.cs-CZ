---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti Autotask | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Autotask síti na pracovišti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 7f820f24e8e9493fa2e1c075f2ef61d7eaa84f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a><span data-ttu-id="37e23-103">Kurz: Azure Active Directory integrace s Autotask síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="37e23-103">Tutorial: Azure Active Directory integration with Autotask Workplace</span></span>

<span data-ttu-id="37e23-104">V tomto kurzu zjistíte, jak toointegrate Autotask síti na pracovišti s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="37e23-104">In this tutorial, you learn how toointegrate Autotask Workplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="37e23-105">Integrace s Azure AD Autotask síti na pracovišti vám poskytne hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="37e23-105">Integrating Autotask Workplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="37e23-106">Můžete řídit ve službě Azure AD, který má tooAutotask přístup k síti na pracovišti</span><span class="sxs-lookup"><span data-stu-id="37e23-106">You can control in Azure AD who has access tooAutotask Workplace</span></span>
- <span data-ttu-id="37e23-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAutotask síti na pracovišti (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="37e23-107">You can enable your users tooautomatically get signed-on tooAutotask Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="37e23-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="37e23-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="37e23-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="37e23-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37e23-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="37e23-110">Prerequisites</span></span>

<span data-ttu-id="37e23-111">tooconfigure integrace Azure AD s Autotask síti na pracovišti, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="37e23-111">tooconfigure Azure AD integration with Autotask Workplace, you need hello following items:</span></span>

- <span data-ttu-id="37e23-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="37e23-112">An Azure AD subscription</span></span>
- <span data-ttu-id="37e23-113">Síti na pracovišti Autotask jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="37e23-113">An Autotask Workplace single-sign on enabled subscription</span></span>
- <span data-ttu-id="37e23-114">Musíte být správce nebo správce super v síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="37e23-114">You must be an administrator or super administrator in Workplace.</span></span>
- <span data-ttu-id="37e23-115">Musí mít účet správce v hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="37e23-115">You must have an administrator account in hello Azure AD.</span></span>
- <span data-ttu-id="37e23-116">Hello uživatelů, kteří se využívají tato funkce musí mít účty v síti na pracovišti a hello Azure AD a jejich e-mailové adresy pro obě se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="37e23-116">hello users that will utilize this feature must have accounts within Workplace and hello Azure AD, and their email addresses for both must match.</span></span>

> [!NOTE]
> <span data-ttu-id="37e23-117">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="37e23-117">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="37e23-118">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="37e23-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="37e23-119">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="37e23-119">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="37e23-120">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="37e23-120">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="37e23-121">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="37e23-121">Scenario description</span></span>
<span data-ttu-id="37e23-122">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="37e23-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="37e23-123">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="37e23-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="37e23-124">Přidání Autotask síti na pracovišti z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="37e23-124">Adding Autotask Workplace from hello gallery</span></span>
2. <span data-ttu-id="37e23-125">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="37e23-125">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-autotask-workplace-from-hello-gallery"></a><span data-ttu-id="37e23-126">Přidání Autotask síti na pracovišti z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="37e23-126">Adding Autotask Workplace from hello gallery</span></span>
<span data-ttu-id="37e23-127">tooconfigure hello integrace síti na pracovišti Autotask do Azure AD, je nutné tooadd síti na pracovišti Autotask hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="37e23-127">tooconfigure hello integration of Autotask Workplace into Azure AD, you need tooadd Autotask Workplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="37e23-128">**tooadd Autotask síti na pracovišti z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="37e23-128">**tooadd Autotask Workplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="37e23-129">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="37e23-129">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="37e23-131">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="37e23-131">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="37e23-132">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="37e23-132">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="37e23-134">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="37e23-134">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="37e23-136">Hello vyhledávacího pole zadejte **síti na pracovišti Autotask**, vyberte **síti na pracovišti Autotask** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="37e23-136">In hello search box, type **Autotask Workplace**, select  **Autotask Workplace**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Autotask síti na pracovišti v hello seznamu výsledků](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="37e23-138">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="37e23-138">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="37e23-139">V této části nakonfigurujete a testovací Azure AD jednotné přihlašování se Autotask pracovišti podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="37e23-139">In this section, you configure and test Azure AD single sign-on with Autotask Workplace based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="37e23-140">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v síti na pracovišti Autotask je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="37e23-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Autotask Workplace is tooa user in Azure AD.</span></span> <span data-ttu-id="37e23-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v síti na pracovišti Autotask musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="37e23-141">In other words, a link relationship between an Azure AD user and hello related user in Autotask Workplace needs toobe established.</span></span>

<span data-ttu-id="37e23-142">V síti na pracovišti Autotask, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="37e23-142">In Autotask Workplace, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="37e23-143">tooconfigure a testu Azure AD jednotné přihlašování s Autotask síti na pracovišti, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="37e23-143">tooconfigure and test Azure AD single sign-on with Autotask Workplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="37e23-144">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="37e23-144">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="37e23-145">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="37e23-145">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="37e23-146">**[Vytvoření zkušebního uživatele síti na pracovišti Autotask](#create-an-autotask-workplace-test-user)**  -toohave protějšek Britta Simon v síti na pracovišti Autotask, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="37e23-146">**[Create an Autotask Workplace test user](#create-an-autotask-workplace-test-user)** - toohave a counterpart of Britta Simon in Autotask Workplace that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="37e23-147">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37e23-147">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="37e23-148">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="37e23-148">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="37e23-149">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="37e23-149">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="37e23-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Autotask síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="37e23-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Autotask Workplace application.</span></span>

<span data-ttu-id="37e23-151">**tooconfigure Azure AD jednotné přihlašování s Autotask síti na pracovišti, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="37e23-151">**tooconfigure Azure AD single sign-on with Autotask Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="37e23-152">V portálu Azure, na hello hello **síti na pracovišti Autotask** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="37e23-152">In hello Azure portal, on hello **Autotask Workplace** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="37e23-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="37e23-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. <span data-ttu-id="37e23-156">Pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu, proveďte následující kroky na hello hello **Autotask síti na pracovišti domény a adresy URL** části:</span><span class="sxs-lookup"><span data-stu-id="37e23-156">If you wish tooconfigure hello application in **IDP** initiated mode, perform hello following steps on hello **Autotask Workplace Domain and URLs** section:</span></span>

    ![Autotask síti na pracovišti domény a adresy URL jeden přihlašování informace o rozšíření IDP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    <span data-ttu-id="37e23-158">a.</span><span class="sxs-lookup"><span data-stu-id="37e23-158">a.</span></span> <span data-ttu-id="37e23-159">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="37e23-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span></span>

    <span data-ttu-id="37e23-160">b.</span><span class="sxs-lookup"><span data-stu-id="37e23-160">b.</span></span> <span data-ttu-id="37e23-161">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="37e23-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span></span>

4. <span data-ttu-id="37e23-162">Pokud chcete aplikace hello tooconfigure v **SP** initiated režimu, zkontrolujte **zobrazit upřesňující nastavení adresy URL** a provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="37e23-162">If you wish tooconfigure hello application in **SP** initiated mode, check **Show advanced URL settings** and perform hello following steps:</span></span>

    ![Autotask síti na pracovišti domény a adresy URL jeden přihlašování informace pro poskytovatele služeb](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    <span data-ttu-id="37e23-164">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.awp.autotask.net/loginsso`</span><span class="sxs-lookup"><span data-stu-id="37e23-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/loginsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="37e23-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="37e23-165">These values are not real.</span></span> <span data-ttu-id="37e23-166">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="37e23-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="37e23-167">Obraťte se na [tým podpory Autotask pracoviště klienta](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="37e23-167">Contact [Autotask Workplace Client support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget these values.</span></span> 

5. <span data-ttu-id="37e23-168">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="37e23-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. <span data-ttu-id="37e23-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="37e23-170">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="37e23-172">V okně prohlížeče jiných webových hello přihlásit se tooWorkplace Online pomocí přihlašovacích údajů správce.</span><span class="sxs-lookup"><span data-stu-id="37e23-172">In a different web browser window, Log in tooWorkplace Online using hello administrator credentials.</span></span>

    >[!Note]
    ><span data-ttu-id="37e23-173">Při konfiguraci hello deklarací identity, bude nutné subdoména toobe zadán.</span><span class="sxs-lookup"><span data-stu-id="37e23-173">When configuring hello IdP, a subdomain will need toobe specified.</span></span> <span data-ttu-id="37e23-174">tooconfirm hello správné subdomény, přihlášení tooWorkplace Online.</span><span class="sxs-lookup"><span data-stu-id="37e23-174">tooconfirm hello correct subdomain, login tooWorkplace Online.</span></span> <span data-ttu-id="37e23-175">Po přihlášení, zkontrolujte v adrese URL hello Poznámka toohello subdomény.</span><span class="sxs-lookup"><span data-stu-id="37e23-175">Once logged in, make note toohello subdomain in hello URL.</span></span>
    ><span data-ttu-id="37e23-176">je součástí hello mezi hello "https://" a ".awp.autotask.net/" a musí být v USA, Evropa, certifikační autority nebo au Hello subdomény.</span><span class="sxs-lookup"><span data-stu-id="37e23-176">hello subdomain is hello part between hello “https://“ and “.awp.autotask.net/“ and should be us, eu, ca, or au.</span></span>

8. <span data-ttu-id="37e23-177">Přejděte příliš**konfigurace** > **jednotné přihlašování** a provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="37e23-177">Go too**Configuration** > **Single Sign-On** and perform hello following steps:</span></span>

    ![Autotask Single Sign-on konfigurace](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    <span data-ttu-id="37e23-179">a.</span><span class="sxs-lookup"><span data-stu-id="37e23-179">a.</span></span> <span data-ttu-id="37e23-180">Vyberte hello **Metadata souboru XML** možnost a pak nahrajte hello **soubor XML s metadaty** stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="37e23-180">Select hello **XML Metadata File** option, and then upload hello **Metadata XML** downloaded from Azure portal.</span></span>

    <span data-ttu-id="37e23-181">b.</span><span class="sxs-lookup"><span data-stu-id="37e23-181">b.</span></span> <span data-ttu-id="37e23-182">Klikněte na tlačítko **povolení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="37e23-182">Click **Enable SSO**.</span></span>
    
    ![Autotask jednotné přihlášení schválit konfigurace](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    <span data-ttu-id="37e23-184">c.</span><span class="sxs-lookup"><span data-stu-id="37e23-184">c.</span></span> <span data-ttu-id="37e23-185">Vyberte hello **potvrzuji, tyto informace jsou správné a důvěřovat této IdP** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="37e23-185">Select hello **I confirm this information is correct and I trust this IdP** check box.</span></span>

    <span data-ttu-id="37e23-186">d.</span><span class="sxs-lookup"><span data-stu-id="37e23-186">d.</span></span> <span data-ttu-id="37e23-187">Klikněte na tlačítko **schválit**.</span><span class="sxs-lookup"><span data-stu-id="37e23-187">Click **Approve**.</span></span>
     
>[!Note]
><span data-ttu-id="37e23-188">Pokud potřebujete pomoc s konfigurací Autotask síti na pracovišti, najdete v tématu [tuto stránku](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget pomoc s pracovního účtu.</span><span class="sxs-lookup"><span data-stu-id="37e23-188">If you require assistance with configuring Autotask Workplace, please see [this page](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget assistance with your Workplace account.</span></span>

> [!TIP]
> <span data-ttu-id="37e23-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="37e23-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="37e23-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="37e23-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="37e23-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="37e23-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="37e23-192">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="37e23-192">Create an Azure AD test user</span></span>

<span data-ttu-id="37e23-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="37e23-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="37e23-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="37e23-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="37e23-196">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="37e23-196">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="37e23-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="37e23-198">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="37e23-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="37e23-200">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="37e23-202">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="37e23-202">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="37e23-204">a.</span><span class="sxs-lookup"><span data-stu-id="37e23-204">a.</span></span> <span data-ttu-id="37e23-205">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="37e23-205">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="37e23-206">b.</span><span class="sxs-lookup"><span data-stu-id="37e23-206">b.</span></span> <span data-ttu-id="37e23-207">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="37e23-207">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="37e23-208">c.</span><span class="sxs-lookup"><span data-stu-id="37e23-208">c.</span></span> <span data-ttu-id="37e23-209">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="37e23-209">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="37e23-210">d.</span><span class="sxs-lookup"><span data-stu-id="37e23-210">d.</span></span> <span data-ttu-id="37e23-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="37e23-211">Click **Create**.</span></span>

### <a name="create-an-autotask-workplace-test-user"></a><span data-ttu-id="37e23-212">Vytvoření zkušebního uživatele síti na pracovišti Autotask</span><span class="sxs-lookup"><span data-stu-id="37e23-212">Create an Autotask Workplace test user</span></span>

<span data-ttu-id="37e23-213">V této části vytvoříte volal Britta Simon v Autotask uživatele.</span><span class="sxs-lookup"><span data-stu-id="37e23-213">In this section, you create a user called Britta Simon in Autotask.</span></span> <span data-ttu-id="37e23-214">Spojte se s [tým podpory síti na pracovišti Autotask](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello uživatelů v síti na pracovišti Autotask platformy hello.</span><span class="sxs-lookup"><span data-stu-id="37e23-214">Please work with [Autotask Workplace support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello users in hello Autotask Workplace platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="37e23-215">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="37e23-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="37e23-216">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooAutotask přístup k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="37e23-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAutotask Workplace.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="37e23-218">**tooassign Britta Simon tooAutotask síti na pracovišti, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="37e23-218">**tooassign Britta Simon tooAutotask Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="37e23-219">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="37e23-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="37e23-221">V seznamu aplikace hello vyberte **Autotask síti na pracovišti**.</span><span class="sxs-lookup"><span data-stu-id="37e23-221">In hello applications list, select **Autotask Workplace**.</span></span>

    ![Hello síti na pracovišti Autotask odkaz v seznamu aplikace hello](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. <span data-ttu-id="37e23-223">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="37e23-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="37e23-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="37e23-225">Click **Add** button.</span></span> <span data-ttu-id="37e23-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="37e23-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="37e23-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="37e23-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="37e23-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="37e23-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="37e23-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="37e23-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="37e23-231">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="37e23-231">Test single sign-on</span></span>

<span data-ttu-id="37e23-232">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="37e23-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="37e23-233">Po kliknutí na tlačítko hello síti na pracovišti Autotask dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace Autotask síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="37e23-233">When you click hello Autotask Workplace tile in hello Access Panel, you should get automatically signed-on tooyour Autotask Workplace application.</span></span>
<span data-ttu-id="37e23-234">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="37e23-234">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37e23-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="37e23-235">Additional resources</span></span>

* [<span data-ttu-id="37e23-236">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="37e23-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="37e23-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="37e23-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_203.png

