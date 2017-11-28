---
title: 'Kurz: Azure Active Directory integrace s PlanMyLeave | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: 44a6782e44ef22fc957544960be1742f9eed6e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a><span data-ttu-id="6a350-103">Kurz: Azure Active Directory integrace s PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="6a350-103">Tutorial: Azure Active Directory integration with PlanMyLeave</span></span>

<span data-ttu-id="6a350-104">V tomto kurzu zjistíte, jak toointegrate PlanMyLeave s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6a350-104">In this tutorial, you learn how toointegrate PlanMyLeave with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6a350-105">Integrace PlanMyLeave s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6a350-105">Integrating PlanMyLeave with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6a350-106">Můžete řídit ve službě Azure AD, který má přístup tooPlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="6a350-106">You can control in Azure AD who has access tooPlanMyLeave</span></span>
- <span data-ttu-id="6a350-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPlanMyLeave (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a350-107">You can enable your users tooautomatically get signed-on tooPlanMyLeave (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6a350-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="6a350-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="6a350-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6a350-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a350-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a350-110">Prerequisites</span></span>

<span data-ttu-id="6a350-111">Integrace služby Azure AD s PlanMyLeave tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6a350-111">tooconfigure Azure AD integration with PlanMyLeave, you need hello following items:</span></span>

- <span data-ttu-id="6a350-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a350-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6a350-113">PlanMyLeave jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6a350-113">A PlanMyLeave single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="6a350-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a350-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="6a350-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6a350-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6a350-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="6a350-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="6a350-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6a350-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="6a350-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6a350-118">Scenario description</span></span>
<span data-ttu-id="6a350-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a350-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6a350-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6a350-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6a350-121">Přidání PlanMyLeave z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6a350-121">Adding PlanMyLeave from hello gallery</span></span>
2. <span data-ttu-id="6a350-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a350-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-planmyleave-from-hello-gallery"></a><span data-ttu-id="6a350-123">Přidání PlanMyLeave z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6a350-123">Adding PlanMyLeave from hello gallery</span></span>
<span data-ttu-id="6a350-124">tooconfigure hello integrace PlanMyLeave do Azure AD, je nutné tooadd PlanMyLeave hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6a350-124">tooconfigure hello integration of PlanMyLeave into Azure AD, you need tooadd PlanMyLeave from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6a350-125">**tooadd PlanMyLeave z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a350-125">**tooadd PlanMyLeave from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a350-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6a350-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6a350-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6a350-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6a350-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6a350-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6a350-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a350-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6a350-133">Hello vyhledávacího pole zadejte **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="6a350-133">In hello search box, type **PlanMyLeave**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. <span data-ttu-id="6a350-135">Na panelu výsledků hello vyberte **PlanMyLeave**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a350-135">In hello results panel, select **PlanMyLeave**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6a350-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a350-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6a350-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PlanMyLeave podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6a350-138">In this section, you configure and test Azure AD single sign-on with PlanMyLeave based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6a350-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v PlanMyLeave je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a350-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PlanMyLeave is tooa user in Azure AD.</span></span> <span data-ttu-id="6a350-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v PlanMyLeave musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6a350-140">In other words, a link relationship between an Azure AD user and hello related user in PlanMyLeave needs toobe established.</span></span>

<span data-ttu-id="6a350-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="6a350-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in PlanMyLeave.</span></span>

<span data-ttu-id="6a350-142">tooconfigure a testu Azure AD jednotné přihlašování s PlanMyLeave, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6a350-142">tooconfigure and test Azure AD single sign-on with PlanMyLeave, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6a350-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6a350-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6a350-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6a350-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6a350-145">**[Vytvoření zkušebního uživatele PlanMyLeave](#creating-a-planmyleave-test-user)**  -toohave protějšek Britta Simon v PlanMyLeave, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a350-145">**[Creating a PlanMyLeave test user](#creating-a-planmyleave-test-user)** - toohave a counterpart of Britta Simon in PlanMyLeave that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="6a350-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a350-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6a350-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6a350-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6a350-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a350-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6a350-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="6a350-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your PlanMyLeave application.</span></span>

<span data-ttu-id="6a350-150">**tooconfigure Azure AD jednotné přihlašování s PlanMyLeave, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a350-150">**tooconfigure Azure AD single sign-on with PlanMyLeave, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a350-151">V hello Azure Management portal na hello **PlanMyLeave** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6a350-151">In hello Azure Management portal, on hello **PlanMyLeave** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6a350-153">Na hello **jednotného přihlašování** dialogové okno stránky, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="6a350-153">On hello **Single sign-on** dialog page, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. <span data-ttu-id="6a350-155">Na hello **PlanMyLeave domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6a350-155">On hello **PlanMyLeave Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    <span data-ttu-id="6a350-157">a.</span><span class="sxs-lookup"><span data-stu-id="6a350-157">a.</span></span> <span data-ttu-id="6a350-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company-name>.planmyleave.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="6a350-158">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company-name>.planmyleave.com/Login.aspx`</span></span>
    
    <span data-ttu-id="6a350-159">b.</span><span class="sxs-lookup"><span data-stu-id="6a350-159">b.</span></span> <span data-ttu-id="6a350-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company-name>.planmyleave.com`</span><span class="sxs-lookup"><span data-stu-id="6a350-160">In hello **Identifer** textbox, type a URL using hello following pattern: `https://<company-name>.planmyleave.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6a350-161">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6a350-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="6a350-162">Máte tooupdate tyto hodnoty s hello skutečné přihlášení adresy URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6a350-162">You have tooupdate these values with hello actual Sign On URL and Identifier.</span></span> <span data-ttu-id="6a350-163">Obraťte se na [tým podpory PlanMyLeave](mailto:support@planmyleave.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6a350-163">Contact [PlanMyLeave support team](mailto:support@planmyleave.com) tooget these values.</span></span>

4. <span data-ttu-id="6a350-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="6a350-164">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. <span data-ttu-id="6a350-166">Na hello **vytvořit nový certifikát** dialogové okno, klikněte na ikonu hello kalendáře a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="6a350-166">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="6a350-167">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a350-167">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="6a350-169">Na hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a350-169">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. <span data-ttu-id="6a350-171">V místní nabídce hello **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a350-171">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6a350-173">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6a350-173">On hello **SAML Signing Certificate** section, click **Certificate (base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. <span data-ttu-id="6a350-175">Na hello **PlanMyLeave konfigurace** klikněte na tlačítko **konfigurace PlanMyLeave** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6a350-175">On hello **PlanMyLeave Configuration** section, click **Configure PlanMyLeave** tooopen **Configure sign-on** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. <span data-ttu-id="6a350-178">V okně prohlížeče jiný web Přihlaste se ke klientovi PlanMyLeave jako správce.</span><span class="sxs-lookup"><span data-stu-id="6a350-178">In a different web browser window, log into your PlanMyLeave tenant as an administrator.</span></span>

11. <span data-ttu-id="6a350-179">Přejděte příliš**nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="6a350-179">Go too**System Setup**.</span></span> <span data-ttu-id="6a350-180">Pak na hello **zabezpečení správy** klikněte na část **nastavení společnosti SAML** .</span><span class="sxs-lookup"><span data-stu-id="6a350-180">Then on hello **Security Management** section click **Company SAML settings** .</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. <span data-ttu-id="6a350-182">Na hello **SAML nastavení** klikněte ikonu editor.</span><span class="sxs-lookup"><span data-stu-id="6a350-182">On hello **SAML Settings** section, click editor icon.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. <span data-ttu-id="6a350-184">Na hello **nastavení SAML aktualizace** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6a350-184">On hello **Update SAML Settings** section, perform hello following steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    <span data-ttu-id="6a350-186">a.</span><span class="sxs-lookup"><span data-stu-id="6a350-186">a.</span></span>  <span data-ttu-id="6a350-187">V hello **přihlašovací adresa URL** textovému poli, vložte hodnotu hello z **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a350-187">In hello **Login URL** textbox, put hello value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="6a350-188">b.</span><span class="sxs-lookup"><span data-stu-id="6a350-188">b.</span></span>  <span data-ttu-id="6a350-189">V poznámkovém bloku otevřete soubor stažený certifikátu, kopírovat pouze hello obsah mezi hello---začít certifikát a---End certifikát---ho do schránky a pak ji vložit toohello **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6a350-189">Open your downloaded certificate file in notepad, copy only hello content between hello ---Begin Certificate--- and ---End certificate---- of it into your clipboard, and then paste it toohello **Certificate** textbox.</span></span>

    <span data-ttu-id="6a350-190">c.</span><span class="sxs-lookup"><span data-stu-id="6a350-190">c.</span></span> <span data-ttu-id="6a350-191">Nastavení"**je povolit**"příliš"**Ano**".</span><span class="sxs-lookup"><span data-stu-id="6a350-191">Set "**Is Enable**" too"**Yes**".</span></span>

    <span data-ttu-id="6a350-192">d.</span><span class="sxs-lookup"><span data-stu-id="6a350-192">d.</span></span> <span data-ttu-id="6a350-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6a350-193">Click **Save**.</span></span>



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6a350-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a350-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="6a350-195">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6a350-195">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6a350-197">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a350-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a350-198">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6a350-198">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6a350-200">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6a350-200">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6a350-202">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a350-202">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6a350-204">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6a350-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6a350-206">a.</span><span class="sxs-lookup"><span data-stu-id="6a350-206">a.</span></span> <span data-ttu-id="6a350-207">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6a350-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6a350-208">b.</span><span class="sxs-lookup"><span data-stu-id="6a350-208">b.</span></span> <span data-ttu-id="6a350-209">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6a350-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6a350-210">c.</span><span class="sxs-lookup"><span data-stu-id="6a350-210">c.</span></span> <span data-ttu-id="6a350-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6a350-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6a350-212">d.</span><span class="sxs-lookup"><span data-stu-id="6a350-212">d.</span></span> <span data-ttu-id="6a350-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6a350-213">Click **Create**.</span></span> 



### <a name="creating-a-planmyleave-test-user"></a><span data-ttu-id="6a350-214">Vytvoření zkušebního uživatele PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="6a350-214">Creating a PlanMyLeave test user</span></span>

<span data-ttu-id="6a350-215">Hello cílem této části je toocreate volal Britta Simon v PlanMyLeave uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a350-215">hello objective of this section is toocreate a user called Britta Simon in PlanMyLeave.</span></span> <span data-ttu-id="6a350-216">PlanMyLeave podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="6a350-216">PlanMyLeave supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="6a350-217">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="6a350-217">There is no action item for you in this section.</span></span> <span data-ttu-id="6a350-218">Pokud ještě neexistuje, se během pokusu o tooaccess PlanMyLeave vytvoří nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a350-218">A new user will be created during an attempt tooaccess PlanMyLeave if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="6a350-219">Pokud potřebujete toocreate uživatelé ručně, je nutné toocontact [tým podpory PlanMyLeave](mailto:support@planmyleave.com).</span><span class="sxs-lookup"><span data-stu-id="6a350-219">If you need toocreate an user manually, you need toocontact [PlanMyLeave support team](mailto:support@planmyleave.com).</span></span>



### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6a350-220">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6a350-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6a350-221">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooPlanMyLeave svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="6a350-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooPlanMyLeave.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6a350-223">**tooassign Britta Simon tooPlanMyLeave, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a350-223">**tooassign Britta Simon tooPlanMyLeave, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a350-224">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6a350-224">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6a350-226">V seznamu aplikace hello vyberte **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="6a350-226">In hello applications list, select **PlanMyLeave**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. <span data-ttu-id="6a350-228">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6a350-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6a350-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a350-230">Click **Add** button.</span></span> <span data-ttu-id="6a350-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a350-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6a350-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6a350-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6a350-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a350-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6a350-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a350-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="6a350-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a350-236">Testing single sign-on</span></span>

<span data-ttu-id="6a350-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6a350-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6a350-238">Když kliknete na dlaždici PlanMyLeave hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour PlanMyLeave aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a350-238">When you click hello PlanMyLeave tile in hello Access Panel, you should get automatically signed-on tooyour PlanMyLeave application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="6a350-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6a350-239">Additional resources</span></span>

* [<span data-ttu-id="6a350-240">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6a350-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6a350-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6a350-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png