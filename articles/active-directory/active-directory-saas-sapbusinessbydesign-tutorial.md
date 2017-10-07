---
title: 'Kurz: Azure Active Directory integrace s SAP Business ByDesign | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP Business ByDesign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: c14714fd27f8d7fc555f25c7be83fad2b0d7f333
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a><span data-ttu-id="71466-103">Kurz: Integrace Azure Active Directory se službou SAP Business ByDesign</span><span class="sxs-lookup"><span data-stu-id="71466-103">Tutorial: Azure Active Directory integration with SAP Business ByDesign</span></span>

<span data-ttu-id="71466-104">V tomto kurzu zjistíte, jak toointegrate SAP Business ByDesign službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="71466-104">In this tutorial, you learn how toointegrate SAP Business ByDesign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="71466-105">Integrace SAP Business ByDesign s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="71466-105">Integrating SAP Business ByDesign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="71466-106">Můžete ovládat ve službě Azure AD, který má přístup tooSAP ByDesign firmy.</span><span class="sxs-lookup"><span data-stu-id="71466-106">You can control in Azure AD who has access tooSAP Business ByDesign.</span></span>
- <span data-ttu-id="71466-107">Vaši uživatelé tooautomatically get přihlášeného tooSAP obchodní ByDesign (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="71466-107">You can enable your users tooautomatically get signed-on tooSAP Business ByDesign (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="71466-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="71466-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="71466-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="71466-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71466-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="71466-110">Prerequisites</span></span>

<span data-ttu-id="71466-111">Integrace služby Azure AD s SAP Business ByDesign tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="71466-111">tooconfigure Azure AD integration with SAP Business ByDesign, you need hello following items:</span></span>

- <span data-ttu-id="71466-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="71466-112">An Azure AD subscription</span></span>
- <span data-ttu-id="71466-113">SAP Business ByDesign jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="71466-113">An SAP Business ByDesign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="71466-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="71466-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="71466-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="71466-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="71466-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="71466-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="71466-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="71466-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="71466-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="71466-118">Scenario description</span></span>
<span data-ttu-id="71466-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="71466-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="71466-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="71466-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="71466-121">Přidání SAP Business ByDesign z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="71466-121">Adding SAP Business ByDesign from hello gallery</span></span>
2. <span data-ttu-id="71466-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="71466-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-business-bydesign-from-hello-gallery"></a><span data-ttu-id="71466-123">Přidání SAP Business ByDesign z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="71466-123">Adding SAP Business ByDesign from hello gallery</span></span>
<span data-ttu-id="71466-124">tooconfigure hello integrace ByDesign obchodní SAP do Azure AD, je nutné tooadd SAP Business ByDesign hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="71466-124">tooconfigure hello integration of SAP Business ByDesign into Azure AD, you need tooadd SAP Business ByDesign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="71466-125">**tooadd SAP Business ByDesign z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="71466-125">**tooadd SAP Business ByDesign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="71466-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="71466-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="71466-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="71466-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="71466-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="71466-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="71466-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="71466-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="71466-133">Hello vyhledávacího pole zadejte **SAP Business ByDesign**, vyberte **SAP Business ByDesign** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="71466-133">In hello search box, type **SAP Business ByDesign**, select **SAP Business ByDesign** from result panel then click **Add** button tooadd hello application.</span></span>

    ![SAP Business ByDesign v seznamu výsledků hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="71466-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="71466-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="71466-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP Business ByDesign podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="71466-136">In this section, you configure and test Azure AD single sign-on with SAP Business ByDesign based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="71466-137">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v SAP Business ByDesign je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="71466-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP Business ByDesign is tooa user in Azure AD.</span></span> <span data-ttu-id="71466-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SAP Business ByDesign musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="71466-138">In other words, a link relationship between an Azure AD user and hello related user in SAP Business ByDesign needs toobe established.</span></span>

<span data-ttu-id="71466-139">V SAP Business ByDesign, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="71466-139">In SAP Business ByDesign, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="71466-140">tooconfigure a testu Azure AD jednotné přihlašování s SAP Business ByDesign, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="71466-140">tooconfigure and test Azure AD single sign-on with SAP Business ByDesign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="71466-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="71466-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="71466-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="71466-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="71466-143">**[Vytvořit testovací uživatele s SAP Business ByDesign](#create-an-sap-business-bydesign-test-user)**  -toohave protějšek Britta Simon v ByDesign SAP Business, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="71466-143">**[Create an SAP Business ByDesign test user](#create-an-sap-business-bydesign-test-user)** - toohave a counterpart of Britta Simon in SAP Business ByDesign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="71466-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="71466-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="71466-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="71466-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="71466-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="71466-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="71466-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="71466-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP Business ByDesign application.</span></span>

<span data-ttu-id="71466-148">**tooconfigure Azure AD jednotné přihlašování s SAP Business ByDesign, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="71466-148">**tooconfigure Azure AD single sign-on with SAP Business ByDesign, perform hello following steps:**</span></span>

1. <span data-ttu-id="71466-149">V portálu Azure, na hello hello **SAP Business ByDesign** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="71466-149">In hello Azure portal, on hello **SAP Business ByDesign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="71466-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="71466-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. <span data-ttu-id="71466-153">Na hello **SAP Business ByDesign domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="71466-153">On hello **SAP Business ByDesign Domain and URLs** section, perform hello following steps:</span></span>

    ![SAP Business ByDesign domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    <span data-ttu-id="71466-155">a.</span><span class="sxs-lookup"><span data-stu-id="71466-155">a.</span></span> <span data-ttu-id="71466-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="71466-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<servername>.sapbydesign.com`</span></span>

    <span data-ttu-id="71466-157">b.</span><span class="sxs-lookup"><span data-stu-id="71466-157">b.</span></span> <span data-ttu-id="71466-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="71466-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<servername>.sapbydesign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="71466-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="71466-159">These values are not real.</span></span> <span data-ttu-id="71466-160">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="71466-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="71466-161">Obraťte se na [tým podpory SAP Business ByDesign klienta](https://www.sap.com/products/cloud-analytics.support.html) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="71466-161">Contact [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) tooget these values.</span></span>

4. <span data-ttu-id="71466-162">Na hello **uživatelské atributy** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="71466-162">On hello **User Attributes** section, perform hello following steps:</span></span>

    ![SAP Business ByDesign atribut části](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    <span data-ttu-id="71466-164">a.</span><span class="sxs-lookup"><span data-stu-id="71466-164">a.</span></span> <span data-ttu-id="71466-165">V **uživatelský identifikátor** seznamu, vyberte hello **ExtractMailPrefix()** funkce.</span><span class="sxs-lookup"><span data-stu-id="71466-165">In **User Identifier** list, select hello **ExtractMailPrefix()** function.</span></span>
    
    <span data-ttu-id="71466-166">b.</span><span class="sxs-lookup"><span data-stu-id="71466-166">b.</span></span> <span data-ttu-id="71466-167">Z hello **e-mailu** seznamu, vyberte hello atribut uživatele chcete toouse týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="71466-167">From hello **Mail** list, select hello user attribute you want toouse for your implementation.</span></span> <span data-ttu-id="71466-168">Například pokud chcete, aby toouse hello EmployeeID jako jedinečný identifikátor uživatele a hodnota atributu hello jsou uloženy v hello ExtensionAttribute2, pak vyberte user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="71466-168">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select user.extensionattribute2.</span></span>   

5. <span data-ttu-id="71466-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="71466-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. <span data-ttu-id="71466-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="71466-171">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="71466-173">Na hello **SAP Business ByDesign konfigurace** klikněte na tlačítko **konfigurace SAP Business ByDesign** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="71466-173">On hello **SAP Business ByDesign Configuration** section, click **Configure SAP Business ByDesign** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="71466-174">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="71466-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![SAP Business ByDesign konfigurace](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. <span data-ttu-id="71466-176">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="71466-176">tooget SSO configured for your application, perform hello following steps:</span></span>
   
    <span data-ttu-id="71466-177">a.</span><span class="sxs-lookup"><span data-stu-id="71466-177">a.</span></span> <span data-ttu-id="71466-178">Přihlaste na portál SAP Business ByDesign tooyour s právy správce.</span><span class="sxs-lookup"><span data-stu-id="71466-178">Sign on tooyour SAP Business ByDesign portal with administrator rights.</span></span>
   
    <span data-ttu-id="71466-179">b.</span><span class="sxs-lookup"><span data-stu-id="71466-179">b.</span></span> <span data-ttu-id="71466-180">Přejděte příliš**aplikace a běžné úlohy správy uživatele** a klikněte na tlačítko hello **zprostředkovatele Identity** kartě.</span><span class="sxs-lookup"><span data-stu-id="71466-180">Navigate too**Application and User Management Common Task** and click hello **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="71466-181">c.</span><span class="sxs-lookup"><span data-stu-id="71466-181">c.</span></span> <span data-ttu-id="71466-182">Klikněte na tlačítko **nového poskytovatele Identity** a vyberte hello metadata XML soubor, který jste si stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="71466-182">Click **New Identity Provider** and select hello metadata XML file that you have downloaded from hello Azure portal.</span></span> <span data-ttu-id="71466-183">Importováním hello metadata hello systému automaticky nahrává hello dodejka certifikát a certifikát pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="71466-183">By importing hello metadata, hello system automatically uploads hello required signature certificate and encryption certificate.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    <span data-ttu-id="71466-185">d.</span><span class="sxs-lookup"><span data-stu-id="71466-185">d.</span></span> <span data-ttu-id="71466-186">tooinclude hello **adresa URL služby příjemce Assertion** do hello žádost SAML, vyberte **zahrnují Assertion příjemce adresa URL služby**.</span><span class="sxs-lookup"><span data-stu-id="71466-186">tooinclude hello **Assertion Consumer Service URL** into hello SAML request, select **Include Assertion Consumer Service URL**.</span></span>
   
    <span data-ttu-id="71466-187">e.</span><span class="sxs-lookup"><span data-stu-id="71466-187">e.</span></span> <span data-ttu-id="71466-188">Klikněte na tlačítko **aktivovat jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="71466-188">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="71466-189">f.</span><span class="sxs-lookup"><span data-stu-id="71466-189">f.</span></span> <span data-ttu-id="71466-190">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="71466-190">Save your changes.</span></span>
   
    <span data-ttu-id="71466-191">g.</span><span class="sxs-lookup"><span data-stu-id="71466-191">g.</span></span> <span data-ttu-id="71466-192">Klikněte na tlačítko hello **Moje systému** kartě.</span><span class="sxs-lookup"><span data-stu-id="71466-192">Click hello **My System** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    <span data-ttu-id="71466-194">h.</span><span class="sxs-lookup"><span data-stu-id="71466-194">h.</span></span> <span data-ttu-id="71466-195">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portál Azure ho do hello **Azure AD adresa URL přihlašování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="71466-195">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal it into hello **Azure AD Sign On URL** textbox.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    <span data-ttu-id="71466-197">i.</span><span class="sxs-lookup"><span data-stu-id="71466-197">i.</span></span> <span data-ttu-id="71466-198">Zadejte, zda zaměstnanec hello ručně vybrat mezi přihlásit pomocí ID uživatele a heslo nebo jednotného přihlašování k výběrem **výběr zprostředkovatele Identity ruční**.</span><span class="sxs-lookup"><span data-stu-id="71466-198">Specify whether hello employee can manually choose between logging on with user ID and password or SSO by selecting **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="71466-199">j.</span><span class="sxs-lookup"><span data-stu-id="71466-199">j.</span></span> <span data-ttu-id="71466-200">V hello **jednotného přihlašování k adrese URL** části, zadejte adresu URL hello, který má být používána hello zaměstnanec toologon toohello systému.</span><span class="sxs-lookup"><span data-stu-id="71466-200">In hello **SSO URL** section, specify hello URL that should be used by hello employee toologon toohello system.</span></span> 
    <span data-ttu-id="71466-201">Hello odeslané na adresu URL tooEmployee rozevíracího seznamu můžete zvolit hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="71466-201">In hello URL Sent tooEmployee dropdown list, you can choose between hello following options:</span></span>
   
    <span data-ttu-id="71466-202">**Adresa URL jednotného přihlašování**</span><span class="sxs-lookup"><span data-stu-id="71466-202">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="71466-203">systém Hello odešle jenom hello normální systému URL toohello zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="71466-203">hello system sends only hello normal system URL toohello employee.</span></span> <span data-ttu-id="71466-204">Zaměstnanec Hello nelze přihlášení pomocí jednotného přihlašování a musí používat heslo nebo místo toho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="71466-204">hello employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="71466-205">**ADRESA URL JEDNOTNÉHO PŘIHLAŠOVÁNÍ**</span><span class="sxs-lookup"><span data-stu-id="71466-205">**SSO URL**</span></span> 
   
    <span data-ttu-id="71466-206">systém Hello odešle jenom hello zaměstnanec toohello jednotného přihlašování k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="71466-206">hello system sends only hello SSO URL toohello employee.</span></span> <span data-ttu-id="71466-207">Hello zaměstnanců může přihlásit pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="71466-207">hello employee can log on using SSO.</span></span> <span data-ttu-id="71466-208">Prostřednictvím hello IdP přesměruje požadavek na ověření.</span><span class="sxs-lookup"><span data-stu-id="71466-208">Authentication request is redirected through hello IdP.</span></span>
   
    <span data-ttu-id="71466-209">**Automatický výběr**</span><span class="sxs-lookup"><span data-stu-id="71466-209">**Automatic Selection**</span></span>
   
    <span data-ttu-id="71466-210">Pokud jednotného přihlašování není aktivní, odešle hello systému hello normální systému URL toohello zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="71466-210">If SSO is not active, hello system sends hello normal system URL toohello employee.</span></span> <span data-ttu-id="71466-211">Pokud je aktivní jednotné přihlašování, hello systému zkontroluje, zda hello zaměstnanec má heslo.</span><span class="sxs-lookup"><span data-stu-id="71466-211">If SSO is active, hello system checks whether hello employee has a password.</span></span> <span data-ttu-id="71466-212">Pokud heslo je k dispozici, jednotného přihlašování k adrese URL a adresy URL bez jednotného přihlašování k odešlou toohello zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="71466-212">If a password is available, both SSO URL and Non-SSO URL are sent toohello employee.</span></span> <span data-ttu-id="71466-213">Ale pokud zaměstnanec hello nemá žádné heslo, pouze hello URL jednotného přihlašování k odešle toohello zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="71466-213">However, if hello employee has no password, only hello SSO URL is sent toohello employee.</span></span>
   
    <span data-ttu-id="71466-214">kB.</span><span class="sxs-lookup"><span data-stu-id="71466-214">k.</span></span> <span data-ttu-id="71466-215">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="71466-215">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="71466-216">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="71466-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="71466-217">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="71466-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="71466-218">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="71466-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="71466-219">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="71466-219">Create an Azure AD test user</span></span>

<span data-ttu-id="71466-220">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="71466-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="71466-222">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="71466-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="71466-223">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="71466-223">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="71466-225">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="71466-225">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="71466-227">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="71466-227">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="71466-229">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="71466-229">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    <span data-ttu-id="71466-231">a.</span><span class="sxs-lookup"><span data-stu-id="71466-231">a.</span></span> <span data-ttu-id="71466-232">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="71466-232">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="71466-233">b.</span><span class="sxs-lookup"><span data-stu-id="71466-233">b.</span></span> <span data-ttu-id="71466-234">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="71466-234">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="71466-235">c.</span><span class="sxs-lookup"><span data-stu-id="71466-235">c.</span></span> <span data-ttu-id="71466-236">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="71466-236">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="71466-237">d.</span><span class="sxs-lookup"><span data-stu-id="71466-237">d.</span></span> <span data-ttu-id="71466-238">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="71466-238">Click **Create**.</span></span>
 
### <a name="create-an-sap-business-bydesign-test-user"></a><span data-ttu-id="71466-239">Vytvořit uživatele s SAP Business ByDesign testu</span><span class="sxs-lookup"><span data-stu-id="71466-239">Create an SAP Business ByDesign test user</span></span>

<span data-ttu-id="71466-240">V této části vytvoříte uživatele volal Britta Simon v SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="71466-240">In this section, you create a user called Britta Simon in SAP Business ByDesign.</span></span> <span data-ttu-id="71466-241">Spojte se s [tým podpory SAP Business ByDesign klienta](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello uživatelé v platformě SAP Business ByDesign hello.</span><span class="sxs-lookup"><span data-stu-id="71466-241">Please work with [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello users in hello SAP Business ByDesign platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="71466-242">Zkontrolujte, zda hodnota NameID by měl odpovídat hello pole pro uživatelské jméno v platformě SAP Business ByDesign hello.</span><span class="sxs-lookup"><span data-stu-id="71466-242">Please make sure that NameID value should match with hello username field in hello SAP Business ByDesign platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="71466-243">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="71466-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="71466-244">V této části povolíte tak, že udělíte přístup tooSAP obchodní ByDesign toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="71466-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP Business ByDesign.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="71466-246">**tooassign tooSAP Britta Simon obchodní ByDesign, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="71466-246">**tooassign Britta Simon tooSAP Business ByDesign, perform hello following steps:**</span></span>

1. <span data-ttu-id="71466-247">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="71466-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="71466-249">V seznamu aplikace hello vyberte **SAP Business ByDesign**.</span><span class="sxs-lookup"><span data-stu-id="71466-249">In hello applications list, select **SAP Business ByDesign**.</span></span>

    ![Hello SAP Business ByDesign odkaz v seznamu aplikace hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. <span data-ttu-id="71466-251">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="71466-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="71466-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="71466-253">Click **Add** button.</span></span> <span data-ttu-id="71466-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="71466-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="71466-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="71466-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="71466-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="71466-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="71466-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="71466-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="71466-259">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="71466-259">Test single sign-on</span></span>

<span data-ttu-id="71466-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="71466-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="71466-261">Když kliknete na dlaždici SAP Business ByDesign hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour ByDesign SAP obchodní aplikace.</span><span class="sxs-lookup"><span data-stu-id="71466-261">When you click hello SAP Business ByDesign tile in hello Access Panel, you should get automatically signed-on tooyour SAP Business ByDesign application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71466-262">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="71466-262">Additional resources</span></span>

* [<span data-ttu-id="71466-263">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="71466-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="71466-264">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="71466-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

