---
title: 'Kurz: Azure Active Directory integrace s ClickTime | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ClickTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: a0259e31164cad6c6c77ed8aac1c50cd9a3e46ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a><span data-ttu-id="d3b5e-103">Kurz: Azure Active Directory integrace s ClickTime</span><span class="sxs-lookup"><span data-stu-id="d3b5e-103">Tutorial: Azure Active Directory integration with ClickTime</span></span>

<span data-ttu-id="d3b5e-104">V tomto kurzu zjistíte, jak toointegrate ClickTime s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d3b5e-104">In this tutorial, you learn how toointegrate ClickTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d3b5e-105">Integrace ClickTime s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-105">Integrating ClickTime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d3b5e-106">Můžete řídit ve službě Azure AD, který má přístup tooClickTime</span><span class="sxs-lookup"><span data-stu-id="d3b5e-106">You can control in Azure AD who has access tooClickTime</span></span>
- <span data-ttu-id="d3b5e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooClickTime (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3b5e-107">You can enable your users tooautomatically get signed-on tooClickTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d3b5e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d3b5e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d3b5e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d3b5e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3b5e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d3b5e-110">Prerequisites</span></span>

<span data-ttu-id="d3b5e-111">Integrace služby Azure AD s ClickTime tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-111">tooconfigure Azure AD integration with ClickTime, you need hello following items:</span></span>

- <span data-ttu-id="d3b5e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3b5e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d3b5e-113">ClickTime jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d3b5e-113">A ClickTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d3b5e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d3b5e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d3b5e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d3b5e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d3b5e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d3b5e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d3b5e-118">Scenario description</span></span>
<span data-ttu-id="d3b5e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d3b5e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d3b5e-121">Přidání ClickTime z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d3b5e-121">Adding ClickTime from hello gallery</span></span>
2. <span data-ttu-id="d3b5e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d3b5e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clicktime-from-hello-gallery"></a><span data-ttu-id="d3b5e-123">Přidání ClickTime z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="d3b5e-123">Adding ClickTime from hello gallery</span></span>
<span data-ttu-id="d3b5e-124">tooconfigure hello integrace ClickTime do Azure AD, je nutné tooadd ClickTime hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-124">tooconfigure hello integration of ClickTime into Azure AD, you need tooadd ClickTime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d3b5e-125">**tooadd ClickTime z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d3b5e-125">**tooadd ClickTime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3b5e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="d3b5e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d3b5e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="d3b5e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="d3b5e-133">Hello vyhledávacího pole zadejte **ClickTime**, vyberte **ClickTime** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-133">In hello search box, type **ClickTime**, select **ClickTime** from result panel then click **Add** button tooadd hello application.</span></span>

    ![ClickTime v seznamu výsledků hello](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d3b5e-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d3b5e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d3b5e-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ClickTime podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d3b5e-136">In this section, you configure and test Azure AD single sign-on with ClickTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d3b5e-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ClickTime je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ClickTime is tooa user in Azure AD.</span></span> <span data-ttu-id="d3b5e-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ClickTime musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-138">In other words, a link relationship between an Azure AD user and hello related user in ClickTime needs toobe established.</span></span>

<span data-ttu-id="d3b5e-139">V ClickTime, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-139">In ClickTime, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d3b5e-140">tooconfigure a testu Azure AD jednotné přihlašování s ClickTime, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-140">tooconfigure and test Azure AD single sign-on with ClickTime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d3b5e-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d3b5e-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d3b5e-143">**[Vytvoření zkušebního uživatele ClickTime](#create-a-clicktime-test-user)**  -toohave protějšek Britta Simon v ClickTime, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-143">**[Create a ClickTime test user](#create-a-clicktime-test-user)** - toohave a counterpart of Britta Simon in ClickTime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d3b5e-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d3b5e-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d3b5e-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d3b5e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d3b5e-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci ClickTime.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ClickTime application.</span></span>

<span data-ttu-id="d3b5e-148">**tooconfigure Azure AD jednotné přihlašování s ClickTime, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d3b5e-148">**tooconfigure Azure AD single sign-on with ClickTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3b5e-149">V portálu Azure, na hello hello **ClickTime** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-149">In hello Azure portal, on hello **ClickTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="d3b5e-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. <span data-ttu-id="d3b5e-153">Na hello **ClickTime domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-153">On hello **ClickTime Domain and URLs** section, perform hello following steps:</span></span>

    ![ClickTime domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    <span data-ttu-id="d3b5e-155">a.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-155">a.</span></span> <span data-ttu-id="d3b5e-156">V hello **identifikátor** textovému poli, zadejte adresu URL jako:`https://app.clicktime.com/sp/`</span><span class="sxs-lookup"><span data-stu-id="d3b5e-156">In hello **Identifier** textbox, type a URL as: `https://app.clicktime.com/sp/`</span></span>
    
    <span data-ttu-id="d3b5e-157">b.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-157">b.</span></span> <span data-ttu-id="d3b5e-158">V hello **adresa URL odpovědi** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-158">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. <span data-ttu-id="d3b5e-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. <span data-ttu-id="d3b5e-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d3b5e-163">Na hello **ClickTime konfigurace** klikněte na tlačítko **konfigurace ClickTime** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-163">On hello **ClickTime Configuration** section, click **Configure ClickTime** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d3b5e-164">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="d3b5e-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace ClickTime](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. <span data-ttu-id="d3b5e-166">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti ClickTime.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-166">In a different web browser window, log into your ClickTime company site as an administrator.</span></span>

8. <span data-ttu-id="d3b5e-167">V panelu nástrojů hello hello nahoře, klikněte na **Předvolby**a potom klikněte na **nastavení zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-167">In hello toolbar on hello top, click **Preferences**, and then click **Security Settings**.</span></span>

9. <span data-ttu-id="d3b5e-168">V hello **předvolby přihlašování** konfigurace části, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-168">In hello **Single Sign-On Preferences** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d3b5e-169">![Nastavení zabezpečení](./media/active-directory-saas-clicktime-tutorial/tic777280.png "nastavení zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="d3b5e-169">![Security Settings](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Security Settings")</span></span>
   
    <span data-ttu-id="d3b5e-170">a.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-170">a.</span></span>  <span data-ttu-id="d3b5e-171">Vyberte **povolit** přihlášení pomocí jednotné přihlašování (SSO) s **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-171">Select **Allow** sign-in using Single Sign-On (SSO) with **Azure AD**.</span></span>
   
    <span data-ttu-id="d3b5e-172">b.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-172">b.</span></span> <span data-ttu-id="d3b5e-173">V hello **koncový bod zprostředkovatele Identity** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-173">In hello **Identity Provider Endpoint** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="d3b5e-174">c.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-174">c.</span></span>  <span data-ttu-id="d3b5e-175">Otevřete hello **certifikátu s kódováním base-64** stáhli z portálu Azure v **Poznámkový blok**, zkopírujte hello obsah a pak ji vložit do hello **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-175">Open hello **base-64 encoded certificate** downloaded from Azure portal in **Notepad**, copy hello content, and then paste it into hello **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="d3b5e-176">d.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-176">d.</span></span>  <span data-ttu-id="d3b5e-177">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d3b5e-178">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="d3b5e-178">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d3b5e-179">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-179">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d3b5e-180">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d3b5e-180">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d3b5e-181">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3b5e-181">Create an Azure AD test user</span></span>
<span data-ttu-id="d3b5e-182">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-182">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="d3b5e-184">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d3b5e-184">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3b5e-185">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-185">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d3b5e-187">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-187">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d3b5e-189">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-189">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d3b5e-191">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-191">In hello **User** dialog box, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d3b5e-193">a.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-193">a.</span></span> <span data-ttu-id="d3b5e-194">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-194">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d3b5e-195">b.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-195">b.</span></span> <span data-ttu-id="d3b5e-196">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-196">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d3b5e-197">c.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-197">c.</span></span> <span data-ttu-id="d3b5e-198">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-198">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d3b5e-199">d.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-199">d.</span></span> <span data-ttu-id="d3b5e-200">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-200">Click **Create**.</span></span>
 
### <a name="create-a-clicktime-test-user"></a><span data-ttu-id="d3b5e-201">Vytvoření zkušebního uživatele ClickTime</span><span class="sxs-lookup"><span data-stu-id="d3b5e-201">Create a ClickTime test user</span></span>

<span data-ttu-id="d3b5e-202">V pořadí tooenable Azure AD Uživatelé toolog do ClickTime musí být zřízená do ClickTime.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-202">In order tooenable Azure AD users toolog into ClickTime, they must be provisioned into ClickTime.</span></span>  
<span data-ttu-id="d3b5e-203">V případě hello ClickTime zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-203">In hello case of ClickTime, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="d3b5e-204">Můžete použít všechny ostatní ClickTime uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ClickTime tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-204">You can use any other ClickTime user account creation tools or APIs provided by ClickTime tooprovision Azure AD user accounts.</span></span>

<span data-ttu-id="d3b5e-205">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d3b5e-205">**tooprovision a user account, perform hello following steps:**</span></span>
1. <span data-ttu-id="d3b5e-206">Přihlaste se tooyour **ClickTime** klienta.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-206">Log in tooyour **ClickTime** tenant.</span></span>
2. <span data-ttu-id="d3b5e-207">V panelu nástrojů hello hello nahoře, klikněte na **společnosti**a potom klikněte na **osoby**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-207">In hello toolbar on hello top, click **Company**, and then click **People**.</span></span>
   
    <span data-ttu-id="d3b5e-208">![Lidé](./media/active-directory-saas-clicktime-tutorial/tic777282.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="d3b5e-208">![People](./media/active-directory-saas-clicktime-tutorial/tic777282.png "People")</span></span>
3. <span data-ttu-id="d3b5e-209">Klikněte na tlačítko **přidat osobu**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-209">Click **Add Person**.</span></span>
   
    <span data-ttu-id="d3b5e-210">![Přidat osobu](./media/active-directory-saas-clicktime-tutorial/tic777283.png "přidat osoba")</span><span class="sxs-lookup"><span data-stu-id="d3b5e-210">![Add Person](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Add Person")</span></span>
4. <span data-ttu-id="d3b5e-211">V části nové osobě hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d3b5e-211">In hello New Person section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d3b5e-212">![Lidé](./media/active-directory-saas-clicktime-tutorial/tic777284.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="d3b5e-212">![People](./media/active-directory-saas-clicktime-tutorial/tic777284.png "People")</span></span>
   
    <span data-ttu-id="d3b5e-213">a.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-213">a.</span></span>  <span data-ttu-id="d3b5e-214">V hello **celý název** textovému poli, celý název typ uživatele jako **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-214">In hello **full name** textbox, type full name of user like **Britta Simon**.</span></span> 
  
    <span data-ttu-id="d3b5e-215">b.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-215">b.</span></span>  <span data-ttu-id="d3b5e-216">V hello **e-mailová adresa** jako typ hello e-mailu uživatele k textovému poli,  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d3b5e-216">In hello **email address** textbox, type hello email of user like **brittasimon@contoso.com**.</span></span>
       
    > [!NOTE]
    > <span data-ttu-id="d3b5e-217">Pokud chcete, můžete nastavit další vlastnosti hello nový uživatel objektu.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-217">If you want to, you can set additional properties of hello new person object.</span></span>
   
    <span data-ttu-id="d3b5e-218">c.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-218">c.</span></span>  <span data-ttu-id="d3b5e-219">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-219">Click **Save**.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="d3b5e-220">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="d3b5e-220">Assign hello Azure AD test user</span></span>

<span data-ttu-id="d3b5e-221">V této části povolíte tak, že udělíte přístup tooClickTime toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooClickTime.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="d3b5e-223">**tooassign Britta Simon tooClickTime, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d3b5e-223">**tooassign Britta Simon tooClickTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3b5e-224">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d3b5e-226">V seznamu aplikace hello vyberte **ClickTime**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-226">In hello applications list, select **ClickTime**.</span></span>

    ![V seznamu aplikace hello ClickTimne odkaz](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. <span data-ttu-id="d3b5e-228">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="d3b5e-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-230">Click **Add** button.</span></span> <span data-ttu-id="d3b5e-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="d3b5e-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d3b5e-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d3b5e-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d3b5e-236">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d3b5e-236">Test single sign-on</span></span>

<span data-ttu-id="d3b5e-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d3b5e-238">Když kliknete na dlaždici ClickTime hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ClickTime aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3b5e-238">When you click hello ClickTime tile in hello Access Panel, you should get automatically signed-on tooyour ClickTime application.</span></span>
<span data-ttu-id="d3b5e-239">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d3b5e-239">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3b5e-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d3b5e-240">Additional resources</span></span>

* [<span data-ttu-id="d3b5e-241">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d3b5e-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d3b5e-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d3b5e-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

