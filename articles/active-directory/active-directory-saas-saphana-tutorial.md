---
title: 'Kurz: Azure Active Directory integrace s SAP HANA | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP HANA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 5ff6bfde0b1d7ab14025a4bc45199f9f826affd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a><span data-ttu-id="2c179-103">Kurz: Azure Active Directory integrace s SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2c179-103">Tutorial: Azure Active Directory integration with SAP HANA</span></span>

<span data-ttu-id="2c179-104">V tomto kurzu zjistíte, jak toointegrate SAP HANA službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2c179-104">In this tutorial, you learn how toointegrate SAP HANA with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2c179-105">Integrace SAP HANA s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2c179-105">Integrating SAP HANA with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2c179-106">Můžete řídit ve službě Azure AD, který má přístup tooSAP HANA</span><span class="sxs-lookup"><span data-stu-id="2c179-106">You can control in Azure AD who has access tooSAP HANA</span></span>
- <span data-ttu-id="2c179-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSAP HANA (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c179-107">You can enable your users tooautomatically get signed-on tooSAP HANA (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2c179-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2c179-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2c179-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2c179-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c179-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2c179-110">Prerequisites</span></span>

<span data-ttu-id="2c179-111">Integrace služby Azure AD s SAP HANA tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2c179-111">tooconfigure Azure AD integration with SAP HANA, you need hello following items:</span></span>

- <span data-ttu-id="2c179-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c179-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2c179-113">SAP HANA jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2c179-113">A SAP HANA single sign-on enabled subscription</span></span>
- <span data-ttu-id="2c179-114">Spuštěné HANA Instance buď na všechny veřejné IaaS, místní, virtuální počítače Azure nebo velké instance SAP v Azure</span><span class="sxs-lookup"><span data-stu-id="2c179-114">A running HANA Instance either on any public IaaS, on-premises, Azure VMs or SAP Large Instances in Azure</span></span>
- <span data-ttu-id="2c179-115">Hello XSA správy webové rozhraní, jakož i HANA Studio nainstalována na hello HANA instance.</span><span class="sxs-lookup"><span data-stu-id="2c179-115">hello XSA Administration Web Interface as well as HANA Studio installed on hello HANA instance.</span></span>

> [!NOTE]
> <span data-ttu-id="2c179-116">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2c179-116">tootest hello steps in this tutorial, we do not recommend using a production environment of SAP HANA.</span></span> <span data-ttu-id="2c179-117">Integrace hello se nejdřív otestujte v vývoj nebo pracovní prostředí hello aplikace a pak použijte hello produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c179-117">Test hello integration first in development or staging environment of hello application and then use hello production environment.</span></span>

<span data-ttu-id="2c179-118">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2c179-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2c179-119">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2c179-119">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2c179-120">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2c179-120">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2c179-121">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2c179-121">Scenario description</span></span>
<span data-ttu-id="2c179-122">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c179-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2c179-123">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2c179-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2c179-124">Přidání SAP HANA z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2c179-124">Adding SAP HANA from hello gallery</span></span>
2. <span data-ttu-id="2c179-125">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c179-125">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-hana-from-hello-gallery"></a><span data-ttu-id="2c179-126">Přidání SAP HANA z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2c179-126">Adding SAP HANA from hello gallery</span></span>
<span data-ttu-id="2c179-127">tooconfigure hello integrace SAP HANA do Azure AD, je nutné tooadd SAP HANA hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2c179-127">tooconfigure hello integration of SAP HANA into Azure AD, you need tooadd SAP HANA from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2c179-128">**tooadd SAP HANA z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c179-128">**tooadd SAP HANA from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c179-129">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c179-129">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="2c179-131">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2c179-131">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2c179-132">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c179-132">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="2c179-134">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c179-134">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="2c179-136">Hello vyhledávacího pole zadejte **SAP HANA**, vyberte **SAP HANA** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c179-136">In hello search box, type **SAP HANA**, select **SAP HANA** from result panel then click **Add** button tooadd hello application.</span></span> 

    ![Hello nové aplikace](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2c179-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c179-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2c179-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SAP HANA podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2c179-139">In this section, you configure and test Azure AD single sign-on with SAP HANA based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2c179-140">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v SAP HANA je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c179-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP HANA is tooa user in Azure AD.</span></span> <span data-ttu-id="2c179-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SAP HANA musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2c179-141">In other words, a link relationship between an Azure AD user and hello related user in SAP HANA needs toobe established.</span></span>

<span data-ttu-id="2c179-142">V SAP HANA přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="2c179-142">In SAP HANA, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2c179-143">tooconfigure a testu Azure AD jednotné přihlašování s SAP HANA, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2c179-143">tooconfigure and test Azure AD single sign-on with SAP HANA, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2c179-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2c179-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2c179-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2c179-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2c179-146">**[Vytvoření zkušebního uživatele SAP HANA](#creating-a-sap-hana-test-user)**  -toohave protějšek Britta Simon v SAP HANA, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c179-146">**[Creating a SAP HANA test user](#creating-a-sap-hana-test-user)** - toohave a counterpart of Britta Simon in SAP HANA that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2c179-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c179-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2c179-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2c179-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2c179-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c179-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2c179-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2c179-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP HANA application.</span></span>

<span data-ttu-id="2c179-151">**tooconfigure Azure AD jednotné přihlašování s SAP HANA, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c179-151">**tooconfigure Azure AD single sign-on with SAP HANA, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c179-152">V portálu Azure, na hello hello **SAP HANA** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2c179-152">In hello Azure portal, on hello **SAP HANA** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2c179-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c179-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. <span data-ttu-id="2c179-156">Na hello **SAP HANA domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2c179-156">On hello **SAP HANA Domain and URLs** section, perform hello following steps:</span></span>

    ![Domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    <span data-ttu-id="2c179-158">a.</span><span class="sxs-lookup"><span data-stu-id="2c179-158">a.</span></span> <span data-ttu-id="2c179-159">V hello **identifikátor** textovému poli, typ jako:`HA100`</span><span class="sxs-lookup"><span data-stu-id="2c179-159">In hello **Identifier** textbox, type as: `HA100`</span></span> 

    <span data-ttu-id="2c179-160">b.</span><span class="sxs-lookup"><span data-stu-id="2c179-160">b.</span></span> <span data-ttu-id="2c179-161">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`</span><span class="sxs-lookup"><span data-stu-id="2c179-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2c179-162">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2c179-162">These values are not real.</span></span> <span data-ttu-id="2c179-163">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2c179-163">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="2c179-164">Obraťte se na [tým podpory SAP HANA klienta](https://cloudplatform.sap.com/contact.html) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2c179-164">Contact [SAP HANA Client support team](https://cloudplatform.sap.com/contact.html) tooget these values.</span></span> 

4. <span data-ttu-id="2c179-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2c179-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    ><span data-ttu-id="2c179-167">Pokud certifikát není aktivní pak nastavte ji jako aktivní kliknutím hello "Zkontrolujte nový certifikát active" políčko v hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c179-167">If certificate is not active then make it active by clicking hello “Make new certificate active” checkbox in hello Azure AD.</span></span> 

5. <span data-ttu-id="2c179-168">Aplikace SAP HANA očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="2c179-168">SAP HANA application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2c179-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="2c179-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="2c179-170">Zde jsme jste namapovali hello **uživatelský identifikátor** s **ExtractMailPrefix()** funkce **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="2c179-170">Here we have mapped hello **User Identifier** with **ExtractMailPrefix()** function of **user.mail**.</span></span> <span data-ttu-id="2c179-171">Díky tomu hodnotu předpony hello e-mailů hello uživatele, který je hello jedinečné ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c179-171">This gives hello prefix value of email of hello user which is hello unique User ID.</span></span> <span data-ttu-id="2c179-172">Ta se odešle toohello SAP HANA aplikace v každé úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2c179-172">This is sent toohello SAP HANA application in every successful response.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. <span data-ttu-id="2c179-174">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2c179-174">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="2c179-175">a.</span><span class="sxs-lookup"><span data-stu-id="2c179-175">a.</span></span> <span data-ttu-id="2c179-176">V hello **uživatelský identifikátor** rozevíracího seznamu vyberte **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="2c179-176">In hello **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>
    
    <span data-ttu-id="2c179-177">b.</span><span class="sxs-lookup"><span data-stu-id="2c179-177">b.</span></span> <span data-ttu-id="2c179-178">V hello **e-mailu** rozevíracího seznamu vyberte **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="2c179-178">In hello **Mail** dropdown list, select **user.mail**.</span></span>

7. <span data-ttu-id="2c179-179">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c179-179">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="2c179-181">tooconfigure jednotného přihlašování na **SAP HANA** straně, přihlášení tooyour **HANA XSA webové konzole** procházením toohello příslušných koncový bod HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2c179-181">tooconfigure single sign-on on **SAP HANA** side, login tooyour **HANA XSA Web Console**  by browsing toohello respective HTTPS-endpoint.</span></span>

    > [!Note]
    > <span data-ttu-id="2c179-182">V konfiguraci hello výchozí adresa URL hello přesměruje hello požadavek tooa přihlašovací obrazovka, která vyžaduje hello přihlašovací údaje ověření SAP HANA databáze toocomplete hello procesem přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c179-182">In hello default configuration, hello URL redirects hello request tooa logon screen, which requires hello credentials of an authenticated SAP HANA database user toocomplete hello logon process.</span></span> <span data-ttu-id="2c179-183">úlohy správy hello oprávnění požadované tooperform SAML musí mít Hello přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c179-183">hello user who logs on must have hello privileges required tooperform SAML administration tasks.</span></span>

9. <span data-ttu-id="2c179-184">Hello XSA webové rozhraní, přejděte v příliš**poskytovatele Identity SAML** a z ní, klikněte na tlačítko hello **"+"** – tlačítko hello dolní části hello obrazovky toodisplay hello přidat informace o poskytovateli Identity podokna a provádění Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c179-184">In hello XSA Web Interface, navigate too**SAML Identity Provider** and from there, click hello **“+”** -button on hello bottom of hello screen toodisplay hello Add Identity Provider Info pane and perform hello following steps:</span></span>

    ![Přidejte zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap1.png)

    <span data-ttu-id="2c179-186">a.</span><span class="sxs-lookup"><span data-stu-id="2c179-186">a.</span></span> <span data-ttu-id="2c179-187">V hello **přidat informace o poskytovateli Identity** podokně, vložte obsah hello hello Metadata XML, který jste si stáhli z portálu Azure do hello **Metadata** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2c179-187">In hello **Add Identity Provider Info** pane, paste hello contents of hello Metadata XML, which you have downloaded from Azure portal into hello **Metadata** textbox.</span></span>

    ![Přidejte nastavení zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap2.png)

    <span data-ttu-id="2c179-189">b.</span><span class="sxs-lookup"><span data-stu-id="2c179-189">b.</span></span> <span data-ttu-id="2c179-190">Pokud hello obsah dokumentu XML hello platné, hello Analýza procesu extrahuje tooinsert hello požadované informace do hello **předmět, Entity ID a vystavitele** polí v hello obecné datové oblasti obrazovky a hello pole adresy URL v hello Cílová oblast obrazovky, například  **základní adresu URL a adresy URL SingleSignOn (*)**.</span><span class="sxs-lookup"><span data-stu-id="2c179-190">If hello contents of hello XML document are valid, hello parsing process extracts hello information required tooinsert into hello **Subject, Entity ID, and Issuer** fields in hello General Data screen area, and hello URL fields in hello Destination screen area, for example, **Base URL and SingleSignOn URL (*)**.</span></span>

    ![Přidejte nastavení zprostředkovatele Identity](./media/active-directory-saas-saphana-tutorial/sap3.png)

    <span data-ttu-id="2c179-192">c.</span><span class="sxs-lookup"><span data-stu-id="2c179-192">c.</span></span> <span data-ttu-id="2c179-193">V poli Název hello hello obecné Data obrazovky oblasti, zadejte název hello nového poskytovatele identity SAML jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c179-193">In hello Name box of hello General Data screen area, enter a name for hello new SAML SSO identity provider.</span></span>

    > [!Note]
    > <span data-ttu-id="2c179-194">Název Hello hello SAML IDP je povinná a musí být jedinečné; Zobrazí se v seznamu hello k dispozici IDPs SAML, který se zobrazí, pokud vyberete SAML jako metodu ověřování hello SAP HANA XS toouse aplikace, například v oblasti obrazovky ověřování hello nástroje správy Křížky artefaktů hello.</span><span class="sxs-lookup"><span data-stu-id="2c179-194">hello name of hello SAML IDP is mandatory and must be unique; it appears in hello list of available SAML IDPs that is displayed, if you select SAML as hello authentication method for SAP HANA XS applications toouse, for example, in hello Authentication screen area of hello XS Artifact Administration tool.</span></span>

10. <span data-ttu-id="2c179-195">Uložte hello podrobnosti o hello nového poskytovatele identity SAML.</span><span class="sxs-lookup"><span data-stu-id="2c179-195">Save hello details of hello new SAML identity provider.</span></span> <span data-ttu-id="2c179-196">Zvolte **Uložit** toosave hello podrobnosti poskytovatele identity SAML hello a přidejte hello nový SAML IDP toohello seznam známých IDPs SAML.</span><span class="sxs-lookup"><span data-stu-id="2c179-196">Choose **Save** toosave hello details of hello SAML identity provider and add hello new SAML IDP toohello list of known SAML IDPs.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. <span data-ttu-id="2c179-198">V sadě HANA Studio v rámci vlastnosti systému hello hello **konfigurace** kartě, právě filtrovat nastavení **saml** a upravte hello **assertion_timeout** z **10 sekund** příliš**120 sekundu**.</span><span class="sxs-lookup"><span data-stu-id="2c179-198">In HANA Studio within hello system properties of hello **Configuration** tab, just filter settings by **saml** and adjust hello **assertion_timeout** from **10 sec** too**120 sec**.</span></span>

    ![nastavení assertion_timeout](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> <span data-ttu-id="2c179-200">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="2c179-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2c179-201">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="2c179-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2c179-202">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2c179-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2c179-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c179-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="2c179-204">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2c179-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2c179-206">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c179-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c179-207">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c179-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2c179-209">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2c179-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2c179-211">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2c179-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2c179-213">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c179-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2c179-215">a.</span><span class="sxs-lookup"><span data-stu-id="2c179-215">a.</span></span> <span data-ttu-id="2c179-216">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2c179-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2c179-217">b.</span><span class="sxs-lookup"><span data-stu-id="2c179-217">b.</span></span> <span data-ttu-id="2c179-218">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2c179-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2c179-219">c.</span><span class="sxs-lookup"><span data-stu-id="2c179-219">c.</span></span> <span data-ttu-id="2c179-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2c179-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2c179-221">d.</span><span class="sxs-lookup"><span data-stu-id="2c179-221">d.</span></span> <span data-ttu-id="2c179-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2c179-222">Click **Create**.</span></span>
 
### <a name="creating-a-sap-hana-test-user"></a><span data-ttu-id="2c179-223">Vytvoření zkušebního uživatele SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2c179-223">Creating a SAP HANA test user</span></span>

<span data-ttu-id="2c179-224">Uživatelé toolog tooenable Azure AD v tooSAP HANA, se musí být zřízená do SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2c179-224">tooenable Azure AD users toolog in tooSAP HANA, they must be provisioned into SAP HANA.</span></span>
<span data-ttu-id="2c179-225">SAP HANA podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="2c179-225">SAP HANA supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="2c179-226">Pokud potřebujete toocreate uživatel ručně, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2c179-226">If you need toocreate a user manually, perform hello following steps:</span></span>

>[!Note]
><span data-ttu-id="2c179-227">Externí ověřování hello hello uživatel používá, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="2c179-227">You can change hello external authentication used by hello user.</span></span>
<span data-ttu-id="2c179-228">Externí uživatelé se ověřují pomocí externího systému, například systém protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="2c179-228">External users are authenticated using an external system, for example a Kerberos system.</span></span> <span data-ttu-id="2c179-229">Podrobné informace o externí identity, kontaktujte vašeho [správce domény](https://cloudplatform.sap.com/contact.html).</span><span class="sxs-lookup"><span data-stu-id="2c179-229">For detailed information about external identities, contact your [domain administrator](https://cloudplatform.sap.com/contact.html).</span></span>

1. <span data-ttu-id="2c179-230">Otevřete hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) jako správce a povolení hello uživatele databáze pro jednotné přihlašování SAML.</span><span class="sxs-lookup"><span data-stu-id="2c179-230">Open hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) as an administrator and enable hello DB-User for SAML SSO.</span></span>

    ![Vytvoření uživatele](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. <span data-ttu-id="2c179-232">Levé značek hello neviditelná políčko toohello **SAML** a postupujte podle pokynů hello konfigurovat odkaz.</span><span class="sxs-lookup"><span data-stu-id="2c179-232">Tick hello invisible checkbox toohello left of **SAML** and follow hello Configure link.</span></span>

3. <span data-ttu-id="2c179-233">Klikněte na tlačítko **přidat** tooadd hello SAML IDP a klikněte na tlačítko **OK** výběr hello odpovídající IDP SAML.</span><span class="sxs-lookup"><span data-stu-id="2c179-233">Click **Add** tooadd hello SAML IDP and click **OK** selecting hello appropriate SAML IDP.</span></span>

4. <span data-ttu-id="2c179-234">Přidat hello **externí Identity** (např.</span><span class="sxs-lookup"><span data-stu-id="2c179-234">Add hello **External Identity** (ex.</span></span> <span data-ttu-id="2c179-235">Zde BrittaSimon), nebo zvolte **"Žádné"** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c179-235">BrittaSimon here) or choose **"Any"** and click **OK**.</span></span>

    >[!Note]
    ><span data-ttu-id="2c179-236">Pokud není zaškrtnuto políčko "Žádné" zaškrtávací políčko, pak hello uživatelské jméno v HANA musí tooexactly shodu hello jméno uživatele hello v hello UPN před příponu domény hello (tj. BrittaSimon@contoso.com by se stala BrittaSimon v HANA).</span><span class="sxs-lookup"><span data-stu-id="2c179-236">If "ANY" check-box is not checked, then hello user name in HANA needs tooexactly match hello name of hello user in hello UPN before hello domain suffix (i.e. BrittaSimon@contoso.com would become BrittaSimon in HANA).</span></span>

5. <span data-ttu-id="2c179-237">Pro účely testování, přiřaďte všechny **"XS"** toohello uživatelské role.</span><span class="sxs-lookup"><span data-stu-id="2c179-237">For testing purposes, assign all **"XS"** roles toohello user.</span></span>

    ![přiřazení rolí](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > <span data-ttu-id="2c179-239">Tato oprávnění, které jsou vhodné pro vaše případy použití, jenom musíte získat.</span><span class="sxs-lookup"><span data-stu-id="2c179-239">You should give those permissions appropriate for your use cases, only.</span></span>

6. <span data-ttu-id="2c179-240">Uložte hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c179-240">Save hello user.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2c179-241">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2c179-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2c179-242">V této části povolíte tak, že udělíte přístup tooSAP HANA toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c179-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP HANA.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="2c179-244">**tooassign Britta Simon tooSAP HANA, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c179-244">**tooassign Britta Simon tooSAP HANA, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c179-245">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c179-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2c179-247">V seznamu aplikace hello vyberte **SAP HANA**.</span><span class="sxs-lookup"><span data-stu-id="2c179-247">In hello applications list, select **SAP HANA**.</span></span>

    ![Přiřadit uživatele](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. <span data-ttu-id="2c179-249">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2c179-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="2c179-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c179-251">Click **Add** button.</span></span> <span data-ttu-id="2c179-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c179-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="2c179-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2c179-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2c179-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c179-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2c179-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c179-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2c179-257">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c179-257">Testing single sign-on</span></span>

<span data-ttu-id="2c179-258">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2c179-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2c179-259">Když kliknete na dlaždici SAP HANA hello v hello přístupového panelu, měli byste obdržet aplikace automaticky přihlášeného tooyour SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2c179-259">When you click hello SAP HANA tile in hello Access Panel, you should get automatically signed-on tooyour SAP HANA application.</span></span>
<span data-ttu-id="2c179-260">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2c179-260">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2c179-261">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2c179-261">Additional resources</span></span>

* [<span data-ttu-id="2c179-262">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2c179-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2c179-263">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2c179-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

