---
title: 'Kurz: Azure Active Directory integrace s TINFOIL SECURITY | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TINFOIL SECURITY."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1a3fa9880d9e026c2d6d6548188df2269ff69139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a><span data-ttu-id="7f1c5-103">Kurz: Azure Active Directory integrace s TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="7f1c5-103">Tutorial: Azure Active Directory integration with TINFOIL SECURITY</span></span>

<span data-ttu-id="7f1c5-104">V tomto kurzu zjistíte, jak toointegrate TINFOIL SECURITY s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7f1c5-104">In this tutorial, you learn how toointegrate TINFOIL SECURITY with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7f1c5-105">Integrace TINFOIL SECURITY s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7f1c5-105">Integrating TINFOIL SECURITY with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7f1c5-106">Můžete řídit ve službě Azure AD, který má přístup tooTINFOIL zabezpečení</span><span class="sxs-lookup"><span data-stu-id="7f1c5-106">You can control in Azure AD who has access tooTINFOIL SECURITY</span></span>
- <span data-ttu-id="7f1c5-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTINFOIL zabezpečení (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f1c5-107">You can enable your users tooautomatically get signed-on tooTINFOIL SECURITY (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7f1c5-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7f1c5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7f1c5-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7f1c5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f1c5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f1c5-110">Prerequisites</span></span>

<span data-ttu-id="7f1c5-111">tooconfigure integrace Azure AD s TINFOIL SECURITY, budete potřebovat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7f1c5-111">tooconfigure Azure AD integration with TINFOIL SECURITY, you need hello following items:</span></span>

- <span data-ttu-id="7f1c5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f1c5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7f1c5-113">TINFOIL SECURITY jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7f1c5-113">A TINFOIL SECURITY single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7f1c5-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7f1c5-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7f1c5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7f1c5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7f1c5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7f1c5-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7f1c5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7f1c5-118">Scenario description</span></span>
<span data-ttu-id="7f1c5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7f1c5-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7f1c5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7f1c5-121">Přidat TINFOIL SECURITY z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7f1c5-121">Add TINFOIL SECURITY from hello gallery</span></span>
2. <span data-ttu-id="7f1c5-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f1c5-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tinfoil-security-from-hello-gallery"></a><span data-ttu-id="7f1c5-123">Přidat TINFOIL SECURITY z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7f1c5-123">Add TINFOIL SECURITY from hello gallery</span></span>
<span data-ttu-id="7f1c5-124">tooconfigure hello integrace TINFOIL SECURITY do Azure AD, je nutné tooadd TINFOIL SECURITY hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-124">tooconfigure hello integration of TINFOIL SECURITY into Azure AD, you need tooadd TINFOIL SECURITY from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7f1c5-125">**tooadd TINFOIL SECURITY z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f1c5-125">**tooadd TINFOIL SECURITY from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f1c5-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7f1c5-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7f1c5-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7f1c5-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7f1c5-133">Hello vyhledávacího pole zadejte **TINFOIL SECURITY**, vyberte **TINFOIL SECURITY** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-133">In hello search box, type **TINFOIL SECURITY**, select  **TINFOIL SECURITY** from result panel then click **Add** button tooadd hello application.</span></span>

    ![TINFOIL SECURITY z Galerie](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="7f1c5-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f1c5-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="7f1c5-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TINFOIL SECURITY podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7f1c5-136">In this section, you configure and test Azure AD single sign-on with TINFOIL SECURITY based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7f1c5-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TINFOIL SECURITY je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TINFOIL SECURITY is tooa user in Azure AD.</span></span> <span data-ttu-id="7f1c5-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TINFOIL SECURITY musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-138">In other words, a link relationship between an Azure AD user and hello related user in TINFOIL SECURITY needs toobe established.</span></span>

<span data-ttu-id="7f1c5-139">V TINFOIL SECURITY přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-139">In TINFOIL SECURITY, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7f1c5-140">tooconfigure a testu Azure AD jednotné přihlašování s TINFOIL SECURITY, budete potřebovat následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7f1c5-140">tooconfigure and test Azure AD single sign-on with TINFOIL SECURITY, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7f1c5-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7f1c5-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7f1c5-143">**[Vytvoření zkušebního uživatele TINFOIL SECURITY](#create-a-tinfoil-security-test-user)**  -toohave protějšek Britta Simon v TINFOIL SECURITY, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-143">**[Create a TINFOIL SECURITY test user](#create-a-tinfoil-security-test-user)** - toohave a counterpart of Britta Simon in TINFOIL SECURITY that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7f1c5-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7f1c5-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="7f1c5-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f1c5-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="7f1c5-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TINFOIL SECURITY application.</span></span>

<span data-ttu-id="7f1c5-148">**tooconfigure Azure AD jednotné přihlašování s TINFOIL SECURITY, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f1c5-148">**tooconfigure Azure AD single sign-on with TINFOIL SECURITY, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f1c5-149">V portálu Azure, na hello hello **TINFOIL SECURITY** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-149">In hello Azure portal, on hello **TINFOIL SECURITY** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7f1c5-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Přihlášení na základě SAML](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. <span data-ttu-id="7f1c5-153">Na hello **TINFOIL zabezpečení domény a adresy URL** část, hello uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-153">On hello **TINFOIL SECURITY Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. <span data-ttu-id="7f1c5-155">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-155">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. <span data-ttu-id="7f1c5-157">mapování atributů tooadd hello vyžaduje, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7f1c5-157">tooadd hello required attribute mappings, perform hello following steps:</span></span>
    
    <span data-ttu-id="7f1c5-158">![Atributy](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="7f1c5-158">![Attributes](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "Attributes")</span></span>
    
    | <span data-ttu-id="7f1c5-159">Název atributu</span><span class="sxs-lookup"><span data-stu-id="7f1c5-159">Attribute Name</span></span>    |   <span data-ttu-id="7f1c5-160">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="7f1c5-160">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="7f1c5-161">ID účtu</span><span class="sxs-lookup"><span data-stu-id="7f1c5-161">accountid</span></span> | <span data-ttu-id="7f1c5-162">UXXXXXXXXXXXXX</span><span class="sxs-lookup"><span data-stu-id="7f1c5-162">UXXXXXXXXXXXXX</span></span> |
    
    <span data-ttu-id="7f1c5-163">a.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-163">a.</span></span> <span data-ttu-id="7f1c5-164">Klikněte na tlačítko **přidat atribut uživatele**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-164">Click **add user attribute**.</span></span>
    
    <span data-ttu-id="7f1c5-165">![Přidat atribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="7f1c5-165">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "Attributes")</span></span>
    
    <span data-ttu-id="7f1c5-166">![Přidat atribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="7f1c5-166">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "Attributes")</span></span>
    
    <span data-ttu-id="7f1c5-167">b.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-167">b.</span></span> <span data-ttu-id="7f1c5-168">V hello **název atributu** textovému poli, typ **accountid**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-168">In hello **Attribute Name** textbox, type **accountid**.</span></span>
    
    <span data-ttu-id="7f1c5-169">c.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-169">c.</span></span> <span data-ttu-id="7f1c5-170">V hello **hodnota atributu** textovému poli, vložte hello účet ID hodnotu, která budete mít později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-170">In hello **Attribute Value** textbox, paste hello account ID value which you will get later on hello tutorial.</span></span>
    
    <span data-ttu-id="7f1c5-171">d.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-171">d.</span></span> <span data-ttu-id="7f1c5-172">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-172">Click **Ok**.</span></span>    

6. <span data-ttu-id="7f1c5-173">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-173">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="7f1c5-175">Na hello **TINFOIL SECURITY Configuration** klikněte na tlačítko **konfigurace TINFOIL SECURITY** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-175">On hello **TINFOIL SECURITY Configuration** section, click **Configure TINFOIL SECURITY** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7f1c5-176">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7f1c5-176">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurace zabezpečení aplikace TINFOIL](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. <span data-ttu-id="7f1c5-178">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-178">In a different web browser window, log into your TINFOIL SECURITY company site as an administrator.</span></span>

9. <span data-ttu-id="7f1c5-179">V panelu nástrojů hello hello nahoře, klikněte na **Můj účet**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-179">In hello toolbar on hello top, click **My Account**.</span></span>
   
    <span data-ttu-id="7f1c5-180">![Řídicí panel](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "řídicí panel")</span><span class="sxs-lookup"><span data-stu-id="7f1c5-180">![Dashboard](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "Dashboard")</span></span>

10. <span data-ttu-id="7f1c5-181">Klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-181">Click **Security**.</span></span>
   
    <span data-ttu-id="7f1c5-182">![Zabezpečení](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="7f1c5-182">![Security](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "Security")</span></span>

11. <span data-ttu-id="7f1c5-183">Na hello **jednotné přihlašování** konfigurace proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f1c5-183">On hello **Single Sign-On** configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="7f1c5-184">![Jednotné přihlašování](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7f1c5-184">![Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="7f1c5-185">a.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-185">a.</span></span> <span data-ttu-id="7f1c5-186">Vyberte **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-186">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="7f1c5-187">b.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-187">b.</span></span> <span data-ttu-id="7f1c5-188">Klikněte na tlačítko **ruční konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-188">Click **Manual Configuration**.</span></span>
   
    <span data-ttu-id="7f1c5-189">c.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-189">c.</span></span> <span data-ttu-id="7f1c5-190">V **SAML Post URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7f1c5-190">In **SAML Post URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal</span></span>
   
    <span data-ttu-id="7f1c5-191">d.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-191">d.</span></span> <span data-ttu-id="7f1c5-192">V **otisků certifikátu SAML** textovému poli, vložte hodnotu hello **kryptografický otisk** který jste zkopírovali z **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-192">In **SAML Certificate Fingerprint** textbox, paste hello value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span>
  
    <span data-ttu-id="7f1c5-193">e.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-193">e.</span></span> <span data-ttu-id="7f1c5-194">Kopírování **vaše ID účtu** a vložte hodnotu hello v **hodnota atributu** textového pole pod **přidat atribut** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-194">Copy **Your Account ID** value and paste hello value in **Attribute Value** textbox under **Add Attribute** section in Azure portal.</span></span>
   
    <span data-ttu-id="7f1c5-195">f.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-195">f.</span></span> <span data-ttu-id="7f1c5-196">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-196">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="7f1c5-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="7f1c5-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7f1c5-198">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7f1c5-199">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7f1c5-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="7f1c5-200">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f1c5-200">Create an Azure AD test user</span></span>
<span data-ttu-id="7f1c5-201">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7f1c5-203">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f1c5-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f1c5-204">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7f1c5-206">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![<span data-ttu-id="7f1c5-207">Uživatelé a skupiny -> všichni uživatelé</span><span class="sxs-lookup"><span data-stu-id="7f1c5-207">Users and groups -> All users</span></span> ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7f1c5-208">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Uživatel](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7f1c5-210">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f1c5-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7f1c5-212">a.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-212">a.</span></span> <span data-ttu-id="7f1c5-213">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7f1c5-214">b.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-214">b.</span></span> <span data-ttu-id="7f1c5-215">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7f1c5-216">c.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-216">c.</span></span> <span data-ttu-id="7f1c5-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7f1c5-218">d.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-218">d.</span></span> <span data-ttu-id="7f1c5-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-219">Click **Create**.</span></span>
 
### <a name="create-a-tinfoil-security-test-user"></a><span data-ttu-id="7f1c5-220">Vytvoření zkušebního uživatele TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="7f1c5-220">Create a TINFOIL SECURITY test user</span></span>

<span data-ttu-id="7f1c5-221">V pořadí tooenable Azure AD Uživatelé toolog do TINFOIL SECURITY musí být zřízená do TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-221">In order tooenable Azure AD users toolog into TINFOIL SECURITY, they must be provisioned into TINFOIL SECURITY.</span></span> <span data-ttu-id="7f1c5-222">V případě hello TINFOIL SECURITY zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-222">In hello case of TINFOIL SECURITY, provisioning is a manual task.</span></span>

<span data-ttu-id="7f1c5-223">**tooget uživatele zřízený, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f1c5-223">**tooget a user provisioned, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f1c5-224">Pokud je uživatel hello součástí účet podnikové sítě, je třeba příliš[kontaktujte tým podpory TINFOIL SECURITY hello](https://www.tinfoilsecurity.com/contact) tooget hello uživatelský účet vytvořený.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-224">If hello user is a part of an Enterprise account, you need too[contact hello TINFOIL SECURITY support team](https://www.tinfoilsecurity.com/contact) tooget hello user account created.</span></span>

2. <span data-ttu-id="7f1c5-225">Pokud je uživatel hello běžný uživatel TINFOIL SECURITY SaaS, můžete přidat uživatele hello spolupracovník tooany lokalit hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-225">If hello user is a regular TINFOIL SECURITY SaaS user, then hello user can add a collaborator tooany of hello user’s sites.</span></span> <span data-ttu-id="7f1c5-226">Tento aktivační události procesu toosend, toohello pozvánku k zadané e-mailu toocreate nový uživatelský účet TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-226">This triggers a process toosend an invitation toohello specified email toocreate a new TINFOIL SECURITY user account.</span></span>

> [!NOTE]
> <span data-ttu-id="7f1c5-227">Můžete použít všechny ostatní TINFOIL SECURITY uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TINFOIL SECURITY tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-227">You can use any other TINFOIL SECURITY user account creation tools or APIs provided by TINFOIL SECURITY tooprovision Azure AD user accounts.</span></span>
> 
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="7f1c5-228">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7f1c5-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="7f1c5-229">V této části povolíte tak, že udělíte přístup tooTINFOIL zabezpečení Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTINFOIL SECURITY.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7f1c5-231">**tooassign Britta Simon tooTINFOIL zabezpečení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f1c5-231">**tooassign Britta Simon tooTINFOIL SECURITY, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f1c5-232">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7f1c5-234">V seznamu aplikace hello vyberte **TINFOIL SECURITY**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-234">In hello applications list, select **TINFOIL SECURITY**.</span></span>

    ![Vyberte TINFOIL SECURITY](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. <span data-ttu-id="7f1c5-236">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7f1c5-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-238">Click **Add** button.</span></span> <span data-ttu-id="7f1c5-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7f1c5-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7f1c5-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7f1c5-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="7f1c5-244">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f1c5-244">Test single sign-on</span></span>

<span data-ttu-id="7f1c5-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7f1c5-246">Když kliknete na dlaždici TINFOIL SECURITY hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="7f1c5-246">When you click hello TINFOIL SECURITY tile in hello Access Panel, you should get automatically signed-on tooyour TINFOIL SECURITY application.</span></span> <span data-ttu-id="7f1c5-247">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7f1c5-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f1c5-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7f1c5-248">Additional resources</span></span>

* [<span data-ttu-id="7f1c5-249">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7f1c5-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7f1c5-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7f1c5-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

