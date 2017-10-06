---
title: 'Kurz: Azure Active Directory integrace s FilesAnywhere | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FilesAnywhere."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: 376364a5c75f8d069ea6390c58586acb378cd8b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a><span data-ttu-id="a4b4d-103">Kurz: Azure Active Directory integrace s FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="a4b4d-103">Tutorial: Azure Active Directory integration with FilesAnywhere</span></span>

<span data-ttu-id="a4b4d-104">V tomto kurzu zjistíte, jak toointegrate FilesAnywhere s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a4b4d-104">In this tutorial, you learn how toointegrate FilesAnywhere with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a4b4d-105">Integrace FilesAnywhere s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-105">Integrating FilesAnywhere with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a4b4d-106">Můžete řídit ve službě Azure AD, který má přístup tooFilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="a4b4d-106">You can control in Azure AD who has access tooFilesAnywhere</span></span>
- <span data-ttu-id="a4b4d-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooFilesAnywhere (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4b4d-107">You can enable your users tooautomatically get signed-on tooFilesAnywhere (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a4b4d-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="a4b4d-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="a4b4d-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a4b4d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4b4d-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a4b4d-110">Prerequisites</span></span>

<span data-ttu-id="a4b4d-111">Integrace služby Azure AD s FilesAnywhere tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-111">tooconfigure Azure AD integration with FilesAnywhere, you need hello following items:</span></span>

- <span data-ttu-id="a4b4d-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4b4d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a4b4d-113">FilesAnywhere jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a4b4d-113">A FilesAnywhere single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="a4b4d-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="a4b4d-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a4b4d-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="a4b4d-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a4b4d-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="a4b4d-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a4b4d-118">Scenario description</span></span>
<span data-ttu-id="a4b4d-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a4b4d-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a4b4d-121">Přidání FilesAnywhere z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a4b4d-121">Adding FilesAnywhere from hello gallery</span></span>
2. <span data-ttu-id="a4b4d-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4b4d-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-filesanywhere-from-hello-gallery"></a><span data-ttu-id="a4b4d-123">Přidání FilesAnywhere z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a4b4d-123">Adding FilesAnywhere from hello gallery</span></span>
<span data-ttu-id="a4b4d-124">tooconfigure hello integrace FilesAnywhere do Azure AD, je nutné tooadd FilesAnywhere hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-124">tooconfigure hello integration of FilesAnywhere into Azure AD, you need tooadd FilesAnywhere from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a4b4d-125">**tooadd FilesAnywhere z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a4b4d-125">**tooadd FilesAnywhere from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4b4d-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a4b4d-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a4b4d-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a4b4d-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a4b4d-133">Hello vyhledávacího pole zadejte **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-133">In hello search box, type **FilesAnywhere**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_search.png)

5. <span data-ttu-id="a4b4d-135">Na panelu výsledků hello vyberte **FilesAnywhere**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-135">In hello results panel, select **FilesAnywhere**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a4b4d-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4b4d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a4b4d-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FilesAnywhere podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a4b4d-138">In this section, you configure and test Azure AD single sign-on with FilesAnywhere based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a4b4d-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v FilesAnywhere je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FilesAnywhere is tooa user in Azure AD.</span></span> <span data-ttu-id="a4b4d-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v FilesAnywhere musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-140">In other words, a link relationship between an Azure AD user and hello related user in FilesAnywhere needs toobe established.</span></span>

<span data-ttu-id="a4b4d-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FilesAnywhere.</span></span>

<span data-ttu-id="a4b4d-142">tooconfigure a testu Azure AD jednotné přihlašování s FilesAnywhere, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-142">tooconfigure and test Azure AD single sign-on with FilesAnywhere, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a4b4d-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a4b4d-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a4b4d-145">**[Vytvoření zkušebního uživatele FilesAnywhere](#creating-a-filesanywhere-test-user)**  -toohave protějšek Britta Simon v FilesAnywhere, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-145">**[Creating a FilesAnywhere test user](#creating-a-filesanywhere-test-user)** - toohave a counterpart of Britta Simon in FilesAnywhere that is linked toohello Azure AD representation of her.</span></span>
3. <span data-ttu-id="a4b4d-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
4. <span data-ttu-id="a4b4d-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a4b4d-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4b4d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a4b4d-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FilesAnywhere application.</span></span>

<span data-ttu-id="a4b4d-150">**tooconfigure Azure AD jednotné přihlašování s FilesAnywhere, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a4b4d-150">**tooconfigure Azure AD single sign-on with FilesAnywhere, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4b4d-151">V hello Azure Management portal na hello **FilesAnywhere** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-151">In hello Azure Management portal, on hello **FilesAnywhere** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a4b4d-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

3. <span data-ttu-id="a4b4d-155">Na hello **FilesAnywhere domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-155">On hello **FilesAnywhere Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url.png)
    
    <span data-ttu-id="a4b4d-157">a.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-157">a.</span></span> <span data-ttu-id="a4b4d-158">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span><span class="sxs-lookup"><span data-stu-id="a4b4d-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span></span>
> [!NOTE]
> <span data-ttu-id="a4b4d-159">Poznámka: Tato hodnota hello **215** je **clientid** a je jenom jako příklad.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-159">Please note that hello value **215** is a **clientid** and is just an example.</span></span> <span data-ttu-id="a4b4d-160">Je třeba tooreplace je hodnota skutečného clientid hello.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-160">You need tooreplace it with hello actual clientid value.</span></span>

4. <span data-ttu-id="a4b4d-161">Na hello **FilesAnywhere domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-161">On hello **FilesAnywhere Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url1.png)

    <span data-ttu-id="a4b4d-163">a.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-163">a.</span></span> <span data-ttu-id="a4b4d-164">Klikněte na hello **zobrazit upřesňující nastavení adresy URL** možnost</span><span class="sxs-lookup"><span data-stu-id="a4b4d-164">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="a4b4d-165">b.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-165">b.</span></span> <span data-ttu-id="a4b4d-166">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<sub domain>.filesanywhere.com/`</span><span class="sxs-lookup"><span data-stu-id="a4b4d-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<sub domain>.filesanywhere.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a4b4d-167">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-167">Please note that these are not hello real values.</span></span> <span data-ttu-id="a4b4d-168">Máte tooupdate tyto hodnoty pomocí hello skutečné přihlašovací adresa URL a odpovědi adresy URL.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-168">You have tooupdate these values with hello actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="a4b4d-169">Obraťte se na [tým podpory FilesAnywhere](mailto:support@FilesAnywhere.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-169">Contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) tooget these values.</span></span> 

5. <span data-ttu-id="a4b4d-170">FilesAnywhere softwarová aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-170">FilesAnywhere Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="a4b4d-171">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-171">Please configure hello following claims for this application.</span></span> <span data-ttu-id="a4b4d-172">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-172">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="a4b4d-173">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-173">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    <span data-ttu-id="a4b4d-175">Když uživatelé přihlásí do s FilesAnywhere získají hello hodnotu hello **clientid** atribut z [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span><span class="sxs-lookup"><span data-stu-id="a4b4d-175">When hello users signs up with FilesAnywhere they get hello value of **clientid** attribute from [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span></span> <span data-ttu-id="a4b4d-176">Máte s hello poskytované FilesAnywhere jedinečnou hodnotu atributu "Id klienta" hello tooadd.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-176">You have tooadd hello "Client Id" attribute with hello unique value provided by FilesAnywhere.</span></span> <span data-ttu-id="a4b4d-177">Všechny tyto atributy, které jsou uvedené výše se vyžadují.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-177">All these attributes shown above are required.</span></span>
    > [!NOTE] 
    > <span data-ttu-id="a4b4d-178">Poznámka: Tato hodnota hello **2331** z **clientid** je jenom jako příklad.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-178">Please note that hello value **2331** of **clientid** is just an example.</span></span> <span data-ttu-id="a4b4d-179">Je nutné tooprovide hello skutečnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-179">You need tooprovide hello actual value.</span></span>


6. <span data-ttu-id="a4b4d-180">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-180">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="a4b4d-181">Název atributu</span><span class="sxs-lookup"><span data-stu-id="a4b4d-181">Attribute Name</span></span> | <span data-ttu-id="a4b4d-182">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="a4b4d-182">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="a4b4d-183">ClientID</span><span class="sxs-lookup"><span data-stu-id="a4b4d-183">clientid</span></span> | <span data-ttu-id="a4b4d-184">*"uniquevalue"*</span><span class="sxs-lookup"><span data-stu-id="a4b4d-184">*"uniquevalue"*</span></span> |

    <span data-ttu-id="a4b4d-185">a.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-185">a.</span></span> <span data-ttu-id="a4b4d-186">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-186">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    <span data-ttu-id="a4b4d-189">b.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-189">b.</span></span> <span data-ttu-id="a4b4d-190">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-190">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="a4b4d-191">c.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-191">c.</span></span> <span data-ttu-id="a4b4d-192">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-192">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="a4b4d-193">d.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-193">d.</span></span> <span data-ttu-id="a4b4d-194">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="a4b4d-194">Click **Ok**</span></span>

7. <span data-ttu-id="a4b4d-195">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-195">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="a4b4d-197">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-197">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

9. <span data-ttu-id="a4b4d-199">Na hello **FilesAnywhere konfigurace** klikněte na tlačítko **konfigurace FilesAnywhere** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-199">On hello **FilesAnywhere Configuration** section, click **Configure FilesAnywhere** tooopen **Configure sign-on** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

10. <span data-ttu-id="a4b4d-202">pro aplikaci na konci FilesAnywhere, obraťte se na dokončení konfigurace jednotného přihlašování k tooget [tým podpory FilesAnywhere](mailto:support@FilesAnywhere.com) a poskytněte tokenu SAML hello stáhnout podpisový certifikát a jednotné přihlašování na (SSO) adresy URL.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-202">tooget SSO configuration complete for your application at FilesAnywhere end, contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) and provide them hello downloaded SAML token signing Certificate and Single Sign On (SSO) URL.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a4b4d-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4b4d-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="a4b4d-204">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-204">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a4b4d-206">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a4b4d-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4b4d-207">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-207">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a4b4d-209">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-209">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a4b4d-211">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-211">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a4b4d-213">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4b4d-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a4b4d-215">a.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-215">a.</span></span> <span data-ttu-id="a4b4d-216">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a4b4d-217">b.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-217">b.</span></span> <span data-ttu-id="a4b4d-218">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a4b4d-219">c.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-219">c.</span></span> <span data-ttu-id="a4b4d-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a4b4d-221">d.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-221">d.</span></span> <span data-ttu-id="a4b4d-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-222">Click **Create**.</span></span> 



### <a name="creating-a-filesanywhere-test-user"></a><span data-ttu-id="a4b4d-223">Vytvoření zkušebního uživatele FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="a4b4d-223">Creating a FilesAnywhere test user</span></span>

<span data-ttu-id="a4b4d-224">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele budou vytvořeny v hello aplikace automaticky.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-224">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> 


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a4b4d-225">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a4b4d-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a4b4d-226">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooFilesAnywhere svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFilesAnywhere.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a4b4d-228">**tooassign Britta Simon tooFilesAnywhere, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a4b4d-228">**tooassign Britta Simon tooFilesAnywhere, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4b4d-229">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-229">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a4b4d-231">V seznamu aplikace hello vyberte **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-231">In hello applications list, select **FilesAnywhere**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_app.png) 

3. <span data-ttu-id="a4b4d-233">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a4b4d-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-235">Click **Add** button.</span></span> <span data-ttu-id="a4b4d-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a4b4d-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a4b4d-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a4b4d-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="a4b4d-241">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4b4d-241">Testing single sign-on</span></span>

<span data-ttu-id="a4b4d-242">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-242">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a4b4d-243">Když kliknete na dlaždici FilesAnywhere hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour FilesAnywhere aplikace.</span><span class="sxs-lookup"><span data-stu-id="a4b4d-243">When you click hello FilesAnywhere tile in hello Access Panel, you should get automatically signed-on tooyour FilesAnywhere application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="a4b4d-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a4b4d-244">Additional resources</span></span>

* [<span data-ttu-id="a4b4d-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a4b4d-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a4b4d-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a4b4d-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_203.png
