---
title: "Kurz: Azure Active Directory integrace s IBM Kenexa průzkum Enterprise | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a IBM Kenexa průzkum Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: cf7ed886b4418ac396ca7056827ee10fd7a19ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a><span data-ttu-id="f8bec-103">Kurz: Azure Active Directory integrace s IBM Kenexa průzkum Enterprise</span><span class="sxs-lookup"><span data-stu-id="f8bec-103">Tutorial: Azure Active Directory integration with IBM Kenexa Survey Enterprise</span></span>

<span data-ttu-id="f8bec-104">V tomto kurzu zjistíte, jak toointegrate IBM Kenexa průzkum Enterprise s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f8bec-104">In this tutorial, you learn how toointegrate IBM Kenexa Survey Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f8bec-105">Integrace IBM Kenexa průzkum Enterprise s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f8bec-105">Integrating IBM Kenexa Survey Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f8bec-106">Můžete ovládat ve službě Azure AD, který má přístup tooIBM Kenexa průzkum Enterprise.</span><span class="sxs-lookup"><span data-stu-id="f8bec-106">You can control in Azure AD who has access tooIBM Kenexa Survey Enterprise.</span></span>
- <span data-ttu-id="f8bec-107">Přihlášení uživatele tooautomatically tooIBM Kenexa průzkum Enterprise můžete povolit pomocí jednotného přihlašování (SSO) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8bec-107">You can enable your users tooautomatically sign in tooIBM Kenexa Survey Enterprise by using single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f8bec-108">Můžete spravovat vaše účty v jednom centrálním místě: hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f8bec-108">You can manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="f8bec-109">Pokud chcete více informací o softwaru tooknow jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f8bec-109">If you want tooknow more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8bec-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f8bec-110">Prerequisites</span></span>

<span data-ttu-id="f8bec-111">tooconfigure integrace Azure AD s IBM Kenexa průzkum Enterprise, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f8bec-111">tooconfigure Azure AD integration with IBM Kenexa Survey Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="f8bec-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8bec-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f8bec-113">Předplatné služby IBM Kenexa průzkum Enterprise jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f8bec-113">An IBM Kenexa Survey Enterprise SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f8bec-114">Při testování hello kroky v tomto kurzu, doporučujeme vám, nepoužívejte provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f8bec-114">When you test hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="f8bec-115">tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="f8bec-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="f8bec-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f8bec-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f8bec-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f8bec-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f8bec-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f8bec-118">Scenario description</span></span>
<span data-ttu-id="f8bec-119">V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f8bec-119">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="f8bec-120">Hello scénáři uvedeném v kurzu hello se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f8bec-120">hello scenario outlined in hello tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="f8bec-121">Přidání IBM Kenexa průzkum Enterprise z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f8bec-121">Adding IBM Kenexa Survey Enterprise from hello gallery</span></span>
* <span data-ttu-id="f8bec-122">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="f8bec-122">Configuring and testing Azure AD SSO</span></span>

## <a name="add-ibm-kenexa-survey-enterprise-from-hello-gallery"></a><span data-ttu-id="f8bec-123">Přidat IBM Kenexa průzkum Enterprise z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f8bec-123">Add IBM Kenexa Survey Enterprise from hello gallery</span></span>
<span data-ttu-id="f8bec-124">tooconfigure hello integrace IBM Kenexa průzkum Enterprise do Azure AD, přidejte IBM Kenexa průzkum Enterprise hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f8bec-124">tooconfigure hello integration of IBM Kenexa Survey Enterprise into Azure AD, add IBM Kenexa Survey Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f8bec-125">tooadd IBM Kenexa průzkum Enterprise z Galerie hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="f8bec-125">tooadd IBM Kenexa Survey Enterprise from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="f8bec-126">V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, klikněte na hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f8bec-126">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Active Directory** button.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="f8bec-128">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="f8bec-130">tooadd aplikaci, klikněte na tlačítko hello **novou aplikaci** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f8bec-130">tooadd an application, click hello **New application** button.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="f8bec-132">Hello vyhledávacího pole zadejte **IBM Kenexa průzkum Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-132">In hello search box, type **IBM Kenexa Survey Enterprise**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. <span data-ttu-id="f8bec-134">V seznamu výsledků hello vyberte **IBM Kenexa průzkum Enterprise**a potom klikněte na hello **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f8bec-134">In hello results list, select **IBM Kenexa Survey Enterprise**, and then click hello **Add** button tooadd hello application.</span></span>

    ![IBM Kenexa průzkum Enterprise v seznamu výsledků hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f8bec-136">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f8bec-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="f8bec-137">V této části můžete nakonfigurovat a otestovat Azure AD přihlášení SSO se IBM Kenexa průzkum Enterprise podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f8bec-137">In this section, you configure and test Azure AD SSO with IBM Kenexa Survey Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f8bec-138">Azure AD pro jednotné přihlašování toowork musí tooidentify hello IBM Kenexa průzkum Enterprise uživatele protějšku ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8bec-138">For SSO toowork, Azure AD needs tooidentify hello IBM Kenexa Survey Enterprise user counterpart in Azure AD.</span></span> <span data-ttu-id="f8bec-139">Jinými slovy Azure AD potřeba vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské ve IBM Kenexa průzkum podniku.</span><span class="sxs-lookup"><span data-stu-id="f8bec-139">In other words, Azure AD must establish a link relationship between an Azure AD user and a related user in IBM Kenexa Survey Enterprise.</span></span>

<span data-ttu-id="f8bec-140">tooestablish hello odkaz vztahu přiřadit hodnotu hello hello **uživatelské jméno** ve IBM Kenexa průzkum podniku jako hodnota hello hello **uživatelské jméno** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8bec-140">tooestablish hello link relationship, assign hello value of hello **user name** in IBM Kenexa Survey Enterprise as hello value of hello **Username** in Azure AD.</span></span>

<span data-ttu-id="f8bec-141">tooconfigure a testování Azure AD přihlášení SSO se IBM Kenexa průzkum Enterprise, dokončení hello stavebních bloků v následujících dvou částech hello.</span><span class="sxs-lookup"><span data-stu-id="f8bec-141">tooconfigure and test Azure AD SSO with IBM Kenexa Survey Enterprise, complete hello building blocks in hello next two sections.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="f8bec-142">Konfigurovat Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f8bec-142">Configure Azure AD SSO</span></span>

<span data-ttu-id="f8bec-143">V této části Povolení jednotného přihlašování Azure AD v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci IBM Kenexa průzkum Enterprise díky hello následující:</span><span class="sxs-lookup"><span data-stu-id="f8bec-143">In this section, you enable Azure AD SSO in hello Azure portal and configure SSO in your IBM Kenexa Survey Enterprise application by doing hello following:</span></span>

1. <span data-ttu-id="f8bec-144">V portálu Azure, na hello hello **IBM Kenexa průzkum Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-144">In hello Azure portal, on hello **IBM Kenexa Survey Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![IBM Kenexa průzkum Enterprise nakonfigurovat jeden přihlašování odkaz][4]

2. <span data-ttu-id="f8bec-146">V hello **jednotného přihlašování** dialogové okno, v hello **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f8bec-146">In hello **Single sign-on** dialog box, in hello **Mode** box, select **SAML-based Sign-on** tooenable SSO.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. <span data-ttu-id="f8bec-148">V hello **IBM Kenexa průzkum podnikové domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f8bec-148">In hello **IBM Kenexa Survey Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![IBM Kenexa průzkum podnikové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    <span data-ttu-id="f8bec-150">a.</span><span class="sxs-lookup"><span data-stu-id="f8bec-150">a.</span></span> <span data-ttu-id="f8bec-151">V hello **identifikátor** textovému poli, zadejte adresu URL s hello následující vzoru:`https://surveys.kenexa.com/<companycode>`</span><span class="sxs-lookup"><span data-stu-id="f8bec-151">In hello **Identifier** textbox, type a URL with hello following pattern: `https://surveys.kenexa.com/<companycode>`</span></span>

    <span data-ttu-id="f8bec-152">b.</span><span class="sxs-lookup"><span data-stu-id="f8bec-152">b.</span></span> <span data-ttu-id="f8bec-153">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL s hello následující vzoru:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span><span class="sxs-lookup"><span data-stu-id="f8bec-153">In hello **Reply URL** textbox, type a URL with hello following pattern: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f8bec-154">Hello předchozí hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f8bec-154">hello preceding values are not real.</span></span> <span data-ttu-id="f8bec-155">Je aktualizovat skutečným identifikátorem hello a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f8bec-155">Update them with hello actual identifier and reply URL.</span></span> <span data-ttu-id="f8bec-156">tooobtain hello skutečnými hodnotami, kontaktujte hello [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="f8bec-156">tooobtain hello actual values, contact hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

4. <span data-ttu-id="f8bec-157">V části **SAML podpisový certifikát**, klikněte na tlačítko **certifikátu (Base64)**a potom uložte hello certifikát souboru tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="f8bec-157">Under **SAML Signing Certificate**, click **Certificate (Base64)**, and then save hello certificate file tooyour computer.</span></span>

    ![odkaz ke stažení certifikátu (Base64) Hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    <span data-ttu-id="f8bec-159">Hello IBM Kenexa průzkum podniková aplikace očekává tooreceive hello zabezpečení kontrolní výrazy Markup Language (SAML) kontrolní výrazy ve specifickém formátu, který vyžaduje jste tooadd vlastních atributů mapování toohello konfigurace vaší atributů tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="f8bec-159">hello IBM Kenexa Survey Enterprise application expects tooreceive hello Security Assertions Markup Language (SAML) assertions in a specific format, which requires you tooadd custom attribute mappings toohello configuration of your SAML token attributes.</span></span> <span data-ttu-id="f8bec-160">Hello hello uživatelský identifikátor deklarace identity v odpovědi hello musí odpovídat hello ID jednotné přihlašování, který je nakonfigurovaný v systému Kenexa hello.</span><span class="sxs-lookup"><span data-stu-id="f8bec-160">hello value of hello user-identifier claim in hello response must match hello SSO ID that's configured in hello Kenexa system.</span></span> <span data-ttu-id="f8bec-161">toomap hello identifikátor příslušné uživatele ve vaší organizaci jako jednotného přihlašování k Internetu Datagram Protocol (IDP), práce s hello [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="f8bec-161">toomap hello appropriate user identifier in your organization as SSO Internet Datagram Protocol (IDP), work with hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> 

    <span data-ttu-id="f8bec-162">Ve výchozím nastavení Azure AD Nastaví uživatelský identifikátor hello jako hodnota hlavní název (UPN) uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="f8bec-162">By default, Azure AD sets hello user identifier as hello user principal name (UPN) value.</span></span> <span data-ttu-id="f8bec-163">Můžete změnit tuto hodnotu na hello **atribut** kartě, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="f8bec-163">You can change this value on hello **Attribute** tab, as shown in hello following screenshot.</span></span> <span data-ttu-id="f8bec-164">integrace Hello funguje pouze po dokončení hello mapování správně.</span><span class="sxs-lookup"><span data-stu-id="f8bec-164">hello integration works only after you've completed hello mapping correctly.</span></span>
    
    ![Dialogové okno uživatelské atributy Hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png) 

5. <span data-ttu-id="f8bec-166">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-166">Click **Save**.</span></span>

    ![Hello nakonfigurovat jednotné přihlašování tlačítko Uložit](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f8bec-168">tooopen hello **konfigurovat přihlášení** okno, v části **IBM Kenexa průzkum podniková konfigurace**, klikněte na tlačítko **konfigurace IBM Kenexa průzkum Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-168">tooopen hello **Configure sign-on** window, under **IBM Kenexa Survey Enterprise Configuration**, click **Configure IBM Kenexa Survey Enterprise**.</span></span> 
 
    ![odkaz Konfigurace IBM Kenexa průzkum Enterprise Hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. <span data-ttu-id="f8bec-170">Kopírování hello **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašovací adresa URL služby** hodnoty z hello **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="f8bec-170">Copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values from hello **Quick Reference** section.</span></span>

8. <span data-ttu-id="f8bec-171">V hello **konfigurovat přihlášení** okno, v části **Stručná referenční příručka**, kopie hello **Sign-Out URL**, **SAML Entity ID**, a  **SAML jeden přihlašovací adresa URL služby** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f8bec-171">In hello **Configure sign-on** window, under **Quick Reference**, copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values.</span></span>

9. <span data-ttu-id="f8bec-172">tooconfigure jednotného přihlašování na hello **IBM Kenexa průzkum Enterprise** straně, odesílat hello Stáhnout **certifikátu (Base64)**, **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašovací adresa URL služby** hodnoty toohello [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="f8bec-172">tooconfigure SSO on hello **IBM Kenexa Survey Enterprise** side, send hello downloaded **Certificate (Base64)**, **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values toohello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

> [!TIP]
> <span data-ttu-id="f8bec-173">Může odkazovat tooa stručným verzi tyto pokyny v hello [portál Azure](https://portal.azure.com) při nastavujete aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f8bec-173">You can refer tooa concise version of these instructions in hello [Azure portal](https://portal.azure.com) while you are setting up hello app.</span></span> <span data-ttu-id="f8bec-174">Po přidání aplikace hello z hello **služby Active Directory** > **podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotného přihlašování** kartě a potom přejdete na Hello vložených dokumentace prostřednictvím hello **konfigurace** části na konci hello.</span><span class="sxs-lookup"><span data-stu-id="f8bec-174">After you add hello app from hello **Active Directory** > **Enterprise Applications** section, simply click hello **single sign-on** tab, and then access hello embedded documentation through hello **Configuration** section at hello end.</span></span> <span data-ttu-id="f8bec-175">toolearn Další informace o funkci embedded dokumentace hello, najdete v části [Azure AD vložených dokumentaci](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="f8bec-175">toolearn more about hello embedded documentation feature, see [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f8bec-176">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8bec-176">Create an Azure AD test user</span></span>
<span data-ttu-id="f8bec-177">V této části vytvoříte testovacího uživatele Britta Simon v hello portálu Azure pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="f8bec-177">In this section, you create test user Britta Simon in hello Azure portal by doing hello following:</span></span>

![Vytvořit testovací uživatele Azure AD][100]

1. <span data-ttu-id="f8bec-179">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f8bec-179">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f8bec-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-181">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f8bec-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f8bec-183">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f8bec-185">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f8bec-185">In hello **User** dialog box, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f8bec-187">a.</span><span class="sxs-lookup"><span data-stu-id="f8bec-187">a.</span></span> <span data-ttu-id="f8bec-188">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-188">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f8bec-189">b.</span><span class="sxs-lookup"><span data-stu-id="f8bec-189">b.</span></span> <span data-ttu-id="f8bec-190">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f8bec-190">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="f8bec-191">c.</span><span class="sxs-lookup"><span data-stu-id="f8bec-191">c.</span></span> <span data-ttu-id="f8bec-192">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="f8bec-192">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="f8bec-193">d.</span><span class="sxs-lookup"><span data-stu-id="f8bec-193">d.</span></span> <span data-ttu-id="f8bec-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-194">Click **Create**.</span></span>
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a><span data-ttu-id="f8bec-195">Vytvořit testovací uživatele s IBM Kenexa průzkum Enterprise</span><span class="sxs-lookup"><span data-stu-id="f8bec-195">Create an IBM Kenexa Survey Enterprise test user</span></span>

<span data-ttu-id="f8bec-196">V této části vytvoříte uživatele volal Britta Simon v IBM Kenexa průzkum Enterprise.</span><span class="sxs-lookup"><span data-stu-id="f8bec-196">In this section, you create a user called Britta Simon in IBM Kenexa Survey Enterprise.</span></span> 

<span data-ttu-id="f8bec-197">toocreate uživatele v hello systému IBM Kenexa průzkum Enterprise a mapy hello ID jednotného přihlašování pro ně, můžete pracovat s hello [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="f8bec-197">toocreate users in hello IBM Kenexa Survey Enterprise system and map hello SSO ID for them, you can work with hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> <span data-ttu-id="f8bec-198">Tato hodnota ID jednotné přihlašování musí být také mapovat toohello hodnota identifikátoru uživatele z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8bec-198">This SSO ID value should also be mapped toohello user identifier value from Azure AD.</span></span> <span data-ttu-id="f8bec-199">Můžete změnit toto výchozí nastavení na hello **atribut** kartě.</span><span class="sxs-lookup"><span data-stu-id="f8bec-199">You can change this default setting on hello **Attribute** tab.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="f8bec-200">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f8bec-200">Assign hello Azure AD test user</span></span>

<span data-ttu-id="f8bec-201">V této části povolit uživatele Britta Simon toouse jednotného přihlašování k Azure tak, že udělíte přístup tooIBM Kenexa průzkum Enterprise.</span><span class="sxs-lookup"><span data-stu-id="f8bec-201">In this section, you enable user Britta Simon toouse Azure SSO by granting access tooIBM Kenexa Survey Enterprise.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="f8bec-203">uživatel tooassign Britta Simon tooIBM Kenexa průzkum Enterprise, hello následující:</span><span class="sxs-lookup"><span data-stu-id="f8bec-203">tooassign user Britta Simon tooIBM Kenexa Survey Enterprise, do hello following:</span></span>

1. <span data-ttu-id="f8bec-204">V hello portálu Azure, otevřete hello **aplikace** zobrazit, přejděte toohello **Directory** zobrazit, vyberte možnost **podnikové aplikace, které**a pak klikněte na tlačítko **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-204">In hello Azure portal, open hello **Applications** view, go toohello **Directory** view, select **Enterprise applications**, and then click **All applications**.</span></span>

    ![Hello "podnikové aplikace" a "Všechny aplikace" odkazy][201] 

2. <span data-ttu-id="f8bec-206">V hello **aplikace** seznamu, vyberte **IBM Kenexa průzkum Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-206">In hello **Applications** list, select **IBM Kenexa Survey Enterprise**.</span></span>

    ![Hello IBM Kenexa průzkum Enterprise odkaz v seznamu aplikace hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. <span data-ttu-id="f8bec-208">V levém podokně hello, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-208">In hello left pane, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="f8bec-210">Klikněte na tlačítko hello **přidat** tlačítko a pak na hello **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-210">Click hello **Add** button and then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="f8bec-212">V hello **uživatelů a skupin** dialogové okno, v hello **uživatelé** seznamu, vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="f8bec-212">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="f8bec-213">V hello **uživatelů a skupin** dialogovém okně klikněte na hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f8bec-213">In hello **Users and groups** dialog box, click hello **Select** button.</span></span>

7. <span data-ttu-id="f8bec-214">V hello **přidat přiřazení** dialogovém okně klikněte na hello **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f8bec-214">In hello **Add Assignment** dialog box, click hello **Assign** button.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f8bec-215">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f8bec-215">Test single sign-on</span></span>

<span data-ttu-id="f8bec-216">V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f8bec-216">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="f8bec-217">Když kliknete na tlačítko hello **IBM Kenexa průzkum Enterprise** dlaždice v hello přístupového panelu, můžete by měl být automaticky přihlášeni v tooyour IBM Kenexa průzkum podniková aplikace.</span><span class="sxs-lookup"><span data-stu-id="f8bec-217">When you click hello **IBM Kenexa Survey Enterprise** tile in hello Access Panel, you should be automatically signed in tooyour IBM Kenexa Survey Enterprise application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8bec-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f8bec-218">Additional resources</span></span>

* [<span data-ttu-id="f8bec-219">Seznam kurzů toointegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f8bec-219">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f8bec-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f8bec-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
