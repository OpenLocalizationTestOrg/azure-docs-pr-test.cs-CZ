---
title: 'Kurz: Azure Active Directory integrace s SuccessFactors | Microsoft Docs'
description: "Zjistěte, jak toouse SuccessFactors s Azure Active Directory tooenable jednotné přihlašování, automatického zřizování a další!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 3f7895d7d5e26fda27f555ae2f14a1645b50dcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a><span data-ttu-id="0d4ad-103">Kurz: Azure Active Directory integrace s SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="0d4ad-103">Tutorial: Azure Active Directory integration with SuccessFactors</span></span>
<span data-ttu-id="0d4ad-104">cílem Hello tohoto kurzu je tooshow můžete jak toointegrate SuccessFactors s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0d4ad-104">hello objective of this tutorial is tooshow you how toointegrate SuccessFactors with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0d4ad-105">Integrace SuccessFactors s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-105">Integrating SuccessFactors with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="0d4ad-106">Můžete řídit ve službě Azure AD, který má přístup tooSuccessFactors</span><span class="sxs-lookup"><span data-stu-id="0d4ad-106">You can control in Azure AD who has access tooSuccessFactors</span></span>
* <span data-ttu-id="0d4ad-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSuccessFactors (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d4ad-107">You can enable your users tooautomatically get signed-on tooSuccessFactors (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="0d4ad-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="0d4ad-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="0d4ad-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0d4ad-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d4ad-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0d4ad-110">Prerequisites</span></span>
<span data-ttu-id="0d4ad-111">Integrace služby Azure AD s SuccessFactors tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-111">tooconfigure Azure AD integration with SuccessFactors, you need hello following items:</span></span>

* <span data-ttu-id="0d4ad-112">Platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="0d4ad-112">A valid Azure subscription</span></span>
* <span data-ttu-id="0d4ad-113">Klienta v SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="0d4ad-113">A tenant in SuccessFactors</span></span>

> [!NOTE]
> <span data-ttu-id="0d4ad-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="0d4ad-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="0d4ad-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="0d4ad-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d4ad-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0d4ad-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0d4ad-118">Scenario description</span></span>
<span data-ttu-id="0d4ad-119">cílem Hello tohoto kurzu je tooenable tootest Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="0d4ad-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0d4ad-121">Přidání SuccessFactors z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0d4ad-121">Adding SuccessFactors from hello gallery</span></span>
2. <span data-ttu-id="0d4ad-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d4ad-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-successfactors-from-hello-gallery"></a><span data-ttu-id="0d4ad-123">Přidání SuccessFactors z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0d4ad-123">Adding SuccessFactors from hello gallery</span></span>
<span data-ttu-id="0d4ad-124">tooconfigure hello integrace SuccessFactors do Azure AD, je nutné tooadd SuccessFactors hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-124">tooconfigure hello integration of SuccessFactors into Azure AD, you need tooadd SuccessFactors from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0d4ad-125">**tooadd SuccessFactors z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0d4ad-125">**tooadd SuccessFactors from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d4ad-126">V hello portál Azure classic, na levém navigačním panelu hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-126">In hello Azure classic portal, on hello left navigation panel, click **Active Directory**.</span></span>
   
    ![Konfigurace jednotného přihlašování][1]
2. <span data-ttu-id="0d4ad-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="0d4ad-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Konfigurace jednotného přihlašování][2]
4. <span data-ttu-id="0d4ad-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Aplikace][3]
5. <span data-ttu-id="0d4ad-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Konfigurace jednotného přihlašování][4]
6. <span data-ttu-id="0d4ad-135">V hello **vyhledávacího pole**, typ **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-135">In hello **search box**, type **SuccessFactors**.</span></span>
   
    ![Konfigurace jednotného přihlašování][5]
7. <span data-ttu-id="0d4ad-137">Na panelu výsledků hello vyberte **SuccessFactors**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-137">In hello results panel, select **SuccessFactors**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Konfigurace jednotného přihlašování][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0d4ad-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d4ad-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0d4ad-140">Hello cílem této části je tooshow vám jak tooconfigure a testování Azure AD jednotné přihlašování s SuccessFactors na základě testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="0d4ad-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with SuccessFactors based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0d4ad-141">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel hello protějškem v SuccessFactors tooan uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SuccessFactors tooan user in Azure AD is.</span></span> <span data-ttu-id="0d4ad-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SuccessFactors musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-142">In other words, a link relationship between an Azure AD user and hello related user in SuccessFactors needs toobe established.</span></span>

<span data-ttu-id="0d4ad-143">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SuccessFactors.</span></span>

<span data-ttu-id="0d4ad-144">tooconfigure a testu Azure AD jednotné přihlašování s SuccessFactors, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-144">tooconfigure and test Azure AD single sign-on with SuccessFactors, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0d4ad-145">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0d4ad-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0d4ad-147">**[Vytvoření zkušebního uživatele SuccessFactors](#creating-a-successfactors-test-user)**  -toohave protějšek Britta Simon v SuccessFactors, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-147">**[Creating a SuccessFactors test user](#creating-a-successfactors-test-user)** - toohave a counterpart of Britta Simon in SuccessFactors that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="0d4ad-148">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0d4ad-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0d4ad-150">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d4ad-150">Configuring Azure AD single sign-on</span></span>
<span data-ttu-id="0d4ad-151">V této části můžete povolit Azure AD jednotné přihlašování v portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your SuccessFactors application.</span></span>

<span data-ttu-id="0d4ad-152">**tooconfigure Azure AD jednotné přihlašování s SuccessFactors, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0d4ad-152">**tooconfigure Azure AD single sign-on with SuccessFactors, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d4ad-153">V portálu Azure classic, na hello hello **SuccessFactors** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-153">In hello Azure classic portal, on hello **SuccessFactors** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    ![Konfigurace jednotného přihlašování][7]
2. <span data-ttu-id="0d4ad-155">Na hello **jak jste by například uživatelé toosign na tooSuccessFactors** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-155">On hello **How would you like users toosign on tooSuccessFactors** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Konfigurace jednotného přihlašování][8]
3. <span data-ttu-id="0d4ad-157">Na hello **konfigurace adresy URL aplikace** stránky, proveďte hello následující kroky a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-157">On hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
    ![Konfigurace jednotného přihlašování][9]
   
    <span data-ttu-id="0d4ad-159">a.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-159">a.</span></span> <span data-ttu-id="0d4ad-160">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí jedné z následujících vzory hello:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-160">In hello **Sign On URL** textbox, type a URL using one of hello following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    <span data-ttu-id="0d4ad-161">b.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-161">b.</span></span> <span data-ttu-id="0d4ad-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí jedné z následujících vzory hello:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-162">In hello **Reply URL** textbox, type a URL using one of hello following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    <span data-ttu-id="0d4ad-163">c.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-163">c.</span></span> <span data-ttu-id="0d4ad-164">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-164">Click **Next**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0d4ad-165">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-165">Please note that these are not hello real values.</span></span> <span data-ttu-id="0d4ad-166">Máte tooupdate tyto hodnoty pomocí hello skutečné přihlašovací adresa URL a odpovědi adresy URL.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-166">You have tooupdate these values with hello actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="0d4ad-167">Obraťte se na tooget tyto hodnoty [tým podpory SuccessFactors](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="0d4ad-167">tooget these values, contact [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

1. <span data-ttu-id="0d4ad-168">Na hello **nakonfigurovat jednotné přihlašování v SuccessFactors** klikněte na tlačítko **stažení certifikátu**a potom uložte soubor certifikátu hello místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-168">On hello **Configure single sign-on at SuccessFactors** page, click **Download certificate**, and then save hello certificate file locally on your computer.</span></span>
   
    ![Konfigurace jednotného přihlašování][10]

2. <span data-ttu-id="0d4ad-170">V okně prohlížeče jiný web, přihlaste se k vaší **portál pro správu SuccessFactors** jako správce.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-170">In a different web browser window, log into your **SuccessFactors admin portal** as an administrator.</span></span>

3. <span data-ttu-id="0d4ad-171">Navštivte **zabezpečení aplikací** a nativní příliš**jeden znak na funkci**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-171">Visit **Application Security** and native too**Single Sign On Feature**.</span></span> 

4. <span data-ttu-id="0d4ad-172">Umístění všechny hodnoty v hello **resetovat tokenu** a klikněte na tlačítko **uložit tokenu** tooenable jednotné přihlašování SAML.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-172">Place any value in hello **Reset Token** and click **Save Token** tooenable SAML SSO.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace][11]

    > [!NOTE] 
    > <span data-ttu-id="0d4ad-174">Tato hodnota se právě používá jako hello zapnout nebo vypnout přepínače.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-174">This value is just used as hello on/off switch.</span></span> <span data-ttu-id="0d4ad-175">Pokud je uloženo žádnou hodnotu, je hello jednotné přihlašování SAML ON.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-175">If any value is saved, hello SAML SSO is ON.</span></span> <span data-ttu-id="0d4ad-176">Pokud je uloženo na prázdnou hodnotu hello jednotné přihlašování SAML je VYPNUTÝ.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-176">If a blank value is saved hello SAML SSO is OFF.</span></span>

1. <span data-ttu-id="0d4ad-177">Snímek obrazovky nativní toobelow a proveďte následující akce hello.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-177">Native toobelow screenshot and perform hello following actions.</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace][12]
   
    <span data-ttu-id="0d4ad-179">a.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-179">a.</span></span> <span data-ttu-id="0d4ad-180">Vyberte hello **jednotné přihlašování SAML v2** přepínač</span><span class="sxs-lookup"><span data-stu-id="0d4ad-180">Select hello **SAML v2 SSO** Radio Button</span></span>
   
    <span data-ttu-id="0d4ad-181">b.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-181">b.</span></span> <span data-ttu-id="0d4ad-182">Nastavte hello Name(e.g. SAml issuer + company name) strany potvrzující SAML.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-182">Set hello SAML Asserting Party Name(e.g. SAml issuer + company name).</span></span>
   
    <span data-ttu-id="0d4ad-183">c.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-183">c.</span></span> <span data-ttu-id="0d4ad-184">V hello **SAML vystavitele** textbox put hello hodnotu **URL vystavitele** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-184">In hello **SAML Issuer** textbox put hello value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="0d4ad-185">d.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-185">d.</span></span> <span data-ttu-id="0d4ad-186">Vyberte **odpovědi (zákazníka generované nebo IdP/přístupový bod)** jako **vyžadují povinné podpis**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-186">Select **Response(Customer Generated/IdP/AP)** as **Require Mandatory Signature**.</span></span>
   
    <span data-ttu-id="0d4ad-187">e.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-187">e.</span></span> <span data-ttu-id="0d4ad-188">Vyberte **povoleno** jako **povolit SAML příznak**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-188">Select **Enabled** as **Enable SAML Flag**.</span></span>
   
    <span data-ttu-id="0d4ad-189">f.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-189">f.</span></span> <span data-ttu-id="0d4ad-190">Vyberte **ne** jako **podpisu požadavku přihlášení (SF generované/SP/RP)**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-190">Select **No** as **Login Request Signature(SF Generated/SP/RP)**.</span></span>
   
    <span data-ttu-id="0d4ad-191">g.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-191">g.</span></span> <span data-ttu-id="0d4ad-192">Vyberte **profil prohlížeče nebo Pozálohovacího** jako **SAML profil**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-192">Select **Browser/Post Profile** as **SAML Profile**.</span></span>
   
    <span data-ttu-id="0d4ad-193">h.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-193">h.</span></span> <span data-ttu-id="0d4ad-194">Vyberte **ne** jako **vynutit období platný certifikát**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-194">Select **No** as **Enforce Certificate Valid Period**.</span></span>
   
    <span data-ttu-id="0d4ad-195">i.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-195">i.</span></span> <span data-ttu-id="0d4ad-196">Zkopírujte obsah hello hello stažený certifikát souboru a pak ji vložit do hello **ověření certifikátu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-196">Copy hello content of hello downloaded certificate file, and then paste it into hello **SAML Verifying Certificate** textbox.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0d4ad-197">obsahu certifikátu Hello musí začínat certifikátu a koncovou značkou certifikátu.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-197">hello certificate content must have begin certificate and end certificate tags.</span></span>

1. <span data-ttu-id="0d4ad-198">Přejděte tooSAML V2 a potom proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-198">Navigate tooSAML V2, and then perform hello following steps:</span></span>
   
    ![Konfigurace jednotného přihlašování na straně aplikace][13]
   
    <span data-ttu-id="0d4ad-200">a.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-200">a.</span></span> <span data-ttu-id="0d4ad-201">Vyberte **Ano** jako **podporu spouštěná SP globální odhlášení**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-201">Select **Yes** as **Support SP-initiated Global Logout**.</span></span>
   
    <span data-ttu-id="0d4ad-202">b.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-202">b.</span></span> <span data-ttu-id="0d4ad-203">V hello **globální odhlášení adresa URL služby (LogoutRequest cíl)** textbox put hello hodnotu **vzdálené adresy URL odhlašovací** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-203">In hello **Global Logout Service URL (LogoutRequest destination)** textbox put hello value of **Remote Logout URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="0d4ad-204">c.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-204">c.</span></span> <span data-ttu-id="0d4ad-205">Vyberte **ne** jako **vyžadují sp musí zašifrovat všechny element NameID**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-205">Select **No** as **Require sp must encrypt all NameID element**.</span></span>
   
    <span data-ttu-id="0d4ad-206">d.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-206">d.</span></span> <span data-ttu-id="0d4ad-207">Vyberte **neurčené** jako **NameID formátu**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-207">Select **unspecified** as **NameID Format**.</span></span>
   
    <span data-ttu-id="0d4ad-208">e.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-208">e.</span></span> <span data-ttu-id="0d4ad-209">Vyberte **Ano** jako **povolit sp iniciované přihlášení (AuthnRequest)**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-209">Select **Yes** as **Enable sp initiated login (AuthnRequest)**.</span></span>
   
    <span data-ttu-id="0d4ad-210">f.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-210">f.</span></span> <span data-ttu-id="0d4ad-211">V hello **odeslán požadavek na jako vydavatel společnosti** textbox put hello hodnotu **vzdálené adresy URL pro přihlášení** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-211">In hello **Send request as Company-Wide issuer** textbox put hello value of **Remote Login URL** from Azure AD application configuration wizard.</span></span>
2. <span data-ttu-id="0d4ad-212">Proveďte tyto kroky, pokud chcete, aby uživatelská jména přihlášení hello toomake malá a velká písmena,.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-212">Perform these steps if you want toomake hello login usernames Case Insensitive, .</span></span>
   
    <span data-ttu-id="0d4ad-213">a.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-213">a.</span></span> <span data-ttu-id="0d4ad-214">Navštivte **nastavení společnosti**(téměř hello dole).</span><span class="sxs-lookup"><span data-stu-id="0d4ad-214">Visit **Company Settings**(near hello bottom).</span></span>
   
    <span data-ttu-id="0d4ad-215">b.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-215">b.</span></span> <span data-ttu-id="0d4ad-216">Zaškrtněte políčko téměř **povolit uživatelské jméno bez rozlišování**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-216">select checkbox near **Enable Non-Case-Sensitive Username**.</span></span>
   
    <span data-ttu-id="0d4ad-217">c.Click **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-217">c.Click **Save**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][29]

    > [!NOTE] 
    > <span data-ttu-id="0d4ad-219">Pokud tooenable zkusíte to, zkontroluje hello systému, pokud vytvoří duplicitní SAML přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-219">If you try tooenable this, hello system checks if it will create a duplicate SAML login name.</span></span> <span data-ttu-id="0d4ad-220">Například pokud hello zákazník má uživatelských jmen uživatel1 a user1.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-220">For example if hello customer has usernames User1 and user1.</span></span> <span data-ttu-id="0d4ad-221">Pořízení rychle rozlišování umožňuje tyto duplikáty.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-221">Taking away case sensitivity makes these duplicates.</span></span> <span data-ttu-id="0d4ad-222">systém Hello získáte chybovou zprávu a neumožní hello funkce.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-222">hello system will give you an error message and will not enable hello feature.</span></span> <span data-ttu-id="0d4ad-223">Hello zákazníka bude nutné toochange mezi hello uživatelských jmen, je ve skutečnosti napsán jiný.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-223">hello customer will need toochange one of hello usernames so it’s actually spelled different.</span></span> 

1. <span data-ttu-id="0d4ad-224">Na hello portál Azure classic, vyberte hello konfigurace přihlášení potvrzení a pak klikněte na tlačítko **Complete** tooclose hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-224">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    ![Aplikace][14]
2. <span data-ttu-id="0d4ad-226">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-226">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    ![Aplikace][15]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0d4ad-228">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d4ad-228">Creating an Azure AD test user</span></span>
<span data-ttu-id="0d4ad-229">Hello cílem této části je toocreate testovacího uživatele na portálu classic hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-229">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][16]

<span data-ttu-id="0d4ad-231">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0d4ad-231">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d4ad-232">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-232">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][17]
2. <span data-ttu-id="0d4ad-234">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-234">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="0d4ad-235">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-235">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][18]
4. <span data-ttu-id="0d4ad-237">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-237">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][19]
5. <span data-ttu-id="0d4ad-239">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-239">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][20]
   
    <span data-ttu-id="0d4ad-241">a.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-241">a.</span></span> <span data-ttu-id="0d4ad-242">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-242">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="0d4ad-243">b.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-243">b.</span></span> <span data-ttu-id="0d4ad-244">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-244">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="0d4ad-245">c.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-245">c.</span></span> <span data-ttu-id="0d4ad-246">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-246">Click **Next**.</span></span>
6. <span data-ttu-id="0d4ad-247">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-247">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][21]
   
    <span data-ttu-id="0d4ad-249">a.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-249">a.</span></span> <span data-ttu-id="0d4ad-250">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-250">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="0d4ad-251">b.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-251">b.</span></span> <span data-ttu-id="0d4ad-252">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-252">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="0d4ad-253">c.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-253">c.</span></span> <span data-ttu-id="0d4ad-254">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-254">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="0d4ad-255">d.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-255">d.</span></span> <span data-ttu-id="0d4ad-256">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-256">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="0d4ad-257">e.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-257">e.</span></span> <span data-ttu-id="0d4ad-258">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-258">Click **Next**.</span></span>
7. <span data-ttu-id="0d4ad-259">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-259">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][22]
8. <span data-ttu-id="0d4ad-261">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0d4ad-261">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD][23]
   
    <span data-ttu-id="0d4ad-263">a.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-263">a.</span></span> <span data-ttu-id="0d4ad-264">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-264">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="0d4ad-265">b.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-265">b.</span></span> <span data-ttu-id="0d4ad-266">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-266">Click **Complete**.</span></span>  

### <a name="creating-a-successfactors-test-user"></a><span data-ttu-id="0d4ad-267">Vytvoření zkušebního uživatele SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="0d4ad-267">Creating a SuccessFactors test user</span></span>
<span data-ttu-id="0d4ad-268">V pořadí tooenable Azure AD Uživatelé toolog do SuccessFactors musí být zřízená do SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-268">In order tooenable Azure AD users toolog into SuccessFactors, they must be provisioned into SuccessFactors.</span></span>  
<span data-ttu-id="0d4ad-269">V případě hello SuccessFactors zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-269">In hello case of SuccessFactors, provisioning is a manual task.</span></span>

<span data-ttu-id="0d4ad-270">tooget uživatelé vytvoření ve SuccessFactors, je třeba toocontact hello [tým podpory SuccessFactors](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="0d4ad-270">tooget users created in SuccessFactors, you need toocontact hello [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0d4ad-271">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="0d4ad-271">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="0d4ad-272">Hello cílem této části je tooenabling toouse Britta Simon Azure jednotné přihlašování, poskytněte tooSuccessFactors svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-272">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSuccessFactors.</span></span>

![Přiřadit uživatele][24]

<span data-ttu-id="0d4ad-274">**tooassign Britta Simon tooSuccessFactors, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0d4ad-274">**tooassign Britta Simon tooSuccessFactors, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d4ad-275">Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-275">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Přiřadit uživatele][25]
2. <span data-ttu-id="0d4ad-277">V seznamu aplikace hello vyberte **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-277">In hello applications list, select **SuccessFactors**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][26]
3. <span data-ttu-id="0d4ad-279">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-279">In hello menu on hello top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][27]
4. <span data-ttu-id="0d4ad-281">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-281">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="0d4ad-282">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-282">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][28]

### <a name="testing-single-sign-on"></a><span data-ttu-id="0d4ad-284">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0d4ad-284">Testing single sign-on</span></span>
<span data-ttu-id="0d4ad-285">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-285">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0d4ad-286">Po kliknutí na tlačítko hello SuccessFactors dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SuccessFactors aplikace.</span><span class="sxs-lookup"><span data-stu-id="0d4ad-286">When you click hello SuccessFactors tile in hello Access Panel, you should get automatically signed-on tooyour SuccessFactors application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d4ad-287">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0d4ad-287">Additional resources</span></span>
* [<span data-ttu-id="0d4ad-288">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d4ad-288">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0d4ad-289">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0d4ad-289">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
