---
title: 'Kurz: Azure Active Directory integrace s MaxxPoint | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a MaxxPoint."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ba026e-96fc-4ae8-b135-0169da810e99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 03b13908add8d8c62f1d1480ed2288658fce14d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-maxxpoint"></a><span data-ttu-id="167d0-103">Kurz: Azure Active Directory integrace s MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="167d0-103">Tutorial: Azure Active Directory integration with MaxxPoint</span></span>

<span data-ttu-id="167d0-104">V tomto kurzu zjistíte, jak toointegrate MaxxPoint s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="167d0-104">In this tutorial, you learn how toointegrate MaxxPoint with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="167d0-105">Integrace MaxxPoint s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="167d0-105">Integrating MaxxPoint with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="167d0-106">Můžete řídit ve službě Azure AD, který má přístup tooMaxxPoint</span><span class="sxs-lookup"><span data-stu-id="167d0-106">You can control in Azure AD who has access tooMaxxPoint</span></span>
- <span data-ttu-id="167d0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMaxxPoint (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="167d0-107">You can enable your users tooautomatically get signed-on tooMaxxPoint (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="167d0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="167d0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="167d0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="167d0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="167d0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="167d0-110">Prerequisites</span></span>

<span data-ttu-id="167d0-111">Integrace služby Azure AD s MaxxPoint tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="167d0-111">tooconfigure Azure AD integration with MaxxPoint, you need hello following items:</span></span>

- <span data-ttu-id="167d0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="167d0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="167d0-113">MaxxPoint jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="167d0-113">A MaxxPoint single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="167d0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="167d0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="167d0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="167d0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="167d0-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="167d0-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="167d0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="167d0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="167d0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="167d0-118">Scenario description</span></span>
<span data-ttu-id="167d0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="167d0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="167d0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="167d0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="167d0-121">Přidání MaxxPoint z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="167d0-121">Adding MaxxPoint from hello gallery</span></span>
2. <span data-ttu-id="167d0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="167d0-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-maxxpoint-from-hello-gallery"></a><span data-ttu-id="167d0-123">Přidání MaxxPoint z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="167d0-123">Adding MaxxPoint from hello gallery</span></span>
<span data-ttu-id="167d0-124">tooconfigure hello integrace MaxxPoint do Azure AD, je nutné tooadd MaxxPoint hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="167d0-124">tooconfigure hello integration of MaxxPoint into Azure AD, you need tooadd MaxxPoint from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="167d0-125">**tooadd MaxxPoint z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="167d0-125">**tooadd MaxxPoint from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="167d0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="167d0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="167d0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="167d0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="167d0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="167d0-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="167d0-131">Klikněte na tlačítko **novou aplikaci** tlačítka v horní části hello dialogové okno tooadd nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="167d0-131">Click **New application** button on hello top of dialog tooadd new application.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="167d0-133">Hello vyhledávacího pole zadejte **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="167d0-133">In hello search box, type **MaxxPoint**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_001.png)

5. <span data-ttu-id="167d0-135">Na panelu výsledků hello vyberte **MaxxPoint**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="167d0-135">In hello results panel, select **MaxxPoint**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="167d0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="167d0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="167d0-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s MaxxPoint podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="167d0-138">In this section, you configure and test Azure AD single sign-on with MaxxPoint based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="167d0-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v MaxxPoint je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="167d0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MaxxPoint is tooa user in Azure AD.</span></span> <span data-ttu-id="167d0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v MaxxPoint musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="167d0-140">In other words, a link relationship between an Azure AD user and hello related user in MaxxPoint needs toobe established.</span></span>

<span data-ttu-id="167d0-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="167d0-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in MaxxPoint.</span></span>

<span data-ttu-id="167d0-142">tooconfigure a testu Azure AD jednotné přihlašování s MaxxPoint, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="167d0-142">tooconfigure and test Azure AD single sign-on with MaxxPoint, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="167d0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="167d0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="167d0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="167d0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="167d0-145">**[Vytvoření zkušebního uživatele MaxxPoint](#creating-a-maxxpoint-test-user)**  -toohave protějšek Britta Simon v MaxxPoint, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="167d0-145">**[Creating a MaxxPoint test user](#creating-a-maxxpoint-test-user)** - toohave a counterpart of Britta Simon in MaxxPoint that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="167d0-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="167d0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="167d0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="167d0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="167d0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="167d0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="167d0-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="167d0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MaxxPoint application.</span></span>

<span data-ttu-id="167d0-150">**tooconfigure Azure AD jednotné přihlašování s MaxxPoint, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="167d0-150">**tooconfigure Azure AD single sign-on with MaxxPoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="167d0-151">V portálu Azure, na hello hello **MaxxPoint** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="167d0-151">In hello Azure portal, on hello **MaxxPoint** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="167d0-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="167d0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_300.png)

3. <span data-ttu-id="167d0-155">Na hello **MaxxPoint domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, žádné potřebovat tooperform žádné kroky.</span><span class="sxs-lookup"><span data-stu-id="167d0-155">On hello **MaxxPoint Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, no need tooperform any steps.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_02.png)
    
4. <span data-ttu-id="167d0-157">Na hello **MaxxPoint domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="167d0-157">On hello **MaxxPoint Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_03.png)

    <span data-ttu-id="167d0-159">a.</span><span class="sxs-lookup"><span data-stu-id="167d0-159">a.</span></span> <span data-ttu-id="167d0-160">Klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** možnost</span><span class="sxs-lookup"><span data-stu-id="167d0-160">Click **Show advanced URL settings** option</span></span>

    <span data-ttu-id="167d0-161">b.</span><span class="sxs-lookup"><span data-stu-id="167d0-161">b.</span></span> <span data-ttu-id="167d0-162">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span><span class="sxs-lookup"><span data-stu-id="167d0-162">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="167d0-163">Upozorňujeme, že se nejedná hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="167d0-163">Please note that this is not hello real value.</span></span> <span data-ttu-id="167d0-164">Máte tooupdate tuto hodnotu s hello skutečné přihlásit na adrese URL.</span><span class="sxs-lookup"><span data-stu-id="167d0-164">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="167d0-165">Volání MaxxPoint týmu na **888-728-0950** tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="167d0-165">Call MaxxPoint team on **888-728-0950** tooget this value.</span></span>

5. <span data-ttu-id="167d0-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="167d0-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_06.png) 

6. <span data-ttu-id="167d0-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="167d0-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="167d0-170">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, použijte na tým podpory MaxxPoint **888-728-0950** a budete vám další pomůže na způsobu tooprovide je hello stahování **soubor XML s metadaty** souboru.</span><span class="sxs-lookup"><span data-stu-id="167d0-170">tooget SSO configured for your application, call MaxxPoint support team on **888-728-0950** and they'll assist you further on how tooprovide them hello downloaded **Metadata XML** file.</span></span> 

> [!TIP]
> <span data-ttu-id="167d0-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="167d0-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="167d0-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="167d0-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="167d0-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="167d0-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="167d0-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="167d0-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="167d0-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="167d0-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="167d0-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="167d0-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="167d0-178">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="167d0-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="167d0-180">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="167d0-180">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="167d0-182">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="167d0-182">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="167d0-184">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="167d0-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="167d0-186">a.</span><span class="sxs-lookup"><span data-stu-id="167d0-186">a.</span></span> <span data-ttu-id="167d0-187">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="167d0-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="167d0-188">b.</span><span class="sxs-lookup"><span data-stu-id="167d0-188">b.</span></span> <span data-ttu-id="167d0-189">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="167d0-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="167d0-190">c.</span><span class="sxs-lookup"><span data-stu-id="167d0-190">c.</span></span> <span data-ttu-id="167d0-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="167d0-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="167d0-192">d.</span><span class="sxs-lookup"><span data-stu-id="167d0-192">d.</span></span> <span data-ttu-id="167d0-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="167d0-193">Click **Create**.</span></span> 

### <a name="creating-a-maxxpoint-test-user"></a><span data-ttu-id="167d0-194">Vytvoření zkušebního uživatele MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="167d0-194">Creating a MaxxPoint test user</span></span>

<span data-ttu-id="167d0-195">V této části vytvoříte volal Britta Simon v MaxxPoint uživatele.</span><span class="sxs-lookup"><span data-stu-id="167d0-195">In this section, you create a user called Britta Simon in MaxxPoint.</span></span> <span data-ttu-id="167d0-196">Zavolejte tým podpory MaxxPoint na **888-728-0950** tooadd hello uživatele v hello MaxxPoint aplikace.</span><span class="sxs-lookup"><span data-stu-id="167d0-196">Please call MaxxPoint support team on **888-728-0950** tooadd hello users in hello MaxxPoint application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="167d0-197">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="167d0-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="167d0-198">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooMaxxPoint svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="167d0-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooMaxxPoint.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="167d0-200">**tooassign Britta Simon tooMaxxPoint, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="167d0-200">**tooassign Britta Simon tooMaxxPoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="167d0-201">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="167d0-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="167d0-203">V seznamu aplikace hello vyberte **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="167d0-203">In hello applications list, select **MaxxPoint**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_50.png) 

3. <span data-ttu-id="167d0-205">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="167d0-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="167d0-207">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="167d0-207">Click **Add** button.</span></span> <span data-ttu-id="167d0-208">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="167d0-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="167d0-210">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="167d0-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="167d0-211">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="167d0-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="167d0-212">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="167d0-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="167d0-213">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="167d0-213">Testing single sign-on</span></span>

<span data-ttu-id="167d0-214">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="167d0-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="167d0-215">Když kliknete na dlaždici MaxxPoint hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour MaxxPoint aplikace.</span><span class="sxs-lookup"><span data-stu-id="167d0-215">When you click hello MaxxPoint tile in hello Access Panel, you should get automatically signed-on tooyour MaxxPoint application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="167d0-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="167d0-216">Additional resources</span></span>

* [<span data-ttu-id="167d0-217">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="167d0-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="167d0-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="167d0-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_203.png