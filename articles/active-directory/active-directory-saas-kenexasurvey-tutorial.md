---
title: "Kurz: Azure Active Directory integrace s IBM Kenexa průzkum Enterprise | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a IBM Kenexa průzkum Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 5c276c23288292a1c54dd9d57177d5072b90c9e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a><span data-ttu-id="b6f16-103">Kurz: Azure Active Directory integrace s IBM Kenexa průzkum Enterprise</span><span class="sxs-lookup"><span data-stu-id="b6f16-103">Tutorial: Azure Active Directory integration with IBM Kenexa Survey Enterprise</span></span>

<span data-ttu-id="b6f16-104">V tomto kurzu zjistěte, jak integrovat IBM Kenexa průzkum Enterprise s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b6f16-104">In this tutorial, you learn how to integrate IBM Kenexa Survey Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b6f16-105">Integrace IBM Kenexa průzkum Enterprise s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b6f16-105">Integrating IBM Kenexa Survey Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b6f16-106">Můžete ovládat ve službě Azure AD, který má přístup do firemní IBM Kenexa zjišťování sítě.</span><span class="sxs-lookup"><span data-stu-id="b6f16-106">You can control in Azure AD who has access to IBM Kenexa Survey Enterprise.</span></span>
- <span data-ttu-id="b6f16-107">Můžete povolit uživatelům automaticky se přihlaste k IBM Kenexa průzkum Enterprise pomocí jednotného přihlašování (SSO) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6f16-107">You can enable your users to automatically sign in to IBM Kenexa Survey Enterprise by using single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="b6f16-108">Můžete spravovat vaše účty v jednom centrálním místě: portál Azure.</span><span class="sxs-lookup"><span data-stu-id="b6f16-108">You can manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="b6f16-109">Pokud chcete získat další informace o softwaru jako integraci aplikace služby (SaaS) s Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b6f16-109">If you want to know more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6f16-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b6f16-110">Prerequisites</span></span>

<span data-ttu-id="b6f16-111">Konfigurace integrace Azure AD s IBM Kenexa průzkum Enterprise, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b6f16-111">To configure Azure AD integration with IBM Kenexa Survey Enterprise, you need the following items:</span></span>

- <span data-ttu-id="b6f16-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6f16-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b6f16-113">Předplatné služby IBM Kenexa průzkum Enterprise jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b6f16-113">An IBM Kenexa Survey Enterprise SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b6f16-114">Při testování kroky v tomto kurzu, doporučujeme vám, nepoužívejte provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b6f16-114">When you test the steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="b6f16-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="b6f16-115">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="b6f16-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b6f16-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b6f16-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b6f16-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b6f16-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b6f16-118">Scenario description</span></span>
<span data-ttu-id="b6f16-119">V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b6f16-119">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="b6f16-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b6f16-120">The scenario outlined in the tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="b6f16-121">Přidání IBM Kenexa průzkum Enterprise z Galerie</span><span class="sxs-lookup"><span data-stu-id="b6f16-121">Adding IBM Kenexa Survey Enterprise from the gallery</span></span>
* <span data-ttu-id="b6f16-122">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="b6f16-122">Configuring and testing Azure AD SSO</span></span>

## <a name="add-ibm-kenexa-survey-enterprise-from-the-gallery"></a><span data-ttu-id="b6f16-123">Přidat IBM Kenexa průzkum Enterprise z Galerie</span><span class="sxs-lookup"><span data-stu-id="b6f16-123">Add IBM Kenexa Survey Enterprise from the gallery</span></span>
<span data-ttu-id="b6f16-124">Při konfiguraci integrace systému IBM Kenexa průzkum Enterprise do služby Azure AD, přidejte IBM Kenexa průzkum Enterprise z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b6f16-124">To configure the integration of IBM Kenexa Survey Enterprise into Azure AD, add IBM Kenexa Survey Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b6f16-125">Pokud chcete přidat IBM Kenexa průzkum Enterprise z galerie, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b6f16-125">To add IBM Kenexa Survey Enterprise from the gallery, do the following:</span></span>

1. <span data-ttu-id="b6f16-126">V [portál Azure](https://portal.azure.com), v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6f16-126">In the [Azure portal](https://portal.azure.com), in the left pane, click the **Azure Active Directory** button.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="b6f16-128">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="b6f16-130">Chcete-li přidat aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6f16-130">To add an application, click the **New application** button.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="b6f16-132">Do vyhledávacího pole zadejte **IBM Kenexa průzkum Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-132">In the search box, type **IBM Kenexa Survey Enterprise**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. <span data-ttu-id="b6f16-134">Vyberte v seznamu výsledků **IBM Kenexa průzkum Enterprise**a klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b6f16-134">In the results list, select **IBM Kenexa Survey Enterprise**, and then click the **Add** button to add the application.</span></span>

    ![IBM Kenexa průzkum Enterprise v seznamu výsledků](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b6f16-136">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b6f16-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="b6f16-137">V této části můžete nakonfigurovat a otestovat Azure AD přihlášení SSO se IBM Kenexa průzkum Enterprise podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b6f16-137">In this section, you configure and test Azure AD SSO with IBM Kenexa Survey Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b6f16-138">Azure AD pro jednotné přihlašování pro práci, musí identifikovat příslušného uživatele IBM Kenexa průzkum Enterprise ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6f16-138">For SSO to work, Azure AD needs to identify the IBM Kenexa Survey Enterprise user counterpart in Azure AD.</span></span> <span data-ttu-id="b6f16-139">Jinými slovy Azure AD potřeba vytvořit vztah propojení mezi uživatele Azure AD a související uživatelské ve IBM Kenexa průzkum podniku.</span><span class="sxs-lookup"><span data-stu-id="b6f16-139">In other words, Azure AD must establish a link relationship between an Azure AD user and a related user in IBM Kenexa Survey Enterprise.</span></span>

<span data-ttu-id="b6f16-140">K navázání vztahu odkaz, přiřadit hodnotu **uživatelské jméno** ve IBM Kenexa průzkum podniku jako hodnotu **uživatelské jméno** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6f16-140">To establish the link relationship, assign the value of the **user name** in IBM Kenexa Survey Enterprise as the value of the **Username** in Azure AD.</span></span>

<span data-ttu-id="b6f16-141">Nakonfigurovat a otestovat Azure AD přihlášení SSO se IBM Kenexa průzkum Enterprise, dokončete stavebních bloků v následujících dvou částech.</span><span class="sxs-lookup"><span data-stu-id="b6f16-141">To configure and test Azure AD SSO with IBM Kenexa Survey Enterprise, complete the building blocks in the next two sections.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="b6f16-142">Konfigurovat Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b6f16-142">Configure Azure AD SSO</span></span>

<span data-ttu-id="b6f16-143">V této části Povolení jednotného přihlašování Azure AD na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci IBM Kenexa průzkum Enterprise následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b6f16-143">In this section, you enable Azure AD SSO in the Azure portal and configure SSO in your IBM Kenexa Survey Enterprise application by doing the following:</span></span>

1. <span data-ttu-id="b6f16-144">Na portálu Azure na **IBM Kenexa průzkum Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-144">In the Azure portal, on the **IBM Kenexa Survey Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![IBM Kenexa průzkum Enterprise nakonfigurovat jeden přihlašování odkaz][4]

2. <span data-ttu-id="b6f16-146">V **jednotného přihlašování** v dialogovém **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b6f16-146">In the **Single sign-on** dialog box, in the **Mode** box, select **SAML-based Sign-on** to enable SSO.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. <span data-ttu-id="b6f16-148">V **IBM Kenexa průzkum podnikové domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b6f16-148">In the **IBM Kenexa Survey Enterprise Domain and URLs** section, perform the following steps:</span></span>

    ![IBM Kenexa průzkum podnikové domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    <span data-ttu-id="b6f16-150">a.</span><span class="sxs-lookup"><span data-stu-id="b6f16-150">a.</span></span> <span data-ttu-id="b6f16-151">V **identifikátor** textovému poli, zadejte adresu URL s vzoru následující:`https://surveys.kenexa.com/<companycode>`</span><span class="sxs-lookup"><span data-stu-id="b6f16-151">In the **Identifier** textbox, type a URL with the following pattern: `https://surveys.kenexa.com/<companycode>`</span></span>

    <span data-ttu-id="b6f16-152">b.</span><span class="sxs-lookup"><span data-stu-id="b6f16-152">b.</span></span> <span data-ttu-id="b6f16-153">V **adresa URL odpovědi** textovému poli, zadejte adresu URL s vzoru následující:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span><span class="sxs-lookup"><span data-stu-id="b6f16-153">In the **Reply URL** textbox, type a URL with the following pattern: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b6f16-154">Předchozí hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="b6f16-154">The preceding values are not real.</span></span> <span data-ttu-id="b6f16-155">Je aktualizovat skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="b6f16-155">Update them with the actual identifier and reply URL.</span></span> <span data-ttu-id="b6f16-156">Chcete-li získat skutečnými hodnotami, obraťte se [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="b6f16-156">To obtain the actual values, contact the [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

4. <span data-ttu-id="b6f16-157">V části **SAML podpisový certifikát**, klikněte na tlačítko **certifikátu (Base64)**a potom uložte soubor certifikátu do počítače.</span><span class="sxs-lookup"><span data-stu-id="b6f16-157">Under **SAML Signing Certificate**, click **Certificate (Base64)**, and then save the certificate file to your computer.</span></span>

    ![Odkaz ke stažení certifikátu (Base64)](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    <span data-ttu-id="b6f16-159">IBM Kenexa průzkum podniková aplikace očekává kontrolní výrazy zabezpečení kontrolní výrazy Markup Language (SAML) v určitém formátu, který vyžaduje, můžete přidat mapování vlastních atributů ke konfiguraci vaší atributů tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="b6f16-159">The IBM Kenexa Survey Enterprise application expects to receive the Security Assertions Markup Language (SAML) assertions in a specific format, which requires you to add custom attribute mappings to the configuration of your SAML token attributes.</span></span> <span data-ttu-id="b6f16-160">Hodnota deklarace identity identifikátor uživatele v odpovědi musí odpovídat ID jednotné přihlašování, který je nakonfigurovaný v Kenexa systému.</span><span class="sxs-lookup"><span data-stu-id="b6f16-160">The value of the user-identifier claim in the response must match the SSO ID that's configured in the Kenexa system.</span></span> <span data-ttu-id="b6f16-161">Pro mapování identifikátor příslušné uživatele ve vaší organizaci jako jednotného přihlašování k Internetu Datagram Protocol (IDP), pracovat [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="b6f16-161">To map the appropriate user identifier in your organization as SSO Internet Datagram Protocol (IDP), work with the [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> 

    <span data-ttu-id="b6f16-162">Ve výchozím nastavení Azure AD Nastaví uživatelský identifikátor jako hodnotu uživatelského jména (UPN).</span><span class="sxs-lookup"><span data-stu-id="b6f16-162">By default, Azure AD sets the user identifier as the user principal name (UPN) value.</span></span> <span data-ttu-id="b6f16-163">Tuto hodnotu můžete změnit na **atribut** kartě, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="b6f16-163">You can change this value on the **Attribute** tab, as shown in the following screenshot.</span></span> <span data-ttu-id="b6f16-164">Integrace funguje pouze po dokončení mapování správně.</span><span class="sxs-lookup"><span data-stu-id="b6f16-164">The integration works only after you've completed the mapping correctly.</span></span>
    
    ![Dialogové okno atributy uživatele](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png)   

5. <span data-ttu-id="b6f16-166">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-166">Click **Save**.</span></span>

    ![Konfigurovat jednotné přihlašování tlačítko Uložit](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b6f16-168">Otevřete **konfigurovat přihlášení** okno, v části **IBM Kenexa průzkum podniková konfigurace**, klikněte na tlačítko **konfigurace IBM Kenexa průzkum Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-168">To open the **Configure sign-on** window, under **IBM Kenexa Survey Enterprise Configuration**, click **Configure IBM Kenexa Survey Enterprise**.</span></span> 
 
    ![Odkaz Konfigurace IBM Kenexa průzkum Enterprise](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. <span data-ttu-id="b6f16-170">Kopírování **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašovací adresa URL služby** hodnoty z **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="b6f16-170">Copy the **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values from the **Quick Reference** section.</span></span>

8. <span data-ttu-id="b6f16-171">V **konfigurovat přihlášení** okno, v části **Stručná referenční příručka**, kopie **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašovací adresa URL služby** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b6f16-171">In the **Configure sign-on** window, under **Quick Reference**, copy the **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values.</span></span>

9. <span data-ttu-id="b6f16-172">Konfigurace jednotného přihlašování na **IBM Kenexa průzkum Enterprise** straně, odesílat stažené **certifikátu (Base64)**, **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašovací adresa URL služby** hodnoty k [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="b6f16-172">To configure SSO on the **IBM Kenexa Survey Enterprise** side, send the downloaded **Certificate (Base64)**, **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values to the [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

> [!TIP]
> <span data-ttu-id="b6f16-173">Můžete se podívat do stručným verzi tyto pokyny v [portál Azure](https://portal.azure.com) při k nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b6f16-173">You can refer to a concise version of these instructions in the [Azure portal](https://portal.azure.com) while you are setting up the app.</span></span> <span data-ttu-id="b6f16-174">Po přidání aplikace z **služby Active Directory** > **podnikové aplikace, které** jednoduše klikněte na položku **jednotného přihlašování** kartě a potom přejdete na vložené dokumentace prostřednictvím **konfigurace** na konci.</span><span class="sxs-lookup"><span data-stu-id="b6f16-174">After you add the app from the **Active Directory** > **Enterprise Applications** section, simply click the **single sign-on** tab, and then access the embedded documentation through the **Configuration** section at the end.</span></span> <span data-ttu-id="b6f16-175">Další informace o funkci embedded dokumentaci najdete v tématu [Azure AD vložených dokumentaci](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="b6f16-175">To learn more about the embedded documentation feature, see [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b6f16-176">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6f16-176">Create an Azure AD test user</span></span>
<span data-ttu-id="b6f16-177">V této části vytvoříte testovacího uživatele Britta Simon na portálu Azure, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b6f16-177">In this section, you create test user Britta Simon in the Azure portal by doing the following:</span></span>

![Vytvořit testovací uživatele Azure AD][100]

1. <span data-ttu-id="b6f16-179">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6f16-179">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b6f16-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-181">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b6f16-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b6f16-183">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b6f16-185">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b6f16-185">In the **User** dialog box, perform the following steps:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b6f16-187">a.</span><span class="sxs-lookup"><span data-stu-id="b6f16-187">a.</span></span> <span data-ttu-id="b6f16-188">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-188">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b6f16-189">b.</span><span class="sxs-lookup"><span data-stu-id="b6f16-189">b.</span></span> <span data-ttu-id="b6f16-190">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6f16-190">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="b6f16-191">c.</span><span class="sxs-lookup"><span data-stu-id="b6f16-191">c.</span></span> <span data-ttu-id="b6f16-192">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="b6f16-192">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="b6f16-193">d.</span><span class="sxs-lookup"><span data-stu-id="b6f16-193">d.</span></span> <span data-ttu-id="b6f16-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-194">Click **Create**.</span></span>
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a><span data-ttu-id="b6f16-195">Vytvořit testovací uživatele s IBM Kenexa průzkum Enterprise</span><span class="sxs-lookup"><span data-stu-id="b6f16-195">Create an IBM Kenexa Survey Enterprise test user</span></span>

<span data-ttu-id="b6f16-196">V této části vytvoříte uživatele volal Britta Simon v IBM Kenexa průzkum Enterprise.</span><span class="sxs-lookup"><span data-stu-id="b6f16-196">In this section, you create a user called Britta Simon in IBM Kenexa Survey Enterprise.</span></span> 

<span data-ttu-id="b6f16-197">K vytvoření uživatele v systému IBM Kenexa průzkum Enterprise a mapování ID jednotného přihlašování pro ně, můžete pracovat [tým podpory IBM Kenexa průzkum Enterprise](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="b6f16-197">To create users in the IBM Kenexa Survey Enterprise system and map the SSO ID for them, you can work with the [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> <span data-ttu-id="b6f16-198">Tato hodnota ID jednotné přihlašování musí taky namapovaný na hodnota identifikátoru uživatele z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6f16-198">This SSO ID value should also be mapped to the user identifier value from Azure AD.</span></span> <span data-ttu-id="b6f16-199">Toto výchozí nastavení můžete změnit na **atribut** kartě.</span><span class="sxs-lookup"><span data-stu-id="b6f16-199">You can change this default setting on the **Attribute** tab.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="b6f16-200">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6f16-200">Assign the Azure AD test user</span></span>

<span data-ttu-id="b6f16-201">V této části povolit uživatele Britta Simon používat jednotného přihlašování k Azure tak, že udělíte přístup do firemní IBM Kenexa zjišťování sítě.</span><span class="sxs-lookup"><span data-stu-id="b6f16-201">In this section, you enable user Britta Simon to use Azure SSO by granting access to IBM Kenexa Survey Enterprise.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="b6f16-203">Pokud chcete přiřadit uživatele Britta Simon IBM Kenexa průzkum Enterprise, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b6f16-203">To assign user Britta Simon to IBM Kenexa Survey Enterprise, do the following:</span></span>

1. <span data-ttu-id="b6f16-204">Na portálu Azure otevřete **aplikace** zobrazení, přejděte na **Directory** zobrazit, vyberte možnost **podnikové aplikace, které**a potom klikněte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-204">In the Azure portal, open the **Applications** view, go to the **Directory** view, select **Enterprise applications**, and then click **All applications**.</span></span>

    !["Podnikové aplikace" a "Všechny aplikace" odkazy][201] 

2. <span data-ttu-id="b6f16-206">V **aplikace** seznamu, vyberte **IBM Kenexa průzkum Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-206">In the **Applications** list, select **IBM Kenexa Survey Enterprise**.</span></span>

    ![IBM Kenexa průzkum Enterprise odkaz v seznamu aplikací](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. <span data-ttu-id="b6f16-208">V levém podokně klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-208">In the left pane, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="b6f16-210">Klikněte na tlačítko **přidat** tlačítko a pak na **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-210">Click the **Add** button and then, in the **Add Assignment** pane, select **Users and groups**.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="b6f16-212">V **uživatelů a skupin** v dialogovém **uživatelé** seznamu, vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b6f16-212">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="b6f16-213">V **uživatelů a skupin** dialogové okno, klikněte **vyberte** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6f16-213">In the **Users and groups** dialog box, click the **Select** button.</span></span>

7. <span data-ttu-id="b6f16-214">V **přidat přiřazení** dialogové okno, klikněte **přiřadit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b6f16-214">In the **Add Assignment** dialog box, click the **Assign** button.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b6f16-215">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b6f16-215">Test single sign-on</span></span>

<span data-ttu-id="b6f16-216">V této části otestovat konfiguraci Azure AD jednotného přihlašování pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b6f16-216">In this section, you test your Azure AD SSO configuration by using the Access Panel.</span></span>

<span data-ttu-id="b6f16-217">Když kliknete **IBM Kenexa průzkum Enterprise** dlaždici na přístupovém panelu můžete by měl být automaticky přihlášeni k vaší aplikaci IBM Kenexa průzkum Enterprise.</span><span class="sxs-lookup"><span data-stu-id="b6f16-217">When you click the **IBM Kenexa Survey Enterprise** tile in the Access Panel, you should be automatically signed in to your IBM Kenexa Survey Enterprise application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6f16-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b6f16-218">Additional resources</span></span>

* [<span data-ttu-id="b6f16-219">Seznam kurzů k integraci aplikací SaaS v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b6f16-219">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b6f16-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b6f16-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
