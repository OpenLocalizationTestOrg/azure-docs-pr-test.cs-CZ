---
title: "Kurz: Azure Active Directory integrace s Proofpoint na vyžádání | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Proofpoint na vyžádání."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 0f9472ddc01f2c18ffc9e8d2b59a17b3b595515e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a><span data-ttu-id="69892-103">Kurz: Azure Active Directory integrace s Proofpoint na vyžádání</span><span class="sxs-lookup"><span data-stu-id="69892-103">Tutorial: Azure Active Directory integration with Proofpoint on Demand</span></span>

<span data-ttu-id="69892-104">V tomto kurzu zjistíte, jak toointegrate Proofpoint na vyžádání pomocí Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="69892-104">In this tutorial, you learn how toointegrate Proofpoint on Demand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="69892-105">Integrace Proofpoint na vyžádání s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="69892-105">Integrating Proofpoint on Demand with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="69892-106">Můžete řídit ve službě Azure AD, který má přístup tooProofpoint na vyžádání</span><span class="sxs-lookup"><span data-stu-id="69892-106">You can control in Azure AD who has access tooProofpoint on Demand</span></span>
- <span data-ttu-id="69892-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooProofpoint na vyžádání (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="69892-107">You can enable your users tooautomatically get signed-on tooProofpoint on Demand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="69892-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="69892-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="69892-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="69892-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69892-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="69892-110">Prerequisites</span></span>

<span data-ttu-id="69892-111">integrace tooconfigure Azure AD s Proofpoint na vyžádání, je nutné hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="69892-111">tooconfigure Azure AD integration with Proofpoint on Demand, you need hello following items:</span></span>

- <span data-ttu-id="69892-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="69892-112">An Azure AD subscription</span></span>
- <span data-ttu-id="69892-113">Proofpoint na vyžádání jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="69892-113">A Proofpoint on Demand single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="69892-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="69892-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="69892-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="69892-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="69892-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="69892-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="69892-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="69892-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="69892-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="69892-118">Scenario description</span></span>
<span data-ttu-id="69892-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="69892-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="69892-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="69892-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="69892-121">Přidání Proofpoint na vyžádání z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="69892-121">Adding Proofpoint on Demand from hello gallery</span></span>
2. <span data-ttu-id="69892-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="69892-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-proofpoint-on-demand-from-hello-gallery"></a><span data-ttu-id="69892-123">Přidání Proofpoint na vyžádání z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="69892-123">Adding Proofpoint on Demand from hello gallery</span></span>
<span data-ttu-id="69892-124">tooconfigure hello integrace Proofpoint na vyžádání do Azure AD, je nutné tooadd Proofpoint na vyžádání z hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="69892-124">tooconfigure hello integration of Proofpoint on Demand into Azure AD, you need tooadd Proofpoint on Demand from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="69892-125">**tooadd Proofpoint na vyžádání z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="69892-125">**tooadd Proofpoint on Demand from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="69892-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="69892-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="69892-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="69892-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="69892-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="69892-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="69892-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="69892-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="69892-133">Hello vyhledávacího pole zadejte **Proofpoint na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="69892-133">In hello search box, type **Proofpoint on Demand**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_search.png)

5. <span data-ttu-id="69892-135">Na panelu výsledků hello vyberte **Proofpoint na vyžádání**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="69892-135">In hello results panel, select **Proofpoint on Demand**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="69892-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="69892-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="69892-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Proofpoint na vyžádání v testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="69892-138">In this section, you configure and test Azure AD single sign-on with Proofpoint on Demand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="69892-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Proofpoint na vyžádání je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="69892-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Proofpoint on Demand is tooa user in Azure AD.</span></span> <span data-ttu-id="69892-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Proofpoint na vyžádání musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="69892-140">In other words, a link relationship between an Azure AD user and hello related user in Proofpoint on Demand needs toobe established.</span></span>

<span data-ttu-id="69892-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Proofpoint na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="69892-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Proofpoint on Demand.</span></span>

<span data-ttu-id="69892-142">tooconfigure a testování Azure AD jednotné přihlašování s Proofpoint na vyžádání, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="69892-142">tooconfigure and test Azure AD single sign-on with Proofpoint on Demand, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="69892-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="69892-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="69892-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="69892-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="69892-145">**[Vytváření Proofpoint na vyžádání testovacího uživatele](#creating-a-proofpoint-on-demand-test-user)**  -toohave protějšek Britta Simon v Proofpoint na vyžádání, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="69892-145">**[Creating a Proofpoint on Demand test user](#creating-a-proofpoint-on-demand-test-user)** - toohave a counterpart of Britta Simon in Proofpoint on Demand that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="69892-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="69892-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="69892-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="69892-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="69892-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="69892-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="69892-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší Proofpoint na vyžádání aplikace.</span><span class="sxs-lookup"><span data-stu-id="69892-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Proofpoint on Demand application.</span></span>

<span data-ttu-id="69892-150">**tooconfigure Azure AD jednotné přihlašování s Proofpoint na vyžádání, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="69892-150">**tooconfigure Azure AD single sign-on with Proofpoint on Demand, perform hello following steps:**</span></span>

1. <span data-ttu-id="69892-151">V portálu Azure, na hello hello **Proofpoint na vyžádání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="69892-151">In hello Azure portal, on hello **Proofpoint on Demand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="69892-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="69892-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
  
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. <span data-ttu-id="69892-155">Na hello **Proofpoint na vyžádání domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="69892-155">On hello **Proofpoint on Demand Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    <span data-ttu-id="69892-157">a.In hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<hostname>.pphosted.com/ppssamlsp_hostname`</span><span class="sxs-lookup"><span data-stu-id="69892-157">a.In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<hostname>.pphosted.com/ppssamlsp_hostname`</span></span>

    <span data-ttu-id="69892-158">b.</span><span class="sxs-lookup"><span data-stu-id="69892-158">b.</span></span> <span data-ttu-id="69892-159">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<hostname>.pphosted.com/ppssamlsp`</span><span class="sxs-lookup"><span data-stu-id="69892-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<hostname>.pphosted.com/ppssamlsp`</span></span>

    <span data-ttu-id="69892-160">c.</span><span class="sxs-lookup"><span data-stu-id="69892-160">c.</span></span>  <span data-ttu-id="69892-161">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span><span class="sxs-lookup"><span data-stu-id="69892-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="69892-162">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="69892-162">These values are not hello real.</span></span> <span data-ttu-id="69892-163">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="69892-163">Update these values with hello actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="69892-164">Obraťte se na [Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="69892-164">Contact [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) tooget these values.</span></span> 

4. <span data-ttu-id="69892-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="69892-165">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

5. <span data-ttu-id="69892-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="69892-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="69892-169">Na hello **Proofpoint konfiguraci na vyžádání** klikněte na tlačítko **Proofpoint konfiguraci na vyžádání** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="69892-169">On hello **Proofpoint on Demand Configuration** section, click **Configure Proofpoint on Demand** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="69892-170">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="69892-170">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

7. <span data-ttu-id="69892-172">tooconfigure jednotného přihlašování na **Proofpoint na vyžádání** straně, je nutné stáhnout hello toosend **Certificate(Base64)**,**SAML Entity ID**, a **SAML Jednotné přihlašování adresa URL služby** příliš[Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services).</span><span class="sxs-lookup"><span data-stu-id="69892-172">tooconfigure single sign-on on **Proofpoint on Demand** side, you need toosend hello downloaded **Certificate(Base64)**,**SAML Entity ID**, and **SAML Single Sign-On Service URL** too[Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services).</span></span>

> [!TIP]
> <span data-ttu-id="69892-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="69892-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="69892-174">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="69892-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="69892-175">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="69892-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="69892-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="69892-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="69892-177">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="69892-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="69892-179">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="69892-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="69892-180">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="69892-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="69892-182">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="69892-182">These values are not hello real.</span></span> <span data-ttu-id="69892-183">Tyto hodnoty aktualizovat pomocí skutečného hello</span><span class="sxs-lookup"><span data-stu-id="69892-183">Update these values with hello actual</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="69892-185">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="69892-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="69892-187">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="69892-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="69892-189">a.</span><span class="sxs-lookup"><span data-stu-id="69892-189">a.</span></span> <span data-ttu-id="69892-190">V hello **název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="69892-190">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="69892-191">b.</span><span class="sxs-lookup"><span data-stu-id="69892-191">b.</span></span> <span data-ttu-id="69892-192">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="69892-192">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="69892-193">c.</span><span class="sxs-lookup"><span data-stu-id="69892-193">c.</span></span> <span data-ttu-id="69892-194">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="69892-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="69892-195">d.</span><span class="sxs-lookup"><span data-stu-id="69892-195">d.</span></span> <span data-ttu-id="69892-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="69892-196">Click **Create**.</span></span>
 
### <a name="creating-a-proofpoint-on-demand-test-user"></a><span data-ttu-id="69892-197">Vytváření Proofpoint na vyžádání testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="69892-197">Creating a Proofpoint on Demand test user</span></span>

<span data-ttu-id="69892-198">V této části vytvoříte uživatele volal Britta Simon v Proofpoint na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="69892-198">In this section, you create a user called Britta Simon in Proofpoint on Demand.</span></span> <span data-ttu-id="69892-199">Práce s [Proofpoint na tým podpory vyžádání klienta](https://www.proofpoint.com/us/support-services) tooadd uživatele v hello Proofpoint na vyžádání platformě.</span><span class="sxs-lookup"><span data-stu-id="69892-199">Work with [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) tooadd users in hello Proofpoint on Demand platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="69892-200">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="69892-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="69892-201">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooProofpoint přístup na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="69892-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooProofpoint on Demand.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="69892-203">**tooassign tooProofpoint Britta Simon na vyžádání, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="69892-203">**tooassign Britta Simon tooProofpoint on Demand, perform hello following steps:**</span></span>

1. <span data-ttu-id="69892-204">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="69892-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="69892-206">V seznamu aplikace hello vyberte **Proofpoint na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="69892-206">In hello applications list, select **Proofpoint on Demand**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png) 

3. <span data-ttu-id="69892-208">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="69892-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="69892-210">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="69892-210">Click **Add** button.</span></span> <span data-ttu-id="69892-211">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="69892-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="69892-213">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="69892-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="69892-214">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="69892-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="69892-215">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="69892-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="69892-216">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="69892-216">Testing single sign-on</span></span>

<span data-ttu-id="69892-217">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="69892-217">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="69892-218">Když kliknete na tlačítko hello **Proofpoint na vyžádání** na hello přístupového panelu dlaždici, můžete by měla být na tooyour Proofpoint na vyžádání aplikaci automaticky přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="69892-218">When you click hello **Proofpoint on Demand** tile on hello Access Panel, you should be automatically signed on tooyour Proofpoint on Demand application.</span></span>
<span data-ttu-id="69892-219">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="69892-219">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>  

## <a name="additional-resources"></a><span data-ttu-id="69892-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="69892-220">Additional resources</span></span>

* [<span data-ttu-id="69892-221">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="69892-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="69892-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="69892-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png

