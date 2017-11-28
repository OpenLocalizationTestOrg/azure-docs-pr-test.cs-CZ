---
title: "Kurz: Azure Active Directory integrace s virtuálními počítači IQNavigator | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a IQNavigator virtuálních počítačů."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 5a5a7dd3f427acfba2f0ae10552a7179db730118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a><span data-ttu-id="07636-103">Kurz: Azure Active Directory integrace s virtuálními počítači IQNavigator</span><span class="sxs-lookup"><span data-stu-id="07636-103">Tutorial: Azure Active Directory integration with IQNavigator VMS</span></span>

<span data-ttu-id="07636-104">V tomto kurzu zjistíte, jak toointegrate IQNavigator virtuálních počítačů s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="07636-104">In this tutorial, you learn how toointegrate IQNavigator VMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="07636-105">Integrace IQNavigator virtuálních počítačů s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="07636-105">Integrating IQNavigator VMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="07636-106">Můžete řídit ve službě Azure AD, který má přístup tooIQNavigator virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="07636-106">You can control in Azure AD who has access tooIQNavigator VMS</span></span>
- <span data-ttu-id="07636-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooIQNavigator virtuálních počítačů (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="07636-107">You can enable your users tooautomatically get signed-on tooIQNavigator VMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="07636-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="07636-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="07636-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="07636-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07636-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07636-110">Prerequisites</span></span>

<span data-ttu-id="07636-111">Integrace služby Azure AD s virtuálními počítači IQNavigator tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="07636-111">tooconfigure Azure AD integration with IQNavigator VMS, you need hello following items:</span></span>

- <span data-ttu-id="07636-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="07636-112">An Azure AD subscription</span></span>
- <span data-ttu-id="07636-113">Virtuální počítače IQNavigator jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="07636-113">A IQNavigator VMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="07636-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="07636-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="07636-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="07636-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="07636-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="07636-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="07636-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="07636-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="07636-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="07636-118">Scenario description</span></span>
<span data-ttu-id="07636-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="07636-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="07636-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="07636-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="07636-121">Přidání IQNavigator virtuálních počítačů z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="07636-121">Adding IQNavigator VMS from hello gallery</span></span>
2. <span data-ttu-id="07636-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="07636-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-iqnavigator-vms-from-hello-gallery"></a><span data-ttu-id="07636-123">Přidání IQNavigator virtuálních počítačů z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="07636-123">Adding IQNavigator VMS from hello gallery</span></span>
<span data-ttu-id="07636-124">tooconfigure hello integrace IQNavigator virtuálních počítačů do Azure AD, je nutné tooadd IQNavigator virtuální počítače z hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="07636-124">tooconfigure hello integration of IQNavigator VMS into Azure AD, you need tooadd IQNavigator VMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="07636-125">**tooadd IQNavigator virtuálních počítačů z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="07636-125">**tooadd IQNavigator VMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="07636-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="07636-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="07636-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="07636-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="07636-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="07636-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="07636-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="07636-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="07636-133">Hello vyhledávacího pole zadejte **virtuální počítače IQNavigator**.</span><span class="sxs-lookup"><span data-stu-id="07636-133">In hello search box, type **IQNavigator VMS**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

5. <span data-ttu-id="07636-135">Na panelu výsledků hello vyberte **IQNavigator virtuální počítače**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="07636-135">In hello results panel, select **IQNavigator VMS**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="07636-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="07636-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="07636-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s virtuálními počítači IQNavigator podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="07636-138">In this section, you configure and test Azure AD single sign-on with IQNavigator VMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="07636-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello ve virtuálních počítačích IQNavigator je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07636-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in IQNavigator VMS is tooa user in Azure AD.</span></span> <span data-ttu-id="07636-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello ve virtuálních počítačích IQNavigator musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="07636-140">In other words, a link relationship between an Azure AD user and hello related user in IQNavigator VMS needs toobe established.</span></span>

<span data-ttu-id="07636-141">Ve virtuálních počítačích IQNavigator, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="07636-141">In IQNavigator VMS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="07636-142">tooconfigure a testování Azure AD jednotné přihlašování s virtuálními počítači IQNavigator, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="07636-142">tooconfigure and test Azure AD single sign-on with IQNavigator VMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="07636-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="07636-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="07636-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="07636-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="07636-145">**[Vytvoření zkušebního uživatele virtuální počítače IQNavigator](#creating-a-iqnavigator-vms-test-user)**  -toohave protějšek Britta Simon ve virtuálních počítačích IQNavigator, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="07636-145">**[Creating a IQNavigator VMS test user](#creating-a-iqnavigator-vms-test-user)** - toohave a counterpart of Britta Simon in IQNavigator VMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="07636-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="07636-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="07636-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="07636-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="07636-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="07636-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="07636-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci IQNavigator virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="07636-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your IQNavigator VMS application.</span></span>

<span data-ttu-id="07636-150">**tooconfigure Azure AD jednotné přihlašování s virtuálními počítači IQNavigator, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="07636-150">**tooconfigure Azure AD single sign-on with IQNavigator VMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="07636-151">V portálu Azure, na hello hello **virtuální počítače IQNavigator** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="07636-151">In hello Azure portal, on hello **IQNavigator VMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="07636-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="07636-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

3. <span data-ttu-id="07636-155">Na hello **IQNavigator virtuálních počítačů domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="07636-155">On hello **IQNavigator VMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    <span data-ttu-id="07636-157">a.</span><span class="sxs-lookup"><span data-stu-id="07636-157">a.</span></span> <span data-ttu-id="07636-158">V hello **identifikátor** textovému poli, hello zadat adresu URL:`iqn.com`</span><span class="sxs-lookup"><span data-stu-id="07636-158">In hello **Identifier** textbox, type hello URL:`iqn.com`</span></span>

    <span data-ttu-id="07636-159">b.</span><span class="sxs-lookup"><span data-stu-id="07636-159">b.</span></span> <span data-ttu-id="07636-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="07636-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span></span>

4. <span data-ttu-id="07636-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="07636-161">Check **Show advanced URL settings**, perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    <span data-ttu-id="07636-163">V hello **předávání stavu** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.iqnavigator.com`</span><span class="sxs-lookup"><span data-stu-id="07636-163">In hello **Relay state** textbox, type a URL using hello following pattern:`https://<subdomain>.iqnavigator.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="07636-164">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="07636-164">These values are not real.</span></span> <span data-ttu-id="07636-165">Tyto hodnoty aktualizujte stavem hello skutečná adresa URL odpovědi a předávání.</span><span class="sxs-lookup"><span data-stu-id="07636-165">Update these values with hello actual Reply URL and Relay state.</span></span> <span data-ttu-id="07636-166">Obraťte se na [tým podpory IQNavigator virtuální počítače klientů](https://www.beeline.com/iqn-product-support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="07636-166">Contact [IQNavigator VMS Client support team](https://www.beeline.com/iqn-product-support/) tooget these values.</span></span> 

5. <span data-ttu-id="07636-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="07636-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="07636-169">toogenerate hello **Metadata** adresu url, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="07636-169">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="07636-170">a.</span><span class="sxs-lookup"><span data-stu-id="07636-170">a.</span></span> <span data-ttu-id="07636-171">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="07636-171">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appregistrations.png)
   
    <span data-ttu-id="07636-173">b.</span><span class="sxs-lookup"><span data-stu-id="07636-173">b.</span></span> <span data-ttu-id="07636-174">Klikněte na tlačítko **koncové body** tooopen **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="07636-174">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpointicon.png)

    <span data-ttu-id="07636-176">c.</span><span class="sxs-lookup"><span data-stu-id="07636-176">c.</span></span> <span data-ttu-id="07636-177">Klikněte na tlačítko hello kopie tlačítko toocopy **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="07636-177">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpoint.png)
     
    <span data-ttu-id="07636-179">d.</span><span class="sxs-lookup"><span data-stu-id="07636-179">d.</span></span> <span data-ttu-id="07636-180">Nyní přejděte na stránce vlastností toohello **virtuální počítače IQNavigator** a kopírování hello **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="07636-180">Now go toohello property page of **IQNavigator VMS** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appid.png)

    <span data-ttu-id="07636-182">e.</span><span class="sxs-lookup"><span data-stu-id="07636-182">e.</span></span> <span data-ttu-id="07636-183">Generovat hello **adresu URL metadat** pomocí hello následující vzoru:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="07636-183">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

7. <span data-ttu-id="07636-184">Aplikace IQNavigator očekávají, že hodnota identifikátoru jedinečná uživatelská hello hello název identifikátor deklarace.</span><span class="sxs-lookup"><span data-stu-id="07636-184">IQNavigator application expect hello unique user identifier value in hello Name Identifier claim.</span></span> <span data-ttu-id="07636-185">Zákazníka můžete namapovat hello správnou hodnotu pro název identifikátor hello deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="07636-185">Customer can map hello correct value for hello Name Identifier claim.</span></span> <span data-ttu-id="07636-186">V takovém případě jsme mají mapovat uživatele hello. UserPrincipalName za účelem ukázku hello.</span><span class="sxs-lookup"><span data-stu-id="07636-186">In this case we have mapped hello user.UserPrincipalName for hello demo purpose.</span></span> <span data-ttu-id="07636-187">Ale podle tooyour nastavení organizace by měla být mapována hello správnou hodnotu pro ni.</span><span class="sxs-lookup"><span data-stu-id="07636-187">But according tooyour organization settings you should map hello correct value for it.</span></span>   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

8. <span data-ttu-id="07636-189">Na hello **konfigurace virtuálních počítačů IQNavigator** klikněte na tlačítko **konfigurace virtuálních počítačů IQNavigator** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="07636-189">On hello **IQNavigator VMS Configuration** section, click **Configure IQNavigator VMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="07636-190">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="07636-190">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png) 

9. <span data-ttu-id="07636-192">tooconfigure jednotného přihlašování na **virtuální počítače IQNavigator** postranní, budete potřebovat toosend hello **adresu URL metadat**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš [Tým podpory pro virtuální počítače IQNavigator](https://www.beeline.com/iqn-product-support/).</span><span class="sxs-lookup"><span data-stu-id="07636-192">tooconfigure single sign-on on **IQNavigator VMS** side, you need toosend hello **Metadata URL**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/).</span></span> <span data-ttu-id="07636-193">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="07636-193">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="07636-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="07636-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="07636-195">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="07636-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="07636-196">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="07636-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="07636-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="07636-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="07636-198">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="07636-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="07636-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="07636-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="07636-201">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="07636-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="07636-203">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="07636-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="07636-205">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="07636-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="07636-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="07636-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="07636-209">a.</span><span class="sxs-lookup"><span data-stu-id="07636-209">a.</span></span> <span data-ttu-id="07636-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="07636-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="07636-211">b.</span><span class="sxs-lookup"><span data-stu-id="07636-211">b.</span></span> <span data-ttu-id="07636-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="07636-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="07636-213">c.</span><span class="sxs-lookup"><span data-stu-id="07636-213">c.</span></span> <span data-ttu-id="07636-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="07636-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="07636-215">d.</span><span class="sxs-lookup"><span data-stu-id="07636-215">d.</span></span> <span data-ttu-id="07636-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="07636-216">Click **Create**.</span></span>
 
### <a name="creating-a-iqnavigator-vms-test-user"></a><span data-ttu-id="07636-217">Vytvoření zkušebního uživatele IQNavigator virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="07636-217">Creating a IQNavigator VMS test user</span></span>

<span data-ttu-id="07636-218">Hello cílem této části je toocreate názvem Britta Simon ve virtuálních počítačích IQNavigator uživatele.</span><span class="sxs-lookup"><span data-stu-id="07636-218">hello objective of this section is toocreate a user called Britta Simon in IQNavigator VMS.</span></span> <span data-ttu-id="07636-219">Práce s [tým podpory pro virtuální počítače IQNavigator](https://www.beeline.com/iqn-product-support/) tooadd hello uživatele v hello účet IQNavigator virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="07636-219">Work with  [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/) tooadd hello users in hello IQNavigator VMS account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="07636-220">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="07636-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="07636-221">V této části je povolit Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup tooIQNavigator virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="07636-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIQNavigator VMS.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="07636-223">**tooassign Britta Simon tooIQNavigator virtuální počítače, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="07636-223">**tooassign Britta Simon tooIQNavigator VMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="07636-224">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="07636-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="07636-226">V seznamu aplikace hello vyberte **virtuální počítače IQNavigator**.</span><span class="sxs-lookup"><span data-stu-id="07636-226">In hello applications list, select **IQNavigator VMS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png) 

3. <span data-ttu-id="07636-228">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="07636-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="07636-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="07636-230">Click **Add** button.</span></span> <span data-ttu-id="07636-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="07636-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="07636-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="07636-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="07636-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="07636-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="07636-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="07636-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="07636-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="07636-236">Testing single sign-on</span></span>

<span data-ttu-id="07636-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="07636-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="07636-238">Po kliknutí na tlačítko hello dlaždice IQNavigator virtuálních počítačů v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikace IQNavigator virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="07636-238">When you click hello IQNavigator VMS tile in hello Access Panel, you should get automatically signed-on tooyour IQNavigator VMS application.</span></span>
<span data-ttu-id="07636-239">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="07636-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="07636-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="07636-240">Additional resources</span></span>

* [<span data-ttu-id="07636-241">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07636-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="07636-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="07636-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_203.png

