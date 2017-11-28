---
title: "Kurz: Azure Active Directory integrace s plošší soubory | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a plošší soubory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 73ca2613b7bbaf9992ecf624ff5defabaa44f7a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a><span data-ttu-id="dd66a-103">Kurz: Azure Active Directory integrace s plošší soubory</span><span class="sxs-lookup"><span data-stu-id="dd66a-103">Tutorial: Azure Active Directory integration with Flatter Files</span></span>

<span data-ttu-id="dd66a-104">V tomto kurzu zjistíte, jak toointegrate plošší soubory s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dd66a-104">In this tutorial, you learn how toointegrate Flatter Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dd66a-105">Integrace plošší soubory s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="dd66a-105">Integrating Flatter Files with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="dd66a-106">Můžete řídit ve službě Azure AD, který má přístup tooFlatter soubory</span><span class="sxs-lookup"><span data-stu-id="dd66a-106">You can control in Azure AD who has access tooFlatter Files</span></span>
- <span data-ttu-id="dd66a-107">Můžete povolit tooautomatically vaši uživatelé získat soubory přihlášeného tooFlatter (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="dd66a-107">You can enable your users tooautomatically get signed-on tooFlatter Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dd66a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dd66a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="dd66a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dd66a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd66a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dd66a-110">Prerequisites</span></span>

<span data-ttu-id="dd66a-111">tooconfigure integrace Azure AD s plošší soubory, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="dd66a-111">tooconfigure Azure AD integration with Flatter Files, you need hello following items:</span></span>

- <span data-ttu-id="dd66a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="dd66a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dd66a-113">Plošší soubory jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="dd66a-113">A Flatter Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dd66a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="dd66a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dd66a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="dd66a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dd66a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="dd66a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dd66a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dd66a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dd66a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="dd66a-118">Scenario description</span></span>
<span data-ttu-id="dd66a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="dd66a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dd66a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="dd66a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dd66a-121">Přidávání plošší souborů z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="dd66a-121">Adding Flatter Files from hello gallery</span></span>
2. <span data-ttu-id="dd66a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dd66a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-flatter-files-from-hello-gallery"></a><span data-ttu-id="dd66a-123">Přidávání plošší souborů z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="dd66a-123">Adding Flatter Files from hello gallery</span></span>
<span data-ttu-id="dd66a-124">tooconfigure hello integrace plošší souborů do Azure AD, je nutné tooadd plošší soubory hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="dd66a-124">tooconfigure hello integration of Flatter Files into Azure AD, you need tooadd Flatter Files from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="dd66a-125">**tooadd plošší soubory z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dd66a-125">**tooadd Flatter Files from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd66a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dd66a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dd66a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="dd66a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="dd66a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd66a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="dd66a-133">Hello vyhledávacího pole zadejte **plošší soubory**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-133">In hello search box, type **Flatter Files**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_search.png)

5. <span data-ttu-id="dd66a-135">Na panelu výsledků hello vyberte **plošší soubory**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd66a-135">In hello results panel, select **Flatter Files**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dd66a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dd66a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dd66a-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s plošší soubory podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="dd66a-138">In this section, you configure and test Azure AD single sign-on with Flatter Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dd66a-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v plošší souborů je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dd66a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Flatter Files is tooa user in Azure AD.</span></span> <span data-ttu-id="dd66a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v souborech plošší musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="dd66a-140">In other words, a link relationship between an Azure AD user and hello related user in Flatter Files needs toobe established.</span></span>

<span data-ttu-id="dd66a-141">V souborech plošší, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="dd66a-141">In Flatter Files, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="dd66a-142">tooconfigure a testu Azure AD jednotné přihlašování s plošší soubory, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="dd66a-142">tooconfigure and test Azure AD single sign-on with Flatter Files, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="dd66a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="dd66a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="dd66a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dd66a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dd66a-145">**[Vytvoření zkušebního uživatele plošší soubory](#creating-a-flatter-files-test-user)**  -toohave protějšek Britta Simon v plošší soubory, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="dd66a-145">**[Creating a Flatter Files test user](#creating-a-flatter-files-test-user)** - toohave a counterpart of Britta Simon in Flatter Files that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="dd66a-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dd66a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dd66a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="dd66a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dd66a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dd66a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dd66a-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci plošší soubory.</span><span class="sxs-lookup"><span data-stu-id="dd66a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Flatter Files application.</span></span>

<span data-ttu-id="dd66a-150">**tooconfigure Azure AD jednotné přihlašování s plošší soubory, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dd66a-150">**tooconfigure Azure AD single sign-on with Flatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd66a-151">V portálu Azure, na hello hello **plošší soubory** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-151">In hello Azure portal, on hello **Flatter Files** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="dd66a-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dd66a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

3. <span data-ttu-id="dd66a-155">Na hello **plošší soubory domény a adresy URL** část, hello uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="dd66a-155">On hello **Flatter Files Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
4. <span data-ttu-id="dd66a-157">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="dd66a-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

5. <span data-ttu-id="dd66a-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd66a-159">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dd66a-161">Na hello **plošší soubory konfigurace** klikněte na tlačítko **konfigurace plošší soubory** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="dd66a-161">On hello **Flatter Files Configuration** section, click **Configure Flatter Files** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="dd66a-162">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="dd66a-162">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

7. <span data-ttu-id="dd66a-164">Přihlášení tooyour plošší soubory aplikace jako správce.</span><span class="sxs-lookup"><span data-stu-id="dd66a-164">Sign-on tooyour Flatter Files application as an administrator.</span></span>

8. <span data-ttu-id="dd66a-165">Klikněte na tlačítko **řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-165">Click **DASHBOARD**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  

9. <span data-ttu-id="dd66a-167">Klikněte na tlačítko **nastavení**a poté proveďte následující kroky na hello hello **společnosti** karty:</span><span class="sxs-lookup"><span data-stu-id="dd66a-167">Click **Settings**, and then perform hello following steps on hello **Company** tab:</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    <span data-ttu-id="dd66a-169">a.</span><span class="sxs-lookup"><span data-stu-id="dd66a-169">a.</span></span> <span data-ttu-id="dd66a-170">Vyberte **pomocí SAML 2.0 pro ověřování**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-170">Select **Use SAML 2.0 for Authentication**.</span></span>
    
    <span data-ttu-id="dd66a-171">b.</span><span class="sxs-lookup"><span data-stu-id="dd66a-171">b.</span></span> <span data-ttu-id="dd66a-172">Klikněte na tlačítko **konfigurace SAML**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-172">Click **Configure SAML**.</span></span>

8. <span data-ttu-id="dd66a-173">Na hello **konfigurace SAML** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="dd66a-173">On hello **SAML Configuration** dialog, perform hello following steps:</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    <span data-ttu-id="dd66a-175">a.</span><span class="sxs-lookup"><span data-stu-id="dd66a-175">a.</span></span> <span data-ttu-id="dd66a-176">V hello **domény** textovému poli, zadejte registrované domény.</span><span class="sxs-lookup"><span data-stu-id="dd66a-176">In hello **Domain** textbox, type your registered domain.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="dd66a-177">Pokud nemáte registrované domény ještě nebyla, obraťte se na podporu plošší soubory team prostřednictvím [ support@flatterfiles.com ](mailto:support@flatterfiles.com).</span><span class="sxs-lookup"><span data-stu-id="dd66a-177">If you don't have a registered domain yet, contact your Flatter Files support team via [support@flatterfiles.com](mailto:support@flatterfiles.com).</span></span> 
    
    <span data-ttu-id="dd66a-178">b.</span><span class="sxs-lookup"><span data-stu-id="dd66a-178">b.</span></span> <span data-ttu-id="dd66a-179">V **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali formuláři portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dd66a-179">In **Identity Provider URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied form Azure portal.</span></span>
   
    <span data-ttu-id="dd66a-180">c.</span><span class="sxs-lookup"><span data-stu-id="dd66a-180">c.</span></span>  <span data-ttu-id="dd66a-181">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="dd66a-181">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="dd66a-182">d.</span><span class="sxs-lookup"><span data-stu-id="dd66a-182">d.</span></span> <span data-ttu-id="dd66a-183">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-183">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="dd66a-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="dd66a-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="dd66a-185">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="dd66a-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="dd66a-186">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dd66a-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dd66a-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dd66a-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="dd66a-188">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="dd66a-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="dd66a-190">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dd66a-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd66a-191">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dd66a-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dd66a-193">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dd66a-195">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="dd66a-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dd66a-197">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dd66a-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dd66a-199">a.</span><span class="sxs-lookup"><span data-stu-id="dd66a-199">a.</span></span> <span data-ttu-id="dd66a-200">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dd66a-201">b.</span><span class="sxs-lookup"><span data-stu-id="dd66a-201">b.</span></span> <span data-ttu-id="dd66a-202">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dd66a-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dd66a-203">c.</span><span class="sxs-lookup"><span data-stu-id="dd66a-203">c.</span></span> <span data-ttu-id="dd66a-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="dd66a-205">d.</span><span class="sxs-lookup"><span data-stu-id="dd66a-205">d.</span></span> <span data-ttu-id="dd66a-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-206">Click **Create**.</span></span>
 
### <a name="creating-a-flatter-files-test-user"></a><span data-ttu-id="dd66a-207">Vytvoření zkušebního uživatele plošší soubory</span><span class="sxs-lookup"><span data-stu-id="dd66a-207">Creating a Flatter Files test user</span></span>

<span data-ttu-id="dd66a-208">Hello cílem této části je toocreate volal Britta Simon v plošší soubory uživatele.</span><span class="sxs-lookup"><span data-stu-id="dd66a-208">hello objective of this section is toocreate a user called Britta Simon in Flatter Files.</span></span>

<span data-ttu-id="dd66a-209">**toocreate uživatel volal Britta Simon v plošší soubory, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dd66a-209">**toocreate a user called Britta Simon in Flatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd66a-210">Přihlaste se tooyour **plošší soubory** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="dd66a-210">Sign on tooyour **Flatter Files** company site as administrator.</span></span>

2. <span data-ttu-id="dd66a-211">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **nastavení**a potom klikněte na hello **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="dd66a-211">In hello navigation pane on hello left, click **Settings**, and then click hello **Users** tab.</span></span>
   
    ![Vytvoření plošší soubory uživatele](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. <span data-ttu-id="dd66a-213">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-213">Click **Add User**.</span></span> 

4. <span data-ttu-id="dd66a-214">Na hello **přidat uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="dd66a-214">On hello **Add User** dialog, perform hello following steps:</span></span>
   
    ![Vytvoření plošší soubory uživatele](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    <span data-ttu-id="dd66a-216">a.</span><span class="sxs-lookup"><span data-stu-id="dd66a-216">a.</span></span> <span data-ttu-id="dd66a-217">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-217">In hello **First Name** textbox, type **Britta**.</span></span>
   
    <span data-ttu-id="dd66a-218">b.</span><span class="sxs-lookup"><span data-stu-id="dd66a-218">b.</span></span> <span data-ttu-id="dd66a-219">V hello **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-219">In hello **Last Name** textbox, type **Simon**.</span></span> 
   
    <span data-ttu-id="dd66a-220">c.</span><span class="sxs-lookup"><span data-stu-id="dd66a-220">c.</span></span> <span data-ttu-id="dd66a-221">V hello **e-mailovou adresu** textovému poli, zadejte e-mailovou adresu na Britta hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dd66a-221">In hello **Email Address** textbox, type Britta's email address in hello Azure portal.</span></span>
   
    <span data-ttu-id="dd66a-222">d.</span><span class="sxs-lookup"><span data-stu-id="dd66a-222">d.</span></span> <span data-ttu-id="dd66a-223">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-223">Click **Submit**.</span></span>   


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="dd66a-224">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="dd66a-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="dd66a-225">V této části povolíte Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup k souborům tooFlatter.</span><span class="sxs-lookup"><span data-stu-id="dd66a-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFlatter Files.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="dd66a-227">**tooassign Britta Simon tooFlatter soubory pak provádějí hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dd66a-227">**tooassign Britta Simon tooFlatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="dd66a-228">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="dd66a-230">V seznamu aplikace hello vyberte **plošší soubory**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-230">In hello applications list, select **Flatter Files**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_app.png) 

3. <span data-ttu-id="dd66a-232">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="dd66a-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="dd66a-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dd66a-234">Click **Add** button.</span></span> <span data-ttu-id="dd66a-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dd66a-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="dd66a-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="dd66a-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="dd66a-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dd66a-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dd66a-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dd66a-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dd66a-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dd66a-240">Testing single sign-on</span></span>

<span data-ttu-id="dd66a-241">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="dd66a-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="dd66a-242">Po kliknutí na tlačítko hello plošší soubory dlaždici v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného plošší soubory aplikace.</span><span class="sxs-lookup"><span data-stu-id="dd66a-242">When you click hello Flatter Files tile in hello Access Panel, you should get automatically signed-on tooyour Flatter Files application.</span></span>
<span data-ttu-id="dd66a-243">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dd66a-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd66a-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dd66a-244">Additional resources</span></span>

* [<span data-ttu-id="dd66a-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dd66a-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dd66a-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dd66a-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png

