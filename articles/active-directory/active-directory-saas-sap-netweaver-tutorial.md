---
title: 'Kurz: Azure Active Directory integrace s SAP NetWeaver | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP NetWeaver."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 69008734864e1e258a0c2ec872e51aa331491fbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a><span data-ttu-id="0cbf3-103">Kurz: Integrace Azure Active Directory se službou SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="0cbf3-103">Tutorial: Azure Active Directory integration with SAP NetWeaver</span></span>

<span data-ttu-id="0cbf3-104">V tomto kurzu zjistíte, jak toointegrate SAP NetWeaver službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0cbf3-104">In this tutorial, you learn how toointegrate SAP NetWeaver with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0cbf3-105">Integrace SAP NetWeaver s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0cbf3-105">Integrating SAP NetWeaver with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0cbf3-106">Můžete řídit ve službě Azure AD, který má přístup tooSAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="0cbf3-106">You can control in Azure AD who has access tooSAP NetWeaver</span></span>
- <span data-ttu-id="0cbf3-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSAP NetWeaver (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0cbf3-107">You can enable your users tooautomatically get signed-on tooSAP NetWeaver (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0cbf3-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0cbf3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0cbf3-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0cbf3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="0cbf3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0cbf3-110">Prerequisites</span></span>

<span data-ttu-id="0cbf3-111">Integrace služby Azure AD s SAP NetWeaver tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="0cbf3-111">tooconfigure Azure AD integration with SAP NetWeaver, you need hello following items:</span></span>

- <span data-ttu-id="0cbf3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0cbf3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0cbf3-113">SAP NetWeaver jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0cbf3-113">An SAP NetWeaver single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0cbf3-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0cbf3-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0cbf3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0cbf3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0cbf3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0cbf3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0cbf3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0cbf3-118">Scenario description</span></span>
<span data-ttu-id="0cbf3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0cbf3-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0cbf3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0cbf3-121">Přidání SAP NetWeaver z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0cbf3-121">Adding SAP NetWeaver from hello gallery</span></span>
2. <span data-ttu-id="0cbf3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0cbf3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-netweaver-from-hello-gallery"></a><span data-ttu-id="0cbf3-123">Přidání SAP NetWeaver z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0cbf3-123">Adding SAP NetWeaver from hello gallery</span></span>
<span data-ttu-id="0cbf3-124">tooconfigure hello integrace SAP NetWeaver do Azure AD, je nutné tooadd SAP NetWeaver hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-124">tooconfigure hello integration of SAP NetWeaver into Azure AD, you need tooadd SAP NetWeaver from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0cbf3-125">**tooadd SAP NetWeaver z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0cbf3-125">**tooadd SAP NetWeaver from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0cbf3-126">V hello  **[portálu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0cbf3-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0cbf3-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0cbf3-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0cbf3-133">Hello vyhledávacího pole zadejte **SAP NetWeaver**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-133">In hello search box, type **SAP NetWeaver**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

5. <span data-ttu-id="0cbf3-135">Na panelu výsledků hello vyberte **SAP NetWeaver**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-135">In hello results panel, select **SAP NetWeaver**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0cbf3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0cbf3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0cbf3-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP NetWeaver podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0cbf3-138">In this section, you configure and test Azure AD single sign-on with SAP NetWeaver based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0cbf3-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v SAP NetWeaver je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP NetWeaver is tooa user in Azure AD.</span></span> <span data-ttu-id="0cbf3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SAP NetWeaver musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-140">In other words, a link relationship between an Azure AD user and hello related user in SAP NetWeaver needs toobe established.</span></span>

<span data-ttu-id="0cbf3-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SAP NetWeaver.</span></span>

<span data-ttu-id="0cbf3-142">tooconfigure a testu Azure AD jednotné přihlašování s SAP NetWeaver, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="0cbf3-142">tooconfigure and test Azure AD single sign-on with SAP NetWeaver, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0cbf3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0cbf3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0cbf3-145">**[Vytváření testovacího uživatele SAP NetWeaver](#creating-an-sap-netweaver-test-user)**  -toohave protějšek Britta Simon v SAP NetWeaver, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-145">**[Creating an SAP NetWeaver test user](#creating-an-sap-netweaver-test-user)** - toohave a counterpart of Britta Simon in SAP NetWeaver that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0cbf3-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0cbf3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0cbf3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0cbf3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0cbf3-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP NetWeaver application.</span></span>

<span data-ttu-id="0cbf3-150">**tooconfigure Azure AD jednotné přihlašování s SAP NetWeaver, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0cbf3-150">**tooconfigure Azure AD single sign-on with SAP NetWeaver, perform hello following steps:**</span></span>

1. <span data-ttu-id="0cbf3-151">V portálu Azure, na hello hello **SAP NetWeaver** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-151">In hello Azure portal, on hello **SAP NetWeaver** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0cbf3-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

3. <span data-ttu-id="0cbf3-155">Na hello **SAP NetWeaver domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0cbf3-155">On hello **SAP NetWeaver Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    <span data-ttu-id="0cbf3-157">a.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-157">a.</span></span> <span data-ttu-id="0cbf3-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<your company instance of SAP NetWeaver>`</span><span class="sxs-lookup"><span data-stu-id="0cbf3-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<your company instance of SAP NetWeaver>`</span></span>

    <span data-ttu-id="0cbf3-159">b.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-159">b.</span></span> <span data-ttu-id="0cbf3-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<your company instance of SAP NetWeaver>`</span><span class="sxs-lookup"><span data-stu-id="0cbf3-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<your company instance of SAP NetWeaver>`</span></span>

    <span data-ttu-id="0cbf3-161">c.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-161">c.</span></span> <span data-ttu-id="0cbf3-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`</span><span class="sxs-lookup"><span data-stu-id="0cbf3-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="0cbf3-163">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-163">These values are not hello real.</span></span> <span data-ttu-id="0cbf3-164">Aktualizovat tyto hodnoty s hello skutečné identifikátor a adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-164">Update these values with hello actual Identifier and Reply URL and Sign-On URL.</span></span> <span data-ttu-id="0cbf3-165">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-165">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="0cbf3-166">Obraťte se na [tým podpory SAP NetWeaver klienta](https://www.sap.com/support.html) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-166">Contact [SAP NetWeaver Client support team](https://www.sap.com/support.html) tooget these values.</span></span> 

4. <span data-ttu-id="0cbf3-167">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

5. <span data-ttu-id="0cbf3-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="0cbf3-171">Na hello **SAP NetWeaver konfigurace** klikněte na tlačítko **konfigurace SAP NetWeaver** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-171">On hello **SAP NetWeaver Configuration** section, click **Configure SAP NetWeaver** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0cbf3-172">Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0cbf3-172">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

7. <span data-ttu-id="0cbf3-174">tooconfigure jednotného přihlašování na **SAP NetWeaver** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** a **SAML Entity ID** příliš[SAP NetWeaver podpory ](https://www.sap.com/support.html).</span><span class="sxs-lookup"><span data-stu-id="0cbf3-174">tooconfigure single sign-on on **SAP NetWeaver** side, you need toosend hello downloaded **Metadata XML** and **SAML Entity ID** too[SAP NetWeaver support](https://www.sap.com/support.html).</span></span> 

> [!TIP]
> <span data-ttu-id="0cbf3-175">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="0cbf3-175">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0cbf3-176">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-176">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0cbf3-177">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0cbf3-177">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0cbf3-178">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0cbf3-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="0cbf3-179">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-179">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0cbf3-181">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0cbf3-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0cbf3-182">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-182">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0cbf3-184">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-184">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0cbf3-186">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-186">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0cbf3-188">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0cbf3-188">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0cbf3-190">a.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-190">a.</span></span> <span data-ttu-id="0cbf3-191">V hello **název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-191">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="0cbf3-192">b.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-192">b.</span></span> <span data-ttu-id="0cbf3-193">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-193">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="0cbf3-194">c.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-194">c.</span></span> <span data-ttu-id="0cbf3-195">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-195">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0cbf3-196">d.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-196">d.</span></span> <span data-ttu-id="0cbf3-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-197">Click **Create**.</span></span>
 
### <a name="creating-an-sap-netweaver-test-user"></a><span data-ttu-id="0cbf3-198">Vytváření testovacího uživatele SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="0cbf3-198">Creating an SAP NetWeaver test user</span></span>

<span data-ttu-id="0cbf3-199">V této části vytvoříte volal Britta Simon v SAP NetWeaver uživatele.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-199">In this section, you create a user called Britta Simon in SAP NetWeaver.</span></span> <span data-ttu-id="0cbf3-200">Práce s vaší [podporu SAP NetWeaver](https://www.sap.com/support.html) tooadd hello uživatelé v platformě SAP NetWeaver hello.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-200">Work with your [SAP NetWeaver support](https://www.sap.com/support.html) tooadd hello users in hello SAP NetWeaver platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0cbf3-201">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="0cbf3-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0cbf3-202">V této části povolíte tak, že udělíte přístup tooSAP NetWeaver toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP NetWeaver.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0cbf3-204">**tooassign Britta Simon tooSAP NetWeaver, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0cbf3-204">**tooassign Britta Simon tooSAP NetWeaver, perform hello following steps:**</span></span>

1. <span data-ttu-id="0cbf3-205">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0cbf3-207">V seznamu aplikace hello vyberte **SAP NetWeaver**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-207">In hello applications list, select **SAP NetWeaver**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. <span data-ttu-id="0cbf3-209">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0cbf3-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-211">Click **Add** button.</span></span> <span data-ttu-id="0cbf3-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0cbf3-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0cbf3-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0cbf3-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0cbf3-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0cbf3-217">Testing single sign-on</span></span>

<span data-ttu-id="0cbf3-218">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0cbf3-219">Když kliknete na dlaždici SAP NetWeaver hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SAP NetWeaver aplikace.</span><span class="sxs-lookup"><span data-stu-id="0cbf3-219">When you click hello SAP NetWeaver tile in hello Access Panel, you should get automatically signed-on tooyour SAP NetWeaver application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0cbf3-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0cbf3-220">Additional resources</span></span>

* [<span data-ttu-id="0cbf3-221">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0cbf3-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0cbf3-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0cbf3-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_203.png

