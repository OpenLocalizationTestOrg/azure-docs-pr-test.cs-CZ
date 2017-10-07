---
title: 'Kurz: Azure Active Directory integrace s HR2day podle Merces | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a HR2day podle Merces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 257233767e1fddbaf115878cb0455acf61b2f5f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a><span data-ttu-id="c7ee0-103">Kurz: Azure Active Directory integrace s HR2day podle Merces</span><span class="sxs-lookup"><span data-stu-id="c7ee0-103">Tutorial: Azure Active Directory integration with HR2day by Merces</span></span>

<span data-ttu-id="c7ee0-104">V tomto kurzu zjistíte, jak toointegrate HR2day podle Merces službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c7ee0-104">In this tutorial, you learn how toointegrate HR2day by Merces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c7ee0-105">Integrace HR2day podle Merces s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c7ee0-105">Integrating HR2day by Merces with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c7ee0-106">Můžete ovládat ve službě Azure AD, který má přístup tooHR2day podle Merces.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-106">You can control in Azure AD who has access tooHR2day by Merces.</span></span>
- <span data-ttu-id="c7ee0-107">Můžete povolit uživatelům tooautomatically získat podepsaný v tooHR2day Merces s účty služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-107">You can enable your users tooautomatically get signed in tooHR2day by Merces with their Azure AD accounts.</span></span>
- <span data-ttu-id="c7ee0-108">Můžete spravovat vaše účty v jednom centrálním místě – hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-108">You can manage your accounts in one central location--hello Azure portal.</span></span>

<span data-ttu-id="c7ee0-109">Další informace o integraci aplikací SaaS v Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c7ee0-109">For more information about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7ee0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c7ee0-110">Prerequisites</span></span>

<span data-ttu-id="c7ee0-111">Integrace služby Azure AD s HR2day podle Merces tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c7ee0-111">tooconfigure Azure AD integration with HR2day by Merces, you need hello following items:</span></span>

- <span data-ttu-id="c7ee0-112">Předplatné služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="c7ee0-113">HR2day podle Merces jednotné přihlašování povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-113">An HR2day by Merces single sign-on enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="c7ee0-114">Není doporučeno, že produkční prostředí tootest hello pomocí kroků v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-114">We don't recommend using a production environment tootest hello steps in this tutorial.</span></span>

<span data-ttu-id="c7ee0-115">tootest hello kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="c7ee0-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="c7ee0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-116">Don't use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="c7ee0-117">Získání [jeden měsíc bezplatnou zkušební verzi Azure AD](https://azure.microsoft.com/pricing/free-trial/) Pokud ještě nemáte ho.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-117">Get a [one-month free trial of Azure AD](https://azure.microsoft.com/pricing/free-trial/) if you don't already have it.</span></span>  

## <a name="scenario-description"></a><span data-ttu-id="c7ee0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c7ee0-118">Scenario description</span></span>
<span data-ttu-id="c7ee0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c7ee0-120">Hello scénář, který je zde uvedených zahrnuje dva hlavní stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c7ee0-120">hello scenario that's outlined here consists of two main building blocks:</span></span>

1. <span data-ttu-id="c7ee0-121">Přidání HR2day podle Merces z Galerie hello.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-121">Adding HR2day by Merces from hello gallery.</span></span>
2. <span data-ttu-id="c7ee0-122">Konfigurace a testování Azure AD jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-hr2day-by-merces-from-hello-gallery"></a><span data-ttu-id="c7ee0-123">Přidat HR2day podle Merces z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c7ee0-123">Add HR2day by Merces from hello gallery</span></span>
<span data-ttu-id="c7ee0-124">tooconfigure hello integrace HR2day podle Merces do Azure AD, přidejte HR2day podle Merces hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-124">tooconfigure hello integration of HR2day by Merces into Azure AD, add HR2day by Merces from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c7ee0-125">**tooadd HR2day podle Merces z Galerie hello hello proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c7ee0-125">**tooadd HR2day by Merces from hello gallery, take hello following steps:**</span></span>

1. <span data-ttu-id="c7ee0-126">V hello [portál Azure](https://portal.azure.com), na hello levém navigačním podokně, vyberte hello **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-126">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c7ee0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-128">Go too**Enterprise applications**.</span></span> <span data-ttu-id="c7ee0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c7ee0-131">tooadd novou aplikaci, vyberte hello **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-131">tooadd a new application, select hello **New application** button on hello top of hello dialog box.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c7ee0-133">Hello vyhledávacího pole zadejte **HR2day podle Merces**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-133">In hello search box, type **HR2day by Merces**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. <span data-ttu-id="c7ee0-135">Na panelu výsledků hello vyberte **HR2day podle Merces**a potom vyberte hello **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-135">In hello results panel, select **HR2day by Merces**, and then select hello **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c7ee0-137">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c7ee0-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="c7ee0-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s HR2day podle Merces podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c7ee0-138">In this section, you configure and test Azure AD single sign-on with HR2day by Merces based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c7ee0-139">Pro toowork jeden přihlašování musí Azure AD tooknow, který je hello příslušného uživatele v HR2day podle Merces tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-139">For single sign-on toowork, Azure AD needs tooknow who hello counterpart user in HR2day by Merces is tooa user in Azure AD.</span></span> <span data-ttu-id="c7ee0-140">Jinými slovy budete potřebovat tooestablish odkaz mezi uživatele Azure AD a související uživatelské hello v HR2day podle Merces.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-140">In other words, you need tooestablish a link between an Azure AD user and hello related user in HR2day by Merces.</span></span>

<span data-ttu-id="c7ee0-141">V HR2day podle Merces přiřadit hello **uživatelské jméno** ve službě Azure AD příliš **uživatelské jméno** tooestablish hello vztah.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-141">In HR2day by Merces, assign hello **user name** in Azure AD too **Username** tooestablish hello relationship.</span></span>

<span data-ttu-id="c7ee0-142">tooconfigure a testu Azure AD jednotné přihlašování s HR2day podle Merces, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c7ee0-142">tooconfigure and test Azure AD single sign-on with HR2day by Merces, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c7ee0-143">[Konfigurovat Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on): Povolit toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-143">[Configure Azure AD single sign-on](#configuring-azure-ad-single-sign-on): Enable your users toouse this feature.</span></span>
2. <span data-ttu-id="c7ee0-144">[Vytvořit testovací uživatele Azure AD](#creating-an-azure-ad-test-user): Test Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-144">[Create an Azure AD test user](#creating-an-azure-ad-test-user): Test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c7ee0-145">[Vytvoření HR2day uživatelem Merces testovací](#creating-an-hr2day-by-merces-test-user): vytvoření protějšek Britta Simon v HR2day podle Merces, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-145">[Create an HR2day by Merces test user](#creating-an-hr2day-by-merces-test-user): Create a counterpart of Britta Simon in HR2day by Merces that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c7ee0-146">[Přiřadit hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user): Povolit Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-146">[Assign hello Azure AD test user](#assigning-the-azure-ad-test-user): Enable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c7ee0-147">[Test jednotného přihlašování](#testing-single-sign-on): Ověřte, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-147">[Test single sign-on](#testing-single-sign-on): Verify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c7ee0-148">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c7ee0-148">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c7ee0-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaše HR2day Merces aplikací.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HR2day by Merces application.</span></span>

<span data-ttu-id="c7ee0-150">**tooconfigure Azure AD jednotné přihlašování s HR2day podle Merces, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c7ee0-150">**tooconfigure Azure AD single sign-on with HR2day by Merces, take hello following steps:**</span></span>

1. <span data-ttu-id="c7ee0-151">V portálu Azure, na hello hello **HR2day podle Merces** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-151">In hello Azure portal, on hello **HR2day by Merces** application integration page, select **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c7ee0-153">tooenable jednotné přihlašování, v hello **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-153">tooenable single sign-on, in hello **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. <span data-ttu-id="c7ee0-155">V hello **HR2day Merces domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c7ee0-155">In hello **HR2day by Merces Domain and URLs** section, take hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    <span data-ttu-id="c7ee0-157">a.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-157">a.</span></span> <span data-ttu-id="c7ee0-158">V hello **přihlašovací adresa URL** pole, zadejte adresu URL pomocí hello následující vzor: `https://<tenantname>.force.com/<instancename>`.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-158">In hello **Sign-on URL** box, type a URL by using hello following pattern: `https://<tenantname>.force.com/<instancename>`.</span></span>

    <span data-ttu-id="c7ee0-159">b.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-159">b.</span></span> <span data-ttu-id="c7ee0-160">V hello **identifikátor** pole, zadejte adresu URL pomocí hello následující vzor: `https://hr2day.force.com/<companyname>`.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-160">In hello **Identifier** box, type a URL by using hello following pattern: `https://hr2day.force.com/<companyname>`.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c7ee0-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-161">These values are not real.</span></span> <span data-ttu-id="c7ee0-162">Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-162">Update these values with hello actual sign-on URL and identifier.</span></span> <span data-ttu-id="c7ee0-163">Kontaktujte hello [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-163">Contact hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl) tooget these values.</span></span> 
 


4. <span data-ttu-id="c7ee0-164">Na hello **SAML podpisový certifikát** vyberte **Certificate(Base64)**a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-164">On hello **SAML Signing Certificate** section, select **Certificate(Base64)**, and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. <span data-ttu-id="c7ee0-166">Tato část popisuje, jak tooenable tooHR2day uživatelé tooauthenticate podle Merces ke svému účtu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-166">This section describes how tooenable users tooauthenticate tooHR2day by Merces with their account in Azure AD.</span></span> <span data-ttu-id="c7ee0-167">Budou to provedete pomocí federace, která je založená na hello protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-167">They do this by using federation that's based on hello SAML protocol.</span></span>

    <span data-ttu-id="c7ee0-168">Vaše HR2day aplikací Merces očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour SAML token.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-168">Your HR2day by Merces application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token.</span></span> <span data-ttu-id="c7ee0-169">Hello následující snímek obrazovky ukazuje příklad tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-169">hello following screenshot shows an example of this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    <span data-ttu-id="c7ee0-171">Před konfigurací hello kontrolního výrazu SAML, bude nutné se obrátit hello [HR2day tým podpory Merces klienta](mailto:servicedesk@merces.nl) a požadovat hello hodnotu atributu hello jedinečný identifikátor pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-171">Before you can configure hello SAML assertion, you must contact hello [HR2day by Merces Client support team](mailto:servicedesk@merces.nl) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="c7ee0-172">Je nutné tuto hodnotu toocomplete hello kroků v další části hello.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-172">You need this value toocomplete hello steps in hello next section.</span></span> 

6. <span data-ttu-id="c7ee0-173">V hello **jednotného přihlašování** dialogové okno, v hello **uživatelské atributy** atribut tokenu SAML hello nakonfigurujte, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-173">In hello **Single sign-on** dialog box, in hello **User Attributes** section, configure hello SAML token attribute as shown in hello following image.</span></span> <span data-ttu-id="c7ee0-174">Proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-174">Then take hello following steps.</span></span>
    
      | <span data-ttu-id="c7ee0-175">Název atributu</span><span class="sxs-lookup"><span data-stu-id="c7ee0-175">Attribute name</span></span>    |   <span data-ttu-id="c7ee0-176">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="c7ee0-176">Attribute value</span></span> |  
    | ------------------- | -------------------- |    
    | <span data-ttu-id="c7ee0-177">ATTR_LOGINCLAIM</span><span class="sxs-lookup"><span data-stu-id="c7ee0-177">ATTR_LOGINCLAIM</span></span> | <span data-ttu-id="c7ee0-178">spojení ([pošty] "102938475Z", "@"</span><span class="sxs-lookup"><span data-stu-id="c7ee0-178">join([mail],"102938475Z","@"</span></span> |
    
      <span data-ttu-id="c7ee0-179">a.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-179">a.</span></span> <span data-ttu-id="c7ee0-180">tooopen hello **přidat atribut** dialogovém okně, vyberte **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-180">tooopen hello **Add Attribute** dialog, select **Add attribute**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="c7ee0-183">b.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-183">b.</span></span> <span data-ttu-id="c7ee0-184">V hello **název** zadejte **ATTR_LOGINCLAIM**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-184">In hello **Name** box, type **ATTR_LOGINCLAIM**.</span></span>

    <span data-ttu-id="c7ee0-185">c.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-185">c.</span></span> <span data-ttu-id="c7ee0-186">Z hello **hodnotu** seznamu, vyberte **Join()**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-186">From hello **Value** list, select **Join()**.</span></span>

    <span data-ttu-id="c7ee0-187">d.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-187">d.</span></span> <span data-ttu-id="c7ee0-188">Z hello **řetězec1** seznamu, vyberte **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-188">From hello **String1** list, select **user.mail**.</span></span>

    <span data-ttu-id="c7ee0-189">e.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-189">e.</span></span> <span data-ttu-id="c7ee0-190">Pro **řetězec2**, zadejte hello jedinečný identifikátor, který zajišťuje HR2day týmu.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-190">For **String2**, type hello unique identifier that's provided by your HR2day team.</span></span>

    <span data-ttu-id="c7ee0-191">f.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-191">f.</span></span> <span data-ttu-id="c7ee0-192">V hello **oddělovače** zadejte  **@** .</span><span class="sxs-lookup"><span data-stu-id="c7ee0-192">In hello **Separator** box, type **@**.</span></span>
    
    <span data-ttu-id="c7ee0-193">g.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-193">g.</span></span> <span data-ttu-id="c7ee0-194">Vyberte **Ok**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-194">Select **Ok**.</span></span>

7. <span data-ttu-id="c7ee0-195">Vyberte hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-195">Select hello **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c7ee0-197">V hello **HR2day Merces konfigurace** vyberte **konfigurace HR2day podle Merces** tooopen hello **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-197">In hello **HR2day by Merces Configuration** section, select **Configure HR2day by Merces** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="c7ee0-198">Kopírování hello **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-198">Copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** from hello **Quick Reference** section.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. <span data-ttu-id="c7ee0-200">tooconfigure jednotné přihlašování pro vaše aplikace, kontaktujte hello [HR2day tým podpory klienta Merces](mailTo:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="c7ee0-200">tooconfigure SSO  for your application, contact hello [HR2day by Merces client support team](mailTo:servicedesk@merces.nl).</span></span> <span data-ttu-id="c7ee0-201">Připojte hello Stáhnout **Certificate(Base64)** souboru tooyour e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-201">Attach hello downloaded **Certificate(Base64)** file tooyour email.</span></span> <span data-ttu-id="c7ee0-202">Také poskytují hello **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** tak, aby mohly být konfigurovány pro integraci jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-202">Also provide hello **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** so that they can be configured for SSO integration.</span></span>

    > [!NOTE]
    ><span data-ttu-id="c7ee0-203">Zmínili toohello Merces týmu, který potřebuje této integrace hello Entity ID toobe nastavit pomocí vzoru hello **https://hr2day.force.com/INSTANCENAME**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-203">Mention toohello Merces team that this integration needs hello Entity ID toobe set with hello pattern **https://hr2day.force.com/INSTANCENAME**.</span></span>

    > [!TIP]
    ><span data-ttu-id="c7ee0-204">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c7ee0-204">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c7ee0-205">Po přidání této aplikace z hello **služby Active Directory** > **podnikové aplikace, které** části, vyberte hello **jednotné přihlašování** kartě. Pak vloží hello přístup k dokumentaci prostřednictvím hello **konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-205">After you add this app from hello **Active Directory** > **Enterprise Applications** section, select hello **Single Sign-On** tab. Then access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c7ee0-206">Další informace o funkci hello embedded dokumentace v hello [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="c7ee0-206">You can read more about hello embedded documentation feature in hello [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c7ee0-207">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c7ee0-207">Create an Azure AD test user</span></span>
<span data-ttu-id="c7ee0-208">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-208">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c7ee0-210">**toocreate testovacího uživatele ve službě Azure AD, hello proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c7ee0-210">**toocreate a test user in Azure AD, take hello following steps:**</span></span>

1. <span data-ttu-id="c7ee0-211">V hello **portál Azure**, na hello levém navigačním podokně, vyberte hello **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-211">In hello **Azure portal**, on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c7ee0-213">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-213">toodisplay hello list of users, go too**Users and groups**, and then select **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c7ee0-215">tooopen hello **uživatele** dialogové okno, vyberte **přidat** hello nahoře dialogového okna hello.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-215">tooopen hello **User** dialog box, select **Add** on hello top of hello dialog box.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c7ee0-217">V hello **uživatele** dialogové okno, hello proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c7ee0-217">In hello **User** dialog box, take hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c7ee0-219">a.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-219">a.</span></span> <span data-ttu-id="c7ee0-220">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-220">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c7ee0-221">b.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-221">b.</span></span> <span data-ttu-id="c7ee0-222">V hello **uživatelské jméno** pole, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-222">In hello **User name** box, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c7ee0-223">c.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-223">c.</span></span> <span data-ttu-id="c7ee0-224">Vyberte **zobrazit hesla**a zapište si ji hello heslo.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-224">Select **Show Password**, and then write down hello password.</span></span>

    <span data-ttu-id="c7ee0-225">d.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-225">d.</span></span> <span data-ttu-id="c7ee0-226">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-226">Select **Create**.</span></span>
 
### <a name="create-an-hr2day-by-merces-test-user"></a><span data-ttu-id="c7ee0-227">Vytvoření HR2day Merces testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="c7ee0-227">Create an HR2day by Merces test user</span></span>

<span data-ttu-id="c7ee0-228">Hello cílem této části je toocreate volá Britta Simon v HR2day Merces uživatele.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-228">hello objective of this section is toocreate a user called Britta Simon in HR2day by Merces.</span></span> <span data-ttu-id="c7ee0-229">Uživatelé hello tooadd v účtu hello HR2day pracovat s hello [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="c7ee0-229">tooadd hello users in hello HR2day account, work with hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span> 

> [!NOTE]
> <span data-ttu-id="c7ee0-230">Pokud potřebujete toocreate uživatel ručně, obraťte se na hello [HR2day tým podpory klienta Merces](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="c7ee0-230">If you need toocreate a user manually, contact hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c7ee0-231">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c7ee0-231">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c7ee0-232">V této části povolíte tak, že udělíte tooHR2day svůj přístup podle Merces toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooHR2day by Merces.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c7ee0-234">**tooassign Britta Simon tooHR2day podle Merces hello proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c7ee0-234">**tooassign Britta Simon tooHR2day by Merces, take hello following steps:**</span></span>

1. <span data-ttu-id="c7ee0-235">V hello Azure aplikace hello portálu, otevřete zobrazení, přejděte toohello directory zobrazení a potom přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-235">In hello Azure portal, open hello applications view, go toohello directory view, and then go too**Enterprise applications**.</span></span> <span data-ttu-id="c7ee0-236">Potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-236">Next, select **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c7ee0-238">V seznamu aplikace hello vyberte **HR2day podle Merces**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-238">In hello applications list, select **HR2day by Merces**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. <span data-ttu-id="c7ee0-240">V nabídce hello na levé straně hello vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-240">In hello menu on hello left, select **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c7ee0-242">Vyberte hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-242">Select hello **Add** button.</span></span> <span data-ttu-id="c7ee0-243">Potom v hello **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-243">Then, in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c7ee0-245">V hello **uživatelů a skupin** dialogové okno, v hello **uživatelé** seznamu, vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-245">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="c7ee0-246">Klikněte na tlačítko hello **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-246">Click hello **Select** button.</span></span>

7. <span data-ttu-id="c7ee0-247">V hello **přidat přiřazení** dialogové okno, vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-247">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c7ee0-248">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c7ee0-248">Test single sign-on</span></span>

<span data-ttu-id="c7ee0-249">Hello cílem této části je tootest vaše konfigurace Azure AD jeden přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-249">hello objective of this section is tootest your Azure AD single sign-on configuration by using hello Access Panel.</span></span>  

<span data-ttu-id="c7ee0-250">Když vyberete hello HR2day podle Merces dlaždice v hello přístupového panelu, můžete získat přihlášeni automaticky tooyour HR2day Merces aplikací.</span><span class="sxs-lookup"><span data-stu-id="c7ee0-250">When you select hello HR2day by Merces tile in hello Access Panel, you automatically get signed in  tooyour HR2day by Merces application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7ee0-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c7ee0-251">Additional resources</span></span>

* [<span data-ttu-id="c7ee0-252">Seznam kurzů o tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c7ee0-252">List of tutorials about how tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c7ee0-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c7ee0-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

