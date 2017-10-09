---
title: "Kurz: Integrace Azure Active Directory pomocí konzoly pro správu Mimecast | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Mimecast konzoly pro správu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5a04a5abd9ff30d484bce0a5c97a1d3e48b69e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a><span data-ttu-id="e7b68-103">Kurz: Azure Active Directory integrace s Mimecast konzoly pro správu</span><span class="sxs-lookup"><span data-stu-id="e7b68-103">Tutorial: Azure Active Directory integration with Mimecast Admin Console</span></span>

<span data-ttu-id="e7b68-104">V tomto kurzu zjistíte, jak toointegrate Mimecast konzoly pro správu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e7b68-104">In this tutorial, you learn how toointegrate Mimecast Admin Console with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e7b68-105">Integrace konzoly pro správu Mimecast s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e7b68-105">Integrating Mimecast Admin Console with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e7b68-106">Můžete ovládat ve službě Azure AD, který má přístup tooMimecast konzoly pro správu.</span><span class="sxs-lookup"><span data-stu-id="e7b68-106">You can control in Azure AD who has access tooMimecast Admin Console.</span></span>
- <span data-ttu-id="e7b68-107">Vaši uživatelé tooautomatically get přihlášeného tooMimecast konzoly pro správu (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7b68-107">You can enable your users tooautomatically get signed-on tooMimecast Admin Console (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e7b68-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e7b68-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="e7b68-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e7b68-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7b68-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e7b68-110">Prerequisites</span></span>

<span data-ttu-id="e7b68-111">tooconfigure integrace Azure AD s Mimecast konzoly pro správu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e7b68-111">tooconfigure Azure AD integration with Mimecast Admin Console, you need hello following items:</span></span>

- <span data-ttu-id="e7b68-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e7b68-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e7b68-113">Konzole pro správu Mimecast jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e7b68-113">A Mimecast Admin Console single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e7b68-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e7b68-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e7b68-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e7b68-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e7b68-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e7b68-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e7b68-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e7b68-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e7b68-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e7b68-118">Scenario description</span></span>
<span data-ttu-id="e7b68-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e7b68-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e7b68-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e7b68-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e7b68-121">Přidání Mimecast konzoly pro správu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e7b68-121">Adding Mimecast Admin Console from hello gallery</span></span>
2. <span data-ttu-id="e7b68-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7b68-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-admin-console-from-hello-gallery"></a><span data-ttu-id="e7b68-123">Přidání Mimecast konzoly pro správu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e7b68-123">Adding Mimecast Admin Console from hello gallery</span></span>
<span data-ttu-id="e7b68-124">tooconfigure hello integrace Mimecast konzoly pro správu do služby Azure AD, je nutné tooadd konzoly pro správu Mimecast hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e7b68-124">tooconfigure hello integration of Mimecast Admin Console into Azure AD, you need tooadd Mimecast Admin Console from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e7b68-125">**tooadd Mimecast konzoly pro správu z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e7b68-125">**tooadd Mimecast Admin Console from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e7b68-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e7b68-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="e7b68-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e7b68-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="e7b68-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e7b68-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="e7b68-133">Hello vyhledávacího pole zadejte **konzoly pro správu Mimecast**, vyberte **konzoly pro správu Mimecast** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e7b68-133">In hello search box, type **Mimecast Admin Console**, select **Mimecast Admin Console** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Mimecast konzoly pro správu v seznamu výsledků hello](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e7b68-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7b68-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e7b68-136">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí konzoly pro správu Mimecast podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e7b68-136">In this section, you configure and test Azure AD single sign-on with Mimecast Admin Console based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e7b68-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v konzole pro správu Mimecast je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7b68-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mimecast Admin Console is tooa user in Azure AD.</span></span> <span data-ttu-id="e7b68-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v konzole pro správu Mimecast musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="e7b68-138">In other words, a link relationship between an Azure AD user and hello related user in Mimecast Admin Console needs toobe established.</span></span>

<span data-ttu-id="e7b68-139">V konzole pro správu Mimecast přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="e7b68-139">In Mimecast Admin Console, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e7b68-140">tooconfigure a testu Azure AD jednotné přihlašování s Mimecast konzoly pro správu, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="e7b68-140">tooconfigure and test Azure AD single sign-on with Mimecast Admin Console, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e7b68-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="e7b68-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e7b68-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e7b68-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e7b68-143">**[Vytvoření zkušebního uživatele konzoly pro správu Mimecast](#create-a-mimecast-admin-console-test-user)**  -toohave protějšek Britta Simon v konzole pro správu Mimecast, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="e7b68-143">**[Create a Mimecast Admin Console test user](#create-a-mimecast-admin-console-test-user)** - toohave a counterpart of Britta Simon in Mimecast Admin Console that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e7b68-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7b68-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e7b68-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="e7b68-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e7b68-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7b68-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e7b68-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Mimecast konzoly pro správu.</span><span class="sxs-lookup"><span data-stu-id="e7b68-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mimecast Admin Console application.</span></span>

<span data-ttu-id="e7b68-148">**tooconfigure Azure AD jednotné přihlašování s Mimecast konzoly pro správu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e7b68-148">**tooconfigure Azure AD single sign-on with Mimecast Admin Console, perform hello following steps:**</span></span>

1. <span data-ttu-id="e7b68-149">V portálu Azure, na hello hello **konzoly pro správu Mimecast** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-149">In hello Azure portal, on hello **Mimecast Admin Console** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="e7b68-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7b68-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. <span data-ttu-id="e7b68-153">Na hello **Mimecast správce konzoly domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e7b68-153">On hello **Mimecast Admin Console Domain and URLs** section, perform hello following steps:</span></span>

    ![Mimecast správce konzoly domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    <span data-ttu-id="e7b68-155">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:</span><span class="sxs-lookup"><span data-stu-id="e7b68-155">In hello **Sign-on URL** textbox, type hello URL:</span></span>
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > <span data-ttu-id="e7b68-156">Adresa URL přihlašování Hello je konkrétní oblasti.</span><span class="sxs-lookup"><span data-stu-id="e7b68-156">hello sign on URL is region specific.</span></span>

4. <span data-ttu-id="e7b68-157">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e7b68-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. <span data-ttu-id="e7b68-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e7b68-159">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e7b68-161">Na hello **konfigurace konzoly správy Mimecast** klikněte na tlačítko **konfigurace konzoly pro správu Mimecast** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e7b68-161">On hello **Mimecast Admin Console Configuration** section, click **Configure Mimecast Admin Console** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e7b68-162">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e7b68-162">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace konzoly správy Mimecast](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. <span data-ttu-id="e7b68-164">V okně prohlížeče jiný web Přihlaste se jako správce do konzoly Správce Mimecast.</span><span class="sxs-lookup"><span data-stu-id="e7b68-164">In a different web browser window, log into your Mimecast Admin Console as an administrator.</span></span>

8. <span data-ttu-id="e7b68-165">Přejděte příliš**služby \> aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-165">Go too**Services \> Application**.</span></span>

    <span data-ttu-id="e7b68-166">![Služby](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "služby")</span><span class="sxs-lookup"><span data-stu-id="e7b68-166">![Services](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "Services")</span></span>

9. <span data-ttu-id="e7b68-167">Klikněte na tlačítko **ověřování profily**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-167">Click **Authentication Profiles**.</span></span>

    <span data-ttu-id="e7b68-168">![Profily ověřování](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "profily ověřování")</span><span class="sxs-lookup"><span data-stu-id="e7b68-168">![Authentication Profiles](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "Authentication Profiles")</span></span>
    
10. <span data-ttu-id="e7b68-169">Klikněte na tlačítko **nový profil ověřování**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-169">Click **New Authentication Profile**.</span></span>

    <span data-ttu-id="e7b68-170">![Nové profily ověřování](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "nové profily ověřování")</span><span class="sxs-lookup"><span data-stu-id="e7b68-170">![New Authentication Profiles](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "New Authentication Profiles")</span></span>

11. <span data-ttu-id="e7b68-171">V hello **profil ověření** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e7b68-171">In hello **Authentication Profile** section, perform hello following steps:</span></span>

    <span data-ttu-id="e7b68-172">![Profil ověření](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "profil ověření")</span><span class="sxs-lookup"><span data-stu-id="e7b68-172">![Authentication Profile](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "Authentication Profile")</span></span>
    
    <span data-ttu-id="e7b68-173">a.</span><span class="sxs-lookup"><span data-stu-id="e7b68-173">a.</span></span> <span data-ttu-id="e7b68-174">V hello **popis** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e7b68-174">In hello **Description** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="e7b68-175">b.</span><span class="sxs-lookup"><span data-stu-id="e7b68-175">b.</span></span> <span data-ttu-id="e7b68-176">Vyberte **vynutit ověřování SAML pro konzolu Správce Mimecast**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-176">Select **Enforce SAML Authentication for Mimecast Admin Console**.</span></span>
    
    <span data-ttu-id="e7b68-177">c.</span><span class="sxs-lookup"><span data-stu-id="e7b68-177">c.</span></span> <span data-ttu-id="e7b68-178">Jako **zprostředkovatele**, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-178">As **Provider**, select **Azure Active Directory**.</span></span>
    
    <span data-ttu-id="e7b68-179">d.</span><span class="sxs-lookup"><span data-stu-id="e7b68-179">d.</span></span> <span data-ttu-id="e7b68-180">Vložení **SAML Entity ID**, který jste zkopírovali z hello portálu Azure do hello **URL vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e7b68-180">Paste **SAML Entity ID**, which you have copied from hello Azure portal into hello **Issuer URL** textbox.</span></span>
    
    <span data-ttu-id="e7b68-181">e.</span><span class="sxs-lookup"><span data-stu-id="e7b68-181">e.</span></span> <span data-ttu-id="e7b68-182">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e7b68-182">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Login URL** textbox.</span></span>

    <span data-ttu-id="e7b68-183">f.</span><span class="sxs-lookup"><span data-stu-id="e7b68-183">f.</span></span> <span data-ttu-id="e7b68-184">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e7b68-184">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Logout URL** textbox.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="e7b68-185">Hello přihlašovací adresa URL hodnota a hodnota adresy URL odhlašovací hello jsou pro hello konzoly pro správu Mimecast hello stejné.</span><span class="sxs-lookup"><span data-stu-id="e7b68-185">hello Login URL value and hello Logout URL value are for hello Mimecast Admin Console hello same.</span></span>
    
    <span data-ttu-id="e7b68-186">g.</span><span class="sxs-lookup"><span data-stu-id="e7b68-186">g.</span></span> <span data-ttu-id="e7b68-187">Kódování base-64 certifikát stažený z portálu Azure v poznámkovém bloku otevřete odebrat hello prvního řádku ("*--*") a poslední řádek hello ("*--*"), hello kopie zbývající obsahu je do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity (Metadata)** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e7b68-187">Open your base-64 certificate downloaded from Azure portal in notepad, remove hello first line (“*--*“) and hello last line (“*--*“), copy hello remaining content of it into your clipboard, and then paste it toohello **Identity Provider Certificate (Metadata)** textbox.</span></span>
    
    <span data-ttu-id="e7b68-188">h.</span><span class="sxs-lookup"><span data-stu-id="e7b68-188">h.</span></span> <span data-ttu-id="e7b68-189">Vyberte **povolit jednotné přihlašování na**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-189">Select **Allow Single Sign On**.</span></span>
    
    <span data-ttu-id="e7b68-190">i.</span><span class="sxs-lookup"><span data-stu-id="e7b68-190">i.</span></span> <span data-ttu-id="e7b68-191">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e7b68-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="e7b68-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e7b68-193">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e7b68-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e7b68-194">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e7b68-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e7b68-195">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e7b68-195">Create an Azure AD test user</span></span>

<span data-ttu-id="e7b68-196">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="e7b68-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="e7b68-198">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e7b68-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e7b68-199">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e7b68-199">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e7b68-201">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-201">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e7b68-203">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b68-203">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e7b68-205">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e7b68-205">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e7b68-207">a.</span><span class="sxs-lookup"><span data-stu-id="e7b68-207">a.</span></span> <span data-ttu-id="e7b68-208">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-208">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e7b68-209">b.</span><span class="sxs-lookup"><span data-stu-id="e7b68-209">b.</span></span> <span data-ttu-id="e7b68-210">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e7b68-210">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="e7b68-211">c.</span><span class="sxs-lookup"><span data-stu-id="e7b68-211">c.</span></span> <span data-ttu-id="e7b68-212">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="e7b68-212">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="e7b68-213">d.</span><span class="sxs-lookup"><span data-stu-id="e7b68-213">d.</span></span> <span data-ttu-id="e7b68-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-214">Click **Create**.</span></span>
 
### <a name="create-a-mimecast-admin-console-test-user"></a><span data-ttu-id="e7b68-215">Vytvoření zkušebního uživatele Mimecast konzoly pro správu</span><span class="sxs-lookup"><span data-stu-id="e7b68-215">Create a Mimecast Admin Console test user</span></span>

<span data-ttu-id="e7b68-216">V pořadí tooenable Azure AD Uživatelé toolog do konzoly pro správu Mimecast musí být zřízená do konzoly pro správu Mimecast.</span><span class="sxs-lookup"><span data-stu-id="e7b68-216">In order tooenable Azure AD users toolog into Mimecast Admin Console, they must be provisioned into Mimecast Admin Console.</span></span> <span data-ttu-id="e7b68-217">V případě hello z konzoly pro správu Mimecast zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="e7b68-217">In hello case of Mimecast Admin Console, provisioning is a manual task.</span></span>

* <span data-ttu-id="e7b68-218">Než můžete vytvořit uživatele, musíte tooregister domény.</span><span class="sxs-lookup"><span data-stu-id="e7b68-218">You need tooregister a domain before you can create users.</span></span>

<span data-ttu-id="e7b68-219">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e7b68-219">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="e7b68-220">Přihlaste se tooyour **konzoly pro správu Mimecast** jako správce.</span><span class="sxs-lookup"><span data-stu-id="e7b68-220">Sign on tooyour **Mimecast Admin Console** as administrator.</span></span>
2. <span data-ttu-id="e7b68-221">Přejděte příliš**adresáře \> interní**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-221">Go too**Directories \> Internal**.</span></span>
   
   <span data-ttu-id="e7b68-222">![Adresáře](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "adresáře")</span><span class="sxs-lookup"><span data-stu-id="e7b68-222">![Directories](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "Directories")</span></span>
3. <span data-ttu-id="e7b68-223">Klikněte na tlačítko **zaregistrovat nové domény**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-223">Click **Register New Domain**.</span></span>
   
   <span data-ttu-id="e7b68-224">![Zaregistrujte novou doménu](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "zaregistrovat nové domény")</span><span class="sxs-lookup"><span data-stu-id="e7b68-224">![Register New Domain](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "Register New Domain")</span></span>
4. <span data-ttu-id="e7b68-225">Po vytvoření nové domény, klikněte na tlačítko **novou adresu**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-225">After your new domain has been created, click **New Address**.</span></span>
   
   <span data-ttu-id="e7b68-226">![Nové adresy](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "novou adresu")</span><span class="sxs-lookup"><span data-stu-id="e7b68-226">![New Address](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "New Address")</span></span>
5. <span data-ttu-id="e7b68-227">V hello adresu dialogové okno Nový proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e7b68-227">In hello new address dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="e7b68-228">![Uložit](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "uložit")</span><span class="sxs-lookup"><span data-stu-id="e7b68-228">![Save](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "Save")</span></span>
   
   <span data-ttu-id="e7b68-229">a.</span><span class="sxs-lookup"><span data-stu-id="e7b68-229">a.</span></span> <span data-ttu-id="e7b68-230">Typ hello **e-mailovou adresu**, **globální název**, **heslo**, a **Potvrdit heslo** atributy platný Azure AD účet má tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="e7b68-230">Type hello **Email Address**, **Global Name**, **Password**, and **Confirm Password** attributes of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span>

   <span data-ttu-id="e7b68-231">b.</span><span class="sxs-lookup"><span data-stu-id="e7b68-231">b.</span></span> <span data-ttu-id="e7b68-232">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-232">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="e7b68-233">Můžete použít jiné nástroje pro tvorbu účet uživatele konzoly pro správu Mimecast nebo rozhraní API poskytovaných konzoly pro správu Mimecast tooprovision Azure AD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e7b68-233">You can use any other Mimecast Admin Console user account creation tools or APIs provided by Mimecast Admin Console tooprovision Azure AD user accounts.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e7b68-234">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e7b68-234">Assign hello Azure AD test user</span></span>

<span data-ttu-id="e7b68-235">V této části povolíte tak, že udělíte přístup tooMimecast konzoly pro správu Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e7b68-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMimecast Admin Console.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="e7b68-237">**tooassign Britta Simon tooMimecast konzoly pro správu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e7b68-237">**tooassign Britta Simon tooMimecast Admin Console, perform hello following steps:**</span></span>

1. <span data-ttu-id="e7b68-238">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e7b68-240">V seznamu aplikace hello vyberte **konzoly pro správu Mimecast**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-240">In hello applications list, select **Mimecast Admin Console**.</span></span>

    ![Hello konzoly pro správu Mimecast odkaz v seznamu aplikace hello](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. <span data-ttu-id="e7b68-242">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e7b68-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="e7b68-244">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e7b68-244">Click **Add** button.</span></span> <span data-ttu-id="e7b68-245">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b68-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="e7b68-247">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="e7b68-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e7b68-248">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b68-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e7b68-249">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e7b68-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e7b68-250">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e7b68-250">Test single sign-on</span></span>

<span data-ttu-id="e7b68-251">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e7b68-251">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e7b68-252">Když kliknete na dlaždici konzoly pro správu Mimecast hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Mimecast konzoly pro správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="e7b68-252">When you click hello Mimecast Admin Console tile in hello Access Panel, you should get automatically signed-on tooyour Mimecast Admin Console application.</span></span>
<span data-ttu-id="e7b68-253">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e7b68-253">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e7b68-254">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e7b68-254">Additional resources</span></span>

* [<span data-ttu-id="e7b68-255">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e7b68-255">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e7b68-256">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e7b68-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

