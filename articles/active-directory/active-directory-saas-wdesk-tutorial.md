---
title: 'Kurz: Azure Active Directory integrace s Wdesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Wdesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 2c278a5e4f50cc362663efc0f826ff0c0fcf6e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a><span data-ttu-id="aa9f4-103">Kurz: Azure Active Directory integrace s Wdesk</span><span class="sxs-lookup"><span data-stu-id="aa9f4-103">Tutorial: Azure Active Directory integration with Wdesk</span></span>

<span data-ttu-id="aa9f4-104">V tomto kurzu zjistíte, jak toointegrate Wdesk s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="aa9f4-104">In this tutorial, you learn how toointegrate Wdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="aa9f4-105">Integrace Wdesk s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-105">Integrating Wdesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="aa9f4-106">Můžete řídit ve službě Azure AD, který má přístup tooWdesk</span><span class="sxs-lookup"><span data-stu-id="aa9f4-106">You can control in Azure AD who has access tooWdesk</span></span>
- <span data-ttu-id="aa9f4-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWdesk (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa9f4-107">You can enable your users tooautomatically get signed-on tooWdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="aa9f4-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="aa9f4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="aa9f4-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="aa9f4-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aa9f4-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa9f4-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="aa9f4-111">Prerequisites</span></span>

<span data-ttu-id="aa9f4-112">Integrace služby Azure AD s Wdesk tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-112">tooconfigure Azure AD integration with Wdesk, you need hello following items:</span></span>

- <span data-ttu-id="aa9f4-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa9f4-113">An Azure AD subscription</span></span>
- <span data-ttu-id="aa9f4-114">Wdesk jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="aa9f4-114">A Wdesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="aa9f4-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="aa9f4-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="aa9f4-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="aa9f4-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aa9f4-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="aa9f4-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="aa9f4-119">Scenario description</span></span>
<span data-ttu-id="aa9f4-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="aa9f4-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="aa9f4-122">Přidání Wdesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="aa9f4-122">Adding Wdesk from hello gallery</span></span>
2. <span data-ttu-id="aa9f4-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="aa9f4-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wdesk-from-hello-gallery"></a><span data-ttu-id="aa9f4-124">Přidání Wdesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="aa9f4-124">Adding Wdesk from hello gallery</span></span>
<span data-ttu-id="aa9f4-125">tooconfigure hello integrace Wdesk do Azure AD, je nutné tooadd Wdesk hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-125">tooconfigure hello integration of Wdesk into Azure AD, you need tooadd Wdesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="aa9f4-126">**tooadd Wdesk z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="aa9f4-126">**tooadd Wdesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa9f4-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="aa9f4-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="aa9f4-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-130">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="aa9f4-132">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="aa9f4-134">Hello vyhledávacího pole zadejte **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-134">In hello search box, type **Wdesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. <span data-ttu-id="aa9f4-136">Na panelu výsledků hello vyberte **Wdesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-136">In hello results panel, select **Wdesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="aa9f4-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="aa9f4-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="aa9f4-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Wdesk podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="aa9f4-139">In this section, you configure and test Azure AD single sign-on with Wdesk based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="aa9f4-140">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Wdesk je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Wdesk is tooa user in Azure AD.</span></span> <span data-ttu-id="aa9f4-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Wdesk musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-141">In other words, a link relationship between an Azure AD user and hello related user in Wdesk needs toobe established.</span></span>

<span data-ttu-id="aa9f4-142">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Wdesk.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Wdesk.</span></span>

<span data-ttu-id="aa9f4-143">tooconfigure a testu Azure AD jednotné přihlašování s Wdesk, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-143">tooconfigure and test Azure AD single sign-on with Wdesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="aa9f4-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="aa9f4-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="aa9f4-146">**[Vytvoření zkušebního uživatele Wdesk](#creating-a-wdesk-test-user)**  -toohave protějšek Britta Simon v Wdesk, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-146">**[Creating a Wdesk test user](#creating-a-wdesk-test-user)** - toohave a counterpart of Britta Simon in Wdesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="aa9f4-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="aa9f4-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="aa9f4-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="aa9f4-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="aa9f4-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Wdesk.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Wdesk application.</span></span>

<span data-ttu-id="aa9f4-151">**tooconfigure Azure AD jednotné přihlašování s Wdesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="aa9f4-151">**tooconfigure Azure AD single sign-on with Wdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa9f4-152">V portálu Azure, na hello hello **Wdesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-152">In hello Azure portal, on hello **Wdesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="aa9f4-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. <span data-ttu-id="aa9f4-156">Na hello **Wdesk domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** initiated režim provedení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-156">On hello **Wdesk Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    <span data-ttu-id="aa9f4-158">a.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-158">a.</span></span> <span data-ttu-id="aa9f4-159">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="aa9f4-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span></span>

    <span data-ttu-id="aa9f4-160">b.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-160">b.</span></span> <span data-ttu-id="aa9f4-161">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="aa9f4-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span></span>

4. <span data-ttu-id="aa9f4-162">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="aa9f4-163">Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-163">If you wish tooconfigure hello application in **SP** initiated mode, perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    <span data-ttu-id="aa9f4-165">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="aa9f4-165">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="aa9f4-166">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-166">These values are not real.</span></span> <span data-ttu-id="aa9f4-167">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-167">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="aa9f4-168">Tyto hodnoty z portálu WDesk získat konfiguraci hello jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-168">You get these values from WDesk portal when you configure hello SSO.</span></span> 
  
5. <span data-ttu-id="aa9f4-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. <span data-ttu-id="aa9f4-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="aa9f4-173">V okně prohlížeče jiný web, tooWdesk přihlášení jako správce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-173">In a different web browser window, login tooWdesk as a Security Administrator.</span></span>

8. <span data-ttu-id="aa9f4-174">Hello dole vlevo, klikněte na tlačítko **správce** a zvolte **správce účtu**:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-174">In hello bottom left, click **Admin** and choose **Account Admin**:</span></span>
 
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. <span data-ttu-id="aa9f4-176">V Wdesk správce, přejděte příliš**zabezpečení**, pak **SAML** > **SAML nastavení**:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-176">In Wdesk Admin, navigate too**Security**, then **SAML** > **SAML Settings**:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. <span data-ttu-id="aa9f4-178">V části **obecné nastavení**, zkontrolujte hello **povolit SAML jednotné přihlašování**:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-178">Under **General Settings**, check hello **Enable SAML Single Sign On**:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. <span data-ttu-id="aa9f4-180">V části **podrobné informace o poskytovateli služby**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-180">Under **Service Provider Details**, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      <span data-ttu-id="aa9f4-182">a.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-182">a.</span></span> <span data-ttu-id="aa9f4-183">Kopírování hello **přihlašovací adresa URL** a vložte jej do **přihlašovací adresa Url** textové pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-183">Copy hello **Login URL** and paste it in **Sign-on Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="aa9f4-184">b.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-184">b.</span></span> <span data-ttu-id="aa9f4-185">Kopírování hello **adresu Url metadat** a vložte jej do **identifikátor** textové pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-185">Copy hello **Metadata Url** and paste it in **Identifier** textbox on Azure portal.</span></span>
       
      <span data-ttu-id="aa9f4-186">c.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-186">c.</span></span> <span data-ttu-id="aa9f4-187">Kopírování hello **příjemce url** a vložte jej do **adresa Url odpovědi** textové pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-187">Copy hello **Consumer url** and paste it in **Reply Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="aa9f4-188">d.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-188">d.</span></span> <span data-ttu-id="aa9f4-189">Klikněte na tlačítko **Uložit** na portálu Azure toosave hello změny.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-189">Click **Save** on Azure portal toosave hello changes.</span></span>      

12. <span data-ttu-id="aa9f4-190">Klikněte na tlačítko **konfigurovat nastavení IdP** tooopen **upravit nastavení IdP** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-190">Click **Configure IdP Settings** tooopen **Edit IdP Settings** dialog.</span></span> <span data-ttu-id="aa9f4-191">Klikněte na tlačítko **zvolit soubor** toolocate hello **Metadata.xml** soubor uložit z portálu Azure, pak nahrajte ho.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-191">Click **Choose File** toolocate hello **Metadata.xml** file you saved from Azure portal, then upload it.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. <span data-ttu-id="aa9f4-193">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-193">Click **Save changes**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> <span data-ttu-id="aa9f4-195">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="aa9f4-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="aa9f4-196">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="aa9f4-197">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="aa9f4-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="aa9f4-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa9f4-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="aa9f4-199">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="aa9f4-201">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="aa9f4-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa9f4-202">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="aa9f4-204">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="aa9f4-206">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="aa9f4-208">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="aa9f4-210">a.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-210">a.</span></span> <span data-ttu-id="aa9f4-211">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="aa9f4-212">b.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-212">b.</span></span> <span data-ttu-id="aa9f4-213">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="aa9f4-214">c.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-214">c.</span></span> <span data-ttu-id="aa9f4-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="aa9f4-216">d.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-216">d.</span></span> <span data-ttu-id="aa9f4-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-217">Click **Create**.</span></span>
 
### <a name="creating-a-wdesk-test-user"></a><span data-ttu-id="aa9f4-218">Vytvoření zkušebního uživatele Wdesk</span><span class="sxs-lookup"><span data-stu-id="aa9f4-218">Creating a Wdesk test user</span></span>

<span data-ttu-id="aa9f4-219">Uživatelé toolog tooenable Azure AD v tooWdesk, se musí být zřízená do Wdesk.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-219">tooenable Azure AD users toolog in tooWdesk, they must be provisioned into Wdesk.</span></span> <span data-ttu-id="aa9f4-220">V Wdesk zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-220">In Wdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="aa9f4-221">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="aa9f4-221">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa9f4-222">Přihlaste se tooWdesk jako správce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-222">Log in tooWdesk as a Security Administrator.</span></span>
2. <span data-ttu-id="aa9f4-223">Přejděte příliš**správce** > **správce účtu**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-223">Navigate too**Admin** > **Account Admin**.</span></span>

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. <span data-ttu-id="aa9f4-225">Klikněte na tlačítko **členy** pod **osoby**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-225">Click **Members** under **People**.</span></span>

4. <span data-ttu-id="aa9f4-226">Klikněte na tlačítko **přidat člena** tooopen **přidat člena** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-226">Now click **Add Member** tooopen **Add Member** dialog box.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. <span data-ttu-id="aa9f4-228">V **uživatele** textové pole, zadejte uživatelské jméno hello uživatele jako  **brittasimon@contoso.com**  a klikněte na tlačítko **pokračovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-228">In **User** text box, enter hello username of user like **brittasimon@contoso.com** and click **Continue** button.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  <span data-ttu-id="aa9f4-230">Zadejte podrobnosti hello, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="aa9f4-230">Enter hello details as shown below:</span></span>
  
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    <span data-ttu-id="aa9f4-232">a.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-232">a.</span></span> <span data-ttu-id="aa9f4-233">V **e-mailu** textové pole, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="aa9f4-233">In **E-mail** text box, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="aa9f4-234">b.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-234">b.</span></span> <span data-ttu-id="aa9f4-235">V **křestní jméno** textové pole, zadejte hello křestní jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-235">In **First Name** text box, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="aa9f4-236">c.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-236">c.</span></span> <span data-ttu-id="aa9f4-237">V **příjmení** text zadejte příjmení uživatele jako hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-237">In **Last Name** text box, enter hello last name of user like **Simon**.</span></span>

7. <span data-ttu-id="aa9f4-238">Klikněte na tlačítko **uložit člen** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-238">Click **Save Member** button.</span></span>  

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="aa9f4-240">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="aa9f4-240">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="aa9f4-241">V této části povolíte tak, že udělíte přístup tooWdesk toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-241">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWdesk.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="aa9f4-243">**tooassign Britta Simon tooWdesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="aa9f4-243">**tooassign Britta Simon tooWdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa9f4-244">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-244">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="aa9f4-246">V seznamu aplikace hello vyberte **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-246">In hello applications list, select **Wdesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. <span data-ttu-id="aa9f4-248">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-248">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="aa9f4-250">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-250">Click **Add** button.</span></span> <span data-ttu-id="aa9f4-251">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="aa9f4-253">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-253">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="aa9f4-254">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="aa9f4-255">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="aa9f4-256">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="aa9f4-256">Testing single sign-on</span></span>

<span data-ttu-id="aa9f4-257">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-257">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="aa9f4-258">Když kliknete na dlaždici Wdesk hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Wdesk aplikace.</span><span class="sxs-lookup"><span data-stu-id="aa9f4-258">When you click hello Wdesk tile in hello Access Panel, you should get automatically signed-on tooyour Wdesk application.</span></span>
<span data-ttu-id="aa9f4-259">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="aa9f4-259">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="aa9f4-260">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="aa9f4-260">Additional resources</span></span>

* [<span data-ttu-id="aa9f4-261">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aa9f4-261">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="aa9f4-262">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="aa9f4-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

