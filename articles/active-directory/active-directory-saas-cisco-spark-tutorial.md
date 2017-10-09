---
title: 'Kurz: Azure Active Directory integrace s Cisco Spark | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Cisco Spark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 386c4fd816095e1c61de01dd1dee1bbd00311a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a><span data-ttu-id="ca6df-103">Kurz: Azure Active Directory integrace s Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="ca6df-103">Tutorial: Azure Active Directory integration with Cisco Spark</span></span>

<span data-ttu-id="ca6df-104">V tomto kurzu zjistíte, jak toointegrate Cisco Spark v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ca6df-104">In this tutorial, you learn how toointegrate Cisco Spark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ca6df-105">Integrace Cisco Spark s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ca6df-105">Integrating Cisco Spark with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ca6df-106">Můžete řídit ve službě Azure AD, který má přístup tooCisco Spark</span><span class="sxs-lookup"><span data-stu-id="ca6df-106">You can control in Azure AD who has access tooCisco Spark</span></span>
- <span data-ttu-id="ca6df-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCisco Spark (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ca6df-107">You can enable your users tooautomatically get signed-on tooCisco Spark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ca6df-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ca6df-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ca6df-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ca6df-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca6df-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ca6df-110">Prerequisites</span></span>

<span data-ttu-id="ca6df-111">tooconfigure integrace Azure AD s Cisco Spark, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ca6df-111">tooconfigure Azure AD integration with Cisco Spark, you need hello following items:</span></span>

- <span data-ttu-id="ca6df-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ca6df-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ca6df-113">Cisco Spark jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ca6df-113">A Cisco Spark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ca6df-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ca6df-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ca6df-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ca6df-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ca6df-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ca6df-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ca6df-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca6df-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ca6df-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ca6df-118">Scenario description</span></span>
<span data-ttu-id="ca6df-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ca6df-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ca6df-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ca6df-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ca6df-121">Přidání Cisco Spark z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ca6df-121">Adding Cisco Spark from hello gallery</span></span>
2. <span data-ttu-id="ca6df-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ca6df-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cisco-spark-from-hello-gallery"></a><span data-ttu-id="ca6df-123">Přidání Cisco Spark z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ca6df-123">Adding Cisco Spark from hello gallery</span></span>
<span data-ttu-id="ca6df-124">tooconfigure hello integrace Spark Cisco do Azure AD, je nutné tooadd Cisco Spark hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ca6df-124">tooconfigure hello integration of Cisco Spark into Azure AD, you need tooadd Cisco Spark from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ca6df-125">**tooadd Spark Cisco z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ca6df-125">**tooadd Cisco Spark from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca6df-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ca6df-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ca6df-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ca6df-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ca6df-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ca6df-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ca6df-133">Hello vyhledávacího pole zadejte **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-133">In hello search box, type **Cisco Spark**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. <span data-ttu-id="ca6df-135">Na panelu výsledků hello vyberte **Cisco Spark**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca6df-135">In hello results panel, select **Cisco Spark**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ca6df-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ca6df-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ca6df-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cisco Spark podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="ca6df-138">In this section, you configure and test Azure AD single sign-on with Cisco Spark based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ca6df-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Cisco Spark je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ca6df-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cisco Spark is tooa user in Azure AD.</span></span> <span data-ttu-id="ca6df-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Cisco Spark musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ca6df-140">In other words, a link relationship between an Azure AD user and hello related user in Cisco Spark needs toobe established.</span></span>

<span data-ttu-id="ca6df-141">V Cisco Spark přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="ca6df-141">In Cisco Spark, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ca6df-142">tooconfigure a testování Azure AD jednotné přihlašování s Cisco Spark, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ca6df-142">tooconfigure and test Azure AD single sign-on with Cisco Spark, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ca6df-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ca6df-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ca6df-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca6df-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ca6df-145">**[Vytvoření zkušebního uživatele Cisco Spark](#creating-a-cisco-spark-test-user)**  -toohave protějšek Britta Simon v Cisco Spark, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ca6df-145">**[Creating a Cisco Spark test user](#creating-a-cisco-spark-test-user)** - toohave a counterpart of Britta Simon in Cisco Spark that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ca6df-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ca6df-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ca6df-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ca6df-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ca6df-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ca6df-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ca6df-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="ca6df-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cisco Spark application.</span></span>

<span data-ttu-id="ca6df-150">**tooconfigure Azure AD jednotné přihlašování s Cisco Spark, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ca6df-150">**tooconfigure Azure AD single sign-on with Cisco Spark, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca6df-151">V portálu Azure, na hello hello **Cisco Spark** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-151">In hello Azure portal, on hello **Cisco Spark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ca6df-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ca6df-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. <span data-ttu-id="ca6df-155">Na hello **Cisco Spark domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ca6df-155">On hello **Cisco Spark Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    <span data-ttu-id="ca6df-157">a.</span><span class="sxs-lookup"><span data-stu-id="ca6df-157">a.</span></span> <span data-ttu-id="ca6df-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://web.ciscospark.com/#/signin`</span><span class="sxs-lookup"><span data-stu-id="ca6df-158">In hello **Sign-on URL** textbox, type a URL as: `https://web.ciscospark.com/#/signin`</span></span>

    <span data-ttu-id="ca6df-159">b.</span><span class="sxs-lookup"><span data-stu-id="ca6df-159">b.</span></span> <span data-ttu-id="ca6df-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://idbroker.webex.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="ca6df-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://idbroker.webex.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ca6df-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="ca6df-161">This value is not real.</span></span> <span data-ttu-id="ca6df-162">Aktualizujte tuto hodnotu s hello skutečné identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ca6df-162">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="ca6df-163">Obraťte se na [tým podpory Cisco Spark klienta](https://support.ciscospark.com/) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ca6df-163">Contact [Cisco Spark Client support team](https://support.ciscospark.com/) tooget this value.</span></span> 
 
4. <span data-ttu-id="ca6df-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ca6df-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. <span data-ttu-id="ca6df-166">Aplikací Spark Cisco očekává hello SAML kontrolní výrazy toocontain konkrétní atributy.</span><span class="sxs-lookup"><span data-stu-id="ca6df-166">Cisco Spark application expects hello SAML assertions toocontain specific attributes.</span></span> <span data-ttu-id="ca6df-167">Nakonfigurujte hello následující atributy pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ca6df-167">Configure hello following attributes  for this application.</span></span> <span data-ttu-id="ca6df-168">Můžete spravovat hello hodnoty těchto atributů z hello **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca6df-168">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="ca6df-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="ca6df-169">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. <span data-ttu-id="ca6df-171">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ca6df-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="ca6df-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="ca6df-172">Attribute Name</span></span>  | <span data-ttu-id="ca6df-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="ca6df-173">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    |   <span data-ttu-id="ca6df-174">UID</span><span class="sxs-lookup"><span data-stu-id="ca6df-174">uid</span></span>    | <span data-ttu-id="ca6df-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="ca6df-175">user.userprincipalname</span></span> |   

    <span data-ttu-id="ca6df-176">a.</span><span class="sxs-lookup"><span data-stu-id="ca6df-176">a.</span></span> <span data-ttu-id="ca6df-177">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ca6df-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="ca6df-180">b.</span><span class="sxs-lookup"><span data-stu-id="ca6df-180">b.</span></span> <span data-ttu-id="ca6df-181">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="ca6df-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="ca6df-182">c.</span><span class="sxs-lookup"><span data-stu-id="ca6df-182">c.</span></span> <span data-ttu-id="ca6df-183">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="ca6df-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="ca6df-184">d.</span><span class="sxs-lookup"><span data-stu-id="ca6df-184">d.</span></span> <span data-ttu-id="ca6df-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-185">Click **Ok**.</span></span>

7. <span data-ttu-id="ca6df-186">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ca6df-186">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="ca6df-188">Přihlaste se příliš[správu spolupráce cloudu Cisco](https://admin.ciscospark.com/) pomocí svých přihlašovacích údajů správce s úplnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="ca6df-188">Sign in too[Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

9. <span data-ttu-id="ca6df-189">Vyberte **nastavení** a v části hello **ověřování** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-189">Select **Settings** and under hello **Authentication** section, click **Modify**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. <span data-ttu-id="ca6df-191">Vyberte **integrovat zprostředkovatele identity 3. stran. (Rozšířené)**  a přejděte toohello další obrazovce.</span><span class="sxs-lookup"><span data-stu-id="ca6df-191">Select **Integrate a 3rd-party identity provider. (Advanced)** and go toohello next screen.</span></span>

11. <span data-ttu-id="ca6df-192">Na hello **importovat Idp Metadata** stránky, buď přetáhněte soubor metadat hello Azure AD na stránku hello nebo použít hello souboru prohlížeče možnost toolocate a nahrát soubor metadat hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ca6df-192">On hello **Import Idp Metadata** page, either drag and drop hello Azure AD metadata file onto hello page or use hello file browser option toolocate and upload hello Azure AD metadata file.</span></span> <span data-ttu-id="ca6df-193">Pak vyberte **vyžadovat certifikát podepsaný certifikační autoritou v metadatech (bezpečnější)** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-193">Then, select **Require certificate signed by a certificate authority in Metadata (more secure)** and click **Next**.</span></span> 
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. <span data-ttu-id="ca6df-195">Vyberte **Test jednotného přihlašování k připojení**a když se otevře novou kartu prohlížeče, ověřování s Azure AD po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ca6df-195">Select **Test SSO Connection**, and when a new browser tab opens, authenticate with Azure AD by signing in.</span></span>

13. <span data-ttu-id="ca6df-196">Vrátí toohello **správu spolupráce cloudu Cisco** kartu prohlížeče. Pokud byl hello test úspěšný, vyberte **tento test proběhl úspěšně. Možnost jednotného přihlašování** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-196">Return toohello **Cisco Cloud Collaboration Management** browser tab. If hello test was successful, select **This test was successful. Enable Single Sign-On option** and click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="ca6df-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ca6df-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ca6df-198">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ca6df-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ca6df-199">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ca6df-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ca6df-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ca6df-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="ca6df-201">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ca6df-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ca6df-203">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ca6df-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca6df-204">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ca6df-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ca6df-206">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ca6df-208">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="ca6df-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ca6df-210">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ca6df-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ca6df-212">a.</span><span class="sxs-lookup"><span data-stu-id="ca6df-212">a.</span></span> <span data-ttu-id="ca6df-213">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ca6df-214">b.</span><span class="sxs-lookup"><span data-stu-id="ca6df-214">b.</span></span> <span data-ttu-id="ca6df-215">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ca6df-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ca6df-216">c.</span><span class="sxs-lookup"><span data-stu-id="ca6df-216">c.</span></span> <span data-ttu-id="ca6df-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ca6df-218">d.</span><span class="sxs-lookup"><span data-stu-id="ca6df-218">d.</span></span> <span data-ttu-id="ca6df-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-219">Click **Create**.</span></span>
 
### <a name="creating-a-cisco-spark-test-user"></a><span data-ttu-id="ca6df-220">Vytvoření zkušebního uživatele Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="ca6df-220">Creating a Cisco Spark test user</span></span>

<span data-ttu-id="ca6df-221">V této části vytvoříte uživatele volal Britta Simon v Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="ca6df-221">In this section, you create a user called Britta Simon in Cisco Spark.</span></span> <span data-ttu-id="ca6df-222">V této části vytvoříte uživatele volal Britta Simon v Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="ca6df-222">In this section, you create a user called Britta Simon in Cisco Spark.</span></span>

1. <span data-ttu-id="ca6df-223">Přejděte toohello [správu spolupráce cloudu Cisco](https://admin.ciscospark.com/) pomocí svých přihlašovacích údajů správce s úplnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="ca6df-223">Go toohello [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

2. <span data-ttu-id="ca6df-224">Klikněte na tlačítko **uživatelé** a potom **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-224">Click **Users** and then **Manage Users**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. <span data-ttu-id="ca6df-226">V hello **spravovat uživatelská** vyberte **ručně přidat nebo změnit uživatele** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-226">In hello **Manage User** window, select **Manually add or modify users** and click **Next**.</span></span>

4. <span data-ttu-id="ca6df-227">Vyberte **názvy a e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-227">Select **Names and Email address**.</span></span> <span data-ttu-id="ca6df-228">Potom vyplňte hello textbox následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ca6df-228">Then, fill out hello textbox as follows:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    <span data-ttu-id="ca6df-230">a.</span><span class="sxs-lookup"><span data-stu-id="ca6df-230">a.</span></span> <span data-ttu-id="ca6df-231">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-231">In hello **First Name** textbox, type **Britta**.</span></span> 
    
    <span data-ttu-id="ca6df-232">b.</span><span class="sxs-lookup"><span data-stu-id="ca6df-232">b.</span></span> <span data-ttu-id="ca6df-233">V hello **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-233">In hello **Last Name** textbox, type **Simon**.</span></span>
    
    <span data-ttu-id="ca6df-234">c.</span><span class="sxs-lookup"><span data-stu-id="ca6df-234">c.</span></span> <span data-ttu-id="ca6df-235">V hello **e-mailová adresa** textovému poli, typ  **britta.simon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="ca6df-235">In hello **Email address** textbox, type **britta.simon@contoso.com**.</span></span>

5. <span data-ttu-id="ca6df-236">Klikněte na tlačítko hello plus tooadd Britta Simon přihlásit.</span><span class="sxs-lookup"><span data-stu-id="ca6df-236">Click hello plus sign tooadd Britta Simon.</span></span> <span data-ttu-id="ca6df-237">Pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-237">Then, click **Next**.</span></span>

6. <span data-ttu-id="ca6df-238">V hello **přidat služby pro uživatele** okně klikněte na tlačítko **Uložit** a potom **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-238">In hello **Add Services for Users** window, click **Save** and then **Finish**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ca6df-239">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ca6df-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ca6df-240">V této části povolíte tak, že udělíte přístup tooCisco Spark Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ca6df-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCisco Spark.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ca6df-242">**tooassign tooCisco Britta Simon Spark, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ca6df-242">**tooassign Britta Simon tooCisco Spark, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca6df-243">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ca6df-245">V seznamu aplikace hello vyberte **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-245">In hello applications list, select **Cisco Spark**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. <span data-ttu-id="ca6df-247">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ca6df-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ca6df-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ca6df-249">Click **Add** button.</span></span> <span data-ttu-id="ca6df-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ca6df-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ca6df-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ca6df-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ca6df-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ca6df-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ca6df-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ca6df-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ca6df-255">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ca6df-255">Testing single sign-on</span></span>

<span data-ttu-id="ca6df-256">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="ca6df-256">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="ca6df-257">Když kliknete na dlaždici hello Cisco Spark v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Cisco Spark aplikace.</span><span class="sxs-lookup"><span data-stu-id="ca6df-257">When you click hello Cisco Spark tile in hello Access Panel, you should get automatically signed-on tooyour Cisco Spark application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ca6df-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ca6df-258">Additional resources</span></span>

* [<span data-ttu-id="ca6df-259">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ca6df-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ca6df-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ca6df-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

