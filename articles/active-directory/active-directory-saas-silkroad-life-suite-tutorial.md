---
title: "Kurz: Azure Active Directory integrace s SilkRoad životnosti Suite | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SilkRoad životnosti Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: 07367282ab42b7332f166d64743b4b447aec4935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a><span data-ttu-id="64590-103">Kurz: Azure Active Directory integrace s SilkRoad životnosti Suite</span><span class="sxs-lookup"><span data-stu-id="64590-103">Tutorial: Azure Active Directory integration with SilkRoad Life Suite</span></span>
<span data-ttu-id="64590-104">cílem Hello tohoto kurzu je tooshow můžete jak toointegrate SilkRoad životnosti Suite a Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="64590-104">hello objective of this tutorial is tooshow you how toointegrate SilkRoad Life Suite with Azure Active Directory (Azure AD).</span></span> 

<span data-ttu-id="64590-105">Integrace SilkRoad životnosti Suite s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="64590-105">Integrating SilkRoad Life Suite with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="64590-106">Můžete řídit ve službě Azure AD, který má přístup tooSilkRoad Suite životnosti</span><span class="sxs-lookup"><span data-stu-id="64590-106">You can control in Azure AD who has access tooSilkRoad Life Suite</span></span> 
* <span data-ttu-id="64590-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSilkRoad Suite života jednotného přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="64590-107">You can enable your users tooautomatically get signed-on tooSilkRoad Life Suite single sign-on (SSO) with their Azure AD accounts</span></span>

<span data-ttu-id="64590-108">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="64590-108">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64590-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="64590-109">Prerequisites</span></span>
<span data-ttu-id="64590-110">tooconfigure integrace Azure AD s SilkRoad životnosti Suite, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="64590-110">tooconfigure Azure AD integration with SilkRoad Life Suite, you need hello following items:</span></span>

* <span data-ttu-id="64590-111">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="64590-111">An Azure AD subscription</span></span>
* <span data-ttu-id="64590-112">Předplatné SilkRoad životnosti Suite jednotné přihlašování povoleno</span><span class="sxs-lookup"><span data-stu-id="64590-112">A SilkRoad Life Suite SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="64590-113">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="64590-113">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="64590-114">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="64590-114">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="64590-115">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="64590-115">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="64590-116">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64590-116">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="64590-117">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="64590-117">Scenario Description</span></span>
<span data-ttu-id="64590-118">cílem Hello tohoto kurzu je tooenable jste tootest Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="64590-118">hello objective of this tutorial is tooenable you tootest Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="64590-119">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="64590-119">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="64590-120">Přidání SilkRoad životnosti Suite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="64590-120">Adding SilkRoad Life Suite from hello gallery</span></span> 
2. <span data-ttu-id="64590-121">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="64590-121">Configuring and testing Azure AD SSO</span></span>

## <a name="add-silkroad-life-suite-from-hello-gallery"></a><span data-ttu-id="64590-122">Přidat Suite životnosti SilkRoad z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="64590-122">Add SilkRoad Life Suite from hello gallery</span></span>
<span data-ttu-id="64590-123">tooconfigure hello integraci sady životnosti SilkRoad do služby Azure AD, je nutné tooadd SilkRoad životnosti Suite hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="64590-123">tooconfigure hello integration of SilkRoad Life Suite into Azure AD, you need tooadd SilkRoad Life Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="64590-124">**tooadd Suite životnosti SilkRoad z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="64590-124">**tooadd SilkRoad Life Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="64590-125">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="64590-125">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="64590-127">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="64590-127">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="64590-128">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="64590-128">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Aplikace][2]

4. <span data-ttu-id="64590-130">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="64590-130">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Aplikace][3]

5. <span data-ttu-id="64590-132">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="64590-132">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Aplikace][4]

6. <span data-ttu-id="64590-134">Hello vyhledávacího pole zadejte **SilkRoad životnosti Suite**.</span><span class="sxs-lookup"><span data-stu-id="64590-134">In hello search box, type **SilkRoad Life Suite**.</span></span>
   
    ![Aplikace][5]

7. <span data-ttu-id="64590-136">V podokně výsledků hello, vyberte **Suite životnosti SilkRoad**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="64590-136">In hello results pane, select **SilkRoad Life Suite**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Aplikace][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="64590-138">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="64590-138">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="64590-139">Hello cílem této části je tooshow můžete jak tooconfigure a testování Azure AD přihlášení SSO se SilkRoad životnosti Suite podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="64590-139">hello objective of this section is tooshow you how tooconfigure and test Azure AD SSO with SilkRoad Life Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="64590-140">Pro jednotné přihlašování toowork Azure AD musí tooknow, co uživatel hello protějškem v SilkRoad životnosti Suite tooan uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64590-140">For SSO toowork, Azure AD needs tooknow what hello counterpart user in SilkRoad Life Suite tooan user in Azure AD is.</span></span> <span data-ttu-id="64590-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v sadě životnosti SilkRoad musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="64590-141">In other words, a link relationship between an Azure AD user and hello related user in SilkRoad Life Suite needs toobe established.</span></span>

<span data-ttu-id="64590-142">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v sadě SilkRoad životnosti.</span><span class="sxs-lookup"><span data-stu-id="64590-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SilkRoad Life Suite.</span></span>

<span data-ttu-id="64590-143">tooconfigure a testu Azure AD jednotné přihlašování s SilkRoad životnosti Suite, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="64590-143">tooconfigure and test Azure AD single sign-on with SilkRoad Life Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="64590-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="64590-144">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="64590-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64590-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="64590-146">**[Vytvoření zkušebního uživatele Suite životnosti SilkRoad](#creating-a-silkroad-life-suite-test-user)**  -toohave protějšek Britta Simon životní sady SilkRoad, který je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64590-146">**[Creating a SilkRoad Life Suite test user](#creating-a-silkroad-life-suite-test-user)** - toohave a counterpart of Britta Simon in SilkRoad Life Suite that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="64590-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="64590-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="64590-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="64590-148">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="64590-149">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="64590-149">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="64590-150">Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure classic a tooconfigure jednotné přihlašování v aplikaci SilkRoad životnosti Suite.</span><span class="sxs-lookup"><span data-stu-id="64590-150">hello objective of this section is tooenable Azure AD SSO in hello Azure classic portal and tooconfigure SSO in your SilkRoad Life Suite application.</span></span>

<span data-ttu-id="64590-151">**tooconfigure Azure AD jednotné přihlašování s SilkRoad životnosti Suite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="64590-151">**tooconfigure Azure AD single sign-on with SilkRoad Life Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="64590-152">Web společnosti SilkRoad tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="64590-152">Sign-on tooyour SilkRoad company site as administrator.</span></span> 

  >[!NOTE] 
  > <span data-ttu-id="64590-153">tooobtain přístup toohello SilkRoad životnosti Suite ověřování aplikace pro konfiguraci federační služby Microsoft Azure AD, kontaktujte prosím podporu SilkRoad nebo vaším zástupcem SilkRoad služby.</span><span class="sxs-lookup"><span data-stu-id="64590-153">tooobtain access toohello SilkRoad Life Suite Authentication application for configuring federation with Microsoft Azure AD, please contact SilkRoad Support or your SilkRoad Services representative.</span></span>
  > 

2. <span data-ttu-id="64590-154">Přejděte příliš**poskytovatele služeb**a potom klikněte na **Federation podrobnosti**.</span><span class="sxs-lookup"><span data-stu-id="64590-154">Go too**Service Provider**, and then click **Federation Details**.</span></span> 
   
    ![Azure AD jednotné přihlášení][10] 

3. <span data-ttu-id="64590-156">Klikněte na tlačítko **stáhnout federačních metadat**a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="64590-156">Click **Download Federation Metadata**, and then save hello metadata file on your computer.</span></span>
   
    ![Azure AD jednotné přihlášení][11] 

4. <span data-ttu-id="64590-158">V portálu Azure classic, na hello hello **Suite životnosti SilkRoad** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**  Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="64590-158">In hello Azure classic portal, on hello **SilkRoad Life Suite** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][6] 

5. <span data-ttu-id="64590-160">Na hello **jak jste by například uživatelé toosign na tooSilkRoad životnosti Suite** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="64590-160">On hello **How would you like users toosign on tooSilkRoad Life Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][7] 

6. <span data-ttu-id="64590-162">Na hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64590-162">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>
   
    ![Azure AD jednotné přihlášení][8]   
 1. <span data-ttu-id="64590-164">V hello **přihlašovací adresa URL** textovému poli, adresa URL typu hello používá váš web Suite životnosti SilkRoad tooyour toosign na uživatele (např: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span><span class="sxs-lookup"><span data-stu-id="64590-164">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour SilkRoad Life Suite site (e.g.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span></span>  
 2. <span data-ttu-id="64590-165">Otevřete hello Stáhnout **Silkroad** soubor metadat.</span><span class="sxs-lookup"><span data-stu-id="64590-165">Open hello downloaded **Silkroad** metadata file.</span></span> 
 3. <span data-ttu-id="64590-166">Vyhledejte hello **AssertionConsumerService** značku a potom kopie hello **umístění** atribut.</span><span class="sxs-lookup"><span data-stu-id="64590-166">Locate hello **AssertionConsumerService** tag, and then copy hello **Location** attribute.</span></span>         
   
    ![Azure AD jednotné přihlášení][21] 
 4. <span data-ttu-id="64590-168">Vložte hodnotu hello do hello **adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="64590-168">Paste hello value into hello **Reply URL** textbox.</span></span>  
 5. <span data-ttu-id="64590-169">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="64590-169">Click **Next**.</span></span>

6. <span data-ttu-id="64590-170">Na hello **nakonfigurovat jednotné přihlašování v SilkRoad životnosti Suite** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64590-170">On hello **Configure single sign-on at SilkRoad Life Suite** page, perform hello following steps:</span></span>
   
    ![Azure AD jednotné přihlášení][9]  
 1. <span data-ttu-id="64590-172">Klikněte na tlačítko Stáhnout certifikát a uložte soubor hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="64590-172">Click Download certificate, and then save hello file on your computer.</span></span>  
 2. <span data-ttu-id="64590-173">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="64590-173">Click **Next**.</span></span>

7. <span data-ttu-id="64590-174">Ve vaší **SilkRoad** aplikace, klikněte na tlačítko **ověřování zdrojů**.</span><span class="sxs-lookup"><span data-stu-id="64590-174">In your **SilkRoad** application, click **Authentication Sources**.</span></span>
   
    ![Azure AD jednotné přihlášení][12] 

8. <span data-ttu-id="64590-176">Klikněte na tlačítko **přidat zdroj ověřování**.</span><span class="sxs-lookup"><span data-stu-id="64590-176">Click **Add Authentication Source**.</span></span> 
   
    ![Azure AD jednotné přihlášení][13] 

9. <span data-ttu-id="64590-178">V hello **přidat zdroj ověřování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="64590-178">In hello **Add Authentication Source** section, perform hello following steps:</span></span> 
   
    ![Azure AD jednotné přihlášení][14]  
 1. <span data-ttu-id="64590-180">V části **možnost 2 – soubor metadat**, klikněte na tlačítko **Procházet** tooupload hello stáhnout soubor metadat.</span><span class="sxs-lookup"><span data-stu-id="64590-180">Under **Option 2 - Metadata File**, click **Browse** tooupload hello downloaded metadata file.</span></span>  
 2. <span data-ttu-id="64590-181">Klikněte na tlačítko **vytvořit zprostředkovatelů Identity pomocí dat souboru**.</span><span class="sxs-lookup"><span data-stu-id="64590-181">Click **Create Identity Provider using File Data**.</span></span>

10. <span data-ttu-id="64590-182">V hello **ověřování zdrojů** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="64590-182">In hello **Authentication Sources** section, click **Edit**.</span></span> 
    
     ![Azure AD jednotné přihlášení][15] 

11. <span data-ttu-id="64590-184">Na hello **upravit zdroj ověřování** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="64590-184">On hello **Edit Authentication Source** dialog, perform hello following steps:</span></span> 
    
     ![Azure AD jednotné přihlášení][16] 
 1. <span data-ttu-id="64590-186">Jako **povoleno**, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="64590-186">As **Enabled**, select **Yes**.</span></span>   
 2. <span data-ttu-id="64590-187">V hello **IdP popis** textovému poli, zadejte popis pro konfiguraci (například: *Azure AD jednotného přihlašování k*).</span><span class="sxs-lookup"><span data-stu-id="64590-187">In hello **IdP Description** textbox, type a description for your configuration (e.g.: *Azure AD SSO*).</span></span>  
 3. <span data-ttu-id="64590-188">V hello **IdP název** textovému poli, zadejte název, který je konkrétní tooyour konfigurace (např: *Azure SP*).</span><span class="sxs-lookup"><span data-stu-id="64590-188">In hello **IdP Name** textbox, type a name that is specific tooyour configuration (e.g.: *Azure SP*).</span></span>  
 4. <span data-ttu-id="64590-189">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="64590-189">Click **Save**.</span></span>

12. <span data-ttu-id="64590-190">Zakažte všechny ostatní zdroje ověřování.</span><span class="sxs-lookup"><span data-stu-id="64590-190">Disable all other authentication sources.</span></span> 
    
     ![Azure AD jednotné přihlášení][17]

13. <span data-ttu-id="64590-192">V portálu Azure classic, na hello hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="64590-192">In hello Azure classic portal, on hello **Single sign-on confirmation** page, click **Next**.</span></span>  
    
     ![Azure AD jednotné přihlášení][18]

14. <span data-ttu-id="64590-194">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="64590-194">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
    
     ![Azure AD jednotné přihlášení][19]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="64590-196">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="64590-196">Create an Azure AD test user</span></span>
<span data-ttu-id="64590-197">Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="64590-197">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="64590-199">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="64590-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="64590-200">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="64590-200">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. <span data-ttu-id="64590-202">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="64590-202">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="64590-203">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="64590-203">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="64590-205">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="64590-205">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="64590-207">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64590-207">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. <span data-ttu-id="64590-209">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="64590-209">As Type Of User, select New user in your organization.</span></span>  
 2. <span data-ttu-id="64590-210">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="64590-210">In hello User Name **textbox**, type **BrittaSimon**.</span></span> 
 3. <span data-ttu-id="64590-211">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="64590-211">Click **Next**.</span></span>

6. <span data-ttu-id="64590-212">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64590-212">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. <span data-ttu-id="64590-214">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="64590-214">In hello **First Name** textbox, type **Britta**.</span></span>    
 2. <span data-ttu-id="64590-215">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="64590-215">In hello **Last Name** textbox, type, **Simon**.</span></span> 
 3. <span data-ttu-id="64590-216">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="64590-216">In hello **Display Name** textbox, type **Britta Simon**.</span></span> 
 4. <span data-ttu-id="64590-217">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="64590-217">In hello **Role** list, select **User**.</span></span>
 5. <span data-ttu-id="64590-218">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="64590-218">Click **Next**.</span></span>

7. <span data-ttu-id="64590-219">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="64590-219">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="64590-221">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="64590-221">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. <span data-ttu-id="64590-223">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="64590-223">Write down hello value of hello **New Password**.</span></span> 
 2. <span data-ttu-id="64590-224">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="64590-224">Click **Complete**.</span></span>   

### <a name="create-a-silkroad-life-suite-test-user"></a><span data-ttu-id="64590-225">Vytvoření zkušebního uživatele SilkRoad životnosti Suite</span><span class="sxs-lookup"><span data-stu-id="64590-225">Create a SilkRoad Life Suite test user</span></span>
<span data-ttu-id="64590-226">Hello cílem této části je toocreate uživatele názvem Britta Simon v sadě SilkRoad životnosti.</span><span class="sxs-lookup"><span data-stu-id="64590-226">hello objective of this section is toocreate a user called Britta Simon in SilkRoad Life Suite.</span></span> <span data-ttu-id="64590-227">Na Britta musí mít ID jednotné přihlašování (někdy se také označuje tooas *AuthParam*) odpovídající na Britta **emailaddress** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64590-227">Britta's must have an SSO ID (sometimes referred tooas an *AuthParam*) that matches Britta's **emailaddress** in Azure AD.</span></span>

<span data-ttu-id="64590-228">**toocreate uživatele volat Britta Simon v sadě životnosti SilkRoad, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="64590-228">**toocreate a user called Britta Simon in SilkRoad Life Suite, perform hello following steps:**</span></span>

- <span data-ttu-id="64590-229">Požádejte uživatele, který má jako vaše Suite SilkRoad životnosti podpory team toocreate **jednotného přihlašování k ID** atribut hello stejnou hodnotu jako hello **emailaddress** z Britta Simon ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64590-229">Ask your SilkRoad Life Suite support team toocreate a user that has as **SSO ID** attribute hello same value as hello **emailaddress** of Britta Simon in Azure AD.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="64590-230">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="64590-230">Assign hello Azure AD test user</span></span>
<span data-ttu-id="64590-231">Hello cílem této části je tooenable toouse Britta Simon jednotného přihlašování k Azure tak, že udělíte svůj přístup tooSilkRoad Suite životnosti.</span><span class="sxs-lookup"><span data-stu-id="64590-231">hello objective of this section is tooenable Britta Simon toouse Azure SSO by granting her access tooSilkRoad Life Suite.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="64590-233">**tooassign tooSilkRoad Britta Simon životnosti Suite, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="64590-233">**tooassign Britta Simon tooSilkRoad Life Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="64590-234">Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="64590-234">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="64590-236">V seznamu aplikace hello vyberte **SilkRoad životnosti Suite**.</span><span class="sxs-lookup"><span data-stu-id="64590-236">In hello applications list, select **SilkRoad Life Suite**.</span></span>
   
    ![Přiřadit uživatele][202] 

3. <span data-ttu-id="64590-238">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="64590-238">In hello menu on hello top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203] 

4. <span data-ttu-id="64590-240">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="64590-240">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="64590-241">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="64590-241">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="64590-243">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="64590-243">Test single sign-on</span></span>
<span data-ttu-id="64590-244">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="64590-244">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="64590-245">Když kliknete na dlaždici SilkRoad životnosti Suite hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SilkRoad životnosti sada aplikací.</span><span class="sxs-lookup"><span data-stu-id="64590-245">When you click hello SilkRoad Life Suite tile in hello Access Panel, you should get automatically signed-on tooyour SilkRoad Life Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64590-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="64590-246">Additional Resources</span></span>
* [<span data-ttu-id="64590-247">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="64590-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="64590-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="64590-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





