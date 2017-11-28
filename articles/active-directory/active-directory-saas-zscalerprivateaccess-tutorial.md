---
title: "Kurz: Azure Active Directory integrace s Zscaler privátní přístup (ZPA) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Zscaler privátní přístup (ZPA)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 0370cff60c8ac15bd1919acccc924da1e50dc45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a><span data-ttu-id="8a386-103">Kurz: Azure Active Directory integrace s Zscaler privátní přístup (ZPA)</span><span class="sxs-lookup"><span data-stu-id="8a386-103">Tutorial: Azure Active Directory integration with Zscaler Private Access (ZPA)</span></span>

<span data-ttu-id="8a386-104">V tomto kurzu zjistíte, jak toointegrate Zscaler privátní přístup (ZPA) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8a386-104">In this tutorial, you learn how toointegrate Zscaler Private Access (ZPA) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8a386-105">Integrace Zscaler privátní přístup (ZPA) s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8a386-105">Integrating Zscaler Private Access (ZPA) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8a386-106">Můžete řídit ve službě Azure AD, který má přístup tooZscaler privátní přístup (ZPA)</span><span class="sxs-lookup"><span data-stu-id="8a386-106">You can control in Azure AD who has access tooZscaler Private Access (ZPA)</span></span>
- <span data-ttu-id="8a386-107">Vaši uživatelé tooautomatically get přihlášeného tooZscaler privátní přístup (ZPA) (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a386-107">You can enable your users tooautomatically get signed-on tooZscaler Private Access (ZPA) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8a386-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="8a386-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="8a386-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8a386-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a386-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8a386-110">Prerequisites</span></span>

<span data-ttu-id="8a386-111">tooconfigure integrace Azure AD s Zscaler privátní přístup (ZPA), je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8a386-111">tooconfigure Azure AD integration with Zscaler Private Access (ZPA), you need hello following items:</span></span>

- <span data-ttu-id="8a386-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a386-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8a386-113">Zscaler privátní přístup (ZPA) jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8a386-113">A Zscaler Private Access (ZPA) single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="8a386-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8a386-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="8a386-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8a386-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8a386-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="8a386-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="8a386-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8a386-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="8a386-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8a386-118">Scenario description</span></span>
<span data-ttu-id="8a386-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8a386-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8a386-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8a386-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8a386-121">Přidání Zscaler privátní přístup (ZPA) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8a386-121">Adding Zscaler Private Access (ZPA) from hello gallery</span></span>
2. <span data-ttu-id="8a386-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a386-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zscaler-private-access-zpa-from-hello-gallery"></a><span data-ttu-id="8a386-123">Přidání Zscaler privátní přístup (ZPA) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8a386-123">Adding Zscaler Private Access (ZPA) from hello gallery</span></span>
<span data-ttu-id="8a386-124">tooconfigure hello integrace z Zscaler privátní přístup (ZPA) do Azure AD, je nutné tooadd Zscaler privátní přístup (ZPA) hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8a386-124">tooconfigure hello integration of Zscaler Private Access (ZPA) into Azure AD, you need tooadd Zscaler Private Access (ZPA) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8a386-125">**tooadd Zscaler privátní přístup (ZPA) z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8a386-125">**tooadd Zscaler Private Access (ZPA) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8a386-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8a386-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8a386-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8a386-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8a386-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8a386-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8a386-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a386-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8a386-133">Hello vyhledávacího pole zadejte **Zscaler privátní přístup (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="8a386-133">In hello search box, type **Zscaler Private Access (ZPA)**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. <span data-ttu-id="8a386-135">Na panelu výsledků hello vyberte **Zscaler privátní přístup (ZPA)**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a386-135">In hello results panel, select **Zscaler Private Access (ZPA)**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8a386-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a386-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8a386-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Zscaler privátní přístup (ZPA) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="8a386-138">In this section, you configure and test Azure AD single sign-on with Zscaler Private Access (ZPA) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8a386-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Zscaler privátní přístup (ZPA) je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8a386-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler Private Access (ZPA) is tooa user in Azure AD.</span></span> <span data-ttu-id="8a386-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Zscaler privátní přístup (ZPA) musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8a386-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler Private Access (ZPA) needs toobe established.</span></span>

<span data-ttu-id="8a386-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="8a386-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zscaler Private Access (ZPA).</span></span>

<span data-ttu-id="8a386-142">tooconfigure a testování Azure AD jednotné přihlašování s Zscaler privátní přístup (ZPA), je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="8a386-142">tooconfigure and test Azure AD single sign-on with Zscaler Private Access (ZPA), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8a386-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8a386-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8a386-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8a386-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8a386-145">**[Vytvoření zkušebního uživatele Zscaler privátní přístup (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**  -toohave protějšek Britta Simon v Zscaler privátní přístup (ZPA), která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8a386-145">**[Creating a Zscaler Private Access (ZPA) test user](#creating-a-zscaler-private-access-(zpa)-test-user)** - toohave a counterpart of Britta Simon in Zscaler Private Access (ZPA) that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="8a386-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8a386-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8a386-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8a386-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8a386-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a386-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8a386-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="8a386-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Zscaler Private Access (ZPA) application.</span></span>

<span data-ttu-id="8a386-150">**tooconfigure Azure AD jednotné přihlašování s Zscaler privátní přístup (ZPA), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8a386-150">**tooconfigure Azure AD single sign-on with Zscaler Private Access (ZPA), perform hello following steps:**</span></span>

1. <span data-ttu-id="8a386-151">V hello Azure Management portal na hello **Zscaler privátní přístup (ZPA)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8a386-151">In hello Azure Management portal, on hello **Zscaler Private Access (ZPA)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8a386-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="8a386-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="8a386-155">Na hello **Zscaler privátní přístup (ZPA) domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8a386-155">On hello **Zscaler Private Access (ZPA) Domain and URLs** section, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    <span data-ttu-id="8a386-157">a.</span><span class="sxs-lookup"><span data-stu-id="8a386-157">a.</span></span> <span data-ttu-id="8a386-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span><span class="sxs-lookup"><span data-stu-id="8a386-158">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span></span>

    <span data-ttu-id="8a386-159">b.</span><span class="sxs-lookup"><span data-stu-id="8a386-159">b.</span></span> <span data-ttu-id="8a386-160">V hello **identifikátor** textovému poli, typ:`https://samlsp.private.zscaler.com/auth/metadata`</span><span class="sxs-lookup"><span data-stu-id="8a386-160">In hello **Identifier** textbox, type: `https://samlsp.private.zscaler.com/auth/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8a386-161">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8a386-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="8a386-162">Máte tooupdate tyto hodnoty s hello skutečné přihlášení adresy URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8a386-162">You have tooupdate these values with hello actual Sign On URL and Identifier.</span></span> <span data-ttu-id="8a386-163">Zde, doporučujeme vám toouse hello jedinečnou hodnotu adresy URL v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8a386-163">Here we suggest you toouse hello unique value of URL in hello Identifier.</span></span> <span data-ttu-id="8a386-164">Obraťte se na [tým podpory Zscaler privátní přístup (ZPA)](https://help.zscaler.com/zpa-submit-ticket) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8a386-164">Contact [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) tooget these values.</span></span>

4. <span data-ttu-id="8a386-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="8a386-165">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. <span data-ttu-id="8a386-167">Na hello **vytvořit nový certifikát** dialogové okno, klikněte na ikonu hello kalendáře a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="8a386-167">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="8a386-168">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a386-168">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="8a386-170">Na hello **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a386-170">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. <span data-ttu-id="8a386-172">V místní nabídce hello **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a386-172">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="8a386-174">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8a386-174">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. <span data-ttu-id="8a386-176">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="8a386-176">In a different web browser window, log into your Zscaler Private Access (ZPA) company site as an administrator.</span></span>

10. <span data-ttu-id="8a386-177">Přejděte příliš**správce** a pak klikněte na **Idp konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="8a386-177">Navigate too**Administrator** and then click **Idp Configuration**.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. <span data-ttu-id="8a386-179">V hello **Idp konfigurace** klikněte na tlačítko **přidat novou konfiguraci IDP**.</span><span class="sxs-lookup"><span data-stu-id="8a386-179">In hello **Idp Configuration** section, click **Add New IDP Configuration**.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. <span data-ttu-id="8a386-181">V hello **novou konfiguraci IDP** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8a386-181">In hello **New IDP Configuration** section, perform hello following steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    <span data-ttu-id="8a386-183">a.</span><span class="sxs-lookup"><span data-stu-id="8a386-183">a.</span></span> <span data-ttu-id="8a386-184">Klikněte na tlačítko **vyberte soubor** a nahrát soubor stažený metadat.</span><span class="sxs-lookup"><span data-stu-id="8a386-184">Click **Select File** and upload your downloaded metadata file.</span></span>

    <span data-ttu-id="8a386-185">b.</span><span class="sxs-lookup"><span data-stu-id="8a386-185">b.</span></span> <span data-ttu-id="8a386-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a386-186">Click **Save** button.</span></span>
    


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8a386-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8a386-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="8a386-188">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8a386-188">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8a386-190">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8a386-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8a386-191">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8a386-191">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8a386-193">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="8a386-193">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8a386-195">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a386-195">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8a386-197">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8a386-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8a386-199">a.</span><span class="sxs-lookup"><span data-stu-id="8a386-199">a.</span></span> <span data-ttu-id="8a386-200">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8a386-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8a386-201">b.</span><span class="sxs-lookup"><span data-stu-id="8a386-201">b.</span></span> <span data-ttu-id="8a386-202">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8a386-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8a386-203">c.</span><span class="sxs-lookup"><span data-stu-id="8a386-203">c.</span></span> <span data-ttu-id="8a386-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8a386-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8a386-205">d.</span><span class="sxs-lookup"><span data-stu-id="8a386-205">d.</span></span> <span data-ttu-id="8a386-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8a386-206">Click **Create**.</span></span> 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a><span data-ttu-id="8a386-207">Vytvoření zkušebního uživatele Zscaler privátní přístup (ZPA)</span><span class="sxs-lookup"><span data-stu-id="8a386-207">Creating a Zscaler Private Access (ZPA) test user</span></span>

<span data-ttu-id="8a386-208">V této části vytvoříte uživatele názvem Britta Simon v Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="8a386-208">In this section, you create a user called Britta Simon in Zscaler Private Access (ZPA).</span></span> <span data-ttu-id="8a386-209">Spojte se s [tým podpory Zscaler privátní přístup (ZPA)](https://help.zscaler.com/zpa-submit-ticket) tooadd hello uživatelé v platformě hello Zscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="8a386-209">Please work with [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) tooadd hello users in hello Zscaler Private Access (ZPA) platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8a386-210">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8a386-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8a386-211">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte svůj přístup tooZscaler privátní přístup (ZPA).</span><span class="sxs-lookup"><span data-stu-id="8a386-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooZscaler Private Access (ZPA).</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8a386-213">**tooassign tooZscaler Britta Simon privátní přístup (ZPA), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8a386-213">**tooassign Britta Simon tooZscaler Private Access (ZPA), perform hello following steps:**</span></span>

1. <span data-ttu-id="8a386-214">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8a386-214">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8a386-216">V seznamu aplikace hello vyberte **Zscaler privátní přístup (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="8a386-216">In hello applications list, select **Zscaler Private Access (ZPA)**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. <span data-ttu-id="8a386-218">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8a386-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8a386-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8a386-220">Click **Add** button.</span></span> <span data-ttu-id="8a386-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a386-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8a386-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8a386-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8a386-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a386-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8a386-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8a386-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="8a386-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8a386-226">Testing single sign-on</span></span>

<span data-ttu-id="8a386-227">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8a386-227">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8a386-228">Když kliknete na dlaždici hello Zscaler privátní přístup (ZPA) v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Zscaler privátní přístup (ZPA) aplikace.</span><span class="sxs-lookup"><span data-stu-id="8a386-228">When you click hello Zscaler Private Access (ZPA) tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler Private Access (ZPA) application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="8a386-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8a386-229">Additional resources</span></span>

* [<span data-ttu-id="8a386-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a386-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8a386-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8a386-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png