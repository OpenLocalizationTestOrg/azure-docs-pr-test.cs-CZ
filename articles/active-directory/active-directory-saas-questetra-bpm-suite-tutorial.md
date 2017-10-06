---
title: 'Kurz: Azure Active Directory integrace se sadou Questetra BPM Suite | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Questetra BPM Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: jeedes
ms.openlocfilehash: 4907e3b5751cd79f994fbd2ebcb7faec4eac34e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a><span data-ttu-id="c1e7b-103">Kurz: Azure Active Directory integrace se sadou Questetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="c1e7b-103">Tutorial: Azure Active Directory integration with Questetra BPM Suite</span></span>
<span data-ttu-id="c1e7b-104">cílem Hello tohoto kurzu je tooshow můžete jak toointegrate Questetra BPM Suite a Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c1e7b-104">hello objective of this tutorial is tooshow you how toointegrate Questetra BPM Suite with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="c1e7b-105">Integrace sady BPM Questetra s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-105">Integrating Questetra BPM Suite with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="c1e7b-106">Můžete řídit ve službě Azure AD, který má přístup tooQuestetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="c1e7b-106">You can control in Azure AD who has access tooQuestetra BPM Suite</span></span> 
* <span data-ttu-id="c1e7b-107">Vaši uživatelé tooautomatically get přihlášeného tooQuestetra Suite BPM (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1e7b-107">You can enable your users tooautomatically get signed-on tooQuestetra BPM Suite (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="c1e7b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="c1e7b-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="c1e7b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c1e7b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1e7b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1e7b-110">Prerequisites</span></span>
<span data-ttu-id="c1e7b-111">tooconfigure integrace Azure AD s Questetra BPM Suite, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-111">tooconfigure Azure AD integration with Questetra BPM Suite, you need hello following items:</span></span>

* <span data-ttu-id="c1e7b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1e7b-112">An Azure AD subscription</span></span>
* <span data-ttu-id="c1e7b-113">[Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c1e7b-113">An [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c1e7b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="c1e7b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="c1e7b-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="c1e7b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1e7b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="c1e7b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c1e7b-118">Scenario Description</span></span>
<span data-ttu-id="c1e7b-119">cílem Hello tohoto kurzu je tooenable tootest Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="c1e7b-120">scénář Hello uvedených v tomto kurzu se skládá ze tří hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-120">hello scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="c1e7b-121">Přidání Questetra BPM Suite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c1e7b-121">Adding Questetra BPM Suite from hello gallery</span></span> 
2. <span data-ttu-id="c1e7b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1e7b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-questetra-bpm-suite-from-hello-gallery"></a><span data-ttu-id="c1e7b-123">Přidání Questetra BPM Suite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c1e7b-123">Adding Questetra BPM Suite from hello gallery</span></span>
<span data-ttu-id="c1e7b-124">tooconfigure hello integraci sady BPM Questetra do služby Azure AD, je nutné tooadd Questetra BPM Suite hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-124">tooconfigure hello integration of Questetra BPM Suite into Azure AD, you need tooadd Questetra BPM Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c1e7b-125">**tooadd Suite BPM Questetra z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1e7b-125">**tooadd Questetra BPM Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e7b-126">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="c1e7b-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="c1e7b-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Aplikace][2]

4. <span data-ttu-id="c1e7b-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Aplikace][3]

5. <span data-ttu-id="c1e7b-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Aplikace][4]

6. <span data-ttu-id="c1e7b-135">Hello vyhledávacího pole zadejte **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-135">In hello search box, type **Questetra BPM Suite**.</span></span>
   
    ![Aplikace][5]

7. <span data-ttu-id="c1e7b-137">V podokně výsledků hello, vyberte **Questetra BPM Suite**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-137">In hello results pane, select **Questetra BPM Suite**, and then click **Complete** tooadd hello application.</span></span>

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c1e7b-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1e7b-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c1e7b-139">Hello cílem této části je tooshow vám jak tooconfigure a testování Azure AD jednotné přihlašování se sadou Questetra BPM Suite na základě testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c1e7b-139">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with Questetra BPM Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c1e7b-140">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v Questetra BPM Suite tooan uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Questetra BPM Suite tooan user in Azure AD is.</span></span> <span data-ttu-id="c1e7b-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v sadě BPM Questetra musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-141">In other words, a link relationship between an Azure AD user and hello related user in Questetra BPM Suite needs toobe established.</span></span>  
<span data-ttu-id="c1e7b-142">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v sadě BPM Questetra.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Questetra BPM Suite.</span></span>

<span data-ttu-id="c1e7b-143">tooconfigure a testu Azure AD jednotné přihlašování s Questetra BPM Suite, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-143">tooconfigure and test Azure AD single sign-on with Questetra BPM Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c1e7b-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c1e7b-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c1e7b-146">**[Vytvoření zkušebního uživatele Questetra BPM Suite](#creating-a-questetra-bpm-suite-test-user)**  -toohave protějšek Britta Simon v sadě BPM Questetra, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-146">**[Creating a Questetra BPM Suite test user](#creating-a-questetra-bpm-suite-test-user)** - toohave a counterpart of Britta Simon in Questetra BPM Suite that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="c1e7b-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c1e7b-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c1e7b-149">Konfigurace Azure AD jednotné přihlášení</span><span class="sxs-lookup"><span data-stu-id="c1e7b-149">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="c1e7b-150">Hello cílem této části je tooenable Azure AD jednotné přihlašování v hello portál Azure classic a tooconfigure jednotné přihlašování v aplikaci Questetra BPM Suite.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-150">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your Questetra BPM Suite application.</span></span>

<span data-ttu-id="c1e7b-151">**tooconfigure Azure AD jednotné přihlašování s Questetra BPM Suite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1e7b-151">**tooconfigure Azure AD single sign-on with Questetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e7b-152">V portálu Azure classic, na hello hello **Questetra BPM Suite** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování**  Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-152">In hello Azure classic portal, on hello **Questetra BPM Suite** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][8]

2. <span data-ttu-id="c1e7b-154">Na hello **jak jste by například uživatelé toosign na tooQuestetra BPM Suite** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-154">On hello **How would you like users toosign on tooQuestetra BPM Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][9]

3. <span data-ttu-id="c1e7b-156">V okně prohlížeče jiný web, přihlaste se k vaší **Questetra BPM Suite** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-156">In a different web browser window, log into your **Questetra BPM Suite** company site as an administrator.</span></span>

4. <span data-ttu-id="c1e7b-157">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-157">In hello menu on hello top, click **System Settings**.</span></span> 
   
    ![Azure AD jednotné přihlášení][10]

5. <span data-ttu-id="c1e7b-159">tooopen hello **SingleSignOnSAML** klikněte na tlačítko **jednotné přihlašování (SAML)**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-159">tooopen hello **SingleSignOnSAML** page, click **SSO (SAML)**.</span></span> 
   
    ![Azure AD jednotné přihlášení][11]

6. <span data-ttu-id="c1e7b-161">V portálu Azure classic, na hello hello **nakonfigurovat nastavení aplikace** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-161">In hello Azure classic portal, on hello **Configure App Settings** dialog page, perform hello following steps:</span></span> 
   
    ![Konfigurovat nastavení aplikace][13]
   
    <span data-ttu-id="c1e7b-163">a.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-163">a.</span></span> <span data-ttu-id="c1e7b-164">Na jste **Questetra BPM Suite** společnosti lokality v hello SP informace oddílu, kopie hello **adresa URL služby ACS**a pak ji vložit do hello **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-164">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **ACS URL**, and then paste it into hello **Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="c1e7b-165">b.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-165">b.</span></span> <span data-ttu-id="c1e7b-166">Na jste **Questetra BPM Suite** společnosti lokality v hello SP informace oddílu, kopie hello **Entity ID**a pak ji vložit do hello **URL vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-166">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **Entity ID**, and then paste it into hello **Issuer URL** textbox.</span></span>
   
    <span data-ttu-id="c1e7b-167">c.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-167">c.</span></span> <span data-ttu-id="c1e7b-168">Na jste **Questetra BPM Suite** společnosti lokality v hello SP informace oddílu, kopie hello **adresa URL služby ACS**a pak ji vložit do hello **adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-168">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **ACS URL**, and then paste it into hello **Reply URL** textbox.</span></span>
   
    <span data-ttu-id="c1e7b-169">d.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-169">d.</span></span> <span data-ttu-id="c1e7b-170">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-170">Click **Next**.</span></span>

7. <span data-ttu-id="c1e7b-171">Na hello **nakonfigurovat jednotné přihlašování v Questetra BPM Suite** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-171">On hello **Configure single sign-on at Questetra BPM Suite** page, click **Download certificate**, and then save hello certificate file locally on your computer.</span></span>
   
    ![Konfigurovat jednotné přihlašování][14]

8. <span data-ttu-id="c1e7b-173">Na jste **Questetra BPM Suite** společnosti lokality, provedení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-173">On you **Questetra BPM Suite** company site, perform hello following steps:</span></span> 
   
    ![Konfigurovat jednotné přihlašování][15]
   
    <span data-ttu-id="c1e7b-175">a.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-175">a.</span></span> <span data-ttu-id="c1e7b-176">Vyberte **povolit jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-176">Select **Enable Single Sign-On**.</span></span>
   
    <span data-ttu-id="c1e7b-177">b.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-177">b.</span></span> <span data-ttu-id="c1e7b-178">Na portálu Azure classic hello, zkopírujte hello **URL vystavitele** hodnotu a pak ji vložit do hello **Entity ID** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-178">On hello Azure classic portal, copy hello **Issuer URL** value, and then paste it into hello **Entity ID** textbox.</span></span>
   
    <span data-ttu-id="c1e7b-179">c.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-179">c.</span></span> <span data-ttu-id="c1e7b-180">Na portálu Azure classic hello, zkopírujte hello **jeden přihlašování adresa URL služby** hodnotu a pak ji vložit do hello **přihlašovací adresa URL stránky** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-180">On hello Azure classic portal, copy hello **Single Sign-On Service URL** value, and then paste it into hello **Sign-in page URL** textbox.</span></span>
   
    <span data-ttu-id="c1e7b-181">d.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-181">d.</span></span> <span data-ttu-id="c1e7b-182">Na portálu Azure classic hello, zkopírujte hello **jednu adresu URL služby Sign-Out** hodnotu a pak ji vložit do hello **adresy URL odhlašovací stránky** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-182">On hello Azure classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Sign-out page URL** textbox.</span></span>
   
    <span data-ttu-id="c1e7b-183">e.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-183">e.</span></span> <span data-ttu-id="c1e7b-184">V hello **NameID formátu** textovému poli, typ **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-184">In hello **NameID format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="c1e7b-185">f.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-185">f.</span></span> <span data-ttu-id="c1e7b-186">Vytvořte soubor kódováním base-64 z stažený certifikát.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-186">Create a base-64 encoded file from your downloaded certificate.</span></span> 

    >[!TIP] 
    ><span data-ttu-id="c1e7b-187">Další podrobnosti najdete v tématu [jak tooconvert binární certifikátů do textového souboru](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="c1e7b-187">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

    <span data-ttu-id="c1e7b-188">g.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-188">g.</span></span> <span data-ttu-id="c1e7b-189">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit do hello **ověření certifikátu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-189">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it into hello **Validation certificate** textbox.</span></span> 

    <span data-ttu-id="c1e7b-190">h.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-190">h.</span></span> <span data-ttu-id="c1e7b-191">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-191">Click **Save**.</span></span>

1. <span data-ttu-id="c1e7b-192">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-192">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Co je služba Azure AD Connect][17]

2. <span data-ttu-id="c1e7b-194">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-194">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Co je služba Azure AD Connect][18]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c1e7b-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1e7b-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="c1e7b-197">Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-197">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="c1e7b-198">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1e7b-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e7b-199">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-199">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytvoření zkušebního uživatele Azure AD][100] 

2. <span data-ttu-id="c1e7b-201">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-201">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="c1e7b-202">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-202">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Vytvoření zkušebního uživatele Azure AD][101] 

4. <span data-ttu-id="c1e7b-204">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-204">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Vytvoření zkušebního uživatele Azure AD][102] 

5. <span data-ttu-id="c1e7b-206">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-206">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Vytvoření zkušebního uživatele Azure AD][103]
   
    <span data-ttu-id="c1e7b-208">a.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-208">a.</span></span> <span data-ttu-id="c1e7b-209">Jako **typ uživatele**, vyberte **nového uživatele ve vaší organizaci**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-209">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="c1e7b-210">b.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-210">b.</span></span> <span data-ttu-id="c1e7b-211">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-211">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="c1e7b-212">c.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-212">c.</span></span> <span data-ttu-id="c1e7b-213">Klikněte na Další.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-213">Click Next.</span></span>

6. <span data-ttu-id="c1e7b-214">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-214">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Vytvoření zkušebního uživatele Azure AD][104] 
   
    <span data-ttu-id="c1e7b-216">a.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-216">a.</span></span> <span data-ttu-id="c1e7b-217">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-217">In hello **First Name** textbox, type **Britta**.</span></span> 
   
    <span data-ttu-id="c1e7b-218">b.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-218">b.</span></span> <span data-ttu-id="c1e7b-219">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-219">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="c1e7b-220">c.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-220">c.</span></span> <span data-ttu-id="c1e7b-221">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-221">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="c1e7b-222">d.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-222">d.</span></span> <span data-ttu-id="c1e7b-223">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-223">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="c1e7b-224">e.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-224">e.</span></span> <span data-ttu-id="c1e7b-225">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-225">Click **Next**.</span></span>

7. <span data-ttu-id="c1e7b-226">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-226">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytvoření zkušebního uživatele Azure AD][105]  

8. <span data-ttu-id="c1e7b-228">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-228">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Vytvoření zkušebního uživatele Azure AD][106]   
   
    <span data-ttu-id="c1e7b-230">a.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-230">a.</span></span> <span data-ttu-id="c1e7b-231">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-231">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="c1e7b-232">b.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-232">b.</span></span> <span data-ttu-id="c1e7b-233">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-233">Click **Complete**.</span></span>   

### <a name="creating-a-questetra-bpm-suite-test-user"></a><span data-ttu-id="c1e7b-234">Vytvoření zkušebního uživatele Questetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="c1e7b-234">Creating a Questetra BPM Suite test user</span></span>
<span data-ttu-id="c1e7b-235">Hello cílem této části je toocreate názvem Britta Simon v sadě BPM Questetra uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-235">hello objective of this section is toocreate a user called Britta Simon in Questetra BPM Suite.</span></span>

<span data-ttu-id="c1e7b-236">**toocreate uživatele volat Britta Simon v sadě BPM Questetra, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1e7b-236">**toocreate a user called Britta Simon in Questetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e7b-237">Web společnosti Questetra BPM Suite tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-237">Sign-on tooyour Questetra BPM Suite company site as an administrator.</span></span>
2. <span data-ttu-id="c1e7b-238">Přejděte příliš**nastavení systému > seznam uživatelů > Nový uživatel**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-238">Go too**System Settings > User List > New User**.</span></span> 
3. <span data-ttu-id="c1e7b-239">V dialogovém okně Nový uživatel hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c1e7b-239">On hello New User dialog, perform hello following steps:</span></span> 
   
    ![Vytvoření zkušebního uživatele][300] 
   
    <span data-ttu-id="c1e7b-241">a.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-241">a.</span></span> <span data-ttu-id="c1e7b-242">V hello **název** textovému poli, zadejte uživatelské jméno je Britta ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-242">In hello **Name** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="c1e7b-243">b.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-243">b.</span></span> <span data-ttu-id="c1e7b-244">V hello **e-mailu** textovému poli, zadejte uživatelské jméno je Britta ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-244">In hello **Email** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="c1e7b-245">c.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-245">c.</span></span> <span data-ttu-id="c1e7b-246">V hello **heslo** textovému poli, zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-246">In hello **Password** textbox, type a password.</span></span>

4. <span data-ttu-id="c1e7b-247">Klikněte na tlačítko **přidat nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-247">Click **Add new user**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c1e7b-248">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c1e7b-248">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="c1e7b-249">Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování, poskytněte svůj přístup tooQuestetra BPM Suite.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-249">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooQuestetra BPM Suite.</span></span>

![Co je služba Azure AD Connect][200]

<span data-ttu-id="c1e7b-251">**tooassign tooQuestetra Britta Simon BPM Suite, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c1e7b-251">**tooassign Britta Simon tooQuestetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e7b-252">Na hello Azure klikněte na portálu classic, zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-252">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Co je služba Azure AD Connect][201]
2. <span data-ttu-id="c1e7b-254">V seznamu aplikace hello vyberte **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-254">In hello applications list, select **Questetra BPM Suite**.</span></span>
   
    ![Co je služba Azure AD Connect][205]
3. <span data-ttu-id="c1e7b-256">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-256">In hello menu on hello top, click **Users**.</span></span>
   
    ![Co je služba Azure AD Connect][202]
4. <span data-ttu-id="c1e7b-258">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-258">In hello Users list, select **Britta Simon**.</span></span>
   
    ![Co je služba Azure AD Connect][203]
5. <span data-ttu-id="c1e7b-260">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-260">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Co je služba Azure AD Connect][204]

### <a name="testing-single-sign-on"></a><span data-ttu-id="c1e7b-262">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1e7b-262">Testing Single Sign-On</span></span>
<span data-ttu-id="c1e7b-263">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-263">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="c1e7b-264">Když kliknete na dlaždici Questetra BPM Suite hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Questetra BPM sada aplikací.</span><span class="sxs-lookup"><span data-stu-id="c1e7b-264">When you click hello Questetra BPM Suite tile in hello Access Panel, you should get automatically signed-on tooyour Questetra BPM Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1e7b-265">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c1e7b-265">Additional Resources</span></span>
* [<span data-ttu-id="c1e7b-266">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c1e7b-266">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c1e7b-267">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c1e7b-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 
