---
title: 'Kurz: Azure Active Directory integrace s ServiceChannel | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a ServiceChannel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.openlocfilehash: 956371a1e99dcba4137c271ecfe8a62b9ec64a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a><span data-ttu-id="0792f-103">Kurz: Azure Active Directory integrace s ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="0792f-103">Tutorial: Azure Active Directory integration with ServiceChannel</span></span>

<span data-ttu-id="0792f-104">V tomto kurzu zjistíte, jak toointegrate ServiceChannel s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0792f-104">In this tutorial, you learn how toointegrate ServiceChannel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0792f-105">Integrace ServiceChannel s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0792f-105">Integrating ServiceChannel with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0792f-106">Můžete řídit ve službě Azure AD, který má přístup tooServiceChannel</span><span class="sxs-lookup"><span data-stu-id="0792f-106">You can control in Azure AD who has access tooServiceChannel</span></span>
- <span data-ttu-id="0792f-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooServiceChannel (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0792f-107">You can enable your users tooautomatically get signed-on tooServiceChannel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0792f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="0792f-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="0792f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0792f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0792f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0792f-110">Prerequisites</span></span>

<span data-ttu-id="0792f-111">Integrace služby Azure AD s ServiceChannel tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="0792f-111">tooconfigure Azure AD integration with ServiceChannel, you need hello following items:</span></span>

- <span data-ttu-id="0792f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0792f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0792f-113">ServiceChannel jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0792f-113">A ServiceChannel single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0792f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0792f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0792f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0792f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0792f-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="0792f-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="0792f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0792f-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0792f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0792f-118">Scenario description</span></span>
<span data-ttu-id="0792f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0792f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0792f-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0792f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0792f-121">Přidání ServiceChannel z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0792f-121">Adding ServiceChannel from hello gallery</span></span>
2. <span data-ttu-id="0792f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0792f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-servicechannel-from-hello-gallery"></a><span data-ttu-id="0792f-123">Přidání ServiceChannel z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0792f-123">Adding ServiceChannel from hello gallery</span></span>
<span data-ttu-id="0792f-124">tooconfigure hello integrace ServiceChannel do Azure AD, je nutné tooadd ServiceChannel hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0792f-124">tooconfigure hello integration of ServiceChannel into Azure AD, you need tooadd ServiceChannel from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0792f-125">**tooadd ServiceChannel z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0792f-125">**tooadd ServiceChannel from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0792f-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0792f-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0792f-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0792f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0792f-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0792f-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0792f-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0792f-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0792f-133">Hello vyhledávacího pole zadejte **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="0792f-133">In hello search box, type **ServiceChannel**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. <span data-ttu-id="0792f-135">Na panelu výsledků hello vyberte **ServiceChannel**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0792f-135">In hello results panel, select **ServiceChannel**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0792f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0792f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0792f-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ServiceChannel podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="0792f-138">In this section, you configure and test Azure AD single sign-on with ServiceChannel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0792f-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v ServiceChannel je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0792f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ServiceChannel is tooa user in Azure AD.</span></span> <span data-ttu-id="0792f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v ServiceChannel musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="0792f-140">In other words, a link relationship between an Azure AD user and hello related user in ServiceChannel needs toobe established.</span></span>

<span data-ttu-id="0792f-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="0792f-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ServiceChannel.</span></span>

<span data-ttu-id="0792f-142">tooconfigure a testu Azure AD jednotné přihlašování s ServiceChannel, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="0792f-142">tooconfigure and test Azure AD single sign-on with ServiceChannel, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0792f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="0792f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0792f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0792f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0792f-145">**[Vytvoření zkušebního uživatele ServiceChannel](#creating-a-servicechannel-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0792f-145">**[Creating a ServiceChannel test user](#creating-a-servicechannel-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="0792f-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0792f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0792f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="0792f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0792f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0792f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0792f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="0792f-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your ServiceChannel application.</span></span>

<span data-ttu-id="0792f-150">**tooconfigure Azure AD jednotné přihlašování s ServiceChannel, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0792f-150">**tooconfigure Azure AD single sign-on with ServiceChannel, perform hello following steps:**</span></span>

1. <span data-ttu-id="0792f-151">V hello Azure Management portal na hello **ServiceChannel** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0792f-151">In hello Azure Management portal, on hello **ServiceChannel** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0792f-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="0792f-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. <span data-ttu-id="0792f-155">Na hello **ServiceChannel domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0792f-155">On hello **ServiceChannel Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    <span data-ttu-id="0792f-157">a.</span><span class="sxs-lookup"><span data-stu-id="0792f-157">a.</span></span> <span data-ttu-id="0792f-158">V hello **identifikátor** textovému poli, hodnota typu hello jako:`http://adfs.<domain>.com/adfs/service/trust`</span><span class="sxs-lookup"><span data-stu-id="0792f-158">In hello **Identifier** textbox, type hello value as: `http://adfs.<domain>.com/adfs/service/trust`</span></span>

    <span data-ttu-id="0792f-159">b.</span><span class="sxs-lookup"><span data-stu-id="0792f-159">b.</span></span> <span data-ttu-id="0792f-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<customer domain>.servicechannel.com/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="0792f-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<customer domain>.servicechannel.com/saml/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0792f-161">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0792f-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="0792f-162">Máte tooupdate tyto hodnoty pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0792f-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="0792f-163">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="0792f-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="0792f-164">Obraťte se na [tým podpory ServiceChannel](https://servicechannel.zendesk.com/hc/en-us) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0792f-164">Contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us) tooget these values.</span></span>

4. <span data-ttu-id="0792f-165">Aplikace ServiceChannel očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování tooyour tokenu atributy konfigurace SAML.</span><span class="sxs-lookup"><span data-stu-id="0792f-165">Your ServiceChannel application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="0792f-166">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="0792f-166">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="0792f-167">**NameIdentifier (identifikátor uživatele)** hello jenom povinné deklarace identity a hello výchozí hodnota je **user.userprincipalname** ale ServiceChannel očekává tento toobe namapována na **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="0792f-167">**NameIdentifier(User Identifier)** is hello only mandatory claim and hello default value is **user.userprincipalname** but ServiceChannel expects this toobe mapped with **user.mail**.</span></span> <span data-ttu-id="0792f-168">Pokud plánujete zřizování uživatelů jenom v době tooenable, měli byste přidat hello následující deklarace identity, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="0792f-168">If you are planning tooenable Just In Time user provisioning, then you should add hello following claims as shown below.</span></span> <span data-ttu-id="0792f-169">**Role** deklarace identity musí toobe namapované příliš**user.assignedroles** obsahující hello role uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="0792f-169">**Role** claim needs toobe mapped too**user.assignedroles** which contains hello role of hello user.</span></span>  

    <span data-ttu-id="0792f-170">Najdete Průvodce ServiceChannel [sem](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) další pokyny na deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="0792f-170">You can refer ServiceChannel guide [here](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) for more guidance on claims.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > <span data-ttu-id="0792f-172">Kliknutím na [sem](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) tooknow jak tooconfigure **Role** ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="0792f-172">Please click [here](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) tooknow how tooconfigure **Role** in Azure AD</span></span>

5. <span data-ttu-id="0792f-173">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavte atributy hello.</span><span class="sxs-lookup"><span data-stu-id="0792f-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span>

    | <span data-ttu-id="0792f-174">Název atributu</span><span class="sxs-lookup"><span data-stu-id="0792f-174">Attribute Name</span></span> | <span data-ttu-id="0792f-175">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="0792f-175">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="0792f-176">Role</span><span class="sxs-lookup"><span data-stu-id="0792f-176">Role</span></span>| <span data-ttu-id="0792f-177">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="0792f-177">user.assignedroles</span></span> |

    <span data-ttu-id="0792f-178">a.</span><span class="sxs-lookup"><span data-stu-id="0792f-178">a.</span></span> <span data-ttu-id="0792f-179">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0792f-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    <span data-ttu-id="0792f-182">b.</span><span class="sxs-lookup"><span data-stu-id="0792f-182">b.</span></span> <span data-ttu-id="0792f-183">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="0792f-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="0792f-184">c.</span><span class="sxs-lookup"><span data-stu-id="0792f-184">c.</span></span> <span data-ttu-id="0792f-185">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="0792f-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="0792f-186">d.</span><span class="sxs-lookup"><span data-stu-id="0792f-186">d.</span></span> <span data-ttu-id="0792f-187">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="0792f-187">Click **Ok**</span></span>
    
6. <span data-ttu-id="0792f-188">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0792f-188">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. <span data-ttu-id="0792f-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0792f-190">Click **Save**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="0792f-192">Na hello **ServiceChannel konfigurace** klikněte na tlačítko **konfigurace ServiceChannel** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0792f-192">On hello **ServiceChannel Configuration** section, click **Configure ServiceChannel** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0792f-193">Upozorňujeme hello **SAML Enitity ID** z hello **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="0792f-193">Please note hello **SAML Enitity ID** from hello **Quick Reference** section.</span></span>

9. <span data-ttu-id="0792f-194">tooconfigure jednotného přihlašování na **ServiceChannel** straně, je nutné stáhnout hello toosend **certifikátu (Base64)** a **SAML Entity ID** příliš[ Tým podpory ServiceChannel](https://servicechannel.zendesk.com/hc/en-us).</span><span class="sxs-lookup"><span data-stu-id="0792f-194">tooconfigure single sign-on on **ServiceChannel** side, you need toosend hello downloaded **certificate (Base64)** and **SAML Entity ID** too[ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us).</span></span> <span data-ttu-id="0792f-195">To bude nastavené v pořadí toohave hello jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="0792f-195">They will set this up in order toohave hello SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0792f-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0792f-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="0792f-197">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0792f-197">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0792f-199">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0792f-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0792f-200">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0792f-200">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0792f-202">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0792f-202">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0792f-204">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0792f-204">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0792f-206">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0792f-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0792f-208">a.</span><span class="sxs-lookup"><span data-stu-id="0792f-208">a.</span></span> <span data-ttu-id="0792f-209">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0792f-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0792f-210">b.</span><span class="sxs-lookup"><span data-stu-id="0792f-210">b.</span></span> <span data-ttu-id="0792f-211">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0792f-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0792f-212">c.</span><span class="sxs-lookup"><span data-stu-id="0792f-212">c.</span></span> <span data-ttu-id="0792f-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0792f-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0792f-214">d.</span><span class="sxs-lookup"><span data-stu-id="0792f-214">d.</span></span> <span data-ttu-id="0792f-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0792f-215">Click **Create**.</span></span> 

### <a name="creating-a-servicechannel-test-user"></a><span data-ttu-id="0792f-216">Vytvoření zkušebního uživatele ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="0792f-216">Creating a ServiceChannel test user</span></span>

<span data-ttu-id="0792f-217">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele budou vytvořeny v hello aplikace automaticky.</span><span class="sxs-lookup"><span data-stu-id="0792f-217">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> <span data-ttu-id="0792f-218">Pro úplné uživatelské zřizování, obraťte se na [ServiceChannel tým podpory](https://servicechannel.zendesk.com/hc/en-us)</span><span class="sxs-lookup"><span data-stu-id="0792f-218">For full user provisioning, please contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us)</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0792f-219">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="0792f-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0792f-220">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooServiceChannel svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="0792f-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooServiceChannel.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0792f-222">**tooassign Britta Simon tooServiceChannel, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0792f-222">**tooassign Britta Simon tooServiceChannel, perform hello following steps:**</span></span>

1. <span data-ttu-id="0792f-223">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0792f-223">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0792f-225">V seznamu aplikace hello vyberte **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="0792f-225">In hello applications list, select **ServiceChannel**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. <span data-ttu-id="0792f-227">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0792f-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0792f-229">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0792f-229">Click **Add** button.</span></span> <span data-ttu-id="0792f-230">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0792f-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0792f-232">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="0792f-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0792f-233">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0792f-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0792f-234">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0792f-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0792f-235">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0792f-235">Testing single sign-on</span></span>

<span data-ttu-id="0792f-236">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0792f-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0792f-237">Když kliknete na dlaždici ServiceChannel hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ServiceChannel aplikace.</span><span class="sxs-lookup"><span data-stu-id="0792f-237">When you click hello ServiceChannel tile in hello Access Panel, you should get automatically signed-on tooyour ServiceChannel application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0792f-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0792f-238">Additional resources</span></span>

* [<span data-ttu-id="0792f-239">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0792f-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0792f-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0792f-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_203.png