---
title: "Kurz: Azure Active Directory integrace s SensoScientific bezdrátové teploty monitorování systému | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SensoScientific bezdrátové teploty monitorování systému."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: 4eabf7fc6457c217fd5c0c2539ab88c8110055e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a><span data-ttu-id="17876-103">Kurz: Azure Active Directory integrace s SensoScientific bezdrátové teploty monitorování systému</span><span class="sxs-lookup"><span data-stu-id="17876-103">Tutorial: Azure Active Directory integration with SensoScientific Wireless Temperature Monitoring System</span></span>

<span data-ttu-id="17876-104">V tomto kurzu zjistíte, jak toointegrate SensoScientific bezdrátové teploty monitorování systém s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="17876-104">In this tutorial, you learn how toointegrate SensoScientific Wireless Temperature Monitoring System with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="17876-105">Integrace SensoScientific bezdrátové teploty monitorování systému s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="17876-105">Integrating SensoScientific Wireless Temperature Monitoring System with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="17876-106">Můžete řídit ve službě Azure AD, který má přístup tooSensoScientific bezdrátové teploty monitorování systému</span><span class="sxs-lookup"><span data-stu-id="17876-106">You can control in Azure AD who has access tooSensoScientific Wireless Temperature Monitoring System</span></span>
- <span data-ttu-id="17876-107">Vaši uživatelé tooautomatically get přihlášeného tooSensoScientific bezdrátové teploty monitorování systému (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="17876-107">You can enable your users tooautomatically get signed-on tooSensoScientific Wireless Temperature Monitoring System (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="17876-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="17876-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="17876-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="17876-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17876-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="17876-110">Prerequisites</span></span>

<span data-ttu-id="17876-111">tooconfigure integrace Azure AD s SensoScientific bezdrátové teploty monitorování systému, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="17876-111">tooconfigure Azure AD integration with SensoScientific Wireless Temperature Monitoring System, you need hello following items:</span></span>

- <span data-ttu-id="17876-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="17876-112">An Azure AD subscription</span></span>
- <span data-ttu-id="17876-113">SensoScientific bezdrátové teploty monitorování systému jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="17876-113">A SensoScientific Wireless Temperature Monitoring System single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="17876-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="17876-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="17876-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="17876-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="17876-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="17876-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="17876-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="17876-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="17876-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="17876-118">Scenario description</span></span>
<span data-ttu-id="17876-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="17876-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="17876-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="17876-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="17876-121">Přidání SensoScientific bezdrátové teploty monitorování systému z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="17876-121">Adding SensoScientific Wireless Temperature Monitoring System from hello gallery</span></span>
2. <span data-ttu-id="17876-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="17876-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-hello-gallery"></a><span data-ttu-id="17876-123">Přidání SensoScientific bezdrátové teploty monitorování systému z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="17876-123">Adding SensoScientific Wireless Temperature Monitoring System from hello gallery</span></span>
<span data-ttu-id="17876-124">tooconfigure hello integrace SensoScientific bezdrátové teploty monitorování systému do Azure AD, je nutné tooadd SensoScientific bezdrátové teploty monitorování systému hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="17876-124">tooconfigure hello integration of SensoScientific Wireless Temperature Monitoring System into Azure AD, you need tooadd SensoScientific Wireless Temperature Monitoring System from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="17876-125">**tooadd SensoScientific bezdrátové teploty monitorování systému z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17876-125">**tooadd SensoScientific Wireless Temperature Monitoring System from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="17876-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="17876-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="17876-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="17876-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="17876-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="17876-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="17876-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17876-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="17876-133">Hello vyhledávacího pole zadejte **SensoScientific bezdrátové teploty monitorování systému**.</span><span class="sxs-lookup"><span data-stu-id="17876-133">In hello search box, type **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

5. <span data-ttu-id="17876-135">Na panelu výsledků hello vyberte **SensoScientific bezdrátové teploty monitorování systému**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="17876-135">In hello results panel, select **SensoScientific Wireless Temperature Monitoring System**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="17876-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="17876-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="17876-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SensoScientific bezdrátové teploty monitorování systému podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="17876-138">In this section, you configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="17876-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v SensoScientific bezdrátové teploty monitorování systému je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="17876-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SensoScientific Wireless Temperature Monitoring System is tooa user in Azure AD.</span></span> <span data-ttu-id="17876-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SensoScientific bezdrátové teploty monitorování systému musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="17876-140">In other words, a link relationship between an Azure AD user and hello related user in SensoScientific Wireless Temperature Monitoring System needs toobe established.</span></span>

<span data-ttu-id="17876-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="17876-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SensoScientific Wireless Temperature Monitoring System.</span></span>

<span data-ttu-id="17876-142">tooconfigure a testu Azure AD jednotné přihlašování s SensoScientific bezdrátové teploty monitorování systému, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="17876-142">tooconfigure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="17876-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="17876-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="17876-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="17876-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="17876-145">**[Vytvoření zkušebního uživatele SensoScientific bezdrátové teploty monitorování systému](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  -toohave protějšek Britta Simon v SensoScientific bezdrátové teploty monitorování systému, který je propojený reprezentace toohello Azure AD uživatel.</span><span class="sxs-lookup"><span data-stu-id="17876-145">**[Creating a SensoScientific Wireless Temperature Monitoring System test user](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)** - toohave a counterpart of Britta Simon in SensoScientific Wireless Temperature Monitoring System that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="17876-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17876-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="17876-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="17876-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="17876-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="17876-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="17876-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="17876-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SensoScientific Wireless Temperature Monitoring System application.</span></span>

<span data-ttu-id="17876-150">**tooconfigure Azure AD jednotné přihlašování s SensoScientific bezdrátové teploty monitorování systému, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17876-150">**tooconfigure Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, perform hello following steps:**</span></span>

1. <span data-ttu-id="17876-151">V portálu Azure, na hello hello **SensoScientific bezdrátové teploty monitorování systému** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="17876-151">In hello Azure portal, on hello **SensoScientific Wireless Temperature Monitoring System** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="17876-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17876-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

3. <span data-ttu-id="17876-155">Na hello **SensoScientific bezdrátové teploty monitorování systému domény a adresy URL** části, bez nutnosti tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure:</span><span class="sxs-lookup"><span data-stu-id="17876-155">On hello **SensoScientific Wireless Temperature Monitoring System Domain and URLs** section, no need tooperform any steps as hello app is already pre-integrated with Azure:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

4. <span data-ttu-id="17876-157">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="17876-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

5. <span data-ttu-id="17876-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17876-159">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="17876-161">Na hello **SensoScientific bezdrátové teploty monitorování System Configuration** klikněte na tlačítko **nakonfigurujte systém monitorování SensoScientific bezdrátové teploty** tooopen  **Konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="17876-161">On hello **SensoScientific Wireless Temperature Monitoring System Configuration** section, click **Configure SensoScientific Wireless Temperature Monitoring System** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="17876-162">Kopírování hello **Sign-Out adresu URL, SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="17876-162">Copy hello **Sign-Out URL, SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

7. <span data-ttu-id="17876-164">Přihlášení tooyour aplikaci SensoScientific bezdrátové teploty monitorování systému jako správce.</span><span class="sxs-lookup"><span data-stu-id="17876-164">Sign on tooyour SensoScientific Wireless Temperature Monitoring System application as an administrator.</span></span>

8. <span data-ttu-id="17876-165">V navigační nabídce hello hello nahoře, klikněte na **konfigurace** a goto **konfigurace** pod **jednotné přihlašování** tooopen hello jeden znak na nastavení.</span><span class="sxs-lookup"><span data-stu-id="17876-165">In hello navigation menu on hello top, click **Configuration** and goto **Configure** under **Single Sign On** tooopen hello Single Sign On Settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

9. <span data-ttu-id="17876-167">V **jeden znak v nastavení** formuláře provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="17876-167">In **Single Sign On Settings** form perform hello following steps:</span></span>
 
    <span data-ttu-id="17876-168">a.</span><span class="sxs-lookup"><span data-stu-id="17876-168">a.</span></span> <span data-ttu-id="17876-169">Vyberte **název vystavitele** jako Azure AD.</span><span class="sxs-lookup"><span data-stu-id="17876-169">Select **Issuer Name** as Azure AD.</span></span>
    
    <span data-ttu-id="17876-170">b.</span><span class="sxs-lookup"><span data-stu-id="17876-170">b.</span></span> <span data-ttu-id="17876-171">Vložení hello **SAML Entity ID** který jste zkopírovali z portálu Azure do pole Adresa URL vystavitele.</span><span class="sxs-lookup"><span data-stu-id="17876-171">Paste hello **SAML Entity ID** which you have copied from Azure portal into Issuer URL textbox.</span></span>
    
    <span data-ttu-id="17876-172">c.</span><span class="sxs-lookup"><span data-stu-id="17876-172">c.</span></span> <span data-ttu-id="17876-173">Vložení hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure do jednoho přihlášení adresa URL služby textového pole.</span><span class="sxs-lookup"><span data-stu-id="17876-173">Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal into Single Sign-On Service URL textbox.</span></span>

    <span data-ttu-id="17876-174">d.</span><span class="sxs-lookup"><span data-stu-id="17876-174">d.</span></span> <span data-ttu-id="17876-175">Vložení hello **Sign-Out URL** který jste zkopírovali z portálu Azure do jedné adresy URL služby Sign-Out textové pole.</span><span class="sxs-lookup"><span data-stu-id="17876-175">Paste hello **Sign-Out URL** which you have copied from Azure portal into Single Sign-Out Service URL textbox.</span></span>

    <span data-ttu-id="17876-176">e.</span><span class="sxs-lookup"><span data-stu-id="17876-176">e.</span></span> <span data-ttu-id="17876-177">Procházejte hello certifikát, který jste si stáhli z portálu Azure a sem odeslání.</span><span class="sxs-lookup"><span data-stu-id="17876-177">Browse hello certificate which you have downloaded from Azure portal and upload here.</span></span>
    
    <span data-ttu-id="17876-178">f.</span><span class="sxs-lookup"><span data-stu-id="17876-178">f.</span></span> <span data-ttu-id="17876-179">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="17876-179">Click **Save**.</span></span>
  
> [!TIP]
> <span data-ttu-id="17876-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="17876-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="17876-181">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="17876-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="17876-182">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="17876-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="17876-183">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="17876-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="17876-184">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="17876-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="17876-186">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17876-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="17876-187">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="17876-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="17876-189">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="17876-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="17876-191">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="17876-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="17876-193">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="17876-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="17876-195">a.</span><span class="sxs-lookup"><span data-stu-id="17876-195">a.</span></span> <span data-ttu-id="17876-196">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="17876-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="17876-197">b.</span><span class="sxs-lookup"><span data-stu-id="17876-197">b.</span></span> <span data-ttu-id="17876-198">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="17876-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="17876-199">c.</span><span class="sxs-lookup"><span data-stu-id="17876-199">c.</span></span> <span data-ttu-id="17876-200">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="17876-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="17876-201">d.</span><span class="sxs-lookup"><span data-stu-id="17876-201">d.</span></span> <span data-ttu-id="17876-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="17876-202">Click **Create**.</span></span>
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a><span data-ttu-id="17876-203">Vytvoření zkušebního uživatele SensoScientific bezdrátové teploty monitorování systému</span><span class="sxs-lookup"><span data-stu-id="17876-203">Creating a SensoScientific Wireless Temperature Monitoring System test user</span></span>

<span data-ttu-id="17876-204">Uživatelé toolog tooenable Azure AD v tooSensoScientific bezdrátové teploty monitorování systému, se musí být zřízená do SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="17876-204">tooenable Azure AD users toolog in tooSensoScientific Wireless Temperature Monitoring System, they must be provisioned into SensoScientific Wireless Temperature Monitoring System.</span></span> <span data-ttu-id="17876-205">Práce s [tým podpory SensoScientific bezdrátové teploty monitorování systému](https://www.sensoscientific.com/contact-us/) pro přidání uživatelů hello hello SensoScientific bezdrátové teploty monitorování systému platformu.</span><span class="sxs-lookup"><span data-stu-id="17876-205">Work with [SensoScientific Wireless Temperature Monitoring System support team](https://www.sensoscientific.com/contact-us/) to add hello users in hello SensoScientific Wireless Temperature Monitoring System platform.</span></span> <span data-ttu-id="17876-206">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17876-206">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="17876-207">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="17876-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="17876-208">V této části povolíte tak, že udělíte přístup tooSensoScientific bezdrátové teploty monitorování systému toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="17876-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSensoScientific Wireless Temperature Monitoring System.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="17876-210">**tooassign Britta Simon tooSensoScientific bezdrátové teploty monitorování systému, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="17876-210">**tooassign Britta Simon tooSensoScientific Wireless Temperature Monitoring System, perform hello following steps:**</span></span>

1. <span data-ttu-id="17876-211">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="17876-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="17876-213">V seznamu aplikace hello vyberte **SensoScientific bezdrátové teploty monitorování systému**.</span><span class="sxs-lookup"><span data-stu-id="17876-213">In hello applications list, select **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

3. <span data-ttu-id="17876-215">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="17876-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="17876-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="17876-217">Click **Add** button.</span></span> <span data-ttu-id="17876-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17876-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="17876-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="17876-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="17876-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17876-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="17876-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="17876-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="17876-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="17876-223">Testing single sign-on</span></span>

<span data-ttu-id="17876-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="17876-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> <span data-ttu-id="17876-225">Klikněte na dlaždici hello SensoScientific bezdrátové teploty sledování systému v hello přístupového panelu, bude aplikace automaticky přihlášeného tooyour SensoScientific bezdrátové teploty monitorování systému.</span><span class="sxs-lookup"><span data-stu-id="17876-225">Click hello SensoScientific Wireless Temperature Monitoring System tile in hello Access Panel, you will be automatically signed-on tooyour SensoScientific Wireless Temperature Monitoring System application.</span></span> <span data-ttu-id="17876-226">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="17876-226">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17876-227">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="17876-227">Additional resources</span></span>

* [<span data-ttu-id="17876-228">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="17876-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="17876-229">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="17876-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_203.png

