---
title: 'Kurz: Azure Active Directory integrace s M soubory | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a M soubory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: e1d268da53312b1d2c12e29d66b1a5b66d0cdcce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a><span data-ttu-id="22f2a-103">Kurz: Azure Active Directory integrace s M – soubory</span><span class="sxs-lookup"><span data-stu-id="22f2a-103">Tutorial: Azure Active Directory integration with M-Files</span></span>

<span data-ttu-id="22f2a-104">V tomto kurzu zjistíte, jak toointegrate M soubory s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="22f2a-104">In this tutorial, you learn how toointegrate M-Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="22f2a-105">Integrace M soubory s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="22f2a-105">Integrating M-Files with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="22f2a-106">Můžete řídit ve službě Azure AD, který má přístup tooM – soubory</span><span class="sxs-lookup"><span data-stu-id="22f2a-106">You can control in Azure AD who has access tooM-Files</span></span>
- <span data-ttu-id="22f2a-107">Můžete povolit uživatelům tooautomatically get přihlášeného tooM – soubory (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="22f2a-107">You can enable your users tooautomatically get signed-on tooM-Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="22f2a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="22f2a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="22f2a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="22f2a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22f2a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="22f2a-110">Prerequisites</span></span>

<span data-ttu-id="22f2a-111">tooconfigure integrace Azure AD se soubory M, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="22f2a-111">tooconfigure Azure AD integration with M-Files, you need hello following items:</span></span>

- <span data-ttu-id="22f2a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="22f2a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="22f2a-113">M soubory jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="22f2a-113">A M-Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="22f2a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="22f2a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="22f2a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="22f2a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="22f2a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="22f2a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="22f2a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="22f2a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="22f2a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="22f2a-118">Scenario description</span></span>
<span data-ttu-id="22f2a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="22f2a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="22f2a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="22f2a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="22f2a-121">Přidávání M souborů z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="22f2a-121">Adding M-Files from hello gallery</span></span>
2. <span data-ttu-id="22f2a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="22f2a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-m-files-from-hello-gallery"></a><span data-ttu-id="22f2a-123">Přidávání M souborů z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="22f2a-123">Adding M-Files from hello gallery</span></span>
<span data-ttu-id="22f2a-124">tooconfigure hello integrace M souborů do Azure AD, je nutné tooadd M soubory hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="22f2a-124">tooconfigure hello integration of M-Files into Azure AD, you need tooadd M-Files from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="22f2a-125">**tooadd M soubory z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="22f2a-125">**tooadd M-Files from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="22f2a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="22f2a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="22f2a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="22f2a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="22f2a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22f2a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="22f2a-133">Hello vyhledávacího pole zadejte **M soubory**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-133">In hello search box, type **M-Files**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_search.png)

5. <span data-ttu-id="22f2a-135">Na panelu výsledků hello vyberte **M soubory**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="22f2a-135">In hello results panel, select **M-Files**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="22f2a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="22f2a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="22f2a-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s M-soubory podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="22f2a-138">In this section, you configure and test Azure AD single sign-on with M-Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="22f2a-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v souborech M je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22f2a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in M-Files is tooa user in Azure AD.</span></span> <span data-ttu-id="22f2a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v souborech M musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="22f2a-140">In other words, a link relationship between an Azure AD user and hello related user in M-Files needs toobe established.</span></span>

<span data-ttu-id="22f2a-141">V souborech M, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="22f2a-141">In M-Files, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="22f2a-142">tooconfigure a testování Azure AD jednotné přihlašování se soubory M, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="22f2a-142">tooconfigure and test Azure AD single sign-on with M-Files, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="22f2a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="22f2a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="22f2a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22f2a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="22f2a-145">**[Vytvoření zkušebního uživatele M soubory](#creating-a-m-files-test-user)**  -toohave protějšek Britta Simon v souborech M, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="22f2a-145">**[Creating a M-Files test user](#creating-a-m-files-test-user)** - toohave a counterpart of Britta Simon in M-Files that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="22f2a-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="22f2a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="22f2a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="22f2a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="22f2a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="22f2a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="22f2a-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci M soubory.</span><span class="sxs-lookup"><span data-stu-id="22f2a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your M-Files application.</span></span>

<span data-ttu-id="22f2a-150">**tooconfigure Azure AD jednotné přihlašování se soubory M, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="22f2a-150">**tooconfigure Azure AD single sign-on with M-Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="22f2a-151">V portálu Azure, na hello hello **M soubory** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-151">In hello Azure portal, on hello **M-Files** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="22f2a-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="22f2a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_samlbase.png)

3. <span data-ttu-id="22f2a-155">Na hello **M soubory domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="22f2a-155">On hello **M-Files Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_url.png)

    <span data-ttu-id="22f2a-157">a.</span><span class="sxs-lookup"><span data-stu-id="22f2a-157">a.</span></span> <span data-ttu-id="22f2a-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span><span class="sxs-lookup"><span data-stu-id="22f2a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span></span>

    <span data-ttu-id="22f2a-159">b.</span><span class="sxs-lookup"><span data-stu-id="22f2a-159">b.</span></span> <span data-ttu-id="22f2a-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.cloudvault.m-files.com`</span><span class="sxs-lookup"><span data-stu-id="22f2a-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.cloudvault.m-files.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="22f2a-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="22f2a-161">These values are not real.</span></span> <span data-ttu-id="22f2a-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="22f2a-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="22f2a-163">Obraťte se na [tým podpory M soubory klienta](mailto:support@m-files.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="22f2a-163">Contact [M-Files Client support team](mailto:support@m-files.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="22f2a-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="22f2a-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_certificate.png) 

5. <span data-ttu-id="22f2a-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22f2a-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="22f2a-168">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [tým podpory M soubory](mailto:support@m-files.com) a poskytněte hello stáhnout Metadata.</span><span class="sxs-lookup"><span data-stu-id="22f2a-168">tooget SSO configured for your application, contact [M-Files support team](mailto:support@m-files.com) and provide them hello downloaded Metadata.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="22f2a-169">Pokud chcete tooconfigure jednotné přihlašování pro vás aplikace pracovní plochy M souboru, postupujte podle hello další kroky.</span><span class="sxs-lookup"><span data-stu-id="22f2a-169">Follow hello next steps if you want tooconfigure SSO for you M-File desktop application.</span></span> <span data-ttu-id="22f2a-170">Pokud chcete pouze tooconfigure jednotné přihlašování pro verzi webové M soubory nejsou potřeba žádné další kroky.</span><span class="sxs-lookup"><span data-stu-id="22f2a-170">No extra steps are required if you only want tooconfigure SSO for M-Files web version.</span></span>  

7. <span data-ttu-id="22f2a-171">Postupujte podle hello další kroky tooconfigure hello M souboru aplikace pracovní plochy tooenable jednotné přihlašování s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22f2a-171">Follow hello next steps tooconfigure hello M-File desktop application tooenable SSO with Azure AD.</span></span> <span data-ttu-id="22f2a-172">toodownload M-soubory, přejděte příliš[M soubory stáhnout](https://www.m-files.com/en/download-latest-version) stránky.</span><span class="sxs-lookup"><span data-stu-id="22f2a-172">toodownload M-Files, go too[M-Files download](https://www.m-files.com/en/download-latest-version) page.</span></span>

8. <span data-ttu-id="22f2a-173">Otevřete hello **nastavení plochy M soubory** okno.</span><span class="sxs-lookup"><span data-stu-id="22f2a-173">Open hello **M-Files Desktop Settings** window.</span></span> <span data-ttu-id="22f2a-174">Potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-174">Then, click **Add**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_10.png)

9. <span data-ttu-id="22f2a-176">Na hello **vlastnosti připojení trezoru dokumentu** okně provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22f2a-176">On hello **Document Vault Connection Properties** window, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_11.png)  

    <span data-ttu-id="22f2a-178">V části hello typ oddílu serveru hello hodnoty následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="22f2a-178">Under hello Server section type, hello values as follows:</span></span>  

    <span data-ttu-id="22f2a-179">a.</span><span class="sxs-lookup"><span data-stu-id="22f2a-179">a.</span></span> <span data-ttu-id="22f2a-180">Pro **název**, typ `<tenant-name>.cloudvault.m-files.com`.</span><span class="sxs-lookup"><span data-stu-id="22f2a-180">For **Name**, type `<tenant-name>.cloudvault.m-files.com`.</span></span> 
 
    <span data-ttu-id="22f2a-181">b.</span><span class="sxs-lookup"><span data-stu-id="22f2a-181">b.</span></span> <span data-ttu-id="22f2a-182">Pro **číslo portu**, typ **4466**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-182">For **Port Number**, type **4466**.</span></span> 

    <span data-ttu-id="22f2a-183">c.</span><span class="sxs-lookup"><span data-stu-id="22f2a-183">c.</span></span> <span data-ttu-id="22f2a-184">Pro **protokol**, vyberte **HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-184">For **Protocol**, select **HTTPS**.</span></span> 

    <span data-ttu-id="22f2a-185">d.</span><span class="sxs-lookup"><span data-stu-id="22f2a-185">d.</span></span> <span data-ttu-id="22f2a-186">V hello **ověřování** pole, vyberte **konkrétní Windows uživatele**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-186">In hello **Authentication** field, select **Specific Windows user**.</span></span> <span data-ttu-id="22f2a-187">Potom zobrazí se výzva s podpisový stránky.</span><span class="sxs-lookup"><span data-stu-id="22f2a-187">Then, you are prompted with a signing page.</span></span> <span data-ttu-id="22f2a-188">Vložte přihlašovací údaje Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22f2a-188">Insert your Azure AD credentials.</span></span> 

    <span data-ttu-id="22f2a-189">e.</span><span class="sxs-lookup"><span data-stu-id="22f2a-189">e.</span></span> <span data-ttu-id="22f2a-190">Pro hello **úložiště na serveru**, vyberte hello odpovídající úložiště na serveru.</span><span class="sxs-lookup"><span data-stu-id="22f2a-190">For hello **Vault on Server**,  select hello corresponding vault on server.</span></span>
 
    <span data-ttu-id="22f2a-191">f.</span><span class="sxs-lookup"><span data-stu-id="22f2a-191">f.</span></span> <span data-ttu-id="22f2a-192">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-192">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="22f2a-193">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="22f2a-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="22f2a-194">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="22f2a-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="22f2a-195">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="22f2a-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="22f2a-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="22f2a-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="22f2a-197">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="22f2a-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="22f2a-199">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="22f2a-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="22f2a-200">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="22f2a-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="22f2a-202">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="22f2a-204">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="22f2a-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="22f2a-206">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="22f2a-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="22f2a-208">a.</span><span class="sxs-lookup"><span data-stu-id="22f2a-208">a.</span></span> <span data-ttu-id="22f2a-209">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="22f2a-210">b.</span><span class="sxs-lookup"><span data-stu-id="22f2a-210">b.</span></span> <span data-ttu-id="22f2a-211">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="22f2a-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="22f2a-212">c.</span><span class="sxs-lookup"><span data-stu-id="22f2a-212">c.</span></span> <span data-ttu-id="22f2a-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="22f2a-214">d.</span><span class="sxs-lookup"><span data-stu-id="22f2a-214">d.</span></span> <span data-ttu-id="22f2a-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-215">Click **Create**.</span></span>
 
### <a name="creating-a-m-files-test-user"></a><span data-ttu-id="22f2a-216">Vytvoření zkušebního uživatele M – soubory</span><span class="sxs-lookup"><span data-stu-id="22f2a-216">Creating a M-Files test user</span></span>

<span data-ttu-id="22f2a-217">Hello cílem této části je toocreate uživatele volat Britta Simon v souborech M.</span><span class="sxs-lookup"><span data-stu-id="22f2a-217">hello objective of this section is toocreate a user called Britta Simon in M-Files.</span></span> <span data-ttu-id="22f2a-218">Práce s [tým podpory M soubory](mailto:support@m-files.com) tooadd hello uživatele v hello M soubory.</span><span class="sxs-lookup"><span data-stu-id="22f2a-218">Work with  [M-Files support team](mailto:support@m-files.com) tooadd hello users in hello M-Files.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="22f2a-219">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="22f2a-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="22f2a-220">V této části povolíte tak, že udělíte přístup tooM – soubory toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="22f2a-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooM-Files.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="22f2a-222">**tooassign Britta Simon tooM – soubory, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="22f2a-222">**tooassign Britta Simon tooM-Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="22f2a-223">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-223">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="22f2a-225">V seznamu aplikace hello vyberte **M soubory**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-225">In hello applications list, select **M-Files**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_app.png) 

3. <span data-ttu-id="22f2a-227">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="22f2a-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="22f2a-229">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="22f2a-229">Click **Add** button.</span></span> <span data-ttu-id="22f2a-230">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22f2a-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="22f2a-232">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="22f2a-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="22f2a-233">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22f2a-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="22f2a-234">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="22f2a-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="22f2a-235">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="22f2a-235">Testing single sign-on</span></span>

<span data-ttu-id="22f2a-236">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="22f2a-236">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="22f2a-237">Po kliknutí na tlačítko hello M soubory dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour M soubory aplikace.</span><span class="sxs-lookup"><span data-stu-id="22f2a-237">When you click hello M-Files tile in hello Access Panel, you should get automatically signed-on tooyour M-Files application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22f2a-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="22f2a-238">Additional resources</span></span>

* [<span data-ttu-id="22f2a-239">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="22f2a-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="22f2a-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="22f2a-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_203.png

