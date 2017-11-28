---
title: 'Kurz: Azure Active Directory integrace s Datahug | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Datahug."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: 79ccb939c7a19720bcf696af141f747db00c8a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a><span data-ttu-id="63167-103">Kurz: Azure Active Directory integrace s Datahug</span><span class="sxs-lookup"><span data-stu-id="63167-103">Tutorial: Azure Active Directory integration with Datahug</span></span>

<span data-ttu-id="63167-104">V tomto kurzu zjistíte, jak toointegrate Datahug s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="63167-104">In this tutorial, you learn how toointegrate Datahug with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="63167-105">Integrace Datahug s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="63167-105">Integrating Datahug with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="63167-106">Můžete řídit ve službě Azure AD, který má přístup tooDatahug</span><span class="sxs-lookup"><span data-stu-id="63167-106">You can control in Azure AD who has access tooDatahug</span></span>
- <span data-ttu-id="63167-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDatahug (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="63167-107">You can enable your users tooautomatically get signed-on tooDatahug (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="63167-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="63167-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="63167-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="63167-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="63167-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="63167-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63167-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="63167-111">Prerequisites</span></span>

<span data-ttu-id="63167-112">Integrace služby Azure AD s Datahug tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="63167-112">tooconfigure Azure AD integration with Datahug, you need hello following items:</span></span>

- <span data-ttu-id="63167-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="63167-113">An Azure AD subscription</span></span>
- <span data-ttu-id="63167-114">Datahug jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="63167-114">A Datahug single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="63167-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="63167-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="63167-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="63167-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="63167-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="63167-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="63167-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="63167-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="63167-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="63167-119">Scenario description</span></span>
<span data-ttu-id="63167-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="63167-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="63167-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="63167-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="63167-122">Přidání Datahug z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="63167-122">Adding Datahug from hello gallery</span></span>
2. <span data-ttu-id="63167-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="63167-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-datahug-from-hello-gallery"></a><span data-ttu-id="63167-124">Přidání Datahug z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="63167-124">Adding Datahug from hello gallery</span></span>
<span data-ttu-id="63167-125">tooconfigure hello integrace Datahug do Azure AD, je nutné tooadd Datahug hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="63167-125">tooconfigure hello integration of Datahug into Azure AD, you need tooadd Datahug from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="63167-126">**tooadd Datahug z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63167-126">**tooadd Datahug from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="63167-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="63167-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="63167-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="63167-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="63167-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="63167-130">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="63167-132">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="63167-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="63167-134">Hello vyhledávacího pole zadejte **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="63167-134">In hello search box, type **Datahug**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. <span data-ttu-id="63167-136">Na panelu výsledků hello vyberte **Datahug**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="63167-136">In hello results panel, select **Datahug**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="63167-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="63167-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="63167-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Datahug podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="63167-139">In this section, you configure and test Azure AD single sign-on with Datahug based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="63167-140">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Datahug je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="63167-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Datahug is tooa user in Azure AD.</span></span> <span data-ttu-id="63167-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Datahug musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="63167-141">In other words, a link relationship between an Azure AD user and hello related user in Datahug needs toobe established.</span></span>

<span data-ttu-id="63167-142">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Datahug.</span><span class="sxs-lookup"><span data-stu-id="63167-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Datahug.</span></span>

<span data-ttu-id="63167-143">tooconfigure a testu Azure AD jednotné přihlašování s Datahug, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="63167-143">tooconfigure and test Azure AD single sign-on with Datahug, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="63167-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="63167-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="63167-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="63167-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="63167-146">**[Vytvoření zkušebního uživatele Datahug](#creating-a-datahug-test-user)**  -toohave protějšek Britta Simon v Datahug, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="63167-146">**[Creating a Datahug test user](#creating-a-datahug-test-user)** - toohave a counterpart of Britta Simon in Datahug that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="63167-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="63167-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="63167-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="63167-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="63167-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="63167-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="63167-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Datahug.</span><span class="sxs-lookup"><span data-stu-id="63167-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Datahug application.</span></span>

<span data-ttu-id="63167-151">**tooconfigure Azure AD jednotné přihlašování s Datahug, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63167-151">**tooconfigure Azure AD single sign-on with Datahug, perform hello following steps:**</span></span>

1. <span data-ttu-id="63167-152">V portálu Azure, na hello hello **Datahug** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="63167-152">In hello Azure portal, on hello **Datahug** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="63167-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="63167-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. <span data-ttu-id="63167-156">Na hello **Datahug domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="63167-156">On hello **Datahug Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    <span data-ttu-id="63167-158">a.</span><span class="sxs-lookup"><span data-stu-id="63167-158">a.</span></span> <span data-ttu-id="63167-159">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://apps.datahug.com/identity/<uniqueID>`</span><span class="sxs-lookup"><span data-stu-id="63167-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://apps.datahug.com/identity/<uniqueID>`</span></span>

    <span data-ttu-id="63167-160">b.</span><span class="sxs-lookup"><span data-stu-id="63167-160">b.</span></span> <span data-ttu-id="63167-161">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://apps.datahug.com/identity/<uniqueID>/acs`</span><span class="sxs-lookup"><span data-stu-id="63167-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://apps.datahug.com/identity/<uniqueID>/acs`</span></span>

4. <span data-ttu-id="63167-162">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="63167-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="63167-163">Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="63167-163">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    <span data-ttu-id="63167-165">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://apps.datahug.com/`</span><span class="sxs-lookup"><span data-stu-id="63167-165">In hello **Sign-on URL** textbox, type a URL as: `https://apps.datahug.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="63167-166">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="63167-166">These values are not hello real.</span></span> <span data-ttu-id="63167-167">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="63167-167">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="63167-168">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="63167-168">Here we suggest you toouse hello unique value of string in hello Identifier and Reply URL.</span></span> <span data-ttu-id="63167-169">Obraťte se na [tým podpory Datahug klienta](http://datahug.com/about/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="63167-169">Contact [Datahug Client support team](http://datahug.com/about/contact-us/) tooget these values.</span></span> 

5. <span data-ttu-id="63167-170">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="63167-170">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  <span data-ttu-id="63167-172">Zkontrolujte **"Zobrazit upřesňující nastavení podpisový certifikát"** a provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="63167-172">Check **“Show advanced certificate signing settings”** and perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    <span data-ttu-id="63167-174">a.</span><span class="sxs-lookup"><span data-stu-id="63167-174">a.</span></span> <span data-ttu-id="63167-175">V **podepisování možnost**, vyberte **kontrolního výrazu SAML přihlašovací**.</span><span class="sxs-lookup"><span data-stu-id="63167-175">In **Signing Option**, select **Sign SAML assertion**.</span></span>
    
    <span data-ttu-id="63167-176">b.</span><span class="sxs-lookup"><span data-stu-id="63167-176">b.</span></span> <span data-ttu-id="63167-177">V **podpisového algoritmu**, vyberte **SHA1**.</span><span class="sxs-lookup"><span data-stu-id="63167-177">In **Signing algorithm**, select **SHA1**.</span></span>
 
7. <span data-ttu-id="63167-178">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="63167-178">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="63167-180">Na hello **Datahug konfigurace** klikněte na tlačítko **konfigurace Datahug** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="63167-180">On hello **Datahug Configuration** section, click **Configure Datahug** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="63167-181">Kopírování hello **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="63167-181">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. <span data-ttu-id="63167-183">tooconfigure jednotného přihlašování na **Datahug** straně, je nutné stáhnout hello toosend **soubor XML s metadaty**, **SAML Entity ID** a **SAML-služby přihlášení Adresa URL** příliš[Datahug podporu](http://datahug.com/about/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="63167-183">tooconfigure single sign-on on **Datahug** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** too[Datahug support](http://datahug.com/about/contact-us/).</span></span> <span data-ttu-id="63167-184">Nastavují tuto aplikaci toohave hello jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="63167-184">They set this application up toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="63167-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="63167-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="63167-186">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="63167-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="63167-187">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="63167-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="63167-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="63167-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="63167-189">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="63167-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="63167-191">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63167-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="63167-192">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="63167-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="63167-194">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="63167-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="63167-196">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="63167-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="63167-198">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="63167-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="63167-200">a.</span><span class="sxs-lookup"><span data-stu-id="63167-200">a.</span></span> <span data-ttu-id="63167-201">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="63167-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="63167-202">b.</span><span class="sxs-lookup"><span data-stu-id="63167-202">b.</span></span> <span data-ttu-id="63167-203">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="63167-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="63167-204">c.</span><span class="sxs-lookup"><span data-stu-id="63167-204">c.</span></span> <span data-ttu-id="63167-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="63167-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="63167-206">d.</span><span class="sxs-lookup"><span data-stu-id="63167-206">d.</span></span> <span data-ttu-id="63167-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="63167-207">Click **Create**.</span></span>
 
### <a name="creating-a-datahug-test-user"></a><span data-ttu-id="63167-208">Vytvoření zkušebního uživatele Datahug</span><span class="sxs-lookup"><span data-stu-id="63167-208">Creating a Datahug test user</span></span>

<span data-ttu-id="63167-209">Uživatelé toolog tooenable Azure AD v tooDatahug, se musí být zřízená do Datahug.</span><span class="sxs-lookup"><span data-stu-id="63167-209">tooenable Azure AD users toolog in tooDatahug, they must be provisioned into Datahug.</span></span>  
<span data-ttu-id="63167-210">Při zřizování Datahug, je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="63167-210">When Datahug, provisioning is a manual task.</span></span>

<span data-ttu-id="63167-211">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63167-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="63167-212">Přihlaste se tooyour Datahug společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="63167-212">Log in tooyour Datahug company site as an administrator.</span></span>

2. <span data-ttu-id="63167-213">Pozastavte ukazatel myši nad hello **ozubené kolo** v horním pravém rohu hello a klikněte na **nastavení**</span><span class="sxs-lookup"><span data-stu-id="63167-213">Hover over hello **cog** in hello top right-hand corner and click **Settings**</span></span>
   
   ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/1.png)

3. <span data-ttu-id="63167-215">Zvolte **osoby** a klikněte na tlačítko hello **přidat uživatele** karta</span><span class="sxs-lookup"><span data-stu-id="63167-215">Choose **People** and click hello **Add Users** tab</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/2.png)

4. <span data-ttu-id="63167-217">Zadejte e-mailu hello hello osoby, které byste chtěli toocreate na účtu a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="63167-217">Type hello email of hello person you would like toocreate an account for and click **Add**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > <span data-ttu-id="63167-219">Můžete odeslat e-mailu toouser registrace výběrem **odeslání Uvítacího e-mailu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="63167-219">You can send registration mail toouser by selecting **Send welcome email** checkbox.</span></span>  
    > <span data-ttu-id="63167-220">Při vytváření účtu služby Salesforce Neodesílat hello uvítací e-mail.</span><span class="sxs-lookup"><span data-stu-id="63167-220">If you are creating an account for Salesforce do not send hello welcome email.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="63167-221">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="63167-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="63167-222">V této části povolíte tak, že udělíte přístup tooDatahug toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="63167-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDatahug.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="63167-224">**tooassign Britta Simon tooDatahug, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63167-224">**tooassign Britta Simon tooDatahug, perform hello following steps:**</span></span>

1. <span data-ttu-id="63167-225">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="63167-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="63167-227">V seznamu aplikace hello vyberte **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="63167-227">In hello applications list, select **Datahug**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. <span data-ttu-id="63167-229">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="63167-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="63167-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="63167-231">Click **Add** button.</span></span> <span data-ttu-id="63167-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="63167-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="63167-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="63167-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="63167-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="63167-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="63167-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="63167-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="63167-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="63167-237">Testing single sign-on</span></span>

<span data-ttu-id="63167-238">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="63167-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="63167-239">Když kliknete na dlaždici Datahug hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Datahug aplikace.</span><span class="sxs-lookup"><span data-stu-id="63167-239">When you click hello Datahug tile in hello Access Panel, you should get automatically signed-on tooyour Datahug application.</span></span> <span data-ttu-id="63167-240">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="63167-240">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="63167-241">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="63167-241">Additional resources</span></span>

* [<span data-ttu-id="63167-242">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63167-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="63167-243">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="63167-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png

