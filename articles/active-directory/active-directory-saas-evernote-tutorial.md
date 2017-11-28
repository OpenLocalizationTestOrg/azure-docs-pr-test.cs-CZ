---
title: 'Kurz: Azure Active Directory integrace s Evernote | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Evernote."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 4d7017e571ed12a0b155aa188c6b0ecb3c9898a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a><span data-ttu-id="f7acb-103">Kurz: Azure Active Directory integrace s Evernote</span><span class="sxs-lookup"><span data-stu-id="f7acb-103">Tutorial: Azure Active Directory integration with Evernote</span></span>

<span data-ttu-id="f7acb-104">V tomto kurzu zjistíte, jak toointegrate Evernote s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f7acb-104">In this tutorial, you learn how toointegrate Evernote with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f7acb-105">Integrace Evernote s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f7acb-105">Integrating Evernote with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f7acb-106">Můžete ovládat ve službě Azure AD, který má přístup tooEvernote.</span><span class="sxs-lookup"><span data-stu-id="f7acb-106">You can control in Azure AD who has access tooEvernote.</span></span>
- <span data-ttu-id="f7acb-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooEvernote (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f7acb-107">You can enable your users tooautomatically get signed-on tooEvernote (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f7acb-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f7acb-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="f7acb-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f7acb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7acb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f7acb-110">Prerequisites</span></span>

<span data-ttu-id="f7acb-111">Integrace služby Azure AD s Evernote tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f7acb-111">tooconfigure Azure AD integration with Evernote, you need hello following items:</span></span>

- <span data-ttu-id="f7acb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f7acb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f7acb-113">Evernote jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f7acb-113">An Evernote single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f7acb-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7acb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f7acb-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f7acb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f7acb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f7acb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f7acb-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f7acb-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f7acb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f7acb-118">Scenario description</span></span>
<span data-ttu-id="f7acb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7acb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f7acb-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f7acb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f7acb-121">Přidání Evernote z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f7acb-121">Adding Evernote from hello gallery</span></span>
2. <span data-ttu-id="f7acb-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f7acb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evernote-from-hello-gallery"></a><span data-ttu-id="f7acb-123">Přidání Evernote z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f7acb-123">Adding Evernote from hello gallery</span></span>
<span data-ttu-id="f7acb-124">tooconfigure hello integrace Evernote do Azure AD, je nutné tooadd Evernote hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f7acb-124">tooconfigure hello integration of Evernote into Azure AD, you need tooadd Evernote from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f7acb-125">**tooadd Evernote z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f7acb-125">**tooadd Evernote from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f7acb-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f7acb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="f7acb-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f7acb-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="f7acb-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f7acb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="f7acb-133">Hello vyhledávacího pole zadejte **Evernote**, vyberte **Evernote** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f7acb-133">In hello search box, type **Evernote**, select **Evernote** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Evernote v seznamu výsledků hello](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f7acb-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f7acb-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f7acb-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Evernote podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="f7acb-136">In this section, you configure and test Azure AD single sign-on with Evernote based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f7acb-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Evernote je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f7acb-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Evernote is tooa user in Azure AD.</span></span> <span data-ttu-id="f7acb-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Evernote musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f7acb-138">In other words, a link relationship between an Azure AD user and hello related user in Evernote needs toobe established.</span></span>

<span data-ttu-id="f7acb-139">V Evernote, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="f7acb-139">In Evernote, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f7acb-140">tooconfigure a testu Azure AD jednotné přihlašování s Evernote, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f7acb-140">tooconfigure and test Azure AD single sign-on with Evernote, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f7acb-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f7acb-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f7acb-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f7acb-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f7acb-143">**[Vytvořit testovací uživatele s Evernote](#create-an-evernote-test-user)**  -toohave protějšek Britta Simon v Evernote, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="f7acb-143">**[Create an Evernote test user](#create-an-evernote-test-user)** - toohave a counterpart of Britta Simon in Evernote that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f7acb-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f7acb-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f7acb-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f7acb-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f7acb-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f7acb-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f7acb-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Evernote.</span><span class="sxs-lookup"><span data-stu-id="f7acb-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Evernote application.</span></span>

<span data-ttu-id="f7acb-148">**tooconfigure Azure AD jednotné přihlašování s Evernote, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f7acb-148">**tooconfigure Azure AD single sign-on with Evernote, perform hello following steps:**</span></span>

1. <span data-ttu-id="f7acb-149">V portálu Azure, na hello hello **Evernote** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-149">In hello Azure portal, on hello **Evernote** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="f7acb-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f7acb-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. <span data-ttu-id="f7acb-153">Na hello **Evernote domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure v rozšíření IDP iniciované režimu hello:</span><span class="sxs-lookup"><span data-stu-id="f7acb-153">On hello **Evernote Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in IDP initiated mode:</span></span>

    ![Evernote domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    <span data-ttu-id="f7acb-155">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://www.evernote.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="f7acb-155">In hello **Identifier** textbox, type hello URL: `https://www.evernote.com/saml2`</span></span>

4. <span data-ttu-id="f7acb-156">Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a proveďte následující krok, pokud chcete aplikace hello tooconfigure hello **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="f7acb-156">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Evernote domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    <span data-ttu-id="f7acb-158">V hello **přihlásit na adrese URL** textovému poli, hello zadat adresu URL:`https://www.evernote.com/Login.action`</span><span class="sxs-lookup"><span data-stu-id="f7acb-158">In hello **Sign on URL** textbox, type hello URL: `https://www.evernote.com/Login.action`</span></span>   

5. <span data-ttu-id="f7acb-159">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f7acb-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. <span data-ttu-id="f7acb-161">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f7acb-161">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="f7acb-163">Na hello **Evernote konfigurace** klikněte na tlačítko **konfigurace Evernote** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f7acb-163">On hello **Evernote Configuration** section, click **Configure Evernote** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f7acb-164">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f7acb-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Evernote](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. <span data-ttu-id="f7acb-166">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Evernote.</span><span class="sxs-lookup"><span data-stu-id="f7acb-166">In a different web browser window, log into your Evernote company site as an administrator.</span></span>

9. <span data-ttu-id="f7acb-167">Přejděte příliš**'konzoly pro správu.**</span><span class="sxs-lookup"><span data-stu-id="f7acb-167">Go too**'Admin Console'**</span></span>

    ![Konzole pro správu](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. <span data-ttu-id="f7acb-169">Z hello **'Konzoly pro správu'**, přejděte příliš**, zabezpečení,** a vyberte **' jednotné přihlašování.**</span><span class="sxs-lookup"><span data-stu-id="f7acb-169">From hello **'Admin Console'**, go too**‘Security’** and select **‘Single Sign-On’**</span></span>

    ![Nastavení jednotného přihlašování](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. <span data-ttu-id="f7acb-171">Nakonfigurujte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f7acb-171">Configure hello following values:</span></span>

    ![Nastavení certifikátu](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    <span data-ttu-id="f7acb-173">a.</span><span class="sxs-lookup"><span data-stu-id="f7acb-173">a.</span></span>  <span data-ttu-id="f7acb-174">**Povolit jednotné přihlašování:** je ve výchozím nastavení povolené jednotné přihlašování (klikněte na tlačítko **zakázat jednotné přihlašování** tooremove hello jednotného přihlašování k požadavku)</span><span class="sxs-lookup"><span data-stu-id="f7acb-174">**Enable SSO:** SSO is enabled by default (Click **Disable Single Sign-on** tooremove hello SSO requirement)</span></span>

    <span data-ttu-id="f7acb-175">b.</span><span class="sxs-lookup"><span data-stu-id="f7acb-175">b.</span></span> <span data-ttu-id="f7acb-176">Vložení **SAML jednotné přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **URL požadavku HTTP SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f7acb-176">Paste **SAML Single sign-on Service URL** value, which you have copied from hello Azure portal into hello **SAML HTTP Request URL** textbox.</span></span>

    <span data-ttu-id="f7acb-177">c.</span><span class="sxs-lookup"><span data-stu-id="f7acb-177">c.</span></span> <span data-ttu-id="f7acb-178">Otevřete hello stažený certifikát z Azure AD v včetně "Začít certifikát" a "END CERTIFICATE" hello obsahu Poznámkový blok a zkopírujte a vložte jej do hello **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f7acb-178">Open hello downloaded certificate from Azure AD in a notepad and copy hello content including "BEGIN CERTIFICATE" and "END CERTIFICATE" and paste it into hello **X.509 Certificate** textbox.</span></span> 

    <span data-ttu-id="f7acb-179">d.Click **uložit změny**</span><span class="sxs-lookup"><span data-stu-id="f7acb-179">d.Click **Save Changes**</span></span>

> [!TIP]
> <span data-ttu-id="f7acb-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="f7acb-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f7acb-181">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="f7acb-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f7acb-182">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f7acb-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f7acb-183">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f7acb-183">Create an Azure AD test user</span></span>

<span data-ttu-id="f7acb-184">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f7acb-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="f7acb-186">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f7acb-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f7acb-187">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f7acb-187">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f7acb-189">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-189">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f7acb-191">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7acb-191">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f7acb-193">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f7acb-193">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    <span data-ttu-id="f7acb-195">a.</span><span class="sxs-lookup"><span data-stu-id="f7acb-195">a.</span></span> <span data-ttu-id="f7acb-196">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-196">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f7acb-197">b.</span><span class="sxs-lookup"><span data-stu-id="f7acb-197">b.</span></span> <span data-ttu-id="f7acb-198">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f7acb-198">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="f7acb-199">c.</span><span class="sxs-lookup"><span data-stu-id="f7acb-199">c.</span></span> <span data-ttu-id="f7acb-200">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="f7acb-200">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="f7acb-201">d.</span><span class="sxs-lookup"><span data-stu-id="f7acb-201">d.</span></span> <span data-ttu-id="f7acb-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-202">Click **Create**.</span></span>
 
### <a name="create-an-evernote-test-user"></a><span data-ttu-id="f7acb-203">Vytvořit uživatele s Evernote testu</span><span class="sxs-lookup"><span data-stu-id="f7acb-203">Create an Evernote test user</span></span>

<span data-ttu-id="f7acb-204">V pořadí tooenable Azure AD Uživatelé toolog do Evernote musí být zřízená do Evernote.</span><span class="sxs-lookup"><span data-stu-id="f7acb-204">In order tooenable Azure AD users toolog into Evernote, they must be provisioned into Evernote.</span></span>  
<span data-ttu-id="f7acb-205">V případě hello Evernote zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="f7acb-205">In hello case of Evernote, provisioning is a manual task.</span></span>

<span data-ttu-id="f7acb-206">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f7acb-206">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="f7acb-207">Přihlaste se tooyour Evernote společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="f7acb-207">Log in tooyour Evernote company site as an administrator.</span></span>

2. <span data-ttu-id="f7acb-208">Klikněte na tlačítko hello **'Konzoly pro správu'**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-208">Click hello **'Admin Console'**.</span></span>

    ![Konzole pro správu](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. <span data-ttu-id="f7acb-210">Z hello **'Konzoly pro správu'**, přejděte příliš**"Přidat uživatele,**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-210">From hello **'Admin Console'**, go too**‘Add users’**.</span></span>

    ![Přidat testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. <span data-ttu-id="f7acb-212">**Přidání členů týmu** v hello **e-mailu** textovému poli, zadejte e-mailovou adresu hello uživatelského účtu a klikněte na **pozvat.**</span><span class="sxs-lookup"><span data-stu-id="f7acb-212">**Add team members** in hello **Email** textbox, type hello email address of user account and click **Invite.**</span></span>

    ![Přidat testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. <span data-ttu-id="f7acb-214">Po odeslání pozvánky hello držitel účtu Azure Active Directory se zobrazí Pozvánka hello tooaccept e-mailu.</span><span class="sxs-lookup"><span data-stu-id="f7acb-214">After invitation is sent, hello Azure Active Directory account holder will receive an email tooaccept hello invitation.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="f7acb-215">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f7acb-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="f7acb-216">V této části povolíte tak, že udělíte přístup tooEvernote toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f7acb-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEvernote.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="f7acb-218">**tooassign Britta Simon tooEvernote, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f7acb-218">**tooassign Britta Simon tooEvernote, perform hello following steps:**</span></span>

1. <span data-ttu-id="f7acb-219">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f7acb-221">V seznamu aplikace hello vyberte **Evernote**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-221">In hello applications list, select **Evernote**.</span></span>

    ![v seznamu aplikace hello Hello Evernote odkaz](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. <span data-ttu-id="f7acb-223">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f7acb-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="f7acb-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f7acb-225">Click **Add** button.</span></span> <span data-ttu-id="f7acb-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7acb-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="f7acb-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f7acb-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f7acb-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7acb-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f7acb-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7acb-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f7acb-231">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f7acb-231">Test single sign-on</span></span>

<span data-ttu-id="f7acb-232">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f7acb-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f7acb-233">Když kliknete na dlaždici Evernote hello v hello přístupového panelu, měli byste obdržet přihlášeného tooyour Evernote aplikace.</span><span class="sxs-lookup"><span data-stu-id="f7acb-233">When you click hello Evernote tile in hello Access Panel, you should get signed-on tooyour Evernote application.</span></span> <span data-ttu-id="f7acb-234">Pomocí svého osobního účtu, budete mít přihlášení jako účtu organizace, ale pak toolog potřeba.</span><span class="sxs-lookup"><span data-stu-id="f7acb-234">You'll be logging in as an Organization account but then need toolog in with your personal account.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f7acb-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f7acb-235">Additional resources</span></span>

* [<span data-ttu-id="f7acb-236">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f7acb-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f7acb-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f7acb-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png

