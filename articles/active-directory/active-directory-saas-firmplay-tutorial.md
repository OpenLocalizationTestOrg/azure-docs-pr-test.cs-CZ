---
title: "Kurz: Azure Active Directory integrace s FirmPlay - hájící zájmy zaměstnanec pro nábor | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FirmPlay - hájící zájmy zaměstnanec pro nábor."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: f143e0bb8f2a42de880d77e5f033694ce3f09cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a><span data-ttu-id="9aa91-103">Kurz: Azure Active Directory integrace s FirmPlay - hájící zájmy zaměstnanec pro nábor</span><span class="sxs-lookup"><span data-stu-id="9aa91-103">Tutorial: Azure Active Directory integration with FirmPlay - Employee Advocacy for Recruiting</span></span>

<span data-ttu-id="9aa91-104">V tomto kurzu zjistíte, jak toointegrate FirmPlay - hájící zájmy zaměstnanec pro nábor službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9aa91-104">In this tutorial, you learn how toointegrate FirmPlay - Employee Advocacy for Recruiting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9aa91-105">Integrace FirmPlay - hájící zájmy zaměstnanec pro nábor s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9aa91-105">Integrating FirmPlay - Employee Advocacy for Recruiting with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9aa91-106">Můžete řídit ve službě Azure AD, který má přístup tooFirmPlay - hájící zájmy zaměstnanec pro nábor</span><span class="sxs-lookup"><span data-stu-id="9aa91-106">You can control in Azure AD who has access tooFirmPlay - Employee Advocacy for Recruiting</span></span>
- <span data-ttu-id="9aa91-107">Vaši uživatelé tooautomatically get přihlášeného tooFirmPlay - hájící zájmy zaměstnanec pro nábor (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="9aa91-107">You can enable your users tooautomatically get signed-on tooFirmPlay - Employee Advocacy for Recruiting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9aa91-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="9aa91-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="9aa91-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9aa91-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9aa91-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9aa91-110">Prerequisites</span></span>

<span data-ttu-id="9aa91-111">tooconfigure integrace Azure AD s FirmPlay - hájící zájmy zaměstnanec pro nábor, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9aa91-111">tooconfigure Azure AD integration with FirmPlay - Employee Advocacy for Recruiting, you need hello following items:</span></span>

- <span data-ttu-id="9aa91-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9aa91-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9aa91-113">FirmPlay - hájící zájmy zaměstnanec pro jednotné přihlašování v předplatném povolené o přijetí</span><span class="sxs-lookup"><span data-stu-id="9aa91-113">A FirmPlay - Employee Advocacy for Recruiting single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="9aa91-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9aa91-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="9aa91-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9aa91-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9aa91-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="9aa91-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9aa91-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9aa91-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="9aa91-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9aa91-118">Scenario description</span></span>
<span data-ttu-id="9aa91-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9aa91-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9aa91-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9aa91-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9aa91-121">Přidání FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9aa91-121">Adding FirmPlay - Employee Advocacy for Recruiting from hello gallery</span></span>
2. <span data-ttu-id="9aa91-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9aa91-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-hello-gallery"></a><span data-ttu-id="9aa91-123">Přidání FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9aa91-123">Adding FirmPlay - Employee Advocacy for Recruiting from hello gallery</span></span>
<span data-ttu-id="9aa91-124">integrace hello tooconfigure FirmPlay - hájící zájmy zaměstnanec pro nábor do služby Azure AD, je nutné tooadd FirmPlay - hájící zájmy zaměstnanec pro nábor hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9aa91-124">tooconfigure hello integration of FirmPlay - Employee Advocacy for Recruiting into Azure AD, you need tooadd FirmPlay - Employee Advocacy for Recruiting from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9aa91-125">**tooadd FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9aa91-125">**tooadd FirmPlay - Employee Advocacy for Recruiting from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9aa91-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9aa91-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9aa91-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9aa91-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9aa91-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9aa91-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9aa91-133">Hello vyhledávacího pole zadejte **FirmPlay - hájící zájmy zaměstnanec pro nábor**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-133">In hello search box, type **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. <span data-ttu-id="9aa91-135">Na panelu výsledků hello vyberte **FirmPlay - hájící zájmy zaměstnanec pro nábor**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9aa91-135">In hello results panel, select **FirmPlay - Employee Advocacy for Recruiting**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9aa91-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9aa91-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9aa91-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9aa91-138">In this section, you configure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9aa91-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FirmPlay - hájící zájmy zaměstnanec pro nábor je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9aa91-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FirmPlay - Employee Advocacy for Recruiting is tooa user in Azure AD.</span></span> <span data-ttu-id="9aa91-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FirmPlay - hájící zájmy zaměstnanec pro nábor musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9aa91-140">In other words, a link relationship between an Azure AD user and hello related user in FirmPlay - Employee Advocacy for Recruiting needs toobe established.</span></span>

<span data-ttu-id="9aa91-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v FirmPlay - hájící zájmy zaměstnanec pro nábor.</span><span class="sxs-lookup"><span data-stu-id="9aa91-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FirmPlay - Employee Advocacy for Recruiting.</span></span>

<span data-ttu-id="9aa91-142">tooconfigure a testování Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9aa91-142">tooconfigure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9aa91-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9aa91-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9aa91-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9aa91-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9aa91-145">**[Vytváření FirmPlay - hájící zájmy zaměstnanec pro nábor testovacího uživatele](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  -toohave protějšek Britta Simon v FirmPlay: hájící zájmy zaměstnanců, pro který je o přijetí propojené toohello Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="9aa91-145">**[Creating a FirmPlay - Employee Advocacy for Recruiting test user](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)** - toohave a counterpart of Britta Simon in FirmPlay: Employee Advocacy for Recruiting that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9aa91-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9aa91-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9aa91-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9aa91-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9aa91-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9aa91-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9aa91-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v vaší FirmPlay - hájící zájmy zaměstnanec nábor aplikace.</span><span class="sxs-lookup"><span data-stu-id="9aa91-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FirmPlay - Employee Advocacy for Recruiting application.</span></span>

<span data-ttu-id="9aa91-150">**tooconfigure Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9aa91-150">**tooconfigure Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, perform hello following steps:**</span></span>

1. <span data-ttu-id="9aa91-151">V hello Azure Management portal na hello **FirmPlay - hájící zájmy zaměstnanec pro nábor** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-151">In hello Azure Management portal, on hello **FirmPlay - Employee Advocacy for Recruiting** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9aa91-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="9aa91-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. <span data-ttu-id="9aa91-155">Na hello **FirmPlay - hájící zájmy zaměstnanec domény o přijetí a adresy URL** oddílem se v hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<your-subdomain>.firmplay.com/`</span><span class="sxs-lookup"><span data-stu-id="9aa91-155">On hello **FirmPlay - Employee Advocacy for Recruiting Domain and URLs** section, in hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<your-subdomain>.firmplay.com/`</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > <span data-ttu-id="9aa91-157">Upozorňujeme, že se nejedná hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9aa91-157">Please note that this is not hello real value.</span></span> <span data-ttu-id="9aa91-158">Máte tooupdate tuto hodnotu s hello skutečné přihlásit na adrese URL.</span><span class="sxs-lookup"><span data-stu-id="9aa91-158">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="9aa91-159">Obraťte se na [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9aa91-159">Contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) tooget this value.</span></span> 

4. <span data-ttu-id="9aa91-160">Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-160">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. <span data-ttu-id="9aa91-162">Na hello **vytvořit nový certifikát** dialogové okno, klikněte na ikonu hello kalendáře a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-162">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="9aa91-163">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9aa91-163">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="9aa91-165">Na hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9aa91-165">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. <span data-ttu-id="9aa91-167">V místní nabídce hello **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-167">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9aa91-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9aa91-169">On hello **SAML Signing Certificate** section, click **Certificate (base64)** and then save hello certificate file on your computer.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. <span data-ttu-id="9aa91-171">Na hello **FirmPlay - hájící zájmy zaměstnanec pro přijetí konfigurace** klikněte na tlačítko **konfigurace FirmPlay - hájící zájmy zaměstnanec pro nábor** tooopen **konfigurovat přihlašování**dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9aa91-171">On hello **FirmPlay - Employee Advocacy for Recruiting Configuration** section, click **Configure FirmPlay - Employee Advocacy for Recruiting** tooopen **Configure sign-on** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. <span data-ttu-id="9aa91-174">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) a poskytnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="9aa91-174">tooget SSO configured for your application, contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) and provide them with hello following:</span></span> 

    <span data-ttu-id="9aa91-175">• hello Stáhnout **soubor certifikátu**</span><span class="sxs-lookup"><span data-stu-id="9aa91-175">•  hello downloaded **Certificate file**</span></span>

    <span data-ttu-id="9aa91-176">• hello **SAML jeden přihlašování adresa URL služby**</span><span class="sxs-lookup"><span data-stu-id="9aa91-176">•  hello **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="9aa91-177">• hello **SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="9aa91-177">•  hello **SAML Entity ID**</span></span>

    <span data-ttu-id="9aa91-178">• hello **Sign-Out adresy URL**</span><span class="sxs-lookup"><span data-stu-id="9aa91-178">•  hello **Sign-Out URL**</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9aa91-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9aa91-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="9aa91-180">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9aa91-180">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9aa91-182">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9aa91-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9aa91-183">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9aa91-183">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9aa91-185">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9aa91-185">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9aa91-187">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9aa91-187">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9aa91-189">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9aa91-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9aa91-191">a.</span><span class="sxs-lookup"><span data-stu-id="9aa91-191">a.</span></span> <span data-ttu-id="9aa91-192">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9aa91-193">b.</span><span class="sxs-lookup"><span data-stu-id="9aa91-193">b.</span></span> <span data-ttu-id="9aa91-194">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9aa91-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9aa91-195">c.</span><span class="sxs-lookup"><span data-stu-id="9aa91-195">c.</span></span> <span data-ttu-id="9aa91-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9aa91-197">d.</span><span class="sxs-lookup"><span data-stu-id="9aa91-197">d.</span></span> <span data-ttu-id="9aa91-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-198">Click **Create**.</span></span> 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a><span data-ttu-id="9aa91-199">Vytváření FirmPlay - hájící zájmy zaměstnanec pro nábor testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9aa91-199">Creating a FirmPlay - Employee Advocacy for Recruiting test user</span></span>

<span data-ttu-id="9aa91-200">V této části vytvoříte volal Britta Simon v FirmPlay - hájící zájmy zaměstnanec pro nábor uživatele.</span><span class="sxs-lookup"><span data-stu-id="9aa91-200">In this section, you create a user called Britta Simon in FirmPlay - Employee Advocacy for Recruiting.</span></span> <span data-ttu-id="9aa91-201">Spojte se s [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) tooadd hello uživatele v hello FirmPlay - hájící zájmy zaměstnanec pro nábor platformu.</span><span class="sxs-lookup"><span data-stu-id="9aa91-201">Please work with [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) tooadd hello users in hello FirmPlay - Employee Advocacy for Recruiting platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9aa91-202">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9aa91-202">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9aa91-203">V této části povolíte tak, že udělíte svůj přístup tooFirmPlay - hájící zájmy zaměstnanec pro nábor Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9aa91-203">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFirmPlay - Employee Advocacy for Recruiting.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9aa91-205">**tooassign Britta Simon tooFirmPlay - hájící zájmy zaměstnanec pro nábor, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9aa91-205">**tooassign Britta Simon tooFirmPlay - Employee Advocacy for Recruiting, perform hello following steps:**</span></span>

1. <span data-ttu-id="9aa91-206">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-206">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9aa91-208">V seznamu aplikace hello vyberte **FirmPlay - hájící zájmy zaměstnanec pro nábor**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-208">In hello applications list, select **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. <span data-ttu-id="9aa91-210">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9aa91-210">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9aa91-212">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9aa91-212">Click **Add** button.</span></span> <span data-ttu-id="9aa91-213">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9aa91-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9aa91-215">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9aa91-215">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9aa91-216">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9aa91-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9aa91-217">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9aa91-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="9aa91-218">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9aa91-218">Testing single sign-on</span></span>

<span data-ttu-id="9aa91-219">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9aa91-219">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9aa91-220">Po kliknutí na tlačítko hello FirmPlay - hájící zájmy zaměstnanec pro dlaždici nábor v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour FirmPlay - hájící zájmy zaměstnanec nábor aplikace.</span><span class="sxs-lookup"><span data-stu-id="9aa91-220">When you click hello FirmPlay - Employee Advocacy for Recruiting tile in hello Access Panel, you should get automatically signed-on tooyour FirmPlay - Employee Advocacy for Recruiting application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9aa91-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9aa91-221">Additional resources</span></span>

* [<span data-ttu-id="9aa91-222">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9aa91-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9aa91-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9aa91-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png